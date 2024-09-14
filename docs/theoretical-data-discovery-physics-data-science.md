# 理论数据发现：使用物理学来理解数据科学

> 原文：[https://www.kdnuggets.com/2016/07/theoretical-data-discovery-physics-data-science.html](https://www.kdnuggets.com/2016/07/theoretical-data-discovery-physics-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Sevak Avakians，Intelligent Artifacts**。

尽管“数据科学”是一个最近才创造的流行词，但实际上它并不是一个新兴的研究领域。其背后的许多科学原理是物理学家和数学家在过去两个世纪里为解决其他问题而发展起来的（如果你需要数据科学家，雇用一个物理学家吧！）。今天，对数据科学的需求已超越了物理实验室，但其科学原理依旧相同。我们可以继续利用物理学家提供的答案来解决新领域中的问题。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

![量子纠缠](../Images/c2a34edb5832d5a4162af82a921f0a7d.png)

例如，最近我被问到一个非常发人深省的问题，关于[Intelligent Artifacts' Genie Cognitive Computer](http://www.intelligent-artifacts.com/)：

> “……它能否检测数据集中是否缺少了某些关键内容？是否有我应该收集但没有收集的数据？例如，它能否检测到我确实需要一个用于预测的上下文信号，而我遗漏了记录，并告诉我需要开始测量它？或者它能否以合理的信心对缺失信息进行建模？” 

我的初步回应是：

> 不，我目前认为对于任何系统而言这是不可能的。这就像那句俗话，“你不知道你不知道什么。” 从信息理论的角度来看，我不认为有任何方法可以检测数据集中是否缺少关键信息。也许，一种方法是自动化的数据源发现和测试。也就是说，一些算法会寻找不同的数据源，并将其-也许是暂时-纳入数据流中。然后，它（即 Genie's 认知处理器和/或信息分析器）会分析结果。到某个时刻，它可能会发现关键数据。但对于原始问题，我不相信任何代理能够知道是否缺少关键数据，尤其是当这些数据从未被呈现给系统时。

几个晚上后，当我凝视着天花板思考量子力学中的问题时，我突然意识到这个问题在物理学中曾经出现过！具体来说，这个问题涉及量子力学（QM）的非确定性本质，这导致了由阿尔伯特·爱因斯坦描述为“鬼魅般的远距离作用”的纠缠现象。

第一个问题（对我们今天的数据科学来说类似）是相同的过去/当前事件序列，即当前状态，将分叉成多个可能的未来结果。这意味着量子力学的预测不像牛顿力学那样是确定的。

德国物理学家马克斯·玻恩（小插曲：他的孙女是奥莉维亚·纽顿-约翰）开始将量子力学看作是一种统计解释。（这与我们对精灵预测的处理是一致的。根据当前状态，我们通过回顾过去那些在当前状态之后成功的未来状态的频率，来计算未来状态的概率。）这让包括阿尔伯特·爱因斯坦在内的许多人感到非常不安，促使他发表了著名的言论：“上帝不会掷骰子”。为了反驳这一解释，爱因斯坦、波多尔斯基和罗森提出了[EPR思想实验](https://en.wikipedia.org/wiki/EPR_paradox)作为一种悖论，该实验向世界介绍了纠缠现象，即允许粒子瞬间相关其状态的现象，似乎违反了因果关系和光速限制。结果表明，这根本不是悖论。自然确实以这种方式运作！

为了挽救决定论，美国物理学家大卫·玻姆提出了一个想法，即结果之所以如此表现，是因为存在未知的“[局部隐变量](https://en.wikipedia.org/wiki/Local_hidden_variable_theory)”，这些隐变量如果被考虑在内，将会使得预测变得确定。约翰·贝尔（IA咨询委员会主席大卫·麦戈万的朋友）提出了一种[不等式检验](https://en.wikipedia.org/wiki/Bell%27s_theorem)，这一检验被[实验者](https://en.wikipedia.org/wiki/Bell_test_experiments)（如阿兰·阿斯佩等）使用，证明了不存在局部隐变量。

好吧，对我们来说关键在于，这种测试理论上可以对我们的数据集进行，以证明或驳斥任何对预测重要的未知变量。

量子力学与Genie预测之间的区别在于，量子力学依赖于高度约束的严密数学模型。而Genie则依赖于已经观察到的数据。前者允许将模型与数据进行比较，或者像在贝尔不等式中那样，将量子力学模型与确定性“局部隐藏变量”模型进行比较。由于我们设计Genie时不需要数据模型，因此其预测的优势来自于将新数据与过去数据进行比较。此外，它需要一个确定性模型进行比较，而Genie预测则不需要。

这让我回到了最初的结论；无法检测到尚未输入系统的数据。

只要我们花时间将科学探究应用于这些新领域，数据科学将继续受益于物理学家在知识边界上推进的工作。

作者感谢Peter Olausson和David McGoveran在这个问题上进行的启发性讨论。

**简介：[Sevak Avakians](https://www.linkedin.com/in/avakians)** 拥有物理学、电信、信息论和人工智能背景。2008年，Sevak发明了一个名为GAIuS的AGI框架。Genie建立在GAIuS之上，并通过他创办的公司[Intelligent Artifacts](http://www.intelligent-artifacts.com/)提供。

**相关内容：**

+   [娱乐与利润中的人工智能：使用新的Genie认知计算平台进行P2P借贷](/2016/07/ai-fun-profit-genie-cognitive-computing.html)

+   [从研究到财富：物理和生命科学中的数据处理课程](/2016/06/salford-data-wrangling-lessons-physical-life-science.html)

+   [采访：Arno Candel，H2O.ai 从物理学到机器学习的旅程](/2015/01/interview-arno-candel-h2o-physics-machine-learning.html)

### 相关阅读

+   [使用SQL了解数据科学职业趋势](https://www.kdnuggets.com/using-sql-to-understand-data-science-career-trends)

+   [图表：理解数据的自然方式](https://www.kdnuggets.com/2022/10/manning-graphs-natural-way-understand-data.html)

+   [理解机器学习的24本最佳（且免费）书籍](https://www.kdnuggets.com/2020/03/24-best-free-books-understand-machine-learning.html)

+   [选择示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [黑客如何利用数据科学盗取数十亿](https://www.kdnuggets.com/2022/02/4-ways-hackers-data-science-steal-billions.html)

+   [利用数据科学让清洁能源更加公平](https://www.kdnuggets.com/2022/03/data-science-make-clean-energy-equitable.html)
