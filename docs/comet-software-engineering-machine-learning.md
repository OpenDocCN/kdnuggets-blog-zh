# 为什么软件工程流程和工具不适用于机器学习

> 原文：[https://www.kdnuggets.com/2019/12/comet-software-engineering-machine-learning.html](https://www.kdnuggets.com/2019/12/comet-software-engineering-machine-learning.html)

赞助帖子。

**由 [Nikolas Laskaris](https://www.comet.ml/blog/?author=7), [Comet.ml](https://www.comet.ml/)**

“人工智能是新的电力。”至少，这就是[Andrew Ng](https://www.coursera.org/instructor/andrewng)在今年的[Amazon re:MARS](https://remars.amazon.com/)大会上所提出的观点。在他的[主题演讲](https://www.youtube.com/watch?v=j2nGxw8sKYU)中，Ng讨论了人工智能（AI）的快速增长——它不断渗透到各个行业；每天头条中对AI突破、技术或恐惧的持续关注；来自寻求现代化的既有企业（例如：[Sony](https://www.engadget.com/2019/11/19/sony-ai/)）以及从风险投资者在AI领域创始人潮流中进入市场的大量投资。

“人工智能是下一个重大变革，”Ng坚持说，我们正在见证这一变革的展开。

尽管人工智能可能是新的电力（作为[Comet](http://comet.ml/)的数据科学家，我不需要太多说服），但这一领域要实现其潜力仍面临重大挑战。**在这篇博客文章中，我将讨论为什么数据科学家和团队不能依赖软件工程团队过去20年用于机器学习** **(ML)的工具和流程。**

依赖软件工程的工具和流程是合理的——数据科学和软件工程都是以*代码*为主要工具的学科。然而，*数据科学团队正在做的*与软件工程团队正在做的有着根本的不同。检查这两个学科之间的核心差异，有助于明确我们应该如何构建工具和流程以进行人工智能。

在Comet，我们相信，采用专为人工智能设计的工具和流程将帮助从业者解锁并实现Ng所说的那种革命性转变。

### 不同学科，不同流程

软件工程是一个学科，其目标是广泛地设计和实现计算机可以执行以完成定义功能的程序。假设软件程序的输入在预期（或受限）范围内，其行为是可知的。在2015年ICML的一个[talk](https://leon.bottou.org/talks/2challenges)中，Leon Bottou很好地阐述了这一点：在软件工程中，算法或程序可以被证明是*正确的*，意味着在给定特定输入假设的情况下，某些属性在算法或程序终止时将是正确的。

![图](../Images/17ed958cac240f480f66b7d87050da24.png)

来源：[Futurice](https://www.futurice.com/blog/differences-between-machine-learning-and-software-engineering/)

可证明的程序正确性已经塑造了我们在软件工程中使用的工具和流程。考虑一下从可证明正确性中衍生出的一个相关特性：如果一个程序对某些输入值是可证明正确的，那么程序中包含的子程序对于这些输入值也同样是可证明正确的。这就是为什么像[敏捷](https://en.wikipedia.org/wiki/Scaled_agile_framework)这样的工程流程在软件团队中通常是成功和高效的。将这些项目拆解为子任务是有效的。大多数[瀑布模型](https://en.wikipedia.org/wiki/Waterfall_model)和[scrum](https://en.wikipedia.org/wiki/Scrum_(software_development))实现也包括了子任务的划分。

![](../Images/75ab89ff047a322946c1c55059825837.png)

我们看到许多数据科学团队使用与这些软件方法完全相同或大致相似的工作流程。不幸的是，这些方法效果并不好。原因是什么？软件工程的可证明正确性并没有扩展到人工智能和机器学习中。在（监督）机器学习中，对于我们建立的模型，唯一的保证是，如果训练集是某个分布的[iid](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)（独立同分布）样本，那么来自同一分布的另一个iid样本上的性能将会*接近*训练集上的性能。由于不确定性是机器学习的固有特性，子任务划分可能导致不可预见的下游影响。

### **为什么不确定性是机器学习的固有特性？**

部分答案在于这样一个事实：那些既对我们有趣又适合机器学习解决方案（例如自动驾驶汽车、物体识别、图像标注和生成语言模型等）的难题，并没有明确可重复的数学或程序规范。作为规范的替代，机器学习系统通过输入大量数据来检测模式并生成预测。换句话说，*机器学习的目的是创建一个统计代理，它可以作为这些任务之一的规范*。我们希望我们收集的数据是现实世界分布的一个代表性子样本，但实际上我们无法确切知道这个条件被满足的程度。最后，我们使用的算法和模型架构是复杂的，足够复杂到我们无法总是将它们拆解成子模型以准确理解发生了什么。

从这个描述中，机器学习系统的*可知性*障碍应该是显而易见的。机器学习所适用的问题类型固有地缺乏明确的数学规范。在没有规范的情况下，我们使用的统计代理是积累大量我们*希望*是独立同分布且具有代表性的环境数据。我们用来从这些收集的数据中提取模式的模型足够复杂，以至于我们无法可靠地拆解它们并准确理解它们的工作原理。Comet的同事Dhruv Nair写了一系列关于机器学习不确定性的三部分系列文章（这是[第一部分](https://www.comet.ml/blog/?p=662)的链接），如果你想深入探讨这个话题。

然后，考虑一下类似于在机器学习项目上使用的敏捷方法的影响。我们不可能希望将机器学习任务拆解成*子任务*，作为某个更大冲刺的一部分来处理，然后像乐高一样拼凑成一个整体产品、平台或功能，因为我们无法可靠地预测子模型或模型本身的功能。

![图](../Images/f67a036a08977e95aabdbd6e468dcaa3.png)

来源：[Youtube](https://www.youtube.com/watch?v=j2nGxw8sKYU)

Ng在re:MARS上也讨论了这个话题。他透露了他的团队如何采用专为ML设计的工作流系统：**1天冲刺**，结构如下：

1.  每天构建模型并编写代码

1.  设置训练并进行过夜实验

1.  早晨分析结果，然后…

1.  重复

Ng的1天冲刺方法反映了理解和设计从事机器学习的团队的一个关键点：它本质上是**实验性科学**。由于所构建的系统缺乏明确的规范，因为数据收集是一门不完美的科学，并且机器学习模型极其复杂，*实验是必要的*。与其围绕多周冲刺来组织团队流程，不如快速测试多种不同的架构、特征工程选择和优化方法，直到出现一个大致的有效和无效的图景。1天冲刺使团队能够快速移动，短时间内测试许多假设，并开始在建模任务上建立直觉和知识。

### **机器学习工具：实验管理**

假设你采用Andrew Ng的1天冲刺方法或类似的东西（*你应该这样做*）。你正在设置新的超参数，调整特征选择，并每晚运行实验。你使用什么工具来跟踪这些决策以进行每次模型训练？你如何比较实验以查看不同配置的效果？你如何与同事共享实验？你的经理或同事能否可靠地重现你昨天进行的实验？

除了过程之外，你所使用的机器学习工具也很重要。在Comet，我们的使命是通过提供一个能够为你完成这些工作的工具，帮助公司从机器学习中提取商业价值。我们与许多数据科学团队交流时发现，他们仍然依赖于git、电子邮件以及（信不信由你）电子表格来记录每次实验的所有成果。

![图示](../Images/29a1a4003262165cf06abd6618d49848.png)

[Comet](http://www.comet.ml/): 20多个实验的超参数空间可视化

考虑一个建模任务，其中你需要跟踪20个超参数、10个指标、数十种架构和特征工程技术，同时快速迭代，每天运行数十个模型。手动跟踪所有这些成果可能变得极为繁琐。构建一个好的机器学习模型有时就像用50个旋钮调节收音机一样。如果你不记录所有尝试过的配置，找到建模空间中的信号的组合复杂度可能会变得非常繁琐。

![图示](../Images/e38516c338b1ac6067b123f85e705641.png)

[Comet](http://www.comet.ml/): 单次实验的实时指标跟踪和仪表板

我们在构建Comet时考虑了这些需求（以及当我们在谷歌、IBM以及哥伦比亚大学和耶鲁大学的研究团队中从事数据科学和机器学习工作时的需求）。每次训练模型时，都应该有*某种工具*来捕捉你实验的所有成果，并将它们保存在某个中央账本中，以便你可以查看、比较和筛选你（或你的团队）的所有工作。Comet的设计就是为了提供这一功能给机器学习从业者。

衡量工作流程效率是一个[ notoriously difficult](https://gravityflow.io/articles/measure-workflow-automations-roi/)的任务，但我们的用户平均报告称，通过使用Comet节省了*20-30%的时间*（注意：Comet对个人和研究人员免费—[你可以在这里注册](https://www.comet.ml/pricing?opensignup=true&utm_source=Software%20Eng%20vs%20ML&utm_medium=Blog&utm_campaign=Software%20Eng%20vs%20ML%20Blog%20Post)）。这还不包括从对超参数空间的可视化理解、实时指标跟踪、团队协作和实验比较等方面获得的独特见解和知识。这些知识不仅节省时间，而且更重要的是，使得*构建更好的模型*成为可能。

### **展望未来**

忽视有关机器学习工具和过程的问题是很有诱惑力的。在一个负责自动驾驶汽车、语音助手、人脸识别以及许多其他突破性技术的领域中，人们可能会被允许直接投入到构建这些工具的工作中，而不考虑如何最好地构建它们。

如果你相信软件工程技术栈对 AI 的应用*足够好*，你不会被彻底证明对错。毕竟，这是一个充满不确定性的领域。但或许最好像数据科学家看待建模任务一样来考虑这个问题：可能未来的概率分布是什么？什么更可能或不太可能？**一个如此强大且充满潜力的领域，是否会继续依赖为不同学科构建的工具和流程，还是会有新的工具出现，以充分赋能从业者？**

*如果你对这些机器学习工具感兴趣或有任何问题，欢迎通过*[*niko@comet.ml*](mailto:niko@comet.ml)*联系我。*

**额外阅读**

关于机器学习与软件工程差异的博客：

1.  [Futurice 关于机器学习与软件工程的博客](https://www.futurice.com/blog/differences-between-machine-learning-and-software-engineering/)

1.  [KDnuggets 关于机器学习与软件工程的博客](/2017/03/software-engineering-vs-machine-learning-concepts.html)

1.  [Concur Labs 关于机器学习与软件工程的博客](https://blog.concurlabs.com/10-differences-between-ml-and-software-development-part-1-3e21a83094d1)

1.  [微软关于构建机器学习团队流程的案例研究](https://www.microsoft.com/en-us/research/uploads/prod/2019/03/amershi-icse-2019_Software_Engineering_for_Machine_Learning.pdf)

1.  [Leon Bottou 2015年 ICML 演讲幻灯片](https://icml.cc/2015/invited/LeonBottouICML2015.pdf)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

### 了解更多相关主题

+   [ChatGPT 是如何工作的，为什么它有效？](https://www.kdnuggets.com/2023/04/chatgpt-work.html)

+   [5 种自动化数据清理过程的工具](https://www.kdnuggets.com/5-tools-for-automating-data-cleaning-processes)

+   [软件开发者与软件工程师](https://www.kdnuggets.com/2022/05/software-developer-software-engineer.html)

+   [机器学习教科书：随机过程与模拟](https://www.kdnuggets.com/2022/03/datashaping-machine-learning-textbook-stochastic-processes-simulations.html)

+   [别错过！在 2023 年结束前注册免费课程](https://www.kdnuggets.com/dont-miss-out-enroll-in-free-courses-before-2023-ends)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)
