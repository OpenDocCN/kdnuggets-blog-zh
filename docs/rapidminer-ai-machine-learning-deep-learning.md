# 什么是人工智能、机器学习和深度学习？

> 原文：[https://www.kdnuggets.com/2017/07/rapidminer-ai-machine-learning-deep-learning.html](https://www.kdnuggets.com/2017/07/rapidminer-ai-machine-learning-deep-learning.html)

**由RapidMiner提供**。赞助帖子。

几乎每天我们都能在媒体上看到关于人工智能的新闻。以下是一些近期新闻标题的简短汇编：

+   [人工智能来到好莱坞——你的工作安全吗？](http://www.studiodaily.com/2017/04/artificial-intelligence-comes-hollywood/)

+   [这个机器人解释了为什么你不需要担心人工智能](http://www.marketwatch.com/story/this-robot-explains-why-you-shouldnt-worry-about-artificial-intelligence-2017-04-19)——没错，确实如此。一个会说话的机器人肯定能做到让那些反对者不再过度担忧…

+   [人工智能如何学习变得有种族歧视](http://www.vox.com/science-and-health/2017/4/17/15322378/how-artificial-intelligence-learns-how-to-be-racist)——简单来说：它在模仿我们…

+   [人工智能如何改变工程行业](http://www.trendintech.com/2017/04/19/how-artificial-intelligence-might-transform-the-engineering-industry/)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

有趣的是，大多数这些文章都有一种怀疑甚至是负面的语气。这种情绪也受到[比尔·盖茨](https://www.washingtonpost.com/news/the-switch/wp/2015/01/28/bill-gates-on-dangers-of-artificial-intelligence-dont-understand-why-some-people-are-not-concerned/?utm_term=.43c26215dd98)、[埃隆·马斯克](http://fortune.com/2017/03/27/data-sheet-elon-musk-artificial-intelligence/)甚至[斯蒂芬·霍金](http://www.bbc.com/news/technology-30290540)的言论推动。恕我直言，我不会在公共场合谈论关于虫洞的废话，也许我们都应该多关注一下自己的专业领域。

这一切突显了两点：1\. 人工智能和机器学习终于成为主流，**2\. 人们对这些技术了解惊人地少。**

关于这些话题也存在大量炒作。我们都听说过“线性回归”。这并不令人惊讶，因为它是[由勒让德和高斯200多年前发明的](https://en.wikipedia.org/wiki/Regression_analysis)。尽管如此，这种炒作过度可能导致人们在使用这种方法时被带偏。这里是我最喜欢的一些推文交流，体现了这一点：

[@katherinebailey](https://twitter.com/katherinebailey) 因为市场营销？每次有人把简单线性回归称为“人工智能”时，高斯都会在坟墓里翻身。

— RapidMiner (@RapidMiner) [2017年4月15日](https://twitter.com/RapidMiner/status/853246249327308800)

显然，这些术语周围存在很高的混淆。这篇文章应有助于你理解这些领域之间的差异以及它们之间的关系。让我们从下面的图片开始。它解释了三个术语：人工智能、机器学习和深度学习。

![人工智能、机器学习、深度学习](../Images/60199cdd455f97d723d285dc8f3ec709.png)

*人工智能*涵盖了任何使计算机表现得像人的东西。想想那个著名的（虽然有点过时的）[图灵测试](https://en.wikipedia.org/wiki/Turing_test)，用来确定是否如此。如果你在手机上跟Siri对话并得到答案，你已经很接近了。使用机器学习来更具适应性的自动交易系统也已经属于这一类别。

*机器学习*是人工智能的一个子集，处理从数据集中提取模式的问题。这意味着机器可以找到最佳行为的规则，还可以适应世界的变化。许多涉及的算法已经被知道了几十年甚至几个世纪。得益于计算机科学和并行计算的进步，它们现在可以扩展到庞大的数据量。

*深度学习*是使用复杂神经网络的机器学习算法的一种特定类别。从某种意义上说，它是一组相关技术，类似于“决策树”或“支持向量机”组。但由于并行计算的进步，它们最近得到了很多炒作，这就是为什么我在这里单独列出它们。正如你所看到的，深度学习是机器学习方法的一个子集。当有人解释深度学习与机器学习“[截然不同](https://medium.com/intuitionmachine/why-deep-learning-is-radically-different-from-machine-learning-945a4a65da4d)”时，他们是错误的。如果你想要了解不含BS的深度学习视角，看看我之前做的[这个网络研讨会](http://go.rapidminer.com/l/32612/2017-07-10/7xdcv6)。

但是，如果机器学习只是人工智能的一个子集，那么这个领域还有什么其他部分呢？以下是对这三个组中最重要的研究领域和方法的总结：

+   *人工智能：* 机器学习（当然！）、自然语言理解、语言合成、计算机视觉、机器人技术、传感器分析、优化与模拟等。

+   *机器学习：* 深度学习（再说一遍！）、支持向量机、决策树、贝叶斯学习、k均值聚类、关联规则学习、回归等等。

+   *深度学习：* 人工神经网络、卷积神经网络、递归神经网络、长短期记忆网络、深度置信网络等等。

正如你所见，每个领域都有数十种技术，研究人员每周都会生成新的算法。虽然这些算法可能很复杂，但**如上所述的概念差异却并不复杂**。

**我们希望这对你区分这些术语有所帮助。如果你对机器学习感兴趣，可以查看这篇关于** [**如何正确验证机器学习模型**](http://go.rapidminer.com/l/32612/2017-06-30/7wnvbz) **的白皮书。**

### 更多相关内容

+   [停止学习数据科学以寻找目的，并找到目的…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一桩90亿美元的AI失败，详细审查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
