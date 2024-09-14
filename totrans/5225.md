# 掌握 SQL 数据科学的 7 个步骤 — 2019 年版

> 原文：[https://www.kdnuggets.com/2019/05/7-steps-mastering-sql-data-science-2019-edition.html](https://www.kdnuggets.com/2019/05/7-steps-mastering-sql-data-science-2019-edition.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

什么是结构化查询语言（SQL），为什么它对数据管理如此重要？

一段时间前，我写了 [掌握 SQL 数据科学的 7 个步骤](https://www.kdnuggets.com/2016/06/seven-steps-mastering-sql-data-science.html)，这篇文章试图将一些现有的优质免费材料汇总成一个简短但有用的速成课程。然而，包含的材料现在已经过时，许多链接也不再有效，这也是合情合理的，因为原文已经存在几年了。由于近期有人要求更新版本，我认为现在是重新审视这个概念并制定一个新的学习路径来掌握 SQL 数据科学的好时机。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

这一次，我们的目标是再次制定通往 SQL 精通的路径。然而，我们要相对看待“精通”一词，不要期望在学习材料完成后就能达到数据库专家的水平。学习路径旨在帮助那些对数据库、编程、计算机科学概念和/或数据管理有一定理解的人，能够使用 SQL 来管理和查询自己的数据，并扩展到更大的系统中。

![SQL](../Images/7b90faf67d8c5a9b93c5838add6004a0.png)

与每个主题步骤（如视图和连接）拥有大量资源不同，我尝试选择一两个优质的阅读资源，以及一个覆盖特定主题的可访问视频。

所以，拿起你喜欢的饮料，放松一下，让我们在这 7 个步骤中开始掌握 SQL 数据科学。

### 步骤 1：关系型数据库基础

我们从文章开始时提出的问题开始：**什么是结构化查询语言（SQL），为什么它对数据管理如此重要？**

很简单，SQL 是用于管理和查询关系型数据库管理系统（RDBMS）中的数据的语言。

SQL 和 RDBMS（关系数据库管理系统）的术语如此交织，以至于它们经常被混淆，有时是初学者的误解，但通常只是出于方便，SQL 这个术语被用来区分关系系统和非关系数据库系统，后者通常被统称为 "NoSQL"。然而，SQL 技能在非 RDBMS 系统上也并非浪费；所有顶级数据处理框架都在其架构上实现了一些 SQL。

首先，让我们观看和听取 Carnegie Mellon University 的 [Andy Pavlo](http://www.cs.cmu.edu/~pavlo/) 教授讨论数据库、数据库系统、关系模型、关系代数及相关概念。这是一个很好的起点。

### 第二步：SQL 概述

现在我们都熟悉了关系范式，我们将重点转向 SQL。

我们将以以下方式接触 SQL 的初步内容。首先观看下面的视频，标题为“SQL - 简要回顾”，由 Charles Germany 制作，快速了解 SQL 的起源以及通过几个简短的示例了解语法。与视频下方的资源和示例一样，不必担心完全理解语法，只需了解 SQL 在做什么、可以用于什么以及它的样子。

接下来，看看这两个简短的教程：**[SQL 基础查询](http://lgatto.github.io/sql-ecology/01-sql-basic-queries.html)**（来自 Data Carpentry）如其标题所示进行讲解，而 **[SQL 命令列表](https://www.codecademy.com/articles/sql-commands)**（来自 Codecademy）则提供了 SQL 命令及用法的更广泛概述。特别是第二个资源，未来可能会成为一个便捷的参考。其中一些命令在后续步骤中会更详细地讲解，因此，不必担心一开始就理解所有内容。同时，这些初步的示例非常直观，有助于你的理解。

最后，为了准备下一步，启动一个 SQL 环境。你可能不想输入每一个遇到的 SQL 语句，但运行一个 SQL 解释器是很有意义的。我建议在本地安装 SQLite；它是一个简单但功能强大的 SQL 安装程序。

+   **[获取 SQLite](https://www.sqlite.org/index.html)**

+   **[SQLite 命令行外壳](https://sqlite.org/cli.html)**

### 第三步：选择、插入、更新

虽然 SQL 可以用于执行各种数据管理任务，但使用 SELECT 查询数据库、使用 INSERT 插入记录以及使用 UPDATE 更新现有记录是一些最常用的命令，是实际使用 SQL 的好起点。

阅读并完成以下初步练习，所有这些都由 [SQL Course](http://www.sqlcourse.com) 提供。

+   **[选择数据](http://www.sqlcourse.com/select.html)**

+   **[插入数据到表中](http://www.sqlcourse.com/insert.html)**

+   **[更新记录](http://www.sqlcourse.com/update.html)**

本教程，来自[Arachnoid](https://arachnoid.com)的**[MySQL教程1：概述、表、查询](https://arachnoid.com/MySQL/)**，涵盖了SQL基础知识以及初学者到中级查询，并对我们迄今所见的内容进行了全面复习，甚至更多。它从MySQL的角度呈现，但其中的内容并不局限于该平台的SQL实现。

### 第4步：创建、删除、删除

我们的第二组命令包括用于创建和删除表以及删除记录的命令。通过理解这一不断增长的命令集合，许多可以称之为常规数据管理和查询的操作突然变得可实现（当然需要实践）。

通过[SQL课程](http://www.sqlcourse.com)再次尝试以下教程。

+   **[创建表](http://www.sqlcourse.com/create.html)**

+   **[删除表](http://www.sqlcourse.com/drop.html)**

+   **[删除记录](http://www.sqlcourse.com/delete.html)**

### 第5步：视图和连接

进入一些稍微高级的SQL主题。

首先，我们来看视图，它可以被看作是由查询结果填充的虚拟表，在包括应用开发、数据安全和简化数据共享等多个不同场景中都很有用。观看这段来自Socratica的视频以获得见解。

连接有不同的类型，而在学习SQL时，理清它们可能是更复杂的主题之一。这实际上更能证明SQL的易用性，而不是学习连接的实际难度。通过这段Socratica视频获得对连接的更好理解。

查看来自[Code Project](http://www.codeproject.com)的**[SQL连接的可视化表示](http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins)**。

![连接](../Images/0d5efa70866c8ba79b596ec40adda7dc.png)

本教程，来自[Arachnoid](https://arachnoid.com)的**[MySQL教程2：视图和连接](https://arachnoid.com/MySQL/views_joins.html)**，涵盖了视图和连接。它也从MySQL的角度呈现，但其中的内容并不局限于该平台的SQL实现。

### 第6步：高级SQL

高级SQL可能意味着很多不同的东西，因此我们将缩小范围。

首先观看来自卡内基梅隆大学的Andy Pavlo教授的讲座，他讨论了聚合、独特聚合、分组、筛选、字符串操作、日期和时间操作、嵌套查询、表表达式等内容。

在了解了SQL的这些方面后，将注意力转向存储过程这一重要概念，并查看来自[MySQL Tutorial](http://www.mysqltutorial.org)的**[MySQL存储过程](http://www.mysqltutorial.org/mysql-stored-procedure-tutorial.aspx)**教程。

### 第7步：查询优化

一旦你知道如何编写查询，你将希望学习如何优化它们，以便在结果和运行时间上都达到最佳。这在大数据库上的复杂查询中尤其重要。

观看卡内基梅隆大学的[安迪·帕夫洛](http://www.cs.cmu.edu/~pavlo/)教授讲解这个主题的视频讲座。

然后查看来自[初学者 SQL 教程](https://beginner-sql-tutorial.com/sql-query-tuning.htm)的**[SQL 调优或 SQL 优化](https://beginner-sql-tutorial.com/sql-query-tuning.htm)** 教程。

希望到目前为止，你已经学到了足够的 SQL，能够将自己视为数据科学领域的“专家”。但不要止步于此；在线上还有很多免费的优质资源，可以用来在这基础上进一步提升。你永远无法学够 SQL，正如许多其他技能一样，实践是强化的关键。

**相关**：

+   [用 Python 掌握基础机器学习的 7 个步骤 — 2019 版](/2019/01/7-steps-mastering-basic-machine-learning-python.html)

+   [SQL 案例研究：帮助初创公司 CEO 管理数据](/2018/09/sql-case-study-helping-startup-ceo-manage-data.html)

+   [SQL 还是非 SQL：这是一个问题！](/2018/05/sql-not-sql-question.html)

### 更多相关内容

+   [停止学习数据科学以寻找目标，并通过寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成为优秀数据科学家的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
