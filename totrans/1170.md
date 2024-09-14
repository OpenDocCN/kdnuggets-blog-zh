# 数据科学的基础数学：线性方程组简介

> 原文：[https://www.kdnuggets.com/2021/08/essential-math-data-science-introduction-systems-linear-equations.html](https://www.kdnuggets.com/2021/08/essential-math-data-science-introduction-systems-linear-equations.html)

[评论](#comments)[![图片](../Images/e6b504358bea8f63e781d7dbce4b4afc.png)](https://www.essentialmathfordatascience.com/?utm_source=kdnuggets&utm_medium=blog&utm_campaign=kdnuggets_linear_equations)

## 线性方程组

在本文中，你将能够利用你对向量、矩阵和线性组合的了解（分别是[数据科学的基础数学](https://bit.ly/3oGHXyR) 第05章、第06章和第07章）。这将使你能够将数据转换为线性方程组。在本章末尾（在 [数据科学的基础数学](https://bit.ly/3oGHXyR)），你将看到如何使用方程组和线性代数来解决线性回归问题。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

线性方程是变量之间关系的形式化表示。以以下方程定义的变量 *x* 和 *y* 之间的线性关系为例：

![方程式](../Images/81b6ad3602dd2fcd21fb290a04cad0e8.png)

你可以在笛卡尔平面上表示这种关系：

```py
# create x and y vectors
x = np.linspace(-2, 2, 100)
y = 2 * x + 1
plt.plot(x, y)
# [...] Add axes and styles 
```

![图1：方程 $y=2x+1$ 的图示。](../Images/f3b103528b7550b0cf4698f3a979c094.png)

*图1：方程 ![方程式](../Images/81b6ad3602dd2fcd21fb290a04cad0e8.png) 的图示。*

记住，线上的每个点都对应于这个方程的一个解：如果你用线上的一点的坐标替换 *x* 和 *y* 在这个方程中，等式就成立。这意味着有无数的解（线上的每个点）。

还可以考虑使用相同变量的多个线性方程：这就是一个*方程组*。

## 线性方程组

方程组是一组描述变量之间关系的方程。例如，考虑以下示例：

![方程式](../Images/f1fe8a809085e7704309cda4c0bc2b97.png)

你有两个线性方程，它们都描述了变量 *x* 和 *y* 之间的关系。这是一个包含两个方程和两个变量（在这种情况下也称为*未知数*）的系统。

你可以将线性方程组（系统的每一行）视作多个方程，每个方程对应一条直线。这称为 *行图示*。

你也可以将系统视作不同的列，代表系数缩放变量。这称为 *列图示*。让我们深入了解这两种图示。

### 行图示

在行图示中，系统的每一行对应一个方程。在前面的例子中，有两个方程描述了两个变量 *x* 和 *y* 之间的关系。

**行图示的图形表示**

让我们将这两个方程式进行图形表示：

```py
# create x and y vectors
x = np.linspace(-2, 2, 100)
y = 2 * x + 1
y1 = -0.5 * x + 3
plt.plot(x, y)
plt.plot(x, y1)
# [...] 
```

![图 2：我们系统中两个方程式的表示。](../Images/13e3a8434994558b52d80aeaf52edd0d.png)

*图 2：我们系统中两个方程式的表示。*

拥有多个方程式意味着 *x* 和 *y* 的值必须满足更多方程式。记住，来自第一个方程的 *x* 和 *y* 与第二个方程中的 *x* 和 *y* 是相同的。

所有在蓝色线上的点满足第一个方程，所有在绿色线上的点满足第二个方程。这意味着只有在两条线上的交点才满足这两个方程。当 *x* 和 *y* 取值为线交点的坐标时，方程组被解出。

在这个例子中，这个点的 *x* 坐标为 0.8，*y* 坐标为 2.6。如果你将这些值代入方程组中，你会得到：

![方程式](../Images/4fa1fa142db2085796e98db76ca929f2.png)

这是一种几何方式来解决方程组。线性系统解为 *x*=0.8 和 *y*=2.6。

### 列图示

将系统视作列图示：你将系统视作未知值 (*x* 和 *y*) 缩放向量。

为了更好地理解这一点，让我们重新排列方程式，将变量放在一侧，将常数放在另一侧。对于第一个方程式，你有：

![方程式](../Images/93eb1d00f61a16ba872b154f0157879b.png)

对于第二个方程式：

![方程式](../Images/87f14e1e1e983018835231d98f734bce.png)

你现在可以将系统写成：

![方程式](../Images/70ae30afb29f1a98c26cdbe60e173a3d.png)

现在你可以查看图 3 来了解如何将两个方程式转换为一个 *向量方程式*。

![图 3：将方程组视作由变量 <em>x</em> 和 <em>y</em> 缩放的列向量。](../Images/a9c7c301c8b1bf50ec4f2c6840616d2c.png)

*图 3：将方程组视作由变量 *x* 和 *y* 缩放的列向量。*

在图 3 的右侧，你会看到向量方程式。左侧有两个列向量，右侧有一个列向量。正如你在 [数据科学的基础数学](https://bit.ly/3oGHXyR)中看到的，这对应于以下向量的线性组合：

![方程式](../Images/aa31210d8492f7bbdbd56304134cc378.png)

以及

![方程式](../Images/72bf3dfa279443975f21920c0be45994.png)

在列图中，你用一个向量方程代替多个方程。从这个角度看，你要找到使右侧向量出现的左侧向量的线性组合。

列图中的解是一样的。行图和列图只是考虑方程组的两种不同方式：

![方程](../Images/727e2d0973be7597614bc88f63370024.png)

它有效：如果你使用几何上找到的解，你会得到右侧向量。

**列图的图形表示**

让我们考虑将方程组表示为向量的线性组合。再以之前的例子为例：

![方程](../Images/d847d5812e3ff938ac4518239add2c61.png)

图4展示了来自左侧（在图片中蓝色和红色的向量）和右侧（在图片中绿色的向量）方程的两个向量的图形表示。

![图4：由<x>和<y>缩放的向量组合得到右侧向量。](../Images/836aa50f4dca6e9f5340c7dc0f6198d9.png)

*图4：由*x*和*y*缩放的向量组合得到右侧向量。*

你可以在图4中看到，通过组合左侧向量可以得到右侧向量。如果你将向量缩放为2.6和0.8，线性组合可以得到方程右侧的向量。

### 解的数量

在一些线性系统中，没有唯一解。实际上，线性方程组可以有：

+   没有解。

+   一个解。

+   无限多个解。

让我们考虑这三种可能性（包括行图和列图），以了解为什么线性系统不可能有多于一个解或少于无限多个解。

**示例1：无解**

让我们考虑以下的线性方程组，仍然是两个方程和两个变量：

![方程](../Images/b227c1bddc8f0141e4ed5b47f27becac.png)

我们从表示这些方程开始：

```py
# create x and y vectors
x = np.linspace(-2, 2, 100)
y = 2 * x + 1
y1 = 2 * x + 3

plt.plot(x, y)
plt.plot(x, y1)
# [...] Add axes, styles... 
```

![图5：平行的方程线。](../Images/f9398219b887096f53c0e2572972f940.png)

*图5：平行的方程线。*

如图5所示，没有任何点同时位于蓝线和绿线上。这意味着这个方程组没有解。

你也可以通过列图从图形上理解为什么没有解。让我们将方程组写作如下：

![方程](../Images/27310e8c1fc7c3adb19fc873fe1bfb5a.png)

将其写作列向量的线性组合，你有：

![方程](../Images/4e9d599592544d56f7cdc8a9fff4f065.png)

![图6：没有解的线性系统的列图。](../Images/935794e141fc06a4cba6a110743d7b6c.png)

*图6：没有解的线性系统的列图。*

图6显示了系统的列向量。你可以看到，通过组合蓝色和红色向量无法到达绿色向量的端点。原因是这些向量是线性相关的（更多细节见 [数据科学的基础数学](https://bit.ly/3oGHXyR)）。要到达的向量超出了你所组合的向量的跨度。

**示例2. 无限多个解**

你还可能遇到另一个系统具有无限多个解的情况。我们考虑以下系统：

![方程](../Images/007d7642ff374427396a87065e876c4f.png)

```py
# create x and y vectors
x = np.linspace(-2, 2, 100)
y = 2 * x + 1
y1 = (4 * x + 2) / 2

plt.plot(x, y)
plt.plot(x, y1, alpha=0.3)
# [...] Add axes, styles... 
```

![图7：方程直线重叠。](../Images/a77e9545b4708c74e242060389abace1.png)

*图7：方程直线重叠。*

由于方程是相同的，所以两个直线上的点无限多个，因此这个线性方程组有无限多个解。这例如类似于单个方程和两个变量的情况。

从列图的角度来看，你有：

![方程](../Images/ba9cf560ef7c8166aabaa203e84e2e4e.png)

并使用向量表示法：

![方程](../Images/6136f9ce38ccc712a6354c58391bd119.png)

![图8：具有无限多个解的线性系统的列图。](../Images/12812b152e0c617cd69178a7980f4c5e.png)

*图8：具有无限多个解的线性系统的列图。*

图8显示了图形化表示的对应向量。你可以看到，通过蓝色和红色向量的组合，有无限多种方法到达绿色向量的端点。

由于两个向量朝相同方向，因此存在无限多个线性组合可以到达右侧向量。

**总结**

总结来说，你可以有三种可能的情况，如图9所示，包含两个方程和两个变量。

![图9：两个方程和两个变量的三种情况的总结。](../Images/84268c1970aa62950646249dddb28c73.png)

*图9：两个方程和两个变量的三种情况的总结。*

不可能有两条直线交叉超过一次而少于无限次。

这一原理适用于更多维度。例如，在IR³中，三个平面中至少有两个可以平行（无解），三个可以相交（一个解），或三个可以重叠（无限多个解）。

### 矩阵表示线性方程

现在你可以使用列图表示向量方程，你可以进一步使用矩阵来存储列向量。

我们再次考虑以下线性系统：

![方程](../Images/d847d5812e3ff938ac4518239add2c61.png)

记住来自 [数据科学的基础数学](https://bit.ly/3oGHXyR) 的内容，你可以将线性组合写作矩阵-向量乘积。矩阵对应于左侧两个列向量的串联：

![方程](../Images/1d46920e1c25a6461a32012b73758dcc.png)

向量对应于矩阵的列向量的系数权重（此处为 *x* 和 *y*）：

![方程](../Images/07a4724c3464b955d08dc1cdee3c4e3e.png)

你的线性系统变成如下矩阵方程：

![方程](../Images/a6f9af7a9c5599a1cc62bb2b76277001.png)

**符号说明**

这导致了广泛用于书写线性系统的符号表示：

![方程](../Images/0bd46c76e5c603239edf25c11d44745a.png)

其中 *A* 是包含列向量的矩阵，*x* 是系数向量，*b* 是结果向量，我们称之为 *目标向量*。它使你能够从单独考虑方程的微积分，转到将线性系统的每一部分表示为向量和矩阵的线性代数。这种抽象非常强大，并将向量空间理论应用于求解线性方程组。

在列向量图像中，你需要找到方程左侧列向量线性组合的系数。只有当目标向量在这些列向量的跨度内时，解才存在。

**个人简介： [Hadrien Jean](https://hadrienj.github.io/)** 是一位机器学习科学家。他拥有巴黎高等师范学院的认知科学博士学位，并在那里进行基于行为和电生理数据的听觉感知研究。他曾在工业界工作，构建了用于语音处理的深度学习管道。在数据科学和环境交汇处，他从事应用于音频录音的深度学习项目，用于生物多样性评估。他还定期在Le Wagon（数据科学训练营）创作内容和授课，并在他的博客([hadrienj.github.io](http://hadrienj.github.io))上撰写文章。

**相关：**

+   [数据科学的基础数学：基底与基底变换](/2021/05/essential-math-data-science-basis-change-basis.html)

+   [数据科学的基础数学：矩阵的线性变换](/2021/04/essential-math-data-science-linear-transformation-matrices.html)

+   [数据科学的基础数学：信息理论](/2021/01/essential-math-data-science-information-theory.html)

### 更多相关主题

+   [如何使用NumPy求解非线性方程组](https://www.kdnuggets.com/how-to-use-numpy-to-solve-systems-of-nonlinear-equations)

+   [数据科学的基础数学：奇异值分解的视觉介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [如何克服对数学的恐惧，并为数据科学学习数学](https://www.kdnuggets.com/2021/03/overcome-fear-learn-math-data-science.html)

+   [数据科学的基础数学：特征向量及其在PCA中的应用](https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html)

+   [你在数据科学中需要多少数学？](https://www.kdnuggets.com/2020/06/math-data-science.html)

+   [掌握数据科学数学的5门免费课程](https://www.kdnuggets.com/5-free-courses-to-master-math-for-data-science)
