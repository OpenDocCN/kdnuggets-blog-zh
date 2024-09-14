# 基于密度的空间聚类（DBSCAN）

> 原文：[https://www.kdnuggets.com/2017/10/density-based-spatial-clustering-applications-noise-dbscan.html](https://www.kdnuggets.com/2017/10/density-based-spatial-clustering-applications-noise-dbscan.html)

**由 [Abhijit Annaldas](https://www.linkedin.com/in/avannaldas/), Microsoft 提供。**

DBSCAN 是一种不同类型的聚类算法，具有一些独特的优势。正如名称所示，该方法更加关注观测值的接近度和密度来形成簇。这与 K-Means 很不同，在 K-Means 中，观测值成为由最近的中心点表示的簇的一部分。DBSCAN 聚类可以识别离群值，即不会属于任何簇的观测值。由于 DBSCAN 聚类也能够识别簇的数量，因此在我们不知道数据中可能有多少个簇时，它在无监督学习中非常有用。

K-Means 聚类可能将松散相关的观测值聚集在一起。即使观测值在向量空间中相距甚远，每个观测值最终也会成为某个簇的一部分。由于簇依赖于簇元素的均值，每个数据点在形成簇的过程中发挥作用。数据点的微小变化 *可能* 会影响聚类结果。由于簇的形成方式，DBSCAN 大大减少了这个问题。

在 DBSCAN 中，聚类发生基于两个重要参数，即：

+   **邻域 (n)** - 点与（核心点 - 下面讨论）之间的截止距离，以使其被视为簇的一部分。通常称为 *epsilon*（缩写为 *eps*）。

+   **最小点数 (m)** - 形成簇所需的最小点数。通常称为 *minPts*。

DBSCAN 聚类完成后，会出现三种类型的点，即：

+   **核心** - 这是一个点，该点自身距离 *n* 内至少有 *m* 个点。

+   **边界** - 这是一个点，该点至少与一个核心点的距离为 *n*。

+   **噪声** - 这是一个既不是核心点也不是边界点的点。它在距离 *n* 内的点少于 *m* 个。

DBSCAN 聚类可以总结为以下步骤...

1.  对数据集中的每个点 *P*，识别距离 *n* 内的点 *pts*。

    +   如果 *pts* >= *m*，将 *P* 标记为 *核心* 点

    +   如果 *pts* < *m* 且核心点距离 *n*，将 *P* 标记为 *边界* 点

    +   如果 *pts* < *m*，将 *P* 标记为 *噪声* 点

1.  为了便于解释，假设 **一个 *核心* 点及其距离 *n* 内的所有点** 组成一个核心集。所有重叠的核心集被分组到一个簇中。就像多个单独的图被连接形成一个连通图的集合。

![](../Images/4947a12ea2689da7bb4973976189d64c.png)

由于聚类完全依赖于参数 *n* 和 *m*（如上所述），正确选择这些值非常重要。虽然对该领域有较好的领域知识有助于选择这些参数的良好值，但也有一些方法可以在没有深厚领域专业知识的情况下相对准确地估算这些参数。

查看 DBSCAN 演示于 [sklearn 示例](http://scikit-learn.org/stable/auto_examples/cluster/plot_dbscan.html) 并尝试使用 [sklearn.cluster.DBSCAN](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html) 在 sklearn 中实现。

**个人简介：[Abhijit Annaldas](https://www.linkedin.com/in/avannaldas/)** 是一名软件工程师和狂热学习者，他在机器学习领域获得了相当程度的知识和专业技能。他通过学习新事物和不懈练习，日益提高自己的专业能力，并且自 2012 年 6 月以来，在微软印度担任软件工程师，拥有丰富的经验，构建了多种企业级应用，涉及多种 Microsoft 和开源技术。

[原文](http://abhijitannaldas.com/dbscan-clustering-in-machine-learning.html)。经许可转载。

**相关：**

+   [用 Python 和 SciPy 比较距离测量](/2017/08/comparing-distance-measurements-python-scipy.html)

+   [必须知道：如何确定最有用的聚类数？](/2017/05/must-know-most-useful-number-clusters.html)

+   [文本聚类：从非结构化数据中获取快速洞察](/2017/06/text-clustering-unstructured-data.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [机器学习中的 DBSCAN 聚类算法](https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html)

+   [在 Python 中实现 DBSCAN](https://www.kdnuggets.com/2022/08/implementing-dbscan-python.html)

+   [KDnuggets 新闻，8 月 24 日：在 Python 中实现 DBSCAN • 如何…](https://www.kdnuggets.com/2022/n34.html)

+   [解锁聚类：理解 K-Means 聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [前 7 大扩散基应用及演示](https://www.kdnuggets.com/2022/10/top-7-diffusionbased-applications-demos.html)

+   [利用密度链提示解锁 GPT-4 摘要](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)
