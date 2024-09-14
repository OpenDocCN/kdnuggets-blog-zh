# 关于实时机器学习，没有人告诉你的事

> 原文：[`www.kdnuggets.com/2015/11/petrov-real-time-machine-learning.html`](https://www.kdnuggets.com/2015/11/petrov-real-time-machine-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论**由 Dmitry Petrov，FullStackML**。

在今年，我听到并阅读了很多关于实时机器学习的内容。人们通常在讨论信用卡欺诈检测系统时提供这种引人注目的商业场景。他们说他们可以实时地持续更新信用卡欺诈检测模型（参见 [“什么是 Apache Spark？”](https://www.youtube.com/watch?v=SxAxAhn-BDU)、[“...实时用例...”](https://www.mapr.com/blog/game-changing-real-time-use-cases-apache-spark-on-hadoop#.VhBoNJNVK1E) 和 [“实时机器学习”](http://www.slideshare.net/VinothKumarKannan/real-time-machine-learning)）。这看起来很棒，但对我来说**不切实际**。这个场景中缺少一个重要细节——**模型重训练并不需要连续流动的事务数据。相反，你需要的是连续流动的标记（或预标记为欺诈/非欺诈）的事务数据**。

![实时机器学习](img/44b636132296c26fa69da6058d3daebf.png)

*机器学习过程*

创建标记数据可能是大多数机器学习系统中最慢且最昂贵的步骤。**机器学习算法通过标记数据学习检测欺诈交易，类似于人们的学习方式。** 让我们看看它在欺诈检测场景中的工作方式。

**1\. 创建模型**

对于训练信用卡模型，你需要大量的交易示例，并且每个交易都应标记为欺诈或非欺诈。这些标签必须尽可能准确！这就是我们的标记数据集。这个数据集是监督机器学习算法的输入。基于标记数据，算法训练欺诈检测模型。该模型通常表现为一个二分类器，具有真实（欺诈）或虚假（非欺诈）类别。

**标记数据集在此过程中起着核心作用**。更改算法的参数，如特征归一化方法或损失函数，非常容易。例如，我们可以将算法本身从逻辑回归更改为 SVM 或随机森林。然而，你不能更改标记数据集。这些信息是预定义的，你的模型应预测你已经拥有的标签。

**2\. 数据标记过程需要多长时间？**

我们如何标注最新的交易？如果客户报告欺诈交易或信用卡被盗，我们可以立即将交易标记为“欺诈”。我们该如何处理其余的交易？我们可以假设未报告的交易为“非欺诈”。我们应该等多久才能确认它们不是欺诈？上次我朋友丢了信用卡，她说：“我暂时不会报告丢失的信用卡。明天我会去我最后去过的商店，问问他们是否找到了我的信用卡。”幸运的是，商店找到了并归还了她的信用卡。我不是信用卡欺诈领域的专家（我只是一个好卡用户），但根据我的经验，**我们应该等待至少几天再将交易标记为“非欺诈”**。

相反，**如果有人报告了欺诈交易，我们可以立即将这笔交易标记为“欺诈”**。报告欺诈的人可能在丢失几小时或几天后才意识到欺诈，但这是我们能做的最好事情。

这样一来，我们的“最新”标注数据集将受到限制，只有几个“欺诈”交易存在几小时或几天的延迟，以及很多“非欺诈”交易在 2-3 天的延迟内。

**3\. 尝试加快标注过程**

我们的目标是获得“最新”的标注数据。事实上，我们只有“最新欺诈”标签。对于“非欺诈”标签，我们必须等待几天。仅使用“最新欺诈”标注数据构建模型可能看起来是个好主意。然而，我们应该理解**这份标注数据集存在偏差**，这可能会导致模型出现很多问题。

让我们假设昨天开了一家新的大型购物中心，我们收到了一份关于这家店的一次交易的欺诈报告。我们的标注数据集将只包含这家商店的一次交易，标记为“欺诈”。这家店的所有其他交易尚未标注。算法可能会认为这家商店是一个强有力的欺诈预测指标，**所有来自这家店的交易都会被错误地立即“实时”标记为“欺诈”**。实时性的优势也带来了实时性的问题。

**结论**

如我们所见，信用卡欺诈检测业务场景似乎不是实时监督学习的最佳场景。此外，我无法从其他业务领域中想象出一个好的场景。我希望看到实时机器学习的好场景。**如果你有任何信息或想法与社区分享，请告知。**

另见 [Reddit 上的有趣讨论](https://www.reddit.com/r/MachineLearning/comments/3ny2qa/what_no_one_tells_you_about_realtime_machine/) 讨论了本文中的一些想法。

简介： [Dmitry Petrov](https://www.linkedin.com/profile/view?id=AB4AAAB3AbYByL9xPpYxo65dZ-GMy-SFw-wP0Wk&authType=name&authToken=Zi_p&trk=wonton-desktop)，博士，现为微软的数据科学家。他曾是某大学的研究员。

[原文](http://fullstackml.com/2015/10/07/what-no-one-tells-you-about-real-time-machine-learning/)。

**相关：**

+   流实时分析的模式

+   实时分析中的 Spark SQL

+   访谈：Ted Dunning，MapR 讲述大数据中实时的真正含义

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关主题

+   [为什么你需要学习不止一种编程语言！](https://www.kdnuggets.com/2022/06/need-learn-one-programming-language.html)

+   [打破数据障碍：如何通过零样本、单样本和少样本学习…](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [机器学习不像你的大脑 第一部分：神经元很慢，…](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [GPT-4：8 种模型合一；秘密被揭晓](https://www.kdnuggets.com/2023/08/gpt4-8-models-one-secret.html)

+   [ETL 与 ELT：哪一种适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [Pandas: 如何进行独热编码](https://www.kdnuggets.com/2023/07/pandas-onehot-encode-data.html)
