# 使用 scikit-learn 进行聚类：无监督学习教程

> 原文：[https://www.kdnuggets.com/2023/05/clustering-scikitlearn-tutorial-unsupervised-learning.html](https://www.kdnuggets.com/2023/05/clustering-scikitlearn-tutorial-unsupervised-learning.html)

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/b86ba87744e6c80502bec063c6ee062f.png)

图片由作者提供

聚类是一种流行的无监督机器学习技术，这意味着它用于目标变量或结果变量未提供的数据集。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在无监督学习中，算法的任务是识别数据中的模式和关系，而无需任何预先存在的知识或指导。

聚类的作用是什么？它将相似的数据点分组，使我们能够发现数据中的隐藏模式和关系。

在本文中，我们将探索可用的不同聚类算法及其各自的应用场景，以及用于评估聚类结果质量的重要评价指标。

我们还将演示如何使用流行的 Python 库 scikit-learn 同时开发多个聚类算法。

最后，我们将突出一些最著名的实际应用案例，讨论所使用的算法和评价指标。

但首先，让我们熟悉一下聚类算法。

# 聚类算法是什么？

以下是聚类算法的概述及简短定义。

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/940ae1dc6a569e725132ac247cef1cd2.png)

图片由作者提供

根据 scikit-learn 官方文档，现有 11 种不同的聚类算法：K-Means、Affinity propagation、Mean Shift、Special Clustering、Hierarchical Clustering、Agglomerative Clustering、DBScan、Optics、Gaussian Mixture、Birch、Bisecting K-Means。

[在这里](https://scikit-learn.org/stable/modules/clustering.html#clustering)你可以找到官方文档。

本节将探讨五种最著名和最重要的聚类算法。它们是 K-Means、Mean-Shift、DBScan、高斯混合模型和层次聚类。

在深入探讨之前，我们来看一下下面的图表。它展示了这五种算法在六个不同结构的数据集上的表现。

![使用 scikit-learn 的聚类：无监督学习教程](../Images/0bcfe93fee62fd50217fc1cd654635e0.png)

聚类算法 - 作者提供的图片

在 [scikit-learn 文档](https://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html) 中，您将找到类似的图形，这些图形启发了上面的图片。我将其限制在五种最著名的聚类算法上，并在算法名称旁边添加了数据集的结构，例如 K-Means - 噪声月牙或 K-Means 变化。

显示了六种不同的数据集，全部由 scikit-learn 生成：

+   **噪声圆形：** 该数据集由一个大圆和一个不完全居中的小圆组成。数据中还加入了随机的高斯噪声。

+   **噪声月牙：** 该数据集由两个交错的半月形状组成，这些形状不是线性可分的。数据中还加入了随机的高斯噪声。

+   **斑点：** 该数据集由随机生成的斑点组成，这些斑点在大小和形状上相对均匀。数据集包含三个斑点。

+   **无结构：** 该数据集由随机生成的数据点组成，没有固有的结构或聚类模式。

+   **各向异性分布：** 该数据集由随机生成的各向异性分布的数据点组成。这些数据点通过特定的变换矩阵生成，以使它们沿某些轴延长。

+   **变化：** 该数据集由具有不同方差的随机生成的斑点组成。数据集包含三个斑点，每个斑点具有不同的标准差。

查看这些图形以及每种算法在这些图形上的表现，将帮助我们比较算法在每个数据集上的性能。如果您的数据与这些图形中的结构相似，这可能会对您的数据项目有所帮助。

现在让我们深入了解这五种算法，从 K-Means 算法开始。

## K-Means

![使用 scikit-learn 的聚类：无监督学习教程](../Images/ffefd7b582ec70aa7df0aebbb28c4887.png)

K-Means 2D | 作者提供的图片

![使用 scikit-learn 的聚类：无监督学习教程](../Images/bf81918b942555beb2292df55e50fd6f.png)

K Means 3D | 作者提供的图片

K-Means 是一种流行的聚类算法，它将数据集划分为 K 个不同的聚类，其中 K 是用户指定的超参数。

该算法通过将每个数据点分配到最近的聚类中心，并根据该聚类中所有数据点的平均值重新计算中心点来工作。

该过程持续进行，直到中心点不再移动或达到指定的最大迭代次数。

它还被应用于各种实际场景，如电子商务中的客户细分、医疗保健中的疾病聚类以及计算机视觉中的图像压缩。

要了解 K-Means 或其他算法的实际应用，请继续阅读。我们将在后续部分讨论这些内容。

## DBScan

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/8de3494c09f041a20ae000b8efad224d.png)

DBScan | 图片由作者提供

DBScan（基于密度的空间聚类与噪声）是一种基于密度的聚类算法，通过将高密度区域与低密度区域分隔开来识别簇。

算法根据密度阈值和最小点数将彼此接近的点分组在一起。

DBScan 常用于异常检测、空间聚类和图像分割，目标是识别数据中的不同簇，同时处理噪声或异常数据点。

## 层次聚类

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/682c33f0e069426fb4d86fd57143bc4c.png)

