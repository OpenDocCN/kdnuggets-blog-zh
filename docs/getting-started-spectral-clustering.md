# 谱聚类入门

> 原文：[`www.kdnuggets.com/2020/05/getting-started-spectral-clustering.html`](https://www.kdnuggets.com/2020/05/getting-started-spectral-clustering.html)

评论

**由 [Dr. Juan Camilo Orduz](https://juanitorduz.github.io/)，数学家与数据科学家**

在这篇文章中，我想探讨谱聚类背后的思想。我不打算发展理论。相反，我将通过一个实际的例子来揭示并激发每一步谱聚类算法背后的直觉。我特别推荐两个参考文献：

+   要了解理论的介绍/概述，请参阅 [谱聚类教程](http://www.tml.cs.uni-tuebingen.de/team/luxburg/publications/Luxburg07_tutorial.pdf)，由 [Ulrike von Luxburg 教授](http://www.tml.cs.uni-tuebingen.de/team/luxburg/index.php) 主讲。

+   对于这种聚类方法的具体应用，你可以查看 PyData 的讲座：[使用谱聚类提取相关指标](https://www.youtube.com/watch?v=3heWpR6dC8k)，由 Dr. Evelyn Trautmann 主讲。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 准备笔记本

```py
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('darkgrid', {'axes.facecolor': '.9'})
sns.set_palette(palette='deep')
sns_c = sns.color_palette(palette='deep')
%matplotlib inline

from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()
```

### 生成样本数据

让我们生成一些样本数据。正如我们将看到的，谱聚类对于非凸聚类非常有效。在这个例子中，我们考虑同心圆：

```py
# Set random state. 
rs = np.random.seed(25)

def generate_circle_sample_data(r, n, sigma):
    """Generate circle data with random Gaussian noise."""
    angles = np.random.uniform(low=0, high=2*np.pi, size=n)

    x_epsilon = np.random.normal(loc=0.0, scale=sigma, size=n)
    y_epsilon = np.random.normal(loc=0.0, scale=sigma, size=n)

    x = r*np.cos(angles) + x_epsilon
    y = r*np.sin(angles) + y_epsilon
    return x, y

def generate_concentric_circles_data(param_list):
    """Generates many circle data with random Gaussian noise."""
    coordinates = [ 
        generate_circle_sample_data(param[0], param[1], param[2])
     for param in param_list
    ]
    return coordinates
```

让我们绘制一些示例，以查看参数如何影响数据结构和聚类。

```py
# Set global plot parameters. 
plt.rcParams['figure.figsize'] = [8, 8]
plt.rcParams['figure.dpi'] = 80

# Number of points per circle. 
n = 1000
# Radius. 
r_list =[2, 4, 6]
# Standar deviation (Gaussian noise). 
sigmas = [0.1, 0.25, 0.5]

param_lists = [[(r, n, sigma) for r in r_list] for sigma in sigmas] 
# We store the data on this list.
coordinates_list = []

fig, axes = plt.subplots(3, 1, figsize=(7, 21))

for i, param_list in enumerate(param_lists):

    coordinates = generate_concentric_circles_data(param_list)

    coordinates_list.append(coordinates)

    ax = axes[i]

    for j in range(0, len(coordinates)):

        x, y = coordinates[j]
        sns.scatterplot(x=x, y=y, color='black', ax=ax)
        ax.set(title=f'$\sigma$ = {param_list[0][2]}')

plt.tight_layout()
```

![png](img/c8894108718ff5a45081a704ad65bf04.png)

前两个图显示了 33 个明确的聚类。对于最后一个，聚类结构不太明确。

### 谱聚类算法

尽管我们不会给出所有理论细节，但我们仍将激发谱聚类算法背后的逻辑。

### 图拉普拉斯算子

谱聚类的一个关键概念是 [图拉普拉斯算子](https://en.wikipedia.org/wiki/Laplacian_matrix)。让我们描述它的构造¹：

+   假设我们有一个数据点的数据集

    ![Equation](img/628675573c23169f3faa55b59ce2e594.png)。

+   对于数据集 *X*，我们关联一个（加权）图 *G*，它编码了数据点之间的接近程度。具体来说，

    +   *G* 的节点由每个数据点给出

        ![Equation](img/db373d731ded926a6199712cd8295a29.png)。

    +   如果两个节点 ![方程](img/b1af7e2fbfa7601511b5f4b136868e55.png) 和 ![方程](img/b1af7e2fbfa7601511b5f4b136868e55.png) 是*接近的*，则它们由一条边连接。*接近*的概念取决于我们想要编码的距离。有两种常见的选择。

        +   （欧几里得距离）给定

            ![方程](img/81f088809e21d1638e66b0261198fa0f.png) ![方程](img/584073cbd8da3032d36a754d5eb06ae5.png) 和 ![方程](img/e555ff9b12a949daf21c9f32002fae9f.png) 通过边连接，如果 ![方程](img/da6f736c1fb8e5463247e2198341bbea.png)。在一些应用中，边可能具有形如 ![方程](img/eb417e0b46304c6674fcdc1be311a4e2.png)的权重。

        +   （最近邻） ![方程](img/584073cbd8da3032d36a754d5eb06ae5.png) 和 ![方程](img/e555ff9b12a949daf21c9f32002fae9f.png) 通过边连接，如果 ![方程](img/e555ff9b12a949daf21c9f32002fae9f.png) 是 ![方程](img/e555ff9b12a949daf21c9f32002fae9f.png)的 k-最近邻。

一旦图构建完成，我们可以考虑其相关的 [邻接矩阵](https://en.wikipedia.org/wiki/Adjacency_matrix) ![方程](img/22fc3a5df44a891d66abe7740209bb57.png)，如果 ![方程](img/b1af7e2fbfa7601511b5f4b136868e55.png) 和 ![方程](img/4d4150c9b2a5a30fb031eeec94194cdf.png) 通过边连接，则该矩阵的 ![方程](img/d8d974867821c70d67c0b36b26fdb70c.png) 位置有一个非零值。另一方面，设 ![方程](img/ad0c74480cf07e40fcd0d89db4843165.png) 表示图的 [度矩阵](https://en.wikipedia.org/wiki/Degree_matrix)，这是一个对角矩阵，包含每个节点的度数。然后，图拉普拉斯算子 ![方程](img/d1acf57e680b35fda6ce8a1d54c1809c.png) 被定义为差值 ![方程](img/36bf73978d86cfa96b0e79619e028eed.png)。这个矩阵是对称的，且正半定的，这意味着（通过 [谱定理](https://juanitorduz.github.io/the-spectral-theorem-for-matrices/)）所有的特征值都是实数且非负的。 [这里](https://juanitorduz.github.io/documents/orduz_pydata2018.pdf) 你可以找到有关图拉普拉斯算子定义和性质的更多细节。

### 动机

为什么图拉普拉斯算子对检测聚类很重要？让我们从一个简单的情况开始，其中数据*X*有两个聚类 ![方程](img/394c56d673b3a944a22dfd064b7b1b2d.png) 和 ![方程](img/58f1efbbcaec2f9ce8b58c6f443c7e4e.png)，它们分得如此开，以至于它们对应于 [连通分量](https://en.wikipedia.org/wiki/Component_(graph_theory)) ![方程](img/70daf2b4cc119e119b330d34b719091f.png) 和 ![方程](img/3563c17713e7c121b642fa4d6d6deef8.png) 的关联图 ![方程](img/e3f936b28e6b566fa65492a69c8d19d9.png)。观察图拉普拉斯算子的纯定义，我们可以重新排序点，使得图拉普拉斯算子分解为

![Equation](img/9e1a00f1cbf8f5e76f26beaf7769c9e6.png)

其中 ![Equation](img/0cbf0ffdfb63ef726502c6218bd180b9.png) 和 ![Equation](img/d0733378c3834ee62c808977704e49e1.png) 分别是 ![Equation](img/70daf2b4cc119e119b330d34b719091f.png) 和 ![Equation](img/3563c17713e7c121b642fa4d6d6deef8.png) 的图拉普拉斯矩阵。可以证明，零特征值的核（特征空间）的维度为 22，并且由正交特征向量 ![Equation](img/99ad52ec914e0013f46613d30a94228e.png) 和 ![Equation](img/578eeb5f2e21c06953ba290515bc798f.png) 生成。这一论点易于推广到多个连通分量。

总结来说，关键属性是

> *通过计算对应图拉普拉斯矩阵的零空间的维数，可以获得关联图的连通分量数量。*

如果数据集的关联图是连通的，但我们仍然希望检测簇怎么办？好吧，上述方法在某种程度上对小的扰动保持稳定，可以通过在图拉普拉斯矩阵的小特征值的特征向量矩阵的行上运行 *k*-均值来检测簇。再次，请参阅上述建议的讲义以获取详细信息。

### 算法

下面是（未归一化的）谱聚类的步骤²。根据上述讨论，这些步骤现在应该是合理的。

*输入：* 相似度矩阵 ![Equation](img/c0e21230b0f47a4d142b1e63b751b5a3.png) （即距离的选择），构造的簇数 *k*。

*步骤：*

+   设 *W* 为对应图的（加权）邻接矩阵。

+   计算（未归一化的）拉普拉斯矩阵 *L*。

+   计算 *L* 的前 *k* 个特征向量 ![Equation](img/340e87fdf0f7fa75361e5c8e6a46be1a.png)。

+   设 ![Equation](img/287f7668a0c2dca27024ea343b202992.png) 为包含向量 ![Equation](img/340e87fdf0f7fa75361e5c8e6a46be1a.png) 作为列的矩阵。

+   对于 ![Equation](img/95ed894fa54521bff2c75a83dd08aea0.png)，设 ![Equation](img/2c527f8105d1c5242fee69dc31707f29.png) 为对应于 *U* 的第 *i* 行的向量。

+   使用 *k*-均值算法将点 ![Equation](img/2c527f8105d1c5242fee69dc31707f29.png) 聚类为簇 ![Equation](img/477b4ba9f8acdbd306b0f6d105825c18.png)

![Figure](img/9bc86f62df7589ebf0ebd687c7e37f5e.png)

让我们通过一个例子来重现这些步骤，以更好地理解这个算法的有效性。

### 示例 1：明确的簇

我们考虑上述参数定义的样本数据，`sigma` = 0.1。请注意，上述图中的簇在这种情况下分离得很好。

```py
from itertools import chain

coordinates = coordinates_list[0]

def data_frame_from_coordinates(coordinates): 
    """From coordinates to data frame."""
    xs = chain(*[c[0] for c in coordinates])
    ys = chain(*[c[1] for c in coordinates])

    return pd.DataFrame(data={'x': xs, 'y': ys})

data_df = data_frame_from_coordinates(coordinates)

# Plot the input data.
fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', color='black', data=data_df, ax=ax)
ax.set(title='Input Data');
```

![png](img/f97a4611ce6beab8594af5917ecacced.png)

### K - 均值

让我们首先运行 k-means 算法尝试获取一些聚类。我们使用肘部法则选择聚类数，通过考虑惯性（样本到其最近聚类中心的平方距离之和）作为聚类数的函数。

```py
from sklearn.cluster import KMeans

inertias = []

k_candidates = range(1, 10)

for k in k_candidates:
    k_means = KMeans(random_state=42, n_clusters=k)
    k_means.fit(data_df)
    inertias.append(k_means.inertia_)

fig, ax = plt.subplots(figsize=(10, 6))
sns.scatterplot(x=k_candidates, y = inertias, s=80, ax=ax)
sns.scatterplot(x=[k_candidates[2]], y = [inertias[2]], color=sns_c[3], s=150, ax=ax)
sns.lineplot(x=k_candidates, y = inertias, alpha=0.5, ax=ax)
ax.set(title='Inertia K-Means', ylabel='inertia', xlabel='k');
```

![png](img/b019d8bcb460805d2d043e1ed1dcb3a6.png)

从这个图中我们看到![Equation](img/84d09e70b54382413998df3c984dfc85.png)是一个不错的选择。让我们获取这些聚类。

```py
k_means = KMeans(random_state=25, n_clusters=3)
k_means.fit(data_df)
cluster = k_means.predict(data_df)

cluster = ['k-means_c_' + str(c) for c in cluster]

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df.assign(cluster = cluster), hue='cluster', ax=ax)
ax.set(title='K-Means Clustering');
```

![png](img/e3059a9e5cf46b505f23fa4c429ff056.png)

结果并不令人惊讶，因为*k*-均值生成的是凸聚类。

### 第一步：计算图拉普拉斯算子

在第一步中，我们计算图拉普拉斯算子。我们将使用最近邻生成图来建模我们的数据集。

```py
from sklearn.neighbors import kneighbors_graph
from scipy import sparse

def generate_graph_laplacian(df, nn):
    """Generate graph Laplacian from data."""
    # Adjacency Matrix.
    connectivity = kneighbors_graph(X=df, n_neighbors=nn, mode='connectivity')
    adjacency_matrix_s = (1/2)*(connectivity + connectivity.T)
    # Graph Laplacian.
    graph_laplacian_s = sparse.csgraph.laplacian(csgraph=adjacency_matrix_s, normed=False)
    graph_laplacian = graph_laplacian_s.toarray()
    return graph_laplacian 

graph_laplacian = generate_graph_laplacian(df=data_df, nn=8)

# Plot the graph Laplacian as heat map.
fig, ax = plt.subplots(figsize=(10, 8))
sns.heatmap(graph_laplacian, ax=ax, cmap='viridis_r')
ax.set(title='Graph Laplacian');
```

![png](img/d0d9e2e88c1f3cb09b84bfe253ede257.png)

### 第二步：计算图拉普拉斯算子的谱

接下来，我们计算图拉普拉斯算子的特征值和特征向量。

```py
from scipy import linalg

eigenvals, eigenvcts = linalg.eig(graph_laplacian)
```

特征值由复数表示。由于图拉普拉斯算子是一个对称矩阵，根据[谱定理](https://juanitorduz.github.io/the-spectral-theorem-for-matrices/)我们知道所有特征值必须是实数。让我们验证一下：

```py
np.unique(np.imag(eigenvals))
```

```py
array([0.])
```

```py
# We project onto the real numbers. 
def compute_spectrum_graph_laplacian(graph_laplacian):
    """Compute eigenvalues and eigenvectors and project 
    them onto the real numbers.
    """
    eigenvals, eigenvcts = linalg.eig(graph_laplacian)
    eigenvals = np.real(eigenvals)
    eigenvcts = np.real(eigenvcts)
    return eigenvals, eigenvcts

eigenvals, eigenvcts = compute_spectrum_graph_laplacian(graph_laplacian)
```

现在我们计算![Equation](img/ebbe6af529c5d2cb7495acd213460fb7.png)范数的特征向量。

```py
eigenvcts_norms = np.apply_along_axis(
  lambda v: np.linalg.norm(v, ord=2), 
  axis=0, 
  arr=eigenvcts
)

print('Min Norm: ' + str(eigenvcts_norms.min()))
print('Max Norm: ' + str(eigenvcts_norms.max()))
```

```py
Min Norm: 0.9999999999999997
Max Norm: 1.0000000000000002
```

因此，所有特征向量的长度大约为 1。

然后我们将特征值按升序排序。

```py
eigenvals_sorted_indices = np.argsort(eigenvals)
eigenvals_sorted = eigenvals[eigenvals_sorted_indices]
```

让我们绘制排序后的特征值。

```py
fig, ax = plt.subplots(figsize=(10, 6))
sns.lineplot(x=range(1, eigenvals_sorted_indices.size + 1), y=eigenvals_sorted, ax=ax)
ax.set(title='Sorted Eigenvalues Graph Laplacian', xlabel='index', ylabel=r'$\lambda$');
```

![png](img/f67cfa86f3804094977d27bc0e351c09.png)

### 第三步：寻找小特征值

让我们放大查看小特征值。

```py
index_lim = 10

fig, ax = plt.subplots(figsize=(10, 6))
sns.scatterplot(x=range(1, eigenvals_sorted_indices[: index_lim].size + 1), y=eigenvals_sorted[: index_lim], s=80, ax=ax)
sns.lineplot(x=range(1, eigenvals_sorted_indices[: index_lim].size + 1), y=eigenvals_sorted[: index_lim], alpha=0.5, ax=ax)
ax.axvline(x=3, color=sns_c[3], label='zero eigenvalues', linestyle='--')
ax.legend()
ax.set(title=f'Sorted Eigenvalues Graph Laplacian (First {index_lim})', xlabel='index', ylabel=r'$\lambda$');
```

![png](img/afcad65549982c9e2bef1011d50cad25.png)

从图中我们可以看到前 33 个特征值（排序后）基本上为零。

```py
zero_eigenvals_index = np.argwhere(abs(eigenvals) < 1e-5)
eigenvals[zero_eigenvals_index]
```

```py
array([[-9.42076177e-16],
       [ 8.21825247e-16],
       [ 5.97249344e-16]])
```

对于这些小特征值，我们考虑它们对应的特征向量。

```py
proj_df = pd.DataFrame(eigenvcts[:, zero_eigenvals_index.squeeze()])
proj_df.columns = ['v_' + str(c) for c in proj_df.columns]
proj_df.head()
```

|  | v_0 | v_1 | v_2 |
| --- | --- | --- | --- |
| 0 | 0.031623 | 0.0 | 0.0 |
| 1 | 0.031623 | 0.0 | 0.0 |
| 2 | 0.031623 | 0.0 | 0.0 |
| 3 | 0.031623 | 0.0 | 0.0 |
| 4 | 0.031623 | 0.0 | 0.0 |

让我们将这个数据框可视化为热图：

```py
fig, ax = plt.subplots(figsize=(10, 8))
sns.heatmap(proj_df, ax=ax, cmap='viridis_r')
ax.set(title='Eigenvectors Generating the Kernel of the Graph Laplacian');
```

![png](img/ea0131165b6c60930073f8634160c703.png)

我们可以清楚地看到一个块结构（表示连接的组件）。一般来说，当聚类不是孤立的连接组件时，寻找零特征值或谱间隙过于限制。因此，我们简单地选择我们想要找到的聚类数。

```py
def project_and_transpose(eigenvals, eigenvcts, num_ev):
    """Select the eigenvectors corresponding to the first 
    (sorted) num_ev eigenvalues as columns in a data frame.
    """
    eigenvals_sorted_indices = np.argsort(eigenvals)
    indices = eigenvals_sorted_indices[: num_ev]

    proj_df = pd.DataFrame(eigenvcts[:, indices.squeeze()])
    proj_df.columns = ['v_' + str(c) for c in proj_df.columns]
    return proj_df
```

### 第四步：运行 K 均值聚类

为了选择聚类数（从上面的图中我们已经怀疑是![Equation](img/42aaa1975c4182cd2fa1ac18601919e9.png)），我们对不同的聚类值运行 k-means，并绘制相关的惯性（样本到其最近聚类中心的平方距离之和）。

```py
inertias = []

k_candidates = range(1, 6)

for k in k_candidates:
    k_means = KMeans(random_state=42, n_clusters=k)
    k_means.fit(proj_df)
    inertias.append(k_means.inertia_)
```

```py
fig, ax = plt.subplots(figsize=(10, 6))
sns.scatterplot(x=k_candidates, y = inertias, s=80, ax=ax)
sns.lineplot(x=k_candidates, y = inertias, alpha=0.5, ax=ax)
ax.set(title='Inertia K-Means', ylabel='inertia', xlabel='k');
```

![png](img/c77f9787b4fe915c608ee99ed5748e1e.png)从这个图中我们可以看到，最佳的聚类数是![Equation](img/42aaa1975c4182cd2fa1ac18601919e9.png)。

```py
def run_k_means(df, n_clusters):
    """K-means clustering."""
    k_means = KMeans(random_state=25, n_clusters=n_clusters)
    k_means.fit(df)
    cluster = k_means.predict(df)
    return cluster

cluster = run_k_means(proj_df, n_clusters=3)
```

```py
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(
    xs=proj_df['v_0'], 
    ys=proj_df['v_1'], 
    zs=proj_df['v_2'],
    c=[{0: sns_c[0], 1: sns_c[1], 2: sns_c[2]}.get(c) for c in cluster]
)
ax.set_title('Small Eigenvectors Clusters', x=0.2);
```

![png](img/7ed923e9d74d2a7f774a2643e7021446.png)

### 第五步：分配聚类标签

最后，我们给每个点添加聚类标签。

```py
data_df['cluster'] = ['c_' + str(c) for c in cluster]

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering');
```

![png](img/864ee35d3fb1cfc6b0d007936166eb3e.png)

请注意，通过谱聚类我们可以得到非凸簇。

### 总结

为了总结算法步骤，我们将其总结为一个函数。

```py
def spectral_clustering(df, n_neighbors, n_clusters):
    """Spectral Clustering Algorithm."""
    graph_laplacian = generate_graph_laplacian(df, n_neighbors)
    eigenvals, eigenvcts = compute_spectrum_graph_laplacian(graph_laplacian)
    proj_df = project_and_transpose(eigenvals, eigenvcts, n_clusters)
    cluster = run_k_means(proj_df, proj_df.columns.size)
    return ['c_' + str(c) for c in cluster]
```

### 示例 2

让我们考虑一下噪声更多的数据：

+   3 个簇

```py
data_df = data_frame_from_coordinates(coordinates_list[1])
data_df['cluster'] = spectral_clustering(df=data_df, n_neighbors=8, n_clusters=3)

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering');
```

![png](img/fc7734aa4b32d1d89239b44fd816fcaf.png)

+   2 个簇

```py
data_df = data_frame_from_coordinates(coordinates_list[1])
data_df['cluster'] = spectral_clustering(df=data_df, n_neighbors=8, n_clusters=2)

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering');
```

![png](img/c30b2867ebc3f8be68cfd44b96cefd9f.png)

### 示例 3

对于最后一个数据集，我们基本上发现的结果与使用 k-均值时相同。

```py
data_df = data_frame_from_coordinates(coordinates_list[2])
data_df['cluster'] = spectral_clustering(df=data_df, n_neighbors=8, n_clusters=3)

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering');
```

![png](img/e238deb3594f39f8d89fc3515a6cf9f6.png)

### SpectralClustering（Scikit-Learn）

正如预期的那样，[scikit-learn](https://scikit-learn.org/stable/) 已经有了 [谱聚类实现](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.SpectralClustering.html)。让我们与上面的结果进行比较。

### 示例 2（重新审视）

```py
from sklearn.cluster import SpectralClustering

data_df = data_frame_from_coordinates(coordinates_list[1])

spec_cl = SpectralClustering(
    n_clusters=3, 
    random_state=25, 
    n_neighbors=8, 
    affinity='nearest_neighbors'
)

data_df['cluster'] = spec_cl.fit_predict(data_df[['x', 'y']])
data_df['cluster'] = ['c_' + str(c) for c in data_df['cluster']]

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering - Scikit Learn');
```

![png](img/a42093418da0e5f4e1143431af8f8640.png)

### 示例 3（重新审视）

```py
data_df = data_frame_from_coordinates(coordinates_list[2])

spec_cl = SpectralClustering(
    n_clusters=3, 
    random_state=42, 
    n_neighbors=8, 
    affinity='nearest_neighbors'
)

data_df['cluster'] = spec_cl.fit_predict(data_df[['x', 'y']])
data_df['cluster'] = ['c_' + str(c) for c in data_df['cluster']]

fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', data=data_df, hue='cluster', ax=ax)
ax.set(title='Spectral Clustering - Scikit Learn');
```

![png](img/b9b745d9dd22b0ff3eb7568bb6d20cef.png)

### 最终备注

在具体应用中，有时很难评估选择哪个聚类算法。我通常更喜欢进行一些特征工程（基于直觉/领域知识），并在可能的情况下使用 k-均值。对于上述示例，我们看到一个旋转对称的数据集，这表明可以使用半径作为新的特征：

```py
data_df = data_frame_from_coordinates(coordinates_list[1])

data_df = data_df.assign(r2 = lambda x: np.power(x['x'], 2) + np.power(x['y'], 2))
```

让我们绘制半径特征（它是一维的，但我们将其投影到（对角线）x=yx=y）。

```py
fig, ax = plt.subplots()
sns.scatterplot(x='r2', y='r2', color='black', data=data_df, ax=ax)
ax.set(title='Radius Feature');
```

![png](img/89ccb4ff837b01003f2969aeed5544cb.png)然后，我们可以直接运行 k-均值。

```py
inertias = []

k_candidates = range(1, 10)

for k in k_candidates:
    k_means = KMeans(random_state=42, n_clusters=k)
    k_means.fit(data_df[['r2']])
    inertias.append(k_means.inertia_)

fig, ax = plt.subplots(figsize=(10, 6))
sns.scatterplot(x=k_candidates, y = inertias, s=80, ax=ax)
sns.scatterplot(x=[k_candidates[2]], y = [inertias[2]], color=sns_c[3], s=150, ax=ax)
sns.lineplot(x=k_candidates, y = inertias, alpha=0.5, ax=ax)
ax.set(title='Inertia K-Means', ylabel='inertia', xlabel='k');
```

![png](img/bc7398233baa7d80bd3c690a174d3e2d.png)

```py
k_means = KMeans(random_state=25, n_clusters=3)
k_means.fit(data_df[['r2']])
cluster = k_means.predict(data_df[['r2']])

data_df = data_df.assign(cluster = ['k-means_c_' + str(c) for c in cluster])

fig, ax = plt.subplots()
sns.scatterplot(x='r2', y='r2', hue='cluster', data=data_df, ax=ax)
ax.set(title='Radius Feature (K-Means)');
```

![png](img/d19bc17d0ca6564929915d30c216ba3b.png)最后，我们可视化原始数据及其对应的簇。

```py
fig, ax = plt.subplots()
sns.scatterplot(x='x', y='y', hue='cluster', data=data_df, ax=ax)
ax.set(title='Radius Feature (K-Means)');
```

![png](img/e131f8312c09746af8345d37cb6e2c60.png)

1.  [拉普拉斯特征映射用于降维和数据表示](http://web.cse.ohio-state.edu/~belkin.8/papers/LEM_NC_03.pdf)

1.  [谱聚类教程](http://www.tml.cs.uni-tuebingen.de/team/luxburg/publications/Luxburg07_tutorial.pdf)

**简介：[胡安·卡米洛·奥杜斯博士](https://juanitorduz.github.io/)** ([@juanitorduz](https://twitter.com/juanitorduz)) 是一位基于柏林的数学家和数据科学家。

[原文](https://juanitorduz.github.io/spectral_clustering/)。经许可转载。

**相关：**

+   数据科学家需要了解的 5 种聚类算法

+   聚类关键术语解释

+   通过采样进行 k-均值聚类的迭代初始质心搜索

### 更多相关话题

+   [释放聚类：理解 K-均值聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [k-均值聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [使用 PyCaret 在 Python 中进行聚类的介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [为你的数据集选择合适的聚类算法](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

+   [机器学习中的 DBSCAN 聚类算法](https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html)

+   [什么是 K-Means 聚类以及它的算法是如何工作的？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)
