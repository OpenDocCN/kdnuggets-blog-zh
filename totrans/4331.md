# 正态分布的综合指南

> 原文：[https://www.kdnuggets.com/2021/01/comprehensive-guide-normal-distribution.html](https://www.kdnuggets.com/2021/01/comprehensive-guide-normal-distribution.html)

[评论](#comments)![图](../Images/8c553d9318441d8fecf2313d04e18137.png)

照片由 [Cameron Casey](https://www.pexels.com/@camcasey?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来源于 [Pexels](https://www.pexels.com/photo/action-adult-balance-dusk-1152854/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

数据分布是指数据的分布方式。在本文中，我们将讨论与正态分布相关的基本概念：

+   测量正态性的方式

+   将数据集转换为适应正态分布的方法

+   使用正态分布来表示自然发生的现象并提供统计洞察

### 概述

数据分布在统计学中非常重要，因为我们几乎总是在从一个总体中抽样，而这个总体的完整分布是未知的。样本的分布可能会限制我们可用的统计技术。

![图](../Images/e8de940854969e29359adb6e1caa4b93.png)

正态分布，其中 f(x) = 概率密度函数，σ = 标准差，μ = 均值

正态分布是一种常见的连续概率分布。当数据集符合正态分布时，可以利用许多方便的技术来探索数据：

+   了解每个标准差内的数据百分比

+   线性最小二乘回归

+   基于样本均值的推断（例如，t检验）

在某些情况下，将偏斜的数据集转换为符合正态分布是有益的，从而解锁这一统计技术的使用。当你的数据几乎是正态分布的，只是有一些扭曲时，这尤其相关。稍后会详细讨论这个问题。

正态分布具有以下特征：

+   对称的钟形曲线

+   均值和中位数相等（在分布的中心）

+   ≈68%的数据落在均值的1个标准差以内

+   ≈95%的数据落在均值的2个标准差以内

+   ≈99.7%的数据落在均值的3个标准差以内

![图](../Images/91fce44aff5b111e2418c25f9be8b960.png)

[M.W. Toews](https://commons.wikimedia.org/wiki/User:Mwtoews) 通过 [维基百科](https://en.wikipedia.org/wiki/Normal_distribution#/media/File:Standard_deviation_diagram.svg)

下面是一些你应该熟悉的与正态分布概述相关的术语：

+   **正态分布**：一种对称的概率分布，常用于表示实值随机变量；有时称为钟形曲线或高斯分布

+   **标准差**：衡量一组值的变异或离散程度；计算为方差的平方根

+   **方差**：每个数据点与均值的距离

### 如何使用正态分布

如果你的数据集不符合正态分布，以下是一些建议：

+   **收集更多数据**：小样本量或数据质量不足可能会扭曲你原本正常分布的数据集。正如数据科学中经常遇到的那样，解决方案可能是收集更多的数据。

+   **减少方差来源**：[减少离群值](https://towardsdatascience.com/data-science-new-normal-ca34bcbad8f0)可能会导致数据正态分布。

+   **应用幂变换**：对于偏斜的数据，你可以选择使用 [Box-Cox 方法](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.power_transform.html)，它涉及对观察值进行平方根和对数变换。

在接下来的章节中，我们将探讨一些正态性的度量标准以及如何在数据科学项目中使用它们。

### 偏度

偏度是相对于均值的非对称度量。以下是一个**左偏分布**的图示。

![图示](../Images/aa79b8c873df0fec7b0b66cc8fca4068.png)

[Rodolfo Hermans](https://en.wikipedia.org/wiki/User:Rodolfo_Hermans) 通过 [Wikipedia](https://en.wikipedia.org/wiki/Skewness#/media/File:Negative_and_positive_skew_diagrams_(English).svg)

???? 我总是觉得这有点反直觉，所以这里值得特别注意。这个图形具有**负偏度**。这意味着分布的**尾部**在左侧更长。对我而言，这有点反直觉的是，大多数数据点都集中在右侧。不要被右偏度或正偏度所迷惑，它们会由这个图形的镜像图表示。

### 如何使用偏度

理解偏度很重要，因为它是模型性能的关键因素。要测量偏度，可以使用`[skew](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)`[ 来自](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)`[scipy.stats](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)`[ 模块](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)。

![图示](../Images/9e568997e25d029c332c81cd0ce3aa11.png)

通过 [SciPy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)

偏度度量可以提示我们模型性能在特征值上的潜在偏差。像上面第二个数组那样的正偏特征，将在较低值上提供更好的性能，因为我们在那个范围内提供了更多数据（与较高值离群点相对）。

### 峰度

源自希腊文*kurtos*，意为“弯曲”，峭度是衡量分布尾部厚度的一个指标。峭度通常相对于0来衡量，即正态分布的峭度值，使用[Fisher’s definition](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)来定义。正的峭度值表示尾部更“肥”（即更瘦的钟形曲线和更多的异常值）。

![图像](../Images/a240fcc76bd15aa959e0c6869b0b4aed.png)

拉普拉斯分布的峭度> 0。通过[John D. Cook Consulting](https://www.johndcook.com/blog/2019/02/05/normal-approximation-to-laplace-distribution/)。

### 如何使用峭度

理解峭度可以提供对数据集中异常值存在的洞察。要测量峭度，请使用`[kurtosis](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)`[来自](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)`[scipy.stats](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)`[模块](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)。

![图像](../Images/47145722fa21b8e14b73227e508cc3c7.png)

通过[SciPy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)

负峭度值表示数据围绕均值更紧密地分布，异常值较少。

### 关于正态分布的一个警示

你可能听说过许多自然发生的数据集符合正态分布。这一说法已经被应用于从智商到人体身高的各种数据。

虽然正态分布确实是从自然观察中得出的，并且确实经常出现，但如果过于宽泛地应用这个假设，我们可能会面临过度简化的风险。

> 正态模型在极端情况通常不适用。它通常低估了罕见事件的概率。[*黑天鹅*](https://amzn.to/2TVo5M6)由纳西姆·尼古拉斯·塔勒布（Nassim Nicholas Taleb）提供了许多例子，说明罕见事件并不像正态分布预测的那样*罕见*。

[**为什么身高不符合正态分布**](https://www.johndcook.com/blog/2008/07/20/why-heights-are-not-normally-distributed/)

在我之前的文章中，我对为什么身高呈正态分布进行了推测，也就是说，为什么它们的统计分布...

### 摘要

在这篇关于正态分布的简短文章中，我们涵盖了一些基本概念、如何测量以及如何使用它。要小心不要过度应用正态分布，否则你可能会低估异常值的可能性。希望这篇文章能为你提供对这一常见且极具实用性的统计概念的一些见解。

### 你可能会喜欢的更多文章

[**如何使用聚类创建邻里探索工具**](https://towardsdatascience.com/neighborhood-explorer-a7f374e8527d)

使用sklearn的聚类算法创建一个城市互动仪表板的逐步指南。

[**应对新常态的数据科学——来自一个14亿美元初创公司的经验教训**](https://towardsdatascience.com/data-science-new-normal-ca34bcbad8f0)

后疫情时代，机器学习对商业成功的作用越来越重要。

[**他们在训练营中没有教的10个Python技能**](https://towardsdatascience.com/10-python-skills-419e5e4c4d66)

通过这份编码技巧列表，攀登数据科学和机器学习的新高度。

**简介：[Nicole Janeway Bills](https://www.linkedin.com/in/nicole-janeway-bills/)** 是一位数据科学家，拥有商业和联邦咨询经验。她帮助组织利用他们最重要的资产：一个简单而稳健的数据策略。[**注册获取她的更多文章**](https://page.co/ahje9p)。

[原始内容](https://towardsdatascience.com/normal-distribution-160a93939248)。经许可转载。

**相关内容：**

+   [数据分布概述](/2020/06/overview-data-distributions.html)

+   [数据科学的基础数学：泊松分布](/2020/12/introduction-poisson-distribution-data-science.html)

+   [正常（或近似正常）分布](/2020/05/looking-normally-distributed.html)

* * *

## 我们的3个顶级课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在IT领域

* * *

### 更多相关主题

+   [正常分布全面指南](https://www.kdnuggets.com/2022/06/comprehensive-guide-normal-distribution.html)

+   [事情并不总是正常的：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

+   [如何使用Python确定最佳数据分布](https://www.kdnuggets.com/2021/09/determine-best-fitting-data-distribution-python.html)

+   [MLOps全面指南](https://www.kdnuggets.com/2023/08/comprehensive-guide-mlops.html)

+   [NLP、NLU和NLG：有什么区别？全面指南](https://www.kdnuggets.com/2022/06/nlp-nlu-nlg-difference-comprehensive-guide.html)

+   [卷积神经网络全面指南](https://www.kdnuggets.com/2023/06/comprehensive-guide-convolutional-neural-networks.html)
