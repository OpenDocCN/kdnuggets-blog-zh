# 卷积神经网络的直观解释

> 原文：[https://www.kdnuggets.com/2016/11/intuitive-explanation-convolutional-neural-networks.html/3](https://www.kdnuggets.com/2016/11/intuitive-explanation-convolutional-neural-networks.html/3)

#### 综合起来—使用反向传播进行训练

如上所述，卷积+池化层作为从输入图像中提取特征的工具，而全连接层作为分类器。

注意在**图15**中，由于输入图像是一艘船，目标概率对于船类是1，对于其他三个类别是0，即

+   输入图像 = 船

+   目标向量 = [0, 0, 1, 0]

![Screen Shot 2016-08-07 at 9.15.21 PM.png](../Images/5bb4a90b9c753c8a606458431de7e989.png)

###### 图15：训练ConvNet

卷积网络的整体训练过程可以总结如下：

+   **步骤1：** 我们用随机值初始化所有过滤器和参数/权重

+   **步骤2：** 网络将一张训练图像作为输入，经过前向传播步骤（包括卷积、ReLU和池化操作以及全连接层中的前向传播），并找到每个类别的输出概率。

    +   假设上述船图像的输出概率是[0.2, 0.4, 0.1, 0.3]

    +   由于权重在第一次训练示例中是随机分配的，输出概率也是随机的。

+   **步骤3：** 计算输出层的总误差（对所有4个类别进行求和）

    +   **总误差 = ∑  ½ (目标概率 – 输出概率)²**

+   **步骤4：** 使用反向传播计算相对于网络中所有权重的*梯度*，并使用*梯度下降*更新所有过滤器值/权重和参数值，以最小化输出误差。

    +   权重根据它们对总误差的贡献进行调整。

    +   当再次输入相同的图像时，输出概率现在可能是[0.1, 0.1, 0.7, 0.1]，这更接近目标向量[0, 0, 1, 0]。

    +   这意味着网络已经*学会*通过调整其权重/过滤器以减少输出误差，从而正确分类这张特定的图像。

    +   像过滤器数量、过滤器大小、网络结构等参数在步骤1之前已固定，并且在训练过程中不会改变——只有过滤器矩阵和连接权重的值会被更新。

+   **步骤5：** 对训练集中的所有图像重复步骤2-4。

上述步骤*训练*了ConvNet——这基本上意味着ConvNet的所有权重和参数现在已经优化，以正确分类训练集中的图像。

当一个新的（未见过的）图像输入到 ConvNet 中时，网络将经过前向传播步骤，为每个类别输出一个概率（对于新图像，输出概率是使用已优化的权重来正确分类所有之前训练样本计算的）。如果我们的训练集足够大，网络将（希望）能很好地推广到新图像，并将其分类到正确的类别中。

**注意 1：** 上述步骤已被过度简化，并且为了提供对训练过程的直观理解，避开了数学细节。有关数学公式和深入理解，请参见 [4] 和 [12]。

**注意 2：** 在上述示例中，我们使用了两个交替的卷积和池化层。不过，请注意，这些操作可以在单个 ConvNet 中重复任意次数。实际上，今天一些表现最好的 ConvNet 具有数十个卷积和池化层！此外，并不一定要在每个卷积层后都有一个池化层。如**图 16**所示，我们可以在进行池化操作之前连续进行多个卷积 + ReLU 操作。还请注意下图 16 中每层卷积网络的可视化。

![car.png](../Images/408e15a0f1b5418c7f4a5849f161bdb3.png)

###### 图 16：来源 [4]

#### 卷积神经网络的可视化

一般来说，卷积步骤越多，我们的网络能够学习识别的特征就会越复杂。例如，在图像分类中，卷积网络可能会在第一层从原始像素中学习检测边缘，然后在第二层使用这些边缘检测简单的形状，再在更高层使用这些形状检测更高层次的特征，如面部轮廓 [14]。这在下面的**图 17**中得到了演示——这些特征是使用 [卷积深度置信网络](http://web.eecs.umich.edu/~honglak/icml09-ConvolutionalDeepBeliefNetworks.pdf) 学到的，此图仅用于展示该思想（这只是一个例子：现实中的卷积滤波器可能会检测到对人类没有意义的对象）。

![Screen Shot 2016-08-10 at 12.58.30 PM.png](../Images/b2e500c81759b10bf09e7ce4132e932a.png)

###### 图 17：来自卷积深度置信网络的学习特征

Adam Harley 创建了卷积神经网络在 MNIST 手写数字数据库上训练的惊人可视化。我强烈推荐 [玩一玩这个](http://scs.ryerson.ca/~aharley/vis/conv/flat.html) 以了解 CNN 如何工作的细节。

我们将下面看到网络如何处理输入‘8’。请注意，**图 18** 中的可视化并未单独展示 ReLU 操作。

![conv_all.png](../Images/1cd0018da396b25aab0d675ef4fbfae2.png)

###### 图 18：可视化一个训练过的卷积网络在手写数字上的效果

输入图像包含 1024 个像素（32 x 32 图像），第一卷积层（卷积层 1）是通过将六个独特的 5 × 5（步幅为 1）滤波器与输入图像进行卷积形成的。如所见，使用六个不同的滤波器会产生深度为六的特征图。

卷积层 1 后面跟着池化层 1，该层对卷积层 1 中的六个特征图进行 2 × 2 最大池化（步幅为 2）。你可以将鼠标指针移动到池化层中的任何像素上，并观察它在前一卷积层中形成的 4 x 4 网格（如**图 19**所示）。你会注意到，4 x 4 网格中值最大的像素（最亮的一个）进入池化层。

![Screen Shot 2016-08-06 at 12.45.35 PM.png](../Images/c92ebd4788136abee4a516bfb643eea5.png)

###### 图 19：可视化池化操作

池化层 1 后面跟着十六个 5 × 5（步幅为 1）卷积滤波器，执行卷积操作。随后是池化层 2，该层进行 2 × 2 最大池化（步幅为 2）。这两层使用与上述相同的概念。

然后我们有三个全连接（FC）层。它们是：

+   第一 FC 层中的 120 个神经元

+   第二 FC 层中的 100 个神经元

+   第三 FC 层中的 10 个神经元对应 10 个数字——也称为输出层

请注意在**图 20**中，输出层中的每个 10 个节点都连接到第二个全连接层中的所有 100 个节点（因此得名全连接）。

另外，请注意输出层中唯一明亮的节点对应于 ‘8’——这意味着网络正确地分类了我们的手写数字（更亮的节点表示其输出更高，即 8 在所有数字中具有最高的概率）。

![final.png](../Images/2ba5a08fc4e55407d80314354c093f4b.png)

###### 图 20：可视化全连接层

相同的 3D 版本可在 [这里](http://scs.ryerson.ca/~aharley/vis/conv/) 查看。

#### 其他 ConvNet 架构

卷积神经网络自 1990 年代早期以来就存在。我们讨论了上面的 LeNet，这是最早的卷积神经网络之一。其他一些有影响力的架构列在下面 [3] [4]。

+   **LeNet (1990s)：** 已在本文中介绍。

+   **1990 年代至 2012 年：** 从 1990 年代末到 2010 年代初，卷积神经网络处于孕育阶段。随着数据和计算能力的不断增加，卷积神经网络可以处理的任务变得越来越有趣。

+   **AlexNet (2012) –** 2012年，Alex Krizhevsky（及其他人）发布了[AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)，这是一个比LeNet更深、更宽的版本，并且以较大优势赢得了2012年的ImageNet大型视觉识别挑战赛（ILSVRC）。这一突破性进展相较于之前的方法具有显著改进，目前广泛应用的卷积神经网络（CNN）可以归功于这项工作。

+   **ZF Net (2013) –** ILSVRC 2013的冠军是Matthew Zeiler和Rob Fergus的卷积网络。它被称为[ZFNet](http://arxiv.org/abs/1311.2901)（即Zeiler & Fergus Net的缩写）。通过调整架构超参数，它对AlexNet进行了改进。

+   **GoogLeNet (2014) –** ILSVRC 2014的冠军是来自[Szegedy et al.](http://arxiv.org/abs/1409.4842)的Google卷积网络。其主要贡献是开发了一个*Inception Module*，显著减少了网络中的参数数量（4M，相较于AlexNet的60M）。

+   **VGGNet (2014) –** ILSVRC 2014的亚军是被称为[VGGNet](http://www.robots.ox.ac.uk/~vgg/research/very_deep/)的网络。它的主要贡献在于展示了网络的深度（层数）是良好性能的关键因素。

+   **ResNets (2015) –** 由Kaiming He（及其他人）开发的[Residual Network](http://arxiv.org/abs/1512.03385)赢得了ILSVRC 2015。ResNets目前是最先进的卷积神经网络模型，并且是实际应用中使用卷积神经网络的默认选择（截至2016年5月）。

+   **DenseNet (2016年8月) –** 最近由Gao Huang（及其他人）发布的[Densely Connected Convolutional Network](http://arxiv.org/abs/1608.06993)中，每一层都直接与其他所有层进行前馈连接。DenseNet在五个高度竞争的物体识别基准任务上相较于之前的最先进架构取得了显著的改进。可以在[这里](https://github.com/liuzhuang13/DenseNet)查看Torch实现。

#### 结论

在这篇文章中，我尝试用简单的术语解释卷积神经网络的主要概念。有几个细节我进行了简化或跳过，但希望这篇文章能为你提供一些关于它们如何工作的直观理解。

本文最初受到了[Denny Britz的“理解卷积神经网络用于NLP”](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)的启发（我建议你阅读一下），本文中许多解释基于该文章。为了更深入地理解这些概念，建议你查阅[斯坦福大学卷积神经网络课程的笔记](http://cs231n.github.io/)以及下方参考文献中提到的其他优秀资源。如果你在理解上述概念时遇到任何问题或有问题/建议，欢迎在下方留言。

本文中使用的所有图片和动画均属于其各自的作者，详细信息请参见下方的参考文献部分。

#### 参考文献

1.  [Clarifai 主页](https://www.clarifai.com/)

1.  任少卿，*等*，“Faster R-CNN：基于区域提议网络的实时目标检测”，2015，[arXiv:1506.01497](http://arxiv.org/pdf/1506.01497v3.pdf)

1.  [神经网络架构](http://culurciello.github.io/tech/2016/06/04/nets.html)，Eugenio Culurciello的博客

1.  [CS231n 视觉识别的卷积神经网络，斯坦福大学](http://cs231n.github.io/convolutional-networks/)

1.  [Clarifai / 技术](https://www.clarifai.com/technology)

1.  [机器学习很有趣！第3部分：深度学习与卷积神经网络](https://medium.com/@ageitgey/machine-learning-is-fun-part-3-deep-learning-and-convolutional-neural-networks-f40359318721#.2gfx5zcw3)

1.  [使用卷积的特征提取，斯坦福大学](http://deeplearning.stanford.edu/wiki/index.php/Feature_extraction_using_convolution)

1.  [关于卷积核（图像处理）的维基百科文章](https://en.wikipedia.org/wiki/Kernel_(image_processing))

1.  [视觉的深度学习方法，CVPR 2012教程](http://cs.nyu.edu/~fergus/tutorials/deep_learning_cvpr12)

1.  [Rob Fergus的神经网络，2015年机器学习夏令营](http://mlss.tuebingen.mpg.de/2015/slides/fergus/Fergus_1.pdf)

1.  [CNN中的全连接层有什么作用？](http://stats.stackexchange.com/a/182122/53914)

1.  [卷积神经网络，Andrew Gibiansky](http://andrew.gibiansky.com/blog/machine-learning/convolutional-neural-networks/)

1.  A. W. Harley，“卷积神经网络的交互式节点-链接可视化”，发表于ISVC，第867-877页，2015 ([link](http://scs.ryerson.ca/~aharley/vis/harley_vis_isvc15.pdf))

1.  [理解卷积神经网络用于NLP](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)

1.  [卷积神经网络中的反向传播](http://andrew.gibiansky.com/blog/machine-learning/convolutional-neural-networks/)

1.  [理解卷积神经网络的初学者指南](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks-Part-2/)

1.  Vincent Dumoulin, *等人*, “深度学习卷积算术指南”，2015年，[arXiv:1603.07285](http://arxiv.org/pdf/1603.07285v1.pdf)

1.  [深度学习与普通机器学习的区别是什么？](https://github.com/rasbt/python-machine-learning-book/blob/master/faq/difference-deep-and-normal-learning.md)

1.  [卷积神经网络如何学习不变特征？](https://www.quora.com/How-is-a-convolutional-neural-network-able-to-learn-invariant-features)

1.  [计算机视觉的深度卷积神经网络分类法](http://journal.frontiersin.org/article/10.3389/frobt.2015.00036/full)

[原创帖子](https://ujjwalkarn.me/2016/08/11/intuitive-explanation-convnets/)：转载经许可。

**简介：** [Ujjwal Karn](https://ujjwalkarn.me/) 在机器学习领域拥有 3 年的行业和研究经验，并对将深度学习应用于语言和视觉理解的实际应用感兴趣。

**相关：**

+   [深度学习清除播客中的‘咳嗽’声](/2016/11/deep-learning-cleans-podcast-ahem-sounds.html)

+   [卷积神经网络的工作原理](/2016/08/brohrer-convolutional-neural-networks-explanation.html)

+   [深度学习关键术语解释](/2016/10/deep-learning-key-terms-explained.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [协同过滤的直观解释](https://www.kdnuggets.com/2022/09/intuitive-explanation-collaborative-filtering.html)

+   [使用卷积神经网络（CNNs）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [卷积神经网络综合指南](https://www.kdnuggets.com/2023/06/comprehensive-guide-convolutional-neural-networks.html)

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [支持向量机：直观方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)
