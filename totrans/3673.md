# Chatbot Arena：LLM基准测试平台

> 原文：[https://www.kdnuggets.com/2023/05/chatbot-arena-llm-benchmark-platform.html](https://www.kdnuggets.com/2023/05/chatbot-arena-llm-benchmark-platform.html)

![Chatbot Arena：LLM基准测试平台](../Images/21f44c8b4c0b3a2d946c0ba3951d2490.png)

图片由作者提供

我们都知道大型语言模型（LLM）已经在世界范围内掀起了风暴，在如此短的时间内，信息量实在太大。

# 什么是Chatbot Arena？

为了再添些许变化，[Chatbot Arena](https://chat.lmsys.org/?arena)是由[大型模型系统组织](https://lmsys.org/)（LMSYS Org）创建的LLM基准测试平台。它是一个由加州大学伯克利分校的学生和教职员工创办的开放研究组织。

他们的总体目标是通过使用开放数据集、模型、系统和评估工具的共同开发方法，使大型模型对每个人更加可及。LMSYS团队训练大型语言模型，并广泛提供这些模型，同时开发分布式系统，以加速LLM的训练和推理。

## 对于LLM基准测试的需求

随着ChatGPT持续的热度，开源LLM迅速增长，这些模型经过微调以遵循特定的指令。比如Alpaca和Vicuna，它们基于LLaMA，可以根据用户的提示提供帮助。

然而，对于这种快速发展的事物，社区很难跟上不断的新进展，并有效地对这些模型进行基准测试。由于可能存在开放性问题，对LLM助手进行基准测试可能是一个挑战。

因此，需要进行人工评估，采用成对比较的方法。成对比较是将模型成对比较以判断哪个模型表现更好的过程。

# Chatbot Arena如何运作？

在Chatbot Arena中，用户可以并排与两个匿名模型对话，形成自己的意见，并投票选出哪个模型更好。一旦用户投票，模型的名称将会被揭示。用户可以选择继续与这两个模型对话，或重新开始，与两个新的随机选择的匿名模型对话。

你可以选择同时与两个匿名模型进行对话，或选择你想对话的模型。下面是与两个匿名模型对话的截图示例，展示了LLM对战！

![Chatbot Arena：LLM基准测试平台](../Images/2534b4a61e93804db88b100996f36926.png)

图片截图由作者提供

收集的数据会被计算为Elo评级，并放入[排行榜](https://chat.lmsys.org/?leaderboard)。Elo评级系统是一种用于计算玩家相对技能水平的方法，常用于象棋等游戏中。两个用户之间的评级差异可以预测该场比赛的结果。

截至2023年5月5日，这就是Chatbot Arena排行榜的样子：

![Chatbot Arena: LLM 基准平台](../Images/03f45d5681c629cc3d0ac94cb009eec5.png)

图片来源 [Chatbot Arena](https://lmsys.org/blog/2023-05-03-arena)

如果你想看看这是怎么做的，可以查看 [notebook](https://colab.research.google.com/drive/1lAQ9cKVErXI1rEYq7hTKNaCQ5Q8TzrI5?usp=sharing) 并自己操作投票数据。

真是一个很棒又有趣的主意，对吧？

# 我怎么参与？

Chatbot Arena 的团队邀请整个社区通过贡献自己的模型，加入他们的 LLM 基准测试之旅，并参与对匿名模型的投票。

访问 [Arena](https://arena.lmsys.org) 投票选出你认为更好的模型，如果你想测试特定模型，可以参考 [指南](https://github.com/lm-sys/FastChat/blob/main/docs/arena.md#how-to-add-a-new-model) 将其添加到 Chatbot Arena。

# 总结

那么 Charbot Arena 还会有更多更新吗？根据团队的说法，他们计划进行以下工作：

+   添加更多闭源模型

+   添加更多开源模型

+   定期发布更新的排行榜。例如，每月更新一次。

+   使用更好的采样算法、锦标赛机制和服务系统来支持更多模型。

+   为不同任务类型提供精细化的排名系统。

玩一下 Chatbot Arena，告诉我们你的想法吧！

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是数据科学家、自由技术写作者和 KDnuggets 的社区经理。她特别关注提供数据科学职业建议或教程和理论知识。她还希望探索人工智能如何有助于人类寿命的延续。作为一个热心学习者，她寻求拓宽技术知识和写作技能，同时帮助指导他人。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 更多相关信息

+   [KDnuggets 调查：与同行对比行业支出及趋势](https://www.kdnuggets.com/2023/02/kdnuggets-survey-industry-spend-trends.html)

+   [KDnuggets 调查：与同行对比数据科学支出及趋势](https://www.kdnuggets.com/kdnuggets-survey-benchmark-peers-data-science-spends-trends)

+   [聊天机器人转型：从失败到未来](https://www.kdnuggets.com/2021/12/chatbot-transformation-failure-future.html)

+   [Open Assistant：探索开放和协作的可能性…](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)

+   [用这些课程构建类似 ChatGPT 的聊天机器人](https://www.kdnuggets.com/2023/05/build-chatgptlike-chatbot-courses.html)

+   [用 Hugging Face 和 Gradio 在 5 分钟内构建 AI 聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)
