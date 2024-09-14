# 自然语言处理任务的主要方法

> 原文：[https://www.kdnuggets.com/2018/10/main-approaches-natural-language-processing-tasks.html](https://www.kdnuggets.com/2018/10/main-approaches-natural-language-processing-tasks.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![](../Images/b499b10448af1c7b977d5420fb0113a5.png)

来源：[2017年值得关注的5大语义技术趋势](https://ontotext.com/top-5-semantic-technology-trends-2017/)（ontotext）。

我们之前讨论了若干个[自然语言处理（NLP）入门话题](/2018/06/getting-started-natural-language-processing.html)，我原计划在此时继续介绍一些有用的实际应用。然后我注意到我忽略了一些在继续之前可能需要的重要入门讨论。其中第一个话题将在这里讨论，更多的是对目前常见的自然语言处理任务方法的一般讨论。这也引出了一个问题：具体来说，有哪些类型的NLP任务呢？

另一个被指出的被忽视的基础话题将在下周讨论，那将涉及“我们如何为机器学习系统表示文本？”这个常见问题。

不过，首先回到今天的话题。让我们看看我们可以使用的NLP任务的主要方法。然后我们将查看我们可以用这些方法处理的具体NLP任务。

### NLP任务的方法

虽然并非绝对明确，但解决NLP任务的主要方法有3个大类。

![图片](../Images/06df8524acd26c1d5673e7fcf1602ad5.png)

图1\. 使用 [spaCy](https://spacy.io/) 的依存解析树。

**1\. 基于规则**

基于规则的方法是最早的NLP方法。你可能会问，为什么它们仍然被使用？这是因为它们经过验证且效果良好。应用于文本的规则可以提供很多洞察：想想通过找出哪些词是名词，哪些动词以-ing结尾，或者是否可以识别出一个可识别的Python代码模式，我们可以学到什么。[正则表达式](https://en.wikipedia.org/wiki/Regular_expression) 和 [上下文无关文法](https://en.wikipedia.org/wiki/Context-free_grammar) 是基于规则的NLP方法的经典例子。

基于规则的方法：

+   倾向于关注模式匹配或解析。

+   通常可以被认为是“填空”方法。

+   精度较低，召回率高，意味着它们在特定应用场景中表现优异，但通常在泛化时会出现性能下降。

**2\. “传统”机器学习**

“传统”的机器学习方法包括概率建模、似然最大化和线性分类器。值得注意的是，这些不是神经网络模型（见下文）。

传统的机器学习方法的特点是：

+   训练数据 - 在这种情况下，是带标记的语料库

+   特征工程 - 词类型、周围词汇、大写、复数等

+   在参数上训练模型，然后在测试数据上拟合（这是机器学习系统的一般特点）

+   推理（将模型应用于测试数据）以找到最可能的词汇、下一个词、最佳类别等为特征

+   “语义槽填充”

**3\. 神经网络**

这类似于“传统”机器学习，但有一些不同之处：

+   特征工程通常被跳过，因为网络会“学习”重要特征（这通常是使用神经网络进行NLP的一个主要好处）

+   相反，输入神经网络的是原始参数流（“词汇”-- 实际上是词汇的向量表示），而不是经过工程化的特征

+   非常大的训练语料库

在NLP中有用的特定神经网络包括递归神经网络（RNNs）和卷积神经网络（CNNs）。

**为什么对NLP使用“传统”机器学习（或基于规则）方法？**

+   对于序列标注仍然有效（使用概率建模）

+   神经网络中的一些想法与早期方法非常相似（word2vec在概念上类似于分布语义方法）

+   使用传统方法中的方法来改进神经网络方法（例如，词对齐和注意机制是相似的）

**为什么深度学习胜过“传统”机器学习？**

+   许多应用中的SOTA（例如，机器翻译）

+   目前在NLP领域有大量研究（大多数？）

**重要的是**，神经网络和非神经网络方法本身都可以对现代NLP有用；它们也可以一起使用或研究，以获得最大潜在利益

### NLP任务是什么？

我们有了方法，但任务本身呢？

这些任务类型之间没有明确的界限；然而，目前许多任务已经相当明确。给定的宏观NLP任务可能包括各种子任务。

我们首先概述了主要的方法，因为这些技术通常对初学者很重要，但了解NLP任务的类型是有好处的。以下是NLP任务的主要类别。

**1\. 文本分类任务**

+   表示：词袋模型（不保留词序）

+   目标：预测标签、类别、情感

+   应用：过滤垃圾邮件，根据主要内容对文档进行分类

**2\. 词序列任务**

+   表示：序列（保留词序）

+   目标：语言建模 - 预测下一个/之前的词汇，文本生成

+   应用：翻译、聊天机器人、序列标注（预测序列中每个词的POS标签）、命名实体识别

**3\. 文本意义任务**

+   表示：词向量，即将词汇映射到向量（*n*维数值向量），也称为嵌入

+   目标：我们如何表示意义？

+   应用：寻找相似词（相似向量）、句子嵌入（相对于词嵌入）、主题建模、搜索、问答

![图像](../Images/5b9fbdd9038ed540762215991c040a03.png)

图2\. 使用spaCy的命名实体识别（NER）（文本摘自[这里](https://www.nomadicmatt.com/travel-blogs/three-days-in-new-york-city/)）。

**4\. 序列到序列任务**

+   NLP中的许多任务可以这样表述

+   示例包括机器翻译、总结、简化、问答系统

+   此类系统的特点是编码器和解码器，这些编码器和解码器互补工作以找到文本的隐藏表示，并使用该隐藏表示

**5\. 对话系统**

+   对话系统的2个主要类别，按其使用范围分类

+   目标导向对话系统专注于在特定、受限的领域中发挥作用；精确度更高，但可泛化性较差

+   对话式对话系统关注于在更广泛的背景下提供帮助或娱乐；精确度较低，但泛化性更强

无论是对话系统还是规则基础与更复杂方法在解决NLP任务中的实际差异，请注意精确度与泛化性之间的权衡；通常你会在一个方面做出牺牲以提高另一个方面。

在下一篇文章中覆盖文本数据表示后，我们将深入探讨一些更高级的NLP主题，并进行一些有用NLP任务的实际探索。

**参考文献：**

1.  [从语言到信息](https://web.stanford.edu/class/cs124/)，斯坦福大学

1.  [自然语言处理](https://www.coursera.org/learn/language-processing)，国立研究大学高等经济学院（Coursera）

1.  [自然语言处理中的神经网络方法](https://www.morganclaypool.com/doi/abs/10.2200/S00762ED1V01Y201703HLT037)，Yoav Goldberg

**相关**：

+   [预处理文本数据的一般方法](/2017/12/general-approach-preprocessing-text-data.html)

+   [处理文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

+   [文本数据预处理：Python 实操指南](/2018/03/text-data-preprocessing-walkthrough-python.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速你的网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

### 更多相关主题

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [是什么让Python成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [自然语言处理中的N-gram语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)
