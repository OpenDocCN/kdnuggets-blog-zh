# 开始使用图形数据库查询，附带备忘单！

> 原文：[https://www.kdnuggets.com/getting-started-with-graph-database-queries-with-cheat-sheet](https://www.kdnuggets.com/getting-started-with-graph-database-queries-with-cheat-sheet)

图形数据库每年都在获得关注。它们不会完全取代关系数据库，也不打算这样做。但它们会开始进入数据湖和数据仓库 struggling 的领域。图形数据库在分析事件、资源和人员的网络时更快、更直观：

+   涉及复杂模式的金融交易，以及偶尔的欺诈行为

+   患者、医务人员、设施和设备之间的医疗保健互动

+   客户、供应商、承包商和产品的供应链网络

+   生产物料清单及输入材料的配方

这些类型的网络关系在关系型或维度数据模型中难以建模和可视化。图形数据库提供了一种结构，来模拟业务中的现实世界网络。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

[![开始使用图形数据库查询，附带备忘单！](../Images/ab566ed79c28b876e2665122ca3f03cd.png)](https://www.kdnuggets.com/wp-content/uploads/Queries_with_Cypher_Cheat_Sheet_Pugsley.pdf)

当你开始使用图形数据库和查询语言时，重要的是要为思维模式的变化做好准备。首先，目前还没有广泛接受的标准查询语言像 SQL 一样。正如你在附件中看到的，存在一组竞争的语言和一个委员会在努力让所有人达成一致的 GQL 标准。为了今天的目的，我们将使用 Cypher 查询语言，该语言由顶级数据库供应商 Neo4j 开发和推广。

在图形查询中，我们丢失了一些 SQL 的语法，并获得了其他语法。SELECT 被 MATCH 取代。FROM 和 JOIN 被丢弃。但 WHERE 和 ORDER BY 命令的用法保持不变。像 SUM 和 AVG 这样的聚合函数仍然存在，但 GROUP BY 已被丢弃。最重要的是，我们可以通过节点关系查询图中的模式。在附带的备忘单中，你会看到最常用查询方法的列表。

以下是将用于[附带备忘单](https://www.kdnuggets.com/wp-content/uploads/Queries_with_Cypher_Cheat_Sheet_Pugsley.pdf)的图模型：

[![开始使用图数据库查询，附备忘单！](../Images/5d3a97e58a9a5cb96b65e38e775b857c.png)](https://www.kdnuggets.com/wp-content/uploads/Queries_with_Cypher_Cheat_Sheet_Pugsley.pdf)

我选择了一个租赁图，因为几乎每个人在生活中都曾租过房！显然，如果我们为每个节点添加完整的属性列表，这个图会复杂得多。

下一步是进行一些实践。你可以从[Kaggle](https://www.kaggle.com/datasets/startupsci/awesome-datasets-graph)等来源或[JanusGraph](https://janusgraph.org/)或[Neo4j](https://neo4j.com/developer/example-data/)等供应商处下载一个样本数据集。

如果你在雇主或业余项目中有涉及网络关系的数据集，可以尝试使用图数据库。你会发现那些在关系数据库中显得不合适的数据在图数据库中却很合适！

[**立即下载备忘单！**](https://www.kdnuggets.com/wp-content/uploads/Queries_with_Cypher_Cheat_Sheet_Pugsley.pdf)

[](https://www.linkedin.com/in/spugsley/)****[Stan Pugsley](https://www.linkedin.com/in/spugsley/)**** 是一名驻盐湖城, UT的自由数据工程和分析顾问。他还是犹他大学埃克尔斯商学院的讲师。你可以通过[电子邮件](mailto:S.pugsley@utah.edu)联系作者。

### 更多相关内容

+   [ChatGPT 备忘单](https://www.kdnuggets.com/2023/01/chatgpt-cheat-sheet.html)

+   [使用 Python 清理数据备忘单](https://www.kdnuggets.com/2023/02/data-cleaning-python-cheat-sheet.html)

+   [KDnuggets 2023 备忘单合集](https://www.kdnuggets.com/the-kdnuggets-2023-cheat-sheet-collection)

+   [Streamlit 机器学习备忘单](https://www.kdnuggets.com/2023/01/streamlit-machine-learning-cheat-sheet.html)

+   [Docker 数据科学备忘单](https://www.kdnuggets.com/2023/02/docker-data-science-cheat-sheet.html)

+   [ChatGPT 数据科学备忘单](https://www.kdnuggets.com/2023/03/chatgpt-data-science-cheat-sheet.html)
