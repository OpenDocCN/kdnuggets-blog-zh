# 机器学习与大脑的不同 第三部分：基本架构

> 原文：[https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-3-fundamental-architecture.html](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-3-fundamental-architecture.html)

![机器学习与大脑的不同 第三部分：基本架构](../Images/9fe3c28be5e8e7d37ee7dc2fc4681152.png)

图片由 [Alex wong](https://unsplash.com/@killerfvith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/fundamental-architecture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今天的人工智能（AI）能够做一些非凡的事情。然而，它的功能与人类大脑完成相同任务的方式几乎没有关系。为了使AI克服其固有的局限性并发展到人工通用智能，我们必须认识到大脑与其人工对应物之间的差异。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

考虑到这一点，本系列的九部分将探讨生物神经元的能力和局限性，以及这些与机器学习（ML）的关系。在本系列的前两部分中，我们考察了神经元的缓慢如何使ML学习方法在神经元中显得不切实际，以及感知器的基本算法如何与涉及尖峰的生物神经元模型不同。第三部分探讨了ML和大脑的基本架构。

我们都见过ML神经网络中有序层次的示意图，其中每一层的神经元都与下一层的神经元完全连接。相反，大脑似乎包含了许多看似无序的连接。由于很难追踪大脑中的单个连接，我们不知道大脑的分层结构到底是什么。尽管如此，显然它不是ML的有序分层结构。

这种基本差异严重影响了在机器学习（ML）中可以使用的算法。虽然我们不太关注感知器和ML算法如何依赖有序的分层结构，但如果我们允许感知器与网络中的任何任意神经元建立突触，就会出现问题。

# 突触数量

在每一层都与下一层完全连接的神经网络中，突触的数量随着每一层神经元数量的平方增加（假设所有层大小相同以便于计算）。如果允许网络中的任何神经元与其他任何神经元连接，突触的数量会随着网络中神经元总数的平方增加。一个每层有1000个神经元的10层网络总共有900万个突触（输出层不需要突触）。如果任何神经元可以连接到任何其他神经元，理论上可能有1亿个突触，这可能会导致11倍的性能损失。

在大脑中，大多数这样的突触并不存在。新皮层具有160亿（16x10^9）个神经元，每个神经元都有可能与其他任何神经元连接，即256x10^18个可能的突触——如果每个突触占用1字节，则总共有256艾字节，这个数字极其庞大。然而，实际的突触数量估计为每个神经元104个，总共仅为16x10^13，这只是一个*惊人的*大数字（160太字节）。

这些突触中的很大一部分必须具有接近零的权重。这些“待用突触”用来存储记忆，通过快速改变权重来应对需要。然而，我们最终会得到一个“稀疏数组”，其中大多数条目为零。虽然很容易设计一个只表示非零突触的结构，但如果这样做，今天的GPU惊人的性能提升可能会被浪费。

# 感知器算法

感知器的基本算法对来自上一层的输入值进行求和。如果允许来自后续层的连接，则需要区分已经从前一层计算过的神经元内容和那些尚未计算的内容。如果算法不包括这个扩展，则感知器的值将依赖于神经元的处理顺序。在多线程实现中，这会使得感知器的值变得不可确定。

为了解决这个问题，感知器算法需要两个阶段和两个内部值：PreviousValue 和 CurrentValue。在第一个阶段，算法基于输入神经元的 PreviousValues 计算其 CurrentValue。在第二个阶段，它将当前值转移到前一个值。通过这种方式，所有在计算中使用的 PreviousValues 在第一个阶段的计算期间都是稳定的。这个算法的变化解决了这个问题，但代价是性能下降。

这个两阶段算法的实现可以在开源的Brain Simulator II中找到，文件为NeuronBase.cpp（[https://github.com/FutureAIGuru/BrainSimII](https://github.com/FutureAIGuru/BrainSimII)）。为了恢复这种性能，我们可以构建定制硬件或利用脉冲模型的优势，其中只有在神经元发射时才需要处理。在一台16核桌面计算机上，上述算法的处理速度达到了每秒25亿个突触。

# 数据结构

由于人类新皮层的160亿个神经元不可能完全连接到所有其他神经元，我们可以转向一种更接近生物结构的数据结构。通过这种方式，每个神经元只表示有用的突触，通过维护一个仅包含这些突触的列表或数组。这在内存使用上更高效，且可能更快，因为无需处理所有这些零条目。然而，这确实削弱了GPU处理的优势，因为今天的GPU在处理支持这种结构的小数组、循环和条件语句方面表现不佳。

# 反向传播

将更随机的神经元/突触结构适应到机器学习中的实际问题在于反向传播算法对这种结构的适应性较差。反向传播的梯度下降依赖于稳定的误差面。一旦我们允许循环连接，这个面就不再是时间上的常量。

*在本系列的第四部分，将讨论神经元在表示机器学习所依赖的精确值方面的局限性。*

**[查尔斯·西蒙](https://futureai.guru/Founder.aspx)** 是一位全国知名的企业家和软件开发者，也是FutureAI的首席执行官。西蒙是《计算机会反叛吗？：为人工智能的未来做准备》的作者，并且是AGI研究软件平台Brain Simulator II的开发者。欲了解更多信息，[请访问这里](https://futureai.guru/Founder.aspx)。

### 更多相关话题

+   [机器学习不像你的大脑 第一部分：神经元的慢……](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [机器学习不像你的大脑 第二部分：感知机与神经元](https://www.kdnuggets.com/2022/05/machine-learning-like-brain-part-two-perceptrons-neurons.html)

+   [机器学习不像你的大脑 第四部分：神经元的……](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-4-neuron-limited-ability-represent-precise-values.html)

+   [机器学习不像你的大脑 第五部分：生物神经元……](https://www.kdnuggets.com/2022/07/machine-learning-like-brain-part-5-biological-neurons-cant-summation-inputs.html)

+   [机器学习不像你的大脑 第六部分：……的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [机器学习不像你的大脑 第七部分：神经元的作用…](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-seven-neurons-good.html)
