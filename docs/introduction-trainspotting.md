# Trainspotting 简介：计算机视觉、Caltrain 和预测分析

> 原文：[`www.kdnuggets.com/2016/11/introduction-trainspotting.html`](https://www.kdnuggets.com/2016/11/introduction-trainspotting.html)

**作者：Chloe Mawer、Colin Higgins 和 Matthew Rubashkin，[硅谷数据科学](http://svds.com/)**。

在硅谷数据科学，我们对 Caltrain 有些许痴迷。我们的兴趣源于我们的一半员工每天依赖 Caltrain 上班。我们也希望回馈社区，我们喜欢用数据来做到这一点。除了帮助客户[构建强大的数据系统](http://www.svds.com/building-data-systems-what-do-you-need/?utm_source=kdnuggets&utm_medium=referral)或[利用数据解决业务挑战](http://www.svds.com/five-business-challenges-data-can-solve/?utm_source=kdnuggets&utm_medium=referral)，我们还喜欢从事研发项目，探索新技术，尝试新的算法、假设和创意。我们之前[分析过延迟](http://www.svds.com/the-trains-project-analyzing-caltrain-delays/?utm_source=kdnuggets&utm_medium=referral)使用 Caltrain 的实时 API 来改进到达预测，并且我们[建模过声音](http://www.svds.com/listening-caltrain/?utm_source=kdnuggets&utm_medium=referral)以区分不同的火车。在这篇文章中，我们将开始了解实现 Caltrain 功能的细节。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求

* * *

<http://www.svds.com/wp-content/uploads/2016/09/side-by-side.mp4?_=1>

如果你曾经乘坐过火车，你就会知道 Caltrain 提供的延迟估算有时会有些...不准确。有时一列火车会在原本已经应该离开的十分钟后仍然显示“延迟两分钟”，或者当火车准时到达时却报告延迟。Trainspotting 的想法来源于我们希望将新的数据源整合到延迟预测中，而不仅仅是抓取 Caltrain 的 API 数据。由于我们之前已经设置了一个 Raspberry Pi 来分析火车的鸣笛，我们觉得通过捕捉经过我们靠近 Mountain View 车站的办公室的实时视频和音频来验证来自 Caltrain API 的数据会很有趣。

我们希望我们的物联网 Raspberry Pi 火车检测器回答几个问题：

1.  是否有火车经过？

1.  火车的行进方向是哪个？

1.  火车移动的速度有多快？

单靠声音对第一个问题的回答效果还不错，因为火车的声音相当响亮。为了帮助回答其他问题，我们在 Raspberry Pi 上增加了一个摄像头来捕捉视频。

我们将通过一系列文章来描述这一过程。它们将重点介绍：

1.  Trainspotting 介绍（你在这里）

1.  [Python 中的图像处理](http://www.svds.com/image-processing-python/)

1.  [使用 Python 进行流媒体视频分析](http://www.svds.com/streaming-video-analysis-python/)

1.  流媒体音频分析与传感器融合

1.  在 Raspberry Pi 上识别图像

1.  将 IoT 设备连接到云端

1.  构建可部署的 IoT 设备

让我们快速了解一下这些内容将涵盖什么。

### 探索 Trainspotting

在即将发布的《Python 中的图像处理》文章中，数据科学家 Chloe Mawer 展示了如何使用开源 Python 库（如 OpenCV）来处理图像和视频，以检测火车及其方向。你还可以观看她最近在 [PyCon 2016](https://www.youtube.com/watch?v=MC00XWdl-ms) 的演讲。

在《使用 Python 进行流媒体视频分析》中，数据科学家 Colin Higgins 和数据工程师 Matt Rubashkin 描述了将视频分析提升到新水平的步骤：实现带有多线程的流媒体 Pi 视频分析，以及光/暗适应。下图展示了一些在不同光照条件下检测火车的挑战。

![检测火车在不同光照条件下的挑战](http://www.svds.com/wp-content/uploads/2016/09/Trainspotting_variedlight.png)

**在不同光照条件下检测火车的挑战**

在上面提到的上一篇文章《[监听 Caltrain](http://www.svds.com/listening-caltrain/)》中，我们分析了频率谱，以区分在我们 Sunnyvale 办公室经过的本地列车和快车。自那篇文章以来，SVDS 已经发展并迁移到了 Mountain View。自搬迁后，我们发现新地点的火车声音模式有所不同，因此我们需要一个更灵活的方法。在《流媒体音频分析与传感器融合》中，Colin 描述了音频处理和控制视频及音频的自定义传感器融合架构。

![](http://www.svds.com/wp-content/uploads/2016/09/Pasted-image-at-2016_09_09-12_01-PM.png)

在我们能够检测到火车、其速度和方向后，我们遇到了一个新问题：我们的 Pi 不仅检测到 Caltrains（真正的正例），还检测到 Union Pacific 的货运列车和 VTA 轻轨（虚假正例）。为了提高检测器的虚假正例率，我们使用了在 Google 的机器学习 [TensorFlow 库](https://www.tensorflow.org/versions/r0.9/tutorials/image_recognition/index.html) 中实现的卷积神经网络。我们实现了一个 [自定义的 Inception-V3 模型](https://www.tensorflow.org/versions/r0.8/how_tos/image_retraining/index.html)，并在数千张车辆图像上进行训练，以 >95% 的准确率识别不同类型的火车。Matt 在《在 Raspberry Pi 上识别图像》中详细介绍了这一解决方案。

![Trainspotting_tensorflow](http://www.svds.com/wp-content/uploads/2016/09/Trainspotting_tensorflow.png)

在《将 IoT 设备连接到云端》中，Matt 展示了我们如何使用 Kafka 将我们的 Pi 连接到云端，从而实现通过 Grafana 监控和在 HBase 中持久化。

![用 Grafana 监控我们的 Pi](http://www.svds.com/wp-content/uploads/2016/09/Trainspotting_grafana.png)

**用 Grafana 监控我们的 Pi**

### 工具和下一步计划

在我们完成第一个设备的开发之前，我们希望设置更多这样的设备，以便在轨道上的其他点获取实际数据。考虑到这一点，我们意识到不能总是保证有快速的互联网连接，并且我们希望保持设备本身的低成本。这些要求使得 Raspberry Pi 成为一个极好的选择。Pi 具有足够的计算能力来进行设备端流处理，因此我们可以通过互联网连接发送较小的处理数据流，而且组件便宜。我们为传感器的硬件总成本为 130 美元，并且代码仅依赖于开源库。在《构建可部署的 IoT 设备》中，我们将详细讲解设备硬件和设置，并展示代码的获取方式，帮助你开始自己的 Trainspotting。

![设备和硬件设置用品](http://www.svds.com/wp-content/uploads/2016/09/Trainspotting_supplies.png)

**设备和硬件设置用品**

如果你想了解更多关于 Trainspotting 和 SVDS 数据科学的内容，请关注我们未来的 Trainspotting 博客文章，并可以通过 [这里](http://www.svds.com/newsletter/?utm_source=kdnuggets&utm_medium=referral) 注册我们的新闻通讯。告诉我们你对这个系列中哪些内容最感兴趣。

你还可以在 [Android](https://play.google.com/store/apps/details?id=interprone.caltrain&hl=en) 和 [Apple](https://itunes.apple.com/us/app/caltrain-rider/id897315176?mt=8) 应用商店找到我们的“Caltrain Rider”。我们的应用程序基于包括 HBase 和 Spark 在内的 Hadoop 生态系统，并依赖 Kafka 和 Spark Streaming 进行 Twitter 情感和 Caltrain API 数据的摄取和处理。

**[克洛伊·毛尔](https://www.linkedin.com/in/chloemawer)** 背景涉及地球物理学和水文学，擅长利用数据进行预测和提供有价值的见解。她在学术研究和工程领域的经验使她能够应对新颖的问题，并创造切实有效的解决方案。

**[科林·希金斯](https://www.linkedin.com/in/colinahiggins)** 的背景是早期帕金森病药物发现，他利用高维生物物理数据集来建模动态蛋白质结构对药物选择性的影响。在加入 SVDS 之前，他为一家初创公司提供咨询，开发了一种基于自然语言处理的用户匹配算法。

**[马修·布巴什金](https://www.linkedin.com/in/mrubash1)** 背景涵盖光学物理和生物医学研究，并在软件开发、数据库工程和数据分析方面拥有广泛的经验。他喜欢与客户紧密合作，开发简单而强大的解决方案来解决困难的问题。

[原始链接](http://svds.com/introduction-to-trainspotting/?utm_source=kdnuggets&utm_medium=referral)。经许可转载。

**相关：**

+   理解计算机视觉的 7 个步骤

+   用深度学习预测未来人类行为

+   你不能再忽视的 5 个机器学习项目

### 更多相关话题

+   [数据管理的 6 件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [计算机视觉中的 TensorFlow - 简化迁移学习](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [发现计算机视觉的世界：介绍 MLM 最新的…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的 5 种应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [DINOv2: Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：在 5…中构建一个机器学习 Web 应用](https://www.kdnuggets.com/2022/n10.html)
