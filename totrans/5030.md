# Python 中的统计数据分析

> 原文：[https://www.kdnuggets.com/2016/07/statistical-data-analysis-python.html](https://www.kdnuggets.com/2016/07/statistical-data-analysis-python.html)

**由克里斯托弗·丰内斯贝克，范德比尔特大学医学院**。

> **编辑说明**：本教程最初作为课程教学材料发布，可能包含对其他课程的脱离上下文的引用；这并不影响材料的有效性或有用性。

### 描述

本教程将介绍使用 Python 进行统计数据分析，使用存储为 Pandas DataFrame 对象的数据。数据分析中的大部分工作涉及到数据的导入、清理和转换，以便进行分析。因此，本课程的前半部分包括对基本和中级 Pandas 使用的两部分概述，展示如何有效地操作内存中的数据集。这包括任务如索引、对齐、连接/合并方法、日期/时间类型以及处理缺失数据。接下来，我们将介绍使用 Pandas 和 Matplotlib 进行绘图和可视化，重点是创建有效的数据可视化，同时避免常见的陷阱。最后，参与者将学习使用 Numpy、Scipy 和 Pandas 中的一些高级函数进行统计数据建模的方法。这将包括将数据拟合到概率分布中，使用线性和非线性模型估计变量之间的关系，以及对自助法的简要介绍。每个教程部分将涉及样本数据集的实际操作和分析，数据集将在课程前提供给参与者。

![Pandas IPython](../Images/dca790229b1f2b9ae167294328581727.png)

本教程的目标受众包括所有新的 Python 用户，尽管我们建议用户还参加入门阶段的 NumPy 和 IPython 课程。

### 学生指南

对于熟悉 Git 的学生，你可以简单地克隆此仓库以获取所有教程材料（iPython 笔记本和数据）。另外，你也可以 [下载包含材料的压缩文件](https://github.com/fonnesbeck/statistical-analysis-python-tutorial/archive/master.zip)。第三种选择是通过点击下列各节标题来查看静态笔记本。

### 大纲

**[Pandas 入门](http://nbviewer.ipython.org/urls/gist.github.com/fonnesbeck/5850375/raw/c18cfcd9580d382cb6d14e4708aab33a0916ff3e/1.+Introduction+to+Pandas.ipynb)**

+   导入数据

+   Series 和 DataFrame 对象

+   索引、数据选择和子集

+   层次索引

+   读取和写入文件

+   排序和排名

+   缺失数据

+   数据总结

**[使用 Pandas 进行数据处理](http://nbviewer.ipython.org/urls/gist.github.com/fonnesbeck/5850413/raw/3a9406c73365480bc58d5e75bc80f7962243ba17/2.+Data+Wrangling+with+Pandas.ipynb)**

+   日期/时间类型

+   合并和连接 DataFrame 对象

+   拼接

+   重塑 DataFrame 对象

+   数据透视

+   数据转换

+   排列和抽样

+   数据聚合和 GroupBy 操作

**[绘图与可视化](http://nbviewer.ipython.org/urls/gist.github.com/fonnesbeck/5850463/raw/a29d9ffb863bfab09ff6c1fc853e1d5bf69fe3e4/3.+Plotting+and+Visualization.ipynb)**

+   Pandas 与 Matplotlib 的绘图比较

+   条形图

+   直方图

+   箱形图

+   分组图

+   散点图

+   格子图

**[统计数据建模](http://nbviewer.ipython.org/urls/gist.github.com/fonnesbeck/5850483/raw/5e049b2fdd1c83ae386aa3205d3fc50a1a05e5b0/4.+Statistical+Data+Modeling.ipynb)**

+   统计建模

+   将数据拟合到概率分布

+   拟合回归模型

+   模型选择

+   自助法

### 所需软件包

+   Python 2.7 或更高版本（包括 Python 3）

+   pandas >= 0.11.1 及其依赖项

+   NumPy >= 1.6.1

+   matplotlib >= 1.0.0

+   pytz

+   IPython >= 0.12

+   pyzmq

+   tornado

可选：statsmodels，xlrd 和 openpyxl

对于运行最新版本 Mac OS X（10.8）的学生，获取所有软件包的最简单方法是安装 [Scipy Superpack](http://bit.ly/scipy_superpack)，该软件包与 OS X 随附的 Python 2.7.2 兼容。

否则，另一种简单的安装所有必要软件包的方法是使用 Continuum Analytics 的 [Anaconda](http://docs.continuum.io/anaconda/install.html)。

### 统计阅读清单

[生态侦探：用数据对抗模型](https://www.amazon.com/Ecological-Detective-Confronting-Models-Data/dp/0691034974/ref=sr_1_1?s=books&ie=UTF8&qid=1372250186&sr=1-1&keywords=ecological+detective)，Ray Hilborn 和 Marc Mangel

虽然针对生态学家，但 Mangel 和 Hilborn 确定了科学家可以用来为他们的数据构建有用且可信模型的关键方法。他们不回避数学，但这本书非常易读且充满示例。

[使用回归和多层次/层级模型的数据分析](https://www.amazon.com/Analysis-Regression-Multilevel-Hierarchical-Models/dp/052168689X/ref=sr_1_1?s=books&ie=UTF8&qid=1372250274&sr=1-1&keywords=gelman)，Andrew Gelman 和 Jennifer Hill

应用层次建模的首选参考。

[统计学习的元素](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)，Hastie, Tibshirani 和 Friedman

为统计学家提供的全面机器学习指南。

[贝叶斯统计方法第一课程](https://www.amazon.com/Bayesian-Statistical-Methods-Springer-Statistics/dp/1441928286/ref=sr_1_24?ie=UTF8&qid=1372250835&sr=8-24&keywords=bayesian)，Peter Hoff

一本很好的入门书籍，适合开始学习贝叶斯方法。

[回归建模策略](https://www.amazon.com/Regression-Modeling-Strategies-Applications-Statistics/dp/1441929185/ref=sr_1_1?s=books&ie=UTF8&qid=1372250898&sr=1-1&keywords=harrell+regression)，Frank Harrell

Frank Harrell 的回归建模技巧。我每周都会拿出来用。

**简介： [克里斯托弗·丰内斯贝克](https://twitter.com/fonnesbeck)** 是范德比尔特大学医学院生物统计学系的助理教授。他专注于计算统计学、贝叶斯方法、元分析和应用决策分析。他来自温哥华，BC，并获得乔治亚大学的博士学位。

[原文](https://github.com/fonnesbeck/statistical-analysis-python-tutorial)。经许可转载。

**相关：**

+   [掌握Python机器学习的7个步骤](/2015/11/seven-steps-machine-learning-python.html)

+   [数据科学实战：Kaggle第3部分 – 数据清洗](/2016/06/doing-data-science-kaggle-walkthrough-data-cleaning.html)

+   [Python数据科学：Pandas与Spark DataFrame的关键区别](/2016/01/python-data-science-pandas-spark-dataframe-differences.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关内容

+   [Python中的统计函数](https://www.kdnuggets.com/2022/10/statistical-functions-python.html)

+   [统计学习简介，Python版：免费书籍](https://www.kdnuggets.com/2023/07/introduction-statistical-learning-python-edition-free-book.html)

+   [10个Python统计函数](https://www.kdnuggets.com/10-python-statistical-functions)

+   [数据科学家应了解的5个统计悖论](https://www.kdnuggets.com/2023/02/5-statistical-paradoxes-data-scientists-know.html)

+   [基础回顾第2周：数据库、SQL、数据管理和…](https://www.kdnuggets.com/back-to-basics-week-2-database-sql-data-management-and-statistical-concepts)

+   [什么是统计偏斜？](https://www.kdnuggets.com/2022/11/statistical-skew.html)
