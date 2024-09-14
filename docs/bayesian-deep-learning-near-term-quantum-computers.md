# 贝叶斯深度学习与近期量子计算机：量子机器学习中的警示故事

> 原文：[https://www.kdnuggets.com/2019/07/bayesian-deep-learning-near-term-quantum-computers.html](https://www.kdnuggets.com/2019/07/bayesian-deep-learning-near-term-quantum-computers.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[彼得·维特克](https://peterwittek.com/)（阿米尔·费兹普尔和尼克·莫里森编辑）**

*这篇博客文章是由[《量子计算机上的贝叶斯深度学习》](https://link.springer.com/article/10.1007%2Fs42484-019-00004-7)一文的作者撰写的量子机器学习概述。文章探讨了量子计算领域中的机器学习应用。本文的作者希望实验结果能够影响量子机器学习的未来发展。*

由于研究问题、教育项目和人才需求不断增加，机器学习成为当今技术领域最热门的话题之一。与学习算法的成功相并行的是，量子计算硬件的发展在过去几年中加速了。实际上，我们正处于实现量子优势的门槛上，也就是在某些特定应用领域（例如[机器学习](https://en.wikipedia.org/wiki/Quantum_machine_learning)）相较于经典算法获得速度或其他性能提升。这听起来很令人兴奋，但不要急于丢弃你的GPU集群；量子计算机和你的并行处理单元解决的是不同类别的问题。

如果你查看下图，你将快速了解如何思考量子计算在机器学习中的角色。随着待解决问题复杂性的增长，人工智能堆栈已经开始包含越来越多不同的硬件后端，包括从现成的CPU到张量处理单元（TPUs）和神经形态芯片。然而，仍然存在一些问题过于复杂而难以解决，其中一些问题适合用量子计算解决。换句话说，量子计算机可以像过去的GPU一样，使创建更复杂的机器学习和深度学习模型成为可能。

![](../Images/d054a7663750a7b89adfcca616e64068.png)

***图1。** 人工智能已经使用了异构的硬件后端组合。在未来，这种组合将包括量子技术，这些技术能够支持某些学习算法。基于斯蒂夫·朱尔维特森在机器学习与智能市场会议上的[讲座](http://www.marketforintelligence.com/talks/why-will-machine-intelligence-be-so-transformational/)。*

量子计算机使用“量子比特”（qubits）：量子比特是0和1之间的特殊概率分布。量子比特的数量和我们可以执行的操作的质量（保真度）是衡量量子计算机优劣的最重要指标。量子机器学习使用量子计算机来运行学习算法的某些部分。各种量子机器学习提案的近期可行性各不相同。一些量子算法已经可以在当前量子硬件上运行（例如，需要少量量子比特和量子门的算法，称为变分算法），其他算法则需要更好的量子计算机，具有更多且质量更高的量子比特（如最著名的算法如Shor算法的因式分解或Grover算法的无结构搜索）。到目前为止，仅有少数基准结果可用于在真实硬件上运行实际的量子机器学习算法。

所有这些基准测试都专注于那些为这些嘈杂、早期量子设备设计的更明显的算法类别。我们则关注另一个极端：让我们选择一个最困难的算法原语（所有步骤都是基本操作，并且不调用其他算法），看看我们能得到什么结果。为什么？一个著名的算法是[量子矩阵求逆算法](https://en.wikipedia.org/wiki/Quantum_algorithm_for_linear_systems_of_equations)（简称HHL，取自[原始文章](https://arxiv.org/abs/0811.3171)的作者），它在矩阵求逆方面相比经典算法提供了指数级的加速，是量子机器学习中最常见的算法原语之一。我们相信使用量子计算机有其优势，但我们也认为必须考虑当前量子硬件的能力。因此，我们开始了一项展示情况可能变得多么糟糕的任务。

量子矩阵求逆算法存在无数的警告：它需要数千个[错误校正量子比特](https://en.wikipedia.org/wiki/Quantum_error_correction)和更多的量子门。换句话说，它需要一个完美的大规模通用量子计算机。这与我们今天拥有的少量嘈杂量子比特和由于量子比特只能维持短时间的叠加状态（也称为相干时间）而能够构建的浅层电路形成鲜明对比。叠加状态是我们前面提到的0和1之间的特殊概率分布。如果相干时间不好，概率分布不在我们的控制之下：我们无法对其进行变换以表达计算。

鉴于量子矩阵反演是量子机器学习中的常见子程序，并且它需要非常高质量的量子计算机，我们想要讲述一个警示故事。我们发现了一个机会：在2017年末出现了[two](https://arxiv.org/abs/1711.00165) [papers](https://openreview.net/forum?id=H1-nGgWC-) ，它们将深度学习与高斯过程联系了起来。这是一个重要的成就，因为将深度学习与贝叶斯技术（其中高斯过程是一个例子）结合起来，可以从网络中获取更多信息。特别是，它提供了了解预测不确定性的简便方法。不幸的是，贝叶斯学习是困难的，即使在最好的经典硬件上也是如此。这就是为什么利用这一新发现的联系，我们提出了一种新的量子贝叶斯深度学习协议，该协议内部使用量子矩阵反演来实现加速。从表面上看，新协议看起来不错：这是一个在经典计算中很难解决的问题，我们应用了一些数学技巧，通过使用量子算法使其速度显著提升！不幸的是，这种加速需要非常非常高质量的量子计算机。

我们利用这个机会实现了量子矩阵反演算法，并在现代量子计算机上进行了基准测试。相关的[论文](https://doi.org/10.1007/s42484-019-00004-7)刚刚发表（开放版可以在[arXiv](https://arxiv.org/abs/1806.11463)上找到），还有一个匹配的[git 仓库](https://gitlab.com/apozas/bayesian-dl-quantum/)。下图概述了我们提出的内容。

![](../Images/94f7e58948478db9b352e773c1572538.png)

***图 2.** 量子贝叶斯深度学习的示意图。在某些条件下，神经网络中的一层神经元可以被视为等同于高斯过程。最近的研究表明，如果我们考虑许多层，这种等价关系仍然成立。为了快速训练高斯过程，我们使用了量子辅助协议，并在量子处理单元上运行它。图像来源：Alejandro Pozas-Kerstjens。*

结果证实了尽管量子矩阵反演被广泛使用，但它并不是实现实际量子优势的首选目标。任何门噪声都会立即破坏计算，这很直观，因为即使是一个小的矩阵反演也需要几十个门。量子计算结果的读取是通过对设备的测量来完成的。测量量子比特的设备中的故障也会影响算法的性能，不过，由于需要进行的测量较少，这种误差源的影响较小。

![](../Images/9a245ce50eaf60da17d2115aea824607.png)

***图 3.** 反转 4x4 矩阵后的最终状态的保真度。使用不同噪声水平和噪声模型进行的模拟。*

为了在真实硬件上运行，我们需要简化，因为完整算法即使对2x2矩阵来说也过于深奥。幸运的是，[我们找到了所需的解决方案](https://arxiv.org/abs/1110.2232)：一个专门为求逆2x2矩阵设计的浅层电路。虽然这对任何实际应用无关，但这个电路有略多于十二个门（包括状态准备）和有趣的结果。我们使用了[16量子比特的IBM芯片](https://www.research.ibm.com/ibm-q/technology/devices/#ibmq_16_melbourne)和[8量子比特的Agave芯片](https://medium.com/rigetti/how-to-write-a-quantum-program-in-10-lines-of-code-for-beginners-540224ac6b45)。Agave芯片具有特定的线性拓扑，增加了一些额外的门来按需操作量子状态。IBM QPU在测量后成功率为89%，相当于0.78的保真度。Agave QPU的成功率为50%，相当于零保真度，但我们认为这主要是由于线性拓扑和需要插入的额外门。这些并不多，但足以使算法的执行时间超过量子比特的相干时间。

为了量子机器学习（QML）算法而创建的内容，如果在GPU集群上可以有效完成，则没有意义。由于贝叶斯深度学习在使用经典资源时很难执行，我们通过量子协议进行了攻击。凭借这一理论，我们在允许量子计算机训练深度神经网络方面取得了非平凡的进展。

在实现方面，我们建立了量子矩阵求逆算法的完整实现，这可以作为未来量子计算机的一个严格基准。我们得出结论，最好远离这些抽象算法，更多关注今天量子硬件的局限性。我们希望经典和量子机器学习社区能发现这些结果具有趣味性，并且希望下一代量子编码者能够从这次实验中学习。

[原文](https://aisc.ai.science/blog/2019/bayesian-deep-learning-and-near-term-quantim-computers/)。经许可转载。

**相关内容：**

+   [预训练、变压器和双向性](/2019/07/pre-training-transformers-bi-directionality.html)

+   [生成对抗网络也需要关注](/2019/03/gans-need-some-attention-too.html)

+   [大规模图像分类器的演变](/2019/05/large-scale-evolution-image-classifiers.html)

* * *

## 我们的前3课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

### 更多相关话题

+   [成为优秀数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [前往又回来的…一个 RAPIDS 故事](https://www.kdnuggets.com/2023/06/back-again-rapids-tale.html)

+   [强化学习：教计算机做出最佳决策](https://www.kdnuggets.com/2023/07/reinforcement-learning-teaching-computers-make-optimal-decisions.html)

+   [停止学习数据科学以寻找目标，而是通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
