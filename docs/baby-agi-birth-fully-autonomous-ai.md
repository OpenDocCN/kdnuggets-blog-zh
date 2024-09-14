# Baby AGI: 完全自主 AI 的诞生

> 原文：[`www.kdnuggets.com/2023/04/baby-agi-birth-fully-autonomous-ai.html`](https://www.kdnuggets.com/2023/04/baby-agi-birth-fully-autonomous-ai.html)

![Baby AGI: 完全自主 AI 的诞生](img/d94f24230a58ad9a8e3bf8c74b7e7999.png)

图片由作者提供

任务管理是每个企业中非常重要的一个元素。它在营销管道、技术和时尚等领域都有应用。它是从项目开始到结束监控任务的过程。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

随着人工智能的持续繁荣，我们看到越来越多的应用程序被发布，以帮助特定的过程，例如任务管理。

# 什么是 Baby AGI？

[Baby AGI](https://github.com/yoheinakajima/babyagi) 是一个使用 [OpenAI](https://openai.com/) 和 [Pinecone](https://www.pinecone.io/) API，以及 [LangChain](https://python.langchain.com/en/latest/index.html) 框架的 Python 脚本，用于创建、组织、优先排序以及执行任务。Baby AGI 的过程是，它将使用基于前一个任务结果的预定义目标创建任务。

这是通过使用 OpenAI 的自然语言处理（NLP）功能来实现的，这允许系统根据目标创建新任务。它使用 Pinecone 来存储特定任务的结果并检索上下文，以及 LangChain 框架来处理决策过程。

例如，您向系统提出一个目标，系统将持续优先排序需要实现或完成的任务，以达到目标。一旦这些任务完成，它们将被存储在记忆中。

系统在无限循环中运行，并通过 4 个步骤执行：

1.  第一个任务从任务列表中提取

1.  任务被发送到执行代理，并根据上下文使用 OpenAI API 完成任务

1.  结果被存储到 Pinecone 中

1.  新任务根据目标和前一个任务的结果进行创建和优先排序。

## 执行代理

```py
execution_agent
```

执行代理是系统的核心，它利用 OpenAI 的 API 来处理任务。该功能接受两个参数：目标和任务，用于向 OpenAI 的 API 发送提示。这将返回任务的结果作为字符串。

## 任务创建代理

```py
task_creation_agent()
```

任务创建代理功能使用 OpenAI 的 API 根据当前对象和之前任务的结果创建新任务。该功能接受四个参数：目标、前一个任务的结果、任务描述和当前任务列表。

然后向 OpenAI 的 API 发送一个提示，该 API 将返回一个新的任务列表作为字符串。该功能将这些新任务作为字典列表返回，每个字典包含任务的名称。

## 优先级代理

```py
prioritization_agent()
```

最后一步是任务列表的排序和优先级排序。这是优先级代理功能参与的地方，它使用 OpenAI 的 API 对任务列表进行重新排序。该功能接受一个参数，即当前任务的 ID。然后，它会向 OpenAI 的 API 发送一个提示，并返回一个重新排序的新的任务列表，以编号列表的形式呈现。

![Baby AGI: 完全自主 AI 的诞生](img/48ed67b35ce8e79f9b79e882cfcedf0d.png)

图片来源 [Yohei](https://twitter.com/yoheinakajima/status/1640934493489070080/photo/1)（Baby AGI 的创作者）

上述流程展示了 Baby AGI 系统的工作原理。它基于我们讨论的三个代理：执行、创建和优先级排序。

你根据已有的目标开始任务，然后转到从记忆中获取上下文的查询。这将其发送给创建代理，该代理接收数据并将其发送到记忆中。然后，它会经过一个队列，这个队列经过任务的优先级排序。

Baby AGI 具有完成任务、根据先前结果生成新任务以及实时优先排序任务的能力。该系统正在探索并展示大型语言模型的潜力，例如 GPT，以及它如何自主地执行任务。

# 如何使用 Baby AGI？

使用 Baby AGI 的步骤如下：

1.  首先，你需要通过 git clone https://github.com/yoheinakajima/babyagi.git 克隆仓库，并进入克隆的仓库。

1.  第二步是安装所需的包：pip install -r requirements.txt

1.  第三步是将 .env.example 文件复制到 .env：cp .env.example .env。在这里你将设置所需的变量。

1.  第四步是将你的 API 密钥设置到各自的变量中，例如 OpenAI 的 OPENAI_API_KEY 和 OPENAPI_API_MODEL，Pinecone 的 PINECONE_API_KEY 变量中。

1.  第五步是设置 PINECONE_ENVIRONMENT 变量中的 Pinecone 环境。

1.  第六步是设置 TABLE_NAME 变量中的表名。这是任务结果将被存储的地方。

1.  如果你已经有了一个目标，你可以在 OBJECTIVE 变量中设置任务管理系统的目标。（可选）

1.  如果你已经有了第一个任务，你可以在 INITIAL_TASK 变量中设置它。（可选）

1.  运行脚本。

Baby AGI 是一个任务管理系统，因此脚本被设计为持续运行。这可能导致高 API 使用、内存和资金消耗。监控系统的行为以及确保应用程序的有效性是重要的。

# Baby AGI 的未来

Yohei（BabyAGI 的创造者）表示，他们计划在系统中进行未来的改进，包括集成的安全/保障代理、并行任务等。

下面的图片定义了这些未来计划：

![Baby AGI: 完全自主 AI 的诞生](img/2122b344feb388ff4e0e093c649f90a5.png)

图片来自 [Yohei](https://twitter.com/yoheinakajima/status/1640934493489070080/photo/1)（Baby AGI 的创造者）

# 总结

单独的自主 AI 可以提供诸如数据录入、项目管理等琐碎任务的协助。这将使员工能够更多地关注需要更多时间和精力的任务，而无需担心可以通过 Baby AGI 等系统完成的小任务。

如果你想查看 Baby AGI 的安装和测试过程，请查看 Matthew Berman 的这个视频：[NEW BabyAGI](https://www.youtube.com/watch?v=pAtguEz7CBs)

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是 KDnuggets 的数据科学家、自由职业技术作家和社区经理。她特别关注提供数据科学职业建议或教程以及围绕数据科学的理论知识。她还希望探索人工智能如何能够有利于人类寿命的不同方式。作为一个热衷学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关主题

+   [人工智能与开源软件：天生一对？](https://www.kdnuggets.com/ai-and-open-source-software-separated-at-birth)

+   [深度神经网络不会引导我们走向 AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [我们离 AGI 还有多远？](https://www.kdnuggets.com/how-close-are-we-to-agi)

+   [AgentGPT: 浏览器中的自主 AI 代理](https://www.kdnuggets.com/2023/06/agentgpt-autonomous-ai-agents-browser.html)

+   [为什么你需要了解自主 AI 代理](https://www.kdnuggets.com/2023/06/need-know-autonomous-ai-agents.html)

+   [基于 LLM 的自主代理的增长](https://www.kdnuggets.com/the-growth-behind-llmbased-autonomous-agents)
