# 在你的笔记本电脑上使用LLMs的5种方法

> 原文：[https://www.kdnuggets.com/5-ways-to-use-llms-on-your-laptop](https://www.kdnuggets.com/5-ways-to-use-llms-on-your-laptop)

![在你的笔记本电脑上使用LLMs的5种方法](../Images/959531228a0af4fd6fcf399f422784d8.png)

图片由作者提供

在线访问ChatGPT非常简单——你只需一个互联网连接和一个好的浏览器。然而，这样做可能会妨碍你的隐私和数据。OpenAI会存储你的提示响应和其他元数据以重新训练模型。虽然这对一些人来说可能不是问题，但注重隐私的人可能更愿意在本地使用这些模型，而没有任何外部跟踪。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

在这篇文章中，我们将探讨五种在本地使用大语言模型（LLMs）的方法。大部分软件兼容所有主要操作系统，并可以轻松下载和安装以供立即使用。通过在你的笔记本电脑上使用LLMs，你可以自由选择自己的模型。你只需从HuggingFace中心下载模型并开始使用。此外，你可以授权这些应用访问你的项目文件夹并生成上下文相关的响应。

# 1\. GPT4All

[GPT4All](https://gpt4all.io/index.html)是一个前沿的开源软件，使用户能够轻松下载和安装最先进的开源模型。

只需从网站上下载GPT4ALL并在系统上安装。接下来，从面板中选择适合你需求的模型并开始使用。如果你安装了CUDA（Nvidia GPU），GPT4ALL将自动开始使用你的GPU以每秒最多生成30个token的快速响应。

![在你的笔记本电脑上使用LLMs的5种方法](../Images/c68d67ddd46c9442bee98a695cfe0c78.png)

你可以提供对包含重要文档和代码的多个文件夹的访问权限，GPT4ALL将使用检索增强生成（Retrieval-Augmented Generation）生成响应。GPT4ALL用户友好、快速，并且在AI社区中很受欢迎。

阅读有关GPT4ALL的博客，了解更多功能和用例：[终极开源大语言模型生态系统](/2023/05/ultimate-opensource-large-language-model-ecosystem.html)。

# 2\. LM Studio

[LM Studio](https://lmstudio.ai/) 是一个新软件，相较于 GPT4ALL 提供了多个优势。用户界面非常出色，你可以通过几次点击安装 Hugging Face Hub 中的任何模型。此外，它提供 GPU 卸载和 GPT4ALL 中没有的其他选项。然而，LM Studio 是封闭源代码的，无法通过读取项目文件生成上下文相关的响应。

![5 Ways To Use LLMs On Your Laptop](../Images/4f5f0c4245979ed4d3f59ee7f7e28f52.png)

LM Studio 提供对数千个开源 LLM 的访问，使你能够启动一个本地推理服务器，其行为类似于 OpenAI 的 API。你可以通过互动用户界面和多个选项修改 LLM 的响应。

此外，请阅读 [Run an LLM Locally with LM Studio](/run-an-llm-locally-with-lm-studio) 以了解更多有关 LM Studio 及其关键功能的信息。

# 3\. Ollama

[Ollama](https://github.com/ollama/ollama) 是一个命令行界面 (CLI) 工具，能够快速操作大型语言模型，如 Llama 2、Mistral 和 Gemma。如果你是黑客或开发人员，这个 CLI 工具是一个极好的选择。你可以下载并安装软件，并使用 `the llama run llama2` 命令开始使用 LLaMA 2 模型。你可以在 GitHub 仓库中找到其他模型命令。

![5 Ways To Use LLMs On Your Laptop](../Images/33c8e965b3f5c20325e58d9f9749d31c.png)

它还允许你启动一个本地 HTTP 服务器，并与其他应用程序集成。例如，你可以通过提供本地服务器地址来使用 Code GPT VSCode 扩展，开始将其用作 AI 编程助手。

使用这些 [Top 5 AI Coding Assistants](/top-5-ai-coding-assistants-you-must-try) 改善你的编码和数据工作流程。

# 4\. LLaMA.cpp

[LLaMA.cpp](https://github.com/ggerganov/llama.cpp) 是一个提供 CLI 和图形用户界面 (GUI) 的工具。它允许你在本地无障碍地使用任何开源 LLM。该工具高度可定制，并能快速响应任何查询，因为它完全用纯 C/C++ 编写。

![5 Ways To Use LLMs On Your Laptop](../Images/e9ff13b2925d46e2dde511a831ca7d36.png)

LLaMA.cpp 支持所有类型的操作系统、CPU 和 GPU。你还可以使用多模态模型，如 LLaVA、BakLLaVA、Obsidian 和 ShareGPT4V。

了解如何 [Run Mixtral 8x7b On Google Colab For Free](/running-mixtral-8x7b-on-google-colab-for-free) 使用 LLaMA.cpp 和 Google GPUs。

# 5\. NVIDIA Chat with RTX

要使用 [NVIDIA Chat with RTX](https://www.nvidia.com/en-us/ai-on-rtx/chat-with-rtx-generative-ai/)，你需要在笔记本电脑上下载并安装 Windows 11 应用程序。该应用程序兼容于具有 30 系列或 40 系列 RTX NVIDIA 显卡、至少 8GB 内存和 50GB 可用存储空间的笔记本电脑。此外，你的笔记本电脑应至少具有 16GB 内存，以便顺畅运行 Chat with RTX。

![5 Ways To Use LLMs On Your Laptop](../Images/62b35ec13c71bd6451ed392e51013cb6.png)

使用 Chat with RTX，你可以在笔记本电脑上本地运行 LLaMA 和 Mistral 模型。这是一个快速高效的应用程序，甚至可以从你提供的文档或 YouTube 视频中学习。然而，需要注意的是，Chat with RTX 依赖于 TensorRTX-LLM，该技术仅支持 30 系列或更新的 GPU。

# 结论

如果你想在确保数据安全和隐私的同时利用最新的 LLMs，可以使用 GPT4All、LM Studio、Ollama、LLaMA.cpp 或 NVIDIA Chat with RTX 等工具。每种工具都有其独特的优势，无论是易于使用的界面、命令行访问，还是对多模态模型的支持。通过正确的设置，你可以拥有一个强大的 AI 助手，生成定制化的上下文感知响应。

我建议从 GPT4All 和 LM Studio 开始，因为它们涵盖了大部分基本需求。之后，你可以尝试 Ollama 和 LLaMA.cpp，最后尝试 Chat with RTX。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个 AI 产品，帮助那些面临心理健康问题的学生。

### 更多相关话题

+   [通过 openplayground 在你的笔记本电脑上轻松探索 LLMs](https://www.kdnuggets.com/2023/04/explore-llms-easily-laptop-openplayground.html)

+   [使用 DuckDB 和… 将你的笔记本电脑变成个人分析引擎](https://www.kdnuggets.com/turn-your-laptop-into-a-personal-analytics-engine-with-duckdb-and-motherduck)

+   [用 LLMs 将非结构化数据转换为结构化洞察的 5 种方法](https://www.kdnuggets.com/5-ways-of-converting-unstructured-data-into-structured-insights-with-llms)

+   [利用 AI 进行供应链管理的 5 种方法](https://www.kdnuggets.com/2022/02/5-ways-ai-supply-chain-management.html)

+   [你可以利用 ChatGPT 的代码解释器进行数据科学的 5 种方式](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)

+   [利用 ChatGPT Vision 进行数据分析的 5 种方法](https://www.kdnuggets.com/5-ways-you-can-use-chatgpt-vision-for-data-analysis)
