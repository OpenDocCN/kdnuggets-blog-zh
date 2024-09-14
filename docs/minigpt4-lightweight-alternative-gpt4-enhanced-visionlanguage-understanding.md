# MiniGPT-4：一种轻量级的 GPT-4 替代方案，增强视觉-语言理解

> 原文：[https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html](https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html)

![MiniGPT-4：一种轻量级的 GPT-4 替代方案，增强视觉-语言理解](../Images/a8065b229bd81d10ac5915b9cbd4dd3a.png)

作者提供的图片

我们看到 ChatGPT 的 [开源替代品](/2023/04/8-opensource-alternative-chatgpt-bard.html) 发展迅速，但没有人专注于提供多模态能力的 GPT-4 替代品。GPT-4 是一种先进且强大的多模态模型，能够接受图像和文本输入并输出文本响应。它可以更准确地解决复杂问题并从错误中学习。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业之路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

在这篇文章中，我们将了解 MiniGPT-4，这是一种开源替代品，旨在提供轻量级的视觉和文本理解能力，与 OpenAI 的 GPT-4 类似。

# 什么是 MiniGPT-4？

与 GPT-4 类似，MiniGPT-4 能够生成详细的图像描述、使用图像编写故事，以及利用手绘用户界面创建网站。它通过使用更先进的大型语言模型（LLM）来实现这一点。

你可以通过尝试演示来亲身体验：[MiniGPT-4 - 由 Vision-CAIR 提供的 Hugging Face 空间](https://huggingface.co/spaces/Vision-CAIR/minigpt4)。

![MiniGPT-4：一种轻量级的 GPT-4 替代方案，增强视觉-语言理解](../Images/e8ba20ea87bdaa0b5c63e9e15eb48ec3.png)

作者提供的图片 | MiniGPT-4 演示

[MiniGPT-4：利用先进的大型语言模型增强视觉-语言理解](https://github.com/Vision-CAIR/MiniGPT-4/blob/main/MiniGPT_4.pdf) 的作者发现，直接在原始图像-文本对上进行预训练可能会产生缺乏连贯性的较差结果，包括重复和断裂的句子。为了解决这个问题，他们策划了一个高质量、良好对齐的数据集，并使用对话模板对模型进行了微调。

MiniGPT-4 模型在计算上非常高效，因为他们仅训练了一个投影层，利用了大约 500 万对对齐的图像-文本对。

# MiniGPT-4 如何工作？

MiniGPT-4 将一个冻结的视觉编码器与一个名为 Vicuna 的冻结 LLM 对齐，使用的只是一个投影层。视觉编码器由预训练的 ViT 和 Q-Former 模型组成，这些模型通过一个线性投影层与先进的 Vicuna 大型语言模型连接。

![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/3ffbd6cd020c6ce173649aeaadc957c4.png)

作者提供的图片 | MiniGPT-4 的架构。

MiniGPT-4 仅需训练线性层即可将视觉特征与 Vicuna 对齐。因此，它体积小、所需计算资源少，并且产生的结果与 GPT-4 相似。

# 结果

如果你查看 [minigpt-4.github.io](https://minigpt-4.github.io/##Results:~:text=of%20MiniGPT%2D4.-,Results,-Acknowledgement) 的官方结果，你会看到作者通过上传手绘 UI 并要求其生成 HTML/JS 网站创建了一个网站。MiniGPT-4 理解了上下文，并生成了 HTML、CSS 和 JS 代码。这真是令人惊叹。

![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/b9659591239a34c0a4fd206ffaf4b638.png)![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/66fe0efa5e05d4ffef73526e5b0bde17.png)

图片来源于 [minigpt-4.github.io](https://minigpt-4.github.io/##Results:~:text=of%20MiniGPT%2D4.-,Results,-Acknowledgement)

他们还展示了如何通过提供食物图像生成食谱、为产品编写广告、描述复杂图像、解释画作等。

让我们通过访问 [MiniGPT-4](https://huggingface.co/spaces/Vision-CAIR/minigpt4) 演示自行尝试。正如我们所见，我提供了 Bing AI 生成的图像，并要求 MiniGPT-4 使用它编写一个故事。结果令人惊叹。

故事是连贯的。

![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/4100135b4ad29364b00cfea1c211ff08.png)

作者提供的图片 | MiniGPT-4 演示

我想了解更多，因此我让它继续写作，就像一个 AI 聊天机器人一样，它不断编写情节。

![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/19adcb1a7ff0d2012b34a246a782cabd.png)

作者提供的图片 | MiniGPT-4 演示

在第二个例子中，我让它帮助我改进图像设计，然后要求它为博客生成图像字幕。

![MiniGPT-4: A Lightweight Alternative to GPT-4 for Enhanced Vision-language Understanding](../Images/9878fc323da686c9292cde6507b7bc2b.png)

作者提供的图片 | MiniGPT-4 演示

MiniGPT-4 真是太棒了。它从错误中学习，并生成高质量的回应。

# 局限性

MiniGPT-4 具有许多先进的视觉-语言能力，但仍然面临一些局限性。

+   目前，即使使用高端 GPU，模型推断仍然很慢，这可能导致结果延迟。

+   该模型基于大型语言模型（LLMs），因此继承了它们的一些局限性，例如推理能力不可靠和产生虚假知识。

+   该模型的视觉感知有限，可能难以识别图像中的详细文本信息。

# 开始使用

该项目包括源代码的训练、微调和推理。还包括公开的模型权重、数据集、研究论文、演示视频以及 Hugging Face 演示的链接。

你可以开始编程，开始在你的数据集上微调模型，或者通过官方页面上的各种演示实例体验模型。

+   **官方网站：** [minigpt-4.github.io](https://minigpt-4.github.io)

+   **研究论文：** [MiniGPT-4/MiniGPT_4.pdf](https://github.com/Vision-CAIR/MiniGPT-4/blob/main/MiniGPT_4.pdf)

+   **GitHub：** [Vision-CAIR/MiniGPT-4](https://github.com/Vision-CAIR/MiniGPT-4)

+   **Gradio 演示：** [MiniGPT-4 演示](https://48da7e23bcadec7551.gradio.live/)

+   **演示视频：** [MiniGPT-4：利用先进的大型语言模型提升视觉-语言理解](https://www.youtube.com/watch?v=__tftoxpBAw)

+   **模型权重：** [Vision-CAIR/MiniGPT-4](https://huggingface.co/Vision-CAIR/MiniGPT-4)

+   **数据集：** [Vision-CAIR/cc_sbu_align](https://huggingface.co/datasets/Vision-CAIR/cc_sbu_align)

这是模型的第一个版本。你将在接下来的日子里看到一个更改进的版本，敬请关注。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些挣扎于心理疾病的学生打造一个 AI 产品。

### 相关内容

+   [ChatGLM-6B：一种轻量级、开源的 ChatGPT 替代品](https://www.kdnuggets.com/2023/04/chatglm6b-lightweight-opensource-chatgpt-alternative.html)

+   [将人类与 AI 代理结合起来提升客户体验](https://www.kdnuggets.com/2024/06/softweb/bringing-human-and-ai-agents-together-for-enhanced-customer-experience)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [OpenChatKit：开源 ChatGPT 替代品](https://www.kdnuggets.com/2023/03/openchatkit-opensource-chatgpt-alternative.html)

+   [8 种开源 ChatGPT 和 Bard 替代方案](https://www.kdnuggets.com/2023/04/8-opensource-alternative-chatgpt-bard.html)

+   [HuggingChat Python API：你的无成本替代方案](https://www.kdnuggets.com/2023/05/huggingchat-python-api-alternative.html)
