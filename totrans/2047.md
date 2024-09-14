# 为什么ETL的未来不是ELT，而是EL(T)

> 原文：[https://www.kdnuggets.com/2020/12/future-etl-is-elt.html](https://www.kdnuggets.com/2020/12/future-etl-is-elt.html)

[评论](#comments)

**由[John Lafleur](https://www.linkedin.com/in/jeanhenrilafleur/)，Airbyte.io联合创始人**。

![](../Images/e532db7a6e5a91f7cfef4274593b636c.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

在过去十年中，我们存储和管理数据的方式发生了彻底变化。我们从ETL世界转变到ELT世界，Fivetran等公司推动了这一趋势。然而，我们认为这还不会就此停止；ELT在我们看来是向EL(T)过渡（将EL与T解耦）。要理解这一点，我们需要探讨这种趋势的根本原因，因为它们可能揭示了未来的发展方向。

这正是我们将在本文中讨论的内容。我是[Airbyte](https://airbyte.io)的联合创始人，这是一个即将到来的数据集成开源标准。

### ETL存在什么问题？

历史上，数据管道过程包括**提取**、**转换**和**加载**数据到仓库或数据湖中。这个顺序存在严重的缺陷。

**不灵活性**

ETL固有地很僵化。它迫使数据分析师事先了解他们将如何使用数据、将生成哪些报告。任何更改都可能代价高昂，并且可能会影响初始提取下游的数据消费者。

**缺乏可见性**

对数据进行的每一次转换都会掩盖一些底层信息。分析师不会看到仓库中的所有数据，只会看到在转换阶段保留下来的数据。这是有风险的，因为结论可能基于未正确切分的数据。

**分析师缺乏自主性**

最后但同样重要的是，建立一个基于ETL的数据管道通常超出了分析师的技术能力。这通常需要工程人才的密切参与，以及额外的代码来提取和转换每个数据源。

复杂的工程项目的替代方案是进行临时分析和报告，这种方法费时且最终不可持续。

### 什么改变了，为什么ELT更好

**基于云的计算和数据存储**

由于本地计算和存储的高成本，ETL方法曾经是必要的。随着基于云的数据仓库（如Snowflake）的快速增长和云计算及存储成本的急剧下降，继续在最终目的地加载之前进行转换的理由几乎不存在。实际上，交换这两个步骤使得分析师能够以更自主的方式做得更好。

**ELT支持分析师的敏捷决策**

当分析师可以在转换数据之前加载数据时，他们不必事先确定确切需要生成的见解，从而决定所需的确切架构。

相反，底层源数据被直接复制到数据仓库中，形成一个**“单一真实来源”**。分析师可以根据需要对数据进行转换。分析师始终可以回到原始数据，并且不会因为可能**损害数据完整性的转换**而受到影响，从而拥有更大的自由度。这使得商业智能过程变得无与伦比地更灵活和安全。

**ELT促进了公司整体的数据素养**

当与基于云的商业智能工具（如Looker、Mode和Tableau）结合使用时，ELT方法还扩展了跨组织的分析访问。即使是相对非技术用户也可以访问商业智能仪表板。

我们在Airbyte也是ELT的忠实粉丝。但ELT[并未完全解决数据集成问题](https://airbyte.io/articles/data-engineering-thoughts/how-we-can-commoditize-data-integration-pipelines/)并且有其自身的问题。我们认为EL需要与T完全解耦。

### 目前正在发生什么变化，以及为什么EL(T)是未来趋势

**数据湖和数据仓库的合并**

Andreessen Horowitz关于[数据基础设施如何演变](https://a16z.com/2020/10/15/the-emerging-architectures-for-modern-data-infrastructure/)的分析非常出色。以下是他们在与行业领导者进行大量访谈后提出的现代数据基础设施架构图。

![](../Images/a2fdbe3ebab3bb46a95a2cd37da1e975.png)

数据基础设施在高层次上有两个主要目的：

1.  通过使用数据来帮助商业领导者做出更好的决策 - **分析用例**

1.  将数据智能构建到面向客户的应用程序中，包括通过机器学习 - **操作用例**

围绕这些广泛用例形成了两个平行的生态系统。

数据仓库构成了分析生态系统的基础。大多数仓库以结构化格式存储数据。它们旨在从核心业务指标中生成见解，通常使用SQL（尽管Python的受欢迎程度在增长）。

数据湖是操作生态系统的骨干。通过以原始形式存储数据，它提供了应用程序和更高级数据处理需求所需的灵活性、规模和性能。数据湖支持包括 Java/Scala、Python、R 和 SQL 在内的广泛语言。

令人真正感兴趣的是，现代数据仓库和数据湖开始彼此相似——两者都提供商品存储、原生水平扩展、半结构化数据类型、ACID 事务、交互式 SQL 查询等等。

所以你可能在想数据仓库和数据湖是否在趋向融合。它们会在堆栈中变得可互换吗？数据仓库也会用于操作用例吗？

**EL(T) 支持两种用例：分析和操作 ML**

与 ELT 相比，EL 完全将提取-加载部分与可能发生的任何可选转换解耦。

操作用例在利用传入数据的方式上都是独特的。有些可能使用独特的转换过程；有些可能根本不使用任何转换。

关于分析用例，分析师最终需要将传入的数据标准化以满足他们自己的需求。但将 EL 从 T 中解耦将允许他们选择任何他们想要的标准化工具。[DBT](https://www.getdbt.com/) 最近在数据工程和数据科学团队中获得了很多关注。它已成为转换的开源标准。即使是 Fivetran 也与他们集成，以便团队可以使用他们习惯的 DBT。

**EL 比较快，并利用整个生态系统**

转换是所有边缘情况所在的地方。对于公司内的每个特定需求，每个工具都有其独特的模式标准化。

将 EL 从 T 中解耦，使得行业可以开始覆盖连接器的长尾。在 Airbyte，我们正在建立一个 “[连接器制造厂](https://airbyte.io/articles/data-engineering-thoughts/how-to-build-thousands-of-connectors/)”，以便在几个月内实现 1,000 个预构建的连接器。

此外，如上所述，它将帮助团队以更轻松的方式利用整个生态系统。你开始看到每个需求的开源标准。从某种意义上说，未来的数据架构可能会是这样的：

![](../Images/cdeabfeb3d3ce3d7b3f694a75d84cda7.png)

最终，提取和加载将与转换解耦。你同意我们的观点吗？

[原文](https://airbyte.io/articles/data-engineering-thoughts/why-the-future-of-etl-is-not-elt-but-el/)。转载经许可。

**简介：** [约翰·拉弗勒](https://twitter.com/JeanLafleur) 是 [Airbyte](http://airbyte.io) 的联合创始人，该公司是数据集成的新开源标准。我还是 SDTimes、Linux.com、TheNewStack、Dzone 的作者……也是一个快乐的丈夫和父亲 :)

**相关：**

+   [ETL与ELT：考虑数据仓库的进展](https://www.kdnuggets.com/2018/05/etl-vs-elt-considering-advancement-data-warehouses.html)

+   [数据工程简介](https://www.kdnuggets.com/2020/12/introduction-data-engineering.html)

+   [数据工程师的角色正在发生变化](https://www.kdnuggets.com/2019/01/role-data-engineer-changing.html)

### 更多相关内容

+   [构建一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [ETL与ELT：哪个适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [ETL与ELT：数据集成对决](https://www.kdnuggets.com/2022/08/etl-elt-data-integration-showdown.html)

+   [SQL与数据集成：ETL与ELT](https://www.kdnuggets.com/2023/01/sql-data-integration-etl-elt.html)
