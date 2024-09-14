# 处理数据泄漏

> 原文：[https://www.kdnuggets.com/2021/10/dealing-data-leakage.html](https://www.kdnuggets.com/2021/10/dealing-data-leakage.html)

[评论](#comments)

**由 [Susan Currie Sivek, Ph.D.](https://www.linkedin.com/in/ssivek/)，高级数据科学记者**

![图示](../Images/0837488b007543b7781e158aa9753758.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

捕捉自 [via GIPHY](https://giphy.com/gifs/cartoonhangover-cartoons-bravestwarriors-etBlg1EQTz6pCzdKmd)

你正在为即将到来的考试做准备。考试是开卷的，所以你在复习时使用参考资料，表现非常好。

但当你在考试当天出现时，突然被告知考试不再是开卷的了。结果就不那么好了。

[via GIPHY](https://giphy.com/gifs/funny-exam-vXKDrRey28YCY)

这听起来像是学术过度成就者的焦虑梦，但它类似于在机器学习模型中发生目标泄漏的情况。假设你建立了一个预测特定结果的模型，并用帮助模型进行预测的信息对其进行训练。那个模型可能表现得很好……也许表现得异常好。但如果有些信息在模型实际进行预测时无法获得，其真实表现将会下降。这就是目标泄漏的结果——数据科学家的焦虑梦！

我最近听到我们的一位内部 Alteryx 专家称目标泄漏为机器学习中最棘手的问题。但它是如何发生的，如何避免这个问题？它与“数据泄漏”有何关系？

[via GIPHY](https://giphy.com/gifs/underground-leak-sprinkler-Nf1O8cYzypC1O)

### 目标泄漏

目标泄漏发生在模型使用了在预测时无法获得的数据进行训练时。模型在最初的训练和测试中表现良好，但当投入生产时，由于缺少现在已不再可用的数据，模型表现变差。就像你在书本旁学习，然后在考试时没有这些书本，模型也缺少了在训练过程中提升其表现的有用信息。

这里有一些代表目标泄漏的场景：

+   在用于训练模型的数据集中将需要预测的结果作为特征包含在内（这听起来可能很傻，但确实可能发生；例如，复制并重命名你的目标变量字段，然后忘记这一复制，可能会导致你无意中使用额外版本的目标作为预测变量）；

+   包含一个表示学生在大学里就读年限的特征，用于预测学生是否会接受该大学的录取邀请；

+   包含一个表示订阅月数的特征，用于预测潜在客户是否会订阅；

+   包含一个表示火灾相关保险索赔是否被批准的特征，用于预测某种类型墙板房屋的火灾；和

+   包含来自其他数据集的信息，这些信息在预测时模型无法获得的详细内容。

在所有这些情况下，当模型构建时包含了在预测时无法知道的信息。我们无法在尝试确定客户是否会订阅时知道他们会订阅多少个月。（我们可以构建一个预测订阅月数的模型吗？当然可以。但我们会基于已知订阅者的数据，而不是所有可能订阅或不订阅的客户数据。）类似地，如果我们告诉模型某些材料建造的房屋曾经提出过火灾保险索赔，那么我们就把火灾发生后的知识引入了试图预测火灾的模型中。

即使是看似无害的细节，如文件大小或时间戳，也可能无意中成为目标变量的代理。例如，2013年的Kaggle竞赛[因这种问题被暂停](https://www.kaggle.com/c/the-icml-2013-whale-challenge-right-whale-redux/discussion/4865#25839)并重新整理数据集。发现（并认真报告）泄露的团队曾短暂地在排行榜上名列前茅！

数据泄露导致的结果是对训练数据的过拟合。你的模型可能在具备额外知识的情况下预测得非常好——在开卷考试中表现出色——但在预测时没有这些信息时表现就不好。

[通过 GIPHY](https://giphy.com/gifs/fix-having-leak-hQtlzrmpE0gKY)

### 训练-测试污染

另一种数据泄露的形式有时称为“训练-测试污染”。这个问题可能不涉及你的目标变量，但它会影响模型性能。这是我们可能无意中将未来数据的知识添加到训练数据中的另一种方式，导致性能指标看起来比实际生产中要好。（顺便提一下，如果你查找更多关于这个话题的阅读，注意“数据泄露”这个词有时也被网络安全人员用来谈论[数据泄露事件](https://en.wikipedia.org/wiki/Data_breach)。）

一种常见的训练-测试污染发生方式是，在将数据集拆分为训练集和测试集之前，或在使用交叉验证之前，先对整个数据集进行预处理。

例如，[数据归一化](https://community.alteryx.com/t5/Data-Science/Normalization-Standardization-and-Regularization-in-Alteryx-and/ba-p/733996?utm_content=827583&utm_source=kdn)需要使用数据集中每个变量的数值范围。对整个数据集进行归一化时，会在评估时将“知识”提供给模型。然而，投入生产的模型不会拥有这些知识，因此在预测时表现不会那么好。类似地，对整个数据集进行标准化会不当地告知模型整个数据集的均值和标准差。填补缺失值也使用了有关数据集的汇总统计数据（例如，中位数、均值）。

所有这些线索都可能帮助模型在你的训练和测试数据上的表现优于当模型最终引入全新的数据时。[这篇文章](https://machinelearningmastery.com/data-preparation-without-data-leakage/)提供了对这种数据泄漏的深入探讨，包括演示代码。

另一个问题可能会出现，如果你使用 [k-折交叉验证](https://community.alteryx.com/t5/Data-Science/Holdouts-and-Cross-Validation-Why-the-Data-Used-to-Evaluate-your/ba-p/448982?utm_content=827583&utm_source=kdn) 来评估你的模型。只要你的数据集中每个人/来源仅包含一个观察值，这种类型的泄漏对你来说不应该是个问题。然而，如果你的数据集中每个人或来源有多个观察值（即，数据行），那么在创建训练和测试模型的数据子集或“折”时，来自同一来源的所有观察值需要被归为一组。

例如，如果来自A人的观察值被包含在训练组和测试组中，你可能会使用A人的训练数据来预测A人的测试数据。模型在测试集上的表现似乎更好——测试集也包含A人——因为它已经从训练集中知道了A人的一些信息。但在生产中，它不会有这种先前接触的优势。有关这个问题的更多详细信息（有时称为“组泄漏”），请查看这篇文章。

[via GIPHY](https://giphy.com/gifs/xyShS1HDFNNZe)

### 处理数据泄漏

当你家中的水龙头滴水时，你可以通过声音和水洼来识别。但这些类型的泄漏可能很难检测。你仍然可以进行预防性维护和修理来解决这个问题。

异常良好的模型性能可能是泄漏的迹象。如果你的模型表现得惊人地好，抵制自满的诱惑，避免急于发布。这种表现可能是普通的过拟合造成的，但也可能反映了目标或数据泄漏。

为了尽量避免数据泄漏，你可以进行全面的[探索性数据分析](https://community.alteryx.com/t5/Data-Science/Adventures-in-Data-Exploratory-Data-Analysis/ba-p/545267?utm_content=827583&utm_source=kdn)（EDA），并查找与结果变量有特别高相关性的特征。值得仔细检查这些关系，以确保如果将高度相关的特征一起用于模型中，不会存在泄漏的潜在风险。如果你有一个具有许多特征的高维数据集，这项审查可能会很具挑战性，因此在 Alteryx Designer 中使用像[皮尔逊相关工具](https://community.alteryx.com/t5/Alteryx-Designer-Knowledge-Base/Tool-Mastery-Pearson-Correlation/ta-p/388744?utm_content=827583&utm_source=kdn)这样的工具，并过滤和/或可视化其输出可能会有帮助。

为了避免训练-测试污染，请确保在应用任何转换（如归一化）之前，将数据分成训练集/测试集/保留集，然后在训练集上训练模型。接下来，将相同的参数应用于测试集/保留集并进行转换，然后测试模型的性能。

[通过 GIPHY](https://giphy.com/gifs/cartoonhangover-cartoons-bravestwarriors-etBlg1EQTz6pCzdKmd)

此外，确保你充分理解数据集中所有的特征。[这个 Kaggle 示例](https://www.kaggle.com/alexisbcook/data-leakage)展示了如何在信用卡申请批准数据集中，一个名为“支出”的特征如果用于预测批准申请，可能会导致目标泄漏。“支出”作为特征名称可能有很多含义，但在这种情况下，它指的是信用卡用户在卡上花费了多少。这一特征暗示他们确实被批准了卡片，并不当地影响了模型的批准预测，因为在预测时支出信息是不可用的。

最后，检查特征的相对重要性并审查其他可解释性工具可能会帮助你发现泄漏。在上述信用卡申请批准的例子中，“支出”可能看起来是预测信用卡批准中一个非常重要的特征。如果有疑问，你可以尝试移除一个特征，看看模型的性能如何变化，然后确定模型是否突然表现得更为真实。

[通过 GIPHY](https://giphy.com/gifs/season-16-the-simpsons-16x16-xT5LMXBrV5NkVZHKms)

### 你的模型防泄漏

我希望这个概述为你提供了一些新的技术，以帮助你避免这些泄漏情况！通过仔细的EDA和对数据集的全面了解，以及正确的预处理和交叉验证设置，你应该能够保持目标的良好控制，并使数据集免于污染。

### 推荐阅读

+   [向数据科学家询问：数据泄漏](https://insidebigdata.com/2014/11/26/ask-data-scientist-data-leakage/)

+   [机器学习中的数据泄漏](https://machinelearningmastery.com/data-leakage-machine-learning/)

+   [机器学习中的目标泄漏](https://aiukraine.com/wp-content/uploads/2018/09/12_00-Yuriy-Guts-Target-Leakage-in-Machine-Learning-.pdf)

最初发布于[Alteryx Community Data Science Blog](https://community.alteryx.com/t5/Data-Science/Dealing-with-Data-Leakage/ba-p/827583?utm_content=827583&utm_source=kdn)。

**简介：[Susan Currie Sivek, Ph.D.](https://www.linkedin.com/in/ssivek/)** 是Alteryx Community的高级数据科学记者，她与全球观众探讨数据科学概念。她也是[数据科学混音器](https://community.alteryx.com/t5/Data-Science-Mixer/bg-p/mixer?utm_content=733996&utm_source=kdn)播客的主持人。她在学术界和社会科学方面的背景为她研究数据和传达复杂观点的方式提供了信息，同时她的新闻培训也为她的创意注入了灵感。

[原文](https://community.alteryx.com/t5/Data-Science/Dealing-with-Data-Leakage/ba-p/827583?utm_content=827583&utm_source=kdn)。经授权转载。

**相关内容：**

+   [机器学习的持续训练 – 成功策略的框架](/2021/04/continuous-training-machine-learning.html)

+   [AI训练数据的5篇重要论文](/2020/06/5-essential-papers-ai-training-data.html)

+   [Python中的数据集拆分最佳实践](/2020/05/dataset-splitting-best-practices-python.html)

### 更多相关话题

+   [处理文本数据中的噪声标签](https://www.kdnuggets.com/2023/04/dealing-noisy-labels-text-data.html)

+   [处理推荐和搜索中的位置偏差](https://www.kdnuggets.com/2023/03/dealing-position-bias-recommendations-search.html)

+   [数据科学职位标题的导航：数据分析师 vs 数据科学家…](https://www.kdnuggets.com/navigating-data-science-job-titles-data-analyst-vs-data-scientist-vs-data-engineer)

+   [数据科学家、数据工程师及其他数据职业解释](https://www.kdnuggets.com/2021/05/data-scientist-data-engineer-data-careers-explained.html)

+   [数据工程师和数据科学家通用的高保真合成数据](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [数据科学家 vs 数据分析师 vs 数据工程师](https://www.kdnuggets.com/2022/01/data-scientist-data-analyst-data-engineer.html)
