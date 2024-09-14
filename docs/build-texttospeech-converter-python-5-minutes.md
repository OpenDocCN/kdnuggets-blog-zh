# 用 Python 在 5 分钟内构建文本转语音转换器

> 原文：[`www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html`](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

![用 Python 在 5 分钟内构建文本转语音转换器](img/8d2b2f12265d4ab25a88fb1752e01b5f.png)

[Kelly Sikkema](https://unsplash.com/@kellysikkema)

对于你的早期编程技能，最好的事情就是项目。你可能拥有知识，但将其应用到实际中才是真正的挑战，并保持你的竞争力。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

最近几个月我采访了一些前辈，他们提到的新人常见问题是缺乏将技能应用到项目或现实问题中的经验。所以，我觉得创建这个快速 5 分钟的项目来帮助应用和提升你的技能会很有趣。

我选择了展示如何在 Python 中构建一个文本转语音转换器，这不仅简单，而且有趣且互动。我将向你展示两种用 Python 实现的方法。

那么，让我们开始吧。

# 使用 pyttsx3

## 要求

对于这个快速简单的构建，你需要以下模块：pyttsx3。这个模块是一个文本转语音转换库，兼容 Python 2 和 3。

要安装这个模块，请输入以下内容：

```py
pip install pyttsx3
```

## 导入

现在你需要将库导入到你的环境中：

```py
import pyttsx3
```

## 引擎实例

我们现在将初始化‘init’函数以获得引擎实例

```py
engine = pyttsx3.init()
```

## 告诉我们的引擎要说什么

使用引擎上的‘say’方法，我们输入希望被朗读的文本

```py
engine.say('Oh my. I can't believe I did this in less than 5 minutes')
```

## 现在听听吧

我们现在使用“运行并等待”方法来处理语音命令

```py
engine.runAndWait()
```

就这样。现在另一个..

# 使用 gTTS API

## 要求

对于这个文本转语音转换器，我们需要 Google 文本转语音 API。它可以轻松将输入的文本转换为音频，然后保存为 mp3 文件。

这可以用于多种语言，如英语、印地语、泰米尔语、法语、德语等。

要安装 API，请输入以下内容：

```py
pip install gTTS
```

## 导入

现在你需要将库导入到你的环境中：

```py
from gtts import gTTS
```

你还需要导入 os 以播放音频：

```py
import os
```

## 输入你的文本

```py
text = 'Learn how to build something with Python in 5 minutes'
```

## 选择你的语言

选择你喜欢的语言。你可以通过点击这个 [链接](https://gtts.readthedocs.io/en/latest/module.html#languages-gtts-lang) 查找语言列表。

```py
language = 'en'
```

## 将文本传递给引擎

你还可以选择音频播放速度是快还是慢。

```py
myobj = gTTS(text=mytext, lang=language, slow=False)
```

## 将你的音频保存为 .mp3

```py
myobj.save("mytext2speech.mp3")
```

## 该听听了

mpg123 是一个免费的开源音频播放器，我们将添加它来指定我们希望 .mp3 文件在哪个程序中播放。

```py
os.system("mpg321 mytext2speech.mp3")
```

哒哒！你选择的媒体播放器应该已经说了：

> “学习如何在 5 分钟内用 Python 构建东西”

# 总结一下

这篇文章完全是为了让你探索你的 Python 技能，然后构建更好更酷的项目。& 为了一点乐趣！

**[妮莎·阿里亚](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一位数据科学家和自由技术写作人。她特别关注提供数据科学职业建议或教程以及围绕数据科学的理论知识。她还希望探索人工智能如何有益于人类寿命的不同方式。她是一位热衷于学习、寻求拓展技术知识和写作技能的热心者，同时帮助引导他人。

### 更多相关话题

+   [用 Python 在 5 分钟内构建网页抓取器](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)

+   [FastAPI 教程：用 Python 几分钟内构建 API](https://www.kdnuggets.com/fastapi-tutorial-build-apis-with-python-in-minutes)

+   [5 分钟内构建机器学习网页应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：5 分钟内构建机器学习网页应用](https://www.kdnuggets.com/2022/n10.html)

+   [用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [语音识别指标的演变](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)
