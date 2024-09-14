# 数学 2.0：机器学习的基本重要性

> 原文：[https://www.kdnuggets.com/2021/09/math-fundamental-importance-machine-learning.html](https://www.kdnuggets.com/2021/09/math-fundamental-importance-machine-learning.html)

[评论](#comments)

**由 [Dr. Claus Horn](https://www.linkedin.com/in/aiscientist/)，人工智能研究员和讲师**

![图片](../Images/a7e53d444aaef2ee5f95db965fe91c1b.png)

一些人，特别是在当前数据科学的热潮中，将机器学习视为仅仅是另一种算法。它是数字化的一部分，帮助我们自动化事务，仅此而已。不幸的是，这种解释完全忽略了主要的观点。*机器学习不仅仅是编程计算机的另一种方式；它代表了我们理解世界方式的根本转变。它是数学 2.0。*

科学理论通过建立世界的模型来帮助我们理解世界。它们的有用性在于它们允许我们对未来做出预测。直到历史上的这一点，我们对世界的最复杂模型都是用数学语言（数学 1.0）编写的。现在这一点正在发生变化。即将到来的科学模型将是机器学习模型（可能是神经网络）：数学 2.0。

原因在于机器学习模型允许我们描述更高复杂度的现象。我们在数学理论中能够描述的功能关系非常有限，相比之下，例如现代深度学习能够很好地将一万像素值映射到狗或猫的概念。

回到 2003 年，在我攻读物理学博士学位期间，我们在扫描几拍字节的数据时，寻找新型基本粒子的迹象，这些数据记录在德国高能物理中心 DESY（当时是世界上最大的数据库之一，谷歌成立才五年）。

我们发现，对观测变量应用独立选择的常规过程有些乏味，因为在改变一个变量的切割之后，我们必须重新检查所有其他变量。因此我们想：我们能否自动化这个过程？事实上，我们想到的一个简单算法允许我们自动优化选择。后来我们发现，计算机科学家有一个术语来形容我们所做的事情：他们称之为机器学习。

很快，这种新方法显然意味着我们需要调整我们的科学工作流程。与其利用所有物理学知识来增加信号与背景的比率，不如仅做最小限度的清理切割，让算法来完成工作。后来，随着深度学习的出现，CERN 的研究人员意识到，即使是物理量的重建也是适得其反的。仅凭原始测量数据，深度学习能够超越任何人工选择的物理学家（见下图）。*因此，数学 2.0 让我们能看到基于数学 1.0 的模型无法看到的粒子。*

![图 1](../Images/61581513f384fe24890192efb4d13359.png)

**图 1**: 基于低级特征（黑色）和物理学家通常使用的高级特征（红色）的深度学习算法性能比较。 （图取自参考文献1的2014年《自然》论文。）

人们一直在疑惑为何大多数物理模型非常简单，大多数情况下仅涉及三次或更低次的多项式。也许原因在于我们只能看到我们能够用语言表达的内容。

物理学中发生的事情现在也发生在其他领域：一段给定的氨基酸序列总是以相同的方式折叠。在这里有很多规律性！实际上，是蛋白质的结构决定了它的功能。然而，*我们无法产生一个数学函数来描述这种关系。但我们可以构建一个能够做到这一点的机器学习模型。* 构建这样一个模型是一个重要的里程碑，因此有关于 AlphaFold（这个模型的名称）是否值得诺贝尔奖的猜测。

由于 Math 2.0 让我们描述比 Math 1.0 更复杂的关系，未来十年可能会看到生物学的转变。数字生物学将用 Math 2.0 的语言书写。其他科学领域中也会有许多机会，特别是那些关系更复杂的领域，如社会科学。

*因此，掌握 Math 2.0 的语言应该是每个学术课程的核心组成部分，也是每位学生，尤其是在科学领域的学生，的基本能力。*

当然，科学理论不仅仅是数学。主要的困难在于找到合适的概念和量来描述给定范围的现象。这一点不会改变。但数学不仅帮助我们建立模型和推导预测。它还允许我们通过代数计算和推导新的简化洞察，并通过微积分回答关于系统动态的问题。

我们仍处于 Math 2.0 革新的起始阶段，但我预测，与数学的发展类似，我们将会看到一个新领域的出现，这个领域将研究*机器学习模型的系统*及其构建方式、自动优化自身的方式，以及如何利用这些模型获得新的洞察，使我们以新的视角看待世界。

这些发展将带来科学计算的下一层次，并为我们提供**一种新的科学方法，这要归功于 Math 2.0。**

**参考文献：**

1.  Baldi, P., Sadowski, P. & Whiteson, D. 利用深度学习在高能物理中寻找奇异粒子。*Nat Commun* **5**, **4308 (2014)。 https://doi.org/10.1038/ncomms5308

**个人简介: [Dr. Claus Horn](https://www.linkedin.com/in/aiscientist/)** 是一位人工智能领域的研究员和讲师，他相信21世纪人类条件进步的最高潜力存在于人工智能与生命科学的交汇点。

[原文](https://www.linkedin.com/pulse/math-20-fundamental-importance-machine-learning-dr-claus-horn/)。经许可转载。

**相关：**

+   [机器学习如何利用线性代数解决数据问题](/2021/09/machine-learning-leverages-linear-algebra-solve-data-problems.html)

+   [反脆弱性与机器学习](/2021/09/antifragility-machine-learning.html)

+   [自然语言处理中的线性代数](/2021/08/linear-algebra-natural-language-processing.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关话题

+   [如何克服对数学的恐惧并学习数据科学中的数学](https://www.kdnuggets.com/2021/03/overcome-fear-learn-math-data-science.html)

+   [使用基础和现代算法解决计算机科学问题](https://www.kdnuggets.com/2023/11/packt-tackle-computer-science-problems-fundamental-modern-algorithms-machine-learning)

+   [机器学习不像你的大脑 第3部分：基础架构](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-3-fundamental-architecture.html)

+   [机器学习不像你的大脑 第6部分：精确突触权重的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [机器学习中的预处理重要性](https://www.kdnuggets.com/2023/02/importance-preprocessing-machine-learning.html)

+   [机器学习中的可重复性重要性](https://www.kdnuggets.com/2023/06/importance-reproducibility-machine-learning.html)
