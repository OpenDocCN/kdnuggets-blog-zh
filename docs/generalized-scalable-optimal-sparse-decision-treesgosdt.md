# 广义且可扩展的最佳稀疏决策树 (GOSDT)

> 原文：[`www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html`](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

![广义且可扩展的最佳稀疏决策树 (GOSDT)](img/f314c76c07388670f39e82fba455f7ea.png)

图片来源 [fabrikasimf](https://www.freepik.com/free-photo/network-with-pins_20988125.htm#query=Decision%20Trees&position=49&from_view=search&track=ais) 在 Freepik

我经常谈论可解释 AI（XAI）方法及其如何被调整以解决一些阻碍公司构建和部署 AI 解决方案的痛点。如果你需要快速回顾 XAI 方法，可以查看我的 [博客](https://medium.com/@supreetkaur_66831/explainable-ai-xai-building-interpretable-models-d616b0fccd33)。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

一种这样的 XAI 方法是决策树。由于其可解释性和简洁性，决策树历史上获得了显著的关注。然而，许多人认为决策树不能准确，因为它们看起来简单，而像 C4.5 和 CART 这样的贪婪算法不能很好地优化它们。

这个说法部分有效，因为某些变体的决策树，如 C4.5 和 CART，具有以下缺点：

1.  易于过拟合，特别是当树变得过深且分支过多时。这可能会导致在新数据上的表现不佳。

1.  由于需要根据输入特征的值做出多个决策，因此在大数据集上评估和预测可能会比较慢。

1.  处理连续变量可能很困难，因为它们要求将变量划分为多个更小的区间，这可能会增加树的复杂性，并使识别数据中的有意义模式变得困难。

1.  通常被称为“贪婪”算法，它在每一步做出局部最优决策，而不考虑这些决策对未来步骤的影响。次优树是 CART 的输出，但没有“真正的”指标来衡量它。

更复杂的算法，如集成学习方法，可以解决这些问题。但通常它们被认为是“黑箱”，因为算法的底层工作机制不透明。

然而，近期的研究表明，如果优化决策树（而不是使用像 C4.5 和 CART 这样的贪婪方法），它们可以在许多情况下与黑箱模型一样准确。GOSDT 是一个可以帮助优化并解决上述一些缺点的算法。GOSDT 是一个生成稀疏最优决策树的算法。

本博客旨在对 GOSDT 进行温和的介绍，并展示如何在数据集上实现它的示例。

本博客基于几位优秀人士发表的研究论文。你可以在[这里](https://arxiv.org/pdf/2006.08690.pdf)阅读这篇论文。这个博客并不是该论文的替代品，也不会涉及极其数学化的细节。这是为数据科学从业者提供的指南，用于了解这个算法并在日常用例中加以利用。

简而言之，GOSDT 解决了几个主要问题：

1.  能够很好地处理不平衡数据集并优化各种目标函数（不仅仅是准确率）。

1.  完全优化树，而不是贪婪地构建它们。

1.  它几乎与贪婪算法一样快，因为它解决了决策树的 NP-hard 优化问题。

# GOSDT 树如何解决上述问题？

1.  GOSDT 树通过哈希树使用动态搜索空间来提高模型的效率。通过限制搜索空间并使用边界来识别相似变量，GOSDT 树可以减少找到最佳切分所需的计算次数。这可以显著提高计算时间，特别是在处理连续变量时。

1.  在 GOSDT 树中，切分的边界应用于部分树，并用于从搜索空间中消除许多树。这使得模型可以专注于剩余的树（这可以是部分树）并更高效地评估它。通过减少搜索空间，GOSDT 树可以快速找到最佳切分，并生成更准确、更易解释的模型。

1.  GOSDT 树旨在处理不平衡数据，这在许多实际应用中是一个常见挑战。GOSDT 树通过加权准确度指标来解决不平衡数据，该指标考虑了数据集中不同类别的相对重要性。当有一个预定的准确度阈值时，这尤其有用，因为它允许模型专注于正确分类对应用更关键的样本。

# 总结 GOSDT 的观察结果

1.  这些树直接优化了训练准确率和叶子数量之间的权衡。

1.  以合理数量的叶子生成优秀的训练和测试准确率

1.  非常适合高度非凸问题

1.  最适合小型或中等数量的特征。但它可以处理多达数万个观测值，同时保持其速度和准确性。

是时候看到所有实际操作了！！在我之前的博客中，我使用 Keras 分类解决了一个贷款申请批准问题。我们将使用相同的数据集来构建一个使用 GOSDT 的分类树。

# 代码示例

作者代码

**[Supreet Kaur](https://www.linkedin.com/in/supreet-kaur1995/)** 是摩根士丹利的副总裁。她是一位健身和科技爱好者，同时也是名为 DataBuzz 的社区的创始人。

### 更多相关内容

+   [Python 中的稀疏矩阵表示](https://www.kdnuggets.com/2020/05/sparse-matrix-representation-python.html)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [适用于稀疏数据的最佳机器学习模型](https://www.kdnuggets.com/2023/04/best-machine-learning-model-sparse-data.html)

+   [从头开始学习机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)

+   [决策树与随机森林的比较与解释](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [揭开决策树的神秘面纱](https://www.kdnuggets.com/demystifying-decision-trees-for-the-real-world)
