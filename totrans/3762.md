# 深度学习可以应用于自然语言处理

> 原文：[https://www.kdnuggets.com/2017/01/deep-learning-applied-natural-language-processing.html](https://www.kdnuggets.com/2017/01/deep-learning-applied-natural-language-processing.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![](../Images/d7c1e236ba300848f00cebec470c16f1.png)

*[图片来源](https://unsplash.com/collections/144450/music-learning-blog-collateral?photo=gN_nIUnjYJI)*

在 LinkedIn 上流传着一篇尝试对深度学习在自然语言处理领域应用提出反对意见的文章。Riza Berkan 撰写的文章 “[谷歌在炒作吗？为什么深度学习不能轻易应用于自然语言](http://www.linkedin.com/pulse/google-hyping-why-deep-learning-cannot-applied-easily-berkan-ph-d?trk=hp-feed-article-title-comment)” 提出了深度学习不可能工作的几个论点，并声称谷歌夸大了其主张。后者的论点当然有些接近阴谋论。

Yannick Vesley 写了一篇反驳文章 “[神经网络非常不错：对 Riza Berkan 的回复](https://www.linkedin.com/pulse/neural-networks-quite-neat-reply-riza-berkan-yannick-versley?deepLinkCommentId=6221869083437072384&anchorTime=1483409186231&trk=hb_ntf_MEGAPHONE_REPLY_TOP_LEVEL_COMMENT)” ，他对 Berkan 提出的每一点进行了辩论。Vesley 的观点确实很到位，但人们不能忽视深度学习理论中还有一些未解之谜。

然而，在我深入探讨之前，我认为读者了解深度学习目前仍是一门实验性科学是非常重要的。也就是说，深度学习的能力实际上是研究人员偶然发现的。确实有很多工程工作涉及到这些机器的优化和改进。然而，它的能力是‘不合理有效的’，简而言之，我们没有很好的理论来解释它的能力。

显然，理解中存在的差距至少在三个开放问题上：

1.  深度学习如何在高维离散空间中进行搜索？

1.  深度学习如何在看似进行死记硬背的情况下实现泛化？

1.  (1) 和 (2) 如何从简单组件中产生？

Berkan 的论点利用了我们当前对缺乏坚实解释的现状，并提出了他自己替代的方法。他主张符号主义方法是救赎之路。不幸的是，他的论点中没有揭示符号主义方法的脆弱性、缺乏泛化性和扩展性。有没有人创建过一个基于规则的系统，能够根据低级特征分类图像，与深度学习相媲美？我认为没有。

然而，深度学习从业者并没有因为缺乏严密的理论基础而停止工作。深度学习确实有效，而且效果出奇地好。当前状态下的深度学习是一门实验科学，很明显，在背后有些我们尚未完全理解的东西。理解的缺失并不会使这种方法无效。

为了更好地理解这些问题，我在早期的一篇文章中写道“[深度学习系统中的架构特性](https://medium.com/intuitionmachine/architecture-ilities-for-deep-learning-12a3ff9bec4e#.rpjhp26b0)”。我基本上阐述了深度学习中的三种能力：

+   **可表达性** — 这个特性描述了机器能够逼近普遍函数的程度。

+   **可训练性** — 深度学习系统学习其问题的效率和速度。

+   **泛化能力** — 机器在未训练过的数据上进行预测的能力。

当然，还有其他能力也需要在深度学习中考虑：可解释性、模块化、可迁移性、延迟、对抗稳定性和安全性。但这些是主要的。

要正确解释这些内容，我们必须考虑最新的实验证据。我在这里写过一篇文章“[重新思考泛化](https://medium.com/intuitionmachine/rethinking-generalization-in-deep-learning-ec66ed684ace#.h418bb3it)”，我再总结一下：

ICLR 2017 提交的“[理解深度学习需要重新思考泛化](https://openreview.net/pdf?id=Sy8gdB9xx)”肯定会颠覆我们对深度学习的理解。以下是他们通过实验发现的总结：

> 1. 神经网络的有效容量足够大，可以对整个数据集进行暴力记忆。
> 
> 2. 即使在随机标签上进行优化仍然很容易。事实上，与在真实标签上训练相比，训练时间仅增加了一个小常数因子。
> 
> 3. 随机化标签仅仅是一种数据转换，不改变学习问题的所有其他属性。

令大多数机器学习从业者感到惊讶的是“暴力记忆”。你看，机器学习一直关注于曲线拟合。在曲线拟合中，你找到一组稀疏的参数来描述你的曲线，然后用它来拟合数据。泛化涉及在点之间插值的能力。这里的主要断层是，深度学习表现出了令人印象深刻的泛化能力，但如果我们仅将其视为记忆存储，这种方法显然行不通。

然而，如果我们把它们视为全息记忆存储，那么泛化问题就有了相当好的解释。在“[深度学习是全息记忆](https://medium.com/intuitionmachine/deep-learning-machines-are-holographic-memories-258272422995#.zgf8bjj77)”中，我指出了实验证据：

> Swapout 学习过程告诉我们，如果你对整个网络的任何子网络进行抽样，得到的预测将与任何其他子网络的预测相似。就像全息记忆一样，你可以切割出一部分，仍然可以重建整个记忆。

事实证明，宇宙本身由一个类似的理论驱动，称为全息原理。实际上，这为开始更扎实的深度学习能力解释提供了一个很好的基础营。我介绍了“[全息原理：深度学习为何有效](https://medium.com/intuitionmachine/the-holographic-principle-and-deep-learning-52c2d6da8d9#.fsgytefke)”，在其中我介绍了一种使用张量网络的技术方法，该方法将高维问题空间缩减为可在可接受响应时间内计算的空间。

所以，再次回到关于 NLP 是否可以通过深度学习方法处理的问题。我们当然知道它是可行的，毕竟，你不是正在阅读和理解这篇文本吗？

在专家数据科学家和机器学习从业者中确实存在许多困惑。当我撰写“[11 个专家对深度学习的误解](https://www.linkedin.com/pulse/11-arguments-experts-get-wrong-deeplearning-carlos-e-perez?articleId=6219628719263215617)”时，我意识到存在这种“反对”。然而，深度学习可能最好的解释是一种简单的直觉，可以 [用五岁孩子能理解的方式解释](https://medium.com/intuitionmachine/deep-learning-explained-to-a-five-year-old-25919b0bf889#.weckq9twr)：

> **DE3p Larenn1g wrok smliair to hOw biarns wrok.**
> 
> 这些机器通过识别完全的模式并将其连接到完整的概念来工作。它们按层工作，就像滤镜一样，将复杂的场景分解成简单的想法。

符号系统无法读取这些内容，但人类可以。

在2015年，NLP 从业者 Chris Manning 讨论了有关深度学习的领域关切（见：[计算语言学与深度学习](http://www.mitpressjournals.org/doi/pdf/10.1162/COLI_a_00239)）。了解他的论点非常重要，因为这些论点并不与深度学习的能力相冲突。他的两个论点说明了为何 NLP 专家不必担心如下：

> （1）对于我们的领域来说，最聪明和最有影响力的机器学习专家将 NLP 作为重点关注领域是非常令人振奋的；（2）我们的领域是语言技术的领域科学；它不是关于最佳的机器学习方法——核心问题仍然是领域问题。

第一个论点并不是对深度学习的批评。第二个论点解释了他不相信一种适用于所有领域的通用机器学习方法。这与上述全息原理的方法并不冲突，该方法强调了网络结构的重要性。

总之，我希望这篇文章能终结关于深度学习在自然语言处理领域不适用的讨论。

如果你仍然不信服，那么也许 Chris Manning 自己应该来说服你：

**简介：[Carlos Perez](http://www.linkedin.com/in/ceperez)** 是一名软件开发人员，目前正在撰写《深度学习设计模式》一书。这是他为博客文章提供灵感的来源。

[原文](https://medium.com/intuitionmachine/why-deep-learning-can-be-applied-to-natural-languages-46c74a6f861f#.wc79wpm3g)。经许可转载。

**相关：**

+   [深度学习为何与机器学习截然不同](/2016/12/deep-learning-radically-different-machine-learning.html)

+   [深度学习智能的五个能力层次](/2016/12/5-capability-levels-deep-learning-intelligence.html)

+   [博弈论揭示深度学习的未来](/2016/12/game-theory-reveals-future-deep-learning.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，找到目标后…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败案例研究](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应了解的三种 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
