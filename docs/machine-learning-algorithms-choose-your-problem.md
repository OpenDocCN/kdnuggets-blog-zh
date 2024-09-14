# 机器学习算法：如何为您的问题选择合适的算法

> 原文：[https://www.kdnuggets.com/2017/11/machine-learning-algorithms-choose-your-problem.html](https://www.kdnuggets.com/2017/11/machine-learning-algorithms-choose-your-problem.html)

**作者：丹尼尔·科尔布特，[Statsbot](https://statsbot.co/)。**

当我刚开始从事数据科学时，我经常面临选择最适合我特定问题的算法的难题。如果您和我一样，当您打开关于机器学习算法的文章时，您会看到大量详细的描述。悖论在于，这些描述并没有简化选择。

在这篇针对 [Statsbot](https://statsbot.co/?utm_source=blog&utm_medium=article&utm_campaign=ml_algorithms)的文章中，我将尝试解释基本概念，并提供在不同任务中使用各种[机器学习算法](https://www.kdnuggets.com/2016/08/10-algorithms-machine-learning-engineers.html)的直观理解。在文章的末尾，您会发现对描述算法的主要特征的结构化概述。

![](../Images/e2c9513a7f45c3a22440f1fc232f0283.png)

首先，您应该区分4种机器学习任务：

+   监督学习

+   无监督学习

+   半监督学习

+   强化学习

****监督学习****

监督学习是从标记的训练数据中推断函数的任务。通过拟合标记的训练集，我们希望找到最优模型参数，以预测其他对象（测试集）上的未知标签。如果标签是一个实数，我们称该任务为*回归*。如果标签来自有限数量的值，并且这些值是无序的，那么它是*分类*。

![](../Images/4d45a1cf4d2f71fa224453236ae5cb11.png)

[插图来源](https://scorecardstreet.wordpress.com/2015/12/09/is-machine-learning-the-new-epm-black/)****无监督学习****

在无监督学习中，我们对对象的信息较少，特别是训练集是未标记的。我们的目标是什么呢？我们可以观察到对象组之间的一些相似性，并将它们纳入适当的簇中。一些对象可能与所有簇差异很大，我们将这些对象视为异常。

![](../Images/d836f2ac4018e070a4ab9505ffcfbce2.png)

[插图来源](http://www.constonline.com/machine-learning)****半监督学习****

半监督学习任务包括我们之前描述的两种问题：它们使用标记和未标记的数据。这对于那些无法标记其数据的人来说是一个很好的机会。该方法使我们能够显著提高准确性，因为我们可以在少量标记数据的训练集中使用未标记数据。

![](../Images/edd4b3e75b3eb44b8bbb000d0859b3e2.png)

[插图来源](https://makarandtapaswi.wordpress.com/2013/04/30/labeled-data-unlabeled-data-and-constraints/)****强化学习****

强化学习不同于我们之前讨论的任务，因为这里没有标记或未标记的数据集。RL 是一个关注软件代理如何在某种环境中采取行动以最大化累积奖励概念的机器学习领域。

![](../Images/abe95d648fc95a7dbdee8ad2af554539.png)

[插图来源](https://www.analyticsvidhya.com/blog/2016/12/getting-ready-for-ai-based-gaming-agents-overview-of-open-source-reinforcement-learning-platforms/)想象一下，你是一个在某个陌生地方的机器人，你可以执行活动并从环境中获得奖励。每次行动后，你的行为变得越来越复杂和聪明，因此你正在训练自己在每一步上以最有效的方式行为。在生物学中，这被称为对自然环境的适应。

### **常用的机器学习算法**

现在我们对机器学习任务的类型有了一些直观了解，让我们探讨一些最受欢迎的算法及其在现实生活中的应用。

****线性回归和线性分类器****

这些可能是机器学习中最简单的算法。你有对象的特征 x1,...xn（矩阵 A）和标签（向量 b）。你的目标是根据某些损失函数（例如，[均方误差](https://en.wikipedia.org/wiki/Mean_squared_error)或[平均绝对误差](https://en.wikipedia.org/wiki/Mean_absolute_error)）找到这些特征的最优权重 w1,…wn 和偏差。在 MSE 的情况下，有一个来自最小二乘法的数学方程：

![](../Images/fb1c31b0f0462977d283e9a05300f1ef.png)

在实践中，利用梯度下降法优化它更为容易，这种方法计算效率更高。尽管这个算法很简单，但当你有成千上万个特征时（例如，词袋模型或 [文本分析](https://blog.statsbot.co/text-classifier-algorithms-in-machine-learning-acc115293278) 中的 n-gram），它表现得相当好。更复杂的算法在处理许多特征和不是很大的数据集时容易过拟合，而线性回归则提供了不错的质量。

![](../Images/1a2216d08f22bde56881b17581a07d31.png)

[插图来源](http://newsdog.today/a/article/582ebdf11290711e26997ce2/)为了防止过拟合，我们通常使用正则化技术，如 lasso 和 ridge。其思想是将权重的绝对值之和和权重的平方和分别添加到我们的损失函数中。请阅读文章末尾的这些算法的精彩教程。

****逻辑回归****

不要将这些分类算法与使用“回归”作为标题的回归方法混淆。逻辑回归执行二分类，因此标签输出是二进制的。我们定义 P(y=1|x) 为在给定输入特征向量 *x* 的条件下，输出 *y* 为 1 的条件概率。系数 *w* 是模型希望学习的权重。

![](../Images/e855e13ba183987726dfda91dd239bd4.png)

由于该算法计算每个类别的归属概率，因此你应该考虑概率与 0 或 1 的差异，并将其在所有对象上平均，就像我们在进行线性回归时所做的那样。这样的损失函数是交叉熵的平均值：

![](../Images/81855fd0b652a38e72747275a175be89.png)

不要慌，我会让这变得简单。让*y*表示正确答案：0 或 1，*y_pred* — 预测答案。如果*y*等于 0，那么加法和的第一个加数为 0，第二个加数是根据对数的性质，预测的*y_pred*与 0 之间的距离越小。类似地，当*y*等于 1 时也是如此。

逻辑回归的优点是什么？它对特征进行线性组合，并应用非线性函数（sigmoid），所以它是神经网络的一个非常小的实例！

****决策树****

另一种流行且易于理解的算法是决策树。它们的图形帮助你看到你的思路，并且它们的引擎需要系统化、记录下来的思考过程。

这个算法的思想非常简单。在每个节点，我们在所有特征和所有可能的分裂点中选择最佳的分裂。每个分裂的选择方式是为了最大化某个函数。在分类树中，我们使用交叉熵和基尼指数。在回归树中，我们最小化在该区域内落入的点的目标值与我们分配给它的预测变量之间的平方误差之和。

![](../Images/7ec924d7758bde871ca16dbfff0f7d9a.png)

[插图来源](http://cway-quantlab.blogspot.ru/2017/06/optimize-trading-system-with-gradient_21.html)我们对每个节点递归地执行此过程，直到遇到停止标准为止。停止标准可以从节点中的最小叶子数到树的高度。单棵树很少被单独使用，但与许多其他树组合在一起时，它们构建了非常高效的算法，如随机森林或[梯度树提升](https://en.wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting)。

****K-均值****

有时你不知道任何标签，你的目标是根据对象的特征分配标签。这称为*聚类任务*。

假设你想将所有数据对象分成 k 个簇。你需要从数据中选择 k 个随机点，并将它们命名为簇的中心。其他对象的簇由最近的簇中心定义。然后，簇的中心会被转换，直到收敛。

![](../Images/ff45aa98605029a4b985372839d13b64.png)

这是最清晰的聚类技术，但仍然存在一些缺点。首先，你应该知道簇的数量，这个数量是我们无法预知的。其次，结果依赖于开始时随机选择的点，算法也不保证我们能达到函数的全局最小值。

存在一系列具有不同优缺点的聚类方法，你可以在推荐阅读中了解它们。

****主成分分析 (PCA)****

你是否曾在最后一夜或最后几小时为一次困难的考试做准备？你没有机会记住所有信息，但你想在有限的时间内最大化记住的信息，例如，首先学习那些出现在许多考卷中的定理等等。

主成分分析基于相同的思想。该算法提供了降维功能。有时你有一个广泛的特征范围，这些特征可能彼此高度相关，而模型可能会在大量数据上过拟合。然后，你可以应用PCA。

你应该计算某些向量上的投影，以最大化数据的方差，并尽可能少地丢失信息。令人惊讶的是，这些向量是数据集特征的相关矩阵的特征向量。

![](../Images/c3a9ddc36b0a2508f20f621fa0eb8729.png)

[插图来源](https://www.analyticsvidhya.com/blog/2016/03/practical-guide-principal-component-analysis-python/)算法现在已经很清楚：

1.  我们计算特征列的相关矩阵，并找到该矩阵的特征向量。

1.  我们将这些多维向量进行投影计算，来计算所有特征在这些向量上的投影。

新特征是投影的坐标，其数量取决于计算投影的特征向量的数量。

****神经网络****

当我们谈论逻辑回归时，我已经提到过神经网络。存在许多在非常具体任务中有价值的不同架构。更常见的是，它是一系列层或组件，它们之间有线性连接，并且遵循非线性特性。

如果你在处理图像，卷积深度神经网络会显示出很好的效果。非线性由卷积和池化层表示，这些层能够捕捉图像的特征。

![](../Images/06494bd8dec7517c259587a5f14568b0.png)

[插图来源](http://www.smash.com/ostagram-uses-neural-networks-merge-two-pictures-psychedelic-weirdness/)对于处理文本和序列，你最好选择*递归神经网络*。RNN包含LSTM或GRU模块，能够处理我们事先知道维度的数据。也许，RNN最著名的应用之一是*[机器翻译](https://blog.statsbot.co/machine-learning-translation-96f0ed8f19e4)*。

### **结论**

我希望能向你解释最常用的机器学习算法的常见认知，并提供如何为你的特定问题选择一种算法的直观理解。为了让事情对你更简单，我准备了它们主要特征的结构化概述。

**线性回归和线性分类器。** 尽管看似简单，但在特征非常多的情况下，它们非常有用，而更好的算法往往会过拟合。

**逻辑回归**是最简单的非线性分类器，通过参数的线性组合和非线性函数（sigmoid）进行二分类。

**决策树**常类似于人们的决策过程，易于解释。但它们通常用于随机森林或梯度提升等组合中。

**K均值**更为原始，但这是一个非常易于理解的算法，可以作为各种问题的基准。

**主成分分析（PCA）**是减少特征空间维度并尽量减少信息损失的绝佳选择。

**神经网络**是新一代机器学习算法，可以应用于许多任务，但其训练需要巨大的计算复杂性。

### **推荐来源**

+   [聚类方法概述](http://scikit-learn.org/stable/modules/clustering.html#overview-of-clustering-methods)

+   [Python中岭回归和套索回归的完整教程](https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-ridge-lasso-regression-python/)

+   [关于AI的初学者YouTube频道，提供很棒的教程和示例](https://www.youtube.com/channel/UCWN3xxRkmTPmbKwht9FuE5A)

**个人简介: [丹尼尔·科尔布特](https://medium.com/@daniilkorbut)** 是 [Statsbot](https://statsbot.co/) 的初级数据科学家。

[原文](https://blog.statsbot.co/machine-learning-algorithms-183cc73197c?utm_source=kdnuggets&utm_medium=post&utm_campaign=ml_algrithms)。经许可转载。

**相关内容:**

+   [初学者的十大机器学习算法](/2017/10/top-10-machine-learning-algorithms-beginners.html)

+   [理解机器学习算法](/2017/10/understanding-machine-learning-algorithms.html)

+   [机器学习算法：简明技术概述 - 第1部分](/2017/08/machine-learning-algorithms-concise-technical-overview-part-1.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个90亿美元的AI失败，分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每位数据科学家都应了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
