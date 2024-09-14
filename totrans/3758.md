# 文本分析：入门指南

> 原文：[https://www.kdnuggets.com/2017/03/text-analytics-primer.html](https://www.kdnuggets.com/2017/03/text-analytics-primer.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

> **编辑注：** 以下是对伊利诺伊大学教授及文本分析专家彭刘的采访，由市场科学家凯文·格雷进行，彭刘简明扼要地概述了该领域的现状。

**凯文·格雷：** 我看到“文本分析”和“文本挖掘”在市场研究人员中被多种方式使用，并且通常可以互换使用。这些术语对你来说是什么意思？

**彭刘：** 我的理解是这两个术语意思相同。学术界的人更倾向于使用文本挖掘这个术语，尤其是数据挖掘研究者，而文本分析则主要在行业中使用。我很少看到学者使用文本分析这个术语。还有一个相关的术语，叫做自然语言处理（NLP）。文本挖掘和文本分析通常指将数据挖掘和机器学习算法应用于文本数据。NLP 涵盖了这些内容，并且还有其他更传统的自然语言任务，如机器翻译、句法分析、语义分析等。但这些术语之间并没有明确的划分。

![](../Images/bbc1856f69a15cd3b8faf34219d778c5.png)

彭刘 ([来源](https://uicscience.tumblr.com/post/125758780768/bing-liu-professor-of-computer-science-uic))。

**KG：** 能简要介绍一下文本分析/挖掘的历史以及它是如何随着时间发展的吗？

**BL：** 它源于三个研究领域：信息检索、数据挖掘和自然语言处理（NLP）。信息检索始于1970年代，主要处理文本检索。也就是说，给定一个查询，可以是几个关键词或一个完整的文档，我们希望从文本集合或语料库中找到相关的文档。网络搜索引擎是巨大的信息检索系统。

传统的数据挖掘使用结构化数据，如数据库表格。在1990年代末，研究人员开始将文本作为数据，这催生了文本挖掘。早期的文本挖掘基本上是在文本数据上应用数据挖掘和机器学习算法，而没有使用诸如解析、词性标注、摘要等自然语言处理技术。

NLP 有更长的历史。它始于1950年代，目标是让计算机理解人类语言。随着文本挖掘研究在过去10年左右扩展其范围，它开始使用自然语言处理技术，如解析、词性标注、指代消解等。从自然语言处理会议涵盖的主题来看，文本挖掘现在已成为自然语言处理的一部分。我的研究最初是从传统的数据挖掘开始的。随后我研究了情感分析或意见挖掘，这使我转向了自然语言处理。

**KG：** 它在市场研究和其他领域是如何使用的？

**BL:** 文本分析在营销和许多其他领域得到了广泛应用。我最熟悉的是情感分析的应用。在营销中，营销人员通常希望了解消费者对公司产品以及竞争对手产品的看法。这些意见可以通过分析在线评论或其他形式的社交媒体帖子来获得。基于这些意见，营销人员可以制定适合不同市场细分的营销信息。公众意见在许多其他应用领域也非常有用，例如股市预测、消费者情感预测、政治选举预测等。

**KG:** 文本分析面临的主要技术挑战是什么？

**BL:** 这完全取决于你感兴趣的任务是什么。一些任务完成得相当好，例如命名实体识别。但许多其他任务在准确性上仍需大量改进。最终的挑战是自然语言理解。尽管研究人员对此进行了长期研究，但进展并不大。目前的文本分析技术仍主要基于传统的语言学规则和统计机器学习及数据挖掘算法。这些方法仍无法实现真正的理解。由于这个问题，大多数文本分析任务的准确性仍然相对较低。

**KG:** 人工智能在文本分析中扮演什么角色？

**BL:** 高级文本分析是人工智能（AI）的一部分。机器学习和数据挖掘等其他AI领域的进展对文本分析产生了重大影响。我会说，过去二十年文本分析的主要进展来自于更好的机器学习技术。

**KG:** 是否有许多人对文本分析存在误解或错误认识？

**BL:** 在学术界，我没有发现关于文本分析的重大误解或误解。我对行业情况不太确定。我所知道的唯一一点是，人们对文本分析可能有非常高的期望，但如果你想做得好且准确，这是一个非常具有挑战性的问题。

**KG:** 最后，展望十年后，你认为文本分析将能做到现在无法做到的哪些事情？是否有一些事情在可预见的未来对于文本分析来说是不可能实现的？

**BL:** 让我们讨论自然语言处理，而不是文本分析，因为高级文本分析需要自然语言处理。随着深度学习等机器学习技术的发展，我们肯定会看到更好的文本分析算法，准确性远超我们今天能实现的水平。但在可预见的未来，像我们人类那样理解自然语言是非常不可能的，因为自然语言具有高度的抽象性。我们写的每一句话背后都有大量的常识知识，我们假设读者是知道这些知识的。显然，计算机程序并不知道这些知识。学习、表示和推理常识知识是一个主要的挑战。

**KG:** 谢谢你，Bing！

这篇文章最初发表于 [**GreenBook**](http://www.greenbookblog.org/2017/01/24/text-analytics-a-primer/) 2017年1月24日。

**[Kevin Gray](https://www.linkedin.com/in/cannongray/)** 是 [**Cannon Gray**](http://cannongray.com/home) 的总裁，Cannon Gray 是一家市场科学与分析咨询公司。

[**Bing Liu**](https://www.cs.uic.edu/~liub/) 是伊利诺伊大学芝加哥分校（UIC）的计算机科学终身教授。他在爱丁堡大学获得人工智能博士学位，并且是情感分析和意见挖掘、机器学习及相关主题的多本书籍和文章的作者。

**相关内容：**

+   [自然语言处理关键术语解释](/2017/02/natural-language-processing-key-terms-explained.html)

+   [文本挖掘亚马逊手机评论：有趣的见解](/2017/01/data-mining-amazon-mobile-phone-reviews-interesting-insights.html)

+   [贝叶斯基础解释](/2016/12/bayesian-basics-explained.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 该主题的更多信息

+   [统计学导论：Statology 入门](https://www.kdnuggets.com/introduction-to-statistics-statology-primer)

+   [概率：Statology 入门](https://www.kdnuggets.com/probability-statology-primer)

+   [数据描述：Statology 入门](https://www.kdnuggets.com/describing-data-statology-primer)

+   [数据可视化：Statology 入门](https://www.kdnuggets.com/visualizing-data-statology-primer)

+   [使用 BERT 对长文本进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [如何使用 ChatGPT 将文本转换为 PowerPoint 演示文稿](https://www.kdnuggets.com/2023/08/chatgpt-convert-text-powerpoint-presentation.html)
