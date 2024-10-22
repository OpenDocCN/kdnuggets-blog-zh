# 掌握数据科学 SQL 的 7 个步骤

> 原文：[`www.kdnuggets.com/2016/06/seven-steps-mastering-sql-data-science.html/2`](https://www.kdnuggets.com/2016/06/seven-steps-mastering-sql-data-science.html/2)

### 第 4 步：创建、删除、删除

我们的第二组命令包括那些用于创建和删除表以及删除记录的命令。理解这不断增长的命令集合，突然之间，许多可以称之为*常规数据管理和查询*的操作变得可实现（当然，需要实践）。

**创建**

+   [创建表（来自 SQL 课程）](http://www.sqlcourse.com/create.html)

+   [创建表（来自 SQL Zoo）](http://sqlzoo.net/wiki/CREATE_TABLE)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的 IT 组织

* * *

**删除**

+   [删除表（来自 SQL 课程）](http://www.sqlcourse.com/drop.html)

+   [删除表（来自 SQL Zoo）](http://sqlzoo.net/wiki/DROP)

**删除**

+   [删除记录（来自 SQL 课程）](http://www.sqlcourse.com/delete.html)

+   [删除（来自 SQL Zoo）](http://sqlzoo.net/wiki/DELETE)

### 第 5 步：视图与连接

进入一些稍微复杂的 SQL 主题。首先，我们来看一下视图，可以将其视为由查询结果填充的虚拟表，适用于包括应用开发、数据安全和简化数据共享在内的多种场景。

首先，了解一下视图是什么：

+   [使用视图（来自 Tutorials Point）](http://www.tutorialspoint.com/sql/sql-using-views.htm)

对于数据科学领域的初学者，我会把视图称为“可有可无”。由于重点可能更多放在数据探索上，我会说下一个主题——连接，是“必须掌握”的。

阅读有关连接是什么以及它们的重要性（并获取一些示例）：

+   [什么是连接？（来自 sql-joins.com）](http://www.sql-join.com/)

+   [SQL 连接（来自 Mode Analytics）](https://community.modeanalytics.com/sql/tutorial/sql-joins/)

连接有不同的类型，学习 SQL 时可能会涉及其中一个较复杂的主题是将它们搞清楚。这实际上更多是 SQL 易用性的证明，而不是学习连接的实际难度。

观看解释内连接的视频，然后查看外连接和交叉连接的视频：

+   [内连接](https://www.youtube.com/watch?v=0FEjw2HnfDs)

+   [外连接与交叉连接](https://www.youtube.com/watch?v=3t2X1jczt4)

![SQL 连接的可视化表示](http://i.stack.imgur.com/VQ5XP.png)

SQL 连接的可视化表示。

查看这个 SQL 连接的可视化表示：

+   [SQL 连接的可视化表示（来自 Code Project）](http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins)

最后，这个教程回顾了连接和视图：

+   [MySQL 教程 2：视图和连接（来自 arachnoid.com）](http://arachnoid.com/MySQL/views_joins.html)

### 第 6 步：数据科学中的 SQL

好的，你在学习 SQL 上已经取得了一些进展。你可以查询一些数据，创建和管理一些表格，如果需要还可以创建视图，甚至在一些更复杂的查询中使用连接。但你为什么要学习这些呢？是为了数据科学，对吧？让我们暂时离开技术，来了解一下这个话题。

这里有几个讨论 SQL 在数据科学中可以用于什么的讨论：

+   [数据科学中的 SQL（来自 yhat）](http://blog.yhat.com/posts/sql-for-data-scientists.html)

+   [数据科学家如何使用 SQL？（来自 Quora）](https://www.quora.com/How-do-data-scientists-use-SQL)

### 第 7 步：SQL 与 Python、R 的集成

我们经常会发现 SQL 被嵌入在用其他编程语言编写的软件中，作为更大系统的一部分。例如，在 web 开发中，你可能会发现 PHP 或 Ruby 或其他语言通过 SQL 调用数据库，以输入、修改或检索应用程序相关的数据。在数据科学中，你可能会看到 SQL 被调用作为某些用 Python 或 R 编写的应用程序的一部分。因此，了解这些语言如何与 SQL 配合并不是坏主意。

![SQLite Python](img/b89a683dd98a7eac3d19d4da80aaa787.png)

使用 Python 和 SQLite 执行 SQL 查询。

**Python**

要了解 Python 和 SQL 如何协同工作，请阅读 [Sebastian Raschka](https://twitter.com/rasbt?lang=en) 关于在 Python 中使用 SQLite 的精彩详细文章：

+   [Python 中的 SQLite 教程](http://sebastianraschka.com/Articles/2014_sqlite_in_python_tutorial.html)

**R**

这里有一对资源用于实现 R 和 SQL 的集成，它们从不同的角度探讨了这一主题：

+   [SQL 和 R（来自 Simple Talk）](https://www.simple-talk.com/dotnet/software-tools/sql-and-r-/)

+   [RSQLite](https://github.com/rstats-db/RSQLite)

**进一步**

如果你觉得不断阅读 SQL 相关的内容并进行练习是你的节奏，我推荐你阅读以下（免费提供的）书籍：

+   [通过艰难的方式学习 SQL](http://sql.learncodethehardway.org/)，作者 Zed A. Shaw

**相关**：

+   掌握 Python 中机器学习的 7 个步骤

+   理解深度学习的 7 个步骤

+   R 学习路径：从初学者到专家的 7 个步骤

### 更多关于此主题

+   [掌握数据科学的 SQL 7 步骤](https://www.kdnuggets.com/2022/04/7-steps-mastering-sql-data-science.html)

+   [掌握数据宇宙：成功数据科学职业的关键步骤](https://www.kdnuggets.com/mastering-the-data-universe-key-steps-to-a-thriving-data-science-career)

+   [KDnuggets™ 新闻 22:n05，2 月 2 日：掌握机器…的 7 个步骤](https://www.kdnuggets.com/2022/n05.html)

+   [掌握数据科学的 Python 7 步骤](https://www.kdnuggets.com/2022/06/7-steps-mastering-python-data-science.html)

+   [掌握数据科学项目管理的 7 个敏捷步骤](https://www.kdnuggets.com/2023/07/7-steps-mastering-data-science-project-management-agile.html)

+   [掌握 SQL、Python、数据清理、数据…的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
tps://www.kdnuggets.com/2022/06/7-steps-mastering-python-data-science.html)

+   [掌握数据科学项目管理与敏捷的 7 个步骤](https://www.kdnuggets.com/2023/07/7-steps-mastering-data-science-project-management-agile.html)

+   [关于掌握 SQL、Python、数据清洗、数据预处理和探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
