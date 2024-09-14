# GANs 也需要一些关注

> 原文：[`www.kdnuggets.com/2019/03/gans-need-some-attention-too.html`](https://www.kdnuggets.com/2019/03/gans-need-some-attention-too.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Bilal Shahid（编辑：Taraneh Khazaei，Lindsay Brin）**

自注意力生成对抗网络（SAGAN；[Zhang 等人，2018](https://arxiv.org/pdf/1805.08318.pdf)）是使用自注意力范式来捕捉现有图像中的长程空间关系以更好地合成新图像的卷积神经网络。AISC 最近介绍并讨论了这篇论文，由 Xiyang Chen 主导。事件的详细信息可以在[AISC 网站](https://aisc.a-i.science/events/2018-06-11/)上找到，完整的演示可以在[YouTube](https://www.youtube.com/watch?v=FdeHlC4QiqA)上查看。在这里，我们提供了这篇论文及其主要贡献的概述。

### 传统 GAN 的挑战

虽然传统的深度卷积 GAN 在生成相当逼真的图像方面表现优异，但它们无法捕捉图像中的长程依赖。这些传统的 GAN 对于不包含大量结构和几何信息的图像（例如，描绘海洋、天空和田野的图像）表现良好。然而，当图像中的信息变化率很高时，传统的 GAN 往往无法获取所有这些变化，从而未能忠实地表示全局关系。这些非局部依赖在某些类别的图像中始终出现。例如，GAN 可以绘制具有逼真毛发的动物图像，但通常无法绘制分开的脚部。

![图](img/4dcc98be4dcd3cddecb226515d6946d3.png)

***来自之前的最先进 GAN（具有投影判别器的 CGAN；[Miyato 等人，2018](https://arxiv.org/abs/1802.05637)）的输出图像***

由于卷积运算符的表示能力有限（即感受野是局部的），传统的 GAN 只能在经过几层卷积之后才能捕捉长程关系。缓解此问题的一种方法是增加卷积核的大小，但这在统计和计算上都不高效。各种注意力和自注意力模型已被提出，以捕捉和利用结构模式和非局部关系。然而，这些模型通常无法在计算效率和建模长程关系之间取得平衡。

### GAN 的自注意力

这个功能差距就是 SAGAN 的作用所在。[Zhang et al. (2018)](https://arxiv.org/abs/1805.08318) 提出了一种方法，其中 GAN 模型配备了一种工具来捕捉图像中的长程、多层次关系。这个工具就是自注意力机制。自注意力试图将输入特征的不同部分关联起来，以计算适合当前任务的输入表示。自注意力的思想已经成功应用于阅读理解（[Cheng et al., 2016](https://arxiv.org/pdf/1601.06733.pdf)）、文本蕴含（[Parikh et al., 2016](https://arxiv.org/pdf/1606.01933.pdf)）和视频处理（[X. Wang et al., 2017](https://arxiv.org/pdf/1711.07971.pdf)）等领域。

将自注意力引入图像合成领域受到“非局部神经网络”（[X. Wang et al., 2017](https://arxiv.org/pdf/1711.07971.pdf)）的启发，该网络利用自注意力捕捉视频序列中的时空信息。一般而言，自注意力只是计算单个位置的响应作为所有位置特征的加权和。这个机制允许网络关注图像中结构上相关但空间上相隔较远的区域。

![图](img/a4ab4243f2515f6668c656ceb7f12da7.png)

***《自注意力生成对抗网络中提出的自注意力模块》（[Zhang et al., 2018](https://arxiv.org/abs/1805.08318)）***

在 SAGAN 中，自注意力模块与卷积网络协同工作，并使用键-值-查询模型（[Vaswani et al., 2017](https://arxiv.org/abs/1706.03762)）。该模块将卷积神经网络生成的特征图转换为三个特征空间。这些特征空间分别称为键 f(x)、值 h(x) 和查询 g(x)，通过将原始特征图传递通过三个不同的 1x1 卷积图生成。然后，键 f(x) 和查询 g(x) 矩阵相乘。接下来，对乘法结果的每一行应用 softmax 操作。由 softmax 生成的注意力图确定了网络应该关注图像的哪些区域，如公式（1）所述，来自 Zhang et al. 2018：

![](img/2be4597853e42395afd54d41f488cdbe.png)

因此，注意力图会与值 h(x) 相乘，生成自注意力特征图，如下所示（公式（2）来自 Zhang et al. 2018）：

![](img/f31e7f12fd2d1aabab75c10c5e58f6fc.png)

最终，输出是通过将原始输入特征图与缩放后的自注意力图相加来计算的。缩放参数 ???? 在开始时初始化为 0，以使网络首先关注局部信息。随着缩放参数 ???? 在训练过程中更新，网络逐渐学习关注图像的非局部区域（公式（3）来自 Zhang et al. 2018）：

![](img/376658f037439643d4b741aa2caa969e.png)

![图像](img/454076caa5eaf57d706de907c61b99f5.png)

***自注意力生成对抗网络输出图像 ([Zhang, et al., 2018](https://arxiv.org/abs/1805.08318))***

### 处理 GAN 训练中的不稳定性

SAGAN 论文的另一个贡献与生成对抗网络（GANs）训练不稳定的问题有关。论文提出了两种处理此问题的技术：光谱归一化和双时间尺度更新规则（TTUR）。

当生成器经过良好条件化时，表现更佳且训练动态得到了改善 ([Odena, et al., 2018](https://arxiv.org/abs/1802.08768))。使用 SAGAN 时，生成器的条件化通过 [光谱归一化](https://christiancosgrove.com/blog/2018/01/04/spectral-normalization-explained.html) 来实现。这种方法最初是在 ([Miyato et al. 2018](https://arxiv.org/pdf/1802.05957.pdf)) 中引入的，但仅针对判别器网络，以解决生成器未能很好地学习目标分布的问题。SAGAN 在生成器和判别器网络中都采用光谱归一化，限制两个网络中权重矩阵的光谱范数。这一过程是有益的，因为它在不需要任何超参数调节的情况下限制了 Lipschitz 常数，防止了参数幅度和异常梯度的激增，并允许与生成器相比，判别器的更新次数减少。

除了光谱归一化，论文还使用了 TTUR ([Heusel et al., 2018](https://arxiv.org/abs/1706.08500)) 来解决使用正则化判别器时学习缓慢的问题。使用正则化判别器的方法通常需要每次生成器更新时多次更新判别器。为了加快学习速度，生成器和判别器使用不同的学习率进行训练。

### 结论

SAGAN 是图像生成技术的一项重大进步。有效整合自注意力技术使网络能够以计算高效的方式真实捕捉和关联长距离空间信息。在判别器和生成器网络中使用光谱归一化，以及 TTUR，不仅降低了训练的计算成本，还提高了训练稳定性。

[原文](https://aisc.a-i.science/blog/2019/self-attention-gan/)。经授权转载。

**相关：**

+   对抗样本解释

+   生成对抗网络 – 论文阅读路线图

+   什么是一些“高级”人工智能和机器学习在线课程？

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT

* * *

### 更多相关话题

+   [为 RNN 添加注意力机制](https://www.kdnuggets.com/2022/03/packt-adding-attention-mechanism-rnns.html)

+   [终于有一本关于注意力机制的书！](https://www.kdnuggets.com/2022/11/mlm-finally-book-attention.html)

+   [事情并不总是正常：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

+   [我使用 ChatGPT（每天）5 个月。这里有一些隐藏的宝藏…](https://www.kdnuggets.com/2023/07/used-chatgpt-every-day-5-months-hidden-gems-change-life.html)

+   [一些惊人的提示工程技术，以提升我们的 LLM 模型](https://www.kdnuggets.com/some-kick-ass-prompt-engineering-techniques-to-boost-our-llm-models)

+   [它活了！用 Python 和一些便宜的组件打造你的第一个机器人…](https://www.kdnuggets.com/2023/06/manning-build-first-robots-python-cheap-basic-components.html)
