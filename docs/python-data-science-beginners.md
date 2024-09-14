# Python 数据科学入门

> 原文：[https://www.kdnuggets.com/2019/02/python-data-science-beginners.html](https://www.kdnuggets.com/2019/02/python-data-science-beginners.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Saurabh Hooda](https://www.linkedin.com/in/hoodasaurabh/)，Hackr.io**

### 为什么选择 Python？

Python 是一种流行的高级面向对象编程语言，被大量软件开发人员广泛使用。Guido van Rossum 于 1991 年设计了它，Python 软件基金会进一步发展了它。但问题是，既然已经有数十种基于面向对象概念的编程语言，为什么还需要这一个新语言？因此，开发此语言的主要目的是强调代码的可读性和科学及数学计算（如 NumPy、SymPy、Orange）。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

Python 的语法非常简洁且长度短。Python 是一种开源且可移植的语言，支持一个庞大的标准库。

### 从一个 Python 示例开始

```py

# To Add Two Numbers
num1 = 1
num2 = 8
sum = num1+num2
print(sum)

```

**输出**

`9`

### 什么是数据科学？

![Image](../Images/22d0342dcca0d430046104de14c04693.png)

你一定听说过数据科学，但你对这个术语了解多少？谁能成为数据科学家？

数据科学是一个包含各种工具、数据接口和应用机器学习原理的集合，用于从原始数据中发现隐藏的模式。原始数据存储在企业数据仓库中，并以创意方式使用，以从中生成商业价值。

![Image](../Images/a11a8589bdff4f0ba154e6f546960d03.png)

通过这个信息图可以理解数据科学的应用。

![Image](../Images/a8f35408a3c7b211c99111d959ef615a.png)

数据分析师和数据科学家是不同的；数据分析师负责处理数据历史并解释发生了什么，而数据科学家需要各种高级机器学习算法，通过分析概念来识别特定事件的发生。

### Python 数据科学简介

有多种编程语言可以用于数据科学（如 SQL、Java、Matlab、SAS、R 等），但**Python** 是数据科学家在所有这些编程语言中最为偏好的选择。

Python 具有一些卓越的优点，包括：

+   Python 非常强大且简单，因此易于学习。如果你是初学者，不需要担心语法问题。

+   Python 支持许多平台，如 Windows、Mac、Linux 等。

+   Python 是一种高级编程语言，因此你可以用接近英语的简单语言编写程序，这将内部转换为低级代码。

+   Python 是一种解释型语言，这意味着它一次运行一条指令。

+   Python 可以进行数据可视化、数据分析和数据处理；NumPy 和 Pandas 是一些用于处理的库。

+   Python 提供了各种强大的库用于机器学习和科学计算。使用这种语言，可以在相对简单的语法中轻松进行各种复杂的科学计算和机器学习算法。

开发人员偏爱 Python 的原因有很多。我们需要定义一些术语来解释，从数据处理开始。

数据处理用于快速而轻松地提取、过滤和转换数据，并且结果高效。有两个重要的库用于执行这些任务：NumPy 和 Pandas。

**NumPy** 是一个开源库，可以在 Python 中免费使用，代表着 Numerical Python。它是一个流行的 Python 库，在科学计算中非常有用，提供了数组对象以及集成 C 和 C++ 的工具。NumPy 提供了一个强大的 N 维数组，以行和列的形式呈现。这些可以从 Python 列表初始化。要使用它，首先需要通过命令提示符输入以下命令来安装库：`conda install numpy`。然后你可以进入 IDE 并输入 `import numpy` 以使用它。

**示例**：创建一个 NumPy 一维数组

首先，你需要导入 NumPy 库。为此，写：

```py

import numpy as np

```

创建一个数组：

```py

a = np.array([1, 2, 3])
a

```

**输出**

`array([1, 2, 3])`

类似地，**Pandas** 是一个强大的库，因其创建数据框的能力而闻名，并且可以用于数据处理和数据分析。Pandas 适用于各种数据，如矩阵、统计数据、观察数据等。要安装 Pandas，你需要按照与 NumPy 相同的步骤，在命令提示符中输入：`conda install pandas`。然后你可以进入 IDE 并输入 `import pandas` 以使用它。

**示例**：创建一个 Pandas 操作

首先，你需要导入 Pandas 库。为此，写：

```py

import pandas as pd

```

创建 2 个列表：

```py

lst1 = [‘a’, ’b’, ’c’]
lst2 = [1, 2, 3]
pd.Series(lst1)

```

**输出：**

```py

  0   a
  1   b
  2   c
dtype: object

```

在输出中，0、1、2 是索引。如果你想根据你的参考显示索引值，可以执行以下操作：

```py

lst1 = [‘a’, ’b’, ’c’]
lst2 = [1, 2, 3]
pd.Series(lst1, index=lst2)

```

**输出：**

```py

  1   a
  2   b
  3   c
dtype: object

```

### 如何选择最佳的 Python 数据科学框架

Python 拥有许多用于数据分析、数据处理和数据可视化的框架。Python 编程是数据科学的理想选择，适用于评估大型数据集、可视化数据集等。

数据分析和 [Python 编程](https://hackr.io/tutorials/learn-python) 互为补充。Python 是数据科学的一个了不起的语言，非常适合那些希望进入数据科学领域的人。它支持大量的数组库和框架，提供了干净高效的数据科学工作选择。各种框架和库具有特定的用途，必须根据你的需求进行选择。在这里，我们列出了一些用于数据科学的 [最佳 Python 框架](https://hackr.io/blog/19-python-frameworks)。

### 初学者最佳 Python 数据科学框架

![Image](../Images/69e025e93034a4ffc321c7d33a38affe.png)

**NumPy：** 如我们之前总结的，NumPy 是 Numerical Python 的简称。它是 Python 编程中最受欢迎的库，并且是数据科学高级工具的基础。深入理解 NumPy 数组有助于有效使用 Pandas，特别是对数据科学家来说。NumPy 多才多艺，你可以处理多维数组和矩阵。NumPy 具有许多与统计、数值计算、线性代数、傅里叶变换等相关的内置函数。NumPy 是科学计算的标准库，提供强大的工具以与 C 和 C++ 集成。如果你想精通数据科学，那么 NumPy 是必须学习的库。

**SciPy：** 它是一个开源库，用于计算各种模块，如图像处理、积分、插值、特殊函数、优化、线性代数、傅里叶变换、聚类等许多任务。这个库与 NumPy 一起使用，以执行高效的数值计算。

**SciKit：** 这个受欢迎的库用于数据科学中的机器学习，提供各种分类、回归和聚类算法，支持支持向量机、朴素贝叶斯、梯度提升和逻辑回归。SciKit 旨在与 SciPy 和 NumPy 兼容。

**Pandas：** Pandas 以在 Python 中提供数据框而闻名。与其他领域特定语言如 R 相比，这是一个强大的数据分析库。使用 Pandas 更容易处理缺失数据，支持处理来自多个不同资源的不同索引数据，并支持自动数据对齐。它还提供数据分析和数据结构的工具，如合并、重塑或切片数据集，并且在处理时间序列数据时非常有效，提供强大的工具来加载来自 Excel、平面文件、数据库和快速 HDF5 格式的数据。

**Matplotlib：** Matplotlib 代表 Python 中的数学绘图库。这是一个主要用于数据可视化的库，包括 3D 图、直方图、图像图、散点图、条形图和带有缩放和平移功能的功率谱，以适应不同硬拷贝格式的发布。它支持几乎所有平台，如 Windows、Mac 和 Linux。该库还作为 NumPy 库的扩展。Matplotlib 具有一个 `pyplot` 模块，用于可视化，通常与 MATLAB 相比较。

这些库是初学者开始使用[Python 编程语言](http://techgeekbuzz.com/best-way-to-learn-python/)进行数据科学的最佳选择。还有许多其他 Python 库，例如用于自然语言处理的NLTK、用于网络挖掘的Pattern、用于深度学习的Theano、IPython、用于网页抓取的Scrapy、Mlpy、Statsmodels等。但是，对于刚开始学习数据科学的初学者来说，熟悉上述顶级库是必不可少的。

我们希望这篇文章能帮助你选择最佳的数据科学框架或库。如果你还有任何问题或需要指导或支持，可以联系我们。

**简历： [Saurabh Hooda](https://www.linkedin.com/in/hoodasaurabh/)** 曾在全球电信和金融巨头公司担任多个职位。在 Infosys 和 Sapient 工作了十年后，他创办了自己的第一家公司 Leno，解决了一个超本地化的图书共享问题。他对产品营销和分析很感兴趣。他的最新项目 [Hackr.io](https://hackr.io/) 推荐了每种编程语言的最佳[数据科学教程](https://hackr.io/tutorials/learn-data-science)和在线编程课程。所有教程均由编程社区提交和投票。

原创。转载需经许可。

**相关：**

+   [冷启动问题：如何构建你的机器学习组合](/2019/01/cold-start-problem-machine-learning.html)

+   [探索 Python 基础知识](/2019/01/exploring-python-basics.html)

+   [2018 年人工智能和数据科学的进展及 2019 年趋势](/2019/02/ai-data-science-advances-trends.html)

### 更多相关内容

+   [停止学习数据科学以寻找目标，并寻找目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [每位数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [一个90亿美元的人工智能失败，深度剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
