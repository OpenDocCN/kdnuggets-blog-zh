# 数据工程——数据科学的“表亲”，颇具挑战

> 原文：[`www.kdnuggets.com/2021/01/data-engineering-troublesome.html`](https://www.kdnuggets.com/2021/01/data-engineering-troublesome.html)

评论

**作者：[Lissie Mei](https://www.linkedin.com/in/lan-mei/)，Visa 数据科学家**。

我们总是认为数据科学是“21 世纪最性感的工作”。当涉及到从传统公司向分析公司转型时，公司的期待或数据科学家们都希望尽快进入数据分析的炫目世界。但情况总是如此吗？

* * *

## 我们的三大推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业之路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

### 一个麻烦的开始

自从我们 UC Davis 的实习团队开始与 Hilti，一家领先的电动工具及相关服务制造公司，合作以来，我们制定了几个精彩的蓝图：定价自动化、倾向模型……与这样一家伟大的公司合作对我们而言是一个宝贵的机会，我们迫不及待地想要发挥我们的分析技能以创造商业价值。但当我们开始接触数据时，我们发现，与数据驱动的公司如电子商务公司相比，从传统公司直接获取干净且结构化的数据是非常困难的。

由于我主要负责项目的数据清理和工程，我亲眼见证了由于数据准备不足，我们在分析进展中受到的阻碍。

![](img/c0a93d8bf5dd94040db94ce4650ab50e.png)

*我亲眼见证了由于数据准备不足，我们在分析进展中受到的阻碍。*

我们直接与财务团队合作，但另一个团队，定价运营，实际上负责数据库的管理。最初，由于我们几乎无法及时请求和查询数据或联系相关人员，流程进展缓慢。此外，由于 Hilti 的销售数据较为敏感，公司缺乏安全的数据传输方式，每次数据请求都需要耗时的数据掩码处理。第三，数据工程的不足导致了多个参考表之间的不一致，我们几乎无法建立可靠的模型或得出结论。最后，我们必须处理各种数据类型：CSV、JSON、SQLite 等……虽然这是一个学习的好机会。

大约两个月后，我们准备好了所有的数据，每一个异常情况也都被讨论并解决。

### 深入探索时间！

我们精心开发的可视化框架和模型迫不及待想要尝试新数据。然而，当我们用实际数据展示第一个提案时，最尴尬的事情发生了。

![](img/feb009606c86ede07382d0e4db1377e8.png)

猜猜怎么了？那些大数字似乎对不上。经过简短的讨论，我们意识到我们根本没有收到完整的数据。我们只关注了数据的细节，例如异常值和数据源之间的关系，却忘记了进行基本的检查，比如总和和计数。这是我将终身铭记的教训。真的！

### 数据工程为何如此重要

我从数据工程的经历中学到的最重要的一点是，那些在幕后工作的角色，如数据工程师，实际上掌握了创新的门户。当传统公司考虑利用数据时，最有效且最初的行动应该是改善数据工程流程。拥有优秀的数据工程师，公司可以构建一个健康且可扩展的数据管道，这使得数据分析师进行数据挖掘和发现商业洞察变得更容易。

![](img/8704583cbadc582876b072f55f0a5ddb.png)

我还了解到，为什么许多公司要求数据分析师掌握编程相关工具，比如 Python 和 Scala，除了分析工具如 SQL 和 Excel。通常，我们不能期待一个“全栈”分析师，但确实需要有能够与工程人员和管理人员沟通的人。尽管明确的工作分配对高效率很重要，但每个数据工具的专家确实很有吸引力。

![](img/50aaa0cbe34aea6f932299ef4c7cf59d.png)

*全栈……有道理！*

我期待自己未来能够学习前端和后端的知识，比如 Java、JavaScript、Kafka、Spark 和 Hive，我相信这些最终会成为我经验中的亮点。

[原文](https://towardsdatascience.com/data-engineering-the-cousin-of-data-science-is-troublesome-3a9332b532ae)。经授权转载。

**相关：**

+   [如何像数据科学家一样思考](https://www.kdnuggets.com/2020/05/think-like-data-scientist-data-analyst.html)

+   [如何构建颠覆性的 数据科学团队：10 大最佳实践](https://www.kdnuggets.com/2019/07/disruptive-data-science-teams-best-practices.html)

+   [构建高效数据科学团队](https://www.kdnuggets.com/2019/03/building-effective-data-science-teams.html)

### 相关话题

+   [成为优秀数据科学家需要的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
