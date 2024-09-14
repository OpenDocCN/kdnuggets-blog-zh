# 数据科学家的困境：冷启动问题 – 十个机器学习示例

> 原文：[https://www.kdnuggets.com/2019/01/data-scientist-dilemma-cold-start-machine-learning.html](https://www.kdnuggets.com/2019/01/data-scientist-dilemma-cold-start-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Kirk D. Borne](https://twitter.com/KirkDBorne)，Booz Allen**。

![](../Images/519ed9af061c6441775306ce40648d59.png)

**来源：[https://www.yuspify.com/blog/cold-start-problem-recommender-systems/](https://www.yuspify.com/blog/cold-start-problem-recommender-systems/)**

古代哲学家孔子被认为说过“研究你的过去以了解你的未来。”这一智慧不仅适用于生活，也适用于机器学习。具体来说，标签数据（过去的事物）的可用性和应用对于标记之前未见的数据（未来的事物）是监督机器学习的基础。

如果没有过去数据中的标签（诊断、类别、已知结果），那么我们如何在标记（解释）未来数据方面取得进展？这将是一个问题。

在无监督机器学习中也出现了一个相关问题。在这些应用中，没有关于标签训练数据存在的要求或假设——我们本质上是在对数据中的模式进行参数化或特征化（*例如*，趋势、相关性、细分、聚类、关联）。

许多无监督学习模型如果事先知道哪些参数化选择最佳，可能会更容易收敛并更有价值。如果我们不能知道这一点（*即*因为它确实是无监督学习），那么我们至少希望知道我们的最终模型在解释数据方面是最佳的（以某种方式）。

在这两种应用（监督学习和无监督学习）中，如果我们没有这些初步的见解和验证指标，那么如何启动模型构建并朝着最佳解决方案前进？

这个挑战被称为***冷启动问题***！解决这个问题的办法很简单（有点）：我们做一个猜测——一个初始猜测！通常，这将是一个完全随机的猜测。

听起来真是……真是……随机！我们如何知道这是否是一个好的初始猜测？我们如何从这个随机的初始选择中推进我们的模型（参数化）？我们如何知道我们的进展是否朝着更准确的模型方向发展？怎么做？怎么做？怎么做？

这可能是一个真正的挑战。当然，没有人说“冷启动”问题会很容易。任何曾经在冰冻的早晨尝试启动一辆非常寒冷的汽车的人，都知道冷启动挑战的痛苦。在这样的早晨，没有什么比这更令人沮丧的了。但在这样的早晨，没有什么比发动机启动、汽车开始前进并逐渐提升性能的那一刻更令人振奋和鼓舞的了。

面对机器学习中的冷启动问题的数据科学家的经历可能非常相似，尤其是在我们的模型开始取得进展并表现出不断提高的性能时的兴奋感。

我们将在最后列出几个例子。但在此之前，让我们讨论一下目标函数。这才是真正解锁冷启动挑战中性能的关键。这是我们将列举的大多数例子中的神奇成分。

目标函数（也称为成本函数或收益函数）提供了模型性能的客观衡量。它可能像模型正确分类标签的百分比（在分类模型中），或模型曲线与数据点偏差的平方和（在回归模型中），或相对分离的簇的紧凑度（在聚类分析中）一样简单。

目标函数的价值不仅在于它的最终值（*即，* 给我们一个定量的整体模型性能评分），它的巨大（也许是最大的）价值体现在指导我们从最初的随机模型（冷启动零点）到最终成功（希望是最优）的模型。在这些中间步骤中，它作为评估（或验证）指标。

通过在步骤零（冷启动）测量评估指标，然后在对模型参数进行调整后再次测量，我们可以了解这些调整是否导致了更好的模型性能或更差的表现。然后，我们知道是否继续在相同方向上调整模型参数，还是转向相反方向。这被称为梯度下降。

梯度下降方法基本上是找出性能误差曲线的斜率（*即，* 梯度），当我们从一个模型进展到下一个模型时。正如我们在小学代数课上学到的，我们需要两个点来找出曲线的斜率。因此，只有在运行并评估两个模型后，我们才能得到两个性能点——然后最新点的曲线斜率会指导我们下一个模型参数调整的选择：要么（a）继续在与上一步相同的方向上调整（如果性能误差减少），以继续下降误差曲线；要么（b）在相反方向上调整（如果性能误差增加），以转向并开始下降误差曲线。

注意，爬山算法与梯度下降是相反的，但本质上是相同的。爬山算法不是最小化误差（成本函数），而是最大化准确性（收益函数）。同样，我们测量两个模型的性能曲线的斜率，然后向表现更好的模型的方向前进。在这两种情况下（爬山算法和梯度下降），我们希望达到一个最优点（最大准确性或最小误差），然后宣布这是最佳解决方案。当我们记得我们从一个初始的随机猜测开始（作为冷启动）时，这真是令人惊讶和满足的。

当我们的机器学习模型有许多参数（对于深度神经网络，这些参数可能有成千上万），计算变得更加复杂（可能涉及多维梯度计算，称为张量）。但原则是相同的：定量发现每一步模型构建进展中，需要对每一个模型参数进行哪些调整（大小和方向），以便朝着目标函数的最优值（*例如*，最小化误差、最大化准确性、最大化拟合优度、最大化精确度、最小化假阳性等）前进。在深度学习中，和典型的神经网络模型一样，估计这些对模型参数调整的方法（*即*，对每个网络节点之间的边权重）被称为[反向传播](http://neuralnetworksanddeeplearning.com/chap2.html)。这仍然是基于梯度下降的。

对于梯度下降、反向传播，或许所有的机器学习方法，可以这样理解：*“机器学习是一组从经验中学习的数学算法。良好的判断来自经验，而经验则来自于错误的判断。”* 在我们的例子中，随机冷启动模型的初始猜测可以视为“错误的判断”，但随后经验（*即*，来自于如梯度下降等验证指标的反馈）会带来“良好的判断”（更好的模型），进入我们的模型构建工作流程。

以下是数据科学中冷启动问题的十个例子，其中机器学习的算法和技术在模型向最优解的进展中产生了良好的判断：

+   聚类分析（如K-Means聚类），其中初始的聚类均值和聚类数量事先未知（因此最初是随机选择的），但可以利用聚类的紧凑性来评估、迭代并改进一组聚类，逐步进展到最终的最优聚类集（*即*，[最紧凑且分离最好的聚类](https://en.wikipedia.org/wiki/Dunn_index)）。

+   神经网络中，网络边上的初始权重是随机分配的（冷启动），但使用反向传播迭代模型到达最优网络（具有最高分类性能）。

+   TensorFlow深度学习，使用与简单神经网络相同的反向传播技术，但在非常高维的深度网络层和边缘权重的参数空间中使用张量来计算权重调整。

+   回归分析使用数据点与模型曲线之间偏差的平方和来找到最佳拟合曲线。在线性回归中，存在一个封闭形式的解（可以从[线性最小二乘技术](http://mathworld.wolfram.com/LeastSquaresFitting.html)中推导出来）。非线性回归的解通常不是一个封闭形式的数学方程组，但偏差平方和的最小化仍然适用 —— [梯度下降可以用于迭代工作流](https://towardsdatascience.com/linear-regression-using-gradient-descent-97a6c8700931)来寻找最佳曲线。请注意，K均值聚类实际上是[piecewise regression](https://www.researchgate.net/publication/259478129_Piecewise_regression_mixture_for_simultaneous_curve_clustering_and_optimal_segmentation)的一个例子。

+   非凸优化，其中目标函数具有许多山峰和谷底，因此梯度下降和爬山算法通常只会收敛到局部最优，而不是全局最优。像[遗传算法](https://www.kdnuggets.com/2018/03/introduction-optimization-with-genetic-algorithm.html)、[粒子群优化](https://en.wikipedia.org/wiki/Particle_swarm_optimization)（当梯度无法计算时）和其他[evolutionary computing](https://slideplayer.com/slide/12452454/)方法用于生成大量随机（冷启动）模型，然后对每个模型进行迭代，直到找到全局最优（或直到时间和资源耗尽，然后选择你找到的最好的一个）。[参见下图，该图说明了遗传算法的一个示例用例。另请参阅图形下方的NOTE关于遗传算法，这同样适用于其他进化算法，表明这些算法并不是专门的机器学习算法，而实际上是[元学习算法](http://rocketdatascience.org/?p=789)]。

+   kNN ([k-最近邻](https://www.analyticsvidhya.com/blog/2018/03/introduction-k-neighbours-algorithm-clustering/))是一种监督学习技术，其中数据集本身成为模型。换句话说，将新的数据点分配到特定组（该组可能有或没有类别标签或特定含义）仅仅是基于找到哪些现有数据点类别（组）在对新数据点的最近邻进行投票时占多数。需要检查的最近邻的数量是某个数k，可以最初是任意的（冷启动），然后进行调整以提高模型性能。

+   朴素贝叶斯分类，它将贝叶斯定理应用于具有类别标签的大数据集，但其中一些属性和特征的组合在训练数据中没有表示（*即*，一个冷启动挑战）。通过假设不同的属性是数据项的相互独立特征，可以估计新数据项的类别标签的后验概率，该数据项具有在训练数据中找不到的特征向量（属性集）。这有时被称为贝叶斯信念网络（BBN），是另一个示例，其中数据集成为模型，不同属性的个别出现频率可以指示不同属性组合的预期出现频率。

+   马尔可夫建模（序列信念网络）是BBN对序列的扩展，这些序列可以包括网络日志、购买模式、基因序列、语音样本、视频、股票价格或任何其他时间序列、空间序列或参数序列。

+   关联规则挖掘，它搜索发生频率高于随机抽样的数据集预期的共现关联。关联规则挖掘是另一个示例，其中数据集成为模型，没有关于关联的先验知识（*即*，冷启动挑战）。这种技术也称为市场篮子分析，已用于简单的冷启动客户购买推荐，但也用于如[tropical storm (hurricane) intensification prediction](https://ams.confex.com/ams/pdfpapers/84949.pdf)等特殊用例。

+   社交网络（链接）分析，其中网络中的模式（*例如*，中心性、覆盖范围、分离度、密度、社群等）编码关于网络的知识（*例如*，网络中最权威或最有影响力的节点），通过应用如[PageRank](https://en.wikipedia.org/wiki/PageRank#Other_uses)的算法，未提前了解这些模式（*即*，冷启动）。

最后，作为额外补充，我们提到一个特殊情况——推荐引擎，其中冷启动问题是一个持续研究的主题。研究挑战是找到对新客户或新产品的最佳推荐，这些客户或产品以前未见过。查看这些与此挑战相关的文章：

1.  [推荐系统中的冷启动问题](https://www.yuspify.com/blog/cold-start-problem-recommender-systems/)

1.  [解决推荐系统中的冷启动问题](http://kojinoshiba.com/recsys-cold-start/)

1.  [解决推荐系统中的冷启动问题](https://medium.com/@InDataLabs/approaching-the-cold-start-problem-in-recommender-systems-e225e0084970)

我们在开头提到孔子及其智慧。这是另一种形式的**智慧**： [https://rapidminer.com/wisdom/](https://rapidminer.com/wisdom/)—[RapidMiner](https://docs.rapidminer.com/)智慧会议。会议非常精彩，提供了许多优秀的教程、用例、应用程序和客户评价。我很荣幸在他们2018年在新奥尔良的会议上担任主旨发言人，主题是*“清除数据科学和机器学习中的迷雾：一些不寻常地方的常见嫌疑犯”*。你可以在这里找到我的幻灯片演示： [KirkBorne-RMWisdom2018.pdf](http://www.kirkborne.net/RMWisdom2018/KirkBorne-RMWisdom2018.pdf)

![](../Images/ff702805254f331745ddb05592f7c367.png)

**注意：** [遗传算法](http://www.cs.ubc.ca/labs/beta/Courses/CPSC532H-13/Slides/content-session-4-slides.pdf)（GAs）是[元学习](http://rocketdatascience.org/?p=789)的一个例子。它们本身不是机器学习算法，但GAs可以应用于机器学习模型和任务的集合中，以在一组局部最优解中找到最优模型（或许是全局最优模型）。

[原始版本](https://www.linkedin.com/pulse/data-scientists-dilemma-cold-start-problem-ten-machine-kirk-borne/)。转载已获许可。

**个人简介**： [Kirk D. Borne](https://twitter.com/KirkDBorne) 是Booz Allen Hamilton的首席数据科学家和执行顾问。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [冷启动问题：如何建立你的机器学习投资组合](https://www.kdnuggets.com/2019/01/cold-start-problem-machine-learning.html)

+   [谁是数据科学家？](https://www.kdnuggets.com/2018/12/who-is-data-scientist.html)

+   [AI的冷启动问题](https://www.kdnuggets.com/2018/04/cold-start-ai.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关话题

+   [人工智能的十年回顾](https://www.kdnuggets.com/2023/06/ten-years-ai-review.html)

+   [实施推荐系统的十大关键经验](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [挑选示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [通过示例了解集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [SQL LIKE 操作符示例](https://www.kdnuggets.com/2022/09/sql-like-operator-examples.html)

+   [如果我重新开始学习数据科学，我会怎么做？](https://www.kdnuggets.com/2020/08/start-learning-data-science-again.html)
