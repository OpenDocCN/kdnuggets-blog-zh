# 2022 年 5 个实用的数据科学项目，帮助你解决实际业务问题

> 原文：[`www.kdnuggets.com/2021/12/5-practical-data-science-projects.html`](https://www.kdnuggets.com/2021/12/5-practical-data-science-projects.html)

评论

![](img/7f6cb49deda58c60b79433484e84f76a.png)

*照片由 [XPS](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/project?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。*

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 事务

* * *

> “告别无用的副项目。”

自从我开始写文章以来已经快两年了——这相当于写了 175 篇文章！我之前的一些文章的一个缺点是，我建议了一些有趣的数据科学项目，但**不够实际**。

成为数据科学家的最简单方法之一是展示你已经完成了类似的项目，并且这些项目与职位描述相符。因此，我想与您分享一些我在职业生涯中亲自完成的**实用**数据科学项目，这些项目将增强你的经验和作品集。

### 1\. 客户倾向建模

**什么？**

倾向模型是预测某人会做某事的模型。举几个例子：

+   网站访问者注册账户的可能性。

+   注册用户支付和订阅的可能性。

+   用户推荐其他用户的可能性。

倾向建模不仅涉及“谁”和“什么”——还涉及“何时”（何时应该针对你所识别的用户）和“如何”（如何向目标用户传达信息？）。

**为什么？**

倾向建模使你可以更明智地分配资源，从而提高效率，同时获得更好的结果。举个例子：与其发送一个用户点击的概率为 0%-100%的电子邮件广告，不如通过倾向建模，针对那些有 50%以上点击概率的用户。减少邮件数量，增加转化率！

**如何：**

下面是两个代码演示，展示了如何构建基本的倾向模型：

+   [客户购买倾向](https://www.kaggle.com/benpowis/customer-propensity-to-purchase)

+   [营销分析、分类和 EDA](https://www.kaggle.com/jalenguzman/marketing-analytics-classification-and-eda#Classification-Algorithms)

这里有两个数据集，你可以用来构建一个倾向模型。请注意每个数据集中提供的特征类型：

+   [客户购买倾向数据集](https://www.kaggle.com/benpowis/customer-propensity-to-purchase-data) - 记录购物者在在线商店互动的数据集

+   [营销活动](https://www.kaggle.com/rodsaldanha/arketing-campaign?select=marketing_campaign.csv) - 提升营销活动的利润

### 2\. 指标预测

**什么？**

指标预测是自解释的——它指的是对给定指标（如收入或总用户数）在短期内进行预测。

具体来说，预测涉及使用历史数据作为输入来生成预测输出。即使输出本身并不完全准确，预测也可以用来衡量特定指标的总体趋势。

**为什么？**

预测基本上就像是窥视未来。通过（以一定的信心水平）预测未来会发生什么，你可以更加主动地做出更明智的决策。结果是，你将有更多时间做出决定，并最终降低失败的可能性。

**如何：**

第一个资源提供了几种时间序列模型的总结：

+   [时间序列预测模型概述](https://towardsdatascience.com/an-overview-of-time-series-forecasting-models-a2fa7a358fcb)

第二个资源提供了使用 Prophet 创建时间序列模型的逐步指南，Prophet 是 Facebook 专为时间序列建模开发的 Python 库：

+   [使用 Prophet 进行时间序列分析和预测](https://www.kaggle.com/elenapetrova/time-series-analysis-and-forecasts-with-prophet)

### 3\. 推荐系统

**什么？**

推荐系统是旨在向用户建议最相关信息的算法，无论是亚马逊上的类似产品，Netflix 上的类似电视节目，还是 Spotify 上的类似歌曲。

推荐系统主要有两种类型：协同过滤和基于内容的过滤。

+   *基于内容的*推荐系统根据之前选择的项目特征推荐特定项目。例如，如果我之前看了很多动作片，它会将其他动作片排名更高。

+   *协同过滤*则是根据类似用户的反应来筛选用户可能喜欢的项目。例如，如果我喜欢歌曲 A，而其他人喜欢歌曲 A *以及*歌曲 C，那么我会被推荐歌曲 C。

**为什么？**

推荐系统是最广泛使用且最实用的数据科学应用之一。不仅如此，它在数据产品中的投资回报率也最高之一。[据估计，亚马逊在 2019 年由于其推荐系统销售额增加了 29%。](https://rejoiner.com/resources/amazon-recommendations-secret-selling-online/#:~:text=%E2%80%9CJudging%20by%20Amazon's%20success,%20the,the%20same%20time%20last%20year.) 同样，[Netflix 宣称其推荐系统在 2016 年价值高达 10 亿美元](https://www.businessinsider.com/netflix-recommendation-engine-worth-1-billion-per-year-2016-6)!

但是什么让它如此有利可图？正如我之前提到的，关键在于一个词：**相关性**。通过向用户提供更多**相关**的产品、节目或歌曲，你最终会增加他们购买更多产品和/或保持更长时间的可能性。

**如何：**

**资源和数据集**

+   [推荐系统简介 - 1：基于内容的过滤和协作过滤](https://towardsdatascience.com/introduction-to-recommender-systems-1-971bd274f421)

+   [Netflix 电影和电视节目](https://www.kaggle.com/shivamb/netflix-shows)

+   [餐馆推荐挑战](https://www.kaggle.com/mrmorj/restaurant-recommendation-challenge?select=orders.csv)

+   [Spotify 推荐](https://www.kaggle.com/bricevergnou/spotify-recommendation?select=yes.py)

### 4\. 深度分析

**什么？**

深度分析只是对特定问题或主题的深入分析。它们可以是*探索性的*，以发现新信息和见解，也可以是*调查性的*，以了解问题的原因。

这不是一个广泛讨论的技能，部分原因是它需要经验，但这并不意味着你不能提高它！像其他任何技能一样，这只是一个练习的问题。

**为什么？**

深度分析对于任何数据相关的专业人士来说都是必不可少的。能够找出为什么某些东西不起作用，或者能够找到灵丹妙药，是区别优秀与一般的关键。

**资源和数据集**

以下是一些你可以自己尝试的深度分析任务：

+   [医疗成本](https://www.kaggle.com/ravichaubey1506/healthcare-cost)

+   [IBM 人力资源分析员工流失与绩效](https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset)

+   [营销分析](https://www.kaggle.com/jackdaoud/marketing-data/tasks?taskId=2986)

### 5\. 客户细分

**什么？**

客户细分是将客户基础划分为几个细分市场的实践。

最常见的细分类型是按人口统计进行，但还有许多其他类型的细分，包括地理、心理、需求和价值细分。

**为什么？**

细分对企业有几个重要的价值：

+   它使你能够进行更有针对性的营销，并向每个细分市场传达更个性化的信息。年轻的青少年和多个孩子的父母看重的东西大相径庭。

+   当资源有限时，它允许你优先考虑特定的细分市场，尤其是那些更具盈利性的细分市场。

+   细分市场还可以作为其他应用的基础，如追加销售和交叉销售。

**如何：**

+   [客户细分](https://www.kaggle.com/fabiendaniel/customer-segmentation)

+   [客户细分（K-Means）| 分析](https://www.kaggle.com/kushal1996/customer-segmentation-k-means-analysis)

**数据集**

+   [购物中心客户细分数据](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python)

+   [客户细分分类](https://www.kaggle.com/kaushiksuresh147/customer-segmentation)

[原文](https://towardsdatascience.com/5-practical-data-science-projects-that-will-help-you-solve-real-business-problems-for-2022-a5a3904ea39b)。已获得许可转载。

**相关**：

+   19 个适合初学者的数据科学项目创意

+   数据科学家：如何推销你的项目和自己

+   计算机视觉中的 10 个 AI 项目创意

### 相关主题

+   [数据科学项目，帮助你解决现实世界问题](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 个预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [想用你的数据技能解决全球问题？这里有一些建议…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [每个数据科学家都应该知道的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
