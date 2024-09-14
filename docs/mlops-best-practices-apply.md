# MLOps：最佳实践及如何应用

> 原文：[https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

![MLOps：最佳实践及如何应用](../Images/099be6ac2e4a145aabcf52077afbbf4a.png)

编辑封面

你可能已经对机器学习及其在当今世界的应用非常熟悉。人工智能（AI）和机器学习（ML）促进了智能软件的开发，这些软件能够准确预测结果并自动化通常由人类执行的各种工作。将机器学习纳入应用程序至关重要，但确保其平稳运行对组织而言更为关键。

公司利用一套被称为“[机器学习操作](/2021/10/machine-learning-model-development-operations-principles-practice.html)”或MLOps的最佳实践来实现这一目的。MLOps对任何企业的未来繁荣至关重要。

根据德勤的说法，市场可能在[2025年扩大$40亿](https://www2.deloitte.com/us/en/insights/focus/tech-trends/2021/mlops-industrialized-ai.html)，这意味着自2019年以来增长了近12倍。

尽管机器学习为各种业务流程带来了众多优势，但公司在实施ML方法以提高生产力方面仍面临困难。

# 最佳MLOps实践及如何应用

你不能仅仅注册一个新的SaaS供应商或创建新的云计算实例，然后期望MLOps能正常运作。这需要细致的准备和团队及部门之间的统一方法，无论你是[启动DAO](https://www.doola.com/blog/how-to-start-a-dao)还是注册了LLC。以下是成功实施MLOps的一些最佳实践。

## 跨多个市场细分验证模型

模型可以被重复使用，但软件不能。模型的有效性会随着时间的推移而减少，需要重新训练过程。每种新情况都需要调整模型。你需要一个训练管道来完成这一任务。

虽然实验监控可以帮助我们管理模型版本和可重复性，但在使用模型之前验证它们也是至关重要的。

离线或在线验证是企业可以根据优先级选择的选项。使用测试数据集评估模型是否适合实现业务目标，重点关注精度、准确性等指标。在做出推广决策之前，应该将这些指标与当前的生产/基准模型进行比较。

如果你的[实验得到良好跟踪](/2020/06/machine-learning-experiment-tracking.html)并在元数据方面得到管理，那么进行推广或回滚是可能的。在这篇文章中，我们将探讨如何使用A/B测试验证在线模型，以查看其在真实数据上的表现。

机器学习系统越来越意识到它们可能从数据中获取的偏差。例如，Twitter 的图像裁剪工具对某些用户无效。可以通过将模型的表现与不同用户群体进行比较来发现和修复这种不准确性。还应该在各种数据集上测试模型的表现，以确认它符合要求。

## 尝试新事物，并跟踪结果

超参数搜索和[特征工程](https://www.heavy.ai/technical-glossary/feature-engineering)是不断发展的领域。ML 团队的目标是根据当前的技术状态和数据中变化的模式，生产出最佳的系统。

然而，这需要跟上最新的趋势和标准。同时，测试这些概念，以确定它们是否可以帮助你的机器学习 (ML) 系统表现得更好。

数据、代码和超参数都可以用于实验。每种可能的变量组合都会产生可与其他实验结果比较的指标。调查发生的环境也可能会改变结果。

你可能还想部署[时间追踪软件](https://www.freshbooks.com/timesheets-and-time-tracking)来确保结果的及时性，并跟踪每个项目所花费的时间。

## 了解你的 MLOps 的成熟度

像 Microsoft 和 Google 等领先的云服务提供商使用 MLOps 采用的成熟度模型。

实施 MLOps 需要组织变革和新的工作实践。这会随着组织的系统和程序逐渐发展而发生。

成功的 MLOps 实施要求对组织的 MLOps 成熟度进展进行诚实的评估。在进行有效的成熟度评估后，公司可以学习如何提高到更高的成熟度级别。部署过程的变化，如[实施 DevOps](/2020/08/data-science-meets-devops-mlops-jupyter-git-kubernetes.html)或引入新团队成员，都是其中的一部分。

有多种方法可以存储机器学习的数据，如特征存储。特征存储对数据基础设施相对成熟的组织非常有帮助。他们需要确保不同的数据团队使用相同的特征，并减少重复劳动。如果组织只有少数几个数据科学家或分析师，特征存储可能不值得投入。

组织可以通过利用 MLOps 成熟度模型，让其技术、流程和团队一起成熟。这确保了在实施之前可以进行迭代和测试工具的可能性。

## 进行成本效益分析

确保你了解MLOps能为你的组织做什么。如果你按照策略进行另一次购买，你可以高效地处理每一个交易。假设你是一个寻找最佳汽车的买家。当然，你会有广泛的选择——例如，跑车、SUV、紧凑型车、豪华轿车等等。你必须首先选择最符合你需求的类别，以进行成本效益购买，然后根据你的预算分析不同的型号和类别。

选择最适合你公司的MLOps技术时也适用相同的原则。例如，运动型车辆和SUV有不同的优缺点。同样，你可以分析几种MLOps工具的优缺点。

要做出一个[明智的战略决策](/2022/04/informs-driving-better-business-decisions.html)，你必须考虑多个变量，包括公司预算和目标、你计划进行的MLOps活动、你打算处理的数据集的来源和格式以及你团队的能力。

## 保持开放的沟通渠道

产品经理和用户体验设计师可以影响支持你系统的产品如何与客户互动。机器学习工程师、DevOps工程师、数据科学家、数据可视化专家和软件开发人员共同合作，以实施和管理长期的机器学习系统。

员工绩效由经理和企业主进行评估和认可，而合规专业人员则验证活动是否符合公司的政策和监管标准。

如果机器学习系统在面对不断变化的用户和数据模式及期望时继续[实现商业目标](https://online.hbs.edu/blog/post/why-is-strategic-planning-important)，它们需要进行沟通。

## 将自动化融入你的工作流程

公司的MLOps成熟度可能通过广泛和先进的自动化得到提升。在缺乏MLOps的环境中，许多机器学习任务必须手动完成。这包括特征工程、数据清洗和转换、将训练和测试数据分成较小的块、构建模型训练代码等。

数据科学家通过手动执行这些程序，给错误留下空间，并浪费本可以用于探索的时间。

持续再培训是自动化应用的一个完美示例，其中数据分析师可以建立验证、[数据摄取](https://www.striim.com/blog/what-is-data-ingestion-and-why-this-technology-matters/)、特征工程、实验和模型测试的管道。持续再培训可以防止模型漂移，并通常被视为自动化机器学习的早期步骤。

# 结论

总的来说，机器学习很复杂，但如果你运用MLOps来改善参与开发和实施的团队之间的沟通，还是可以实现的。它不仅详细高效，而且还可能节省公司建立新机器学习系统的时间和成本。

**[Nahla Davies](http://nahlawrites.com/)** 是一位软件开发者和技术作家。在全职从事技术写作之前，她曾在一家Inc. 5,000的体验品牌组织担任首席程序员，服务的客户包括三星、时代华纳、Netflix和索尼等。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

### 更多相关主题

+   [你应了解的MLOps最佳实践](https://www.kdnuggets.com/2023/04/mlops-best-practices-know.html)

+   [数据仓储和ETL最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [11个AWS云及数据迁移的最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [将ChatGPT融入数据科学工作流程：技巧与最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [5个最佳端到端开源MLOps工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

+   [创建领域特定AI模型的最佳实践](https://www.kdnuggets.com/2022/07/best-practices-creating-domainspecific-ai-models.html)
