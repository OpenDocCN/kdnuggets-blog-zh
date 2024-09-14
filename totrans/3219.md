# 随机森林®，解析

> 原文：[https://www.kdnuggets.com/2017/10/random-forests-explained.html](https://www.kdnuggets.com/2017/10/random-forests-explained.html)

![](../Images/32bd7c1f79cf7bfb80a751e5590bb162.png)

在这篇文章中，我们将概述一种非常流行的集成方法——随机森林（®）。我们首先讨论这种集成学习算法的基本组件——决策树，然后介绍其底层算法和训练程序。我们还将讨论该工具的一些变体和优势，最后提供一些资源，帮助你入门这一强大的工具。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供IT支持

* * *

*决策树简述*

决策树是一种机器学习算法，能够适应复杂的数据集，并执行分类和回归任务。树的核心思想是在训练集中寻找一对变量-值，并将其拆分，以生成“最佳”的两个子集。目标是基于最佳拆分标准创建分支和叶子，这一过程称为树的生长。具体来说，在每个分支或节点处，一个条件语句会根据特定变量的固定阈值对数据点进行分类，从而实现数据拆分。为了进行预测，每个新实例从根节点（树的顶部）开始，沿着分支移动，直到到达一个叶子节点，此时不再进行进一步的分支。

用于训练树的算法称为CART（®）（分类与回归树）。正如我们之前提到的，算法寻找最佳的特征-值对来创建节点和分支。在每次拆分后，这一任务会递归进行，直到达到树的最大深度或找到一个优化的树。根据任务的不同，算法可能使用不同的度量标准（基尼不纯度、信息增益或均方误差）来衡量拆分的质量。需要提到的是，由于CART算法的贪婪特性，找到一个最优的树并不保证，通常情况下，一个合理的估计就足够了。

树有很高的过拟合训练数据的风险，并且如果在生长阶段没有适当约束和正则化，它们可能会变得计算复杂。这种过拟合意味着模型的低偏差、高方差权衡。因此，为了解决这个问题，我们使用集成学习，这种方法可以纠正过度学习的习惯，并希望得到更好、更强的结果。

*什么是集成方法？*

集成方法或集成学习算法通过汇总由不同预测因子集产生的多个输出以获得更好的结果。正式地说，基于一组“弱”学习者，我们试图为模型使用一个“强”学习者。因此，使用集成方法的目的是：通过多样化预测因子集来平均单个预测的结果，从而降低方差，得到一个强大的预测模型，减少对训练集的过拟合。

在我们的例子中，随机森林（强学习者）作为决策树（弱学习者）的集成来执行回归和分类等不同任务。

![](../Images/2b5e97066ca2c2499491ffcb8bd2221c.png)

*随机森林是如何训练的？*

随机森林通过袋装方法进行训练。袋装或自助聚合方法包括随机抽取训练数据的子集，拟合模型到这些较小的数据集上，并汇总预测结果。由于我们进行有放回抽样，这种方法允许多个实例在训练阶段被重复使用。树袋装方法包括抽取训练集的子集，对每个子集拟合一棵决策树，并汇总它们的结果。

随机森林方法通过将袋装法应用于特征空间来引入更多的随机性和多样性。即，不是贪婪地寻找最佳预测因子来创建分支，而是随机抽取预测因子空间中的元素，从而增加更多多样性并降低树的方差，但代价是偏差相同或更高。这个过程也被称为“特征袋装”，正是这个强大的方法使得模型更加稳健。

现在我们来看看如何使用随机森林进行预测。记住，在决策树中，一个新实例从根节点走到最底层，直到在叶节点被分类。在随机森林算法中，每个新数据点经过相同的过程，但现在它访问集成中的所有不同树，这些树是使用训练数据和特征的随机样本生长起来的。根据手头的任务，聚合所用的函数会有所不同。对于分类问题，它使用个体树预测的模式或最频繁的类别（也称为多数投票），而对于回归任务，它使用每棵树的平均预测值。

尽管这是一种在机器学习中使用的强大且准确的方法，但你应该始终交叉验证你的模型，因为可能会出现过拟合。此外，尽管它非常稳健，但随机森林算法运行缓慢，因为在训练阶段必须生成许多树，正如我们所知，这是一个贪婪的过程。

*变体*

正如我们之前所述，随机森林使用训练数据和特征空间的采样子集，从而产生高多样性和随机性以及低方差。现在，我们可以更进一步，通过不仅寻找随机预测器，还考虑这些变量的随机阈值来引入更多的多样性。因此，它不是寻找特征和阈值的最佳配对用于分裂，而是使用这两者的随机样本来创建不同的分支和节点，从而进一步用偏差换取方差。这个集成方法也被称为极端随机树或额外树。这个模型也用更多的偏差换取较低的方差，但训练更快，因为它不像随机森林那样寻找最佳解。

*附加属性*

随机森林的另一个重要特性是它们在确定特征或变量重要性时非常有用。因为重要特征往往位于每棵树的顶部，而不重要的变量位于底部，可以通过测量这一点的平均深度来衡量其重要性。

*更多信息*

你可以在[这个维基百科文章](https://en.wikipedia.org/wiki/Random_forest)中找到更多关于随机森林的信息。约翰霍普金斯大学Coursera的[实践机器学习](https://www.coursera.org/learn/practical-machine-learning/lecture/XKsl6/random-forests)课程提供了一个直观的方法，并且在R中有应用。对于Python实现，请[跟随这段代码](https://github.com/ageron/handson-ml/blob/master/07_ensemble_learning_and_random_forests.ipynb)，它来自Aurélien Géron的[书籍](https://shop.oreilly.com/product/0636920052289.do)《动手机器学习：Scikit-Learn和TensorFlow》。

Random Forests™ 是Leo Breiman和Adele Cutler的商标，并被[Salford Systems](https://www.salford-systems.com/products/randomforests)独家授权用于商业软件发布。CART(®) 是加利福尼亚统计软件公司的注册商标，并被[Salford Systems](https://www.salford-systems.com/products/cart)独家授权。

*相关内容*

+   [Python中的随机森林](/2016/12/random-forests-python.html)

+   [理解机器学习算法](/2017/10/understanding-machine-learning-algorithms.html)

+   [与算法的亲密接触](/2017/03/dataiku-top-algorithms.html)

### 更多相关话题

+   [决策树与随机森林的解释](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [停止学习数据科学，寻找目标，并以目标为…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的人工智能失败，深度剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [数据科学学习统计学的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
