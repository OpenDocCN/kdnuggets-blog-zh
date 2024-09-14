# 深度学习阅读组：图像识别中的深度残差学习

> 原文：[https://www.kdnuggets.com/2016/09/deep-learning-reading-group-deep-residual-learning-image-recognition.html](https://www.kdnuggets.com/2016/09/deep-learning-reading-group-deep-residual-learning-image-recognition.html)

![](../Images/7de88231876e55a10fbd949387f67285.png)

今天的[论文提出了一种新的卷积网络架构](https://arxiv.org/abs/1512.03385)。这篇论文由微软研究院的He、Zhang、Ren和Sun撰写。我要在开始之前警告你：这篇论文非常古老。它发布于2015年底深度学习的黑暗时代，我相当确定它的原始格式是莎草纸；幸运的是，有人扫描了它，以便未来几代人可以阅读。然而，它仍然值得抹去尘土并翻阅，因为它提出的架构已经被反复使用，包括在[我们之前阅读的一些论文中](https://gab41.lab41.org/lab41-reading-group-deep-networks-with-stochastic-depth-564321956729)：[具有随机深度的深度网络](https://arxiv.org/abs/1603.09382)。

He *et al.* 开始时指出了一个看似矛盾的情况：非常深的网络表现比中等深度的网络更差，也就是说，尽管增加网络的层数通常会提高性能，但在某些点之后，新层开始阻碍网络。他们将这种效应称为网络降级。

如果你一直关注我们的[之前的帖子](https://gab41.lab41.org/lab41-reading-group-deep-networks-with-stochastic-depth-564321956729)，这不会让你感到惊讶；随着网络变得更深，诸如梯度消失等训练问题会变得更严重，因此你会期望更多的层会在某个点之后使网络表现变差。但作者预见了这种推理，并指出几种其他深度学习方法，如[批量归一化](https://arxiv.org/abs/1502.03167)（参见[我们的帖子](https://gab41.lab41.org/batch-normalization-what-the-hey-d480039a9e3b)的总结），基本上解决了这些训练问题，但网络的表现仍然随着深度的增加而变得越来越差。例如，他们比较了20层和56层的网络，并发现56层网络表现远不如20层；见下图。

[![CIFAR Nets](../Images/fa20235ad1477a7fa79c4b093a35095e.png)](https://cdn-images-1.medium.com/max/1500/1*UbNuXfVNVfTq1WkMZyJ1QA.png)

*20层和56层网络在CIFAR-10上的比较。注意56层网络在训练和测试中的表现都较差。*

然后作者设立了一个思想实验（或者说[思想实验](https://en.wiktionary.org/wiki/gedankenexperiment)如果你像我一样是一个恢复中的物理学家）来证明更深的网络应该总是表现更好。他们的论点如下：

1.  从一个表现良好的网络开始；

1.  添加强制为恒等函数的额外层，即，它们只是传递到达它们的任何信息而不进行更改；

1.  这个网络更深，但必须与原始网络具有相同的性能，因为新层没有做任何事情。

1.  网络中的层可以学习恒等函数，因此如果它是最优的，它们应该能够准确复制这个深层网络的性能。

这个思想实验促使他们提出了深度残差学习架构。他们构建了他们所谓的残差构建块网络。下图展示了其中一个这样的块。这些块被称为ResBlocks。

[![ResBlock](../Images/0798dc5151f41fd870ef7235d82b7976.png)](https://cdn-images-1.medium.com/max/1500/1*4sO3vjCdUlYQZcRq-GNqyw.png)

*一个ResBlock；顶部学习一个残差函数 f(x)，而底部的信息保持不变。图像改编自[黄等人的随机深度论文](https://arxiv.org/abs/1603.09382)。*

ResBlock由普通网络层构建，这些层通过[线性整流单元](https://en.wikipedia.org/wiki/Rectifier_%28neural_networks%29)（ReLUs）连接，并且在下面有一个通过的连接，保持来自先前层的信息不变。ResBlock的网络部分可以包含任意数量的层，但最简单的情况是两层。

要了解ResBlock背后的数学：假设一组层如果学习一个特定的函数*h(x)*，将表现最好。作者指出，可以学习残差*f(x) = h(x) − x*，然后将其与原始输入结合，以恢复*h(x)*，如下所示：*h(x) = f(x) + x*。这可以通过向网络中添加一个*+x*组件来实现，回想一下我们的思想实验，这只是恒等函数。作者希望将这种“传递”添加到他们的层中将有助于训练。与大多数深度学习一样，这种方法只有直观的支持，而没有更深层次的理解。然而，正如作者所展示的，它确实有效，而这正是许多从业者所关心的唯一问题。

论文还探讨了对ResBlock的一些修改。第一个是创建具有三层的瓶颈块，其中中间层通过使用较少的输入和输出来限制信息流。第二个是测试不同类型的传递连接，包括学习完整的投影矩阵。尽管更复杂的传递连接性能更好，但只是略微提高，而且训练时间成本更高。

论文的其余部分测试了网络的性能。作者发现，他们的网络在使用通行功能后表现优于没有通行功能的相同网络；请参见下图的绘图以了解这一点。他们还发现，能够训练更深层的网络，并且仍然表现出改进的性能，最终训练了一个152层的ResNet，表现优于较浅的网络。他们甚至训练了一个1202层的网络以证明其可行性，但发现其性能不如论文中检查的其他网络。

[![](../Images/a10ad9ecf70715537142a85783583456.png)](https://cdn-images-1.medium.com/max/1500/1*f8TRIszXY6xGqtcMBWvD3g.png)

*两个网络性能的比较：左侧的网络不使用ResBlocks，而右侧的网络使用了。注意34层的网络比18层的网络表现更好，但仅在使用ResBlocks时。*

就这样！他 *et al.* 提出了一个新的架构，受到思想实验的启发，并希望它能比以前的架构表现更好。他们构建了几个网络，包括一些非常深的网络，发现他们的新架构确实改善了网络的性能。虽然我们对深度学习的基本原理没有获得进一步的理解，但我们确实获得了一种使网络表现更好的新方法，最终也许这已经足够好。

**[亚历山大·古德](https://twitter.com/alex_gude)** 目前是Lab41的数据科学家，研究推荐系统算法。他拥有加州大学伯克利分校的物理学学士学位和明尼苏达大学双城分校的基础粒子物理学博士学位。

**[Lab41](http://www.lab41.org)** 是一个“挑战实验室”，美国情报界与学术界、工业界以及In-Q-Tel的同行共同合作，解决大数据问题。它允许来自不同背景的参与者接触思想、人才和技术，以探索数据分析中有效和无效的方面。作为一个开放、协作的环境，Lab41促进了参与者之间的宝贵关系。

[原文](https://gab41.lab41.org/lab41-reading-group-deep-residual-learning-for-image-recognition-ffeb94745a1f)。经许可转载。

**相关内容：**

+   [在深度学习中，架构工程是新的特征工程](/2016/07/deep-learning-architecture-engineering-feature-engineering.html)

+   [深度学习进展：七月更新](/2016/08/deep-learning-july-update.html)

+   [深度学习网络为何会扩展？](/2016/07/deep-learning-networks-scale.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关内容

+   [用于图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [逐步指南：阅读和理解SQL查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [利用AI读取思维：研究人员将脑电波转换为图像](https://www.kdnuggets.com/2023/03/reading-minds-ai-researchers-translate-brain-waves-images.html)

+   [2024年阅读清单：5本关于人工智能的必读书籍](https://www.kdnuggets.com/2024-reading-list-5-essential-reads-on-artificial-intelligence)

+   [语音识别指标的发展](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [5个需求量大但不被足够认可的IT职位](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition)
