# 通过采样进行的迭代初始质心搜索

> 原文：[https://www.kdnuggets.com/2018/09/iterative-initial-centroid-search-sampling-k-means-clustering.html](https://www.kdnuggets.com/2018/09/iterative-initial-centroid-search-sampling-k-means-clustering.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

在这篇文章中，我们将探讨通过迭代方法搜索k均值聚类的**更好**初始质心集合，并通过对我们的完整数据集样本执行此过程来实现。

我们所说的“更好”是什么意思？由于k均值聚类旨在通过连续的迭代收敛到最优的簇中心（质心）和基于这些质心的簇成员，因此初始质心的定位越优化，k均值聚类算法所需的迭代次数就会越少。因此，考虑找到一个**更好**的初始质心位置集合是一种优化k均值聚类过程的有效方法。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

我们将采取不同的方法，具体来说，就是从我们的完整数据集中抽取一个样本，然后对其进行短时间的k均值聚类算法运行（而不是收敛），这些短时间运行必然包括质心初始化过程。我们将使用若干个随机初始化的质心重复这些短时间运行，并跟踪测量指标的改善——簇内平方和——以确定聚类成员的优度（或者至少是评估此项的有效指标之一）。与随机质心初始化迭代过程相关的最终质心提供最低的惯性值的质心集合将被带入我们的完整数据集聚类过程中。

希望这项前期工作能导致我们完整聚类过程中获得更好的初始质心集合，从而减少k均值聚类的迭代次数，最终缩短完全聚类数据集所需的时间。

显然，这并不是优化质心初始化的唯一方法。过去我们讨论过[朴素分片质心初始化方法](/2017/03/naive-sharding-centroid-initialization-method.html)，这是一种用于最佳质心初始化的确定性方法。其他形式的k均值聚类算法的修改也采用了不同的方法来解决这个问题（请参见[k-means++](https://en.wikipedia.org/wiki/K-means%2B%2B)以作比较）。

本文将以如下方式进行我们的任务：

+   准备数据

+   准备我们的样本

+   执行质心初始化搜索迭代，以确定我们“最佳”的初始质心集合

+   使用结果对完整数据集进行聚类

未来的帖子将对不同方法的质心初始化结果进行比较，以便更全面地理解实施的实际情况。现在，让我们介绍并探讨这种特定的质心初始化方法。

### 数据准备

对于这个概述，我们将使用[3D道路网络数据集](https://archive.ics.uci.edu/ml/machine-learning-databases/00246/)。

![图片](../Images/cc415cd127859d36ec700e056464955c.png)

由于这个特定的数据集没有缺失值，也没有类别标签，我们的数据准备主要包括归一化，并删除一列，该列标识地理位置，这些额外的3列测量数据来源于此，对于我们的任务没有用处。有关更多详细信息，请参见[数据集描述](https://archive.ics.uci.edu/ml/datasets/3D+Road+Network+(North+Jutland,+Denmark))。

```py

import numpy as np
import pandas as pd
from sklearn import preprocessing

# Read dataset
data = pd.read_csv('3D_spatial_network.csv', header=None)

# Drop first column (not required)
data.drop(labels=0, axis=1, inplace=True)

# Normalize data (min/max scaling)
data_arr = data.values
sc = preprocessing.MinMaxScaler()
data_sc = sc.fit_transform(data_arr)
data = pd.DataFrame(data_sc)
```

让我们查看一下我们的数据样本：

```py

data.sample(10)

```

![数据集样本](../Images/11a1d744521afd259993b79de39b450d.png)

### 准备样本

接下来，我们将提取用于找到“最佳”初始质心的样本。让我们明确我们正在做什么：

+   我们正在从数据集中提取一组样本

+   然后，我们将对这些样本数据进行连续的k均值聚类，每次迭代将：

    +   随机初始化k个质心，并进行n次迭代的k均值聚类算法

    +   每个质心的初始惯性（簇内平方和）将被记录下来，同时也会记录其最终惯性，提供最大惯性增量的初始质心将被选为我们对完整数据集进行聚类的初始质心

+   然后，我们将对完整数据集进行全面的k均值聚类，使用在上一步找到的初始簇

2个重要点：

1.  为什么不使用惯性最大的减少量？（希望这个区域的初始动量会持续下去。）这样做也是一个有效的选择（并且只需更改一行代码即可实现）。这是一个任意的初步实验选择，可能需要更多调查。然而，在多个样本上的重复执行和比较最初显示，最低惯性和惯性最大减少量大多数时间是重合的，因此这个决定*可能*实际上是任意的，但在实践中也可能没有影响。

1.  特别说明的是，我们并不是从数据集中多次抽样（例如，每次质心初始化的迭代都从数据集中抽样一次）。我们为一个质心初始化**搜索**的所有迭代抽样一次。从中我们将随机推导出多个初始质心。这与每次质心初始化迭代的重复抽样的概念形成对比。

下面，我们设置：

+   样本大小作为我们完整数据集的比例

+   为了可重复性设置的随机状态

+   我们数据集的聚类数（k）

+   我们的k均值算法的迭代次数（n）

+   在我们的样本数据集上进行聚类时，尝试找到最佳初始质心的次数

然后我们设置我们的样本数据

```py

# Some variables
SAMPLE_SIZE = 0.1
RANDOM_STATE = 42
NUM_CLUSTERS = 10     # k
NUM_ITER = 3          # n
NUM_ATTEMPTS = 5      # m

data_sample = data.sample(frac=SAMPLE_SIZE, random_state=RANDOM_STATE, replace=False)
data_sample.shape
```

现在我们有了数据样本（`data_sample`），我们准备进行质心初始化的迭代以进行比较和选择。

### 对样本数据进行聚类

由于Scikit-learn的k均值聚类实现不允许轻松获取聚类迭代之间的质心，我们不得不稍微修改工作流程。虽然`verbose`选项确实直接在屏幕上输出一些有用的信息，并且重定向此输出然后后期解析是获取所需内容的一种方法，但我们将编写自己的外部迭代循环来自己控制*n*变量。

这意味着我们需要计算迭代次数，并在每次聚类步骤运行后捕获我们需要的内容。然后我们将把那个聚类迭代循环封装在一个质心初始化循环中，该循环将从我们的样本数据中初始化*k*个质心，共*m*次。这是我们特定k均值质心初始化过程中的超参数，超出了“常规”k均值。

根据上述参数，我们将把数据集聚类为10个簇（NUM_CLUSTERS，或*k*），我们将运行3次迭代的质心搜索（NUM_ITER，或*n*），我们将尝试5个随机初始质心（NUM_ATTEMPTS，或*m*），然后确定“最佳”质心集合，以便进行完整的聚类（在我们的案例中，度量是最低的簇内平方和，或惯性）。

然而，在任何聚类之前，让我们看看在任何聚类迭代之前单次初始化k均值的样本。

```py

from sklearn.cluster import KMeans

km = KMeans(n_clusters=NUM_CLUSTERS, init='random', max_iter=1, n_init=1)#, verbose=1)
km.fit(data_sample)

print('Pre-clustering metrics')
print('----------------------')
print('Inertia:', km.inertia_)
print('Centroids:', km.cluster_centers_)
```

```py

Pre-clustering metrics
----------------------
Inertia: 898.5527121490726
Centroids: [[0.42360342 0.20208702 0.26294088]
 [0.56835267 0.34756347 0.14179924]
 [0.66005691 0.73147524 0.38203476]
 [0.23935675 0.08942105 0.11727529]
 [0.58630271 0.23417288 0.45793108]
 [0.1982982  0.11219503 0.23924021]
 [0.79313864 0.52773534 0.1334036 ]
 [0.54442269 0.60599501 0.17600424]
 [0.14588389 0.29821987 0.18053109]
 [0.73877864 0.8379479  0.12567452]]

```

在下面的代码中，请注意，由于我们自己管理这些连续迭代，我们必须手动跟踪每次迭代开始和结束时的质心。然后，我们将这些结束质心输入到下一个循环迭代中作为初始质心，并运行一次迭代。这有点繁琐，令人烦恼的是我们不能直接从Scikit-learn的实现中获得这一点，但并不困难。

```py

final_cents = []
final_inert = []

for sample in range(NUM_ATTEMPTS):
    print('\nCentroid attempt: ', sample)
    km = KMeans(n_clusters=NUM_CLUSTERS, init='random', max_iter=1, n_init=1)#, verbose=1) 
    km.fit(data_sample)
    inertia_start = km.inertia_
    intertia_end = 0
    cents = km.cluster_centers_

    for iter in range(NUM_ITER):
        km = KMeans(n_clusters=NUM_CLUSTERS, init=cents, max_iter=1, n_init=1)
        km.fit(data_sample)
        print('Iteration: ', iter)
        print('Inertia:', km.inertia_)
        print('Centroids:', km.cluster_centers_)
        inertia_end = km.inertia_
        cents = km.cluster_centers_

    final_cents.append(cents)
    final_inert.append(inertia_end)
    print('Difference between initial and final inertia: ', inertia_start-inertia_end)

```

```py

Centroid attempt:  0
Iteration:  0
Inertia: 885.1279991728289
Centroids: [[0.67629991 0.54950506 0.14924545]
 [0.78911957 0.97469266 0.09090362]
 [0.61465665 0.32348368 0.11496346]
 [0.73784495 0.83111278 0.11263995]
 [0.34518925 0.37622882 0.1508636 ]
 [0.18220657 0.18489484 0.19303869]
 [0.55688642 0.35810877 0.32704852]
 [0.6884195  0.65798194 0.48258798]
 [0.62945726 0.73950354 0.21866185]
 [0.52282355 0.12252092 0.36251485]]
Iteration:  1
Inertia: 861.7158412685387
Centroids: [[0.67039882 0.55769658 0.15204125]
 [0.78156936 0.96504069 0.09821352]
 [0.61009844 0.33444322 0.11527662]
 [0.75151713 0.79798919 0.1225065 ]
 [0.33091899 0.39011157 0.14788905]
 [0.18246521 0.18602087 0.19239602]
 [0.55246091 0.3507018  0.33212609]
 [0.68998302 0.65595219 0.48521344]
 [0.60291234 0.73999001 0.23322449]
 [0.51953015 0.12140833 0.34820443]]
Iteration:  2
Inertia: 839.2470653106332
Centroids: [[0.65447477 0.55594052 0.15747416]
 [0.77412386 0.952986   0.10887517]
 [0.60761544 0.34326727 0.11544127]
 [0.77183027 0.76936972 0.12249837]
 [0.32151587 0.39281244 0.14797103]
 [0.18240552 0.18375276 0.19278224]
 [0.55052636 0.34639191 0.33667632]
 [0.691699   0.65507199 0.48648245]
 [0.59408317 0.73763362 0.23387334]
 [0.51879974 0.11982321 0.34035345]]
Difference between initial and final inertia:  99.6102464383905

...

```

完成后，让我们看看质心搜索的结果。首先检查我们最终惯性的列表（或簇内平方和），寻找最低值。然后，我们将相关的质心设置为下一步的初始质心。

```py

# Get best centroids to use for full clustering
best_cents = final_cents[final_inert.index(min(final_inert))]
best_cents
```

下面是这些质心的样子：

```py
array([[0.55053207, 0.16588572, 0.44981164],
       [0.78661867, 0.77450779, 0.11764745],
       [0.656176  , 0.55398196, 0.4748823 ],
       [0.17621429, 0.13463117, 0.17132811],
       [0.63702675, 0.14021011, 0.18632431],
       [0.60838757, 0.39809226, 0.14491584],
       [0.43593405, 0.49377153, 0.14018223],
       [0.16800744, 0.34174697, 0.19503396],
       [0.40169376, 0.15386471, 0.23633233],
       [0.62151433, 0.72434071, 0.25946183]])

```

### 运行完整的k-means聚类

现在，我们可以用最佳的初始质心在完整数据集上运行k-means聚类。由于Scikit-learn允许我们传入一组初始质心，我们可以通过以下相对简单的代码来利用这一点。

```py

km_full = KMeans(n_clusters=NUM_CLUSTERS, init=best_cents, max_iter=100, verbose=1, n_init=1)
km_full.fit(data)

```

这次k-means运行在13次迭代中收敛：

```py

...

start iteration
done sorting
end inner loop
Iteration 13, inertia 7492.170210199639
center shift 1.475641e-03 within tolerance 4.019354e-06

```

为了对比，这里是使用随机初始化质心（“常规”k-means）的完整k-means聚类运行：

```py

km_naive = KMeans(n_clusters=NUM_CLUSTERS, init='random', max_iter=100, verbose=1, n_init=1)
km_naive.fit(data)

```

此次运行耗时39次迭代，惯性几乎相同：

```py

...

start iteration
done sorting
end inner loop
Iteration 39, inertia 7473.495361902045
center shift 1.948248e-03 within tolerance 4.019354e-06

```

我将13次迭代与39次迭代（或类似情况）的执行时间差异留给读者自行探索。不用说，提前在样本数据（在我们的案例中，是完整数据集的10%）上花费几次周期，长期来看节省了大量周期，同时没有牺牲我们的整体聚类指标。

当然，在做出任何一般性结论之前，需要额外的测试，在未来的文章中，我将对多种数据集上的若干质心初始化方法进行实验，并使用一些附加指标进行比较，希望能够更清晰地了解优化无监督学习工作流程的方法。

**相关**：

+   [通过朴素分片质心初始化方法提高k-means聚类效率](/2017/03/naive-sharding-centroid-initialization-method.html)

+   [使用Python和SciPy比较距离测量](/2017/08/comparing-distance-measurements-python-scipy.html)

+   [从头开始的Python机器学习工作流程 第二部分：k-means聚类](/2017/06/machine-learning-workflows-python-scratch-part-2.html)

### 更多相关内容

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [解放聚类：理解K-Means聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [什么是K-Means聚类以及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [动手实践无监督学习：k-means聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [使用网格搜索和随机搜索进行超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过Uplimit的搜索与机器学习课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)