层次聚类 | 图片由作者提供

层次聚类是一种通过合并或分裂簇来构建树状结构的聚类算法。

根据采用的方法，算法可以是聚合型（自下而上）或分裂型（如上图所示）。

层次聚类已在各种实际应用中使用，如社交网络分析、图像分割和生态研究，其目标是识别簇与子簇之间的有意义关系。

## Mean-Shift 聚类

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/ef77715493326f95d741b11f5255109b.png)

Mean-Shift 聚类 | 图片由作者提供

Mean-shift 聚类是一种非参数算法，不需要对簇的形状或数量进行先验假设。

该算法通过将每个数据点向局部均值（上图中的 x）移动直到收敛来工作，其中核密度函数估计局部均值。

Mean-shift 算法将簇识别为特征空间中的高密度区域。

Mean-shift 聚类已在实际应用中使用，例如图像分割、视频监控中的物体跟踪以及 [网络入侵检测中的异常检测](https://www.stratascratch.com/blog/machine-learning-algorithms-explained-anomaly-detection/?utm_source=blog&utm_medium=click&utm_campaign=kdn+clustering+unsupervised+learning)，其目标是识别数据中的不同区域或模式。

## 高斯混合聚类

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/65fb3a8e31358038c73e9495d08d855d.png)

高斯混合模型在月牙形数据集中的应用 | 图片由作者提供 ![使用 scikit-learn 进行聚类：无监督学习教程](../Images/d66db08ceabfe04abbe8b9f81fb9462d.png)

K Means 聚类模型在月牙形数据集中的应用 | 图片由作者提供

高斯混合模型是一种概率聚类算法，通过高斯分布的混合来建模数据点的分布。该算法将一组高斯分布拟合到数据上，其中每个高斯分布对应一个独立的簇。

高斯混合模型已在各种实际应用中使用，如语音识别、基因表达分析和面部识别，其目标是对数据的潜在分布进行建模，并根据拟合的高斯分布识别聚类。

从上图可以看出，高斯混合模型在捕捉椭圆形数据点的趋势和绘制椭圆形聚类方面具有更好的能力。

总体而言，每种聚类算法都有其独特的优缺点。算法的选择取决于具体问题和数据集的特征。

理解每种算法的细微差别及其使用场景对于实现准确和有意义的结果至关重要。

# 聚类评估指标

应用算法后，你需要评估其性能，以查看是否有改进的空间，或在算法性能不符合标准时更改算法。为此，你应该使用评估指标。

这里是最流行的一些概述。

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/de76f99ec499d990675696d2f5dff256.png)

图片来源：作者

当然，这些并不是全部。你可以在 scikit-learn 上获取完整的列表。它列出了以下评估指标：Rand 指数、基于互信息的分数、轮廓系数、Fowlkes-Mallows 分数、同质性、完整性、V 计量、Calinski-Harabasz 指数、Davies-Bouldin 指数、混淆矩阵、配对混淆矩阵。

