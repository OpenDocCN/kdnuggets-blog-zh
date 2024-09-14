# OpenChatKit: 开源的 ChatGPT 替代品

> 原文：[https://www.kdnuggets.com/2023/03/openchatkit-opensource-chatgpt-alternative.html](https://www.kdnuggets.com/2023/03/openchatkit-opensource-chatgpt-alternative.html)

![OpenChatKit: 开源的 ChatGPT 替代品](../Images/f9019f2441d2a0bcb680a4bc00478b70.png)

作者提供的图片

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织在 IT 方面

* * *

开源与闭源之间的战争已经持续了一段时间。在 OpenAI 推出 [GPT-3](https://openai.com/blog/gpt-3-apps) 作为闭源模型后，EleutherAI 推出了名为 GPT-Neo 的开源替代品，提供了对比结果。同样，当 [DALL·E 2](https://openai.com/product/dall-e-2) 推出时，Stability AI 发布了开源版本 DALL·E 2，称为 [Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release)。

我们都知道 [ChatGPT](https://openai.com/blog/chatgpt/) 以及人们如何渴望获得模型的开源版本，并在更多控制下安全地构建他们的应用。目前，ChatGPT 提供了 API 访问和微调能力，但你将使用他们的服务和机器来执行各种任务。

2023 年 3 月 10 日，Together Computer 发布了开源版本的 ChatGPT，称为 [OpenChatKit](https://www.together.xyz/blog/openchatkit)。开源替代品允许开发者对聊天机器人的行为有更多控制，并根据其特定需求进行定制。此外，它对更广泛的用户和社区更加可及，尤其是那些可能没有资源访问专有模型的用户。

# 什么是 OpenChatKit

[OpenChatKit](https://huggingface.co/togethercomputer/GPT-NeoXT-Chat-Base-20B) 提供了一套强大的开源工具，用于创建通用和专业的聊天机器人应用。它是模型的第一个版本，开发者发布了一套工具和流程，通过社区贡献来改进模型。

Together Computer 发布了在 Apache-2.0 许可下的 [OpenChatKit 0.15](https://github.com/togethercomputer/OpenChatKit)，包括源代码、模型权重和训练数据集。

你可以在 Hugging Face 上试用基于模型的演示：[OpenChatKit](https://huggingface.co/spaces/togethercomputer/OpenChatKit)。它类似于 ChatGPT，你写一个提示，模型会用答案、代码块、表格或文本来回应你。

![OpenChatKit: 开源 ChatGPT 替代方案](../Images/05f65b7dbcc1c61523b4c56c5f67c645.png)

图片来自作者 | [OpenChatKit](https://huggingface.co/spaces/togethercomputer/OpenChatKit)

OpenChatKit 附带基础机器人和创建自定义聊天机器人应用程序的构建块。

**该工具包由 4 个组件组成：**

1.  针对聊天的指令调优大型语言模型，微调自 EleutherAI 的 GPT-NeoX-20B。

1.  关于如何微调模型以在特定任务上实现高准确率的说明。

1.  可扩展的检索系统，用于使用来自 Wikipedia、新闻源或体育比分的知识更新机器人响应。

1.  从 GPT-JT-6B 微调而来，旨在进行审查以筛选出机器人回应的问题。

# 指令调优的大型语言模型

OpenChatKit 的基础是一个名为 GPT-NeoXT-Chat-Base-20B 的大型语言模型。它基于 EleutherAI 的 GPT-NeoX 模型，并在 4300 万条高质量对话指令上进行了微调。开发团队特别关注了多轮对话、问答、分类、提取和总结等多个任务的调优。

![OpenChatKit: 开源 ChatGPT 替代方案](../Images/c67904e521b13536b4b629b1c541e902.png)

图片来自 [TOGETHER](https://www.together.xyz/blog/openchatkit)

该模型开箱即用，提供了一个强大的基础。正如我们所见，它在 HELM 基准测试中得分高于其基础模型 GPT-NeoX。GPT-NeoXT-Chat-Base-20B 模型在问答、提取和分类任务上表现相当出色。

# 模型的局限性

这是该模型的第一个版本，你会看到许多错误、漏洞和合适的答案。在本次会议中，我们将审查模型难以理解的一些领域。

+   **基于知识**：聊天机器人可能会给出事实错误的结果。ChatGPT 也存在相同的问题。团队正在开发一个检索系统，以更新错误信息。

+   **基于代码**：该模型没有在足够大的源代码语料库上进行训练，因此编写准确的代码可能会遇到困难。你可能会感到沮丧。

+   **上下文切换**：如果在对话过程中开始谈论其他话题，聊天机器人不会自动切换主题，而是继续提供与之前话题相关的答案。

+   **重复**：聊天机器人有时会重复回复或卡住。你可以刷新页面来重置它。

+   **创意回答**：与 ChatGPT 不同，聊天机器人不会生成文章或创意故事。它仅限于短小的回复。

# 结论

OpenChatKit是一个很好的倡议，借助社区的帮助，我们可以很快看到一个更好的聊天机器人版本。如果你期待OpenChatKit能像ChatGPT一样提供令人惊叹的回答，你可能会失望，因为它还处于早期阶段，并且训练数据集不够多样化。

在这篇文章中，我们了解了ChatGPT开源版本的宝贵见解，这对开发者和数据科学社区来说是好消息。此外，我们还探索了其工作原理，并深入研究了该工具包的四个组成部分，这些部分可以帮助创建一个完全可定制的聊天机器人，配备最新的新闻更新和管理功能。

## 资源

尝试演示并阅读更多关于模型的信息，以获取有关模型微调和其他重要工具的资讯。

+   [Hugging Face 演示](https://huggingface.co/spaces/togethercomputer/OpenChatKit)

+   [模型](https://huggingface.co/togethercomputer/GPT-NeoXT-Chat-Base-20B)

+   [GitHub 仓库](https://github.com/togethercomputer/OpenChaT)

+   [公告](https://www.together.xyz/blog/openchatkit)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个AI产品，以帮助那些遭受心理疾病困扰的学生。

### 更多相关内容

+   [8个开源ChatGPT和Bard替代品](https://www.kdnuggets.com/2023/04/8-opensource-alternative-chatgpt-bard.html)

+   [ChatGLM-6B：一个轻量级的开源ChatGPT替代品](https://www.kdnuggets.com/2023/04/chatglm6b-lightweight-opensource-chatgpt-alternative.html)

+   [Dolly 2.0：用于商业用途的ChatGPT开源替代品](https://www.kdnuggets.com/2023/04/dolly-20-chatgpt-open-source-alternative-commercial.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [MiniGPT-4：对GPT-4的轻量级替代方案，增强…](https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html)

+   [HuggingChat Python API：你的免费替代方案](https://www.kdnuggets.com/2023/05/huggingchat-python-api-alternative.html)
