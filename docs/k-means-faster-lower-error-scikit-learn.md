# K-Means 比 Scikit-learn 快 8 倍，误差低 27 倍，代码仅需 25 行

> 原文：[https://www.kdnuggets.com/2021/01/k-means-faster-lower-error-scikit-learn.html](https://www.kdnuggets.com/2021/01/k-means-faster-lower-error-scikit-learn.html)

[评论](#comments)

**由[Jakub Adamczyk](https://github.com/j-adamczyk)，计算机科学学生，Python 和机器学习爱好者**。

![](../Images/a2f968618e10547fcf2c6c6a6bbac309.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

在[我上一篇关于 faiss 库的文章](https://towardsdatascience.com/make-knn-300-times-faster-than-scikit-learns-in-20-lines-5e29d74e76bb)中，我展示了如何通过使用[Facebook 的 faiss 库](https://github.com/facebookresearch/faiss)将 kNN 加速至比 Scikit-learn 快 300 倍，仅需 20 行代码。但我们可以用它做更多的事情，包括更快且更准确的 K-Means 聚类，代码仅需 25 行！

K-Means 是一种迭代算法，它将数据点聚类到 *k* 个簇中，每个簇由一个均值/中心点（质心）表示。训练从一些初始猜测开始，然后在两个步骤之间交替进行：分配和更新。

在分配阶段，我们将每个点分配到最近的簇（使用点与质心之间的欧几里得距离）。在更新步骤中，我们通过计算当前步骤中分配给该簇的所有点的均值来重新计算每个质心。

聚类的最终质量计算为簇内距离的总和，对于每个簇，我们计算该簇内点与其质心之间的欧几里得距离的总和。这也称为惯性。

对于预测，我们在新点和质心之间执行 1-最近邻搜索（kNN，*k* = 1）。

### Scikit-learn 与 faiss

在这两个库中，我们需要指定算法的超参数：簇的数量、重启的次数（每次从不同的初始猜测开始）以及最大迭代次数。

从例子中可以看出，该算法的核心是搜索最近邻，特别是最近的质心，适用于训练和预测。这也是 faiss 比 Scikit-learn 快几个数量级的地方！它利用了出色的 C++ 实现、尽可能的并发，甚至可以使用 GPU（如果你需要的话）。

### 使用 faiss 实现 K-Means 聚类

faiss的一个伟大特点是它提供了安装和构建说明以及带有示例的优秀文档。安装后，我们可以编写实际的聚类代码。代码相当简单，因为我们只是模仿Scikit-learn的API。

重要元素：

+   faiss有一个内置的*Kmeans*类专门用于此任务，但其参数的名称与Scikit-learn不同（参见[文档](https://github.com/facebookresearch/faiss/wiki/Faiss-building-blocks:-clustering,-PCA,-quantization)）。

+   我们必须确保使用*np.float32*类型，因为faiss仅支持这种类型。

+   *kmeans.obj*通过训练返回一个误差列表，因此为了得到最终的误差，就像在Scikit-learn中一样，我们使用[-1]索引。

+   预测是通过*Index*数据结构完成的，这是faiss的基本构建块，所有的最近邻搜索都使用它。

+   在预测中，我们进行kNN搜索，*k* = 1，返回*自.cluster_centers_*中的最近质心的索引（索引[1]，因为*index.search()*返回距离和索引）。

### 时间和准确性比较

我选择了一些在Scikit-learn中可用的流行数据集进行比较。比较了训练和预测时间。为了更容易阅读，我明确写出基于faiss的聚类比Scikit-learn快多少倍。为了比较误差，我只写了基于faiss的聚类实现了多少倍的更低误差（因为数字较大且不太具参考性）。

所有这些时间都是通过*time.process_time()*函数测量的，该函数测量进程时间而非挂钟时间，以获得更准确的结果。结果是100次运行的平均值，除了MNIST，因为Scikit-learn花费的时间太长，我只做了5次运行。

![](../Images/c5ed1c5e3c332b11b91dc3e0e19051d7.png)

*训练时间（图像由作者提供）。*

![](../Images/325ecf334311ed067d590720ead00ade.png)

*预测时间（图像由作者提供）。*

![](../Images/c00dfc80bbbc4290db07ab33415e23d9.png)

*训练误差（图像由作者提供）。*

如我们所见，对于小数据集的K-Means聚类（前4个数据集），基于faiss的版本训练速度较慢且误差较大。对于预测，它的表现普遍更快。

对于较大的MNIST数据集，faiss明显胜出。训练速度快20.5倍是巨大的，特别是因为它将时间从将近3分钟减少到不到8秒！预测速度快1.5倍也是不错的。然而，真正的成就的是误差降低了27.5倍。这意味着对于较大的现实世界数据集，基于faiss的版本准确性高得多。而且这只需要25行代码！

基于此：如果你有一个大（至少几千个样本）的现实世界数据集，基于faiss的版本就更好。对于小的玩具数据集，Scikit-learn是更好的选择；然而，如果你有GPU，GPU加速的faiss版本可能会更快（我还未检查过以确保公平的CPU比较）。

### 总结

通过25行代码，我们可以利用faiss库为K-Means聚类提供显著的速度和准确性提升，适用于合理大小的数据集。如果需要，你可以使用GPU、多个GPU等进一步提高，faiss文档中对此做了很好的解释。

[原文](https://towardsdatascience.com/k-means-8x-faster-27x-lower-error-than-scikit-learns-in-25-lines-eaedc7a3a0c8)。已获许可转载。

**相关：**

+   [数据科学关键算法解释：从k-means到k-medoids聚类](https://www.kdnuggets.com/2020/12/algorithms-explained-k-means-k-medoids-clustering.html)

+   [K-means聚类算法完整指南](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html)

+   [KNN中最受欢迎的距离度量及其使用时机](https://www.kdnuggets.com/2020/11/most-popular-distance-metrics-knn.html)

### 更多相关话题

+   [成为优秀数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [少于15行代码实现多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [停止学习数据科学以找到目标，然后找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
