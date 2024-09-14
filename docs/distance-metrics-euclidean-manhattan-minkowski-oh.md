# 距离度量：欧几里得、曼哈顿、闵可夫斯基，哦我的！

> 原文：[https://www.kdnuggets.com/2023/03/distance-metrics-euclidean-manhattan-minkowski-oh.html](https://www.kdnuggets.com/2023/03/distance-metrics-euclidean-manhattan-minkowski-oh.html)

![距离度量：欧几里得、曼哈顿、闵可夫斯基，哦我的！](../Images/38926b26a469bc32f4997a6c0967aed9.png)

作者提供的图片

如果你对机器学习很熟悉，你会知道数据集中的数据点和随后工程化的特征都是n维空间中的点（或向量）。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

任何两个点之间的距离也反映了它们之间的相似性。监督学习算法如K最近邻（KNN）和聚类算法如K均值聚类使用*距离度量*的概念来捕捉数据点之间的*相似性*。

在聚类中，评估的距离度量用于将数据点分组。而在KNN中，该距离度量用于找到与给定数据点最近的K个点。

在这篇文章中，我们将回顾距离度量的属性，然后查看最常用的距离度量：欧几里得距离、曼哈顿距离和闵可夫斯基距离。接着我们将介绍如何使用scipy模块中的内置函数在Python中计算这些距离。

我们开始吧！

# 距离度量的属性

在我们学习各种距离度量之前，让我们回顾一下度量空间中任何距离度量应满足的属性 [1]：

## 1\. 对称性

如果**x**和**y**是度量空间中的两点，则**x**和**y**之间的距离应等于**y**和**x**之间的距离。

![距离度量：欧几里得、曼哈顿、闵可夫斯基，哦我的！X](../Images/8d0181c4a4367ba172f66ecef1b3a6cc.png)

## 2\. 非负性

距离应始终为非负值。这意味着它应大于或等于零。

![距离度量：欧几里得、曼哈顿、闵可夫斯基，哦我的！X](../Images/0ba509fa3edd8991880e13078fe4bd56.png)

只有当**x**和**y**表示同一点时，即**x = y**，上述不等式才成立（d(**x**,**y**) = 0）。

## 3\. 三角不等式

给定三点**x**、**y**和**z**，距离度量应满足三角不等式：

![距离度量：欧几里得、曼哈顿、闵可夫斯基，哦我的！X](../Images/b3a6dafbe1e4b28fb98eeeb9580dc33b.png)

# 欧几里得距离

欧几里得距离是度量空间中任意两点之间的最短距离。考虑两个点**x**和**y**，它们在二维平面上的坐标分别为 (x1, x2) 和 (y1, y2)。

点**x**和**y**之间的欧几里得距离如图所示：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！](../Images/ac9ed2ea51c21ef49048747ff9ea279f.png)

作者提供的图片

这个距离由两个点的相应坐标之间的平方差的和的平方根给出。在数学上，二维平面中点**x**和**y**之间的欧几里得距离为：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！X](../Images/ee199019fe32a9fd034dfe08f9ba9122.png)

扩展到 n 维，点**x**和**y**的形式为**x** = (x1, x2, …, xn) 和 **y** = (y1, y2, …, yn)，我们有以下欧几里得距离方程：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！X](../Images/7d942f99d6ad7911c908cafe35a12944.png)

## 在 Python 中计算欧几里得距离

文章中提到的欧几里得距离和其他距离度量可以使用 SciPy 中**spatial**模块的便利函数进行计算。

首先，让我们从 Scipy 的`spatial`模块中导入`distance`：

```py
from scipy.spatial import distance
```

然后我们初始化两个点**x**和**y**如下：

```py
x = [3,6,9]
y = [1,0,1]
```

我们可以使用`euclidean`便利函数来找到点**x**和**y**之间的欧几里得距离：

```py
print(distance.euclidean(x,y))
Output >> 10.198039027185569
```

# 曼哈顿距离

曼哈顿距离，也称为**出租车**距离或**城市街区**距离，是另一种流行的距离度量。假设你在一个二维平面内，并且只能沿着如图所示的轴移动：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！](../Images/e8c72474c3378160747be420118de38b.png)

作者提供的图片

