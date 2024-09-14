# 如何选择支持向量机核

> 原文：[`www.kdnuggets.com/2016/06/select-support-vector-machine-kernels.html`](https://www.kdnuggets.com/2016/06/select-support-vector-machine-kernels.html)

对于一个任意的数据集，你通常不知道哪个核可能效果最好。我建议首先从最简单的假设空间开始——因为你对数据了解不多——然后逐步过渡到更复杂的假设空间。因此，如果你的数据集线性可分，线性核表现良好；然而，如果你的数据集不是线性可分的，线性核就不够用了（几乎是字面上的意思；）。

为了简化（以及可视化目的），我们假设我们的数据集仅包含 2 个维度。下面，我绘制了在鸢尾花数据集的 2 个特征上线性 SVM 的决策区域：

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

![选择 SVM 核](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels/1.png)

这完全可以正常工作。接下来是 RBF 核 SVM：

![选择 SVM 核](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels/2.png)

现在，看起来线性和 RBF 核 SVM 在这个数据集上都可以表现得很好。那么，为什么要偏好更简单的线性假设呢？在这种情况下考虑奥卡姆剃刀原则。线性 SVM 是一个参数模型，而 RBF 核 SVM 则不是，后者的复杂性随着训练集的大小增加。不仅训练 RBF 核 SVM 更昂贵，你还需要保留核矩阵，并且在预测时将数据投影到这个“无限”的更高维空间使数据变得线性可分的代价也更高。此外，你还需要调整更多的超参数，因此模型选择也更昂贵！最后，更复杂的模型更容易过拟合！

好吧，我刚才说的关于核方法的内容听起来都非常负面，但这实际上取决于数据集。例如，如果你的数据不是线性可分的，那么使用线性分类器就没有意义了：

![选择 SVM 核](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels/3.png)

在这种情况下，RBF 核会更有意义：

![选择 SVM 核](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels/4.png)

无论如何，我不会过于纠结于多项式核。在实践中，它在效率（计算和预测）方面的作用较小。因此，经验法则是：对于线性问题使用线性 SVM（或逻辑回归），对于非线性问题使用如径向基函数核等非线性核。

RBF 核 SVM 的决策区域实际上也是一个线性决策区域。RBF 核 SVM 所做的实际上是创建特征的非线性组合，将样本提升到一个更高维的特征空间，在那里你可以使用线性决策边界来分隔你的类别：

![选择 SVM 核](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels/5.png)

好的，上面我给你展示了一个直观的例子，我们可以在 2 维空间中可视化我们的数据……但在实际问题中，即数据集有超过 2 个维度时，我们该如何处理？在这里，我们需要关注我们的目标函数：最小化铰链损失。我们会设置一个超参数搜索（例如网格搜索），并将不同的核函数相互比较。根据损失函数（或像准确率、F1、MCC、ROC auc 等性能指标），我们可以确定哪个核函数对于给定任务是“合适的”。

**简历： [塞巴斯蒂安·拉施卡](https://twitter.com/rasbt)** 是一位‘数据科学家’和机器学习爱好者，对 Python 及开源充满热情。《[Python 机器学习](https://www.packtpub.com/big-data-and-business-intelligence/python-machine-learning)》的作者。密歇根州立大学。

[原文](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/select_svm_kernels.md)。经许可转载。

**相关：**

+   深度学习何时比 SVM 或随机森林更有效？

+   分类作为学习机器的发展

+   为什么从头实现机器学习算法？

### 更多相关话题

+   [支持向量机：一种直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python 向量数据库和向量索引：构建 LLM 应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [如何在机器学习中从巨大的数据集中正确选择样本](https://www.kdnuggets.com/2019/05/sample-huge-dataset-machine-learning.html)

+   [如何在 Pandas 中使用 [ ]、.loc、iloc、.at 等选择行和列](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)
