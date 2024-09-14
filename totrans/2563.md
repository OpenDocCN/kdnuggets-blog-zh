# 高性能深度学习：如何训练更小、更快、更好的模型 – 第3部分

> 原文：[https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part3.html](https://www.kdnuggets.com/2021/07/high-performance-deep-learning-part3.html)

[评论](#comments)

![](../Images/9970edb2850289adc136ca465d25bc5e.png)

在前面的部分（[第1部分](https://www.kdnuggets.com/2021/06/efficiency-deep-learning-part1.html) & [第2部分](https://www.kdnuggets.com/2021/06/high-performance-deep-learning-part2.html)）中，我们讨论了为什么效率对深度学习模型实现高性能的帕累托最优模型至关重要，以及深度学习中效率的关注领域。现在，让我们深入探讨这些关注领域中的工具和技术示例。

### 压缩技术

如前所述，压缩技术是可以帮助实现神经网络中一个或多个层的更高效表示的通用技术，可能会带来质量上的折衷。这种效率可能来自于提高一个或多个足迹指标，如模型大小、推理延迟、收敛所需的训练时间等，以换取尽可能少的质量损失。模型通常可能被过度参数化。在这种情况下，这些技术也有助于提高对未见数据的泛化能力。

**剪枝**：一种流行的压缩技术是剪枝，其中我们*剪除*不重要的网络连接，从而使网络*稀疏*。LeCun 等人[1]在其论文《最优大脑损伤》中，通过将神经网络中的参数（层间连接）数量减少了四倍，同时提高了推理速度和泛化能力。

![](../Images/d6a9408ffb6dca74628c0775dea39cdb.png)

*神经网络中剪枝的示意图。*

哈西比等人（Hassibi et al.）和朱等人（Zhu et al.）的最优大脑外科医生工作（OBD）采用了类似的方法[2]和[3]。这些方法使用已经预训练到合理质量的网络，然后迭代性地剪除具有最低*显著性*得分的参数，该得分衡量了特定连接的重要性，以使对验证损失的影响最小化。剪枝完成后，使用剩余参数对网络进行微调。该过程重复进行，直到网络被剪枝到所需的水平。

在各种剪枝工作中，差异发生在以下维度：

+   **显著性**：这是确定应该剪枝哪个连接的启发式方法。它可以基于连接权重相对于损失函数的二阶导数[1, 2]、连接权重的大小[3]等。

+   **无结构与结构化修剪：** 最灵活的修剪方式是无结构（或随机）修剪，在这种方法中，所有给定的参数被平等对待。在结构化修剪中，参数按大于1的块进行修剪（例如：在权重矩阵中按行修剪或在卷积滤波器中按通道修剪（例如：[4,5]）。结构化修剪使得在推理时能够更容易地利用大小和延迟的增益，因为这些修剪后的参数块可以智能地跳过存储和推理。

![](../Images/df10db1ba63ca3d898f7ea733cc4662c.png)

*无结构与结构化修剪权重矩阵的对比。*

+   **分配：** 可以为每一层设置相同的修剪预算，或者可以按层分配[6]。直觉上，有些层更适合修剪。例如，通常情况下，前几层已经足够小，无法承受显著的稀疏性[7]。

+   **调度**：另一个标准是修剪的数量和时机？我们是否希望每轮修剪相同数量的参数[8]，还是在开始时以较快的速度修剪，然后逐渐放慢[9]？

+   **再生长：** 在某些情况下，网络允许再生长被修剪的连接[9]，以便网络始终以相同的连接修剪百分比运行。

在实际应用中，具有有意义块大小的结构化修剪可以帮助提高延迟。Elsen等人[7]构建了稀疏卷积网络，这些网络在保留相同的Top-1准确率的同时，以约66%的参数超越了其密集网络，提升了1.3 - 2.4倍。他们通过库将NHWC（通道在最后）标准密集表示转换为适用于其快速内核的NCHW（通道优先）‘块压缩稀疏行’（BCSR）表示，以适应在ARM设备、WebAssembly等上进行快速推理[10]。尽管他们也对可以加速的稀疏网络类型引入了一些限制。总体而言，这是朝着修剪网络在足迹指标上取得实际改进的一个有希望的步骤。

**量化：** 量化是另一种流行的压缩技术。它利用了这样一个观点：典型网络中的几乎所有权重都是32位浮点值，如果我们可以接受一定程度的模型质量损失（准确率、精度、召回率等），我们可以将这些值存储在较低精度的格式中（16位、8位、4位等）。

例如，当模型持久化时，我们可以将权重矩阵中的最小值映射为0，最大值映射为2^b-1（其中b是精度位数），并将它们之间的所有值线性外推为整数值。通常，这可能足以减少模型大小。例如，如果b = 8，我们将32位浮点权重映射到8位无符号整数。这将导致空间减少4倍。在推理（计算模型预测）时，我们可以使用量化值以及数组的最小和最大浮点值来恢复原始浮点值的有损表示（由于舍入误差）。这一步称为*权重量化*，因为我们正在量化模型的权重。

![](../Images/ef17b953a75298d6add43c233b60465d.png)

*将连续高精度值映射到离散低精度整数值。[来源](https://sahnimanas.github.io/post/quantization-in-tflite)*

对于具有大量参数的较大网络，由于内置冗余，有损表示和舍入误差可能是可以接受的，但对于较小的网络，这可能会导致准确性下降，这些网络可能对这些误差更为敏感。

我们可以通过模拟训练过程中的权重量化舍入行为来解决这个问题（以实验方式）。我们通过在模型训练图中添加量化和反量化激活和权重矩阵的节点来实现这一点，使得神经网络操作的训练时间输入看起来与推理阶段的输入相同。这些节点被称为假量化节点。以这种方式训练使网络对推理模式下的量化行为更具鲁棒性。注意，现在我们在训练过程中同时进行*激活量化*和权重量化。训练时间模拟量化的步骤由Jacob等人和Krishnamoorthi等人详细描述[11,12]。

![](../Images/29b6800efbd8727fa22c549a99ea5b28.png)

*原始模型训练图和包含假量化节点的图。[来源](https://arxiv.org/abs/1712.05877)*

由于权重和激活都在模拟量化模式下运行，这意味着所有层都接收到可以用更低精度表示的输入，模型训练后应足够强大，以便直接在低精度下进行数学运算。例如，如果我们训练模型以在8位域中复制量化，则该模型可以部署以使用8位整数进行矩阵乘法和其他操作。

在资源受限的设备（如移动设备、嵌入式设备和物联网设备）上，使用像GEMMLOWP [13]这样的库可以将8位操作的速度提升1.5 - 2倍，这些库依赖于ARM处理器上的Neon内在支持 [14]。此外，像Tensorflow Lite这样的框架使用户可以直接使用量化操作，而无需担心底层实现。

![](../Images/10e885ceb9afbb996524328488062f36.png)

*模型的Top-1准确率与模型延迟（量化与非量化模型）的对比。[来源](https://arxiv.org/abs/1712.05877)*

除了剪枝和量化，还有其他技术，如低秩矩阵分解、K均值聚类、权重共享等，这些技术也正在积极用于模型压缩 [15]。

总体而言，我们发现压缩技术可以用来减少模型的占用空间（大小、延迟等），同时以牺牲一些质量（准确性、精确度、召回率等）作为交换。

### 学习技术

**蒸馏**：如前所述，学习技术试图以不同的方式训练模型，以获得更好的性能。例如，Hinton等人 [16] 在他们的开创性工作中，探讨了如何教会较小的网络从较大的模型/模型集成中提取*暗知识*。他们使用较大的*教师模型*在现有标记数据上生成软标签。

软标签为每个可能的类别分配一个概率，而不是原始数据中的硬二进制值。直观上，这些软标签捕捉了不同类别之间的关系，模型可以从中学习。例如，卡车与汽车比与苹果更相似，而模型可能无法直接从硬标签中学习到这一点。学生网络学习在这些软标签上最小化交叉熵损失，同时也在原始的真实硬标签上进行优化。这些损失函数的权重可以根据实验结果进行调整。

![](../Images/23d7835765b7909ccef6823a37f0376d.png)

*从较大的预训练教师模型中蒸馏出一个较小的学生模型。*

在论文中，Hinton等人 [16] 能够用一个单独的蒸馏模型来紧密匹配10模型集成在语音识别任务中的准确性。还有其他综合研究 [17,18] 展示了较小模型在模型质量上的显著改进。例如，Sanh等人 [18] 能够蒸馏出一个保留97% BERT-Base性能的学生模型，同时该模型在CPU上体积小40%，速度快60%。

**数据增强**：通常对于大型模型和复杂任务来说，你拥有的数据越多，提升模型性能的机会就越大。然而，获取高质量的标记数据通常既慢又昂贵，因为它们通常需要人工参与。学习这些由人类标记的数据被称为监督学习。当我们有资源支付标签时，它效果很好，但我们可以并且应该做得更好。

数据增强是一种提升模型性能的巧妙方法。通常，这涉及对数据进行转换，使其不需要重新标记（标签不变的转换）。例如，如果你在训练神经网络来分类图像为包含狗或猫，旋转图像不会改变标签。其他转换方法可以是水平/垂直翻转、拉伸、裁剪、添加高斯噪声等。类似地，如果你在检测给定文本的情感，引入一个错别字可能不会改变标签。

这种标签不变的转换方法已在流行的深度学习模型中广泛使用。当你有大量类别和/或某些类别的样本较少时，它们尤其方便。

![](../Images/9da7e52f7ea25c9451f2d97b4284e496.png)

*一些常见的数据增强类型。[来源](http://ai.stanford.edu/blog/data-augmentation/)*

还有其他转换方法，比如 Mixup [19]，它以加权的方式混合来自两个不同类别的输入，并将标签视为两个类别的加权组合。这个想法是模型应该能够提取出对这两个类别都相关的特征。

这些技术确实为流程引入了数据效率。这与教孩子在不同上下文中识别现实生活中的物体并没有太大区别。

**自监督学习**：在一个相关领域取得了迅速进展，我们可以学习通用模型，完全跳过提取数据意义时对标签的需求。通过对比学习等方法 [20]，我们可以训练一个模型，使其学习输入的表示，使得相似的输入具有相似的表示，而不相关的输入具有非常不同的表示。这些表示是 n 维向量（嵌入），然后可以在其他任务中作为特征使用，在这些任务中我们可能没有足够的数据从头训练模型。我们可以将使用无标签数据的第一步视为 *预训练*，将下一步视为 *微调*。

![](../Images/94acb34136fcc76346915833461861aa.png)

*对比学习。[来源](https://blog.einstein.ai/prototypical-contrastive-learning-pushing-the-frontiers-of-unsupervised-learning/)*

这种在未标记数据上进行预训练和在标记数据上进行微调的两步过程也在NLP社区中迅速获得接受。ULMFiT [21] 开创了训练通用语言模型的理念，其中模型学习解决给定句子中预测下一个词的任务。

作者发现，使用大量预处理但未标记的数据（如来源于英文维基百科页面的WikiText-103）作为预训练步骤是一个不错的选择。这足以让模型学习语言的一般特性。作者发现，对这种预训练模型进行二分类问题的微调只需100个标记样本（相比之下，通常需要10,000个标记样本）。

![](../Images/6e9dbfec387cb26415c2c53842c0e667.png)

*ULMFiT的快速收敛。[来源](https://nlp.fast.ai/classification/2018/05/15/introducing-ulmfit.html)*

![](../Images/ceb4033c6ba21d1776eb88d58283bda2.png)

*在大语料库上进行预训练并在相关数据集上进行微调的高级方法。[来源](https://nlp.fast.ai/classification/2018/05/15/introducing-ulmfit.html)*

这一思想也在BERT模型中得到了探索，其中预训练步骤涉及学习一个双向掩码语言模型，使得模型必须预测句子中间缺失的词。

总体而言，学习技术帮助我们提高模型质量而不影响模型的足迹。这可以用于提升模型部署的质量。如果原始模型质量令人满意，你还可以通过简单地减少网络中的参数数量来交换新获得的质量提升，从而改善模型大小和延迟，直到恢复到最低可行的模型质量。

在下一部分中，我们将继续探讨适用于剩余三个重点领域的工具和技术示例。此外，欢迎查阅我们的[调查论文](https://arxiv.org/abs/2106.08962)，详细探讨了这个话题。

### 参考文献

[1] Yann LeCun, John S Denker, and Sara A Solla. 1990\. 最优脑损伤。在神经信息处理系统进展中。598–605.

[2] Babak Hassibi, David G Stork, and Gregory J Wolff. 1993\. 最优脑外科医生和通用网络剪枝。在IEEE国际神经网络会议。IEEE, 293–299.

[3] Michael Zhu and Suyog Gupta. 2018\. 剪枝还是不剪枝：探索剪枝在模型压缩中的有效性。第六届国际学习表征会议，ICLR 2018，加拿大温哥华，2018年4月30日 - 5月3日，研讨会论文集。OpenReview.net. https://openreview.net/forum?id=Sy1iIDkPM

[4] Sajid Anwar, Kyuyeon Hwang, and Wonyong Sung. 2017\. 深度卷积神经网络的结构化剪枝。ACM《计算系统新兴技术期刊》（JETC）13, 3 (2017), 1–18.

[5] Hao Li, Asim Kadav, Igor Durdanovic, Hanan Samet 和 Hans Peter Graf. 2016\. 为高效 ConvNets 剪枝滤波器。在 ICLR（海报）。

[6] Xin Dong, Shangyu Chen 和 Sinno Jialin Pan. 2017\. 通过逐层最优大脑外科医生学习剪枝深度神经网络。arXiv 预印本 arXiv:1705.07565 (2017)。

[7] Erich Elsen, Marat Dukhan, Trevor Gale 和 Karen Simonyan. 2020\. 快速稀疏卷积网络。在 IEEE/CVF 计算机视觉与模式识别会议论文集。14629–14638。

[8] Song Han, Jeff Pool, John Tran 和 William J Dally. 2015\. 为高效神经网络学习权重和连接。arXiv 预印本 arXiv:1506.02626 (2015)。

[9] Tim Dettmers 和 Luke Zettlemoyer. 2019\. 从头开始的稀疏网络：更快的训练而不损失性能。arXiv 预印本 arXiv:1907.04840 (2019)。

[10] XNNPACK。https://github.com/google/XNNPACK

[11] Benoit Jacob, Skirmantas Kligys, Bo Chen, Menglong Zhu, Matthew Tang, Andrew Howard, Hartwig Adam 和 Dmitry Kalenichenko. 2018\. 神经网络的量化和训练，用于高效的整数运算推断。在 IEEE 计算机视觉与模式识别会议论文集。2704–2713

[12] Raghuraman Krishnamoorthi. 2018\. 量化深度卷积网络以实现高效推断：白皮书。arXiv (2018 年 6 月)。arXiv:1806.08342 https://arxiv.org/abs/1806.08342v1

[13] GEMMLOWP。https://github.com/google/gemmlowp

[14] Arm Ltd. 2021\. SIMD ISAs | Neon – Arm Developer。https://developer.arm.com/architectures/instruction-sets/simdisas/neon [在线；访问时间 2021 年 6 月 3 日]。

[15] Rina Panigrahy. 2021\. 矩阵压缩操作符。https://blog.tensorflow.org/2020/02/matrix-compressionoperator-tensorflow.html

[16] Geoffrey Hinton, Oriol Vinyals 和 Jeff Dean. 2015\. 提炼神经网络中的知识。arXiv 预印本 arXiv:1503.02531 (2015)。

[17] Gregor Urban, Krzysztof J Geras, Samira Ebrahimi Kahou, Ozlem Aslan, Shengjie Wang, Rich Caruana, Abdelrahman Mohamed, Matthai Philipose 和 Matt Richardson. 2016\. 深度卷积网络真的需要深度和卷积吗？arXiv 预印本 arXiv:1603.05691 (2016)。

[18] Victor Sanh, Lysandre Debut, Julien Chaumond 和 Thomas Wolf. 2019\. DistilBERT，BERT 的蒸馏版：更小、更快、更便宜、更轻便。arXiv 预印本 arXiv:1910.01108 (2019)。

[19] Hongyi Zhang, Moustapha Cisse, Yann N Dauphin 和 David Lopez-Paz. 2017\. mixup: 超越经验风险最小化。arXiv 预印本 arXiv:1710.09412 (2017)。

[20] Ting Chen, Simon Kornblith, Mohammad Norouzi 和 Geoffrey Hinton. 2020\. 用于视觉表征对比学习的简单框架。国际机器学习会议论文集。PMLR, 1597–1607。

[21] Jeremy Howard 和 Sebastian Ruder. 2018\. 用于文本分类的通用语言模型微调。arXiv 预印本 arXiv:1801.06146 (2018)。

**个人简介**：Gaurav Menghani 是 Google Research 的高级软件工程师，他领导的研究项目旨在优化大型机器学习模型，以便在从微控制器到基于 TPU 的服务器等设备上实现高效训练和推断。他的工作对 YouTube、Cloud、Ads、Chrome 等平台的超过10亿活跃用户产生了积极影响。他还将与 Manning 出版社合作撰写一本关于高效机器学习的即将出版的书。在 Google 之前，Gaurav 在 Facebook 工作了 4.5 年，为 Facebook 的搜索系统和大规模分布式数据库做出了重要贡献。他拥有石溪大学的计算机科学硕士学位。

twitter.com/GauravML

linkedin.com/in/gauravmenghani

www.gaurav.im

**相关内容：**

+   [高性能深度学习，第1部分](https://www.kdnuggets.com/2021/06/efficiency-deep-learning-part1.html)

+   [高性能深度学习：如何训练更小、更快、更优的模型 – 第2部分](https://www.kdnuggets.com/2021/06/high-performance-deep-learning-part2.html)

+   [扩展机器学习模型的5个挑战](https://www.kdnuggets.com/2020/10/5-challenges-scaling-machine-learning-models.html)

* * *

## 我们的3大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

### 更多相关内容

+   [7种方式让ChatGPT帮助你更快更好地编程](https://www.kdnuggets.com/2023/06/7-ways-chatgpt-makes-code-better-faster.html)

+   [oBERT：复合稀疏化为NLP提供更快更准确的模型](https://www.kdnuggets.com/2022/05/obert-compound-sparsification-delivers-faster-accurate-models-nlp.html)

+   [如何从头开始构建和训练Transformer模型……](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [通过参与竞赛让机器学习速度提高4倍](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [为什么我们总是需要人类来训练AI——有时是实时的](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [使用Tensorflow训练图像分类模型的指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)
