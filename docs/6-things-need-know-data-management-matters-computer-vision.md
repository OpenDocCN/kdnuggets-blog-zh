# 关于数据管理和其在计算机视觉中的重要性的6个事项

> 原文：[https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

![关于数据管理和其在计算机视觉中的重要性的6个事项](../Images/b0af84a5ae3b4ebc13f3260f0f327cdd.png)

照片由 [Nana Smirnova](https://unsplash.com/es/@nananadolgo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

向工业4.0迈进以及数字化自动化的加速普及正对各行业的组织施加压力。企业利用数据的能力正逐渐成为竞争优势的关键来源——这一原则在构建和维护计算机视觉应用时尤为重要。视觉自动化模型由捕捉我们物理世界数字化表现的图像和视频驱动。在大多数企业中，媒体通过多个传感器边缘设备进行捕捉，并存在于孤立的源系统中，这使得在构建旨在自动化视觉检查的计算机视觉系统时，跨环境集成媒体成为一个核心挑战，需要解决。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作。

* * *

在模型架构（即构建神经网络的代码）越来越商品化和稳定的世界中，媒体变得更加重要。这意味着有更多被行业信任的模型类型，现在关注点减少在调整这些模型的代码以优化性能，而更多地放在使用必要的数据训练行业标准模型，以最佳服务你的应用。在数据驱动的计算机视觉模型开发中，组织必须不断用新的真实数据训练模型，以防止[领域和数据漂移](https://machinelearningmastery.com/gentle-introduction-concept-drift-machine-learning/)。因此，无论是大型还是小型计算机视觉供应商，都必须确保组织能够以编程方式持续大规模整合媒体。接下来，我们将探讨在评估计算机视觉数据管理解决方案时，我们认为至关重要的几个领域。

# **预构建的数据连接器**

要管理数据，首先必须将所有数据集中到一个地方。你今天的数据可能分布在各种商业云环境、本地系统和边缘设备上。你需要的是一种简化连接所有硬件和软件的方法。预构建的连接可以使这些过程在几次点击之间完成，而不是几行代码。最好还有一个代码编辑器（Python SDK），以便在需要时构建自定义连接。总之，你不想每次需要集成新的媒体源时都去敲IT部门的门，这对每个人来说都很麻烦！

# **数据组织**

将来自不同来源的数据整合在一起可能是一项复杂的工作。为了快速找到所需的数据，组织结构如文件夹、桶或数据集将帮助你整理媒体。你可能会根据捕获日期来组织数据，或者你可能希望将由单一生产线捕获的所有数据放在同一个文件夹中。选择权在你，只要确保保持一致即可。

# **数据可视化**

现在你的所有数据都集中在一个地方，但如何快速筛选这些数据以找到感兴趣的媒体呢？这就需要一个使浏览大规模媒体变得简单的可视化工具。这可能是你认为理所当然的事情，但看到页面上快速刷新并动态出现的缩略图，或者能够快速浏览不需要漫长加载时间的图片轮播，都是今天组织在尝试使用像Google Cloud Platform这样的商业消费应用来存储大规模媒体时面临的问题。简而言之，你需要采用一个具有灵活且可扩展的后端的解决方案，该解决方案专为大数据构建，并让你以最小的加载时间快速浏览你的媒体。请记住，将所有这些数据集中在一个地方对你的计算机视觉模型的成功至关重要。

# **数据可搜索性**

那张图片到底在什么文件夹里？在处理数百万个媒体文件时，你不希望被这个问题困住。一个像Google那样的简单搜索栏将帮助你通过名称快速搜索并找到感兴趣的媒体。一些计算机视觉平台还允许你使用一种强大的工具——“[视觉搜索](https://www.clarifai.com/use-cases/visual-search)”，它会自动检测图像的内容并让你基于此进行搜索，使媒体更易发现。记住，维持大规模媒体的快速搜索性能是一个独特的问题，需要一个能够随数据集规模增加而扩展的系统。

# **元数据管理和过滤器**

说到大数据集中的媒体发现，我们来说说元数据的好处。元数据让你保存“关于数据的数据”，例如*这张图片是10月4日由Antigua工厂的Linespex相机拍摄的*，比仅仅图片的文件名提供了更多关于媒体的信息。而且，一些数据管理平台允许你基于元数据创建过滤器，让你更细致地找到感兴趣的媒体。

# **数据溯源**

这个文件来源于哪里？完整的数据溯源将帮助你轻松回答这个问题，让你了解你的媒体来源，而无需猜测或追踪源系统。数据溯源意味着你将知道哪些媒体来自哪个系统，以及该系统向平台发送新媒体的频率。大多数组织会利用这些信息根据运营需求选择新的训练数据。例如，如果检测生产线7上的缺陷的模型表现下降，我们首先尝试获取一些来自监控该生产线的相机的新媒体。

将计算机视觉模型从研发转移到生产的关键在于更好地管理企业媒体。这对公司至关重要，以便利用数据获得竞争优势。为成功做好准备意味着在选择计算机视觉合作伙伴时，你需要认真对待数据管理。

**[穆尔塔扎·霍穆斯](https://www.linkedin.com/in/murtaza-khomusi-2549b475/)** 是数据和 AI 领域经验丰富的市场推广领导者。他从 Palantir 等大型组织的成功初创到上市转型中积累了经验，最近还在系列 A 计算机视觉初创公司 CrowdAI 担任产品营销负责人。他本质上是一个建设者，非常专注于与业务里程碑和日常执行对齐的深思熟虑的战略。他坚信 Drucker 的话：“文化吃掉战略”以及“如果没有写下来，它就不存在。” 他在数字化转型、云迁移、AI、系统集成、决策数据、分析、计算机视觉、网络安全和 SaaS 市场方面拥有技术专长。

### 了解更多关于这个话题

+   [为什么数据可视化的技能提升很重要（及如何开始）](https://www.kdnuggets.com/2022/07/sphere-upskilling-data-vis-matters.html)

+   [超越编码：为什么人性化的触感很重要](https://www.kdnuggets.com/beyond-coding-why-the-human-touch-matters)

+   [构建 LLM 应用程序时需要了解的 5 件事](https://www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html)

+   [7 种你不知道可以用低代码工具实现的功能](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [TensorFlow 用于计算机视觉——转移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [DINOv2：Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)
