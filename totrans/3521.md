# 使用迁移学习回收深度学习模型

> 原文：[https://www.kdnuggets.com/2015/08/recycling-deep-learning-representations-transfer-ml.html](https://www.kdnuggets.com/2015/08/recycling-deep-learning-representations-transfer-ml.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

深度学习模型无疑是许多机器感知问题的最先进技术。使用具有多层隐藏层的神经网络和数百万个参数，深度学习算法利用了硬件能力和海量数据集的丰富性。相比之下，[线性模型很快会饱和并且表现不佳。](/2015/03/more-training-data-or-complex-models.html) 但如果你没有大数据呢？

[![alex-net](../Images/cea8af8f7a15a3b311bf1c9aced99a13.png)](/wp-content/uploads/alex-net.jpeg)

假设你有一个新颖的研究问题，例如根据照片识别癌症痣。进一步假设你生成了一个包含10,000张标记图像的数据集。虽然这看起来像是大数据，但与深度学习取得最大成功的权威数据集相比，这只是微不足道的。似乎一切都已经失去。幸运的是，还有希望。

考虑斯坦福大学教授[李飞飞](http://vision.stanford.edu/feifeili/)创建的著名ImageNet数据库。该数据集包含数百万张图像，每张图像属于1000个不同的层次化组织的物体类别中的一个。正如[李博士在她的TED演讲中提到的](/2015/03/more-training-data-or-complex-models.html)，一个孩子在成长过程中会看到数百万张不同的图像。从直观上看，为了训练一个足够强大的模型，以便与人类物体识别能力竞争，可能需要一个类似规模的大型训练集。此外，在这种数据规模下占主导地位的方法，可能与在较小数据集上相同任务时的主导方法不同。李博士的洞察力迅速得到了回报。亚历克斯·克里日夫斯基、伊利亚·苏茨克维和杰弗里·辛顿轻松赢得了2012年ImageNet比赛，确立了卷积神经网络（CNN）在计算机视觉中的最先进地位。

[![imagenet_logo](../Images/cf46dc9cb368c60807a3f825a02b3827.png)](/wp-content/uploads/imagenet_logo.png)

扩展李博士的隐喻，虽然一个孩子在成长过程中会看到数百万张图像，但一旦长大，人类可以轻松学习识别一个新的物体类别，而无需从头学习如何看图像。现在考虑一个在ImageNet数据集上训练的CNN。给定来自1000个ImageNet类别中的任何一个类别的图像，神经网络的最上层隐藏层构成了一个足够灵活的单一向量表示，可以用于将图像分类到这1000个类别中的每一个。合理的推测是，这种表示也可能对一个尚未见过的额外物体类别有用。

按照这种思路，计算机视觉研究人员现在通常使用预训练的卷积神经网络（CNN）为新任务生成表示，其中数据集可能不足以从头训练整个CNN。另一种常见的策略是采用预训练的ImageNet网络，然后对整个网络进行微调以适应新任务。这通常通过端到端的反向传播和随机梯度下降来完成。我去年在与[Oscar Beijbom](http://vision.ucsd.edu/~beijbom/website/)的对话中首次了解了这种微调技术。Beijbom是UCSD的视觉研究员，他成功地使用这一技术来区分各种类型的珊瑚图像。

[去年NIPs会议上的一篇出色论文](http://arxiv.org/abs/1411.1792)，由康奈尔大学的Jason Yosinski与Jeff Clune、Yoshua Bengio和Hod Lipson合作完成，针对这个问题进行了严谨的实证调查。作者重点关注ImageNet数据集以及闻名于ImageNet的8层AlexNet架构。他们指出，卷积神经网络的最底层长期以来已被知道类似于传统的计算机视觉特征，如边缘检测器和Gabor滤波器。此外，他们指出，最上层隐藏层在某种程度上专门化于其训练任务。因此，他们系统地探讨了以下问题：

+   从广泛有用到高度专业化特征的过渡发生在哪里？

+   微调整个网络与冻结转移层的相对性能如何？

为了研究这些问题，作者将注意力限制在ImageNet数据集上，但考虑了将数据划分为子集A和B的两种50-50分割。在第一组实验中，他们通过随机将每个图像类别分配到子集A或子集B来分割数据。在第二组实验中，作者考虑了更具对比性的分割，将自然图像分配到一个集合，而将人造物体图像分配到另一个集合。

作者研究了一个案例，其中网络在任务A上进行预训练，然后在任务B上进一步训练，保留前k层并随机初始化。他们还考虑了相同的实验，但在任务B上进行预训练，随机初始化顶部的8-k层，然后继续在任务B上训练。

在所有情况下，他们发现对预训练网络进行端到端的微调优于完全冻结转移的k层。有趣的是，他们还发现当预训练层被冻结时，特征的迁移能力在预训练层的数量上是非线性的。他们假设这归因于相邻层之间节点的复杂相互作用，这些作用难以重新学习。

-   如果要浪漫地说，这可能类似于对一个完全成长的成年人进行半球切除术，将右半脑替换为一块空白的儿童大脑。忽略这个例子的生物学荒谬性，可能直观地认为，新的右脑半球在填补左半球已经完全适应的预期角色时会遇到困难。

[![半球切除术](../Images/6756492ffa3a95fef566cc13f22a5e15.png)](/wp-content/uploads/hemispherectomy.jpg)

-   我参考了[原始论文](http://arxiv.org/abs/1411.1792)以获得实验的完整描述。本文未涉及的一个问题，但可能会激发未来的研究，是当新任务的样本数量远少于原任务时，这些结果是否仍然有效。在实践中，原任务和新任务之间标签样本数量的不平衡，通常是激励迁移学习的原因。一个初步的方法可能是重复这项工作，但将类别的分割比例改为999比1，而不是500比500。

-   通常，数据标注是昂贵的。对于许多任务而言，可能甚至是不可承受的。因此，经济和学术界都对如何利用现有标签在任务中表现良好而产生深刻理解充满兴趣。Yosinski 的工作在这方面迈出了重要的一步。

![扎卡里·蔡斯·利普顿](../Images/240b273c667af1a53a99fd93d1fd39ce.png) **[扎卡里·蔡斯·利普顿](http://zacklipton.com)** 是加州大学圣地亚哥分校计算机科学工程系的博士生。在[生物医学信息学部](http://healthsciences.ucsd.edu/som/medicine/divisions/dbmi/pages/default.aspx)资助下，他对机器学习的理论基础和应用都很感兴趣。除了在UCSD的工作，他还曾在微软研究院实习。

**相关：**

+   [深度学习与经验主义的胜利](/2015/07/deep-learning-triumph-empiricism-over-theoretical-mathematical-guarantees.html)

+   [别急：质疑深度学习的智商结果](/2015/06/questioning-deep-learning-iq-results.html)

+   [模型可解释性的神话](/2015/04/model-interpretability-neural-networks-deep-learning.html)

+   [(深度学习的深层缺陷)](/2015/01/deep-learning-flaws-universal-machine-learning.html)

+   [数据科学中最常用、最混淆和滥用的术语](/2015/02/data-science-confusing-jargon-abused.html)

+   [差分隐私：如何使隐私与数据挖掘兼容](/2015/01/differential-privacy-data-mining-compatible.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 了解更多相关内容

+   [TensorFlow 在计算机视觉中的应用 - 迁移学习简化版](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [什么是迁移学习？](https://www.kdnuggets.com/2022/01/transfer-learning.html)

+   [图像识别和自然语言处理中的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [使用 PyTorch 进行迁移学习的实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [探索小数据场景中的迁移学习潜力](https://www.kdnuggets.com/exploring-the-potential-of-transfer-learning-in-small-data-scenarios)

+   [利用迁移学习提升模型性能](https://www.kdnuggets.com/using-transfer-learning-to-boost-model-performance)
