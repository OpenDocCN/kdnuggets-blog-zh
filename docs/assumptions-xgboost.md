# XGBoost 的假设是什么？

> 原文：[`www.kdnuggets.com/2022/08/assumptions-xgboost.html`](https://www.kdnuggets.com/2022/08/assumptions-xgboost.html)

![XGBoost 的假设是什么？](img/640a6c40bc036eae9847f9795f190915.png)

[Faye Cornish](https://unsplash.com/@fcornish) 通过 Unsplash

在我们深入探讨 XGBoost 的假设之前，我将对该算法进行概述。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织进行 IT 工作

* * *

XGBoost 代表极端梯度提升，是一种监督学习算法，属于梯度提升决策树（GBDT）家族的机器学习算法。

让我们首先深入了解提升方法。

# 从提升到 XGBoost

提升方法是将一组弱学习器结合成一个强学习器，以减少训练误差的水平。提升方法有助于处理偏差-方差权衡，使其更有效。有不同类型的提升算法，如 AdaBoost（自适应提升）、梯度提升、XGBoost 等。

现在让我们深入了解 XGBoost

如前所述，XGBoost 是对梯度提升决策树（GBM）的扩展，以其速度和性能而闻名。

预测是基于结合一组更简单、更弱的模型进行的——这些模型是以序列形式创建的决策树。这些模型通过 if-then-else 的真假特征问题评估其他决策树，以此来评估和估计产生正确决策的概率。

它包含以下三个要素：

1.  一个需要优化的损失函数。

1.  一个弱学习器用来进行预测。

1.  一种加性模型，用于增加到弱学习器中以减少错误

# XGBoost 的特点

XGBoost 有三个特点：

## 1\. 梯度树提升

树集合模型需要以加性方式进行训练。这意味着这是一个迭代和顺序的过程，其中决策树一步步地增加。添加的树数量是固定的，并且每次迭代时，损失函数值应该减少。

## 2\. 正则化学习

正则化学习有助于最小化损失函数，并防止过拟合或欠拟合的发生——帮助平滑最终学习到的权重。

## 3\. 收缩和特征子采样

这两种技术用于进一步防止过拟合。

收缩减少了每棵树对整体模型的影响程度，并为未来的树提供了改进的空间。

特征子采样是你可能在随机森林算法中见过的。特征位于数据的列部分，它不仅能防止过拟合，还能加快并行算法的计算速度。

# XGBoost 超参数

```py
import xgboost as xgb
```

XGBoost 超参数分为 4 个组：

1.  一般参数

1.  增强器参数

1.  学习任务参数

1.  命令行参数

一般参数、增强器参数和任务参数在运行 XGBoost 模型之前设置。命令行参数仅在 XGBoost 的控制台版本中使用。

如果参数未正确调整，可能会导致过拟合。然而，调优 XGBoost 模型的参数是困难的。

敬请期待关于调优 XGBoost 超参数的即将发布的文章。

# XGBoost 的假设是什么？

XGBoost 的主要假设是：

+   XGBoost 可能假设每个输入变量的编码整数值具有顺序关系

+   XGBoost 假设你的数据可能不完整（即它可以处理缺失值）

由于算法**不假设所有值都存在**，它可以默认处理缺失值。在处理基于树的算法时，缺失值在训练阶段会被学习。这导致：

+   XGBoost 能够处理稀疏性

XGBoost 仅管理数值向量，因此如果你有分类变量，它们需要转换为数值变量。

你将不得不将一个密集的数据框（矩阵中几乎没有零）转换为一个稀疏矩阵（矩阵中有大量零）。这意味着 XGBoost 能够将变量转换为稀疏矩阵格式作为输入。

# 结论

在这个博客中，你已经了解了：提升方法与 XGBoost 的关系；XGBoost 的特点；它如何减少损失函数值和过拟合。我们简要介绍了 4 个超参数，接下来将发布一篇专注于调优 XGBoost 超参数的文章。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一位数据科学家和自由技术作家。她特别关注提供数据科学职业建议或教程以及数据科学的理论知识。她还希望探索人工智能在延长人类生命方面的不同方式。作为一名热心学习者，她希望扩大她的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [如何加快 XGBoost 模型训练](https://www.kdnuggets.com/2021/12/speed-xgboost-model-training.html)

+   [调优 XGBoost 超参数](https://www.kdnuggets.com/2022/08/tuning-xgboost-hyperparameters.html)

+   [利用 XGBoost 进行时间序列预测](https://www.kdnuggets.com/2023/08/leveraging-xgboost-timeseries-forecasting.html)

+   [GBM 和 XGBoost 之间的区别是什么？](https://www.kdnuggets.com/wtf-is-the-difference-between-gbm-and-xgboost)
