# 数据科学的基础数学：特征向量及其在 PCA 中的应用

> 原文：[https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html](https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html)

*矩阵分解*，也叫做*矩阵因式分解*，是将一个矩阵拆分成多个部分的过程。在数据科学的背景下，你可以例如用它来选择数据的部分，旨在减少维度而不丢失太多信息（例如在主成分分析中，你将在这篇文章的后面看到）。一些操作也可以更容易地在分解后的矩阵上进行计算。

在这篇文章中，你将学习矩阵的特征分解。理解它的一种方法是将其视为一种特殊的基变换（有关基变换的更多细节，请参阅[我上一篇文章](https://hadrienj.github.io/posts/Essential-Math-for-Data-Science-Change-of-Basis/)）。你将首先了解特征向量和特征值，然后看到它如何应用于主成分分析（PCA）。主要思想是将矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 的特征分解视为基变换，其中新的基向量是特征向量。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

# 特征向量和特征值

如你在[数据科学的基础数学](https://www.essentialmathfordatascience.com/?utm_source=hadrienj&utm_medium=blog&utm_campaign=hadrienj_2021-02-01-Essential-Math-for-Data-Science-Eigendecomposition.md)第七章中看到的，你可以将矩阵视为线性变换。这意味着如果你取任意一个向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) 并对其应用矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png)，你会得到一个变换后的向量 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png)。

以以下例子为例：

![Equation](../Images/dee23a1453606cccba63318b471db7cd.png)

和

![Equation](../Images/ffd25749845bb0ffe9bb83c6cf3555ea.png)

如果你将 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 应用于向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) （使用矩阵-向量乘积），你会得到一个新向量：

![Equation](../Images/ee1a6407bd3de954cc6b370389f22b49.png)

![Equation](../Images/6043576e294a8d22f397ca24b1e957fd.png)

![Equation](../Images/20a8b29fa654acfac64968c9d78f3cdc.png)

![Equation](../Images/f61d37b006830115c275a9ef5faa4c42.png)

让我们绘制初始向量和变换后的向量：

```py
u = np.array([1.5, 1])

A = np.array([
   [1.2, 0.9],
   [0, -0.4]
])

v = A @ u
```

```py
plt.quiver(0, 0, u[0], u[1], color="#2EBCE7", angles='xy', scale_units='xy', scale=1)

plt.quiver(0, 0, v[0], v[1], color="#00E64E", angles='xy', scale_units='xy', scale=1)

# [...] Add axes, styles, vector names
```

![数据科学基础数学：特征向量及其在PCA中的应用](../Images/d9ef15de870b38c85005ee8a32e0ea9e.png)

图1：矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 对向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) 的变换成向量 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png)。

注意，如你所料，变换后的向量 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png) 并不与初始向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) 在同一方向上。这种方向的变化是你可以用 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 变换的大多数向量的特征。

然而，考虑以下向量：

![Equation](../Images/f2e88aceae6e8e7f7b10526bb8bd06b6.png)

让我们将矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 应用于向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 以获得一个向量 ![Equation](../Images/ea571e13bda6431149977a7da4e30a98.png)：

```py
x = np.array([-0.4902, 0.8715])

y = A @ x

plt.quiver(0, 0, x[0], x[1], color="#2EBCE7", angles='xy',
          scale_units='xy', scale=1)

plt.quiver(0, 0, y[0], y[1], color="#00E64E", angles='xy',
          scale_units='xy', scale=1)

# [...] Add axes, styles, vector names
```

![数据科学基础数学：特征向量及其在PCA中的应用](../Images/aaa03eb844fe9221ed7a97da30f230d6.png)

图2：矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 对特殊向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 的变换。

从图2中可以看出，向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 与矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 具有特殊关系：它被缩放（带有负值），但初始向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 和变换后的向量 ![Equation](../Images/ea571e13bda6431149977a7da4e30a98.png) 都在同一条直线上。

向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 是 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 的*特征向量*。它仅被一个值缩放，这个值称为矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 的*特征值*。矩阵 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 的特征向量是在矩阵变换时被缩短或拉长的向量。特征值是缩短或拉长向量的缩放因子。

从数学上讲，向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 是 ![Equation](../Images/a4101ce2e79e16bd0d29088d759ceed9.png) 的特征向量，如果：

![Equation](../Images/ec7ce5865031a7580f2590d4162f88b9.png)

