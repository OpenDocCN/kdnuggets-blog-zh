# 构建推荐系统

> 原文：[https://www.kdnuggets.com/2019/04/building-recommender-system.html](https://www.kdnuggets.com/2019/04/building-recommender-system.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Matthew Mahowald， [开放数据组](https://www.opendatagroup.com/)**。

推荐系统是今天最突出的机器学习实例之一。它们决定了你的 Facebook 新闻源中出现的内容、产品在 Amazon 上的展示顺序、Netflix 队列中建议的视频，以及无数其他例子。但什么是推荐系统，它们是如何工作的？这篇文章是探索构建推荐系统的一些常见技术及其实现的系列文章中的第一篇。

![数据工程](../Images/efe8223e7c92f882a8d32fd9e5e37cb1.png)

### 什么是推荐系统？

推荐系统是一种信息过滤模型，用于对用户的项目进行排名或评分。一般有两种排名方法：

+   **基于内容的过滤**，其中推荐项目基于项目之间的相似性和用户的显式偏好；以及

+   **协同过滤**，其中向用户推荐的项目基于其他具有相似交易历史和特征的用户的偏好。

协同过滤中使用的信息可以是显式的，即用户对每个项目提供评分，也可以是隐式的，即从用户行为（购买、浏览等）中提取用户偏好。最成功的推荐系统使用结合两种过滤方法的混合方法。

### MovieLens 数据集

为了使讨论更具实质性，让我们关注于使用一个具体的例子来构建推荐系统。[GroupLens](https://grouplens.org/) 是明尼苏达大学的一个研究小组，他们慷慨地提供了 [MovieLens 数据集](https://grouplens.org/datasets/movielens/)。该数据集包含约 2000 万条用户评分，涉及 27,000 部电影，由 138,000 名用户评分。此外，电影还包括类别和日期信息。我们将使用此数据集来构建

### 简单的基于内容的过滤

让我们构建一个简单的推荐系统，该系统使用基于内容的过滤（即项目相似性）来推荐电影给我们观看。首先，从 MovieLens 加载电影数据集，并对类别字段进行多重热编码：

```py
import pandas as pd
import numpy as np

movies = pd.read_csv("movies.csv")
*# dummy encode the genre*
movies = movies.join(movies.genres.str.get_dummies("|"))

```

`genres` 特征由一个或多个管道符号（“|”）分隔的类别组成。上面的最后一行添加了每个可能类别的列，如果类别标签存在，则在该条目中放置 1，否则放置 0。

基于这些标签，让我们生成一些基于项目相似性的推荐。对于类别数据（如标签），*余弦相似度*是一种非常常见的相似度度量。对于任何两个项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 和 ![j](../Images/4b5e5503442fdfa2029fcc9208d6ca1a.png "j")， ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 和 ![j](../Images/4b5e5503442fdfa2029fcc9208d6ca1a.png "j") 的余弦相似度就是 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 和 ![j](../Images/4b5e5503442fdfa2029fcc9208d6ca1a.png "j") 之间夹角的余弦值，其中 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 和 ![j](../Images/4b5e5503442fdfa2029fcc9208d6ca1a.png "j") 被解释为特征空间中的向量。请记住，余弦值是从这些向量的内积中获得的：

![ \cos \theta = \frac{i \cdot j}{||i|| ||j||} ](../Images/3a502cd0acbdd797c1f8c8e41b7b7a9f.png " \cos \theta = \frac{i \cdot j}{||i|| ||j||} ")

作为一个具体的例子，考虑电影 $i := $ 《玩具总动员》（类型标签：“冒险”、“动画”、“儿童”、“喜剧”和“奇幻”）和 $j := $ 《勇敢的心》（类型标签：“冒险”、“儿童”和“奇幻”）。点积 ![i \cdot j](../Images/18c1f73139dbcc8f400317cc1558e470.png "i \cdot j") 是 3（这两部电影有三个标签相同）。 ![||i|| = \sqrt{5}](../Images/f9e775e1a1102b4e3de42072875562fd.png "||i|| = \sqrt{5}") 和 ![||j|| = \sqrt{3}](../Images/c683224c2c69453efeae2284e2fe7f58.png "||j|| = \sqrt{3}"), 所以这两部电影之间的余弦相似度是

![ \cos \theta = \frac{3}{\sqrt{15}} \approx 0.775 ](../Images/86e2624d8c47a992e299576a1b6b9b64.png " \cos \theta = \frac{3}{\sqrt{15}} \approx 0.775 ")

我们可以计算数据集中所有项目的余弦相似度：

```py
from sklearn.metrics.pairwise import cosine_similarity

*# compute the cosine similarity* cos_sim = cosine_similarity(movies.iloc[:,3:])

```

数据集中的第一部电影是《玩具总动员》。让我们找出与《玩具总动员》相似的电影：

```py
*# Let's get the top 5 most similar films:* toystory_top5 = np.argsort(sim[0])[-5:][::-1]

*# array([   0, 8219, 3568, 9430, 3000, 2809, 2355, 6194, 8927, 6948, 7760,
#       1706, 6486, 6260, 5490])* 
```

| 电影ID | 类型 |
| --- | --- |
| 《玩具总动员》（1995） | 冒险, 动画, 儿童, 喜剧, 奇幻 |
| 《极速蜗牛》（2013） | 冒险, 动画, 儿童, 喜剧, 奇幻 |
| 《怪兽电力公司》（2001） | 冒险, 动画, 儿童, 喜剧, 奇幻 |
| 《海洋奇缘》（2016） | 冒险, 动画, 儿童, 喜剧, 奇幻 |
| 《皇帝的新装》（2000） | 冒险, 动画, 儿童, 喜剧, 奇幻 |

前五部电影的类型标签与《玩具总动员》完全相同，因此其余弦相似度为 1。事实上，对于这里使用的样本数据，共有十三部电影的相似度为 1；最相似的没有完全相同标签的电影是2006年的《蚂蚁大兵》，它有额外的类型标签“IMAX”。

### 简单协同过滤

协同过滤根据相似用户喜欢的内容来推荐物品。幸运的是，在 MovieLens 数据集中，我们拥有丰富的用户偏好信息，以电影评分的形式存在：每个用户给一个或多个电影打分，分数在 1 到 5 之间，表示他们对电影的喜爱程度。我们可以将向用户推荐物品的问题视为一个 *预测* 任务：根据用户对其他电影的评分，预测他们对该电影的可能评分。

一种简单的方法是使用其他用户的评分为每个物品分配一个加权评分：

![ \hat{r}_{u,i} = \frac{\sum_{v\neq u} s(u,v) r_{v, i}}{\sum_{v \neq u} s(u,v)} ](../Images/5f6421befb7e1eb480685c5260bd8dd9.png " \hat{r}_{u,i} = \frac{\sum_{v\neq u} s(u,v) r_{v, i}}{\sum_{v \neq u} s(u,v)} ")

其中 ![\hat{r}_{u,i}](../Images/bd523a20b848a5e6853139d33b96e37c.png "\hat{r}_{u,i}") 是用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png "u") 对物品 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 的预测评分，![s(u,v)](../Images/f6abcb8dc87947b544bb5ff5fba0b39e.png "s(u,v)") 是用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png "u") 和 ![v](../Images/73161e4d85d5c3cc1fa03163b0a92a77.png "v") 之间相似度的度量，![r](../Images/dc16d9309e95f2dd60bba8a2d99d78b4.png "r") 是用户 ![v](../Images/73161e4d85d5c3cc1fa03163b0a92a77.png "v") 对物品 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 的已知评分。

