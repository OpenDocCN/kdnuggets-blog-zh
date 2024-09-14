# 决策树修剪：如何和为什么

> 原文：[`www.kdnuggets.com/2022/09/decision-tree-pruning-hows-whys.html`](https://www.kdnuggets.com/2022/09/decision-tree-pruning-hows-whys.html)

![决策树修剪：如何和为什么](img/ad5cf415445386976861e940e07248ed.png)

[Devin H](https://unsplash.com/@devin_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) via Unsplash

让我们回顾一下决策树。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

决策树是一种非参数的监督学习方法，可用于分类和回归任务。目标是通过学习从数据特征中推断出的简单决策规则，建立一个能够预测目标变量值的模型。

决策树由以下部分组成

1.  根节点 - 决策树的最顶部，是你尝试做出的**最终决策**。

1.  内部节点 - 从根节点分支出来，表示不同的选项。

1.  叶子节点 - 这些节点附加在分支的末端，表示每个动作的可能结果。

就像任何其他机器学习算法一样，最烦人的事情就是过拟合。决策树就是容易过拟合的机器学习算法之一。

过拟合是指模型完全拟合训练数据，却难以或无法泛化测试数据。这发生在模型记住了训练数据中的噪声，而没有捕捉到帮助处理测试数据的重要模式。

降低决策树过拟合的技术之一是修剪。

# 什么是决策树修剪，它为何重要？

修剪是一种移除决策树中阻止其完全生长的部分的技术。它移除的部分是不提供分类实例能力的部分。一个完全生长的决策树很可能会导致过拟合训练数据，因此修剪是重要的。

简单来说，决策树修剪的目的是构建一个在训练数据上表现较差但在测试数据上泛化更好的算法。调整决策树模型的超参数可以大大改善模型表现，节省时间和金钱。

# 如何修剪决策树？

剪枝有两种类型：预剪枝和后剪枝。我将介绍它们以及它们是如何工作的。

## 预剪枝

决策树的预剪枝技术是在训练管道之前调整超参数。它涉及到一种被称为‘早期停止’的启发式方法，该方法会停止决策树的生长 - 防止其达到完整深度。

它会停止树的构建过程，以避免生成样本量较小的叶子。在树的每个分裂阶段，将会监控交叉验证误差。如果误差值不再减少 - 那么我们就停止决策树的生长。

可以调节的超参数以进行早期停止和防止过拟合是：

`max_depth`、`min_samples_leaf` 和 `min_samples_split`

这些相同的参数也可以用于调整以获得稳健模型。然而，你应该谨慎，因为早期停止也可能导致欠拟合。

## 后剪枝

后剪枝与预剪枝相反，允许决策树模型生长到其完整深度。一旦模型生长到其完整深度，树枝将被移除以防止模型过拟合。

算法将继续将数据划分为更小的子集，直到最终产生的子集在结果变量方面相似。树的最终子集将仅包含少量数据点，从而使树能够完全学习数据。然而，当引入与学习数据不同的新数据点时 - 它可能无法得到良好的预测。

可用于后剪枝和防止过拟合的超参数是：`ccp_alpha`

`ccp` 代表成本复杂度剪枝，可以用作控制树大小的另一种选项。较高的`ccp_alpha`值将导致更多节点被修剪。

成本复杂度剪枝（后剪枝）步骤：

1.  将决策树模型训练到其完整深度

1.  使用`cost_complexity_pruning_path()`计算`ccp_alphas`值

1.  用不同的`ccp_alphas`值训练你的决策树模型，并计算训练和测试性能分数

1.  绘制每个`ccp_alphas`值的训练和测试分数。

这个超参数也可以用于调整以获得最佳拟合模型。

# 提示

这里有一些你可以在决策树剪枝时应用的提示：

+   如果节点变得非常小，则不要继续分裂

+   在没有早期停止的情况下，最小误差（交叉验证）剪枝是一种好的技术

+   构建一个完整深度的树，然后通过在每个阶段应用统计检验来向后工作

+   修剪一个内部节点，并将其下方的子树上移一级

# 结论

在这篇文章中，我已经详细讲解了两种修剪技术及其用途。决策树非常容易出现过拟合——因此修剪是算法中一个至关重要的阶段。如果你想了解更多关于使用成本复杂度修剪的后期修剪决策树的内容，请点击这个[链接](https://scikit-learn.org/stable/auto_examples/tree/plot_cost_complexity_pruning.html#:~:text=Cost%20complexity%20pruning%20provides%20another,the%20number%20of%20nodes%20pruned/)。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)**是一名数据科学家和自由技术写作员。她特别关注于提供数据科学职业建议或教程以及数据科学的理论知识。她还希望探索人工智能如何或可以有益于人类寿命的不同方式。作为一个热衷学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关主题

+   [用 Python 和 Scikit-learn 简化决策树可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [决策树算法，解释](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)

+   [通过实现理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [讲述一个伟大的数据故事：一个可视化决策树](https://www.kdnuggets.com/2021/02/telling-great-data-story-visualization-decision-tree.html)

+   [随机森林与决策树：主要区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [KDnuggets™ 新闻 22:n09, 3 月 2 日: 讲述一个伟大的数据故事：A…](https://www.kdnuggets.com/2022/n09.html)
