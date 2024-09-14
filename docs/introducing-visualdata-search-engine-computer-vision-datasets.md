# 介绍VisualData：一个计算机视觉数据集的搜索引擎

> 原文：[https://www.kdnuggets.com/2018/09/introducing-visualdata-search-engine-computer-vision-datasets.html](https://www.kdnuggets.com/2018/09/introducing-visualdata-search-engine-computer-vision-datasets.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Jie Feng](http://www.jiefeng.org)，人性化AI**

[![图片](../Images/cc9b2be1d91a31e749bc31423776af4b.png)](http://www.visualdata.io/)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供IT支持

* * *

计算机视觉无疑将在不久的将来改变机器与我们及环境互动的几乎所有方面。虽然这一领域仍然年轻（起源于1960年代），但得益于机器学习的最新进展，技术首次在历史上开始发挥作用。自动驾驶汽车、机器人技术、虚拟现实/增强现实等应用激励了人们进入这一领域，并将技术应用于更广泛的领域。它已经成为人工智能最[活跃](https://medium.com/iotforall/visual-data-and-the-killer-app-for-ai-5cfaf71db66a)的子领域之一。

关于计算机视觉的疯狂想法每天都在发生，涉及新的研究、边项目或业务用例。要从事计算机视觉工作，你需要三样东西：数据、算法和计算。像谷歌和亚马逊这样的巨头公司通过其云平台提供了强大的计算能力。研究界（例如arXiv）和开源社区（例如Github）为我们带来了更好的算法和易于使用的代码。现在学习这一复杂的主题是最佳时机，而这一主题曾经需要博士级别的经验。但仍然存在一个问题——数据。更具体地说，就是具有高质量注释的视觉数据。构建一个良好的数据集需要在规模、内容平衡、视觉变化等方面进行大量思考，以帮助训练的模型在未见过的案例中表现良好。标注大规模数据集可能非常昂贵且缓慢，这使得预算有限的个人或小公司仍然面临实验新想法的障碍。

解决这个问题的答案是开源数据集。与其自己构建数据集，不如利用已经由学术研究者、爱好者和公司贡献的丰富计算机视觉数据集。这些数据集涵盖了从识别对象到重建 3D 房间，从在视频中找人到识别照片中的衬衫等多种主题。它们通常附带人工标注的高质量标签。近年来，新的数据集甚至包括了预训练的深度学习模型，你可以直接尝试，而无需自己训练。然而，发现这些数据集并不容易。它们通常分散在创作者的网站上，很难比较不同的数据集以选择适合特定任务的最佳数据集。对于新手来说，也很难找出合适的关键字来进行 Google 搜索。

这就是为什么我们构建了[**VisualData**](http://www.visualdata.io/)，这是一个计算机视觉数据集的搜索引擎。目前，每个数据集都是人工筛选的，并标记了相关主题以便过滤，你可以在搜索栏中输入关键字，这些关键字将与数据集的标题和描述匹配，以便轻松找到最佳的使用数据集。它们按照发布时间排序，因此最新添加的数据集会被优先显示。详细视图显示了数据集的完整描述，并链接到开源代码/模型。将来还会逐步添加其他有用的信息，例如关于数据集的文章/教程、使用难度等。下面展示了一些网站的示例截图。

![Image](../Images/9f148fefd01d8ccd97bff4182bc43310.png)

从左到右：按（多个）主题过滤，按关键字搜索，数据集详细信息。

这是我希望在攻读博士学位时能存在的网站。现在，我希望它能帮助对计算机视觉感兴趣的人轻松探索现有数据，从而更快地进行实验。它仍处于早期阶段，未来版本中还有一些很酷的想法。我们希望了解它如何对你更有用。

附言：喜欢这个 Facebook [页面](https://www.facebook.com/visualdataio/) 或关注我们的 [Twitter](https://www.twitter.com/visualdataio) 以获取新数据集或代码发布的更新，并提供反馈和功能建议。

**简介：[Jie Feng](https://twitter.com/flyfengjie)** 拥有哥伦比亚大学计算机科学博士学位，并且是 VisualData 的创始人。

[原文](https://medium.com/@jiefeng/introducing-visualdata-eaadc87b4afc)。已获许可转载。

**相关：**

+   [宣布微软研究开源数据，云托管平台用于共享数据集](/2018/06/microsoft-research-open-data.html)

+   [DIY 深度学习项目](/2018/06/diy-deep-learning-projects.html)

+   [使用 Numpy 和 OpenCV 进行基本图像数据分析 – 第 1 部分](/2018/07/basic-image-data-analysis-numpy-opencv-p1.html)

### 相关话题

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [探索计算机视觉的世界：介绍MLM最新的……](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [通过Uplimit的机器学习搜索课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第1部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [Qdrant：开源向量搜索引擎与托管云平台](https://www.kdnuggets.com/2023/02/qdrant-open-source-vector-search-engine-managed-cloud-platform.html)

+   [TensorFlow用于计算机视觉 - 轻松实现迁移学习](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)
