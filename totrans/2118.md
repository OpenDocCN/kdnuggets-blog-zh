# 开始使用Claude 3 Opus，这款新模型刚刚超越了GPT-4和Gemini

> 原文：[https://www.kdnuggets.com/getting-started-with-claude-3-opus-that-just-destroyed-gpt-4-and-gemini](https://www.kdnuggets.com/getting-started-with-claude-3-opus-that-just-destroyed-gpt-4-and-gemini)

![开始使用Claude 3 Opus，这款新模型刚刚超越了GPT-4和Gemini](../Images/040d47cea687ae6f0cbaa2cb7dbd2af0.png)

图片由作者提供

Anthropic最近推出了一系列新的AI模型，这些模型在基准测试中超越了GPT-4和Gemini。随着AI行业的快速增长和发展，Claude 3模型正在作为大型语言模型（LLMs）中的下一大突破取得显著进展。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

在这篇博客文章中，我们将深入探讨Claude 3模型的性能基准。我们还将了解支持简单、异步和流响应生成的新Python API以及其增强的视觉能力。

# 介绍Claude 3

Claude 3在AI技术领域迈出了重要的一步。它在各种评估基准上超越了最先进的语言模型，包括MMLU、GPQA和GSM8K，展示了在复杂任务中接近人类的理解和流利度。

Claude 3模型有三种变体：**Haiku, Sonnet, 和 Opus**，每种都有其独特的能力和优势。

1.  **Haiku**是速度最快、性价比最高的模型，能够在不到三秒的时间内阅读和处理信息密集型的研究论文。

1.  **Sonnet**比Claude 2和2.1快2倍，擅长于需要快速响应的任务，如知识检索或销售自动化。

1.  **Opus**提供与Claude 2和2.1相似的速度，但智能水平更高。

根据下表，Claude 3 Opus在所有LLMs基准测试中都优于GPT-4和Gemini Ultra，使其成为AI领域的新领军者。

![开始使用Claude 3 Opus，这款新模型刚刚超越了GPT-4和Gemini](../Images/cc90746e0753f33f6edb6ca2afc6f324.png)

表格来自[Claude 3](https://www.anthropic.com/news/claude-3-family)

Claude 3模型的一项重要改进是其强大的视觉能力。它们可以处理各种视觉格式，包括照片、图表、图形和技术图解。

![开始使用Claude 3 Opus，这款新模型刚刚超越了GPT-4和Gemini](../Images/ce9b9ad29ec991686afafb763bee7dcf.png)

来自 [Claude 3](https://www.anthropic.com/news/claude-3-family) 的表格

你可以通过访问 [https://www.anthropic.com/claude](https://www.anthropic.com/claude) 并创建一个新账户来开始使用最新的模型。与 OpenAI playground 相比，这很简单。

![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/350900bfa398543c48fd9bd29cf2ed66.png)

# 设置

1.  在我们安装 Python 包之前，我们需要访问 [https://console.anthropic.com/dashboard](https://console.anthropic.com/dashboard) 并获取 API 密钥。 ![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/e35274e094c1ad2dad9b83f1c26cb53e.png)

1.  不直接提供 API 密钥来创建客户端对象，你可以设置 `ANTHROPIC_API_KEY` 环境变量并将其作为密钥提供。

1.  使用 PIP 安装 `anthropic` Python 包。

```py
pip install anthropic
```

1.  使用 API 密钥创建客户端对象。我们将使用客户端进行文本生成、访问视觉功能和流式传输。

```py
import os
import anthropic
from IPython.display import Markdown, display

client = anthropic.Anthropic(
    api_key=os.environ["ANTHROPIC_API_KEY"],
)
```

# 仅适用于 Claude 2 的 API

让我们尝试旧的 Python API 来测试它是否仍然有效。我们将提供完成 API，包括模型名称、最大令牌长度和提示。

```py
from anthropic import HUMAN_PROMPT, AI_PROMPT

completion = client.completions.create(
    model="claude-3-opus-20240229",
    max_tokens_to_sample=300,
    prompt=f"{HUMAN_PROMPT} How do I cook a original pasta?{AI_PROMPT}",
)
Markdown(completion.completion)
```

错误显示我们不能对 `claude-3-opus-20240229` 模型使用旧的 API。我们需要改用 Messages API。

![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/36137926508f3b470647677e9cb279a0.png)

# Claude 3 Opus Python API

让我们使用 Messages API 生成响应。我们需要提供 messages 参数，包含角色和内容的字典列表，而不是提示。

```py
Prompt = "Write the Julia code for the simple data analysis."
message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": Prompt}
    ]
)
Markdown(message.content[0].text)
```

使用 IPython Markdown 会以 Markdown 格式显示响应。这意味着它会以干净的方式展示项目符号、代码块、标题和链接。

![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/3b23614ebc9c2208dcc23338ec126d94.png)

# 添加系统提示

我们还可以提供系统提示来定制你的响应。在我们的例子中，我们要求 Claude 3 Opus 用乌尔都语回应。

```py
client = anthropic.Anthropic(
    api_key=os.environ["ANTHROPIC_API_KEY"],
)

Prompt = "Write a blog about neural networks."

message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    system="Respond only in Urdu.",
    messages=[
        {"role": "user", "content": Prompt}
    ]
)

Markdown(message.content[0].text)
```

Opus 模型相当不错。我是说我可以非常清晰地理解它。

![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/15bd04b3d93bc77056790592b011976d.png)

# Claude 3 异步

同步 API 按顺序执行 API 请求，直到收到响应才会调用下一个请求，这样会阻塞执行。而异步 API 允许多个并发请求而不会阻塞，使其更加高效和可扩展。

1.  我们需要创建一个异步 Anthropic 客户端。

1.  使用 async 创建主函数。

1.  使用 await 语法生成响应。

1.  使用 await 语法运行主函数。

```py
import asyncio
from anthropic import AsyncAnthropic

client = AsyncAnthropic(
    api_key=os.environ["ANTHROPIC_API_KEY"],
)

async def main() -> None:

    Prompt = "What is LLMOps and how do I start learning it?"

    message = await client.messages.create(
        max_tokens=1024,
        messages=[
            {
                "role": "user",
                "content": Prompt,
            }
        ],
        model="claude-3-opus-20240229",
    )
    display(Markdown(message.content[0].text))

await main()
```

![开始使用刚刚击败 GPT-4 和 Gemini 的 Claude 3 Opus](../Images/42d5f10d98aba7b5bee21925429f59bf.png)

> **注意：** 如果你在 Jupyter Notebook 中使用 async，尝试使用 await main()，而不是 asyncio.run(main())

# Claude 3 流式传输

流媒体是一种方法，使得语言模型的输出可以在生成后立即处理，而无需等待完整的响应。这种方法通过逐个返回输出标记来减少感知延迟，而不是一次性返回所有内容。

我们将使用 `messages.stream` 进行响应流式处理，而不是 `messages.create`，并使用循环显示响应中可用的多个单词。

```py
from anthropic import Anthropic

client = anthropic.Anthropic(
    api_key=os.environ["ANTHROPIC_API_KEY"],
)

Prompt = "Write a mermaid code for typical MLOps workflow."

completion = client.messages.stream(
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": Prompt,
        }
    ],
    model="claude-3-opus-20240229",
)

with completion as stream:
    for text in stream.text_stream:
            print(text, end="", flush=True)
```

如我们所见，我们生成响应的速度相当快。

![开始使用 Claude 3 Opus，这款刚刚击败了 GPT-4 和 Gemini 的模型](../Images/7e119f1f7917447fe8d0844b12ee8fbb.png)

# Claude 3 流媒体与异步

我们也可以使用异步函数进行流式处理。只需发挥创意，将它们结合起来即可。

```py
import asyncio
from anthropic import AsyncAnthropic

client = AsyncAnthropic()

async def main() -> None:

    completion = client.messages.stream(
        max_tokens=1024,
        messages=[
            {
                "role": "user",
                "content": Prompt,
            }
        ],
        model="claude-3-opus-20240229",
    )
    async with completion as stream:
        async for text in stream.text_stream:
            print(text, end="", flush=True)

await main()
```

![开始使用 Claude 3 Opus，这款刚刚击败了 GPT-4 和 Gemini 的模型](../Images/8b8c68b465c4942b83c54ace5d9c9b92.png)

# Claude 3 视觉

Claude 3 视觉已经随着时间的推移有所改进，要获得响应，你只需将 base64 类型的图像提供给消息 API。

在这个示例中，我们将使用 [郁金香](https://www.pexels.com/photo/tulips-in-a-vase-against-a-green-background-20230232/)（图像 1）和 [火烈鸟](https://www.pexels.com/photo/flamingos-in-the-water-20255306/)（图像 2）来自 Pexel.com，通过提问关于图像的问题来生成响应。

![开始使用 Claude 3 Opus，这款刚刚击败了 GPT-4 和 Gemini 的模型](../Images/d7a19b3cb2258a045c67974c6500d93d.png)

我们将使用 `httpx` 库从 pexel.com 获取两张图片并将其转换为 base64 编码。

```py
import anthropic
import base64
import httpx

client = anthropic.Anthropic()

media_type = "image/jpeg"

img_url_1 = "https://images.pexels.com/photos/20230232/pexels-photo-20230232/free-photo-of-tulips-in-a-vase-against-a-green-background.jpeg"

image_data_1 = base64.b64encode(httpx.get(img_url_1).content).decode("utf-8")

img_url_2 = "https://images.pexels.com/photos/20255306/pexels-photo-20255306/free-photo-of-flamingos-in-the-water.jpeg"

image_data_2 = base64.b64encode(httpx.get(img_url_2).content).decode("utf-8")
```

我们将 base64 编码的图像提供给消息 API 的图像内容块。请按照下面的编码模式进行操作，以成功生成响应。

```py
message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "image",
                    "source": {
                        "type": "base64",
                        "media_type": media_type,
                        "data": image_data_1,
                    },
                },
                {
                    "type": "text",
                    "text": "Write a poem using this image."
                }
            ],
        }
    ],
)
Markdown(message.content[0].text)
```

我们得到了关于郁金香的一首美丽诗歌。

![开始使用 Claude 3 Opus，这款刚刚击败了 GPT-4 和 Gemini 的模型](../Images/d04ec008d2fdc28475af8ded03291d3b.png)

# Claude 3 视觉与多图像

让我们尝试将多个图像加载到同一个 Claude 3 消息 API 中。

```py
message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "Image 1:"
                },
                {
                    "type": "image",
                    "source": {
                        "type": "base64",
                        "media_type": media_type,
                        "data": image_data_1,
                    },
                },
                {
                    "type": "text",
                    "text": "Image 2:"
                },
                {
                    "type": "image",
                    "source": {
                        "type": "base64",
                        "media_type": media_type,
                        "data": image_data_2,
                    },
                },
                {
                    "type": "text",
                    "text": "Write a short story using these images."
                }
            ],
        }
    ],
)
Markdown(message.content[0].text)
```

我们有一个关于郁金香和火烈鸟的短篇故事。

![开始使用 Claude 3 Opus，这款刚刚击败了 GPT-4 和 Gemini 的模型](../Images/75159192948b3a25b4dad56e2f4b3598.png)

如果你在运行代码时遇到问题，这里有一个 [Deepnote 工作区](https://deepnote.com/workspace/abid-5efa63e7-7029-4c3e-996f-40e8f1acba6f/project/Getting-Started-With-Claude-3-Opus-bbf9554c-e970-4aad-8235-b186020cae90/notebook/Notebook%201-4471da3c2e1548258df3124e607a0494)，你可以在这里查看并自行运行代码。

# 结论

我认为 Claude 3 Opus 是一个很有前途的模型，尽管它可能没有 GPT-4 和 Gemini 快。我相信付费用户可能会有更好的速度。

在本教程中，我们了解了来自 Anthropic 的新模型系列 Claude 3，回顾了其基准测试，并测试了其视觉能力。我们还学习了如何生成简单、异步和流式响应。虽然现在还很难说它是否是最好的 LLM，但如果查看官方测试基准，我们可以看到 AI 的新王者已经登上了王座。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理健康问题的学生打造一款 AI 产品。

### 更多相关内容

+   [Claude 2 API 入门指南](https://www.kdnuggets.com/getting-started-with-claude-2-api)

+   [认识 Gorilla：UC 伯克利和微软的 API 增强型 LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

+   [检测 ChatGPT、GPT-4、Bard 和 Claude 的十大工具](https://www.kdnuggets.com/2023/05/top-10-tools-detecting-chatgpt-gpt4-bard-llms.html)

+   [ChatGPT 失宠：Claude 如何成为新的 AI 领袖](https://www.kdnuggets.com/2023/07/chatgpt-dethroned-claude-became-new-ai-leader.html)

+   [免费访问 Claude AI 的三种方法](https://www.kdnuggets.com/2023/06/3-ways-access-claude-ai-free.html)

+   [如何免费访问和使用 Gemini API](https://www.kdnuggets.com/how-to-access-and-use-gemini-api-for-free)
