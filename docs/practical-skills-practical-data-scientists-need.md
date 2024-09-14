# 实用数据科学家需要的实际技能

> 原文：[https://www.kdnuggets.com/2016/05/practical-skills-practical-data-scientists-need.html](https://www.kdnuggets.com/2016/05/practical-skills-practical-data-scientists-need.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Noah Lorang](https://twitter.com/noahhlo), Basecamp**。

![技能](../Images/d148738d1d32c44877be61597b10e380.png)当我写到我[主要只用算术](https://m.signalvnoise.com/data-scientists-mostly-just-do-arithmetic-and-that-s-a-good-thing-c6371885f7f6#.spcfjporf)时，很多人问我如果没有复杂的算法，数据科学家需要哪些技能或工具。我提到的这种神秘的“基础数学”是什么？这是我对实际工作所需技能的看法，我在[Basecamp](https://basecamp.com/?source=svn)的工作主要是进行简单分析，以解决实际的业务问题。

#### 最重要的技能：能够理解业务和问题

我会在一会儿讨论你可以在教科书中学习的实际技能，但首先我必须强调一点：**数据科学家的真正核心技能是理解业务和问题的能力，以及强烈的知识好奇心**。你作为一个业务要实现什么？你的客户是谁？你什么时候销售你的产品？业务的基本经济是什么？利润率高还是适中？你有很多小客户还是少数大客户？你的产品范围有多广？你在与谁竞争？业务面临的挑战是什么，你在解决这个问题或为决策提供意见时的作用是什么？可信的答案范围是什么？谁参与解决这个问题？分析是否真的能带来不同？在这个问题上投入多少时间是值得的？

#### 理解数据

在你查看任何数据或进行任何数学计算之前，数据科学家需要理解数据的来源、结构和意义。即使其他人去获取存储数据的地方并将数据提供给你，你仍然需要理解数据的来源及其各个部分的含义。数据质量在组织之间和内部之间差异很大；在某些情况下，你会有一个详尽的数据字典，而在其他情况下你可能什么都没有。无论如何，你都需要能够回答以下问题：

+   我解决问题需要哪些数据？

+   那些数据在哪里？在关系数据库中？在磁盘上的日志文件中？在第三方服务中？

+   数据有多全面（时间和范围）？是否存在覆盖或保留的漏洞？

+   数据中的每个字段在实际的人类或计算机行为中是什么意思？

+   数据中的每个字段有多准确？它来自直接观察、自我报告、第三方来源还是推测？

+   我如何以最小化违反他人隐私的风险的方式使用这些数据？

#### SQL 技能

无论是好是坏，大多数数据科学家需要的数据都存在于使用 SQL 的关系数据库中，无论是 MySQL、Postgres、Hive、Impala、Redshift、BigQuery、Teradata、Oracle 还是其他什么。你的使命是将数据从这些关系数据库中解放出来，而不使数据库实例崩溃，拉取过多或过少的数据，获取不准确的数据，或等待一年才能完成查询。

几乎每个数据科学家编写的查询，用于获取数据以分析以解决业务问题的，将是一个 SELECT 语句。我认为必需的 SQL 基本概念和函数有：

+   DESCRIBE 和 EXPLAIN

+   WHERE 子句，包括 IN（…）

+   GROUP BY

+   连接，主要是左连接和内连接

+   使用已索引的字段

+   LIMIT 和 OFFSET

+   LIKE 和 REGEXP

+   if()

+   字符串操作，主要是 left() 和 lower()

+   日期操作：date_add、datediff、到和从 UNIX 时间戳、时间组件提取

+   regexp_extract（如果你运气好使用支持它的数据库）或 substring_index（如果运气稍差）

+   子查询

#### 基本数学技能

一旦你有了一些数据，你就可以进行一些数学运算。我认为的必备数学技能和概念的清单不长：

+   算术（加法、减法、乘法、除法）

+   百分比（总量、差异与其他值的比较）

+   均值和中位数（以及均值与中位数的比较）

+   百分位数

+   直方图和累积分布函数

+   对概率、随机性和抽样的理解

+   增长率（简单和复合）

+   幂分析（用于比例和均值）

+   显著性检验（用于比例和均值）

这并不是一组非常复杂的内容。**这不是关于数学，而是关于你解决的问题。**

#### 稍微高级一点的数学概念

有时，一些更高级的数学或 SQL 概念或技能对常见的业务问题是有价值的。我常用的一些较常见的内容包括：

+   如果你的数据库支持，使用分析函数（lead()、lag()、rank() 等）

+   现值和未来值以及折现率

+   生存分析

+   线性和逻辑回归

+   词袋模型文本表示

有些问题需要更高级的技术，我并不是想贬低或忽视这些技术。**如果你的业务确实可以从深度学习等技术中受益，恭喜你！** 那可能意味着你已经解决了业务面临的所有简单问题。

[原文](https://m.signalvnoise.com/practical-skills-that-practical-data-scientists-need-da71e6b93f95#.xnit6hk9u)

**相关：**

+   [构建数据科学职业的免费建议](/2016/05/advice-building-data-science-career.html)

+   [数据科学家 – 让自己未来可用](/2016/05/data-scientists-future-proof-yourselves.html)

+   [数据科学家的职业建议 – 去赚更多的钱](/2016/03/career-advice-data-scientists-make-more-money.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织中的 IT 工作

* * *

### 更多相关话题

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [数据科学家的实用统计学](https://www.kdnuggets.com/2023/05/practical-statistics-data-scientists.html)

+   [数据科学入门：你需要了解的 10 项核心技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [成为数据工程师所需的 9 项技能](https://www.kdnuggets.com/2021/03/9-skills-become-data-engineer.html)

+   [想用你的数据技能解决全球问题？这是…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [数据分析师职位晋升所需的技能](https://www.kdnuggets.com/2022/09/data-analyst-skills-need-next-promotion.html)
