# 3分钟了解偏差-方差权衡

> 原文：[https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

![图](../Images/7df63cf0b2d25ce28caa4881db095175.png)

平衡它们就像魔法一样

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加快你的网络安全职业发展。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 领域支持你的组织

* * *

**偏差**和**方差**是训练机器学习模型时需要调整的**核心参数**。

当我们讨论预测模型时，**预测错误**可以分解为两个主要子组件：**偏差**导致的错误和**方差**导致的错误。

**偏差-方差权衡**是**偏差**引入的**错误**与方差产生的**错误**之间的**张力**。为了理解如何充分利用这一权衡，避免模型欠拟合或过拟合，我们首先学习偏差和方差。

# 由于偏差导致的错误

由于***偏差***导致的错误是模型的预测值与***真实值***之间的***距离***。在这种错误中，模型对训练数据关注较少，***过于简单化***模型，不学习模式。模型通过**未考虑所有特征**而**学习错误的关系**。

# 由于方差导致的错误

对于给定数据点或值的模型预测的变异性是***告诉我们数据的分布情况***。在这种错误中，模型对训练数据**过度关注**，甚至到记住它而不是从中学习的程度。具有高方差错误的模型无法对未见过的数据进行有效的泛化。

> 如果偏差与方差是阅读行为，它可以像略读文本与逐字记忆文本之间的区别。

我们希望我们的机器模型从其接触的数据中学习，而不是*“对它有一个大致的了解”*或“*逐字记忆*”。

# 偏差—方差权衡

偏差-方差权衡是关于平衡和***找到一个最佳点***，在偏差导致的错误和*方差*导致的错误之间。

> 这是一个欠拟合与过拟合的困境

![图](../Images/00485a8686aab56bede94ceac42d4b20.png)

图由 Jake VanderPlas 绘制

如果模型用灰色线表示，我们可以看到，高偏差模型是一个过于简单化数据的模型，而高方差模型是一个过于复杂以至于过拟合数据的模型。

## 简而言之：

+   **偏差**是模型为了使目标函数更容易逼近而做出的简化假设。

+   **方差**是指在不同的训练数据下，目标函数估计值的变化量。

+   **偏差-方差权衡**是我们的机器模型在偏差和方差引入的误差之间表现最佳的甜蜜点。

*在这篇文章中，我们讨论了* ***偏差和方差的概念含义***。接下来，我们将探索该概念在****代码中的应用。***

## 未来阅读

+   [*理解偏差-方差权衡*](http://scott.fortmann-roe.com/docs/BiasVariance.html) 由 Scott Fortmann-Roe 编著

+   [*统计学习的元素*](https://web.stanford.edu/~hastie/ElemStatLearn/) 由 Trevor Hastie、Robert Tibshirani 和 Jerome Friedman 编著

**[布伦达·哈利](https://twitter.com/brendahali)** (**[LinkedIn](https://www.linkedin.com/in/brenda-hali/)**) 是一位驻华盛顿特区的市场数据专家。她热衷于推动女性在科技和数据领域的参与。

[原文](https://towardsdatascience.com/understanding-bias-variance-trade-off-in-3-minutes-c516cb013513)。转载已获许可。

### 更多相关话题

+   [5分钟构建机器学习网络应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [用 Python 在5分钟内构建网页爬虫](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)

+   [5种机器学习模型的5分钟解释](https://www.kdnuggets.com/5-machine-learning-models-explained-in-5-minutes)

+   [FastAPI 教程：用 Python 迅速构建 API](https://www.kdnuggets.com/fastapi-tutorial-build-apis-with-python-in-minutes)

+   [KDnuggets 新闻 2022年3月9日：5分钟构建机器学习网络应用](https://www.kdnuggets.com/2022/n10.html)

+   [介绍 OpenChat：构建自定义聊天机器人平台的免费简单平台](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)
