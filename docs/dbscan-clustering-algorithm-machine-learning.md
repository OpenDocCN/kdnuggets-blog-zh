# 机器学习中的DBSCAN聚类算法

> 原文：[https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html](https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html)

![图示](../Images/d2cb5b01a0ce10194d0ada7a6d683441.png)

[来源](https://www.stratio.com/blog/graph-database-clustering-solution/)

> * * *
> 
> ## 我们的前三个课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求
> 
> * * *
> 
> 2014年，DBSCAN算法在领先的数据挖掘会议ACM [SIGKDD](https://en.wikipedia.org/wiki/SIGKDD)上获得了“时间考验奖”（颁发给理论和实践中获得大量关注的算法）。
> 
> — [维基百科](https://en.wikipedia.org/wiki/DBSCAN)

# 介绍

聚类分析是一种无监督学习方法，它将数据点分为几个特定的组或群体，使得同一组的数据点具有相似的属性，而不同组的数据点在某种意义上具有不同的属性。

它包括许多不同的方法，基于不同的距离度量。例如，K均值（点之间的距离）、亲和传播（图距离）、均值漂移（点之间的距离）、DBSCAN（最近点之间的距离）、高斯混合（到中心的马氏距离）、谱聚类（图距离）等。

从本质上讲，所有的聚类方法都使用相同的方法，即首先计算相似性，然后利用这些相似性将数据点聚集成组或批次。这里我们将重点讨论**基于密度的噪声应用空间聚类**（**DBSCAN**）聚类方法。

如果你对聚类算法不熟悉，我建议你阅读[使用K均值聚类进行图像分割的介绍](https://towardsdatascience.com/introduction-to-image-segmentation-with-k-means-clustering-83fd0a9e2fc3)。你也可以阅读关于[层次聚类](https://medium.com/swlh/what-is-hierarchical-clustering-c04e9972e002)的文章。

**为什么我们需要像DBSCAN这样的基于密度的聚类算法，而不是已经有K均值聚类？**

K-均值聚类可能将松散相关的观测值聚集在一起。即使观测值在向量空间中分散得很远，每个观测值最终也会成为某个簇的一部分。由于簇依赖于簇元素的均值，每个数据点在形成簇时都会发挥作用。数据点的轻微变化可能会影响聚类结果。这一问题在 DBSCAN 中由于簇形成方式的不同得到了很大程度的减少。除非我们遇到一些奇特的形状数据，否则这通常不是一个大问题。

另一个挑战是 *k*-均值算法，你需要指定簇的数量（“*k*”）才能使用它。很多时候，我们不知道合理的 *k* 值是什么 *a priori*。

DBSCAN 的优点是你不需要指定要使用的簇的数量。你只需要一个计算值之间距离的函数和一些关于什么距离被认为是“接近”的指导。DBSCAN 在各种不同的分布中也比 *k*-均值算法产生更合理的结果。下图说明了这一点：

![图](../Images/e50803f50d89d5d6ebad229f6ccec366.png)

[致谢](https://github.com/NSHipster/DBSCAN)

# 基于密度的聚类算法

**基于密度**的**聚类** 指的是无监督学习方法，通过识别数据中的独特组/簇来进行分类，其基于这样一个理念：数据空间中的簇是高点密度的连续区域，由低点密度的连续区域与其他簇隔开。

基于密度的空间聚类（DBSCAN）是基于密度的聚类的基础算法。它可以从大量数据中发现不同形状和大小的簇，这些数据包含噪声和离群点。

DBSCAN 算法使用两个参数：

+   **minPts:** 为了使一个区域被认为是密集的，聚集在一起的最小点数（阈值）。

+   **eps (ε):** 用于定位任何点的邻域内点的距离度量。

如果我们探索两个叫做密度可达性（Density Reachability）和密度连接性（Density Connectivity）的概念，就能理解这些参数。

**可达性** 从密度的角度来说，建立一个点从另一个点可达的条件是它在该点的特定距离（eps）范围内。

**连接性**，另一方面，涉及基于传递性的链式方法来确定点是否位于特定簇中。例如，如果 p->r->s->t->q，则 p 和 q 点可以连接，其中 a->b 表示 b 在 a 的邻域内。

DBSCAN 聚类完成后，有三种类型的点：

![](../Images/0cf15c12e52f0f6b8bc9a54334ba2888.png)

+   **核心点** — 这是一个距离自身不超过 *n* 的区域内至少有 *m* 个点的点。

+   **边界点** — 这是一个在距离 *n* 内至少有一个核心点的点。

+   **噪声** — 这是一个既不是核心点也不是边界点的点。它在自身距离 *n* 内的点少于 *m* 个。

**DBSCAN聚类的算法步骤**

+   算法通过任意选择数据集中的一个点（直到所有点都被访问）来进行。

+   如果在半径为‘ε’的范围内有至少‘minPoint’个点，则我们将这些点视为同一簇的一部分。

+   然后，通过递归地重复每个邻近点的邻域计算来扩展簇。

![图](../Images/c8cebaa015d5f15db51754a37b800bf8.png)

[DBSCAN的实际应用](https://www.digitalvidya.com/blog/the-top-5-clustering-algorithms-data-scientists-should-know/)

# 参数估计

每个数据挖掘任务都有参数问题。每个参数都会以特定的方式影响算法。对于DBSCAN，需要**ε**和**minPts**这两个参数。

+   **minPts**: 作为经验法则，最小*minPts*可以从数据集中的维度数*D*推导出来，公式为`***minPts* ≥ *D* + 1**`。低值`***minPts* = 1**`是没有意义的，因为那样每个点本身就会形成一个簇。`***minPts* ≤ 2**`的结果将与使用单链接度量的[hierarchical clustering](https://en.wikipedia.org/wiki/Hierarchical_clustering)相同，树状图的剪切高度为ε。因此，*minPts*至少应选择为3。然而，对于有噪声的数据集，较大的值通常更好，会产生更显著的簇。作为经验法则，可以使用`** *minPts* = 2·*dim***`，但对于非常大的数据集、噪声数据或包含许多重复的数据，可能需要选择更大的值。

+   **ε**: 可以通过使用一个[k-distance图](https://en.wikipedia.org/wiki/Nearest_neighbor_graph)来选择ε的值，该图绘制了距离到`***k* = *minPts*-1**`最近邻的距离，并按从大到小的顺序排列。好的ε值是在该图上显示“肘部”的位置：如果ε选择得太小，大部分数据将无法被聚类；而ε值过高，则聚类会合并，大多数对象会在同一个簇中。一般来说，小的ε值是更可取的，作为经验法则，只有少量的点应该在彼此的这个距离之内。

+   **距离函数**: 距离函数的选择与ε的选择密切相关，并对结果有重大影响。通常，在选择参数ε之前，需要首先确定数据集的合理相似度度量。没有此参数的估计，但需要为数据集选择适当的距离函数。

# 使用Scikit-learn的DBSCAN Python实现

让我们首先对球形数据应用DBSCAN进行聚类。

我们首先生成750个球形训练数据点及其对应的标签。之后对训练数据的特征进行标准化，最后应用sklearn库中的DBSCAN。

![](../Images/41b07d566d9620c082570549d34d0606.png)

![图](../Images/1e525079ab694b4045301028a5362803.png)

DBSCAN对球形数据的聚类

上述结果中的黑色数据点代表离群点。接下来，应用DBSCAN对非球形数据进行聚类。

![](../Images/7b63b835b30fbbbf9fdb264633ed815c.png)

![图](../Images/9a389869897afcf88efcddccd8c73c75.png)

DBSCAN对非球形数据的聚类

这完全是完美的。如果与K-means进行比较，它会给出完全不正确的结果，如下所示：

![图](../Images/7083a07ac0e7d75433e0c904a6bd8a6c.png)

K-means 聚类结果

# DBSCAN的复杂度

+   **最佳情况:** 如果使用索引系统存储数据集，以便邻域查询以对数时间执行，则平均运行时间复杂度为`**O(nlogn)**`。

+   **最坏情况:** 如果没有使用索引结构或在退化数据上（例如所有点在小于ε的距离内），最坏情况下的运行时间复杂度仍为`**O(*n*²)**`。

+   **平均情况:** 根据数据和算法实现，与最佳/最坏情况相同。

# 结论

基于密度的聚类算法可以学习任意形状的簇，借助Level Set Tree算法，可以在展示广泛密度差异的数据集中学习簇。

但是，我要指出，与像K-Means这样的参数化聚类算法相比，这些算法在调整时要复杂一些。比如DBSCAN的ε参数或Level Set Tree的参数相比于K-Means的簇数量参数，直观理解较难，因此为这些算法选择好的初始参数值更具挑战性。

本文到此为止。希望你们阅读愉快，请在评论区分享你的建议/观点/问题。

**感谢阅读！**

**个人简介: [Nagesh Singh Chauhan](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是CirrusLabs的大数据开发工程师。他在电信、分析、销售、数据科学等各个领域拥有超过4年的工作经验，专注于各种大数据组件。

[原文](https://medium.com/swlh/dbscan-clustering-algorithm-in-machine-learning-d465a4cca1e8)。经许可转载。

### 更多相关内容

+   [在Python中实现DBSCAN](https://www.kdnuggets.com/2022/08/implementing-dbscan-python.html)

+   [KDnuggets新闻，8月24日：在Python中实现DBSCAN • 如何……](https://www.kdnuggets.com/2022/n34.html)

+   [聚类揭示：理解K-Means聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [为你的数据集选择正确的聚类算法](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

+   [什么是K-Means聚类及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [使用scikit-learn进行聚类：无监督学习教程](https://www.kdnuggets.com/2023/05/clustering-scikitlearn-tutorial-unsupervised-learning.html)
