# ChatGLM-6B：轻量级开源ChatGPT替代品

> 原文：[https://www.kdnuggets.com/2023/04/chatglm6b-lightweight-opensource-chatgpt-alternative.html](https://www.kdnuggets.com/2023/04/chatglm6b-lightweight-opensource-chatgpt-alternative.html)

![ChatGLM-6B: 轻量级开源ChatGPT替代品](../Images/9dfdd021af6594d2e14b5f24f29d0ddb.png)

图片由作者提供

最近，我们都很难跟上LLM领域的最新发布。在过去几周里，几个开源ChatGPT替代品变得非常受欢迎。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

在这篇文章中，我们将了解**ChatGLM**系列和**ChatGLM-6B**，一个开源且轻量级的ChatGPT替代品。

让我们开始吧！

# 什么是ChatGLM？

中国清华大学的研究人员开发了ChatGLM系列模型，这些模型的性能与GPT-3和BLOOM等其他模型相当。

ChatGLM 是一个双语大型语言模型，训练时涵盖了中文和英文。目前，以下模型已提供：

+   ChatGLM-130B：一个开源的LLM

+   ChatGLM-100B：未开源，但通过邀请制访问

+   ChatGLM-6B：一个轻量级的开源替代品

虽然这些模型可能与生成预训练变换器（GPT）系列的大型语言模型相似，但**通用语言模型（GLM）预训练框架**使它们有所不同。我们将在下一节中详细了解这一点。

# ChatGLM是如何工作的？

在机器学习中，你可能会知道GLM作为*广义线性模型*，但ChatGLM中的GLM代表*通用语言模型*。

## GLM预训练框架

LLM预训练已经被广泛研究，并且仍然是一个活跃的研究领域。让我们试着理解GLM预训练和GPT风格模型之间的关键区别。

GPT-3系列模型使用的是仅解码器的自回归语言建模。另一方面，在GLM中，目标的优化被制定为**自回归空白填充问题**。

![ChatGLM-6B: 轻量级开源ChatGPT替代品](../Images/23df7f00c157d578290e442e5cae1240.png)

GLM | [图片来源](https://arxiv.org/abs/2103.10360)

简单来说，*自回归空白填充*涉及将连续的文本段落留空，然后顺序地重建这些空白的文本。除了较短的掩码外，还有一个较长的掩码，它随机移除句子末尾的长文本空白。这样做的目的是让模型在自然语言理解和生成任务中表现合理。

另一个区别在于所使用的注意力类型。GPT 大型语言模型组使用单向注意力，而 GLM 大型语言模型组使用 **双向注意力**。使用双向注意力可以更好地捕捉依赖关系，并能提高自然语言理解任务的性能。

## GELU 激活

在 GLM 中，使用 GELU（高斯误差线性单元）激活代替了 ReLU 激活 [1]。

![ChatGLM-6B: 一个轻量级的开源 ChatGPT 替代品](../Images/9f6d2d8afebf440a8d9f0363b8a4db6d.png)

GELU、ReLU 和 ELU 激活 | [图像来源](https://arxiv.org/abs/1606.08415)

GELU 激活对所有输入有非零值，形式如下 [3]：

![ChatGLM-6B: 一个轻量级的开源 ChatGPT 替代品](../Images/c42f168b0c87a3a5c1a921182a2a019d.png)

与 ReLU 激活相比，GELU 激活被发现可以提高性能，尽管计算上比 ReLU 更为复杂。

在 GLM 系列的 LLM 中，ChatGLM-130B 是一个开源模型，表现与 GPT-3 的 Da-Vinci 模型相当。如前所述，截至本文撰写时，存在一个 ChatGLM-100B 版本，仅限邀请访问。

## ChatGLM-6B

关于 ChatGLM-6B 的以下细节使其对终端用户更为友好：

+   约有 62 亿个参数。

+   该模型在1万亿个标记上进行预训练——英文和中文各占一半。

+   随后，使用了如监督微调和带有人类反馈的强化学习等技术。

# ChatGLM 的优点和局限性

让我们通过回顾 ChatGLM 的优点和局限性来总结讨论：

## 优点

从一个双语模型到一个可以在本地运行的开源模型，ChatGLM-6B 具有以下优势：

+   大多数主流大型语言模型都在大量英文文本上进行训练，而其他语言的大型语言模型则不那么常见。ChatGLM 系列 LLM 是双语的，是中文的绝佳选择。该模型在英文和中文方面都表现良好。

+   ChatGLM-6B 为用户设备进行了优化。终端用户的设备通常计算资源有限，因此在没有高性能 GPU 的情况下，几乎无法在本地运行 LLM。通过 [INT4 量化](https://developer.nvidia.com/blog/int4-for-ai-inference/)，ChatGLM-6B 可以以低至 6GB 的内存要求运行。

+   在总结和单一及多查询聊天等各种任务上表现良好。

+   尽管与其他主流 LLMs 相比参数数量大幅减少，ChatGLM-6B 支持最长为 2048 的上下文长度。

## 限制

接下来，我们列出 ChatGLM-6B 的一些限制：

+   尽管 ChatGLM 是一个双语模型，但其在英语中的表现可能不尽如人意。这可以归因于训练中使用的指令主要为中文。

+   由于 ChatGLM-6B 相比 BLOOM、GPT-3 和 ChatGLM-130B 等其他 LLMs 具有**更少的参数**，因此在上下文过长时，其性能可能会较差。因此，ChatGLM-6B 可能比参数更多的模型更频繁地给出不准确的信息。

+   小型语言模型具有*有限的内存容量*。因此，在多轮对话中，模型的性能可能会略有下降。

+   偏见、虚假信息和毒性是所有 LLM 的限制，ChatGLM 也容易受到这些问题的影响。

# 结论

作为下一步，运行本地的 ChatGLM-6B 或尝试 HuggingFace spaces 上的演示。如果你想要深入了解 LLM 的工作原理，这里有一份 [大型语言模型的免费课程列表](/2023/03/top-free-courses-large-language-models.html)。

# 参考文献

[1] Z Du, Y Qian 等，[GLM: 一种自回归空白填充的通用语言模型预训练](https://arxiv.org/abs/2103.10360)，ACL 2022

[2] A Zheng, X Liu 等，[GLM-130B - 一个开放的双语预训练模型](https://arxiv.org/abs/2210.02414)，ICML 2023

[3] D Hendryks, K Gimpel，[高斯误差线性单元（GELUs）](https://arxiv.org/abs/1606.08415)，arXiv，2016

[4] [ChatGLM-6B: HuggingFace Spaces 上的演示](https://huggingface.co/spaces/multimodalart/ChatGLM-6B)

[5] [GitHub 仓库](https://github.com/THUDM/ChatGLM-6B/blob/main/README_en.md)

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位技术作家，喜欢创建长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过编写教程、操作指南等方式与开发者社区分享她的学习经验。

### 更多相关主题

+   [MiniGPT-4: 一种轻量级的 GPT-4 替代品，用于增强…](https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html)

+   [OpenChatKit: 开源的 ChatGPT 替代品](https://www.kdnuggets.com/2023/03/openchatkit-opensource-chatgpt-alternative.html)

+   [8 个 ChatGPT 和 Bard 的开源替代品](https://www.kdnuggets.com/2023/04/8-opensource-alternative-chatgpt-bard.html)

+   [Dolly 2.0: 用于商业用途的 ChatGPT 开源替代品](https://www.kdnuggets.com/2023/04/dolly-20-chatgpt-open-source-alternative-commercial.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [HuggingChat Python API: 你的免费替代品](https://www.kdnuggets.com/2023/05/huggingchat-python-api-alternative.html)
