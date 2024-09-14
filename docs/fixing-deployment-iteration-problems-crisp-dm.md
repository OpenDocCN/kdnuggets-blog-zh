# 修复CRISP-DM中的部署和迭代问题

> 原文：[https://www.kdnuggets.com/2017/02/fixing-deployment-iteration-problems-crisp-dm.html](https://www.kdnuggets.com/2017/02/fixing-deployment-iteration-problems-crisp-dm.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

这是三篇文章中的最后一篇。第一篇概述了 [CRISP-DM 使用中的四个问题及解决方法](/2017/01/four-problems-crisp-dm-fix.html)，第二篇讨论了 [为CRISP-DM带来业务清晰度](/2017/01/business-clarity-crisp-dm.html)。这篇文章讨论了分析工作完成后分析如何被部署和管理的两个相关问题。关注业务决策过程有助于解决这两个问题。在CRISP-DM中，这涉及到部署阶段和迭代循环的变更，如图1所示。

![](../Images/729c9cdb5b27a1e5bc27d43814e6f2c6.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

**图1：CRISP-DM中的部署和管理活动。**

令人沮丧的是，许多本来可以帮助业务的分析模型实际上并未发挥作用。许多分析模型被开发出来但并未在合理的时间框架内（或者根本没有）被部署和使用。正如最近国际分析学院网络研讨会的调查结果所示，60%的项目未能在“几个月”内对其分析结果采取行动。其他调查和研究结果表明，对于许多这些分析，几个月变成了几年，而且许多分析根本没有被部署。希望开发有影响力的分析的团队需要将将其分析部署到生产环境中，并将其与业务环境集成，作为项目中的关键步骤。这就是CRISP-DM中的部署阶段的作用。成功部署涉及多个因素。

团队必须选择合适的分析交付选项。分析的风格（例如是视觉的还是数值的，是固定的还是交互的）必须与意图的决策风格匹配。嵌入在网站中的自动决策需要不同于由呼叫中心代表做出的手动决策。必须用于移动工作人员的分析与支持桌面工作人员的分析有所不同，依此类推。决策模型揭示了组织的影响，并展示了谁的行为需要改变，以及系统或流程需要围绕分析决策进行重新编码。

分析团队还必须记住，决策需要将分析模型与决策逻辑结合起来，以使其可操作。决策模型展示了如何做到这一点，建模决策的非分析元素，以及展示分析如何被决策所使用。例如，处理索赔的决策模型可能包括欺诈预测，以及审计、法规和基于政策的排除。

最后，还有项目的组织变革部分。改变人们做决策的方式，或改变系统为他们做决策的程度，几乎总是复杂的。决策模型提供了一个框架，用于理解谁在何时何地做出哪些决策。它还将这种决策过程与新的和现有的业务指标联系起来。这提供了对必要组织变革的清晰画面。

*成功的分析团队不会认为项目完成，直到业务看到改善——生产分析只是一个中间点，而非最终目标。*

### 解决方案：规划分析演变

政策和法规会发生变化。市场和竞争者会做出反应。今天的好决策可能明天就不再适用。随着数据变化，分析也会过时，必须监控并保持最新。一个已部署却未监控和未更改的分析模型，可能会变得效果下降，甚至可能变得有害。

分析团队需要识别可能导致分析模型效果下降的业务和环境因素。他们必须定义需要捕获的数据，以显示分析是否有效，并确定何时因这些原因效果下降。他们还应考虑机器学习和自适应分析技术是否适合不断完善模型，以及业务可能对这些模型要求什么样的边界或警报条件。如果需要手动更新，那么这些更新的时间表或触发条件也需要定义。应该制定清晰的计划来逐步改进每一个已部署的分析模型。

决策模型展示了决策的结构。这一结构可以用于确定应跟踪哪些中间结果和业务成果。结合有关模型及其行为的数据，这允许对分析模型及其影响的业务决策进行持续监控和改进。

决策模型还清楚地显示了哪些外部业务影响因素对决策制定至关重要，以便可以监测这些因素的变化。例如，新的或变更的法规可能会影响决策的约束条件，从而削弱分析的有效性，除非进行相应的调整。

*成功的分析团队专注于不断发展的大规模分析组合，而不是个别手工制作的一次性模型*

要了解一家全球信息技术领袖如何使用决策建模，请查看这份[来自国际分析研究所的领先实践简报](http://www.decisionmanagementsolutions.com/bringing-clarity-to-data-science-projects-with-decision-modeling-a-case-study/)。

**简介：[詹姆斯·泰勒](http://www.decisionmanagementsolutions.com/about)** 是如何使用分析技术构建决策管理系统的领先专家，这些系统帮助公司改善决策制定并发展具有敏捷性、分析性和适应性的业务。他为各类公司提供战略咨询，与各个行业的客户合作，推动决策建模、分析及其他决策技术的应用。

**相关：**

+   [基于标准的预测分析部署](/2016/06/zementis-standards-based-deployment-predictive-analytics.html)

+   [为 CRISP-DM 带来业务清晰度](/2017/01/business-clarity-crisp-dm.html)

+   [使用 CRISP-DM 的四个问题及解决方法](/2017/01/four-problems-crisp-dm-fix.html)

### 更多相关主题

+   [数据协作失败的地方（以及修复的4个技巧）](https://www.kdnuggets.com/2023/01/collaboration-fails-around-data-4-tips-fixing.html)

+   [理解 Python 的迭代和成员资格：__contains__ 和 __iter__ 魔法方法指南](https://www.kdnuggets.com/understanding-pythons-iteration-and-membership-a-guide-to-__contains__-and-__iter__-magic-methods)

+   [回到基础 第4周：高级主题和部署](https://www.kdnuggets.com/back-to-basics-week-4-advanced-topics-and-deployment)

+   [7 大模型部署和服务工具](https://www.kdnuggets.com/top-7-model-deployment-and-serving-tools)

+   [机器学习算法的完整端到端部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [从数据收集到模型部署：数据科学项目的6个阶段](https://www.kdnuggets.com/2023/01/data-collection-model-deployment-6-stages-data-science-project.html)
