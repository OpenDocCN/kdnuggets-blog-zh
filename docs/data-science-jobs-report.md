# 数据科学职位报告 2019：Python 大幅上升，TensorFlow 快速增长，R 使用量翻倍超过 SAS

> 原文：[`www.kdnuggets.com/2019/06/data-science-jobs-report.html`](https://www.kdnuggets.com/2019/06/data-science-jobs-report.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者 [Bob Muenchen](http://r4stats.com/)，分析分析世界**。

在我持续追踪*[数据科学软件的普及度](http://r4stats.com/articles/popularity/)*的过程中，我刚刚更新了我的就业市场分析。为了避免你阅读整个文档，我在这里复制了该部分内容。

### **职位广告**

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

测量数据科学软件的普及度或市场份额的最佳方法之一是计算那些将知识作为要求的职位广告数量。职位广告信息丰富且有资金支持，因此它们可能是衡量每种软件当前受欢迎程度的最佳指标。职位需求的变化图表为我们提供了未来可能会变得更加流行的良好线索。

Indeed.com 是美国最大的招聘网站，使其职位广告收集成为最好的选择。正如他们的联合创始人兼前首席执行官 Paul Forster 所述，Indeed.com 包括“来自 1000 多个独特来源的所有职位，包括主要的招聘网站 – Monster、CareerBuilder、HotJobs、Craigslist – 以及数百份报纸、协会和公司网站。” Indeed.com 还拥有出色的搜索功能。它曾经有一个职位趋势图表工具，但这个工具显然已经关闭。

使用 Indeed.com 搜索职位很容易，但以确保在不同包之间公平比较的方式搜索软件则具有挑战性。一些软件仅用于数据科学（例如 SPSS、Apache Spark），而其他软件既用于数据科学职位，也用于更广泛的报告撰写职位（例如 SAS、Tableau）。通用语言（例如 Python、C、Java）在数据科学职位中被广泛使用，但绝大多数使用这些语言的职位与数据科学无关。为了公平比较，我制定了一个协议，将每个软件的搜索范围仅限定于数据科学家的职位。该协议的详细信息在另一篇文章中描述，* [如何搜索数据科学职位](http://r4stats.com/articles/how-to-search-for-data-science-jobs/) *。本节中的所有图表都使用这些程序来进行所需的查询。

我在 2019 年 5 月 27 日和 2017 年 2 月 24 日收集了本节讨论的职位数量。有人可能会认为单一天的样本可能不太稳定，但大量的职位来源使得 Indeed.com 的职位数量相当一致。使用相同协议收集的 2017 年和 2014 年的数据的相关系数为 r=.94，p=.002。

图 1a 显示 Python 以 27,374 个职位领先，其次是 SQL 的 25,877 个职位。Java 和 Amazon 的机器学习 (ML) 工具的职位数量大约低 25%，在 17,000 个左右。R 和 C 变体排在其后，大约有 13,000 个职位。人们常常比较 R 和 Python，但在获得数据科学职位方面，R 的职位只有 Python 的一半。当然，这并不意味着这些职位是相同的。我仍然看到更多的统计学家使用 R 和机器学习领域的人更喜欢 Python，但 Python 的确正在蓬勃发展！从 Hadoop 开始，职位数量逐渐下降。R 也常常与 SAS 进行比较，SAS 只有 8,123 个职位，而 R 有 13,800 个。

图 1a 的比例尺非常宽，以至于底部的包 H20 看起来似乎为零，而实际上它有 257 个职位。

![](img/6ebdbc08c2b0b9a249b5e1dc93afbdae.png)

*图 1a。更受欢迎的软件的数据科学职位数量。*

为了比较不太受欢迎的软件，我在图 1b 中将它们单独绘制。Mathematica 和 Julia 是这个集合的领先者，每个大约有 219 个职位。古老的 FORTRAN 语言仍然顽强地保有 195 个职位。开源的 WEKA 软件和 IBM 的 Watson 紧随其后，各有大约 185 个职位。从 XGBOOST 开始，职位数量呈现出相当稳定的缓慢下降趋势。

有几个工具使用工作流界面：Enterprise Miner、KNIME、RapidMiner 和 SPSS Modeler。它们的职位数量都在 50 到 100 个之间。在许多其他受欢迎度指标中，RapidMiner 超过了非常相似的 KNIME 工具，但在这里后者的职位数量多出 50%。Alteryx 也是基于工作流的工具，但它已从其他工具中脱颖而出，在图 1a 上显示有 901 个职位。

![](img/7493328421ac7f8dbe9b87c7afbf76ff.png)

*图 1b. 低于 250 个广告的数据科学软件工具的职位数量。*

在解释图 1b 中的尺度时，看似零的确实是零。从 Systat 开始，没有一个软件包的工作列表超过 10 个。

重要的是要注意，图 1a 和图 1b 中显示的值是单一时间点的快照。更受欢迎的软件的职位数量每天变化不大。因此，图 1a 中显示的软件的相对排名在接下来的一两年内不太可能发生太大变化。图 1b 中显示的不太受欢迎的软件包的职位数量非常低，它们的排名更有可能从月到月发生变化，但与主要软件包相比，它们的位置应该保持更稳定。

接下来，让我们看看 2017 年到现在（2019 年）工作的变化。图 1c 显示了那些在 2017 年时至少有 100 个职位列表的软件包的百分比变化。如果没有这样的限制，软件从 2017 年的 1 个职位增加到 2019 年的 5 个职位将有 500%的增长，但仍然不会引起太大兴趣。工作市场正在升温或增长的软件以红色显示，而那些降温的软件以蓝色显示。

![](img/83dde99876d334754fa62a17a9264acd.png)

*图 1c. 从 2017 年到 2019 年职位列表的百分比变化。仅显示 2017 年时至少有 100 个职位的软件。*

Tensorflow 是来自 Google 的深度学习软件，其增长最快，达到 523%。接下来是 Apache Flink，一个分析流数据的工具，增长为 289%。H2O 紧随其后，增长为 150%。Caffe 是另一个深度学习框架，其 123%的增长反映了人工智能算法的流行。

Python 显示出“仅”97%的增长，但其受欢迎程度已经如此之高，以至于其增加的 13,471 个职位超越了许多其他软件包的总职位数！

Tableau 显示出类似的增长率，尽管增加的职位数相对较少，为 4,784 个。

从 Julia 语言开始，我们看到增长放缓。我惊讶于 SAS 和 SPSS 的职位仍在增长，尽管分别仅为 6%和 1%。

[原文](http://r4stats.com/2019/05/28/data-science-jobs-report-2019-python-way-up-tensorflow-growing-rapidly-r-use-double-sas/)。已获得许可转载。

**简介：** 罗伯特·A·穆恩琴（Robert A. Muenchen）是一个拥有 35 年经验的 ASA 认证专业统计师™，目前担任田纳西大学 OIT 研究计算支持（前身为统计咨询中心）经理。他是《*R for SAS and SPSS Users*》一书的作者，并与约瑟夫·M·希尔比（Joseph M. Hilbe）共同编写了《*R for Stata Users*》，Bob 也是 r4stats.com 的创建者，这是一个受欢迎的网站，致力于分析数据科学软件的趋势，评审这些软件，并帮助人们学习 R 语言。

**相关：**

+   [你需要知道的：现代开源数据科学/机器学习生态系统](https://www.kdnuggets.com/2019/06/top-data-science-machine-learning-tools.html)

+   [数据科学家 – 美国年度最佳职位](https://www.kdnuggets.com/2019/05/data-scientist-best-job-careercast.html)

+   [如何辨别一个好数据科学家职位与一个差的职位](https://www.kdnuggets.com/2019/04/recognize-good-data-scientist-job-from-bad.html)

### 了解更多此话题

+   [每个数据科学家都应该知道的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [通过数据科学将收入翻倍的 5 种方法](https://www.kdnuggets.com/2022/05/5-ways-double-income-data-science.html)

+   [停止学习数据科学以寻找目标，然后找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [是什么让 Python 成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成为伟大数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)
