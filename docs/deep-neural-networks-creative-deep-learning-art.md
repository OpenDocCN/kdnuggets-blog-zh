# 深度神经网络是否具有创意？

> 原文：[https://www.kdnuggets.com/2016/05/deep-neural-networks-creative-deep-learning-art.html](https://www.kdnuggets.com/2016/05/deep-neural-networks-creative-deep-learning-art.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

深度神经网络是否具有创意？这似乎是一个合理的问题。谷歌的 ["Inceptionism"](https://googleresearch.blogspot.com/2015/06/inceptionism-going-deeper-into-neural.html) 技术通过迭代修改图像，增强深层网络中特定神经元的激活。图像看起来 *迷幻*，将岩石变成建筑物或叶子变成昆虫。另一种由德国图宾根大学的 Leon Gatys 提出的神经生成模型，[可以从一张图像（例如梵高的画作）](https://www.wired.co.uk/news/archive/2015-09/01/art-algorithm-recreates-paintings) 中提取风格，并将其应用到另一张图像（例如照片）的内容中。

[生成对抗网络（GANs），由 Ian Goodfellow 提出的](http://arxiv.org/abs/1406.2661)，通过建模已见图像的分布来合成新颖图像。此外，字符级递归神经网络（RNN）语言模型现在遍布互联网，似乎能够幻想出 [莎士比亚的片段](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)、Linux 源代码，甚至 [特朗普的仇恨推文](https://twitter.com/deepdrumpf)。显然，这些进展源自有趣的研究，并值得它们所激发的迷恋。

[![computer-deep-learning-algorithm-painting-masters-fb__700](../Images/334d2d490f5951174898b30ba07c7b8c.png)](/wp-content/uploads/computer-deep-learning-algorithm-painting-masters-fb__700.jpg)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

在这篇博客文章中，我们不讨论工作的质量（这点值得称赞），也不解释方法（这已被做过无数次），而是探讨一个问题：这些网络是否可以合理地称为“创意”？已经有人提出这一主张。[deepart.io](deepart.io) 的着陆页宣称“将你的照片转变为艺术”。如果我们接受创意是艺术的前提条件，那么这里隐含了这一主张。

在[Engadget.com上的一篇文章](http://www.engadget.com/2015/12/02/neural-network-journalism-philip-k-dick/)中，亚伦·苏普里斯描述了一个基于字符的RNN，建议较高的采样温度使网络“更具创造力”。在这种观点中，创造力与连贯性相对立。

我们能否接受一种主要由随机性构成的创造力观？接受一种由随机数生成器最大化创造力的定义是否有意义？在这种观点下，创造力归结为熵的最大化。有趣的是，这似乎与深度学习先驱尤尔根·施密特胡伯（Juergen Schmidhuber）所提出的创造力观点相反，[他建议低熵，即短描述长度，是艺术的定义特征。](http://people.idsia.ch/~juergen/locoart/locoart.html) 更重要的是，这似乎与我们赋予人类的创造力观念相矛盾。将查理·帕克、贝多芬、陀思妥耶夫斯基和毕加索标记为创造性似乎没有争议，但他们的作品显然是连贯的。[![charlie-parker-001](../Images/b80a7b5409ff69e8e5a874f72b400ccd.png)](/wp-content/uploads/charlie-parker-001.jpg)

**采样是不够的**

我们似乎急于将创造力归因于的算法，往往可以分为两类。首先是像Inceptionism和Deep Style这样的确定性过程。其次是随机模型，如RNN语言模型和生成对抗网络，它们建模了训练数据下的分布，为从近似分布中生成新图像/文本提供了采样机制。*注：给定随机的自动编码器权重，Deep Style可以被归入随机生成机制的类别。同样，通过随机选择节点进行增强，Inceptionism也可以被随机化。*

在确定性情况下，创造力进入过程的两个地方。首先，《Inceptionism》和《Deep Style》的创作者本身极具创造力。这些论文介绍了以前不存在的新颖而引人注目的工具。其次，这些工具的使用者可以对处理哪些图像以及使用哪些模型参数设置做出创造性的决策。但当然，就像相机不能被真实地描述为创造性一样，这些工具也没有自主性。虽然这些能力令人印象深刻，但就创造性过程而言，这些工具充当了超酷的Photoshop滤镜，但本身并不表达创造力。

评估随机模型的创造能力似乎更为困难。生成对抗网络和RNN能够自行生成图像，仅需一个随机种子，这引起了我们的想象力。最近的研究论文和博客文章，包括我自己的，倾向于将这些模型拟人化，将其生成过程描述为“幻觉”的行为。然而，我认为这些模型虽然令人兴奋且充满潜力，但尚未达到可以称之为“创造性”的合理标准。

想想飞机。曾几何时，地球上没有飞机。然后一些富有创造力的人发明了飞机。当然，在没有飞机的世界中，从现有技术的分布中进行任何随机抽样都无法产生飞机。我们用来形容创造力的最极致的术语，并不是指那些生产出模仿现有事物的无偏样本的个人，而是指那些具有发散思维能力的人，他们能够同时产生引人注目且令人惊讶的想法和艺术。这项工作并不反映以前工作的分布，而是实际改变了它。也就是说，在飞机发明之后，观察到飞机的概率大大增加了。

同样的观点也适用于被认为非常有创造力的艺术家。例如，巴赫通过发展对位法改变了音乐创作，或者查理·帕克，他的即兴演奏语言，被称为比波普，改变了数十年和多种音乐风格中的器乐即兴演奏。相反，那些能够令人信服地模仿的艺术家和作家，可能更准确地被描述为技艺高超而非创造力。介绍生成对抗网络（GANs）和深度风格的论文之所以具有创造性，正是因为它们开辟了新的研究领域。

[![鸟-飞机](../Images/da1e121caca762b1315945889595ce3c.png)](/wp-content/uploads/bird-airplane.jpg)

**建立创造性机器是否可能？**

有必要澄清的是，我对创造性机器的概念没有宗教上的对立。人脑本身就是一种创造性机器，认为创造力是碳的属性，无法在硅中复制，这种说法是荒谬的。

然而，在将人类的创造力美德归于今天的模型之前，我们应该犹豫。确定性图像生成器非常吸引人，但并不具备自主性。产生逼真输出的随机模型可能具备某种类似于技艺的特征。但要让人工智能展现创造力，它必须做的不仅仅是模仿。也许强化学习者在发现新策略时确实展现了创造力。例如，AlphaGo在围棋中表现出了超人的水平，[发现了与人类玩家已知策略有巨大差异的策略](https://www.washingtonpost.com/news/innovations/wp/2016/03/15/what-alphagos-sly-move-says-about-machine-creativity/)。这个系统可以合理地称为具有创造性。然而，人类的创造力不仅体现在产生策略方面，也体现在发明需要解决的问题的更广泛领域。真正具有创造力的人工智能似乎是不可避免的。虽然我们应该认真考虑这一可能性，但我们也应警惕过早的宣告和将算法拟人化的诱惑。

![Zachary Chase Lipton](../Images/240b273c667af1a53a99fd93d1fd39ce.png) **[Zachary Chase Lipton](http://zacklipton.com)** 是加州大学圣地亚哥分校计算机科学工程系的博士生。在[生物医学信息学部](http://healthsciences.ucsd.edu/som/medicine/divisions/dbmi/pages/default.aspx)资助下，他对机器学习的理论基础和应用都很感兴趣。除了在UCSD的工作外，他还曾在微软研究院实习，担任亚马逊的机器学习科学家，并且是KDnuggets的贡献编辑。

**相关：**

+   [深度学习是否来自魔鬼？](/2015/10/deep-learning-vapnik-einstein-devil-yandex-conference.html)

+   [MetaMind 与 IBM Watson Analytics 和 Microsoft Azure Machine Learning 竞争](/2015/01/metamind-ibm-watson-analytics-microsoft-azure-machine-learning.html)

+   [深度学习与经验主义的胜利](/2015/07/deep-learning-triumph-empiricism-over-theoretical-mathematical-guarantees.html)

+   [模型可解释性的神话](/2015/04/model-interpretability-neural-networks-deep-learning.html)

+   [(深度学习的深层缺陷) 的深层缺陷](/2015/01/deep-learning-flaws-universal-machine-learning.html)

+   [数据科学中最常用、最混淆和滥用的行话](/2015/02/data-science-confusing-jargon-abused.html)

### 更多相关内容

+   [如何使用 GPT 生成创意内容与 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [深度神经网络不会引领我们走向 AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [最先进的深度学习的可解释预测和现在预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [神经网络与深度学习：一本教科书（第二版）](https://www.kdnuggets.com/2023/07/aggarwal-neural-networks-deep-learning-textbook-2nd-edition.html)

+   [在使用神经网络之前尝试的10件简单事情](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [使用 PyTorch 的可解释神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)
