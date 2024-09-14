# 实现深度学习方法和文本数据的特征工程：FastText

> 原文：[https://www.kdnuggets.com/2018/05/implementing-deep-learning-methods-feature-engineering-text-data-fasttext.html](https://www.kdnuggets.com/2018/05/implementing-deep-learning-methods-feature-engineering-text-data-fasttext.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

> **编辑注释：** 这篇文章只是一个更全面、深入的原文的一部分，[原文链接](https://towardsdatascience.com/understanding-feature-engineering-part-4-deep-learning-methods-for-text-data-96c44370bbfa)，涵盖了比这里更多的内容。

### FastText模型

[**FastText**](https://fasttext.cc/)模型首次由Facebook在2016年介绍，作为对普通Word2Vec模型的扩展和改进。基于原始论文[*‘Enriching Word Vectors with Subword Information’* by Mikolov et al.](https://arxiv.org/pdf/1607.04606.pdf)，这是深入了解该模型如何工作的绝佳读物。总体来说，FastText是一个学习词表示以及进行稳健、快速、准确的文本分类的框架。该框架由[Facebook](https://www.facebook.com/)在[**GitHub**](https://github.com/facebookresearch/fastText)上开源，并声称具备以下功能。

+   最新的[英语词向量](https://fasttext.cc/docs/en/english-vectors.html)。

+   157种语言的词向量训练于维基百科和Crawl，[链接](https://github.com/facebookresearch/fastText/blob/master/docs/crawl-vectors.md)。

+   用于[语言识别](https://fasttext.cc/docs/en/language-identification.html#content)和[各种监督任务](https://fasttext.cc/docs/en/supervised-models.html#content)的模型。

尽管我还未从头实现该模型，但根据研究论文，以下是我对模型工作的理解。一般来说，像Word2Vec这样的预测模型通常将每个词视为一个独立的实体（例如，***where***）并为该词生成一个密集的嵌入。然而，这对于具有大量词汇和许多稀有词的语言来说是一个严重的限制，这些稀有词在不同语料中可能不会出现很多。Word2Vec模型通常忽略每个词的形态结构，将词视为一个单一的实体。FastText模型*将每个词视为一个字符n-gram的袋子。* 这在论文中也称为子词模型。

我们在单词的开头和结尾添加特殊的边界符号 **<** 和 **>**。这使我们能够区分前缀和后缀与其他字符序列。我们还将单词 ***w*** 本身包含在其 n-gram 集中，以学习每个单词的表示（除了它的字符 n-gram）。以单词 ***where*** 和 ***n=3***（三元组）为例，它将由字符 n-gram 表示：***<wh, whe, her, ere, re>*** 以及表示整个单词的特殊序列 ***<where>***。请注意，序列 ***<her>*** 对应于单词 ***her*** 与单词 ***where*** 的三元组 ***her*** 是不同的。

实际上，论文建议提取所有的 n-gram，***n ≥*** ***3*** 和 ***n ≤*** ***6***。这是一种非常简单的方法，可以考虑不同的 n-gram 集，例如提取所有前缀和后缀。我们通常将每个 n-gram 关联一个向量表示（嵌入）。因此，我们可以通过 n-gram 向量表示的总和或这些 n-gram 嵌入的平均值来表示一个单词。因此，由于这种基于字符的 n-gram 的作用，稀有单词获得良好表示的机会更高，因为它们的基于字符的 n-gram 应该会出现在语料库中的其他单词中。

### 应用 FastText 特性于机器学习任务

`gensim` 包提供了很好的封装，给我们提供了接口来利用在 `gensim.models.fasttext` 模块下可用的 FastText 模型。让我们再次在我们的***圣经语料库***上应用这个，并查看我们感兴趣的单词及其最相似的单词。

![](../Images/999728a20ab50605ab08b779dee4097f.png)

你可以看到，与我们的 Word2Vec 模型相比，结果中存在许多相似之处，每个感兴趣的单词都有相关的相似单词。你注意到有什么有趣的关联和相似之处吗？

![](../Images/b3b49b9c52202eed979cbd9f1a64f398.png)

摩西、他的兄弟亚伦和摩西的帐幕

> **注意：** 运行这个模型计算开销大，通常比 skip-gram 模型花费更多时间，因为它考虑了每个单词的 n-gram。如果使用 GPU 或较好的 CPU 进行训练效果更好。我在 AWS `***p2.x***` 实例上进行了训练，花了大约 10 分钟，而在普通系统上则需要超过 2 到 3 小时。

现在让我们使用[***主成分分析 (PCA)***](https://en.wikipedia.org/wiki/Principal_component_analysis)来将单词嵌入维度降至 2D，然后可视化。

![](../Images/cd3a3180e4b5a37fc702bbb58d372a6b.png)

在我们的圣经语料库中可视化 FastText 单词嵌入

我们可以看到很多有趣的模式！*诺亚*、他的儿子*Shem*和祖父*Methuselah*彼此接近。我们还看到*上帝*与*Moses*和*埃及*相关，那里经历了包括*饥荒*和*瘟疫*在内的圣经灾难。此外，*耶稣*和一些他的*门徒*也彼此接近。

要访问任何词嵌入，你可以按如下方式用词索引模型。

```py
ft_model.wv['jesus']

array([-0.23493268,  0.14237943,  0.35635167,  0.34680951,    
        0.09342121,..., -0.15021783, -0.08518736, -0.28278247,   
       -0.19060139], dtype=float32)
```

拥有这些嵌入，我们可以执行一些有趣的自然语言任务。其中之一是找出不同词（实体）之间的相似性。

```py
print(ft_model.wv.similarity(w1='god', w2='satan'))
print(ft_model.wv.similarity(w1='god', w2='jesus'))

Output
------
0.333260876685
0.698824900473
```

我们可以看到***‘god’***与***‘jesus’***的关联比与***‘satan’***的关联更为紧密，基于我们《圣经》语料库中的文本。相当相关！

考虑到词嵌入的存在，我们甚至可以从一堆词中找出奇异词。

```py
st1 = "god jesus satan john"
print('Odd one out for [',st1, ']:',  
      ft_model.wv.doesnt_match(st1.split()))

st2 = "john peter james judas"
print('Odd one out for [',st2, ']:', 
      ft_model.wv.doesnt_match(st2.split()))

Output
------
Odd one out for [ god jesus satan john ]: satan
Odd one out for [ john peter james judas ]: judas
```

在这两种情况下，奇异实体与其他词汇之间都有有趣且相关的结果！

### 结论

这些示例应该能让你对利用深度学习语言模型提取文本数据特征以及解决诸如词汇语义、上下文和数据稀疏性等问题的新颖高效策略有一个很好的了解。接下来将详细介绍利用深度学习模型进行图像数据特征工程的策略，敬请期待！

要了解连续数值数据的特征工程策略，请查看本系列的 [**第1部分**](https://towardsdatascience.com/understanding-feature-engineering-part-1-continuous-numeric-data-da4e47099a7b)！

要了解离散分类数据的特征工程策略，请查看本系列的 [**第2部分**](https://towardsdatascience.com/understanding-feature-engineering-part-2-categorical-data-f54324193e63)！

要了解非结构化文本数据的传统特征工程策略，请查看本系列的 [**第3部分**](https://towardsdatascience.com/understanding-feature-engineering-part-3-traditional-methods-for-text-data-f6f7d70acd41)！

本文使用的所有代码和数据集可以从我的 [**GitHub**](https://github.com/dipanjanS/practical-machine-learning-with-python/tree/master/bonus%20content/feature%20engineering%20text%20data) 访问。

代码也可以作为 [**Jupyter notebook**](https://github.com/dipanjanS/practical-machine-learning-with-python/blob/master/bonus%20content/feature%20engineering%20text%20data/Feature%20Engineering%20Text%20Data%20-%20Advanced%20Deep%20Learning%20Strategies.ipynb) 获取。

除非明确注明，否则架构图属于我的版权。请随意使用，但如果你想在自己的工作中使用它们，请记得注明来源。

如果你对我的文章或数据科学有任何反馈、评论或有趣的见解，欢迎通过我的LinkedIn社交媒体渠道联系我。

**个人简介： [Dipanjan Sarkar](https://www.linkedin.com/in/dipanzan)** 是英特尔的数据科学家、作者、Springboard的导师、作家以及体育和情景喜剧迷。

[原文](https://towardsdatascience.com/understanding-feature-engineering-part-4-deep-learning-methods-for-text-data-96c44370bbfa)。经授权转载。

**相关内容：**

+   [文本数据预处理：Python 实践指南](/2018/03/text-data-preprocessing-walkthrough-python.html)

+   [预处理文本数据的一般方法](/2017/12/general-approach-preprocessing-text-data.html)

+   [处理文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

### 更多相关话题

+   [2022 特征工程峰会：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [机器学习中的特征工程实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [为多变量时间序列建立可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [使用 RAPIDS cuDF 在特征工程中利用 GPU](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [初学者的特征工程](https://www.kdnuggets.com/feature-engineering-for-beginners)
