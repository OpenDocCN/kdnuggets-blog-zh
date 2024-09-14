# 大数据如何实时拯救生命：IoV数据分析帮助预防事故

> 原文：[https://www.kdnuggets.com/how-big-data-is-saving-lives-in-real-time-iov-data-analytics-helps-prevent-accidents](https://www.kdnuggets.com/how-big-data-is-saving-lives-in-real-time-iov-data-analytics-helps-prevent-accidents)

![大数据如何实时拯救生命：IoV数据分析帮助预防事故](../Images/807e4f5d57c2f19d7e603e226737eb85.png)

图片来源：[罗伯托·尼克松](https://www.pexels.com/photo/person-sitting-inside-car-2526127/)

车联网（IoV）是汽车工业与物联网（IoT）结合的产物。预计IoV数据将越来越大，特别是随着电动车成为汽车市场的新增长引擎。问题是：你的数据平台是否准备好了？这篇文章展示了一个适用于IoV的OLAP解决方案的样子。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

# IoV数据有什么特别之处？

IoV的理念很直观：创建一个网络，使车辆能够彼此之间或与城市基础设施共享信息。常常被解释不足的是每辆车内部的网络。每辆车上都有一个称为控制器局域网（CAN）的系统，它作为电子控制系统的通信中心。对于在道路上行驶的汽车来说，CAN是安全和功能的保障，因为它负责：

+   **车辆系统监控**：CAN是车辆系统的脉搏。例如，传感器将检测到的温度、压力或位置发送到CAN；控制器通过CAN向执行器发出指令（如调整阀门或驱动电机）。

+   **实时反馈**：通过CAN，传感器将速度、转向角度和刹车状态发送到控制器，控制器及时调整车辆以确保安全。

+   **数据共享与协调**：CAN允许各种设备之间的数据交换（如状态和指令），使整个系统更加高效和性能优越。

+   **网络管理与故障排除**：CAN监视系统中的设备和组件。它识别、配置并监控设备以进行维护和故障排除。

由于CAN的繁忙，你可以想象每天通过CAN传输的数据规模。在本篇文章中，我们谈论的是一家连接了400万辆车的汽车制造商，每天需要处理1000亿条CAN数据。

# IoV数据处理

将这些巨大的数据规模转化为指导产品开发、生产和销售的有价值信息是关键部分。像大多数数据分析工作负载一样，这归结于数据写入和计算，这也是存在挑战的地方：

+   **大规模数据写入**：传感器在汽车的各个地方都有：车门、座椅、刹车灯……此外，许多传感器会收集多个信号。400万辆车累计的数据吞吐量达到数百万TPS，这意味着每天有数十TB的数据。随着汽车销售的增加，这个数字还在增长。

+   **实时分析**：这也许是“时间就是生命”最好的体现。汽车制造商收集来自车辆的实时数据，以识别潜在故障，并在损害发生之前修复它们。

+   **低成本计算和存储**：谈论巨大的数据规模时，很难不提到其成本。低成本使得大数据处理具有可持续性。

# 从Apache Hive到Apache Doris：向实时分析的过渡

就像罗马不是一天建成的，一个实时数据处理平台也不是一蹴而就的。汽车制造商曾经依赖批量分析引擎（Apache Hive）和一些流处理框架和引擎（Apache Flink，Apache Kafka）的组合，以获得接近实时的数据分析性能。直到实时成为问题，他们才意识到实时的必要性。

## 接近实时数据分析平台

这曾经对他们有效：

![实时大数据如何拯救生命：IoV数据分析帮助预防事故](../Images/f0014c2e83f16109e153d931d9bd4f20.png)

来自CAN和车辆传感器的数据通过4G网络上传到云网关，云网关将数据写入Kafka。然后，Flink处理这些数据并将其转发到Hive。在Hive中经过几个数据仓库层后，汇总的数据被导出到MySQL。最后，Hive和MySQL将数据提供给应用层，用于数据分析、仪表盘等。

由于Hive主要设计用于批处理而非实时分析，你可以看出它在这种用例中的不匹配。

+   **数据写入**：由于数据规模巨大，从Flink到Hive的数据摄取时间明显较长。此外，Hive仅支持按分区粒度更新数据，这对于某些情况来说是不够的。

+   **数据分析**：基于Hive的分析解决方案带来了高查询延迟，这是一个多因素问题。首先，Hive在处理1亿行的大表时比预期要慢。其次，在Hive中，数据通过Spark SQL从一层提取到另一层，这可能需要一些时间。第三，由于Hive需要与MySQL配合以满足应用需求，Hive和MySQL之间的数据传输也增加了查询延迟。

## 实时数据分析平台

当他们在场景中加入实时分析引擎时，会发生这样的情况：

![实时大数据如何挽救生命：物联网数据分析帮助防止事故](../Images/e07a206c858c8f8431db0cacdca2bc9d.png)

与旧的基于Hive的平台相比，这个新平台在以下三方面更为高效：

+   **数据写入**：将数据导入到[Apache Doris](https://doris.apache.org/)非常快捷和简单，无需复杂的配置和额外组件的引入。它支持多种数据导入方式。例如，在这种情况下，数据通过[Stream Load](https://doris.apache.org/docs/data-operate/import/import-way/stream-load-manual)从Kafka写入Doris，通过[Broker Load](https://doris.apache.org/docs/data-operate/import/import-way/broker-load-manual)从Hive写入Doris。

+   **数据分析**：以实例展示Apache Doris的查询速度，它可以在几秒钟内返回一个包含1000万行的结果集。此外，它还可以作为一个[统一查询网关](https://doris.apache.org/docs/lakehouse/multi-catalog/)，快速访问外部数据（Hive, MySQL, Iceberg等），使分析师无需在多个组件之间切换。

+   **计算和存储成本**：Apache Doris使用Z-Standard算法，可以实现3~5倍的数据压缩比。这就是它如何帮助降低数据计算和存储成本的原因。此外，压缩可以仅在Doris中完成，因此不会消耗Flink的资源。

一个好的实时分析解决方案不仅关注数据处理速度，还考虑到整个数据管道的每一步，并使每一步都顺畅。以下是两个例子：

### 1\. CAN数据的排列

在Kafka中，CAN数据按CAN ID的维度进行排列。然而，为了数据分析，分析师需要比较来自不同车辆的信号，这意味着需要将不同CAN ID的数据连接到一个平面表中，并按时间戳对齐。从这个平面表中，他们可以派生出不同的表以用于不同的分析目的。这种转换是通过Spark SQL实现的，在旧的基于Hive的架构中，处理时间较长，而且SQL语句维护成本高。此外，数据是以每天批量更新的，这意味着他们只能获取前一天的数据。

在Apache Doris中，他们只需要使用[聚合键模型](https://doris.apache.org/docs/data-table/data-model#aggregate-model)构建表，指定VIN（车辆识别码）和时间戳作为聚合键，并通过REPLACE_IF_NOT_NULL定义其他数据字段。使用Doris，他们不需要处理SQL语句或平面表，而是能够从实时数据中提取实时见解。

![大数据如何实时拯救生命：物联网数据分析帮助预防事故](../Images/a0375b73a24a80a493e2c4b0f4ec8fc7.png)

### 3\. DTC数据查询

所有CAN数据中，DTC（诊断故障代码）值得高度关注和单独存储，因为它告诉你汽车出了什么问题。每天，制造商接收大约10亿条DTC信息。为了从DTC中捕获拯救生命的信息，数据工程师需要将DTC数据与MySQL中的DTC配置表关联起来。

他们过去的做法是每天将DTC数据写入Kafka，在Flink中处理，并将结果存储在Hive中。这样，DTC数据和DTC配置表被存储在两个不同的组件中。这造成了一个困境：一个有10亿行的DTC表很难写入MySQL，而从Hive查询则很慢。由于DTC配置表也在不断更新，工程师只能定期将其某个版本导入Hive。这意味着他们无法总是将DTC数据与最新的DTC配置相关联。

如前所述，Apache Doris可以作为统一的查询网关。这得益于其[多目录](https://doris.apache.org/docs/lakehouse/multi-catalog/)功能。他们将DTC数据从Hive导入Doris，然后在Doris中创建一个MySQL目录，以映射到MySQL中的DTC配置表。当所有这些完成后，他们可以简单地在Doris中联接这两张表，并获得实时查询响应。

![大数据如何实时拯救生命：物联网数据分析帮助预防事故](../Images/415f1de3b2195683b1c5357f15d07a53.png)

# 结论

这是一个实际的物联网实时分析解决方案。它针对非常大规模的数据设计，目前支持一个每天接收100亿行新数据的汽车制造商，以改善驾驶安全和体验。

构建适合您使用案例的数据平台并不容易，希望这篇文章能帮助您构建自己的分析解决方案。

**[](https://www.linkedin.com/in/zaki-lu-99a06b148/)**[Zaki Lu](https://www.linkedin.com/in/zaki-lu-99a06b148/)**** 是前百度产品经理，现在是Apache Doris开源社区的DevRel。

### 更多相关主题

+   [今天90%的代码是为了防止失败，而这本身就是一个问题](https://www.kdnuggets.com/2022/07/90-today-code-written-prevent-failure-problem.html)

+   [金融中的Python：在Jupyter Notebook中实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [为何我们永远需要人类来训练AI——有时需要实时训练](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [实时AI与机器学习的特征存储](https://www.kdnuggets.com/2022/03/feature-stores-realtime-ai-machine-learning.html)

+   [AI的实时翻译](https://www.kdnuggets.com/2022/07/realtime-translations-ai.html)

+   [如何使用图数据库构建实时推荐引擎](https://www.kdnuggets.com/2023/08/build-realtime-recommendation-engine-graph-databases.html)
