# 机器学习的顶级 R 包

> 原文：[`www.kdnuggets.com/2017/02/top-r-packages-machine-learning.html`](https://www.kdnuggets.com/2017/02/top-r-packages-machine-learning.html)

**由 [Michael Li](https://github.com/tianhuil/) 和 [Paul Paczuski](https://github.com/pavopax/)，The Data Incubator。**

在 The Data Incubator [The Data Incubator](https://www.thedataincubator.com/)，我们以拥有最新的数据科学课程为荣。我们的课程很大一部分基于来自企业和政府合作伙伴的反馈，了解他们希望学习的技术。但我们希望采用更加数据驱动的方法来确定我们在 [数据科学企业培训](https://www.thedataincubator.com/training.html) 和为希望进入行业的数据科学职业的硕士和博士提供的 [免费奖学金](https://www.thedataincubator.com/fellowship.html) 中应该教授的内容。以下是结果。

### 排名

* * *

## 我们的前三课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

最受欢迎的机器学习包有哪些？让我们来看看基于包下载和社交网站活动的排名。

![](https://github.com/thedataincubator/data-science-blogs/blob/master/img/rank.png)

*值得注意的：OneR: 1 (SO); mlr: 2 (Github); ranger: 4 (Github); SuperLearner: 5 (Github)*

排名基于 CRAN ([The Comprehensive R Archive Network](https://cran.r-project.org/)) 下载的平均排名和 Stack Overflow 活动 ([完整排名见 [CSV] ](https://github.com/thedataincubator/data-science-blogs/blob/master/ranking.csv))。CRAN 下载数据来自过去一年。Stack Overflow 根据问题正文中的包名以及 'R' 标签的结果数量进行排名。GitHub 排名基于仓库的星标数。详见 [方法](https://github.com/thedataincubator/data-science-blogs/blob/master/top-r-packages.md#Methods)。

### Caret 位居首位，神经网络在其他算法重磅选手中表现突出

也许并不令人惊讶，`caret` 排在首位。它是一个用于创建机器学习工作流的通用包，与一些特定算法的包良好集成，这些包在排名中紧随其后。

这些包括 `e1071`（用于 SVM）、`rpart`（树）、`glmnet`（<wbr>正则化回归）以及，可能对 R 来说比较意外的神经网络（`nnet`）。有关这些包的详细信息，请参见 [下面](https://github.com/thedataincubator/data-science-blogs/blob/master/top-r-packages.md#Package-details)。

排名揭示了 R 包社区的碎片化。几个顶级包，如 `rpart` 和 `tree`，实现了相同的算法，这与 Python 的 `scikit-learn` 的一致性 - 和广度 - 形成了对比。

尽管如此，如果你喜欢 R 的数据处理能力（如 `tidyverse`），你可以使用这些包来进行一些强大的建模，而不必切换到 Python。此外，随着更多功能被添加到 [`modelr`](https://github.com/hadley/modelr)，一个 `tidy 工具`，我们可能很快会看到它被纳入这个列表中。

### 包详情

`caret` 是一个用于创建机器学习工作流的通用包，并在这个排名中位居榜首。接下来是一些实现特定机器学习算法的包：随机森林（`randomForest`）、支持向量机（`e1071`）、分类和回归树（`rpart`）以及正则化回归模型（`glmnet`）。

`nnet` 实现了神经网络，而 `tree` 包则实现了树结构。`party` 用于递归分割和二叉树的可视化，`arules` 用于关联挖掘。SVM 和其他核方法在 `kernlab` 中实现。`h2o` 包用于可扩展的机器学习，并且是更大 H2O 项目的一部分。`ROCR` 用于模型评估，包括 ROC 曲线，而 `gbm` 实现了梯度提升。更多的分割算法可以通过 `RWeka` 访问，而 `rattle` 是一个用于数据挖掘的 R 图形用户界面。

一些包在 Github 上表现突出：`mlr` 和 `SuperLearner` 是另外两个提供类似功能的元包，而 `ranger` 提供了随机森林的 C++ 实现。

最后，`OneR` 在 Stack Overflow 上排名第一，但 SO API 经常将其自动更正为“one”，所以结果不可靠。

### 方法

下面，我们描述了得出这个排名的方法论。

**第 1 步：获取一个详尽的机器学习包列表**

从一开始，我们就设想我们的排名建立在包下载量、Stack Overflow 和 Github 活动的结合上。我们知道存在一些 API 可以提供这些指标。

然而，获得所有 R 包的初步机器学习列表是一项更艰巨的任务。这个列表需要是详尽的、客观的且最新的。一个糟糕的初始列表将会显著影响我们的排名。

询问周围的人有所帮助。一位朋友向我们推荐了 ["CRAN 任务视图：机器学习与统计学习"](https://cran.r-project.org/web/views/MachineLearning.html)，它在底部有一个很好的列表，并且易于抓取。

它的优点在于，软件包列表来自权威来源（CRAN 是“官方” R 包存储库），并且定期更新（最后更新时间：2017 年 1 月 6 日）。感谢作者，[托斯滕·霍瑟恩](http://user.math.uzh.ch/hothorn/)，他通过电子邮件也非常响应。

之前的想法是使用 Google 查找“顶级 R 机器学习包”的列表，然后尝试抓取所有包名称，进行合并，并将该列表作为起点。但抛开工程任务，我们还发现当前可用的列表相对于我们的需求质量较差。它们过时，没有明确指定方法，且往往非常主观。

**确定客观指标**

一个好的排名需要对“最佳”有明确的定义，并且需要使用好的指标构建。

我们将“最佳”定义为“最受欢迎”。这不一定意味着这些包被广泛喜爱（用户可能因为 API 很糟糕而频繁搜索 Stack Overflow）。

我们为排名选择了 3 个组件：

+   下载量：来自 CRAN 镜像的下载次数

+   Github：包在其主仓库页面上的星标数量

+   Stack Overflow：包含包名称并标记为 'R' 的问题数量

**CRAN 下载量**

有几个 CRAN 镜像，我们使用了 R-Studio 镜像，因为它具有方便的 API。RStudio 可能是最广泛使用的 R IDE，但它不是唯一的。如果我们汇总了其他 CRAN 镜像的下载量，我们的排名可能会有所改进（虽然可能不会显著）。

**GitHub**

最初，我们通过查询 Github 的搜索 API 来寻找包的 Github 页面，可能带有“language:R”，但这被证明不可靠。有时很难挑选出正确的 Github 仓库，并且并非所有 R 包都是用 R 语言实现的（搜索 API 中的“language:R”参数似乎指的是仓库使用的最流行的语言）。

相反，我们回到 CRAN 寻找这些网址。每个包都有一个官方 CRAN 页面，其中包括有用的信息，包括源代码链接。这就是我们获取包的 Github 仓库位置的地方。

之后，通过 API 获取 Github 星标很容易。

**Stack Overflow**

从 Stack Overflow 获取有用结果很棘手。一些 R 包名如 `tree` 和 `earth` 存在明显的困难：Stack Overflow 的结果可能没有过滤到仅针对 R 包的结果，因此我们首先在查询中添加了一个 'r' 字符串，这大大帮助了我们。

一个好的（最佳的？）策略是寻找问题正文中的包名，然后添加 'r' 标签（这与添加 'r' 字符串不同）。

### 构建排名

我们仅在每个 3 个指标中对包进行排名，并取其平均排名。没有复杂的操作。

### 其他注意事项

所有数据于 2017 年 1 月 19 日下载。CRAN 下载量数据来自过去 365 天：2016 年 1 月 19 日至 2017 年 1 月 19 日。

### 数据科学的顶级 R 包？

这个项目起初是对“数据科学”顶级包的排名，但我们很快发现范围太广。

数据科学家做许多不同的工作，你几乎可以将任何 R 包归类为帮助数据科学家的工具。我们是否应该包括字符串处理包？数据库读取包怎么样？

另一个更长的项目可以是使用更多的“数据科学”来制定一个关于“数据科学”顶级 R 包的排名。

### 资源

源代码可在[The Data Incubator](https://www.thedataincubator.com/)'s [Github](https://github.com/thedataincubator/data-science-blogs/)上获得。如果你有兴趣了解更多，请考虑：

1.  [数据科学企业培训](https://www.thedataincubator.com/training.html)

1.  [面向希望进入行业的硕士和博士的免费八周奖学金](https://www.thedataincubator.com/fellowship.html)

1.  [招聘数据科学家](https://www.thedataincubator.com/hiring.html)

**相关：**

+   按受欢迎程度排列的前 20 个 R 包

+   R 中的 ARIMA 预测介绍

+   利用 Anaconda 轻松构建和分享 R 包

### 更多相关话题

+   [2023 年需要了解的顶级数据 Python 包](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

+   [3 个用于数据可视化的 Julia 包](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [KDnuggets 新闻，6 月 22 日：主要监督学习算法…](https://www.kdnuggets.com/2022/n25.html)

+   [每个机器学习工程师都应该掌握的 5 项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [KDnuggets 新闻，12 月 14 日：3 门免费的机器学习课程…](https://www.kdnuggets.com/2022/n48.html)
