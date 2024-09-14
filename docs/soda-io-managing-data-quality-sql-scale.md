# 如何使用 SQL 和规模管理数据质量

> 原文：[`www.kdnuggets.com/2021/05/soda-io-managing-data-quality-sql-scale.html`](https://www.kdnuggets.com/2021/05/soda-io-managing-data-quality-sql-scale.html)

**由 Tom Baeyens，Soda.io 首席技术官兼联合创始人**

### 为什么数据管理？

在过去三年里，我从软件工程师转变为数据工程师。当我与 Soda 的联合创始人 Maarten Masschelein 一起工作，解决隐性和未被检测的数据问题时，我无意中进入了数据管理领域。作为软件工程背景的我，编写单元测试和监控生产中的应用程序是理所当然的，但在数据方面却大相径庭。虽然大多数组织知道他们应该进行测试，但却没有策略，也不知道如何开始解决这个问题，这使得他们的系统暴露在风险中，并可能导致他们正在构建的数据产品出现严重的下游问题。

随着越来越多的产品以数据为核心输入，测试和监控使用的数据质量变得比以往任何时候都重要。因此，我们致力于构建一个数据可观测平台，使组织能够发现、优先处理和解决数据问题。

### 定义良好的数据质量

我们从 Soda SQL 开始，它于 2021 年 2 月发布。它是我们为数据密集型环境提供的第一个开源数据测试、监控和剖析工具。它与现有的数据工程工作流程协作，创建了一种快速简便的方法来定义对您的业务而言何为良好质量的数据。这使得数据工程师能够定义测试并防范在数据集、数据湖和数据仓库中未被检测出的隐性数据问题。

### 开源的救援

Soda SQL 是一个开源工具，具有简单的命令行界面（CLI）和 Python 库，通过指标收集来测试您的数据。它利用 YAML 配置文件作为输入，准备 SQL 查询，这些查询在数据库中的表上运行测试，以计算各种指标和测试。非常容易发现无效、缺失或意外的数据。因为 Soda SQL 利用--你猜对了-- SQL，所以数据可以保持原位，并且可以利用现有的计算引擎。

如果测试失败，Soda SQL 允许您停止管道，防止坏数据造成损害。随着指标的计算，诊断信息也会被捕获，以帮助分析如果检测到数据问题。然后可以采取步骤作为一个数据团队优先处理并协作解决问题。Soda SQL 可以单独手动使用，也可以与数据编排工具集成，以便根据扫描结果调度扫描和自动化操作。

您可以查看[5 分钟教程](https://docs.soda.io/soda-sql/getting-started/5_min_tutorial.html?utm_source=blog&utm_medium=blogpost&utm_campaign=KD%20Nuggets%20promo)了解如何开始，但这里有一个简单的示例：

1.  简单的指标和测试可以在扫描 YAML 配置文件中配置。以下是此类文件内容的示例：

![Soda Sql Fig1](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

1.  基于这些配置文件，Soda SQL 将在每次新数据到达时扫描你的数据，如下所示：

![Soda Sql Fig2](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

### 使每个人更接近数据

我们刚刚发布了 Soda Cloud，这是一款 web 应用程序，可以实时监控 Soda SQL 指标和测试结果。Soda Cloud 为工程师与数据团队中的其他人员之间创造了透明度。通过这种协作，数据团队可以提前解决隐性数据问题。Soda Cloud 扩展了 Soda SQL，两者无缝协作。

首先，Soda Cloud 通过一个指标数据库扩展了 Soda SQL，从而可以随时间可视化测量和测试结果。这使得能够监控随时间变化的情况和所有指标上的异常检测。

这些可视化和数据概况已经在更大的数据团队中创建了透明度。所有数据团队中的人员都可以看到实际存在的数据和执行的测试。

但 Soda Cloud 更进一步。它允许非技术人员通过简单的 UI 和 3 步向导构建和维护自己的监控器。这一点很重要，因为它消除了对领域知识的瓶颈。如果他们不需要涉及数据工程师来测试他们的领域逻辑，那么意味着更多的领域知识将用于定义什么是良好的数据。因此，将捕获更多的坏数据，防止各种损害。

Soda Cloud 通过提供一个中央平台来跟踪和评分数据在核心质量维度上的健康状况，从而解决了发现隐性数据问题的问题。

数据和分析工程师有了每次数据转换时测试数据的方法，以确保数据管道的可靠性。通过 Soda SQL，可以停止和隔离数据生产。Soda Cloud 可视化数据集的健康状况，并作为数据问题的沟通中心。

![Soda Sql Fig3](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

![Soda Sql Fig4](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

数据消费者和生产者现在可以轻松对齐关注点、预期和衡量标准，以确保数据保持适用性。我们还与电子邮件和 Slack 集成，确保在适当的时间通知相关人员，以便诊断、优先处理和解决数据问题。

![Soda Sql Fig5](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

![Soda Sql Fig6](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)

我们的使命是让每个人都更接近数据，因为我们相信数据质量是一项团队运动。每一个涉及数据的人（我们认为现在业务中的每个人都涉及数据）都需要理解数据、信任数据，并掌握数据。

我在 Soda 的主要责任是确保数据工程师喜欢使用我们的产品，并帮助他们迅速解决实际问题。我们通过结合云平台和一组开源开发者工具来解决问题，这些工具为数据团队提供了所需的可配置性，以创建端到端的可观察性。

高质量数据适用于每个人。访问 [GitHub 上的 Soda SQL](https://github.com/sodadata/soda-sql?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion) 和我们的免费试用版 Soda Starter，前往 [Soda.io](https://www.soda.io/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=soda%20sql%20promotion)（延长至 2021 年 6 月 30 日）。我们的 Slack 社区和文档包含最佳实践和有用的资源。

提前解决潜在的数据问题。祝好运！

* * *

## 我们的三大推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求。

* * *

### 更多相关话题

+   [数据质量维度：用伟大的期望保障数据质量](https://www.kdnuggets.com/2023/03/data-quality-dimensions-assuring-data-quality-great-expectations.html)

+   [作为数据科学家管理你的可重用 Python 代码](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

+   [管理数据科学项目的 4 个步骤](https://www.kdnuggets.com/2022/05/4-steps-managing-data-science-project.html)

+   [管理数据科学团队的 5 个技巧](https://www.kdnuggets.com/5-tips-for-managing-data-science-teams)

+   [管理深度学习数据集的新方法](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)

+   [在生产中通过 MLOps 管理模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)
