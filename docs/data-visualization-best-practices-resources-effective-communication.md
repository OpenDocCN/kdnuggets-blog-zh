# 数据可视化最佳实践与有效沟通资源

> 原文：[https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html](https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html)

![数据可视化最佳实践与有效沟通资源](../Images/8ad3598c257a37a42529fe63f8ce5d57.png)

作者提供的图像

这是我的观点：好的数据可视化是客观的。它确实是一门艺术，但与现代艺术是否好坏的辩论不同，后者无法回答，数据可视化确实有“好”与“坏”之分。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

我们都见过糟糕的图表，并且能够客观地说那个图表很差。同样，我们也见过令人惊叹的数据可视化作品，它们简明而巧妙地传达了复杂的话题。

这是我[最喜欢的之一](https://en.wikipedia.org/wiki/John_Snow#/media/File:Snow-cholera-map-1.jpg)，仅作为优秀数据可视化的一个例子。这是一张1854年霍乱爆发的地图。通过这个简陋的点图，医生约翰·斯诺能够看到死亡率最高的区域，并最终找出霍乱的原因——一个被污染的水井。今天看来这很简单，但在1854年是突破性的。

这很有趣，它内容丰富，吸引进一步调查，并且确定了一个趋势。即使没有流行病学知识，你也可以直观地感受到这里发生了什么。换句话说，这是一项优秀的数据可视化。

![数据可视化最佳实践与有效沟通资源](../Images/09788974b0c5d9e358beb7aff4171119.png)

图片来自[维基百科](https://en.wikipedia.org/wiki/John_Snow#/media/File:Snow-cholera-map-1.jpg)

如果你想看看一个糟糕的例子，看看我为这篇文章生成的图表，我在试图弄清楚人们是否喜欢我的图表制作技能。

这很糟糕。你无法理解我在说什么；它没有帮助发现任何趋势或模式。只需一眼，你就可以说，“是的，Nate，这需要改进。”

![数据可视化最佳实践与有效沟通资源](../Images/d8cf965cc30febc0fe6c7e22c831fcff.png)

作者提供的图像

好消息是，由于这是客观的艺术，你可以学会做好它。这不是一种天赋，而是一种学习的技能。

为了帮助你避免糟糕的数据可视化，我将讨论一些最佳实践。虽然这有点像艺术，但你可以应用一些科学方法来确保你的数据可视化有效地传达信息。

# 什么是数据可视化？

数据可视化就是让数字讲述一个故事。那是我的定义；维基百科则[更加枯燥地定义](https://en.wikipedia.org/wiki/Data_and_information_visualization)为“设计和创建易于沟通和理解的大量复杂数据和信息的图形或视觉表示的过程。”

许多工作最终都会涉及到数据可视化，因为现在还没有单独的“数据可视化专家”这一职位。数据分析师、业务分析师、数据科学家，甚至后端开发者可能会被要求创建一个图形来传达一些关键细节。

例如，作为一个后端开发者，你可能会发现你的数据分析团队希望你[创建一个可视化图表](https://boot.dev/course/7bbb53ed-2106-4f6b-b885-e7645c2ff9d8/36c4edda-52be-4679-a7d4-8fd81c7f7bdb/9b512cbf-c523-4fe5-ac42-1a19469ee110)，来表示二叉搜索树中的结果。作为数据科学家，你将[被要求](https://www.stratascratch.com/blog/what-makes-a-good-data-project/?utm_source=blog&utm_medium=click&utm_campaign=kdn+data+viz+best+practices)将复杂的财务数据转化为对高层管理人员有意义的图表。

数据可视化就是沟通，简单明了。

# 为什么良好的数据可视化很重要？

这就像在问为什么良好的沟通很重要一样。但让我们更深入地解析一下。良好的数据可视化之所以重要，有几个不同的原因。

## 传达复杂信息

想象一下你是1854年的约翰·斯诺。你的病人正在死亡。你知道这是一个模式，你知道这与一个特定的受污染的水井有关。你试图向疲惫不堪、持怀疑态度的城市官员解释这一点，他们并不真正相信疾病会以这种方式传播。

你能想象试图向某人描述那个霍乱图表吗？你会怎么做？这几乎是不可能的。

相比之下，非专家可以看到这个图表并立即理解发生了什么。死亡的模式与地理位置相匹配。这些家庭是从那个水井取水的。他的图表一眼就传达了复杂的信息。这就是良好数据可视化的优势之一。

## 识别模式和趋势

假设你是一个在医疗公司工作的数据科学家。你正在尝试分析病人数据以改善护理，所以你在查看病人统计数据、病史和治疗结果。

当你进行典型的统计分析时，你不会注意到任何突出的模式。然而，当你将死亡率和年龄放在散点图上时，你会发现65岁以上的病人死亡率急剧上升。

![数据可视化最佳实践与有效沟通的资源](../Images/f965452e8b02b64f67b9c1b7cf5f58f5.png)

图片由作者提供

现在你可以将这些发现传递给医疗从业者，以便他们可以研究逆转这一趋势的方法。

# 做好数据可视化的最佳实践是什么？

好了，现在你了解了什么是好的数据可视化以及为什么它很重要。让我们来看看你可以应用的数据可视化最佳实践，以确保你创建出令人惊叹、难忘、引人入胜的图表。

## 了解你的受众

这是最重要的一步。你为谁创建这个数据可视化？他们感兴趣什么？他们已经具备什么基本理解？他们需要这个数据可视化做什么？

例如，假设你是一名数据分析师，试图向CTO解释电子邮件营销活动对品牌各个受众细分的有效性。此次会议的结果将决定下一个季度的整个电子邮件营销策略。

但你忘记了，对你来说是第二天性——如CTR、CTA和像“细分A”这样的细分名称——对于非专家来说并不容易理解。

你展示了以下这种糟糕的效果，并且不得不花整个会议时间重新沟通所有术语和细分名称的具体含义。CTO 感到困惑、不满意，无法做出决策。

![数据可视化最佳实践与有效沟通的资源](../Images/08b25bb0252702396f58c6fa3d88e438.png)

图片由作者提供

相反，你应该将其简化为决策者做出决策所需的主要关键因素，并确保一切对该受众来说都是有意义的。这是一个好的数据可视化版本可能的样子：

![数据可视化最佳实践与有效沟通的资源](../Images/46d290b076863c2f60ef5661296e61ae.png)

图片由作者提供

受众能够清楚地理解数据并做出决策。

## 保持简洁

你知道，当你现在观看 [星球大战](https://makeagif.com/i/6kEv8G) 时，它感觉有点像电影制片人最近发现了所有可以使用的PowerPoint过渡效果，并且仅仅因为乐趣和新奇而使用了每一个？

这是一种不良的数据可视化实践。好的数据可视化实践是尽可能保持简单。

例如，几年前有一个做3D图表的流行趋势。这并没有增加任何传递的信息。但它很花哨，所以人们喜欢它。

![数据可视化最佳实践与有效沟通的资源](../Images/af7929d75946b71c401a6f2e028f61ec.png)

图片来源于 [语义学者](https://www.semanticscholar.org/paper/A-gene-hypermethylation-profile-of-human-cancer.-Esteller-Corn/ac5ea7102ec637690014716104946fe79a29c195)

良好的数据可视化意味着你要保持对数据的关注。如果不需要交互，就不要让它具有交互性。不必要的颜色也不要添加。如果可以通过使标题自解释来去除额外的图例，那就更好。

## 选择正确的图表类型

假设你想展示时间的变化。最好的图表类型是什么？

你对这个问题的回答可能意味着数据可视化的好坏之间的差别，好的数据可视化应当简洁明了，而不是应该被掩盖的怪物。

供参考，正确的答案是折线图。将时间放在 x 轴上，将你所测量的其他因素放在 y 轴上。

![数据可视化最佳实践与有效沟通资源](../Images/773da1771338565989c7380f5b33f9f0.png)

作者提供的图片

回到我之前的丑陋饼图。你可以清楚地看到，这显然不是用来解答我想得到的答案的正确数据可视化类型。饼图表示某种整体性；它非常适合用于累加百分比。所以如果 55% 的员工认为这个图表很好，但 45% 的员工不这样认为，那么饼图适合传达这一发现。

但对于一堆开放文本框的回答？饼图比没用还要糟糕。

这里有一张不错的表格，可以粗略地指导你在何时使用哪种数据可视化类型。请记住，你是自己数据的专家，所以要对此保持一定的警惕。

| 折线图 | 趋势随时间变化 |
| --- | --- |
| 条形图 | 比较组之间的值 |
| 饼图 | 显示不同组的比例 |
| 散点图 | 两个变量之间的关系 |
| 热力图 | 以矩阵格式可视化数据 |
| 树图 | 层级数据 |

我也鼓励你查看数据可视化，并记录你喜欢和不喜欢的内容。记住，数据可视化是客观的。通过一些思考，你可以准确指出哪些有效，哪些无效，并将这些发现应用到你自己的数据可视化中。

## 提供背景信息

最后，你应该始终解释数据可视化背后的原因。数据单位是什么？数据代表什么？还需要哪些相关信息来支持你的观点？

看这个例子了解不该做什么：

![数据可视化最佳实践与有效沟通资源](../Images/c632c7a0a4bd19647344664073cc82de.png)

图片来源于 [Tableau](http://public.tableausoftware.com/views/MLSSalaries/MLSPUDashboard?:embed=y&:display_count=no)

这过于复杂，这已经违反了我们的第二条良好数据可视化准则。但它也没有提供任何背景信息。我应该从中得到什么结论？那些字母是什么意思？为什么那些矩形的比例不对？

如果需要提供定义，就加上它们。如果你认为行业基准能更好地说明你的发现的意义，就添加它。最重要的是，记住你在讲一个故事。如果你只是想提供数字，你可以给人们一个表格。但你不是。你在塑造叙事。这就是为什么背景如此重要。

记住，你是这些数字的专家。你在传达一个想法。你需要提供任何你认为有助于最好地阐述你的观点的补充材料。

# 学习数据可视化的资源

了解数据可视化的方式有两种：学习和实践。让我们分别了解这两种方法。

## 阅读/观看/消费关于数据可视化的内容

首先，你应该扎根于数据可视化的基础。我推荐以下资源：

+   我喜欢David McCandless的[YouTube讲座](https://www.youtube.com/watch?v=5Zg-C8AAIGg)，作为数据可视化之美的起点。

+   [Greg Martin的数据可视化介绍](https://www.youtube.com/watch?v=7eSgGWq6o3I)也是一个很好的入门视频。

+   Simplilearn在YouTube上有一个[27分钟的微型教程](https://www.youtube.com/watch?v=MiiANxRHSv4)。

+   [IBM的Python数据可视化课程](https://coursera.pxf.io/c/3311133/1213620/14726?subId1=dvibmpython&u=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fpython-for-data-visualization&partnerpropertyid=3245039) 是一个不错的下一个步骤，由Coursera主办。它是免费的。

## DIY风格

一旦你完成了听、看和学习的过程，是时候应用你所知道的内容了。获取来自以下来源的可靠数据：

+   [Statista](https://www.statista.com/)

+   [Tidy Tuesdays GitHub](https://github.com/rfordatascience/tidytuesday)

+   你自己的生活——你吃的东西，你花费的时间，你的情绪，你的职业应用，任何事情！

然后，尝试自己制作数据可视化。考虑数据，想想你有什么问题，想发现什么趋势，以及哪些地方令人困惑，可以改进清晰度。

你可以使用像[The Pudding](https://pudding.cool/)或[Kaggle](https://www.kaggle.com/)这样的平台，获取关于你可以提出或回答的问题的灵感。

我还建议你查看一下真实面试官在数据科学面试中提出的问题。像[StrataScratch](https://www.stratascratch.com/?utm_source=blog&utm_medium=click&utm_campaign=kdn+data+viz+best+practices)这样的平台可以帮助你在实际案例中练习数据可视化技能。

想要更多？[掌握数据可视化的30个资源](/2022/11/30-resources-mastering-data-visualization.html) 是一个很好的数据可视化资源汇总列表。

# 良好数据可视化沟通的最佳实践

那句经典名言：“一图胜千言。”如果这是真的，那么好的数据可视化就是一个图书馆的文字。

良好的数据可视化是几乎所有有意义的公司决策的支柱。它帮助来自不同部门的人以对所有方都有意义的方式进行沟通。这就是如何将一堆数字整理成一个故事。

但它很容易出错。要正确进行数据可视化，请记住，你需要考虑观众的需求，保持尽可能简单，选择正确类型的图表，并始终提供背景信息。

希望这本插图指南能帮助你更好地理解什么是良好的数据可视化，以及如何在未来制作出最佳的数据可视化。

**[Nate Rosidi](https://www.stratascratch.com)** 是一位数据科学家和产品战略专家。他还是一名兼职教授，教授分析学，并且是 [StrataScratch](https://www.stratascratch.com/) 的创始人，该平台帮助数据科学家准备顶级公司面试的真实面试问题。可以通过 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 与他联系。

### 更多相关主题

+   [提示工程101：掌握有效的LLM沟通](https://www.kdnuggets.com/prompt-engineering-101-mastering-effective-llm-communication)

+   [掌握数据可视化的30个资源](https://www.kdnuggets.com/2022/11/30-resources-mastering-data-visualization.html)

+   [数据仓储和ETL最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [云计算和数据迁移到 AWS 云的11个最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [将 ChatGPT 集成到数据科学工作流程中：技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [数据科学团队协作的5个最佳实践](https://www.kdnuggets.com/2023/06/5-best-practices-data-science-team-collaboration.html)
