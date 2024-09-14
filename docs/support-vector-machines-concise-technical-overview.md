# 支持向量机：简明技术概述

> 原文：[https://www.kdnuggets.com/2016/09/support-vector-machines-concise-technical-overview.html](https://www.kdnuggets.com/2016/09/support-vector-machines-concise-technical-overview.html)

分类涉及建立一个模型，将数据分为不同的类别。通过输入一组预先标记类别的训练数据来构建此模型，以便算法能够学习。然后，模型通过输入另一组类别未标记的数据集来使用，根据从训练集中学到的内容预测其类别。著名的分类方案包括决策树和支持向量机，以及其他众多方案。由于这种类型的算法需要明确的类别标记，分类是一种监督学习形式。

如前所述，支持向量机（SVM）是一种特定的分类策略。SVM通过将训练数据集转换为更高的维度，然后检查类别之间的最佳分隔边界或边界来工作。在SVM中，这些边界被称为超平面，通过定位支持向量或最本质上定义类别的实例以及其边距，即超平面与其支持向量之间的最短距离平行的线来确定。因此，SVM能够分类线性和非线性数据。

支持向量机的核心思想是，当维度足够高时，可以始终找到一个超平面，将特定类别与其他所有类别分开，从而划分数据集成员类别。当重复足够多次时，可以生成足够的超平面以在*n*维空间中分离所有类别。重要的是，SVM不仅寻找任何分离超平面，而是**最大间隔**超平面，即与各类别支持向量保持等距离的超平面。

![SVM](../Images/d5f9a91b5d6ed87326db5bc95500daf4.png)

**图1：最大间隔超平面及支持向量。**

当数据是线性可分的时，可以选择许多分离线。这样的超平面可以表示为

![方程](../Images/364a39a508b7052231835d606e63da3e.png)

其中**W**是权重向量，*b*是标量偏置，X是训练数据（形式为(x[1], x[2] )）。如果我们将偏置*b*视为额外的权重，则方程可以表示为

![方程](../Images/87db00ed9a21b0870f5fd9380e1e2eec.png)

这可以转化为一对线性不等式，求解大于或小于零，其中任何一个满足的情况都表示一个特定点位于超平面之上或之下。找到最大边际超平面，即距离支持向量等距的超平面，是通过将线性不等式组合成一个方程并将其转化为受约束的二次优化问题来完成的，使用拉格朗日形式并通过[Karush-Kuhn-Tucker 条件](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)进行求解。

在此转换之后，最大边际超平面可以表示为

![方程](../Images/e5b7422f4c8f0ccfaaec65e3a45bf2eb.png)

其中 *b* 和 *α[i]* 是学习到的参数，*n* 是支持向量的数量，*i* 是一个支持向量实例，*t* 是训练实例的向量，*y[i]* 是特定训练实例的类值，*a(i)* 是支持向量的向量。一旦识别出最大边际超平面并完成训练，仅支持向量与模型相关，因为它们定义了最大边际超平面；所有其他训练实例都可以忽略。

当数据不是线性可分的时，数据首先会通过某个函数转换到更高维空间，然后在该空间中寻找超平面，另一个二次优化问题。为了避免增加的计算复杂度，所有计算可以在原始输入数据上进行，这些数据可能具有较低的维度。这种较高的计算复杂度基于这样一个事实：在更高维度中，需要计算每个实例与每个支持向量的点积。使用核函数，它将一个实例映射到由特定函数创建的特征空间，在原始的低维数据上使用。一个常见的核函数是多项式核，它表示为

![方程](../Images/66125590acc35a340f97de70f2cefccd.png)

计算 2 个向量的点积，并将结果提升到 *n* 的幂。

SVM 是强大且广泛使用的分类算法，在当前深度学习兴起之前，它们已经吸引了大量的研究关注。尽管对旁观者而言，它们可能不再是最前沿的算法，但它们在特定领域确实取得了巨大的成功，[并且仍然是机器学习从业者和数据科学家工具包中最受欢迎的分类算法之一](/2016/09/poll-algorithms-used-data-scientists.html)。

上述内容是对 SVM 的一个非常高层次的概述。支持向量机是一个复杂的分类器，更多的信息可以通过[从这里开始](/tag/support-vector-machines)找到。

**相关**:

+   [数据挖掘历史：支持向量机的发明](/2016/07/guyon-data-mining-history-svm-support-vector-machines.html)

+   [支持向量机：简单解释](/2016/07/support-vector-machines-simple-explanation.html)

+   [如何选择支持向量机核](/2016/06/select-support-vector-machine-kernels.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

### 更多相关主题

+   [支持向量机：一种直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何变革客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python向量数据库和向量索引：构建LLM应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [人工智能伦理：导航智能机器的未来](https://www.kdnuggets.com/2023/04/ethics-ai-navigating-future-intelligent-machines.html)

+   [通过数据隐私学习实施技术隐私解决方案…](https://www.kdnuggets.com/2022/04/manning-data-privacy-learn-implement-technical-privacy-solutions-tools-scale.html)
