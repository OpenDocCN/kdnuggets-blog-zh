# R 与 Python 在数据科学中的对决：胜者是…

> 原文：[https://www.kdnuggets.com/2015/05/r-vs-python-data-science.html](https://www.kdnuggets.com/2015/05/r-vs-python-data-science.html)

由**[Martijn Theuwissen](https://www.kdnuggets.com/author/martijn-theuwissen "Posts by Martijn Theuwissen")**于2015年5月26日在[数据科学工具](https://www.kdnuggets.com/tag/data-science-tools)、[DataCamp](https://www.kdnuggets.com/tag/datacamp)、[Python](https://www.kdnuggets.com/tag/python)、[Python 与 R](https://www.kdnuggets.com/tag/python-vs-r)、[R](https://www.kdnuggets.com/tag/r)上发表。

在[DataCamp](https://www.datacamp.com/)上，我们的学生经常询问他们是否应该使用 R 和/或 Python 来处理日常的数据分析任务。虽然我们主要提供[交互式 R 教程](https://www.datacamp.com/courses)，但我们总是回答，这一选择取决于他们面临的数据分析挑战的类型。

Python 和 R 都是流行的统计编程语言。虽然 R 的功能是为统计学家开发的（比如 R 强大的数据可视化能力！），Python 由于其易于理解的语法而受到赞誉。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

在这篇文章中，我们将突出 R 和 Python 之间的一些差异，以及它们在数据科学和统计世界中的各自角色。如果你喜欢视觉表现，务必查看相关的信息图表 [“数据科学战争：R 与 Python”。](http://blog.datacamp.com/r-or-python-for-data-analysis/)

**介绍 R**

[Ross Ihaka 和 Robert Gentleman](https://en.wikipedia.org/wiki/R_%28programming_language%29)在1995年创建了开源语言 R，作为 S 编程语言的实现。其目的是开发一种语言，专注于提供更好、更用户友好的数据分析、统计和图形模型的方法。最初，R 主要用于学术和研究，但最近企业界也开始发现 R。这使得 R 成为企业界增长最快的统计语言之一。

R的主要优势之一是其庞大的社区，通过邮件列表、用户贡献的文档和非常活跃的Stack Overflow小组提供支持。还有[CRAN](http://cran.r-project.org/)，这是一个巨大的R包库，用户可以轻松贡献。 这些包是R函数和数据的集合，使得可以立即获得最新的技术和功能，而无需从头开始开发所有内容。

最后，如果你是一个有经验的程序员，你可能不会难以适应R。然而，作为初学者，你可能会发现自己在陡峭的学习曲线中挣扎。幸运的是，现在有许多很好的学习资源可以咨询。

**介绍Python**

[Python](https://en.wikipedia.org/wiki/Python_%28programming_language%29) 是由Guido Van Rossem于1991年创建的，强调生产力和代码可读性。那些希望深入数据分析或应用统计技术的程序员是Python在统计领域的主要用户之一。

当你在工程环境中工作时，你可能会更喜欢Python。它是一种灵活的语言，非常适合做一些新颖的事情，而且由于其关注可读性和简洁性，学习曲线相对较低。

与R类似，Python也有包。[PyPi](https://pypi.python.org/pypi)是Python包索引，包含用户可以贡献的库。就像R一样，Python有一个很棒的社区，但由于它是通用语言，所以稍微分散。然而，Python在数据科学中的应用正迅速在Python世界中占据主导地位：期望正在增长，更多创新的数据科学应用将在这里诞生。

**R与Python：常用数字**

在网上，你可以找到许多比较R和Python采用情况和受欢迎程度的数字。虽然这些数据通常很好地指示了这两种语言在计算机科学整体生态系统中的演变情况，但将它们并排比较是困难的。 主要原因是你会发现R只存在于数据科学环境中； 另一方面，作为一种通用语言，Python广泛应用于许多领域，如Web开发。这通常会使排名结果倾向于Python，而薪资受到一些负面影响。

![R与Python数字](../Images/0eb09d69ebb8a6b50ef17b89287ac9a4.png) ****何时以及如何使用R？**

R主要用于数据分析任务需要独立计算或在单独服务器上进行分析时。它非常适合探索性工作，并且由于大量的包和现成的测试，几乎适用于任何类型的数据分析，这些工具通常为你提供必要的工具，让你快速上手。R甚至可以成为大数据解决方案的一部分。

在开始使用 R 时，一个好的第一步是安装令人惊叹的 [RStudio IDE](http://www.rstudio.com/products/rstudio/)。完成后，我们建议你查看以下一些流行的包：

+   [dplyr](http://www.rdocumentation.org/packages/dplyr)、[plyr](http://www.rdocumentation.org/packages/plyr) 和 [data.table](http://www.rdocumentation.org/packages/data.table) 用于轻松操作数据包，

+   [stringr](http://www.rdocumentation.org/packages/stringr) 用于操作字符串，

+   [zoo](http://www.rdocumentation.org/packages/zoo) 用于处理规则和不规则时间序列，

+   [ggvis](http://www.rdocumentation.org/packages/ggvis)、[lattice](http://www.rdocumentation.org/packages/lattice) 和 [ggplot2](http://www.rdocumentation.org/packages/ggplot2) 用于数据可视化，并且

+   [caret](http://www.rdocumentation.org/packages/caret) 用于机器学习

**何时以及如何使用 Python？**

**当你的数据分析任务需要与网页应用集成，或统计代码需要整合到生产数据库中时，你可以使用 Python。作为一种完整的编程语言，它是实现生产使用算法的绝佳工具。**

**虽然 Python 数据分析包的初期阶段曾是一个问题，但这些年已经有了显著改善。确保安装 [NumPy](http://www.numpy.org/) /SciPy（科学计算）和 [pandas](http://pandas.pydata.org/)（数据处理），使 Python 可用于数据分析。同时查看一下 [matplotlib](http://matplotlib.org/) 制作图形，以及 [scikit-learn](http://scikit-learn.org/stable/) 进行机器学习。**

与 R 不同，Python 并没有明确的“获胜”IDE。我们建议你查看一下 [Spyder](https://pythonhosted.org/spyder/)、[IPython Notebook](http://ipython.org/notebook.html) 和 [Rodeo](https://github.com/yhat/rodeo/)，看看哪个最适合你的需求。

**R 和 Python：数据科学数字**

**如果你查看最近针对数据分析编程语言的调查，R 通常是明显的赢家。如果专注于 Python 和 R 的数据分析社区，类似的模式也会出现。**

**![R vs Python Activity](../Images/ada7e91400069e11f1feb5c6c723bcc0.png)**

尽管上述数据如此，但有迹象表明更多人正在从 R 转向 Python。此外，还有一部分人群在适当的时候同时使用这两种语言。这正符合我们对学生的建议。

如果你计划开始数据科学的职业生涯，两种语言都很重要。就业趋势显示对这两种技能的需求增加，薪资远高于平均水平。

**R：优缺点**

##### *优点：一张图胜过千言万语*

可视化数据通常比单纯的原始数字更易于理解和有效。R 和可视化是完美的组合。一些必看的可视化包包括 ggplot2、ggvis、googleVis 和 rCharts。

##### *优点：R 生态系统*

R 拥有丰富的前沿包和活跃的社区。包可在 CRAN、BioConductor 和 Github 上获得。你可以在 [Rdocumentation](http://www.rdocumentation.org/) 上搜索所有 R 包。

##### *优点：R 是数据科学的通用语言*

R 是由统计学家为统计学家开发的。他们可以通过 R 代码和包传达思想和概念，你不一定需要计算机科学背景就能入门。此外，它在学术界之外的采纳率也在增加。

##### *优缺点：R 较慢*

R 的开发目的是为了让统计学家的工作更轻松，而不是为了让你的计算机更轻松。尽管 R 由于代码编写不佳可能被认为较慢，但有多个包可以提高 R 的性能：pqR、renjin 和 FastR、Riposte 等。

##### *缺点：R 的学习曲线陡峭*

R 的学习曲线并不简单，特别是如果你习惯于图形用户界面进行统计分析的话。如果不熟悉，寻找包也可能会非常耗时。

**Python：优缺点**

##### *优点：IPython Notebook*

IPython Notebook 使得使用 Python 和数据变得更容易。你可以轻松地与同事共享笔记本，而不需要他们安装任何东西。这大大减少了组织代码、输出和笔记文件的开销，让你有更多时间进行实际工作。

##### *优点：通用语言*

Python 是一种通用语言，易于学习且直观。这使得其学习曲线相对平缓，加快了编写程序的速度。简而言之，你需要更少的编码时间，有更多时间来玩耍！

此外，Python 测试框架是一个内置的、入门门槛低的测试框架，鼓励良好的测试覆盖率。这确保了你的代码是可重用和可靠的。

##### *优点：多用途语言*

Python 把背景各异的人们聚集在一起。作为一种通用、易于理解的语言，程序员都知道，统计学家也能轻松学习，你可以构建一个与工作流每个部分都集成的工具。

##### *优缺点：可视化*

可视化在选择数据分析软件时是一个重要标准。尽管 Python 有一些不错的可视化库，如 Seaborn、Bokeh 和 Pygal，但可能选择过多。此外，与 R 相比，可视化通常更复杂，结果也不总是那么令人满意。

##### *缺点：Python 是一个挑战者*

Python 是 R 的挑战者。它没有提供数百个必要的 R 包的替代品。虽然它在追赶，但是否会让人们放弃 R 仍然不明确。

**最终赢家是..**

**由你决定！作为数据科学家，你的工作是选择最适合需求的语言。一些可以帮助你的问题：**

**1. 你想解决哪些问题？**

1.  学习一门语言的实际成本是多少？

1.  在你的领域中，常用的工具有哪些？

1.  还有其他可用的工具吗？这些工具与常用工具有何关系？

希望这对你有帮助！

**关于 DataCamp**

[DataCamp](https://www.datacamp.com "Kaggle 机器学习教程 R") 是一个在线互动教育平台，提供数据科学和 R 编程课程。每门课程围绕特定的数据科学主题构建，并结合视频讲解和浏览器内编码挑战，让你通过实践学习。[你可以随时随地免费开始每门课程](https://www.datacamp.com/courses "R 教程和课程")。

**相关：**

+   [R 领先 RapidMiner，Python 迎头赶上，大数据工具增长，Spark 点燃](/2015/05/poll-r-rapidminer-python-big-data-spark.html)

+   [数据科学的语法：Python 与 R](/2015/03/the-grammar-data-science-python-vs-r.html/3)

+   [顶级 KDnuggets 推文，4月2-5日：数据科学生态系统：数据整理的实用工具和技巧](/2015/04/top-tweets-apr02-05.html)

### 更多相关话题

+   [宣布博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [通过《快速 Python 数据科学》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [KDnuggets 新闻，5月4日：9 门免费哈佛课程学习数据…](https://www.kdnuggets.com/2022/n18.html)

+   [优化 Python 代码性能：深入探讨 Python 性能分析工具](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python 枚举：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [数据科学、数据可视化及…的 38 个顶级 Python 库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)********
