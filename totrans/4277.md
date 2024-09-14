# 朴素英语解释的集成方法：自助法（Bagging）

> 原文：[https://www.kdnuggets.com/2021/05/ensemble-methods-explained-plain-english-bagging.html](https://www.kdnuggets.com/2021/05/ensemble-methods-explained-plain-english-bagging.html)

[评论](#comments)

**由[Claudia Ng](https://www.linkedin.com/in/claudian37/)，高级数据科学家**

在本文中，我将介绍一种流行的同质模型集成方法——自助法（bagging）。同质集成方法结合了大量相同算法的基础估计器或弱学习器。

同质集成的原理是“群体智慧”——多个多样化模型的集体预测优于单个模型的预测。要实现这一点，有三个要求：

1.  模型必须是独立的；

1.  每个模型的表现略好于随机猜测；

1.  所有单独的模型在其自身的表现上相似。

当这三个要求得到满足时，添加更多模型应该能提高集成模型的表现。

集成方法有助于减少方差并防止对训练数据集的过拟合，从而使模型更好地学习一般化模式，而不是对训练数据集中的噪声过拟合。

### 自助法

### 自助法的工作原理

在自助法中，大量独立的弱模型被组合在一起，以相同的目标学习相同的任务。术语“自助法”来源于`**b**ootstrap + **agg**regat**ing**`，其中每个弱学习器在用替换抽样的随机子样本上进行训练，然后将模型的预测结果进行聚合。

自助法保证了独立性和多样性，因为每个数据子样本是独立抽样的，并且我们得到不同的子集来训练基础估计器。

基础估计器是表现仅比随机猜测稍好的一类弱学习器。一个这样的模型示例是最大深度为三的浅层决策树。这些模型的预测结果通过平均值进行组合。

自助法既可以应用于分类问题，也可以应用于回归问题。对于回归问题，最终预测将是基础估计器预测的平均值（软投票）。对于分类问题，最终预测将是多数投票（硬投票）。

![](../Images/1a9eebf7d3097b0745810830dfd1012f.png)

自助法算法的图示

### 使用 Scikit-Learn 实现自助法算法

你可以使用`[BaggingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingRegressor.html)`或`[BaggingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingClassifier.html)`在 Python 包 Scikit-Learn 中构建你自己的自助法算法。

首先，实例化你的基础估算器，并将其作为`BaggingRegressor`或`BaggingClassifier`中的基础估算器。下面是一个以线性回归作为基础估算器的袋装回归器示例，以及一个以决策树分类器作为基础估算器的袋装分类器示例。默认的估算器数量为10。

```py
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import BaggingRegressor

reg_lr = LinearRegression()
reg_bag = BaggingRegressor(base_estimator=reg_lr)
reg_bag.fit(X_train, y_train)

====================================================================

from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import BaggingClassifier

clf_dt = DecisionTreeClassifier(max_depth=3)
clf_bag = BaggingClassifier(base_estimator=clf_dt)
clf_bag.fit(X_train, y_train)
```

### 随机森林

随机森林®是袋装算法的一个流行示例。它使用平均值将多个在训练数据集子集上训练的单独决策树进行集成。

使用[scikit-learn的随机森林算法](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)在Python中，你可以指定树特定的参数。以下是一些需要调整的重要超参数，以使其针对你的数据集进行优化：

+   `n_estimators`：训练的树木数量以进行聚合。通常100到500棵树就足够了，通常更多的树会改进你的模型（但收益递减），但计算成本也会更高；

+   `max_depth`：树的最大深度。较深的树有助于减少偏差，但会增加方差。随机森林算法中多个树的聚合可以帮助解决这一问题，但仍需小心；

+   `max_features`：每次分裂时考虑的最大特征数。一个好的起点通常是特征数量的平方根。

随机森林算法中的另一个重要概念是袋外（OOB）评分。在执行自助法时，会有实例未被纳入用于训练估算器的子样本。这些样本外的实例可以用来评估模型，得到一个袋外（OOB）评分，本质上类似于随机森林模型的伪验证集。要获得OOB评分，在初始化随机森林对象时将`oob_score=True`。

```py
rf = RandomForestClassifier(n_estimators=100, oob_score=True)
rf.fit(X_train, y_train)
print(rf.oob_score_)
```

请注意，如果特征中存在空值，scikit-learn的随机森林算法会返回错误，因此请记得在调用fit之前用pandas的`fillna`填充空值，否则会抛出错误。

### 袋装的优缺点

+   **减少方差**：由于采样是通过自助法真正随机进行的，袋装通常有助于减少方差并对抗过拟合。

+   **易于并行化**：估算器是独立的，因此可以在袋装过程中并行构建模型。

+   **更高的稳定性和鲁棒性**：大量聚合的估算器有助于提供更高的稳定性和鲁棒性。

+   **难以解释**：袋装算法的最终预测基于基础估算器的平均预测。虽然这提高了准确性，但模型变得更难以解释。

### 总结

Bagging 基于集体学习的思想，其中许多独立的弱学习器在引导的子样本数据上进行训练，然后通过平均进行聚合。它可以应用于分类和回归问题。

随机森林算法是一个流行的 bagging 算法示例。在为你的数据集调整随机森林算法的超参数时，需要关注三个重要领域：i) 树的数量 (`n_estimators`)，ii) 修剪树（从 `max_depth` 开始，但也探索节点中所需的样本和/或分裂），iii) 每次分裂时考虑的最大特征数量 (`max_features`)。

Bagging 是一个非常强大的模型集成方法示例。希望这能激励你在下次处理预测建模问题时尝试使用 bagging 算法！

**简介：[Claudia Ng](https://www.linkedin.com/in/claudian37/)** 是高级数据科学家。

[原文](https://pub.towardsai.net/ensemble-methods-explained-in-plain-english-bagging-47bef8ac7690)。经授权转载。

**相关：**

+   [XGBoost：它是什么，何时使用它](/2020/12/xgboost-what-when.html)

+   [全面的集成学习指南——你需要知道的所有内容](/2021/05/comprehensive-guide-ensemble-learning.html)

+   [微软探索集成学习的三个关键谜团](/2021/02/microsoft-explores-three-key-mysteries-ensemble-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关内容

+   [10 个基本统计概念（通俗易懂）](https://www.kdnuggets.com/10-basic-statistical-concepts-in-plain-english)

+   [何时集成技术是一个好选择？](https://www.kdnuggets.com/2022/07/would-ensemble-techniques-good-choice.html)

+   [带示例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [集成学习技术：Python 中随机森林的逐步讲解](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)
