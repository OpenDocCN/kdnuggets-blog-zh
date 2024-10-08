# 解锁选择完美机器学习算法的秘密！

> 原文：[`www.kdnuggets.com/2023/07/ml-algorithm-choose.html`](https://www.kdnuggets.com/2023/07/ml-algorithm-choose.html)

在解决数据科学问题时，您需要做出的关键决策之一是选择使用哪种[machine learning](https://medium.com/p/313730eb5aa2)算法。

机器学习算法有数百种可供选择，每种算法都有其优缺点。有些算法在特定类型的问题或数据集上可能比其他算法更有效。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 工作

* * *

[“无免费午餐” (NFL) 定理](https://en.wikipedia.org/wiki/No_free_lunch_theorem) 认为，没有一种算法对每个问题都最好，换句话说，所有算法的表现都在所有可能问题的平均表现中相同。

![选择哪个 ML 算法？](img/db7baeaa9d1b6dda200b7c6976d98f00.png)

不同的机器学习模型

在这篇文章中，我们将讨论在为您的问题选择模型时需要考虑的主要点以及如何比较不同的机器学习算法。

# 关键算法方面

以下列表包含了您在考虑特定机器学习算法时可以问自己的 10 个问题：

1.  该算法可以解决哪类型的问题？它能仅解决回归或分类问题，还是两者皆能？它能处理多类/多标签问题还是仅二分类问题？

1.  该算法是否对数据集有任何假设？例如，一些算法假设数据是线性可分的（如感知机或线性 SVM），而其他算法假设数据是正态分布的（如高斯混合模型）。

1.  关于算法的性能是否有任何保证？例如，如果算法尝试解决优化问题（如逻辑回归或神经网络），它是否保证找到全局最优解或仅局部最优解？

1.  训练模型有效需要多少数据？有些算法，如深度神经网络，比其他算法更能处理大量数据。

1.  该算法是否容易过拟合？如果是，它是否提供处理过拟合的方法？

1.  该算法在训练和预测过程中需要多少运行时间和内存？

1.  为了准备数据以供算法使用，需要哪些数据预处理步骤？

1.  算法有多少个超参数？超参数较多的算法在训练和调整时需要更多的时间。

1.  算法的结果是否容易解释？在许多问题领域（如医学诊断），我们希望能够用人类的语言解释模型的预测。一些模型可以被轻松可视化（例如决策树），而其他模型则表现得更像一个黑箱（例如神经网络）。

1.  算法是否支持在线（增量）学习，即我们是否可以在不从头开始重建模型的情况下对其进行额外样本的训练？

# 算法比较示例

例如，假设我们比较两个最流行的算法：[决策树](https://medium.com/@roiyeho/decision-trees-part-1-da4e613d2369)和[神经网络](https://medium.com/@roiyeho/perceptrons-the-first-neural-network-model-8b3ee4513757)，并根据上述标准进行比较。

## 决策树

1.  决策树可以处理分类和回归问题。它们也可以轻松处理多类别和多标签问题。

1.  决策树算法对数据集没有任何特定的假设。

1.  决策树是使用贪婪算法构建的，这种算法并不能保证找到最优树（即最小化分类所有训练样本所需测试次数的树）。然而，如果我们不断扩展其节点直到所有叶节点中的样本都属于同一类，决策树可以在训练集上达到 100%的准确率。这种树通常不是好的预测器，因为它们会过拟合训练集中的噪声。

1.  决策树即使在小型或中型数据集上也能表现良好。

1.  决策树容易过拟合。然而，我们可以通过使用树剪枝来减少过拟合。我们还可以使用[集成方法](https://medium.com/@roiyeho/introduction-to-ensemble-methods-226a5a421687)，例如随机森林，这些方法结合了多个决策树的输出。这些方法在过拟合方面表现较好。

1.  构建决策树的时间为*O*(*n*²*p*)，其中 n 是训练样本的数量，*p* 是特征的数量。决策树中的预测时间取决于树的高度，通常与*n*的对数成正比，因为大多数决策树都相当平衡。

1.  决策树不需要任何数据预处理。它们可以无缝处理不同类型的特征，包括数值特征和分类特征。它们也不需要对数据进行归一化。

1.  决策树有几个关键的超参数需要调整，特别是如果你使用了剪枝方法，比如树的最大深度以及用来决定如何分裂节点的纯度度量。

1.  决策树易于理解和解释，我们可以轻松地对其进行可视化（除非树非常大）。

1.  决策树不能轻易修改以考虑新的训练样本，因为数据集的细微变化可能会导致树的拓扑结构发生重大变化。

## 神经网络

1.  神经网络是现存最通用和灵活的机器学习模型之一。它们几乎可以解决任何类型的问题，包括分类、回归、时间序列分析、自动内容生成等。

1.  神经网络对数据集没有假设，但数据需要被规范化。

1.  神经网络使用梯度下降法进行训练。因此，它们只能找到局部最优解。然而，有多种技术可以用来避免陷入局部最小值，例如动量和自适应学习率。

1.  深度神经网络需要大量数据进行训练，通常在数百万个样本点的数量级。一般来说，网络越大（层数和神经元越多），所需的数据量也越多。

1.  过大的网络可能会记住所有训练样本而无法很好地泛化。对于许多问题，你可以从一个小型网络（例如，只有一两层隐藏层）开始，逐渐增加其规模，直到出现过拟合。你还可以添加 [正则化](https://medium.com/@roiyeho/regularization-19b1879415a1) 以应对过拟合。

1.  神经网络的训练时间取决于许多因素（网络的大小、训练所需的梯度下降迭代次数等）。然而，预测时间非常快，因为我们只需对网络进行一次前向传播即可获得标签。

1.  神经网络要求所有特征都为数值型并进行规范化。

1.  神经网络有许多超参数需要调整，如层数、每层的神经元数量、使用哪个激活函数、学习率等。

1.  神经网络的预测结果难以解释，因为它们基于大量神经元的计算，每个神经元对最终预测的贡献都很小。

1.  神经网络可以轻松适应包括额外的训练样本，因为它们使用增量学习算法（随机梯度下降）。

# 时间复杂度

下表比较了一些流行算法的训练和预测时间（*n* 是训练样本的数量，*p* 是特征的数量）。

![选择哪个机器学习算法？](img/c71fb69ae4ca01cad051997816b89691.png)

# Kaggle 竞赛中最成功的算法

根据 2016 年进行的一项调查，Kaggle 竞赛获胜者最常用的算法是梯度提升算法（XGBoost）和神经网络（参见 [这篇文章](https://www.kaggle.com/code/msjgriffiths/r-what-algorithms-are-most-successful-on-kaggle/report?scriptVersionId=0)）。

在 2015 年的 29 名 Kaggle 竞赛获胜者中，有 8 人使用了 XGBoost，9 人使用了深度神经网络，11 人则使用了两者的集成。

XGBoost 主要用于处理结构化数据（例如关系表格）的问题，而神经网络在处理非结构化问题（例如涉及图像、声音或文本的问题）方面更为成功。

查看这种情况是否仍然存在或者趋势是否已经改变会很有趣（有人愿意接受这个挑战吗？）

感谢阅读！

**[Roi Yehoshua 博士](https://www.linkedin.com/in/roi-yehoshua/)** 是波士顿东北大学的教授，教授数据科学硕士课程。他在多机器人系统和强化学习方面的研究已经在人工智能领域的顶级期刊和会议上发表。他还是 Medium 社交平台的顶级作者，经常发布有关数据科学和机器学习的文章。

[原文](https://medium.com/towards-artificial-intelligence/which-ml-algorithm-to-choose-f9caf674219e)。经许可转载。

### 更多相关主题

+   [在 60 分钟内解锁 LLM 的秘密，与 Andrej Karpathy 一起](https://www.kdnuggets.com/unlock-the-secrets-of-llms-in-a-60-minute-with-andrej-karpathy)

+   [为你的数据集选择合适的聚类算法](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

+   [释放 AI 的力量 - KDnuggets 和 Machine 的特别发布…](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [通过这个免费的 DevOps 短期课程释放你的潜力](https://www.kdnuggets.com/2023/03/corise-unlock-potential-with-this-free-devops-crash-course.html)

+   [ChatGPT 驱动的数据探索：解锁数据集中的隐藏见解](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

+   [解锁你的下一步行动：在热门数据技能提升课程中节省高达 67%](https://www.kdnuggets.com/2023/03/datacamp-unlock-next-move-save-67-indemand-data-upskilling.html)
