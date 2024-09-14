# 5种ChatGPT无法完成的编码任务

> 原文：[https://www.kdnuggets.com/5-coding-tasks-chatgpt-cant-do](https://www.kdnuggets.com/5-coding-tasks-chatgpt-cant-do)

![5 Coding Tasks ChatGPT Can't Do](../Images/79007134ebc240c2d9faecfe5f30200a.png)

作者提供的图片

我喜欢把ChatGPT看作是StackOverflow的一个更智能的版本。非常有帮助，但短期内不会取代专业人士。作为前数据科学家，我在ChatGPT发布时花了大量时间进行尝试。我对它的编码能力相当印象深刻。它能从零开始生成相当有用的代码；它可以对我自己的代码提供建议。如果我要求它帮我解决错误信息，它在调试方面表现也很好。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT

* * *

但不可避免的是，使用它的时间越长，我就越遇到它的局限性。对于那些担心ChatGPT会取代他们工作的开发者来说，以下是ChatGPT无法做到的事情列表。

![5 Coding Tasks ChatGPT Can't Do](../Images/c9bd2195d82f79dd755ee06f64bb475d.png)

# 1\. 任何贵公司会专业使用的代码

第一个限制不是关于其能力，而是法律问题。任何完全由ChatGPT生成的代码，如果您将其复制粘贴到公司产品中，可能会使您的雇主面临麻烦的诉讼。

这是因为ChatGPT会自由地从其训练数据中提取代码片段，这些数据来自互联网各处。Reddit用户ChunkyHabaneroSalsa [解释说](https://www.reddit.com/r/Python/comments/12wsx2g/comment/jhh3ho1/?utm_source=share&utm_medium=web2x&context=3)：“我让ChatGPT为我生成了一些代码，我立刻认出了它从哪个GitHub仓库获取了大部分内容。”

最终，我们无法确定ChatGPT的代码来源，也无法知道它遵循了什么许可证。即使代码完全是从零生成的，ChatGPT生成的任何内容本身也不能获得版权。正如Bloomberg Law的作者Shawn Helms和Jason Krieser [指出的](https://www.bloomberglaw.com/external/document/XDDQ1PNK000000/copyrights-professional-perspective-copyright-chaos-legal-implic)，“‘衍生作品’是‘基于一个或多个现有作品的作品。’ChatGPT是基于现有作品进行训练的，并且根据这种训练生成输出。”

如果您使用ChatGPT生成代码，您可能会发现自己面临雇主的麻烦。

# 2\. 任何需要批判性思维的任务

这是一个有趣的测试：让 ChatGPT 创建一个在 Python 中运行统计分析的代码。

这是否是正确的统计分析？可能不是。ChatGPT 不知道数据是否符合测试结果有效所需的假设。ChatGPT 也不知道利益相关者想看到什么。

例如，我可能会让 ChatGPT 帮助我确定不同年龄组的满意度评分是否存在统计学上的显著差异。ChatGPT 建议使用独立样本 T 检验，并未发现年龄组之间有统计学上的显著差异。但 T 检验在这里并不是最佳选择，原因有几个，比如可能存在多个年龄组，或者数据不符合正态分布。

![5 Coding Tasks ChatGPT Can't Do](../Images/7ee9e63545f462b08f8a687717114ff3.png)

图片来自 [decipherzone.com](https://www.decipherzone.com/blog-detail/chat-gpt-memes)

一位 [全栈数据科学家](https://www.stratascratch.com/blog/how-to-become-a-full-stack-data-scientist/?utm_source=blog&utm_medium=click&utm_campaign=kdn+coding+chatgpt+cant+do) 会知道检查哪些假设和运行什么样的测试，并可以给 ChatGPT 更具体的指令。但 ChatGPT 自身会高兴地为错误的统计分析生成正确的代码，从而使结果不可靠且无法使用。

对于需要更多批判性思维和问题解决的问题，ChatGPT 并不是最佳选择。

# 3\. 理解利益相关者优先事项

任何数据科学家都会告诉你，工作的一部分是理解和解释项目中的利益相关者优先事项。ChatGPT，或者任何 AI，都不能完全理解或管理这些。

首先，利益相关者的优先事项通常涉及复杂的决策，这些决策不仅考虑数据，还涉及人类因素、业务目标和市场趋势。

例如，在应用程序重新设计中，你可能会发现市场营销团队希望优先考虑用户参与功能，销售团队推动支持交叉销售的功能，而客户支持团队需要更好的应用内支持功能来帮助用户。

ChatGPT 可以提供信息并生成报告，但它无法做出与不同利益相关者的多样化且有时相互竞争的利益相符的微妙决策。

此外，利益相关者管理通常需要高度的情商——即能够与利益相关者产生共鸣，理解他们的关切，并对他们的情感做出回应。ChatGPT 缺乏情商，无法管理利益相关者关系中的情感方面。

你可能不会将其视为编码任务，但目前正在为该新功能推出编写代码的数据科学家清楚其中有多少工作与利益相关者的优先事项相关。

# 4\. 新颖问题

ChatGPT 无法提出真正新颖的内容。它只能重新混合和重新构造从其训练数据中学到的东西。

![ChatGPT 无法完成的 5 个编码任务](../Images/9c7f4a026c63989d61ed4197fc1b2dc3.png)

图片来自 [theinsaneapp.com](https://www.theinsaneapp.com/2023/05/chatgpt-memes.html)

想知道如何更改 R 图中的图例大小吗？没问题——ChatGPT 可以从成千上万的 StackOverflow 答案中获取相同问题的解决方案。但是（使用我要求 ChatGPT 生成的一个例子），如果是一个它不太可能遇到的情况，比如组织一个社区聚餐，每个人的菜肴必须包含一个以其姓氏首字母为开头的成分，并确保有多样的菜肴，那该怎么办？

当我测试这个提示时，它给了我一些 Python 代码，决定了*菜肴*的名字必须与姓氏匹配，甚至没有正确捕捉到成分要求。它还要求我想出26种菜肴类别，每种字母一个。这不是一个聪明的答案，可能是因为这是一个完全新颖的问题。

# 5.道德决策制定

最后但同样重要的是，ChatGPT 无法道德地编写代码。它没有能力像人类那样做出价值判断或理解代码的道德影响。

道德编码涉及考虑代码可能如何影响不同的群体，确保它不歧视或造成伤害，并做出符合道德标准和社会规范的决策。

例如，如果你让 ChatGPT 编写一个贷款审批系统的代码，它可能会生成一个基于历史数据的模型。然而，它无法理解该模型可能因数据中的偏见而拒绝向边缘化社区提供贷款的社会影响。需要人类开发者识别公平与公正的需求，寻求并纠正数据中的偏见，并确保代码符合道德实践。

值得指出的是，人们也不是完美的——有人编写了[亚马逊的偏见招聘工具](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G)，还有人编写了[谷歌照片分类](https://www.theverge.com/2018/1/12/16882408/google-racist-gorillas-photo-recognition-algorithm-ai)，将黑人识别为猩猩。但人类在这方面做得更好。ChatGPT 缺乏编写道德代码所需的同理心、良知和道德推理能力。

人类可以理解更广泛的背景，识别人类行为的细微差别，并进行关于对错的讨论。我们参与道德辩论，权衡特定方法的利弊，并对我们的决策负责。当我们犯错时，我们可以从中学习，这有助于我们的道德成长和理解。

# 最终思考

我喜欢Reddit用户Empty_Experience_10对这一点的[看法](https://www.reddit.com/r/learnpython/comments/zifoql/comment/j2nvxq9/?utm_source=share&utm_medium=web2x&context=3)： “如果你只会编程，你就不是一名软件工程师，没错，你的工作会被替代。如果你认为软件工程师高薪是因为他们会写代码，那你对软件工程师的真正含义有根本性的误解。”

我发现ChatGPT在调试、代码审查方面非常出色，而且比查找StackOverflow上的答案要快一些。但“编码”远不止于将Python输入键盘。这包括了解你业务的目标，理解在算法决策时必须多么小心，与利益相关者建立关系，真正了解他们的需求和原因，并寻找实现这些目标的方式。

这是讲故事，知道何时选择饼图或柱状图，以及理解数据试图传达的叙事。它关乎于将复杂的想法用简单的术语传达给利益相关者，让他们能够理解并据此做出决策。

ChatGPT无法做到这些。只要你能做到，你的工作就安全。

[](https://twitter.com/StrataScratch)****[Nate Rosidi](https://twitter.com/StrataScratch)****是一名数据科学家，专注于产品战略。他还是一名兼职教授，教授分析学，同时是StrataScratch的创始人，该平台帮助数据科学家准备面试，提供来自顶级公司的真实面试问题。Nate撰写关于职业市场的最新趋势，提供面试建议，分享数据科学项目，并涵盖所有SQL相关内容。

### 更多相关话题

+   [ChatGPT作为教育资源是否可信？](https://www.kdnuggets.com/2023/05/chatgpt-trusted-educational-resource.html)

+   [5种利用ChatGPT代码解释器进行数据科学的方法](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)

+   [5种利用ChatGPT视觉进行数据分析的方法](https://www.kdnuggets.com/5-ways-you-can-use-chatgpt-vision-for-data-analysis)

+   [5种用Python自动化的任务](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [HuggingGPT：解决复杂AI任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)
