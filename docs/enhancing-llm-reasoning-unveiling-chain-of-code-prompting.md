# 提升 LLM 推理：揭示代码链提示

> 原文：[https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting](https://www.kdnuggets.com/enhancing-llm-reasoning-unveiling-chain-of-code-prompting)

![提升 LLM 推理：揭示代码链提示](../Images/d823c899c7e1971459756d0f14197abe.png)

图片由作者使用 DALL•E 3 创建

## 关键要点

+   代码链（CoC）是一种与语言模型互动的新方法，通过代码编写和选择性代码模拟来增强推理能力。

+   CoC 扩展了语言模型在逻辑、算术和语言任务中的能力，特别是在需要这些技能结合的任务中。

+   使用 CoC，语言模型可以编写代码并模拟不能编译的部分，提供了一种独特的解决复杂问题的方法。

+   CoC 在大型和小型 LLM 中都显示出有效性。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

> 关键思想是鼓励 LM 将语言子任务格式化为灵活的伪代码，以便编译器可以明确捕获未定义行为，并交给 LM（作为 'LMulator'）进行模拟。

# 引言

新的语言模型（LM）提示、通信和训练技术不断涌现，以增强 LM 的推理和性能能力。其中之一是[代码链（CoC）](https://arxiv.org/abs/2312.04474)的开发，这是一种旨在推进 LM 代码驱动推理的方法。这种技术融合了传统编码和创新的 LM 代码执行模拟，成为应对复杂语言和算术推理任务的强大工具。

CoC 的特点在于其处理融合逻辑、算术和语言处理的复杂问题的能力，这长期以来一直是标准 LLM 用户所面临的挑战。CoC 的有效性不仅限于大型模型，还扩展到各种规模，展示了其在 AI 推理中的多样性和广泛适用性。

![提升 LLM 推理：揭示代码链提示](../Images/32bcfb05aca53e28692305b6b6e913b8.png)

**图 1**：代码链方法和过程比较（图来自论文）

# 理解代码链

CoC是语言模型功能的范式转变；这不仅仅是一个简单的提示策略，以增加从语言模型中引出期望响应的机会。相反，CoC重新定义了语言模型对上述推理任务的方法。

从本质上讲，CoC使语言模型不仅能够编写代码，还能够仿真部分代码，尤其是那些不可直接执行的部分。这种双重性使语言模型能够处理更广泛的任务，将语言细微差别与逻辑和算术问题解决结合起来。CoC能够将语言任务格式化为伪代码，并有效地弥合传统编码与人工智能推理之间的差距。这种桥接使得系统在复杂问题解决方面更加灵活和高效。LMulator作为CoC功能增强的主要组成部分，能够模拟和解释代码执行输出，这些输出否则无法直接提供给语言模型。

CoC在不同基准测试中表现出色，显著超越了现有的方法，如思维链，特别是在需要语言和计算推理相结合的场景中。

> 实验表明，代码链在各种基准测试中优于思维链和其他基线；在BIG-Bench Hard上，代码链达到了84%，比思维链提高了12%。

![提升LLM推理能力：揭示代码链提示](../Images/882f6a27df262eaef84a64624292c6e3.png)

**图2**：代码链性能比较（图像来源于论文）

# 实现代码链

CoC的实现涉及一种独特的推理任务方法，整合了编码和仿真过程。CoC鼓励语言模型将复杂的推理任务格式化为伪代码，然后进行解释和解决。该过程包括多个步骤：

1.  确定推理任务：确定需要推理的语言或算术任务

1.  代码编写：语言模型编写伪代码或灵活的代码片段来概述解决方案。

1.  代码仿真：对于那些不可直接执行的代码部分，语言模型仿真预期的结果，有效地模拟代码执行

1.  结合输出：语言模型结合实际代码执行和其仿真结果，形成问题的全面解决方案

这些步骤使语言模型能够通过“用代码思考”来处理更广泛的推理问题，从而增强了它们的解决问题能力。

作为CoC框架的一部分，LMulator可以在几个具体方面显著帮助改进代码和推理：

+   错误识别和仿真：当语言模型编写包含错误或不可执行部分的代码时，LMulator可以模拟该代码在运行时的行为，揭示逻辑错误、无限循环或边缘情况，并指导语言模型重新思考和调整代码逻辑。

+   处理未定义行为：在代码涉及标准解释器无法执行的未定义或模糊行为的情况下，LMulator 利用语言模型对上下文和意图的理解来推断输出或行为应该是什么，在传统执行失败的地方提供有理由的模拟输出。

+   提高代码推理能力：当需要语言和计算推理的混合时，LMulator 允许语言模型迭代其自身的代码生成，模拟各种方法的结果，实际上是在代码中进行‘推理’，从而得出更准确和高效的解决方案。

+   边缘案例探索：LMulator 可以通过模拟不同的输入来探索和测试代码如何处理边缘案例，这在确保代码的健壮性和能够处理各种场景方面特别有用。

+   学习反馈循环：当 LMulator 模拟和识别代码中的问题或潜在改进时，这些反馈可以被语言模型用来学习和完善其编码和解决问题的方法，这是一个持续的学习过程，随着时间的推移提高模型的编码和推理能力。

LMulator 通过提供一个模拟和迭代改进的平台，增强了语言模型编写、测试和完善代码的能力。

# 结论

CoC 技术是提升语言模型推理能力的进步。CoC 通过将代码编写与选择性代码仿真相结合，拓宽了语言模型可以解决的问题范围。这一方法展示了 AI 处理需要细致思考的更复杂、现实世界任务的潜力。值得注意的是，CoC 在小型和大型语言模型中均表现优异，为越来越多的小型模型提供了可能提升其推理能力并将其效果接近大型模型的途径。

要深入了解，请[参考完整论文](https://arxiv.org/abs/2312.04474)。

[](https://www.linkedin.com/in/mattmayo13/)****[马修·梅奥](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/)和[Statology](https://www.statology.org/)的主编，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的特约编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他的使命是普及数据科学领域的知识。Matthew 从6岁开始编程。

### 相关主题

+   [揭示大语言模型中链式思维提示的力量](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html)

+   [使用密度提示链解锁 GPT-4 总结](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)

+   [Orca LLM：模拟 ChatGPT 推理过程](https://www.kdnuggets.com/2023/06/orca-llm-reasoning-processes-chatgpt.html)

+   [3 个基于研究的 LLM 高级提示技术，提升效率…](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [揭示 CTGAN 的潜力：利用生成 AI 进行…](https://www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html)

+   [揭示无监督学习](https://www.kdnuggets.com/unveiling-unsupervised-learning)
