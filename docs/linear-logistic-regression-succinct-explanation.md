# 线性回归与逻辑回归：简明解释

> 原文：[https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

它们都以字母‘L’开头并以回归结尾，因此人们可能会混淆它们是可以理解的。线性回归和逻辑回归是两种常用的机器学习算法，都源自于监督学习。

总结：

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

**监督学习**是指算法在标记的数据集上进行学习，并分析训练数据。这些标记的数据集具有输入和预期的输出。

**无监督学习**是在未标记的数据上进行学习，推断更多关于隐藏结构的信息，以产生准确和可靠的输出。

线性回归与逻辑回归之间的关系在于它们使用标记的数据集进行预测。然而，它们之间的主要区别在于它们的使用方式。线性回归用于解决回归问题，而逻辑回归用于解决分类问题。

**分类**是关于通过识别对象所属的类别来预测标签，基于不同的参数。

**回归**是关于通过寻找因变量和自变量之间的相关性来预测连续的输出。

![线性回归与逻辑回归：简明解释](../Images/dfe7d0f607596f0c73af5c8853f1daf8.png)

来源: [Javatpoint](https://www.javatpoint.com/regression-vs-classification-in-machine-learning)

# 线性回归

线性回归被认为是从监督学习中派生出的最简单的机器学习算法之一，主要用于解决回归问题。

**线性回归**的用途是对连续的因变量进行预测，并利用自变量的辅助和知识。线性回归的总体目标是找到最佳拟合线，能够准确预测连续因变量的输出。连续值的例子包括房价、年龄和薪水。

简单线性回归是一种回归模型，用直线估计单个自变量和一个因变量之间的关系。如果有两个以上的自变量，我们称之为多重线性回归。

使用最佳拟合线的策略有助于我们理解因变量和自变量之间的关系；这种关系应为线性关系。

## 线性回归的公式

如果你还记得高中数学，你会记得公式：***y = mx + b***，它表示直线的斜率截距。‘y’和‘x’表示变量，‘m’描述直线的斜率，‘b’描述 y 轴截距，即直线穿过 y 轴的位置。

对于线性回归，‘y’表示因变量，‘x’表示自变量，????0表示 y 轴截距，????1表示斜率，描述了自变量与因变量之间的关系。

![线性与逻辑回归的简明解释](../Images/e59fc95300b68f0d53ebd08222aade1d.png)

来源：[TowardsDataScience](https://towardsdatascience.com/simple-linear-regression-in-python-numpy-only-130a988c0212)

# 逻辑回归

逻辑回归也是一种非常流行的机器学习算法，属于监督学习的分支。逻辑回归可用于回归和分类任务，但主要用于分类。如果你想了解更多关于用于分类任务的逻辑回归，请点击此链接。

逻辑回归的一个例子是预测今天是否会下雨，使用 0 或 1，是或否，或真和假。

**逻辑回归**的用途是通过自变量的辅助和知识来预测分类因变量。逻辑回归的总体目标是对输出进行分类，输出只能在 0 和 1 之间。

在逻辑回归中，加权输入的总和会通过一个称为 Sigmoid 函数的激活函数，该函数将值映射到 0 和 1 之间。

![线性与逻辑回归的简明解释](../Images/b258e6d701fe7960fa34dde9514b8b22.png)

来源：[维基百科](https://en.wikipedia.org/wiki/Sigmoid_function)

## Sigmoid 函数的公式

![线性与逻辑回归的简明解释](../Images/80ed0a68d4665eff4cd32d814bdde821.png)

逻辑回归基于**最大似然估计**，这是一种在给定一些观测数据的情况下估计假设概率分布参数的方法。

# 成本函数

**成本函数**是一个用于计算误差的数学公式，它是预测值与实际值之间的差异。它简单地衡量了模型在估计 x 和 y 之间关系的能力上的错误程度。

要了解更多关于成本函数的信息，请点击此链接。

## 线性回归

线性回归的成本函数是均方根误差，也称为均方误差（MSE）。

MSE 衡量观察值的实际值和预测值之间的平均平方差。成本将以一个单一的数字输出，这与我们当前的权重集相关。我们使用成本函数的原因是为了提高模型的准确性；最小化 MSE 就能实现这一点。

MSE 的公式：

![线性回归与逻辑回归：简明解释](../Images/dc644e002638624a6007becd4e32f56a.png)

## 逻辑回归

逻辑回归的成本函数不能使用 MSE，因为我们的预测函数是非线性的（由于 sigmoid 变换）。因此，我们使用一种称为交叉熵的成本函数，也称为对数损失。

交叉熵衡量了给定随机变量或事件集的两个概率分布之间的差异。

交叉熵的公式：

![线性回归与逻辑回归：简明解释](../Images/f9276f5b5e3a1f671416c6cb4760d326.png)

# 线性回归与逻辑回归比较

| **线性回归** | **逻辑回归** |
| --- | --- |
| 用于预测连续因变量，使用一组给定的自变量。 | 用于预测分类因变量，使用一组给定的自变量。 |
| 产生的输出必须是连续值，如价格和年龄。 | 产生的输出必须是分类值，如0或1，是或否。 |
| 因变量和自变量之间的关系必须是线性的。 | 因变量和自变量之间的关系不需要是线性的。 |
| 用于解决回归问题。 | 用于解决分类问题。 |
| 我们找到并使用最佳拟合线来帮助我们轻松预测输出。 | 我们使用S曲线（Sigmoid）来帮助我们对预测输出进行分类。 |
| 最小二乘估计方法用于估计准确性。 | 最大似然估计方法用于估计准确性。 |
| 自变量之间可能存在多重共线性。 | 自变量之间不应存在多重共线性。 |

![线性回归与逻辑回归：简明解释](../Images/81e1998bc8155b36a9dcb92cd029857d.png)

来源: [javatpoint](https://www.javatpoint.com/linear-regression-vs-logistic-regression-in-machine-learning)

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由技术写作人。她特别关注提供数据科学职业建议或教程，并且对数据科学的理论知识充满兴趣。她还希望探索人工智能如何有助于人类寿命的延续。她是一个热衷学习者，寻求拓宽技术知识和写作技能，同时帮助指导他人。

### 更多相关话题

+   [KDnuggets 新闻 22:n12, 3月23日: 最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [线性回归与逻辑回归比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [用于分类的逻辑回归](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)

+   [逻辑回归是如何工作的？](https://www.kdnuggets.com/2022/07/logistic-regression-work.html)

+   [分类指标概述：逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)
