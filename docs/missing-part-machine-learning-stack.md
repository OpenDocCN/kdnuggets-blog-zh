# 机器学习栈中缺失的关键部分

> 原文：[`www.kdnuggets.com/2020/04/missing-part-machine-learning-stack.html`](https://www.kdnuggets.com/2020/04/missing-part-machine-learning-stack.html)

评论

**作者：[Malo Marrec](https://www.linkedin.com/in/malo-marrec/)，cloudnative.fr 独立从业者**。

![](img/eddab375a6fe2bcdb19f51b4eb2cea5d.png)

几年前，大多数机器学习（ML）和数据科学平台的计划集中在解决*计算编排*问题上。许多公司认为，管理和共享 GPU 是一个重大问题，Kubernetes 将成为基石，围绕它构建工具是正确的选择。因此，像 Rise ML、我共同创办的公司 Clusterone，或者像 Kubeflow 这样的开源项目应运而生。一些项目失败了，一些成功了，大多数仍处于早期阶段（[Kubeflow 最近宣布了 1.0 版本候选](https://medium.com/kubeflow/kubeflow-1-0-cloud-native-ml-for-everyone-a3950202751)）。

在我看来，那时大多数关注点都在于训练模型，但没有一个框架涵盖机器学习的所有环节。那些声称涵盖完整故事的框架专注于训练和部署。数据管理、特征提取、特征选择被主要平台忽视了，数据科学家和数据工程师之间的沟通鸿沟也是如此。大多数平台公司忽略了这一痛点，“我们不想重新构建 ETL”是他们的理由。

现在的情况发生了变化。机器学习团队意识到，他们在模型开发的第一步：特征定义和提取上浪费了大量时间。随着机器学习实验和管道开发受到软件工程最佳实践（版本化、标准化、隔离或容器化）的影响，特征工程需要变得更加结构化和可靠。代码管理中适用的严谨性在数据和特征管理中缺失，但这正在改变。

在过去的一年里，我们看到围绕一个新趋势（但其实是旧观念）的热潮：**特征存储**。

我们还看到像 [Logical Clocks](https://www.logicalclocks.com/) 和 [Kaskada](https://kaskada.com/) 这样的公司，以及开源项目如 [Feast](https://blog.gojekengineering.com/feast-bridging-ml-models-and-data-efd06b7d1644) 的重大公告和融资，逐渐获得关注。

### 为什么选择特征存储

让我们从一个例子开始。假设我在一家酒店预订平台公司的数据科学团队工作。我负责构建一个酒店容量预测系统。我有一个记录客户入住时间的数据表和一个记录客户退房时间的数据表。我想预测一个特定的酒店是否会有空闲房间。第一步是尝试定义有用的数据观察方式。在我们的例子中，通过比较每位客户的入住日期和退房日期，我们可以判断酒店的某个房间是否空闲。我可以在酒店层面汇总这些数据，并知道某一时间点有多少房间是可用的/空闲的。从这个*特性*中，我可以尝试预测在给定的容量下是否会有空闲房间。

为了达到这个目的，我已经探索了数据，编写了提取潜在有用特性的代码——包括查询、清理、过滤和汇总来自酒店预订数据库的数据的代码。只有在完成这些工作后，我才开始着手我的模型。

结果是，这个*特性*（酒店中的空闲房间数量）对公司内其他数据科学团队的许多人可能都很有用：比如预测高层财务结果的团队，构建帮助酒店预测清洁需求的产品的团队等。但这些团队每一个都在重新实现相同的特性计算管道，以便在他们的算法中使用相同的特性。

更糟糕的是，为了将模型投入生产，数据科学团队通常将模型交给数据工程团队，由他们将其调整为生产环境所需的状态（可扩展、运行更快等）。该团队通常会重写特性计算代码（例如，添加更高效的实时表格比较方式，以便在添加新预订时进行处理）。

到一天结束时，你可能会发现定义一个特性（比如酒店的空闲房间数量）的方法有很多种。

“作为数据科学家，你和我会以不同的方式看待事物，比如以不同的方式总结数据。这会产生很大的影响。每次我们都必须在进入生产环境时重新编译每个特性。” Swedbank 的首席数据科学家 Davit Bzhalava 说道。

这里有几个问题：

+   这**没有单一的真实来源**，这意味着模型在生产环境和训练环境中可能表现不同。

+   **大量的返工**是由于多个团队重写内容，无论是因为缺乏集中定义特性的方式，还是因为没有意识到特性已经在其他地方定义过。

+   团队最终**大量复制数据**，特别是在“一个有很多遗留系统的大组织中，你必须从很多地方查询数据。人们最终会复制大量数据来整合这些数据”，Davit Bzhalava 补充道。

这就是特性存储的作用所在。

特性存储是一个集中管理数据特性的地方，特性在这里被定义、整理、一致提供和共享。它提供了从数据源到模型的追溯性。

从架构上讲，Logical Clocks 的首席执行官 Dr. Jim Dowling 表示，“这是一个双数据库。”他说：“你有一个低延迟数据库，用于在在线应用中快速访问特征数据。然后，你有一个可扩展的数据库，可以存储大量的特征数据，用于训练更大更好的模型。”

此外，特征存储重新定义了团队的工作方式，位于数据科学工作流的核心，这可能就是它们市场出现花费了一段时间的原因：它们是复杂的产品。

### 获得势头

项目构建特征存储正在获得势头。一切始于 2017 年，当时 Uber 工程部门推出了其内部的[Michelangelo 平台](https://eng.uber.com/michelangelo-machine-learning-platform/)。从那时起，我们看到了一些开源项目获得了势头，例如[Feast](https://feast.dev/)，这是一个由 Gojek 维护的开源特征存储，Gojek 是一款南亚地区的按需预订应用。

与任何技术一样，一些公司更倾向于采用开源技术，这通常需要将多个组件集成到一个平台中。一些公司则倾向于选择商业产品。

首批向市场宣布其产品的两家公司是 Logical Clocks 和 Kaskada。Logical Clocks 在 2019 年 1 月发布了一个[开源](https://www.logicalclocks.com/news/introducing-the-feature-store-the-data-warehouse-for-machine-learning)特征存储及其商业支持版本。根据公司网站，Kaskada 目前处于[Beta](https://kaskada.com/)阶段。这两家公司的想法是提供一个端到端的、集成的平台，突出显示特征存储。

“我们在 2018 年底发布了第一个开源特征存储，现在我们在 AWS 云上发布了第一个托管特征存储。当我们在 2018 年开始构建特征存储时，我几乎觉得已经太晚了。2017 年底，Uber 曾写过关于特征存储的文章。但由于从头到尾都需要相当多的工程工作，直到现在，实施它们的人并不多。” Logical Clocks 的 Dr. Dowling 如是说。

我从总部位于西雅图的初创公司 Kaskada 那里听到了类似的观点，[该公司最近筹集了 800 万美元](https://www.google.com/search?q=kaskada+raises+8M&oq=kaskada+raises+8M&aqs=chrome..69i57j33.3481j0j7&sourceid=chrome&ie=UTF-8)。Kaskada 的首席执行官 Davor Bonaci 表示：“我们正在构建一个端到端的特征工程平台。我们不仅仅是一个特征存储。我们使你能够构建和可视化特征。然后，所有这些都可以生产并监控生产中的数据。我怀疑这将是最大的差异点，特别是与开源软件相比。”

### 采纳触发因素

我总是想知道是什么触发了平台软件的采用。确实，它会提供下一层次的能力，或者像 Davor Bonaci 所说的“使公司具备类似亚马逊、谷歌的数据基础设施”。但另一方面，它会要求流程和思维方式的改变。数据科学家将需要更多地考虑定义特征和计算问题的最佳实践。数据工程师将更早地参与到过程中，而不仅仅是接手模型。

似乎特征存储现在获得采用的原因是，经过几年的实验，公司现在开始在生产中运行模型。他们看到这样做的复杂性，并感到需要更稳健的工程（这让我很想起 DevOps：一旦你达到一定规模，你就会重新架构、自动化，并确保你不依赖于那个知道如何运行事情的团队成员）。

我询问了什么样的特征存储是好的，以及何时是采用的最佳时机。

Swedebank 的 Davit Bzhalava 表示，采用的触发因素是看到团队成员进出并感到需要将 DevOps 原则应用于 ML。“*你需要达到部署点才会意识到这是一个问题。然后你有生产中的模型，人员进出，你意识到需要让事情更稳健。我们经历了这一点，然后运行了特征存储的 POC 来解决这个问题。我们现在正在考虑如何在组织层面推广它*。” 我在听这个特征存储真实案例的集合时，也听到了类似的想法。

Kaskada 公司的首席执行官 Davor Bonaci 同意这个观点。“如果我们回顾几年前的市场，只有少数几家公司拥有有意义的 ML 用例。今天，我们看到公司正在过渡到生产中的实际工作负载。他们开始意识到，通过松散的特征定义实践，他们浪费了多少时间。” 他还给出了选择特征存储的看法。“有些特征存储是实际的引擎，存储公式并按需计算。有些只是数据库。那么，它是数据库，还是有表达和计算特征的方法的存储？”

逻辑时钟公司的首席执行官 Dr. Dowling 总结道：“特征存储可以通过整合公司范围内的特征工程工作，减少开发成本和市场推出时间，并改善数据工程和数据科学团队之间的协作。尽管特征存储的能力可能有所不同——我们的特征存储版本特征和特征数据，但总体来说，声称特征存储将通过将单一的端到端 ML 管道分解为更可管理和可测试的特征工程和模型训练管道来帮助改善 ML 模型工程，这并不不合理。”

近年来，许多公司将机器学习从实验阶段发展到生产阶段。这催生了对更强大、高效的模型开发方法的需求，特别是在被大多数人忽视的特征计算和管理领域。特征库的兴起引发了新一波的机器学习平台。虽然采用仍处于初期阶段，但我们可能会在未来几年看到大玩家的崛起和重大举措。

**相关内容：**

+   [高级特征工程和预处理的 4 个技巧](https://www.kdnuggets.com/2019/08/4-tips-advanced-feature-engineering-preprocessing.html)

+   [特征提取指南](https://www.kdnuggets.com/2019/06/hitchhikers-guide-feature-extraction.html)

+   [特征工程快速指南](https://www.kdnuggets.com/2019/02/quick-guide-feature-engineering.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [如何在预算内建立你的数据科学堆栈](https://www.kdnuggets.com/2022/01/data-science-stack-budget.html)

+   [弹性的 ML 堆栈是模块化的](https://www.kdnuggets.com/2022/06/comet-resilient-ml-stack-modular.html)

+   [免费全栈 LLM 实训营](https://www.kdnuggets.com/2023/06/free-full-stack-llm-bootcamp.html)

+   [本周 AI 动态，8 月 7 日：生成性 AI 进军 Jupyter 和堆栈…](https://www.kdnuggets.com/2023/mm/this-week-ai-2023-08-07.html)

+   [全栈一切？数据科学、开发技术的组织交集](https://www.kdnuggets.com/2022/08/full-stack-everything-organizational-intersections-data-science-dev-tech.html)

+   [6 个原因说明通用语义层对数据堆栈的好处](https://www.kdnuggets.com/2024/01/cube-6-reasons-why-a-universal-semantic-layer-is-beneficial)
