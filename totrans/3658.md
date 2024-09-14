# 用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人

> 原文：[https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)

![用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/40ebd6261d24c1e08e64db05b9d97008.png)

作者提供的图片

本短教程将使用 Microsoft DialoGPT 模型、Hugging Face Space 和 Gradio 界面构建一个简单的聊天机器人。你将能够使用类似的技术在 5 分钟内开发和自定义自己的应用程序。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

# 1\. 创建一个新 Space

1.  访问 **hf.co** 并创建一个免费账户。之后，点击右上角的 **显示图像** 并选择“新建 Space”选项。

1.  填写表单，包括应用程序名称、许可证、Space 硬件和可见性。

![用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/6043add36e3c68180aa06e9e2b5fd3e4.png)

图片来源于 Space

1.  点击“创建 Space”来初始化应用程序。

1.  你可以克隆仓库并从本地系统推送文件，或者使用浏览器在 Hugging Face 上创建和编辑文件。

![用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/18b3d1c34f6e08c0b69cb7743928d923.png)

图片来源于 [AI ChatBot](https://huggingface.co/spaces/kingabzpro/AI-ChatBot)

# 2\. 创建 ChatBot 应用程序文件

我们将点击“文件”选项卡 **>** + 添加文件 **>** 创建新文件。

![用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/dfd3cf303eea87cb8b701e9958c1dcc0.png)

图片来源于 [kingabzpro/AI-ChatBot](https://huggingface.co/spaces/kingabzpro/AI-ChatBot/tree/main)

创建一个 [Gradio](https://www.gradio.app/) 界面。你可以复制我的代码。

![用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/ece649c4542309b99d60ecf749fa4fcf.png)

图片来源于 [app.py](https://huggingface.co/spaces/kingabzpro/AI-ChatBot/blob/main/app.py)

我已经加载了“microsoft/DialoGPT-large”分词器和模型，并创建了一个 `predict` 函数来获取响应和创建历史记录。

```py
from transformers import AutoModelForCausalLM, AutoTokenizer
import gradio as gr
import torch

title = "????AI ChatBot"
description = "A State-of-the-Art Large-scale Pretrained Response generation model (DialoGPT)"
examples = [["How are you?"]]

tokenizer = AutoTokenizer.from_pretrained("microsoft/DialoGPT-large")
model = AutoModelForCausalLM.from_pretrained("microsoft/DialoGPT-large")

def predict(input, history=[]):
    # tokenize the new input sentence
    new_user_input_ids = tokenizer.encode(
        input + tokenizer.eos_token, return_tensors="pt"
    )

    # append the new user input tokens to the chat history
    bot_input_ids = torch.cat([torch.LongTensor(history), new_user_input_ids], dim=-1)

    # generate a response
    history = model.generate(
        bot_input_ids, max_length=4000, pad_token_id=tokenizer.eos_token_id
    ).tolist()

    # convert the tokens to text, and then split the responses into lines
    response = tokenizer.decode(history[0]).split("<|endoftext|>")
    # print('decoded_response-->>'+str(response))
    response = [
        (response[i], response[i + 1]) for i in range(0, len(response) - 1, 2)
    ]  # convert to tuples of list
    # print('response-->>'+str(response))
    return response, history

gr.Interface(
    fn=predict,
    title=title,
    description=description,
    examples=examples,
    inputs=["text", "state"],
    outputs=["chatbot", "state"],
    theme="finlaymacklon/boxy_violet",
).launch()
```

此外，我为我的应用程序提供了一个自定义主题：[boxy_violet](https://huggingface.co/spaces/finlaymacklon/boxy_violet)。你可以浏览 Gradio 的 [主题画廊](https://huggingface.co/spaces/gradio/theme-gallery)来根据你的喜好选择主题。

# 3\. 创建需求文件

现在，我们需要创建一个 `requirement.txt` 文件，并添加所需的 Python 包。

![使用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/a132514c0d34b6900045bb5435cef1a2.png)

来自[requirements.txt](https://huggingface.co/spaces/kingabzpro/AI-ChatBot/blob/main/requirements.txt)的图片

```py
transformers
torch
```

之后，你的应用将开始构建，几分钟内，它会下载模型并加载模型推断。

![使用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/65bd64fbe17b887c658315de6bef540b.png)

# 4. Gradio 演示

Gradio 应用看起来很棒。我们只需为每种不同的模型架构创建一个 `predict` 函数，以获取响应并维护历史记录。

你现在可以在[kingabzpro/AI-ChatBot](https://huggingface.co/spaces/kingabzpro/AI-ChatBot)上聊天和互动，或者使用 https://kingabzpro-ai-chatbot.hf.space 将你的应用嵌入到你的网站上。

![使用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/d3643614cfafc1be127fc56a7430ea93.png)

来自[kingabzpro/AI-ChatBot](https://huggingface.co/spaces/kingabzpro/AI-ChatBot)的图片

你还感到困惑吗？在[Spaces](https://huggingface.co/spaces)上查找数百个聊天机器人应用，以获取灵感并了解模型推断。

例如，如果你有一个经过“LLaMA-7B”微调的模型，搜索[模型](https://huggingface.co/decapoda-research/llama-7b-hf)并向下滚动查看模型的各种实现。

![使用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](../Images/90c55921fab332cab4f5742b9619dc22.png)

来自[decapoda-research/llama-7b-hf](https://huggingface.co/decapoda-research/llama-7b-hf)的图片

# 结论

总之，这篇博客提供了一个快速而简单的教程，教你如何在 5 分钟内使用 Hugging Face 和 Gradio 创建 AI 聊天机器人。通过一步步的说明和可定制的选项，任何人都可以轻松创建自己的聊天机器人。

很有趣，希望你学到了些东西。请在评论区分享你的 Gradio 演示。如果你在寻找更简单的解决方案，查看一下[OpenChat: The Free & Simple Platform for Building Custom Chatbots in Minutes](/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理健康困扰的学生开发 AI 产品。

### 相关话题

+   [用这些课程构建类似 ChatGPT 的聊天机器人](https://www.kdnuggets.com/2023/05/build-chatgptlike-chatbot-courses.html)

+   [如何从头开始构建和训练一个Transformer模型，使用…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [Open Assistant：探索开放和协作聊天机器人的可能性](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)

+   [5分钟内构建一个机器学习网页应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [5分钟内用Python构建一个网页抓取器](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)

+   [KDnuggets 新闻 2022年3月9日：5分钟内构建一个机器学习网页应用](https://www.kdnuggets.com/2022/n10.html)
