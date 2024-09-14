# DeepMind 正在使用这种旧技术来评估机器学习模型的公平性

> 原文：[https://www.kdnuggets.com/2019/10/deepmind-using-old-technique-evaluate-fairness-machine-learning-models.html](https://www.kdnuggets.com/2019/10/deepmind-using-old-technique-evaluate-fairness-machine-learning-models.html)

[评论](#comments)![图](../Images/09293a7813510fd0c0f8ad29659a6c90.png)

支持机器学习系统的一个常见论点是，它们能够在不受人为主观性影响的情况下做出决策。然而，这个论点只是部分正确。虽然机器学习系统不会基于情感或感觉做出决策，但它们确实通过训练数据集继承了很多人类偏见。偏见之所以重要，是因为它会导致不公平。在过去几年里，开发能够减轻偏见影响并提高机器学习系统公平性的技术取得了很大进展。最近，[DeepMind 发布了一篇研究论文，提出使用一种叫做因果贝叶斯网络（CBN）的旧统计技术来构建更公平的机器学习系统](https://arxiv.org/abs/1907.06430)。

我们如何在机器学习系统的背景下定义公平性？人们通常根据主观标准定义公平性。在机器学习模型的背景下，公平性可以表示为敏感属性（如种族、性别等）与模型输出之间的关系。虽然这个定义在方向上是正确的，但它并不完整，因为在评估公平性时必须考虑模型的数据生成策略。大多数公平性定义表达了模型输出相对于敏感信息的特性，而没有考虑数据生成机制中相关变量之间的关系。由于不同的关系会要求模型满足不同的属性才能被认为是公平的，这可能导致错误地将表现出不良/合法偏见的模型分类为公平/不公平。从这个角度来看，识别数据生成机制中的不公平路径与理解模型本身同样重要。

理解机器学习模型中公平性分析的另一个相关点是，它的特征超越了技术构造，通常涉及社会学概念。从这个意义上说，可视化数据集是识别潜在偏见和不公平的重要组成部分。在市场上的不同框架中，DeepMind 依赖于一种叫做 [因果](https://www.cambridge.org/gb/academic/subjects/philosophy/philosophy-science/causality?format=HB) [贝叶斯](https://pdfs.semanticscholar.org/c4bc/ad0bb58091ecf9204ddb5db7dce749b0d461.pdf) [网络](https://mitpress.mit.edu/books/causation-prediction-and-search-second-edition)（CBNs）的方法来表示和估计数据集中的不公平性。

### 因果贝叶斯网络作为不公平性的视觉表示

因果贝叶斯网络（CBNs）是一种用于使用图结构表示因果关系的统计技术。从概念上讲，CBN 是由表示随机变量的节点组成的图，这些节点通过表示因果影响的链接连接。DeepMind 方法的新颖之处在于使用 CBNs 来建模数据集中的不公平属性的影响。通过将不公平性定义为图中来自敏感属性的有害影响的存在，CBNs 提供了一种简单直观的视觉表示，用于描述数据集中的不同可能的不公平情景。此外，CBNs 为我们提供了一种强大的定量工具，用于测量数据集中的不公平性，并帮助研究人员开发解决该问题的技术。

CBN 的一个更正式的数学定义是一个由节点组成的图，这些节点代表个别变量，并通过因果关系连接。在 CBN 结构中，从节点 X 到节点 Z 的路径定义为从 X 开始到 Z 结束的一系列连接节点。如果存在从 X 到 Z 的因果路径，即路径中的连接指向序列中接下来的节点，那么 X 是 Z 的原因（对 Z 有影响）。

让我们在一个著名的统计案例研究的背景下说明 CBNs。1975 年，伯克利大学的一组研究人员发表了有关统计中的偏见和不公平性的一项著名研究。该研究基于一个大学录取场景，其中申请者根据资格 Q、部门选择 D 和性别 G 被录取；女性申请者更常申请某些部门（为了简单起见，我们将性别视为二元的，但这并不是框架强加的必要限制）。将该场景建模为 CBN，我们得到以下结构。在该图中，路径 G→D→A 是因果的，而路径 G→D→A←Q 是非因果的。

![](../Images/82339756cc1d3143d59d3518fc7e3be0.png)

### CBNs 和不公平性

CBNs 如何帮助确定数据集中不公平性的因果表示？我们的大学录取示例清楚地展示了如何将不公平关系建模为 CBN 中的路径。然而，虽然 CBN 可以清晰地测量直接路径中的不公平性，但间接因果关系高度依赖于上下文因素。例如，考虑我们可以评估不公平性的大学的以下三种变体。在这些示例中，红色路径总或部分用于指示不公平和部分不公平的链接。

第一个示例说明了一个情景，其中女性申请者自愿申请接受率低的部门，因此路径 G→D 被认为是公平的。

![](../Images/f564e3045b608fb200dc8d598bfd3a7d.png)

现在，考虑前一个例子的一个变体，其中女性申请者由于系统性历史或文化压力申请接受率较低的部门，因此路径 G→D 被认为是不公平的（因此，路径 D→A 成为部分不公平）。

![](../Images/23e8ed2f4a7618cbc667d3a6495bce89.png)

继续在上下文游戏中，如果我们的学院自愿降低女性选择更多的部门的录取率，会发生什么呢？好吧，路径 G→D 被认为是公平的，但路径 D→A 部分是不公平的。

![](../Images/ab3a8186bdd64fd6c0aa3ee69a934140.png)

在这三个例子中，CBNs 提供了一个描述可能不公平情境的可视化框架。然而，对不公平关系影响的解释往往依赖于 CBN 之外的上下文因素。

到目前为止，我们已经使用 CBNs 来识别数据集中不公平的关系，但如果我们能量化这些关系会怎么样呢？事实证明，我们的技术的一个小变种可以用来量化数据集中的不公平性，并探索缓解不公平性的方法。量化不公平性的主要思想在于引入反事实情境，使我们能够询问模型的特定输入是否受到不公平对待。在我们的场景中，一个反事实模型将允许我们询问是否被拒绝的女性申请者（G=1, Q=q, D=d, A=0）在一个性别为男性的反事实世界中是否会得到相同的决定。在这个简单的例子中，假设录取决定是作为 G、Q 和 D 的确定性函数 f 得到的，即 A = f(G, Q, D)，这就相当于在询问 f(G=0, Q=q, D=d) = 0，即是否一个具有相同部门选择和资格的男性申请者也会被拒绝。

![](../Images/078005d7b5ba99fbd807ed9b36e41922.png)

随着机器学习在软件应用中变得越来越重要，创建公平模型的意义也将变得更加相关。DeepMind 的论文表明，CBNs 可以提供一个可视化框架，用于检测机器学习模型中的不公平性以及量化其影响的模型。这种技术可能帮助我们设计出代表人类最佳价值观并缓解一些偏见的机器学习模型。

[原文](https://towardsdatascience.com/deepmind-is-using-this-old-technique-to-evaluate-fairness-in-machine-learning-models-f33bce98196e)。经许可转载。

**相关：**

+   [DeepMind 和 Waymo 如何利用进化竞争训练自动驾驶车辆](/2019/09/deepmind-waymo-evolutionary-competition-self-driving-vehicles.html)

+   [重塑想象力：DeepMind 构建能够自发重放过去经历的神经网络](/2019/10/recreating-imagination-deepmind-builds-neural-networks-spontaneously-replay-past-experiences.html)

+   [DeepMind 静悄悄地开源了三种新型强化学习框架](/2019/09/deepmind-three-new-impressive-reinforcement-learning-frameworks.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [评估你的机器学习模型的（更好）方法](https://www.kdnuggets.com/2022/01/much-better-approach-evaluate-machine-learning-model.html)

+   [评估大型语言模型的更好方法](https://www.kdnuggets.com/a-better-way-to-evaluate-llms)

+   [关于可信图神经网络的全面调查：…](https://www.kdnuggets.com/2022/05/comprehensive-survey-trustworthy-graph-neural-networks-privacy-robustness-fairness-explainability.html)

+   [审计机器学习公平性的快速有效方法](https://www.kdnuggets.com/2023/01/fast-effective-way-audit-ml-fairness.html)

+   [DeepMind 在深度学习推动数学进步方面的新努力](https://www.kdnuggets.com/2021/12/inside-deepmind-new-efforts-deep-learning-advance-mathematics.html)

+   [提示工程中的并行处理：思维骨架…](https://www.kdnuggets.com/parallel-processing-in-prompt-engineering-the-skeleton-of-thought-technique)
