# 如何将机器学习/深度学习模型部署到网络

> 原文：[`www.kdnuggets.com/2021/04/deploy-machine-learning-models-to-web.html`](https://www.kdnuggets.com/2021/04/deploy-machine-learning-models-to-web.html)

评论

![](img/21d362b53192e6a8cdf55bee781e6668.png)

如果你已经在机器学习领域工作了一段时间，你一定创建过一些机器学习或深度学习模型。你一定会想，人们会如何使用你的 Jupyter notebook？答案是*他们不会*。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加入网络安全职业的快车道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

人们无法使用你的 Jupyter notebooks，你需要将你的模型部署为 API 或完整的网络服务，或在移动设备、Raspberry PI 等上。

在本文中，你将学习如何将深度学习模型部署为 REST API，并添加一个表单以获取用户输入，并返回模型的预测结果。

我们将使用 FastAPI 创建 API 并免费部署到 Heroku 上。

### 第 1 步：安装

你需要安装必要的软件包。

**1\. FastAPI + Uvicorn**

我们将使用 FastAPI 来处理 API，并使用 Uvicorn 服务器来运行和托管这个 API。

```py
$ pip install fastapi uvicorn

```

**2\. Tensorflow 2**

我们将在本教程中使用 Tensorflow 2，你可以使用你选择的框架。

```py
$ pip install tensorflow==2.0.0

```

**3\. Heroku**

你可以在 Ubuntu 上通过终端直接使用以下命令安装 Heroku，

```py
$ sudo snap install --classic heroku

```

在 macOS 上，你可以通过以下方式安装，

```py
$ brew tap heroku/brew && brew install heroku

```

对于 Windows，你可以从官方 [网站](https://devcenter.heroku.com/articles/heroku-cli) 安装压缩文件。

**4\. Git**

你还需要安装 git 并在 GitHub 上创建一个帐户，以便我们可以直接推送到 GitHub 并将主分支连接到我们的 Heroku，这样它就会自动部署。

你可以使用*apt*在 Debian 上安装 git。

```py
$ sudo apt install git-all

```

要在 Windows 上安装，你可以直接从 [这里](https://git-scm.com/download/win)下载。

要在 macOS 上安装，你可以安装 XCode 命令行工具并运行以下命令来激活它，

```py
git --version

```

你也可以从 macOS 上的 [git 官网](https://git-scm.com/download/mac) 安装。

### 第 2 步：创建我们的深度学习模型

我们将创建一个简单的深度学习模型，该模型与情感分析相关。使用的数据集可以从 [Kaggle](https://www.kaggle.com/crowdflower/first-gop-debate-twitter-sentiment)下载，与 GOP 推文相关。

我们将创建这个模型、训练它并保存，以便我们可以在 API 中使用保存的模型，而不必每次 API 启动时都训练模型权重。我们将在文件 **model.py** 中创建这个模型。

```py
import pandas as pd
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Embedding, LSTM, SpatialDropout1D
from sklearn.model_selection import train_test_split
import re

```

在这里，我们导入了重要的库，这些库将帮助我们创建模型和清理数据。我不会深入探讨深度学习模型或 Tensorflow 的工作原理。有关详细信息，你可以查看 [KDnuggets](https://www.kdnuggets.com/2020/07/getting-started-tensorflow2.html) 上的文章，对于情感分析模型的工作，请查看 [CNVRG](https://cnvrg.io/sentiment-analysis-python) 上的文章。

我们将使用 Pandas 读取数据。

```py
data = pd.read_csv('archive/Sentiment.csv')

# Keeping only the neccessary columns
data = data[['text','sentiment']]

```

我们将创建一个函数，使用正则表达式移除推文中的不需要的字符。

```py
def preProcess_data(text):
   text = text.lower()
   new_text = re.sub('[^a-zA-z0-9\s]','',text)
   new_text = re.sub('rt', '', new_text)
   return new_text

data['text'] = data['text'].apply(preProcess_data)

```

我们将使用 Tensorflow 的分词器对数据集进行分词，并使用 Tensorflow 的 pad_sequences 对序列进行填充。

```py
max_fatures = 2000

tokenizer = Tokenizer(num_words=max_fatures, split=' ')
tokenizer.fit_on_texts(data['text'].values)
X = tokenizer.texts_to_sequences(data['text'].values)
X = pad_sequences(X, 28) 

Y = pd.get_dummies(data['sentiment']).values

```

现在我们将数据集拆分为训练集和测试集。

```py
X_train, X_test, Y_train, Y_test = train_test_split(X,Y, test_size = 0.20)

```

现在是设计和创建深度学习模型的时候了。我们将简单地使用嵌入层和一些带有 dropout 的 LSTM 层。

```py
embed_dim = 128
lstm_out = 196

model = Sequential()
model.add(Embedding(max_fatures, embed_dim,input_length = X.shape[1]))
model.add(SpatialDropout1D(0.4))
model.add(LSTM(lstm_out, dropout=0.3, recurrent_dropout=0.2, return_sequences=True))
model.add(LSTM(128,recurrent_dropout=0.2))
model.add(Dense(3,activation='softmax'))

model.compile(loss = 'categorical_crossentropy', optimizer='adam',metrics = ['accuracy'])

```

我们现在将拟合模型。

```py
batch_size = 512

model.fit(X_train, Y_train, epochs = 10, batch_size=batch_size, validation_data=(X_test, Y_test))

```

现在深度学习模型已经训练完成，我们将保存模型，以便每次重新加载服务器时无需重新训练。我们只需使用训练好的模型。请注意，我没有进行太多超参数调优或模型改进，你可以自行进行以部署改进后的模型。

```py
model.save('sentiment.h5')

```

在这里，我们已将模型保存为 'hdf5' 格式。你可以在 [这篇文章](https://www.kdnuggets.com/2021/02/saving-loading-models-tensorflow.html) 中了解更多关于模型保存和加载的信息。

### 第 3 步：使用 FAST API 创建 REST API

我们将使用 FAST API 创建一个 REST API。我们将创建一个名为 **app.py** 的新文件。我们将首先进行重要的导入。

```py
import numpy as np
from fastapi import FastAPI, Form
import pandas as pd
from starlette.responses import HTMLResponse
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
import tensorflow as tf
import re

```

在这里，我们从 FastAPI 库中导入了 *FastAPI* 和 *Form*，通过这些我们将为 API 创建一个输入表单和端点。我们还从 *starlette.response* 中导入了 *HTMLResponse*，这将帮助我们创建输入表单。

我们将首先创建一个输入表单，以便用户可以输入数据，即我们可以测试情感的测试字符串。

```py
app = FastAPI()

@app.get('/predict', response_class=HTMLResponse)
def take_inp():
    return '''
        <form method="post">
        <input maxlength="28" name="text" type="text" value="Text Emotion to be tested" />
        <input type="submit" />'''

```

我们在第一行创建了 FastAPI 应用，并在 /predict 路由上使用了 get 方法，这将返回一个 HTML 响应，以便用户可以看到真实的 HTML 页面，并使用 post 方法在表单中输入数据。我们将利用这些数据进行预测。

你现在可以通过运行以下命令来启动你的应用程序。

```py
uvicorn app:app --reload

```

这将使你的应用程序在 localhost 上运行。在 *http://127.0.0.1:8000/predict* 路由下，你可以看到输入表单。

![](img/ca483a807ab03e9028fa828b5c149db3.png)

现在让我们定义一些辅助函数，这些函数将用于预处理数据。

```py
data = pd.read_csv('archive/Sentiment.csv')
tokenizer = Tokenizer(num_words=2000, split=' ')
tokenizer.fit_on_texts(data['text'].values)

def preProcess_data(text):
    text = text.lower()
    new_text = re.sub('[^a-zA-z0-9\s]','',text)
    new_text = re.sub('rt', '', new_text)
    return new_text

def my_pipeline(text):
    text_new = preProcess_data(text)
    X = tokenizer.texts_to_sequences(pd.Series(text_new).values)
    X = pad_sequences(X, maxlen=28)
    return X

```

这些函数本质上执行相同的工作，用于清理和预处理数据，这些函数已在我们的 model.py 文件中使用。

现在我们将在 "/predict" 路由上创建一个 POST 请求，以便可以将使用表单发布的数据传递到我们的模型中，并进行预测。

```py
@app.post('/predict')
def predict(text:str = Form(...)):
    clean_text = my_pipeline(text) #clean, and preprocess the text through pipeline
    loaded_model = tf.keras.models.load_model('sentiment.h5') #load the saved model 
    predictions = loaded_model.predict(clean_text) #predict the text
    sentiment = int(np.argmax(predictions)) #calculate the index of max sentiment
    probability = max(predictions.tolist()[0]) #calulate the probability
    if sentiment==0:
         t_sentiment = 'negative' #set appropriate sentiment
    elif sentiment==1:
         t_sentiment = 'neutral'
    elif sentiment==2:
         t_sentiment='postive'
    return { #return the dictionary for endpoint
         "ACTUALL SENTENCE": text,
         "PREDICTED SENTIMENT": t_sentiment,
         "Probability": probability
    }

```

现在代码有点多。让我们来拆解它。我们在 POST 请求上定义了一个路由 "/predict"，表单中的数据将作为我们的输入。我们在函数参数中将其指定为*Form(…)*。我们将文本传递给 pipeline 函数，以便它可以返回清理和预处理后的数据，然后我们可以将其输入到加载的模型中并获取预测。我们可以使用 numpy 的*argmax*函数来获取最高预测的索引。我们可以使用 Python 的*max*函数来挑选最大概率。请注意，FastAPI 的端点必须返回一个字典或一个*Pydantic* 基础模型。

现在你可以通过以下方式运行你的应用程序：

```py
$ uvicorn app:app --reload

```

在 "/predict" 路由上，你可以给模型提供输入。

![](img/130049d8adfb200d84b4b44fef322d5e.png)

模型将预测情感并返回结果。

![](img/83418447b21170dc3e7bedd583a9aed9.png)

我们还可以在主页上创建一个虚拟路由，即“/”，以确保它也能正常工作。

```py
@app.get('/')
def basic_view():
    return {"WELCOME": "GO TO /docs route, or /post or send post request to /predict "}

```

你可以在这里查看完整的代码：

**FastAPI 上的文档路由**

FastAPI 为每个应用程序提供了一个出色的“/docs”路由，你可以在这里测试你的 API 以及它的请求和路由。

在我们的 API 中，总共有 3 个路由：

![](img/8f4bfbb77669529c73ef39baa7eadee0.png)

我们可以通过点击它们来测试所有 3 个请求。我们将测试最重要的一个，即对预测路由的 POST 请求，它执行所有的计算。

![](img/7bb8707e108be543154a804039714738.png)

点击“尝试一下”以传入所需的文本并获取其情感：

![](img/0e2f80e43fd58c731d136e78d96c8a7c.png)

现在你可以在响应中检查结果：

![](img/c0db77efa57b3cd92cf426ad98861945.png)

响应状态码 200 表示请求成功，你将得到一个有效的期望输出。

### 第 4 步：添加有助于部署的适当文件

要为你的应用程序在 Heroku 上定义 Python 版本，你需要在你的文件夹中添加一个*runtime.txt*文件。在该文件中，你可以定义你的 Python 版本。只需在其中写入合适的 Python 版本。注意这是一个敏感文件，所以确保以指定的正确格式写入，否则 Heroku 会报错。

```py
python-3.6.13

```

要在 Heroku 上运行 uvicorn 服务器，你需要添加一个 Procfile。注意这个文件没有扩展名。只需创建一个名为“*Procfile*”的文件。在 Procfile 中添加以下命令。

```py
web: uvicorn app:app --host=0.0.0.0 --port=${PORT:-5000}

```

请注意，你需要在 0.0.0.0 上运行服务器，并且端口应该是 5000。

另一个重要的文件是*requirments.txt*文件。将你项目所需的所有重要库添加到其中。

```py
sklearn
fastapi
pandas
pydantic
tensorflow==2.0.0
uvicorn
h5py==2.10.0
python-multipart

```

你可以添加一个*.gitignore*文件来忽略你不需要的文件：

```py
__pycache__
model.py

```

### 第 5 步：在 Github 上部署

下一步是在 GitHub 上部署这个 Web 应用程序。你需要在 [GitHub](https://github.com/) 上创建一个新的仓库。然后打开命令行并将目录更改为项目目录。

你需要初始化仓库：

```py
$ git init

```

然后添加所有文件：

```py
$ git add -A

```

提交所有文件：

```py
$ git commit -m "first commit"

```

将分支更改为 main：

```py
$ git branch -M main

```

将文件夹连接到 GitHub 上的仓库：

```py
$ git remote add origin https://github.com/username/reponame.git

```

推送仓库：

```py
$ git push -u origin main

```

### 第 6 步：在 Heroku 上部署

你需要在 Heroku 仪表板上创建一个新的应用程序。

![](img/ae54f7c7872f46ae23adf7e7666ad0bf.png)

为你的应用程序选择一个合适的名称。

在部署部分，在部署方法中选择 GitHub。

![](img/7ffd3800a31de1615cb5b610508263f1.png)

在这里搜索你的仓库，并连接到它。

![](img/c6e6a30fc28c124cf0f3f090cd0878e0.png)

你可以选择自动部署，这样每次在 GitHub 上的部署分支有变更时，应用程序将自动部署。第一次需要手动部署应用程序。然后每次你在 GitHub 上更新部署分支时，它将会自动部署。

![](img/4ee145d19449ea0144c2f1dc0e855f6a.png)

点击“部署分支”将启动部署过程，你可以通过点击“更多”来查看日志，这可以帮助你查看应用程序的日志，如果遇到错误，你可以看到。

![](img/2ee3e95bc44948700c828857b7fdbf66.png)

一旦构建成功，你可以点击“打开应用程序”来检查你的应用程序。你可以访问你在应用程序中之前定义的所有路由，并进行测试。

![](img/922f6247d9b0abdf5cbe13e0f7c4e093.png)

**查看部署历史**

你可以通过检查 GitHub 上底部左侧的环境标签来查看应用程序的部署历史记录。

![](img/d2a5eea06168d3cacdae3c4abe57dbad.png)

这也会显示所有的部署历史记录。

![](img/9eb2568dd680657124294dfa1adeb00f.png)

**使用 Python Requests 访问你的 API**

你可以访问你的 API，这意味着你可以在你的普通代码中使用这个 API 来执行情感分析任务。

```py
import requests #install using pip if not already

url = 'https://sentiment-analysis-gopdebate.herokuapp.com/predict'
data = {'text':'Testing Sentiments'} #test is the function params

resp = requests.post(url, data=data) #post request
print(resp.content)

```

![](img/4e301cf35d5f910a673e9b67698696c8.png)

你将收到的输出就像在端点中看到的一样。

**使用 Curl 访问你的 API**

Curl 是一个命令行工具（你可以从[这里](https://everything.curl.dev/get)下载），用于从命令行发起请求。我们可以使用以下命令发送请求。

```py
$ curl -X 'POST' \
   'https://sentiment-analysis-gopdebate.herokuapp.com/predict' \
   -H 'accept: application/json' \
   -H 'Content-Type: application/x-www-form-urlencoded' \
   -d 'text=WORST%20SHOW%20EVER

```

在 -X 参数之后我们提到了请求的类型，即 POST 请求。然后 -H 显示我们的 API 使用的头部信息，分别是 application/JSON 和内容类型。接着我们需要使用 -d 参数提供数据并传递文本。要添加空格，请使用 *%20*。

![](img/a492c7457118263f52f325cd312e63b4.png)

你可以在我的 GitHub 仓库[这里](https://github.com/ahmadmustafaanis/SentimentAnalysisDeployment)查看完整的代码。

### **学习成果**

在这篇文章中，你学会了如何通过 Heroku 和 GitHub 将你的机器学习/深度学习模型部署为 REST API。你还学会了如何使用 Python *requests* 模块和 CURL 访问该 API。

**相关：**

+   [MLOps 概述](https://www.kdnuggets.com/2021/03/overview-mlops.html)

+   [数据科学作为一种产品——为什么这么难？](https://www.kdnuggets.com/2020/12/data-science-product-hard.html)

+   [使用 TensorFlow Serving 将训练好的模型部署到生产环境](https://www.kdnuggets.com/2020/11/serving-tensorflow-models.html)

### 更多相关主题

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成为伟大数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用 Heroku 部署机器学习 Web 应用](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)

+   [停止学习数据科学以寻找目标，并找到目标……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
