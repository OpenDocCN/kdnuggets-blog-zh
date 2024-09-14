# Orca LLM: 模拟 ChatGPT 的推理过程

> 原文：[`www.kdnuggets.com/2023/06/orca-llm-reasoning-processes-chatgpt.html`](https://www.kdnuggets.com/2023/06/orca-llm-reasoning-processes-chatgpt.html)

![Orca LLM: 模拟 ChatGPT 的推理过程](img/480d8edf9cc4de3cce60f27feb4daa41.png)

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

在大型语言模型（LLM）领域，始终在追求在不影响效率的情况下提升小型模型的能力。传统的方法是使用模仿学习，小型模型从大型基础模型（LFM）生成的输出中学习。然而，这种方法面临几个挑战，包括来自浅层 LFM 输出的有限模仿信号、小规模的同质训练数据以及缺乏严格评估。这常常导致小型模型模仿风格但无法模仿 LFM 的推理过程。

论文**[Orca: 从 GPT-4 的复杂解释踪迹中渐进学习](https://arxiv.org/abs/2306.02707)**介绍了 Orca，一个设计用来模仿大型基础模型（LFM）如 GPT-4 的推理过程的 13 亿参数模型。与传统的大型语言模型（LLM）不同，Orca 采用了一种独特的训练方法，将渐进学习和教师辅助相结合，以克服小型学生模型与其大型对应模型之间的能力差距。

# 训练方法

Orca 的训练过程包括两个阶段。

在第一阶段，Orca 在 FLAN-5M 上进行训练，该训练包括 ChatGPT 的增强技术。这一中级教师助手有助于弥合 Orca 与 GPT-4 之间的能力差距，后者具有显著更大的参数规模。通过利用 ChatGPT 的能力，Orca 受益于改进的模仿学习表现。

在第二阶段，Orca 接受 FLAN-1M 的训练，该训练融合了 GPT-4 的增强技术。这种渐进的学习方法遵循课程学习范式，其中学生模型从简单的例子学习，然后再处理更具挑战性的例子。通过逐步将 Orca 暴露于越来越复杂的推理和逐步解释中，该模型提升了其推理能力和模仿技能。

# 优势和贡献

Orca 的训练方法相比传统 LLM 具有多个优势。

首先，它通过利用中间教师模型解决了能力差距问题，使 Orca 能够从更有能力的来源学习。这种方法已被证明能够提高较小学生模型的模仿学习性能。

其次，Orca 训练的渐进学习方面使模型能够逐步积累知识。通过从简单的示例开始，逐渐引入更复杂的示例，Orca 为推理和生成解释打下了更坚实的基础。

此外，Orca 模仿像 GPT-4 这样的 LFMs 的推理过程的能力，为各种任务的增强性能打开了可能性。通过利用 GPT-4 的解释痕迹和逐步思维过程提供的丰富信号，Orca 获得了宝贵的见解，并提升了自身能力。

# 性能基准

Orca 在复杂的零-shot 推理基准测试中表现出色。在 Big-Bench Hard (BBH)等基准测试中，比传统的最先进的指令调整模型 Vicuna-13B 高出 100%以上，在 AGIEval 中高出 42%以上。此外，Orca 在 BBH 基准测试中取得了与 ChatGPT 相同的分数，并在 SAT、LSAT、GRE 和 GMAT 等专业和学术考试中表现具有竞争力。考虑到这些都是零-shot 设置且没有链式思维，Orca 仍表现出竞争力，但与 GPT-4 相比略逊一筹，这一点尤其令人印象深刻。

# 含义与未来方向

Orca 的发展代表了 LLMs 领域的重大进步。通过学习丰富的信号并模仿 LFMs 的推理过程，Orca 能够以高度准确性执行复杂的推理任务。这具有广泛的影响，尤其是在需要复杂推理和问题解决的领域。

此外，这项研究表明，从逐步 AI 模型解释中学习是一种有前景的方向，有助于提升模型能力。这为 LLMs 领域的研究和开发开辟了新的途径。

# 结论

Orca 提出了一种新的大规模语言模型训练方法，将渐进学习和教师辅助结合在一起，以增强模仿学习。通过利用中间教师模型，并逐渐将学生模型暴露于更复杂的示例中，Orca 克服了能力差距，提升了推理和生成解释的能力。该论文的发现有助于模仿学习技术的进步，并对未来语言模型的发展产生了影响。

有关 Orca 及其研究的更多细节，请参阅[微软的介绍文章](https://www.microsoft.com/en-us/research/publication/orca-progressive-learning-from-complex-explanation-traces-of-gpt-4/)和[相关研究论文](https://arxiv.org/pdf/2306.02707.pdf)。

### 更多相关话题

+   [增强 LLM 推理：揭示链式代码提示](https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting)

+   [Web LLM：将 LLM 聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [ReAct，推理与行动：用工具增强 LLM！](https://www.kdnuggets.com/react-reasoning-and-acting-augments-llms-with-tools)

+   [思维传播：复杂推理的类比方法……](https://www.kdnuggets.com/thought-propagation-an-analogical-approach-to-complex-reasoning-with-large-language-models)

+   [Visual ChatGPT：微软结合 ChatGPT 和 VFMs](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [ChatGPT CLI：将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)
