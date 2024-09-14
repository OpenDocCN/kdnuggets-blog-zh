# 使用 Python 和 NLTK 构建你的第一个聊天机器人

> 原文：[https://www.kdnuggets.com/2019/05/build-chatbot-python-nltk.html](https://www.kdnuggets.com/2019/05/build-chatbot-python-nltk.html)

[评论](#comments)![图示名称](../Images/7eb4dbb7ffb5abf78cd6b1da715b6cdc.png)来源：[https://octodev.net/most-advanced-chatbot-apps-powered-by-artificial-intellect/](https://octodev.net/most-advanced-chatbot-apps-powered-by-artificial-intellect/)

> “**聊天机器人**（也称为**对话机器人**、**闲聊机器人**、**机器人**、**IM 机器人**、**互动代理**或**人工对话实体**）是一个[计算机程序](https://en.wikipedia.org/wiki/Computer_program)或一个[人工智能](https://en.wikipedia.org/wiki/Artificial_intelligence)，通过听觉或文本方法进行[对话](https://en.wikipedia.org/wiki/Conversation)。这些程序通常被设计为模拟人类在对话中的行为，从而通过[图灵测试](https://en.wikipedia.org/wiki/Turing_test)。聊天机器人通常用于[对话系统](https://en.wikipedia.org/wiki/Dialog_system)中，具有多种实际用途，包括客户服务或信息获取。一些聊天机器人使用复杂的[自然语言处理](https://en.wikipedia.org/wiki/Natural_language_processing)系统，但许多简单的系统则扫描输入中的关键词，然后从[数据库](https://en.wikipedia.org/wiki/Database)中提取具有最匹配关键词或最相似措辞模式的回复。”
> 
> — 来源 [维基百科](https://en.wikipedia.org/wiki/Chatbot)

聊天机器人并不是很新，其中之一是 ELIZA，它在 1960 年代初期创建，值得探索。为了成功构建一个对话引擎，它应当考虑以下事项：

1.  了解目标受众

1.  了解交流的自然语言。

1.  了解用户的意图或愿望

1.  提供可以回答用户的问题的响应

今天我们将学习如何使用 Python 的 NLTK 库创建一个简单的聊天助手或聊天机器人。

[NLTK](https://www.nltk.org/) 有一个模块，[nltk.chat](https://www.nltk.org/api/nltk.chat.html)，它通过提供一个通用框架来简化这些引擎的构建。

在这个博客中，我使用了来自 nltk.chat.util 的两个导入：

**聊天**：这是一个包含聊天机器人所使用的所有逻辑的类。

**反射**：这是一个包含一组输入值及其对应输出值的词典。它是一个可选的词典，你可以使用它。你也可以创建自己的词典，格式如下，并在代码中使用。如果你查看 nltk.chat.util，你会看到它的值如下：

```py
reflections = {
  "i am"       : "you are",
  "i was"      : "you were",
  "i"          : "you",
  "i'm"        : "you are",
  "i'd"        : "you would",
  "i've"       : "you have",
  "i'll"       : "you will",
  "my"         : "your",
  "you are"    : "I am",
  "you were"   : "I was",
  "you've"     : "I have",
  "you'll"     : "I will",
  "your"       : "my",
  "yours"      : "mine",
  "you"        : "me",
  "me"         : "you"
}

```

你也可以创建你自己的反射词典，格式与上面相同，并在代码中使用它。以下是一个示例：

```py
my_dummy_reflections= {
    "go"     : "gone",
    "hello"    : "hey there"
}

```

并将其用作：

```py

 chat = Chat(pairs, my_dummy_reflections)
```

使用上述Python的NLTK库概念，让我们构建一个简单的聊天机器人，而不使用任何机器学习或深度学习算法。因此，显然我们的聊天机器人将是一个体面的，但不是智能的。

源代码：

```py
from nltk.chat.util import Chat, reflections

pairs = [
    [
        r"my name is (.*)",
        ["Hello %1, How are you today ?",]
    ],
     [
        r"what is your name ?",
        ["My name is Chatty and I'm a chatbot ?",]
    ],
    [
        r"how are you ?",
        ["I'm doing good\nHow about You ?",]
    ],
    [
        r"sorry (.*)",
        ["Its alright","Its OK, never mind",]
    ],
    [
        r"i'm (.*) doing good",
        ["Nice to hear that","Alright :)",]
    ],
    [
        r"hi|hey|hello",
        ["Hello", "Hey there",]
    ],
    [
        r"(.*) age?",
        ["I'm a computer program dude\nSeriously you are asking me this?",]

    ],
    [
        r"what (.*) want ?",
        ["Make me an offer I can't refuse",]

    ],
    [
        r"(.*) created ?",
        ["Nagesh created me using Python's NLTK library ","top secret ;)",]
    ],
    [
        r"(.*) (location|city) ?",
        ['Chennai, Tamil Nadu',]
    ],
    [
        r"how is weather in (.*)?",
        ["Weather in %1 is awesome like always","Too hot man here in %1","Too cold man here in %1","Never even heard about %1"]
    ],
    [
        r"i work in (.*)?",
        ["%1 is an Amazing company, I have heard about it. But they are in huge loss these days.",]
    ]

[
        r"(.*)raining in (.*)",
        ["No rain since last week here in %2","Damn its raining too much here in %2"]
    ],
    [
        r"how (.*) health(.*)",
        ["I'm a computer program, so I'm always healthy ",]
    ],
    [
        r"(.*) (sports|game) ?",
        ["I'm a very big fan of Football",]
    ],
    [
        r"who (.*) sportsperson ?",
        ["Messy","Ronaldo","Roony"]
],
    [
        r"who (.*) (moviestar|actor)?",
        ["Brad Pitt"]
],
    [
        r"quit",
        ["BBye take care. See you soon :) ","It was nice talking to you. See you soon :)"]

],
]

def chatty():
    print("Hi, I'm Chatty and I chat alot ;)\nPlease type lowercase English language to start a conversation. Type quit to leave ") #default message at the start
    chat = Chat(pairs, reflections)
    chat.converse()

if __name__ == "__main__":
    chatty()

```

代码非常简单，但我们还是来理解一下。

一旦调用函数chatty()，将显示默认消息：

![figure-name](../Images/8a0c1ea2438a7e72bc4395816ba89835.png)

接下来，我创建了一个包含**pairs**（包含问题和答案集合的元组列表）和**reflections**（如上所述）的Chat类实例。

下一步是触发对话：

```py

chat.converse()

```

一个简单的对话：

![figure-name](../Images/7261554d8400c0198ff5ce2da31553be.png)与Chatty的简单对话

正如你所见，我们只是将可能的问题和答案硬编码到列表pairs中。

让我们更多地与Chatty互动：

![figure-name](../Images/f5e026d518bb3631c2c2394dbb1e5e30.png)与Chatty的简单对话

nltk.chat聊天机器人基于问题中存在的关键字的正则表达式工作。所以你可以添加任何数量的按正确格式编写的问题，以避免你的聊天机器人在确定正则表达式时感到困惑。

在这篇博客中，我用简单的步骤解释了如何使用NLTK构建自己的聊天机器人，当然它不是一个智能聊天机器人。

希望大家喜欢阅读。

快乐学习!!!

如有疑问/建议，请通过[LinkedIn](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)与我联系。

**简介: [纳吉什·辛格·乔汉](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)**是CirrusLabs的一名大数据开发人员。他在电信、分析、销售、数据科学等多个领域拥有超过4年的工作经验，并在各种大数据组件方面有专长。

[原文](https://towardsdatascience.com/build-your-first-chatbot-using-python-nltk-5d07b027e727)。已获得许可重新发布。

**相关：**

+   [如果聊天机器人要成功，它们需要这个](/2018/05/chatbots-succeed-need-logic.html)

+   [认识Lucy：创建聊天机器人原型](/2017/09/meet-lucy-chatbot-prototype.html)

+   [机器如何理解我们的语言：自然语言处理简介](/2018/10/machines-understand-language-introduction-natural-language-processing.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关内容

+   [用这些课程构建类似 ChatGPT 的聊天机器人](https://www.kdnuggets.com/2023/05/build-chatgptlike-chatbot-courses.html)

+   [用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

+   [它活过来了！用 Python 和一些便宜的…构建你的第一个机器人](https://www.kdnuggets.com/2023/06/manning-build-first-robots-python-cheap-basic-components.html)

+   [聊天机器人转型：从失败到未来](https://www.kdnuggets.com/2021/12/chatbot-transformation-failure-future.html)

+   [开放助手：探索开放和协作的可能性…](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)

+   [聊天机器人竞技场：LLM 基准测试平台](https://www.kdnuggets.com/2023/05/chatbot-arena-llm-benchmark-platform.html)
