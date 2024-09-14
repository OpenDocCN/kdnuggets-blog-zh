# 基于内容的推荐系统使用自然语言处理（NLP）

> 原文：[https://www.kdnuggets.com/2019/11/content-based-recommender-using-natural-language-processing-nlp.html](https://www.kdnuggets.com/2019/11/content-based-recommender-using-natural-language-processing-nlp.html)

[评论](#comments)

**作者 [James Ng](https://www.linkedin.com/in/jnyh/)，数据科学，项目管理**

![图](../Images/7f8e754ee66d96b7400675d994e0621d.png)

Netflix 屏幕截图

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

当我们在互联网上为产品和服务提供评分时，我们表达的所有偏好和共享的数据（无论是显式的还是隐式的），都被推荐系统用来生成推荐。最常见的例子包括亚马逊、谷歌和 Netflix。

推荐系统有两种类型：

+   **协作** 过滤器 — 基于用户评分和消费习惯将相似用户分组，然后向用户推荐产品/服务

+   **基于内容** 过滤器 — 根据产品/服务的属性进行推荐

![](../Images/2dcdb2ad2ab5c8169026821cdf25dc26.png)

在这篇文章中，我结合了电影的属性如类型、情节、导演和主要演员，以计算其与另一部电影的余弦相似度。数据集是从 [data.world](https://data.world/studentoflife/imdb-top-250-lists-and-5000-or-so-data-records) 下载的 IMDB 前 250 部英语电影。

### **步骤 1：导入 Python 库和数据集，执行探索性数据分析（EDA）**

确保已安装 [快速自动关键词提取](https://pypi.org/project/rake-nltk/)（RAKE）库（或通过 pip 安装 rake_nltk）。

```py
from rake_nltk import Rake
import pandas as pd
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.feature_extraction.text import CountVectorizerdf = pd.read_csv('IMDB_Top250Engmovies2_OMDB_Detailed.csv')
df.head()
```

探索数据集后，发现有 250 部电影（行）和 38 个属性（列）。然而，只有 5 个属性是有用的：‘标题’，‘导演’，‘演员’，‘情节’，和‘类型’。以下是 10 位热门导演的列表。

```py
df['Director'].value_counts()[0:10].plot('barh', figsize=[8,5], fontsize=15, color='navy').invert_yaxis()
```

![图](../Images/143eada4ced3d015d1a321b93796f356.png)

250 部电影中最受欢迎的 10 位导演

### **步骤 2：数据预处理** 删除停用词、标点符号、空白字符，并将所有词转换为小写字母

首先，需要使用自然语言处理（NLP）对数据进行预处理，以获得仅包含每部电影所有属性（以文字形式）的单列。之后，通过向量化将这些信息转换为数字，为每个词分配分数。随后可以计算余弦相似度。

我使用Rake函数从‘Plot’列中的整个句子中提取最相关的词。为此，我将该函数应用于‘Plot’列下的每一行，并将关键字列表分配给新列‘Key_words’。

```py
df['Key_words'] = ''
r = Rake()for index, row in df.iterrows():
    r.extract_keywords_from_text(row['Plot'])
    key_words_dict_scores = r.get_word_degrees()
    row['Key_words'] = list(key_words_dict_scores.keys())
```

![图示](../Images/bcdf4a2340589736a2acc2c23f0548f2.png)

Rake函数用于提取关键字

演员和导演的名字被转换为唯一的身份值。这是通过将所有名字和姓氏合并为一个词来完成的，这样Chris Evans和Chris Hemsworth将显示为不同的（否则，它们将有50%的相似度，因为它们都有‘Chris’）。每个词都需要转换为小写以避免重复。

```py
df['Genre'] = df['Genre'].map(lambda x: x.split(','))
df['Actors'] = df['Actors'].map(lambda x: x.split(',')[:3])
df['Director'] = df['Director'].map(lambda x: x.split(','))for index, row in df.iterrows():
    row['Genre'] = [x.lower().replace(' ','') for x in row['Genre']]
    row['Actors'] = [x.lower().replace(' ','') for x in row['Actors']]
    row['Director'] = [x.lower().replace(' ','') for x in row['Director']]
```

![图示](../Images/b8795a8fae02d7d719a9cbe33210019e.png)

所有名字都被转换为唯一的身份值

### **步骤3：通过结合列属性创建‘Bag_of_words’的词表示**

数据预处理后，这4列‘Genre’，‘Director’，‘Actors’和‘Key_words’被合并成一列‘Bag_of_words’。最终的数据框只有2列。

```py
df['Bag_of_words'] = ''
columns = ['Genre', 'Director', 'Actors', 'Key_words']for index, row in df.iterrows():
    words = ''
    for col in columns:
        words += ' '.join(row[col]) + ' '
    row['Bag_of_words'] = words

df = df[['Title','Bag_of_words']]
```

![图示](../Images/91daeda133caf11fa48dc92452cf647b.png)

最终的词表示是新列‘Bag_of_words’

### **步骤4：为‘Bag_of_words’创建向量表示，并创建相似度矩阵**

推荐模型只能读取和比较一个向量（矩阵）与另一个向量，因此我们需要使用**CountVectorizer**将‘Bag_of_words’转换为向量表示，这是一种对‘Bag_of_words’列中每个词的频率进行计数的简单计数器。一旦得到包含所有词计数的矩阵，就可以应用cosine_similarity函数来比较电影之间的相似度。

![图示](../Images/b0f86d764647679bc9d67f18e2fa7d1d.png)

计算相似度矩阵中值的余弦相似度公式

```py
count = CountVectorizer()
count_matrix = count.fit_transform(df['Bag_of_words'])cosine_sim = cosine_similarity(count_matrix, count_matrix)
print(cosine_sim)
```

![图示](../Images/1d1df8bd478bfd856cc8ae17e8f9f435.png)

相似度矩阵（250行 x 250列）

接下来是创建一个电影标题系列，以便系列索引可以匹配相似度矩阵的行和列索引。

```py
indices = pd.Series(df['Title'])
```

### **步骤5：运行并测试推荐模型**

最后的步骤是创建一个函数，该函数以电影标题作为输入，并返回前10部相似电影。该函数将输入的电影标题与相似度矩阵的对应索引匹配，并按降序提取相似度值的行。通过提取前11个值并随后丢弃第一个索引（即输入电影本身），可以找到前10部相似电影。

```py
def recommend(title, cosine_sim = cosine_sim):
    recommended_movies = []
    idx = indices[indices == title].index[0]
    score_series = pd.Series(cosine_sim[idx]).sort_values(ascending = False)
    top_10_indices = list(score_series.iloc[1:11].index)

    for i in top_10_indices:
        recommended_movies.append(list(df['Title'])[i])

    return recommended_movies
```

现在我准备好测试模型了。让我们输入我最喜欢的电影“复仇者联盟”，看看推荐结果。

```py
recommend('The Avengers')
```

![图示](../Images/2a87a4521728f581560f69a88a9a7a1f.png)

与“复仇者联盟”相似的前10部电影

### 结论

模型推荐了非常相似的电影。根据我的“领域知识”，我可以看到一些相似性主要基于导演和情节。我已经看过大多数这些推荐电影，并期待观看那些少数未看过的电影。

带有内联注释的**Python**代码可在我的[GitHub](https://github.com/JNYH)上找到，欢迎参考。

[**JNYH/movie_recommender**

使用自然语言处理（NLP）的内容推荐系统。构建基于内容的电影推荐器的指南……](https://github.com/JNYH/movie_recommender?source=post_page-----159d0925a649----------------------)

感谢阅读！

**简介：[James Ng](https://www.linkedin.com/in/jnyh/)** 对从数据中发掘洞察有深入的兴趣，热衷于将实践数据科学、强大的业务领域知识和敏捷方法论结合起来，为企业和社会创造指数级的价值。热衷于提升数据流畅度，使数据易于理解，以便业务能够做出数据驱动的决策。

[原文](https://towardsdatascience.com/content-based-recommender-using-natural-language-processing-nlp-159d0925a649). 经许可转载。

**相关：**

+   [YouTube 如何推荐你的下一个视频](/2019/10/youtube-recommending-next-video.html)

+   [在线聊天的主题提取与分类](/2019/11/topics-extraction-classification-online-chats.html)

+   [文本编码：回顾](/2019/11/text-encoding-review.html)

### 更多相关主题

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)

+   [自然语言处理关键术语解析](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [自然语言处理的温和介绍](https://www.kdnuggets.com/2022/06/gentle-introduction-natural-language-processing.html)
