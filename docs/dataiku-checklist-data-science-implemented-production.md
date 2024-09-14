# 实现数据科学生产的检查清单

> 原文：[`www.kdnuggets.com/2017/06/dataiku-checklist-data-science-implemented-production.html`](https://www.kdnuggets.com/2017/06/dataiku-checklist-data-science-implemented-production.html)

**由 Dataiku 提供。**赞助文章。

构建数据科学项目和训练模型只是第一步。将模型投入生产环境是公司经常失败的地方。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

实际上，将模型实施到现有的数据科学和 IT 堆栈中对许多公司来说非常复杂。原因？设计环境和实际生产环境之间的工具和技术的不一致。

一年多来，我们[调查了成千上万家公司](https://pages.dataiku.com/how-do-companies-build-production-ready-data-analytics-projects)涵盖各种行业和数据科学发展，了解他们如何克服这些困难，并分析了结果。以下是你在设计到生产管道中需要牢记的关键事项。

![Dataiku](https://pages.dataiku.com/hubfs/PDF/Infographics/keys-data-science-production.pdf?t=1496154983353)

[点击查看高质量信息图](https://pages.dataiku.com/hubfs/PDF/Infographics/keys-data-science-production.pdf?t=1496154983353)。

![Dataiku](img/46669443ac9d47d6ec621d4ec92feab5.png)

### 1\. 你的打包策略

数据项目是复杂的。它包含大量不同格式的数据存储在不同位置，还有很多代码行和脚本，这些代码和脚本用不同的语言将原始数据转换为预测。如果你不支持在生产过程中对代码或数据进行适当打包，这将会很棘手，尤其是当你处理预测时。

典型的发布流程应包括：

1.  设立版本控制工具来管理代码版本。

1.  创建打包脚本，将代码和数据打包成 zip 文件。

1.  将其部署到生产环境中。

![Dataiku](img/b931bbd4b34af4465740526cf224a40c.png)

### 2\. 你的模型优化和再训练

小的迭代是长期准确预测的关键，因此建立一个重新训练、验证和部署模型的流程至关重要。实际上，模型需要不断发展，以适应新行为和基础数据的变化。

高效重新训练的关键是将其设置为数据科学生产工作流中的一个独立步骤。换句话说，一个自动命令每周重新训练一个预测模型候选者，对该模型进行评分和验证，并在经过人工操作员简单验证后进行更换。

![Dataiku](img/0d7f511009a16dfac934c6f8a0b48cee.png)

### 3\. 你的业务团队的参与

在我们的调查中，我们发现公司报告在生产部署中遇到许多困难与业务团队的有限参与之间存在强烈的相关性。

在项目开发过程中，这是至关重要的，以确保最终产品对业务用户是可理解和可用的。一旦数据产品投入生产，它仍然是业务用户评估模型性能的重要成功因素，因为他们的工作基于此。实现这一点有多种方式；最受欢迎的方法是设置实时仪表板以监控并深入了解模型性能。自动发送包含关键指标的电子邮件可能是一个更安全的选择，以确保业务团队随时掌握信息。

![Dataiku](img/b5c9acd5161589e7732cea1810d64bc9.png)

### 4\. 你的 IT 环境一致性策略

现代数据科学依赖于多种技术，如 Python、R、Scala、Spark 和 Hadoop，以及开源框架和库。当生产环境依赖于 JAVA、.NET 和 SQL 数据库等技术时，这可能会导致问题，因为可能需要对项目进行完全重编码。

工具的增多也会带来维护生产环境和设计环境的困难，需要保持当前版本和软件包（一个数据科学项目可能依赖于多达 100 个 R 包、40 个 Python 包和数百个 Java/Scala 包）。为了解决这个问题，两种流行的解决方案是维护一个通用包列表或为每个数据项目设置虚拟机环境。

![Dataiku](img/7e4d2e932aef82bc76b5445e7c3f8f59.png)

### 5\. 你的回滚策略

在有效的监控到位之后，下一个里程碑是建立回滚策略，以应对性能指标下降的情况。回滚策略基本上是一个保险计划，以防你的生产环境失败。

一个好的回滚策略必须包括数据项目的所有方面，包括数据、数据模式、转换代码和软件依赖关系。它还必须是用户（即使他们不是经过培训的数据工程师）可以访问的过程，以确保在失败时能快速反应。

![Dataiku](img/d98bc978e49e73f54c12204d5152b960.png)

### 6\. 你的项目和流程的可审计性

能够审计以了解每个输出版本对应哪个代码是至关重要的。追踪数据科学工作流对于追踪任何不当行为、证明没有非法数据使用或隐私侵犯、避免敏感数据泄漏，或展示数据流的质量和维护都非常重要。

控制版本的最常见方法是（毫不意外地）Git 或 SVN。然而，保持有关数据库系统的信息日志（包括表创建、修改和架构更改）也是一种最佳实践。

![Dataiku](img/4206027f8a53d9da67669e4f59870180.png)

### 7\. 无论你的用例是否需要实时评分或在线机器学习

实时评分和在线学习在许多用例中越来越受欢迎，包括评分欺诈预测或定价。许多进行评分的公司使用批处理和实时评分的组合，甚至仅使用实时评分。越来越多的公司报告使用在线机器学习。

这些技术在生产环境、回滚和故障转移策略、部署等方面带来了复杂性。然而，它们并不需要为这些技术设置独立的过程和堆栈，只需要监控调整。

![Dataiku](img/0ccbea74dc1fbaec700861ccd9cfd745.png)

### 8\. 你的系统和过程的可扩展性

随着数据科学系统在数据量和数据项目的增加中扩展，保持性能至关重要。这意味着建立一个足够弹性的系统，以应对显著的过渡，不仅是数据的纯量或请求数量，还有复杂性或团队可扩展性。

但可扩展性问题可能会意外出现，例如未清空的垃圾箱、大量日志文件或未使用的数据集。通过制定检查工作流效率或监控作业执行时间的策略来预防这些问题。

**如果你想要了解更多关于[优化设计到生产过程的最佳实践](https://pages.dataiku.com/how-do-companies-build-production-ready-data-analytics-projects)，可以查看我们的广泛生产调查结果。**

### 更多相关主题

+   [将机器学习模型部署到云中的生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [为生产优先排序数据科学模型](https://www.kdnuggets.com/2022/04/prioritizing-data-science-models-production.html)

+   [使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [将机器学习算法全面部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [将机器学习从 PoC 过渡到生产](https://www.kdnuggets.com/2022/05/operationalizing-machine-learning-poc-production.html)

+   [功能商店峰会 2023：在生产环境中部署 ML 模型的实用策略](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)