其中 ![Equation](../Images/ef17aed938e5576b0a5e042246b4ac47.png)（读作“lambda”）是与特征向量 ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png) 对应的特征值。

> **特征向量**
> 
> 矩阵的特征向量是非零向量，当矩阵作用于它们时只是被重新缩放。如果缩放因子为正，初始向量和变换后的向量的方向相同；如果为负，则方向相反。

**特征向量的数量**

一个![方程](../Images/9a60fefb21d8c288f3b817f86bef589d.png)-by-![方程](../Images/9a60fefb21d8c288f3b817f86bef589d.png)矩阵最多具有![方程](../Images/9a60fefb21d8c288f3b817f86bef589d.png)个线性独立的特征向量。然而，每个特征向量乘以一个非零标量也是一个特征向量。如果你有：

![方程](../Images/5184f65ed2fd4fc2c8ca10dfc0147dcd.png)

然后：

![方程](../Images/450cc7ad9f7b7c3b13e31d23d61da926.png)

具有![方程](../Images/d571d7685ad3d9f4acf544844e771d21.png)任何非零值。

这排除了零向量作为特征向量，因为你会有

![方程](../Images/31c9d00058c36ce6f668e66d7d1179a5.png)

在这种情况下，每个标量将是一个特征值，因此会变得未定义。

# 实践项目：主成分分析

*主成分分析*（PCA）是一种算法，用于减少数据集的维度。例如，它在减少计算时间、压缩数据或避免所谓的*维数灾难*方面很有用。它也对可视化很有帮助：高维数据很难可视化，减少维数可以有助于绘制数据。

