# 主题建模方法：Top2Vec 与 BERTopic

> 原文：[https://www.kdnuggets.com/2023/01/topic-modeling-approaches-top2vec-bertopic.html](https://www.kdnuggets.com/2023/01/topic-modeling-approaches-top2vec-bertopic.html)

![主题建模方法：Top2Vec 与 BERTopic](../Images/668abce1b24f95ea2750b95c939a3722.png)

图片由 [Mikechie Esparagoza](https://www.pexels.com/photo/photo-of-assorted-letter-board-quote-hanged-on-wall-1742370/) 拍摄

每天，我们大多数时候都在处理未标记的文本，而监督学习算法完全无法用于从数据中提取信息。自然语言的一个子领域可以揭示大量文本中的潜在结构。这一学科称为主题建模，专门用于从文本中提取主题。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您所在组织的 IT 工作

* * *

在这种情况下，传统的方法，如潜在狄利克雷分配（Latent Dirichlet Allocation）和非负矩阵分解（Non-Negative Matrix Factorization），未能很好地捕捉词之间的关系，因为它们基于词袋模型。

因此，我们将重点关注两种有前景的方法，Top2Vec 和 BERTopic，这些方法通过利用预训练语言模型生成主题，从而解决这些缺陷。让我们开始吧！

# Top2Vec

Top2Vec 是一种能够通过使用预训练的词向量和创建有意义的嵌入主题、文档和词向量，自动检测文本中的主题的模型。

在这种方法中，提取主题的过程可以分为不同的步骤：

1.  **创建语义嵌入**：创建联合嵌入的文档和词向量。其思想是，相似的文档在嵌入空间中应更接近，而不相似的文档之间应保持距离。

1.  **减少文档嵌入的维度**：应用降维方法对于在减少高维空间的同时保留大部分文档嵌入的变异性非常重要。此外，这也允许识别密集区域，其中每个点代表一个文档向量。UMAP 是在此步骤中选择的典型降维方法，因为它能够保留高维数据的局部和全局结构。

1.  **识别文档簇**：应用基于密度的聚类方法HDBScan来寻找相似文档的密集区域。如果文档不在密集簇中，则被标记为噪声；如果属于密集区域，则被标记为一个标签。

1.  **在原始嵌入空间中计算质心**：质心通过考虑高维空间而不是缩减的嵌入空间来计算。经典策略包括计算所有属于密集区域的文档向量的算术平均值，这些区域是通过HDBSCAN在先前步骤中获得的。通过这种方式，为每个簇生成一个主题向量。

1.  **为每个主题向量找到词汇**：与文档向量最近的词向量在语义上是最具代表性的。

## Top2Vec的示例

在本教程中，我们将分析来自[data.world](https://data.world/crowdflower/mcdonalds-review-sentiment)的数据集中麦当劳的负面评论。从这些评论中识别主题对跨国公司来说是有价值的，以便改进产品和美国地区的快餐连锁店组织。

```py
import pandas as pd
from top2vec import Top2Vec

file_path = "McDonalds-Yelp-Sentiment-DFE.csv"
df = pd.read_csv(
    file_path,
    usecols=["_unit_id", "city", "review"],
    encoding="unicode_escape",
)
df.head()
docs_bad = df["review"].values.tolist()
```

![主题建模方法：Top2Vec与BERTopic](../Images/5679d764b1c931d55b32169dcd6791b3.png)

通过一行代码，我们将执行之前解释的所有Top2Vec步骤。

```py
topic_model = Top2Vec(
    docs_bad,
    embedding_model="universal-sentence-encoder",
    speed="deep-learn",
    tokenizer=tok,
    ngram_vocab=True,
    ngram_vocab_args={"connector_words": "phrases.ENGLISH_CONNECTOR_WORDS"},
)
```

Top2Vec的主要论点是：

+   docs_bad: 是一个字符串列表。

+   universal-sentence-encoder: 是选择的预训练嵌入模型。

+   deep-learn: 是一个决定生成文档向量质量的参数。

```py
topic_model.get_num_topics() #3
topic_words, word_scores, topic_nums = topic_model.get_topics(3)

for topic in topic_nums:
    topic_model.generate_topic_wordcloud(topic)
```

最重要的是

![主题建模方法：Top2Vec与BERTopic](../Images/342e839d6dad2a34bf4cf34cdd41b104.png)![主题建模方法：Top2Vec与BERTopic](../Images/28b15cea4b0bc64984e1b42d9173a0d6.png)![主题建模方法：Top2Vec与BERTopic](../Images/d79a930ee5541b0a66f0479e165d19f5.png)

从词云中，我们可以推测主题0是关于麦当劳服务的一般投诉，如“服务慢”、“服务糟糕”和“订单错误”，而主题1和2分别指的是早餐食品（麦芬、饼干、鸡蛋）和咖啡（冰咖啡和杯咖啡）。

现在，我们尝试使用两个关键词搜索文档：错误和慢：

```py
(
    documents,
    document_scores,
    document_ids,
) = topic_model.search_documents_by_keywords(
    keywords=["wrong", "slow"], num_docs=5
)
for doc, score, doc_id in zip(documents, document_scores, document_ids):
    print(f"Document: {doc_id}, Score: {score}")
    print("-----------")
    print(doc)
    print("-----------")
    print()
```

输出：

```py
Document: 707, Score: 0.5517634093633295
-----------
horrible.... that is all. do not go there.
-----------

Document: 930, Score: 0.4242547340973836
-----------
no drive through :-/
-----------

Document: 185, Score: 0.39162203345993046
-----------
the drive through line is terrible. they are painfully slow.
-----------

Document: 181, Score: 0.3775083338082392
-----------
awful service and extremely slow. go elsewhere.
-----------

Document: 846, Score: 0.35400602635951994
-----------
they have bad service and very rude
-----------
```

# BERTopic

> *“BERTopic是一种主题建模技术，它利用变换器和c-TF-IDF创建密集的簇，从而使主题易于解释，同时保留主题描述中的重要词汇。”*

正如名字所示，BERTopic利用强大的变换器模型来识别文本中的主题。这个主题建模算法的另一个特点是使用TF-IDF的变体，称为基于类别的TF-IDF变体。

像Top2Vec一样，它不需要知道主题的数量，而是自动提取主题。

此外，与Top2Vec类似，它是一个涉及不同阶段的算法。前三个步骤是相同的：创建嵌入文档、使用UMAP进行降维和使用HDBScan进行聚类。

随后的阶段开始与Top2Vec有所不同。在使用HDBSCAN找到密集区域后，每个主题被标记为一个词袋表示，这个表示考虑了词汇是否出现在文档中。然后，将属于同一集群的文档视为一个唯一文档，并应用TF-IDF。因此，对于每个主题，我们识别出最相关的词汇，这些词汇应该具有最高的c-TF-IDF。

## BERTopic示例

我们在相同的数据集上重复分析。

我们将使用BERTopic从评论中提取主题：

```py
model_path_bad = 'model/bert_bad'
topic_model_bad = train_bert(docs_bad,model_path_bad)
freq_df = topic_model_bad.get_topic_info()
print("Number of topics: {}".format( len(freq_df)))
freq_df['Percentage'] = round(freq_df['Count']/freq_df['Count'].sum() * 100,2)
freq_df = freq_df.iloc[:,[0,1,3,2]]
freq_df.head()
```

![主题建模方法：Top2Vec与BERTopic](../Images/3f49f3d932e79c03c92cc4f49904f3af.png)

模型返回的表格提供了关于提取的14个主题的信息。主题对应于主题标识符，除了被忽略的所有离群点，这些离群点标记为-1。

现在，我们将进入最有趣的部分，即将我们的主题可视化为互动图表，例如每个主题的最相关术语的可视化、主题间距离图、嵌入空间的二维表示和主题层级。

让我们开始展示前十个主题的条形图。对于每个主题，我们可以观察最重要的词汇，并根据c-TF-IDF评分按降序排列。一个词汇的相关性越高，它的评分就越高。

第一个主题包含通用词汇，如位置和食物，主题1为顺序和等待，主题2为最差和服务，主题3为地方和肮脏，等等。

在可视化条形图之后，我们可以查看主题间距离图。我们将c-TF-IDF评分的维度降到二维空间，以便在图中可视化这些主题。底部有一个滑块，可以选择将其标记为红色的主题。我们可以注意到，主题被分成了两个不同的集群，一个包含诸如食物、鸡肉和地点等通用主题，另一个则包含不同的负面方面，如最差服务、肮脏、地方和寒冷。

下一张图表展示了评论与主题之间的关系。特别是，它可以帮助理解为何某条评论被分配到特定主题，并与最相关的词汇对齐。例如，我们可以关注红色集群，它对应于主题2，包含了一些关于最差服务的词汇。这个密集区域中的文档似乎相当负面，比如“糟糕的客户服务和更糟糕的食物”。

# TopVec与BERTopic的区别

表面上看，这些方法有许多共同点，如自动确定主题数量、大多数情况下无需预处理、应用UMAP降低文档嵌入的维度，然后使用HDBSCAN对这些降低的文档嵌入进行建模，但在将主题分配给文档的方式上，它们在根本上是不同的。

Top2Vec通过寻找靠近聚类中心的单词来创建主题表示。

与Top2Vec不同，BERTopic不考虑聚类中心，而是将聚类中的所有文档视为一个独特的文档，并使用基于类的TF-IDF变体提取主题表示。

| **Top2Vec** | **BERTopic** |
| --- | --- |
| 基于聚类中心提取主题的策略。 | 基于c-TF-IDF提取主题的策略。 |
| 不支持动态主题建模。 | 支持动态主题建模。 |
| 它为每个主题构建词云，并提供主题、文档和单词的搜索工具。 | 它允许构建互动可视化图，帮助解释提取的主题。 |

# 参考文献

主题建模是自然语言处理中一个不断发展的领域，应用广泛，包括评论、音频和社交媒体帖子。正如所示，本文概述了Topi2Vec和BERTopic这两种有前景的方法，它们可以帮助你用少量代码识别主题，并通过数据可视化来解释结果。如果你对这些技术有疑问，或者有其他检测主题的方法建议，请在评论中写下。

**[尤金妮亚·安内洛](https://www.linkedin.com/in/eugenia-anello/)** 目前是意大利帕多瓦大学信息工程系的研究员。她的研究项目专注于持续学习与异常检测的结合。

### 更多相关主题

+   [文本摘要的方法概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [机器学习的数据标注：市场概况、方法和工具](https://www.kdnuggets.com/2021/12/data-labeling-ml-overview-and-tools.html)

+   [一本将彻底改变你组织的…](https://www.kdnuggets.com/2022/02/manning-new-book-revolutionize-way-organization-approaches-data.html)

+   [AI生成的体育亮点：不同的方法](https://www.kdnuggets.com/2022/03/aigenerated-sports-highlights-different-approaches.html)

+   [机器学习的甜蜜点：NLP和文档分析中的纯方法](https://www.kdnuggets.com/2022/05/machine-learning-sweet-spot-pure-approaches-nlp-document-analysis.html)

+   [3种数据插补方法](https://www.kdnuggets.com/2022/12/3-approaches-data-imputation.html)
