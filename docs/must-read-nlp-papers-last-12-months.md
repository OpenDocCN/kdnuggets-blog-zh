# 过去 12 个月必读的 NLP 论文

> 原文：[`www.kdnuggets.com/2023/03/must-read-nlp-papers-last-12-months.html`](https://www.kdnuggets.com/2023/03/must-read-nlp-papers-last-12-months.html)

![过去 12 个月必读的 NLP 论文](img/def988fd066629d818503834d972600a.png)

由[Anil Sharma](https://www.pexels.com/photo/birds-perched-on-tree-branch-10952775/) 拍摄的照片，发布于[Pexels](https://www.pexels.com/photo/birds-perched-on-tree-branch-10952775/)

自从 2018 年 10 月**突破性发布**的[BERT](https://arxiv.org/abs/1810.04805)以来，通过巧妙的优化和增强计算，机器学习达到了更高的高度。BERT，即双向编码器表示变换器，引入了神经网络架构的新范式。**变换器**在机器学习能力方面起到了重要的解锁作用。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

自然语言处理（NLP）领域的进一步进展改善了外语翻译，增强了无代码应用程序，提高了聊天机器人的流利度，并迅速为一系列最先进的基准设定了新的标准。

除了这些显著成就，大型语言模型（LLMs）的发展也不乏争议。在 2021 年发表的"[随机鹦鹉](https://dl.acm.org/doi/10.1145/3442188.3445922)"论文中，包括机器学习工程师和伦理学家 Timnit Gebru 在内的研究团队批评了这些模型：

+   施加了一个令人谴责的**环境成本**

+   **排除边缘化声音** 通过不优雅的数据集策划

+   **剽窃** 互联网内容和盗用人类创作者的作品

Gebru 被迅速解雇了谷歌伦理人工智能团队的职位。

## 在这篇文章中

我们探讨了过去一年中发表的四篇 NLP 论文，它们代表了最新的进展。理解这些发展将提高你作为数据科学家的能力，并使你处于这个动态研究领域的前沿。

## 1. [训练计算最优的大型语言模型](https://arxiv.org/abs/2203.15556)

本文探讨了使用变换器架构的语言模型的理想模型大小和标记数。它旨在回答在预定计算预算下，模型的理想参数数量和数据集大小是什么。

研究人员发现，在以往的案例中，大型语言模型（LLMs）似乎严重训练不足。作者批评这些团队过于强调计算资源的扩展，而忽视了训练数据量的重要性。

作者总结认为，为了实现计算最优化训练，模型规模和训练标记的数量应该等比例扩展。换句话说，

> 每当模型规模翻倍时，训练标记的数量也应翻倍。

研究表明，一个相对较小的模型（70B 参数）在训练数据量增加 4 倍的情况下，能够在最先进的基准测试（如多任务语言理解（[MMLU](https://paperswithcode.com/sota/multi-task-language-understanding-on-mmlu)）中 consistently 超越较大的模型（最高可达 530B 参数）。

增强的训练数据使得较小的模型在推理和微调时可以使用显著更少的计算资源。这对下游应用具有良好的前景。

**总结** — 本文表明，之前对扩展规律的理解是错误的。实际上，当使用足够广泛的标记数量进行训练时，较小的网络可以显著优于较大的网络。

## 2. [通过人类反馈训练语言模型以遵循指令](https://arxiv.org/abs/2203.02155)

增强 LLMs 的计算能力并不会自动提升它们解读用户意图的能力。作为这个事实的一个令人担忧的结果，LLMs 可能会提供不真实或有害的结果。

> 本文强调了一种使用人类反馈来微调语言模型的新方法，以便在各种任务中更好地使输出与用户意图对齐。

研究人员从 OpenAI API 提示集合开始，收集了一个数据集。他们利用这些数据通过监督学习对 GPT-3 进行微调。随后，基于用户输入的强化学习生成了一个新的数据集，用于对模型输出进行排名。研究人员然后使用这些数据进一步微调监督模型，最终得到一个他们称之为 InstructGPT 的模型。

与原始的 GPT-3 相比，InstructGPT 的参数少了 100 倍，但它在人工评估中却能够超越 GPT-3。

在测试数据上，InstructGPT 模型更有可能诚实回应，且不太可能生成有害内容。尽管 InstructGPT 仍偶尔会犯基本错误，这些发现表明，通过人机互动的微调是使语言模型与人类意图匹配的一条可行路线。

**总结** — 本文表明，进行人类反馈强化学习是一种极其有用的低资源方式，可以使现有模型更具实用性。

## 3. [通用智能体](https://www.deepmind.com/publications/a-generalist-agent)

本文探讨了改进后的模型，这些模型能够进行 Atari 游戏、图像描述、文本生成、利用机器人手臂堆叠物理块等更多任务。

> 模型 Gato 由一个单一的神经网络组成，在各种任务中权重保持不变。

Gato 源于放大的行为克隆，这是一种序列建模挑战。将多个模态编码到一个单一的标记向量空间中构成了研究人员在努力过程中面临的最大障碍。研究在标准视觉和语言数据集的标记化方面取得了一些进展。此外，研究人员还寻求了新的解决方案，以应对典型序列模型问题中的上下文窗口长度确定问题。

**总结** — 这篇论文展示了多模态模型表现良好，并且很可能是建模范式的未来。与以前只能在狭窄领域表现的最先进模型相比，Gato 执行了能够处理各种任务和多种模态的通用策略。

## 4. [大型语言模型是零样本推理者](https://arxiv.org/abs/2205.11916)

大型语言模型（LLMs）在使用狭窄、特定任务的示例进行少样本学习方面非常出色。这篇研究论文表明，LLMs 也是合格的零样本推理者，特别是当用“我们逐步思考”这个短语进行提示时。

是的，你没读错。

> 指示 LLM“逐步思考”实际上能显著改善结果，足以为其撰写论文。

作者 Kojima 等人创建的模型在推理任务上超越了现有的基准，例如算术（如 MultiArith、GSM8K、AQUA-RAT、SVAMP）、符号推理（如 Last Letter、Coin Flip）和逻辑推理（如 Date Understanding、Tracking Shuffled Objects）。

这一单一提示“逐步思考”的适应性表明，零样本技能此前被显著低估了。通过采用要求更高认知负荷的语言框架来提出问题，可能会显著恢复非常高水平的多任务能力。

我的脑袋被震撼了。

**总结** — 这篇论文表明，LLM 回答的质量在很大程度上依赖于提示的措辞。

## 摘要

过去四年间，机器学习取得了显著进展。只有时间才能证明这种发展速度是否能够持续下去。

这些论文讨论了 NLP 的最新改进，揭示了在训练过程中涉及更大数据集和人类反馈强化学习方面还有相当大的改进空间。

最近的研究还探讨了通过对模型输入提示进行简单的调整，来创建多模态范式和增强零样本推理能力。

**[Nicole Janeway Bills](https://www.linkedin.com/in/nicole-janeway-bills/)** 是数据战略专业人员的社区组织者。她在培训数据从业者以快速有效地通过 CDMP 考试方面有着验证的成功记录。在她作为数据战略顾问的工作中，Nicole 帮助建立了数据收集、数据存储和数据分析功能。她运用最佳实践来解决客户最紧迫的挑战。此外，她还曾在联邦和商业咨询团队中担任数据科学家和项目经理。她的业务经验包括自然语言处理、云计算、统计测试、定价分析、ETL 过程以及网页和应用程序开发。

[原文](https://medium.com/towards-data-science/must-read-nlp-papers-f9d38cda0b65)。经许可转载。

### 更多相关话题

+   [KDnuggets 新闻，4 月 27 日：关于代码论文的简要介绍；…](https://www.kdnuggets.com/2022/n17.html)

+   [2023 年必读的顶尖机器学习论文](https://www.kdnuggets.com/2023/03/top-machine-learning-papers-read-2023.html)

+   [你应该阅读的生成代理研究论文](https://www.kdnuggets.com/generative-agent-research-papers-you-should-read)

+   [2024 年必读的 5 篇机器学习论文](https://www.kdnuggets.com/5-machine-learning-papers-to-read-in-2024)

+   [2023 年你必须阅读的 5 本免费数据科学书籍](https://www.kdnuggets.com/2023/01/5-free-data-science-books-must-read-2023.html)

+   [8 篇创新性的 BERT 知识蒸馏论文，它们改变了……](https://www.kdnuggets.com/2022/09/eight-innovative-bert-knowledge-distillation-papers-changed-nlp-landscape.html)
