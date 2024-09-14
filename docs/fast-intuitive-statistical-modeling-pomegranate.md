# 使用 Pomegranate 进行快速而直观的统计建模

> 原文：[https://www.kdnuggets.com/2020/12/fast-intuitive-statistical-modeling-pomegranate.html](https://www.kdnuggets.com/2020/12/fast-intuitive-statistical-modeling-pomegranate.html)

[评论](#comments)

### 什么是 Pomegranate？

首先，它是一种美味的水果。

![图像](../Images/c763727dc1da12b587b94061e7bd3bd0.png)

图像来源：[Pixabay](https://pixabay.com/photos/pomegranate-fruit-seeds-food-fresh-3259161/)（可商用）

但对于喜爱水果的数据科学家来说，还有双重的惊喜！

这也是一个 [Python 包](https://pomegranate.readthedocs.io/en/latest/index.html)，实现了从单一概率分布到如 [**贝叶斯网络**](https://en.wikipedia.org/wiki/Bayesian_network) 和 [**隐马尔可夫模型**](https://en.wikipedia.org/wiki/Hidden_Markov_model) 等组合模型的快速灵活的概率模型。

这个包的核心思想是 **所有概率模型都可以视为一个概率分布。** 这意味着它们都为样本提供概率估计，并且可以根据样本及其相关权重进行更新/拟合。这个认识的主要后果是，实施的类可以比其他常见包提供的类更灵活地堆叠和链式连接。

### 与 Scipy 或 Numpy 有何不同？

这是个公平的问题。然而，你会发现 Pomegranate 包中的实现类非常直观，尽管它们涵盖了广泛的统计建模方面，但接口却很统一。

+   常见分布

+   马尔可夫链

+   贝叶斯网络

+   隐马尔可夫模型

+   贝叶斯分类器

这就像将多个 Python 库中的有用方法结合在一起，拥有一个统一而直观的 API。

让我们看看这个巧妙的小包的一些酷炫用法。

### 概率分布

让我们用 `NormalDistribution` 类来初始化。

![帖子图像](../Images/703465a91389ed5457606cff8252a516.png)

生成几个样本，

![帖子图像](../Images/ad5bcb1ff9b1d91e8dc2b6e9e90b22d6.png)

现在，我们可以轻松检查一个样本数据点（或它们的数组）属于此分布的概率。

![帖子图像](../Images/db8cb522d3cc5baa34e5988a56350053.png)

### 数据拟合

这就是更有趣的地方。用数据样本进行拟合非常简单和快速。

如上所初始化，我们可以检查 `n1` 对象的参数（均值和标准差）。我们期望它们为 5.0 和 2.0。

![帖子图像](../Images/6a1cfdf759de6d173fe69898f8172835.png)

现在，让我们通过向高斯分布中添加随机噪声来创建一些合成数据。

![图像](../Images/c5993281371810a23c1b2e782ab25ee5.png)

图像由作者生成

我们可以将这组新数据拟合到 `n1` 对象中，然后检查估计的参数。

![帖子图像](../Images/7c658d2d264e526dcf5c8c368257a455.png)

呼！看来 `n1` 已经更新了其估计的均值和标准差参数以匹配输入数据。直方图的峰值接近于图中的 4.0，这也正是估计的均值所显示的。

### 从数据样本直接创建分布

我们展示了如何将数据拟合到分布类。或者，可以直接从数据创建对象，

![Image for post](../Images/bc0cd9e59ce71876e85f096f5ae09646.png)

### 原生支持绘图（直方图）

使用 `plot()` 方法在分布类上绘图很简单，该方法还支持 Matplotlib 直方图方法的所有关键字。我们通过绘制 Beta 分布对象来进行说明。

![Figure](../Images/a229ebbbe710bb318cda2780f75080b7.png)

作者生成的图像

### 离散分布

这就更有趣了。你可以传递一个**字典**，其中键可以是***任何对象***，值是相应的概率，而不是将参数传递给已知的统计分布（例如正态分布或贝塔分布）。

这是一些霍格沃茨角色的插图。注意，**当我们尝试计算‘海格’的概率时，我们得到一个平坦的零**，因为分布对‘海格’对象没有任何有限的概率。

![Image for post](../Images/756bc6090c3e728c4149c6b0dfd076a2.png)

### 将数据拟合到离散分布

通过将数据拟合到离散分布对象，我们可以做更有趣的事情。这是一个虚构的 DNA 核酸序列的例子。通常，这种类型的序列数据以字符串形式存在，我们可以用简单的代码读取数据并计算序列中四种核酸的概率。

![Image for post](../Images/323bfd09209999834b0c5979ba65cdfe.png)

### 一个简单的 DNA 序列匹配应用

我们可以用极其简单（且天真的）几行代码编写一个 DNA 序列匹配应用。假设** 序列中核酸的频率/概率相似的越接近**。有些随意，我们选择计算这种距离度量的均方根距离。

你可以查看 [Jupyter notebook](https://github.com/tirthajyoti/Stats-Maths-with-Python/blob/master/Pomegranate.ipynb) 获取帮助函数和确切的代码，但这里是一个示例输出。

![Image for post](../Images/41e20711244ff6980df8d267a261e3b4.png)

### 高斯混合模型

Pomegranate 使处理来自多个高斯分布的数据变得简单。

让我们创建一些合成数据，

![Figure](../Images/0e2981d673fa702e3dd1e3afab1dc9d7.png)

作者生成的图像

和往常一样，我们可以用一行代码直接从数据创建模型。当我们打印模型的估计参数时，我们观察到它捕捉了真实情况（生成器分布的参数）相当准确。

![Image for post](../Images/6b39f4d7d708a32e5c4fcf59a6d2c21d.png)

一旦模型通过数据样本生成后，我们可以轻松计算概率并绘制图表。代码在 [Notebook](https://github.com/tirthajyoti/Stats-Maths-with-Python/blob/master/Pomegranate.ipynb)中，下面是说明性的图表——左侧显示了一个单一的高斯分布，右侧显示了混合模型。

![图形](../Images/5afcdba8190d030088333622206e0012.png)

由作者生成的图像

### 马尔可夫链

我们可以轻松地使用 Pomegranate 建模一个简单的马尔可夫链，并计算任何给定序列的概率。

对于这个示例，我们将使用典型的雨天-阴天-晴天的例子。以下是转换概率表，

![帖子图片](../Images/914f7bed0b3dce6fe89753a53ea901a1.png)

![图形](../Images/a4609561aa136af2688d48e3eb90d48d.png)

由作者生成的图像

我们还知道，平均而言，有20%的雨天，50%的晴天和30%的阴天。我们在 `MarkovChain` 类中编码了离散分布和转换矩阵，

![帖子图片](../Images/d263fea7b5fa5d2f6e8fa10559c82617.png)

现在，我们可以使用这个对象计算任何给定序列的概率。例如，如果你查看上面的表格，你可以相信**像“*阴天-雨天-阴天*”这样的序列具有较高的可能性，而像“*雨天-晴天-雨天-晴天*”这样的序列不太可能出现**。我们可以通过精确的概率计算来确认这一点（我们取对数来处理小概率数值），

![帖子图片](../Images/6dce0ded2aab26e85c87c684046346e1.png)

### 数据拟合到 GMM 类

我们编写了一个小函数来生成随机的雨天-阴天-晴天序列，并将其输入到 GMM 类中。

![帖子图片](../Images/3a0b35730c28dad0ff586cb9670f763b.png)

首先，我们为14天的观察数据提供——“雨天-晴天-雨天-晴天-雨天-晴天-雨天-雨天-晴天-晴天-晴天-雨天-晴天-阴天”。为我们计算了概率转换表。

![帖子图片](../Images/2af80fb6e4d3310f0099d575073acdb0.png)

如果我们生成一个10年的随机序列，即3650天，那么我们得到以下表格。**由于我们的随机生成器是均匀的，根据马尔可夫链的特性，转换概率将假设约0.333的极限值**。

![帖子图片](../Images/3b4088637259d45c6ead59283ddaffcb.png)

### 隐马尔可夫模型

使用 Pomegranate 中的 HMM 类，你可以做很多酷的事情。这里，我们只是展示了一个小示例，通过 HMM 预测**检测长字符串中子序列的高密度出现**。

假设我们记录了《哈利·波特》小说中四个角色的名字，它们在一章中一个接一个地出现，我们对检测哈利和邓布利多一起出现的部分感兴趣。然而，由于他们可能会交谈并在这些部分提到罗恩或海格的名字，因此子序列并不干净，即不严格包含哈利和邓布利多的名字。

![图](../Images/d5af1845d9a8c8181c501ecde5b111f7.png)

图片拼贴由作者从 Wikimedia Commons 和 Pixabay 图片创建

以下代码初始化了一个均匀概率分布，一个偏斜概率分布，两个具有名称的状态，以及一个包含这些状态的 HMM 模型。

![文章图片](../Images/81f86a6c986642934159c8ea4342ce8c.png)

然后，我们需要添加状态转移概率，并“烘焙”模型以最终确定内部结构。请注意**高自环概率**，即状态倾向于以高概率停留在当前状态。

![文章图片](../Images/ef435a6fa8d69e8298f815a3724ad55d.png)

现在，我们有一个观测序列，我们将其作为参数传递给 HMM 模型的`predict`方法。转移和发射概率将被计算，并且会预测出一系列 1 和 0，我们可以**注意到 0 的岛屿，表示包含“哈利-邓布利多”出现的部分**。

![文章图片](../Images/5b1e4faed165ad47cb74d05a8862243c.png)

### 总结

在本文中，我们介绍了一个快速且直观的统计建模库 Pomegranate，并展示了一些有趣的使用示例。更多教程可以在这里找到。本文中的示例也受到这些教程的启发。

[**Pomegranate 的 GitHub 仓库教程**](https://github.com/jmschrei/pomegranate/tree/master/tutorials)

该库提供了来自各种统计领域的实用类——一般分布、马尔可夫链、高斯混合模型、贝叶斯网络——具有统一的 API，可以快速实例化观察到的数据，并用于参数估计、概率计算和预测建模。

你可以查看作者的[**GitHub**](https://github.com/tirthajyoti?tab=repositories)**仓库**，获取机器学习和数据科学方面的代码、想法和资源。如果你和我一样，对人工智能/机器学习/数据科学充满热情，请随时[在 LinkedIn 上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)或[在 Twitter 上关注我](https://twitter.com/tirthajyotiS)。

[**Tirthajyoti Sarkar - 高级首席工程师 - 半导体、人工智能、机器学习**](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)

通过写作使数据科学/机器学习概念易于理解

[原文](https://towardsdatascience.com/statistical-modeling-with-pomegranate-fast-and-intuitive-4d605d9c33a9)。经许可转载。

**相关内容：**

+   [数据分布概述](/2020/06/overview-data-distributions.html)

+   [概率分布之前](/2020/07/before-probability-distributions.html)

+   [比较机器学习模型：统计显著性与实际显著性](/2019/01/comparing-machine-learning-models-statistical-vs-practical-significance.html)

### 更多相关内容

+   [支持向量机：一种直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [协同过滤的直观解释](https://www.kdnuggets.com/2022/09/intuitive-explanation-collaborative-filtering.html)

+   [机器学习项目的简单快速数据流处理](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)

+   [快速有效的机器学习公平性审计方法](https://www.kdnuggets.com/2023/01/fast-effective-way-audit-ml-fairness.html)

+   [BERT在稀疏性下的最快速度是多少？](https://www.kdnuggets.com/2022/04/fast-bert-go-sparsity.html)

+   [通过快速克里金法（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)
