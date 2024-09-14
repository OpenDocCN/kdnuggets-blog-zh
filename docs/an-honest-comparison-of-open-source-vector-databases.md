# 开源向量数据库的诚实比较

> 原文：[`www.kdnuggets.com/an-honest-comparison-of-open-source-vector-databases`](https://www.kdnuggets.com/an-honest-comparison-of-open-source-vector-databases)

![开源向量数据库的诚实比较](img/acbd5b0114632cb43344e5c059872e6a.png)

图像来源于 DALL-E 3

向量数据库提供了广泛的好处，特别是在生成式人工智能（AI）领域，更具体地说，在大规模语言模型（LLMs）中。这些好处可以从先进的索引到准确的相似性搜索，帮助实现强大的最先进项目，

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在这篇文章中，我们将对三种开源向量数据库进行诚实的比较，这些数据库建立了令人印象深刻的声誉——Chroma、Milvus 和 Weaviate。我们将探讨它们的使用案例、主要功能、性能指标、支持的编程语言等，以提供对每个数据库的全面且公正的概述。

# 什么是向量数据库？

从最简单的定义来看，向量数据库将信息存储为向量（向量嵌入），这是数据对象的数值版本。

因此，向量嵌入是一种强大的方法，用于在非常大且非结构化或半结构化的数据集中进行索引和搜索。这些数据集可以包括文本、图像或传感器数据，向量数据库将这些信息整理成可管理的格式。

向量数据库通过高维向量进行工作，这些向量可以包含数百个不同的维度，每个维度与数据对象的特定属性相关联。因此，创建了无与伦比的复杂性。

不要与向量索引或向量搜索库混淆，向量数据库是一个完整的管理解决方案，用于以以下方式存储和过滤元数据：

+   完全可扩展

+   可以轻松备份

+   允许动态数据更改

+   提供高水平的安全性

## 使用开源向量数据库的好处

开源向量数据库相对于授权的替代方案提供了许多好处，例如：

+   他们是一个**灵活的解决方案**，可以很容易地修改以适应特定需求，而授权选项通常是为特定项目设计的。

+   开源向量数据库**得到大量开发者社区的支持**，这些社区随时准备协助解决问题或提供改进项目的建议。

+   开源解决方案具有**无许可费用、订阅费用或任何意外费用**的优点，在项目实施过程中非常经济实惠。

+   由于开源向量数据库的透明特性，**开发者可以更有效地工作**，了解每个组件以及数据库的构建方式。

+   开源产品**随着技术变化不断改进和演变**，因为它们得到了活跃社区的支持。

# 开源向量数据库比较：Chroma 与 Milvus 与 Weaviate

现在我们已经了解了什么是向量数据库以及开源解决方案的好处，让我们考虑市场上一些最受欢迎的选项。我们将重点关注 Chroma、Milvus 和 Weaviate 的优点、特性和用例，然后进行直接的对比，以确定哪个选项最适合您的需求。

## 1\. Chroma

Chroma 旨在帮助各种规模的开发者和企业创建 LLM 应用程序，提供[构建复杂项目所需的所有资源](https://www.trychroma.com/blog/chroma_0.4.0)。Chroma 确保项目高度可扩展，并以最佳方式工作，以便能够快速存储、搜索和检索高维向量。

它因其作为极其灵活的解决方案而广受欢迎，提供了广泛的部署选项。此外，Chroma 可以直接在云端部署，也可以在现场运行，使其成为任何业务的可行选择，无论其 IT 基础设施如何。

### 用例

Chroma 还支持多种数据类型和格式，使其适用于几乎任何应用程序。然而，Chroma 的一个关键优势是其对音频数据的支持，使其成为音频搜索引擎、音乐推荐应用程序以及其他声音项目的首选。

## 2\. Milvus

Milvus 在机器学习和数据科学领域获得了很高的声誉，在向量索引和查询方面表现出色。利用强大的算法，Milvus 提供了闪电般的处理和数据检索速度[以及 GPU 支持](https://milvus.io/blog/unveiling-milvus-2-3-milestone-release-offering-support-for-gpu-arm64-cdc-and-other-features.md)，即使在处理非常大的数据集时也如此。Milvus 还可以与其他流行的框架如 PyTorch 和 TensorFlow 集成，使其能够加入到现有的机器学习工作流程中。

### 用例

Milvus 以其在相似度搜索和分析方面的能力而闻名，并且对多种编程语言有广泛的支持。这种灵活性意味着开发人员不仅限于后端操作，还可以在前端执行通常保留给服务器端语言的任务。例如，你可以[用 JavaScript 生成 PDF](http://apryse.com/blog/javascript/how-to-generate-pdfs-with-javascript)同时利用 Milvus 的实时数据。这为应用开发开辟了新途径，尤其是针对教育内容和无障碍应用程序。

这个开源向量数据库可以在广泛的行业和大量应用中使用。另一个显著的例子涉及电子商务，在这里，Milvus 可以驱动精确的推荐系统，以根据客户的偏好和购买习惯推荐产品。

它也适用于图像/视频分析项目，协助图像相似度搜索、物体识别和基于内容的图像检索。另一个关键用例是自然语言处理（NLP），提供文档聚类和语义搜索功能，同时为问答系统提供基础。

## 3\. Weaviate

我们的诚实比较中的第三个开源向量数据库是 Weaviate，它提供了[自托管和完全托管的解决方案](https://weaviate.io/blog/weaviate-1-21-release)。由于其出色的性能、简洁性和高度可扩展性，无数企业正在使用 Weaviate 来处理和管理大型数据集。

Weaviate 能够管理多种数据类型，非常灵活，可以存储向量和数据对象，这使得它非常适合需要多种搜索技术的应用（例如：向量搜索和关键词搜索）。

### 用例

就其用途而言，Weaviate 非常适合用于像企业资源计划软件中的数据分类项目或涉及以下内容的应用：

+   相似度搜索

+   语义搜索

+   图像搜索

+   电子商务产品搜索

+   推荐引擎

+   网络安全威胁分析与检测

+   异常检测

+   自动化数据协调

现在我们对每个向量数据库可以提供的功能有了初步了解，让我们考虑一下区分每个开源解决方案的更细节，在我们实用的比较表中。

## 比较表

|  | **Chroma** | **Milvus** | **Weaviate** |
| --- | --- | --- | --- |
| 开源状态 | 是 - Apache-2.0 许可证 | 是 - Apache-2.0 许可证 | 是 - BSD-3-Clause 许可证 |
| 发行日期 | 2023 年 2 月 | 2019 年 10 月 | 2021 年 1 月 |
| 使用案例 | 适用于广泛的应用，支持多种数据类型和格式。专注于基于音频的搜索项目和图像/视频检索。 | 适用于广泛的应用，支持大量数据类型和格式。非常适合电子商务推荐系统、自然语言处理和图像/视频分析 | 适用于广泛的应用，支持多种数据类型和格式。非常适合企业资源规划软件中的数据分类。 |
| 关键特点 | 使用非常便捷。开发、测试和生产环境都在 Jupyter Notebook 上使用相同的 API。强大的搜索、过滤和密度估计功能。 | 使用内存和持久存储相结合的方式，提供高速查询和插入性能。提供自动数据分区、负载均衡和故障容错，以处理大规模向量数据。支持多种向量相似性搜索算法。 | 提供基于 GraphQL 的 API，提供在与知识图谱互动时的灵活性和高效性。支持实时数据更新，以确保知识图谱保持最新。其模式推断功能自动化定义数据结构的过程。 |
| 支持的编程语言 | Python 或 JavaScript | Python、Java、C++ 和 Go | Python、JavaScript 和 Go |
| 社区和行业认可 | 拥有强大的社区，并提供一个 Discord 频道以回答实时问题。 | 在 GitHub、Slack、Reddit 和 Twitter 上有活跃的社区。超过 1000 个企业用户。文档详尽。 | 拥有专门的论坛和活跃的 Slack、Twitter 和 LinkedIn 社区。此外，还有定期的播客和新闻通讯。文档详尽。 |
| 性能指标 | 不适用 | [链接](https://milvus.io/docs/benchmark.md) | [链接](https://weaviate.io/developers/weaviate/benchmarks/ann) |
| GitHub Stars | 9k | 23.5k | 7.8k |

# 结论

我们诚实比较指南中的每个开源向量数据库都非常强大、可扩展且完全免费。这可能会使选择完美的解决方案变得有些困难，但通过了解您正在处理的确切项目和所需的支持水平，可以使这一过程变得更容易。

Chroma 是最新的解决方案，在社区支持方面不如其他两个数据库，但其易用性和灵活性使其成为一个很好的选择，尤其适合涉及音频搜索的项目。

Milvus 拥有最高的 GitHub Star 评分和强大的社区支持，有大量企业信任该向量数据库以满足他们的需求。因此，Milvus 是自然语言处理和图像/视频分析项目的不错选择。

最后，Weaviate 提供自托管和完全管理的解决方案，拥有广泛的文档和支持。一个关键的应用场景是企业资源规划软件中的数据分类，但这个解决方案适合多种项目。

[](http://nahlawrites.com/)****[娜赫拉·戴维斯](http://nahlawrites.com/)****是一位软件开发人员和技术写作者。在全职从事技术写作之前，她曾担任过—除了其他有趣的工作之外—一所 Inc. 5,000 体验品牌组织的首席程序员，该组织的客户包括三星、时代华纳、Netflix 和索尼。

### 这个话题的更多信息

+   [Python 向量数据库和向量索引：LLM 应用架构](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [Qdrant：具有管理云平台的开源向量搜索引擎](https://www.kdnuggets.com/2023/02/qdrant-open-source-vector-search-engine-managed-cloud-platform.html)

+   [全面指南：Pinecone 向量数据库](https://www.kdnuggets.com/a-comprehensive-guide-to-pinecone-vector-databases)

+   [AI 和 LLM 使用案例中的向量数据库](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)

+   [使用向量数据库的语义搜索](https://www.kdnuggets.com/semantic-search-with-vector-databases)

+   [什么是向量数据库以及它们为何对 LLMs 重要？](https://www.kdnuggets.com/2023/06/vector-databases-important-llms.html)
