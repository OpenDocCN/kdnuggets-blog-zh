# 计算机视觉路线图

> 原文：[https://www.kdnuggets.com/2020/10/roadmap-computer-vision.html](https://www.kdnuggets.com/2020/10/roadmap-computer-vision.html)

[评论](#comments)![图](../Images/d5b378a317859ba27b3ef9d8ae612920.png)

图片由 [Ennio Dybeli](https://unsplash.com/@ennio5?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

### 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

计算机视觉（CV）如今是人工智能的主要应用之一（例如：图像识别、目标跟踪、多标签分类）。在本文中，我将带你了解组成计算机视觉系统的一些主要步骤。

计算机视觉系统的标准工作流程表示如下：

1.  一组图像进入系统。

1.  特征提取器用于对这些图像进行预处理并提取特征。

1.  机器学习系统利用提取的特征来训练模型并进行预测。

我们将简要地走过数据在这三种不同步骤中可能经历的一些主要过程。

### 图像进入系统

在尝试实现计算机视觉系统时，我们需要考虑两个主要组件：图像采集硬件和图像处理软件。部署计算机视觉系统的主要要求之一是测试其鲁棒性。我们的系统实际上应该能够对环境变化（如光照、方向、缩放的变化）保持不变，并能够重复执行其设计任务。为了满足这些要求，可能需要对系统的硬件或软件施加某种形式的约束（例如：远程控制光照环境）。

一旦图像从硬件设备中获取，就有许多可能的方式在软件系统中数值表示颜色（颜色空间）。最著名的颜色空间之一是 RGB（红、绿、蓝）和 HSV（色调、饱和度、明度）。使用 HSV 颜色空间的一个主要优点是，通过仅使用 HS 组件，我们可以使系统对光照变化具有不变性（图 1）。

![图](../Images/396d650ef2563ff57ffadeefb4b5f126.png)

图 1：RGB 与 HSV 颜色空间 [1]

### 特征提取器

### 图像预处理

一旦图像进入系统并使用色彩空间表示后，我们可以对图像应用不同的操作符以改善其表示：

+   **点操作符**：我们使用图像中的所有点来创建原始图像的转换版本（以便明确图像内部的内容，而不改变其内容）。点操作符的一些示例包括：强度归一化、直方图均衡化和阈值化。点操作符通常用于帮助人类视觉更好地可视化图像，但对于计算机视觉系统不一定提供任何优势。

+   **组操作符**：在这种情况下，我们从原始图像中提取一组点，以在图像的转换版本中创建一个单一的点。这种类型的操作通常通过使用卷积来完成。可以使用不同类型的卷积核与图像进行卷积，以获得我们的转换结果（图 2）。一些示例包括：直接平均、Gaussian 平均和中值滤波。对图像应用卷积操作可以减少图像中的噪声并改善平滑度（尽管这也可能稍微模糊图像）。由于我们使用一组点来创建新图像中的单一新点，新图像的维度必然会低于原始图像。解决此问题的一种方法是应用零填充（将像素值设置为零）或在图像的边界使用更小的模板。使用卷积的主要限制之一是当处理大型模板时的执行速度，解决此问题的一种可能方法是使用傅里叶变换。

![图示](../Images/b07b0d336704ac7a164992c3c85c1f26.png)

图 2: [卷积核](https://stats.stackexchange.com/questions/296679/what-does-kernel-size-mean/296701)

一旦对图像进行了预处理，我们可以应用更高级的技术以尝试通过使用如一阶边缘检测（例如 Prewitt 操作符、Sobel 操作符、Canny 边缘检测器）和霍夫变换等方法来提取图像中的边缘和形状。

### 特征提取

一旦对图像进行了预处理，可以使用特征提取器从图像中提取4种主要的特征形态：

+   **全局特征**：整个图像作为一个整体进行分析，并从特征提取器中得到一个单一的特征向量。一个简单的全局特征示例是像素值的直方图。

+   **网格或块基特征**：图像被划分为不同的块，并从每个不同的块中提取特征。提取图像块中特征的主要技术之一是密集 SIFT（尺度不变特征变换）。这种类型的特征主要用于训练机器学习模型。

+   **基于区域的特征**：图像被分割成不同的区域（例如，使用阈值分割或K-Means聚类等技术，然后通过连接组件将其连接成段），并从这些区域中的每一个提取特征。可以使用区域和边界描述技术（如矩和链码）来提取特征。

+   **局部特征**：在图像中检测到多个单一兴趣点，并通过分析兴趣点邻近的像素来提取特征。可以从图像中提取的两个主要兴趣点类型是角点和斑点，这些可以通过如Harris & Stephens检测器和高斯拉普拉斯方法等方法提取。最后，可以通过如SIFT（尺度不变特征变换）等技术从检测到的兴趣点提取特征。局部特征通常用于匹配图像以构建全景/3D重建或从数据库中检索图像。

一旦提取了一组判别特征，我们可以利用这些特征来训练机器学习模型进行推断。特征描述符可以通过如[OpenCV](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_feature2d/py_sift_intro/py_sift_intro.html)这样的库在Python中轻松应用。

### 机器学习

在计算机视觉中，分类图像的一个主要概念是视觉词袋（BoVW）。为了构建视觉词袋，我们首先需要通过从一组图像中提取所有特征（例如使用基于网格的特征或局部特征）来创建词汇表。接着，我们可以计算提取的特征在图像中出现的次数，并从结果中构建一个频率直方图。使用频率直方图作为基本模板，我们可以通过比较直方图来最终判断图像是否属于同一类别（见图 3）。

这个过程可以总结为以下几个步骤：

1.  我们首先通过使用特征提取算法（如SIFT和Dense SIFT）从图像数据集中提取不同的特征来构建词汇表。

1.  其次，我们使用如K-Means或DBSCAN等算法对我们词汇表中的所有特征进行聚类，并利用聚类中心来总结我们的数据分布。

1.  最后，我们可以通过计算词汇表中不同特征在图像中出现的次数来构建每个图像的频率直方图。

新图像可以通过对每个要分类的图像重复相同的过程，然后使用任何分类算法来找出我们词汇表中哪个图像最像我们的测试图像，从而进行分类。

![图](../Images/678873a6185b020045f6be05fb058d7d.png)

图 3: 视觉词袋 [2]

如今，得益于卷积神经网络（CNNs）和递归人工神经网络（RCNNs）等人工神经网络架构的创建，已经能够构思出一种计算机视觉的替代工作流（见图 4）。

![图像](../Images/133882f0c85d62f5e5f8e210f3c705ea.png)

图 4：计算机视觉工作流 [3]

在这种情况下，深度学习算法结合了计算机视觉工作流中的特征提取和分类步骤。使用卷积神经网络时，神经网络的每一层都应用不同的特征提取技术（例如，第 1 层检测边缘，第 2 层在图像中找到形状，第 3 层进行图像分割等……）然后将特征向量提供给全连接层分类器。

机器学习在计算机视觉中的进一步应用包括多标签分类和目标识别等领域。在多标签分类中，我们旨在构建一个能够正确识别图像中有多少对象以及它们属于哪个类别的模型。而在目标识别中，我们则更进一步，旨在识别图像中不同对象的位置。

### 联系方式

如果你想保持更新我的最新文章和项目，*请在 Medium 上关注我* [follow me on Medium](https://medium.com/@pierpaoloippolito28?source=post_page---------------------------) 并订阅我的 [邮件列表](http://eepurl.com/gwO-Dr?source=post_page---------------------------)。以下是我的一些联系信息：

+   [Linkedin](https://uk.linkedin.com/in/pier-paolo-ippolito-202917146?source=post_page---------------------------)

+   [个人博客](https://pierpaolo28.github.io/blog/?source=post_page---------------------------)

+   [个人网站](https://pierpaolo28.github.io/?source=post_page---------------------------)

+   [Medium 个人资料](https://towardsdatascience.com/@pierpaoloippolito28?source=post_page---------------------------)

+   [GitHub](https://github.com/pierpaolo28?source=post_page---------------------------)

+   [Kaggle](https://www.kaggle.com/pierpaolo28?source=post_page---------------------------)

### 参考文献

[1] 用作海滩清扫器的模块化机器人，Felippe Roza。Researchgate。访问链接：[https://www.researchgate.net/figure/RGB-left-and-HSV-right-color-spaces_fig1_310474598](https://www.researchgate.net/figure/RGB-left-and-HSV-right-color-spaces_fig1_310474598)

[2] OpenCV 中的视觉词袋，Vision & Graphics Group*。*Jan Kundrac。访问链接：[https://vgg.fiit.stuba.sk/2015-02/bag-of-visual-words-in-opencv/](https://vgg.fiit.stuba.sk/2015-02/bag-of-visual-words-in-opencv/)

[3] 深度学习与传统计算机视觉的比较。Haritha Thilakarathne，NaadiSpeaks。访问链接：[https://naadispeaks.wordpress.com/2018/08/12/deep-learning-vs-traditional-computer-vision/](https://naadispeaks.wordpress.com/2018/08/12/deep-learning-vs-traditional-computer-vision/)

**个人简介： [Pier Paolo Ippolito](https://www.linkedin.com/in/pierpaolo28/)** 是一位数据科学家，拥有南安普顿大学人工智能硕士学位。他对人工智能进展和机器学习应用（如金融和医学）有浓厚兴趣。可以通过 [Linkedin](https://www.linkedin.com/in/pierpaolo28/) 联系他。

[原文](https://towardsdatascience.com/roadmap-to-computer-vision-79106beb8be4)。经授权转载。

**相关：**

+   [自然语言处理（NLP）路线图](/2020/10/roadmap-natural-language-processing-nlp.html)

+   [加速计算机视觉：亚马逊提供的免费课程](/2020/08/accelerated-computer-vision-free-course-amazon.html)

+   [计算机视觉食谱：最佳实践和示例](/2020/09/computer-vision-recipes-best-practices-examples.html)

### 更多相关内容

+   [TensorFlow用于计算机视觉 - 转移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [发现计算机视觉的世界：介绍MLM最新的…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的5个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [你需要了解的数据管理的6个要点及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [KDnuggets 新闻 2022年3月9日：在5分钟内构建机器学习网页应用](https://www.kdnuggets.com/2022/n10.html)

+   [DINOv2：Meta AI的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)
