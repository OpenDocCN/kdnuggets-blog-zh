# 如何在计算机视觉中做一切

> 原文：[https://www.kdnuggets.com/2019/02/everything-computer-vision.html](https://www.kdnuggets.com/2019/02/everything-computer-vision.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![图](../Images/8e51a1b5b1c74e6f15f1ee81b3c1b006.png)

Mask-RCNN进行对象检测和实例分割

想做计算机视觉吗？如今深度学习是实现这一目标的途径。大规模的数据集加上深度卷积神经网络（CNN）的表现能力，构建了超级准确和稳健的模型。唯一仍然存在的挑战是：如何设计你的模型。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

在计算机视觉这样一个广泛而复杂的领域中，解决方案并不总是明确的。计算机视觉中的许多标准任务都需要特别考虑：分类、检测、分割、姿态估计、增强与修复以及动作识别。虽然用于每种任务的最先进网络都展示了共同的模式，但它们仍然需要独特的设计调整。

那么我们如何为所有这些不同的任务构建模型呢？

让我向你展示如何用深度学习做计算机视觉中的一切！

### 分类

最著名的！图像分类网络以*固定大小的输入*开始。输入图像可以有任意数量的通道，但RGB图像通常为3。当你设计网络时，分辨率在技术上可以是任何大小，只要足够大以支持网络中所有的下采样。例如，如果你在网络中下采样4次，那么你的输入图像大小至少需要为4² = 16 x 16像素。

随着你深入网络，空间分辨率会降低，因为我们试图将所有信息压缩成一个一维向量表示。为了确保网络始终有能力传递它提取的所有信息，我们会根据深度增加特征图的数量，以适应空间分辨率的降低。即，我们在下采样过程中丢失了空间信息，为了弥补这种损失，我们扩展特征图以增加语义信息。

在选择的一定量下采样后，特征图被矢量化并输入到一系列全连接层中。最后一层的输出数量与数据集中类别的数量相同。

![](../Images/591b3afafa3c1ba51183474506ac49c5.png)

### 目标检测

目标检测有两种类型：一阶段和两阶段。它们都以“锚框”开始；这些是默认的边界框。我们的探测器将预测这些框与真实框之间的*差异*，而不是直接预测框。

在一个两阶段探测器中，我们自然有两个网络：一个框提议网络和一个分类网络。框提议网络提出了边界框的坐标，这些坐标是基于它认为对象可能存在的高概率区域；这些坐标相对于锚框是*相对的*。分类网络则对每个边界框中的潜在对象进行分类。

在一阶段探测器中，提议和分类网络被融合成一个单独的阶段。网络直接预测边界框坐标和框内的类别。由于两个阶段融合在一起，一阶段探测器通常比两阶段的更快。但由于任务的分离，两阶段探测器的准确性更高。

![图示](../Images/969ef8b51646f839a7a9648728c4337a.png)

Faster-RCNN 两阶段目标检测架构 ![图示](../Images/0be8d94083d3a339242c26d3f45aa040.png)

SSD 一阶段目标检测架构

### 分割

分割是计算机视觉中较为独特的任务之一，因为网络需要学习低层次和高层次的信息。低层次信息用于准确地按像素分割图像中的每个区域和对象，而高层次信息则用于直接分类这些像素。这导致网络被设计为将早期层和高分辨率（低层次空间信息）的信息与更深层次和低分辨率（高层次语义信息）的信息结合起来。

如下所示，我们首先通过标准分类网络处理图像。然后，我们从网络的每一阶段提取特征，从而利用从低到高的一系列信息。每个信息层级在合并之前会独立处理。当信息被合并时，我们上采样特征图，以最终达到完整的图像分辨率。

想了解更多关于深度学习分割的细节，请查看 [这篇文章](https://towardsdatascience.com/semantic-segmentation-with-deep-learning-a-guide-and-code-e52fc8958823)。

![图示](../Images/3c3bf26f47d1e4fc2a532b728a7deab7.png)

GCN 分割架构

### 姿态估计

姿态估计模型需要完成两个任务：（1）检测图像中每个身体部位的关键点（2）找出如何正确连接这些关键点。这分为三个阶段：

(1) 使用标准分类网络从图像中提取特征

(2) 给定这些特征，训练一个子网络来预测一组2D热图。每个热图与特定关键点相关，并包含关于每个图像像素是否可能存在关键点的置信度值。

(3) 再次利用分类网络的特征，我们训练一个子网络来预测一组2D矢量场，每个矢量场编码了关键点之间的关联程度。高关联的关键点被认为是连接在一起的。

以这种方式训练模型与子网络将共同优化关键点的检测和连接。

![图示](../Images/e271162f9bf7faad065120ef15091627.png)

OpenPose姿态估计架构

### 增强与恢复

增强和恢复网络是它们自己独特的存在。我们不对这些进行*任何*下采样，因为我们真正关心的是高像素/空间准确性。下采样会降低空间准确性，因为它减少了用于空间准确性的像素数。相反，所有处理都在全图像分辨率下进行。

我们首先将要增强/恢复的图像以全分辨率传递给我们的网络，不做任何修改。该网络仅由多个卷积和激活函数堆叠而成。这些模块通常受到启发，并且偶尔直接复制自最初为图像分类开发的模块，如[Residual Blocks](https://arxiv.org/pdf/1512.03385.pdf)、[Dense Blocks](https://arxiv.org/pdf/1608.06993.pdf)、[Squeeze Excitation Blocks](https://arxiv.org/pdf/1709.01507.pdf)等。最后一层没有激活函数，甚至没有sigmoid或softmax，因为我们希望直接预测图像像素，而不需要任何概率或分数。

这就是这些类型网络的全部内容！在图像的全分辨率下进行大量处理，以实现高空间准确性，使用已被证明在其他任务中有效的相同卷积。

![图示](../Images/4241804302e82822e35bd2ecaa6d868c.png)

EDSR超分辨率架构

### 动作识别

动作识别是少数几个特定需要*视频数据*才能良好工作的应用之一。要分类一个动作，我们需要了解场景随时间发生的变化，这自然要求我们需要视频。我们的网络必须学会*空间*和*时间*信息，即*空间*和*时间*的变化。最适合这个任务的网络是*3D-CNN*。

3D-CNN，顾名思义，是一种使用3D卷积的卷积网络！它们与常规CNN的不同之处在于卷积应用于3个维度：宽度、高度和*时间*。因此，每个输出像素的预测基于其周围像素和相同位置的前后帧中的像素的计算！

![图示](../Images/39e99ebba274c96308b31a606822f0d3.png)

直接传递大批量图像

视频帧可以通过几种方式传递：

(1) 直接在大批量中，如第一张图所示。由于我们传递的是一系列帧，因此同时提供了空间和时间信息。

![图示](../Images/e5e704adf682f38d75b8971205ff2a50.png)

单帧 + 光流（左）。视频 + 光流（右）

(2) 我们也可以在一个流中传递单个图像帧（数据的空间信息）及其对应的视频光流表示（数据的时间信息）。我们会使用常规的2D CNN从这两者中提取特征，然后将它们结合后传递给我们的3D CNN，后者结合了这两种信息。

(3) 将我们的帧序列传递给一个3D CNN，将视频的光流表示传递给另一个3D CNN。这两个数据流都有空间和时间信息。这将是最慢的选项，但也可能是最准确的，因为我们对视频的两种不同表示进行特定处理，而这两种表示包含了所有信息。

所有这些网络输出视频的动作分类。

### 喜欢学习吗？

在[ twitter](https://twitter.com/GeorgeSeif94)上关注我，我会发布关于最新和最伟大的AI、技术和科学的内容！

**简介：[George Seif](https://towardsdatascience.com/@george.seif94)** 是一位认证的极客和AI/机器学习工程师。

[原文](https://towardsdatascience.com/how-to-do-everything-in-computer-vision-2b442c469928)。经许可转载。

**相关：**

+   [快速轻松解决任何图像分类问题](/2018/12/solve-image-classification-problem-quickly-easily.html)

+   [Scikit Learn简介：Python机器学习的黄金标准](/2019/02/introduction-scikit-learn-gold-standard-python-machine-learning.html)

+   [如何为机器学习设置Python环境](/2019/02/setup-python-environment-machine-learning.html)

### 更多相关话题

+   [TensorFlow在计算机视觉中的应用 - 转移学习轻松实现](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍MLM的最新……](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的5个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [关于数据管理你需要知道的 6 件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：在 5 天内构建一个机器学习 Web 应用](https://www.kdnuggets.com/2022/n10.html)

+   [DINOv2：Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)
