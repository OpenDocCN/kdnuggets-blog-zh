# 逻辑回归中的正则化：更好的拟合和更好的泛化？

> 原文：[https://www.kdnuggets.com/2016/06/regularization-logistic-regression.html](https://www.kdnuggets.com/2016/06/regularization-logistic-regression.html)

正则化不会提高模型用于学习参数（特征权重）的数据集上的性能。然而，它可以提高泛化性能，即在新的、未见过的数据上的表现，这正是我们想要的。

从直观上讲，我们可以把正则化看作是对复杂度的惩罚。增加正则化强度会惩罚“较大”的权重系数——我们的目标是防止模型捕捉到“特异性”、“噪声”或“在不存在模式的地方假设模式”。

再次强调，我们不希望模型记住训练数据集，我们希望模型对新的、未见过的数据有良好的泛化能力。

更具体来说，我们可以把正则化看作是增加（或提高）偏差的过程，如果我们的模型存在（高）方差（即，它过拟合了训练数据）。另一方面，过多的偏差会导致欠拟合（高偏差的一个特征指标是模型在训练和测试数据集上的表现都很“差”）。我们知道，在未正则化的模型中，我们的目标是最小化成本函数，即，我们想找到与全局成本最小值对应的特征权重（记住，逻辑回归的成本函数是凸的）。

[![](../Images/246f23a1b7db46b1291182f90099e8e3.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/regularized-logistic-regression-performance/unregularized.png)

现在，如果我们对成本函数进行正则化（例如，通过L2正则化），我们在成本函数（J）中增加一个额外的项，该项随着参数权重（w）值的增加而增加；请记住，我们添加的正则化还引入了一个新的超参数lambda，用于控制正则化强度。

[![](../Images/00455a306f38c40c05ede9be3aa95de4.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/regularized-logistic-regression-performance/l2-term.png)

因此，我们的新问题是最小化成本函数，给定这个附加的约束。

[![](../Images/c60871e2ad31b45d65061c2e7dbb97cb.png)](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/regularized-logistic-regression-performance/regularized.png)

从直观上讲，我们可以把图中坐标中心的“球体”看作是我们的“预算”。现在，我们的目标仍然是相同的：我们想要最小化成本函数。然而，我们现在受到正则化项的约束；我们希望在保持在我们的“预算”（即球体）内的同时尽可能接近全局最小值。

**个人简介: [塞巴斯蒂安·拉施卡](https://twitter.com/rasbt)** 是一位‘数据科学家’和机器学习爱好者，对 Python 和开源有着极大的热情。著作有《[Python 机器学习](https://www.packtpub.com/big-data-and-business-intelligence/python-machine-learning)》。密歇根州立大学。

[原始文档](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/regularized-logistic-regression-performance.md)。经许可转载。

**相关:**

+   [深度学习何时优于 SVM 或随机森林？](/2016/04/deep-learning-vs-svm-random-forest.html)

+   [分类作为学习机器的发展](/2016/04/development-classification-learning-machine.html)

+   [为什么从头实现机器学习算法？](/2016/05/implement-machine-learning-algorithms-scratch.html)

### 更多相关内容

+   [比较线性回归与逻辑回归](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [分类指标指南: 逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [用自定义指令调整 ChatGPT 以适应您的需求](https://www.kdnuggets.com/2023/08/tailor-chatgpt-fit-needs-custom-instructions.html)

+   [线性回归与逻辑回归: 简洁的解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12, 3月 23: 最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [分类的逻辑回归](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)
