# 数据科学与商业：学习预期价值框架的3个理由

> 原文：[https://www.kdnuggets.com/2018/07/data-science-business-expected-value-framework.html](https://www.kdnuggets.com/2018/07/data-science-business-expected-value-framework.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[Matt Dancho](https://www.linkedin.com/in/mattdancho/)。**

实施数据科学在商业中的最困难且最关键的部分之一是**量化投资回报率或ROI**。在这篇文章中，我们强调了**学习预期价值框架的三个理由，该框架将机器学习分类模型与ROI连接起来**。此外，我们会指向我们最近发布的新视频，[预期价值框架：使用H2O建模员工流失](https://youtu.be/amGLWN4hmY0)，这是我们旗舰课程的一部分：[数据科学与商业 (DS4B 201)](https://university.business-science.io/p/hr201-using-machine-learning-h2o-lime-to-predict-employee-turnover/?product_id=635023&coupon_code=DS4B_15)。该视频概述了使用H2O减少员工流失并计算ROI的步骤，将关键的H2O功能与过程联系起来。最后，我们将讨论一些与将预期价值应用于商业中的机器学习分类问题相关的**预期价值框架常见问题**。

### **学习预期价值框架的3个理由**

* * *

## 我们的前3名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

如果你想将数据科学与机器学习分类器的ROI联系起来，这里有你需要了解预期价值的3个理由。我们将使用与员工流失（也称为员工离职或员工流失）相关的[DS4B 201课程](https://university.business-science.io/p/hr201-using-machine-learning-h2o-lime-to-predict-employee-turnover/?product_id=635023&coupon_code=DS4B_15)中的示例。

**原因 #1: 分类机器学习算法经常最大化错误的指标**

F1是平衡精度和召回率的阈值（换句话说，它在减少假阳性和假阴性方面寻求一个相对平衡的阈值）。问题在于，在商业中，假阳性（类型1错误）和假阴性（类型2错误）相关的成本*很少*相等。实际上，在许多情况下，假阴性的成本要高得多（可能是3倍或更多！）。

**示例: 员工流失的类型1和类型2错误成本**

我们开发了一个预测算法，该算法发现员工在加班过多时离职的可能性是正常情况的5倍。

![期望值](../Images/7bf139ec477fcfceaa6a51bb379f9957.png)

*从H2O + LIME结果计算预期流失成本*我们开发了一个建议，使用极其强大的H2O分类模型以及LIME来减少加班，并解释结果。像许多算法一样，我们默认通过处理类型1和类型2错误来优化。这会以大致相同的比例错误分类离职员工（类型2错误）和留任员工（类型1错误）。估计减少加班对员工的成本是如果员工离职，生产力损失的30%。然而，**错误减少留任员工的加班成本是类型1错误的30%或3倍，还把它们当作相同处理！** ***商业问题的最佳阈值***几乎总是低于F1阈值。这引出了你需要了解期望值框架的第二个理由。

**理由 #2: 解决方案是最大化期望值**

当我们需要使用商业成本来计算期望值时，我们可以通过迭代计算来找到**最大化商业问题期望利润或节省的最佳阈值**。通过在不同的阈值下迭代计算节省金额，我们可以查看哪个阈值优化了目标方法。

在详细的示例中，我们可以看到，在阈值优化结果中，最大节省（$546K）发生在阈值0.149处，这比在最大F1阈值下的节省（$470K）**节省了16%**。值得一提的是，最大化F1的阈值是0.280，对于包含总人口15%的测试集来说，由于不够优化，导致了$76K的额外成本（$546K - $470K）。**将这种低效扩展到整个数据集（训练数据 + 测试数据），这是每年错失的$500K机会！**

![优化结果](../Images/9e5a1ad59956d2f362432d653c954343.png)

然而，模型基于多个假设，包括平均加班百分比、每位员工的预期净利润等等。

**理由 #3: 期望值可以测试假设中的变异性**

我们可以使用灵敏度分析以及期望值。我们测试模型假设对员工离职期望利润（或节省）的影响。

在下面的人力资源示例中，我们测试了*平均加班百分比*和*每位员工的净收入*的一系列值，因为我们对未来的估计可能存在偏差。在下面显示的**敏感性分析结果**中，我们可以在盈利能力热图中看到，只要平均加班百分比小于或等于25%，实施有针对性的加班政策就能为组织节省资金。

![盈利能力热图](../Images/0b3b0d548e8b2c8f534b89473249aade.png)

*敏感性分析结果（盈利能力热图）*

哇！我们不仅可以测试最大化商业案例的最佳阈值，还可以使用期望值测试每年和每人变化的输入范围。如果你有兴趣了解如何将期望值框架应用于你的业务，我们会展示如何操作，提供代码，并展示在我们在[**数据科学商业课程（DS4B 201课程）**](mailto:https://university.business-science.io/p/hr201-using-machine-learning-h2o-lime-to-predict-employee-turnover/?product_id=635023&coupon_code=DS4B_15)**中的其他行业应用实例。

**简介: [Matt Dancho](https://www.linkedin.com/in/mattdancho/)** 是一位数据驱动的决策者。热衷于学习新工具、开发软件和与人合作以获取见解并做出更好的决策。喜欢与团队和个人合作以推动运营卓越。驻扎在美国宾夕法尼亚州州立大学。

**相关：**

+   [数据科学中的数据使用的4个层次](https://www.kdnuggets.com/2018/07/4-levels-data-usage-data-science.html)

+   [使用R进行H2O深度学习](https://www.kdnuggets.com/2018/01/deep-learning-h2o-using-r.html)

+   [科学债务——它对数据科学意味着什么？](https://www.kdnuggets.com/2018/05/scientific-debt.html)

### 更多相关话题

+   [MLOps 一团糟，但这在意料之中](https://www.kdnuggets.com/2022/03/mlops-mess-expected.html)

+   [5 个你需要合成数据的理由](https://www.kdnuggets.com/2023/02/5-reasons-need-synthetic-data.html)

+   [避免数据科学职业的前5个理由](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)

+   [7 个你为何难以找到数据科学工作的理由](https://www.kdnuggets.com/7-reasons-why-youre-struggling-to-land-a-data-science-job)

+   [机器学习没有为我的业务带来价值。为什么？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)

+   [7 个你不应该成为数据科学家的理由](https://www.kdnuggets.com/7-reasons-why-you-shouldnt-become-a-data-scientist)
