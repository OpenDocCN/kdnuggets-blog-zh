# 边缘AI的承诺及有效采纳的方法

> 原文：[https://www.kdnuggets.com/the-promise-of-edge-ai-and-approaches-for-effective-adoption](https://www.kdnuggets.com/the-promise-of-edge-ai-and-approaches-for-effective-adoption)

![边缘AI的承诺及有效采纳的方法](../Images/366302b2c027adc5b084a96d267af716.png)

图片由编辑提供

当前的技术格局正经历着向边缘计算的关键转变，这一转变由生成AI（GenAI）和传统AI工作负载的快速进展推动。过去依赖于云计算的这些AI工作负载现在遇到了云端AI的局限性，包括数据安全、主权和网络连接问题。

为了绕过云端AI的这些限制，组织们正寻求拥抱边缘计算。边缘计算能够在数据产生和消费的地点实时分析和响应，这就是为什么组织认为它对AI创新和业务增长至关重要的原因。

边缘AI承诺以零到最小延迟实现更快的处理速度，可以显著改变新兴应用程序。虽然边缘设备的计算能力不断提高，但仍有一些限制可能使得实施高精度AI模型变得困难。模型量化、模仿学习、分布式推理和分布式数据管理等技术和方法可以帮助消除更高效、更具成本效益的边缘AI部署的障碍，从而使组织能够挖掘其真正潜力。

# “仅云端”AI方法无法满足下一代应用的需求

云端AI推理常常受到延迟问题的影响，导致设备与云环境之间的数据传输延迟。组织们已经意识到将数据跨区域移动到云端，然后再从云端回到边缘的成本。这可能会阻碍那些需要极快实时响应的应用程序，比如金融交易或工业安全系统。此外，当组织必须在网络连接不可靠的偏远地点运行AI驱动的应用程序时，云端并不总是可用。

“仅云端”AI战略的局限性变得越来越明显，特别是对于那些要求快速实时响应的下一代AI驱动应用。网络延迟等问题可能会减慢在云端应用程序中提供的洞察和推理，从而导致延迟和与云端和边缘环境之间的数据传输相关的成本增加。这对于实时应用程序，尤其是在网络连接间歇的偏远地区尤为严重。随着AI在决策和推理中的核心地位，数据移动的物理成本可能非常高，对业务结果产生负面影响。

[Gartner](https://www.gartner.com/en/newsroom/press-releases/2023-08-01-gartner-identifies-top-trends-shaping-future-of-data-science-and-machine-learning.html)预测，到2025年，超过55%的深度神经网络数据分析将在边缘系统的捕获点进行，而2021年这一比例不到10%。边缘计算有助于缓解延迟、可扩展性、数据安全性、连接性等挑战，重塑数据处理方式，并加速AI的采用。采用离线优先的方法开发应用将对敏捷应用的成功至关重要。

通过有效的边缘策略，组织可以从其应用程序中获得更多价值，并更快地做出业务决策。

# 通过不断演进的技术和方法使边缘AI成为可能

随着AI模型日益复杂，应用架构变得更加复杂，将这些模型部署到计算受限的边缘设备上变得更加明显。然而，技术的进步和不断演变的方法正在为在边缘计算框架内高效集成强大AI模型铺平道路，包括：

## 模型压缩与量化

模型剪枝和量化等技术对于减少AI模型的大小而不显著影响其准确性至关重要。模型剪枝从模型中删除冗余或非关键的信息，而量化则减少了模型参数中使用的数字的精度，使模型在资源受限的设备上运行更轻便、更快速。模型量化是一种压缩大型AI模型的技术，以提高可移植性并减少模型大小，使模型更轻便，更适合边缘部署。通过使用微调技术，包括通用后训练量化（GPTQ）、低秩适配（LoRA）和量化LoRA（QLoRA），模型量化降低了模型参数的数值精度，使模型在如平板、边缘网关和手机等边缘设备上更高效、更易获取。

## 边缘专用AI框架

专门为边缘计算设计的AI框架和库可以简化边缘AI工作负载的部署过程。这些框架经过优化，以适应边缘硬件的计算限制，并支持以最小性能开销高效执行模型。

## 带有分布式数据管理的数据库

具备向量搜索和实时分析等功能，帮助满足边缘的操作要求，并支持本地数据处理，处理各种数据类型，如音频、图像和传感器数据。这在像自动驾驶车辆软件这样的实时应用中尤其重要，其中各种数据类型不断被收集，并且必须实时分析。

## 分布式推理

将模型或工作负载分布在多个边缘设备上，并使用本地数据样本而无需实际数据交换，可以减轻潜在的合规性和数据隐私问题。对于涉及众多边缘设备和物联网设备的应用，例如智能城市和工业物联网，分布式推理是必须考虑的关键因素。

# AI 工作负载的平衡配置

尽管 AI 一直在云端处理，但与边缘的平衡将对加速 AI 计划至关重要。大多数，甚至所有行业，都已认识到 AI 和 GenAI 是一种竞争优势，这也是为什么在边缘收集、分析和迅速获得洞察将变得越来越重要。随着组织在 AI 使用上的演变，实施模型量化、多模态能力、数据平台及其他边缘策略将有助于推动实时、具有意义的商业成果。

**[](https://www.linkedin.com/in/pradhanrahul/)**[Rahul Pradhan](https://www.linkedin.com/in/pradhanrahul/)****是 Couchbase（NASDAQ: BASE）的产品与战略副总裁，Couchbase 提供一种领先的现代数据库，30% 的财富 100 强公司依赖于此。Rahul 拥有超过 20 年的经验，领导和管理工程及产品团队，专注于数据库、存储、网络和云安全技术。在加入 Couchbase 之前，他曾领导 Dell EMC 新兴技术和中型存储部门的产品管理与业务战略团队，将全闪存 NVMe、云和 SDS 产品推向市场。

### 更多主题

+   [云存储的采用是当前商业的需求](https://www.kdnuggets.com/2022/02/cloud-storage-adoption-need-hour-business.html)

+   [AI 在算法交易中的采用如何影响…](https://www.kdnuggets.com/2022/04/adoption-ai-algorithmic-trading-affected-finance-industry.html)

+   [如何通过 ML 模型可解释性加速 AI 采用之旅…](https://www.kdnuggets.com/2022/07/ml-model-explainability-accelerates-ai-adoption-journey-financial-services.html)

+   [机器学习的数据标注：市场概况、方法和工具](https://www.kdnuggets.com/2021/12/data-labeling-ml-overview-and-tools.html)

+   [机器学习的甜蜜点：NLP 和文档分析中的纯粹方法](https://www.kdnuggets.com/2022/05/machine-learning-sweet-spot-pure-approaches-nlp-document-analysis.html)

+   [多标签 NLP：类别不平衡和损失函数的分析…](https://www.kdnuggets.com/2023/03/multilabel-nlp-analysis-class-imbalance-loss-function-approaches.html)
