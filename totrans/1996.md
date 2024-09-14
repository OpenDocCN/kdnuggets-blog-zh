# 如何在 Snowflake 上构建流式半结构化分析平台

> 原文：[https://www.kdnuggets.com/2023/07/build-streaming-semistructured-analytics-platform-snowflake.html](https://www.kdnuggets.com/2023/07/build-streaming-semistructured-analytics-platform-snowflake.html)

![如何在 Snowflake 上构建流式半结构化分析平台](../Images/0a41ce35c69079e4de0a6381e75f2dbb.png)

编辑者图片

# 介绍

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

Snowflake 是一种 SaaS，即软件即服务，特别适合对大量数据进行分析。该平台极其易于使用，非常适合业务用户、分析团队等从不断增长的数据集中获得价值。本文将深入探讨在 Snowflake 上为医疗数据创建流式半结构化分析平台的组件。我们还将讨论此阶段的一些关键考虑因素。

# 背景

医疗行业支持多种不同的数据格式，但我们将考虑最新的半结构化格式，即 FHIR（快速医疗互操作资源）来构建我们的分析平台。这种格式通常将所有以患者为中心的信息嵌入到一个 JSON 文档中。该格式包含大量信息，如所有医院接触、实验室结果等。当分析团队拥有一个可查询的数据湖时，可以提取有价值的信息，例如有多少患者被诊断为癌症等。假设所有这些 JSON 文件每 15 分钟通过不同的 AWS 服务或端 API 端点推送到 AWS S3（或其他公共云存储）。

# 架构设计

![如何在 Snowflake 上构建流式半结构化分析平台](../Images/62bc8cb5a28e2c4ab13d65099ab2f1da.png)

# 架构组件

1.  **AWS S3 到 Snowflake RAW 区域：**

1.  数据需要不断地从 AWS S3 流式传输到 Snowflake 的 RAW 区域。

1.  Snowflake 提供 Snowpipe 管理服务，可以以连续流式方式读取 S3 中的 JSON 文件。

1.  在 Snowflake RAW 区域中需要创建一个具有变体列的表，以保存原生格式的 JSON 数据。

1.  **Snowflake RAW 区域到流：**

1.  Streams 是一种管理更改数据捕获服务，能够捕获所有新到达的 JSON 文档到 Snowflake RAW 区域

1.  Streams应指向Snowflake RAW Zone表，并应设置为append=true

1.  Streams就像任何表一样，易于查询。

1.  **Snowflake任务1：**

1.  Snowflake任务类似于调度程序。查询或存储过程可以使用cron作业符号安排运行。

1.  在此架构中，我们创建任务1来从Streams中获取数据，并将其导入临时表。此层将被截断并重新加载。

1.  这样做是为了确保每15分钟处理一次新的JSON文档。

1.  **Snowflake任务2：**

1.  此层将原始JSON文档转换为分析团队可以轻松查询的报告表。

1.  要将JSON文档转换为结构化格式，可以使用Snowflake的lateral flatten功能。

1.  Lateral flatten是一个易于使用的功能，可以展开嵌套的数组元素，并可以使用‘：’符号轻松提取。

# 关键考虑事项

1.  推荐使用Snowpipe处理少量大文件。如果小文件在外部存储中未合并，成本可能会很高。

1.  在生产环境中，确保创建自动化进程以监控streams，因为一旦它们变得陈旧，数据将无法从中恢复。

1.  单个JSON文档允许的最大大小为16MB压缩，能够加载到Snowflake中。如果您有超出这些大小限制的大型JSON文档，请确保在将其导入Snowflake之前有一个拆分的过程。

# 结论

由于JSON文档中嵌入的元素的嵌套结构，管理半结构化数据始终具有挑战性。在设计最终报告层之前，请考虑即将到来的数据量的逐步和指数增长。本文旨在展示如何轻松构建半结构化数据的流管道。

**[Milind Chaudhari](https://www.linkedin.com/in/milind-chaudhari/)** 是一位经验丰富的数据工程师/数据架构师，拥有十年的工作经验，使用各种传统和现代工具构建数据湖/湖仓。他对数据流架构充满热情，并且还是Packt和O'Reilly的技术审稿人。

### 更多相关主题

+   [天高任鸟飞：了解JetBlue如何使用Monte Carlo和Snowflake…](https://www.kdnuggets.com/2022/12/monte-carlo-jetblue-snowflake-build-trust-improve-model-accuracy.html)

+   [Snowflake数据仓库初学者指南](https://www.kdnuggets.com/2022/02/data-warehousing-snowflake-beginners.html)

+   [提升Snowflake生产力的6大工具](https://www.kdnuggets.com/2023/08/top-6-tools-improve-productivity-snowflake.html)

+   [机器学习项目的简单快速数据流](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)

+   [使用Kafka和Risingwave构建Formula 1流数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)

+   [流式 LLM 介绍：适用于无限长度输入的 LLM](https://www.kdnuggets.com/introduction-to-streaming-llm-llms-for-infinite-length-inputs)
