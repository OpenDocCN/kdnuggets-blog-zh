# 数据科学项目雇主希望看到：如何展示业务影响

> 原文：[https://www.kdnuggets.com/2018/12/data-science-projects-business-impact.html](https://www.kdnuggets.com/2018/12/data-science-projects-business-impact.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：约翰·沙利文，** [**DataOptimal**](https://www.dataoptimal.com/)

找工作并不容易，你需要让自己脱颖而出。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

我喜欢的一种策略是建立展示业务影响的投资组合项目。

预测花卉类型可能对初学者有用，但在现实世界中，你将直接或间接地处理一些针对业务方面的工作。

我将逐步讲解如何构建一个[在R中预测客户流失的模型](https://www.dataoptimal.com/churn-prediction-with-r/)，以展示显著的业务影响。

以下是过程的简要概述：

1.  确定项目范围

1.  准备数据

1.  适配模型

1.  进行预测

1.  展示业务影响

1.  **确定项目范围**

在任何实际的数据科学项目开始时，你需要通过提出一系列[问题](https://towardsdatascience.com/how-to-construct-valuable-data-science-projects-in-the-real-world-203a4f520d54)来开始。

以下是几个好的开始：

+   你试图解决什么问题？

+   你的潜在解决方案是什么？

+   你将如何评估你的模型？

假设你在电信行业工作，并且你可以访问客户数据。你的老板来找你，问道，“我们如何利用现有数据改善业务？”这问题相当模糊，所以这是我如何回答上述问题来制定策略以回答老板问题的方式：

*你试图解决什么问题？*

在查看了你的数据后，你注意到获取新客户的成本是留住现有客户的五倍。现在更具体的问题是，“我们如何提高客户留存率以降低成本？”

*你的潜在解决方案是什么？*

为了提高客户留存率，我们需要识别潜在的不满客户。如果我们能在客户生命周期的早期进行干预，我们可以提供折扣或替代服务，试图阻止不满客户流失。由于我们有客户数据的访问权限，我们可以构建一个机器学习模型，尝试预测可能会流失的不满客户。为了简化，我们将只使用逻辑回归模型。

*你将如何评估你的模型？*

我们将使用一系列机器学习评估指标（ROC，AUC，灵敏度，特异性）以及以业务为导向的指标（成本节约）。

![数据科学项目 图1](../Images/6545c3790ef5a46ff59e7365b649132d.png)

1.  **准备数据**

下一步是准备数据。

这个工作流程会因项目而异，但在我们的示例中，我将使用以下工作流程：

1.  导入数据

1.  快速查看

1.  清理数据

1.  拆分数据

我省略了完整的探索阶段，因为我希望未来专门写一篇文章来讨论它。探索阶段与建模阶段一样重要，甚至更重要。

要查看包含所有步骤的完整代码，请查看[原始帖子](https://www.dataoptimal.com/churn-prediction-with-r/)。这里是R语言中前两步的快速快照：

![数据科学项目 图3](../Images/5f343d179ca6d41f08ea6aa92b343bf7.png)

![数据科学项目 图4](../Images/cc7d80fc7866c91f067279d015343ac7.png)

虽然未显示，但在清理步骤中，我使用中位数填补了缺失值。这是一种简单的方法，但你绝对应该查看更[严格的统计方法](https://github.com/matthewbrems/ODSC-missing-data-may-18/blob/master/Analysis%20with%20Missing%20Data.pdf)。

在最后一步，我将数据拆分为训练集和测试集，分别使用75%和25%的数据。这总是一个好的做法，有助于防止过拟合。

1.  **拟合模型**

为了实现逻辑回归模型，我们将使用[广义线性模型](https://www.statmethods.net/advstats/glm.html)（GLM）函数，即GLM。

有不同类型的[广义线性模型](https://en.wikipedia.org/wiki/Generalized_linear_model)，其中包括逻辑回归。为了指定我们要执行二元逻辑回归，我们将使用参数“family=binomial”。

![数据科学项目 图5](../Images/6a959b99b3287724ef9efe77a24df7d5.png)

1.  **进行预测**

现在我们已经拟合了模型，是时候看看它的表现如何了。

为此，我们将使用“测试”数据集进行预测。我们将传入前一节的“拟合”模型。为了预测概率，我们将指定“type=response”。

![数据科学项目 图6](../Images/e2bde9d67f27f9f2a30cbfc127b9f488.png)

![数据科学项目 图7](../Images/42aaa57f218254a33fdc626d62b621b1.png)

我们将设置响应阈值为0.5，因此如果预测概率高于0.5，我们将把这个响应转换为“是”。

下一步是将字符响应转换为 [因子类型](https://stats.idre.ucla.edu/r/modules/factor-variables/)。这样可以确保编码对于逻辑回归模型是正确的。

![数据科学项目图 8](../Images/6dc0c647fa0c325ac12d3397f518d073.png)

我们稍后将更详细地查看阈值，所以不用担心为什么我们设置为 0.5。

最终步骤是评估我们的模型。

一个有用的工具是混淆矩阵。它显示了每个类别的正确和错误预测的数量。

![数据科学项目图 9](../Images/36f45f9b642cd406c9d577c8c9116bec.png)

![数据科学项目图 10](../Images/466276412516da952a6d0e54de31dfb6.png)

[敏感性](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)（真实正类率）和 [特异性](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)（真实负类率）也是“confusionMatrix”函数报告的有用指标。

![数据科学项目图 11](../Images/50bbadcc14428e51bc274af429eb46d0.png)

另一个有用的指标是 [接收者操作特征](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc)（ROC）曲线下面积，也称为 AUC。

ROC 是一个很好的工具，因为它绘制了真实正类率（TPR）与假正类率（FPR）在阈值变化时的关系。以下是如何使用“ROCR”库来绘制它：

![数据科学项目图 12](../Images/13476ee743f9740b686b7ba974f59931.png)

![数据科学项目图 13](../Images/f1c8f69c80eacdff58ec566e5c638adf.png)

使用此图的一个有用方法是计算曲线下面积，也称为 AUC。AUC 的值范围从 0 到 1，1 为最佳。以下是计算 AUC 的 R 代码：

![数据科学项目图 14](../Images/5666b67f54332d5215e228018da252a9.png)

![数据科学项目图 15](../Images/1f0b4d2e490611a49978079169d34a6d.png)

我们的模型的 AUC 为 0.85，这相当不错。如果我们只是随机猜测，我们的 ROC 将是 45 度的直线。这将对应于 AUC 为 0.5。至少，我们的表现优于随机猜测，所以我们知道我们的模型至少添加了一些价值！

1.  **展示业务影响**

最终步骤是将我们迄今为止所做的一切转化为业务影响。

让我们先对成本做一些假设。我们假设在电信行业获取新客户的成本是 $300。我们之前提到的数据表明，获取新客户的成本是保留现有客户的五倍，所以我们的保留成本将是 $60。

这是这些成本与四种预测类型相关的简要总结：

+   错误负类（预测客户不会流失，但实际上他们会）：$300

+   真正正类（预测客户会流失，并且他们确实会）：$60

+   假正类（预测客户会流失，但实际上他们不会）：$60

+   真阴性（预测客户不会流失，而他们确实不会流失）：$0

如果我们将每种预测类型的数量乘以相关成本并求和，我们得到以下成本公式：

成本 = FN($300) + TP($60) + FP($60) + TN($0)

让我们计算在不同阈值（0.1, 0.2, 0.3,…,0.9, 1.0）下每位客户的成本。在初始化阈值向量“thresh”后，我可以循环遍历每个阈值并进行预测。由于我在按客户计算成本，因此我将按测试集中的总数据点数量进行除法。

![数据科学项目图16](../Images/2dd2375c3478c1532b662e73603c8df9.png)

最后，我将结果放入数据框中，连同我称之为“简单”模型的内容。这是我们之前的逻辑回归模型，默认阈值为0.5。

![数据科学项目图17](../Images/f5c19ee061d9f44cc876d7fe78365202.png) ![数据科学项目图18](../Images/29d33ce4a19f601ae64cb3694a85a8e9.png)

图表显示，客户的最低成本约为$40，阈值为0.2。

假设我们的公司目前使用的是“简单”模型，该模型在0.5的阈值下每位客户的成本约为$48。

如果我们有大约500,000的客户基础，那么从简单模型切换到优化模型每年可节省$4MM的成本！这种成本节省是雇主们非常希望看到的显著业务影响。

**结论**

在求职过程中，脱颖而出的最佳方法之一是构建展示真实业务影响的作品集项目。

如果你能提出聪明的商业问题并像现实世界中的数据科学家一样完成项目，你将立即对雇主更具价值。

欲了解完整的逐步教程和R代码，请查看[原始帖子](https://www.dataoptimal.com/churn-prediction-with-r/)。

一如既往，确保在你的GitHub页面和LinkedIn个人资料上记录你的工作。

保持积极，继续构建项目，你将顺利找到工作。祝你求职顺利！

**个人简介**：**约翰·沙利文**是数据科学学习博客[DataOptimal](https://www.dataoptimal.com/)的创始人。你可以在Twitter上关注他 @DataOptimal。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [公司应对数据科学的3大挑战](https://www.kdnuggets.com/2018/11/mathworks-3-challenges-companies-data-science.html)

+   [我在Kaggle竞赛中排名前2%的秘密秘诀](https://www.kdnuggets.com/2018/11/secret-sauce-top-kaggle-competition.html)

+   [管理者的数据科学入门](https://www.kdnuggets.com/2018/11/intro-data-science-managers.html)

### 更多相关话题

+   [停止学习数据科学以寻找目标，并找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功的数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的AI失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么使Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
