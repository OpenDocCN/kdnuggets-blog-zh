# 如何在你的第一个Kaggle竞赛中排名前10%

> 原文：[https://www.kdnuggets.com/2016/11/rank-ten-precent-first-kaggle-competition.html/4](https://www.kdnuggets.com/2016/11/rank-ten-precent-first-kaggle-competition.html/4)

### Home Depot 搜索相关性

在这一部分，我将分享我在[Home Depot 搜索相关性竞赛](https://www.kaggle.com/c/home-depot-product-search-relevance)中的解决方案，以及我从竞赛后顶尖团队中学到的东西。

本次竞赛的任务是预测搜索词在Home Depot网站上的结果相关性。相关性是三名人工评估者的平均分，范围在1~3之间。因此这是一个回归任务。数据集包含搜索词、产品标题/描述以及品牌、尺寸和颜色等属性。评估指标是[RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation)。

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

这很像[Crowdflower 搜索结果相关性](https://www.kaggle.com/c/crowdflower-search-relevance)。不同之处在于[Cohen's Kappa](https://en.wikipedia.org/wiki/Cohen%27s_kappa#Weighted_kappa)在Crowdflower竞赛中被使用，这使得回归分数的最终截止点更加复杂。此外，Crowdflower没有提供属性。

**EDA**

在我参加竞赛时，有几个相当好的EDA，特别是[这个](https://www.kaggle.com/briantc/home-depot-product-search-relevance/homedepot-first-dataexploreation-k)。我了解到：

+   许多搜索词/产品出现了多次。

+   文本相似性是很好的特征。

+   许多产品没有属性特征。这会是一个问题吗？

+   产品ID似乎具有很强的预测能力。然而，训练集和测试集之间的产品ID重叠度不是很高。这会导致过拟合吗？

**预处理**

你可以在[GitHub](https://github.com/dnc1994/Kaggle-Playground/blob/master/home-depot/Preprocess.ipynb)上找到我如何进行预处理和特征工程的详细信息。我这里只给出一个简要总结：

1.  使用[错别字词典](https://www.kaggle.com/steubk/home-depot-product-search-relevance/fixing-typos)来修正搜索词中的错别字。

1.  统计属性。找出那些频繁出现且容易被利用的属性。

1.  将训练集与测试集合并。这很重要，否则你将不得不进行两次特征变换。

1.  对所有文本字段进行**[词干提取](https://en.wikipedia.org/wiki/Stemming)**和**[分词](https://en.wikipedia.org/wiki/Text_segmentation#Word_segmentation)**。一些**归一化**（包括数字和单位）和**同义词替换**是手动执行的。

**特征**

+   *属性特征

    +   产品是否包含某个属性（品牌、尺寸、颜色、重量、室内/室外、能源之星认证等）

    +   某个属性是否与搜索词匹配

+   元特征

    +   每个文本字段的长度

    +   产品是否包含属性字段

    +   品牌（编码为整数）

    +   产品ID

+   匹配

    +   搜索词是否出现在产品标题/描述/属性中

    +   搜索词在产品标题/描述/属性中的出现次数和比例

    +   *搜索词的第 i 个单词是否出现在产品标题/描述/属性中

+   搜索词与产品标题/描述/属性之间的文本相似度

    +   [BOW](https://en.wikipedia.org/wiki/Bag-of-words_model) [余弦相似度](https://en.wikipedia.org/wiki/Cosine_similarity)

    +   [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) 余弦相似度

    +   [Jaccard 相似度](https://en.wikipedia.org/wiki/Jaccard_index)

    +   *[编辑距离](https://en.wikipedia.org/wiki/Edit_distance)

    +   [Word2Vec](https://en.wikipedia.org/wiki/Word2vec) 距离（我没有包括这个，因为它的表现差且计算缓慢。不过，似乎我使用得不对。）

+   **[潜在语义分析](https://en.wikipedia.org/wiki/Latent_semantic_indexing)：通过对从BOW/TF-IDF向量化获得的矩阵进行[SVD分解](https://en.wikipedia.org/wiki/Singular_value_decomposition)，我们获得了不同搜索词/产品组的潜在表示。这使我们的模型能够区分不同组并为特征分配不同的权重，从而在一定程度上解决了数据依赖和产品缺乏某些特征的问题。**

请注意，上面列出的带`*`的特征是我最后添加的一批特征。问题是，训练了这些特征的数据模型的表现比之前的模型差。起初我认为特征数量的增加会要求重新调整模型参数。然而，在浪费了大量CPU时间进行网格搜索后，我仍然无法超越旧模型。我认为这可能是上述**特征相关性**的问题。我实际上知道一个可能有效的解决方案，即**通过堆叠将不同特征版本训练的模型进行组合**。不幸的是，我没有足够的时间尝试它。**事实上，大多数顶尖团队认为使用不同预处理和特征工程管道训练的模型的集成是成功的关键**。

**模型**

起初我使用`RandomForestRegressor`来构建模型。然后我尝试了**Xgboost**，结果发现它比Sklearn快了两倍以上。从那时起，我每天做的基本上就是在工作站上运行网格搜索，同时在笔记本上处理特征。

本次比赛的数据集验证并不简单。数据不是独立同分布的，许多记录是相关的。我多次使用更好的特征/参数，结果LB分数却更差。正如许多成功的Kaggle选手所反复强调的那样，在这种情况下，你必须相信自己的CV分数。因此，我决定在交叉验证中使用10折而不是5折，并在接下来的尝试中忽略LB分数。

**集成**

我的最终模型是由4个基础模型组成的集成模型：

+   `RandomForestRegressor`

+   `ExtraTreesRegressor`

+   `GradientBoostingRegressor`

+   `XGBRegressor`

堆叠模型也是一个`XGBRegressor`。

问题在于我所有的基础模型高度相关（最低相关系数为0.9）。我考虑将线性回归、SVM 回归和`XGBRegressor`（使用线性提升器）纳入集成模型，但这些模型的 RMSE 分数比我最终使用的4个模型高出0.02（这在排行榜上相当于数百个名次）。因此，我决定不再使用更多模型，尽管它们会带来更多的多样性。

好消息是，尽管基础模型高度相关，堆叠模型仍然大大提升了我的分数。**更重要的是，自从开始使用堆叠模型后，我的CV分数和LB分数完全同步。**

在比赛的最后两天，我还做了一件事：**使用大约20个不同的随机种子生成集成模型，并将它们的加权平均作为最终提交**。这实际上是一种**袋装法（bagging）**。理论上是有意义的，因为在堆叠中，我使用80%的数据来训练每次迭代中的基础模型，而100%的数据用于训练堆叠模型。因此它的效果不够干净。进行多次不同种子的运行可以确保**每次使用不同的80%数据**，从而降低信息泄露的风险。然而，通过这种方式，我只取得了`0.0004`的提升，这可能仅仅是由于随机性造成的。

比赛结束后，我发现我最好的单一模型在私人排行榜上的分数是`0.46378`，而我最好的堆叠集成模型的分数是`0.45849`。这代表了174名和98名之间的差距。换句话说，特征工程和模型调优让我进入了前10%，而堆叠模型让我进入了前5%。

**经验教训**

从顶级团队分享的解决方案中有很多值得学习的地方：

+   产品标题中有一种模式。例如，产品是否附带某种配件将通过标题末尾的`With/Without XXX`来指示。

+   使用外部数据。例如使用[WordNet](https://wordnet.princeton.edu/)或[Reddit 评论数据集](https://www.kaggle.com/reddit/reddit-comments-may-2015)来训练同义词和[上位词](https://en.wikipedia.org/wiki/Hyponymy_and_hypernymy)。

+   一些基于**字母**而不是**词语**的特征。一开始我对此感到困惑。但如果考虑到这一点，就会有意义。例如，获得第三名的团队在计算文本相似性时考虑了匹配的字母数量。他们认为**较长的词语更为具体，因此更有可能被人类赋予高相关性分数**。他们还使用逐字比较 (`difflib.SequenceMatcher`) 来测量**视觉相似性**，他们认为这对人类很重要。

+   对词语进行词性标注，找到**[头词](https://en.wikipedia.org/wiki/Head_(linguistics))**并在计算各种距离度量时使用它们。

+   从产品标题/描述字段的 TF-IDF 中提取排名靠前的三元组，并计算搜索词出现在这些三元组中的比率。反之亦然。这就像从另一个角度计算潜在的索引。

+   一些新颖的距离度量方法，如[Word Movers Distance](http://jmlr.org/proceedings/papers/v37/kusnerb15.pdf)。

+   除了 SVD，部分人使用了[NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization)。

+   生成**配对多项式交互**在排名靠前的特征之间。

+   **对于交叉验证，构建训练集和测试集中产品 ID 不重叠的分割，以及 ID 重叠的分割。然后我们可以使用这些分割和相应的比率来估算公共/私人 LB 分割在本地交叉验证中的影响。**

### 总结

**要点**

1.  **在竞赛早期开始进行集成**是一个明智的选择。结果是，我在最后几天仍在处理特征。

1.  建立一个能够自动训练模型和记录最佳参数的管道是优先事项。

1.  **特征最为重要！** 我在这次竞赛中没有花足够的时间在特征上。

1.  如果可能，花一些时间手动检查原始数据中的模式。

**提出的问题**

我在这次竞赛中遇到的几个问题具有很高的研究价值。

1.  如何在相关数据下进行可靠的交叉验证。

1.  如何量化**多样性与准确性之间的权衡**在集成学习中。

1.  如何处理对模型性能有害的特征交互。以及**如何确定新特征在这种情况下是否有效**。

**初学者建议**

1.  选择一个你感兴趣的竞赛。**如果你对问题领域已有一些见解会更好。**

1.  无论是遵循我的方法还是别人的方法，开始探索、理解和建模数据。

1.  从论坛和脚本中学习。查看其他人如何解释数据和构建特征。

1.  **查找之前比赛的获胜者访谈/博客文章。这些非常有帮助，特别是如果来自与你正在进行的比赛有相似之处的比赛。**

1.  在你达到相当好的分数（例如 10% ~ 20%）后，或者你觉得没有太多新特征的空间时（这通常是错误的），开始使用集成方法。

1.  如果你认为自己有机会赢得奖项，试着组队合作！

1.  **不要在比赛结束前放弃。至少每天尝试一些新东西。**

1.  从比赛后的顶级团队分享中学习。反思你的方法。**如果可能，花些时间验证你所学到的东西。**

1.  休息一下吧！

### 参考文献

1.  [轻松战胜 Kaggle - 董颖](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiPxZHewLbMAhVKv5QKHb3PCGwQFggcMAA&url=http%3A%2F%2Fwww.ke.tu-darmstadt.de%2Flehre%2Farbeiten%2Fstudien%2F2015%2FDong_Ying.pdf&usg=AFQjCNE9o2BcEkqdnu_-lQ3EFD3eRAFWiw&sig2=oiU8TCEH57EYF9v9l6Scrw&bvm=bv.121070826,d.dGo)

1.  [搜索结果相关性获胜者访谈：第一名，程龙陈](https://github.com/ChenglongChen/Kaggle_CrowdFlower/blob/master/BlogPost/BlogPost.md)

1.  [(中文) 保诚人寿保险评估解决方案 - Nutastray](http://rstudio-pubs-static.s3.amazonaws.com/158725_5d2f977f4004490e9b095c0ef9357c6b.html)

![Linghao Zhang](../Images/47effce75b02d75ca99360d2793970ea.png)**简介：[Linghao Zhang](https://www.linkedin.com/in/linghaozh)** 是复旦大学计算机科学大四学生和 Strikingly 的数据挖掘工程师。他的兴趣包括机器学习、数据挖掘、自然语言处理、知识图谱和大数据分析。

[原文](https://dnc1994.com/2016/05/rank-10-percent-in-first-kaggle-competition-en/)。经授权转载。

**相关：**

+   [接近（几乎）任何机器学习问题](/2016/08/approaching-almost-any-machine-learning-problem.html)

+   [自动化数据科学与机器学习：与 Auto-sklearn 团队的访谈](/2016/10/interview-auto-sklearn-automated-data-science-machine-learning-team.html)

+   [数据科学基础：集成学习者简介](/2016/11/data-science-basics-intro-ensemble-learners.html)

### 更多相关内容

+   [LinkedIn 如何利用机器学习对你的动态进行排名](https://www.kdnuggets.com/2022/11/linkedin-uses-machine-learning-rank-feed.html)

+   [如何在没有任何工作经验的情况下获得数据科学的第一份工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [使用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [它活了！使用 Python 和一些便宜的组件构建你的第一个机器人](https://www.kdnuggets.com/2023/06/manning-build-first-robots-python-cheap-basic-components.html)

+   [从零到英雄：使用 PyTorch 创建你的第一个机器学习模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)

+   [部署你的第一个机器学习模型](https://www.kdnuggets.com/deploying-your-first-machine-learning-model)
