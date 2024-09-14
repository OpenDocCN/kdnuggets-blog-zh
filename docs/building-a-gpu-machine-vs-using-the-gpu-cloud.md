# 构建 GPU 机器与使用 GPU 云

> 原文：[https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)

![构建 GPU 机器与使用 GPU 云](../Images/387f75aa5996018e4a05b6596babc6fc.png)

编辑图片

图形处理单元（GPU）的出现以及它们解锁的指数级计算能力，对初创公司和企业业务而言都是一个划时代的时刻。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

GPUs 提供了强大的计算能力，能够执行涉及 AI、[机器学习](/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html) 和 3D 渲染等技术的复杂任务。

然而，在利用这种大量计算能力时，科技界在理想解决方案上面临十字路口。你应该构建一个专用的 GPU 机器还是利用 GPU 云？

本文深入探讨了这一争论的核心，剖析了每种选择的成本影响、性能指标和可扩展性因素。

# 什么是 GPU？

GPU（图形处理单元）是设计用来快速渲染图形和图像的计算机芯片，通过几乎瞬时完成数学计算。历史上，GPU 常与个人游戏电脑相关联，但它们也用于专业计算，技术进步需要额外的计算能力。

GPU 最初的开发是为了减少现代图形密集型应用程序对 CPU 的工作负载，通过并行处理渲染 2D 和 3D 图形，这种方法涉及多个处理器处理单个任务的不同部分。

在商业中，这种方法有效地加速了工作负载，并提供足够的处理能力，以支持人工智能（AI）和机器学习（ML）建模等项目。

## GPU 使用案例

近年来，GPU 发展迅速，变得比早期的 GPU 更加可编程，使其能够在广泛的应用场景中使用，例如：

+   使用 Blender 和 ZBrush 等软件，快速渲染实时的 2D 和 3D 图形应用程序

+   视频编辑和视频内容创建，特别是 4k、8k 或高帧率的作品

+   提供图形能力以在现代显示器上显示视频游戏，包括 4k。

