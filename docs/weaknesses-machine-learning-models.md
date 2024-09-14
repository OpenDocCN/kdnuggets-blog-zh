# 如何发现机器学习模型中的弱点

> 原文：[https://www.kdnuggets.com/2021/09/weaknesses-machine-learning-models.html](https://www.kdnuggets.com/2021/09/weaknesses-machine-learning-models.html)

[评论](#comments)

**由 [Michael Berk](https://www.linkedin.com/in/michael-berk-48783a146/), Tubi 数据科学家**

每当你使用总结统计来简化数据时，你都会丢失信息。模型准确性也不例外。当简化模型的拟合到总结统计时，你失去了确定性能最低/最高以及原因的能力。

![机器学习准确性表现 深度学习模型漂移偏差 FreaAI Frea ai IBM 数据切片](../Images/f4d29e3365fe6c2bbed992bac867cd27.png)

图 1：模型表现差的数据区域示例。图片由作者提供。

为了解决这个问题，IBM 的研究人员最近开发了一种名为 [FreaAI](https://arxiv.org/pdf/2108.05620.pdf) 的方法，它识别出模型准确率较差的可解释数据切片。工程师可以根据这些切片采取必要措施，以确保模型按预期运行。

FreaAI 不幸的是不是开源的，但许多概念可以很容易地在你喜欢的技术栈中实现。让我们深入了解一下……

## 技术总结

FreaAI 在测试数据中找到统计上显著低性能的切片。它们被返回给工程师进行检查。方法步骤如下：

1.  **使用最高先验密度 (HPD) 方法找到准确率低的单变量数据切片。** 这些单变量数据切片减少了搜索空间，显示了数据中更可能存在问题的地方。

1.  **使用决策树找到准确率低的双变量数据切片。** 这些双变量数据切片减少了分类预测变量和二阶交互的搜索空间，显示了数据中更可能存在问题的地方。

1.  **移除所有不符合特定启发式规则的数据切片。** 主要的两个规则是测试集的最小支持度和统计上显著的误差增加。

## 但是，实际上发生了什么？

那些都是术语，我们来慢慢了解一下实际情况……

## 1\. 问题

在开发模型时，我们经常使用“准确性”指标来判断模型的拟合度。例如均方误差，这在图 2 中定义并用于线性回归。

![机器学习准确性表现 深度学习模型漂移偏差 FreaAI Frea ai IBM 数据切片](../Images/e8f1354a8ec32a3f741874c6a5b525cf.png)

图 2：均方误差公式。图片由作者提供 — [源](https://www.probabilitycourse.com/chapter9/9_1_5_mean_squared_error_MSE.php)。

但是，这个平均误差只告诉我们 **平均** 来说表现如何。我们不知道模型在数据的某些区域表现很好还是在其他区域表现很差。

这是一个长期存在的预测建模问题，最近引起了很多关注。

## 2\. 解决方案

一个解决方案是 FreaAI。该方法在 IBM 开发，旨在确定我们数据中模型表现不佳的地方。

主要有两个步骤。第一步是创建数据切片，第二步是确定模型在这些数据切片中的表现是否不佳。FreaAI 的输出是一组“位置”，这些位置的数据中模型表现不佳。

### **2.1\. 数据切片**

组合测试 (CT) 是一个框架，它依次查看所有预测变量组合以找到表现不佳的区域。例如，如果我们有两个类别预测变量，颜色和形状，我们会查看所有可能的组合，并查看准确性下降的地方。

然而，利用组合测试在大型数据集上是计算上不可行的 — 每增加一列，我们会看到所需组合数量的指数增长。因此，我们需要定义一种方法来帮助我们搜索特征，以找到潜在的不准确区域。

![机器学习准确性表现深度学习模型漂移偏差 FreaAI Frea ai IBM 数据切片](../Images/94461909e59d19dc5df2267481024fb4.png)

图 3：50% 最高密度区域 (HDR) 的蓝色示例。图像由作者提供 — [源](https://mathematica.stackexchange.com/questions/173282/computing-credible-region-highest-posterior-density-from-empirical-distributio)。

FreaAI 使用的第一种方法利用了叫做 [最高密度区域 (HDR)](https://stats.stackexchange.com/questions/148439/what-is-a-highest-density-region-hdr) 的技术（见图 3）。简而言之，HDR 找出数值特征中数据占比最高的最小区域，即高密度区域。在图 3 中，区域由水平蓝色虚线区分 — 我们的数据中有 50% 在该线之上。

从那里我们通过一个 ε 值（默认 0.05）逐步减少这个范围，并观察准确性的提高。**如果准确性在给定的迭代中提高，我们知道模型在前一次迭代和当前迭代之间的区域表现不佳。**

要确定数值预测变量的不良拟合区域，我们会对测试集中的所有预测变量迭代运行 HDR 方法。

相当酷，对吧？

第二种方法利用决策树来处理所有非数值预测变量以及两个特征的组合。简而言之，我们拟合一个决策树，并寻找使准确性最小化的特征切分。

![机器学习准确性表现深度学习模型漂移偏差 FreaAI Frea ai IBM 数据切片](../Images/9fbe22ea608bbee93f4f618bc932facf.png)

图 4：关于连续单变量预测变量“年龄”的决策树示例。图像由作者提供。

在图 4 中，每个*决策节点*（蓝色）是对特征的拆分，每个*终端节点*（数字）是该拆分的准确度。通过拟合这些树，我们可以真正缩小搜索空间，更快地找到表现不佳的区域。**此外，由于树对许多不同类型的数据都非常强健，我们可以在分类预测变量或多个预测变量上运行，以捕捉交互效应。**

这种决策树方法会重复用于所有特征组合，以及那些不是数字的单一特征。

### 2.2. 数据切片的启发式方法

到目前为止，我们只关注了通过准确度开发数据切片，但还有其他启发式方法可以帮助我们找到**有用的**数据切片：

1.  **统计显著性**：为了确保我们只关注准确度有显著下降的数据切片，我们仅保留性能低于误差置信区间下界 4% 的切片。通过这样做，我们可以以*α* 的概率断言我们的数据切片具有更高的误差。

1.  **可解释性**：我们还希望确保发现的问题区域可以付诸行动，因此我们在创建组合时仅查看两个或三个特征。通过限制低阶交互作用，我们的工程师更有可能开发解决方案。

1.  **最小支持**：最后，数据切片必须有足够的误差值得调查。我们要求至少有 2 次分类错误，或覆盖 5% 的测试误差 — 取较大的值作为标准。

还值得注意的是，你可以根据业务需求调整其他启发式方法。错误分类某些用户比其他用户更严重，因此你可以将其纳入数据切片标准中 — 参考 [精确度/召回率权衡](https://towardsdatascience.com/accuracy-precision-recall-or-f1-331fb37c5cb9)。

## 3. 总结和要点

所以，你看到了 — FreaAI 的全部荣耀。

再次说明，FreaAI 不是开源的，但希望将来能向公众发布。在此期间，你可以将我们讨论的框架应用到你自己的预测模型中，确定系统性表现不佳的地方。

### 3.1. 总结

总结一下，FreeAI 使用 HDR 和决策树来缩小我们预测变量的搜索空间。然后，它迭代地查看单个特征及其组合，以确定性能较低的地方。这些低性能区域具有一些额外的启发式方法，以确保发现具有可操作性。

### 3.2. 你为什么要关心？

首先，这个框架帮助工程师识别模型的弱点。当这些弱点被发现时，它们可以（希望）得到纠正，从而改善预测。对于黑箱模型（如神经网络），这种改进尤其诱人，因为没有模型系数。

**通过隔离表现不佳的数据区域，我们可以窥视黑箱。**

FreaAI 还具有其他应用的有趣潜力。一个例子是识别模型漂移，即训练后的模型随着时间推移变得越来越无效。IBM 刚刚发布了一个 [用于确定模型漂移的假设检验框架](https://arxiv.org/pdf/2108.05319.pdf)。

另一个有趣的应用是确定模型偏差。在这种情况下，偏差是指不公平的概念，例如基于性别拒绝向某人提供贷款。通过查看模型性能较低的不同数据拆分，你可以发现偏差的领域。

*感谢阅读！我将撰写 37 篇帖子，将学术研究引入数据科学行业。请查看我的评论，获取本文主要来源的链接以及一些有用的资源。*

**简介： [迈克尔·伯克](https://www.linkedin.com/in/michael-berk-48783a146/)** (**[https://michaeldberk.com/](https://michaeldberk.com/)**) 是 Tubi 的数据科学家。

[原文](https://towardsdatascience.com/how-to-find-weaknesses-in-your-machine-learning-models-ae8bd18880a3)。经许可转载。

**相关：**

+   [反脆弱性与机器学习](/2021/09/antifragility-machine-learning.html)

+   [与 Github Actions、Iterative.ai、Label Studio 和 NBDEV 的 MLOps 探险](/2021/09/adventures-mlops-github-actions-iterative-ai-label-studio-and-nbdev.html)

+   [数学 2.0：机器学习的基础重要性](/2021/09/math-fundamental-importance-machine-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并寻找目标以...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成为出色数据科学家所需的 5 种关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [学习数据科学的统计学顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
