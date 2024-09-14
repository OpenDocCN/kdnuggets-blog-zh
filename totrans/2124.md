# 3 种基于研究的高级提示技术，以提高 LLM 的效率和速度优化

> 原文：[https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

![3 种基于研究的高级提示技术，以提高 LLM 的效率和速度优化](../Images/d642c3b80a0d5f0ea0a540b7e50a7889.png)

图片由 [Freepik](https://www.freepik.com/free-vector/hand-drawn-flat-design-npl-illustration_22112068.htm#query=ai%20prompting&position=20&from_view=search&track=ais&uuid=a658aa34-aa02-4ce1-a502-c03d3396f395) 提供

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

像 OpenAI 的 GPT 和 Mistral 的 Mixtral 这样的语言模型（LLM）在 AI 驱动的应用程序开发中扮演着越来越重要的角色。这些模型生成类似人类的结果的能力使它们成为内容创作、代码调试以及其他耗时任务的完美助手。

然而，与 LLM 互动时常见的一个挑战是可能遇到事实不准确的信息，通常被称为幻觉。这些情况的原因并不难以理解。LLM 被训练来提供对提示的满意回答；在不能提供答案的情况下，它们会编造一个。幻觉也可能受到输入类型和训练这些模型时所用偏见的影响。

在本文中，我们将探讨三种基于研究的高级提示技术，这些技术作为减少幻觉发生的有希望的方法，同时提高 LLM 生成结果的效率和速度。

# 提示工程基础

为了更好地理解这些高级技术带来的改进，我们有必要讨论一下提示编写的基础知识。在 AI 的上下文中（在本文中指 LLM），提示指的是一组字符、词语、标记或一组指令，这些都用于指导 AI 模型理解人类用户的意图。

提示工程指的是创建提示的艺术，以更好地指导相关 LLM 的行为和生成的输出。通过使用不同的技术更好地传达人的意图，开发者可以提升模型在准确性、相关性和一致性方面的结果。

下面是一些在编写提示时应遵循的基本技巧：

+   简洁明了

+   通过指定所需的输出格式来提供结构

+   如果可能，请提供参考或示例。

所有这些都将帮助模型更好地理解你的需求，并增加获得令人满意答案的机会。

下面是一个很好的例子，展示了如何使用上述所有提示来查询AI模型：

> 提示 = “你是一个专家AI提示工程师。请生成关于提示生成最新进展的2句总结，重点关注幻觉的挑战以及使用高级提示技术解决这些挑战的潜力。输出应为Markdown格式。”

然而，遵循这些前面讨论的基本技巧并不总能保证最佳结果，特别是当处理复杂任务时。

# 可以在应用中实施的实践研究驱动的高级提示技术

来自微软和谷歌等知名AI机构的领先研究人员已经投入了大量资源进行LLM优化，即积极研究幻觉的常见原因并寻找有效的方法来解决这些问题。以下提示技术已被发现能够为研究中的LLM提供更好的上下文感知指令，从而增加获得更好相关结果的机会，并减少获得不准确或荒谬信息的可能性。

以下是一些研究驱动的高级提示技术示例：

## 1. 情感化说服提示

[微软研究人员2023年的一项研究](https://arxiv.org/abs/2307.11760)发现，使用情感语言和说服性提示，即“EmotionPrompts”，可以将LLM性能提高超过10%。

这种风格为给定的提示添加了个人化的情感元素，将请求转化为一个具有重要意义和显著后果的请求。它几乎像是在与人对话；使用情感角度有助于传达任务的重要性，激发更深的专注和承诺。这一策略对需要更高问题解决和创造力技能的任务非常有用。

我们来看一个简单的例子，其中情感被用来增强提示：

**基本提示：**“编写一个Python脚本来排序一个数字列表。”

![3种研究驱动的高级提示技术用于LLM效率和速度优化](../Images/d485d3086d170a1022bce1a856679829.png)

**情感化的** **说服力：**“我很兴奋能提升我的Python技能，我需要写一个脚本来排序数字。这是我作为开发者职业生涯中的一个关键步骤。”

![3种研究驱动的高级提示技术用于LLM效率和速度优化](../Images/203e3ecccd526968c1f6144eba846efd.png)

虽然两种提示变体产生了类似的代码结果，但“EmotionPrompts”技术帮助创建了更简洁的代码，并在生成的结果中提供了额外的解释。

![3种研究驱动的高级提示技术，用于提高 LLM 效率和速度优化](../Images/a3b5f85dccc129633bf542b407a7be37.png)

[Finxter](https://blog.finxter.com/impact-of-monetary-incentives-on-the-performance-of-gpt-4-turbo-an-experimental-analysis/) 进行的另一项有趣实验发现，向 LLM 提供金钱奖励也可以提高其表现——几乎像是吸引人类的财务激励一样。

## 2\. Chain-of-Thought 提示

另一种由[匹兹堡大学研究小组](https://arxiv.org/abs/2309.08008)发现有效的提示技术是 Chain-of-Thought 风格。这种技术采用逐步的方法，引导模型按照期望的输出结构进行。这种逻辑方法帮助模型为复杂任务或问题制定更相关且结构化的响应。

这是一个如何基于给定模板创建 Chain-of-Thought 风格提示的示例（使用 OpenAI 的 ChatGPT 和 GPT-4）：

**基本提示:** "为面向大城市小型企业主的金融应用制定一个数字营销计划。"

![3种研究驱动的高级提示技术，用于提高 LLM 效率和速度优化](../Images/61811acb47438bc9ffe38cae1246c4b2.png)

**Chain of Thought 提示:**

"为大城市的小型企业主制定一个数字营销策略。重点关注：

1.  选择在该商业人群中受欢迎的数字平台。

1.  创建有吸引力的内容，如网络研讨会或其他相关工具。

1.  生成与传统广告不同的成本效益战术。

1.  将这些战术量身定制以满足城市小型企业的需求，从而提高客户转化率。

使用独特且可操作的步骤命名和详细说明计划的每个部分。

Chain-of-Thought 提示技术从初步观察中生成了更精确和可操作的结果。

![3种研究驱动的高级提示技术，用于提高 LLM 效率和速度优化](../Images/688617d5caaa8e2c7b0810c3ed4b3ad2.png)

# 3\. Step-Back 提示

由七名 Google 的[Deepmind 研究人员](https://arxiv.org/abs/2310.06117)提出的 Step-Back-Prompting 技术旨在模拟处理 LLM 时的推理。这类似于在解决复杂问题之前教给学生概念的基本原理。

要应用这种技术，你需要在请求模型提供答案之前指出问题背后的基本原理。这确保了模型能够获得充分的背景，这将帮助它提供技术上正确且相关的答案。

我们来看看两个示例（使用 OpenAI 的 ChatGPT 和 GPT-4）：

**示例 1:**

****基本提示**: "疫苗是如何工作的？"

![3种研究驱动的高级提示技术，用于提高 LLM 效率和速度优化](../Images/790b4b0a7b05eb184c11093ae558be67.png)

**使用 Step-Back 技术的提示**

1.  “哪些生物机制使疫苗能够防御疾病？”![3个基于研究的高级提示技术，提高LLM效率和速度优化](../Images/9dd8265721e63f404fd4313d0a410f1b.png)

1.  “你能解释一下疫苗引发的身体免疫反应吗？”![3个基于研究的高级提示技术，提高LLM效率和速度优化](../Images/a3b5f85dccc129633bf542b407a7be37.png)

虽然基本提示提供了令人满意的答案，但使用“后退一步”技巧提供了更深入、更技术性的回答。这对于你可能遇到的技术问题尤其有用。

![3个基于研究的高级提示技术，提高LLM效率和速度优化](../Images/85428c9a7c933587587dbe8b3c82e1cb.png)

# 结论

随着开发者继续为现有的AI模型构建新颖的应用，对先进提示技术的需求也在增加，这些技术可以提升大型语言模型的能力，不仅理解我们的言语，还能理解其中的意图和情感，从而生成更准确和上下文相关的输出。

**[Mahmud Adeleye](https://www.linkedin.com/in/mahmudadeleye/)**是一位拥有丰富经验的软件工程师，专注于设计AI驱动的软件应用。他还为JavaScript开发者提供了一个开源的AI课程： https://github.com/thestriver/ai-for-javascript-course。

### 更多关于这个话题

+   [5个提升数据效率和速度的Python技巧](https://www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed)

+   [提升LLM推理能力：揭示代码链提示](https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting)

+   [SQL查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [LLM手册：从业者的策略和技术](https://www.kdnuggets.com/llm-handbook-strategies-and-techniques-for-practitioners)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [10个高级Git技巧](https://www.kdnuggets.com/10-advanced-git-techniques)
