# 数据科学家的四项工作

> 原文：[`www.kdnuggets.com/2021/01/four-jobs-data-scientist.html`](https://www.kdnuggets.com/2021/01/four-jobs-data-scientist.html)

评论

**由[罗杰·D·彭](http://www.biostat.jhsph.edu/~rpeng/)、[@jhubiostat](https://twitter.com/jhubiostat)教授；《R 编程数据科学》作者；[@simplystats](https://twitter.com/simplystats)**。

![data-scientist](img/8b75ef59eb455d02cc6bfb874b0dc877.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

2019 年，我写了一篇关于[数据科学的支柱](https://simplystatistics.org/2019/01/18/the-tentpoles-of-data-science/)的文章，尝试提炼数据科学家的关键技能。在文章中，我写道：

> 当我问自己“什么是数据科学？”时，我倾向于思考以下五个组成部分。数据科学是（1）将设计思维应用于数据问题；（2）创建和管理数据转换和处理的工作流程；（3）通过协商人际关系来识别背景、分配资源和描述数据分析产品的受众；（4）应用统计方法来量化证据；（5）将数据分析信息转化为连贯的叙述和故事。

我的观点是，如果你是一位优秀的数据科学家，那么你在数据科学的五个支柱方面都很出色。相反，如果你在所有五个支柱上都很擅长，那么你很可能是一位优秀的数据科学家。

我对这些技能的感觉还是一样的，但我现在的感觉是，这篇文章实际上让数据科学家的工作看起来比实际要简单。这是因为它将所有这些技能包裹成一个工作，而实际上，数据科学需要在**四个**工作中都表现出色。为了说明我的意思，我们必须退一步，问一个更基本的问题。

### 数据科学的核心是什么？

这是一个每个人都在问的问题，我认为也很难回答。任何领域中，总有一些问题的区分

+   这个领域的核心是什么？

+   那些人在该领域通常做些什么？

如果还不清楚，这些问题并不是一样的。例如，在统计学中，根据大多数博士项目的课程，领域的核心涉及统计方法、统计理论、概率论，也许还有一些计算。数据分析通常不会在正式课堂上教授（即在教室里），而是作为论文或研究项目的一部分来掌握。许多标记为“数据科学”或“数据分析”的课程只是教授更多的方法，如机器学习、聚类或降维。正式的软件工程技术通常也不会被教授，但在实践中往往会被使用。

可以说，数据分析和软件工程是统计学家*做*的事情，但这并不是该领域的核心。是否正确无关紧要。我只是想说，我们必须在某处做出区分。统计学家总是会*做*比该领域核心更多的工作。

在数据科学领域，我认为我们正在共同盘点数据科学家往往做什么。问题在于，目前看起来一片混乱。传统统计确实倾向于成为活动的核心，但计算机科学、软件工程、认知科学、伦理学、沟通等也一样。这几乎不能定义为领域的核心，而只是活动的枚举。

问题是，我们能否定义出*所有*数据科学家都在做的事情？如果我们必须教所有数据科学学生一些东西，而不知道他们未来可能去哪里，这会是什么呢？我认为在某个阶段，所有数据科学家都必须参与到*基本的数据分析迭代*中。

### 数据分析迭代

基本数据分析迭代分为四个部分。一旦问题确定并且获得或收集数据的计划可用，我们可以做以下事情：

1.  构建一个**预期结果集合**

1.  设计/构建/应用一个**数据分析系统**到数据上

1.  诊断分析系统输出中的**异常**

1.  做出一个**决策**关于下一步要做什么

虽然这个迭代对于许多人可能熟悉或显而易见，但这种熟悉掩盖了其中的复杂性。特别是，每一步迭代都需要数据科学家扮演不同的角色，涉及非常不同的技能。这就像是一场独角戏，其中数据科学家在从一个步骤到下一个步骤时需要换装。这就是我所说的*数据科学家的四个角色*。

### 四个角色

基本数据分析迭代的每一个步骤都要求数据科学家扮演四种不同的角色：(1) 科学家；(2) 统计学家；(3) 系统工程师；(4) 政治家。让我们更详细地了解每个角色。

**科学家**

科学家提出问题并负责了解科学的现状以及关键的知识缺口。科学家还设计任何收集新数据的实验并执行数据收集。科学家必须与统计学家合作，设计一个数据分析系统，并最终从任何数据分析中构建一个*预期结果集*。

科学家在开发一个能够产生我们预期结果集的系统中发挥着关键作用。这个系统的组件可能包括文献综述、元分析、初步数据或来自同事的轶事数据。我在这里广泛使用“科学家”这个术语。在其他环境中，这可能是政策制定者或产品经理。

**统计学家**

统计学家与科学家合作，设计分析系统来分析任何数据收集工作的生成数据。他们指定系统如何运行，生成什么输出，并获取实现系统所需的资源。统计学家利用统计理论和个人经验来选择将要应用的分析系统的不同组件。

统计学家在这里的角色是将数据分析系统应用于数据并生成数据分析输出。该输出可以是回归系数、均值、图表或预测。但必须生成某种可以与我们的预期结果集进行比较的东西。如果输出偏离了我们的预期结果集，那么接下来的任务是确定这种偏差的原因。

**系统工程师**

一旦将分析系统应用于数据，只有两种可能的结果：

1.  输出符合我们的期望，或者

1.  输出不符合我们的期望（异常）。

在出现异常的情况下，系统工程师的责任是基于对数据收集过程、分析系统和科学知识状态的了解，诊断异常的潜在根本原因。

最近，Emma Vitz [在推特上写道](https://twitter.com/EmmaVitz/status/1330697959156027392?s=20)：

> 如何向初级人员教授调试技能？我觉得这是一项非常重要的技能，但似乎我们没有结构化的教学框架。

对于软件和数据分析来说，挑战在于错误或意外行为可能来自任何地方。任何复杂的系统由多个组件组成，其中一些可能是你的责任，而许多则是他人的。但错误和异常不会尊重这些边界！可能会有一个在一个组件中发生的问题，只有在你查看数据分析输出时才会被发现。

因此，如果你负责诊断问题，那么你有责任调查系统中每个组件的行为。如果这是你不太熟悉的内容，那么你需要*变得*熟悉它，要么通过自学，要么（更可能地）与实际负责的人交谈。

数据分析输出中常见的意外行为源于数据收集过程，但分析数据的统计学家可能不负责这一方面。尽管如此，识别异常的系统工程师必须回过头来，与统计学家和科学家沟通，以搞清楚每个组件的工作原理。

最终，系统工程师的任务是全面审视所有影响数据分析输出的活动，以识别任何偏差。一旦这些根本原因被解释清楚，我们就可以决定如何处理这些新信息。

**政治家**

政治家的工作是做出决策，同时平衡各方利益，以实现合理的结果。我认识的大多数统计学家和科学家会对被认为是政治家或认为政治会在任何形式的科学中发挥作用感到反感。然而，我的想法更基础：在任何数据分析迭代中，我们都在不断地做出关于该做什么的决策，同时考虑各种冲突因素。为了化解这些冲突并达成合理的协议，必须具备一种关键技能，即谈判。

在数据分析迭代的各个阶段，政治家必须就以下方面进行谈判：（1）分析中成功的定义；（2）执行分析的资源；以及（3）在看到分析系统的输出并诊断出任何异常的根本原因后，决定下一步该做什么。关于下一步该做什么的决定从根本上涉及数据和科学之外的因素。

政治家必须确定问题的利益相关者是谁，以及他们究竟*想要*什么（而不是他们的*立场*是什么）。例如，一位研究人员可能会说：“我们需要 p 值小于 0.05。”那是他们的立场。但他们*想要*的更可能是“在高影响力期刊上发表文章。”另一个例子可能是一个研究人员需要赶上紧迫的发表截止日期，而另一个研究人员则想进行耗时（但更可靠）的分析。显然，立场相冲突，但可以说，两位研究人员共享相同的目标，即严格的高影响力出版物。

识别职位与潜在需求是谈判合理结果的关键任务。根据我的经验，这一过程很少涉及数据（尽管数据可能用于某些论证）。这一过程的主导因素往往是各方之间的关系性质和资源的限制（如时间）。

### 应用迭代

如果你在阅读此内容时发现自己说“我不是 X”，其中 X 是科学家、统计学家、系统工程师或政治家，那么很可能你在数据科学方面存在不足。我认为一个好的数据科学家必须在这些领域中具备一些技能，以完成基本的数据分析迭代。

在任何给定的分析中，迭代可以从一次应用到几十次甚至几百次。如果你曾经制作过同一数据集的两个图，那么你可能已经进行了两次迭代。尽管迭代的具体细节和频率在应用中可能会有所不同，但核心性质和涉及的技能变化不大。

[原文](https://simplystatistics.org/2020/11/24/the-four-jobs-of-the-data-scientist/)。经许可转载。

**相关：**

+   [没有人谈论的核心数据科学技能](https://www.kdnuggets.com/2020/11/essential-data-science-skills-no-one-talks-about.html)

+   [现代数据科学技能：8 类技能、核心技能和热门技能](https://www.kdnuggets.com/2020/09/modern-data-science-skills.html)

+   [这些数据科学技能将成为你的超级力量](https://www.kdnuggets.com/2020/08/data-science-skills-superpower.html)

### 更多相关话题

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每位数据科学家都应了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目的，并寻找目的以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [90 亿美元的 AI 失败，经过审视](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么使 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
