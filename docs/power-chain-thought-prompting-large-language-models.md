# 揭示链式思维提示在大型语言模型中的力量

> 原文：[https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html)

![揭示链式思维提示在大型语言模型中的力量](../Images/786c67d6492286a8cfa0ded2b2775d7e.png)

作者通过 Midjourney 创建的图像

# 介绍链式思维提示

大型语言模型（LLMs）已经彻底改变了人工智能领域，提供了前所未有的自然语言理解和生成能力。然而，它们执行复杂推理任务的能力一直是密切研究的主题。一种在这方面表现出希望的技术是链式思维（CoT）提示。本文探讨了 CoT 提示的复杂性及其对 LLMs 未来的影响。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 工作

* * *

CoT 提示，如在[最近的论文](https://arxiv.org/pdf/2201.11903.pdf)中介绍的，是一种鼓励 LLM 解释其推理过程的方法。这是通过向模型提供几个示例来实现的，其中推理过程被明确地概述。然后，LLM 预计会在回答提示时遵循类似的推理过程。这种技术已被发现显著提高模型在需要复杂推理任务上的表现。

CoT 提示的一个主要优势是其提升 LLM 在需要算术、常识和符号推理任务中的表现的能力。该技术已被证明可以提高性能，特别是对于大约 1000 亿参数的模型。然而，较小的模型被发现会产生不合逻辑的思维链，导致比标准提示方法更低的准确性。

# 理解链式思维提示

从本质上讲，CoT 提示是关于引导 LLM 按步骤思考。这是通过向模型提供几个示例来实现的，这些示例概述了推理过程。然后，模型在回答提示时预计会遵循类似的思维链。这种方法对于需要一系列推理步骤才能生成响应的复杂任务特别有效。

这里是一个 CoT 提示示例（取自 [这篇文章](https://machinelearningmastery.com/prompt-engineering-for-effective-interaction-with-chatgpt/) 机器学习大师上的文章），使用了少量示例策略：

> 问：乔有 20 个鸡蛋。他又买了 2 箱鸡蛋。每箱包含 12 个鸡蛋。乔现在有多少个鸡蛋？
> 
> 答：乔开始时有 20 个鸡蛋。2 箱 12 个鸡蛋就是 24 个鸡蛋。20 + 24 = 44。因此，乔现在有 44 个鸡蛋，答案是 44。
> 
> 问：农民约翰有 93 只鸡。如果他卖给农民比尔 20 只鸡，并买回两倍于卖出的数量，他现在有多少只鸡？

ChatGPT 的回应：

> 答：农民约翰卖给农民比尔 20 只鸡，因此他剩下了 93 – 20 = 73 只鸡。他然后购买了卖出的两倍数量的鸡，因此他买了 2 * 20 = 40 只鸡。将这些新买的鸡加到剩余的鸡上，农民约翰现在有 73 + 40 = 113 只鸡。

CoT 提示还可以在零-shot 设置中使用。这涉及在原始提示中添加类似“我们一步一步来思考”的短语，这也可以与少量示例提示一起使用。这种简单的添加已被发现能有效提高模型在没有很多示例的任务上的性能。

# 自动化思维链提示

尽管 CoT 提示可能有效，但它通常需要手动制作示例，这可能会耗费时间并导致次优解。为了解决这个问题，[研究人员提出了](https://arxiv.org/abs/2210.03493) 一种被称为自动化思维链（Auto-CoT）的方法。这种方法利用 LLM 自动生成演示的推理链，从而消除了手动工作的需要。

Auto-CoT 包含两个主要阶段：问题聚类和演示抽样。在问题聚类阶段，将给定数据集中的问题划分为几个簇。在演示抽样阶段，从每个簇中选择一个代表性问题，并使用简单启发式的 Zero-Shot-CoT 生成其推理链。这个过程鼓励模型使用简单且准确的演示。

Auto-CoT 过程：

1.  问题聚类：将给定数据集中的问题划分为几个簇

1.  演示抽样：从每个簇中选择一个代表性问题，并使用简单启发式的 Zero-Shot-CoT 生成其推理链

# 限制与未来方向

尽管 CoT 提示显示出前景，但它并非没有局限性。首先，它仅在使用约 1000 亿参数的模型时才能获得性能提升。较小的模型往往会产生不合理的思维链，导致比标准提示更低的准确性。此外，CoT 提示的性能提升通常与模型的大小成正比。

尽管存在这些限制，CoT 提示在提升 LLM 推理能力的探索中代表了一个重要的进步。未来的研究可能会集中在改进这一技术，并探索使其在更广泛的任务和模型规模中更为有效的方法。

# 结论

思维链提示代表了人工智能领域的重要进展，特别是在提升大语言模型的推理能力方面。通过鼓励这些模型解释其推理过程，CoT 提示在改进需要算术、常识和符号推理的复杂任务的表现上显示出了潜力。尽管该技术有其局限性，但它为 LLM 的未来打开了令人兴奋的可能性。

随着我们不断突破 LLM 的能力界限，像 CoT 提示这样的技术将发挥关键作用。通过使这些模型逐步思考并解释其推理过程，我们不仅可以提高它们在复杂任务中的表现，还可以获得对其内部机制的宝贵洞察。虽然完全推理的 LLM 之路仍然漫长，但有了像 CoT 提示这样的技术，我们无疑走在了正确的道路上。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家和 KDnuggets 的总编辑，KDnuggets 是著名的在线数据科学和机器学习资源。他的兴趣领域包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系到。

### 更多相关主题

+   [思维传播：一种复杂推理的类比方法…](https://www.kdnuggets.com/thought-propagation-an-analogical-approach-to-complex-reasoning-with-large-language-models)

+   [探索思维树提示：AI 如何学习推理…](https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html)

+   [增强 LLM 推理：揭示代码链提示的奥秘](https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting)

+   [通过密度链提示解锁 GPT-4 总结](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)

+   [自动化思维链：AI 如何促使自己进行推理](https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html)

+   [深入探索：解开 CNN 池化层的魔力](https://www.kdnuggets.com/diving-into-the-pool-unraveling-the-magic-of-cnn-pooling-layers)
