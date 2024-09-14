# 揭开 StableCode 的面纱：AI 辅助编码的新视野

> 原文：[https://www.kdnuggets.com/2023/08/unveiling-stablecode-new-horizon-ai-assisted-coding](https://www.kdnuggets.com/2023/08/unveiling-stablecode-new-horizon-ai-assisted-coding)

![揭开 StableCode 的面纱：AI 辅助编码的新视野](../Images/cc5e7417c80286067e2cebfb992edead.png)

图片由作者使用 Midjourney 创建

# 概述

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

在不断发展的软件开发领域，为提高效率和可访问性而创建了各种工具和平台。其中最新的创新之一是 **[StableCode](https://stability.ai/blog/stablecode-llm-generative-ai-coding)**，这是 Stability AI 推出的一个大型语言模型（LLM）生成式 AI 产品。旨在帮助经验丰富的程序员和有志开发者，StableCode 承诺将彻底改变我们处理编码的方式。

StableCode 是由 Stability AI 开发的 AI 驱动助手，能够智能自动补全，响应指令，并管理长段代码。它包含三个专门的模型，每个模型针对编码过程中的不同方面。StableCode 在来自多种编程语言的超过 5600 亿个标记的广泛数据集上进行训练，旨在提高程序员的生产力并降低入门门槛。

尽管现有的对话 AI 助手如 Llama、ChatGPT 和 Bard 已展示了代码编写能力，但它们并未针对开发者体验进行优化。StableCode 加入了 [GitHub Copilot](https://github.com/copilot) 和其他开源模型等工具，提供了更具针对性和高效的编码体验。本文探讨了 StableCode 的独特功能、底层技术以及它对开发者社区的潜在影响。

# StableCode 详情

StableCode 由三个专门的模型构成：

+   **基础模型：** 在包括 Python、Go、Java、JavaScript、C、markdown 和 C++ 在内的多种编程语言上进行训练。

+   **指令模型：** 调整以处理特定使用案例，帮助解决复杂的编程任务。

+   **长上下文窗口模型：** 构建以一次处理更多代码，允许用户同时查看或编辑多达五个中等大小的 Python 文件。

标准自动补全模型 StableCode-Completion-Alpha-3B-4K 提供单行和多行建议，随着开发者的输入，提升效率和准确性。

指令模型 StableCode-Instruct-Alpha-3B 利用自然语言提示执行编码任务，实现与代码的更直观互动。

StableCode 拥有长达 16,000 个标记的上下文窗口，可以管理广泛的代码库，提供对编码过程的更全面视图和控制。

StableCode 的训练涉及对 [BigCode](https://bigcode-project.github.io/) 数据的大量过滤和清洗。该模型在特定编程语言上经过了连续训练，采用了类似自然语言领域建模的方法。

与其他模型更重视当前标记而非过去标记不同，StableCode 使用旋转位置嵌入（RoPE），确保对代码功能的考虑更为平衡，没有固定的叙述结构。

StableCode 的独特功能和技术承诺显著提升开发者的工作流程。它具有大多数现有模型两倍的上下文长度和经过精心调整的模型，提供了更高的效率和精度。

通过提供一个智能且易于访问的平台，StableCode 有潜力降低新程序员的入门门槛，促进一个更具包容性和多样化的开发者社区。

![揭示 StableCode：AI 辅助编码的新视野](../Images/bffacd44e5765ac81cc8bbea5b152142.png)

与相似规模（3B）的模型进行的 HumanEval 基准测试比较

来源：[Stability AI](https://stability.ai/blog/stablecode-llm-generative-ai-coding)

# 结论

StableCode 代表了编码辅助技术发展的重要一步。它独特的专用模型、智能自动补全和先进技术的结合，使其与现有工具区别开来。通过提供更为定制和高效的编码体验，它成为了软件开发领域的一个革命性工具。

不仅仅是一个编码助手，StableCode 体现了 Stability AI 赋能下一个十亿软件开发者的愿景。通过使技术更易于访问和提供更公平的编码资源，StableCode 准备好帮助塑造软件开发的未来，并激励新一代程序员。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是一名数据科学家及 KDnuggets 的主编，KDnuggets 是重要的数据科学和机器学习在线资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系。

### 更多相关信息

+   [揭示 CTGAN 的潜力：利用生成性 AI 进行…](https://www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html)

+   [揭示 Midjourney 5.2：AI 图像生成的飞跃](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)

+   [揭示 Meta 的 Llama 2 的力量：生成性 AI 的飞跃？](https://www.kdnuggets.com/2023/07/unveiling-power-metas-llama-2-leap-forward-generative-ai.html)

+   [揭示无监督学习](https://www.kdnuggets.com/unveiling-unsupervised-learning)

+   [揭示神经魔法：深入激活函数](https://www.kdnuggets.com/unveiling-neural-magic-a-dive-into-activation-functions)

+   [揭示隐藏模式：层次聚类入门](https://www.kdnuggets.com/unveiling-hidden-patterns-an-introduction-to-hierarchical-clustering)
