# 数据科学入门 Python

> 原文：[https://www.kdnuggets.com/getting-started-with-python-for-data-science](https://www.kdnuggets.com/getting-started-with-python-for-data-science)

![数据科学入门 Python](../Images/5184a1a748fb8e759327ff24b49805db.png)

图片由作者提供

夏天结束了，该回到学习或自我发展计划上来了。你们中的许多人可能在夏天期间考虑了下一步要做什么，如果这与数据科学有关 - 你需要阅读这个博客。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织中的 IT

* * *

生成式 AI、ChatGPT、Google Bard - 这些可能是你最近几个月听到的许多术语。随着这股风潮，很多人正在考虑进入科技领域，比如数据科学。

不同角色的人希望保住他们的工作，因此他们会致力于发展适应当前市场的技能。这是一个竞争激烈的市场，我们看到越来越多的人对数据科学产生兴趣；在这个领域有成千上万的在线课程、训练营和硕士（MSc）课程可供选择。

如果你想知道可以参加哪些免费的数据科学课程，请阅读 [2023 年最佳免费数据科学在线课程](/2023/03/top-free-data-science-online-courses-2023.html)

话虽如此，如果你想进入数据科学领域，你需要了解 Python。

# Python 在数据科学中的角色

[Python](https://www.python.org/) 由荷兰程序员 Guido van Rossum 于 1991 年 2 月开发。该设计重视代码的易读性。语言的构造和面向对象的方法帮助新手和现有程序员编写清晰易懂的代码，从小项目到大项目，从使用小数据到大数据。

31 年后，Python 被认为是今天最值得学习的编程语言之一。

Python 包含各种库和框架，因此你不必从头开始做所有事情。这些预构建的组件包含有用且易读的代码，你可以将其应用到你的程序中。例如，[NumPy](https://numpy.org/)、[Matplotlib](https://matplotlib.org/)、[SciPy](https://scipy.org/)、[BeautifulSoup](https://pypi.org/project/beautifulsoup4/) 等。

如果你想了解更多关于Python库的内容，请阅读以下文章：[2022年数据科学家应该知道的Python库](/2022/04/python-libraries-data-scientists-know-2022.html)。

Python高效、快速且可靠，这使得开发人员能够以最小的努力创建应用程序、执行分析和生成可视化输出。这就是你成为数据科学家的所需的一切！

# 设置Python

如果你想成为数据科学家，我们将通过一步步的指南帮助你开始使用Python：

## 安装Python

首先，你需要下载最新版本的Python。你可以通过访问官方网站[这里](https://www.python.org/downloads/)来查找最新版本。

根据你的操作系统，按照安装说明进行操作，直到完成。

## 选择你的IDE或代码编辑器

IDE是集成开发环境，它是程序员用来更高效地开发软件代码的软件应用程序。代码编辑器有相同的目的，但它是一个文本编辑程序。

如果你不确定选择哪个，我将提供一些流行的选项：

+   [Visual Studio Code](https://code.visualstudio.com/) (VSCode)

+   [PyCharm](https://www.jetbrains.com/pycharm/)

+   [Jupyter Notebook](https://jupyter.org/)

当我开始我的数据科学职业生涯时，我使用了VSC和Jupyter Notebook，这在我的数据科学学习和互动编码中非常有用。一旦你选择了适合你需求的工具，安装它，并按照说明书进行操作。

# 学习基础知识

在深入全面的项目之前，你需要先学习基础知识。让我们开始吧。

## 变量和数据类型

变量是用于存储数据值的容器的术语。数据值有多种数据类型，如整数、浮点数、字符串、列表、元组、字典等。学习这些非常重要，它们构建了你的基础知识。

在以下示例中，变量是一个名称，它包含值“John”。数据类型是字符串：`name = "John" `。

## 运算符和表达式

运算符是允许计算任务的符号，如加法、减法、乘法、除法、指数等。在Python中，表达式是运算符和操作数的组合。

例如 `x = x + 1 0x = x + 10 x = x+ 10`

## 控制结构

控制结构通过指定代码的执行流程使你的编程生活更轻松。在Python中，你需要学习几种类型的控制结构，如条件语句、循环和异常处理。

例如：

```py
if x > 0: 
    print("Positive") 
else: 
    print("Non-positive")
```

## 函数

函数是一个代码块，这个代码块只有在被调用时才会执行。你可以使用`def`关键字创建一个函数。

例如

```py
def greet(name): 
    return f"Hello, {name}!"
```

## 模块和库

Python中的一个模块是包含Python定义和语句的文件。它可以定义函数、类和变量。一个库是相关模块或包的集合。模块和库可以通过使用`import`语句导入。

例如，上面提到Python包含了各种库和框架，如NumPy。你可以通过运行以下命令导入这些不同的库：

```py
import numpy as np
import pandas as pd
import math
import random 
```

你可以使用Python导入各种库和模块。

# 处理数据

一旦你对基础知识及其工作原理有了更好的理解，你的下一步是利用这些技能处理数据。你需要学习如何：

## 使用Pandas导入和导出数据

[Pandas](https://pandas.pydata.org/) 是数据科学领域广泛使用的Python库，它提供了一种灵活而直观的方式来处理各种规模的数据集。假设你有一个CSV文件数据，你可以使用pandas通过以下方式导入数据集：

```py
import pandas as pd

example_data = pd.read_csv("data/example_dataset1.csv")
```

## 数据清洗和操作

数据清洗和操作是数据科学项目中数据预处理阶段的重要步骤，你将原始数据筛查所有的不一致、错误和缺失值，转化为可以用于分析的结构化格式。

数据清洗的要素包括：

+   处理缺失值

+   数据重复

+   异常值

+   数据转换

+   数据类型清洗

数据操作的要素包括：

+   数据选择和过滤

+   数据排序

+   数据分组

+   数据连接和合并

+   创建新变量

+   数据透视和交叉表分析

你需要学习所有这些要素及其在Python中的使用方法。想现在开始，可以 [学习数据清洗和预处理数据科学的免费电子书。](/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

## 统计分析

作为数据科学家的工作的一部分，你需要找到如何筛查你的数据以识别趋势、模式和洞察。你可以通过统计分析来实现这一点。这是收集和分析数据以识别模式和趋势的过程。

这个阶段用于通过数值分析去除偏差，使你能够进一步研究、开发统计模型等。得出的结论用于决策过程，以便根据过去的趋势做出未来的预测。

统计分析有6种类型：

1.  描述性分析

1.  推断分析

1.  预测分析

1.  处方分析

1.  探索性数据分析

1.  因果分析

在这篇博客中，我将深入探讨探索性数据分析。

### 探索性数据分析（EDA）

一旦你清洗和操作了数据，它就准备好进行下一步：探索性数据分析。这时，数据科学家会分析和调查数据集，并创建主要特征/变量的总结，以帮助他们获得进一步的洞察并创建数据可视化。

EDA工具包括

+   预测建模，如线性回归

+   聚类技术，如 K-means 聚类

+   降维技术，如主成分分析（PCA）

+   单变量、双变量和多变量可视化

数据科学的这个阶段可能是最具挑战性的方面，需要大量的实践。库和模块可以提供帮助，但你需要理解手头的任务以及你想要的结果，以确定你需要什么EDA工具。

# 数据可视化

EDA 用于获得更多见解并创建数据可视化。作为数据科学家，你需要创建你发现的可视化。这可以是基本的可视化，如折线图、柱状图和散点图，但你也可以非常有创意，例如热图、染色地图和气泡图。

有各种数据可视化库可以使用，但这些是最受欢迎的：

+   [Matplotlib](https://matplotlib.org/)

+   [Seaborn](https://seaborn.pydata.org/)

+   [Plotly](https://plotly.com/python/)

数据可视化可以更好地沟通，特别是对于那些技术水平不高的利益相关者。

# 总结

本博客旨在指导初学者学习数据科学职业生涯中需要掌握的 Python 学习步骤。每个阶段都需要时间和注意力来掌握。由于我不能详细讲解每一项内容，因此我创建了一个简短的列表，以进一步指导你：

+   [数据清理在数据科学中的重要性](/2023/08/importance-data-cleaning-data-science.html)

+   [数据科学入门：初学者指南](/2023/07/introduction-data-science-beginner-guide.html)

+   [如何从不同背景过渡到数据科学？](/2023/05/transition-data-science-different-background.html)

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家、自由技术作家以及 KDnuggets 的社区经理。她特别感兴趣于提供数据科学职业建议或教程以及数据科学的理论知识。她还希望探索人工智能如何或可以促进人类生命的延续。她是一个热衷于学习的学生，希望扩大她的技术知识和写作技能，同时帮助指导他人。

### 相关主题

+   [5 个步骤开始使用 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [开始使用 Python 生成器](https://www.kdnuggets.com/2023/02/getting-started-python-generators.html)

+   [开始使用 PyTest：轻松编写和运行 Python 测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [开始使用 Go 编程进行数据科学](https://www.kdnuggets.com/getting-started-with-go-programing-for-data-science)

+   [开始清理数据](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)

+   [自动文本摘要入门](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)
