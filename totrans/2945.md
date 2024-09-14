# 构建计算机视觉模型：方法和数据集

> 原文：[https://www.kdnuggets.com/2019/05/computer-vision-model-approaches-datasets.html](https://www.kdnuggets.com/2019/05/computer-vision-model-approaches-datasets.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Javier Couto](https://twitter.com/JCoutoNLP)，Tryolabs**。

计算机视觉是机器学习中最热门的子领域之一，鉴于其[广泛的应用](https://tryolabs.com/resources/introductory-guide-computer-vision/#industry-applications)和巨大的潜力。其目标是复制人类视觉的强大能力。但这如何通过算法实现呢？

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

让我们深入了解最重要的数据集和方法。

### **现有数据集**

计算机视觉算法并非魔法。它们需要数据才能工作，它们的效果仅能与输入的数据质量相匹配。以下是根据任务收集正确数据的不同来源：

其中一个最大且最著名的数据集是**[ImageNet](http://www.image-net.org/)**，这是一个拥有 1400 万张图像的现成数据集，手动标注了使用**[WordNet](https://wordnet.princeton.edu/)** 概念。在全球数据集中，100 万张图像包含边界框注释。

![](../Images/47bdf49b1e7bdf044830642dc62105e9.png)

**带有边界框的 ImageNet 图像。[图片来源](http://www.image-net.org/bbox_fig/kit_fox.JPG)**

![](../Images/1de2789c0f6f512afa807174edb804a3.png)

**带有对象属性注释的 ImageNet 图像。[图片来源](http://www.image-net.org/attribute_fig/pullfigure.jpg)**

另一个著名的数据集是**[Microsoft Common Objects in Context (COCO)](http://cocodataset.org/#home)**，包含 328,000 张图像，包括 91 种对象类型，这些类型对于 4 岁的孩子来说很容易识别，总共有 250 万个标注实例。

![](../Images/e60ccd5b9feab0cca16b8b7d9ebd07f8.png)

**来自 COCO 数据集的标注图像示例。[图片来源](https://arxiv.org/abs/1405.0312)**

尽管可用的数据集不是很多，但有几个适合不同任务的，例如 **[CelebFaces 属性数据集](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)**（CelebA，包含超过 20 万张名人图像的面部属性数据集）；**[室内场景识别](http://web.mit.edu/torralba/www/indoor.html)** 数据集（15,620 张室内场景图像）；和 **[植物图像分析](https://www.plant-image-analysis.org/dataset)** 数据集（来自 11 个不同物种的 100 万张植物图像）。

### **一般策略**

**[深度学习方法和技术](https://tryolabs.com/blog/2018/12/19/major-advancements-deep-learning-2018/)** 深刻地改变了计算机视觉以及人工智能的其他领域，以至于对于许多任务，其使用被认为是标准的。特别是，**[卷积神经网络](https://www.kdnuggets.com/2016/11/intuitive-explanation-convolutional-neural-networks.html)**（CNN）利用传统计算机视觉技术取得了超越现有技术水平的成果。

这四个步骤概述了使用 CNN 构建计算机视觉模型的一般方法：

1.  创建一个由标注图像组成的数据集，或者使用现有的数据集。标注可以是图像类别（用于分类问题）；一对对的边界框和类别（用于物体检测问题）；或每个图像中感兴趣对象的逐像素分割（用于实例分割问题）。

1.  从每张图像中提取与当前任务相关的特征。这是建模问题的关键点。例如，用于识别面孔的特征，与基于面部标准的特征显然不同于用于识别旅游景点或人体器官的特征。

1.  基于提取的特征训练深度学习模型。训练意味着向机器学习模型输入许多图像，模型将根据这些特征学习如何解决手头的任务。

1.  使用未在训练阶段使用的图像评估模型。通过这样做，可以测试训练模型的准确性。

这个策略非常基础，但能够很好地达到目的。这样的做法，被称为 **[监督式机器学习](https://www.kdnuggets.com/2017/11/3-different-types-machine-learning.html)**，需要一个涵盖模型需要学习的现象的数据集。

### **训练物体检测模型**

#### **维奥拉和琼斯方法**

有许多方法可以解决物体检测挑战。多年来，流行的方法是保罗·维奥拉和迈克尔·琼斯在论文中提出的 **[鲁棒实时物体检测](http://www.hpl.hp.com/techreports/Compaq-DEC/CRL-2001-1.pdf)**。

尽管可以训练以检测多种类别的对象，但这一方法最初是以面部检测为目标的。它速度快且简单，已被实现到傻瓜相机中，允许在实时面部检测中使用极少的处理能力。

该方法的核心特征是使用基于**[Haar特征](https://en.wikipedia.org/wiki/Haar-like_feature)**的大量二分类器进行训练。这些特征表示边缘和线条，在扫描图像时计算非常简单。

![](../Images/bc6c26478e97521e0942d50b5f5a4dce.png)

**Haar特征。[图片来源](https://docs.opencv.org/3.4.3/haar_features.jpg)**

尽管相当基础，但在面部识别的具体情况下，这些特征可以捕捉到如鼻子、嘴巴或眉毛之间的距离等重要元素。这是一种监督方法，需要许多正例和负例的目标对象样本。

**检测蒙娜丽莎的面部**

### **基于CNN的方法**

深度学习在机器学习领域带来了真正的变革，尤其是在计算机视觉领域，基于深度学习的方法现在在许多常见任务中处于最前沿。

在为实现目标检测而提出的不同深度学习方法中，**[R-CNN](https://arxiv.org/abs/1311.2524)**（具有CNN特征的区域）特别容易理解。该工作的作者提出了一个三阶段的过程：

1.  使用区域提案方法提取可能的对象。

1.  使用CNN识别每个区域的特征。

1.  使用**[支持向量机（SVMs）](https://en.wikipedia.org/wiki/Support_vector_machine)**对每个区域进行分类。

![](../Images/2be7c020983b89d8c01e3077de68f762.png)

**R-CNN架构。[图片来源](https://arxiv.org/abs/1311.2524)**

原作中选择的区域提案方法是**[选择性搜索](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)**，尽管R-CNN算法对于采用的具体区域提案方法没有偏好。第3步非常重要，因为它减少了对象候选数量，使得该方法计算开销较小。

这里提取的特征不如之前提到的Haar特征直观。总而言之，使用CNN从每个区域提案中提取一个4096维的特征向量。由于CNN的特性，输入必须始终具有相同的维度。这通常是CNN的一个弱点，各种方法以不同的方式解决这个问题。就R-CNN方法而言，训练好的CNN架构需要固定尺寸的227 × 227像素的输入。由于提议的区域大小与此不同，作者的方法简单地将图像扭曲以适应所需的尺寸。

![](../Images/528b58be52eb7121ca6d994c8c7cbcd8.png)

**与CNN所需输入维度匹配的扭曲图像示例。[图片来源](https://arxiv.org/abs/1311.2524)**

尽管取得了优异的成果，但训练过程中遇到了一些障碍，最终该方法被其他方法超越。文章中详细回顾了一些这些内容，**[深度学习目标检测：终极指南](https://tryolabs.com/blog/2017/08/30/object-detection-an-overview-in-the-age-of-deep-learning/)**。

本文摘自*计算机视觉入门指南*，作者为Tryolabs，最初发布于[这里](https://tryolabs.com/resources/introductory-guide-computer-vision/)。 

**简介**：[Javier Couto](https://twitter.com/JCoutoNLP) 是Tryolabs的自由职业机器学习顾问和数据科学家，专注于应用机器学习解决商业问题。

**资源：**

+   [在线和基于Web的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [用于分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [像业余爱好者一样思考，像专家一样行动：计算机视觉职业生涯的经验教训](https://www.kdnuggets.com/2019/05/kanade-lessons-career-computer-vision.html)

+   [使用卷积神经网络和OpenCV预测年龄和性别](https://www.kdnuggets.com/2019/04/predict-age-gender-using-convolutional-neural-network-opencv.html)

+   [使用RetinaNet在航空图像中进行行人检测](https://www.kdnuggets.com/2019/03/pedestrian-detection-aerial-images-retinanet.html)

### 更多相关主题

+   [数据管理的6件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [TensorFlow在计算机视觉中的应用 - 让迁移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍MLM的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的5个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [DINOv2：Meta AI的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [KDnuggets新闻 2022年3月9日：在5分钟内构建一个机器学习Web应用程序…](https://www.kdnuggets.com/2022/n10.html)
