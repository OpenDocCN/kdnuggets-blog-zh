# 这里是如何在 GPU 上加速你的数据科学

> 原文：[https://www.kdnuggets.com/2019/07/accelerate-data-science-on-gpu.html](https://www.kdnuggets.com/2019/07/accelerate-data-science-on-gpu.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

数据科学家需要计算能力。无论你是在用 Pandas 处理大型数据集，还是用 Numpy 在大规模矩阵上进行计算，你都需要一台强大的机器，以便在合理的时间内完成工作。

近年来，数据科学家常用的 Python 库在利用 CPU 性能方面已经相当出色。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

Pandas 使用 C 语言编写的底层基础代码，能够很好地处理超过 100GB 大小的数据集。如果你的内存不足以容纳这样的数据集，你可以使用方便的分块函数，一次处理一块数据。

### GPU 与 CPU：并行处理

面对大规模数据，CPU 真的不够用。

超过 100GB 大小的数据集将拥有很多数据点，在*百万*甚至*十亿*的范围内。处理这么多点时，不管你的 CPU 多快，它的核心数也不足以进行高效的并行处理。如果你的 CPU 有 20 个核心（这将是相当昂贵的 CPU），你一次只能处理 20 个数据点！

在时钟速度更为重要的任务中，CPU 更具优势——或者你根本没有 GPU 实现。如果你要执行的过程有 GPU 实现，那么如果任务可以从并行处理中受益，GPU 将会更加有效。

![figure-name](../Images/45f79f5c92359a10ccb30f2b8fbbef83.png)多核系统如何更快处理数据。对于单核系统（左），所有 10 个任务都分配给一个节点。对于双核系统（右），每个节点处理 5 个任务，从而加倍处理速度

深度学习已经充分利用了 GPU。深度学习中的许多卷积操作是重复性的，因此可以在 GPU 上大大加速，甚至达到 100 倍。

现在的数据科学也没有不同，因为许多重复的操作在大数据集上执行，使用库如 Pandas、Numpy 和 Scikit-Learn。这些操作在 GPU 上实现也并不复杂。

最后，解决方案来了。

### 使用 Rapids 进行 GPU 加速

[Rapids](https://rapids.ai/?source=post_page---------------------------) 是一套加速数据科学的软件库，通过利用 GPU 提供加速。它使用低级 CUDA 代码实现快速、GPU 优化的算法，同时在其上有一个易于使用的 Python 层。

Rapids 的美妙之处在于它与数据科学库的无缝集成——像 Pandas 数据框这样的数据可以轻松地传递给 Rapids 以进行 GPU 加速。下面的图示说明了 Rapids 如何实现低级加速，同时保持易于使用的顶层。

![figure-name](../Images/f511cd54476f8f41e8586d8a442db6e8.png)

Rapids 利用多个 Python 库：

+   [**cuDF**](https://github.com/rapidsai/cudf?source=post_page---------------------------) — Python GPU 数据框。它可以在数据处理和操作方面做几乎所有 Pandas 能做的事情。

+   [**cuML**](https://github.com/rapidsai/cuml?source=post_page---------------------------) — Python GPU 机器学习。它包含了 Scikit-Learn 具有的许多 ML 算法，格式非常相似。

+   [**cuGraph**](https://github.com/rapidsai/cuml?source=post_page---------------------------) — Python GPU 图处理。它包含许多常见的图分析算法，包括 PageRank 和各种相似度度量。

### 如何使用 Rapids 的教程

**安装**

现在你将了解如何使用 Rapids！

要安装它，请访问[网站](https://rapids.ai/start.html?source=post_page---------------------------)，在那里你会看到如何安装 Rapids。你可以通过[Conda](https://anaconda.org/anaconda/conda?source=post_page---------------------------)直接在你的机器上安装，或者直接拉取 Docker 容器。

在安装时，你可以设置系统规格，如 CUDA 版本以及你希望安装哪些库。例如，我有 CUDA 10.0，并且想要安装所有库，所以我的安装命令是：

```py

conda install -c nvidia -c rapidsai -c numba -c conda-forge -c pytorch -c defaults cudf=0.8 cuml=0.8 cugraph=0.8 python=3.6 cudatoolkit=10.0

```

一旦命令运行完成，你就可以开始进行 GPU 加速的数据科学了。

**设置我们的数据**

在本教程中，我们将通过修改版的[DBSCAN 演示](https://github.com/rapidsai/notebooks/blob/branch-0.8/tutorials/DBSCAN_Demo_Full.ipynb?source=post_page---------------------------)进行操作。我将使用[Nvidia Data Science Work Station](https://towardsdatascience.com/nvidias-new-data-science-workstation-a-review-and-benchmark-e451db600551?source=post_page---------------------------)进行测试，该设备配备了 2 个 GPU。

DBSCAN 是一种基于密度的 [聚类算法](https://towardsdatascience.com/the-5-clustering-algorithms-data-scientists-need-to-know-a36d136ef68?source=post_page---------------------------)，可以自动对数据进行分类，而无需用户指定有多少组。它在 [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html?source=post_page---------------------------) 中有一个实现。

我们将从设置所有导入项开始，包括加载数据、可视化数据和应用 ML 模型的库。

```py

import os
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
from sklearn.datasets import make_circles

```

*make_circles* 函数将自动创建一个类似于两个圆的复杂数据分布，我们将对其应用 DBSCAN。

让我们从创建 100,000 个点的数据集并在图中可视化它开始：

```py

X, y = make_circles(n_samples=int(1e5), factor=.35, noise=.05)
X[:, 0] = 3*X[:, 0]
X[:, 1] = 3*X[:, 1]
plt.scatter(X[:, 0], X[:, 1])
plt.show()

```

![figure-name](../Images/12a054c98c39b6df2d4aecb2381e666c.png)

**在 CPU 上运行 DBSCAN**

使用 Scikit-Learn 在 CPU 上运行 DBSCAN 很简单。我们将导入我们的算法并设置一些参数。

```py

from sklearn.cluster import DBSCAN
db = DBSCAN(eps=0.6, min_samples=2)

```

现在我们可以通过 Scikit-Learn 的一个函数调用来应用 DBSCAN 于我们的圆形数据。在我们的函数前加上`%%time`可以告诉 Jupyter Notebook 测量其运行时间。

```py

%%time
y_db = db.fit_predict(X)

```

对于这 100,000 个点，运行时间为 8.31 秒。结果图如下所示。

![figure-name](../Images/d3b76a1e32d8538015ea451df118ecf2.png)使用 Scikit-Learn 在 CPU 上运行 DBSCAN 的结果

**使用 Rapids 在 GPU 上运行 DBSCAN**

现在让我们用 Rapids 提速吧！

首先，我们将把数据转换为 `pandas.DataFrame`，然后使用它创建 `cudf.DataFrame`。Pandas 数据框可以无缝地转换为 cuDF 数据框，而数据格式不会发生变化。

```py

import pandas as pd
import cudf

X_df = pd.DataFrame({'fea%d'%i: X[:, i] for i in range(X.shape[1])})
X_gpu = cudf.DataFrame.from_pandas(X_df)

```

然后我们将从 cuML 中导入并初始化一个 *特殊* 版本的 DBSCAN，它是 GPU 加速的。cuML 版本的 DBSCAN 函数格式与 Scikit-Learn 完全相同 —— 参数、风格、函数均相同。

```py

from cuml import DBSCAN as cumlDBSCAN

db_gpu = cumlDBSCAN(eps=0.6, min_samples=2)

```

最后，我们可以在测量运行时间的同时运行 GPU DBSCAN 的预测函数。

```py

%%time
y_db_gpu = db_gpu.fit_predict(X_gpu)
```

GPU 版本的运行时间为 4.22 秒，几乎是 2 倍的加速。结果图与 CPU 版本完全相同，因为我们使用的是相同的算法。

![figure-name](../Images/d3b76a1e32d8538015ea451df118ecf2.png)使用 cuML 在 GPU 上运行 DBSCAN 的结果

### 使用 Rapids GPU 获得超高速

从 Rapids 获得的加速量取决于我们处理的数据量。一个好的经验法则是，较大的数据集将受益于 GPU 加速。将数据在 CPU 和 GPU 之间传输时会有一些开销 —— 随着数据集的增大，这种开销变得更“值得”。

我们可以用一个简单的例子来说明这一点。

我们将创建一个随机数的 Numpy 数组，并对其应用 DBSCAN。我们将比较常规 CPU DBSCAN 和来自 cuML 的 GPU 版本的速度，同时增加和减少数据点的数量，以观察这如何影响运行时间。

下面的代码演示了这个测试：

```py

import numpy as np

n_rows, n_cols = 10000, 100
X = np.random.rand(n_rows, n_cols)
print(X.shape)

X_df = pd.DataFrame({'fea%d'%i: X[:, i] for i in range(X.shape[1])})
X_gpu = cudf.DataFrame.from_pandas(X_df)

db = DBSCAN(eps=3, min_samples=2)
db_gpu = cumlDBSCAN(eps=3, min_samples=2)

%%time
y_db = db.fit_predict(X)

%%time
y_db_gpu = db_gpu.fit_predict(X_gpu)

```

查看下方 Matplotlib 结果的图示：

![figure-name](../Images/6ec9f65915ba982f6379a5af54f22934.png)

使用 GPU 代替 CPU 时，性能提升幅度非常显著。即使在 10,000 点（最左边）时，我们仍然能获得 4.54 倍的加速。在高端情况下，使用 10,000,000 点时，切换到 GPU 可以获得 88.04 倍的加速！

### 喜欢学习吗？

在 [twitter](https://twitter.com/GeorgeSeif94?source=post_page---------------------------) 上关注我，了解最新的 AI、技术和科学动态！也可以在 [LinkedIn](https://www.linkedin.com/in/georgeseif/?source=post_page---------------------------) 上与我联系！

### 推荐阅读

想了解更多关于数据科学的内容吗？[***Python 数据科学手册***](https://amzn.to/2GZN4Xv?source=post_page---------------------------) 是学习如何用 Python 进行*真实*数据科学的最佳资源！

另外，提醒一下，我通过 Amazon 关联链接支持这个博客，因为分享好书对大家都有帮助！作为 Amazon 联盟会员，我从合格的购买中获得收益。

**个人简介：[George Seif](https://towardsdatascience.com/@george.seif94)** 是一位认证极客和 AI / 机器学习工程师。

[原文](https://towardsdatascience.com/heres-how-you-can-accelerate-your-data-science-on-gpu-4ecf99db3430)。经授权转载。

**相关：**

+   [Nvidia 的新数据科学工作站——评论与基准测试](/2019/07/nvidia-new-data-science-workstation.html)

+   [探索 Transformer 架构——第 2 部分：Transformer 的工作原理简述](/2019/07/transformer-architecture-part-2.html)

+   [XGBoost 在 GPUs 上：解锁机器学习的性能和生产力](/2018/12/nvidia-xgboost-gpu-machine-learning-performance-productivity.html)

### 更多相关主题

+   [构建 GPU 机器与使用 GPU 云](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)

+   [通过 Uplimit 的 Metaflow 加速你的机器学习之旅…](https://www.kdnuggets.com/2023/10/uplimit-accelerate-your-machine-learning-journey-metaflow-mastery-course)

+   [如何使用分析来加速业务增长？](https://www.kdnuggets.com/2022/12/analytics-accelerate-business-growth.html)

+   [想利用你的数据技能解决全球问题？这里有一些建议…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [使用 RAPIDS cuDF 在特征工程中利用 GPU](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [掌握 GPUs：Python 中 GPU 加速数据帧的初学者指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)