[这里](https://scikit-learn.org/stable/modules/clustering.html#clustering)你可以查看官方文档。

我们将坚持使用流行的算法，并从 Rand 指数开始。

## Rand 指数

Rand 指数评估真实聚类标签与预测聚类标签之间的相似性。

该指数的范围从 0 到 1，其中 1 表示真实标签和预测标签之间的完美匹配。

Rand 指数通常用于图像分割、文本聚类和文档聚类等领域，其中数据的真实标签是已知的。

## 轮廓系数

轮廓系数基于聚类的分离程度和每个聚类内部数据点的相似度来衡量聚类的质量。

该系数的范围从 -1 到 1，其中 1 表示聚类分离良好且紧凑，-1 表示聚类不正确。

轮廓系数通常用于市场细分、客户画像和产品推荐，其目标是基于客户行为和偏好识别有意义的聚类。

## Fowlkes-Mallows 分数

Fowlkes-Mallows 指数以两位研究人员的名字命名，Edward Fowlkes 和 S.G. Mallows，他们在 1983 年提出了这一指标。

该指数衡量聚类算法的真实标签与预测标签之间的相似性。

该分数的范围从 0 到 1，其中 1 表示真实标签和预测标签之间的完美匹配。

Fowlkes-Mallows 分数通常用于图像分割、文本聚类和文档聚类，其中数据的真实标签是已知的。

## Davies-Bouldin 指数

Davies-Bouldin 指数以两位研究人员 David L. Davies 和 Donald W. Bouldin 的名字命名，他们在 1979 年提出了这一指标。

该指数的范围从 0 到无穷大，较低的值表示更好的聚类质量。

它对于识别数据中的最佳簇数量和检测簇重叠或彼此过于相似的情况非常有用。然而，该指数假设簇是球形的且具有相似的密度，这在现实世界的数据集中可能并不总是成立。

Davies-Bouldin 指数通常用于市场细分、客户画像和产品推荐，其目标是基于客户行为和偏好识别有意义的簇。

## Calinski-Harabasz 指数

Calinski-Harabasz 指数以 T. Calinski 和 J. Harabasz 的名字命名，他们在 1974 年提出了这一指标。

Calinski-Harabasz 指数根据簇之间的分离程度和每个簇内的数据点相似度来衡量聚类质量。

该指数的范围从 0 到无穷大，较高的值表示更好的聚类效果。

Calinski-Harabasz 指数通常用于市场细分、客户画像和产品推荐，其目标是基于客户行为和偏好识别有意义的簇。

## 比较评价指标

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/853188381b0cb4016331ac5d79a6a165.png)

图片作者

对于 Rand 指数、轮廓系数和 Fowlkes-Mallows 分数，较高的值表示更好的聚类性能。

最佳得分是 1。

对于 Davies-Bouldin 指数，较低的值表示更好的聚类性能。

最佳得分是 0。

对于 Calinski-Harabasz 指数，最高得分表示更好的性能。

最佳得分是 ∞。（无穷大。）

理论上，Calinski-Harabasz (CH) 指数的最佳得分应该是无穷大，因为这表明簇间离散度远高于簇内离散度。然而，在实践中实现无穷大的 CH 指数值是不现实的。

最佳得分没有固定的上限，因为它依赖于具体的数据和聚类情况。

别忘了：没有哪个算法或脚本是完美的。如果你在这些评价指标中达到了最佳得分，你的模型很可能存在过拟合。

# 如何在 scikit-learn 中一次性开发多个聚类算法？

这里的目的是对 Iris 数据集应用多个聚类算法，并使用不同的评价指标计算其性能。

在这里我们将使用 IRIS 数据集。

你可以在 [这里](https://archive.ics.uci.edu/ml/datasets/iris) 找到这个数据集。

鸢尾花数据集是一个著名的多类分类数据集，包含 150 个鸢尾花样本，每个样本具有四个特征（萼片和花瓣的长度和宽度）。

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/612e90b34d5c306110f2a628231861e9.png)

