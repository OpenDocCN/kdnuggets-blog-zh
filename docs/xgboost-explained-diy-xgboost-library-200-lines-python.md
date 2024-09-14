# XGBoost 详解：用少于 200 行 Python 代码实现 DIY XGBoost 库

> 原文：[https://www.kdnuggets.com/2021/05/xgboost-explained-diy-xgboost-library-200-lines-python.html](https://www.kdnuggets.com/2021/05/xgboost-explained-diy-xgboost-library-200-lines-python.html)

[comments](#comments)

**由 [Guillaume Saupin](https://www.linkedin.com/in/guillaume-saupin-5802aa31/)，Verteego 的 CTO 提供**

![](../Images/2af80546d15b24dbb95b3e7f06ad2b1d.png)

照片由[Jens Lelie](https://unsplash.com/@madebyjens?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

XGBoost 可能是数据科学中使用最广泛的库之一。全球许多数据科学家都在使用它。它是一个非常通用的算法，可用于执行分类、回归以及置信区间，如本文[文章](https://towardsdatascience.com/confidence-intervals-for-xgboost-cac2955a8fde)所示。但有多少人真正理解它的基本原理呢？

你可能会认为，像 XGBoost 这样表现出色的机器学习算法可能使用了非常复杂和先进的数学方法。你可能会想象它是一个软件工程的杰作。

你部分正确。XGBoost 库确实相当复杂，但如果仅考虑应用于决策树的梯度提升的数学公式，其实并没有那么复杂。

下面你将详细了解如何使用少于 200 行代码，通过梯度提升方法训练**回归**决策树。

### 决策树

在深入数学细节之前，让我们回顾一下决策树的基本概念。原理非常简单：将值关联到一组特征，通过遍历二叉树。二叉树的每个节点都附带一个条件；叶子节点包含值。

如果条件为真，我们继续使用左节点进行遍历；如果不成立，我们使用右节点。一旦到达叶子节点，我们就得到了预测结果。

正如常常所说，一图胜千言：

![](../Images/f681ccf5b863618aa3ba615f5f7ed042.png)

三层决策树。图像由作者提供。

附加到节点的条件可以视为一个决策，因此得名**决策树**。

这种结构在计算机科学历史上相当古老，并且在几十年里成功使用过。基本实现如下：

### 堆叠多个树

尽管决策树在许多应用中取得了一些成功，例如专家系统（在 AI 冬季之前），但它仍然是一个非常基础的模型，无法处理现实世界数据中通常遇到的复杂性。

我们通常将这种类型的估算器称为***弱模型***。

为了克服这一局限性，九十年代出现了将多个*弱模型*组合起来以创建一个*强模型：****集成学习****。* 的想法。

这种方法可以应用于任何类型的模型，但由于决策树简单、快速、通用且易于解释，因此它们被广泛使用。

可以部署各种策略来组合模型。例如，我们可以使用每个模型预测的加权和。甚至更好的是，使用贝叶斯方法根据学习进行组合。

XGBoost 和所有**boosting**方法使用另一种方法：每个新模型尝试补偿前一个模型的错误。

### 优化决策树

如上所述，使用决策树进行预测很直接。使用*集成学习*时，工作并不复杂：我们只需对每个模型的贡献进行求和即可。

真正复杂的是构建树本身！我们如何找到在每个训练数据集节点上应用的最佳条件？这就是数学帮助我们的地方。完整的推导可以在[XGBoost 文档](https://xgboost.readthedocs.io/en/latest/tutorials/model.html)中找到。在这里，我们将仅关注本文的感兴趣公式。

和往常一样，在机器学习中，我们希望设置模型参数，以便使模型在训练集上的预测最小化给定的目标：

![](../Images/1c6ea3b60063b729682ac9a687e59dd2.png)

目标公式。公式由作者提供。

请注意，这个目标由两个部分组成：

+   一种用于测量预测错误的指标。这就是著名的损失函数*l(y, y_hat)*。

+   另一个*omega*，用于控制模型复杂性。

如 XGBoost [文档](https://xgboost.readthedocs.io/en/latest/tutorials/model.html)所述，复杂性是目标中非常重要的一部分，它允许我们调整偏差/方差权衡。可以使用许多不同的函数来定义这个正则化项。XGBoost 使用：

![](../Images/0ce54f0ee55300ffbfdaa119f4139291.png)

正则化项。公式由作者提供。

这里* T* 是叶子的总数，而*w_j* 是附加到每个叶子的权重。这意味着较大的权重和较多的叶子会受到惩罚。

由于错误通常是复杂的非线性函数，我们使用二阶泰勒展开对其进行线性化：

![](../Images/30f9a52c0856023e980b2ed196a90f7a.png)

损失的二阶展开。公式由作者提供。

其中：

![](../Images/d15672d7944ba013a374896777a30f45.png)

高斯和 Hessian 公式。公式由作者提供。

线性化是针对预测项计算的，因为我们希望估计预测变化时错误的变化。线性化是至关重要的，因为它将简化错误的最小化。

我们希望通过梯度提升实现的目标是找到最佳的*delta_y_i*，以最小化损失函数，即我们希望找到如何修改现有的树，以使修改改善预测。

在处理树模型时，有两类参数需要考虑：

+   定义树本身的参数：每个节点的条件，树的深度

+   附加到每个叶子的值。这些值就是预测值本身。

探索每个树的配置将过于复杂，因此梯度提升树方法仅考虑将一个节点拆分为两个叶子。这意味着我们必须优化三个参数：

+   拆分值：我们根据什么条件来拆分数据

+   附加到左侧叶子的值

+   附加到右侧叶子的值

在 XGBoost 文档中，关于目标的叶子 *j* 的最佳值由以下公式给出：

![](../Images/30ba452986e0c2f8e110335381fe9963.png)

相对于目标的最优叶子值。作者公式。

其中 *G_j* 是附加到节点 *j* 的训练点的梯度之和，*H_j* 是附加到节点 *j* 的训练点的赫希矩阵之和。

使用此最优参数获得的目标函数的减少量是：

![](../Images/92074bf0fd43651aab5b4918039c0764.png)

使用最优权重时的目标改善。作者公式。

选择正确的拆分值是使用暴力方法完成的：我们计算每个拆分值的改进，并保留最佳的拆分值。

我们现在拥有所有所需的数学信息，可以通过添加新叶子来提升初始树的性能，以符合给定的目标。

在具体实施之前，让我们花些时间理解这些公式的含义。

### 理解梯度提升

让我们深入了解权重是如何计算的，以及 *G_j* 和 *H_i* 的值。由于它们分别是相对于预测的损失函数的梯度和赫希矩阵，我们必须选择一个损失函数。

我们将专注于平方误差，这是一种常用的损失函数，也是 XGBoost 的默认目标：

![](../Images/bd91ae15b1e00e8ed1dd2fc927f73301.png)

平方误差损失函数。作者公式

这是一个相当简单的公式，其梯度是：

![](../Images/9941cb63747e203b1bc9de0f40549606.png)

损失函数的梯度。作者公式。

和赫希矩阵：

![](../Images/4821077093599e50bb41001008817d81.png)

损失函数的赫希矩阵。作者公式。

因此，如果我们记住最大化误差减少的最优权重公式：

![](../Images/30ba452986e0c2f8e110335381fe9963.png)

相对于目标的最优叶子值。作者公式。

我们意识到，最优权重，即我们添加到先前预测中的值，是先前预测与实际值之间的平均误差的相反数（当正则化禁用时，即 *lambda = 0*）。使用平方损失来训练带有梯度提升的决策树仅仅归结为在每个新节点中用平均误差更新预测。

我们还看到 *lambda* 具有预期的效果，即确保权重不会过大，因为权重与 *lambda* 成反比。

### 训练决策树

现在进入简单的部分。假设我们有一个现有的决策树，能够在给定的误差范围内进行预测。我们希望通过分裂一个节点来减少误差并改善附加目标。

实现这一点的算法相当直接：

+   选择一个感兴趣的特征

+   根据所选特征的值对当前节点附加的数据点进行排序

+   选择一个可能的分裂值

+   将此分裂值以下的数据点放入右节点，将以上的数据点放入左节点

+   计算父节点、右节点和左节点的目标减少

+   如果左节点和右节点的目标减少总和大于父节点的目标减少总和，则将分裂值保持为最佳值

+   对每个分裂值进行迭代

+   如果有最佳分裂值，则使用它，并添加两个新节点

+   如果没有分裂改善目标，则不要添加子节点。

生成的代码创建了一个DecisionTree类，该类通过一个目标、一组估计器（即树的数量）和一个最大深度进行配置。

正如承诺的代码少于200行：

训练的核心编码在函数*_find_best_split*中。它本质上遵循上述详细步骤。

请注意，为了支持任何类型的目标，而不必手动计算梯度和Hessian，我们使用自动微分和[jax](https://jax.readthedocs.io/en/latest/index.html)库来自动化计算。

最初，我们从一个只有一个节点的树开始，其叶子节点的值为零。由于我们模仿XGBoost，我们还使用一个基准分数，设置为待预测值的平均值。

此外，请注意在第126行，我们如果达到初始化树时定义的最大深度则停止树的构建。我们还可以使用其他条件，比如每个叶子节点的最小样本数量或新权重的最小值。

另一个非常重要的点是选择用于分裂的特征。这里为了简化起见，特征是随机选择的，但我们本可以使用更智能的策略，例如使用方差最大的特征。

### 结论

在本文中，我们了解了梯度提升如何训练决策树。为了进一步提高我们的理解，我们编写了训练决策树集成和使用它们进行预测所需的最小行数。

深刻理解我们用于机器学习的算法是绝对关键的。这不仅有助于我们构建更好的模型，更重要的是使我们能够根据需求调整这些模型。

例如，在梯度提升的情况下，调整损失函数是提高预测精度的一个绝佳方法。

**简历： [Guillaume Saupin](https://www.linkedin.com/in/guillaume-saupin-5802aa31/)** 是 Verteego 的首席技术官。

[原文](https://towardsdatascience.com/diy-xgboost-library-in-less-than-200-lines-of-python-69b6bf25e7d9)。经许可转载。

**相关内容：**

+   [梯度提升决策树 – 概念解释](/2021/04/gradient-boosted-trees-conceptual-explanation.html)

+   [XGBoost：它是什么，何时使用](/2020/12/xgboost-what-when.html)

+   [众人拾柴火焰高：集成学习的案例](/2019/09/ensemble-learning.html)

* * *

## 我们的 3 大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 支持

* * *

### 更多相关内容

+   [少于 15 行代码的多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [KDnuggets 新闻，7月20日：机器学习算法解释…](https://www.kdnuggets.com/2022/n29.html)

+   [机器学习算法每个不到 1 分钟解释](https://www.kdnuggets.com/2022/07/machine-learning-algorithms-explained-less-1-minute.html)

+   [在不到 6 个月的时间内成为商业智能分析师](https://www.kdnuggets.com/become-a-business-intelligence-analyst-in-less-than-6-months)

+   [使用 Streamlit DIY 自动化机器学习](https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html)

+   [黑色星期五优惠 - 在 DataCamp 上以更低的价格掌握机器学习](https://www.kdnuggets.com/2022/11/datacamp-black-friday-deal-master-machine-learning-less-datacamp.html)
