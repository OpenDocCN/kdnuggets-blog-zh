# SQL中的OLAP查询：回顾

> 原文：[https://www.kdnuggets.com/2018/09/olap-queries-sql-refresher.html](https://www.kdnuggets.com/2018/09/olap-queries-sql-refresher.html)

[评论](#comments)

**作者：[Wilfried Lemahieu](https://www.linkedin.com/in/wilfried-lemahieu-458397/)，[Seppe vanden Broucke](https://www.linkedin.com/in/seppevandenbroucke/)，[Bart Baesens](https://www.linkedin.com/in/bart-baesens-403bb83/)**

![数据库管理原则](../Images/947e7a2cdbcea1f2005cffb1cb010457.png)

* * *

## 我们的前三推荐课程

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

以下文章基于我们最近的书籍：《数据库管理原则 - 存储、管理和分析大数据与小数据的实用指南》（详见 [www.pdbmbook.com](http://www.pdbmbook.com)）。

在这篇文章中，我们详细讲解了如何在SQL中实现OLAP查询。为了方便执行OLAP查询和数据聚合，SQL-99引入了对GROUP BY语句的三种扩展：CUBE、ROLLUP和GROUPING SETS操作符。

**CUBE**操作符计算指定属性类型每个子集上的GROUP BY的并集。其结果集表示基于源表的多维立方体。考虑以下SALES TABLE。

| **产品** | **季度** | **地区** | **销售额** |
| --- | --- | --- | --- |
| A | Q1 | Europe | 10 |
| A | Q1 | America | 20 |
| A | Q2 | Europe | 20 |
| A | Q2 | America | 50 |
| A | Q3 | America | 20 |
| A | Q4 | Europe | 10 |
| A | Q4 | America | 30 |
| B | Q1 | Europe | 40 |
| B | Q1 | America | 60 |
| B | Q2 | Europe | 20 |
| B | Q2 | America | 10 |
| B | Q3 | America | 20 |
| B | Q4 | Europe | 10 |
| B | Q4 | America | 40 |

*示例 SALESTABLE。*

我们现在可以制定以下SQL查询：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY CUBE (QUARTER, REGION)

```

基本上，这个查询计算了SALESTABLE的2² = 4种分组的并集，分组为：{(quarter,region), (quarter), (region), ()}，其中()表示一个空的分组列表，代表整个SALESTABLE的总聚合。换句话说，由于quarter有4个值，region有2个值，结果多重集将包含4*2+4*1+1*2+1或15个元组，如表1所示。维度列Quarter和Region中已添加了NULL值以指示发生的聚合。如果需要，可以用更有意义的‘ALL’替换这些NULL值。更具体地说，我们可以添加2个CASE子句，如下所示：

```py

SELECT CASE WHEN grouping(QUARTER) = 1 THEN 'All' ELSE QUARTER END AS QUARTER, CASE WHEN grouping(REGION) = 1 THEN 'All' ELSE REGION END AS REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY CUBE (QUARTER, REGION)

```

**grouping()** 函数在聚合过程中生成 NULL 值时返回 1，否则返回 0。这有助于区分生成的 NULL 和可能的真实 NULL。我们不会将其添加到后续的 OLAP 查询中，以避免不必要的复杂化。

此外，注意第五行销售额的 NULL 值。这代表了在原始 SALESTABLE 中不存在的属性组合，因为显然在欧洲的 Q3 没有销售产品。注意，除了 SUM()，SELECT 语句中还可以使用其他 SQL 聚合函数，如 MIN()、MAX()、COUNT() 和 AVG()。

| **季度** | **区域** | **销售额** |
| --- | --- | --- |
| Q1 | Europe | 50 |
| Q1 | America | 80 |
| Q2 | Europe | 40 |
| Q2 | America | 60 |
| Q3 | Europe | NULL |
| Q3 | America | 40 |
| Q4 | Europe | 20 |
| Q4 | America | 80 |
| Q1 | NULL | 130 |
| Q2 | NULL | 100 |
| Q3 | NULL | 40 |
| Q4 | NULL | 90 |
| NULL | Europe | 110 |
| NULL | America | 250 |
| NULL | NULL | 360 |

**表 1：使用 Cube 运算符的 SQL 查询结果。**

**ROLLUP** 运算符计算指定属性类型列表的每个前缀的并集，从最详细的到总计。它特别适用于生成包含小计和总计的报告。ROLLUP 与 CUBE 运算符的主要区别在于前者生成一个显示指定属性类型值层级的聚合结果集，而后者生成一个显示所有选定属性类型值组合的聚合结果集。因此，属性类型的提及顺序对于 ROLLUP 很重要，但对于 CUBE 运算符则不然。考虑以下查询：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY ROLLUP (QUARTER, REGION)

```

此查询生成了三个分组的并集 {(季度, 区域), (季度}, ()}，其中 () 代表完全聚合。结果多重集将有 4*2+4+1 或 13 行，显示在表 2 中。可以看到，区域维度首先被汇总，然后是季度维度。注意与表 1 中 CUBE 运算符的结果相比，被省略的两行。

| **季度** | **区域** | **销售额** |
| --- | --- | --- |
| Q1 | Europe | 50 |
| Q1 | America | 80 |
| Q2 | Europe | 40 |
| Q2 | America | 60 |
| Q3 | Europe | NULL |
| Q3 | America | 40 |
| Q4 | Europe | 20 |
| Q4 | America | 80 |
| Q1 | NULL | 130 |
| Q2 | NULL | 100 |
| Q3 | NULL | 40 |
| Q4 | NULL | 90 |
| NULL | NULL | 360 |

**表 2：使用 ROLLUP 运算符的 SQL 查询结果。**

前面的示例将 GROUP BY ROLLUP 构造应用于两个完全独立的维度，它也可以应用于表示同一维度上不同聚合级别（因此不同详细级别）的属性类型。例如，假设 SALESTABLE 元组表示了更详细的城市级别销售数据，并且表中包含三个与位置相关的列：City、Country 和 Region。我们可以制定以下 ROLLUP 查询，分别按城市、国家、地区和总计计算销售总额：

```py

SELECT REGION, COUNTRY, CITY, SUM(SALES)
FROM SALESTABLE
GROUP BY ROLLUP (REGION, COUNTRY, CITY)

```

请注意，在这种情况下，SALESTABLE 将包括 City、Country 和 Region 属性类型在一个表中。由于这三种属性类型表示同一维度中的不同详细级别，它们在传递上相互依赖，说明这些数据仓库数据确实是非规范化的。

**GROUPING SETS** 操作符生成的结果集等同于多个简单 GROUP BY 子句的 UNION ALL 结果。考虑以下示例：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY GROUPING SETS ((QUARTER), (REGION))

```

该查询等同于：

```py

SELECT QUARTER, NULL, SUM(SALES)
FROM SALESTABLE
GROUP BY QUARTER
UNION ALL
SELECT NULL, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY REGION

```

结果见表 3。

| **季度** | **地区** | **销售额** |
| --- | --- | --- |
| Q1 | NULL | 130 |
| Q2 | NULL | 100 |
| Q3 | NULL | 40 |
| Q4 | NULL | 90 |
| NULL | 欧洲 | 110 |
| NULL | 美国 | 250 |

**表 3：使用 GROUPING SETS 操作符的 SQL 查询结果**

可以在单个 SQL 查询中使用多个 CUBE、ROLLUP 和 GROUPING SETS 语句。不同组合的 CUBE、ROLLUP 和 GROUPING SETS 可以生成等效的结果集。考虑以下查询：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY CUBE (QUARTER, REGION)

```

该查询等同于：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY GROUPING SETS ((QUARTER, REGION), (QUARTER), (REGION), ())

```

同样，以下查询：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY ROLLUP (QUARTER, REGION)

```

与以下内容相同：

```py

SELECT QUARTER, REGION, SUM(SALES)
FROM SALESTABLE
GROUP BY GROUPING SETS ((QUARTER, REGION), (QUARTER),())

```

给定需要聚合和检索的数据量，OLAP SQL 查询可能会非常耗时。加快性能的一种方法是将这些 OLAP 查询转换为物化视图。例如，使用 CUBE 操作符的 SQL 查询可以用来预计算选定维度上的聚合，结果可以存储为物化视图。物化视图的一个缺点是需要额外的努力定期刷新这些物化视图，但通常公司对接近当前版本的数据是满意的，因此同步可以在夜间或固定时间间隔进行。

欲了解更多信息，我们很高兴推荐我们的最新书籍：[数据库管理原理 - 大数据与小数据存储、管理和分析的实用指南](http://www.pdbmbook.com)。

**简介**：**[Wilfried Lemahieu](https://www.linkedin.com/in/wilfried-lemahieu-458397/)** 是 KU Leuven（比利时）的教授，研究领域包括大数据存储、集成与分析、数据质量以及业务流程管理和服务导向。

**[塞佩·范登·布鲁克](https://www.linkedin.com/in/seppevandenbroucke/)** 于 2014 年在比利时 KU Leuven 获得应用经济学博士学位。目前，塞佩在 KU Leuven 的决策科学与信息管理系担任助理教授。塞佩的研究兴趣包括商业数据挖掘和分析、机器学习、过程管理和过程挖掘。他的工作已发表在知名国际期刊上，并在顶级会议上展示。

[**巴特·贝森斯**](https://www.linkedin.com/in/bart-baesens-403bb83/)是 KU Leuven 的副教授，并且是南安普顿大学（英国）的讲师。他在分析、客户关系管理、网络分析、欺诈检测和信用风险管理方面做了广泛的研究。他的研究成果已发表在知名国际期刊上（例如《机器学习》、《管理科学》、《IEEE 神经网络汇刊》、《IEEE 知识与数据工程汇刊》、《IEEE 演化计算汇刊》、《机器学习研究杂志》等），并在国际顶级会议上进行了展示。

**相关：**

+   [关于数据库管理、SQL、数据仓库、商业智能、OLAP、大数据、NoSQL 数据库、数据质量、数据治理和分析的 YouTube 视频 – 免费](https://www.kdnuggets.com/2018/05/baesens-youtube-videos-database-management-sql-big-data-analytics-free.html)

+   [远程数据科学：如何从 Jupyter 笔记本将 R 和 Python 执行发送到 SQL 服务器](https://www.kdnuggets.com/2018/07/r-python-execution-sql-server-jupyter.html)

+   [SQL 备忘单](https://www.kdnuggets.com/2018/07/sql-cheat-sheet.html)

### 更多相关话题

+   [OLAP 死了吗？](https://www.kdnuggets.com/2022/10/olap-dead.html)

+   [OLAP 与 OLTP：数据处理系统的比较分析](https://www.kdnuggets.com/2023/08/olap-oltp-comparative-analysis-data-processing-systems.html)

+   [4 个有用的中级 SQL 查询用于数据科学](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [解决 5 个棘手的 SQL 查询](https://www.kdnuggets.com/2020/11/5-tricky-sql-queries-solved.html)

+   [解决 5 个复杂的 SQL 问题：难解查询解释](https://www.kdnuggets.com/2022/07/5-hardest-things-sql.html)

+   [KDnuggets 新闻，12 月 7 日：十大数据科学神话破解 • 4…](https://www.kdnuggets.com/2022/n47.html)
