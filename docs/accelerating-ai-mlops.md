# 加速人工智能与 MLOps

> 原文：[`www.kdnuggets.com/2021/11/accelerating-ai-mlops.html`](https://www.kdnuggets.com/2021/11/accelerating-ai-mlops.html)

评论

**作者：[Yochay Ettun](https://www.linkedin.com/in/yochayettun/)，cnvrg.io 的首席执行官兼联合创始人**。

![](img/48afb2bff1fb2e3e30045c24f69e1f4c.png)

尽管人工智能（AI）拥有巨大的潜力，但它在大多数行业中仍未广泛应用。大多数 AI 项目陷入困境。埃森哲估计，[80% 到 85% 的公司](https://www.accenture.com/us-en/insights/artificial-intelligence/ai-investments) 的 AI 项目仍处于概念验证阶段，而近乎 [80% 的企业未能在整个组织中扩大 AI 部署](https://cnvrg.io/wp-content/uploads/2021/01/fundamentals-of-mlops.pdf)。

### 实现 AI 运营的障碍

一旦 AI 建成，许多组织在将模型投入生产时遇到困难。将 AI 转化为实际价值可能需要几个月甚至几年。根据 Gartner 的研究，[只有 53% 的项目能够从人工智能（AI）原型进入生产阶段。](https://www.gartner.com/en/newsroom/press-releases/2020-10-19-gartner-identifies-the-top-strategic-technology-trends-for-2021)

目前，开发人工智能/机器学习（ML）模型的标准仍然非常少。数据科学家是熟练的数学家，但为了成功将模型投入生产，他们还需要在软件开发方法论和 DevOps 方面的经验。模型通常是从头开始创建的，没有一致的软件开发过程、测试程序或衡量其性能的 KPI。

熟练的数据科学家是稀缺资源。公司更希望他们将时间花在擅长的领域——提供高影响力的算法上。然而，由于不可预见的操作问题，他们往往被其他任务所消耗，包括跟踪和监控、配置、计算资源管理、服务基础设施、特征提取和模型部署。

此外，今天的基础设施环境如同丛林。数据科学家可以利用无尽的计算选项组合来处理不同的 AI 工作负载，包括 CPU、GPU、AI 加速器、云计算、本地部署、混合云、共置等。因此，执行作业以在合理价格下实现高性能具有很大的复杂性和不可预见的挑战。

### 使用 MLOps 加速 AI 部署

这就是 MLOps（机器学习操作）可以发挥作用的地方，帮助数据科学家解决操作问题。MLOps 是一套用于数据科学家和运维专业人员之间协作与沟通的实践，以自动化大规模生产环境中机器学习和深度学习模型的部署。它使模型与业务需求对齐、实施标准且一致的开发流程、扩展和管理多样化的数据管道变得更容易。

大多数公司正在使用 MLOps 来自动化机器学习管道、监控、生命周期管理和治理。通过使用 MLOps，可以监控模型的性能下降或检测结果的漂移，表明模型需要使用更完整或更新的数据进行重新训练。MLOps 还用于不断实验新方法，以利用新技术，如最新的语言处理和文本与图像分析。

对 MLOps 解决方案的需求巨大。一个由社区主导的努力，收集了所有不同的工具，包括开源工具，结果列出了超过 330 种不同的 MLOps 工具。[Cognilytica](https://www.devopsonline.co.uk/mlops-and-automl-tools-to-increase-in-the-next-few-years/) 的报告预测，MLOps 工具将呈指数增长，到 2025 年市场规模将达到 1250 亿美元，这将代表 33%的年增长率。

有大量证据表明，实施 MLOps 的公司更有可能成功。以下是两个例子。

[ST Unitas](http://www.stunitas.com/front/main/en) 是一家领先的全球教育科技公司，同时也是《普林斯顿评论》的拥有者——美国顶尖的教育服务。该公司需要扩大其电子学习系统的部署，包括 CONECTS，这是一个处理数学问题图像并提供个性化测试的平台，旨在提高三百万学生的分数。实施 MLOps 平台后，ST Unitas 在短短几个月内将人工智能活动扩大了 10 倍，而无需招聘额外的工程师，提升了性能和处理量。

[Playtika](https://www.playtika.com/) 是一家领先的游戏娱乐公司，拥有超过 1000 万的日活跃用户（DAU），他们利用大量数据通过基于游戏内动作的用户体验（UX）定制来重塑游戏格局。Playtika 的大规模机器学习（ML）处理每分钟收集了数百万用户和数千个特征，使用批处理和网络服务，但这些解决方案都无法足够快速地处理数据以实时生成预测。通过实施 MLOps 平台，他们将性能提高了 40%，成功处理量增加了最高 50%。现在，Playtika 能够每天处理 9TB 的数据，为其科学家提供所需的数据，创建一个不断变化和自适应的游戏环境，继续推出收入最高的游戏。

在新的在线经济下，公司们竞相利用人工智能洞察力，真正实现数据驱动。但即便机器学习模型是基于可靠的算法构建，并且在完整的清洁数据集上成功训练，如果模型不能在现有的 IT 基础设施上运行，人工智能永远无法发挥其真正潜力。提前审查和寻找解决操作问题的方案可以帮助人工智能项目成功完成。

**简介：** [约查伊·埃顿](https://www.linkedin.com/in/yochayettun/)是一位经验丰富的技术领导者，他因在 AI 进步方面的成就以及建立 cnvrg.io 而入选 2020 年《福布斯》30 位 30 岁以下精英榜。约查伊从 7 岁开始编写代码。他在以色列国防军（IDF）情报单位服役了 4 年，并在耶路撒冷希伯来大学（HUJI）获得计算机科学学士学位，期间创立了 HUJI 创新实验室。约查伊曾担任 Webbing Labs 的首席技术官，并为 AI 和机器学习公司担任顾问。在 3 年的咨询工作之后，约查伊与联合创始人利亚·科尔本决定创建一个工具，帮助数据科学家和公司通过 cnvrg.io 扩展他们的 AI 和机器学习。该公司继续帮助财富 500 强公司的数据科学团队管理、构建和自动化从研究到生产的机器学习。

**相关内容：**

+   [机器学习模型开发与模型操作：原则与实践](https://www.kdnuggets.com/2021/10/machine-learning-model-development-operations-principles-practice.html)

+   [通过 ModelOps 扩展和管理 AI 计划](https://www.kdnuggets.com/2021/09/scale-govern-ai-modelops.html)

+   [MLOps 与 ModelOps：差异及其重要性](https://www.kdnuggets.com/2021/09/mlops-modelops-difference.html)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

### 更多相关话题

+   [开源工具在加速数据科学进步中的作用](https://www.kdnuggets.com/2023/05/role-open-source-tools-accelerating-data-science-progress.html)

+   [MLOps 中的机器学习设计模式](https://www.kdnuggets.com/2022/02/design-patterns-machine-learning-mlops.html)

+   [MLOps 一团糟，但这在意料之中](https://www.kdnuggets.com/2022/03/mlops-mess-expected.html)

+   [全面指南：MLOps](https://www.kdnuggets.com/2023/08/comprehensive-guide-mlops.html)

+   [Ploomber 与 Kubeflow：简化 MLOps](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)

+   [在纽约市 Rev 3 活动中与数据科学社区联系，第 1…](https://www.kdnuggets.com/2022/03/domino-connect-data-science-community-nyc-mlops-conference.html)
