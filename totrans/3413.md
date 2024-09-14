# 销售电话的数据科学：意外的词汇揭示了问题或成功的信号

> 原文：[https://www.kdnuggets.com/2016/09/data-science-sales-calls-words-trouble-success.html](https://www.kdnuggets.com/2016/09/data-science-sales-calls-words-trouble-success.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Amit Bendov](https://www.linkedin.com/in/amitbendov)，Gong.io的首席执行官和联合创始人**。

![船头](../Images/4254dfb3cffabd81e1f815a7681fd9ab.png)

将近一个世纪以来，对泰坦尼克号可能安息的地点理论层出不穷。由前沿深海测绘船Argo拍摄的标志性照片终结了这一辩论。类似地，直到最近，宇宙的年龄仍然不确定。通过显示宇宙的膨胀率，哈勃望远镜提供了一个估计：137.5亿年。

偶尔会出现一个图像，它终结了数十年的理论、信念和观点。

这些图像令人震惊，且往往与您预期的样子不太一样。

当我们看到从分析大量销售电话数据库中得出的惊人初步结果时，我们就是这种感觉。

尽管这不是揭示宇宙秘密一样深奥的问题，但如何进行成功的销售对话是一个古老的问题，每天影响着数百万人。对此也没有缺乏意见：亚马逊上有超过13,000本关于销售的书籍。

### 首先，关于数据的一点背景。

+   来自18家美国B2B公司的超过25,000小时的销售电话记录。

+   电话记录被分离为扬声器、转录、清理（机器转录相当“嘈杂”），然后使用人工智能和机器学习来寻找各种模式。（有趣的想法：一个人仅仅听这些电话就需要12年。）

+   电话记录与相应的交易历史记录（“机会”）进行了匹配，以确定交易结果和速度。

注意：有几十个因素可能影响交易的成功与否。有些与卖方相关，有些与买方相关。在这篇文章中，我会挑选两个有用的买方信号。将来我会分享一些销售人员常说的内容。

### 所以这是第一个信号：你的交易什么时候结束？

在几乎任何初始对话中，销售人员都会试图了解交易的时间框架。

她可能会问“你预计什么时候开始这个项目？”或“这个计划的时间表是什么？”

对于回答的变体有数百种可能性，从“哦，我们昨天就需要这个”到“2017年下半年”。

结果发现，最具指示性的词之一是*"probably"*。比如“可能在二月”或者“可能在第一季度末之前”。提到这个词与交易时间的相关性很高。事实上，当客户在讨论时间时使用“probably”一词时，预测的准确性高达73%。不过，在你过于兴奋之前，我需要提醒你：这是相关性，不是因果关系。我们了解这一区别，对吧？:) 这并不意味着交易会成功。只是说如果成功的话，时间可能是准确的。

现在你可能可以看到为什么这对我们来说是一个“阿戈时刻”。我曾认为“probably”有些负面。谁会想到这个词实际上是一个积极的信号？这就是机器学习的力量：事实，而不是意见。

由于这是相关性的，我们只能猜测原因：“我们昨天就需要这个”，并不是一个很有深度的回答，也并不真正有意义。同样的，“我相信明年某个时候”也没有多大意义。但“可能在三月”是一个更谨慎、更深思熟虑的回答。因此，他们很可能是认真的。

### 现在来看一个沉默的杀手：“反信号”。

在慢速交易的通话中出现的顶级短语之一是*"to figure out"*。比如“我们需要弄清楚我们的策略……”或者“我需要与团队讨论，然后我们需要弄清楚……”，特别是在结束通话并讨论下一步时。

在这里，这也并不意味着交易已经结束，但表明有一个障碍，所以要保持警惕。你可能还需要将交易的成功概率估计在你通常范围的下限。

这也是我未曾想到的顶级信号，但一旦我们看到它，它就显得完全合乎逻辑。另一个阿戈时刻。

目前就这些了，大家 - 我希望你们会觉得有用。

**简介：[Amit Bendov](https://www.linkedin.com/in/amitbendov)**是Gong.io的首席执行官兼联合创始人。他在高速增长的科技创业公司中拥有超过20年的高级领导经验；管理研发、市场营销和销售，涉及北美、欧洲和亚太地区的全球企业。他的履历包括将科技公司从零销售到成功退出/IPO，从小团队到大组织，从默默无闻到炙手可热的公司。

[原文](https://www.linkedin.com/pulse/phrase-signals-trouble-your-deal-plus-another-thats-actually-bendov)。经授权转载。

**相关**：

+   [环游世界60天：在普通话中让深度语音工作](/2016/02/getting-deep-speech-work-mandarin-baidu.html)

+   [采访：Ali Vanderveld，Groupon关于分析驱动销售团队的关键要素](/2015/07/interview-ali-vanderveld-groupon-analytics-powered-sales-force.html)

+   [微软正在变成M(ai)crosoft](/2016/04/microsoft-becoming-m-ai-crosoft.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 了解更多相关主题

+   [你可以用 R 做的 5 件令人惊讶的事情](https://www.kdnuggets.com/2022/08/5-surprising-things-r.html)

+   [Python 中的情感分析：超越词袋模型](https://www.kdnuggets.com/sentiment-analysis-in-python-going-beyond-bag-of-words)

+   [我的数据科学六个月成功故事](https://www.kdnuggets.com/2023/04/data-science-six-months-success-story.html)

+   [上下文、一致性和协作对数据…至关重要](https://www.kdnuggets.com/2022/01/context-consistency-collaboration-essential-data-science-success.html)

+   [数据科学面试必备的 21 张备忘单：解锁…](https://www.kdnuggets.com/2022/06/21-cheat-sheets-data-science-interviews.html)

+   [数据科学方法驱动业务成功](https://www.kdnuggets.com/2023/10/nwu-data-science-methods-drive-business-success)