在这个实践项目中，你将使用你可以在[数据科学基础数学](https://www.essentialmathfordatascience.com/?utm_source=hadrienj&utm_medium=blog&utm_campaign=hadrienj_2021-02-01-Essential-Math-for-Data-Science-Eigendecomposition.md)一书中学习的各种概念，如基变换（第7.5节和9.2节，一些示例[在这里](https://hadrienj.github.io/posts/Essential-Math-for-Data-Science-Change-of-Basis/)）、特征分解（第9章）或协方差矩阵（第2.1.3节），以理解PCA是如何工作的。

在第一部分，你将学习投影、解释方差和误差最小化之间的关系，首先通过一点理论，然后通过对啤酒数据集（啤酒消费与温度的关系）进行PCA编码。请注意，你还会在[数据科学基础数学](https://www.essentialmathfordatascience.com/?utm_source=hadrienj&utm_medium=blog&utm_campaign=hadrienj_2021-02-01-Essential-Math-for-Data-Science-Eigendecomposition.md)中找到另一个示例，在那里你将使用Sklearn对音频数据进行PCA，以根据类别可视化音频样本，然后压缩这些音频样本。

## 深入分析

### 理论背景

PCA的目标是将数据投影到较低维度的空间，同时尽可能保留数据中包含的所有信息。这个问题可以被视为一个*垂直最小二乘*问题，也称为*正交回归*。

在这里你会看到，当投影线与数据方差最大的方向相符时，正交投影的误差被最小化。

### 方差和投影

首先需要理解的是，当你的数据集的特征不完全不相关时，一些方向的方差大于其他方向。

![数据科学中的基本数学：特征值和主成分分析的应用](../Images/8d12f3afe57864e2400e68d5d2be0c74.png)>

图3：在向量![方程](../Images/a1bb06b27d7ad44981ed7b8aae646595.png)（红色）方向上的数据方差大于在向量![方程](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png)（绿色）方向上的数据方差。

将数据投影到低维空间意味着你可能会丢失一些信息。在图3中，如果将二维数据投影到一条线上的话，投影数据的方差告诉你你丢失了多少信息。例如，如果投影数据的方差接近零，这意味着数据点会被投影到非常接近的位置：你丢失了大量信息。

因此，PCA的目标是改变数据矩阵的基，使得具有最大方差的方向（图3中的![方程](../Images/a1bb06b27d7ad44981ed7b8aae646595.png)）成为第一个*主成分*。第二个成分是与第一个成分正交的具有最大方差的方向，以此类推。

当你找到PCA的组件时，你会改变数据的基，使得这些组件成为新的基向量。这个变换后的数据集具有新的特征，这些特征是组件，它们是初始特征的线性组合。通过仅选择一些组件来减少维度。

![数据科学中的基本数学：特征值和主成分分析的应用](../Images/bf56f430ef597ae9d416efcb74b7a827.png)

图4：基变换，使得最大方差位于![方程](../Images/18d793480e7f7fb6e486026b18729633.png)-轴。

作为说明，图4显示了基变换后的数据：最大方差现在与![方程](../Images/18d793480e7f7fb6e486026b18729633.png)-轴相关。例如，你可以只保留这一维度。

换句话说，将主成分分析（PCA）表达为基变换，其目标是找到一个新的基（该基是初始基的线性组合），使得数据的方差在第一个维度上达到最大。

### 最小化误差

找到最大化方差的方向类似于最小化数据与其投影之间的误差。

![数据科学中的基本数学：特征值和主成分分析的应用](../Images/0a186ddc1f3ae7e09e131edc60cc8444.png)

图5：最大化方差的方向也是与最小误差（用灰色表示）相关的方向。

在图5中你可以看到，左图中显示了较低的误差。由于投影是正交的，投影方向上的方差不会影响误差。

### 寻找最佳方向

在改变数据集的基之后，你应该有一个特征之间的协方差接近零（例如图4）。换句话说，你希望转换后的数据集具有对角协方差矩阵：每对主成分之间的协方差为零。

你可以在 [数据科学的基础数学](https://www.essentialmathfordatascience.com/?utm_source=hadrienj&utm_medium=blog&utm_campaign=hadrienj_2021-02-01-Essential-Math-for-Data-Science-Eigendecomposition.md) 第9章中看到，你可以使用特征分解将矩阵对角化（使矩阵变为对角矩阵）。因此，你可以计算数据集的协方差矩阵的特征向量。它们将给你协方差矩阵对角化的新基方向。

总结来说，主成分是计算为数据集协方差矩阵的特征向量。此外，特征值提供了相应特征向量的解释方差。因此，通过根据特征值的降序对特征向量进行排序，你可以按重要性顺序对主成分进行排序，并最终去除与小方差相关的主成分。

### 计算PCA

**数据集**

让我们用2015年在巴西圣保罗的啤酒消费和温度数据集来说明PCA是如何工作的。

让我们加载数据，并绘制温度作为消费的函数：

```py
data_beer = pd.read_csv("https://raw.githubusercontent.com/hadrienj/essential_math_for_data_science/master/data/beer_dataset.csv")
plt.scatter(data_beer['Temperatura Maxima (C)'],
           data_beer['Consumo de cerveja (litros)'],
           alpha=0.3)

# [...] Add labels and custom axes
```

![数据科学的基础数学：特征向量与PCA应用](../Images/768a9d226e65a2449abd32e1520240cc.png)

图6：温度作为啤酒消费的函数。

现在，让我们创建数据矩阵 ![Equation](../Images/fad5207092e118bbda7de8e70211a6f4.png) ，包含两个变量：温度和消费。

```py
X = np.array([data_beer['Temperatura Maxima (C)'],
           data_beer['Consumo de cerveja (litros)']]).T

X.shape
```

```py
(365, 2)
```

矩阵 ![Equation](../Images/fad5207092e118bbda7de8e70211a6f4.png) 有365行和两列（这两个变量）。

**协方差矩阵的特征分解**

如你所见，第一步是计算数据集的协方差矩阵：

```py
C = np.cov(X, rowvar=False)

C
```

```py
array([[18.63964745, 12.20609082],
       [12.20609082, 19.35245652]])
```

记住，你可以这样理解：对角线值分别是第一个和第二个变量的方差。这两个变量之间的协方差大约为12.2。

现在，你将计算该协方差矩阵的特征向量和特征值：

```py
eigvals, eigvecs = np.linalg.eig(C)
eigvals, eigvecs
```

```py
(array([ 6.78475896, 31.20734501]),
array([[-0.71735154, -0.69671139],
       [ 0.69671139, -0.71735154]]))
```

你可以将特征向量存储为两个向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) 和 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png)。

```py
u = eigvecs[:, 0].reshape(-1, 1)

v = eigvecs[:, 1].reshape(-1, 1)
```

让我们用数据绘制特征向量（注意你应该使用中心化的数据，因为它是计算协方差矩阵时使用的数据）。

你可以按对应的特征值缩放特征向量，这些特征值是解释的方差。为了可视化目的，我们使用三个标准差的向量长度（等于三倍的解释方差的平方根）：

```py
X_centered = X - X.mean(axis=0)

plt.quiver(0, 0,
          2 * np.sqrt(eigvals[0]) * u[0], 2 * np.sqrt(eigvals[0]) * u[1],
          color="#919191", angles='xy', scale_units='xy', scale=1,
          zorder=2, width=0.011)

plt.quiver(0, 0,
          2 * np.sqrt(eigvals[1]) * v[0], 2 * np.sqrt(eigvals[1]) * v[1],
          color="#FF8177", angles='xy', scale_units='xy', scale=1,
          zorder=2, width=0.011)

plt.scatter(X_centered[:, 0], X_centered[:, 1], alpha=0.3)

# [...] Add axes
```

![数据科学的基本数学：特征向量及其在 PCA 中的应用](../Images/7e866c41618f3917728a398331a0f01a.png)

图7：特征向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) （灰色）和 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png) （红色），按解释的方差进行缩放。

