# 理解 AI 中的代理环境

> 原文：[https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html](https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html)

# AI 代理简要介绍

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

在开始文章之前，理解 AI 中的代理（agent）是很重要的。代理基本上是一个帮助 AI、机器学习或深度强化学习做出决策或触发 AI 做出决策的实体。从软件的角度来看，它被定义为能够做出决策并根据环境变化或从外部环境获取输入后做出不同决策的实体。代理在这些领域中的作用非常重要，因为 AI 模型的所有参数主要依赖于代理的表现。简单来说，快速感知外部变化并对其做出反应的代理，模型得到的结果就越好。因此，代理在[人工智能](https://algoscale.com/artificial-intelligence-solution-providers/) 、机器学习和深度学习中的作用总是非常重要。AI 代理的最常见例子是 Alexa 和 Siri。因为它们都能够根据用户的请求行动，从互联网收集信息，提供解决方案，并做出相应的行动。这些代理非常智能，能够收集用户的完整信息，甚至建议与用户相关的广告和推荐。(*Intelligent Agents in Artificial Intelligence | Engineering Education (EngEd) Program | Section*, 2019)

![一个闭环系统，其中系统从环境中获取一些输入，并向环境提供一些动作](../Images/d1d9433c1ad2357bc63b68e535bb1d34.png)

图 1\. 一个闭环系统，其中系统从环境中获取一些输入，并向环境提供一些动作 | 图片来源：编辑

# 智能代理

智能代理是一种基于软件的程序，能够理解情况、感知环境，并根据过去的历史和常规训练来优化搜索。智能代理最重要的部分基本上是用户输入，即用户感兴趣的事物类型。智能代理的一个常见例子是 Facebook、YouTube 和 Google 上使用的代理。它们总是展示吸引用户的广告。例如，如果你想在线购买香水，并且在互联网上搜索了两到三个不同的香水店，那么在你之后的社交媒体新闻源中，你会看到与香水相关的不同广告。这些广告是因为背后有智能人工智能代理在处理所有的搜索历史和用户输入。智能代理的几种类型包括人类代理、软件代理和机器人代理。(*What Is an Intelligent Agent? - Definition from WhatIs.Com*, n.d.)

# 代理类型

在人工智能和机器学习中，存在几种类型的代理（Electronic, n.d.）。这一部分将简要讨论它们的各个类型。

+   简单反射代理是最常见的一种代理类型。它们仅根据当前感知做出反应，忽略所有的过去历史和路径。一个常见的例子是房间清洁代理，它仅在房间里有灰尘时才会工作，而不考虑过去的情况。

+   基于模型的反射代理只能部分观察环境。它们还会跟踪情况。这里的“模型”一词指的是世界上发生的事情，它有一个内部状态，基本上代表了基于模型历史的当前状态。

+   基于目标的代理不仅需要当前状态、过去状态和需要做什么。它们还需要有关目标或目的的信息。它们更具能力，因为它们根据其他机器学习技术绘制路径，以更好和更有效地达成目标。它们根据环境的情况选择行动。

+   基于效用的代理与基于目标的代理类似，但它们还有额外的责任来关注效用。它们在每一步工作中向模型提供效率和工作状态。它们通过反馈系统提供真实的效率数字，以检查系统的有效性和效率。

+   学习型代理在这方面是最智能的类型。它们从用户输入、之前的历史、环境和周围情况中学习。它们起初像传统代理一样，拥有环境的基本知识。它包含不同的组件，如学习元素，这些元素通过失败和成就来提高代理的能力。另一个组件是批评，通过它学习元素得到反馈，判断工作是否朝着正确的方向进行。第三个是性能元素；这个组件负责衡量系统的性能并保持系统的正常运作。最后一个组件是问题生成器，通过它系统将获得新的和最新的经验，并通过不断的负载工作来改进工作。 (*AI 代理的类型 - Javatpoint*，n.d.)

# AI 代理的应用

这些代理程序应用或工作的地方是什么？这些代理程序大多数是基于软件的。它们在游戏、搜索和自动表单填写中工作。在游戏中，不同的公司为了让他们的游戏更具竞争力，使用这些基于 AI 的代理程序。这些代理程序大多数是学习型代理，类似于从用户的操作中学习的代理，通过玩大量游戏和过去的经验可以从初学者水平提升到专业水平。还有一些游戏使用了非常低级的代理程序，这些代理程序通常是简单的反射型代理，因为它们随着时间的推移没有改进，到了某个时刻，用户会对游戏感到厌倦而停止游戏。以同样的方式，这些代理程序在搜索引擎中也会工作。如观察到的，谷歌仅根据用户的搜索历史、之前的输入和过去的经验显示相关页面。这些代理程序在这种搜索中非常强大，能够非常快速地捕捉数据。 (*Intelligent Agents: Characteristics and Applications | AI*, n.d.)

## 参考文献

+   *Intelligent Agents: Characteristics and Applications | AI*。（n.d.）。检索于2022年2月26日，网址为 https://www.engineeringenotes.com/artificial-intelligence-2/intelligent-agents/intelligent-agents-characteristics-and-applications-ai/35618

+   *人工智能中的智能代理 | 工程教育（EngEd）计划 | Section*。（n.d.）。检索于2022年2月26日，网址为 https://www.section.io/engineering-education/intelligent-agents-in-ai/

+   *AI 代理的类型 - Javatpoint*。（n.d.）。检索于2022年2月26日，网址为 https://www.javatpoint.com/types-of-ai-agents

+   *什么是智能代理？ - Definition from WhatIs.com*。（n.d.）。检索于2022年2月26日，网址为 https://www.techtarget.com/searchenterpriseai/definition/agent-intelligent-agent

**[Neeraj Agarwal](https://www.linkedin.com/in/neeagl/)** 是 [Algoscale](https://www.linkedin.com/company/algoscale) 的创始人，该公司是一家涵盖数据工程、应用AI、数据科学和产品工程的数据咨询公司。他在这一领域拥有超过9年的经验，帮助了从初创公司到财富100强的各种组织处理和存储大量原始数据，以将其转化为可操作的洞察，从而促进更好的决策和更快的业务价值。

### 更多相关主题

+   [你应该阅读的生成性代理研究论文](https://www.kdnuggets.com/generative-agent-research-papers-you-should-read)

+   [机器学习算法的完整端到端部署](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [在3分钟内理解偏差-方差权衡](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [理解贝叶斯定理的三种方法将提升你的数据科学技能](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [通过实现理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [理解 Python 中的可迭代对象与迭代器](https://www.kdnuggets.com/2022/01/understanding-iterables-iterators-python.html)
