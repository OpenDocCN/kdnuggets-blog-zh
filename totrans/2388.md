# L1 和 L2 正则化的区别

> 原文：[https://www.kdnuggets.com/2022/08/difference-l1-l2-regularization.html](https://www.kdnuggets.com/2022/08/difference-l1-l2-regularization.html)

## 主要要点

+   正则化回归可以用于特征选择和降维

+   这里讨论了两种类型的正则化回归模型：岭回归（L2 正则化）和套索回归（L1 正则化）

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

# 基本线性回归

假设我们有一个数据集，其中包含 ![Equation](../Images/5781aece7d1bee67c4452ec40f937488.png) 特征和 ![Equation](../Images/9a60fefb21d8c288f3b817f86bef589d.png) 观测，如下所示：

![L1 和 L2 正则化的区别](../Images/8664a8312e813afcfc45f3f84711e690.png)

一个基本的回归模型可以表示为：

![L1 和 L2 正则化的区别](../Images/fec366f491948e2acd4b7323434ccbb3.png)

其中 ![Equation](../Images/a69ef9f4c6b668ea38e06114527c5291.png) 是 ![Equation](../Images/0cba9b3c1e8c66051135c64cb5a82947.png) 特征矩阵，而 ![Equation](../Images/ac5243e674d1418e7fe029c9dc9d1ae9.png) 是权重系数或回归系数。在基本线性回归中，通过最小化下面给出的损失函数来获得回归系数：

![L1 和 L2 正则化的区别](../Images/261f513d49b5cf8d3ecb54c68247e734.png)

# 岭回归：L2 正则化

在 L2 正则化中，通过最小化 L2 损失函数来获得回归系数，如下所示：

![L1 和 L2 正则化的区别](../Images/703e1ac83ef68b73193db7eb6c8cce94.png)

其中

![L1 和 L2 正则化的区别](../Images/28e67f19bb0fbbdb6218db28eb163ad6.png)

这里，α ∈[0, 1] 是正则化参数。L2 正则化在 Python 中实现为：

```py
from sklearn.linear_model import Ridge
lasso = Ridge(alpha=0.7)
Ridge.fit(X_train_std,y_train_std)
y_train_std=Ridge.predict(X_train_std)
y_test_std=Ridge.predict(X_test_std)
Ridge.coef_
```

# 套索回归：L1 正则化

在 L1 正则化中，通过最小化 L1 损失函数来获得回归系数，如下所示：

![L1 和 L2 正则化的区别](../Images/503c1dd1d54e3a5e44abf8c9d9dbc043.png)

其中

![L1 和 L2 正则化的区别](../Images/305d1f7114877d4a758b4cddef3a5822.png)

L1 正则化在 Python 中实现为：

```py
from sklearn.linear_model import Lasso
lasso = Lasso(alpha=0.7)
lasso.fit(X_train_std,y_train_std)
y_train_std=lasso.predict(X_train_std)
y_test_std=lasso.predict(X_test_std)
lasso.coef_
```

在L1和L2正则化中，当正则化参数（α ∈[0, 1]）增加时，这会导致L1范数或L2范数减小，从而将一些回归系数压缩为零。因此，L1和L2正则化模型用于特征选择和降维。L2正则化相较于L1正则化的一个优点是L2损失函数容易求导。

# 案例研究：L1回归实现

使用游轮数据集实现L1正则化的实现可以在这里找到：[Lasso回归的实现](https://github.com/bot13956/ML_Model_for_Predicting_Ships_Crew_Size)。

下图展示了R2分数和L1范数作为正则化参数的函数关系。

![L1和L2正则化之间的区别](../Images/70d937267aaa1d06ce9ed5cde2493797.png)

从上图中可以观察到，随着正则化参数α的增加，回归系数的范数变得越来越小。这意味着更多的回归系数被压缩为零，从而增加了偏差误差（过于简化）。在α保持较低值时，例如α=0.1或更低，偏差-方差权衡的最佳值。

**[本杰明·O·泰约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)**是一位物理学家、数据科学教育者和作家，同时也是DataScienceHub的所有者。之前，本杰明曾在中欧大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关话题

+   [SQL和对象关系映射（ORM）之间的区别是什么？](https://www.kdnuggets.com/2022/02/difference-sql-object-relational-mapping-orm.html)

+   [数据分析师和数据科学家之间的区别是什么？](https://www.kdnuggets.com/2022/03/difference-data-analysts-data-scientists.html)

+   [机器学习中训练数据和测试数据的区别](https://www.kdnuggets.com/2022/08/difference-training-testing-data-machine-learning.html)

+   [效率决定生物神经元与人工神经元的区别](https://www.kdnuggets.com/2022/11/efficiency-spells-difference-biological-neurons-artificial-counterparts.html)

+   [GBM和XGBoost之间的区别是什么？](https://www.kdnuggets.com/wtf-is-the-difference-between-gbm-and-xgboost)

+   [正则化到底是什么？它有什么作用？](https://www.kdnuggets.com/wtf-is-regularization-and-what-is-it-for)
