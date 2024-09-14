# 数据科学中的降维是什么？

> 原文：[https://www.kdnuggets.com/2019/01/dimension-reduction-data-science.html](https://www.kdnuggets.com/2019/01/dimension-reduction-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

现在我们可以访问大量数据。大量数据可能会导致我们将所有可用数据输入到预测模型中以预测目标变量。本文旨在解释引入大量特征时常见的问题，并提供我们可以利用的解决方案来解决这些问题。

*每个数据科学家和机器学习专家都必须理解降维技术是什么以及何时使用它们。*

![](../Images/006d7d77736e6ecab546ceb5e7acf823.png)

**照片由 [Sergi Kabrera](https://unsplash.com/@skabrera?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)**

**让我们更好地理解这些问题**

有时我们为数据科学项目收集数据，结果收集了一大堆特征。其中一些特征（称为变量）并不像其他特征那么重要。有时这些特征之间是相关的。偶尔我们会因为引入过多的特征而导致过拟合问题。大量特征使数据集变得稀疏。

此外，存储具有大量特征的数据集需要更大的空间。而且，分析和可视化具有大量维度的数据集会变得非常困难。

> *降维可以减少训练机器学习模型所需的时间，同时也有助于消除过拟合。*

本文概述了我们可以遵循的技术，以将数据集压缩到一个新的低维特征子空间中。我还将提供有关重要降维技术的详细信息。

*请阅读 FinTechExplained 的*[免责声明](https://medium.com/p/87dba77241c7?source=your_stories_page---------------------------)*。*

**我如何定义降维？**

想象一下，你想将一大堆文件通过电子邮件发送给朋友。上传和发送这些文件可能会花费更长时间。你可以通过将文件压缩后再发送电子邮件来加快文件上传的过程。压缩文件将大量数据压缩成较小的等效集合。

> *降维与压缩数据的原理相同。*
> 
> *降维是将大量特征压缩到一个较低维度的新特征子空间中，而不丢失重要信息。*

尽管有一个小的区别，即降维技术在降低维度时会丢失一些信息。

视觉化大量维度是比较困难的。维度减少技术可以将20多个维度的特征空间转变为2或3维子空间。

**不同的维度减少技术有哪些？**

在深入了解关键技术之前，让我们快速了解机器学习的两个主要领域：

1.  有监督的——当训练集的结果已知时

1.  无监督的——当最终结果未知时

如果你想更好地理解机器学习，请查看我的文章：

[8分钟了解机器学习](https://medium.com/fintechexplained/introduction-to-machine-learning-4b2d7c57613b)

有许多减少维度的技术，如前向/后向特征选择或通过计算相关特征的加权平均来合并维度。然而，在这篇文章中，我将探讨两种主要的维度减少技术：

#### 线性判别分析 (LDA)：

LDA用于压缩监督数据。

当我们有大量特征（类别），且数据呈正态分布且特征之间没有相关性时，我们可以使用LDA来减少维度。LDA是Fisher线性判别的广义版本。

*计算z-score以规范化高度偏斜的特征。*

如果你想了解如何丰富特征和计算z-score，请查看这篇文章：

[处理数据以提高机器学习模型的准确性](https://medium.com/fintechexplained/processing-data-to-improve-machine-learning-models-accuracy-de17c655dc8e)

Sci-kit learn提供了易于使用的LDA工具：

```py
from sklearn.lda import LDA
my_lda = LDA(n_components=3)
lda_components = my_lda.fit_transform(X_train, Y_train)

```

这段代码将生成整个数据集的三个LDA组件。

![](../Images/a9b51b9491e5ee73e742e88d09cb8150.png)

**照片由 [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)**

#### **主成分分析 (PCA)：**

它们主要用于压缩无监督数据。

PCA是一种非常有用的技术，可以帮助去噪声和检测数据中的模式。PCA用于减少图像、文本内容和语音识别系统中的维度。

Sci-kit learn库提供了强大的PCA组件分类器。以下代码片段演示了如何创建PCA组件：

```py
from sklearn.decomposition import PCA
pca_classifier = PCA(n_components=3)
my_pca_components = pca_classifier.fit_transform(X_train)

```

**了解PCA的工作原理是明智的。**

#### 理解PCA

本文的这一部分提供了过程的概述：

+   PCA技术分析整个数据集，然后找到具有最大方差的点。

+   它创建了新的变量，使得新变量与原始变量之间存在线性关系，从而最大化方差。

+   然后为特征创建协方差矩阵以理解其多重共线性。

+   一旦计算了方差-协方差矩阵，PCA将使用收集到的信息来减少维度。它从原始特征轴计算正交轴。这些是具有最大方差的方向轴。

首先计算方差-协方差矩阵的特征向量。该向量表示最大方差的方向，这些方向称为主成分。然后生成定义主成分大小的特征值。

*特征值是PCA组件。*

因此，对于N维度，将有一个NxN方差-协方差矩阵，因此，我们将有一个N值和N特征值矩阵的特征向量。

我们可以使用以下Python模块来创建组件：

使用linalg.eig创建特征向量

使用numpy.cov计算方差-协方差矩阵

我们需要选择最能代表数据集的特征向量。这些是具有最高特征值的向量。

> *选择捕捉大约70%方差的特征向量。*

*记住，具有最大特征值的特征向量是方差最高的，并且最接近原始数据集。同时，特征向量数量越多，计算性能越慢。*

*我通常选择2-3个顶级特征向量来代表数据集。*

如果我们希望scikit-learn提供所有的PCA组件以便评估方差，则将PCA初始化为None组件：

![](../Images/7cf98875a6bedcf8791460f87c4e2e29.png)

**照片由 [Chang Qing](https://unsplash.com/@lee0201?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)**

*在执行PCA之前，规范化/标准化数据是重要的，因为PCA对特征数据的尺度敏感。*

#### 核主成分分析 (KDA)：

*它们用于非线性降维。*

当我们拥有非线性特征时，可以将它们投影到更大的特征集上，以去除它们的相关性并使其线性。

本质上，非线性数据被映射和转换到更高维空间。然后使用PCA来减少维度。然而，这种方法的一个缺点是计算开销非常大。

就像在PCA中，我们首先计算方差-协方差矩阵，然后准备具有最高方差的特征向量和特征值来计算主成分。

然后我们计算核矩阵。这要求我们构建相似性矩阵。矩阵随后通过创建特征值和特征向量进行分解。

Sci-Kit Learn提供了核PCA模块。要使用核PCA，我们可以使用以下代码片段：

```py
from sklearn.decomposition import KernelPCA
kpca = KernelPCA(n_components=2,kernel='rbf', gamma=45)
kpca_components = kpca.fit_transform(X)

```

Gamma是RBF核的调节参数。

![](../Images/cb6d57451b3efb60222c26792b1d2e47.png)

**照片由 [Billy Huynh](https://unsplash.com/@billy_huy?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)**

### 降维的好处

本节简要概述了降维的核心好处。

我们现在可以访问大量的数据。当我们构建训练于图像、声音和/或文本内容上的预测模型时，输入特征集可能会有大量的特征。这增加了空间需求，进一步导致过拟合，并减慢了模型训练的时间。有时，特征的引入会带来比预期更多的噪声。

提高计算密集型任务效率的关键方法之一是减少维度，同时确保大部分关键信息得以保留。这也消除了具有强相关性的特征，并减少了过拟合。

### 总结

本文概述了我们可以遵循的技术，以将数据集压缩到新的低维特征子空间中。它还提供了重要降维技术的详细信息。

最后，总结了降维的好处。

如果有任何问题，请告诉我。

[原文](https://medium.com/fintechexplained/what-is-dimension-reduction-in-data-science-2aa5547f4d29)。经授权转载。

**资源：**

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [数据科学淘金热：数据科学中的热门职位及如何获取](https://www.kdnuggets.com/2019/01/top-jobs-data-science.html)

+   [降维 : PCA 是否真的能改善分类结果？](https://www.kdnuggets.com/2018/07/dimensionality-reduction-pca-improve-classification-results.html)

+   [必知：什么是维度灾难？](https://www.kdnuggets.com/2017/04/must-know-curse-dimensionality.html)

### 更多相关话题

+   [数据科学中的降维技术](https://www.kdnuggets.com/2022/09/dimensionality-reduction-techniques-data-science.html)

+   [停止学习数据科学以寻找目标，并寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最低要求：你需要了解的 10 项基本技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [KDnuggets™ 新闻 22:n06, 2 月 9 日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)

+   [数据科学定义幽默：一系列古怪的名言…](https://www.kdnuggets.com/2022/02/data-science-definition-humor.html)

+   [5 个数据科学项目以学习 5 个关键数据科学技能](https://www.kdnuggets.com/2022/03/5-data-science-projects-learn-5-critical-data-science-skills.html)