图片来源 [R 语言机器学习入门](https://www.datacamp.com/community/tutorials/machine-learning-in-r)

数据集中有三个类别，代表三种不同类型的鸢尾花。

数据集通常用于机器学习和模式识别任务，特别是用于测试和比较不同的[分类算法](https://www.stratascratch.com/blog/overview-of-machine-learning-algorithms-classification/?utm_source=blog&utm_medium=click&utm_campaign=kdn+clustering+unsupervised+learning)。它由英国统计学家和生物学家 Ronald Fisher 于 1936 年引入。

这里我们将编写代码，导入加载数据集所需的库，实施五种聚类算法（DBSCAN、K-Means、层次聚类、高斯混合模型和均值漂移），并使用五个指标评估它们的性能。

为此，我们将把评估指标和算法添加到字典中，并用两个 for 循环相互应用它们。

但我们这里有一个例外。**rand_score** 和 **fowlkes_mallows_score** 函数将聚类结果与真实标签进行比较，因此我们将添加**if-else 块**来提供这些结果。

然后我们将把这些结果添加到数据框中，以进行进一步分析。

这是代码。

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.cluster import (
    DBSCAN,
    KMeans,
    AgglomerativeClustering,
    MeanShift,
)
from sklearn.mixture import GaussianMixture
from sklearn.metrics import (
    silhouette_score,
    calinski_harabasz_score,
    davies_bouldin_score,
    rand_score,
    fowlkes_mallows_score,
)

# Load the Iris dataset
iris = load_iris()
X = iris.data
y = iris.target

# Implement clustering algorithms
dbscan = DBSCAN(eps=0.5, min_samples=5)
kmeans = KMeans(n_clusters=3, random_state=42)
agglo = AgglomerativeClustering(n_clusters=3)
gmm = GaussianMixture(n_components=3, covariance_type="full")
ms = MeanShift()

# Evaluate clustering algorithms with three evaluation metrics
labels = {
    "DBSCAN": dbscan.fit_predict(X),
    "K-Means": kmeans.fit_predict(X),
    "Hierarchical": agglo.fit_predict(X),
    "Gaussian Mixture": gmm.fit_predict(X),
    "Mean Shift": ms.fit_predict(X),
}

metrics = {
    "Silhouette Score": silhouette_score,
    "Calinski Harabasz Score": calinski_harabasz_score,
    "Davies Bouldin Score": davies_bouldin_score,
    "Rand Score": rand_score,
    "Fowlkes-Mallows Score": fowlkes_mallows_score,
}

for name, label in labels.items():
    for metric_name, metric_func in metrics.items():
        if metric_name in ["Rand Score", "Fowlkes-Mallows Score"]:
            score = metric_func(y, label)
        else:
            score = metric_func(X, label)
        pred_df = pred_df.append(
            {"Algorithm": name, "Metric": metric_name, "Score": score},
            ignore_index=True,
        )
# Display the DataFrame
pred_df.head(10) 
```

这是输出结果。

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/1d90f8f90ddb9442c4fdffbaa5ca69be.png)

预测数据框 | 图片由作者提供

现在，让我们制作一个可视化图表，以更好地查看结果。这里的目的是创建聚类算法评估指标的可视化图。

以下代码将数据透视为算法作为列，指标作为行，然后为每个指标生成条形图。这允许对不同评估指标的聚类算法性能进行轻松比较。

这是代码。

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Pivot the data to have algorithms as columns and metrics as rows
pivoted_df = pred_df.pivot(
    index="Metric", columns="Algorithm", values="Score"
)

# Define the three metrics to plot
metrics = [
    "Silhouette Score",
    "Calinski Harabasz Score",
    "Davies Bouldin Score",
]

# Define a colormap to use for each algorithm
cmap = plt.get_cmap("Set3")

# Plot a bar chart for each metric
fig, axs = plt.subplots(nrows=1, ncols=3, figsize=(15, 5))

# Add a main title to the figure
fig.suptitle("Comparing Evaluation Metrics", fontsize=16, fontweight="bold")

for i, metric in enumerate(metrics):
    ax = pivoted_df.loc[metric].plot(kind="bar", ax=axs[i], rot=45)
    ax.set_xticklabels(ax.get_xticklabels(), ha="right")
    ax.set_ylabel(metric)
    ax.set_title(metric, fontstyle="italic")

    # Iterate through the algorithm names and set the color for each bar
    for j, alg in enumerate(pivoted_df.columns):
        ax.get_children()[j].set_color(cmap(j))
plt.show()
```

这是输出结果。

![使用 scikit-learn 进行聚类：无监督学习教程](../Images/2f18af9a20aa92829646aad6cb366d7c.png)

图片由作者提供

总结来说，均值漂移算法根据轮廓系数和戴维斯-鲍尔丁分数表现最佳。

K-Means 算法在 Calinski Harabasz 分数上表现最佳，而 GMM 算法在 Rand 分数和 Fowlkes-Mallows 分数上表现最佳。

聚类算法之间没有明显的赢家，因为每种算法在不同的指标上表现良好。

最佳算法的选择取决于具体需求以及在聚类问题中对每个评估指标的重要性分配。

# 聚类的实际应用

现在，让我们看看我们算法和评估指标的实际例子，以进一步掌握逻辑。

这里是我们将详细讨论的例子的概述。

![使用scikit-learn进行聚类：无监督学习教程](../Images/972c3d2614ebd9a056c3dcbcf4f2fac6.png)

作者提供的图像

## 超市连锁个性化

![使用scikit-learn进行聚类：无监督学习教程](../Images/f4b3799950da5654b6fde265d9a6bf28.png)

作者提供的图像

**实际例子**：一家超市连锁希望为其客户创建个性化的营销活动。他们使用K-Means聚类根据客户的购买习惯、人口统计数据和店铺访问频率来细分客户。这些细分帮助公司量身定制营销信息，以更好地吸引和服务客户。

**算法：** K-Means聚类

K-Means被选择是因为它是一个简单、高效且广泛使用的聚类算法，适用于大数据集。它可以快速识别模式并根据输入特征创建明确的客户细分。

**评估指标：** Silhouette Score

Silhouette Score用于评估客户细分的质量，通过测量每个数据点在其分配的聚类内的适配度，与其他聚类进行比较。这有助于确保聚类是紧凑且分离良好的，这对于创建有效的个性化营销活动至关重要。

## 欺诈交易

![使用scikit-learn进行聚类：无监督学习教程](../Images/adfd49556ef14f99235136dfb81e5d4c.png)

作者提供的图像

**实际例子**：一家信用卡公司希望检测欺诈交易。他们使用DBSCAN根据交易金额、时间和地点等因素对交易进行聚类。那些不符合任何聚类的异常交易会被标记为潜在欺诈，进行进一步调查。

**算法：** DBSCAN

DBSCAN被选择是因为它是一个基于密度的聚类算法，能够识别形状和大小各异的聚类，并检测不属于任何聚类的噪声点。这使它适合检测异常模式或离群点，如潜在的欺诈交易。

**评估指标：** Silhouette Score

选择Silhouette Score作为评估指标是因为它有助于评估DBSCAN的有效性，通过将正常交易与表示欺诈的潜在离群点分开。

更高的Silhouette Score表示常规交易的聚类之间分隔良好，噪声点（离群点）也被有效分离。这种分离使得识别和标记显著偏离正常模式的可疑交易变得更容易。

## 癌症基因组学关系

![使用scikit-learn进行聚类：无监督学习教程](../Images/5e4d4ab765bfbe4f23aabb53260eb6eb.png)

作者提供的图像

**现实生活中的例子**：研究癌症基因组学的研究人员希望理解不同类型癌细胞之间的关系。他们使用层次聚类根据基因表达模式对细胞进行分组。得到的簇帮助他们识别癌症类型之间的共性和差异，并开发针对性的治疗方法。

**算法**：凝聚层次聚类

选择凝聚层次聚类是因为它创建了一个树状结构（树状图），这使研究人员能够以多层次的粒度可视化和解释癌细胞之间的关系。这种方法可以揭示嵌套的子群体，并帮助研究人员理解基于基因表达模式的癌症类型的层次组织。

**评估指标**：Calinski-Harabasz指数

选择Calinski-Harabasz指数是因为它测量了簇间离散度与簇内离散度的比率。对于癌症基因组学，它帮助研究人员评估基于基因表达模式的癌细胞分组的质量，判断组间是否明显且分隔良好。

## 自主驾驶汽车

![使用scikit-learn的聚类：无监督学习教程](../Images/d065fcf84da57950cd29cdcf02083f70.png)

作者提供的图像

**现实生活中的例子**：一家自动驾驶汽车公司希望提高汽车识别周围物体的能力。他们使用均值漂移聚类将汽车摄像头捕捉的图像根据颜色和纹理分割成不同的区域，这帮助汽车识别和跟踪行人及其他车辆等物体。

**算法**：均值漂移聚类

选择均值漂移聚类是因为它是一种非参数的基于密度的算法，可以自动适应数据的底层结构和规模。

这使得它特别适合于图像分割任务，其中簇或区域的数量可能事先未知，并且区域的形状可能复杂且不规则。

**评估指标**：Fowlkes-Mallows评分（FMS）

选择Fowlkes-Mallows评分是因为它测量了两个聚类之间的相似性，通常将算法的输出与真实的聚类进行比较。

在自动驾驶汽车的背景下，FMS可以用于评估均值漂移聚类算法在分割图像方面的效果，相较于人工标注的分割结果。

## 新闻推荐

![使用scikit-learn的聚类：无监督学习教程](../Images/0b50b4b60373daaae7dd97f598d463d8.png)

作者提供的图像

**实际案例**：一个在线新闻平台希望将文章分组，以改善对用户的内容推荐。他们使用Gaussian Mixture Models根据从文本中提取的特征（如词频和术语共现）对文章进行聚类。通过识别不同的主题，该平台可以推荐与用户兴趣更相关的文章。

**算法**：Gaussian Mixture Model (GMM) 聚类

选择Gaussian Mixture Models是因为它们是一种概率生成方法，可以建模复杂的重叠簇。这对文本数据特别有用，因为文章可能属于多个主题或具有共享特征。GMM可以捕捉这些细微差别，并提供软聚类，为每篇文章分配属于每个主题的概率。

**评估指标**：Silhouette Coefficient

选择Silhouette Coefficient是因为它衡量了簇的紧密性和分离度，有助于评估主题分配的质量。

更高的Silhouette Coefficient表明一个主题内的文章彼此更相似，并且与其他主题明显不同，这对准确的内容推荐至关重要。

如果你想了解更多关于无监督算法的信息，你可以在这里获取更多资料：“[Unsupervised Learning Algorithms](https://www.stratascratch.com/blog/overview-of-machine-learning-algorithms-unsupervised-learning/?utm_source=blog&utm_medium=click&utm_campaign=kdn+clustering+unsupervised+learning)”。还可以查看“[Supervised vs Unsupervised Learning](https://www.stratascratch.com/blog/supervised-vs-unsupervised-learning/?utm_source=blog&utm_medium=click&utm_campaign=kdn+clustering+unsupervised+learning)”，这两种方法是机器学习领域我们应该了解的。

# 结论

总之，聚类是一种重要的无监督学习技术，用于在没有预先了解类标签的情况下发现数据中的相似性或模式。

我们讨论了不同的聚类算法，包括K-Means、Mean Shift、DBScan、Gaussian Mixture和Hierarchical Clustering，以及它们的应用场景和实际应用。

此外，我们还探索了各种评估指标，包括Silhouette Coefficient、Calinski-Harabasz Index和Davies-Bouldin Index，这些指标帮助我们评估聚类结果的质量。

我们还学习了如何使用scikit-learn同时开发多个聚类算法，并使用我们已发现的指标对其进行评估。

最后，我们讨论了一些利用聚类算法解决实际问题的热门应用。

如果你还有问题，这里有一篇文章解释了[Clustering and its algorithms](https://www.stratascratch.com/blog/machine-learning-algorithms-explained-clustering/?utm_source=blog&utm_medium=click&utm_campaign=kdn+clustering+unsupervised+learning)。

聚类有广泛的应用，从市场营销中的客户细分到计算机视觉中的图像识别，它是发现数据中隐藏模式和洞察的**重要工具**。

**[内特·罗西迪](https://www.stratascratch.com)** 是一名数据科学家和产品策略专家。他还是一名兼任教授，教授分析学，并且是[StrataScratch](https://www.stratascratch.com/)，一个帮助数据科学家准备顶级公司面试问题的平台的创始人。可以通过[Twitter: StrataScratch](https://twitter.com/StrataScratch)或[LinkedIn](https://www.linkedin.com/in/nathanrosidi/)与他联系。

### 更多相关主题

+   [无监督学习实操：K均值聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [聚类大解放：理解K均值聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [无监督的解缠表示学习在类别不平衡数据集中的应用](https://www.kdnuggets.com/2023/01/unsupervised-disentangled-representation-learning-class-imbalanced-dataset-elastic-infogan.html)

+   [探索无监督学习指标](https://www.kdnuggets.com/2023/04/exploring-unsupervised-learning-metrics.html)

+   [揭示无监督学习](https://www.kdnuggets.com/unveiling-unsupervised-learning)

+   [机器学习中的DBSCAN聚类算法](https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html)
