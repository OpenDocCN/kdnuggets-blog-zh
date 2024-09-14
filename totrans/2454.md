# 3个理由说明你应该使用线性回归模型而不是神经网络

> 原文：[https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

![3 Reasons Why You Should Use Linear Regression Models Instead of Neural Networks](../Images/57e381d5ad862d71d48bd56edd375075.png)

首先，我并不是说线性回归比深度学习更好。

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

其次，如果你明确对深度学习相关的应用感兴趣，比如计算机视觉、图像识别或语音识别，那么这篇文章对你可能不太相关。

但对其他人来说，我想谈谈为什么我认为你更应该学习回归分析而不是深度学习。为什么？因为时间是有限的资源，你如何分配时间将决定你在学习旅程中的进展。

因此，我将提出我的观点，为什么我认为你应该在学习深度学习之前学习回归分析。

# 首先，什么是回归分析？

简而言之，**回归分析** 通常与 **线性回归** 互换使用。

更一般地说，回归分析指的是一组统计方法，用于估计因变量和自变量之间的关系。

然而，一个大的误解是回归分析仅指线性回归，**这** **并非如此。** 回归分析中有许多统计技术，既强大又有用。这让我想到了我的第一个观点：

**观点 #1\. 回归分析更具多样性，适用范围广泛。**

线性回归和神经网络都是你可以用来进行预测的模型。但是，回归分析不仅仅限于进行预测，它还允许你做许多其他事情，包括但不限于：

+   **回归分析让你理解变量之间关系的强度**。通过像R平方/调整R平方这样的统计测量，回归分析可以告诉你模型解释了数据总变异性的多少。

+   **回归分析告诉你模型中哪些预测变量在统计上显著，哪些不是**。简单来说，如果你给回归模型 50 个特征，你可以找出哪些特征是目标变量的有效预测因子，哪些不是。

+   **回归分析可以为每个估计的回归系数提供置信区间**。你不仅可以为每个特征估计一个系数，还可以得到一个系数范围，并有一个置信水平（例如，99% 置信度）来表示该系数位于其中。

+   以及更多……

我的观点是，回归分析中有很多统计技术，可以让你回答比“我们能否根据 X(s) 预测 Y？”更多的问题。

**第 #2 点：回归分析不像黑箱那样复杂，更容易沟通。**

我在选择模型时总是考虑的两个重要因素是它有多**简单**和多**可解释**。

为什么？

一个更简单的模型意味着更容易沟通模型本身的工作原理以及如何解读模型结果。

例如，大多数商业用户可能会比反向传播更快地理解最小二乘和（即最佳拟合线）。这很重要，因为企业关心的是模型中潜在逻辑的工作原理——在商业中，没有什么比不确定性更糟糕的了——而黑箱正是这个问题的绝佳代名词。

最终，理解模型中数字是如何得出的以及如何解读这些数字是很重要的。

**第 #3 点：学习回归分析将使你对统计推断有更好的理解。**

不管你信不信，学习回归分析让我成为了更好的编码员（Python *和* R）、更好的统计学家，并且让我对构建模型有了更好的理解。

为了让你更兴奋一点，回归分析帮助我学到了以下内容（不仅限于此）：

+   构建简单和多重回归模型。

+   进行残差分析并应用 Box-Cox 等变换。

+   计算回归系数和残差的置信区间。

+   通过假设检验来确定模型和回归系数的统计显著性。

+   使用 R 平方、MSPE、MAE、MAPE、PM 等指标来评估模型，列表还在继续……

+   使用方差膨胀因子 (VIF) 识别多重共线性。

+   使用部分 F 检验比较不同的回归模型。

这只是我学到的内容的一小部分，而且我才刚刚开始。所以如果你觉得这听起来很有趣，我建议你去了解一下，至少看看你能学到什么。

## 如何学习回归分析？

最近，我发现学习新话题的最佳方法是找到大学或学院的课程讲座或课程笔记。网络上免费提供的资源实在是太多了。

特别是，我将推荐两个你可以用来入门的优秀资源：

**[宾夕法尼亚州立大学 | STAT 501 回归方法](https://online.stat.psu.edu/stat501/)**

**[特伦斯·辛](https://www.linkedin.com/in/terenceshin/)** 是一位对数据充满热情的专家，拥有超过 3 年的 SQL 经验和 2 年的 Python 经验，同时也是 Towards Data Science 和 KDnuggets 的博主。

[原文](https://towardsdatascience.com/3-reasons-why-you-should-use-linear-regression-models-instead-of-neural-networks-16820319d644)。转载已获许可。

### 更多相关主题

+   [3 个数据科学家应该使用 LightGBM 的理由](https://www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html)

+   [在数据科学项目中停止硬编码 - 改用配置文件](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

+   [忘记 PIP、Conda 和 requirements.txt！改用 Poetry 吧，之后你会感谢我的…](https://www.kdnuggets.com/2023/07/forget-pip-conda-requirementstxt-poetry-instead-thank-later.html)

+   [避免从事数据科学职业的 5 个理由](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)

+   [获得认证的 5 个理由](https://www.kdnuggets.com/2023/05/sas-5-reasons-get-certified.html)

+   [4 个你不应该使用机器学习的理由](https://www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html)
