# 2019年人体姿态估计指南

> 原文：[https://www.kdnuggets.com/2019/08/2019-guide-human-pose-estimation.html](https://www.kdnuggets.com/2019/08/2019-guide-human-pose-estimation.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![图](../Images/55d0d8b6f3e8490e3924480c94501134.png)

照片由[David Hofmann](https://unsplash.com/@davidhofmann)拍摄，来源于[Unsplash](https://unsplash.com/search/photos/dance)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

人体姿态估计指的是在图像中推断姿态的过程。本质上，它涉及预测图像或视频中人物关节的位置。这个问题有时也被称为人体关节的定位。值得注意的是，姿态估计还有各种子任务，如单一姿态估计、多人图像中的姿态估计、拥挤场所中的姿态估计以及视频中的姿态估计。

姿态估计可以在3D或2D中进行。人体姿态估计的一些应用包括：

+   [活动识别](https://www.researchgate.net/publication/277411291_Real-Time_Human_Pose_Estimation_and_Gesture_Recognition_from_Depth_Images_Using_Superpixels_and_SVM_Classifier)

+   [动画](https://blog.deepmotion.com/2019/05/21/markerless-augmented-reality-for-ar-avatars/)

+   [游戏](https://www.microsoft.com/en-us/research/publication/key-developments-in-human-pose-estimation-for-kinect/)

+   [增强现实](https://arxiv.org/abs/1806.09316)

在我们将要重点介绍的论文中使用的一些方法包括**自下而上**和**自上而下**。本质上，自下而上的方法是从高分辨率到低分辨率处理，而自上而下的方法则是从低分辨率到高分辨率处理。

自上而下的方法首先使用边界框[物体检测器](https://heartbeat.fritz.ai/a-2019-guide-to-object-detection-9509987954c3)来识别和定位单个人体实例。接下来估计单个人的姿态。自下而上的方法则从定位不带身份的语义实体开始，然后将其分组为人体实例。

我们将现在查看一些为解决人体姿态估计问题而进行的研究：

1.  [DeepPose: 通过深度神经网络进行人体姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#0057)

1.  [使用卷积网络的高效目标定位](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#e6c7)

1.  [基于迭代错误反馈的人体姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#ae35)

1.  [用于人体姿态估计的堆叠沙漏网络](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#b8ef)

1.  [卷积姿态机器](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#480f)

1.  [DeepCut：用于多人的姿态估计的联合子集分区和标注](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#aca3)

1.  [用于人体姿态估计和跟踪的简单基线](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#9ee6)

1.  [RMPE：区域性多人姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#b177)

1.  [OpenPose：使用部件亲和场进行实时多人2D姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#7c7f)

1.  [现实世界拥挤场景的人体姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#9b96)

1.  [DensePose：野外密集人体姿态估计](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#0c65)

1.  [PersonLab：基于自下而上、部件化、几何嵌入模型的人体姿态估计和实例分割](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73#36d4)

### DeepPose：通过深度神经网络进行的人体姿态估计（CVPR, 2014）

本文建议使用深度神经网络（DNNs）来解决这一机器学习任务。本文的作者是来自谷歌的亚历山大·托舍夫（Alexander Toshev）和克里斯蒂安·施泽迪（Christian Szegedy）。姿态估计的公式化本身是对关节进行基于DNN的回归。作者在标准基准测试集，如MPII、LSP和FLIC数据集上实现了最先进的结果。他们还分析了联合训练具有重复中间监督的多阶段架构的效果。

**[DeepPose：通过深度神经网络进行的人体姿态估计](https://arxiv.org/abs/1312.4659?source=post_page-----c10b79b64b73----------------------)**

我们提出了一种基于深度神经网络（DNNs）的人体姿态估计方法。姿态估计被公式化为…

DNN能够捕捉所有关节的内容，并且不需要使用图形模型。如下面所示，网络由七层组成。这些层包括一个池化层、一个卷积层和一个全连接层。

卷积层和全连接层是唯一具有可学习参数的层。它们都包含线性变换，后跟一个修正线性单元。网络输入图像的大小为220 × 220，学习率设置为0.0005。全连接层的dropout正则化设置为0.6。该模型使用的一些数据集包括[电影标记帧 (FLIC)](https://bensapp.github.io/flic-dataset.html)和[利兹体育数据集](http://sam.johnson.io/research/lsp.html)。

![图](../Images/ed678246402935f1a64e6189c27dbe0b.png)

[源](https://arxiv.org/abs/1312.4659)

下图展示了模型在正确部件百分比 (PCP) 评估指标上的表现。

![图](../Images/09ab5fcd5228837e18a7ae3c8145dbda.png)

[源](https://arxiv.org/abs/1312.4659)![图](../Images/d844ddd9e34ef892417fb3c069ffadd9.png)

[源](https://arxiv.org/abs/1312.4659)

### 高效目标定位使用卷积网络 (2015)

这篇论文提出了一种ConvNet架构，用于预测单目RGB图像中的人体关节位置。本文作者来自纽约大学。该模型允许增加池化操作，从而提高计算效率。

**[高效目标定位使用卷积网络](https://arxiv.org/abs/1411.4280?source=post_page-----c10b79b64b73----------------------)**

最近，在人体姿态估计方面取得了深度卷积网络的**最先进**性能…

网络首先执行身体部位定位，并输出低分辨率的逐像素热图。该热图显示了关节在图像中每个空间位置出现的概率。

![图](../Images/0c1c9fd0bcd6a1dd095524e4fe56c674.png)

[源](https://arxiv.org/pdf/1411.4280.pdf)

论文还介绍了一个网络，该网络利用热图回归模型中的隐藏层特征以提高定位准确性。该模型使用多分辨率ConvNet架构，并实现了具有重叠上下文的滑动窗口检测器，以生成粗略的热图输出。滑动窗口通常是一个具有固定高度和宽度的矩形框。框在图像上滑动。当框滑动时，分类器尝试识别该区域是否存在对当前任务感兴趣的物体。

![图](../Images/09b5c5a0d8d08b47e98115ff5f6c92d8.png)

[源](https://arxiv.org/pdf/1411.4280.pdf)

下图展示了本文提出的完整模型架构。该架构使用Torch7实现，并通过FLIC和MPII-Human-Pose数据集进行评估。

![图](../Images/4128b5ce22ea37e2f254554d21aea4c4.png)

[源](https://arxiv.org/pdf/1411.4280.pdf)

通过在FLIC数据集上使用标准PCK（关键点正确百分比）度量以及在MPII数据集上使用PCKh度量来评估模型的性能。

![图](../Images/98c7f764c9b6efd5148d3ede2a104914.png)

[来源](https://arxiv.org/pdf/1411.4280.pdf)![图](../Images/5df7c18ed1dfd58cf04526539ce08c06.png)

[来源](https://arxiv.org/pdf/1411.4280.pdf)

### 迭代误差反馈的人体姿态估计（2016）

这篇论文提出了一个框架，扩展了层级特征提取器的表达能力，包括输入和输出空间。它通过引入自上而下的反馈来实现这一点。本文的作者来自加州大学伯克利分校。

输出不是一次性预测的；而是使用一个自我修正的模型来反馈误差预测。作者称这个过程为迭代误差反馈（IEF）。该模型在挑战性的MPII和LSP基准测试中对关节姿态估计任务产生了出色的结果。

**[迭代误差反馈的人体姿态估计](https://arxiv.org/abs/1507.06550?source=post_page-----c10b79b64b73----------------------)**

层级特征提取器，如卷积网络（ConvNets），在...

下图展示了用于2D人体姿态估计的迭代误差反馈（IEF）实现。左侧面板显示了输入图像I和关键点的初步猜测Y0。这里的三个关键点分别对应右腕（绿色）、左腕（蓝色）和头顶（红色）。该架构中的函数f被建模为卷积神经网络。函数g将每个2D关键点位置转换为一个高斯热图通道。

![图](../Images/9c5cc095f61ad5b345ae4f612f998531.png)

[来源](https://arxiv.org/abs/1507.06550)![图](../Images/09eae34ba0923ca93c2f0f6d73b64e93.png)

这个模型可以通过这个数学方程进行可视化。![图](../Images/2a684ba4918beac3a95a03ef33f35278.png)

[来源](https://arxiv.org/abs/1507.06550)

下表显示了该模型的性能。

![图](../Images/5b32f85a566d7c1d7b0f4887328cf447.png)

[来源](https://arxiv.org/abs/1507.06550)

### 堆叠小时玻璃网络用于人体姿态估计（2016）

这篇论文认为，反复的自下而上和自上而下的处理加上中间监督可以提高所提出网络的性能。该网络被称为“堆叠小时玻璃”，因为为了生成最终预测，进行了一系列的下采样和上采样处理。本文的作者来自密歇根大学。

**[堆叠小时玻璃网络用于人体姿态估计](https://arxiv.org/abs/1603.06937?source=post_page-----c10b79b64b73----------------------)**

这项工作介绍了一种新型的卷积网络架构，用于人体姿态估计任务。特征...

![](../Images/dc6abd6307386c4c1ff37cf606af45f8.png)

网络在FLIC和MPII人体姿态基准上进行了测试。它在MPII上所有关节的平均准确度提高了2%以上，在困难的关节如踝关节和膝关节上提高了4-5%。

![图](../Images/a31b700b1fafcf1ef8e3746964ce9b64.png)

[来源](https://arxiv.org/abs/1603.06937)

沙漏架构旨在捕捉每个尺度上的信息。网络输出像素级的预测。网络设置包括卷积层和用于特征处理的最大池化层。网络输出热图，预测每个像素级别上特定关节的出现。

![图示](../Images/11db08a91e470d495235f4e2e3993281.png)

[来源](https://arxiv.org/abs/1603.06937)

下图显示了模型在各种身体部位上的表现。

![图示](../Images/928170938b5018d6ca45f2079330fc9e.png)

[来源](https://arxiv.org/abs/1603.06937)

### **卷积姿态机器（2016）**

本文介绍了用于细致姿态估计的卷积姿态机器（CPMs）。CPMs由一系列卷积网络组成，这些网络生成每个部位位置的2D信念图。本文来自卡内基梅隆大学机器人研究所。

**[卷积姿态机器](https://arxiv.org/abs/1602.00134?source=post_page-----c10b79b64b73----------------------)**

姿态机器提供了一个顺序预测框架，用于学习丰富的隐式空间模型。在这项工作中我们展示了…

在CPM的每个阶段，前一阶段生成的图像特征和信念图用作输入。

![图示](../Images/f7a2e25316f2331dfeed679200733639.png)

[来源](https://arxiv.org/pdf/1602.00134.pdf)

网络通过卷积架构的顺序组合来学习隐式空间模型。它还引入了一种系统的方法来设计和训练这样的架构，以便学习图像特征和图像依赖的空间模型用于结构化预测任务。这不需要使用任何图形模型风格的推断。

网络在MPII、LSP和FLIC数据集上进行了测试。

![图示](../Images/46757ee79bd0744587d3a9cf02b90b5b.png)

[来源](https://arxiv.org/pdf/1602.00134.pdf)

模型的PCKh-0.5得分达到了87.95%的前沿结果，而踝部的PCKh0.5得分为78.28%。它已经使用Caffe实现，并且[代码已开源](https://github.com/shihenw/convolutional-pose-machines-release)。

![图示](../Images/965e2b2b97fffb78eefd8913c1d2490d.png)

[来源](https://arxiv.org/pdf/1602.00134.pdf)

### DeepCut: 关节子集划分与标注用于多人人体姿态估计（CVPR 2016）

本文提出了一种用于检测多人人体姿态的方法。该模型通过检测图像中的人数，然后预测每个图像的关节位置。本文由马克斯·普朗克研究所和斯坦福大学联合完成。

**[DeepCut: 关节子集划分与标注用于多人人体姿态估计](https://arxiv.org/abs/1511.06645?source=post_page-----c10b79b64b73----------------------)**

本文考虑了对现实世界图像中多个人体姿态进行细致估计的任务。我们提出了…

![图](../Images/699d93f9201c67d4a4e3081b918006a9.png)

[来源](https://arxiv.org/abs/1511.06645)

为了获得强大的部位检测器，作者调整了FastRCN以适应这个任务。他们在两个方面进行了修改：提议生成和检测区域大小。该模型在预测各种身体部位方面的性能如下图所示。

![图](../Images/4bc0448de2b8aea3707c2ff1bbedfa60.png)

[来源](https://arxiv.org/abs/1511.06645)

由于使用身体部位检测的提议可能效果不佳，作者使用了一个步幅为32像素的全卷积VGG，并将步幅减少到8像素。他们将图像输入缩放到340像素的站立高度，这样可以获得最佳效果。

对于损失函数，他们首先尝试了输出不同身体部位概率的softmax。后来，他们在输出神经元上实现了sigmoid激活函数和交叉熵损失。最终，他们发现sigmoid激活函数比softmax损失函数获得了更好的结果。该模型在Leeds Sports Poses (LSP)、LSP Extended (LSPET)和MPII Human Pose上进行了训练和评估。

![图](../Images/74fb000184fe58032a6c7d35fdaa6ed5.png)

[来源](https://arxiv.org/abs/1511.06645)![图](../Images/a39fec1a973acf60777fbbbcdef4ad1e.png)

[来源](https://arxiv.org/abs/1511.06645)

### 《用于人体姿态估计和跟踪的简单基线》（EECV, 2018）

这篇论文的姿态估计解决方案基于在[ResNet](https://en.wikipedia.org/wiki/Residual_neural_network)上添加的反卷积层。该模型在COCO测试开发集上达到了73.7的mAP。其姿态跟踪模型达到了74.6的mAP评分和57.8的MOTA（多目标跟踪准确率）评分。本文的作者来自微软亚洲研究院和电子科技大学。

**[用于人体姿态估计和跟踪的简单基线](https://arxiv.org/abs/1804.06208?source=post_page-----c10b79b64b73----------------------)**

最近几年在姿态估计方面取得了显著进展，并且对姿态跟踪的兴趣不断增加。 在…

该网络使用的方法在ResNet架构的最后卷积阶段上添加了几个反卷积层。这种结构使得从深度和低分辨率图像生成热图变得非常容易。默认使用三个带有批量归一化和ReLU激活的反卷积层。

![图](../Images/eb348438c6b5f460b7903dfbfc766bb5.png)

[来源](https://arxiv.org/pdf/1804.06208.pdf)

下图展示了姿态跟踪框架的提议流程。视频中的姿态跟踪首先通过估计人体姿态来进行，为其分配一个唯一标识符，然后在各帧之间跟踪该标识符。

![图](../Images/db6bc67d352424ee370c1b8a5a4130f3.png)

[来源](https://arxiv.org/pdf/1804.06208.pdf)

以下是该模型与其他模型的比较。

![图](../Images/ef431646692f65135a831fa749b6f869.png)

[来源](https://arxiv.org/pdf/1804.06208.pdf)

### RMPE: 区域多人物体姿态估计（2018）

本文提出了一个区域多人物体姿态估计（RMPE）框架，用于在不准确的人体边界框中进行估计。该框架包含三个组件：对称空间变换网络（SSTN）、参数化姿态非极大值抑制（NMS）和姿态引导提议生成器（PGPG）。该框架在MPII（多人物体）数据集上实现了76.7的mAP。本文的作者来自中国上海交通大学和腾讯优图。

**[RMPE: 区域多人物体姿态估计](https://arxiv.org/abs/1612.00137?source=post_page-----c10b79b64b73----------------------)**

在实际环境中的多人物体姿态估计具有挑战性。尽管最先进的人类检测器已证明…

![图像](../Images/0d27121f2c13c8570ea2ad5d4117a89b.png)

[来源](https://arxiv.org/abs/1612.00137)

在该框架中，人类检测器获得的边界框被输入到“对称STN + SPPE”模块。然后自动生成姿态提议。这些姿态通过参数化姿态NMS进行微调，以获得估计的人体姿态。在训练中，引入了“并行SPPE”以避免局部最小值。

![图像](../Images/096f1ecce3bd5cb97d7f60f73e64689a.png)

[来源](https://arxiv.org/abs/1612.00137)

下面的图展示了模型与其他框架的性能比较，以及一些通过它获得的姿态预测。

![图像](../Images/d5759be84d4332212c7b654881a1d4e8.png)

[来源](https://arxiv.org/abs/1612.00137)

### OpenPose: 实时多人物体2D姿态估计使用部位关联字段（2019）

OpenPose是一个开源的实时多人物体2D姿态检测系统，包括身体、脚、手和面部关键点。本文提出了一种实时检测图像和视频中2D人体姿态的方法。该方法使用了称为部位关联字段（PAFs）的非参数表示。本文的一些作者来自IEEE。

**[OpenPose: 实时多人物体2D姿态估计使用部位关联字段](https://arxiv.org/abs/1812.08008?source=post_page-----c10b79b64b73----------------------)**

实时多人物体2D姿态估计是让机器理解人类的关键组成部分…

![图像](../Images/507abb2d77ebcbd20fb97a2ac46e9386.png)

[来源](https://arxiv.org/abs/1812.08008)

如下所示，该方法以图像作为CNN的输入，并预测用于检测身体部位的置信度图和用于部位关联的PAFs。本文还展示了一个注释脚数据集，包含15K个人脚实例。该数据集已公开发布。

![](../Images/0ab5f986ea08fcd12822cf88d1aa56fa.png)

网络架构迭代地预测编码部位间关联（以蓝色显示）和检测置信度图（以米色显示）的关联字段。

![Figure](../Images/3319378587ca10cea5ff768783a76e5a.png)

[source](https://arxiv.org/abs/1812.08008)

OpenPose 也是一个周边软件和 API，能够从各种来源中获取图像。例如，可以选择输入为相机视频、网络摄像头、视频或图像。它运行在不同的平台上，如 Ubuntu、Windows、Mac OS X 和嵌入式系统（例如，Nvidia Tegra TX2）。它还支持不同的硬件，如 CUDA GPU、OpenCL GPU 和仅 CPU 的设备。

OpenPose 包含三个模块：身体+脚检测、手部检测和面部检测。它已在 MPII 人体多人物数据集、COCO 关键点挑战数据集以及论文提出的脚数据集上进行了评估。下图展示了 OpenPose 与其他模型的比较结果。

![Figure](../Images/19fe9ad7f2cfa40147ffbff4d7271a02.png)

[source](https://arxiv.org/abs/1812.08008)

### 现实世界拥挤场景的人体姿态估计 (AVSS, 2019)

本文提出了用于估计人群姿态的多种方法。在这些密集人群区域中估计姿态的挑战包括人员彼此接近、相互遮挡以及部分可见性。本文作者来自弗劳恩霍夫光电研究所和卡尔斯鲁厄理工学院 KIT。

**[现实世界拥挤场景的人体姿态估计](https://arxiv.org/abs/1907.06922?source=post_page-----c10b79b64b73----------------------)**

人体姿态估计最近在采用深度卷积神经网络方面取得了显著进展…

![Figure](../Images/69bc1ea9e8b85e61a7ce6e20f9911d9d.png)

[source](https://arxiv.org/abs/1907.06922)

用于优化拥挤图像的姿态估计的方法之一是使用 ResNet50 网络作为骨干的单人姿态估计器。该方法是一种两阶段的自上而下的方法，首先定位每个人，然后对每个预测进行单人姿态估计。

论文还介绍了两个遮挡检测网络；Occlusion Net 和 Occlusion Net Cross Branch。Occlusion Net 在两个转置卷积后进行拆分，以便在之前的层中学习联合表示。Occlusion Net Cross Branch 在一个转置卷积后进行拆分。遮挡检测网络输出每个姿态两个热图集。一个热图用于可见关键点，另一个用于遮挡关键点。

![Figure](../Images/586cd342ebef504cdcc1125b88332296.png)

[source](https://arxiv.org/abs/1907.06922)

下表展示了该模型与其他模型的性能比较。

![Figure](../Images/d4250b836d4064589ba4fb90a3c4b027.png)

[source](https://arxiv.org/abs/1907.06922)

### DensePose: Dense Human Pose Estimation In The Wild (2018)

这是一篇来自INRIA-CentraleSupelec和Facebook AI Research的论文，其目标是将RGB图像的所有人体像素映射到人体的3D表面。论文还介绍了一个DensePose-COCO数据集。该数据集包含50000张经过人工注释的COCO图像的图像与表面对应关系。作者随后使用该数据集训练基于CNN的系统，以在背景、遮挡和尺度变化的情况下提供密集的对应关系。对应关系基本上表示了一个图像中的图像如何与另一个图像中的像素对应。

![图](../Images/418b249d40b2e6d48af2b3e32b4a5c1c.png)

[来源](https://arxiv.org/pdf/1802.00434.pdf)

**[DensePose: Dense Human Pose Estimation In The Wild](https://arxiv.org/abs/1802.00434?source=post_page-----c10b79b64b73----------------------)**

在这项工作中，我们建立了RGB图像与基于表面的人的表示之间的密集对应关系……

在这个模型中，单张RGB图像被作为输入，用于建立表面点和图像像素之间的对应关系。

![图](../Images/58d6a82539aa873ea2748c4e3fd5ba66.png)

[来源](https://arxiv.org/abs/1802.00434)

该模型的这种方法是通过与Mask-RCNN系统结合建立的。该模型在GTX 1080 GPU上对240 × 320图像的运行速度为20–26帧每秒，对240 × 320图像的运行速度为4–5帧每秒。

![图](../Images/ceffce83779d1920fc62ffcd494adb6c.png)

[来源](https://arxiv.org/abs/1802.00434)

作者将Dense Regression（DenseReg）系统与Mask-RCNN架构相结合，提出了DensePose-RCNN系统。

![图](../Images/72f6e710b2c95542388340e3ef4f0568.png)

[来源](https://arxiv.org/abs/1802.00434)

该模型使用一个全卷积网络，专门用于生成分类和回归头，以进行分配和坐标预测。作者使用了MaskRCNN关键点分支中使用的相同架构。它由8层交替的3×3全卷积层和ReLU层堆叠而成，具有512个通道。

![图](../Images/5acc00760af4216e598c4db272924d01.png)

[来源](https://arxiv.org/abs/1802.00434)

作者在一个包含2300个人的1500张图像的测试集上进行了实验，并在一个包含48000人训练集上进行了实验。下图展示了其与其他方法的性能比较。

![图](../Images/e7c18974e7ebfd363aaf7334b0ffd599.png)

[来源](https://arxiv.org/abs/1802.00434)![图](../Images/ab30fc8ee857fa55f8549e28ea20f8e9.png)

[来源](https://arxiv.org/abs/1802.00434)

### PersonLab: 基于自下而上的部分模型进行姿态估计和实例分割（2018）

这篇论文的作者来自Google。他们提出了一种无框的自下而上的姿态估计和实例分割方法，适用于多人的图像。

![图](../Images/45b18cdd4e6d34b5143b3111719d91b9.png)

[来源](https://arxiv.org/pdf/1803.08225.pdf)

这意味着作者首先检测身体部位，然后将这些部位归类为人类实例。该方法在COCO测试集上的关键点平均精度为0.665（单尺度推理）和0.687（多尺度推理）。

**[PersonLab: 基于自下而上的部分基础几何模型的人物姿态估计与实例分割](https://arxiv.org/abs/1803.08225?source=post_page-----c10b79b64b73----------------------)**

我们提出了一种无框的自下而上的方法，用于姿态估计和实例分割任务。

本文提出的模型是一个无框的完全卷积系统，该系统首先预测图像中每个人的所有关键点。该模型在COCO关键点数据集上进行训练。

在关键点检测阶段，该模型检测图像中可见的关键点。PersonLab系统在标准COCO关键点任务以及仅针对人员类别的COCO实例分割任务上进行了评估。

![图](../Images/5d51faa167fc8433d13f5db55a8e06da.png)

[来源](https://arxiv.org/pdf/1803.08225.pdf)

下图展示了其在COCO关键点测试集上的表现。

![图](../Images/98580f66ab64fccb46db21736e4f1155.png)

[来源](https://arxiv.org/pdf/1803.08225.pdf)

### 结论

我们现在应该对一些最常见的——以及一些非常新的——人类姿态估计技术有了了解，这些技术适用于各种情境。

上述提到并链接到的论文/摘要也包含了其代码实现的链接。我们很乐意看到你在测试后获得的结果。

**个人简介: [Derrick Mwiti](https://derrickmwiti.com/)** 是一名数据分析师、作家和导师。他致力于在每项任务中取得优异成果，并且是Lapid Leaders Africa的导师。

[原文](https://heartbeat.fritz.ai/a-2019-guide-to-human-pose-estimation-c10b79b64b73)。转载已获许可。

**相关:**

+   [2019年语义分割指南](/2019/08/2019-guide-semantic-segmentation.html)

+   [2019年目标检测指南](/2019/08/2019-guide-object-detection.html)

+   [使用Luminoth进行目标检测](/2019/03/object-detection-luminoth.html)

### 更多相关主题

+   [R与Python（再次）：人因因素视角](https://www.kdnuggets.com/2022/01/r-python-human-factor-perspective.html)

+   [深度学习与人类认知能力之间的差距](https://www.kdnuggets.com/2022/10/gap-deep-learning-human-cognitive-abilities.html)

+   [缩小人类理解与机器学习之间的差距：…](https://www.kdnuggets.com/2023/06/closing-gap-human-understanding-machine-learning-explainable-ai-solution.html)

+   [超越人类边界：超智能的崛起](https://www.kdnuggets.com/beyond-human-boundaries-the-rise-of-superintelligence)

+   [自然语言处理：用AI架起人类沟通的桥梁](https://www.kdnuggets.com/natural-language-processing-bridging-human-communication-with-ai)

+   [超越编程：为何人情味至关重要](https://www.kdnuggets.com/beyond-coding-why-the-human-touch-matters)
