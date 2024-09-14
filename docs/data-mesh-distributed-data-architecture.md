# 数据网格及其分布式数据架构

> 原文：[`www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html`](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)

![数据网格及其分布式数据架构](img/48a44ce10ca7a27509371732af4ec1bf.png)

图片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleria?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

企业希望更快响应并提供卓越的客户体验，这需要对数据管理进行全面的重塑。迄今为止，技术已经解决了存储和处理大数据的问题。它还具备了将大数据进行深度分析的能力。与此同时，预计到 2025 年，先进数据管理解决方案的全球市场规模将达到 1229 [亿美元。](https://www.marketsandmarkets.com/Market-Reports/enterprise-data-management-market-69219339.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 管理

* * *

然而，数据来源类型和数量的日益多样化继续阻碍无缝的数据生命周期。迄今为止，数据管理领域主要是将数据捕获和流式传输到一个中心化的数据湖中。数据湖会进一步处理和清洗数据集。展望未来，数据专业人士找到了一种通过数据网格来解决数据来源扩展性的全新方法。

### 什么是数据网格？

[数据网格是一种分布式](https://www.k2view.com/what-is-data-mesh)架构解决方案，用于分析数据的生命周期管理。基于去中心化，数据网格消除了数据可用性和访问性的障碍。它使用户能够从多个来源捕捉和操作洞察，无论这些来源的地点和类型如何。随后，它执行自动化查询，而无需将数据传输到中心化的数据湖中。

数据网格的分布式架构去中心化了每个业务领域的所有权。这意味着每个领域都掌控着分析和运营用例的数据质量、隐私、新鲜度、准确性和合规性。

### 从中心化数据湖迁移到分布式网格

随着数据源数量的不断增加，数据湖无法进行按需集成。使用数据网格，将大量数据倾倒入数据湖的做法正处于过时的边缘。

新的数据管理框架确保了[所有节点的协作参与](https://martinfowler.com/articles/data-mesh-principles.html)，每个节点控制一个特定的业务单元。它通过遵循数据即产品的原则来实现这一点。这意味着每个数据集都被视为一个数字产品，包含干净、完整和有结论的数据集。这些数据集可以随时随地按需交付。对于一个快速增长的数据管理生态系统来说，数据网格是提供组织数据洞察的重要方法。

所有权的去中心化减少了对工程师和科学家的依赖。每个业务单元控制其自身的特定领域数据。然而，每个领域仍然依赖于中心化的标准化数据建模、安保协议和治理合规政策。

### 使用数据网格和数据结构

任何关于数据管理的讨论如果遗漏了数据结构架构，那将是不完整和无关的。关于数据结构和数据网格相互竞争的说法是一个误解。这是不正确的。[Gartner](https://www.gartner.com/doc/reprints?id=1-280YDJZH&ct=211111&st=sb&utm_campaign=TY%20Mailers&utm_medium=email&_hsmi=182672238&_hsenc=p2ANqtz-8kQ8DrUdogEgQB4Ulx4InzOu_gy95yCENtpZq6NkaVKEUffOhkVsdICYrSdhJFoVWz5mnIrB_Jw7Vv_63MkvWj7NH8yg&utm_content=182672238&utm_source=hs_automation) 已经对这两个标题进行了并排讨论，并澄清了这一点。

数据结构是一种经典而相关的架构，推动了[不同工业中的结构使用](https://expersight.com/global-data-fabric-market-trend-analysis/)。它自动发现并提出管理架构，从而简化整个数据生命周期。它还支持验证数据对象和上下文引用，以便重新使用这些对象。数据网格通过利用当前的主题领域专长并为数据对象准备解决方案来实现这一点。

关于数据结构和数据网格相互竞争的说法是一个误解。这是不正确的。实际上，数据结构可能在从数据网格架构中提取最佳价值方面发挥重要作用。

### 使用基于实体的数据结构实施数据网格

以 K2View 的基于实体的数据结构架构为例。它为每个业务实体保存数据到一个独立的微型数据库，从而支持成千上万个这些数据库。此外，通过将‘业务实体’和‘数据作为产品’的概念融合，他们的结构支持数据网格设计模式的实施。在这里，数据结构创建了一个连接来自多个来源的数据集的集成层。这为操作和分析工作负载提供了全景视图。

实体基础结构标准化了所有数据产品的语义定义。根据规定，它建立了数据摄取方法和治理政策，以确保数据集的安全性。由于基础结构的支持，网格模式在实体级存储方面表现更好。

因此，对于网格分布式网络中的每个业务领域，都会部署一个专属的基础结构节点。这些特定于特定业务实体的领域拥有对服务和管道的本地控制，以便为消费者访问产品。

### 去中心化数据所有权模型

企业必须将来自多个来源的多种数据类型导入集中式存储库，如数据湖。在这里，数据处理通常消耗大量精力，并且容易出错。查询这种异构数据集进行分析会直接增加成本。因此，数据专业人士一直在寻找这种集中式方法的替代方案。凭借 Mesh 的分布式架构，他们能够实现每个业务实体的所有权去中心化。现在，这种模式减少了生成定性见解的时间，从而增加了核心目标的价值——快速访问数据并影响关键业务决策。

去中心化方法解决了更多问题。例如，传统数据管理中的查询方法可能会因数据量无法控制的增长而失去效率。这必然会迫使整个流程发生变化，并最终无法响应。因此，随着数据源数量的增加，响应时间急剧下降。这一直影响着提取数据价值和扩大业务成果的过程敏捷性。

通过去中心化，Mesh [将所有权分配](https://roundup.getdbt.com/p/data-mesh-contracts-and-distributed)给不同的领域，以应对数据量的挑战，并最终在其层面上、为其相关的数据集执行查询。因此，该架构使企业流程能够缩短事件与其消费分析之间的差距。企业能够在关键决策制定方面进行改进。

通过提供数据即服务架构，Mesh 带来了业务运营的敏捷性。它不仅减少了 IT 积压，还使数据团队只处理精简和相关的数据流。

因此，授权的消费者可以轻松访问他们各自的数据集，而无需了解其底层复杂性。

### 结论

从数字数据开始，Web 3.0 致力于去中心化企业流程。数据管理是这一方向上的一个重要用例。显然，集中式权威在处理爆炸性增长的数据方面已经超出了某个极限。期待 2022 年，它将把 Data Mesh 架构推向前沿。

**[Yash Mehta](https://www.linkedin.com/in/yash-mehta-esthan/)** 是一位物联网和大数据爱好者，他在 IDG、IEEE、Entrepreneur 等刊物上发表了许多文章。他共同开发了像[Getlua](https://getlua.com/)这样的平台，允许用户轻松地[合并多个文件](https://getlua.com/merge-pdf)。他还创办了一个研究平台，从专家那里生成可操作的见解。

### 更多相关内容

+   [KDnuggets™ 新闻 22:n07，2 月 16 日：如何学习机器学习数学](https://www.kdnuggets.com/2022/n07.html)

+   [数据网格架构：重新构想数据管理](https://www.kdnuggets.com/2022/05/data-mesh-architecture-reimagining-data-management.html)

+   [KDnuggets 新闻，5 月 18 日：5 个免费机器学习托管平台](https://www.kdnuggets.com/2022/n20.html)

+   [探索数据网格：数据架构的范式转变](https://www.kdnuggets.com/exploring-data-mesh-a-paradigm-shift-in-data-architecture)

+   [如何使用 Apache Kafka 构建可扩展的数据架构](https://www.kdnuggets.com/2023/04/build-scalable-data-architecture-apache-kafka.html)

+   [文本分类任务的最佳架构：基准测试](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)
