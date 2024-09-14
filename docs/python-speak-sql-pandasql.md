# 让 Python 使用 SQL 通过 pandasql

> 原文：[`www.kdnuggets.com/2017/02/python-speak-sql-pandasql.html`](https://www.kdnuggets.com/2017/02/python-speak-sql-pandasql.html)

本文最初出现在 [Yhat 博客](http://blog.yhat.com/)。**Yhat**（[官网](https://www.yhat.com/)）是一家位于布鲁克林的公司，旨在使数据科学对开发人员、数据科学家和企业都适用。Yhat 提供一个软件平台，用于部署和管理预测算法作为 REST API，同时消除了与生产环境相关的痛苦工程障碍，如测试、版本控制、扩展和安全性。

* * *

### 介绍

我最喜欢 Python 的一件事是，用户可以观察 R 社区，然后模仿其中的最佳部分。我坚信，语言的有用性在于它的库和工具。

本文介绍了 `pandasql`，这是我们（Yhat）编写的一个 Python 包，用于模拟 R 包 [`sqldf`](https://github.com/ggrothendieck/sqldf)。它是一个小巧但强大的库，由仅仅 [358 行代码](https://github.com/yhat/pandasql) 组成。`pandasql` 的理念是让 Python 能够使用 SQL。对于那些来自 SQL 为主背景或仍然“以 SQL 为思维方式”的用户，`pandasql` 是一个不错的方式，可以充分利用这两种语言的优势。

在本介绍中，我们将展示如何在我们为数据探索和分析构建的集成开发环境（IDE）[Rodeo](https://www.yhat.com/products/rodeo) 内部快速上手 `pandasql`。Rodeo 是一个开源且完全免费的工具。如果你是 R 用户，它类似于 [RStudio](https://www.rstudio.com/) 的工具。到目前为止，Rodeo 只能运行 Python 代码，但上周 [我们添加了](http://blog.yhat.com/posts/rodeo-2.4.0.html) 对其他多种语言（markdown、JSON、julia、SQL、markdown）的语法高亮支持。正如你可能读到或猜到的那样，我们对 Rodeo 有大计划，包括添加 SQL 支持，以便你可以在 Rodeo 内部直接运行 SQL 查询，即使没有我们便捷的 `pandasql`。更多内容将在接下来的一两周内发布！

### 下载 Rodeo

从 [Yhat 网站上的 Rodeo 页面](https://www.yhat.com/products/rodeo) 下载适用于 Mac、Windows 或 Linux 的 Rodeo 开始。

ps 如果你下载 Rodeo 遇到问题或有疑问，我们全天候（好吧，差不多）监控我们的 [讨论论坛](http://discuss.yhat.com/)。

### 如果你感兴趣的话，提供一点背景信息

在幕后，`pandasql` 使用 `pandas.io.sql` 模块在 `DataFrame` 和 SQLite 数据库之间转移数据。操作在 SQL 中执行，结果返回，然后数据库被销毁。该库大量使用 `pandas` 的 `write_frame` 和 `frame_query`，这两个函数让你可以读写 `pandas` 和（大多数）任何 SQL 数据库。

### 安装 `pandasql`

使用 Rodeo 的包管理器窗格安装 `pandasql`。只需搜索 `pandasql` 并点击安装包。

![Rodeo](img/df01e2be992655e3d8df842a35b7b555.png)

如果你喜欢这种方式，你也可以在文本编辑器中运行 `! pip install pandasql` 来安装。

### 查看数据集

`pandasql` 有两个内置的数据集，我们将在下面的示例中使用它们。

+   `meat`：来自美国农业部的数据集，包含有关牲畜、乳制品和家禽展望及生产的指标

+   `births`：来自联合国统计司的数据集，包含按月的活产统计数据

运行以下代码来查看数据集。

在 Rodeo 内部，你实际上不需要使用 print.variable.head() 语句，因为你可以直接检查数据框。

![Rodeo](img/e16146ab2559389c09b51db6b64af166.png)

### 奇怪的图表

注意，图表会同时出现在控制台和图表标签（右下角标签）中。

提示：你可以通过点击窗格顶部的箭头来“弹出”你的图表。如果你在使用多个显示器，并希望将其中一个专门用于数据可视化，这非常方便。

![Rodeo](img/033f5bc0a2de9b060afac0a299c60ecc.png)

### 使用方法

为了使这篇文章简洁易读，我们只给出了大部分查询的代码片段和几行结果。

如果你在使用 Rodeo，这里有一些开始时的提示：

+   `Run Script` 确实会运行你在文本编辑器中编写的所有内容。

+   你可以突出显示一段代码并通过点击 `Run Line` 或按 Command + Enter 来运行它。

+   你可以调整窗格的大小（当我不做绘图时，我会缩小右下角的窗格）。

**基础知识（Basics）**

编写一些 SQL 语句并通过将 DataFrames 替换为表格来在你的 `pandas` `DataFrame` 上执行。

`pandasql` 创建一个数据库、架构及所有内容，加载你的数据，并运行你的 SQL。

**聚合（Aggregation）**

`pandasql` 支持聚合。你可以在 `group by` 子句中使用[别名列名](http://www.w3schools.com/sql/sql_alias.asp)或列号。

**`locals()` 与 `globals()`**

`pandasql` 需要访问你会话/环境中的其他变量。你可以在执行 SQL 语句时将 `locals()` 传递给 `pandasql`，但如果你运行很多查询，这可能会很麻烦。为了避免每次都传递 locals，你可以在脚本中添加这个辅助函数来设置 `globals()`，如下所示：

**连接（joins）**

你可以使用普通 SQL 语法来连接数据框。

**`WHERE` 条件**

这是一个 `WHERE` 子句。

### 这只是 SQL

由于 `pandasql` 是由 [SQLite3](https://docs.python.org/2/library/sqlite3.html) 驱动的，你可以做几乎所有 SQL 能做的事情。这里有一些使用常见 SQL 特性（如子查询、排序、函数和联合）的示例。

### 最后的思考

![Rodeo](img/9d2174120c35e418253586777099d061.png)

`pandas` 是一个非常出色的数据分析工具，我们认为这主要归功于它极易理解、简洁且表达力强。最终，有*大量*理由去学习`merge`、`join`、`concatenate`、`melt`和其他原生`pandas`特性，以便更好地切割和处理数据。查看一下[文档](http://pandas.pydata.org/pandas-docs/stable/merging.html)获取一些示例。

我们希望`pandasql`能成为 Python 和`pandas`新手的有用学习工具。在我个人学习 R 的经历中，`sqldf`是一个熟悉的接口，帮助我尽快高效地使用新工具。

我们希望你能查看`pandasql`和 Rodeo；如果你这样做了，请[告诉我们你的想法](http://discuss.yhat.com/)！

[原文](http://blog.yhat.com/posts/pandasql-intro.html)。经许可转载。

**相关：**

+   优秀的机器学习算法极简和干净实现合集

+   Pandas 备忘单：Python 中的数据科学和数据整理

+   Python 中的统计数据分析

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

### 更多相关内容

+   [在 Pandas 中使用 Pandasql 进行 SQL 操作](https://www.kdnuggets.com/sql-in-pandas-with-pandasql)

+   [预测制作：Python 中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [Ploomber 与 Kubeflow：简化 MLOps](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)

+   [数据质量在成功机器学习中的重要性](https://www.kdnuggets.com/2022/03/significance-data-quality-making-successful-machine-learning-model.html)

+   [让智能文档处理更智能：第一部分](https://www.kdnuggets.com/2023/02/making-intelligent-document-processing-smarter-part-1.html)

+   [从数据分析师到数据策略师：产生影响的职业路径](https://www.kdnuggets.com/2023/05/data-analyst-data-strategist-career-path-making-impact.html)
