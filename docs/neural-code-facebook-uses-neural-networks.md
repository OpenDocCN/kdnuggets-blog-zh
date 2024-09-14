# 神经代码搜索：Facebook 如何利用神经网络帮助开发者搜索代码片段

> 原文：[https://www.kdnuggets.com/2019/07/neural-code-facebook-uses-neural-networks.html](https://www.kdnuggets.com/2019/07/neural-code-facebook-uses-neural-networks.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [comments](#comments)

![](../Images/080a800f2e408eaeed765a38fd92c8bb.png)

现如今，Google 和 StackOverflow 是每位开发者的好帮手。在进行特定项目时，开发者经常依赖外部信息来源来寻找解决方案或代码片段。然而，搜索结果通常基于元数据，而不是代码本身提供的信息。例如，如果开发者在 StackOverflow 上发布一个代码片段，它通常会包括描述和标签，以帮助将其索引以便未来搜索。该方法依赖于元数据的准确性来提供准确的搜索结果，但也忽略了可以直接从代码中推断出的信息。为了应对这一限制，Facebook AI Research（FAIR）团队一直在研究一种方法，利用自然语言处理（NLP）和信息检索（IR）直接从源代码文本中推断搜索相关信息。

FAIR 团队努力的首次成果是一个名为 Neural Code Search (NCS) 的工具，它接受自然语言查询，并从代码语料库中直接推断结果。NCS 背后的技术总结在最近发布的两篇研究论文中：

+   [**NCS**](https://dl.acm.org/citation.cfm?id=3211353&fbclid=IwAR3qdp2vDXwBuZb7uw2vR1op7SPngnRr0R6l3pxHrvqWH_0Unj0UXtTYpKA)**:** 这篇论文描述了一种利用无监督模型结合 NLP 和 IR 技术的技术。

+   [**UNIF**](https://arxiv.org/pdf/1905.03813.pdf?fbclid=IwAR3_9ejBpho2yNLfMKVCp5rZ58gAD63T_peTg3RQh8tIRLZkYwH9SquLQI8)**:** 这篇论文提出了一种 NCS 的扩展，使用监督神经网络模型来提高在有良好监督数据可用于训练时的性能。

NCS 和 UNIF 两种技术背后的原理相对相似。这两种方法都依赖于代码片段的向量表示，这些向量可以用于训练模型，使语义相似的代码片段和查询在向量空间中彼此接近。这种技术能够直接使用代码片段语料库回答自然语言查询，而不依赖外部元数据。尽管 NCS 和 UNIF 两种技术相对相似，但 UNIF 通过监督模型扩展了 NCS，从而能为 NLP 查询提供更准确的答案。

### NCS

其核心原理是使用嵌入生成来自代码语料库的向量表示，使得相似的代码片段在向量空间中彼此靠近。以下示例中，有两个不同的方法体，它们都涉及关闭或隐藏 Android 软件键盘（上面的第一个问题）。由于它们具有相似的语义含义，即使它们的代码行不完全相同，它们在向量空间中的表示点也彼此接近。NCS 使用方法级别的粒度在向量空间中嵌入每个代码片段。

![](../Images/697238c45896d8a3545d08028ae0dbf2.png)

创建向量表示的过程包括三个主要步骤：

1) 提取词汇

2) 构建词嵌入

3) 构建文档嵌入

4) 自然语言搜索检索

![](../Images/89c413a0e256dc571ed6f13139d6fe9b.png)

给定一个特定的代码片段，NCS 提取不同的语法部分，如方法名、方法调用、枚举、字符串文字和注释。这些工件随后使用标准的英语约定进行分词。对于输入语料库中的每个文档，NCS 以一种能够学习每个词的嵌入的方式对源代码进行分词。在这个步骤之后，我们为每个方法体提取的词列表类似于自然语言文档。

