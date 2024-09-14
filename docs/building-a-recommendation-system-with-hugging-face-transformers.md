# 使用 Hugging Face Transformers 构建推荐系统

> 原文：[`www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers`](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

![使用 Hugging Face Transformers 构建推荐系统](img/a61a757863330ab6619d9dd0c0bdd909.png)

[图像由 jcomp 提供，来源于 Freepik](https://www.freepik.com/free-vector/man-who-thinks-idea-is-admired-by-thumbs-up_11879378.htm#fromView=search&page=1&position=31&uuid=f9d98eb0-c0b9-4681-a4df-3026685c1dea)

在现代时代，我们依赖手机和计算机上的软件。许多应用程序，如电子商务、电影流媒体、游戏平台等，改变了我们的生活，因为这些应用程序使事情变得更加简单。为了更进一步，企业通常会提供能够根据数据进行推荐的功能。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

推荐系统的基础是根据输入预测用户可能感兴趣的内容。系统将根据项目之间的相似性（基于内容的过滤）或行为（协作过滤）提供最接近的项目。

有许多推荐系统架构的方法，我们可以使用[Hugging Face Transformers](https://huggingface.co/docs/transformers/en/index)包。如果你不知道，Hugging Face Transformers 是一个开源的 Python 包，允许 API 轻松访问所有支持文本处理、生成等任务的预训练 NLP 模型。

本文将使用 Hugging Face Transformers 包基于嵌入相似度开发一个简单的推荐系统。让我们开始吧。

## 使用 Hugging Face Transformers 开发推荐系统

在开始教程之前，我们需要安装所需的包。为此，您可以使用以下`code`：

```py
pip install transformers torch pandas scikit-learn
```

您可以通过他们的网站选择适合您环境的版本进行[Torch](https://pytorch.org/get-started/locally/)安装。

至于数据集示例，我们将使用来自[Kaggle](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database?select=anime.csv)的动漫推荐数据集示例。

一旦环境和数据集准备好，我们将开始教程。首先，我们需要读取数据集并进行准备。

```py
import pandas as pd

df = pd.read_csv('anime.csv')

df = df.dropna()
df['description'] = df['name'] +' '+ df['genre'] + ' ' +df['type']+' episodes: '+ df['episodes']
```

在上述代码中，我们使用 Pandas 读取数据集并删除了所有缺失的数据。然后，我们创建了一个名为“description”的特征，其中包含了可用数据中的所有信息，如名称、类型、类别和集数。这个新列将成为我们推荐系统的基础。如果能有更完整的信息，例如动漫情节和概要，会更好，但我们暂时就用这个吧。

接下来，我们将使用 Hugging Face Transformers 加载嵌入模型并将文本转换为数值向量。具体来说，我们将使用句子嵌入来转换整个句子。

推荐系统将基于我们将要执行的所有动漫“描述”的嵌入。我们将使用余弦相似度方法，它衡量两个向量的相似度。通过测量动漫“描述”嵌入与用户查询输入嵌入之间的相似度，我们可以获得精准的推荐项目。

嵌入相似度方法听起来简单，但与经典的推荐系统模型相比，它可能更为强大，因为它能够捕捉词语之间的语义关系，为推荐过程提供上下文意义。

在本教程中，我们将使用 Hugging Face 的嵌入模型句子变换器。为了将句子转换为嵌入，我们将使用以下代码。

```py
from transformers import AutoTokenizer, AutoModel
import torch
import torch.nn.functional as F

def mean_pooling(model_output, attention_mask):
    token_embeddings = model_output[0] #First element of model_output contains all token embeddings
    input_mask_expanded = attention_mask.unsqueeze(-1).expand(token_embeddings.size()).float()
    return torch.sum(token_embeddings * input_mask_expanded, 1) / torch.clamp(input_mask_expanded.sum(1), min=1e-9)

tokenizer = AutoTokenizer.from_pretrained('sentence-transformers/all-MiniLM-L6-v2')
model = AutoModel.from_pretrained('sentence-transformers/all-MiniLM-L6-v2')

def get_embeddings(sentences):
  encoded_input = tokenizer(sentences, padding=True, truncation=True, return_tensors='pt')

  with torch.no_grad():
      model_output = model(**encoded_input)

  sentence_embeddings = mean_pooling(model_output, encoded_input['attention_mask'])

  sentence_embeddings = F.normalize(sentence_embeddings, p=2, dim=1)

  return sentence_embeddings
```

尝试嵌入过程，并用以下代码查看向量结果。不过，我不会展示输出，因为它相当长。

```py
sentences = ['Some great movie', 'Another funny movie']
result = get_embeddings(sentences)
print("Sentence embeddings:")
print(result)
```

为了简化操作，Hugging Face 维护了一个用于嵌入句子变换器的 Python 包，这将把整个转换过程缩减到 3 行代码。请使用以下代码安装必要的包。

```py
pip install -U sentence-transformers
```

然后，我们可以用以下代码将整个动漫“描述”转换为嵌入。

```py
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')

anime_embeddings = model.encode(df['description'].tolist())
```

当嵌入数据库准备好后，我们会创建一个函数来接收用户输入并执行余弦相似度，以作为推荐系统。

```py
from sklearn.metrics.pairwise import cosine_similarity

def get_recommendations(query, embeddings, df, top_n=5):
    query_embedding = model.encode([query])
    similarities = cosine_similarity(query_embedding, embeddings)
    top_indices = similarities[0].argsort()[-top_n:][::-1]
    return df.iloc[top_indices]
```

现在一切准备就绪，我们可以尝试推荐系统。以下是从用户输入查询中获取前五个动漫推荐的示例。

```py
query = "Funny anime I can watch with friends"
recommendations = get_recommendations(query, anime_embeddings, df)
print(recommendations[['name', 'genre']])
```

```py
Output>>
                                         name  \
7363  Sentou Yousei Shoujo Tasukete! Mave-chan   
8140            Anime TV de Hakken! Tamagotchi   
4294      SKET Dance: SD Character Flash Anime   
1061                        Isshuukan Friends.   
2850                       Oshiete! Galko-chan   

                                             genre  
7363  Comedy, Parody, Sci-Fi, Shounen, Super Power  
8140          Comedy, Fantasy, Kids, Slice of Life  
4294                       Comedy, School, Shounen  
1061        Comedy, School, Shounen, Slice of Life  
2850                 Comedy, School, Slice of Life 
```

结果是所有的喜剧动漫，因为我们想要搞笑的动漫。它们大多数也包括了适合与朋友一起观看的类型。当然，如果我们有更详细的信息，推荐会更好。

## 结论

推荐系统是一种根据输入预测用户可能感兴趣内容的工具。使用 Hugging Face Transformers，我们可以构建一个利用嵌入和余弦相似度方法的推荐系统。嵌入方法非常强大，因为它能够考虑文本的语义关系和上下文意义。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据的技巧。Cornellius 在各种 AI 和机器学习主题上进行撰写。

### 更多相关主题

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [如何使用 GPT 生成创意内容与 Hugging Face](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [使用 Python 为亚马逊产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [实施推荐系统的十个关键经验教训](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [如何使用图数据库构建实时推荐引擎](https://www.kdnuggets.com/2023/08/build-realtime-recommendation-engine-graph-databases.html)
