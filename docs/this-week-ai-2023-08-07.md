# 本周AI动态，8月7日：生成性AI进入Jupyter和Stack Overflow • ChatGPT更新

> 原文：[https://www.kdnuggets.com/2023/mm/this-week-ai-2023-08-07.html](https://www.kdnuggets.com/2023/mm/this-week-ai-2023-08-07.html)

![生成性AI进入Jupyter和Stack Overflow • ChatGPT更新](../Images/7bb2119b4b7ee021871a2a624a188aca.png)

图片由编辑使用 Midjourney 创建

欢迎来到本周的“本周AI动态”版块，由KDnuggets提供。这个精心策划的每周帖子旨在让你了解人工智能快速发展的世界中的最引人注目的进展。从塑造我们对AI在社会中角色的突破性头条新闻，到发人深省的文章、深刻的学习资源和推动我们知识边界的研究亮点，这个帖子提供了对AI当前格局的全面概述。此每周更新旨在让你在这一不断发展的领域中保持更新和知情。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

# 标题

“标题”部分讨论了过去一周在人工智能领域的主要新闻和进展。信息涵盖了从政府AI政策到技术进步和企业AI创新的各个方面。

???? **[Jupyter中的生成性AI](https://blog.jupyter.org/generative-ai-in-jupyter-3f7174824862?gi=47d7909ee7ed)**

开源项目Jupyter团队发布了Jupyter AI，这是一种新扩展，将生成性AI功能直接引入Jupyter笔记本和JupyterLab IDE。Jupyter AI允许用户通过聊天互动和魔法命令利用大型语言模型来解释代码、生成新代码和内容、回答有关本地文件的问题等。它在设计时考虑了负责任的AI，允许对模型选择和AI生成输出的跟踪进行控制。Jupyter AI支持像Anthropic、AWS、Cohere和OpenAI这样的提供商。其目标是以伦理的方式使AI更易于访问，以提升Jupyter笔记本的体验。

???? **[宣布OverflowAI](https://stackoverflow.blog/2023/07/27/announcing-overflowai/)**

Stack Overflow宣布了OverflowAI，这是他们将AI能力整合到公共问答平台、Stack Overflow for Teams以及IDE扩展等新产品中的项目。其功能包括语义搜索以寻找更相关的结果、摄取企业知识以加快内部问答的启动、一个访问Stack Overflow内容的Slack聊天机器人，以及一个在开发者工作流程中呈现答案的VS Code扩展。他们旨在利用其社区中超过5800万个问题，同时通过对AI生成内容的归属和透明度来确保信任。目标是通过负责任地使用AI来提升开发者的效率，将他们与上下文中的解决方案相连接。

???? **[ChatGPT更新](https://twitter.com/OpenAI/status/1687159114047291392)**

在过去的一周中，我们推出了若干小更新，以提升ChatGPT的使用体验。这些更新包括引入提示示例以帮助用户开始聊天、建议回复以促使更深层次的互动，以及为Plus用户提供默认使用GPT-4的偏好设置。此外，还引入了多文件上传功能（在Plus用户的代码解释器测试版中）、新的保持登录功能以及一系列键盘快捷键，以提高可用性。

# 文章

“文章”部分展示了一系列引人深思的人工智能相关作品。每篇文章深入探讨一个特定主题，为读者提供关于AI的各种见解，包括新技术、革命性方法和开创性的工具。

???? **[我在3天内创建了一个AI应用](https://www.kdnuggets.com/2023/08/created-ai-app-3-days.html)**

作者通过ChatGPT提示实验，创建了一个名为Tally.Work的AI驱动的求职信生成器网络应用，仅用了3天时间，前端使用了Bubble.io，生成文本则通过OpenAI API完成。该应用程序以用户的简历和职位描述为输入，输出定制的求职信。目标是构建一个具有巨大潜在用户基础的应用。尽管AI生成的文本尚不完美，但可以帮助创建有用的初稿。作者相信AI将消除许多繁琐的任务，如求职信，并希望这个项目能引领更多有趣的AI应用的出现。总体来看，它展示了使用无代码工具和AI API快速构建和推出应用创意的可能性。

???? **[生产环境中部署生成模型的三大挑战](https://towardsdatascience.com/three-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3)**

文章讨论了在生产环境中部署生成性 AI 模型（如 GPT-3 和 Stable Diffusion）的三个主要挑战：模型的巨大规模导致高计算成本、可能传播有害偏见的偏见以及需要调整的一致性输出质量。解决方案包括模型压缩、在无偏数据上训练、后处理过滤器、提示工程和模型微调。总体而言，文章概述了公司必须仔细解决这些问题，以成功利用生成模型，同时避免潜在的负面影响。

# 工具

"工具"部分列出了社区创建的有用应用程序和脚本，供那些希望投入实际 AI 应用的人使用。在这里，你将找到各种工具类型，从大型综合代码库到小型利基脚本。*请注意，这些工具的分享不代表任何背书，也不提供任何形式的保证。在安装和使用任何软件之前，请自行进行调查！*

????️ **[机器人写作室](https://github.com/jbpayton/robot-writers-room)**

> 该存储库演示了如何与人类协作使用 AI 来集思广益和完善故事创意。AI 不是取代人类，而是作为创意伙伴，提出想法并进行研究。在每一步，人类可以接受、拒绝或修改 AI 的建议。写作中的主要挑战之一是构思创意。该项目旨在通过提供一个创意伙伴来帮助作家克服创作瓶颈。

????️ **[格但斯克 AI](https://github.com/jmaczan/gdansk-ai)**

> 格但斯克 AI 是一个全栈 AI 语音聊天机器人（语音转文本、LLM、文本转语音），集成了 Auth0、OpenAI、Google Cloud API 和 Stripe - Web 应用、API 和 AI

# 研究亮点

"研究亮点"部分突出了人工智能领域的重要研究。该部分包括突破性的研究、探索新理论以及讨论该领域的潜在影响和未来方向。

???? **[ToolLLM: 使大语言模型掌握 16000+ 真实世界 API](https://arxiv.org/abs/2307.16789v1)**

论文介绍了 ToolLLM，一个用于增强开源大语言模型工具使用能力的框架。它构建了一个名为 ToolBench 的数据集，包含 49 个类别中涉及 16,000 个真实世界 API 的指令。ToolBench 是通过 ChatGPT 自动生成的，几乎不需要人工干预。为了提升推理能力，作者提出了一种深度优先搜索决策树方法，使模型能够评估多个推理轨迹。他们还开发了一个自动评估器 ToolEval，以高效评估工具使用能力。通过在 ToolBench 上微调 LLaMA，他们获得了 ToolLLaMA，该模型在 ToolEval 上表现出色，包括对未见过的 API 具有良好的泛化能力。总体而言，ToolLLM 提供了一种解锁开源 LLM 中复杂工具使用的方法。

???? **[MetaGPT: 多智能体协作框架的元编程](https://arxiv.org/abs/2308.00352v2)**

本文介绍了MetaGPT，一个旨在改善大语言模型在复杂任务中的协作的框架。它将现实世界的标准化操作程序融入提示中，以指导多代理协调。像ProductManager和Architect这样的角色会生成符合行业惯例的结构化输出。共享的环境和记忆能够促进知识共享。在软件任务中，MetaGPT生成了比AutoGPT和AgentVerse更多的代码和文档，并且成功率更高，显示了它在跨专业代理之间分解问题的能力。标准化的工作流程和输出旨在减少对话中的不连贯性。总体而言，MetaGPT展示了一种将人类专业知识捕捉到代理中以解决复杂现实问题的方法。

### 更多相关内容

+   [免费 4 周数据科学课程：AI质量管理](https://www.kdnuggets.com/2022/02/truera-free-4-week-data-science-course-ai-quality-management.html)

+   [本周提升搜索应用的8种方法](https://www.kdnuggets.com/2022/09/corise-8-ways-improve-search-application-week.html)

+   [回到基础 第1周：Python编程与数据科学基础](https://www.kdnuggets.com/back-to-basics-week-1-python-programming-data-science-foundations)

+   [回到基础 第3周：机器学习介绍](https://www.kdnuggets.com/back-to-basics-week-3-introduction-to-machine-learning)

+   [回到基础 第4周：高级主题与部署](https://www.kdnuggets.com/back-to-basics-week-4-advanced-topics-and-deployment)

+   [回到基础 第2周：数据库、SQL、数据管理及…](https://www.kdnuggets.com/back-to-basics-week-2-database-sql-data-management-and-statistical-concepts)