在词汇提取之后，NCS 继续使用 [FastText 框架](https://fasttext.cc/) 构建词嵌入。在此过程中，FastText 使用一个可以在大语料库上无监督训练的两层密集神经网络计算向量表示。更具体地说，NCS 使用跳字模型，其中目标词的嵌入用于预测固定窗口大小内上下文词的嵌入。

这一阶段的最终步骤是通过提取的词嵌入表示方法体的一般意图。NCS 通过计算方法体中一组词的词嵌入向量的加权平均值来实现这一点。这个公式的结果被称为文档嵌入，用于创建每个方法体的索引，以便进行有效的搜索检索。

使用向量表示作为起点，NCS 现在可以尝试回答与代码片段相关的自然语言查询。给定自然语言查询，NCS 使用 FastText 框架遵循类似的分词过程。利用相同的 FastText 嵌入矩阵，NCS 平均词的向量表示以创建查询句子的文档嵌入；不在词汇表中的词会被丢弃。NCS 然后使用标准相似度搜索算法，[FAISS](https://github.com/facebookresearch/faiss) 来找到与查询最接近的文档向量。

![](../Images/4a1ffb905908fccc1f398da3fc34dc74.png)

### UNIF

作为一种无监督技术，NCS 的优势在于可以快速简便地直接从代码语料中构建知识。然而，NCS 的主要限制之一是模型隐含地假设查询中的词汇来自与源代码中提取的词汇相同的领域，因为查询和代码片段都映射到同一向量空间。然而，许多代码语料并不传达任何相关的语义信息。例如，下面的代码片段获取内存空间中的可用块。仅通过无监督方式生成词嵌入，NCS 将无法匹配诸如“获取内部存储器上的空闲空间”这样的相关查询。

```py
File path = Environment.getDataDirectory();
StatFs stat = new StatFs(path.getPath());
long blockSize = stat.getBlockSize();
long availableBlocks = stat.getAvailableBlocks();
return Formatter.formatFileSize(this, availableBlocks * blockSize);

```

UNIF 是对 NCS 的一种监督扩展，旨在弥合自然语言词汇与源代码词汇之间的差距。UNIF 实质上使用监督学习来修改初始的词嵌入矩阵 T，并分别为代码和查询词生成两个嵌入矩阵 Tc 和 Tq。我们还用基于学习的注意力加权方案替代了代码词嵌入的 TF-IDF 加权。这种变化实际上考虑了一些代码语料中的语义不匹配。

![](../Images/700f1ec76d6696dfe81212b8b3b9ece1.png)

### NCS 和 UNIF 实践

FAIR 团队将 NCS 和 UNIF 与最先进的信息检索模型进行了比较。使用 StackOverflow 问题的起始数据集，NCS 在各种任务中超越了流行的 BM25 模型。

![](../Images/a8d58432722c936eff7184903b511cae.png)

同样，添加 UNIF 扩展显示了 NCS 性能的逐步提升。

![](../Images/5189a0560330e460c9f8d40e98251857.png)

NCS 和 UNIF 都提供了一种巧妙而简单的实现方法用于信息检索。以代码搜索开始是提供即时价值的一种简单方式，但 NCS 和 UNIF 背后的理念适用于许多神经搜索和信息检索场景。

[原文](https://towardsdatascience.com/neural-code-search-how-facebook-uses-neural-networks-to-help-developers-search-for-code-snippets-9db9e0090780)。经许可转载。

**相关：**

+   [自然语言处理 (NLP) 指南](https://www.kdnuggets.com/2019/05/guide-natural-language-processing-nlp.html)

+   [从知识图谱中提取知识，使用 Facebook 的 Pytorch-BigGraph](https://www.kdnuggets.com/2019/05/extracting-knowledge-graphs-facebook-pytorch-biggraph.html)

+   [自然语言处理关键术语解析](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关话题

+   [低代码：开发者还需要吗？](https://www.kdnuggets.com/2022/04/low-code-developers-still-needed.html)

+   [LinkedIn 如何利用机器学习来排名你的信息流](https://www.kdnuggets.com/2022/11/linkedin-uses-machine-learning-rank-feed.html)

+   [热门编程语言及其用途](https://www.kdnuggets.com/2021/05/top-programming-languages.html)

+   [KDnuggets™ 新闻 22:n04，1月26日：高薪副业…](https://www.kdnuggets.com/2022/n04.html)

+   [KDnuggets 新闻，11月16日：LinkedIn 如何利用机器学习 •…](https://www.kdnuggets.com/2022/n45.html)

+   [Python 上下文管理器的 3 个有趣用途](https://www.kdnuggets.com/3-interesting-uses-of-python-context-managers)
