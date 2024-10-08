# Orange 3 数据挖掘平台的新功能

> 原文：[`www.kdnuggets.com/2015/12/top-7-new-features-orange-3.html/2`](https://www.kdnuggets.com/2015/12/top-7-new-features-orange-3.html/2)

**5\. 专注于交互式可视化。** 载入数据，估算距离，绘制层次聚类，选择直方图中的有趣分支，并展示这些数据在 PCA 图中的位置。或者进行 k-means 聚类，绘制其轮廓图，选择轮廓低的实例，并在最具信息量的散点图中揭示这些异常值。交互式可视化可以选择数据子集以供进一步探索，这一直是 Orange 的强大特性之一。在 Orange 3 中，我们通过使每个视觉显示都具有交互性，进一步专注于探索性数据分析。

![silhouette-scatterplot](https://dl.dropboxusercontent.com/u/28911943/Orange/silhouette-scatterplot.png)

**6\. 基于 Numpy 的数据存储。** 虽然 Orange 的可视化编程前端变化不大，但在内部有几次重大改造。我们将自定义数据存储实现（旧的 ExampleTable 类）替换为新的（Orange.data.Table），它将实际数据存储在 numpy 数组中。对数据进行操作的算法也不再是 C 库的一部分，而是使用 Cython 代码和高效的 numpy 操作实现的。

**7\. scikit-learn 算法的封装器。** 以 Numpy 为基础的数据存储替代了 Orange 自有的内部数据格式，这只是一个更大愿景的部分：与现在大量的基于 Python 的数据科学库的互操作性。我们喜欢 Orange 提供的算法套件，特别关注符号学习，但也很高兴能够整合提供经过充分测试的强大数据挖掘算法的外部库。例如 scikit-learn，我们礼貌地借用了多个算法。如果其中任何一个尚未包含，Orange 提供了一组简化 scikit-learn 方法封装的类。封装缩短了 scikit-learn 的“知道自己在做什么”和 Orange 的“无论数据和格式如何，它都能工作”哲学之间的差距。

![ajda](img/4fa002545c278d94611e6e87be8c5f2c.png)**Ajda Pretnar** 是卢布尔雅那大学生物信息学实验室的项目助理。她拥有国际关系硕士学位。她负责编写 Orange 的技术文档、软件测试和管理错误报告。她是数据挖掘教育教程的项目协调员。

**相关内容：**

+   数据挖掘、分析、数据科学和知识发现的软件

+   50 个有用的机器学习与预测 API

+   Spark + 深度学习：使用 SparkNet 进行分布式深度神经网络训练

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [数据成熟度金字塔：从报告到主动的智能数据平台](https://www.kdnuggets.com/the-data-maturity-pyramid-from-reporting-to-a-proactive-intelligent-data-platform)

+   [KDnuggets 新闻，5 月 18 日：5 个免费机器学习托管平台](https://www.kdnuggets.com/2022/n20.html)

+   [数据共享平台的 5 个关键组件](https://www.kdnuggets.com/2022/05/5-key-components-data-sharing-platform.html)

+   [Qdrant：开源向量搜索引擎与托管云平台](https://www.kdnuggets.com/2023/02/qdrant-open-source-vector-search-engine-managed-cloud-platform.html)

+   [介绍 OpenChat：构建自定义聊天机器人的免费简易平台](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)

+   [5 步骤快速上手 Google Cloud Platform](https://www.kdnuggets.com/5-steps-google-cloud-platform)
，用于快速构建…](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)

+   [通过 5 个步骤开始使用谷歌云平台](https://www.kdnuggets.com/5-steps-google-cloud-platform)
