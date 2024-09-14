# 如何通过 API 创建和部署一个简单的情感分析应用

> 原文：[https://www.kdnuggets.com/2021/06/create-deploy-sentiment-analysis-app-api.html](https://www.kdnuggets.com/2021/06/create-deploy-sentiment-analysis-app-api.html)

[评论](#comments)![图](../Images/577cfe14fcb8f4b13d547bfba1b66c5c.png)

图片来源：[Reputation X](https://blog.reputationx.com/reputation-sentiment)

假设你已经为某个特定任务构建了一个 NLP 模型，无论是文本分类、问题回答、翻译还是其他什么。你已经在本地测试过，表现良好。你也让其他人测试过，表现依旧良好。现在你想将其推广到更大的受众，不管是你合作的开发团队、特定的终端用户群体，还是普通公众。你决定使用 REST API，因为这是你认为的最佳选项。接下来怎么办？

FastAPI 可能会有所帮助。[FastAPI](https://fastapi.tiangolo.com/) 是一个用于构建 Python API 的 Web 框架。我们将在本文中使用 FastAPI 构建一个 REST API，服务于一个 NLP 模型，该模型可以通过 GET 请求进行查询，并对这些查询作出响应。

在这个例子中，我们将跳过构建自己的模型，而是利用 [HuggingFace](https://huggingface.co/transformers/) Transformers 库的 [Pipeline 类](https://huggingface.co/transformers/main_classes/pipelines.html)。[Transformers](https://huggingface.co/transformers/) 中充满了可以直接使用的 SOTA NLP 模型，还可以根据特定用途和高性能进行微调。该库的管道可以总结为：

> 这些管道是使用模型进行推理的一个极好的简单方式。这些管道是抽象了库中大部分复杂代码的对象，提供了一个简单的 API，专门用于几个任务，包括命名实体识别、掩码语言建模、情感分析、特征提取和问题回答。

使用 Transformers 库、FastAPI 和极少的代码，我们将创建并部署一个非常简单的 [情感分析](https://www.kdnuggets.com/2018/08/emotion-sentiment-analysis-practitioners-guide-nlp-5.html) 应用。我们还将看到将这种方法扩展到更复杂的应用程序是相当简单的。

### 开始入门

如上所述，我们将使用 Transformers 和 FastAPI 来构建这个应用，这意味着你需要在系统上安装这些工具。你还需要安装 [Uvicorn](https://www.uvicorn.org/)，这是 FastAPI 作为后端所依赖的 [ASGI 服务器](https://asgi.readthedocs.io/en/latest/)。我在我的 *buntu 系统上通过 pip 轻松安装了它们。

```py
pip install transformers
pip install fastapi
pip install uvicorn
```

就这些，接下来…

### 分析情感

由于我们将使用 Transformers 来构建我们的 NLP 流水线，我们先来看看如何使其单独工作。这非常简单，只需将很少的基本参数传递给流水线对象即可开始。具体而言，我们需要定义一个任务——我们想做什么——和一个模型——我们将用来执行任务的东西。其实就这些。我们可以选择提供额外的参数或将流水线微调到我们的特定任务和数据，但对于我们的目的来说，直接使用现成的模型应该就足够了。

对我们来说，任务是`sentiment-analysis`，模型是`[nlptown/bert-base-multilingual-uncased-sentiment](https://huggingface.co/nlptown/bert-base-multilingual-uncased-sentiment)`。这是一个用于多语言情感分析的 BERT 模型，由 [NLP Town](https://www.nlp.town/) 贡献到 HuggingFace 模型库。请注意，第一次运行该脚本时，会将大型模型下载到系统中，因此请确保有足够的空闲空间。

下面是设置独立情感分析应用的代码：

```py
from transformers import pipeline

text = 'i love this movie!!! :)'

# Instantiate a pipeline object with our task and model passed as parameters
nlp = pipeline(task='sentiment-analysis', 
               model='nlptown/bert-base-multilingual-uncased-sentiment')

# Pass the text to our pipeline and print the results
print(f'{nlp(text)}')
```

调用流水线对象返回的是预测标签及其相应的概率。在我们的案例中，任务和模型的组合会产生 1（负面）到 5（正面）之间的标签，以及预测概率。让我们运行一下脚本看看它的表现。

```py
python test_model.py

>>> [{'label': '5 stars', 'score': 0.923753023147583}]
```

这就是使用 Transformers 库及其 Pipeline 类将一些功能基本的 NLP 任务特定的现成模型启动并运行所需要的全部内容。

如果你想在我们继续将其通过 REST API 部署之前再多测试一下，可以尝试这个修改过的脚本，它允许我们从命令行传递文本，并以更好格式化的回复响应：

```py
import sys
from transformers import pipeline

if len(sys.argv) != 2:
    print('Usage: python model_test.py <input_string>')
    sys.exit(1)

text = sys.argv[1]

# Instantiate a pipeline object with our task and model passed as parameters
nlp = pipeline(task='sentiment-analysis',
               model='nlptown/bert-base-multilingual-uncased-sentiment')

# Get and process result
result = nlp(text)

sent = ''
if (result[0]['label'] == '1 star'):
    sent = 'very negative'
elif (result[0]['label'] == '2 star'):
    sent = 'negative'
elif (result[0]['label'] == '3 stars'):
    sent = 'neutral'
elif (result[0]['label'] == '4 stars'):
    sent = 'positive'
else:
    sent = 'very positive'

prob = result[0]['score']

# Format and print results
print(f"{{'sentiment': '{sent}', 'probability': '{prob}'}}")
```

让我们运行几次，看看它的表现：

```py
python model_test.py 'the sky is blue'

>>> {'sentiment': 'neutral', 'probability': '0.2726307213306427'}

python model_test.py 'i really hate this restaurant!'

>>> {'sentiment': 'very negative', 'probability': '0.9228281378746033'}

python model_test.py 'i love this movie!!! :)'

>>> {'sentiment': 'very positive', 'probability': '0.923753023147583'}
```

这具有更好的功能，因为我们不必将要分析的文本硬编码到程序中，并且我们还使结果更具用户友好性。让我们扩展这个更有用的版本，并将其作为 REST API 部署。

### 创建 API

现在是通过 FastAPI 部署这个非常简单的情感分析应用的时候了。如果你想了解更多关于 FastAPI 的信息以及如何使用这个框架格式化你的代码，请查看其文档。你需要知道的是，我们将创建 FastAPI 的一个实例（`app`），然后必须定义获取请求，将其附加到 URL，并通过函数为这些请求分配响应。

下面，我们将定义两个这样的 GET 请求；一个用于根 URL (`'/'`)，显示欢迎信息，另一个用于接受字符串以进行情感分析 (`'/sentiment_analysis/'`)。这两个请求的代码都非常简单；你应该能识别出`'/sentiment_analysis/'`请求调用的`analyze_sentiment()`函数中的大部分内容。

```py
from transformers import pipeline
from fastapi import FastAPI

nlp = pipeline(task='sentiment-analysis',
               model='nlptown/bert-base-multilingual-uncased-sentiment')

app = FastAPI()

@app.get('/')
def get_root():
    return {'message': 'This is the sentiment analysis app'}

@app.get('/sentiment_analysis/')
async def query_sentiment_analysis(text: str):
    return analyze_sentiment(text)

def analyze_sentiment(text):
    """Get and process result"""

    result = nlp(text)

    sent = ''
    if (result[0]['label'] == '1 star'):
        sent = 'very negative'
    elif (result[0]['label'] == '2 star'):
        sent = 'negative'
    elif (result[0]['label'] == '3 stars'):
        sent = 'neutral'
    elif (result[0]['label'] == '4 stars'):
        sent = 'positive'
    else:
        sent = 'very positive'

    prob = result[0]['score']

    # Format and return results
    return {'sentiment': sent, 'probability': prob}
```

现在我们有了一个能够接受 GET 请求并进行情感分析的 REST API。在尝试之前，我们必须运行 Uvicorn 网络服务器，它将提供必要的后台功能。为此，假设你已将上述代码保存在名为 `main.py` 的文件中，并将 FastAPI 实例的名称保留为 `app`，运行以下命令：

```py
 uvicorn main:app --reload
```

你应该会看到这样的结果：

```py
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [18271] using statreload
INFO:     Started server process [18273]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

按指示打开浏览器，访问 http://127.0.0.1:8000，你应该会看到：

![API response](../Images/7fcd0ce954d7df772c423d60d1414df4.png)

如果你看到欢迎信息，恭喜，成功了！我们可以尝试使用浏览器地址栏进行一些请求，将下面引号中的内容粘贴到请求 URL 和查询字符串后面 (`http://127.0.0.1:8000/sentiment_analysis/?text=`)：

**“欢迎来到我的家！”**

![API response](../Images/d0d6c8735adcaceda7fa038bfce7dcad.png)

**“我不喜欢你的猫”**

![API response](../Images/30aebef4b84ba3aab21496426d743fe8.png)

**“那部电影只是一般般”**

![API response](../Images/b82560d7baf0820f9857c290570c40e6.png)

很好，我们得到了结果！那如果我们想把它当作 API 来访问呢？在 Python 中，我们可以使用 requests 库。确保它已经安装，可以使用以下命令：

```py
pip install requests
```

然后试试这样的脚本：

```py
import requests

query = {'text':'i love the fettucine alfredo and would definitely recommend this restaurant to my friends!'}
response = requests.get('http://127.0.0.1:8000/sentiment_analysis/', params=query)
print(response.json())
```

保存后，执行脚本，你应该会得到如下结果：

```py
python rest_request.py

>>> {'sentiment': 'very positive', 'probability': 0.8293750882148743}

```

这也成功了。太好了！

你可以在 [这里](https://docs.python-requests.org/en/master/) 阅读更多关于 requests 库的信息。

### 结论

显然，我们本可以做得更多。数据预处理会很有用。进行错误检查也是明智的。作为确认，尝试使用一些奇怪的字符或不将冗长的请求用标点符号括起来，看看会发生什么。

下次我保证我们将扩展今天所做的内容，制作一些更强大、更有趣的东西。我已经在着手准备了。

**相关内容**：

+   [使用 5 个重要的自然语言处理库入门](/2021/02/getting-started-5-essential-nlp-libraries.html)

+   [立即学习自然语言处理的神经网络](/2021/04/learn-neural-networks-natural-language-processing-now.html)

+   [使用 TPOT 进行机器学习管道优化](/2021/05/machine-learning-pipeline-optimization-tpot.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 了解更多相关主题

+   [使用 Heroku 部署机器学习 Web 应用](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)

+   [如何收集客户情感分析的数据](https://www.kdnuggets.com/2022/12/collect-data-customer-sentiment-analysis.html)

+   [使用同态加密对加密数据进行情感分析](https://www.kdnuggets.com/2022/12/zama-sentiment-analysis-encrypted-data-homomorphic-encryption.html)

+   [Python 中的情感分析：超越词袋模型](https://www.kdnuggets.com/sentiment-analysis-in-python-going-beyond-bag-of-words)

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [如何成功部署数据科学项目](https://www.kdnuggets.com/2022/01/successfully-deploy-data-science-projects.html)
