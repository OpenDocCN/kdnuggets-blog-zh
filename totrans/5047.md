# 学习 Python 以进行数据分析和数据科学的综合指南

> 原文：[https://www.kdnuggets.com/2016/04/datacamp-learning-python-data-analysis-data-science.html](https://www.kdnuggets.com/2016/04/datacamp-learning-python-data-analysis-data-science.html)

Python 被广泛用于数据分析，你可能已经考虑过自己学习它（如果没有，或者你仍在寻找额外的动力来开始，看看下面的理由为何你应该学习 Python）。当然，自学可能会有挑战，一些指导总是有帮助的。本文正是为你提供了学习 Python 以处理数据的指导。

我们将讨论你应该采取的学习 Python 的步骤，并提供一些基本资源，例如 DataCamp 的免费 [数据分析 Python 课程和教程](https://www.datacamp.com/courses/intro-to-python-for-data-science) 以及阅读和学习材料。

**步骤 0：学习 Python 的理由**

为什么要学习 Python 作为数据分析工具？

+   它是一个流行的数据分析工具：首先，Python 本身就是 [最受欢迎的数据分析工具之一](/polls/2015/analytics-data-mining-data-science-software-used.html)。35% 的数据科学家使用 Python，它领先于 SQL 和 SAS，仅次于 R。

+   通用编程语言：尽管还有其他非常流行且优秀的计算工具用于数据分析（例如 R、SAS），Python 是唯一真正的通用编程语言。查看这个 [信息图表](https://www.datacamp.com/community/tutorials/r-or-python-for-data-analysis) 获取更详细的比较。

+   流行编程语言：此外，与其他通用编程语言（例如 Java、C++、PHP）相比，Python 是 [最受欢迎的编程语言之一](http://blog.codeeval.com/codeevalblog/2016/2/2/most-popular-coding-languages-of-2016)。

+   如果这还不够，Python 还是 [顶级美国大学计算机科学教学的首选语言](http://cacm.acm.org/blogs/blog-cacm/176450-python-is-now-the-most-popular-introductory-teaching-language-at-top-us-universities/fulltext)。

作为旁注：正如在 [“R 或 Python？考虑同时学习两者”](/2016/03/r-python-learning-both-datacamp.html) 中所描述的，我们不建议你只学习 Python 而忽略其他。然而，学习 Python 是你职业生涯中可以做的最好的事情之一。Python 被计算机科学家广泛采用，有很多理由，而成为众多数据分析工具的首选主要是因为学习和使用 Python 的便利性。尽管如此，设定学习路径可能会有挑战，所以这就是我们现在要做的。

![Python](../Images/27283ea3616968fb7071321afe354d09.png)

**步骤 1：为数据分析设置 Python 环境**

+   从 Continuum Analytics 下载 Python：[ANACONDA](https://www.continuum.io/downloads)

+   可选（从 Yhat 下载 Rodeo）：[Rodeo IDE](http://rodeo.yhat.com/)

设置 Python 环境以进行数据分析相对简单。最方便的方法是从 Continuum Analytics 下载免费的 [Anaconda 套件](https://www.continuum.io/downloads)，它包含了核心的 Python 语言，以及所有必需的库，包括 NumPy、Pandas、SciPy、Matplotlib 和 IPython。通过使用图形安装程序，下载 Python 就像下载任何计算机程序一样简单。

安装后，你将获得一个包含多个程序的启动器。最重要的程序是 iPython 笔记本，也叫做 [Jupyter](http://jupyter.org/) 笔记本。一旦你启动笔记本，终端将打开，并且笔记本将在你的浏览器中打开。不要感到困惑！你不需要互联网连接来创建或使用笔记本。简单来说，浏览器被用作环境，而不是独立的程序，你可以在其中编写代码。

然而，你不局限于使用基于浏览器的 Jupyter 笔记本。如果你更喜欢集成开发环境（IDE），Yhat 提供的 [Rodeo](https://www.yhat.com/products/rodeo) 是一个很好的数据分析选择。如果你对 [RStudio](https://www.rstudio.com/) 熟悉，那么 Rodeo 对 Python 来说非常类似。务必尝试这两种选择，毕竟，你使用的 Python 环境最终将取决于你的个人偏好。

![datacamp-python-3](../Images/8b256126b25f20d88a0d689a992f9113.png)

**步骤 2：学习基础知识和基本原理**

现在你准备开始学习 Python 编程了。有几种很好的方法可以做到这一点。鉴于你希望学习 Python 进行数据分析，最好的选择是 DataCamp 提供的 [Introduction for Python for Data Science](https://www.datacamp.com/courses/intro-to-python-for-data-science) 课程。这个免费的课程包含视频教程和互动浏览器练习，是一种通过实践学习的好方法，而不是单纯阅读概念和查看示例。你不会通过阅读书籍来学习绘画。你会拿起画笔开始绘画。这就是我们建议你开始学习 Python 的方式！除了入门课程，DataCamp 还提供了 [Intermediate Python for Data Science](https://www.datacamp.com/courses/intermediate-python-for-data-science) 课程，进一步提升你的技能。

另一个相当有用的资源是 [Codecademy](https://www.codecademy.com/learn/python) 的 Python 课程。虽然这个课程并不专注于数据，而是 Python 编程，但它是练习 Python 语法和获得编程概念的好方法，这些概念在处理数据时对你会很有帮助。

**步骤 3：用于数据分析的 Python 包**

Python 是一种通用语言，通常用于数据分析和数据科学以外的其他任务。然而，Python 在处理数据时极其有用的原因是它的库，这些库为用户提供了必要的功能。以下是用于数据处理的主要 Python 库。你应该花时间了解这些包的基本用途。

+   [Numpy](http://www.numpy.org/) 和 [Scipy](http://www.scipy.org/about.html) – 基础科学计算。

+   [Pandas](http://pandas.pydata.org/) – 数据处理和分析。

+   [Matplotlib](http://matplotlib.org/) – 绘图和可视化。

+   [Scikit-learn](http://scikit-learn.org/stable/index.html) – 机器学习和数据挖掘。

+   [StatsModels](http://statsmodels.sourceforge.net/) – 统计建模、测试和分析。

![datacamp-python-2](../Images/8cf6f8f20eca501b50bd10f08cb0a60d.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护。

* * *

### 更多相关内容

+   [NLP、NLU 和 NLG：有什么不同？全面指南](https://www.kdnuggets.com/2022/06/nlp-nlu-nlg-difference-comprehensive-guide.html)

+   [使用 Google 的 NotebookLM 进行数据科学：全面指南](https://www.kdnuggets.com/using-google-notebooklm-for-data-science-a-comprehensive-guide)

+   [数据分析师必备工具全面指南](https://www.kdnuggets.com/a-comprehensive-guide-to-essential-tools-for-data-analysts)

+   [正态分布全面指南](https://www.kdnuggets.com/2022/06/comprehensive-guide-normal-distribution.html)

+   [MLOps 全面指南](https://www.kdnuggets.com/2023/08/comprehensive-guide-mlops.html)

+   [卷积神经网络全面指南](https://www.kdnuggets.com/2023/06/comprehensive-guide-convolutional-neural-networks.html)
