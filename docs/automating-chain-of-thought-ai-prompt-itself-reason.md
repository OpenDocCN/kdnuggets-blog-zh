# 自动化思维链：AI 如何自我提示进行推理

> 原文：[https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html](https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html)

![自动化思维链：AI 如何自我提示进行推理](../Images/7aba366ad9c38f56a4801543271f5602.png)

图片由作者使用 Midjourney 创建

# 重点

+   思维链（CoT）提示通过提供逐步示例来提升 LLM 的推理能力

+   CoT 演示的人工创建需要相当的人工努力

+   本文探讨了使用 LLM 自身自动生成 CoT 演示的方法

+   提出的 Auto-CoT 方法先对问题进行聚类，然后从中抽取多样化的问题进行自我提示

+   实验表明，Auto-CoT 的表现与人工创建的 CoT 相匹配，无需人工参与

# 介绍

论文《[**大型语言模型中的自动化思维链提示**](https://arxiv.org/abs/2210.03493)》探讨了为大型语言模型（LLMs）如 GPT-4 创建有效的“思维链”（CoT）提示的自动化方法。CoT 提示涉及向 LLM 展示示例，这些示例展示了从问题到最终答案的逐步推理链。这可以提升在复杂推理任务中的表现。

# 讨论

然而，目前最好的 CoT 提示结果仍然需要人工手动创建演示，包括为每个任务量身定制的精心设计的问题和详细的推理步骤。作者提出通过让 LLM 自动生成自己的 CoT 演示来消除这种人工努力。他们的关键方法称为 Auto-CoT，其工作原理是首先根据语义相似性对给定任务的问题进行聚类。Auto-CoT 然后从不同的聚类中抽取一组多样化的问题。对于每个抽样的问题，Auto-CoT 使用 LLM 自身以零样本模式生成从问题到答案的推理链。它应用简单的启发式方法，根据长度和简单性选择推理链。

作者在涵盖算术、常识和符号逻辑问题的 10 个推理数据集上评估了 Auto-CoT。结果表明，Auto-CoT 的表现与基于人工创建演示的 CoT 提示相匹配或超出，不需要任何人工设计演示。一个关键见解是，使用基于多样性的抽样而不是基于相似性的检索来选择提示问题，可以减轻由 LLM 的零样本推理生成的不完善演示的影响。Auto-CoT 在性能上也显著超越了类似问题检索或随机抽样等基线方法。

总体而言，这项工作提供了有力的证据，表明LLM可以自我提示以展示复杂的多步骤推理。Auto-CoT本质上是一个LLM生成多样的CoT示例，另一个LLM使用这些示例进行推理。作者认为，这种自我提示的方法可能显著扩展提示技术，并使LLM在复杂推理任务中的少样本学习能力大大提高。局限性包括潜在的计算成本和在更多无约束问题上的扩展问题。但自动化提示的能力减少了人力和定制需求。

# 研究问答

*Auto-CoT与其他自动化提示创建的方法（如检索增强提示）相比如何？*

检索增强提示通过检索相关数据示例来进行提示，而不是让LLM生成演示。一个关键的区别是，Auto-CoT不需要标记示例的数据集，而是依赖LLM自身的零-shot推理。检索可能更具样本效率，但需要数据收集。Auto-CoT完全自动化，但可能会受到演示不完美的影响。

*Auto-CoT能否应用于逻辑推理之外的自然语言生成任务？*

聚类和自我提示的方法在那些结构不太规范但连贯性重要的文本任务中显得很有前景。例如，Auto-CoT可以为创造性写作提供写作规划示例，或为对话机器人提供对话示例。主要挑战在于定义合适的聚类方法以及训练LLM的零-shot生成以提供高质量的演示。

*这项研究的创新之处是什么？*

关键的创新在于使用LLM本身生成演示以进行提示，而不是依赖手动创建。这使得提示变得更加自动化和任务自适应。选择多样化问题以进行自我提示的聚类方法也是一种创新。

*这项研究的广泛意义是什么？*

这项研究可能显著减少设计有效提示所需的人力和专业知识。它可能使LLM能够更快地学习新任务，并从更少的数据中获得知识，增强其少样本学习能力。自我提示的方法可以应用于扩展类似上下文学习的提示技术。

*这项研究中是否存在潜在问题或遗漏？*

一个潜在问题是Auto-CoT依赖于基于Sentence-BERT的相似性特征进行问题聚类。在语义相似性与推理相似性不匹配的任务上，性能可能会受到影响。这种方法可能还会比标准提示方法产生更高的计算成本。

*这项研究的逻辑下一步是什么？*

重要的下一步包括探索 Auto-CoT 如何扩展到更复杂和开放的推理任务，将其与外部知识源的检索集成，并研究该方法是否可以通过元学习以更高效的样本学习，而不是仅依赖于预训练的大型语言模型。分析集群数量、样本大小和性能之间的相互关系也是一个待解问题。

# 收获

+   Auto-CoT 减少了对手工制作示例来提示语言模型的需求。

+   使用 Auto-CoT 的自我提示组合一个生成多样示例的语言模型，另一个进行推理。

+   样本问题的多样性是克服不完美零样本推理链的关键。

+   这种方法可以扩展提示技术，并使语言模型成为更好的少量样本学习者。

+   Auto-CoT 展示了自动化提示以减少人工努力的潜力。

+   下一步包括将 Auto-CoT 扩展到更复杂的推理任务和更大的语言模型。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家和 KDnuggets 的主编，这是一个开创性的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。可以通过 editor1 at kdnuggets[dot]com 联系他。

### 相关阅读

+   [探索思维树提示：AI 如何通过搜索学习推理……](https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html)

+   [揭示链式思维提示在大型语言模型中的力量](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html)

+   [提示工程中的并行处理：思维骨架法……](https://www.kdnuggets.com/parallel-processing-in-prompt-engineering-the-skeleton-of-thought-technique)

+   [通过验证链解锁可靠生成：一……](https://www.kdnuggets.com/unlocking-reliable-generations-through-chain-of-verification)

+   [思维传播：一种类比方法解决复杂推理……](https://www.kdnuggets.com/thought-propagation-an-analogical-approach-to-complex-reasoning-with-large-language-models)

+   [构建供应链管道所需的6种数据科学技术](https://www.kdnuggets.com/2022/01/6-data-science-technologies-need-build-supply-chain-pipeline.html)
