# 深度学习的扩展是可预测的，经验上

> 原文：[https://www.kdnuggets.com/2018/05/deep-learning-scaling-predictable-empirically.html](https://www.kdnuggets.com/2018/05/deep-learning-scaling-predictable-empirically.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

[深度学习的扩展是可预测的，经验上](https://arxiv.org/abs/1712.00409) Hestness等人，*arXiv，2017年12月*

*感谢Nathan Benaich在他的优秀[2018年第一季度AI世界总结](https://www.getrevue.co/profile/nathanbenaich/issues/your-guide-to-ai-in-q1-2018-by-nathan-ai-100379)中突出了这篇论文*

这是一项非常出色的研究，具有深远的影响，甚至可能在某些情况下影响公司战略。研究始于一个简单的问题：“我们如何改进深度学习的最新技术？”我们有三条主要攻击线：

1.  我们可以寻找改进的*模型架构*。

1.  我们可以*扩展计算*。

1.  我们可以创建*更大的训练数据集*。

> 随着DL应用领域的增长，我们希望更深入地理解训练集规模、计算规模和模型准确性改进之间的关系，以推动技术的前沿。

寻找更好的模型架构通常依赖于“不可依赖的顿悟”，正如结果所示，与增加可用数据量相比，影响有限。当然，我们已经知道这一点，包括从2009年的Google论文中，[‘数据的非理性有效性](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/35179.pdf)’。今天的论文结果帮助我们量化了在各种深度学习应用中的数据优势。理解的关键在于以下方程：

![\displaystyle \epsilon(m) \sim \alpha m^{\beta_g} + \gamma](../Images/fdc3d545505be6fa9d5f9b5ca22b8968.png)

这表明一般化误差![\epsilon](../Images/a97aa34ff2e2a0e8c415f52f0e96e2e2.png)作为训练数据量![m](../Images/152a02f7d270706469057ec5bc183e60.png)的函数，遵循具有指数![\beta_g](../Images/dd7a8a5ee65d76f35eff545a1f01f234.png)的*幂律*。经验上，![\beta_g](../Images/4e84cad13f39dbf8713190b80b67cf2c.png)通常在-0.07到-0.35之间。误差当然是，较低更好，因此为负指数。我们通常在对数-对数图上绘制幂律，其中它们结果为直线，梯度表示指数。

制作*更好的模型*可以将y截距降低（直到达到不可约错误水平），但似乎不会影响幂律系数。另一方面，*更多的数据*将使我们沿着改进的幂律路径前进。

![](../Images/fd2828eb8b5f8b54ff75d2a9596763f7.png)

### 三个学习区域

实际应用的学习曲线可以分为三个区域：

![](../Images/a498412270207a1e2a0024c1231235b8.png)

+   **小数据区域**是模型在数据不足的情况下挣扎学习的地方，模型的表现只能和“最佳”或“随机”猜测一样好。

+   中间区域是**幂律区域**，在该区域，幂律指数定义了曲线的陡峭程度（对数对数尺度上的斜率）。这个指数是*模型表示数据生成函数的难度指标*。

    > “*本文的结果表明，幂律指数不容易通过先验理论来预测，并且**可能依赖于问题领域或数据分布的某些方面。***”

+   **不可减少误差区域**是指模型无法改进的非零下界误差。对于足够大的训练集，模型在这个区域会达到饱和。

### 关于模型大小

> 我们预期模型参数的数量与数据集的适配应遵循![s(m) \sim \alpha m^{\beta_p}](../Images/24d8f00443ba3e4ce18240cfff2d93e0.png)，其中![s(m)](../Images/2203762900df00c7411cf70a847bfdda.png)是适应大小为![m](../Images/152a02f7d270706469057ec5bc183e60.png)的训练集所需的模型大小，而![\beta_p \in [0.5,1]](../Images/c5436a709bbb30c80b8049944cde8cd7.png)。

最佳拟合模型在训练分片大小上以次线性增长。较高的![\beta_p](../Images/c98dfbc3e9d18694997557bb9e9c1e2b.png)值表明模型在较大的数据集上对额外参数的使用效果较差。尽管模型大小扩展存在差异，“*对于给定的模型架构，我们可以准确预测能够最好地适应越来越大数据集的模型大小。*”

### 幂律的影响

> 可预测的学习曲线和模型大小扩展表明，深度学习可能会有一些重要的影响。对于机器学习从业者和研究人员而言，可预测的扩展可以帮助模型和优化调试以及迭代时间，并提供估计提高模型准确性的最有影响力的下一步的方法。在操作上，可预测的曲线可以指导是否以及如何增长数据集或计算。

一个有趣的结果是，*模型探索*可以在较小的数据集上进行（因此更快/更便宜）。数据集需要足够大，以显示曲线的幂律区域的准确性。然后，可以将最有前景的模型扩展到更大的数据集上，以确保相应的准确性提升。这是因为扩展训练集和模型很可能会在模型之间带来相同的相对增益。在建立公司时，我们在扩展业务之前会寻找产品市场匹配。在深度学习中，似乎类比为在扩展之前寻找*模型问题匹配*，即先搜索再扩展的策略：

![](../Images/a1f261ca02c750e1ec53a7d399af0955.png)

如果你需要对投资更多数据收集的投资回报率或可能的准确性提升做出商业决策，幂律可以帮助你预测回报：

![](../Images/957ad0a1cf564181bfeeadc305e9c9e9.png)

相反，如果在幂律区域内的泛化误差偏离幂律预测，这表明增加模型规模可能会有所帮助，或者更广泛的超参数搜索（即更多的计算）可能会有所帮助：

![](../Images/241cec1bf74a0100d141caba41031176.png)

> …可预测的学习和模型规模曲线可能提供了一种预测达到特定准确度水平的计算需求的方法。

### 你能超越幂律吗？

模型架构改进（例如，增加模型深度）似乎仅仅是将学习曲线向下移动，但不会改善幂律指数。

> 我们尚未找到影响幂律指数的因素。要在增加数据集规模时超越幂律，模型需要用逐渐减少的数据学习更多的概念。换句话说，模型必须从每个额外的训练样本中提取更多的边际信息。

如果我们*能够*找到改进幂律指数的方法，那么某些问题领域的潜在准确性提升是“巨大的”。

### 实证数据

支持这些的实证数据是通过在多个不同领域使用最新深度学习模型测试各种训练数据大小（以2的幂次方）收集的。

这是神经机器翻译的学习曲线。右侧可以看到每个训练集大小的最佳拟合模型的结果。随着训练集大小的增加，实证误差偏离幂律趋势，可能需要更全面的超参数搜索来将其重新对齐。

![](../Images/162e60cbdae4d8249de4838b19dd9cba.png)

下一个领域是词语言模型。虽然使用了各种模型架构，它们的差异很大，但都表现出相同的学习曲线特征，由幂律指数表征。

![](../Images/a62fa308c7a0a853d9170cc7826d1761.png)

这是字符语言模型的结果：

![](../Images/c583cb656be9cbf924c71afd78ff0b29.png)

在图像分类中，当数据不足以学习出良好的分类器时，我们可以在图中看到“数据稀少区域”出现：

![](../Images/ab3deb189ed0a1f1f331582c99ad0b29.png)

最后，这里是语音识别：

![](../Images/2ad30be4abd103ec4c201536b32f984f.png)

> 我们实证验证了，随着在四个机器学习领域（机器翻译、语言建模、图像处理和语音识别）中为最先进（SOTA）模型架构增加训练集，DL模型的准确性按幂律增长。这些幂律学习曲线在所有测试领域、模型架构、优化器和损失函数中都存在。

[原文](https://blog.acolyer.org/2018/03/28/deep-learning-scaling-is-predictable-empirically/)。转载经许可。

**相关内容：**

+   [如何让AI更易获得](/2018/04/make-ai-more-accessible.html)

+   [高级API是否让机器学习变得更简单？](/2018/04/high-level-apis-dumbing-down-machine-learning.html)

+   [逐步推导卷积神经网络与全连接网络的关系](/2018/04/derivation-convolutional-neural-network-fully-connected-step-by-step.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关话题

+   [通过 Apache Gobblin 扩展数据管理](https://www.kdnuggets.com/2023/01/scaling-data-management-apache-gobblin.html)

+   [使用 Python 进行数据扩展](https://www.kdnuggets.com/2023/07/data-scaling-python.html)

+   [扩展你的网络数据驱动产品时需要知道的事情](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)

+   [学习数据科学、机器学习和深度学习的扎实计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [15 本免费的机器学习和深度学习书籍](https://www.kdnuggets.com/2022/10/15-free-machine-learning-deep-learning-books.html)
