# Segment Anything Model: 图像分割的基础模型

> 原文：[https://www.kdnuggets.com/2023/07/segment-anything-model-foundation-model-image-segmentation.html](https://www.kdnuggets.com/2023/07/segment-anything-model-foundation-model-image-segmentation.html)

分割，即识别属于对象的图像像素，是计算机视觉的核心。这一过程用于从科学成像到照片编辑的各种应用，技术专家必须具备高度的技能，并拥有大量标注数据的 AI 基础设施，以进行准确建模。

Meta AI 最近揭晓了其 [Segment Anything 项目](https://github.com/facebookresearch/segment-anything)，这是一个图像分割数据集和模型，包含 Segment Anything Model (SAM) 和 [SA-1B 掩模数据集](https://ai.facebook.com/datasets/segment-anything/)，这是迄今为止最大的数据集，旨在支持计算机视觉基础模型的进一步研究。他们为研究用途提供了 SA-1B，而 SAM 则在 Apache 2.0 开源许可证下授权，任何人都可以使用这个 [**演示**](https://segment-anything.com/demo) 来尝试 SAM 和你的图像！

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

![Segment Anything Model: 图像分割的基础模型](../Images/8ff2e4f2d8b63cf085c76b165d8ad402.png)

Segment Anything Model / 图像由 [**Meta AI**](https://segment-anything.com/) 提供

# 朝着泛化分割任务的方向前进

之前，分割问题通常采用两类方法：

+   交互式分割，用户通过迭代地优化掩模来指导分割任务。

+   自动分割允许选择性地分割如猫或椅子等物体类别，但这需要大量标注对象用于训练（例如，数千个甚至数万个分割猫的例子），以及计算资源和技术专长来训练分割模型。两种方法都没有提供一种通用的、完全自动的分割解决方案。

**SAM** 在一个模型中使用了交互式和自动化分割。提出的界面支持灵活使用，通过工程化的提示（如点击、框选或文本）使得各种分割任务成为可能。

SAM的开发使用了一个广泛的高质量数据集，其中包含超过十亿个掩码，这些掩码是该项目的一部分，赋予了它对训练期间未观察到的新类型物体和图像进行泛化的能力。因此，实践者无需再收集自己的分割数据或专门为其用例量身定制模型。

这些能力使得SAM能够在任务和领域之间进行泛化，这是其他图像分割软件之前从未做到的。

# SAM的功能和应用场景

SAM具有强大的功能，使得分割任务更加高效：

+   **各种输入提示**：引导分割的提示使用户能够轻松执行不同的分割任务，无需额外的训练要求。你可以使用交互式点和框进行分割，自动分割图像中的所有内容，并为模糊提示生成多个有效的掩码。在下图中，我们可以看到通过输入文本提示完成了某些物体的分割。

![Segment Anything Model: Foundation Model for Image Segmentation](../Images/7f04ca955056286284698c7ed2ae9cb9.png)

使用文本提示框的边界框。

+   **与其他系统的集成**：SAM可以接受来自其他系统的输入提示，例如未来可能会接受来自AR/VR头显的用户视线并选择物体。

+   **可扩展输出**：输出的掩码可以作为其他AI系统的输入。例如，物体掩码可以在视频中进行跟踪、启用图像编辑应用、提升到3D空间，甚至可以用于创意用途，例如合成。

+   **零样本泛化**：SAM已经发展出了对物体的理解，使其能够快速适应不熟悉的物体，而无需额外的训练。

+   **多重掩码生成**：当面对物体分割的不确定性时，SAM可以生成多个有效的掩码，为解决实际环境中的分割问题提供重要帮助。

+   **实时掩码生成**：SAM可以在预计算图像嵌入后，实时生成任何提示的分割掩码，实现与模型的实时交互。

# 理解SAM：它是如何工作的？

![Segment Anything Model: Foundation Model for Image Segmentation](../Images/a46efe7d097d397cc92e12351492b055.png)

SAM模型概述 / 图片来源于 [Segment Anything](https://arxiv.org/abs/2304.02643)

近年来，自然语言处理和计算机视觉领域的一项重要进展是基础模型，这些模型通过“提示”实现了对新数据集和任务的零样本和少样本学习。Meta AI研究人员训练SAM，以便为任何提示返回有效的分割掩码，例如前景/背景点、粗略的框/掩码、自由形式文本或指示图像中目标物体的任何信息。

有效的掩码意味着即使提示可能指代多个对象（例如：衬衫上的一个点可能代表它本身或穿着它的人），其输出应仅为一个对象提供合理的掩码？—？从而预训练模型并通过提示解决一般下游分割任务。

研究人员观察到，预训练任务和交互式数据收集对模型设计施加了特定的限制。最重要的是，实时模拟必须在 CPU 上高效运行，以允许标注者实时交互使用 SAM 进行高效标注。尽管运行时限制导致了质量和运行时限制之间的权衡，但简单设计在实际中产生了令人满意的结果。

在 SAM 的底层，一个图像编码器为图像生成一次性嵌入，而一个轻量级编码器将任何提示实时转换为嵌入向量。这些信息源随后由一个轻量级解码器结合，该解码器根据用 SAM 计算的图像嵌入预测分割掩码，因此 SAM 可以在 Web 浏览器中为任何给定提示在 50 毫秒内生成分段。

# 构建 SA-1B：分割 10 亿个掩码

构建和训练模型需要访问一个在训练开始时不存在的庞大而多样的数据池。今天的分割数据集发布是迄今为止最大的。标注者使用 SAM 交互式标注图像，然后用这些新数据更新 SAM？—？重复这一周期多次，以不断优化模型和数据集。

SAM 使得收集分割掩码比以往更快，每个掩码交互式标注只需 14 秒；这一过程比使用快速标注界面的边界框标注（仅需 7 秒）慢两倍。可比的大规模分割数据收集工作包括 COCO 完全手动多边形掩码标注，耗时约 10 小时；SAM 模型辅助的标注工作甚至更快；其每个掩码的标注时间比之前的模型辅助大规模数据标注快了 6.5 倍，而数据标注时间则慢了 2 倍！

仅通过交互式标注掩码不足以生成 SA-1B 数据集，因此开发了一个数据引擎。该数据引擎包含三个“齿轮”，首先是辅助标注者，然后是与辅助标注结合的完全自动化标注，以增加收集的掩码的多样性，最后是数据集扩展的完全自动化掩码创建。

根据人类评估研究，SA-1B最终数据集包含超过11亿个分割掩码，采集自1100万张有许可和隐私保护的图像，这些掩码的数量是现有任何分割数据集的4倍。经过这些人类评估验证，这些掩码与之前手动注释的数据集相比，展现了更高的质量和多样性。

SA-1B的图像来自多个国家的图像提供商，代表了不同的地理区域和收入水平。虽然某些地理区域仍然代表性不足，但SA-1B由于其图像数量更多和对所有区域的总体覆盖更好，因此提供了更大的代表性。

研究人员进行测试，旨在揭示模型在性别表现、肤色感知、年龄范围以及呈现人物的感知年龄方面的任何偏见，发现SAM模型在各个群体中表现相似。他们希望这能使最终的工作在实际应用中更加公平。

虽然SA-1B促进了研究成果，但它也可以使其他研究人员训练基础模型以进行图像分割。此外，这些数据可能成为具有附加注释的新数据集的基础。

# 未来的工作与总结

Meta AI 研究人员希望通过共享他们的研究和数据集，可以加速图像分割和图像与视频理解领域的研究。由于这个分割模型可以作为更大系统的一部分来执行这一功能。

在这篇文章中，我们介绍了什么是SAM及其能力和使用案例。随后，我们介绍了它是如何工作的，以及它是如何训练的，以便概述模型。最后，我们以未来愿景和工作总结了文章。如果你想了解更多关于SAM的信息，确保阅读[论文](https://arxiv.org/abs/2304.02643)并尝试[演示](https://segment-anything.com/demo)。

# 参考文献

+   [介绍 Segment Anything：朝着首个图像分割基础模型迈进](https://ai.facebook.com/blog/segment-anything-foundation-model-image-segmentation/)

+   [SA-1B 数据集](https://ai.facebook.com/datasets/segment-anything/)

+   [Segment Anything](https://arxiv.org/abs/2304.02643?fbclid=IwAR1Hl8xCPsDfrqqKSVfRwBzSOkKiBDXO2i0HhCvwHE6uvAq_9YtiOfvYsfs)

**[Youssef Rafaat](https://www.linkedin.com/in/youssef-hosni-b2960b135)** 是一位计算机视觉研究员和数据科学家。他的研究专注于为医疗保健应用开发实时计算机视觉算法。他还在营销、金融和医疗领域担任数据科学家超过3年。

### 更多相关话题

+   [学习如何使用 ChatGPT 学习 Python（或其他任何东西）](https://www.kdnuggets.com/2023/02/learn-python-chatgpt.html)

+   [什么是基础模型，它们是如何工作的？](https://www.kdnuggets.com/2023/05/foundation-models-work.html)

+   [使用聚类分析对数据进行分段](https://www.kdnuggets.com/using-cluster-analysis-to-segment-your-data)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [关于语义分割注释的误解](https://www.kdnuggets.com/2022/01/misconceptions-semantic-segmentation-annotation.html)

+   [Python 中的客户分段：一种实用方法](https://www.kdnuggets.com/customer-segmentation-in-python-a-practical-approach)
