# 理解集成学习技术

> 原文：[https://www.kdnuggets.com/2020/03/making-sense-ensemble-learning-techniques.html](https://www.kdnuggets.com/2020/03/making-sense-ensemble-learning-techniques.html)

[评论](#comments)

**作者：[Ido Zehori](https://www.linkedin.com/in/ido-zehori/)，[Bigabid](https://www.bigabid.com/)的数据科学团队负责人**

![图示](../Images/733e44c46833a8497ef2f1341a1d187c.png)

图片来源：[Packt](https://hub.packtpub.com/what-is-ensemble-learning/)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

对于许多专注于机器学习（ML）的公司/数据科学家来说，集成学习方法已经成为首选工具。由于集成学习方法结合了多个基础模型，因此它们能够生成更为准确的ML模型。例如，在Bigabid，我们通过集成学习成功解决了从优化LTV（[客户终身价值](https://www.qualtrics.com/experience-management/customer/customer-lifetime-value/)）到[欺诈检测](https://searchsecurity.techtarget.com/definition/fraud-detection)的各种问题。

难以过分强调集成学习在整体ML过程中的重要性，包括[偏差-方差权衡](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)以及三种主要的集成技术：装袋、提升和堆叠。这些强大的技术应成为任何数据科学家工具包的一部分，因为它们是普遍存在的概念。此外，理解它们的基本机制是机器学习领域的核心。

### 集成学习方法概述

集成学习是一种ML范式，其中多个基础模型（通常被称为“弱学习者”）被组合和训练以解决同一问题。这种方法基于这样的理论：通过正确地组合多个基础模型，这些弱学习者可以作为设计更复杂ML模型的构建块。这些集成模型（恰如其分地称为“强学习者”）能够实现更好、更准确的结果。

换句话说，弱学习者只是一个单独表现较差的基础模型。实际上，它的准确性水平仅略高于偶然情况，这意味着它的预测结果仅比随机猜测稍好。这些弱学习者通常也会很简单。通常，基础模型之所以自身表现不佳，是因为它们要么有高偏差，要么方差过大，使它们变得薄弱。

这就是集成学习的作用。该方法试图通过将多个弱学习者组合在一起，来减少总体误差。可以将集成模型视为“两个脑袋总比一个好”的数据科学版本。如果一个模型表现良好，那么多个模型共同工作可以做得更好。

### 关于偏差-方差权衡的一点说明

理解弱学习者的概念以及为什么它会得到这个名字非常重要，因为其原因归结为偏差或方差。更具体地说，机器学习模型的预测错误，即训练模型与真实值之间的差异，可以分解为以下两部分：偏差和方差。例如：

+   **偏差导致的错误：** 这是模型的预期预测与我们希望预测的精确值之间的差异。

+   **方差导致的错误：** 这是模型对特定数据点预测的方差。

如果一个模型过于简单且没有很多参数，那么它可能有高偏差和低方差。相比之下，如果一个模型有很多参数，那么它可能有高方差和低偏差。因此，有必要找到适当的平衡，避免对数据的欠拟合或过拟合，因为这种复杂性权衡是存在偏差和方差之间权衡的原因。简单来说，一个算法不能同时变得更复杂和更简单。

### 集成学习技术 - 结合弱学习者

集成学习有三种主要技术：自助法、提升法和堆叠法。它们的定义如下：

**自助法（Bagging）：**

自助法试图在小样本群体上结合相似的学习者，并计算所有预测的平均值。通常，自助法允许你在不同的样本群体中使用不同的学习者。通过这样做，这种方法有助于减少方差错误。

**提升法（Boosting）**

提升是一种迭代方法，它根据最新的分类结果微调观察的权重。如果一个观察被错误分类，该方法将在下一轮（即训练下一个模型的轮次）增加该观察的权重，并减少误分类的倾向。类似地，如果一个观察被正确分类，那么它将在下一个分类器中减少其权重。权重表示特定数据点正确分类的重要性，这使得顺序分类器可以集中关注之前被误分类的例子。通常，提升方法能减少偏差错误，形成强大的预测模型，但有时可能会在训练数据上过拟合。

**堆叠**

堆叠是一种巧妙的方法，通过组合不同模型提供的信息。使用这种方法，可以将任何类型的学习者用于结合不同学习者的输出。结果可能是通过使用哪些组合模型来减少偏差或方差。

### **集成学习的承诺**

集成学习涉及将多个基础模型结合，以实现更有效、更准确的集成模型，这种模型具有更强大的特性，因此表现更佳。集成学习方法在挑战性数据集上成功创下了记录，并且经常成为 [Kaggle 竞赛](https://www.kaggle.com/competitions)获胜提交的一部分。

值得注意的是，即使是三种主要的集成技术——装袋、提升和堆叠——也可以有变体，这些变体可以更好地设计以更有效地适应特定问题，如分类、回归、时间序列分析等。这首先需要了解当前的问题，并在解决问题时保持创造性！

使用集成学习方法是一种很好的、有前景的解决问题的方法。

**简介：[Ido Zehori](https://www.linkedin.com/in/ido-zehori/)** 是 [Bigabid](https://www.bigabid.com/) 的数据科学团队负责人，该公司开发了针对应用内广告用户获取和重新参与优化的第二代 DSP，提供了一级 DSP 的规模和尖端 DMP 的精准度。

**相关内容：**

+   [集成学习方法：AdaBoost](/2019/09/ensemble-methods-machine-learning-adaboost.html)

+   [随机森林® — 强大的集成学习算法](/2020/01/random-forest-powerful-ensemble-learning-algorithm.html)

+   [解释黑箱模型：使用 LIME 和 SHAP 的集成学习和深度学习](/2020/01/explaining-black-box-models-ensemble-deep-learning-lime-shap.html)

### 更多相关主题

+   [集成学习技术：Python 中随机森林的详细介绍](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [何时使用集成技术是一个好选择？](https://www.kdnuggets.com/2022/07/would-ensemble-techniques-good-choice.html)

+   [带有示例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [数据质量在成功机器学习模型中的重要性](https://www.kdnuggets.com/2022/03/significance-data-quality-making-successful-machine-learning-model.html)

+   [Ploomber与Kubeflow：让MLOps变得更简单](https://www.kdnuggets.com/2022/02/ploomber-kubeflow-mlops-easier.html)

+   [让智能文档处理更智能：第1部分](https://www.kdnuggets.com/2023/02/making-intelligent-document-processing-smarter-part-1.html)
