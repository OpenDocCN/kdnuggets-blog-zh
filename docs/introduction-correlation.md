# 相关性介绍

> 原文：[`www.kdnuggets.com/2023/05/introduction-correlation.html`](https://www.kdnuggets.com/2023/05/introduction-correlation.html)

![相关性介绍](img/7229eb0d0d86903866ca586deb020211.png)

编辑提供的图片

阅读完本文后，读者将学到以下内容：

+   相关性的定义

+   正相关

+   负相关

+   无相关性

+   相关性的数学定义

+   相关系数的 Python 实现

+   协方差矩阵

+   协方差矩阵的 Python 实现

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 方面支持你的组织

* * *

# 相关性

相关性衡量两个变量的共同变化程度。

# 正相关

如果变量***Y***在变量***X***增加时也增加，那么***X***和***Y***是正相关的，如下所示：

![相关性介绍](img/6ca85baa7bbc8012db6b13bcb32960a1.png)

X 和 Y 之间的正相关。 图片由作者提供。

# 负相关

如果变量***Y***在变量***X***增加时减少，那么***X***和***Y***是负相关的，如下所示：

![相关性介绍](img/d0e26ad8ba07ccc8b73da71d06084e5c.png)

X 和 Y 之间的负相关。 图片由作者提供。

# 无相关性

当***X***和**Y**之间没有明显关系时，我们说**X**和***Y***是无相关的，如下所示：

![相关性介绍](img/75101f9df34307dfdfb0fc0a2636b093.png)

X 和 Y 是无相关的。 图片由作者提供。

# 相关性的数学定义

设***X***和***Y***为两个特征

X = (X1 , X2 , . . ., Xn )

Y = (Y1 , Y2 , . . ., Yn )

***X***和***Y***之间的相关系数定义为

![相关性介绍](img/a7db4e9be71b492973664ef1f5d6d610.png)

其中 mu 和 sigma 分别表示均值和标准差，Xstd 是变量 X 的标准化特征。相关系数是***X***和***Y***的标准化特征之间的向量点积（标量积）。相关系数的取值范围在-1 到 1 之间。接近 1 的值表示强正相关，接近-1 的值表示强负相关，接近零的值表示低相关性或无相关性。

## 相关系数的 Python 实现

```py
import numpy as np

import matplotlib.pyplot as plt

n = 100

X = np.random.uniform(1,10,n)

Y = np.random.uniform(1,10,n)

plt.scatter(X,Y)

plt.show() 
```

![相关性介绍](img/392a538b7630ff8ed8124e5c990e5823.png)

X 和 Y 之间没有相关性。图片由作者提供。

```py
X_std = (X - np.mean(X))/np.std(X)

Y_std = (Y - np.mean(Y))/np.std(Y)

np.dot(X_std, Y_std)/n

0.2756215872210571

# Using numpy

np.corrcoef(X, Y)

array([[1\.        , 0.27562159],
       [0.27562159, 1\.        ]])
```

# 协方差矩阵

***协方差矩阵*** 是数据科学和机器学习中非常有用的矩阵。它提供了数据集中特征之间的共同变动（相关性）信息。协方差矩阵定义如下：

![相关性介绍](img/42dbeb899224d092171c0591d0cf6abe.png)

其中 mu 和 sigma 代表给定特征的均值和标准差。这里的 n 是数据集中观察的数量，j 和 k 的下标取值为 1, 2, 3, . . ., m，其中 m 是数据集中的特征数量。例如，如果一个数据集有 4 个特征和 100 个观察值，则 n = 100，m = 4，因此协方差矩阵将是一个 4 x 4 的矩阵。对角线元素将全为 1，因为它们表示特征与自身之间的相关性，根据定义，相关性等于 1。

## 协方差矩阵的 Python 实现

假设我想计算 4 只科技股票（AAPL、TSLA、GOOGL 和 AMZN）在 *1000* 天内的相关程度。我们的数据集有 m = 4 个特征和 n = 1000 个观察值。协方差矩阵将是一个 *4 x 4* 的矩阵，如下图所示。

![相关性介绍](img/b6326d19e65daebf91a0329a9f8d11c7.png)

技术股票之间的协方差矩阵。图片由作者提供。

生成上述图形的代码可以在这里找到：[数据科学和机器学习的基本线性代数](https://pub.towardsai.net/essential-linear-algebra-for-data-science-and-machine-learning-10d47d61000b?sk=b4f3e4b2cb99fbc7a0d196ac7f1b1207)。

# 总结

总之，我们回顾了相关性的基础知识。相关性定义了 2 个变量之间的共同变动程度。相关系数的取值范围在 -1 和 1 之间。接近零的值表示低相关性或无相关性。

**[本杰明·O·塔约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是物理学家、数据科学教育者和作家，同时也是 DataScienceHub 的拥有者。之前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关主题

+   [使用 PyCaret 进行二分类的介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 PyCaret 进行 Python 聚类的介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [数据科学的基本数学：奇异值分解的视觉介绍…](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [数据科学的 Pandas 入门](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

+   [关于代码论文的简要介绍](https://www.kdnuggets.com/2022/04/brief-introduction-papers-code.html)

+   [KDnuggets 新闻，4 月 27 日：关于代码论文的简要介绍；…](https://www.kdnuggets.com/2022/n17.html)
