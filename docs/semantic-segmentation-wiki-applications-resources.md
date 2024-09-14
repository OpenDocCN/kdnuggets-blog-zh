# 语义分割：维基百科、应用和资源

> 原文：[`www.kdnuggets.com/2018/10/semantic-segmentation-wiki-applications-resources.html`](https://www.kdnuggets.com/2018/10/semantic-segmentation-wiki-applications-resources.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Prerak Mody](https://www.linkedin.com/in/prerakmody/?originalSubdomain=in)提供**。

近年来，以深度学习为核心的机器学习技术引起了关注。自动驾驶汽车已经融入了需要算法从输入的图像中识别和学习的深度学习过程。让我们探讨一下语义分割需求的演变。

计算机视觉的最初应用需要识别基本元素，如边缘（线条和曲线）或梯度。然而，像素级别的图像理解只有在**全像素语义分割**这一术语出现后才出现。它将属于同一目标的图像部分进行聚类，从而开启了众多应用的大门。

识别每个像素或将像素分组并分配 classID 的过程经历了以下步骤：

1.  **图像分类**——识别图像中存在的内容

1.  **对象识别（及检测）**——识别图像中存在的内容及其位置（通过边界框）

1.  **语义分割**——识别图像中存在的内容及其位置（通过找到所有属于该内容的像素）

所以，让我们来看看……

### 什么是语义分割？

语义分割是一个经典的计算机视觉问题，它涉及将某些原始数据（例如，2D 图像）作为输入，并将其转换为突出显示感兴趣区域的掩码。许多人使用全像素语义分割这一术语，其中图像中的每个像素都根据它属于哪个目标分配一个 classID。

早期的计算机视觉问题只找到像边缘（线条和曲线）或梯度这样的元素，但它们从未能像人类一样在像素级别上理解图像。语义分割将属于同一目标的图像部分进行聚类，解决了这个问题，因此在许多领域得到了应用。

请注意，语义分割与其他基于图像的任务相比，确实有很大不同和进步，例如，

+   **图像分类** 识别图像中存在的内容。

+   **对象识别（及检测）** 识别图像中存在的内容及其位置（通过边界框）。

+   **语义分割** 识别图像中存在的内容及其位置（通过找到所有属于该内容的像素）。

你的机器学习模型是否需要识别输入 2D 原始图像中的每一个像素？在这种情况下，全像素语义分割注释是你机器学习模型的关键。全像素语义分割将图像中的每个像素分配一个 classID，取决于它属于哪个感兴趣的对象。

那么我们就定义一下语义分割的类型，以更好地理解其概念。

#### 语义分割的类型

1.  **标准语义分割**也称为全像素语义分割。它是将每个像素分类为属于某一对象类别的过程。

1.  **实例感知语义分割**是标准语义分割或全像素语义分割的一个子类型。它不仅将每个像素分类为属于某个对象类别，还为该类别分配实体 ID。

让我们探索语义分割的一些应用领域，以更好地理解这种过程的需求。

#### 语义分割的特性

为了理解图像分割的特性，我们也来看看其他常见的图像分类技术。

这次，我将介绍这三种方法，包括图像分割。

*1) 图像分类……“识别图像是什么”*

*2) 图像检测（识别）“识别图像中的位置”*

*3) 图像分割……“识别意义”*

