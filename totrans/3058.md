# 从头构建一个问答系统

> 原文：[https://www.kdnuggets.com/2018/10/building-question-answering-system-from-scratch.html](https://www.kdnuggets.com/2018/10/building-question-answering-system-from-scratch.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Alvira Swalin](https://www.linkedin.com/in/alvira-swalin)，旧金山大学**

![图片](../Images/ffaad18510a91a7f920a2e0b2e449237.png)

随着我的硕士学业即将结束，我希望从事一个有趣的 NLP 项目，运用我在 [USF](https://www.usfca.edu/arts-sciences/graduate-programs/data-science) 学到的所有技术（不完全准确）。在教授的帮助和与同学们的讨论之后，我决定从零开始构建一个问答模型。我使用了 [斯坦福问答数据集 (SQuAD)](https://rajpurkar.github.io/SQuAD-explorer/)。这个问题非常有名，各大公司都在争先恐后地进入排行榜，并使用像基于注意力的 RNN 模型这样的高级技术来获得最佳准确度。我找到的与 SQuAD 相关的 GitHub 仓库也都使用了 RNN。

然而，我的目标不是达到最先进的准确度，而是学习不同的 NLP 概念，实施它们并探索更多解决方案。我一直相信从基本模型开始以了解基线，这也是我在这里的做法。这一部分将重点介绍 **Facebook 句子嵌入** 及其在构建 QA 系统中的应用。在未来的部分中，我们将尝试实现深度学习技术，特别是针对该问题的序列建模。所有代码可以在这个 [Github 仓库](https://github.com/aswalin/SQuAD) 中找到。

但让我们首先理解问题。我将给出一个简要概述，然而对问题的详细理解可以在 [这里](https://rajpurkar.github.io/mlx/qa-and-squad/) 找到。

### ****SQuAD 数据集****

**S**坦福 **问**题 **答**案 **数**据集（SQuAD）是一个新的阅读理解数据集，包含由众包工作者在一组维基百科文章上提出的问题，每个问题的答案都是对应阅读段落中的一段文本或 *跨度*。SQuAD 拥有超过 100,000 个问答对和 500 多篇文章，规模显著大于以往的阅读理解数据集。

### **问题**

对于训练集中的每个观察，我们有一个 **上下文、问题和文本**。一个这样的观察示例 -

![](../Images/d2e67ce3ed9c8e12b607144dede178a9.png)

目标是找到针对任何新问题和提供的上下文的文本。这是一个封闭的数据集，意味着问题的答案始终是上下文的一部分，并且是连续的上下文跨度。我目前将这个问题分为两个部分 -

+   获取包含正确答案的句子（高亮为黄色）

+   一旦句子确定，从句子中获取正确的答案（高亮绿色）

### 介绍 Infersent，Facebook 句子嵌入

现在我们有各种类型的嵌入 [word2vec](https://deeplearning4j.org/word2vec.html)、[doc2vec](https://medium.com/scaleabout/a-gentle-introduction-to-doc2vec-db3e8c0cce5e)、[food2vec](https://jaan.io/food2vec-augmented-cooking-machine-intelligence/)、[node2vec](https://docs.google.com/presentation/d/1L-633fYUokbYqTC8R5FUfhlmWnpCO5ey6JMZ_kAnJlM/edit?usp=sharing)，那么为什么不试试 sentence2vec 呢。所有这些嵌入的基本思想是使用不同维度的向量来表示实体，从而使计算机更容易理解它们用于各种下游任务。解释这些概念的文章链接在此，以供参考。

传统上，我们会对句子中所有单词的向量取平均，这被称为词袋模型。每个句子被标记为单词，这些单词的向量可以通过 glove 嵌入找到，然后对所有这些向量取平均。这种技术表现尚可，但这并不是一种非常准确的方法，因为它没有考虑单词的顺序。

这里是 [**Infersent**](https://github.com/facebookresearch/InferSent)，它是一种 *句子嵌入* 方法，提供语义句子表示。它在自然语言推理数据上进行了训练，并且在许多不同任务上泛化良好。

过程如下-

> 从训练数据中创建词汇表，并使用该词汇表训练 infersent 模型。一旦模型训练完成，提供句子作为输入到编码器函数，它将返回一个 4096 维的向量，而不考虑句子中的单词数量。 [[demo](https://github.com/facebookresearch/InferSent/blob/master/encoder/demo.ipynb)]

这些嵌入可以用于各种下游任务，例如查找两个句子之间的相似性。我在 Quora-Question Pair kaggle 比赛中实现了相同的功能。你可以在 [这里](https://github.com/aswalin/Kaggle/blob/master/Quora.ipynb) 查看。

关于 SQuAD 问题，下面是我尝试使用句子嵌入解决上一节描述的第一个问题的方式-

+   将段落/上下文拆分成多个句子。我知道的用于处理文本数据的两个包是 - [Spacy](https://spacy.io/usage/spacy-101) 和 [Textblob](http://textblob.readthedocs.io/en/dev/)。我已经使用了 TextBlob 包。它进行智能拆分，不像 spacy 的句子检测，它会根据句号给出随机的句子。下面是一个示例：

![](../Images/ca4451fbbd3a0c447b6c7e972aebb1cb.png)

**示例：段落**![](../Images/18cb89bff424258612e9078996a36da2.png)

**TextBlob 将其拆分成 7 个句子，这很合理**![](../Images/91c395758fe4a535ffc4a4695a281ba3.png)

**Spacy 将其拆分成 12 个句子**

+   使用Infersent模型获取每个句子和问题的向量表示。

+   为每对句子-问题创建基于余弦相似度和欧几里得距离的特征。

### 模型

我进一步使用了两种主要方法来解决这个问题——

+   无监督学习中，我没有使用目标变量。在这里，我返回段落中与给定问题距离最小的句子。

+   监督学习 - 训练集的创建在这一部分非常棘手，原因在于每个部分的句子数量没有固定，并且答案可以从一个词到多个词不等。我找到的唯一实现了逻辑回归的论文是斯坦福团队发布的这次比赛和数据集。他们使用了在这篇[论文](https://arxiv.org/pdf/1606.05250.pdf)中解释的多项式逻辑回归，并创建了**1.8亿个特征（该模型的句子检测准确率为79%）**，但不清楚他们是如何定义目标变量的。如果有人知道，请在评论中澄清一下。我稍后会解释我的解决方案。

**无监督学习模型**

我首先尝试使用欧几里得距离来检测与问题距离最小的句子。该模型的准确率约为45%。然后，我转而使用余弦相似度，准确率从**45%提升到63%**。这很有道理，因为欧几里得距离不考虑向量之间的对齐或角度，而余弦相似度考虑了这些因素。在向量表示的情况下，方向是重要的。

但这种方法没有利用我们所提供的带有目标标签的丰富数据。然而，考虑到解决方案的简单性质，这仍然能在没有任何训练的情况下给出良好的结果。我认为这份不错的表现归功于Facebook的句子嵌入。

**监督学习模型**

在这里，我将目标变量从文本形式转换为包含该文本的句子索引。为了简化，我将段落长度限制为10个句子（大约98%的段落包含10个或更少的句子）。因此，在这个问题中我需要预测10个标签。

对于每个句子，我根据余弦距离构建了一个特征。如果一个段落的句子数量较少，那么我将其特征值替换为1（最大可能的余弦距离），以使总句子数达到10个，以保持一致性。通过示例解释这一过程会更容易。

让我们以训练集的第一条观察/行作为例子。回答所在的句子在上下文中加粗：

**示例**

问题——“1858年，圣母玛利亚在法国鲁尔德显现给了谁？”

**背景**—“从建筑学上讲，这所学校具有天主教特色。在主楼的金色圆顶上是一尊金色的圣母玛利亚雕像。主楼前面立即是基督铜像，双臂高举，上面刻有“Venite Ad Me Omnes”的字样。主楼旁边是圣心大教堂。大教堂的后面是洞窟，这是一个玛利亚祈祷和沉思的地方。**它是法国卢尔德的一个洞窟的复制品，圣母玛利亚据说在1858年显现给圣贝尔纳黛特·苏比鲁斯。** 在主车道的尽头（通过3尊雕像和金色圆顶的直线连接），是一尊简单的现代石雕圣母玛利亚像。”

**文本**—“圣贝尔纳黛特·苏比鲁斯”

在这种情况下，目标变量将变为5，因为这是加粗句子的索引。我们将有10个特征，每个特征对应段落中的一句话。由于这些句子在段落中不存在，column_cos_7、column_cos_8和column_cos_9的缺失值填充为1。

![](../Images/7ec4f696e582859ebe279d905caab25d.png)

**依赖解析**

我为这个问题使用的另一个特征是**“依赖解析树”**。这略微提高了模型的准确性，提升了5%。在这里，我使用了Spacy树解析，因为它有丰富的API用于导航树结构。

![](../Images/4f431c6bf4bc8837ebb7120539b02d2a.png)

![](../Images/dd61f1c282f3cb4d254fdd035a864dab.png)

详情请查阅 [斯坦福讲座](https://web.stanford.edu/~jurafsky/slp3/14.pdf)

> 单词之间的关系通过从头到依赖词的有向标记弧图示在句子上方。我们称之为类型化依赖结构，因为标签来自一个固定的语法关系库存。它还包括一个根节点，明确标记了树的根，即整个结构的头部。

让我们使用Spacy树解析来可视化我们的数据。我使用的是上一节提供的相同示例。

**问题—**“圣母玛利亚在1858年在法国卢尔德显现给了谁？”

![](../Images/4ee78a27dfb5465d1b781369fce13092.png)

**包含答案的句子—** “它是法国卢尔德的一个洞窟的复制品，圣母玛利亚据说在1858年显现给圣贝尔纳黛特·苏比鲁斯。”

![](../Images/2a333ad526351d87dcad356a1ffa0675.png)

**段落中所有句子的根**

![](../Images/212b7aca80799750cf331fae8e684db8.png)

这个想法是将问题的词根（在这种情况下是“**appear**”）与句子的所有词根/子词根进行匹配。由于一个句子中可能有多个动词，我们可以得到多个词根。如果问题的词根包含在句子的词根中，则该句子更有可能回答该问题。考虑到这一点，我为每个句子创建了一个特征，其值为1或0。这里，1表示问题的词根包含在句子的词根中，0则相反。

> 注意: 在比较句子的词根与问题词根之前进行词干提取很重要。在之前的例子中，问题的词根是**appear**，而句中的词根是**appeared**。如果不将**appear**和**appeared**归并为一个共同的术语，它们将无法匹配。

以下示例是经过处理的训练数据中具有2个观察值的转置数据。因此，我们总共有20个特征，结合了10个句子的余弦距离和根匹配。目标变量的范围是0到9。

![](../Images/7b616d1d1df8e6e2ad0a747de57bc06f.png)

> 注意: 对于逻辑回归，标准化数据中的所有列非常重要。

一旦训练数据创建完成，我使用了**多项逻辑回归、随机森林和梯度提升技术**。

**多项逻辑回归**的验证集准确率为**65%**。考虑到原始模型有很多特征且准确率为**79%**，这个模型要简单得多。**随机森林**的准确率为**67%**，最终，**XGBoost**在验证集上的表现最好，准确率为**69%**。

我将添加更多功能（与NLP相关）来改进这些模型。

与特征工程或其他改进相关的想法非常欢迎。所有与上述概念相关的代码都提供 [这里](https://github.com/aswalin/SQuAD)。

在下一部分，我们将重点关注从本部分筛选出的句子中提取文本（正确跨度）。与此同时，查看我的其他博客 [这里](https://medium.com/@aswalin)!

**参考资料**

1.  [包含代码的Github仓库](https://github.com/aswalin/SQuAD)

1.  [Infersent的Facebook Repo](https://github.com/facebookresearch/InferSent)

1.  [解释逻辑回归的最佳资源论文](https://arxiv.org/pdf/1606.05250.pdf)

1.  [解释问题的博客](https://rajpurkar.github.io/mlx/qa-and-squad/)

1.  [排行榜与数据集](https://rajpurkar.github.io/SQuAD-explorer/)

**个人简介: [Alvira Swalin](https://www.linkedin.com/in/alvira-swalin)** ([**Medium**](https://medium.com/@aswalin)) 目前正在USF攻读数据科学硕士学位，我特别感兴趣于机器学习与预测建模。她是Price (Fx)的数据科学实习生。

[原始文档](https://towardsdatascience.com/building-a-question-answering-system-part-1-9388aadff507)。经许可转载。

**相关信息:**

+   [CatBoost 与 Light GBM 与 XGBoost 比较](/2018/03/catboost-vs-light-gbm-vs-xgboost.html)

+   [选择合适的指标来评估机器学习模型 – 第1部分](/2018/04/right-metric-evaluating-machine-learning-models-1.html)

+   [选择合适的指标来评估机器学习模型 – 第2部分](/2018/06/right-metric-evaluating-machine-learning-models-2.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT工作

* * *

### 更多主题

+   [用Python为亚马逊产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [用Hugging Face Transformers构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [如何通过级别系统预测AI成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [学习系统设计：前5名必读书单](https://www.kdnuggets.com/learning-system-design-top-5-essential-reads)

+   [用Python的Watchdog监控你的文件系统](https://www.kdnuggets.com/monitor-your-file-system-with-pythons-watchdog)

+   [从零开始学习机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)
