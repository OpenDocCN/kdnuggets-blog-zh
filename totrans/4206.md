# 使用 Python、SpaCy 和 Streamlit 构建结构化金融新闻源

> 原文：[https://www.kdnuggets.com/2021/09/-structured-financial-newsfeed-using-python-spacy-and-streamlit.html](https://www.kdnuggets.com/2021/09/-structured-financial-newsfeed-using-python-spacy-and-streamlit.html)

[评论](#comments)

**由 [Harshit Tyagi](https://www.linkedin.com/in/tyagiharshit/)，数据科学讲师 | 导师 | YouTuber**

![使用 Python、SpaCy 和 Streamlit 构建结构化金融新闻源](../Images/525c4bf0536a3f880f235189e8c33d1c.png)

自然语言处理的一个非常有趣且广泛使用的应用是命名实体识别（NER）。

从原始和非结构化数据中获取洞察至关重要。上传文档并从中提取重要信息被称为信息检索。

信息检索一直是自然语言处理中的一项主要任务/挑战。命名实体识别（或命名实体链接）在多个领域（如金融、药品、电子商务等）中用于信息检索目的。

在这个教程帖子中，我将展示你如何利用 NEL 开发一个自定义的股票市场新闻源，列出互联网上的热门股票。

## 先决条件

没有特别的先决条件。你可能需要对 Python 和 NLP 的基本任务（如分词、词性标注、依存解析等）有一些了解。

我将详细介绍重要的部分，所以即使你是完全的初学者，也能理解发生了什么。

所以，让我们开始吧，跟着做，你将会有一个简洁的股票新闻源，你可以开始研究了。

## **你需要的工具/设置：**

1.  Google Colab 用于数据和 SpaCy 库的初步测试和探索。

1.  使用 VS Code（或任何编辑器）来编写 Streamlit 应用程序。

1.  股票市场信息（新闻）的来源，我们将对其进行 NER 和后续的 NEL。

1.  需要一个虚拟的 Python 环境（我使用的是 conda），以及 Pandas、SpaCy、Streamlit 和 Streamlit-Spacy（如果你想展示一些 SpaCy 渲染结果的话）等库。

## 目标

本项目的目标是学习并应用命名实体识别，提取重要的实体（在我们的示例中为上市公司），然后使用知识库（Nifty500 公司名单）将每个实体与一些信息进行链接。

我们将从互联网上的 RSS 源获取文本数据，提取热门股票的名称，然后获取这些股票的市场价格数据，以在对这些股票采取任何操作之前测试新闻的真实性。

> 注意：命名实体识别可能不是最前沿的问题，但在行业中有许多应用。

继续使用 Google Colab 进行实验和测试：

## 第一步：提取热门股票新闻数据

为了获得可靠的真实股市新闻，我将使用[Economic Times](https://economictimes.indiatimes.com/markets/stocks/rssfeeds/2146842.cms)和[Money Control](https://www.moneycontrol.com/rss/buzzingstocks.xml) RSS 源进行本教程，但你也可以使用/添加你所在国家的 RSS 源或 Twitter/Telegram（群组）数据，以使你的源更具信息性/准确性。

机会是巨大的。本教程应作为应用 NEL 构建不同领域应用程序的垫脚石，解决不同类型的信息检索问题。

如果你查看 RSS 源，它看起来像这样：

![](../Images/cc1a6f5d622cf5f3e83e7bc7d0f6315a.png)

[https://economictimes.indiatimes.com/markets/rssfeeds/1977021501.cms](https://economictimes.indiatimes.com/markets/rssfeeds/1977021501.cms)

我们的目标是从这个 RSS 源中获取文本标题，然后使用 SpaCy 提取标题中的主要实体。

标题位于 XML 的<title>标签中。

首先，我们需要捕获整个 XML 文档，可以使用`**requests**`库来完成。确保你在 colab 的运行时环境中安装了这些包。

你可以运行以下命令来从 colab 的代码单元中安装几乎任何包：

```py
!pip install <package_name>
```

发送一个`GET`请求到提供的链接以获取 XML 文档。

```py
import requestsresp = requests.get("https://economictimes.indiatimes.com/markets/stocks/rssfeeds/2146842.cms")
```

运行单元以检查响应对象中得到的内容。

它应该会给你一个带有 HTTP 代码 200 的成功响应，如下所示：

![](../Images/32fa6a5235798fcfe47128fcc891a836.png)

现在你有了这个响应对象，我们可以将其内容传递给 BeautifulSoup 类来解析 XML 文档，如下所示：

```py
from bs4 import BeautifulSoupsoup = BeautifulSoup(resp.content, features='xml')
soup.findAll('title')
```

这将给你一个包含所有标题的 Python 列表：

![](../Images/d243f2acf112117879f8e373ee8221c3.png)

图片由作者提供

太棒了，我们已经得到了文本数据，我们将使用 NLP 从中提取主要实体（在本例中是上市公司）。

现在是将 NLP 应用到实践中的时候了。

## 第 2 步：从标题中提取实体

这是激动人心的部分。我们将使用来自`**spaCy**`库的**预训练核心语言模型**来提取标题中的主要实体。

关于 spaCy 和核心模型的简要介绍。

**spaCy**是一个开源 NLP 库，以超快的速度处理文本数据。它是 NLP 研究中的领先库，被广泛用于企业级应用中。spaCy 以其适应问题的能力而闻名，并且支持超过 64 种语言，能够很好地与 TensorFlow 和 PyTorch 兼容。

说到核心模型，spaCy 具有两个主要类别的预训练语言模型，这些模型在不同大小的文本数据上进行训练，以提供最先进的推断。

1.  核心模型——用于通用的基础 NLP 任务。

1.  起始模型——用于需要迁移学习的特定应用程序。我们可以利用模型的学习权重来微调我们的自定义模型，而无需从头开始训练模型。

由于我们在这个教程中的用例是基本的，我们将继续使用 `en_core_web_sm` 核心模型管道。

那么，让我们将它加载到笔记本中：

```py
nlp = spacy.load("en_core_web_sm")
```

***注意：*** Colab 已经为我们下载了这个模型，但如果你尝试在本地系统中运行，你需要使用以下命令首先下载模型：

```py
python -m spacy [download](https://spacy.io/api/cli#download) en_core_web_sm
```

`en_core_web_sm` 基本上是一个针对 CPU 优化的英语管道，具有以下组件：

+   tok2vec — 将令牌转换为向量（对文本数据进行标记化），

+   tagger — 为每个令牌添加相关的元数据。spaCy 利用一些统计模型来预测每个令牌的词性（POS）。更多信息请参见 [文档](https://spacy.io/models/en)。

+   parser — 依赖解析器在令牌之间建立关系。

+   其他组件包括 senter、ner、attribute_ruler、lemmatizer。

现在，为了测试这个模型能为我们做什么，我会将一个单独的标题传递给实例化的模型，然后检查句子的不同部分。

```py
# make sure you extract the text out of <title> tagsprocessed_hline = nlp(headlines[4].text)
```

该管道执行从标记化到命名实体识别（NER）的所有任务。这里我们首先得到令牌：

![](../Images/6d2917dbe55b3aaa0813afa881b488a9.png)

图片来源于作者

你可以使用 `pos_` 属性查看标记的词性。

![](../Images/5efb57780795368a90a99a8bafe56987.png)

图片来源于作者

每个令牌都带有一些元数据。例如，“Trade”是专有名词，“Setup”是名词，“:`” 是标点符号，等等。所有标签的完整列表可以在 [这里](https://spacy.io/models/en) 找到。

然后，你可以通过查看依赖图来了解它们之间的关系，使用 `dep_` 属性：

![](../Images/45aabe9dbacbb7638e622b37d4f2b7a8.png)

图片来源于作者

这里，“Trade”是复合词，“Setup”是根词，“Nifty”是同位语修饰语。再次说明，所有语法标签可以在 [这里](https://spacy.io/models/en) 找到。

你还可以使用以下的 displacy `render()` 方法来可视化令牌之间的关系依赖：

```py
spacy.displacy.render(processed_hline, style='dep',jupyter=True, options={'distance': 120})
```

这将生成如下图表：

![](../Images/aaf67d47e41b6990f01d0a7e67f61cc6.png)

图片来源于作者

## 实体提取

要查看句子的主要实体，你可以在同一代码中将 `**'ent’**` 作为样式传递：

![](../Images/aca72aeee53f1c5d7988bf6430bc89e4.png)

图片来源于作者 — 我使用了另一个标题，因为我们上面用的那个没有任何实体。

我们对不同的实体有不同的标签，例如“day”有 DATE 标签，“Glasscoat”有 GPE 标签，可以是国家/城市/州。我们主要寻找带有 ORG 标签的实体，这些标签能给我们公司、机构、组织等信息。

我们现在能够从文本中提取实体。让我们来提取所有标题中的组织实体。

这将返回如下的公司列表：

![](../Images/ea49df1fea8fe173af6aa627ea237b75.png)

图片来源于作者

很简单，对吧？

这就是 spaCy 的魔力！

下一步是查找所有这些公司在知识库中，以提取该公司的正确股票符号，然后使用像yahoo-finance这样的库提取市场详情，如价格、收益等。

## 第三步 — 命名实体链接

了解市场上哪些股票在活跃，并在你的仪表板上获取其详细信息是这个项目的目标。

我们有公司名称，但为了获取它们的交易详情，我们需要公司的交易股票符号。

由于我在提取印度公司的详细信息和新闻，我将使用[Nifty 500公司（一个CSV文件）](https://www1.nseindia.com/products/content/equities/indices/nifty_500.htm)的外部数据库。

对于每家公司，我们将使用pandas在公司列表中查找它，然后使用[yahoo-finance](https://pypi.org/project/yfinance/)库捕获股票市场统计数据。

图片由作者提供

你应该注意到的一点是，我在将每个股票符号传递给`yfinance`库的`Ticker`类之前，添加了一个`.NS`后缀。这是因为印度NSE股票符号在`yfinance`中以`.NS`后缀存储。

之后，流行的股票将会出现在如下的数据框中：

![](../Images/8a464b3bde7133a42af081ddc2a1cce0.png)

图片由作者提供

太好了！这是不是很棒？这样一个简单却深刻的应用程序，可以帮助你找到正确的股票方向。

现在，为了使其更易于访问，我们可以使用Streamlit将刚刚编写的代码创建为Web应用程序。

## 第四步 — 使用Streamlit构建Web应用程序

该是移动到编辑器，创建一个新项目和虚拟环境来进行NLP应用程序的时候了。

开始使用Streamlit对于这样的演示数据应用程序非常简单。确保你已经安装了streamlit。

```py
pip install Streamlit
```

现在，让我们创建一个名为app.py的新文件，并开始编写功能代码以准备应用程序。

在顶部导入所有所需的库。

```py
import pandas as pdimport requestsimport spacyimport streamlit as stfrom bs4 import BeautifulSoupimport yfinance as yf
```

给你的应用程序添加一个标题：

```py
st.title('Buzzing Stocks :zap:')
```

通过在终端中运行`streamlit run app.p`y来测试你的应用程序。它应该会在你的Web浏览器中打开一个应用程序。

我添加了一些额外的功能，以从多个来源捕获数据。现在，你可以将你选择的RSS源URL添加到应用程序中，数据将被处理，趋势股票将在数据框中显示出来。

要访问完整的代码库，你可以查看我的仓库：

**[GitHub - dswh/NER_News_Feed](https://github.com/dswh/NER_News_Feed)**

你可以添加多个样式元素、不同的数据源和其他类型的处理，以提高其效率和实用性。

我当前状态下的应用程序看起来像横幅中的图片。

如果你想逐步跟随我，请在这里观看我编写这个应用程序的过程：

## 下一步！

除了选择一个金融应用案例外，你也可以选择其他你喜欢的应用。医疗保健、电子商务、研究等等。所有行业都需要处理文档并提取和链接重要实体。尝试另一个想法。

一个简单的想法是提取研究论文中所有重要的实体，然后使用谷歌搜索 API 创建一个知识图谱。

此外，如果你想将股票新闻推送应用提升到另一个水平，你还可以添加一些交易算法来生成买卖信号。

我鼓励你放飞你的想象力。

## 如何与我联系！

如果你喜欢这篇文章并想看到更多类似内容，你可以订阅[**我的新闻通讯**](https://dswharshit.substack.com/publish/settings#twitter-account)**或**[**我的 YouTube 频道**](https://www.youtube.com/channel/UCH-xwLTKQaABNs2QmGxK2bQ)，我会继续分享这样有用且快捷的项目。

如果你是刚开始编程的人或想进入数据科学或机器学习领域，你可以查看我在[**WIP Lane Academy**](https://www.wiplane.com/p/foundations-for-data-science-ml)**的课程**。

感谢 Elliot Gunn。

**简介：[Harshit Tyagi](https://www.linkedin.com/in/tyagiharshit/)** 是一位具有综合网页技术和数据科学（即全栈数据科学）经验的工程师。他已经指导了超过1000名AI/网页/数据科学的求职者，并设计了数据科学和机器学习工程学习课程。此前，Harshit与耶鲁大学、麻省理工学院和加州大学洛杉矶分校的研究科学家一起开发了数据处理算法。

[原文](https://dswharshit.medium.com/d19736fdd70c). 经许可转载。

**相关：**

+   [2021年数据科学学习路线图](/2021/02/data-science-learning-roadmap-2021.html)

+   [机器学习如何利用线性代数解决数据问题](/2021/09/machine-learning-leverages-linear-algebra-solve-data-problems.html)

+   [学习数据科学和机器学习：路线图后的第一步](/2021/08/learn-data-science-machine-learning.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护。

* * *

### 更多相关内容

+   [构建机器学习模型的结构化方法](https://www.kdnuggets.com/2022/06/structured-approach-building-machine-learning-model.html)

+   [使用 spaCy 进行 NLP 入门](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)

+   [使用spaCy进行自然语言处理](https://www.kdnuggets.com/2023/01/natural-language-processing-spacy.html)

+   [使用DAGsHub将Streamlit WebApp部署到Heroku](https://www.kdnuggets.com/2022/02/deploying-streamlit-webapp-heroku-dagshub.html)

+   [使用HuggingFace Pipelines和Streamlit回答问题](https://www.kdnuggets.com/2021/10/simple-question-answering-web-app-hugging-face-pipelines.html)

+   [使用Streamlit进行DIY自动化机器学习](https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html)
