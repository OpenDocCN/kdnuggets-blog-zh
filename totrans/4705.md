# 使用迁移学习和弱监督廉价构建 NLP 分类器

> 原文：[https://www.kdnuggets.com/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html/2](https://www.kdnuggets.com/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html?page=2#comments)

### ****第二步：使用 Snorkel 构建训练集****

构建标签功能是一个非常动手的阶段，但这会得到回报！我预计如果你已经有领域知识，这应该花费一天时间（如果没有，可能需要几天）。此外，**本节内容结合了我为我的项目所做的具体工作和一些通用建议**，这些建议可以应用于你自己的项目。

由于大多数人以前没有使用过 Snorkel 的弱监督，我会尽可能详细地解释我所采用的方法。这个 [教程](https://github.com/HazyResearch/metal/blob/master/tutorials/Basics.ipynb) 是理解主要概念的好方法，但阅读我的工作流程应该能为你节省大量的试错时间。

以下是一个 LF 的示例，如果推文包含对犹太人的常见侮辱之一，则返回 **正面**。否则，它将保持中立。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

```py
*# Common insults against jews.*
INSULTS = r"\bjew (bitch|shit|crow|fuck|rat|cockroach|ass|bast(a|e)rd)"

def insults(tweet_text):
    return POSITIVE if re.search(INSULTS, tweet_text) else ABSTAIN
```

这里是一个 LF 的示例，如果推文的作者提到他或她是犹太人，则返回 **负面**，这通常意味着该推文不是反犹太主义的。

```py
*# If tweet author is Jewish then it's likely not anti-semitic.*
JEWISH_AUTHOR = r"((\bI am jew)|(\bas a jew)|(\bborn a jew)"

def jewish_author(tweet_tweet):
    return NEGATIVE if re.search(JEWISH_AUTHOR, tweet_tweet) else ABSTAIN
```

在设计 LF 时，重要的是要记住 **我们优先考虑高精度而非召回率**。我们希望分类器能够识别更多模式，从而提高召回率。但如果 LF 的精度或召回率不是特别高，也不必担心，Snorkel 会处理这些问题。

一旦你有了一些 LF，你只需构建一个矩阵，每行一个推文，每列 LF 值。Snorkel Metal 有一个非常方便的工具函数来显示你的 LF 摘要。

```py
*# We build a matrix of LF votes for each tweet*
LF_matrix = make_Ls_matrix(LF_set, LFs)

*# Get true labels for LF set*
Y_LF_set = np.array(LF_set['label'])

display(lf_summary(sparse.csr_matrix(LF_matrix), 
                   Y=Y_LF_set, 
                   lf_names=LF_names.values()))
```

我总共有 24 个 LF，但以下是我部分 LF 的 LF 摘要。你可以在表格下方找到每一列的含义。

![Figure](../Images/61cbdc47a245bcd4a768cf0151c13ee0.png)

LF 摘要

列的含义：

+   **精确度：** 正确 LF 预测的比例。你应该确保所有 LF 的精确度至少为 0.5。

+   **覆盖率：** 至少有一个 LF 投票为正或负的样本百分比。你要最大化这一点，同时保持良好的准确性。

+   **极性：** 告诉你 LF 返回的值是什么。

+   **重叠与冲突：** 这告诉你一个 LF 如何与其他 LFs 重叠和冲突。不必过于担心，标签模型实际上会利用这些信息来估计每个 LF 的准确性。

让我们检查一下我们的覆盖率：

```py
label_coverage(LF_matrix)
>> 0.8062755798090041
```

这相当不错！

作为我们弱监督的基线，我们将通过使用多数标签投票模型来评估我们的 LFs，以预测 LF 集中的类。这只是分配一个积极标签，如果大多数 LF 是积极的，所以它基本上假设所有 LF 具有相同的准确性。

```py
from metal.label_model.baselines import MajorityLabelVoter

mv = MajorityLabelVoter()
Y_train_majority_votes = mv.predict(LF_matrix)
print(classification_report(Y_LFs, Y_train_majority_votes))
```

![Figure](../Images/d2ae2e4485ec439f323acc6f507f227b.png)

多数投票者基线的分类报告

我们可以看到，对正类（“1”）的 F1-score 是 0.61。为了提高这一点，我制作了一个电子表格，其中每一行都有一条推文、它的真实标签、以及基于每个 LF 的分配标签。目标是找到 LF 与真实标签不一致的地方，并相应地修正 LF。

![Figure](../Images/4c9dd3dfd4e27c6de09cd16e169f6f34.png)

我用来调整我的 LFs 的 Google 表格

在我的 LFs 具有约 60% 精度和 60% 召回率之后，我继续训练了标签模型。

```py
Ls_train = make_Ls_matrix(train, LFs)

*# You can tune the learning rate and class balance.*
label_model = LabelModel(k=2, seed=123)
label_model.train_model(Ls_train, n_epochs=2000, print_every=1000, 
                        lr=0.0001, 
                        class_balance=np.array([0.2, 0.8]))
```

现在要测试标签模型，我将其与我的测试集进行了验证，并绘制了精度-召回率曲线。我们可以看到，我们能够获得大约 80% 的精度和 20% 的召回率，这相当不错。使用标签模型的一个重大优势是我们现在可以调整预测概率阈值，以获得更好的精度。

![Figure](../Images/44c2019443a72a3723541b05f77eaf72.png)

标签模型的精度-召回率曲线

我还通过检查根据标签模型的前 100 个反犹太主义最强的推文，确保它们有意义来验证我的标签模型是否正常工作。现在我们对我们的标签模型满意了，我们生成了我们的训练标签：

```py
# To use all information possible when we fit our classifier, we can # actually combine our hand-labeled LF set with our training set.

Y_train = label_model.predict(Ls_train) + Y_LF_set
```

这是我 WS 工作流程的总结：

1.  通过 LF 集中的示例，识别一个新的潜在 LF。

1.  将其添加到标签矩阵中，并检查其准确性是否至少为 50%。尽量获得最高的准确性，同时保持良好的覆盖率。如果 LFs 相关，我会将不同的 LFs 组合在一起。

1.  有时你会想使用基础的多数投票模型（在 Snorkel Metal 中提供）来标注你的 LF 集。相应地更新你的 LFs，仅使用多数投票模型即可获得相当不错的得分。

1.  如果你的多数投票模型不够好，那么你可以修正你的 LFs 或返回第 1 步并重复操作。

1.  一旦你的多数投票模型有效，那么就对你的训练集运行你的 LFs。你应该至少有 60% 的覆盖率。

1.  完成后，训练你的标签模型！

1.  为了验证标签模型，我对我的训练集运行了标签模型，并打印了前 100 个反犹太主义最强的推文和 100 个反犹太主义最弱的推文，以确保它正常工作。

现在我们有了标签模型，我们可以为**2.5万条推文计算概率标签，并将其用作训练集**。现在，继续训练我们的分类模型吧！

Snorkel的一般提示：

+   关于LF准确率：在WS步骤中，我们追求高精度。你的所有LF在LF集合上的准确率应至少达到50%。如果能达到75%或更高，那就更好了。

+   关于LF覆盖率：你需要确保至少65%的训练集有一个LF投票为正或负。这被称为Snorkel的LF覆盖率。

+   如果你一开始不是领域专家，你将在标记600个初始数据点的过程中获得新LF的想法。

### **第三步：构建**分类模型****

最后一步是训练我们的分类器，使其能够在噪声较多的手工规则之外进行泛化。

**基准线**

我们将从设定一些基准线开始**。我尝试在没有深度学习的情况下构建最佳模型。我尝试了Tf-idf特征化结合sklearn的逻辑回归、XGBoost和前馈神经网络。

以下是结果。为了获得这些数字，我绘制了一个针对开发集的精确度-召回率曲线，然后选择了我喜欢的分类阈值（尽可能争取90%以上的精确度，同时召回率尽可能高）。

![图](../Images/ebc7de5adec8e85b81bdad411fd25e76.png)

基准线

**尝试ULMFiT**

一旦我们下载了在维基百科上训练的ULM，我们需要调整它以适应推文，因为它们的语言差异较大。我按照[这个很棒的博客](https://towardsdatascience.com/transfer-learning-in-nlp-for-tweet-stance-classification-8ab014da8dde)中的所有步骤和代码操作，并且我还使用了来自Kaggle的[Twitter Sentiment140数据集](https://www.kaggle.com/kazanova/sentiment140)来微调语言模型。

我们从数据集中随机抽取100万条推文，并在这些推文上微调语言模型。这样，语言模型将能够在推特领域进行泛化。

下面的代码加载推文并训练语言模型。我使用了Paperspace的GPU和fastai的公共镜像，这效果非常好。你可以按照[这些步骤](https://github.com/reshamas/fastai_deeplearn_part1/blob/master/tools/paperspace.md)进行设置。

```py
data_lm = TextLMDataBunch.from_df(train_df=LM_TWEETS,         valid_df=df_test, path="")

learn_lm = language_model_learner(data_lm, pretrained_model=URLs.WT103_1, drop_mult=0.5)
```

我们解冻语言模型中的所有层：

```py
learn_lm.unfreeze()
```

我们运行了20个周期。我将周期放入一个循环中，以便在每次迭代后保存模型。我没找到使用fastai轻松做到这一点的方法。

```py
for i in range(20):
    learn_lm.fit_one_cycle(cyc_len=1, max_lr=1e-3, moms=(0.8, 0.7))
    learn_lm.save('twitter_lm')
```

然后我们应该测试语言模型，以确保它至少有一点道理：

```py
learn_lm.predict("i hate jews", n_words=10)
>> 'i hate jews are additional for what hello you brother . xxmaj the'
learn_lm.predict("jews", n_words=10)
>> 'jews out there though probably okay jew back xxbos xxmaj my'
```

像“xxmaj”这样的奇怪标记是fastai添加的一些特殊标记，有助于文本理解。例如，它们为大写字母、句子的开头、重复的词等添加了特殊标记。语言模型的表现可能不是很理想，但这没关系。

现在我们将训练我们的分类器：

```py
*# Classifier model data*
data_clas = TextClasDataBunch.from_df(path = "", 
                                      train_df = df_trn,
                                      valid_df = df_val,                                         
                                      vocab=data_lm.train_ds.vocab, 
                                      bs=32, 
                                      label_cols=0)

learn = text_classifier_learner(data_clas, drop_mult=0.5)
learn.freeze()
```

使用fastai的方法寻找合适的学习率：

```py
learn.lr_find(start_lr=1e-8, end_lr=1e2)
learn.recorder.plot()
```

![](../Images/085c485797cd64642a08e461f5295ae2.png)

我们将通过逐渐解冻来微调分类器：

```py
learn.fit_one_cycle(cyc_len=1, max_lr=1e-3, moms=(0.8, 0.7))
learn.freeze_to(-2)
learn.fit_one_cycle(1, slice(1e-4,1e-2), moms=(0.8,0.7))
learn.freeze_to(-3)
learn.fit_one_cycle(1, slice(1e-5,5e-3), moms=(0.8,0.7))
learn.unfreeze()
learn.fit_one_cycle(4, slice(1e-5,1e-3), moms=(0.8,0.7))
```

![图示](../Images/60007d60c67b4d38edde397b5a420512.png)

一些训练轮次

在微调后，让我们绘制我们的精确率-召回率曲线！看到第一次尝试后的结果非常好。

![图示](../Images/905e709274b4cbfaba8028d2682a822e.png)

ULMFiT在弱监督下的精确率-召回率曲线

我选择了0.63的概率阈值，这使我们获得了**95% 的精准率和39% 的召回率**。这在召回率上有了很大的提升，同时在精准率上也有所提高。

![图示](../Images/8d2c36dbb72e91604057d120b11e3b54.png)

ULMFiT模型的分类报告

**与我们的模型玩得开心**

下面是一个非常酷的例子，展示了模型如何捕捉到“doesn’t”改变了推文的含义！

```py
learn.predict("george soros controls the government")
>> (Category 1, tensor(1), tensor([0.4436, 0.5564]))

learn.predict("george soros doesn't control the government")
>> (Category 0, tensor(0), tensor([0.7151, 0.2849]))
```

这里有一些针对犹太人的侮辱：

```py
learn.predict("fuck jews")
>> (Category 1, tensor(1), tensor([0.1996, 0.8004]))

learn.predict("dirty jews")
>> (Category 1, tensor(1), tensor([0.4686, 0.5314]))
```

这里有一个人呼吁反犹太主义的推文：

```py
learn.predict("Wow. The shocking part is you're proud of offending every serious jew, mocking a religion and openly being an anti-semite.")
>> (Category 0, tensor(0), tensor([0.9908, 0.0092]))
```

这里是其他非反犹太主义的推文：

```py
learn.predict("my cousin is a russian jew from ukraine- ???????????? i'm so glad they here")
>> (Category 0, tensor(0), tensor([0.8076, 0.1924]))

learn.predict("at least the stolen election got the temple jew shooter off the drudgereport. I ran out of tears.")
>> (Category 0, tensor(0), tensor([0.9022, 0.0978]))
```

****弱监督真的有帮助吗？****

我很好奇是否需要弱监督才能获得这种性能，因此我进行了一个小实验。我进行了与之前相同的过程，但没有WS标签，并得到了这个精确率-召回率曲线：

![图示](../Images/887a4d2d3e0a53f16609374ca2f158e2.png)

ULMFiT在没有弱监督下的精确率-召回率曲线

我们可以看到召回率大幅下降（我们只有**10% 的召回率**，而精准率为90%）和ROC-AUC **(-0.15)**，与我们使用WS标签的之前精确率-召回率曲线相比。

**结论**

+   弱监督 + ULMFiT帮助我们达到了95%的精准率和39%的召回率。这远远超过了所有基准，因此非常令人兴奋。我完全没有预料到这一点。

+   这个模型非常容易保持最新。无需重新标注，我们只需更新LFs并重新运行WS + ULMFiT管道。

+   弱监督通过允许ULMFiT更好地泛化，带来了很大的不同。

****下一步****

+   我相信我们可以通过进一步努力改进我的LFs来获得最大的收益，以提高弱监督模型。我会首先包含基于外部知识库如[Hatebase’s](https://hatebase.org/)的仇恨言论模式的LFs。然后，我会基于Spacy的依赖树解析编写新的LFs。

+   我们没有进行任何超参数调整，但这可能会帮助提升Label Model和ULMFiT的性能。

+   我们可以尝试不同的分类模型，比如微调BERT或OpenAI的Transformer。

**个人简介: [Abraham Starosta](https://www.linkedin.com/in/abraham-starosta-ba662764/)** (**[starosta@stanford.edu](mailto:starosta@stanford.edu)**) 来自委内瑞拉，目前在斯坦福大学完成计算机科学硕士学位，专注于人工智能和自然语言处理。在开始斯坦福的硕士课程之前，他曾是Primer AI的数据科学家，这是一家开发文本理解和总结技术的初创公司。他还是Nav Talent的共同创始人，这是一家为顶级初创公司提供技术招聘服务的机构，该机构起步于斯坦福大学。多年来，他还有机会在其他顶级初创公司如Livongo、Zugata和Splunk担任软件工程师。在闲暇时，他喜欢踢足球和打乒乓球。

[原始文章](https://towardsdatascience.com/a-technique-for-building-nlp-classifiers-efficiently-with-transfer-learning-and-weak-supervision-a8e2f21ca9c8)。已获转载许可。

**相关:**

+   [如何解决90%的自然语言处理问题：逐步指南](/2019/01/solve-90-nlp-problems-step-by-step-guide.html)

+   [OpenAI的GPT-2：模型、炒作与争议](/2019/03/openai-gpt-2-model-hype-controversy.html)

+   [更有效的自然语言处理迁移学习](/2018/10/more-effective-transfer-learning-nlp.html)

### 更多相关内容

+   [弱监督建模解析](https://www.kdnuggets.com/2022/05/weak-supervision-modeling-explained.html)

+   [迁移学习在图像识别和自然语言处理中的应用](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [TensorFlow在计算机视觉中的应用 - 迁移学习简化版](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [什么是迁移学习？](https://www.kdnuggets.com/2022/01/transfer-learning.html)

+   [使用PyTorch的迁移学习实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [探索小数据场景中的迁移学习潜力](https://www.kdnuggets.com/exploring-the-potential-of-transfer-learning-in-small-data-scenarios)
