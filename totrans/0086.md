# 生成代理研究论文阅读推荐

> 原文：[https://www.kdnuggets.com/generative-agent-research-papers-you-should-read](https://www.kdnuggets.com/generative-agent-research-papers-you-should-read)

![生成代理研究论文阅读推荐](../Images/de055e807dc0ff09c73c804fcdd73ab0.png)

图片来源于 [pikisuperstar](https://www.freepik.com/free-vector/hand-drawn-bodyguard-illustration_40126467.htm#query=generative%20agents&position=0&from_view=search&track=ais) 在 [Freepik](https://www.freepik.com/)

生成代理是由斯坦福大学和谷歌研究人员在他们的论文[生成代理：人类行为的互动模拟](https://arxiv.org/pdf/2304.03442v2.pdf)（Park *et al*., 2023）中创造的术语。在这篇论文中，研究解释了生成代理是计算软件，能够可信地模拟人类行为。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

在这篇论文中，他们介绍了代理如何通过实施生成模型，尤其是大型语言模型（LLM），像人类一样进行写作、烹饪、演讲、投票、睡觉等行为。代理能够利用自然语言模型展示对自身、其他代理和环境的推断能力。

研究人员构建了一个系统架构，用于存储、综合和应用相关记忆，通过使用大型语言模型来生成可信的行为，从而实现生成代理。该系统由三个组件组成，它们是：

1.  **内存流**。系统记录代理的经历，并作为代理未来行动的参考。

1.  **反思**。系统将经历综合为记忆，以便代理可以学习和表现得更好。

1.  **计划**。系统将前一系统的洞察转化为高层次的行动计划，并使代理能够对环境做出反应。

这些反思和计划系统与内存流协同工作，影响代理的未来行为。

为了模拟上述系统，研究人员专注于创建一个受《模拟人生》游戏启发的互动代理社会。上述架构与ChatGPT连接，并成功展示了其沙盒内的25个代理互动。以下图像展示了一整天代理活动的一个示例。

![生成代理研究论文阅读推荐](../Images/e2e7316fdb15cc94f0b28c8979747c93.png)

生成代理的日常活动和互动（Park *et al*., 2023）

用于创建生成代理并在沙箱中模拟它们的完整代码已经由研究人员开源，你可以在以下[仓库](https://github.com/joonspk-research/generative_agents)中找到。说明非常简单，你可以顺利跟随。

随着生成代理成为一个令人兴奋的领域，基于这一点的研究也在不断进行。在本文中，我们将探讨一些你应该阅读的生成代理相关论文。这些论文有哪些呢？让我们深入了解一下。

## 1\. 软件开发的交流代理

[软件开发的交流代理论文](https://arxiv.org/pdf/2307.07924v3.pdf)（Quan *et al.*, 2023）是一种通过生成代理来革命性改进软件开发的新方法。研究人员提出的前提是如何利用大型语言模型（LLM）的自然语言沟通来简化和统一整个软件开发过程。任务包括开发代码、生成文档、分析需求等。

研究人员指出，使用LLM生成整个软件面临两个主要挑战：幻觉和决策过程中缺乏交叉检查。为了解决这些问题，研究人员提出了一种基于聊天的软件开发框架，称为ChatDev。

ChatDev框架分为四个阶段：设计、编码、测试和文档编写。在每个阶段中，ChatDev会建立几个具有不同角色的代理，例如代码审阅员、软件程序员等。为了确保代理之间的通信顺畅，研究人员开发了一个聊天链，将各个阶段划分为顺序原子子任务。每个子任务都实现了代理之间的协作和互动。

ChatDev框架如下图所示。

![你应该阅读的生成代理研究论文](../Images/0e301ea74a7108fbeff5e0bdd1900a09.png)

提出的ChatDev框架（Quan *et al.*, 2023）

研究人员进行了各种实验来测量ChatDev框架在软件开发中的表现。通过使用*gpt3.5-turbo-16k*，以下是软件统计实验的表现。

![你应该阅读的生成代理研究论文](../Images/8388482d3072479460aece7a7df5572b.png)

ChatDev框架软件统计数据（Quan *et al.*, 2023）

上述数字是关于ChatDev生成的软件系统的统计分析指标。例如，生成的代码行数最少为39行，最多为359行。研究人员还展示了生成的软件系统中有86.66%正常工作。

这是一篇很棒的论文，展示了改变开发者工作方式的潜力。进一步阅读论文以了解ChatDev的完整实现。完整代码也可以在ChatDev的[仓库](https://github.com/openbmb/chatdev)中找到。

## 2\. AgentVerse：促进多代理协作并探索代理中的涌现行为

AgentVerse 是 [*Chen et al*., 2023](https://arxiv.org/pdf/2308.10848.pdf) 论文中提出的一个框架，用于通过大型语言模型模拟代理组，以便在组内进行动态问题解决程序，并根据进展调整组成员。这项研究旨在解决静态组动态的问题，即自主代理在解决问题时无法适应和演变。

AgentVerse 框架试图将框架拆分为四个步骤，包括：

1.  专家招聘：代理调整阶段以与问题和解决方案对齐

1.  协作决策：代理讨论以制定解决问题的方案和策略。

1.  行动执行：代理根据决策在环境中执行行动。

1.  评估：当前条件和目标被评估。如果目标仍未达到，反馈奖励将返回到第一步。

AgentVerse 的整体结构如下面的图像所示。

![你应该阅读的生成代理研究论文](../Images/8252848c049dc5a46f17db9e712b9cc7.png)

AgentVerse 框架（Chen *et al.*, 2023）

研究人员对该框架进行了实验，并将 AgentVerse 框架与单一代理解决方案进行了比较。结果如下面的图像所示。

![你应该阅读的生成代理研究论文](../Images/08e558f4e54d4c6f76389deff904a893.png)

AgentVerse 性能分析（Chen *et al.*, 2023）

AgentVerse 框架在所有展示的任务中通常优于单个代理。这证明了生成代理在解决问题时可能比单个代理表现更好。你可以通过他们的 [repository](https://github.com/openbmb/agentverse) 试用该框架。

## 3\. AgentSims：一个用于大型语言模型评估的开源沙箱

评估 LLM 的能力仍然是社区和领域中的一个悬而未决的问题。限制评估 LLM 能力的三个因素是任务的评估能力有限、脆弱的基准和不客观的指标。为了应对这些问题，[Lin *et al.*, 2023](https://arxiv.org/pdf/2308.04026.pdf) 在他们的论文中提出了一种基于任务的评估作为 LLM 基准的方法。这种方法希望成为评估 LLM 工作的标准，因为它可以缓解提出的所有问题。为了实现这一点，研究人员引入了一个名为 AgentSims 的框架。

AgentSims 是一个具有互动和可视化基础设施的程序，用于策划 LLM 的评估任务。AgentSims 的总体目标是为研究人员和专家提供一个平台，以简化任务设计过程，并将其作为评估工具。AgentSims 的前端如下面的图像所示。

![你应该阅读的生成代理研究论文](../Images/99db7c0d1d441547bc7f5df1a0496889.png)

AgentSims前端（林*等人*., 2023）

由于AgentSims的目标是为需要更简单LLM评估的方法的每个人提供服务，研究人员开发了可以与用户界面交互的前端。你还可以在他们的[网站](https://agentsims.com/)上尝试完整演示，或在AgentSims的[代码库](https://github.com/py499372727/AgentSims/)中访问完整代码。

# 结论

生成智能体是LLMs中模拟人类行为的一种新方法。最新的研究由Park *等人*., 2023进行，展示了生成智能体的巨大潜力。这就是为什么许多基于生成智能体的研究不断涌现，并开启了许多新领域。

在这篇文章中，我们讨论了三种不同的生成智能体研究，包括：

1.  软件开发中的交互智能体论文（[全*等人*., 2023](https://arxiv.org/pdf/2307.07924v3.pdf)）

1.  AgentVerse: 促进多智能体协作并探索智能体中的涌现行为（[*陈等人*., 2023](https://arxiv.org/pdf/2308.10848.pdf)）

3. AgentSims: 一个开源的大型语言模型评估沙盒（[林*等人*., 2023](https://arxiv.org/pdf/2308.04026.pdf)）

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。虽然全职在印尼安联工作，他还是喜欢通过社交媒体和写作媒体分享Python和数据技巧。

### 更多相关话题

+   [理解AI中的智能体环境](https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html)

+   [KDnuggets新闻，4月27日：关于带代码的论文简要介绍；…](https://www.kdnuggets.com/2022/n17.html)

+   [过去12个月必读的自然语言处理论文](https://www.kdnuggets.com/2023/03/must-read-nlp-papers-last-12-months.html)

+   [2023年需阅读的顶级机器学习论文](https://www.kdnuggets.com/2023/03/top-machine-learning-papers-read-2023.html)

+   [2024年需阅读的5篇机器学习论文](https://www.kdnuggets.com/5-machine-learning-papers-to-read-in-2024)

+   [自然语言处理初学者的研究论文](https://www.kdnuggets.com/2022/11/research-papers-nlp-beginners.html)