对于我们的用户相似度度量，我们将查看用户对电影的评分。评分相似的用户将被认为是相似的。处理这些评分数据的一个重要第一步是对我们的评分进行归一化。我们将分三步进行：首先，减去整体平均评分（跨所有电影和用户），使调整后的评分以 0 为中心。接下来，我们对每部电影做同样的操作，以考虑给定电影的平均评分的差异。最后，我们减去每个用户的平均评分——这考虑了个人的变化（例如，一些用户的评分始终较高）。

在数学上，我们的调整评分 ![\tilde{r}_{u,i}](../Images/52a9c49297042d77a17cce1bba397ac4.png "\tilde{r}_{u,i}") 是

![ \tilde{r}_{u,i} := r_{u,i} - \bar{r} - \bar{r}_{i} - \bar{r}_{u} ](../Images/7e596563eff0b2daefe4be7079d610a7.png " \tilde{r}_{u,i} := r_{u,i} - \bar{r} - \bar{r}_{i} - \bar{r}_{u} ")

其中 ![r_{u,i}](../Images/30a460c76c42202245eafbf4be93163d.png "r_{u,i}") 是基础评分，![\bar{r}](../Images/37a022e0abffb5ed0c15433d52b6d2ca.png "\bar{r}") 是总体均值评分，![\bar{r},i](../Images/fa2dead136fcde4516fc578fb3ca28d5.png "\bar{r},i") 是项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 的均值评分（在减去总体均值之后），而 ![\bar{r},u](../Images/f7aa7e448aef9d73800f713f228bac10.png "\bar{r},u") 是用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png "u") 的均值评分（在调整了总体均值评分和项目均值评分之后）。为了方便，我会将调整后的评分 ![\tilde{r}](../Images/15faa8d8c7bbf110f5cdc90f3df75b49.png "\tilde{r}") 称为用户 ![u](../Images/c1155b10039d460302206caf78e70b84.png "u") 对项目 ![i](../Images/7e670de46a6672b7c7196f23e4711b0b.png "i") 的*偏好*。

