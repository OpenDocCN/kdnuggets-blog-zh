# 神经网络 201：自编码器全解

> 原文：[`www.kdnuggets.com/2019/11/all-about-autoencoders.html`](https://www.kdnuggets.com/2019/11/all-about-autoencoders.html)

评论

**由 [Zak Jost](https://blog.zakjost.com)，亚马逊网络服务研究科学家**。

对于刚入门神经网络的人来说，自编码器可能看起来令人畏惧。 但实际上，它们是一个概念上简单而优雅的方法，将为机器学习从业者打开许多大门。 它们可以用于异常检测和缺失值填补，或帮助构建更好的分类器或聚类器。 无论如何，使它们独特的是它们为您提供了一个利用未标记数据的机制，这通常比获取标记数据要容易得多。 例如，获取一组图像要比获取一组每个图像都有标签的图像要容易得多。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

首先，自编码器通过 *无监督* 学习进行训练，这意味着您不需要标签。 自编码器通过使用自身的噪声版本来 *预测自己的输入*，这迫使其利用数据中的结构来学习紧凑的表示方式。 从高层次看，这意味着学会丢弃噪声细节，只保留重要的内容。 一旦您拥有一个能够将数据压缩到紧凑形式并丢弃细节的网络，这将打开许多新门。

为了更好地理解自编码器的工作原理以及如何使用它们，我制作了一个简短的视频，解释了关键概念。

**简介：** [Zak Jost](http://blog.zakjost.com/) ([@ZakJost](https://twitter.com/ZakJost)) 是亚马逊网络服务的机器学习研究科学家，专注于欺诈应用。在此之前，Zak 曾在 Capital One 担任首席数据科学家，构建大规模建模工具以支持业务的投资组合风险评估，并在半导体行业担任材料科学家，致力于薄膜纳米材料的开发。

**相关：**

+   [自编码器：使用 TensorFlow 的急切执行进行深度学习](https://www.kdnuggets.com/2019/05/autoencoders-deep-learning-with-tensorflows-eager-execution.html)

+   [变分自编码器详细解释](https://www.kdnuggets.com/2018/11/variational-autoencoders-explained.html)

+   [深度学习、维度灾难与自编码器](https://www.kdnuggets.com/2015/03/deep-learning-curse-dimensionality-autoencoders.html)

### 相关主题

+   [在神经网络之前尝试的 10 个简单方法](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [使用 PyTorch 的可解释神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [深度神经网络不会引导我们走向通用人工智能（AGI）](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [用最先进的深度学习进行可解释的预测和实时预测…](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [使用卷积神经网络（CNN）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [关于可信赖图神经网络的全面调查：…](https://www.kdnuggets.com/2022/05/comprehensive-survey-trustworthy-graph-neural-networks-privacy-robustness-fairness-explainability.html)
