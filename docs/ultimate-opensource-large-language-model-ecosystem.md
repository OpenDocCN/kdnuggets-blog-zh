# 终极开源大语言模型生态系统

> 原文：[https://www.kdnuggets.com/2023/05/ultimate-opensource-large-language-model-ecosystem.html](https://www.kdnuggets.com/2023/05/ultimate-opensource-large-language-model-ecosystem.html)

![终极开源大语言模型生态系统](../Images/b019a791ef44aab9a22a5fdecd68556d.png)

作者图片

我们正在见证一个开源语言模型生态系统的增长，它们提供了全面的资源，使个人能够为研究和商业目的创建语言应用程序。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

之前，我们曾介绍过 [开放助手](/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html) 和 [OpenChatKit](/2023/03/openchatkit-opensource-chatgpt-alternative.html)。今天，我们将深入探讨 GPT4ALL，它不仅限于特定用途，而是提供全面的构建模块，使任何人都能开发类似于 ChatGPT 的聊天机器人。

# GPT4ALL 项目是什么？

[GPT4ALL](https://github.com/nomic-ai/gpt4all) 是一个提供你所需的一切以便与最先进的自然语言模型合作的项目。你可以访问开源模型和数据集，使用提供的代码进行训练和运行，使用网页界面或桌面应用与它们交互，连接 Langchain 后端以进行分布式计算，并使用 Python API 进行简单集成。

Apache-2 许可的 GPT4All-J 聊天机器人最近由开发者推出，该机器人在大量经过筛选的助手互动语料库上进行训练，包括数学问题、多轮对话、代码、诗歌、歌曲和故事。为了让它更易于访问，他们还发布了 Python 绑定和聊天用户界面，几乎任何人都可以在 CPU 上运行该模型。

你可以通过在桌面上安装原生聊天客户端来亲自尝试。

+   [Mac/OSX](https://gpt4all.io/installers/gpt4all-installer-darwin.dmg)

+   [Windows](https://gpt4all.io/installers/gpt4all-installer-win64.exe)

+   [Ubuntu](https://gpt4all.io/installers/gpt4all-installer-linux.run)

然后，运行 GPT4ALL 程序并下载你选择的模型。你也可以手动下载模型 [这里](https://github.com/nomic-ai/gpt4all-chat#manual-download-of-models) 并在 GUI 中的模型下载对话框指定的位置进行安装。

![终极开源大型语言模型生态系统](../Images/3e40d3940af0629061736ccdb7ea0fcc.png)

作者提供的图片

我在笔记本电脑上使用它的体验非常完美，响应快速且准确。此外，它用户友好，即使是非技术人员也能轻松使用。

![终极开源大型语言模型生态系统](../Images/e02a7b0dd50960521a9abbfd0d57e42a.png)

作者提供的Gif

# GPT4ALL Python 客户端

GPT4ALL 提供 [Python](https://github.com/nomic-ai/pyllamacpp)、[TypeScript](https://github.com/nomic-ai/gpt4all-ts)、[Web Chat接口](https://github.com/nomic-ai/gpt4all-ui) 和 [Langchain](https://python.langchain.com/en/latest/modules/models/llms/integrations/gpt4all.html) 后端。

在这一部分，我们将探讨使用 [nomic-ai/pygpt4all](https://github.com/nomic-ai/pygpt4all) 访问模型的 Python API。

1.  使用PIP安装Python GPT4ALL库。

```py
pip install pygpt4all
```

1.  从 [http://gpt4all.io/models/ggml-gpt4all-l13b-snoozy.bin](http://gpt4all.io/models/ggml-gpt4all-l13b-snoozy.bin) 下载 GPT4All 模型。你也可以在 [这里](https://github.com/nomic-ai/gpt4all-chat#manual-download-of-models) 浏览其他模型。

1.  创建一个文本回调函数，加载模型，并提供提示给 `mode.generate()` 函数以生成文本。查看库的 [文档](https://nomic-ai.github.io/pygpt4all/) 以了解更多信息。

```py
from pygpt4all.models.gpt4all import GPT4All

def new_text_callback(text):
    print(text, end="")

model = GPT4All("./models/ggml-gpt4all-l13b-snoozy.bin")
model.generate("Once upon a time, ", n_predict=55, new_text_callback=new_text_callback) 
```

此外，你可以下载并使用变换器进行推断。只需提供模型名称和版本。在我们的案例中，我们正在访问最新改进的v1.3-groovy模型。

```py
from transformers import AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained(
    "nomic-ai/gpt4all-j", revision="v1.3-groovy"
) 
```

# 入门指南

[nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) 仓库包含用于训练和推断的源代码、模型权重、数据集和文档。你可以先尝试几个模型，然后尝试使用Python客户端或LangChain进行集成。

GPT4ALL 提供了一个 CPU 量化的 GPT4All 模型检查点。要访问它，我们需要：

+   从 [Direct Link](https://the-eye.eu/public/AI/models/nomic-ai/gpt4all/gpt4all-lora-quantized.bin) 或 [[Torrent-Magnet]](https://tinyurl.com/gpt4all-lora-quantized) 下载 gpt4all-lora-quantized.bin 文件。

+   克隆此仓库并将下载的 bin 文件移动到 `chat` 文件夹中。

+   运行适当的命令以访问模型：

    +   **M1 Mac/OSX**: `cd chat;./gpt4all-lora-quantized-OSX-m1`

    +   **Linux**: `cd chat;./gpt4all-lora-quantized-linux-x86`

    +   **Windows (PowerShell)**: `cd chat;./gpt4all-lora-quantized-win64.exe`

    +   **Intel Mac/OSX**: `cd chat;./gpt4all-lora-quantized-OSX-intel`

你还可以前往Hugging Face Spaces并尝试 [Gpt4all](https://huggingface.co/spaces/Monster/GPT4ALL) 演示。虽然它不是官方的，但这是一个开始。

![终极开源大型语言模型生态系统](../Images/e7bd7f43f2e5ee8e130144401423958f.png)

图片来自 [Gpt4all](https://huggingface.co/spaces/Monster/GPT4ALL)

**资源：**

+   技术报告: [GPT4All-J: 一款Apache-2授权的助手风格聊天机器人](https://static.nomic.ai/gpt4all/2023_GPT4All-J_Technical_Report_2.pdf)

+   GitHub: [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all)

+   Python API: [nomic-ai/pygpt4all](https://github.com/nomic-ai/pygpt4all)

+   模型: [nomic-ai/gpt4all-j](https://huggingface.co/nomic-ai/gpt4all-j)

+   数据集: [nomic-ai/gpt4all-j-prompt-generations](https://huggingface.co/datasets/nomic-ai/gpt4all-j-prompt-generations)

+   Hugging Face Demo: [Gpt4all](https://huggingface.co/spaces/Monster/GPT4ALL)

+   ChatUI: [nomic-ai/gpt4all-chat: gpt4all-j聊天](https://github.com/nomic-ai/gpt4all-chat)

GPT4ALL后端: [GPT4All — ???? LangChain 0.0.154](https://python.langchain.com/en/latest/modules/models/llms/integrations/gpt4all.html)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证数据科学专家，热爱构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为面临心理健康困扰的学生开发一个AI产品。

### 更多相关话题

+   [NExT-GPT介绍：任意到任意的多模态大型语言模型](https://www.kdnuggets.com/introduction-to-nextgpt-anytoany-multimodal-large-language-model)

+   [探索Zephyr 7B：最新大型语言模型的全面指南…](https://www.kdnuggets.com/exploring-the-zephyr-7b-a-comprehensive-guide-to-the-latest-large-language-model)

+   [掌握大型语言模型微调的7个步骤](https://www.kdnuggets.com/7-steps-to-mastering-large-language-model-fine-tuning)

+   [免费精通课程：成为大型语言模型专家](https://www.kdnuggets.com/ree-mastery-course-become-a-large-language-model-expert)

+   [Bark: 终极音频生成模型](https://www.kdnuggets.com/2023/05/bark-ultimate-audio-generation-model.html)

+   [顶级开源大型语言模型](https://www.kdnuggets.com/2022/09/john-snow-top-open-source-large-language-models.html)