+   加速机器学习模型，从基本的[图像转换为 jpg](https://xodo.com/heic-to-jpg)到用全面的前端[几分钟内部署自定义调整的模型](https://ubiops.com/deploy-llama-2-with-a-customizable-front-end-in-under-15-minutes-using-only-ubiops-python-and-streamlit/)

+   共享 CPU 工作负载，以在各种应用中提供更高的性能

+   提供训练深度神经网络所需的计算资源

+   挖掘比特币和以太坊等加密货币

重点在于神经网络的开发，每个网络由多个节点组成，每个节点在更广泛的分析模型中执行计算。

GPU 可以通过更高的并行处理增强这些模型在深度学习网络中的性能，从而创建具有更高容错性的模型。因此，现在市场上有许多专门为深度学习项目打造的 GPU，[例如最近发布的 H200](https://www.cnbc.com/2023/11/13/nvidia-unveils-h100-its-newest-high-end-chip-for-training-ai-models.html)。

# 构建 GPU 机器

许多企业，特别是初创公司，选择自己构建 GPU 机器，因为它们具有成本效益，同时提供与[GPU 云解决方案](/2021/05/super-charge-python-pandas-gpus-saturn-cloud.html)相同的性能。然而，这并不是说这样的项目没有挑战。

在这一部分，我们将讨论构建 GPU 机器的利弊，包括预期的成本以及机器管理，这可能会影响安全性和可扩展性等因素。

## 为什么要自己构建 GPU 机器？

构建本地 GPU 机器的关键好处是成本，但这样的项目通常需要显著的内部专业知识。持续维护和未来的修改也是可能使这种解决方案不可行的考虑因素。但是，如果这样的构建在你们团队的能力范围内，或者你们找到了可以为你们交付项目的第三方供应商，财务上的节省可能是显著的。

建议为深度学习项目构建一个可扩展的 GPU 机器，特别是在考虑到云 GPU 服务的租赁成本时，如[Amazon Web Services EC2](https://aws.amazon.com/ec2/)、[Google Cloud](https://cloud.google.com/) 或 [Microsoft Azure](https://azure.microsoft.com)。尽管对于希望尽快启动项目的组织来说，托管服务可能更为理想。

让我们考虑构建一个本地自建 GPU 机器的两个主要好处：成本和性能。

### 成本

如果一个组织正在为人工智能和机器学习项目开发一个大型数据集的深度神经网络，那么运营成本有时可能飙升。这可能阻碍开发者在模型训练期间交付预期结果，并限制项目的可扩展性。因此，财务影响可能导致产品缩减，甚至是一个不适合目的的模型。

建设一个现场自管理的GPU机器可以大幅降低成本，为开发者和数据工程师提供他们在广泛迭代、测试和实验中所需的资源。

然而，这只是触及了本地构建和运行的GPU机器的表面，尤其是对于开源LLM而言，[这些LLM越来越受欢迎](https://deepgram.com/learn/llama-2-paper-explained)。随着实际用户界面的出现，你可能很快会看到你友好的邻居牙医在后台[运行几台4090](https://lambdalabs.com/blog/nvidia-rtx-4090-vs-rtx-3090-deep-learning-benchmark)，用于[如保险验证](https://www.getweave.com/weave-insurance-verification/)、日程安排、数据交叉引用等任务。

### 性能

大规模深度学习和机器学习训练模型/算法需要大量资源，这意味着它们需要极高性能的处理能力。对于需要渲染高质量视频的组织来说也是如此，员工可能需要[多个基于GPU的系统](/2021/09/speeding-neural-network-training-multiple-gpus-dask.html)或最先进的GPU服务器。

自建的GPU系统推荐用于生产规模的数据模型及其训练，一些GPU能够提供双精度，这一特性[使用64位表示数字](https://blogs.nvidia.com/blog/2020/05/14/double-precision-tensor-cores/)，提供更大的数值范围和更好的小数精度。然而，这种功能仅在依赖非常高精度的模型中才是必需的。推荐的双精度系统选项是Nvidia的本地Titan系列GPU服务器。

### 操作

许多组织缺乏管理本地GPU机器和服务器的专业知识和能力。这是因为内部IT团队需要能够配置基于GPU的基础设施以实现最高性能的专家。

此外，缺乏专业知识可能导致安全性不足，从而出现被网络犯罪分子利用的漏洞。未来可能需要扩展系统也可能带来挑战。

# 使用GPU云

本地GPU机器在性能和成本效益方面提供了明显的优势，但前提是组织拥有必要的内部专家。这就是为什么许多组织选择使用GPU云服务，如Saturn Cloud，它提供了完全托管的服务以增加简便性和安心。

云GPU解决方案使深度学习项目对更广泛的组织和行业变得更加可及，许多系统能够匹配自建GPU机器的性能水平。GPU云解决方案的出现是人们[越来越多地投资于AI开发](https://bluetree.ai/best-ai-app-development-companies/)的主要原因之一，尤其是[像Mistral这样的开源模型](https://www.businessinsider.com/mistral-in-talks-to-raise-funding-at-2-billion-valuation-2023-11)，其开源特性非常适合“可租用的vRAM”，并在无需依赖大型供应商（如OpenAI或Anthropic）的情况下运行LLM。

## 成本

根据组织的需求或正在训练的模型，[云GPU解决方案](/2018/11/deep-learning-cloud-providers-cpu-gpu-tpu.html)可能会更便宜，只要每周所需的小时数是合理的。对于较小、数据量较少的项目，可能不需要投资昂贵的H100对，GPU云解决方案提供按合同基础以及各种月度计划的形式，从爱好者到企业都能满足需求。

## 性能

有一系列的CPU云选项能够匹配DIY GPU机器的性能水平，提供最佳平衡的处理器、准确的内存、高性能磁盘和每实例八个GPU来处理各自的工作负载。当然，这些解决方案可能会有一定费用，但组织可以安排按小时计费，以确保只为使用的部分付费。

## 操作

云GPU相对于自建GPU的关键优势在于其操作，专家团队随时准备协助解决任何问题并提供技术支持。自建GPU机器或服务器需要内部管理，或者需要第三方公司远程管理，这会产生额外费用。

使用GPU云服务，任何如网络故障、软件更新、电力中断、设备故障或磁盘空间不足的问题都能快速解决。实际上，借助完全托管的解决方案，这些问题几乎不会发生，因为GPU服务器会被最佳配置，以避免任何过载和系统故障。这意味着IT团队可以专注于业务的核心需求。

# 结论

在选择自建GPU机器还是使用GPU云时，取决于具体的使用案例，大型数据密集型项目需要额外的性能而不产生显著的成本。在这种情况下，自建系统可能提供所需的性能而不会有高额的月度费用。

另外，对于缺乏内部专业知识或不需要顶级性能的组织，托管的云GPU解决方案可能更为合适，由提供商负责机器的管理和维护。

[Nahla Davies](http://nahlawrites.com/)****是一名软件开发者和技术写作人。在将全部精力投入技术写作之前，她曾管理过—在许多其他有趣的事情中—担任一家 Inc. 5000 实验品牌组织的首席程序员，该组织的客户包括三星、时代华纳、Netflix 和索尼。

### 了解更多相关内容

+   [使用 RAPIDS cuDF 利用 GPU 进行特征工程](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [11 个最佳实践：云和数据迁移到 AWS 云](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [宣布博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [掌握 GPU：Python 中 GPU 加速数据框的初学者指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)

+   [生成式 AI 游乐场：使用 Camel-5b 和 Open LLaMA 3B 的 LLMs…](https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-llms-with-camel-5b-and-open-llama-3b)

+   [生成式 AI 游乐场：文本到图像的稳定扩散与…](https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-text-to-image-stable-diffusion)
