# “请解释一下。” 机器学习模型的可解释性

> 原文：[`www.kdnuggets.com/2019/05/interpretability-machine-learning-models.html`](https://www.kdnuggets.com/2019/05/interpretability-machine-learning-models.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：[Olga Mierzwa-Sulima](https://www.linkedin.com/in/olga-mierzwa-sulima-84843539/)，Appsilon**。

[原文](https://appsilon.com/please-explain-black-box/)。经许可转载。

2019 年 2 月，波兰政府在银行法中新增了一项修正案，赋予客户在遭遇负面信用决策时获得解释的权利。这是实施 GDPR 在欧盟的直接后果之一。这意味着，如果决策过程是自动化的，银行需要能够解释为什么贷款没有被批准。

2018 年 10 月，全球头条报道了一个[亚马逊 AI 招聘工具](https://www.theguardian.com/technology/2018/oct/10/amazon-hiring-ai-gender-bias-recruiting-engine)偏爱男性。亚马逊的模型在有偏的数据上进行训练，这些数据倾向于男性候选人。它建立了惩罚包含“女性”一词的简历的规则。

### **不了解模型预测的后果**

上述两个例子的共同点是，银行业的模型和亚马逊构建的模型都是非常复杂的工具，所谓的黑箱分类器，这些工具没有提供直接且易于人类理解的决策规则。

如果金融机构希望继续使用基于机器学习的解决方案，它们将不得不投资于模型可解释性研究。而且它们很可能会这样做，因为这种算法在预测信用风险方面更为准确。另一方面，亚马逊如果对模型进行适当的验证和理解，可能会节省大量金钱和负面新闻。

### **为什么现在？数据建模的趋势。**

自 2014 年以来，机器学习一直保持在 Gartner 的炒作周期榜单上，2018 年被深度学习（机器学习的一种形式）取代，这表明机器学习的采用尚未达到顶峰。

![](img/57e65fff7ae21083675f67d428b48500.png)

[**来源**](https://www.gartner.com/smarterwithgartner/5-trends-emerge-in-gartner-hype-cycle-for-emerging-technologies-2018/)

预测机器学习的增长将进一步加速。根据[Univa 的报告](http://www.univa.com/resources/univa-machine-learning-survey.php)，预计在接下来的两年内，96%的公司将会在生产中使用机器学习。

造成这种情况的原因有：数据收集的广泛性、庞大的计算资源的可用性和活跃的开源社区。机器学习的采用增长伴随着机器学习可解释性研究的增加，这种研究受到如 GDPR、欧盟的“解释权”、安全（医学、自动驾驶车辆）、可重复性、偏见或最终用户期望（调试模型以改进或了解有关研究主题的新信息）等法规的推动。

![](img/d41678e8798ddf49f1a5a07c61346c62.png)

**[来源](http://people.csail.mit.edu/beenkim/papers/BeenK_FinaleDV_ICML2017_tutorial.pdf)**

### **黑盒算法的可解释性可能性**

作为数据科学家，我们应该能够向最终用户解释模型的工作原理。然而，这并不一定意味着要理解模型的每一个部分或生成一套决策规则。

也有可能存在这种情况：这不是必需的：

+   问题已经被很好地研究，

+   模型结果没有后果，

+   终端用户对模型的理解可能会带来系统被操控的风险。

如果我们查看 2018 年[Kaggle 的机器学习和数据科学调查](https://www.kaggle.com/sudhirnl7/data-science-survey-2018)的结果，大约 60%的受访者认为他们能够解释大多数机器学习模型（虽然一些模型仍然难以解释）。最常用的机器学习理解方法是通过查看特征重要性和特征相关性来分析模型特征。

**特征重要性分析**提供了对模型学习内容和可能重要因素的初步良好见解。然而，如果特征之间存在相关性，这种技术可能不可靠。只有当模型变量是可解释的时，它才会提供良好的见解。对于许多[GBM](https://towardsdatascience.com/boosting-algorithm-gbm-97737c63daa3)库，生成[特征重要性图](https://www.r-bloggers.com/variable-importance-plot-and-variable-selection/)相对容易。

在**深度学习**的情况下，情况要复杂得多。当使用神经网络时，你可以查看权重，因为它们包含关于输入的信息，但这些信息是压缩的。而且，你只能分析第一层的连接，因为在更深层次上太复杂了。

难怪在 2016 年，[**LIME**（局部可解释模型-可解释性解释）](https://arxiv.org/abs/1602.04938)论文在 NIPS 会议上发布时产生了巨大影响。LIME 的理念是通过在可解释的输入数据上构建一个更易理解的白盒模型，来局部逼近一个黑盒模型。它在[图像分类的解释](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime)和[文本](https://christophm.github.io/interpretable-ml-book/lime.html#lime-for-text)方面取得了良好的结果。然而，对于表格数据来说，找到可解释的特征非常困难，而且它们的局部解释可能会产生误导。

LIME 在 Python 中实现（[lime](https://github.com/marcotcr/lime)和[Skater](https://github.com/datascienceinc/Skater)）以及 R 中（[lime 包](https://cran.r-project.org/web/packages/lime/index.html)和[iml 包](https://cran.r-project.org/web/packages/iml/index.html)，[live 包](https://cloud.r-project.org/web/packages/live/index.html)），使用起来非常方便。

另一个有前景的想法是 [SHAP (Shapley Additive Explanations)](https://arxiv.org/abs/1705.07874)。它基于博弈论。它假设特征是参与者，模型是联盟，而 Shapley 值则告诉我们如何公平地分配“收益”。这种技术公平地分配影响，使用简单，并提供了视觉上令人信服的实现。

[**DALEX** 包](https://github.com/pbiecek/DALEX)（描述性机器学习解释）在 R 中提供了一套工具，帮助理解复杂模型的工作原理。使用 DALEX，你可以创建模型解释器并进行视觉检查，例如分解图。你可能还会对 [DrWhy.Ai](https://github.com/ModelOriented/DrWhy/blob/master/README.md) 感兴趣，它是由与 DALEX 相同的研究团队开发的。

### **实际应用案例**

#### **检测图片中的对象**

**图像识别** 已经被广泛使用，例如在自动驾驶汽车中检测图片中的汽车、交通灯等，在野生动物保护中检测图片中的特定动物，或在保险中检测作物的洪水情况。

我们将使用原始 LIME 论文中的“哈士奇与狼的例子”来说明模型解释的重要性。分类任务是识别图片中是否有狼。它错误地将西伯利亚哈士奇误判为狼。多亏了 LIME，研究人员能够确定图片中哪些区域对模型的重要性。结果发现，如果图片中有雪，它会被分类为狼。

![](img/9f4faf0e9878b6d177fc2001ead3de57.png)

该算法使用了图片的背景，完全忽略了动物的特征。模型应该关注动物的眼睛。得益于这一发现，模型得以修正，并扩展了训练样本，以防止推理中的雪=狼问题。

### **分类作为决策支持系统**

阿姆斯特丹 UMC 的重症监护病房 [希望预测患者出院时的再入院和/或死亡概率](https://medium.com/@Pacmedhealth/ai-for-health-care-tackling-the-issue-of-interpretability-868be42aaf50)。目标是帮助医生选择合适的时机将患者从 ICU 转移。如果医生理解模型的工作原理，他们更可能在做出最终判断时使用其建议。

为了演示如何使用 LIME 解释模型，我们可以查看 [另一项研究](https://www.researchgate.net/publication/309551203_Machine_Learning_Model_Interpretability_for_Precision_Medicine) 的例子，该研究旨在进行 ICU 中的早期死亡预测。使用随机森林模型（黑箱模型）来预测死亡状态，并使用 lime 包来局部解释每位患者的预测分数。

![](img/f78b78b2133323e495c6249e5d6381b4.png)

[**来源**](https://www.researchgate.net/publication/309551203_Machine_Learning_Model_Interpretability_for_Precision_Medicine)

选定示例中的一位患者死亡概率很高（78%）。模型特征中对死亡率有贡献的因素是较高的心房颤动次数和较高的乳酸水平，这与当前医学理解一致。

### **人类与机器——完美的匹配**

为了成功构建可解释的 AI，我们需要结合数据科学知识、算法和终端用户的专业知识。数据科学的工作并不会在创建模型后结束。这是一个迭代的、通常较长的过程，需要专家提供反馈，以确保结果是稳健的并且易于人类理解。

我们坚信，通过结合人类的专业知识和机器的性能，我们可以得出最佳结论：提升机器结果，克服人类直觉偏差。

**简历**： [Olga Mierzwa-Sulima](https://www.linkedin.com/in/olga-mierzwa-sulima-84843539/) 是 Appsilon 的高级数据科学家和项目负责人。

**资源：**

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [BERT 特征是否可以互换？](https://www.kdnuggets.com/2019/02/bert-features-interbertible.html)

+   [2018 年人工智能和数据科学进展及 2019 年趋势](https://www.kdnuggets.com/2019/02/ai-data-science-advances-trends.html)

+   [2018 年 AI/机器学习进展：Xavier Amatriain 总结](https://www.kdnuggets.com/2019/01/xamat-ai-machine-learning-roundup.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [用 LIME 解释 NLP 模型](https://www.kdnuggets.com/2022/01/explain-nlp-models-lime.html)

+   [使用 SHAP 值进行机器学习模型解释性](https://www.kdnuggets.com/2023/08/shap-values-model-interpretability-machine-learning.html)

+   [使用 Python 和 Scikit-learn 简化决策树解释](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [SHAP：在 Python 中解释任何机器学习模型](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

+   [为什么机器学习模型在沉默中消亡？](https://www.kdnuggets.com/2022/01/machine-learning-models-die-silence.html)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)
