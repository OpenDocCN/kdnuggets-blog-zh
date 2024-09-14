# 通过验证链解锁可靠生成：提示工程的新飞跃

> 原文：[https://www.kdnuggets.com/unlocking-reliable-generations-through-chain-of-verification](https://www.kdnuggets.com/unlocking-reliable-generations-through-chain-of-verification)

![通过验证链解锁可靠生成：提示工程的新飞跃](../Images/11b3cf522091679f8a691802527fa3de.png)

图片由作者使用Midjourney创建

# 主要收获

+   验证链（CoVe）提示工程方法旨在减轻LLMs中的幻觉问题，解决生成看似正确但实际上不准确的信息

+   通过一个四步过程，CoVe使LLMs能够起草、验证和改进回答，培养一种增强准确性的自我验证机制，结构化自我验证

+   CoVe在各种任务中展示了改进的性能，例如基于列表的问题和长文本生成，展示了其在减少幻觉和增强AI生成文本准确性方面的潜力

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT工作

* * *

> 我们研究了语言模型在回应过程中反思其回答以纠正错误的能力。

# 介绍

在人工智能（AI）领域，对准确性和可靠性的不断追求带来了提示工程中的突破性技术。这些技术在指导生成模型提供精确且有意义的回答方面发挥了关键作用。最近出现的[验证链](https://arxiv.org/abs/2309.11495)（CoVe）方法标志着这一追求的重要里程碑。这一创新技术旨在解决大语言模型（LLMs）中的一个臭名昭著的问题——生成看似正确但实际上不准确的信息，俗称幻觉。通过使模型对其回答进行反思并经历自我验证过程，CoVe在提高生成文本的可靠性方面树立了一个有前途的先例。

大型语言模型（LLMs）蓬勃发展的生态系统，凭借其基于大量文档语料库处理和生成文本的能力，在各种任务中表现出卓越的能力。然而，一个持续存在的问题是——生成虚假信息的倾向，尤其是在较少知晓或罕见的话题上。验证链方法在这些挑战中成为了一线希望，提供了一种结构化的方法来最小化虚假信息，并提高生成响应的准确性。

# 理解验证链

CoVe展开了一个四步骤机制，以减轻LLMs中的虚假信息：

+   草拟初步回应

+   规划验证问题以核查草稿

+   独立回答这些问题以避免偏见

+   基于答案生成最终验证回应

这种系统化的方法不仅解决了虚假信息的问题，还包含了一个自我验证的过程，提升了生成文本的正确性。该方法的有效性已在各种任务中得到验证，包括基于列表的问题、闭卷问答和长篇文本生成，展示了虚假信息的减少和性能的提升。

# 实施验证链

采用CoVe涉及将其四步骤过程融入LLMs的工作流程。例如，当任务是生成历史事件列表时，使用CoVe的LLM会首先草拟一个回应，计划验证问题以事实核查每个事件，独立回答这些问题，最后根据验证结果生成经过验证的列表。

CoVe固有的严格验证过程确保了生成的回答具有更高的准确性和可靠性。这种对验证的严谨态度不仅提升了信息质量，还在AI生成过程内培养了问责文化，为实现更可靠的AI生成文本迈出了重要一步。

**示例 1**

+   **问题**：列出20世纪的著名发明。

+   **初稿**：互联网、量子力学、DNA结构发现

+   **验证问题**：互联网是在20世纪发明的吗？量子力学是在20世纪发展的吗？DNA的结构是在20世纪发现的吗？

+   **最终验证回应**：互联网、青霉素发现、DNA结构发现

**示例 2**

+   **问题**：提供一个非洲国家的列表。

+   **初稿**：尼日利亚、埃塞俄比亚、埃及、南非、苏丹

+   **验证问题**：尼日利亚在非洲吗？埃塞俄比亚在非洲吗？埃及在非洲吗？南非在非洲吗？苏丹在非洲吗？

+   **最终验证回应**：尼日利亚、埃塞俄比亚、埃及、南非、苏丹

采用 CoVe 涉及将其四步骤过程整合到 LLM 的工作流程中。例如，当任务是生成历史事件列表时，使用 CoVe 的 LLM 将首先起草一个回应，计划验证问题以核实每个事件，独立回答这些问题，最后根据收到的验证生成一个经过验证的列表。

![Chain-of-Verification 过程](../Images/c9e5c2de1ce7d2a54e3285c45c0d0aab.png)

**图1**：简化的 Chain-of-Verification 过程（图像来源：作者）

该方法需要上下文示例以及提问 LLM 的问题，或者可以对 LLM 进行 CoVe 示例的微调，以便在这种方式下处理每个问题，如有需要。

# 结论

Chain-of-Verification 方法的出现证明了在提示工程中朝着实现可靠和准确的 AI 生成文本所取得的进展。通过直面幻觉问题，CoVe 提供了一种强有力的解决方案，提升了 LLM 生成信息的质量。这种方法的结构化方法，加上其自我验证机制，代表了在促进更可靠和真实的 AI 生成过程方面的重要跃进。

CoVe 的实施是对从业者和研究人员的号召，继续探索和完善提示工程技术。拥抱这些创新方法将对释放大型语言模型的全部潜力至关重要，预示着一个未来，在这个未来中，AI 生成文本的可靠性不仅是一个愿景，而是一个现实。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特约编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他致力于使数据科学社区的知识民主化。Matthew 从6岁起就开始编程。

### 更多相关主题

+   [确保 LLM 的可靠少样本提示选择](https://www.kdnuggets.com/2023/07/ensuring-reliable-fewshot-prompt-selection-llms.html)

+   [通过密度提示解锁 GPT-4 摘要](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)

+   [自动化思维链：人工智能如何自我提示进行推理](https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html)

+   [揭开 Midjourney 5.2 的面纱：AI 图像生成的跃进](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)

+   [揭示 Meta 的 Llama 2 的力量：生成式 AI 的跃进？](https://www.kdnuggets.com/2023/07/unveiling-power-metas-llama-2-leap-forward-generative-ai.html)

+   [设计有效且可靠的机器学习系统！](https://www.kdnuggets.com/2023/05/manning-design-effective-reliable-machine-learning-systems.html)
