# 使用 Pandas 的推文分析初学者指南

> 原文：[https://www.kdnuggets.com/2017/03/beginners-guide-tweet-analytics-pandas.html](https://www.kdnuggets.com/2017/03/beginners-guide-tweet-analytics-pandas.html)

Twitter 为所有用户提供了分析访问权限，但我假设相对较少的普通用户会关注其存在。还有各种其他服务可以帮助进行推文和受众分析，以及与地理和自然语言处理相关的进一步分析，但与一些简单的 Python 配合使用时，Twitter 提供的数据可以非常有用。

这是一个简单的指南，帮助你在 Python 中亲自进行一些分析。与许多其他教程通常从实时 Twitter API 中提取数据不同，我们将使用可下载的 Twitter Analytics 数据，我们的大部分工作将在 Pandas 中完成。

在我们开始之前，先处理必要的导入部分。

### 获取并检查数据

首先我们需要数据。这部分非常简单；前往 Twitter，点击右上角的菜单（你的头像），选择**分析**，在顶部选择**推文**选项卡，使用日期范围选择器选择时间段，然后选择**导出数据**。你使用的数据量无所谓；我们的简单示例适用于任何数量的数据。我选择了默认的**过去 28 天**。

![](../Images/91d318b27d4114f353b60ee7749061af.png)

获取 Twitter 分析数据。

一旦我们有了 CSV 文件，我们将需要将其加载到 Pandas DataFrame 中进行分析。

不用在意所有被丢弃的列；虽然其中很多数据对我们的分析很有用——推文文本、时间、印象、转发等——但许多数据并不有用——所有**推广**的内容——因此我们将从一开始就省略它们。

正如我们在任何数据分析项目中一样，接下来我们查看数据。

![](../Images/8a31b05d700e5357f276a72da7cf708e.png)

运行：

告诉我们我在过去 4 周内发推了可怜的 95 次。数据集不是很大，我们可能不想根据我们的发现做出任何推断，但这是一个足够好的玩具数据集，可以作为起点。

让我们看看我们可以从中提取出哪些有用的分析数据。

### 基本推文统计

因此，考虑到在上述数据集中运行`head()`的输出中显示的数据，并对哪些推文指标可能有用有一个大致的直觉，我们将获取以下统计数据：

+   **转发** - 每条推文的平均转发数和前 5 条转发最多的推文

+   **点赞** - 每条推文的平均点赞数和前 5 条最受欢迎的推文

+   **印象** - 每条推文的平均印象数和前 5 条印象最多的推文

```py
Total tweets this period: 95 

Mean retweets: 1.72 

Top 5 RTed tweets:
------------------
A PyTorch IPython Notebook tutorial on #deeplearning, with an emphasis on #NaturalLanguageProcessing https://t.co/bxiBD42T7E https://t.co/awTsZA8R9v  -  26
On the Origin of #DeepLearning https://t.co/oe7r43HHVS #NeuralNetworks #arxiv https://t.co/BIcba61FR9  -  25
7 MORE Steps to Mastering #MachineLearning With #Python https://t.co/5yAjeUpCfS https://t.co/juWH0rQaNR  -  16
Every Intro to #DataScience Course on the Internet, Ranked https://t.co/rQG7Higk6b https://t.co/b6VveKfJxD  -  8
Pandas & Seaborn - A guide to handle & visualize #data elegantly @tryolabs https://t.co/LPq2q8k1i1 #Python #dataviz https://t.co/k2IoWsttXM  -  7

Mean likes: 3.17 

Top 5 liked tweets:
-------------------
A PyTorch IPython Notebook tutorial on #deeplearning, with an emphasis on #NaturalLanguageProcessing https://t.co/bxiBD42T7E https://t.co/awTsZA8R9v  -  52
7 MORE Steps to Mastering #MachineLearning With #Python https://t.co/5yAjeUpCfS https://t.co/juWH0rQaNR  -  37
On the Origin of #DeepLearning https://t.co/oe7r43HHVS #NeuralNetworks #arxiv https://t.co/BIcba61FR9  -  37
Pandas & Seaborn - A guide to handle & visualize #data elegantly @tryolabs https://t.co/LPq2q8k1i1 #Python #dataviz https://t.co/k2IoWsttXM  -  16
I've been reposted on @YhatHQ - The Current State of Automated #MachineLearning https://t.co/ggAW1Hrmxk https://t.co/8030HhAMMA  -  14

Mean impressions: 674.39

Top 5 tweets with most impressions:
-----------------------------------
On the Origin of #DeepLearning https://t.co/oe7r43HHVS #NeuralNetworks #arxiv https://t.co/BIcba61FR9 - 6409
A PyTorch IPython Notebook tutorial on #deeplearning, with an emphasis on #NaturalLanguageProcessing https://t.co/bxiBD42T7E https://t.co/awTsZA8R9v - 5684
7 MORE Steps to Mastering #MachineLearning With #Python https://t.co/5yAjeUpCfS https://t.co/juWH0rQaNR - 4374
I've been reposted on @YhatHQ - The Current State of Automated #MachineLearning https://t.co/ggAW1Hrmxk https://t.co/8030HhAMMA - 2270
Pandas & Seaborn - A guide to handle & visualize #data elegantly @tryolabs https://t.co/LPq2q8k1i1 #Python #dataviz https://t.co/k2IoWsttXM - 1818
```

我不会对这些指标进行任何分析。不用说，我应该提升我的社交媒体表现。

### 前 #标签 和 @提及

毫无疑问，标签在Twitter中扮演着重要角色，提及也有助于扩展你的网络和影响力。它们共同帮助将“社交”融入社交网络，将像Twitter这样的平台从被动体验转变为非常活跃的体验。了解这个社交网络最社交的方面将是一个有帮助的努力。

```py
Top 10 hashtags:
----------------
machinelearning - 21
deeplearning - 16
python - 15
datascience - 10
neuralnetworks - 10
ai - 5
data - 4
datascientist - 3
tensorflow - 3
rstats - 3

Top 10 mentions:
----------------
kdnuggets - 3
francescoai - 2
jakevdp - 2
quora - 2
yhathq - 2
noahmp - 1
udacity - 1
clavitolo - 1
monkeylearn - 1
nicholashould - 1
```

把一些明显的问题，比如推文文本中的标点在检查Twitter句柄之前被移除（如果你有名为Francesco_AI和FrancescoAI的用户，这可能是个问题），这方法有效，至少相对Pythonic（虽然我相信可以做得更好）。

### 时间序列分析

最后，我们来看看一些非常基础的时间数据。我们将检查基于推文的小时和星期几的平均印象。我再次警告，这基于很少的数据，因此可能得不到有用的信息。然而，考虑到大量的推文数据，可以计划整个社交媒体活动。

虽然这基于印象，但同样可以（并且容易地）基于互动、转发或其他你喜欢的内容。做广告和推广推文？也许你对我们最初从数据集中剥离的一些**推广**指标更感兴趣。

我们必须将Twitter提供的日期字段转换为合法的Python datetime对象，根据其所属的小时段对数据进行分组，识别星期几，然后在DataFrame中捕捉这些数据的几个附加列，我们将在之后用来提取统计信息。

```py
Average impressions per tweet by hour tweeted:
----------------------------------------------
0 - 1 : 141 => 1 tweets
9 - 10 : 445 => 10 tweets
10 - 11 : 611 => 9 tweets
11 - 12 : 1319 => 10 tweets
12 - 13 : 528 => 10 tweets
13 - 14 : 448 => 11 tweets
14 - 15 : 464 => 16 tweets
15 - 16 : 763 => 8 tweets
17 - 18 : 634 => 9 tweets
18 - 19 : 1306 => 8 tweets
19 - 20 : 454 => 1 tweets
21 - 22 : 186 => 1 tweets
23 - 24 : 208 => 1 tweets

Average impressions per tweet by day of week tweeted:
-----------------------------------------------------
Mon : 475 => 20  tweets
Tue : 568 => 18  tweets
Wed : 1418 => 18  tweets
Thu : 545 => 17  tweets
Fri : 432 => 22  tweets
```

看起来我在一天中的时间比较一致地发推。我还发现我的星期三推文、上午11点推文和下午6点推文是我的主要推文。当然，这基于95条推文，因此意义不大且结论不确切。然而，在对一些相当大数据集执行这些相同步骤后，观察到了一些有趣的趋势，这可能有助于商业决策。都是从一些简单的Python开始的。

虽然不是惊天动地，我们基于Pandas的简单Twitter分析代码足以让我们思考如何更好地利用社交媒体。应用于正确的数据，基础脚本可以相当强大。

**相关内容：**

+   [使用Python挖掘Twitter数据第一部分：收集数据](/2016/06/mining-twitter-data-python-part-1.html)

+   [Pandas备忘单：Python中的数据科学和数据清理](/2017/01/pandas-cheat-sheet.html)

+   [在Python中清理数据](/2017/01/tidying-data-python.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

### 了解更多相关主题

+   [初学者的 Pandas Melt 函数指南](https://www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html)

+   [使用 Pandas 的数据摄取：初学者教程](https://www.kdnuggets.com/2022/04/data-ingestion-pandas-beginner-tutorial.html)

+   [初学者的端到端机器学习指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [基本机器学习算法：初学者指南](https://www.kdnuggets.com/2021/05/essential-machine-learning-algorithms-beginners.html)

+   [初学者的 Q 学习指南](https://www.kdnuggets.com/2022/06/beginner-guide-q-learning.html)

+   [使用 Python 的网页抓取初学者指南](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)
