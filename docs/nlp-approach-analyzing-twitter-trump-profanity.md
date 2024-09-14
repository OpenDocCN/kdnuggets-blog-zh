# 一种分析 Twitter、特朗普和脏话的 NLP 方法

> 原文：[`www.kdnuggets.com/2016/11/nlp-approach-analyzing-twitter-trump-profanity.html/2`](https://www.kdnuggets.com/2016/11/nlp-approach-analyzing-twitter-trump-profanity.html/2)

### 第二步：收集数据

现在是时候调用我们的脚本，并用查询‘Donald Trump OR Trump’来抓取包含‘Donald Trump’或‘Trump’的推文，然后将文件写入数据文件夹，名为‘Donald-Trump-OR-Trump.csv’。

```py
python twitter_pull_data.py 'Donald Trump OR Trump'
```

尝试重新运行脚本，但这次将‘Hillary Clinton OR Hillary’作为查询传入。

在数据文件夹中有两个 CSV 文件后，我们现在可以创建一个名为 profanity_analysis.py 的脚本。

### 第三步：数据预处理

在接下来的脚本中，我们将首先清理数据，去除表情符号、标签、转发等。然后，我们将深入探讨英语停用词和脏话算法。

清理我们的推文就到此为止！

### 第四步：检查推文中的脏话

现在，我们将查看[脏话检测算法](https://algorithmia.com/algorithms/nlp/ProfanityDetection)，并发现推文中的脏话。这个算法基于来自[noswearing.com](http://www.noswearing.com/)的约 340 个单词，通过基本的字符串匹配来捕捉脏话。查看脏话算法页面以了解更多算法的细节，并了解如何通过添加自己的攻击性单词来定制单词列表，因为有趣的新攻击性俚语每天都在不断增加到英语中。不相信？看看[Urban Dictionary](http://www.urbandictionary.com/)上出现的新词汇。

脏话函数非常简单明了：

你只是传入已清除英语停用词的单词列表。我们将它们合并成一个语料库，因为我们对所有推文的总体脏话感兴趣，而不是每条推文的脏话。我们的函数 profanity()打印出算法结果以及总脏话数量。在撰写本文时，查询‘Donald Trump OR Trump’有 30 个脏话，而‘Hillary Clinton OR Clinton’返回 8 个脏话。

![](img/0bb21071d7f64eb0ae78fbb90466457f.png)

当我们提取 Twitter 数据时，还获取了 user_id 和转发次数。这很有用，因为你可能想通过一些轻度分析来评估推文的受欢迎程度，从而了解推文是否因脏话使用的多少而更受欢迎或不受欢迎。

如果你想查看完整代码，请查看我们的[示例应用](https://github.com/algorithmiaio/sample-apps/tree/master/Python/tweet-profanity-demo)在 GitHub 上！

### 下一步

一定要查看我们的其他 NLP 算法，例如[social sentiment analysis](https://algorithmia.com/algorithms/nlp/SocialSentimentAnalysis)或[LDA](https://algorithmia.com/algorithms/nlp/LDA)（标签）。像[AnalyzeTweets](https://algorithmia.com/algorithms/nlp/AnalyzeTweets)这样的微服务将上述算法与一个检索推文的算法相结合。该算法返回每条推文的负面和正面情绪，以及每条推文的负面和正面 LDA。你可以创建无尽的组合进行快速探索性分析，或添加像脏话检测或[Nudity Detection](https://algorithmia.com/algorithms/sfw/NudityDetection)这样的算法到你的应用程序中，确保内容适合家庭观看。

享受探索平台的过程，如有任何问题，请随时[联系](https://algorithmia.com/contact)！

**简介：[Stephanie Kim](https://www.linkedin.com/in/stephanielkim)** 是 Algorithmia 的开发者推广专家。

[原文](http://blog.algorithmia.com/nlp-approach-twitter-trump-profanity/)。已获得许可重新发布。

**相关：**

+   特朗普现象：基于推特的回顾

+   使用自然语言处理探索社交媒体多样性

+   人类向量：结合讲者嵌入让你的机器人更强大

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [使用微调的 SciBERT NER 模型和 Neo4j 分析科学文章](https://www.kdnuggets.com/2021/12/analyzing-scientific-articles-finetuned-scibert-ner-model-neo4j.html)

+   [数据分析：分析数据的四种方法及其如何…](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [利用智能分析未来成功的概率…](https://www.kdnuggets.com/2022/02/analyzing-probability-future-success-intelligence-node-attributes-evolution-model.html)

+   [使用 SQL 分析多样性与包容性](https://www.kdnuggets.com/2022/11/analyzing-diversity-inclusion-sql.html)

+   [掌握数据分析的力量：四种数据分析方法](https://www.kdnuggets.com/2023/03/master-power-data-analytics-four-approaches-analyzing-data.html)

+   [如何成为数据科学家的指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)