让我们加载评分数据并计算调整后的评分：

```py
ratings = pd.read_csv("ratings.csv")

mean_rating = ratings['rating'].mean() *# compute mean rating*

pref_matrix = ratings[['userId', 'movieId', 'rating']].pivot(index='userId', columns='movieId', values='rating')

pref_matrix = pref_matrix - mean_rating *# adjust by overall mean*

item_mean_rating = pref_matrix.mean(axis=0)
pref_matrix = pref_matrix - item_mean_rating *# adjust by item mean*

user_mean_rating = pref_matrix.mean(axis=1)
pref_matrix = pref_matrix - user_mean_rating

```

在这一点上，我们可以轻松地为某个用户对他们尚未观看的电影建立一个合理的基准估计：

```py
pref_matrix.fillna(0) + user_mean_rating + item_mean_rating + mean_rating

```

我们可以按如下方式计算与特定用户（在此情况下为用户 0）的距离：

```py
mat = pref_matrix.values
k = 0 *# use the first user*
np.nansum((mat - mat[k,:])**2,axis=1).reshape(-1,1)

```

结果表明，最近的用户是用户 12（距离为 0）：

```py
np.nansum((mat - mat[0,:])**2,axis=1)[1:].argmin() *# returns 11*
*# check it:* np.nansum(mat[12] - mat[0]) # returns 0.0

```

我们找到了用户 12 已看过但用户 0 未看过的两部电影：

```py
np.where(~np.isnan(mat[12]) & np.isnan(mat[0]) == True)
*# returns (array([304, 596]),)* 
mat[12][[304, 596]]
*# returns array([-2.13265214, -0.89476547])* 
```

不幸的是，用户 12 不喜欢用户 0 尚未观看的两部电影！我们应继续计算以考虑所有附近的用户。

### 结论

本文中使用的方法是*基于邻域*的，我们刚刚看到生成基于邻域的推荐时的一个潜在陷阱：邻域可能实际上不会推荐任何用户尚未看过的项目。由于需要计算成对距离，基于邻域的方法也往往在用户数量增加时表现不佳。

在本系列的第 2 部分，我们将查看另一种构建推荐系统的方法，这次使用*潜在因子*方法。潜在因子模型避免了一些基于邻域的方法的陷阱，但正如我们将看到的，它们也有一些自身的挑战！

**资源：**

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [为意外情况做准备](https://www.kdnuggets.com/2019/02/preparing-unexpected.html)

+   [NLP中的词嵌入及其应用](https://www.kdnuggets.com/2019/02/word-embeddings-nlp-applications.html)

+   [人工智能和机器学习的最新进展 – 带代码的论文亮点](https://www.kdnuggets.com/2019/02/paperswithcode-ai-machine-learning-highlights.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

### 相关话题

+   [使用 Python 为 Amazon 产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [等级系统如何帮助预测 AI 成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [学习系统设计：前 5 本必读书籍](https://www.kdnuggets.com/learning-system-design-top-5-essential-reads)

+   [使用 Python 的 Watchdog 监控文件系统](https://www.kdnuggets.com/monitor-your-file-system-with-pythons-watchdog)

+   [通过构建 15 个神经网络项目学习深度学习（2022 年）](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)