在图7中，你可以看到协方差矩阵的特征向量为你提供了数据的重要方向。红色的向量 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png) 与最大的特征值相关联，因此对应于具有最大方差的方向。灰色的向量 ![Equation](../Images/a1bb06b27d7ad44981ed7b8aae646595.png) 与 ![Equation](../Images/97ef2f6485b158f4ab2fca48fc9cc55e.png) 正交，是第二主成分。

然后，你只需使用特征向量作为新的基准向量来改变数据的基准。但首先，你可以根据特征值按降序排序特征向量：

```py
sort_index = eigvals.argsort()[::-1]
eigvals_sorted = eigvals[sort_index]

eigvecs_sorted = eigvecs[:, sort_index]

eigvecs_sorted
```

```py
array([[-0.69671139, -0.71735154],
       [-0.71735154,  0.69671139]])
```

既然你的特征向量已经排序好了，我们来改变数据的基准：

```py
X_transformed = X_centered @ eigvecs_sorted
```

你可以绘制变换后的数据，以检查主成分现在是否不相关：

```py
plt.scatter(X_transformed[:, 0], X_transformed[:, 1], alpha=0.3)

# [...] Add axes
```

![数据科学的基本数学：特征向量及其在 PCA 中的应用](../Images/f9725bfa0d344f604d5cc1d8dc8003fb.png)

图8：新基准下的数据集。

图8显示了新基准下的数据样本。你可以看到第一个维度（ ![Equation](../Images/18d793480e7f7fb6e486026b18729633.png)-轴）对应于具有最大方差的方向。

你可以在这个新基准下只保留数据的第一个主成分，而不会丢失太多信息。

> **协方差矩阵还是奇异值分解？**
> 
> 使用协方差矩阵计算 PCA 的一个警告是，当特征较多时（如在本动手操作的第二部分中的音频数据），计算可能会很困难。因此，通常更倾向于使用奇异值分解（SVD）来计算 PCA。

**[Hadrien Jean](https://hadrienj.github.io/)** 是一位机器学习科学家。他拥有巴黎高等师范学院的认知科学博士学位，在那里他使用行为和电生理数据进行听觉感知研究。他曾在工业界工作，建立了用于语音处理的深度学习管道。在数据科学与环境交汇处，他从事关于生物多样性评估的项目，利用应用于音频录音的深度学习。他还定期在 Le Wagon（数据科学训练营）创建内容并教授课程，并在他的博客上撰写文章（[hadrienj.github.io](http://hadrienj.github.io)）。

[原文](https://hadrienj.github.io/posts/Essential-Math-for-Data-Science-Eigendecomposition/)。经许可转载。

### 更多相关话题

+   [如何克服对数学的恐惧并学习数据科学中的数学](https://www.kdnuggets.com/2021/03/overcome-fear-learn-math-data-science.html)

+   [使用 Scikit-Learn 进行主成分分析 (PCA)](https://www.kdnuggets.com/2023/05/principal-component-analysis-pca-scikitlearn.html)

+   [数据科学的基础数学：奇异值分解的可视化介绍…](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [使用 Google Earth 构建 Python 地理空间应用程序…](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [创建一个 Web 应用程序以提取音频中的主题](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [本周改进搜索应用程序的 8 种方法](https://www.kdnuggets.com/2022/09/corise-8-ways-improve-search-application-week.html)
