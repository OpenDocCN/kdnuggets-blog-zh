# 深度学习阅读小组：SqueezeNet

> 原文：[https://www.kdnuggets.com/2016/09/deep-learning-reading-group-squeezenet.html](https://www.kdnuggets.com/2016/09/deep-learning-reading-group-squeezenet.html)

**作者：Abhinav Ganesh，Lab41。**

![](../Images/4ab887c7999e764c91ffdb9f2d513d5e.png)

[下一篇论文](https://arxiv.org/abs/1602.07360)来自我们的阅读小组，由 Forrest N. Iandola、Matthew W. Moskewicz、Khalid Ashraf、Song Han、William J. Dally 和 Kurt Keutzer 共同撰写。该论文介绍了一种名为“SqueezeNet”的小型 CNN 架构，在 ImageNet 上实现了与 AlexNet 相当的准确率，但参数数量减少了 50 倍。正如你可能已经注意到的，我们对神经网络架构的压缩非常感兴趣，这篇论文确实很突出。

深度学习的难点之一就是参数调优的地狱。这篇论文提倡增加对卷积神经网络设计领域的研究，以大幅减少你需要处理的参数数量。与我们之前的[关于“深度压缩”的帖子](https://gab41.lab41.org/lab41-reading-group-deep-compression-9c36064fb209#.b2l7ziyp0)不同，这篇论文建议通过从更智能的设计开始来缩小网络规模，而不是使用巧妙的压缩方案。作者概述了减少参数大小而最大化准确性的三种主要策略。我现在将为你介绍这些策略。

> 策略 1\. 通过用 1x1 卷积核替换 3x3 卷积核来缩小网络

这个策略通过将一堆 3x3 卷积核替换为 1x1 卷积核，从而减少了参数的数量，减少了 9 倍。最初这让我感到非常困惑。我以为移动 1x1 卷积核在图像上时，每个卷积核看到的信息会更少，因此性能会更差，但事实似乎并非如此！通常较大的 3x3 卷积核捕捉的是彼此接近的像素的空间信息。另一方面，1x1 卷积核专注于单个像素，并捕捉其通道之间的关系，而不是邻近像素之间的关系。如果你想了解更多关于 1x1 卷积核的使用，可以查看[这篇博客文章](http://iamaaditya.github.io/2016/03/one-by-one-convolution/)。

> 策略 2\. 减少剩余 3x3 卷积核的输入数量

这个策略通过基本上使用更少的卷积核来减少参数数量。这是通过将“压缩”层输入到他们称之为“扩展”层中系统地完成的，如下所示：

[![Squeeze](../Images/ad25266823d18359a7a53c22511ff7cb.png)](https://cdn-images-1.medium.com/max/1000/1*C4Y78hoaN0hPxyWJnkG5vQ.png)

*“Fire 模块”。图片来源于论文*

现在是定义一些仅在这篇论文中出现的术语的时候了！正如你在上面看到的，“squeeze”层是由仅包含1x1滤波器的卷积层组成，“expand”层是包含1x1和3x3滤波器混合的卷积层。通过减少“squeeze”层中进入“expand”层的滤波器数量，他们减少了进入这些3x3滤波器的连接数量，从而减少了总参数数量。论文的作者将这种特定的架构称为“火焰模块”，它作为 SqueezeNet 架构的基本构建块。

> 策略 3\. 在网络的后期进行下采样，以便卷积层具有大的激活图。

现在我们已经讨论了减少我们正在处理的参数数量的方法，那么如何充分利用剩余的较小参数集呢？作者认为，通过在后续卷积层中减小步幅，从而在网络后期创建更大的激活/特征图，分类准确性实际上会提高。在网络末尾拥有更大的激活图，与像 [VGG](https://arxiv.org/abs/1409.1556)这样的网络形成了鲜明对比，在这些网络中，激活图在接近网络末端时会变得更小。这种不同的方法非常有趣，他们引用了一篇 [K. He 和 H. Sun 的论文](https://arxiv.org/abs/1412.1710)，该论文类似地应用了延迟下采样，从而提高了分类准确性。

那么，这一切是如何结合在一起的呢？

SqueezeNet 利用上述的“火焰模块”，将这些模块串联在一起，从而实现了一个更小的模型。以下是他们论文中展示的这种串联过程的一些变体：

[![串联过程变体](../Images/7aae430c3c14b8c7ce9e2666873ff69b.png)](https://cdn-images-1.medium.com/max/1000/1*QJGepE_JorGO1LlI0Qy1yA.png)

*左侧：SqueezeNet；中间：带有简单旁路的 SqueezeNet；右侧：带有复杂旁路的 SqueezeNet。图片来自论文。*

我发现这个架构令人惊讶的一点是缺乏全连接层。令人吃惊的是，通常在像 [VGG](https://arxiv.org/abs/1409.1556)这样的网络中，后期的全连接层学习的是CNN早期高层特征与网络试图识别的类别之间的关系。也就是说，全连接层学习的是鼻子和耳朵组成一个脸，而车轮和灯光指示车辆。然而，在这个架构中，这一步额外的学习似乎嵌入在不同“火焰模块”之间的转换中。作者从 [NiN 架构](https://arxiv.org/abs/1312.4400)中获得了这一想法的灵感。

够详细了！这表现如何？

论文的作者展示了一些令人印象深刻的结果。

[![结果](../Images/9ce8e13b58af9a994e6d24879114181c.png)](https://cdn-images-1.medium.com/max/1000/1*RaSehomyb8PZbpg2xnTamQ.png)

*SqueezeNet对比其他CNN架构的基准测试。图像来自论文。*

他们的SqueezeNet架构在模型大小上实现了比AlexNet减少50倍的效果，同时达到了或超过了AlexNet的Top-1和Top-5准确率。但这篇论文最有趣的部分可能是他们将深度压缩（在[我们之前的帖子](https://gab41.lab41.org/lab41-reading-group-deep-compression-9c36064fb209#.qy0o30qcv)中进行了讲解）应用于他们已经更小的模型。这种深度压缩的应用使得模型比AlexNet小了510倍！这些结果非常鼓舞人心，因为它展示了结合不同压缩方法的潜力。作为下一步，我很希望看到这种设计思维如何应用于其他神经网络架构和深度学习应用。

如果这篇论文对你有兴趣，绝对要查看他们在Github上的开源[代码](https://github.com/DeepScale/SqueezeNet)。我只有时间介绍要点，但[他们的论文](https://arxiv.org/abs/1602.07360)中充满了关于参数减少CNN设计的深入讨论。

**[Abhinav Ganesh](https://www.linkedin.com/in/abhinav-ganesh-689b0953)** 目前是Lab41的工程师，致力于将机器学习应用于网络安全。他拥有卡内基梅隆大学电气与计算机工程的学士和硕士学位。

**[Lab41](http://www.lab41.org)** 是一个“挑战实验室”，美国情报界与学术界、工业界和In-Q-Tel的同行汇聚在一起，解决大数据问题。它允许来自不同背景的参与者获取想法、人才和技术，以探索数据分析中的有效性和不足之处。Lab41提供了一个开放、协作的环境，促进了参与者之间的宝贵关系。

[原文](https://gab41.lab41.org/lab41-reading-group-squeezenet-9b9d1d754c75)。经许可转载。

**相关：**

+   [深度学习阅读小组：用于图像识别的深度残差学习](/2016/09/deep-learning-reading-group-deep-residual-learning-image-recognition.html)

+   [深入了解深度学习：七月更新](/2016/08/deep-learning-july-update.html)

+   [理解深度学习的7个步骤](/2016/01/seven-steps-deep-learning.html)

* * *

## 我们的前3名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

### 更多相关内容

+   [利用 AI 读取思维：研究人员将脑波转化为图像](https://www.kdnuggets.com/2023/03/reading-minds-ai-researchers-translate-brain-waves-images.html)

+   [逐步指南：阅读和理解 SQL 查询](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

+   [2024 阅读清单：5 本关于人工智能的必读书籍](https://www.kdnuggets.com/2024-reading-list-5-essential-reads-on-artificial-intelligence)

+   [SQL 的 Group By 和 Partition By 场景：何时及如何组合数据…](https://www.kdnuggets.com/sql-group-by-and-partition-by-scenarios-when-and-how-to-combine-data-in-data-science)

+   [KDnuggets 新闻，6 月 8 日：21 张数据科学备忘单…](https://www.kdnuggets.com/2022/n23.html)

+   [学习数据科学、机器学习和深度学习的全面计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)