点**x**和**y**之间的曼哈顿距离由下式给出：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！X](../Images/a9b095bdeb86be7123d0015342aef152.png)

在 n 维空间中，每个点都有 n 个坐标，曼哈顿距离由下式给出：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！X](../Images/87408c4405d70ffde5dcfb28bc98a09b.png)

尽管曼哈顿距离不会给出任何两个给定点之间的最短距离，但在特征点位于高维空间中的应用中，它通常是首选的[3]。

## 在 Python 中计算曼哈顿距离

我们保留之前示例中的导入和 x、y：

```py
from scipy.spatial import distance

x = [3,6,9]
y = [1,0,1]
```

要计算曼哈顿（或城市街区）距离，我们可以使用`cityblock`函数：

```py
print(distance.cityblock(x,y))
Output >> 16
```

# 闵可夫斯基距离

以德国数学家赫尔曼·闵可夫斯基 [2] 的名字命名，闵可夫斯基距离在一个规范向量空间中的计算方法为：

![距离度量：欧几里得，曼哈顿，闵可夫斯基，哦天哪！X](../Images/720f5d75534f0f54e71e72ae9fdaa633.png)

很容易看出，当**p = 1**时，闵可夫斯基距离方程与曼哈顿距离的形式相同：

![距离度量：欧几里得，曼哈顿，Minkowski，哇！X](../Images/87408c4405d70ffde5dcfb28bc98a09b.png)

同样，对于 **p = 2**，Minkowski 距离等同于欧几里得距离：

![距离度量：欧几里得，曼哈顿，Minkowski，哇！X](../Images/c7080aa7afdcbc73f52ec454da6dbbcb.png)

## 在 Python 中计算 Minkowski 距离

让我们计算点 **x** 和 **y** 之间的 Minkowski 距离：

```py
from scipy.spatial import distance

x = [3,6,9]
y = [1,0,1]
```

除了需要计算距离的点（数组）之外，计算距离的 `minkowski` 函数还需要参数 `p`：

```py
print(distance.minkowski(x,y,p=3))
Output >> 9.028714870948003
```

为了验证当 p = 1 时 Minkowski 距离是否等于曼哈顿距离，我们可以调用 `minkowski` 函数，将 `p` 设置为 1：

```py
print(distance.minkowski(x,y,p=1))
Output >> 16.0
```

让我们还验证一下 p = 2 时的 Minkowski 距离是否等于我们之前计算的欧几里得距离：

```py
print(distance.minkowski(x,y,p=2))
Output >> 10.198039027185569
```

这就是全部了！如果你熟悉规范向量空间，你应该能看到这里讨论的距离度量与 Lp 范数之间的相似性。欧几里得、曼哈顿和 Minkowski 距离等同于规范向量空间中差异向量的 L2、L1 和 Lp 范数。

# 总结

本教程到此为止。我希望你现在对常见的距离度量有所了解。下一步，你可以尝试在训练机器学习算法时使用你学到的不同度量。

如果你想开始学习数据科学，可以查看这个[学习数据科学的 GitHub 仓库列表](/2022/12/learn-data-science-github-repositories.html)。祝学习愉快！

# 参考文献及进一步阅读

[1] [度量空间](https://mathworld.wolfram.com/MetricSpace.html)，Wolfram Mathworld

[2] [Minkowski 距离](https://en.wikipedia.org/wiki/Minkowski_distance)，维基百科

[3] [高维空间中距离度量的惊人行为](https://bib.dbvis.de/uploadedFiles/155.pdf)，CC Agarwal 等

[4] [SciPy 距离函数](https://docs.scipy.org/doc/scipy/reference/spatial.distance.html)，SciPy 文档

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一名技术写作人员，喜欢创建长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过撰写教程、操作指南等方式与开发者社区分享她的学习成果。

### 更多相关内容

+   [更多分类问题的性能评估度量…](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [分类度量讲解：逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [语音识别度量的演变](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [探索无监督学习度量](https://www.kdnuggets.com/2023/04/exploring-unsupervised-learning-metrics.html)

+   [机器学习评估指标：理论与概述](https://www.kdnuggets.com/machine-learning-evaluation-metrics-theory-and-overview)

+   [理解分类指标：评估模型准确性的指南](https://www.kdnuggets.com/understanding-classification-metrics-your-guide-to-assessing-model-accuracy)
