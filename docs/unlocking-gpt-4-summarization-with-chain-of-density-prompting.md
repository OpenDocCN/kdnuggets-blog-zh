# 解锁 GPT-4 摘要能力与 Chain of Density 提示

> 原文：[`www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting`](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)

![解锁 GPT-4 摘要能力与 Chain of Density 提示](img/e86e1d3b41641bee015099baa73ac43a.png)

图像由作者使用 Midjourney 创建

# 关键要点

+   Chain of Density（CoD）是一种新颖的提示工程技术，旨在优化像 GPT-4 这样的**大型语言模型**的摘要任务。

+   该技术处理生成摘要中的信息密度，提供一个既不稀疏也不密集的平衡输出。

+   CoD 对数据科学有实际意义，尤其是在需要高质量、上下文适当的摘要任务中。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

> 选择摘要中包含的“正确”信息量是一项困难的任务。

# 介绍

提示工程是推动生成式 AI 效能提升的动力。虽然现有的提示技术如 [Chain-of-Thought](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html) 和 [Skeleton-of-Thought](https://www.kdnuggets.com/parallel-processing-in-prompt-engineering-the-skeleton-of-thought-technique) 关注结构化和高效的输出，但一种名为 Chain of Density（CoD）的新技术旨在优化文本摘要的质量。这项技术解决了选择“正确”信息量以确保摘要既不过于稀疏也不过于密集的挑战。

# 了解信息密度链

Chain of Density（信息密度链）旨在提高像 GPT-4 这样的**大型语言模型**的摘要能力。它专注于控制生成摘要中的信息密度。一个平衡良好的摘要通常是理解复杂内容的关键，而 CoD 旨在达到这种平衡。它使用特殊的提示来引导 AI 模型包含必要的要点，同时避免不必要的细节。

![CoD 过程示意图](https://www.kdnuggets.com/wp-content/uploads/chain-of-density-example-from-paper.png)

**图 1**：使用示例的密度链过程（来自 [Sparse to Dense: GPT-4 Summarization with Chain of Density Prompting](https://arxiv.org/abs/2309.04269)）（点击放大）

# 实施密度链

实施 CoD 涉及使用一系列链式提示来引导模型生成总结。这些提示旨在控制模型的焦点，引导其关注重要信息，避免无关细节。例如，你可以从一个概括性的总结提示开始，然后跟进具体提示以调整生成文本的密度。

## 密度链提示过程步骤

1.  确定待总结的文本：选择你希望总结的文档、文章或任何文本。

1.  制作初始提示：创建一个针对选定文本的初始总结提示。目标是引导大型语言模型（LLM），如 GPT-4，生成一个基本总结。

1.  分析初始总结：审查从初始提示生成的总结。确定总结是否过于稀疏（缺少关键细节）或过于密集（包含不必要的细节）。

1.  设计链式提示：根据初始总结的密度，构建额外的提示来调整总结的详细程度。这些就是“链式提示”，是密度链技术的核心。

1.  执行链式提示：将这些链式提示反馈给 LLM。这些提示旨在通过添加必要细节来增加密度，或通过删除不必要的信息来降低密度。

1.  审查调整后的总结：检查通过执行链式提示生成的新总结。确保它捕捉到所有关键点，同时避免不必要的细节。

1.  如有必要，进行迭代：如果总结仍未达到所需的信息密度标准，请返回第 4 步，调整链式提示。

1.  完成总结：一旦总结达到所需的信息密度水平，即视为完成并准备使用。

## 密度链提示

以下 CoD 提示直接取自论文。

> 文章：{{ ARTICLE }}
> 
> 你将生成越来越简洁、实体密集的上述文章总结。
> 
> 重复以下 2 个步骤 5 次。
> 
> 第 1 步。识别文章中缺失的 1-3 个信息实体（用“;”分隔）。
> 
> 第 2 步。写一个新的、更紧凑的总结，长度相同，覆盖前一个总结中的每个实体和细节，以及遗漏的实体。
> 
> 遗漏的实体是：
> 
> - 相关的：与主要故事相关。
> 
> - 具体的：描述性但简洁（5 个词或更少）。
> 
> - 新颖的：在前一个总结中没有。
> 
> - 忠实的：在文章中存在。
> 
> - 任何地方：位于文章的任何位置。
> 
> 指导原则：
> 
> - 第一个总结应长（4-5 句，约 80 个单词）且高度非特定，包含除了缺失实体之外的信息很少。使用过于冗长的语言和填充词（例如，“本文讨论了”）以达到约 80 个单词。
> 
> - 让每一个词都发挥作用：重写之前的总结以改进流畅性并为附加实体腾出空间。
> 
> - 通过融合、压缩和移除诸如“本文讨论了”等无信息性短语来腾出空间。
> 
> - 总结应变得高度密集且简洁，但内容自成一体，例如，*即使没有文章也能轻松理解*。
> 
> - 遗漏的实体可以出现在新的总结中的任何位置。
> 
> - 永远不要从先前的总结中删除实体。如果没有空间，可以减少新增实体的数量。
> 
> 记住，每个总结的字数要完全相同。
> 
> 以 JSON 格式回答。JSON 应该是一个长度为 5 的字典列表，字典的键为“Missing_Entities”和“Denser_Summary”。

信息密度链并不是一刀切的解决方案。它需要精心编排的链式提示，以适应任务的具体需求。然而，当正确实施时，它可以显著提高 AI 生成总结的质量和相关性。

# 结论

信息密度链（Chain of Density）为提示工程提供了新的途径，特别是旨在改善总结任务。它对信息密度的控制使其成为生成高质量总结的宝贵工具。通过将 CoD 融入你的项目中，你可以利用下一代语言模型的高级总结能力。

[**Matthew Mayo**](https://www.linkedin.com/in/mattmayo13/) ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 KDnuggets 的主编，Matthew 旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、机器学习算法和探索新兴的 AI。他的使命是让数据科学社区的知识变得更加普及。Matthew 从 6 岁起就开始编程。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的执行主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特约编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法和探索新兴的 AI。他的使命是让数据科学社区的知识变得更加普及。Matthew 从 6 岁起就开始编程。

### 更多相关内容

+   [揭示链式思维提示在大型语言模型中的威力](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html)

+   [增强 LLM 推理能力：揭示代码链提示](https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting)

+   [通过链式验证解锁可靠生成：一个…](https://www.kdnuggets.com/unlocking-reliable-generations-through-chain-of-verification)

+   [文本摘要方法概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [自动化文本摘要入门](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [使用 GPT-3 进行摘要](https://www.kdnuggets.com/2022/04/packt-summarization-gpt3.html)
