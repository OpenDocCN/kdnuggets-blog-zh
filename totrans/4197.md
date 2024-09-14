# 使用Python创建具有异常特征的合成时间序列

> 原文：[https://www.kdnuggets.com/2021/10/synthetic-time-series-anomaly-signatures-python.html](https://www.kdnuggets.com/2021/10/synthetic-time-series-anomaly-signatures-python.html)

[comments](#comments)

![](../Images/2853b86d2ee323dcee3a240c620cd80d.png)

**图片来源**：作者使用 [Pixabay](https://pixabay.com/illustrations/digitization-cyborg-chip-circuit-6399664/) （免费使用）制作

## 为什么选择合成时间序列？

正如我在我的[高引用文章](https://towardsdatascience.com/synthetic-data-generation-a-must-have-skill-for-new-data-scientists-915896c0c1ae)中写的，“*合成数据集是通过程序生成的数据仓库。因此，它不是通过任何现实调查或实验收集的。其主要目的是足够灵活和丰富，以帮助机器学习实践者进行各种分类、回归和聚类算法的有趣实验*。”

[**合成数据生成——新数据科学家的必备技能**](https://towardsdatascience.com/synthetic-data-generation-a-must-have-skill-for-new-data-scientists-915896c0c1ae)

合成时间序列也不例外——它帮助数据科学家尝试各种算法方法，并为真实部署做好准备，这些是仅靠真实数据集无法实现的。

## 工业环境中的时间序列数据

现代工业环境中，时间序列分析有多种丰富的应用，其中一系列传感器从机器、工厂、操作员和业务流程中产生源源不断的数字数据。

压力。温度。电动组件的振动和加速度。质量检查数据。操作员行动日志。

数据不断涌入。这是[工业4.0](https://www.epicor.com/en-us/resource-center/articles/what-is-industry-4-0/)或[智能工厂时代](https://metrology.news/factory-2030-the-coming-of-age-of-the-smart-factory/)的新常态。虽然结构化和半结构化数据在上升，但仍有大量时间序列（或类似时间序列）数据来自现代工厂中嵌入的各种测量点。

[**工厂2030——智能工厂的“成年礼”——计量与质量新闻——在线…**](https://metrology.news/factory-2030-the-coming-of-age-of-the-smart-factory/)

![](../Images/2148876c777ba46aff97a88897de4575.png)

**图片来源**： [Pixabay](https://pixabay.com/illustrations/industry-web-network-artificial-4330186/) （免费使用）

## 异常检测至关重要

大多数时候，它们是“正常的”、“在范围内的”、“如预期的”。但偶尔也会出现异常。这时你需要特别关注。这些是正常数据流中的“异常”数据，需要被捕捉、分析和处理——几乎总是实时的。

在这些数据流中的异常检测是所有现代数据分析产品、服务和初创公司的核心。他们利用从经过验证的时间序列算法到最新的基于神经网络的序列模型的各种手段来检测这些异常特征，并根据业务逻辑创建警报或采取行动。

> 在现代工业环境中，时间序列分析有着丰富的应用，其中大量传感器不断产生数字数据流……

## 合成数据生成是一种强大的辅助工具

关于这些工业数据流，有几个点值得重复，以理解为什么合成时间序列生成可能会变得非常有用。

+   **现实生活中的异常很少见**，需要监控和处理大量数据才能检测到各种有趣的异常。这对希望在短时间内测试和重测多种算法的数据科学家来说并不是好消息。

+   **异常的发生是如此不可预测**，以至于它们的模式很少能被捕捉到任何完善的统计分布中。快速尝试多种异常类型对于生成一个**稳健且可靠**的异常检测系统至关重要。在缺乏常规、可信的异常数据来源的情况下，合成方法提供了实现某种**受控实验**的唯一希望。

+   许多工业传感器生成的数据被认为是**高度保密**的，不能超出本地私有云或现有边缘分析系统。为了**在不妨碍数据安全的情况下重现异常特征**，合成方法是一个明显的选择。

> 这些是正常数据流中的‘异常’，需要被捕捉、分析并采取行动——几乎总是需要实时处理。

在本文中，我们展示了一种简单而直观的方法，来在模仿工业过程的一维合成时间序列数据中创建几种常见的异常特征。我们将使用大家都喜欢的 Python 语言来实现这一点。

> **注意**：这不是一篇关于异常检测算法的文章。我只讨论与合成异常注入时间序列数据（集中于特定应用领域）相关的想法和方法。

## 带有异常的合成时间序列

这里是[**Jupyter notebook**](https://github.com/tirthajyoti/Synthetic-data-gen/blob/master/Notebooks/Time%20series%20synthesis%20with%20anomaly.ipynb)，这里是包含主类对象的 Python 模块，供你使用。

## 工业过程的概念和‘单元过程时间’

![](../Images/b76d062be63451034d17935114d1af0c.png)

**图像来源**：作者创作

上面我们展示了一个典型工业过程和一个‘单位过程时间’的示意图。假设一些原材料（图中的***过程 I/P***）进入一台复杂的机器，而成品（图中的***过程 O/P***）从另一端出来。

我们不需要知道机器内部究竟发生了什么，只需知道它在规律的时间间隔内生成一些数据，即我们可以以时间序列的方式测量过程状态（也许使用一些传感器）。我们想查看这个数据流并检测异常。

因此，为了定义我们的合成时间序列模块，我们至少需要以下内容，

+   过程开始时间

+   过程结束时间

+   单位过程时间（我们接收数据的间隔）

因此，这就是基本的`SyntheticTS`类的定义开始，

![](../Images/ed4b5f58af6c096601acc2f45ec74acc.png)

## ‘正常’过程

为了生成异常，我们需要一个基准正常状态。我们可以字面上使用“**正态分布**”来实现。你可以根据具体的过程类型和情况随时更改，但绝大多数工业过程在其传感器测量方面确实遵循正态分布。

![](../Images/e32ada8481560ebd81bfd8d9391070a3.png)

假设我们有一个工业过程/机器从2021年5月1日启动，并运行到2021年5月6日（一个典型的6天周期，通常在每周维护之前）。单位过程时间为15分钟。我们选择了过程的均值为100，标准差为5。

![](../Images/3ba564c75732b06bb1ae60d36299efab.png)

![](../Images/9f417dbfdf355caaa98c3e2bdf945ea8.png)

![](../Images/b6a6be0e912f49b1b714a147b6fb3f8c.png)

## 基本的‘异常化’方法

作为一种合成数据生成方法，你需要控制异常的以下特征，

+   需要异常的数据比例

+   异常的尺度（它们距离正常值的远近）

+   单侧或双侧（高于或低于正常数据的幅度）

我们不关心确切的代码，而是展示一些关键结果。

## 单侧异常

这是一个例子，

![](../Images/6be322f5559cf1884c3f0e69526f9061.png)

## 变化的异常尺度

我们可以通过简单地改变`anomaly_scale`参数，将异常放置在不同的距离上。

![](../Images/a4110279666cf9be3d20e78c5f347898.png)

这是生成的图示。注意图示的纵轴刻度是如何随着异常的增大而变化的。

![](../Images/5e9e65cc2a880eb4769d0fc8e113c313.png)

## 异常比例的变化

接下来，我们改变异常的比例（保持尺度为2.0不变）。

![](../Images/17582c98f11117f388f269bb89a1b4f0.png)

## 引入‘正向偏移’

这是工业过程中相当常见的情况，过程中由于机器设置或其他原因的突然变化，导致出现明显的偏移。**有时这是计划好的，有时则是无意的**。根据情况，异常检测算法可能需要不同地分析和处理。无论如何，我们需要一种方法在合成数据中引入这种偏移。

在这种情况下，我们选择了数据的10%偏移，即均值为`pct_drift_mean=10`参数。请注意，如果我们不在方法中指定参数`time_drift`，则代码会自动在整个过程的开始和结束时间的中点引入漂移。

![](../Images/0b340736ebd60dad544790d407e55aad.png)

## 在特定位置的负偏移

在以下示例中，我们展示了一个情况，

+   数据在负方向上漂移

+   数据的扩展（方差）以及均值发生了变化

+   偏移从特定位置开始，用户可以选择该位置

这是一个更现实的情况。

![](../Images/6871007d9e376f58d62c63de14f440ef.png)

## 分块异常

在许多情况下，**异常以块状出现并消失**。我们也可以合成这种情况。请注意，这里我们创建了“左右侧异常”，但与所有其他选项类似，我们也可以创建“单侧”变异。

目前，代码创建的分块异常均匀分布在整个时间段内。但在下一次代码更新中，这可以通过单独的时间点和分块的异常特征进行自定义。

![](../Images/57676a76d6f787b595cf7595589aa315.png)

![](../Images/c0fe23837bc4599f186de8de4dd7be77.png)

## 总结

我们展示了一种简单直观的方法来创建具有各种异常签名的一维合成时间序列数据，这些异常在工业使用案例中很常见。这种合成数据生成技术对算法迭代和模型测试非常有用。

## 保持简单

为了聚焦于工业使用案例，**我们没有在基线数据**生成中添加传统的时间序列模式（例如季节性、上升/下降趋势），并保持极其简单作为高斯过程。数据中也没有自回归特性。尽管像ARIMA这样的算法在金融和商业数据分析中非常受欢迎且有用，但工业环境中生成的独立传感器数据通常是正态分布的，我们遵循这一原则。

## 进一步改进

你可以在此基础上进行许多扩展并添加额外功能。以下是一些可能的功能：

+   将各种统计分布的选择作为基线数据生成过程

+   任意位置和特征的分块异常

+   多个合成数据类/对象的组合方法

+   更好的可视化方法

再次提示，[Jupyter notebook 示例在此](https://github.com/tirthajyoti/Synthetic-data-gen/blob/master/Notebooks/Time%20series%20synthesis%20with%20anomaly.ipynb)。请随意 fork 和实验。

您可以查看作者的 [**GitHub**](https://github.com/tirthajyoti?tab=repositories)** 代码库 **，获取机器学习和数据科学中的代码、想法和资源。如果您像我一样对 AI/机器学习/数据科学充满热情，请随时 [在 LinkedIn 上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 或 [在 Twitter 上关注我](https://twitter.com/tirthajyotiS)。

**简介： [Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)** 是 Adapdix Corp. 的数据科学/机器学习经理。他定期为 KDnuggets 和 TDS 等出版物贡献有关数据科学和机器学习的多种主题。他编著了数据科学书籍并参与开源软件的贡献。Tirthajyoti 拥有电子工程博士学位，并正在攻读计算数据分析的硕士学位。可以通过 tirthajyoti at gmail[dot]com 联系他。

**相关内容：**

+   [使用合成数据教 AI 分类时间序列模式](/2021/10/teaching-ai-classify-time-series-patterns-synthetic-data.html)

+   [3 种数据采集、注释和增强工具](/2021/08/3-data-labeling-synthesizing-augmentation-tools.html)

+   [使用 Gretel 和 Apache Airflow 构建合成数据管道](/2021/09/build-synthetic-data-pipeline-gretel-apache-airflow.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关话题

+   [数据科学中的异常检测技术初学者指南](https://www.kdnuggets.com/2023/05/beginner-guide-anomaly-detection-techniques-data-science.html)

+   [BigQuery 中的异常检测：发现隐藏的洞察并推动行动](https://www.kdnuggets.com/anomaly-detection-in-bigquery-uncover-hidden-insights-and-drive-action)

+   [如何利用合成数据克服机器学习模型训练中的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [假装成功：生成真实的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [合成数据社区已经建立，这就是我们需要它的原因](https://www.kdnuggets.com/2022/04/community-synthetic-data-need.html)
