# Dolly 2.0：ChatGPT 开源商业用途替代方案

> 原文：[https://www.kdnuggets.com/2023/04/dolly-20-chatgpt-open-source-alternative-commercial.html](https://www.kdnuggets.com/2023/04/dolly-20-chatgpt-open-source-alternative-commercial.html)

![Dolly 2.0：ChatGPT 开源商业用途替代方案](../Images/d7a0e4eeda75b95cbe58eaf90ced9033.png)

作者提供的图片 | Bing 图片创作者

[Dolly 2.0](https://www.databricks.com/blog/2023/04/12/dolly-first-open-commercially-viable-instruction-tuned-llm) 是一种开源的、遵循指令的大语言模型（LLM），经过人类生成的数据集进行了微调。它可用于研究和商业目的。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

![Dolly 2.0：ChatGPT 开源商业用途替代方案](../Images/6d3c390dddca565bff213d3fe789e4f5.png)

图片来源于 [Hugging Face Space by RamAnanth1](https://huggingface.co/spaces/RamAnanth1/Dolly-v2)

之前，Databricks 团队发布了**Dolly 1.0**，这是一种大语言模型（LLM），具有类似 ChatGPT 的指令跟随能力，训练成本不到 $30。它使用了斯坦福 Alpaca 团队的数据集，该数据集在受限许可（仅限研究）下。

Dolly 2.0 通过在高质量人类生成的指令数据集上微调了 12B 参数的语言模型 ([Pythia](https://arxiv.org/abs/2304.01373))，解决了这个问题，该数据集由 Databricks 员工标注。模型和数据集都可用于商业用途。

# 为什么我们需要商业许可证数据集？

Dolly 1.0 的训练数据来自斯坦福 Alpaca 数据集，该数据集使用了 OpenAI API 创建。该数据集包含了 ChatGPT 的输出，并防止任何人利用该数据集与 OpenAI 竞争。简而言之，你不能基于这个数据集构建商业聊天机器人或语言应用程序。

最近几周发布的大多数最新模型都遇到了相同的问题，例如 [Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html)、[Koala](https://bair.berkeley.edu/blog/2023/04/03/koala/)、[GPT4All](https://github.com/nomic-ai/gpt4all) 和 [Vicuna](https://vicuna.lmsys.org/)。为了应对这些问题，我们需要创建新的高质量数据集以供商业使用，这正是 Databricks 团队通过 databricks-dolly-15k 数据集所做的。

# databricks-dolly-15k 数据集

新的数据集包含 15,000 个高质量的人类标注的提示/回应对，这些数据对设计指令调优的大型语言模型非常有用。[databricks-dolly-15k](https://github.com/databrickslabs/dolly/tree/master/data) 数据集采用 [Creative Commons Attribution-ShareAlike 3.0 Unported License](https://creativecommons.org/licenses/by-sa/3.0/) 许可协议，允许任何人使用、修改并创建商业应用。

## 他们是如何创建 databricks-dolly-15k 数据集的？

OpenAI 的研究 [论文](https://arxiv.org/pdf/2203.02155.pdf) 表示，原始的 InstructGPT 模型是基于 13,000 个提示和回应进行训练的。利用这些信息，Databricks 团队开始着手这项工作，但生成 13k 个问题和答案是一项艰巨的任务。他们不能使用合成数据或 AI 生成的数据，必须对每个问题生成原创答案。这就是他们决定使用 5,000 名 Databricks 员工来创建人类生成数据的原因。

Databricks 举办了一场比赛，前 20 名标注者将获得丰厚的奖品。在这场比赛中，有 5,000 名对 LLMs 非常感兴趣的 Databricks 员工参与了比赛。

# 结果

dolly-v2-12b 不是一个最先进的模型。在一些评估基准上，它的表现不如 dolly-v1-6b。这可能与底层微调数据集的组成和规模有关。Dolly 模型系列仍在积极开发中，所以你可能会在未来看到一个性能更好的更新版本。

简而言之，dolly-v2-12b 模型在性能上优于 EleutherAI/gpt-neox-20b 和 EleutherAI/pythia-6.9b。

![Dolly 2.0: ChatGPT 开源商业替代方案](../Images/13fad0e6224eb488700484ed15d74930.png)

图片来自 [Free Dolly](https://www.databricks.com/blog/2023/04/12/dolly-first-open-commercially-viable-instruction-tuned-llm)

# 入门指南

Dolly 2.0 完全开源。它包括训练代码、数据集、模型权重和推理管道。所有组件都适合商业使用。你可以在 Hugging Face Spaces 上尝试该模型，[Dolly V2 by RamAnanth1](https://huggingface.co/spaces/RamAnanth1/Dolly-v2)。

![Dolly 2.0: ChatGPT 开源商业替代方案](../Images/29d7308f79126c2abd0270170ddc33af.png)

图片来自 [Hugging Face](https://huggingface.co/databricks/dolly-v2-12b)

**资源：**

+   训练和推理代码：[databrickslabs/dolly](https://github.com/databrickslabs/dolly)

+   Dolly 2.0 模型权重：[databricks/dolly-v2-12b](https://huggingface.co/databricks/dolly-v2-12b)

+   databricks-dolly-15k 数据集：[dolly/data](https://github.com/databrickslabs/dolly/tree/master/data)

Dolly 2.0 演示：[Dolly V2 by RamAnanth1](https://huggingface.co/spaces/RamAnanth1/Dolly-v2)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)**（[@1abidaliawan](https://twitter.com/1abidaliawan)）是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个AI产品，帮助那些受心理疾病困扰的学生。

### 更多相关话题

+   [OpenChatKit：开源ChatGPT替代方案](https://www.kdnuggets.com/2023/03/openchatkit-opensource-chatgpt-alternative.html)

+   [8种开源ChatGPT和Bard的替代方案](https://www.kdnuggets.com/2023/04/8-opensource-alternative-chatgpt-bard.html)

+   [ChatGLM-6B：一种轻量级的开源ChatGPT替代方案](https://www.kdnuggets.com/2023/04/chatglm6b-lightweight-opensource-chatgpt-alternative.html)

+   [闭源与开源图像标注](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [MiniGPT-4：一种轻量级的GPT-4替代方案，用于增强…](https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html)
