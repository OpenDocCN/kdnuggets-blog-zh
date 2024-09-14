# 使用 Gensim 的稳健 Word2Vec 模型与应用 Word2Vec 特征于机器学习任务

> 原文：[https://www.kdnuggets.com/2018/04/robust-word2vec-models-gensim.html](https://www.kdnuggets.com/2018/04/robust-word2vec-models-gensim.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

> **编辑注：** 本文只是一个更全面、更深入的原文的一部分，[原文链接](https://towardsdatascience.com/understanding-feature-engineering-part-4-deep-learning-methods-for-text-data-96c44370bbfa)包含了更多内容。

### 使用 Gensim 构建稳健的 Word2Vec 模型

尽管我们的实现已经相当不错，但在大语料库上并没有得到优化。`[**gensim**](https://radimrehurek.com/gensim/)` 框架由 Radim Řehůřek 创建，提供了一个健壮、高效且可扩展的 Word2Vec 模型实现。我们将在我们的圣经语料库上利用它。在我们的工作流程中，我们将对规范化的语料库进行分词，然后关注 Word2Vec 模型中的以下四个参数来构建它。

+   `**size**`**:** 词嵌入的维度

+   `**window**`**:** 上下文窗口大小

+   `**min_count**`**:** 最小词频

+   `**sample**`**:** 高频词的下采样设置

构建模型后，我们将使用感兴趣的词查看每个词的最相似词。

![](../Images/ca2b2f3fb686c91a092f9e806ac00ddb.png)

这里的相似词确实与我们感兴趣的词更相关，这符合预期，因为我们运行了更多次数的模型，这应该带来了更好、更具上下文的嵌入。你注意到任何有趣的关联吗？

![](../Images/775e2a392863dacc34e2c4181fd5dee0.png)

诺亚的儿子被我们的模型识别为最具上下文相似性的实体！

让我们在使用 t-SNE 将维度减少到二维空间后，使用词嵌入向量可视化感兴趣的词及其相似词。

![](../Images/72f48cdd6154c442a2a26cae94e64f8b.png)

使用 t-SNE 可视化我们的 word2vec 词嵌入

红色圆圈是我画的，用于指出我发现的一些有趣的关联。根据我之前描述的内容，我们可以清楚地看到，基于我们模型的词嵌入，***noah*** 和他的儿子们彼此之间非常接近！

### 应用 Word2Vec 特征于机器学习任务

如果你记得阅读过之前的文章 [***Part-3: 传统文本数据方法***](https://towardsdatascience.com/understanding-feature-engineering-part-3-traditional-methods-for-text-data-f6f7d70acd41)，你可能看到我使用了一些实际的机器学习任务特征，如聚类。我们将利用其他顶级语料库，尝试实现相同的目标。首先，我们将基于语料库构建一个简单的 Word2Vec 模型，并可视化嵌入。

![](../Images/8b3bc365fee36478c6c1a7c8621bf9c2.png)

在我们的玩具语料库上可视化 word2vec 词嵌入

记住，我们的语料库非常小，因此为了获得有意义的词嵌入并让模型获得更多的上下文和语义，更多的数据是有帮助的。那么在这种情况下，什么是词嵌入？它通常是每个词的密集向量，如下例所示，表示词***sky***。

```py
w2v_model.wv['sky']

Output
------

array([ 0.04576328,  0.02328374, -0.04483001,  0.0086611 ,  
0.05173225, 0.00953358, -0.04087641, -0.00427487, -0.0456274 ,  
0.02155695], dtype=float32)
```

现在假设我们想要对来自玩具语料库的八个文档进行聚类，我们需要从每个文档中每个词的文档级嵌入中获取信息。一种策略是计算每个文档中每个词的词嵌入的平均值。这是一种非常有用的策略，你可以将其应用到你自己的问题中。现在让我们在我们的语料库上应用这个方法，获取每个文档的特征。

![](../Images/29c8deae1fd070bcbcf525afbcec1bae.png)

文档级别的嵌入

现在我们已经获得了每个文档的特征，让我们使用[***亲和传播***](https://en.wikipedia.org/wiki/Affinity_propagation)算法对这些文档进行聚类。该算法基于数据点之间的*“消息传递”*概念，不需要显式输入簇的数量，而这是基于分区的聚类算法通常需要的。

![](../Images/b39b4c68ab91b60f63ed7e20aa842781.png)

根据我们从 word2vec 获得的文档特征进行簇的分配

我们可以看到，我们的算法已经根据 Word2Vec 特征将每个文档归入了正确的组，非常棒！我们还可以通过使用[***主成分分析 (PCA)***](https://en.wikipedia.org/wiki/Principal_component_analysis)将特征维度降到 2-D 并可视化（通过为每个簇进行颜色编码）来可视化每个文档在每个簇中的位置。

![](../Images/640851dacfaad50813cd37baaf08bd8f.png)

可视化我们的文档簇

一切看起来都很有序，因为每个簇中的文档彼此更接近，而与其他簇距离较远。

**简介： [Dipanjan Sarkar](https://www.linkedin.com/in/dipanzan)** 是 Intel 的数据科学家，作者，Springboard 的导师，作家，体育和情景喜剧迷。

[原文](https://towardsdatascience.com/understanding-feature-engineering-part-4-deep-learning-methods-for-text-data-96c44370bbfa)。转载经许可。

**相关：**

+   [文本数据预处理：Python中的操作指南](/2018/03/text-data-preprocessing-walkthrough-python.html)

+   [处理文本数据的一般方法](/2017/12/general-approach-preprocessing-text-data.html)

+   [处理文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

### 相关主题

+   [在 Python 中应用描述性和推断性统计](https://www.kdnuggets.com/applying-descriptive-and-inferential-statistics-in-python)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [用 Python 自动化的 5 个任务](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

+   [自然语言处理任务中的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [HuggingGPT：解决复杂 AI 任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)

+   [5 个 ChatGPT 无法完成的编码任务](https://www.kdnuggets.com/5-coding-tasks-chatgpt-cant-do)
