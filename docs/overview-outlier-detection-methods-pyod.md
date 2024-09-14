# 来自 PyOD 的异常检测方法概述 – 第 1 部分

> 原文：[https://www.kdnuggets.com/2019/06/overview-outlier-detection-methods-pyod.html](https://www.kdnuggets.com/2019/06/overview-outlier-detection-methods-pyod.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

Matthew Mayo 以前的一篇文章标题为：[异常检测方法的直观可视化](/2019/02/outlier-detection-methods-cheat-sheet.html) 概述了由 [Yue Zhao](https://www.yuezhao.me/) 开发的 [PyOD](https://github.com/yzhao062/pyod) 包。

本文将从这里继续，形成一个多部分系列，概述包中提供的不同异常检测方法。我将使用 PyOD 文档中的信息，以避免因重叠技术而造成混淆。

### [1\. PCA (主成分分析)](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.pca)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

```py
pyod.models.pca.PCA(n_components=None, n_selected_components=None, contamination=0.1, copy=True, whiten=False, svd_solver='auto', tol=0.0, iterated_power='auto', random_state=None, weighted=True, standardization=True)

```

PCA 是一种线性降维方法，通过对数据进行奇异值分解，将其投影到较低维空间。在此过程中，数据的协方差矩阵可以分解为与特征值相关的正交向量，称为特征向量。具有较高特征值的特征向量捕捉数据中的大部分方差。

因此，由 k 个特征向量构建的低维超平面可以捕捉数据中的大部分方差。然而，异常点与正常数据点不同，这在由特征值较小的特征向量构建的超平面上更为明显。因此，可以将样本在所有特征向量上的投影距离之和作为异常分数。

### [2\. MCD (最小协方差行列式)](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.mcd)

```py
pyod.models.mcd.MCD(contamination=0.1, store_precision=True, assume_centered=False, support_fraction=None, random_state=None)

```

最小协方差行列式协方差估计器用于高斯分布的数据，但也可能对从单峰对称分布中提取的数据仍然相关。它不适用于多模态数据（在这种情况下，用于拟合 MinCovDet 对象的算法可能会失败）。应考虑使用投影追寻方法来处理多模态数据集。

### [3\. OCSVM (一类支持向量机)](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.ocsvm)

```py
pyod.models.ocsvm.OCSVM(kernel='rbf', degree=3, gamma='auto', coef0=0.0, tol=0.001, nu=0.5, shrinking=True, cache_size=200, verbose=False, max_iter=-1, contamination=0.1)

```

这是一种无监督学习的异常检测方法。OCSVM 算法将输入数据映射到高维特征空间（通过核函数），并迭代地寻找最佳的最大间隔超平面，以最有效地将训练数据与原点分离。

### [4\. LOF（局部异常因子）](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.lof)

```py
pyod.models.lof.LOF(n_neighbors=20, algorithm='auto', leaf_size=30, metric='minkowski', p=2, metric_params=None, contamination=0.1, n_jobs=1)

```

每个样本的异常评分称为局部异常因子。它衡量给定样本相对于其邻居的局部密度偏差。局部性在于异常评分依赖于对象与周围邻域的隔离程度。更准确地说，局部性由k近邻确定，其距离用于估计局部密度。通过将样本的局部密度与其邻居的局部密度进行比较，可以识别出那些密度明显低于其邻居的样本。这些样本被视为异常值。

**相关内容：**

+   [异常检测方法的直观可视化](/2019/02/outlier-detection-methods-cheat-sheet.html)

+   [异常检测的四种技术](/2018/12/four-techniques-outlier-detection.html)

+   [如何使你的机器学习模型对异常值具有鲁棒性](/2018/08/make-machine-learning-models-robust-outliers.html)

### 更多相关内容

+   [功能数据中异常检测的密度核深度](https://www.kdnuggets.com/density-kernel-depth-for-outlier-detection-in-functional-data)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [Python 字符串方法](https://www.kdnuggets.com/2022/12/python-string-methods.html)

+   [数据科学方法推动业务成功](https://www.kdnuggets.com/2023/10/nwu-data-science-methods-drive-business-success)

+   [每个程序员都应该知道的11种 Python 魔法方法](https://www.kdnuggets.com/11-python-magic-methods-every-programmer-should-know)
