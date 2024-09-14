# 如何为您的数据项目创建采样计划

> 原文：[https://www.kdnuggets.com/2022/11/create-sampling-plan-data-project.html](https://www.kdnuggets.com/2022/11/create-sampling-plan-data-project.html)

在[第1部分](http://bit.ly/quaesita_srstrees1)中，我们看到简单随机抽样（SRS）在实际操作中并不总是*简单*。

# 截至目前的故事

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的IT

* * *

想象一下，你是一名[data scientist](http://bit.ly/quaesita_datascim)，被聘用来[估计](http://bit.ly/quaesita_vocab)下图中松树的平均高度，并描述其[分布](http://bit.ly/quaesita_distributions)。你负责规划和分析，因此你可以在舒适的巢穴中操控一切，而一位热心的登山者会按照你的指示在森林中测量树木。

你绝对不会做的事情是指示登山者***“完全随机选择20棵树。”***（如果你有这个冲动，请回到[第1部分](http://bit.ly/quaesita_srstrees1)。）

![如何为您的数据项目创建采样计划](../Images/4244951c88ebf6efbeec2414d61b86aa.png)

这片森林中的树木有多高？照片由[Dan Otis](https://unsplash.com/@danotis?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

相反，始终努力给出万无一失的指令，因为你永远不知道何时会出现一个野生的傻瓜。

> 始终努力给出万无一失的指令，因为你永远不知道何时会出现一个野生的傻瓜。

# 万无一失的指令

当我挑战我的学生给我更好的——更万无一失的——指令时，团队通常会提出老生常谈的建议：

*“为每棵树分配一个唯一的编号，然后随机抽样这些编号。”*

这已经更好了——至少不再包含“仅仅”这个词——但它仍然不是专业统计学家的级别答案。专业[统计学家](http://bit.ly/quaesita_statistics)经历了足够多的挫折，知道细节决定成败。

![如何为您的数据项目创建采样计划](../Images/de57def84823e66a82596f5812ca0725.png)

照片由[Sebastian Herrmann](https://unsplash.com/@officestock?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 魔鬼在于（现实世界的）细节

在专业统计学家的思维中，那些看似无害的指令迅速演变成一整堆细节：

> “亲爱的徒步旅行者，我提前向你道歉，但你必须一次走完整个森林，给所有树木编号……这就是为什么你如此喜欢徒步旅行！”

在我们甚至还没说完之前，我们的脑子已经在飞速运转：

*用什么油漆？*（写下来！）

*等一下，我们可能需要多少桶油漆？*（写下来！）

*如果油漆用完了怎么办（因为我们不知道森林中有多少棵树）。*（写下来！）

*我们可以在哪里购买更多的油漆桶？离这里最近的城镇在哪里？*（写下来！）

*在我们工作期间，在哪里存放桶会更有效率？*（写下来！）

*我们如何确保合理标记这些树木，以便在抽到它们时能高效找到它们？*（写下来！）

*我们需要一个手推车吗？或者一些助手？*（写下来！）

*我们如何确保没有遗漏任何一棵树？*（写下来！）

*我们预计这将花费多长时间？*（写下来！）

*如果徒步旅行者在项目中途退出怎么办？*（写下来！）

*如果下雨了，油漆脱落怎么办？*（写下来！）

*这所有的费用是多少？*（写下来！）

*如果计划比预期更昂贵，我们的应对计划是什么？*（写下来！）

*我们的预算是多少？*（写下来！）

# 替代抽样方案

啊，预算。随着你开始详细规划，你可能会发现你的理想方法不可行。在这种情况下，你有两个选择。

1.  **选择不同的抽样程序。**[SRS](http://bit.ly/quaesita_srstrees1) 并不是你唯一的选择，它甚至不一定是对你的需求最优的统计方法。它的主要优势是允许你使用初学者技术，但如果你发现实施SRS在现实中比雇用一个懂得高级方法的人更昂贵，那么这就是一个很好的选择。只要记住，使用其他抽样程序意味着你***必须***以不同于STAT101教给你的标准方式来分析数据（除非你选择下面的选项2）。

1.  [**假设**](http://bit.ly/quaesita_saddest)** 远离问题。** 这是经典的绝望中的权宜之计，你会在 [科学](http://bit.ly/quaesita_scientists)领域到处发现。这基本上就是说，你将进行分析，就好像某些事情是真的，即使你知道它们并非如此。这是哲学上不稳定的领域，只有决策者有权批准这样做，他可能会愿意接受一个低质量的决策程序来节省资源。如果你不是做 [数据驱动决策](http://bit.ly/quaesita_inspired)的负责人，你没有权利做出这个决定。

但让我们设想一下，你的徒步旅行者非常渴望在森林里呆得越久越好（哇），以至于他们给你一个固定的项目费率。现在你可以负担得起让这个热爱户外的狂热者为你涂上*所有*的树木。是时候开始规划下一步：随机选择20棵树。

我们如何“仅仅”选择20棵树？一种简单的方法是，等到所有的树都被涂上独特的ID，然后把所有的树木ID放入一个电子表格的列中，拖动=RAND()函数到相邻的列中，并按随机数列的顺序排序，然后选择顶部的前20个ID。

现在我们对更多细节感到担忧：

*标签应该是整数还是其他什么？*（写下来！）

*如果它们很难阅读怎么办？*（写下来！）

*我们确定随机数生成器是随机的吗？*（写下来！）

*如果我们忘记仅以值粘贴特殊怎么办？*（写下来！）

*我们计划带一台笔记本电脑去森林吗？哪一台？*（写下来！）

*如果没有Wi-Fi，软件会正常工作吗？*（写下来！）

*笔记本电脑的电池能坚持多久？*（写下来！）

*我们需要备用笔记本电脑吗？*（写下来！）

一旦我们有了20个随机选择的树木ID——保存在哪里？（写下来！）——是时候向徒步旅行者道歉，可能需要再次在森林中徒步旅行。希望我们已经想出了一个高效的徒步策略来节省时间，因为可怜的徒步旅行者已经精疲力竭了。不过，还有更多的事情！

![如何为数据项目创建采样计划](../Images/0c1905d5226421a4411cbdaa8f478eea.png)

来自维基百科的高程测量高程计的插图：[https://en.wikipedia.org/wiki/Hypsometer](https://en.wikipedia.org/wiki/Hypsometer)

*徒步旅行者将如何进行测量？*（写下来！）

*我们应该使用什么工具？一个[高程计](https://bigtrees.forestry.ubc.ca/measuring-trees/height-measurements/)？*（写下来！）

*我们如何确保徒步旅行者经过正确的培训以正确使用设备？*（写下来！）

*如果树木在[坡道上或被遮挡](https://bigtrees.forestry.ubc.ca/measuring-trees/height-measurements/)，我们应该怎么办？*（写下来！）

*我们可能需要什么额外的设备？*（写下来！）

*测量结果将记录在哪里？纸上？笔记本电脑上？手机上？我们是否计划好如何携带这些数据，以及如果出现故障该怎么办？*（写下来！）

徒步旅行者是否明白测量结果应该记录在树木ID旁边？

树高是徒步旅行者应该测量和记录的关于每棵树的唯一内容吗？当然，还有一些额外的信息可能有用且相对容易获取，包括时间、树木的物理特征、距离徒步旅行者起点的距离、在一个对阳光和土壤营养竞争相关的半径内的其他树木数量（那个半径是多少？）、树木的周长等等。（写下来！但如果你像我一样对树木了解甚少，请先咨询领域专家。）

*如果我们为每个 ID 记录一堆额外的属性，它们应该如何记录？徒步旅行者应该使用什么数据模式？*（写下来！）

*我们如何检查错误？徒步旅行者是否需要进行额外的检查？*（写下来！）

*我们如何验证徒步旅行者是否正确地遵循了指示？*（写下来！）

*对于我们记录的每一个属性，[单位或类别](http://bit.ly/quaesita_datatypes)* 应该由徒步旅行者使用？（写下来！）

在结束之前，我们会通读我们的文档，补充更多细节。如果你草率地编写详细说明，你会因为数据收集员带回来的混乱（因为它不会是你想要的）而恨他们。而你的数据收集员也会正当地*恨你*，因为你给了他们愚蠢的指示，还指望他们读懂你的心思。

有一天，数据收集员将会是你……然后你会恨自己。

> 抽样计划涵盖了数据收集的现实世界实际方面。

这就是我们在工作中艰难学习的方式。多年来我所养成的数据卫生习惯，源于对过去自己糟糕规划的愤怒。年轻时我主要关注数学。时光流逝后的我对现实世界的细节更感兴趣。我希望你能从我的错误中学习，这样你就不必重蹈覆辙。

请答应我，你不会再指示别人“只”做简单的随机抽样。

![如何为您的数据项目创建抽样计划](../Images/3256d486cd84ca1ef452a6ce6216b3aa.png)

图片来源于 [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 抽样计划

当你对细节足够担忧之后（写下来！），你面前的文档被称为**抽样计划**。

抽样计划涵盖了数据收集的现实世界实际方面，不使用它来记录现实世界的数据是极其业余的。如果你尝试跳过规划，你将面临一场非常昂贵的彩排，为即将到来的重新审视做好准备。

# 抽样方案

现在你已经写下了理论部分（抽样程序）和实践部分（抽样计划），你准备将它们合并成一个名为**抽样方案**的单一文档。

> 抽样计划 + 抽样程序 = 抽样方案

顺便提一下，当你从供应商那里购买数据时，应该始终要求提供完整的抽样方案（包括计划，而不仅仅是程序），以确保你理解你的数据实际含义。有关处理二级（继承的）数据的更多技巧，包括购买的数据，请查看我的指南 [这里](http://bit.ly/quaesita_notyours)。

一个真正的专业人士在创建完整的抽样方案之前不会梦想到收集数据。不幸的是，由于数据课程的教学方式——理论多，实践少——往往需要一段时间才能让刚毕业的小白变成真正的专业人士。我们这些在行业中待久了的人可以通过为实践方面的注意加油来发挥作用。毕竟，如果细节都模糊不清，那所有的复杂数学又有什么意义呢？

# 感谢阅读！如何参加 AI 课程？

如果你在这里玩得开心，且你正在寻找一个为初学者和专家设计的有趣应用 AI 课程，这里有一个我为你准备的：

在这里享受整个课程播放列表： [bit.ly/machinefriend](http://bit.ly/machinefriend)

**[Cassie Kozyrkov](https://kozyrkov.medium.com/)** 是谷歌的数据科学家和领导者，致力于将决策智能和安全可靠的 AI 民主化。

[原文](https://towardsdatascience.com/how-to-create-a-sampling-plan-for-your-data-project-3b14bfd81f3a)。经许可转载。

### 更多相关内容

+   [学习数据科学、机器学习和深度学习的稳固计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [用你的数据科学技能创造 5 种收入来源](https://www.kdnuggets.com/2023/03/data-science-skills-create-5-streams-income.html)

+   [从零到英雄：使用 PyTorch 创建你的第一个 ML 模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)

+   [使用 Tableau 创建高效的组合数据源](https://www.kdnuggets.com/2022/05/create-efficient-combined-data-sources-tableau.html)

+   [构建数据管道以使用大型语言模型创建应用程序](https://www.kdnuggets.com/building-data-pipelines-to-create-apps-with-large-language-models)

+   [使用 ChatGPT 创建惊艳的数据可视化](https://www.kdnuggets.com/create-stunning-data-viz-in-seconds-with-chatgpt)
