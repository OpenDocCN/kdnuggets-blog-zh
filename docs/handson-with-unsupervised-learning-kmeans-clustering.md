# 动手实践：无监督学习中的 K-Means 聚类

> 原文：[`www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering`](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

![动手实践：无监督学习中的 K-Means 聚类](img/6c271a0a353692c348f98759efcd5a59.png)

作者提供的图片

K-Means 聚类是数据科学中最常用的无监督学习算法之一。它用于根据数据点之间的相似性自动将数据集划分为簇或组。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

在这个简短的教程中，我们将学习 K-Means 聚类算法的工作原理，并使用 scikit-learn 将其应用于实际数据。此外，我们还将可视化结果以理解数据分布。

# 什么是 K-Means 聚类？

K-Means 聚类是一种无监督机器学习算法，用于解决聚类问题。该算法的目标是发现数据中的组或簇，簇的数量由变量 K 表示。

**K-Means 算法的工作原理如下：**

1.  指定你希望数据分组的簇数 K。

1.  随机初始化 K 个聚类中心或质心。这可以通过随机选择 K 个数据点作为初始质心来完成。

1.  根据欧几里得距离将每个数据点分配给最近的聚类中心。与给定中心最近的数据点被视为该簇的一部分。

1.  通过计算分配到该簇的所有数据点的均值来重新计算聚类中心。

1.  重复步骤 3 和 4，直到中心点停止移动或迭代达到指定的限制。当算法收敛时，即可完成。

![动手实践：无监督学习中的 K-Means 聚类](img/c0e044b06cd030678cea310f7a36cd7e.png)

Gif [由 Alan Jeffares 提供](https://towardsdatascience.com/k-means-a-complete-introduction-1702af9cd8c)

K-Means 的目标是最小化数据点与其分配的聚类中心之间的平方距离总和。通过迭代地将数据点重新分配给最近的中心并将中心移动到其分配点的中心，从而实现更紧凑且分离的聚类。

# K-Means 聚类的实际应用示例

在这些示例中，我们将使用 [商场顾客细分](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) 数据，并应用 K-Means 算法。我们还将使用肘部法则找到最佳的 **K**（簇数）并可视化簇。

## 数据加载

我们将使用 pandas 加载 CSV 文件，并将“CustomerID”设置为索引。

```py
import pandas as pd

df_mall = pd.read_csv("Mall_Customers.csv",index_col="CustomerID")
df_mall.head(3)
```

数据集有 4 列，我们只对其中三列感兴趣：顾客的年龄、年收入和消费得分。

![XXXXX](img/05d61632ccda918cab988849f0140354.png)

## 可视化

为了可视化所有四列，我们将使用 seaborn 的 `scatterplot`。

```py
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(1 , figsize = (10 , 5) )
sns.scatterplot(
    data=df_mall,
    x="Spending Score (1-100)",
    y="Annual Income (k$)",
    hue="Gender",
    size="Age",
    palette="Set2"
);
```

即使没有 K-Means 聚类，我们也可以清楚地看到在消费得分 40-60 和年收入 40k 到 70k 之间的簇。为了发现更多簇，我们将在下一部分使用聚类算法。

![动手实践无监督学习：K 均值聚类](img/38b8c1066d3a86496db4547ab7d7d960.png)

## 标准化

在应用聚类算法之前，标准化数据以消除任何异常值或异常情况至关重要。我们将删除“性别”和“年龄”列，使用其余列来寻找簇。

```py
from sklearn import preprocessing

X = df_mall.drop(["Gender","Age"],axis=1)
X_norm = preprocessing.normalize(X) 
```

## 肘部法则

K-Means 算法中的最佳 K 值可以通过肘部法则找到。这涉及到计算 1-10 个簇的每个 K 值的惯性值并进行可视化。

```py
import numpy as np
from sklearn.cluster import KMeans

def elbow_plot(data,clusters):
    inertia = []
    for n in range(1, clusters):
        algorithm = KMeans(
            n_clusters=n,
            init="k-means++",
            random_state=125,
        )
        algorithm.fit(data)
        inertia.append(algorithm.inertia_)
    # Plot
    plt.plot(np.arange(1 , clusters) , inertia , 'o')
    plt.plot(np.arange(1 , clusters) , inertia , '-' , alpha = 0.5)
    plt.xlabel('Number of Clusters') , plt.ylabel('Inertia')
    plt.show();

elbow_plot(X_norm,10)
```

我们得到了最佳的 K 值为 3。

![动手实践无监督学习：K 均值聚类](img/d03eca3f8033dd0406d4473fce5632ab.png)

## K 均值聚类

我们现在将使用来自 scikit-learn 的 KMeans 算法，并提供 K 值。之后，我们将在训练数据集上进行拟合并获取簇标签。

```py
algorithm = KMeans(n_clusters=3, init="k-means++", random_state=125)
algorithm.fit(X_norm)
labels = algorithm.labels_
```

我们可以使用散点图来可视化这三个簇。

```py
sns.scatterplot(data = X, x = 'Spending Score (1-100)', y = 'Annual Income (k$)', hue = labels, palette="Set2");
```

+   “0”：高消费水平且年收入低的顾客。

+   “1”：中等到高消费水平且年收入中等到高的顾客。

+   “2”：低消费水平且年收入高的顾客。

![动手实践无监督学习：K 均值聚类](img/2cbd08954d81e44a5f8d7b4ac1e03d78.png)

这些见解可以用于创建个性化广告，增加客户忠诚度并提升收入。

## 使用不同的特征

现在，我们将使用年龄和消费得分作为聚类算法的特征。这将给我们一个完整的顾客分布图。我们将重复数据标准化的过程。

```py
X = df_mall.drop(["Gender","Annual Income (k$)"],axis=1)

X_norm = preprocessing.normalize(X) 
```

计算最佳的簇数。

```py
elbow_plot(X_norm,10)
```

对 K-Means 算法进行训练，设置 K=3 个簇。

![动手实践无监督学习：K 均值聚类](img/d7cc507c1b0db3178ac1dacc78a68880.png)

```py
algorithm = KMeans(n_clusters=3, init="k-means++", random_state=125)
algorithm.fit(X_norm)
labels = algorithm.labels_
```

使用散点图可视化这三个簇。

```py
sns.scatterplot(data = X, x = 'Age', y = 'Spending Score (1-100)', hue = labels, palette="Set2");
```

+   “0”：年轻的高消费顾客。

+   “1”：中等消费水平的顾客，年龄从中年到老年。

+   “2”：低消费顾客。

结果表明，公司可以通过针对年龄在 20-40 岁且有可支配收入的个人来增加利润。

![动手实践无监督学习：K 均值聚类](img/59754c9b907b34492adf93f13ea991ec.png)

我们甚至可以通过可视化消费分数的箱线图深入了解。这清楚地显示了集群是基于消费习惯形成的。

```py
sns.boxplot(x = labels, y = X['Spending Score (1-100)']);
```

![动手实践无监督学习：K-Means 聚类](img/2822e2c1ea52d934a8411cc100ec36a2.png)

# 结论

在这个 K-Means 聚类教程中，我们探讨了如何将 K-Means 算法应用于客户细分，以实现精准广告投放。虽然 K-Means 不是完美的通用聚类算法，但它为许多现实世界的用例提供了简单有效的方法。

通过走过 K-Means 工作流程并在 Python 中实现它，我们深入了解了算法如何将数据分成不同的集群。我们学习了使用肘部法则找到最佳集群数量以及可视化聚类数据等技术。

虽然 scikit-learn 提供了许多其他[聚类算法](https://scikit-learn.org/stable/modules/clustering.html)，但 K-Means 因其速度、可扩展性和易于解释而脱颖而出。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan/)) 是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理疾病困扰的学生打造 AI 产品。

### 更多相关内容

+   [解放聚类：理解 K-Means 聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [使用 scikit-learn 进行聚类：无监督学习教程](https://www.kdnuggets.com/2023/05/clustering-scikitlearn-tutorial-unsupervised-learning.html)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [什么是 K-Means 聚类及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [无监督解缠表示学习在类别...](https://www.kdnuggets.com/2023/01/unsupervised-disentangled-representation-learning-class-imbalanced-dataset-elastic-infogan.html)

+   [探索无监督学习指标](https://www.kdnuggets.com/2023/04/exploring-unsupervised-learning-metrics.html)
