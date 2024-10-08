# 利用大数据分析以“少数派报告”方式预防犯罪

> 原文：[`www.kdnuggets.com/2016/04/using-big-data-analytics-prevent-crimes-minority-report-way.html`](https://www.kdnuggets.com/2016/04/using-big-data-analytics-prevent-crimes-minority-report-way.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Thomas Maydon， [Principa](http://www.principa.co.za/) 的信贷解决方案负责人**。

![类似《少数派报告》中预测犯罪的分析方法](img/c0bd9ec1ca3bce7c48f13cb68691429c.png "类似《少数派报告》中预测犯罪的分析方法")

自从我们在《少数派报告》中看到犯罪预防的未来已经快 15 年了——但今天，我们开始看到那些曾经虚构但极具幻想性的犯罪预测和预防方法在世界各地得到实施。下面我将简要提到三个例子，说明分析方法如何已被用于预防犯罪，然后详细讨论第四个例子：利用分析方法防止罪犯再次犯罪。

### 1\. PrePol：识别犯罪“热点”

在洛杉矶和英国，“预测性警务”（“PrePol”）已经部署了二十多年。它在这段时间里当然已经演变了。[如今 PrePol 利用算法识别犯罪“热点”](https://www.theguardian.com/cities/2014/jun/25/predicting-crime-lapd-los-angeles-police-data-analysis-algorithm-minority-report)。结果非常积极。在双盲试验中，它们的效果是传统预防方法的两倍。

### 2\. 智能电网基础设施：识别和预测电力盗窃

在南非，关于采用“智能电网”和“智能电表”的讨论不断，这将使我们的电力公共事业公司 Eskom 和各市政当局能够预测电缆盗窃并识别非法连接。在美国，[电力盗窃被视为第三大盗窃形式](http://deloitte.wsj.com/cio/2013/12/02/using-analytics-to-crack-down-on-electricity-theft/)，引入[计量数据管理](https://en.wikipedia.org/wiki/Smart_meter#Advanced_metering_infrastructure)和“智能电网基础设施”意味着每一秒钟都会生成大量数据，准备进行分析——一个真正的大数据问题。这种“智能电网基础设施”[也被用于水务公司以防止水盗窃](http://bv.com/Home/news/solutions/water/smart-water-technology-benefits-challenges-and-three-action-steps-for-utilities)。

### 3\. 中国的大数据监控达到机器人警察情景

最近，[彭博社报道了中国政府通过大数据监控来帮助预防犯罪的努力](http://www.bloomberg.com/news/articles/2016-03-03/china-tries-its-hand-at-pre-crime?utm_source=digg)，在数据隐私法律有限的国家中。在政府的指示下，该国最大的国有防务承包商正在建立一个分析软件平台，能够交叉引用来自银行账户、工作、爱好、消费模式和监控摄像头的视频信息，以识别潜在的恐怖分子。截至今年 1 月 1 日，中国当局已获得对银行账户、电信以及一个被称为“天网”的国家监控摄像头网络的访问权限。想象一个世界，警察佩戴增强现实眼镜能够识别出人群中的每个人。

![中国政府通过大数据进行监控类似于机器警察场景](img/96fa9b8523e0f5caa7b88b950de34c55.png "中国政府通过大数据进行监控类似于机器警察场景")

在识别个人之后，从他们的医疗记录、社交媒体活动、人口统计数据和警方记录中提取信息，并通过算法进行处理。由此识别出潜在犯罪嫌疑人，警方可以采取或计划行动。这种“机器人警察”类型的场景在中国不再遥远。

### 4\. 预测重新犯罪的机器学习

本月，《实证法律研究杂志》[发表了一篇论文](http://onlinelibrary.wiley.com/doi/10.1111/jels.12098/abstract)，详细介绍了一项大规模研究，探讨了机器学习是否可以有效地辅助法官在家庭暴力初审/保释听证会上作出决定，即是否应当批准被捕者保释。

#### 研究

在这项研究中，对 2007-2011 年期间的 28,000 个家庭暴力初审听证会进行了评估（观察期）。然后评估了这 28,000 名个人在接下来的两年内是否重新犯罪（结果期）。机器分析了包括前科、指控以及年龄和性别等人口统计数据在内的 35 多个特征。使用随机森林（一种统计技术）来评估重新犯罪的可能性。

![机器学习算法预测重新犯罪的可能性](img/ff4dd17b791c5b633d23b996092f7658.png "机器学习算法预测重新犯罪的可能性")

即使使用的数据字段相对较少，结果仍然令人印象深刻。通常，法院会批准大多数人的保释，而 20%的人在接下来的 24 个月内被发现重新犯罪。分析显示，模型本可以更好地选择，仅有 10%的人重新犯罪。

#### 社会影响

在这些问题上，能够预测再次犯罪的概率固然很好，但模型并不完美，错误可能产生重大的社会后果。在这里，法院需要评估假阳性的影响：将某人识别为可能的重犯（尽管他们不是）并拒绝保释，这可能导致他们失去工作并可能失去住房，而被拘留。假阴性——在保释后再次犯罪，而模型预测则相反——也是值得关注的。尽管模型复杂，但它们可能无法考虑到法官所拥有的所有微妙信息。

然而，支持者表示，无论是否使用模型，错误都会发生，如果模型比单独的人为干预表现更好，它不应被忽视。

这类似于也可以使用[机器学习](http://www.cs.columbia.edu/~wfan/PAPERS/AAAI97Workshop.pdf)运行的信用卡交易欺诈模型。假阳性可能导致信用卡被冻结，这会给信用卡持有人带来不便，并增加客户服务电话的数量。银行需要确定其接受可疑交易的阈值。

#### 结论

分析，特别是机器学习，正在证明它们不仅在工业和商业领域具有广泛应用，而且在犯罪预防和法院系统中也具有潜在价值。对犯罪的打击和管理在经验上仍处于起步阶段。这项研究只是许多正在进行的研究之一。

**图片来源：** *二十世纪福克斯（“少数派报告”图片），猎户座影业公司（“机器人警察”和“终结者”图片）*

![Principa 博客文章作者 Thomas Maydon 的照片](img/2f3f4958126acf5e9532ea5ef1159688.png)

****个人简介：** *Thomas Maydon* 是 Principa 的信用解决方案主管，拥有超过 13 年的南非、西非和中东零售信贷市场经验。他在信贷生命周期的所有方面（包括智能前景开发、行为建模、定价分析、催收流程和准备金（包括巴塞尔协议 II）及盈利能力计算）具有丰富经验。*

**相关：**

+   《少数派报告》可视化 – 芝加哥警察分析

+   白宫将数据视为 21 世纪有效警务的催化剂

+   大数据、隐私和安全 – 你站在哪一边？

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 相关话题

+   [大数据如何实时挽救生命：IoV 数据分析帮助…](https://www.kdnuggets.com/how-big-data-is-saving-lives-in-real-time-iov-data-analytics-helps-prevent-accidents)

+   [今天 90% 的代码是为了防止失败，这成了一个问题](https://www.kdnuggets.com/2022/07/90-today-code-written-prevent-failure-problem.html)

+   [利用 AI 和分析引擎更快准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [使用 Pandas fillna() 输入缺失数据的最佳方法](https://www.kdnuggets.com/2023/02/optimal-way-input-missing-data-pandas-fillna.html)

+   [一本将彻底改变您组织方式的新书…](https://www.kdnuggets.com/2022/02/manning-new-book-revolutionize-way-organization-approaches-data.html)

+   [图表：理解数据的自然方式](https://www.kdnuggets.com/2022/10/manning-graphs-natural-way-understand-data.html)**
