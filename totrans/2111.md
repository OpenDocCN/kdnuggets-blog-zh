# 语义搜索与向量数据库

> 原文：[https://www.kdnuggets.com/semantic-search-with-vector-databases](https://www.kdnuggets.com/semantic-search-with-vector-databases)

![语义搜索与向量数据库](../Images/0d380decc923ed3d6b78479336b53c8b.png)

使用[Ideogram.ai](http://ideogram.ai)生成的图像

我相信我们大多数人都使用过搜索引擎。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

甚至有一句话叫“只需谷歌一下。”这句话意味着你应该使用谷歌的搜索引擎来寻找答案。这就是谷歌现在可以被普遍识别为搜索引擎的方式。

为什么搜索引擎如此有价值？搜索引擎允许用户使用有限的查询输入轻松获取互联网信息，并根据相关性和质量组织这些信息。反过来，搜索使得获取之前无法获得的大量知识成为可能。

传统上，搜索引擎获取信息的方法是基于词汇匹配或单词匹配。它运作良好，但有时结果可能不够准确，因为用户意图与输入文本不同。

例如，“红色连衣裙在黑暗中拍摄”这个输入可能有双重含义，特别是词语“拍摄”。更可能的含义是红色连衣裙的照片是在黑暗中拍摄的，但传统的搜索引擎不会理解这一点。这就是语义搜索兴起的原因。

语义搜索可以定义为一种考虑单词和句子含义的搜索引擎。语义搜索的结果是匹配查询含义的信息，这与传统搜索通过单词匹配查询的方式相对。

在自然语言处理（NLP）领域，向量数据库通过利用表示文本意义的高维向量的存储、索引和检索，显著提高了语义搜索能力。因此，语义搜索和向量数据库是密切相关的领域。

本文将讨论语义搜索以及如何使用向量数据库。有了这些信息，让我们深入了解吧。

# 语义搜索如何工作

让我们在向量数据库的背景下讨论语义搜索。

语义搜索的思路基于文本的意义，但我们如何捕捉这些信息呢？计算机无法像人类一样拥有感觉或知识，这意味着“意义”这个词需要指代其他东西。在语义搜索中，“意义”一词将成为适合有意义检索的知识表示。

意义表示形式是嵌入，将文本转换为带有数值信息的向量。例如，我们可以使用 OpenAI 嵌入模型将句子“I want to learn about Semantic Search”进行转换。

```py
[-0.027598874643445015, 0.005403674207627773, -0.03200408071279526, -0.0026835924945771694, -0.01792600005865097,...]
```

那么，这个数值向量是如何捕捉意义的呢？让我们退一步看。你上面看到的结果是句子的嵌入结果。如果你替换了上面句子中的任何一个单词，嵌入输出会有所不同。即使是一个单词也会有不同的嵌入输出。

如果我们看整个图景，单个词语的嵌入与完整句子的嵌入会有显著不同，因为句子嵌入考虑了词语之间的关系和句子的整体意义，而这些在单个词语的嵌入中没有体现。这意味着每个词、句子和文本在其嵌入结果中都是独一无二的。这就是嵌入如何捕捉意义，而不是词汇匹配的方式。

那么，语义搜索如何利用向量工作呢？语义搜索的目的是将你的语料库嵌入到一个向量空间中。这允许每个数据点提供信息（文本、句子、文档等）并成为一个坐标点。查询输入在搜索时会通过嵌入处理成向量，进入相同的向量空间。我们会使用向量相似性度量，如余弦相似度，从我们的语料库中找到与查询输入最接近的嵌入。为了更好地理解，你可以查看下面的图像。

![使用向量数据库的语义搜索](../Images/e2a936a4331997d952b0f9f1b182fa0b.png)

作者提供的图片

每个文档嵌入坐标被放置在向量空间中，查询嵌入也被放置在向量空间中。理论上，与输入的语义意义最接近的文档将被选中。

然而，维护包含所有坐标的向量空间将是一个巨大的任务，尤其是在大规模语料库的情况下。向量数据库更适合存储向量，而不是整个向量空间，因为它允许更好的向量计算，并能随着数据的增长保持效率。

在下面的图像中可以看到使用向量数据库进行语义搜索的高级过程。

![使用向量数据库的语义搜索](../Images/0bb964f620744ef930c6de098053c620.png)

作者提供的图片

在下一节中，我们将通过一个 Python 示例进行语义搜索。

# Python 实现

在本文中，我们将使用开源向量数据库 [Weaviate](https://weaviate.io/)。为了教学目的，我们还使用 Weaviate 云服务（WCS）来存储我们的向量。

首先，我们需要安装Weaviate Python包。

```py
pip install weaviate-client
```

接下来，请通过[Weaviate Console](https://console.weaviate.cloud/signin)注册他们的免费集群，并确保获得集群URL和API密钥。

对于数据集示例，我们将使用[Kaggle的法律文本数据](https://www.kaggle.com/datasets/amohankumar/legal-text-classification-dataset)。为了简化操作，我们还将只使用前100行数据。

```py
import pandas as pd
data = pd.read_csv('legal_text_classification.csv', nrows = 100)
```

![使用向量数据库的语义搜索](../Images/9f1726241c297e30476e590591e70cfc.png)

图片来源：作者

接下来，我们将把所有数据存储到Weaviate云服务上的向量数据库中。为此，我们需要设置与数据库的连接。

```py
import weaviate
import os
import requests
import json

cluster_url = "YOUR_CLUSTER_URL"
wcs_api_key = "YOUR_WCS_API_KEY"
Openai_api_key ="YOUR_OPENAI_API_KEY"

client = weaviate.connect_to_wcs(
    cluster_url=cluster_url,
    auth_credentials=weaviate.auth.AuthApiKey(wcs_api_key),
    headers={
        "X-OpenAI-Api-Key": openai_api_key
    }
)
```

接下来，我们需要连接到Weaviate云服务，并创建一个类（类似于SQL中的表）来存储所有文本数据。

```py
import weaviate.classes as wvc

client.connect()
legal_cases = client.collections.create(
    name="LegalCases",
    vectorizer_config=wvc.config.Configure.Vectorizer.text2vec_openai(),  
    generative_config=wvc.config.Configure.Generative.openai()  
)
```

在上面的代码中，我们创建了一个使用OpenAI Embedding模型的LegalCases类。在后台，存储在LegalCases类中的任何文本对象都会经过OpenAI Embedding模型，并作为嵌入向量进行存储。

让我们尝试将法律文本数据存储到向量数据库中。为此，你可以使用以下代码。

```py
sent_to_vdb = data.to_dict(orient='records')
legal_cases.data.insert_many(sent_to_vdb)
```

你应该会在Weaviate集群中看到你的法律文本数据已经存储在那里。

向量数据库准备好后，让我们尝试语义搜索。Weaviate API使这变得更加简单，如下代码所示。在下面的示例中，我们将尝试查找发生在澳大利亚的案件。

```py
response = legal_cases.query.near_text(
      query="Cases in Australia",
      limit=2
  )

for i in range(len(response.objects)):
  print(response.objects[i].properties)
```

结果如下所示。

```py
{'case_title': 'Castlemaine Tooheys Ltd v South Australia [1986] HCA 58 ; (1986) 161 CLR 148', 'case_id': 'Case11', 'case_text': 'Hexal Australia Pty Ltd v Roche Therapeutics Inc (2005) 66 IPR 325, the likelihood of irreparable harm was regarded by Stone J as, indeed, a separate element that had to be established by an applicant for an interlocutory injunction. Her Honour cited the well-known passage from the judgment of Mason ACJ in Castlemaine Tooheys Ltd v South Australia [1986] HCA 58 ; (1986) 161 CLR 148 (at 153) as support for that proposition.', 'case_outcome': 'cited'}

{'case_title': 'Deputy Commissioner of Taxation v ACN 080 122 587 Pty Ltd [2005] NSWSC 1247', 'case_id': 'Case97', 'case_text': 'both propositions are of some novelty in circumstances such as the present, counsel is correct in submitting that there is some support to be derived from the decisions of Young CJ in Eq in Deputy Commissioner of Taxation v ACN 080 122 587 Pty Ltd [2005] NSWSC 1247 and Austin J in Re Currabubula Holdings Pty Ltd (in liq); Ex parte Lord (2004) 48 ACSR 734; (2004) 22 ACLC 858, at least so far as standing is concerned.', 'case_outcome': 'cited'}
```

如你所见，我们得到了两个不同的结果。在第一个案例中，文档中直接提到了“澳大利亚”这个词，因此更容易找到。然而，第二个结果中没有任何“澳大利亚”这个词。但语义搜索可以找到它，因为有与“澳大利亚”相关的词汇，比如“NSWSC”（新南威尔士州最高法院的缩写）或“Currabubula”（澳大利亚的一个村庄）。

传统的词汇匹配可能会遗漏第二条记录，但语义搜索更加准确，因为它考虑了文档的含义。

这就是使用向量数据库进行简单语义搜索的实现。

# 结论

尽管传统的基于词汇匹配的搜索引擎主导了互联网的信息获取，但这种方法存在一个缺陷，即无法捕捉用户意图。这一局限性催生了**语义搜索**，一种能够解释文档查询含义的搜索引擎方法。通过向量数据库的增强，语义搜索的能力变得更加高效。

在本文中，我们探讨了**语义搜索**的工作原理以及如何使用开源的Weaviate向量数据库进行Python实现。希望这对你有所帮助！

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是数据科学助理经理和数据撰稿人。在 Allianz Indonesia 全职工作时，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 涉及多种 AI 和机器学习主题的撰写。

### 更多相关话题

+   [Python 向量数据库和向量索引：构建 LLM 应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [语义向量搜索如何变革客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [开源向量数据库的诚实比较](https://www.kdnuggets.com/an-honest-comparison-of-open-source-vector-databases)

+   [AI 和 LLM 使用案例中的向量数据库](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)

+   [什么是向量数据库，它们为何对 LLMs 重要？](https://www.kdnuggets.com/2023/06/vector-databases-important-llms.html)

+   [Pinecone 向量数据库的综合指南](https://www.kdnuggets.com/a-comprehensive-guide-to-pinecone-vector-databases)
