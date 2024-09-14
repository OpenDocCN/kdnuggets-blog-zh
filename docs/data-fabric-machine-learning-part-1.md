# 机器学习的数据织物——第1部分

> 原文：[https://www.kdnuggets.com/2019/05/data-fabric-machine-learning-part-1.html](https://www.kdnuggets.com/2019/05/data-fabric-machine-learning-part-1.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![figure-name](../Images/3b6a008a65ca824b4b8855e0ad9ed9ba.png) 图片由[Héizel Vázquez](https://www.instagram.com/heizelvazquez/)提供

阅读第1-b部分：关于数据织物的深度学习：

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

[**机器学习的数据织物。第1-b部分：图上的深度学习。**

*图上的深度学习日益重要。在这里，我将展示机器学习的基本思路……*towardsdatascience.com](https://towardsdatascience.com/the-data-fabric-for-machine-learning-part-1-b-deep-learning-on-graphs-309316774fe7)

### 介绍

如果你在线搜索机器学习，你会发现大约2,050,000,000个结果。是的，真的。很难找到适合所有用途或案例的描述或定义，但有一些很棒的定义。在这里，我将提出一种不同的机器学习定义，着重于一种新的范式——数据织物。

### 目标

**概述**

> 解释数据织物与机器学习的关系。

**具体内容**

+   给出数据织物及其创建生态系统的描述。

+   用几句话解释什么是机器学习。

+   提出一种在数据织物中可视化机器学习洞察的方法。

### 主要理论

如果我们能构建一个支持公司所有数据的**数据织物**，那么在其中获得的**商业洞察**可以被看作是一个**凹痕**。发现这个洞察的**自动过程**被称为**机器学习**。

### 第1节：什么是数据织物？

![figure-name](../Images/81df123d0740e0031bfb34ac77acdce7.png)

我曾[讨论过](https://towardsdatascience.com/deep-learning-for-the-masses-and-the-semantic-layer-f1db5e3ab94b)数据织物，并给出了一个定义（我会在下面再次提供）。

在谈论数据织物时，我们应提到几个词汇：图谱、知识图谱、本体论、语义、链接数据。如果你想要这些定义，请阅读上面的文章；然后我们可以说：

> *数据架构是支持公司所有数据的平台。它如何被管理、描述、组合和普遍访问。这个平台由企业知识图谱构成，以创建一个统一的一致的数据环境。*

让我们把这个定义拆分开来。首先我们需要的是一个**知识图谱**。

知识图谱由整合的数据和信息集合组成，这些集合还包含大量不同数据之间的链接。关键在于，在这个新模型下，**我们在寻找一个答案**。我们想要的是事实——这些事实的来源并不重要。这里的数据可以表示概念、对象、事物、人物，实际上可以是任何你能想到的东西。图谱填充了这些概念之间的关系和连接。

知识图谱还允许你为图中的关系创建结构。通过它，可以建立一个框架来研究数据及其与其他数据的关系（[记得本体论吗？](https://towardsdatascience.com/ontology-and-data-science-45e916288cc5)）。

在这种情况下，我们可以向我们的数据湖提出这个问题：

> 这里有什么？

数据湖的概念也很重要，因为我们需要一个地方来存储我们的数据、管理它并运行我们的任务。但我们需要一个智能数据湖，一个了解我们拥有什么及如何使用它的地方，这就是拥有数据架构的一个好处。

数据架构应该是统一和一致的，这意味着我们应该努力将组织中的所有数据集中到一个地方，并真正管理和治理这些数据。

### 第2节：什么是机器学习？

![figure-name](../Images/17a990317b08ef4cc5d50d958c65faeb.png)[http://www.cognub.com/index.php/cognitive-platform/](http://www.cognub.com/index.php/cognitive-platform/)

机器学习已经存在了一段时间了。关于它有很多很好的描述、书籍、文章和博客，所以我不会用10段文字来让你感到乏味。

我只是想澄清一些要点。

> **机器学习不是魔法**。
> 
> 机器学习是数据科学工作流的一部分。但不是全部。
> 
> 机器学习需要数据来存在。至少现在是这样。

好的，之后，让我给出一个稍微借用并个性化的机器学习定义：

> 机器学习是通过使用能够提取数据模式的算法，自动理解数据及其表示中的模式，而不需要专门为此编程，从而创建解决特定（或多个）问题的模型。

你可以同意这个定义，也可以不同意，目前文献中有很多很好的定义，我只是认为这个定义简单且对我想表达的内容很有用。

### 第3节：在数据架构中进行机器学习

![figure-nme](../Images/ab0e5489a97a3e332021baafdaab2d3a.png)

在爱因斯坦的引力理论（广义相对论）中，他在数学上提出了质量可以扭曲时空，而这种扭曲就是我们理解的引力。我知道如果你不熟悉这个理论，它可能听起来有些奇怪。让我试着解释一下。

在特殊相对论的“平坦”时空中，引力缺席，力学定律呈现出特别简单的形式：只要没有外力作用于物体，它将以恒定速度沿直线在时空中运动（牛顿的第一运动定律）。

但当我们有质量和加速度时，我们可以说我们在引力的作用下。正如惠勒所说：

> 时空告诉物质如何移动；物质告诉时空如何弯曲。

![figure-name](../Images/8b0ae7f43da6f6ce68f73d1657953ee1.png)

在上面的图像中，“立方体”是对时空结构的表示，当质量在其中移动时，它会扭曲它，“线条”的移动方式会告诉我们附近的物体如何在那个附近表现。所以引力就像是：

所以当我们有质量时，我们可以在时空中制造一个“凹痕”，在接近这个凹痕时，我们看到的就是引力。我们必须足够接近这个物体才能感受到它。

这正是我提出的机器学习在数据结构中的作用。我知道我听起来很疯狂。让我解释一下。

假设我们已经创建了一个数据结构。对我来说，最好的工具是Anzo，正如我在其他文章中提到的。

![figure-name](../Images/bb9ab66b19dc0f7afdfbf08d331a2ed9.png)[https://www.cambridgesemantics.com/](https://www.cambridgesemantics.com/)

你可以用Anzo构建一个叫做“企业知识图谱”的东西，当然也可以创建你的数据结构。

图谱的节点和边灵活地捕捉到每个数据源的高分辨率副本——无论是结构化还是非结构化。图谱可以帮助用户快速、互动地回答任何问题，让用户与数据对话以发现**洞察力**。

顺便说一下，这就是我对洞察力的描绘方式：

![figure-name](../Images/f8af3a69f5bf7b33f01be6398252f33c.png)图片来源：[Héizel Vázquez](https://www.instagram.com/heizelvazquez/)

如果我们有数据结构：

![figure-name](../Images/31d9ffa90316974f9ee189b06a006976.png)图片来源：[Héizel Vázquez](https://www.instagram.com/heizelvazquez/)

我提议的观点是，洞察力可以被认为是数据结构中的一个**凹痕**。发现这个洞察力的自动过程就是机器学习。

![figure-name](../Images/4e8419430956a0a2dbe8e15a8985029e.png)图片来源：[Héizel Vázquez](https://www.instagram.com/heizelvazquez/)

所以现在我们可以说：

> 机器学习是使用能够发现这些洞察力的算法的自动过程，这些算法没有被特别编程为此，以创建解决特定（或多个）问题的模型。

通过数据架构生成的洞察本身就是新数据，这些数据作为数据架构的一部分变得显而易见。也就是说，洞察可以扩展图谱，可能会产生进一步的洞察。

在数据架构中，我们面临一个问题，即尝试发现数据中的隐藏洞察，然后通过机器学习来发现它们。这在现实生活中会是什么样子？

[Cambridge Semantics](https://www.cambridgesemantics.com/) 的团队也有答案。Anzo for Machine Learning 解决方案用现代数据平台替代了这种繁琐且易出错的工作，旨在快速集成、协调和转换来自所有相关数据源的数据，形成优化的机器学习准备特征数据集。

数据架构提供了先进的数据转换功能，这对快速有效的特征工程至关重要，有助于从无关的噪声中分离出关键业务信号。

记住，**数据优先**，这一新范式通过内置的图数据库和语义数据层集成并协调所有相关的数据源——无论是结构化数据还是非结构化数据。数据架构传达了数据的业务背景和含义，使业务用户更容易理解和正确利用数据。

可重复性对数据科学和机器学习非常重要，因此我们需要一种简单的方法来重用协调的结构化和非结构化数据，通过管理数据集目录以及数据集成的持续方面，如数据质量处理，这正是数据架构所提供的功能。它还保留了机器学习数据集的数据端到端传承和来源，使得在生产中使用模型时容易找出所需的数据转换。

在接下来的文章中，我将给出一个在这个新框架下进行机器学习的具体示例。

### 结论

机器学习并不新颖，但有一种新的范式来实现它，也许它是该领域的未来（我真是太乐观了）。在数据架构中，我们有像本体、语义、层次、知识图谱等新概念；但所有这些都可以改善我们对机器学习的思考方式和实践。

在这个范式中，我们通过使用能够发现这些洞察而不需特别编程的算法，来在数据架构中发现隐藏的洞察，以创建解决特定（或多个）问题的模型。

感谢 [Ciencia y Datos](http://www.cienciaydatos.org) 的优秀团队对这篇文章的帮助。

感谢你阅读这篇文章。希望你在这里发现了一些有趣的内容 :)。如果这些文章对你有帮助，请与朋友分享！

如果你有问题，欢迎在 Twitter 上关注我：

[**Favio Vázquez (@FavioVaz) | Twitter**

*来自 Favio Vázquez (@FavioVaz) 的最新推文。数据科学家，物理学家和计算工程师。我有一个…*twitter.com](https://twitter.com/faviovaz)

和 LinkedIn：

[**Favio Vázquez — 创始人 — Ciencia y Datos | LinkedIn**

*查看 Favio Vázquez 在 LinkedIn 上的个人资料，这是世界上最大的职业社区。Favio 有 16 个职位在列…*www.linkedin.com](https://www.linkedin.com/in/faviovazquez/)

再见 :)

**简介：[Favio Vazquez](https://www.linkedin.com/in/faviovazquez/)** 是一位物理学家和计算机工程师，专注于数据科学和计算宇宙学。他对科学、哲学、编程和音乐充满热情。他是 Ciencia y Datos 的创始人，这是一本用西班牙语出版的数据科学刊物。他喜欢迎接新挑战，和优秀的团队合作，以及解决有趣的问题。他是 Apache Spark 合作项目的一部分，参与 MLlib、Core 和文档的工作。他热衷于应用他的科学、数据分析、可视化和自动学习知识与专长，以帮助世界变得更美好。

[原文](https://towardsdatascience.com/the-data-fabric-for-machine-learning-part-1-2c558b7035d7)。经许可转载。

**相关:**

+   [我对敏捷数据科学研究的最佳建议](/2019/03/best-tips-agile-data-science-research.html)

+   [如何实时监控机器学习模型](/2019/01/monitor-machine-learning-real-time.html)

+   [人工智能/机器学习进展的年度回顾：Xavier Amatriain 2018 总结](/2019/01/xamat-ai-machine-learning-roundup.html)

### 更多相关内容

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每位初学数据科学者都应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学来寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
