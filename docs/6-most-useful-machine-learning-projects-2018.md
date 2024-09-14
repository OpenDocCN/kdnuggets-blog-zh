# 2018 年最有用的 6 个机器学习项目

> 原文：[https://www.kdnuggets.com/2019/01/6-most-useful-machine-learning-projects-2018.html](https://www.kdnuggets.com/2019/01/6-most-useful-machine-learning-projects-2018.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

过去一年对于人工智能和机器学习来说是辉煌的一年。许多新的高影响力的机器学习应用被发现并公开，特别是在医疗保健、金融、语音识别、增强现实以及更复杂的 3D 和视频应用中。

我们已经看到更多的应用驱动型研究推动了进展，而不是理论研究。尽管这可能有其缺点，但目前它产生了一些巨大的积极影响，生成了可以迅速转化为业务和客户价值的新研发。这一趋势在许多机器学习开源工作中得到了强烈反映。

让我们来看看过去一年中最具实际用途的 6 个机器学习项目。这些项目发布了代码和数据集，使个人开发者和小团队能够学习并立即创造价值。它们可能不是最具理论突破性的作品，但它们是适用和实用的。

### [Fast.ai](https://github.com/fastai/fastai?utm_source=mybridge&utm_medium=blog&utm_campaign=read_more)

Fast.ai 库旨在简化使用现代最佳实践训练快速准确的神经网络。它抽象化了实现深度神经网络时可能遇到的所有细节工作。使用起来非常简单，并且以实践者构建应用的思维方式进行设计。最初为 Fast.ai 课程的学生创建，该库基于易于使用的 Pytorch 库，以干净简洁的方式编写。他们的 [文档](https://docs.fast.ai/) 也非常出色。

![](../Images/9f777ed633133bd3290e49058b7153af.png)

### [Detectron](https://github.com/facebookresearch/Detectron?utm_source=mybridge&utm_medium=blog&utm_campaign=read_more)

Detectron 是 Facebook AI 的物体检测和实例分割研究平台，编写在 Caffe2 上。它包含各种物体检测算法的实现，包括：

+   [Mask R-CNN](https://arxiv.org/abs/1703.06870)：使用 Faster R-CNN 结构进行物体检测和实例分割

+   [RetinaNet](https://arxiv.org/abs/1708.02002)：一种基于特征金字塔的网络，具有独特的 Focal Loss 用于处理困难示例。

+   [Faster R-CNN](https://arxiv.org/abs/1506.01497)：最常见的物体检测网络结构

所有网络都可以使用几种可选的分类骨干网之一：

+   [ResNeXt{50,101,152}](https://arxiv.org/abs/1611.05431)

+   [ResNet{50,101,152}](https://arxiv.org/abs/1512.03385)

+   [Feature Pyramid Networks](https://arxiv.org/abs/1612.03144)（与 ResNet/ResNeXt）

+   [VGG16](https://arxiv.org/abs/1409.1556)

更进一步，这些都配备了在COCO数据集上预训练的模型，因此你可以直接使用它们！它们都已经使用标准评估指标在 [Detectron模型库](https://github.com/facebookresearch/Detectron/blob/master/MODEL_ZOO.md)中进行了测试。

![](../Images/384ebf004af6861afb559596f3fedbe6.png)

Detectron

### FastText

另一个来自Facebook研究的[fastText](https://github.com/facebookresearch/fastText)库旨在文本表示和分类。它提供了150多种语言的预训练词向量。这些词向量可以用于许多任务，包括文本分类、摘要和翻译。

![](../Images/def5ca3b4f9db89f329cd79fd0dfc92a.png)

### [AutoKeras](https://github.com/jhfjhfj1/autokeras?utm_source=mybridge&utm_medium=blog&utm_campaign=read_more)

Auto-Keras是一个用于自动化机器学习（AutoML）的开源软件库。它由[DATA Lab](http://faculty.cs.tamu.edu/xiahu/index.html)在德州A&M大学和社区贡献者开发。AutoML的终极目标是为数据科学或机器学习背景有限的领域专家提供易于访问的深度学习工具。Auto-Keras提供了自动搜索最佳架构和超参数的功能。

![](../Images/dc610d62dc5927a9c2264b46309f1c2f.png)

### [Dopamine](https://github.com/google/dopamine?utm_source=mybridge&utm_medium=blog&utm_campaign=read_more)

多巴胺是由谷歌创建的一个用于快速原型开发强化学习算法的研究框架。它旨在灵活且易于使用，实现标准的RL算法、指标和基准。

根据Dopamine的文档，他们的设计原则是：

+   *简单实验*：帮助新用户进行基准实验

+   *灵活开发*：促进新用户生成新的创新想法

+   *紧凑可靠*：提供一些较旧和更流行的算法的实现

+   *可重复性*：确保结果是可重复的

![](../Images/56ac7bf0cb184299ca352e70a711f6fc.png)

### [vid2vid](https://github.com/NVIDIA/vid2vid?utm_source=mybridge&utm_medium=blog&utm_campaign=read_more)

vid2vid项目是Nvidia最先进的视频到视频合成算法的公开Pytorch实现。视频到视频合成的目标是学习一个映射函数，将输入源视频（例如，语义分割掩膜序列）映射到一个精准描绘源视频内容的输出逼真视频。

这个库的优点在于其选项：它提供了几种不同的vid2vid应用，包括自动驾驶/城市场景、面部和人体姿势。它还附带了包括数据集加载、任务评估、训练功能和多GPU支持等广泛的说明和功能！

![](../Images/5d5748f4f133307d38e634497a00435f.png)

将分割图转换为真实图像

### 荣誉提名

+   [**ChatterBot:**](https://github.com/gunthercox/ChatterBot) 用于会话对话引擎和创建聊天机器人机器学习

+   [**Kubeflow:**](https://github.com/kubeflow/kubeflow) Kubernetes上的机器学习工具包

+   [**imgaug:**](https://github.com/aleju/imgaug) 深度学习中的图像增强

+   [**imbalanced-learn:**](https://github.com/scikit-learn-contrib/imbalanced-learn) 专门针对不平衡数据集的scikit-learn下的Python包

+   [**mlflow:**](https://github.com/mlflow/mlflow) 开源平台，用于管理机器学习生命周期，包括实验、可重复性和部署。

+   [**AirSim:**](https://github.com/Microsoft/AirSim) 微软AI与研究部门开发的基于Unreal Engine / Unity的自动驾驶模拟器

### 喜欢学习吗？

在[twitter](https://twitter.com/GeorgeSeif94)上关注我，了解最新最棒的AI、科技和科学动态！

**简历： [George Seif](https://towardsdatascience.com/@george.seif94)** 是认证的极客和AI / 机器学习工程师。

[原文](https://towardsdatascience.com/the-10-most-useful-machine-learning-projects-of-the-past-year-2018-5378bbd4919f)。转载自原作者授权。

**相关：**

+   [2018年数据科学、深度学习、机器学习中的顶级Python库](/2018/12/top-python-libraries-2018.html)

+   [5个快速简便的Python数据可视化及代码](/2018/07/5-quick-easy-data-visualizations-python-code.html)

+   [数据科学家需要知道的5种聚类算法](/2018/06/5-clustering-algorithms-data-scientists-need-know.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行IT管理

* * *

### 更多相关话题

+   [4个对数据科学有用的中级SQL查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [3个有用的Python自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [KDnuggets 新闻，12月7日：揭穿前10大数据科学神话 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [5个真正有用的Bash脚本用于数据科学](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [Kaggle竞赛对现实世界问题有用吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)

+   [KDnuggets 新闻，8月3日：10个最常用的Tableau函数 • 是…](https://www.kdnuggets.com/2022/n31.html)
