# 向量数据库在 AI 和 LLM 使用案例中的应用

> 原文：[https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)

![向量数据库在 AI 和 LLM 使用案例中的应用](../Images/819ba66755bbc22a8231bc910f0f7852.png)

使用 [Ideogram.ai](http://ideogram.ai) 生成的图像

因此，你可能会听到各种向量数据库的术语。有些人可能理解这些术语，有些人则不然。如果你不太了解也没关系，因为向量数据库近年来才成为一个更加突出的话题。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

向量数据库因生成式 AI，尤其是大型语言模型（LLM）的普及而变得越来越受欢迎。

许多大型语言模型产品，如 GPT-4 和 Gemini，通过为我们的输入提供文本生成能力来帮助我们的工作。实际上，向量数据库在这些大型语言模型产品中扮演了重要角色。

但是，向量数据库是如何工作的？它们在大型语言模型中的相关性是什么？

上述问题是我们将在本文中回答的。好吧，让我们一起来探索它们吧。

# 向量数据库

向量数据库是一种专门的数据库存储，用于存储、索引和查询向量数据。它通常针对高维向量数据进行了优化，因为这通常是机器学习模型，特别是大型语言模型的输出。

在向量数据库的背景下，向量是数据的数学表示。每个向量由表示数据位置的数值点数组成。向量通常用于大型语言模型中，因为向量比文本数据更容易处理。

在大型语言模型空间中，模型可能会有文本输入，并将其转换为表示文本语义和句法特征的高维向量。这一过程我们称之为嵌入。简单来说，嵌入是将文本转换为带有数值数据的向量的过程。

嵌入通常使用一种称为嵌入模型的神经网络模型来表示嵌入空间中的文本。

让我们使用一个示例文本：“我爱数据科学”。使用 OpenAI 的 text-embedding-3-small 模型表示它们会生成一个 1536 维的向量。

```py
[0.024739108979701996, -0.04105354845523834, 0.006121257785707712, -0.02210472710430622, 0.029098540544509888,...]
```

向量中的数字是模型嵌入空间中的坐标。它们共同形成了来自模型的句子意义的独特表示。

向量数据库将负责存储这些嵌入模型输出。用户可以根据需要查询、索引和检索向量。

也许介绍足够了，让我们进入更技术的实际操作。我们将尝试使用一个开源向量数据库 [Weaviate](https://weaviate.io/) 来建立和存储向量。

Weaviate 是一个可扩展的开源向量数据库，作为一个框架用于存储我们的向量。我们可以在 Docker 等实例中运行 Weaviate 或使用 Weaviate Cloud Services (WCS)。

要开始使用 Weaviate，我们需要使用以下代码安装相关包：

```py
pip install weaviate-client
```

为了简化操作，我们将使用 WCS 的沙箱集群作为我们的向量数据库。Weaviate 提供了一个 14 天的免费集群，我们可以使用它来存储向量，而无需注册任何支付方式。为此，您需要首先在他们的 [WCS 控制台](https://console.weaviate.cloud/signin) 上注册。

进入 WCS 平台后，选择创建集群并输入您的 Sandbox 名称。用户界面应如下图所示。

![AI和LLM用例中的向量数据库](../Images/4f9a0cb53ba4630631309397f523f2d8.png)

图片来源：作者

别忘了启用身份验证，因为我们还希望通过 WCS API 密钥访问该集群。集群准备好后，找到 API 密钥和集群 URL，我们将用来访问向量数据库。

一旦准备好，我们将模拟将第一个向量存储在向量数据库中。

对于向量数据库存储示例，我将使用 Kaggle 上的 [Book Collection](https://www.kaggle.com/datasets/kononenko/commonlit-texts) 示例数据集。我只会使用前 100 行和 3 列（标题、描述、简介）。

```py
import pandas as pd
data = pd.read_csv('commonlit_texts.csv', nrows = 100, usecols=['title', 'description', 'intro'])
```

让我们先把数据放在一边，连接到我们的向量数据库。首先，我们需要使用 API 密钥和集群 URL 设置远程连接。

```py
import weaviate
import os
import requests
import json

cluster_url = "Your Cluster URL"
wcs_api_key = "Your WCS API Key"
Openai_api_key ="Your OpenAI API Key"

client = weaviate.connect_to_wcs(
    cluster_url=cluster_url,
    auth_credentials=weaviate.auth.AuthApiKey(wcs_api_key),
    headers={
        "X-OpenAI-Api-Key": openai_api_key
    }
)
```

一旦设置好客户端变量，我们将连接到 Weaviate Cloud 服务并创建一个类来存储向量。Weaviate 中的类是数据集合，类似于关系数据库中的表名。

```py
import weaviate.classes as wvc

client.connect()
book_collection = client.collections.create(
    name="BookCollection",
    vectorizer_config=wvc.config.Configure.Vectorizer.text2vec_openai(),  
    generative_config=wvc.config.Configure.Generative.openai()  
)
```

在上面的代码中，我们连接到 Weaviate 集群并创建了一个 BookCollection 类。该类对象还使用了 OpenAI text2vec 嵌入模型对文本数据进行向量化，并使用 OpenAI 生成模块。

让我们尝试将文本数据存储到向量数据库中。为此，您可以使用以下代码。

```py
sent_to_vdb = data.to_dict(orient='records')
book_collection.data.insert_many(sent_to_vdb)
```

![AI和LLM用例中的向量数据库](../Images/405ed56bdc827843c29d85199652e61d.png)

图片来源：作者

我们已经成功地将数据集存储在向量数据库中！这有多简单？

现在，您可能对向量数据库与 LLM 的使用场景感到好奇。这就是我们接下来要讨论的内容。

# 向量数据库和 LLM 用例

一些 LLM 可以与向量数据库结合使用的用例。让我们一起探索这些用例。

## 语义搜索

语义搜索是通过使用查询的含义来搜索数据，以检索相关结果，而不是仅依赖传统的基于关键字的搜索。

这个过程涉及利用查询的LLM模型嵌入，并在我们的矢量数据库中进行嵌入相似性搜索。

让我们尝试使用Weaviate根据特定查询执行语义搜索。

```py
book_collection = client.collections.get("BookCollection")

client.connect()
response = book_collection.query.near_text(
      query="childhood story,
      limit=2
  )
```

在上述代码中，我们尝试使用Weaviate进行语义搜索，以查找与查询童年故事密切相关的前两本书。语义搜索使用我们之前设置的OpenAI嵌入模型。结果可以在下面看到。

```py
{'title': 'Act Your Age', 'description': 'A young girl is told over and over again to act her age.', 'intro': 'Colleen Archer has written for \nHighlights\n. In this short story, a young girl is told over and over again to act her age.\nAs you read, take notes on what Frances is doing when she is told to act her age. '}

{'title': 'The Anklet', 'description': 'A young woman must deal with unkind and spiteful treatment from her two older sisters.', 'intro': "Neil Philip is a writer and poet who has retold the best-known stories from \nThe Arabian Nights\n for a modern day audience. \nThe Arabian Nights\n is the English-language nickname frequently given to \nOne Thousand and One Arabian Nights\n, a collection of folk tales written and collected in the Middle East during the Islamic Golden Age of the 8th to 13th centuries. In this tale, a poor young woman must deal with mistreatment by members of her own family.\nAs you read, take notes on the youngest sister's actions and feelings."}
```

如你所见，上述结果中没有直接提及童年故事。然而，结果仍然与一个旨在儿童的故事密切相关。

## 生成搜索

生成搜索可以定义为语义搜索的扩展应用。生成搜索，或称为检索增强生成（RAG），利用LLM提示与从矢量数据库中检索的数据进行语义搜索。

使用RAG，查询搜索的结果被处理到LLM中，因此我们可以以我们想要的形式获得结果，而不是原始数据。让我们尝试使用矢量数据库进行RAG的简单实现。

```py
response = book_collection.generate.near_text(
    query="childhood story",
    limit=2,
    grouped_task="Write a short LinkedIn post about these books."
)

print(response.generated)
```

结果可以在下面的文本中看到。

```py
Excited to share two captivating short stories that explore themes of age and mistreatment. "Act Your Age" by Colleen Archer follows a young girl who is constantly told to act her age, while "The Anklet" by Neil Philip delves into the unkind treatment faced by a young woman from her older sisters. These thought-provoking tales will leave you reflecting on societal expectations and family dynamics. #ShortStories #Literature #BookRecommendations 📚
```

如你所见，数据内容与之前相同，但现在已经用OpenAI LLM处理，以提供一个简短的LinkedIn帖子。这样，当我们希望从数据中获得特定形式的输出时，RAG非常有用。

## RAG问答

在我们之前的示例中，我们使用查询获取了所需的数据，RAG将这些数据处理成预期的输出。

然而，我们可以将RAG功能转变为一个问答工具。我们可以通过将它们与LangChain框架结合来实现这一点。

首先，让我们安装必要的包。

```py
pip install langchain 
pip install langchain_community 
pip install langchain_openai 
```

然后，让我们尝试导入所需的包并初始化变量，以使RAG问答工作。

```py
from langchain.chains import RetrievalQA
from langchain_community.vectorstores import Weaviate
import weaviate
from langchain_openai import OpenAIEmbeddings
from langchain_openai.llms.base import OpenAI

llm = OpenAI(openai_api_key = openai_api_key, model_name = 'gpt-3.5-turbo-instruct', temperature = 1)

embeddings = OpenAIEmbeddings(openai_api_key = openai_api_key )

client = weaviate.Client(
    url=cluster_url, auth_client_secret=weaviate.AuthApiKey(wcs_api_key)
)
```

在上述代码中，我们设置了LLM用于文本生成、嵌入模型和Weaviate客户端连接。

接下来，我们将Weaviate连接设置为矢量数据库。

```py
weaviate_vectorstore = Weaviate(client=client, index_name='BookCollection', text_key='intro',by_text = False, embedding=embeddings)
retriever = weaviate_vectorstore.as_retriever()
```

在上述代码中，将Weaviate数据库BookCollection设置为RAG工具，以便在提示时搜索‘intro’特性。

然后，我们将使用下面的代码从LangChain创建问答链。

```py
qa_chain = RetrievalQA.from_chain_type(
    llm=llm, chain_type="stuff", retriever = retriever
)
```

一切现在都准备好了。让我们尝试使用下面的代码示例进行RAG问答。

```py
response = qa_chain.invoke(
    "Who is the writer who write about love between two goldfish?")
print(response)
```

结果显示在下面的文本中。

```py
{'query': 'Who is the writer who write about love between two goldfish?', 'result': ' The writer is Grace Chua.'}
```

使用矢量数据库作为存储所有文本数据的地方，我们可以实现RAG与LangChain一起执行问答。这有多棒？

# 结论

向量数据库是一种专门的存储解决方案，旨在存储、索引和查询向量数据。它通常用于存储文本数据，并与大型语言模型（LLMs）结合实施。本文将尝试进行向量数据库 Weaviate 的动手设置，包括语义搜索、检索增强生成（RAG）和基于 RAG 的问答等示例用例。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰写员。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉猎多种人工智能和机器学习主题。

### 更多相关主题

+   [Python 向量数据库和向量索引：LLM 应用的架构设计](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [NoSQL 数据库及其应用场景](https://www.kdnuggets.com/2023/03/nosql-databases-cases.html)

+   [什么是向量数据库，它们为何对 LLMs 重要？](https://www.kdnuggets.com/2023/06/vector-databases-important-llms.html)

+   [对开源向量数据库的诚实比较](https://www.kdnuggets.com/an-honest-comparison-of-open-source-vector-databases)

+   [使用向量数据库进行语义搜索](https://www.kdnuggets.com/semantic-search-with-vector-databases)

+   [Pinecone 向量数据库全面指南](https://www.kdnuggets.com/a-comprehensive-guide-to-pinecone-vector-databases)
