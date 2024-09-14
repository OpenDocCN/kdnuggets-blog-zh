# YouTube 上的十大数据科学视频

> 原文：[https://www.kdnuggets.com/2016/10/top-10-data-science-videos-youtube.html/2](https://www.kdnuggets.com/2016/10/top-10-data-science-videos-youtube.html/2)

**7\. [数据科学 – 埃因霍温理工大学 – (观看次数: 58K)](https://www.youtube.com/watch?v=7IJfSC8o6Y4)**

类别：广告

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

埃因霍温理工大学对其数据科学新课程的戏剧性广告。广告风格具有电影感，使用计算机图形模仿科幻动作片，类似于《少数派报告》和《盗梦空间》，通过使用Oculus Rift和多点触控界面等技术实现。以下是他们的[本科](https://www.tue.nl/en/education/tue-bachelor-college/undergraduate-programs/data-science/)和[硕士](https://www.tue.nl/en/education/tue-graduate-school/masters-programs/data-science-in-engineering/)课程链接。

**8\. [工作中的数据科学家一天 – (观看次数: 57K)](https://www.youtube.com/watch?v=EaptTxhh6sM)**

类别：教育

RCRtv 探索了德州普莱诺 AT&T Foundry 中数据科学家的一天。

该视频采访了资深数据科学家卡尔提克·拉贾戈帕兰，回答了以下问题：数据科学的定义、背景、如何保持前沿、采用的工具以及对有志于成为数据科学家的人的一些最终建议。

**9\. [数据科学家与数据分析师的区别、角色和资格 – (观看次数: 47K)](https://www.youtube.com/watch?v=GiWqKE-yznE)**

类别：教育

Bigdata Simplified的一段屏幕录制视频，讲述数据科学家和数据分析师之间的区别。视频强调了数据的生成方式、价值以及如何释放其力量，最后介绍了数据分析师和数据科学家的不同特点。

**10\. [数据科学的未来 – 数据科学@斯坦福 – (观看次数: 37K)](https://www.youtube.com/watch?v=hxXIJnjC_HI)**

类别：教育

斯坦福大学医学与遗传学副教授**尤安·阿什利**、斯坦福大学化学教授**维贾伊·潘德**、斯坦福大学工程与电气工程教授**赫克托·加西亚-莫利纳**以及斯坦福大学校长**约翰·亨内西**，他们共同举办了一场长达近26分钟的有趣数据科学会议。他们试图解决的问题包括：这个新兴学科有多真实？它带来了哪些机会和挑战？斯坦福大学如何在研究和教育中培养数据科学？

**观察：**

最近，我有机会与不同的数据科学初创公司进行交流。他们的问题是：如何接触他们的客户，即商业人士、风险投资等。

所以，在这一点上，我将我的观点翻译成几个问题：

1.  是否可以仅仅通过提及数据科学来讨论数据科学？最受欢迎的前十名视频显然不是面向技术文盲观众的，而是那些已经对这个领域有兴趣的人。我的想法是：我们为何不将讨论转向一些更热门的话题，如政策制定、城市规划、安全、健康、艺术和隐私？

1.  与其专注于“说服”那些非数据驱动型的公司去采纳数据驱动的文化，从而聘请数据科学家，我们为什么不先专注于将数据科学传达给大众呢？

无论多么困难，在互联网上，甚至在训练营中，他们都在强调数据科学中讲故事的重要性，我觉得沟通在这个领域中是一个大问题。我不仅仅是在谈论可视化和报告。这些都是过时的、混乱的，并且常常是自我指涉的。我意识到我可能听起来很直接，甚至有点苛刻，但推动我如此坦诚的是希望让数据科学向世界开放的真实愿望。一个关键的绩效参数应该是有多少技术文盲的人能够了解并熟悉这个领域。数据科学可能不是万灵药，但它可以改变世界。

作为这一思考的直接结果，我决定检索和分析2014年第一批500个观看量最多的视频的文本元数据，这些视频的标题中包含“数据科学”和/或“数据科学家”这两个词。收集完所有的元数据后，我将所有标签、描述和标题拼接成一个词袋。我然后使用了来自gensim Python框架的word2vec。分析很简单，但结果很有趣。

元数据中的30725个词汇中，大多数与“业务”（工作、职业、管理、行业等）、“教育”（大学、课程、学习、教程、编程等）和“数据科学工具和技术”（机器学习、算法等）相关。像“未来”和“社会”这样的词汇仅占总数的0.13%。与“未来”最相似的词汇再次与“业务”（工作、职业、信息、产品、首席）相关。“新闻学”、“进步”、“政治”、“心理学”在与数据科学视频相关的术语中不到0.016%。这也是一个似乎不以“名人”为导向的领域。Dr. DJ Patil是唯一一个“突出”的影响者：占总词汇的0.1%。

从这非常简单的分析中可能得出的结论是：

1.  数据科学是一个集中的话题。那些已经了解它的人会寻找它，特别是教程和教育用途。其他人则不会。

1.  目前的数据科学（从沟通的角度来看）严格与职业/业务相关。

1.  显然缺乏将数据科学重新框定为“热门话题”。

我和一位高级数据科学家讨论的一个话题是关于解释数据科学在商业中的可靠性。我喜欢他的话，“*科学*应该让我们想起伽利略的实验科学方法。在这个领域，我们进行大量的实验。这一点真的很难解释。”

我们也应该在沟通方面进行实验。

个人简介：[Marco Nasuto](https://www.linkedin.com/in/marco-giuseppe-nasuto-276a8888)是一名数据科学家、航空航天工程师和电影制片人。目前在丹麦工作。

**相关：**

+   [YouTube上的十大机器学习视频](/2015/06/top-10-machine-learning-videos-youtube.html)

+   [YouTube上最受欢迎的数据挖掘视频](/2015/05/most-viewed-data-mining-videos-youtube.html)

+   [YouTube上最受欢迎的大数据视频](/2015/05/most-viewed-big-data-videos-youtube.html)

### 更多相关主题

+   [成为伟大的数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学以寻找目标，并寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
