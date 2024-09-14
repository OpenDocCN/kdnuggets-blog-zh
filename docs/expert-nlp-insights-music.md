# 企鹅咖啡乐团的 NLP 洞察

> 原文：[https://www.kdnuggets.com/2021/08/expert-nlp-insights-music.html](https://www.kdnuggets.com/2021/08/expert-nlp-insights-music.html)

赞助帖子。

**由 Laura Gorrieri 提供，expert.ai**

请在[此处](https://github.com/coprinus-comatus/music-insights-expertai)找到此线程的笔记本版本。

让我们构建一个小应用程序来调查我最喜欢的艺术家之一。他们叫做"[企鹅咖啡乐团](https://www.penguincafe.com/)"，如果你不认识他们，你将会了解他们的风格。

**我们的数据集**：我从 Piero Scaruffi 的网站上获取的他们专辑评论列表，并保存在一个专门的文件夹中。

**我们的目标**：通过专辑评论更深入地了解一位艺术家。

**我们的实际目标**：查看 [expert.ai 的 NL API](https://developer.expert.ai/ui?utm_medium=placement&utm_source=kdnuggets&utm_campaign=blog) 如何工作及其功能。

**企鹅咖啡乐团的音乐是什么？**

首先，让我们看看从评论中分析的词汇会得出什么。我们将首先将所有评论连接成一个变量，以获得完整的艺术家评论。然后我们将查看其中最常出现的词，希望这能揭示更多关于企鹅咖啡乐团的信息。

```py

*## Code for iterating on the artist's folder and concatenate albums' reviews in one single artist's review*
import os

artist_review = ''
artist_path = 'penguin_cafe_orchestra'
albums = os.listdir(artist_path)

for album in albums:
album_path = os.path.join(artist_path, album)
      with open(album_path, 'r', encoding = 'utf8') as file:
           review = file.read()
           artist_review += review

```

通过浅层语言学方法，我们可以调查包含所有可用评论的艺术家评论。为此，我们使用 matplotlib 和 word cloud 生成一个词云，以告诉我们文本中最常见的词汇。

*# 导入包*

```py

import matplotlib.pyplot as plt
%matplotlib inline

*# Define a function to plot word cloud*
def plot_cloud(wordcloud):
    *# Set figure size*
    plt.figure(figsize=(30, 10))
    *# Display image*
    plt.imshow(wordcloud)
    *# No axis details*
    plt.axis("off");

*# Import package*
from wordcloud import WordCloud, STOPWORDS

*# Generate word cloud*
wordcloud = WordCloud(width = 3000, height = 2000, random_state=1, background_color='white', collocations=False, stopwords = STOPWORDS).generate(artist_review)

*# Plot*
plot_cloud(wordcloud)

```

![专家AI企鹅咖啡词云](../Images/ab67ee776659865bf40688108c2bfd7c.png)

图1：一个词云，其中使用频率最高的词以较大的字体显示，而使用频率较低的词则以较小的字体显示。

**他们的音乐让你感觉如何？**

通过词云，我们对企鹅咖啡乐团有了更多了解。我们知道他们使用了如尤克里里、钢琴和小提琴等乐器，并且他们混合了民谣、民族和古典等多种风格。

尽管如此，我们对艺术家的风格仍然一无所知。我们可以通过查看他们作品中表现出的情感来了解更多。

为此，我们将使用 expert.ai 的 NL API。请在[此处](https://developer.expert.ai/ui?utm_medium=placement&utm_source=kdnuggets&utm_campaign=blog)注册，查找 SDK 文档[此处](https://github.com/therealexpertai/nlapi-python)以及功能文档[此处](https://docs.expert.ai/nlapi/latest/)。

*### 安装 Python SDK*

```py

!pip install expertai-nlapi

*## Code for initializing the client and then use the emotional-traits taxonomy*

import os

from expertai.nlapi.cloud.client import ExpertAiClient
client = ExpertAiClient()

os.environ["EAI_USERNAME"] = 'your_username'
os.environ["EAI_PASSWORD"] = 'your_password'

emotions =[]
weights = []

output = client.classification(body={"document": {"text": artist_review}}, params={'taxonomy': 'emotional-traits', 'language': 'en'})

for category in output.categories:
    emotion = category.label
    weight = category.frequency
    emotions.append(emotion)
    weights.append(weight)

print(emotions)
print(weights)

```

**['幸福', '兴奋', '喜悦', '娱乐', '爱']

[15.86, 31.73, 15.86, 31.73, 4.76]**

为了检索权重，我们使用了“频率”，实际上这是一个百分比。所有频率的总和是 100\。这使得情感频率成为饼图的良好候选项，该饼图使用 matplotlib 绘制。

*# 导入库*

```py

from matplotlib import pyplot as plt
import numpy as np

*# Creating plot*
colors = ['#0081a7','#2a9d8f','#e9c46a','#f4a261', '#e76f51']
fig = plt.figure(figsize =(10, 7))
plt.pie(weights, labels = emotions, colors=colors, autopct='%1.1f%%')

*# show plot*
plt.show()

```

![专家AI饼图](../Images/a51e0ffd85128a4fc331fe8e220b2384.png)

图2：一个饼图，展示每种情感及其百分比。

**他们的最佳专辑是什么？**

如果你想开始聆听他们的作品，看看是否与你感受到的情感一致，你该从哪里开始？我们可以查看每张专辑的情感分析，了解它们的最佳专辑。为此，我们迭代每张专辑的评论，并使用 expert.ai NL API 获取其情感及强度。

*## 迭代每张专辑并获取情感的代码*

```py

sentiment_ratings = []
albums_names = [album[:-4] for album in albums]

for album in albums:
    album_path = os.path.join(artist_path, album)
    with open(album_path, 'r', encoding = 'utf8') as file:
        review = file.read()
        output = client.specific_resource_analysis(
            body={"document": {"text": review}}, params={'language': 'en', 'resource': 'sentiment' })
            sentiment = output.sentiment.overall sentiment_ratings.append(sentiment)

print(albums_names)
print(sentiment_ratings)

```

**['从家中广播', '音乐会节目', '企鹅咖啡馆的音乐', '生命的迹象']**

[11.6, 2.7, 10.89, 3.9]**

现在我们可以使用柱状图可视化每条评论的情感。这将快速提供对企鹅咖啡馆管弦乐队最佳专辑和他们职业生涯的视觉反馈。为此，我们再次使用matplotlib。

```py

import matplotlib.pyplot as plt
plt.style.use('ggplot')

albums_names = [name[:-4] for name in albums]

plt.bar(albums_names, sentiment_ratings, color='#70A0AF') plt.ylabel("Album rating")
plt.title("Ratings of Penguin Cafe Orchestra's album")
plt.xticks(albums_names, rotation=70)
plt.show()

```

![Expert Ai 评分柱状图](../Images/58c21cf638e06489296266c994a8bbcf.png)

最初发布于 [这里](https://community.expert.ai/articles-showcase-48/penguin-cafe-orchestra-insights-with-expert-ai-130)。

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

### 更多相关话题

+   [影响洞察时间的关键因素](https://www.kdnuggets.com/2023/03/key-factors-affecting-time-insights.html)

+   [ChatGPT驱动的数据探索：解锁数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

+   [CDC数据复制：技术、权衡、洞察](https://www.kdnuggets.com/2023/08/cdc-data-replication-techniques-tradeoffs-insights.html)

+   [学术界是否过于迷恋方法论以至于忽视了真正的洞察？](https://www.kdnuggets.com/is-academia-obsessing-over-methodology-at-the-cost-of-true-insights)

+   [专家对开发安全、可靠和可信的AI框架的见解](https://www.kdnuggets.com/expert-insights-on-developing-safe-secure-and-trustworthy-ai-frameworks)

+   [使用LLM将非结构化数据转换为结构化洞察的5种方法](https://www.kdnuggets.com/5-ways-of-converting-unstructured-data-into-structured-insights-with-llms)
