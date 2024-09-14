# 将机器学习模型投入生产的 5 个最佳实践

> 原文：[`www.kdnuggets.com/2020/10/5-best-practices-machine-learning-models-production.html`](https://www.kdnuggets.com/2020/10/5-best-practices-machine-learning-models-production.html)

评论

**由 [Sigmoid Analytics](https://www.sigmoid.com/) 提供**

![生产化 ML 模型](img/af39e966c63a0e29580f52ee9aa584e6.png)

在我们之前的文章中 – [在扩展 ML 模型时需要准备的 5 大挑战](https://www.sigmoid.com/blogs/5-challenges-to-be-prepared-for-before-scaling-machine-learning-models/)，我们讨论了生产化可扩展机器学习（ML）模型的五大挑战。本文的重点是建立使 ML 项目成功的最佳实践。

当前的机器学习模型解决了各行各业中的各种具体业务挑战。选择机器学习模型的方法在很大程度上取决于我们试图解决的业务用例。但在进一步推进之前，我们应该确保选择的模型构建方法是可以投入生产的。

**Sigmoid 的前网络研讨会调查显示，43%的公司认为 ML 生产化和集成具有挑战性。**

由于复杂性，必须在生产过程中尽早消除正确的风险。在模型选择和开发的早期阶段消除更多风险可以减少在生产化阶段的返工。

机器学习生态系统中的各种考虑因素包括——数据集、技术栈、实施和集成这两者，以及部署 ML 模型的团队。然后是坚韧的测试框架，以确保业务结果的一致性。

通过使用以下最佳实践，Yum! Brands 能够通过生产化他们的 MAB 模型进行个性化电子邮件营销，实现了 8%的销售增长。**观看 2 分钟的视频，了解 Yum 的 Scott Kasper 解释最佳实践对生产化 MAB 模型的影响**

**1\. 数据评估**

首先，应检查数据的可行性——我们是否拥有足够的正确数据集来运行机器学习模型？我们是否能快速获得数据以进行预测？

例如，拥有数百万注册客户数据的餐饮连锁（QSRs）。这一庞大的数据量足以让任何 ML 模型在其上运行。

当上述数据风险得到缓解后，应该建立一个数据湖环境，以便轻松而强大地访问各种所需的数据源。数据湖（替代传统的数据仓库）可以节省团队大量的官僚和手动开销。

对数据集进行实验，以确保数据包含足够的信息以带来期望的业务变化，在此步骤中至关重要。此外，处理可用数据的可扩展计算环境是一个主要要求。

当数据科学家清理、整理和处理不同的数据集后，我们强烈建议对数据进行编目，以便未来利用。

最终，应建立强大且经过深思熟虑的治理和安全系统，以便组织中的不同团队可以自由共享数据。

**2\. 评估合适的技术栈**

一旦选择了机器学习（ML）模型，就应该手动运行它们以测试其有效性。例如，在个性化电子邮件营销的情况下——我们发送的促销邮件是否带来了新的转化，还是需要重新考虑我们的策略？

在成功完成手动测试后，需要选择合适的技术。数据科学团队应该能够从多种技术栈中选择，以便他们可以实验并挑选出使机器学习生产化更容易的技术。

选择的技术应根据稳定性、业务用例、未来场景和云准备情况进行基准测试。[Gartner](https://www.gartner.com/en/newsroom/press-releases/2019-11-13-gartner-forecasts-worldwide-public-cloud-revenue-to-grow-17-percent-in-2020) 统计数据表明，云 IaaS 预计将以 24%的年增长率增长，直至 2022 年。

**观看 1 分钟的视频，Mayur Rustagi（CTO & 联合创始人 – Sigmoid）讲述了选择基础设施组件的有效方法**

**3\. 强健的部署方法**

标准化部署过程，使得在不同阶段的测试和集成变得顺畅，是强烈推荐的。

数据工程师应专注于打磨代码库，集成模型（作为 API 端点或批量处理模型），并创建工作流自动化，以便团队可以轻松集成。

拥有访问正确数据集和模型的完整环境对任何机器学习模型的成功至关重要。

**4\. 部署后支持与测试**

选择合适的框架用于日志记录、监控和报告结果，将使本来困难的测试过程变得可控。

ML 环境应进行实时测试并密切监控。在复杂的实验系统中，测试结果应反馈给数据工程团队，以便他们更新模型。

例如，数据工程师可以决定在下一次迭代中加重表现出色的变体的权重，同时减少表现不佳的变体的权重。

还需注意负面或严重错误的结果。必须满足正确的服务水平协议（SLA）。应监控数据质量和模型性能。

生产环境因此会逐渐稳定。

**5\. 变更管理与沟通**

每个机器学习模型的成功在很大程度上依赖于各跨职能团队之间的清晰沟通，以便在正确的步骤中减轻风险。

数据工程和数据科学团队必须[*协同工作*](https://www.sigmoid.com/productionize-ml-models/)才能将机器学习模型投入生产。建议数据科学家完全掌控系统以检查代码和查看生产结果。团队甚至可能需要接受新环境的培训。

透明的沟通将节省大家的精力和时间。

**结论：**

除了上述所有最佳实践外，机器学习模型还应设计为可重复使用且能够应对变化和剧烈事件。最佳情况并不是所有推荐的方法都到位，而是让某些领域足够成熟和可扩展，以便根据时间和业务需求进行上调和下调。

如果你对将机器学习模型投入生产有任何进一步的问题，请通过电子邮件联系我们。有关“规模化生产机器学习模型”的完整网络研讨会录音，请点击 [这里](https://www.youtube.com/watch?v=B663q674vcc&t=2446s)。

[原文](https://www.sigmoid.com/blogs/5-best-practices-for-putting-ml-models-into-production/)。经许可转载。

**相关：**

+   扩展机器学习模型的 5 大挑战

+   机器学习模型部署

+   使用 Python 和 Heroku 创建并部署你的第一个 Flask 应用

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯的捷径。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求

* * *

### 更多相关主题

+   [将机器学习算法完全部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [将 ChatGPT 集成到数据科学工作流程中的技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [创建领域特定 AI 模型的最佳实践](https://www.kdnuggets.com/2022/07/best-practices-creating-domainspecific-ai-models.html)

+   [为生产优先考虑数据科学模型](https://www.kdnuggets.com/2022/04/prioritizing-data-science-models-production.html)

+   [Feature Store Summit 2023: 部署机器学习模型的实用策略…](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)

+   [将机器学习从概念验证到生产环境的实施](https://www.kdnuggets.com/2022/05/operationalizing-machine-learning-poc-production.html)
