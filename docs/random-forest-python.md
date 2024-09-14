# 解释随机森林®（带Python实现）

> 原文：[https://www.kdnuggets.com/2019/03/random-forest-python.html](https://www.kdnuggets.com/2019/03/random-forest-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[学习机器](https://www.thelearningmachine.ai )**

### **随机森林（+Python实现）**

#### **1\. 介绍**

本文由**[学习机器](https://www.thelearningmachine.ai/)**撰写，这是一个新的开源项目，旨在创建一个互动路线图，包含A到Z的概念、方法、算法及其Python或R代码实现的解释，适合各种背景的人群。

查看我们的点击即用**[机器学习思维导图](https://www.thelearningmachine.ai/ml)**，包含算法解释和Python实现。

![机器学习](../Images/d92814dfa1d1e62c7770c4fa70df6c3f.png)

#### **2\. 随机森林**

随机森林是一种灵活、易于使用的机器学习算法，即使在没有超参数调整的情况下，也能大多数时候产生很好的结果。它可以用于分类和回归任务。在这篇文章中，你将学习随机森林算法如何处理分类和回归问题。

*要理解随机森林算法，你首先需要熟悉决策树。请阅读有关决策树的文章* *[这里](https://www.thelearningmachine.ai/tree-id3)**。*

决策树常见的问题之一，特别是那些有许多列的决策树，是它们容易***过拟合***。有时看起来树只是*记住*了数据。这里是过拟合的决策树的典型例子，包括**分类**和**连续**数据：

**I. 分类：**

*如果客户是男性，年龄在15到25岁之间，来自美国，喜欢冰淇淋，有一个德国朋友，讨厌鸟类，并且在2012年8月25日吃过煎饼，那么 - 他很可能会下载《精灵宝可梦Go》。*

**II. 连续型：**

![随机森林](../Images/845b1da88bfb7f1d15edc4a0c5c6c17c.png)

随机森林避免了这个问题：它是由多个决策树组成的集合，而不仅仅是一个。而且，随机森林中的决策树数量越多，泛化效果越好。

更准确地说，随机森林的工作原理如下：

1.  从总共有m个特征（列）的数据集中随机选择k个特征（列）（其中k<<m）。然后，从这些k个特征中构建一个决策树。

1.  重复n次，以便你从不同的随机k特征组合（或称为***bootstrap*** ***sample***）中构建***n***个决策树。

1.  采用每一个构建好的n个决策树，并传递一个随机变量来预测结果。存储预测的结果（目标），从而你将得到来自***n***个决策树的总共***n***个结果。

1.  计算每个预测目标的投票，并取众数（最频繁的目标变量）。换句话说，将高投票的预测目标视为随机森林算法的最终预测。

**在回归问题中，对于一个新的记录，森林中的每棵树都预测Y（输出）的一个值。最终值可以通过取森林中所有树预测值的平均值来计算。或者，在分类问题中，森林中的每棵树都预测新记录所属的类别。最后，新记录被分配到获得多数票的类别。**

**示例：**

詹姆斯想决定在巴黎待一周期间应该去哪些地方。他去找一个在那儿住了一年的朋友，询问他过去去过哪些地方以及是否喜欢。基于他的经验，他会给詹姆斯一些建议。

这是一种典型的决策树算法方法。詹姆斯的朋友根据他一年的个人经验决定了詹姆斯应该去哪些地方。

后来，詹姆斯开始询问越来越多的朋友，以获取建议，他们推荐了自己去过的地方。然后詹姆斯选择了最常被推荐的地方，这就是典型的随机森林算法方法。

***因此，随机森林是一种通过随机选择总共m个特征中的k个特征来构建n棵决策树的算法，并对预测结果取众数（如果是回归问题则取平均值）。***

#### **3. 优缺点**

**优点：**

1.  **可用于分类和回归问题：** 随机森林在处理分类和数值特征时表现良好。

1.  **减少过拟合：** 通过对多个树进行平均，可以显著降低过拟合的风险。

1.  **仅当超过一半的基本分类器出错时才会做出错误预测：** 随机森林非常稳定——即使数据集中引入了新的数据点，整体算法也不会受到太大影响，因为新数据可能会影响一棵树，但很难影响所有的树。

**缺点：**

1.  观察到随机森林在某些噪声分类/回归任务的数据集上会过拟合。

1.  比决策树算法更复杂且计算开销更大。

1.  由于其复杂性，与其他可比算法相比，它们需要更多的训练时间。

### **4. 重要超参数**

随机森林中的超参数要么用于提高模型的预测能力，要么使模型运行更快。下面描述了sklearn内置随机森林函数的超参数：

1.  **提高预测能力**

+   **n_estimators：** 算法在进行最大投票或预测平均值之前构建的树的数量。一般来说，树的数量越多，性能越高，预测越稳定，但计算速度也会变慢。

+   **max_features:** 随机森林允许在单个树中尝试的最大特征数量。Sklearn 提供了几个选项，详细描述见其 [文档](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)。

+   **min_sample_leaf:** 确定拆分内部节点所需的最小叶子数。

1.  **提高模型速度**

+   **n_jobs:** 告诉引擎可以使用多少处理器。如果其值为 1，则只能使用一个处理器。值为“-1”表示没有限制。

+   **random_state:** 使模型的输出可重复。当模型具有确定的 random_state 值并且使用相同的超参数和训练数据时，模型将始终产生相同的结果。

+   **oob_score:** （也称为 oob 采样） - 一种随机森林交叉验证方法。在这种采样中，大约三分之一的数据不用于训练模型，而是用于评估其性能。这些样本称为袋外样本。它与留一法交叉验证方法非常相似，但几乎没有额外的计算负担。

### **5\. Python 实现**

查看/下载位于 git 仓库中的随机森林模板 [这里](https://github.com/the-learning-machine/ML/blob/master/Classification/random_forests.ipynb)。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [用于分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [直观地解释随机森林](https://www.kdnuggets.com/2019/01/random-forests-explained-intuitively.html)

+   [数据科学家面试揭秘](https://www.kdnuggets.com/2018/08/data-scientist-interviews-demystified.html)

+   [为机器学习新手介绍的前 10 个算法](https://www.kdnuggets.com/2018/02/tour-top-10-algorithms-machine-learning-newbies.html)

RANDOM FORESTS 和 RANDOMFORESTS 是 Minitab, LLC 注册的商标。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [每个数据科学家都应该了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让 Python 成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [随机森林与决策树：主要区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [随机森林算法需要归一化吗？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [调整随机森林超参数](https://www.kdnuggets.com/2022/08/tuning-random-forest-hyperparameters.html)

+   [停止学习数据科学以找到目标，并找到目标去……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
