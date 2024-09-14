# 介绍广义积分梯度（GIG）：解释多样化集成机器学习模型的实用方法

> 原文：[https://www.kdnuggets.com/2020/01/generalized-integrated-gradients-explaining-ensemble-models.html](https://www.kdnuggets.com/2020/01/generalized-integrated-gradients-explaining-ensemble-models.html)

[评论](#comments)

**由 [Jay Budzik](https://www.linkedin.com/in/jaybudzik/)，[Zest AI](https://www.zest.ai/)的首席技术官**

+   *集成机器学习模型提供比单一机器学习模型更高的预测准确性和稳定性，正如我们在之前的文章* [*集成模型更优*](https://docs.google.com/document/d/1IFgr8V_ebJqR6oROm0fNStf_a-ctsVJlPU2_A3jONDw/edit)*中所展示的那样。* 但集成模型使用常见的方法如SHAP和积分梯度难以解释和信任。

+   *Zest开发了一种新的方法来解释复杂的集成模型，称为广义积分梯度，使其在诸如信用风险承保等应用中安全使用。与其他方法不同，GIG直接遵循一小套合理规则，无需任意假设。*

### **为什么需要一种新的信用分配方法**

机器学习已被证明能产生更好的承保结果并减少贷款中的偏见。但并非所有的机器学习技术，包括在无监管使用中广泛应用的那些，都建立在透明的基础上。许多部署的算法生成的结果难以解释。最近，研究人员提出了新颖而强大的方法来解释机器学习模型，特别是 [Shapley 加性解释](https://arxiv.org/abs/1705.07874)（SHAP解释器）和 [积分梯度](https://arxiv.org/abs/1703.01365)（IG）。这些方法提供了分配模型用于生成评分的数据变量的机制。它们通过将机器学习模型表示为一个游戏来工作：每个变量是一个玩家，游戏的规则是模型的评分函数，游戏的价值是模型给出的分数。在 [合作博弈](https://en.wikipedia.org/wiki/Cooperative_game_theory)中，信用分配是一个被很好理解的问题。

[SHAP](https://arxiv.org/pdf/1705.07874.pdf)使用各种方法来计算Shapley值，这些方法在原子游戏中表现良好，通过系统地去除每个变量重新计算模型结果，从而识别出最重要的变量。当算法是神经网络时，它运行的是无穷小的微型游戏，这些游戏单独来看并不重要，但在整体中却有意义，因此你需要[集成梯度](https://arxiv.org/pdf/1703.01365.pdf)来解释模型。IG使用Aumann-Shapley值来理解两个申请人之间模型评分的差异。它量化了每个输入变量对模型评分差异的贡献。毕竟，根据模型的不同，一些变化比其他变化更重要，因此我们的任务是测量模型认为哪些变化更重要或不重要。

这两者在各自领域都是很好的创新，但它们都未能为混合类型模型提供令人满意的信用分配，这些模型通常能取得最佳效果。正如我们在早期文章中所解释的（[为什么不直接使用SHAP](https://www.zest.ai/blog/why-you-shouldnt-just-use-shap-to-power-your-ml-explainability-requirements)），SHAP包中实现的解释器要么要求变量在统计上是独立的，要么要求缺失值可以用平均值替代。这两种情况在金融服务中都是不可行的。IG要求模型在各处都是可微分的，这对决策树并不适用，因此它仅适用于像神经网络这样的模型。

像谷歌、Facebook 和微软这样的科技巨头多年来一直在利用集成机器学习的优势。它们构建了使用决策树、神经网络以及全套建模数学的模型，但它们不受像金融服务公司那样的监管约束。银行和贷款机构将极大受益于将这些同样高级的模型应用于信用审批和欺诈检测等领域。（请查看我们的[近期文章](https://www.zest.ai/blog/many-heads-are-better-than-one-making-the-case-for-ensemble-learning)，介绍了一种新的集成方法——深度堆叠，这种方法为一家小型贷款机构带来了1300万美元的利润，并提供了一个更准确且稳定的模型。）

所有的迹象都表明，需要一种新的方式来解释复杂的集成机器学习模型，以适用于高风险应用，例如信用和贷款。这就是我们发明GIG的原因。

### **介绍广义集成梯度**

[广义积分梯度](https://arxiv.org/abs/1909.01869)（GIG）是Zest AI的新信用分配算法，通过应用[测度理论](https://www.amazon.com/Principles-Mathematical-Analysis-International-Mathematics/dp/007054235X)的工具，克服了Shapley和Aumann-Shapley的局限性。GIG是IG的正式扩展，能够准确分配更广泛模型类别的信用，包括当前机器学习领域几乎所有使用的评分函数。GIG是唯一可以严格计算每个变量在多样化模型集中的贡献的方法。

GIG在解释复杂机器学习模型方面的优势在于它避免了对数据进行不现实和潜在危险的假设。GIG直接遵循其公理。使用SHAP和其他基于Shapley值的方法，你必须将输入变量映射到更高维空间中，以使值适用于机器学习函数。这种映射方式不计其数，尚不清楚哪一种是正确的映射。相比之下，GIG完全由数学决定。

GIG通过直接分析模型函数的各个部分来分配信用，以回答“哪些输入变量导致了模型得分的变化？”这个问题。它通过沿着从第一个输入到另一个输入的路径累积模型得分的变化来衡量每个变量的重要性，使用一种独特的公式计算每个变量导致预测函数得分变化的量。

### **应用于实际的信用风险模型**

为了展示GIG的能力，我们用它来解释一个基于实际借贷数据的混合集成模型。该模型如下面所示，之前已被引用，是一个由4个XGBoost模型和2个神经网络模型组成的堆叠集成模型。

![图示](../Images/8d17f0ae5fe8c4a07c819595af391e92.png)

图1：深度堆叠的集成模型：训练数据用于训练多个子模型，其中一些可能是树模型，如XGBoost，其他则是神经网络，然后将这些模型与输入一起进行集成，形成一个更大的模型，使用神经网络。

在任何模型可解释性任务中的基本要求是准确量化模型中每个特征的重要性。我们进行了一系列实验（在我们的论文中描述），表明GIG准确量化了简单模型中变量的影响。这些实验基于已知的输入数据变化构建了一系列玩具模型，并显示GIG能够准确描绘模型从故意修改的数据中学到的内容。这里，我们查看了实际应用。表1显示了图1中更复杂集成模型的特征重要性。该表显示GIG甚至能够解释这个更复杂的实际模型。

| **特征定义** | **重要性** |
| --- | --- |
| 当前收入来源的比例 | 0.045763 |
| 全职工作的数量 | 0.028215 |
| 雇主的数量 | 0.025663 |
| 当前收入来源的数量 | 0.020645 |
| 申请人是否申请了破产 | 0.020339 |
| 申请人是否有第7章破产记录 | 0.020039 |
| 每月收入来源的数量 | 0.017467 |
| 大多数收入来源是当前的 | 0.016271 |
| 债务与收入比 | 0.015139 |
| 收入来源中来自就业的比例 | 0.011887 |
| 申请人是否被免除破产记录 | 0.010515 |
| 申请人是否有第13章破产记录 | 0.010456 |

表1：图1中显示的集合模型的特征重要性。除了计算整体特征重要性，GIG 还允许你量化每个申请人及不同人群（例如表现最好的贷款和表现最差的贷款，或者导致男女申请人批准率差异的变量）的特征重要性。

### **GIG 的工作原理**

正如我们之前提到的，GIG 是基于 IG 和 [奥曼-沙普利](https://en.wikipedia.org/wiki/Shapley_value#Aumann%E2%80%93Shapley_value) 的差异化信用分配函数。差异化信用分配解答了每个输入变化导致模型评分变化的多少。你比较两位申请人的输入集及模型返回的违约可能性（例如，一位申请人可能被拒绝，另一位申请人可能获得批准）。GIG 显示了每个输入变量对导致批准/拒绝决策的评分变化的贡献。

让我们可视化一下这一切是如何运作的。图2显示了一组绿色的获批申请人和一组红色的被拒绝申请人。被拒绝的申请人有较高的拖欠和破产记录。获批申请人则相反。

![图](../Images/bc4a16eb5a04fedb5736f62784938274.png)

图2。该模型考虑了两个变量：破产和拖欠。获得批准的申请人具有低破产和低拖欠记录。而被拒绝的申请人则有高破产和高拖欠记录。

差异化信用分配函数，如 GIG，通过比较每个获批申请人与每个被拒绝申请人之间的差异来解释两类申请人之间的区别：你基本上是在计算每对获批和被拒绝申请人之间的拖欠和破产差异，并取平均值。该过程在下面的图3中进行了说明。

![图](../Images/9ec95896938ec8634aaab264097aabdd.png)

图3。被拒绝申请人（红色）与获批申请人（绿色）之间的评分差异可以通过拖欠和破产次数的差异来解释。

这仅用于说明，图表中缺少模型分数。图 4 显示了更现实的情况，它展示了模型分数如何随着违约和破产变量的变化而在被拒绝的申请人与批准的申请人之间的路径上变化。IG 方法使用 Aumann-Shapley 值来计算违约和破产对模型分数变化的贡献。

![图](../Images/b7830495c61cebc99e92b4693b465efb.png)

图 4\. 这里模型分数由 *y-*轴表示。违约计数和破产在 *x-* 和 *z-* 轴上表示。沿被拒绝的申请人（红点）和批准的申请人（绿点）之间的路径的部分导数积分表示每个变量、违约 *x* 和破产 *z* 对模型分数 *y* 的 Aumann-Shapley 贡献。

一个模型可以具有任意数量的维度，Aumann-Shapley 可以适应它们。但正如我们之前所说，Aumann-Shapley 值仅适用于像神经网络这样的光滑函数。图 5 说明了 Aumann-Shapley（因此 IG）无法解释的函数组合的简单示例。

![图](../Images/460f1e5d811abd165b8a90ed28e26f7d.png)

图 5\. 函数组成的示例。这里的离散部分是红色的，表示在 x=1.75 处有跳跃不连续性的决策树。连续部分用蓝色表示。这两个函数的组合以绿色显示。组合函数在 x=1.75 处包含跳跃不连续性，就像它的离散部分一样。这些类型的函数不能通过 Shapley 或 Aumann-Shapley 充分解释，需要像 GIG 这样的不同方法。

GIG 首先枚举离散函数中的所有不连续点（例如，通过对模型决策树的深度优先搜索），并计算每个不连续点左右的离散函数值。它根据这两个值的平均值分配信用，如下图 6 的左面板所示。对连续部分分配的信用是使用 IG 计算的（图 6 的右面板）。然后将离散和连续的贡献合并，得到一个高度准确的综合贡献。

![图](../Images/5c05dd23d00b6c78486eed6511c58c49.png)

图 6\. GIG 通过计算跳跃处的信用，方法是平均离散部分左右的值，然后与 Aumann-Shapley 在连续部分分配的信用相结合来工作。

详细信息包含在 [GIG 可解释 ML 论文](https://arxiv.org/abs/1909.01869) 中，该论文还数学证明了 GIG 是在一组合理公理下计算这些混合模型信用分配的唯一方法。

### **结论**

集成模型比任何单一建模方法产生更好的结果，但之前一直难以准确解释。我们介绍了一种新方法，[广义集成梯度](https://arxiv.org/abs/1909.01869)，这是在一小组合理公理下唯一能够解释我们在实际机器学习应用中经常遇到的多样集成和函数组合的方法，例如在金融服务中。GIG使得在你的贷款业务中使用这些先进、更有效的模型成为可能，从而在提供更多良好贷款的同时减少不良贷款，并且能够向消费者、监管者和业务负责人解释结果。

[原文](https://www.zest.ai/insights/introducing-gig-a-practical-method-for-explaining-diverse-ensemble-machine-learning-models)。经许可转载。

**简介：[Jay Budzik](https://www.linkedin.com/in/jaybudzik/)** 是 **[Zest AI](https://www.zest.ai/)** 的首席技术官。

**相关：**

+   [机器学习中的集成方法：AdaBoost](/2019/09/ensemble-methods-machine-learning-adaboost.html)

+   [可解释性：揭开黑箱，第 1 部分](/2019/12/explainability-black-box-part1.html)

+   [众多智慧胜过一人：集成学习的案例](/2019/09/ensemble-learning.html)

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关主题

+   [CoRise 如何帮助 Ben Wilson 找到新的数据分析工程师职位…](https://www.kdnuggets.com/2022/08/corise-land-new-job-analytics-engineer.html)

+   [广义和可扩展的最优稀疏决策树(GOSDT)](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [使用 Pandas 数据框的 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [__getitem__ 的介绍：Python 中的魔法方法](https://www.kdnuggets.com/2023/03/introduction-getitem-magic-method-python.html)

+   [带示例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [集成学习技术：使用 Python 中的随机森林的全面指南](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)
