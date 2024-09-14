# 创建真实数据科学作品集项目的逐步指南

> 原文：[https://www.kdnuggets.com/2020/10/guide-authentic-data-science-portfolio-project.html](https://www.kdnuggets.com/2020/10/guide-authentic-data-science-portfolio-project.html)

[评论](#comments)

**由[Felix Vemmer](https://www.linkedin.com/in/felix-vemmer/)，N26的运营智能数据分析师**。

![](../Images/2744df67a05e7bdb3595f17bc40915cc.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

作为一名激励人心的数据科学家，**构建有趣的作品集项目是展示你技能的关键**。当我通过[在线课程学习编码和数据科学](https://medium.com/@felix.vemmer/how-what-and-why-you-should-learn-python-as-a-business-student-ea539df88698)时，我不喜欢数据集都是虚假的数据或已经解决过的，比如[Kaggle上的波士顿房价数据集](https://www.kaggle.com/vikrishnan/boston-house-prices)或[Titanic数据集](https://www.kaggle.com/c/titanic)。

在这篇博客文章中，我想**向你展示我如何开发有趣的数据科学项目想法并一步一步实施**，比如探索德国最大的常旅客论坛Vielfliegertreff。如果你时间有限，可以直接跳到结论TLDR。

### 第1步：选择一个与你的热情话题相关的主题

作为第一步，我考虑一个潜在的项目，满足以下三个要求，使其成为最有趣和最愉快的：

1.  解决**我自己的问题或紧迫问题。**

1.  与某些**最近的事件相关**或特别有趣。

1.  **之前没有被解决或覆盖过。**

由于这些想法仍然相当抽象，让我给你一个关于我的三个项目如何满足要求的概述：

*我自己数据科学作品集项目的概述，符合上述三个要求。*

作为初学者，不要追求完美，而是选择你真正感兴趣的东西，并写下你想要探讨的所有问题。

### 第2步：开始收集你自己的数据集

鉴于你遵循了我的第三个要求，将不会有公开的数据集，你需要自己抓取数据。在抓取了几个网站之后，我有**3个主要框架**用于不同的场景：

*我用于抓取的三个主要框架概述。*

对于Vielfliegertreff，我使用了[scrapy](https://scrapy.org/)作为框架，原因如下：

1.  **没有启用JavaScript**的元素隐藏数据。

1.  该网站**结构复杂，**需要从每个论坛主题开始，遍历所有线程，从所有线程到所有帖子网页。使用**scrapy你可以轻松实现复杂的逻辑**，以有组织的方式生成请求，导致新的回调函数。

1.  帖子数量相当多，因此抓取整个论坛肯定需要一些时间。Scrapy允许你**以惊人的速度异步抓取网站**。

为了让你了解scrapy的强大，我快速对我的MacBook Pro（13英寸，2018年，四个Thunderbolt 3端口）进行了基准测试，配备2.3 GHz四核Intel Core i5，能够**每分钟抓取约3000个页面**：

![](../Images/bdda51e503722d123b1dd830da5b1d67.png)

*Scrapy抓取基准测试。（图片来源：作者）*

为了友好且避免被封锁，重要的是要温和地抓取，例如启用scrapy的[自动调节功能](https://docs.scrapy.org/en/latest/topics/autothrottle.html)。此外，我还通过项目管道将所有数据保存到SQL lite数据库中，以避免重复，并开启了每个URL请求的日志记录，以确保在停止和重新启动抓取过程时不会给服务器带来额外负担。

了解如何进行抓取可以让你自由地自行收集数据集，并教会你关于互联网如何运作、请求是什么以及HTML/XPath的结构等重要概念。

对于我的项目，我最终得到1.47 GB的数据，接近100万条论坛帖子。

### 第三步：清理数据集

使用自己抓取的混乱数据集，投资组合项目中最具挑战性的部分出现了，这也是**数据科学家平均花费60%时间**的部分：

![](../Images/28723ac6f4c63cd503a06283d197b47c.png)

*图片来自[CrowdFlower](https://visit.figure-eight.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport_2016.pdf) 2016。*

与干净的Kaggle数据集不同，你自己的数据集可以帮助你在数据清理方面建立技能，并向未来的雇主展示你已经准备好处理混乱的现实数据集。此外，你还可以通过利用解决一些常见数据清理任务的Python库来探索和利用Python生态系统。

对于Vielfliegertreff的数据集，有一些常见的任务，比如将日期转换为pandas时间戳，将数字从字符串转换为实际的数值数据类型，以及将非常混乱的HTML帖子文本清理成可读且适用于NLP任务的内容。虽然有些任务更复杂，我想**分享我最喜欢的三个库**，它们解决了我一些常见的数据清理问题：

1.  [dateparser](https://github.com/scrapinghub/dateparser)：轻松解析几乎任何网页上常见字符串格式的本地化日期。

1.  [clean-text](https://github.com/jfilter/clean-text)：使用clean-text预处理抓取的数据，创建标准化的文本表示。这个工具也很棒，可以移除个人身份信息，如电子邮件或电话号码等。

1.  [fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy)：像老板一样进行模糊字符串匹配。

### 第4步：数据探索与分析

在Udacity完成数据科学纳米学位时，我遇到了**跨行业数据挖掘标准过程（CRISP-DM）**，我认为这是一个相当有趣的框架，可以系统地组织你的工作。

通过我们目前的流程，我们隐式地遵循了CRISP-DM框架：

表达**业务理解**的步骤是通过以下问题来实现：

1.  COVID-19对像Vielfliegertreff这样的在线常旅客论坛有什么影响？

1.  论坛中一些最佳的帖子是什么？

1.  作为新成员，我应该关注哪些专家？

1.  关于航空公司或机场，人们说过的一些最糟糕或最好的话是什么？

使用抓取的数据，我们现在能够将最初的业务问题转化为具体的数据解释性问题：

1.  每月发布了多少帖子？在2020年初COVID-19之后，帖子是否减少？是否有迹象表明因无法旅行而导致加入平台的人数减少？

1.  按点赞数量排序的前10个帖子有哪些？

1.  谁是发帖最多且平均获得最多点赞的用户？这些用户是我应该定期关注的，以便看到最好的内容。

1.  对每个帖子进行情感分析，结合命名实体识别以识别城市/机场/航空公司，是否会产生有趣的正面或负面评论？

对于Vielfliegertreff项目，可以明确说出帖子数量在逐年减少。随着COVID-19的爆发，我们可以清晰地看到，从2020年1月开始，当欧洲封锁和关闭边境时，帖子数量迅速下降，这也严重影响了旅行：

*按月份创建的帖子。（作者图表）*

此外，用户注册数量在多年间有所下降，论坛似乎自2009年1月启动以来的快速增长逐渐减少：

*用户注册数量按月份统计。（作者图表）*

最后但同样重要的是，我想查看一下最受欢迎的帖子内容。遗憾的是，它是用德语写的，但确实是一篇非常有趣的帖子，一位德国人被允许在美国航母上待了一段时间，并体验了C2飞机的弹射起飞。帖子包含一些非常漂亮的照片和有趣的细节。如果你能理解一些德语，可以点击[这里](https://www.vielfliegertreff.de/reiseberichte/105553-retro-tripreport-flugzeugtraeger.html)查看：

![](../Images/39d77be0b29c7f4437b6ae08734cd115.png)

*来自 Vielfliegertreff 上最受欢迎帖子的示例图片（图片由 [fleckenmann](https://www.vielfliegertreff.de/reiseberichte/105553-retro-tripreport-flugzeugtraeger.html) 提供）。*

### 第五步：通过博客帖子或 Web 应用分享你的工作

完成这些步骤后，你可以进一步创建一个模型，用于分类或预测某些数据点。对于这个项目，我没有尝试以特定方式使用机器学习，尽管我对将帖子情感与特定航空公司关联进行分类有一些有趣的想法。

在另一个项目中，我建模了一个价格预测算法，使用户能够获得任何类型拖拉机的价格估算。该模型随后通过强大的 [streamlit 框架](https://www.streamlit.io/) 部署，您可以在 [这里](https://traktorpreis.herokuapp.com/) 找到它（请耐心等待加载，可能会稍慢）。

另一种分享你工作的方式是像我一样通过 Medium、[Hackernoon](https://hackernoon.com/tagged/python)、[KDNuggets](https://www.kdnuggets.com/) 或其他流行网站的博客帖子。当写关于作品集项目或其他主题的博客帖子时，例如 [精彩的互动 AI 应用](https://medium.com/@felix.vemmer/5-awesome-interactive-ai-apps-for-you-to-try-77bac8ab7554)，我总是尽力让它们**有趣、直观和互动**。以下是我的一些顶级建议：

+   包括漂亮的图片，以便于**理解和打破长文本。**

+   包括互动元素，比如**让用户互动**的推文或视频。

+   通过 airtable 或 plotly 等工具和框架，将枯燥的表格或图表改为**互动式的**。

### 结论与总结

想出一个博客帖子点子，回答你曾经有过的迫切问题或解决你自己的问题。理想情况下，主题的时效性相关，并且之前没有人分析过。根据你的经验、网站结构和复杂性，选择最适合抓取工作的框架。在数据清洗过程中，利用现有的库来解决像解析时间戳或清理文本等棘手的数据清洗任务。最后，选择最佳的方式分享你的工作。无论是一个互动部署的模型/仪表板还是一个写得很好的 Medium 博客帖子，都可以让你在成为数据科学家的道路上与其他申请者区分开来。

[原文](https://towardsdatascience.com/a-step-by-step-guide-for-creating-an-authentic-data-science-portfolio-project-aa641c2f2403)。经许可转载。

**个人简介：** [Felix Vemmer](https://www.linkedin.com/in/felix-vemmer/) 是 N26 的数据分析师，专注于通过网络爬虫和机器学习创建有趣的数据集和项目。

**相关：**

+   [8 个 AI/机器学习项目让你的作品集脱颖而出](https://www.kdnuggets.com/2020/09/8-ml-ai-projects-stand-out.html)

+   [数据科学作品集中应包含的项目](https://www.kdnuggets.com/2019/04/projects-include-data-science-portfolio.html)

+   [如何建立数据科学作品集](https://www.kdnuggets.com/2018/07/build-data-science-portfolio.html)

### 相关话题

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [如何结构化数据科学项目：逐步指南](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
