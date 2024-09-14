# 古德哈特法则在数据科学中的应用及当衡量标准变成目标时会发生什么？

> 原文：[https://www.kdnuggets.com/2020/10/goodharts-law-data-science-measure-target.html](https://www.kdnuggets.com/2020/10/goodharts-law-data-science-measure-target.html)

[评论](#comments)

**作者 [Jamil Mirabito](https://www.linkedin.com/in/jamil-mirabito-62b328aa/)，芝加哥大学与纽约Flatiron School**。

在2002年，布什总统签署了[《无童留后法案》(No Child Left Behind)](https://www.edweek.org/ew/section/multimedia/no-child-left-behind-overview-definition-summary.html)（NCLB），这是一个教育政策，规定所有接受公共资金的学校必须对学生进行年度标准化评估。该法律的一个条款要求学校在年度标准化评估中取得足够的年度进步（AYP），即第三年级学生在当前年度的评估中表现必须优于前一年级的第三年级学生。如果学校持续无法满足AYP要求，将会面临严厉的后果，包括学校重组和关闭。因此，许多学区管理员制定了内部政策，要求教师提高学生的考试成绩，并将这些成绩作为教师质量的衡量标准。最终，面对职业压力，教师们开始“[以考试为导向教学](https://www.chalk.com/resources/teaching-to-the-test-vs-testing-what-you-teach-mastery-based-evaluations/)”。实际上，这种政策无意中*激励*了作弊，以便教师和整个学校系统能够维持必要的资金。其中一个最著名的作弊案件是[亚特兰大公立学校作弊丑闻](https://www.ajc.com/news/timeline-how-the-atlanta-school-cheating-scandal-unfolded/jn4vTk7GZUQoQRJTVR7UHK/)。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

这种无意的后果实际上非常普遍。英国经济学家查尔斯·古德哈特曾说：“当一个衡量标准变成了目标，它就不再是一个好的衡量标准。”这一声明，被称为*古德哈特法则*，实际上可以应用于超越社会政策和经济学的许多现实世界情境。

![](../Images/043dbcbf51821e9fe74b4c5b9888076c.png)

*来源：Jono Hey， [Sketchplanations](https://www.sketchplanations.com/post/167369765942/goodharts-law-when-a-measure-becomes-a-target) (CC BY-NC 3.0)。*

另一个常被引用的例子是，一个呼叫中心经理设定了一个目标，即每天增加接到的电话数量。最终，呼叫中心的员工们在实际客户满意度降低的情况下提高了他们的接电话数量。在观察员工的通话时，经理发现一些员工急于结束通话，而没有确保客户完全满意。这个例子，以及《不让一个孩子掉队》政策的问责措施，强调了**目标可能被操控**这一Goodhart定律最重要的元素之一。

![](../Images/d0d994e14fee9c711971714b11553919.png)

*来源：Szabo Viktor， [Unsplash](https://unsplash.com/photos/UfseYCHvIH0)。*

当考虑到AI和机器学习模型可能容易受到操控和/或入侵时，操控的威胁更大。对84,695个YouTube视频的[2019年分析](https://twitter.com/gchaslot/status/1121603851675553793?s=20)发现，*今日俄罗斯*（一个国有媒体机构）的一段视频被200多个频道推荐，远远超过了其他视频在YouTube上的平均推荐数量。分析结果暗示，俄罗斯以某种方式操控了YouTube的算法，以传播虚假信息。由于平台依赖观看量作为用户满意度的指标，这个问题被进一步加剧。这导致了[*激励阴谋论*](https://www.kdnuggets.com/2019/10/problem-metrics-big-problem-ai.html)，即关于主要媒体机构不可靠和不诚实的阴谋论，以便用户继续从YouTube获取信息。

> *“我们面临的问题是，是否应该因为它能增加人们在网站上停留的时间——并且确实有效——而在大规模上引导人们走向充满虚假信息和谎言的仇恨陷阱。” — [Zeynep Tufekci](https://www.theguardian.com/technology/2018/feb/02/how-youtubes-algorithm-distorts-truth)*

### 那么我们能做些什么呢？

在这方面，重要的是要批判性地思考如何有效地衡量和实现期望的结果，以最小化意外后果。这在很大程度上意味着不要过度依赖单一指标。相反，了解*变量组合*如何影响目标变量或结果，可能有助于更好地 contextualize 数据。 [Chris Wiggins](https://www.datascience.columbia.edu/chris-h-wiggins)，《纽约时报》的首席数据科学家，提供了 [四个有用的步骤](https://www.datascience.columbia.edu/ethical-principles-okrs-and-kpis-what-youtube-and-facebook-could-learn-tukey#fnref5) 来创建道德的计算机算法以避免有害结果：

1.  *首先定义你的原则。我建议[[特别是五项](https://www.datascience.columbia.edu/ethical-principles-okrs-and-kpis-what-youtube-and-facebook-could-learn-tukey#fnref5)]，这些原则受到[贝尔蒙特](https://www.hhs.gov/ohrp/regulations-and-policy/belmont-report/index.html)和[门洛](https://www.caida.org/publications/papers/2012/menlo_report_actual_formatted/)伦理研究报告作者的集体研究的启发，并且关注产品用户的安全。选择这些原则非常重要，同样重要的是预先定义指导你公司的原则，从高层企业目标到单个产品的关键绩效指标（KPIs）[或指标]。*

1.  *下一步：在优化一个KPI之前，考虑这个KPI如何与您的原则对齐或不对齐。现在记录下来，并至少在内部或在线上与用户沟通。*

1.  *下一步：监控用户体验，既要**定量**也要**定性**。考虑你观察到的意外用户体验，以及无论你的KPIs是否在改善，你的原则是否受到挑战。*

1.  *重复一遍：这些冲突是公司学习和成长的机会：我们如何重新思考我们的关键绩效指标（KPIs）以与我们的目标和关键结果（OKRs）对齐，这些应当源于我们的原则？**如果你发现自己说你的某个指标是“事实上的”目标，那你就做错了。***

[原文](https://towardsdatascience.com/on-the-implications-of-goodharts-law-for-data-science-8f4c5cd81d2e)。经允许转载。

**个人简介：** [贾米尔·米拉比托](https://www.linkedin.com/in/jamil-mirabito-62b328aa/) 是芝加哥大学贫困实验室的项目经理，以及纽约市Flatiron School的数据科学学生。

**相关：**

+   [应用伦理于AI的5种方法](https://www.kdnuggets.com/2019/12/5-ways-apply-ethics-ai.html)

+   [数据科学中的6个常见错误及如何避免](https://www.kdnuggets.com/2020/09/6-common-data-science-mistakes.html)

+   [设计伦理算法](https://www.kdnuggets.com/2019/03/designing-ethical-algorithms.html)

### 更多相关话题

+   [停止学习数据科学以寻找目标，并找到目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的五个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的AI失败，经过审查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [为什么 Python 是初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