1.  **图像分类**围绕着识别图像是什么的概念。例如，分类的寿司故事图像被逐一分类为“这是三文鱼，这个多少钱，这个很硬”。[Amazon Rekognition](https://aws.amazon.com/jp/rekognition/)最近从[Amazon](https://aws.amazon.com/jp/rekognition/)发布的对象和场景检测也属于这种图像分类。最初反映的有“杯子、智能手机和瓶子”，但 Amazon Rekognition 现在将整个图像标记为“杯子”和“咖啡杯”。这样，它在多个对象进入图像的场景中无法使用。在这种情况下，你将使用“图像检测（检测）”。

1.  **图像检测**围绕着识别“有什么”以及“它在哪里”的概念。

1.  **图像分割**围绕着识别图像区域的概念。图像分割被称为语义分割，它为每个像素标记该像素所表示的意义，而不是检测整个图像或图像的一部分。既然查看图像更容易，我们来看看实际的图像。

#### 语义分割的应用

1.  **GeoSensing – 用于土地使用**

![](img/847e8e9fa1abe95c05e85883c08b3b74.png)

语义分割问题也可以视为分类问题，其中每个像素被分类为一系列对象类别中的一个。因此，卫星影像的土地使用映射有其应用场景。土地覆盖信息对于各种应用非常重要，例如监测森林砍伐和城市化区域。

要识别卫星图像中每个像素的土地覆盖类型（例如，城市、农业、水域等），土地覆盖分类可以视为多类别的语义分割任务。道路和建筑物检测也是交通管理、城市规划和道路监控的重要研究课题。

公开可用的大规模数据集较少（例如：SpaceNet），而数据标注始终是分割任务的瓶颈。

1.  **用于自动驾驶**

自动驾驶是一项复杂的机器人任务，需要在不断变化的环境中进行感知、规划和执行。由于安全至关重要，这项任务还需要以极高的精确度完成。语义分割提供关于道路上空闲空间的信息，并检测车道标记和交通标志。

![](img/21f784e02f8b0b11db8d69c726c4ff40.png)

1.  **用于人脸分割**

![](img/ca963834b903c564aa8061ef5402c5e0.png)

人脸的语义分割通常涉及皮肤、头发、眼睛、鼻子、嘴巴和背景等类别。人脸分割在许多计算机视觉的面部应用中非常有用，例如性别、表情、年龄和种族的估计。影响人脸分割数据集和模型开发的显著因素包括光照条件的变化、面部表情、面部朝向、遮挡和图像分辨率。

1.  **时尚 – 服装项分类**

![](img/407237c4c0b8ce3a19460bea7bedd7f4.png)

与其他任务相比，服装解析是一项非常复杂的任务，因为它涉及大量类别。这与一般的对象或场景分割问题不同，因为细粒度的服装分类需要基于服装语义、人类姿态的变异性以及可能的大量类别进行更高层次的判断。服装解析在视觉领域得到了积极研究，因为它在现实世界应用中具有巨大的价值，例如电子商务。一些数据集如 Fashionista 和 CFPD 数据集提供了对服装项语义分割的开放访问。

1.  **精准农业**

![](img/713658db11285d11b476492331a576a6.png)

精准农业机器人可以减少需要在田地中喷洒的除草剂量，作物和杂草的语义分割实时协助它们触发除草行动。这些先进的农业图像视觉技术可以减少对农业的人工监控。

[原文](https://blog.playment.io/semantic-segmentation/). 已获许可转载。

**简介**：[Prerak Mody](https://www.linkedin.com/in/prerakmody/?originalSubdomain=in) 是 [Playment](https://playment.io/) 的计算机视觉研究员。Prerak 喜欢探索数据分析如何用于改善城市的市政设施，阅读世界金融相关内容，并且目前对强化学习领域非常感兴趣。

**相关内容:**

+   [使用 YOLO 进行目标检测和图像分类](https://www.kdnuggets.com/2018/09/object-detection-image-classification-yolo.html)

+   [边界框的数据增强：重新思考图像变换用于目标检测](https://www.kdnuggets.com/2018/09/data-augmentation-bounding-boxes-image-transforms.html)

+   [使用 Tensorflow 对象检测和 OpenCV 分析足球比赛](https://www.kdnuggets.com/2018/07/analyze-soccer-game-using-tensorflow-object-detection-opencv.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关主题

+   [关于语义分割注释的误解](https://www.kdnuggets.com/2022/01/misconceptions-semantic-segmentation-annotation.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [使用向量数据库的语义搜索](https://www.kdnuggets.com/semantic-search-with-vector-databases)

+   [语义层的力量：数据工程师指南](https://www.kdnuggets.com/2023/10/cube-power-of-a-semantic-layer-a-data-engineers-guide)

+   [语义层：AI 驱动数据体验的支柱](https://www.kdnuggets.com/2023/10/cube-semantic-layer-backbone-aipowered-data-experiences)

+   [为什么通用语义层对您的数据堆栈有益的 6 个理由](https://www.kdnuggets.com/2024/01/cube-6-reasons-why-a-universal-semantic-layer-is-beneficial)
