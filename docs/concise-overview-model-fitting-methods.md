# 标准模型拟合方法的简明概述

> 原文：[https://www.kdnuggets.com/2016/05/concise-overview-model-fitting-methods.html/2](https://www.kdnuggets.com/2016/05/concise-overview-model-fitting-methods.html/2)

### 3) 随机梯度下降（SGD）

在梯度下降（GD）优化中，我们基于完整的训练集计算成本梯度，因此我们有时也称其为*批量GD*。在数据集非常大的情况下，使用GD可能会非常昂贵，因为我们每次只对训练集进行一次迭代——因此，训练集越大，我们的算法更新权重的速度就越慢，直到收敛到全局成本最小值所需的时间也可能更长（注意，SSE成本函数是凸的）。

在随机梯度下降（SGD；有时也称为*迭代*或*在线* GD）中，我们*不*像上面GD中看到的那样累积权重更新：

![Iterative GD](../Images/df75da212cfd6178769b879d04e8b876.png)

相反，我们在每个训练样本后更新权重：

![Iterative SGD](../Images/6addd7ca1e9e10f4658269976691ca14.png)

在这里，“随机”一词源于基于单个训练样本的梯度是“真实”成本梯度的“随机近似”。由于其随机性质，通向全局成本最小值的路径不像GD那样“直接”，而是可能在我们将成本面可视化为2D空间时“曲折”。然而，已经证明，如果成本函数是凸的（或伪凸的）[1]，SGD几乎可以肯定地收敛到全局成本最小值。此外，还有不同的技巧可以改进基于GD的学习，例如：

+   自适应学习率 η 选择一个减少常数*d*，该常数随时间缩小学习率：

    ![](../Images/7912a1c822b9d201fb58d0b33f3e8e5d.png)

+   通过将前一个梯度的一个因子添加到权重更新中以加速更新的动量学习：

    ![](../Images/66a8769e0d9c7c2cfea9dbbf8481676f.png)

**关于洗牌的说明**

SGD有几种不同的变体，这些变体可以在文献中看到。让我们看看三种最常见的变体：

![Shuffle A](../Images/c7e5510be283093535708988c6cc554e.png)

![Shuffle B](../Images/3b510f5554f7ff0ea8e0e76d518ef035.png)

![Shuffle C](../Images/ce7f311ed56b92dba8bf9225e7e5df12.png)

在场景A [3]中，我们仅在开始时对训练集进行一次洗牌；而在场景B中，我们在每个纪元后对训练集进行洗牌，以防止重复更新周期。在场景A和场景B中，每个训练样本仅在每个纪元中使用一次来更新模型权重。

在场景C中，我们从训练集中随机抽取训练样本并进行替换[2]。如果迭代次数*t*等于训练样本的数量，我们基于训练集的*自助样本*来学习模型。

### 4) 小批量梯度下降（MB-GD）

Mini-Batch 梯度下降（MB-GD）是批量 GD 和 SGD 之间的折中。在 MB-GD 中，我们基于较小的训练样本组更新模型；而不是从 1 个样本（SGD）或所有 *n* 个训练样本（GD）计算梯度，我们从 *1 < k < n* 个训练样本计算梯度（一个常见的 mini-batch 大小是 *k=50*）。

MB-GD 比 GD 收敛所需的迭代次数更少，因为我们更频繁地更新权重；然而，MB-GD 使我们可以利用矢量化操作，这通常带来比 SGD 更高的计算性能提升。

**参考资料**

[1] Bottou, Léon (1998). "在线算法与随机近似". 在线学习与神经网络. 剑桥大学出版社. ISBN 978-0-521-65263-6

[2] Bottou, Léon. "大规模机器学习与 SGD." COMPSTAT'2010 论文集. Physica-Verlag HD, 2010. 177-186.

[3] Bottou, Léon. "SGD 技巧." 神经网络：技巧的艺术. 斯普林格柏林海德堡, 2012. 421-436.

**简介: [Sebastian Raschka](https://twitter.com/rasbt)** 是一位数据科学家和机器学习爱好者，对 Python 和开源有着极大的热情。《[Python 机器学习](https://www.packtpub.com/big-data-and-business-intelligence/python-machine-learning)》的作者。密歇根州立大学。

[原文](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/closed-form-vs-gd.md). 已获许可转载。

**相关：**

+   [深度学习何时优于 SVM 或随机森林？](/2016/04/deep-learning-vs-svm-random-forest.html)

+   [分类的发展作为学习机器](/2016/04/development-classification-learning-machine.html)

+   [为什么从头实现机器学习算法？](/2016/05/implement-machine-learning-algorithms-scratch.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关话题

+   [使用 Python 中的标准差移除异常值](https://www.kdnuggets.com/2017/02/removing-outliers-standard-deviation-python.html)

+   [开发用于分析跟踪的开放标准](https://www.kdnuggets.com/2022/07/developing-open-standard-analytics-tracking.html)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [Python字符串方法](https://www.kdnuggets.com/2022/12/python-string-methods.html)

+   [每个程序员都应该知道的11个Python魔法方法](https://www.kdnuggets.com/11-python-magic-methods-every-programmer-should-know)
