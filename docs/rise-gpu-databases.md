# GPU 数据库的崛起

> 原文：[`www.kdnuggets.com/2017/08/rise-gpu-databases.html`](https://www.kdnuggets.com/2017/08/rise-gpu-databases.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png)评论

**作者：Ami Gal，CEO [SQream](https://sqream.com/)。**

你可能没有意识到，但今年我们设计和使用计算资源的方式发生了重要的变革。这个变化是什么呢？许多企业和云服务提供商开始从传统的*中央处理单元*（*CPU*）处理转向使用*图形处理单元*（*GPU*）。GPU 数据库是这一趋势中的最新发展，它们有潜力彻底改变数据库的运作方式。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

![](img/f416578e0ad12d1f1a26e1f2ddf8c8ba.png)

这就是它们如何改变游戏规则及其最佳应用领域：

### GPU 数据库的好处

在对大量数据执行重复操作时，GPU 数据库提供了显著的改进。这是因为 GPU 每张卡上通常有成千上万的核心和高带宽内存。

GPU 提供了许多独特的好处；

+   更快速的创新。GPU 仍然遵循阿姆达尔定律，效率提升通常是 CPU 的两倍，且发布周期更快。

+   相比于 CPU，GPU 在处理相同工作负载时通常快 10 倍至 100 倍。

+   GPU 比 CPU 小得多（比 CPU 小 6.5 倍至 20 倍）。仅 16 台 GPU 加速服务器就能达到 1000 台 CPU 集群的性能。

+   实时可视化和处理数据的能力。由于数据本身就在强大的图形渲染引擎上，因此结果以闪电般的速度显示出来！

+   数据摄取速率非常快

+   接近实时的数据探索——实时数据探索和快速摄取速率通常意味着数据科学家和机器学习算法从使用 GPU 中受益良多。

### GPU 数据库的亮点

就在一两年前，数据库领域的许多人还将 GPU 数据库视为一种时尚，可能只适用于内存数据库旁的小众领域。他们认为传统数据库仍然是最佳选择。

然而，一些创新公司并没有这样看待它。GPU 数据库的使用迅速扩展，安装遍及所有垂直行业——包括金融、电信，甚至是 notoriously 缓慢发展的政府部门。为什么？简单来说，GPU 数据库在用于分析时表现出色，投资却仅为其一小部分。

GPU 数据库是对 Hadoop 的完美补充，因为 Hadoop 从未设计用于关系数据分析。可以通过美国邮政服务的例子来理解这一优势，该服务现在使用一排 GPU 服务器。美国邮政服务覆盖了 1.54 亿个地址，分布在 20 万个投递路线中，并分析每个投递员的位置信息。因此，可以想象他们有一个大型数据库。

有了这些数据，他们可以估计交付时间，向监督者提供实时通知，并优化临时路线。得益于他们的 GPU 数据库，他们可以在加载网页的时间内处理这些复杂的查询。这非常令人印象深刻。

GPU 数据库为广告技术、金融、电信、零售、安全/IT，甚至能源行业提供了巨大的机会。它们在国防情报领域也得到了广泛应用。

![](img/b42ffa73c20ff2bbf6cbbf8aa1a2d579.png)

### 谁从 GPU 数据库中受益？

尽管所有业务部门从更快的查询、更快的数据摄取和降低的 IT 成本中获益似乎微不足道，但实际上是数据科学家从 GPU 数据库中获益最大。

超快的数据摄取和查询意味着数据科学工作的典型周期从几天减少到几小时。其他工作负载可能从几小时减少到几分钟甚至几秒钟。缩短这些业务关键数据科学和机器学习工作负载的周期，将数据科学家从普通的“二级”数据库用户提升为 GPU 数据库的主要受益者。

### GPU 数据库项目如何实施？

1.  大多数 GPU 数据库在云环境中运行，环境范围从 IBM Bluemix 到 Amazon AWS。不过，也可以在本地数据库和混合设置中运行。一旦数据库设置完成，使用包括在内的行业标准驱动程序通过标准 SQL 查询数据：

+   JDBC, ODBC

+   Python, Jupyter, sklearn…

+   R 和其他机器学习库

之后，扩展通常就像在系统中添加另一块 GPU 一样简单。由于每个 GPU 具有如此强大的计算能力，添加新服务器的情况较少。实际上，使用某些 GPU 数据库，最多可以在标准的 2U 服务器中存储和查询高达 100TB 的原始数据。

大多数 GPU 数据库的整个设置过程通常非常简单，几乎不需要数据建模，也不需要新的或昂贵的开发和使用技能。大多数 GPU 数据库也通常与您的生态系统兼容。它们与您现有的数据源、数据采集工具，甚至您的 BI、报告、分析和可视化工具配合良好。

### 最后思考

鉴于数据量现在每两年翻一番，预计到年底，存储系统将存储约 17.6 万亿字节的数据。然而，大数据的价值在于其分析速度。快速分析能让你的数据以你尚未想象的方式为你的组织增加价值。

如果你的业务依赖于传统数据库，你应该考虑适合你的 GPU 数据库。毕竟，对系统的需求只会持续增加。

**简介：** [**艾米·加尔**](https://sqream.com/about/leadership-team/) 是 SQream 的首席执行官兼联合创始人。

**相关：**

+   深度学习 – 过去、现在和未来

+   机器学习中的并行性：GPUs、CUDA 和实际应用

+   数据分析的 4 种类型

### 主题更多

+   [构建 GPU 机器与使用 GPU 云](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成为出色数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每位初学数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [构建强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
