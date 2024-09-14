# 探索 TensorFlow Quantum：Google 用于创建量子机器学习模型的新框架

> 原文：[https://www.kdnuggets.com/2020/03/tensorflow-quantum-framework-quantum-machine-learning-models.html](https://www.kdnuggets.com/2020/03/tensorflow-quantum-framework-quantum-machine-learning-models.html)

[评论](#comments)![图示](../Images/f1d32081abbc9d91c43b7cc70f1e59b2.png)

[来源](https://www.tensorflow.org/quantum)

量子计算与人工智能（AI）的交汇点有望成为整个技术历史中最令人着迷的进展之一。量子计算的出现可能迫使我们重新构想几乎所有现有的计算范式，而AI也不例外。然而，量子计算机的计算能力也有可能加速许多目前尚不实用的AI领域。AI和量子计算共同工作的第一步是重新构想机器学习模型以适应量子架构。最近，[Google 开源了 TensorFlow Quantum](https://www.tensorflow.org/quantum)，这是一个用于构建量子机器学习模型的框架。

TensorFlow Quantum 的核心理念是将量子算法和机器学习程序交错在 TensorFlow 编程模型中。Google 将这种方法称为量子机器学习，并通过利用一些最新的量子计算框架，如 [Google Cirq](https://github.com/quantumlib/Cirq)，来实现这一点。

### 量子机器学习

当涉及量子计算和 AI 时，我们需要回答的第一个问题是后者如何从量子架构的出现中受益。量子机器学习（QML）是一个广泛的术语，用来指代能够利用量子特性的机器学习模型。最初的 QML 应用集中在重构传统的机器学习模型，以便它们能够在随着量子比特数量呈指数增长的状态空间上快速执行线性代数。然而，量子硬件的进化拓展了 QML 的视野，使其发展到启发式方法，这些方法由于量子硬件的计算能力增加而可以通过实证研究。这一过程类似于 GPU 的创建如何使机器学习发展到深度学习范式。

![](../Images/6cac13a281d3cb539e9289c7737b876c.png)

在 TensorFlow Quantum 的背景下，QML 可以定义为两个主要组成部分：

1.  量子数据集

1.  混合量子模型

### 量子数据集

量子数据是发生在自然或人工量子系统中的任何数据源。这可以是来自量子力学实验的经典数据，或是由量子设备直接生成并作为输入喂入算法的数据。有证据表明，在“量子数据”上的混合量子-经典机器学习应用可能提供量子优势，超越仅使用经典机器学习的原因如下面所述。量子数据展示了[叠加](https://en.wikipedia.org/wiki/Quantum_superposition)和[纠缠](https://en.wikipedia.org/wiki/Quantum_entanglement)，导致联合概率分布，这可能需要指数级的经典计算资源来表示或存储。

### 混合量子模型

就像机器学习可以从训练数据集中推广模型一样，量子机器学习（QML）也能够从量子数据集中推广量子模型。然而，由于量子处理器仍然相对较小且噪声较多，量子模型不能仅凭量子处理器来推广量子数据。混合量子模型提出了一种方案，在这种方案中，量子计算机作为硬件加速器最为有用，与传统计算机协同工作。这种模型非常适合TensorFlow，因为它已经支持跨CPU、GPU和TPU的异构计算。

### Cirq

构建混合量子模型的第一步是能够利用量子操作。为此，TensorFlow Quantum依赖于Cirq，一个用于在近期设备上调用量子电路的开源框架。Cirq包含了指定量子计算所需的基本结构，如量子比特、门、电路和测量算子。Cirq的理念是提供一个简单的编程模型，抽象出量子应用的基本构建块。目前的版本包括以下关键构建块：

+   **电路：** 在Cirq中，电路表示量子电路的最基本形式。一个Cirq电路被表示为一系列包含操作的时刻，这些操作可以在某个抽象的时间滑动中在量子比特上执行。

+   **计划和设备：** 计划是另一种形式的量子电路，其中包含有关门的定时和持续时间的更多详细信息。从概念上讲，计划由一组计划操作以及描述计划将要运行的设备的内容组成。

+   **门：** 在Cirq中，门抽象了对量子比特集合的操作。

+   **模拟器：** Cirq包括一个Python模拟器，可用于运行电路和计划。模拟器架构可以跨多个线程和CPU扩展，这使得它能够运行相当复杂的电路。

### TensorFlow Quantum

TensorFlow Quantum(TFQ) 是一个用于构建 QML 应用程序的框架。TFQ 允许机器学习研究人员在单个计算图中构建量子数据集、量子模型和经典控制参数。

从架构的角度来看，TFQ 提供了一个抽象与 TensorFlow、Cirq 和计算硬件交互的模型。栈顶是待处理的数据。经典数据由 TensorFlow 原生处理；TFQ 增加了处理量子数据的能力，包括量子电路和量子算符。栈的下一级是 TensorFlow 中的 Keras API。由于 TFQ 的核心原则是与核心 TensorFlow（特别是 Keras 模型和优化器）的原生集成，因此这一层跨越了整个栈。Keras 模型抽象下方是我们的量子层和区分器，它们在与经典 TensorFlow 层连接时实现了混合量子-经典自动微分。在层和区分器下方，TFQ 依赖于 TensorFlow 操作，这些操作实例化数据流图。

![](../Images/3ad759fef549f843ee3b251e8a0535c6.png)

从执行的角度来看，TFQ 按照以下步骤来训练和构建 QML 模型。

1.  **准备量子数据集**：量子数据作为张量加载，指定为用 [Cirq](https://github.com/quantumlib/Cirq) 编写的量子电路。TensorFlow 在量子计算机上执行张量以生成量子数据集。

1.  **评估量子神经网络模型**：在此步骤中，研究人员可以使用 Cirq 原型化一个量子神经网络，然后将其嵌入到 TensorFlow 计算图中。

1.  **采样或平均**：此步骤利用了对几个运行的平均方法，涉及步骤（1）和（2）。

1.  **评估经典神经网络模型**：此步骤使用经典深度神经网络来提炼在前面步骤中提取的度量之间的相关性。

1.  **评估成本函数**：与传统机器学习模型类似，TFQ 使用此步骤来评估成本函数。这可以基于模型在分类任务中的准确性（如果量子数据已标记），或根据其他标准（如果任务是无监督的）。

1.  **评估梯度与更新参数** — 在评估成本函数后，应将管道中的自由参数更新为预期能减少成本的方向。

![](../Images/9df14933e5083265993f023fc65a7903.png)

TensorFlow 和 Cirq 的结合使 TFQ 具备了一套丰富的功能，包括更简单且熟悉的编程模型以及同时训练和执行多个量子电路的能力。

量子计算与机器学习之间的桥接工作仍处于非常初步的阶段。显然，TFQ 代表了这一领域最重要的里程碑之一，并利用了量子和机器学习领域的一些最佳知识产权。有关 TFQ 的更多详细信息，请访问 [该项目的网站](https://www.tensorflow.org/quantum)。

[原文](https://towardsdatascience.com/exploring-tensorflow-quantum-googles-new-framework-for-creating-quantum-machine-learning-models-3af27ba916e9)。经许可转载。

**相关：**

+   [关于谷歌自称的量子霸权及其对人工智能的影响](/2019/10/googles-self-proclaimed-quantum-supremacy-impact-artificial-intelligence.html)

+   [2020 年十大科技趋势](/2020/01/top-10-technology-trends-2020.html)

+   [谷歌开源 TFCO 以帮助构建公平的机器学习模型](/2020/03/google-open-sources-tfco-fair-machine-learning-models.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关话题

+   [探索 AI/DL 最新趋势：从元宇宙到量子计算](https://www.kdnuggets.com/2023/07/exploring-latest-trends-aidl-metaverse-quantum-computing.html)

+   [AI/ML 模型的风险管理框架](https://www.kdnuggets.com/2022/03/risk-management-framework-aiml-models.html)

+   [创建领域特定 AI 模型的最佳实践](https://www.kdnuggets.com/2022/07/best-practices-creating-domainspecific-ai-models.html)

+   [创建 AI 驱动的解决方案：理解大型语言模型](https://www.kdnuggets.com/creating-ai-driven-solutions-understanding-large-language-models)

+   [探索谷歌最新的 AI 工具：初学者指南](https://www.kdnuggets.com/exploring-googles-latest-ai-tools-a-beginners-guide)

+   [人工智能的未来：探索下一代生成模型](https://www.kdnuggets.com/2023/05/future-ai-exploring-next-generation-generative-models.html)
