# 使用 SpaCy 自动提取命名实体的 Flask API

> 原文：[https://www.kdnuggets.com/2019/04/building-flask-api-automatically-extract-named-entities-spacy.html](https://www.kdnuggets.com/2019/04/building-flask-api-automatically-extract-named-entities-spacy.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [注释](#comments)

**作者：[Susan Li](https://www.linkedin.com/in/susanli/)，高级数据科学家**

![图](../Images/06957073c64dec6ad611e5c88f73a537.png)

图片来源：Pixabay

目前大量的非结构化文本数据提供了丰富的信息源，如果数据可以被结构化的话。[命名实体识别 (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition)（也称为命名实体提取）是从半结构化和非结构化文本源中构建知识的第一步之一。

只有在进行 NER 后，我们才能至少揭示信息中包含了谁和什么。因此，数据科学团队将能够看到一个结构化的表示，展示一个语料库中所有人名、公司名、地点等的名称，这可以作为进一步分析和研究的起点。

在 [上一篇文章](https://towardsdatascience.com/named-entity-recognition-with-nltk-and-spacy-8c4a7d88e7da)中，我们学习并实践了 [如何使用 NLTK 和 spaCy 构建命名实体识别器](https://towardsdatascience.com/named-entity-recognition-with-nltk-and-spacy-8c4a7d88e7da)。为了更进一步，创建有用的东西，本文将介绍如何使用 spaCy 开发和部署一个简单的命名实体提取器，并通过 Flask API 用 Python 服务它*。

### 一个 Flask API

我们的目标是构建一个 API，我们提供文本，例如《纽约时报》文章（或任何文章）作为输入，我们的命名实体提取器将识别并提取四种类型的实体：组织、人物、地点和金额。基本架构如下：

![图](../Images/585cde79738ef16ac1a52385245ccd80.png)

图 1

为了构建 API，我们需要创建两个文件：

1.  `index.html` 用于处理 API 的模板。

1.  `app.py` 用于处理请求并返回输出文件。

最终产品将如下所示：

![图](../Images/989fa6eb53ead63fcf32dca581c1051d.png)

图 2

我们从构建 API 开始，逐步创建两个文件。我们的项目文件夹结构如下：

+   我们的项目位于 ***Named-Entity-Extractor ***文件夹中。

![图](../Images/1895f7c9d56de5641d784aed7d26a6c4.png)

图 3

+   `templates` 目录与创建的 app.py 文件在同一文件夹中。

![图](../Images/c7f1e889a9f905b696473b42f2c7489a.png)

图 4

+   index.html 位于 templates 文件夹中。

### **index.html**

+   我们将我们的应用命名为“命名实体提取器”

+   使用 [BootstrapCDN](https://www.bootstrapcdn.com/)，将 [stylesheet](https://getbootstrap.com/docs/4.1/getting-started/introduction/) `<link>` 复制粘贴到我们的 `<head>` 中，放在所有其他样式表之前，以加载我们的 CSS。

+   获取 Bootstrap 的导航头部，从 [a template for a simple informational website](https://getbootstrap.com/docs/4.3/examples/jumbotron/#) 获取 navbar。它包含一个称为 [jumbotron](https://getbootstrap.com/docs/4.0/components/jumbotron/) 的大召唤框和三个支持内容。

+   从模板的 [source code](http://view-source:https//getbootstrap.com/docs/4.3/examples/jumbotron/) 中复制粘贴 [navbar](https://getbootstrap.com/docs/4.0/components/navbar/) 代码。

+   Bootstrap 需要一个容器元素来包裹站点内容并容纳我们的网格系统。

+   在我们的案例中，对于第一个容器，我们将创建一个垂直表单，其中包含两个输入字段，一个“清除”按钮和一个“提交”按钮。

+   文本表单控件使用 `form-control` 类进行样式化。

+   我们为用户提供了四个任务选项（即命名实体提取任务），它们是：***Organization***，***Person***，***Geopolitical*** 和 ***Money***。

+   第二个容器为用户的操作提供上下文反馈消息，即命名实体提取的结果。

+   我们不仅希望将命名实体提取结果打印给用户，还希望打印每个命名实体提取的结果数量。

+   将 [JavaScript](https://getbootstrap.com/docs/4.1/getting-started/introduction/) 复制粘贴到我们 HTML 页面接近末尾的 `<script>` 标签中，紧接着闭合的 `</body>` 标签之前。

### **app.py**

我们的 `app.py` 文件相当简单且易于理解。它包含将由 Python 解释器执行以运行 Flask Web 应用程序的主要代码，其中包括用于识别命名实体的 spaCy 代码。

+   我们将应用作为单一模块运行；因此，我们用参数 `__name__` 初始化了一个新的 Flask 实例，让 Flask 知道它可以在与自身所在目录相同的目录中找到 HTML 模板文件夹（`templates`）。

+   我们使用路由装饰器（`@app.route('/')`）来指定应触发 `index` 函数执行的 URL。

+   我们的 `index` 函数简单地渲染了位于 `templates` 文件夹中的 `index.html` HTML 文件。

+   在 `process` 函数内部，我们对用户输入的原始文本应用 nlp，并从原始文本中提取预定的命名实体（***Organization***，***Person***，***Geopolitical*** 和 ***Money***）。

+   我们使用 `POST` 方法将表单数据传输到服务器的消息体中。最后，通过在 `app.run` 方法中设置 `debug=True` 参数，我们进一步激活了 Flask 的调试器。

+   我们使用 `run` 函数仅在 Python 解释器直接执行此脚本时才运行应用程序，这通过使用 `if` 语句与 `__name__ == '__main__'` 进行确保。

我们快完成了！

### 尝试我们的 API

+   启动 ***命令提示符*。

+   进入我们的 ***命名实体提取器*** 文件夹。

![图示](../Images/64bb7dcffead7af0d6de91d0cb17737c.png)

图5

+   打开你的Web浏览器，将 “[http://127.0.0.1:5000/](http://127.0.0.1:5000/)” 粘贴到地址栏中，我们将看到这个表单：

![图示](../Images/d4ef9a0f40e572fa1a51bea04c77bfde.png)

图6

+   我从 [nytimes](https://www.nytimes.com/2019/03/01/world/canada/trudeau-scandal-snc-lavalin.html)复制粘贴了一些文章段落，这是一个加拿大故事：

![图示](../Images/b2e2dd8f627bc6d86d5a2d258b4cc421.png)

图7

+   在“选择任务”下选择“***Organization***”，然后点击“提交”，我们得到的是：

![图示](../Images/790997a85dbe822e89de2695edd43287.png)

图8

+   很好。让我们尝试“***Person***”实体：

![图示](../Images/3a13f59dc8cfc3263caf0d394371f43b.png)

图9

+   “***Geopolitical***”实体：

![图示](../Images/a34c95635d45bceac6a497c5b6d382cc.png)

图10

+   “***Money***”实体：

![图示](../Images/9010368909f99c58dccbbd0c57633cb6.png)

图11

完成了！

如果你按照以上步骤操作并达到了这里，恭喜你！你已经以零成本创建了一个简单但有效的命名实体提取器！回头看，我们只需要创建两个文件，所需的只是开源库和学习如何使用它们来创建这两个文件。

通过构建这样的应用，你已经学会了新技能，并使用这些技能创造了有用的东西。

完整源代码可在这个 [代码库](https://github.com/susanli2016/Named-Entity-Extractor)找到。祝周一愉快！

参考：

**个人简介： [Susan Li](https://www.linkedin.com/in/susanli/)** 正在通过一篇文章改变世界。她是位于加拿大多伦多的高级数据科学家。

[原文](https://towardsdatascience.com/building-a-flask-api-to-automatically-extract-named-entities-using-spacy-2fd3f54ebbc6)。经许可转载。

**相关：**

+   [你需要了解的关于NLP和机器学习的文本预处理](/2019/04/text-preprocessing-nlp-machine-learning.html)

+   [利用迁移学习和弱监督廉价构建NLP分类器](/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html)

+   [简单神经网络与LSTM时间序列预测介绍](/2019/04/introduction-time-series-forecasting-simple-neural-networks-lstm.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

### 更多相关内容

+   [如何利用机器学习自动标记数据](https://www.kdnuggets.com/2022/02/machine-learning-automatically-label-data.html)

+   [使用 Python 创建一个从音频中提取主题的 Web 应用程序](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [使用 spaCy 进行 NLP 入门](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)

+   [使用 spaCy 的自然语言处理](https://www.kdnuggets.com/2023/01/natural-language-processing-spacy.html)

+   [OpenAI 的 Whisper API 用于转录和翻译](https://www.kdnuggets.com/2023/06/openai-whisper-api-transcription-translation.html)

+   [认识 Gorilla：加州大学伯克利分校和微软的 API 增强 LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)
