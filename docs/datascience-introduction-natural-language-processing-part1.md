# 自然语言处理简介，第1部分：词汇单元

> 原文：[https://www.kdnuggets.com/2017/02/datascience-introduction-natural-language-processing-part1.html](https://www.kdnuggets.com/2017/02/datascience-introduction-natural-language-processing-part1.html)

**由[DataScience.com](https://www.datascience.com/blog/introduction-to-natural-language-processing-lexical-units-learn-data-science-tutorials)赞助的帖子。**

在本系列中，我们将深入探讨与自然语言处理的研究和应用相关的核心概念。下面的第一部分介绍了该领域，并解释了如何识别词汇单元作为数据预处理的一种方法。

### 自然语言处理简介

[自然语言处理](https://www.datascience.com/resources/article/2017-predictions-data-science-providing-value)是一组允许计算机和人类进行互动的技术。考虑从某些数据生成过程提取信息的过程：一家公司希望预测其网站上的用户流量，以便提供足够的计算资源（服务器硬件）来满足需求。工程师可以定义相关信息为请求的数据量。由于他们控制数据生成过程，他们可以在网站上添加逻辑，将每个数据请求存储为一个变量。然后，他们可以将测量单位定义为请求的数据量，以字节为单位，从而使我们能够将信息表示为整数。有了良好的信息表示，工程师可以将其存储在表格数据库中，以便分析师可以根据这些历史数据进行预测。

自然语言处理是上述步骤的应用——定义信息的表示、从数据生成过程解析信息以及构建、存储和使用存储信息的数据结构——到嵌入自然语言的信息。

使语言成为自然语言的正是使自然语言处理变得困难的原因；自然语言中信息表示的规则是在没有预定的情况下演变而来的。这些规则可以是高层次的和抽象的，例如如何使用讽刺来传达意义；也可以是相当低层次的，例如使用字符“s”来表示名词的复数。自然语言处理涉及识别并利用这些规则，通过代码将非结构化的语言数据转换为具有模式的信息。语言数据可以是正式的文本，例如报纸文章，也可以是非正式的语音，例如电话交谈的录音。来自不同上下文和数据源的语言表达将具有不同的语法、句法和语义规则。在一个环境中有效的提取和表示自然语言信息的策略，在其他环境中往往无效。

### 商业用途

公司通常可以访问包含有价值信息的自然语言记录。产品评论或甚至Twitter上的推文可能包含与产品相关的特定投诉或功能请求，这可以帮助优先排序和评估提案。[在线市场](https://www.datascience.com/resources/article/business-intelligence-to-data-science-for-retailers)可能有可用的商品描述，有助于定义产品分类法。数字报纸可能有在线文章的档案，可用于构建搜索引擎，让用户找到相关内容。代表自然语言的信息也可以用于构建强大的应用程序，如[回答问题的机器人](https://www.datascience.com/blog/training-a-predictive-model-chatbot-for-business-applications)或翻译软件。

![intro-to-natural-language-processing-part-1-lexical-units-sentiment.png](../Images/e477b47b0cdf19f3986ca94599611841.png)

*自然语言处理可以用来从文本中识别特定的投诉。*

需要自然语言处理的项目通常涉及这些挑战。解决这些问题通常要求我们将多个子任务串联起来，每个子任务可能有多种解决方案。自然语言处理方法的范围可能会让人望而却步，因为它高度专业化、广泛且缺乏一个总体概念框架。虽然自然语言处理的完整总结超出了本文的范围，但我们将介绍一些在通用自然语言处理工作中常用的概念。我们假设我们可以使用文本数据（而非需要额外语音识别步骤的听觉数据）。

### 识别词汇单元

由于自然语言通常由单词组成，许多自然语言处理项目的初始步骤是识别某些原始文本中的单词。然而，单词的概念可能过于限制或模糊。字符串“cats”和“cat”是词典中相同条目的不同形式；它们应该被视为等同吗？“Star Wars”在词典中没有条目，虽然它包含一个空格，但我们认为它是一个整体。这些是定义词汇单元时涉及的挑战，词汇单元代表词汇的基本元素。

对于特定任务，研究人员必须定义什么构成合适的词汇单元。单数和复数形式的单词是否应被视为同一个词汇单元？假设我们正在构建一个问答系统，并收到以下查询：

A: "找到离我最近的电影院。"

B: "找到离我最近的电影院。"

在A中，用户暗示她想查看多个电影院，而在B中，她只是想要一个最近的电影院。忽略单数和复数之间的区别会降低我们应用的质量。

![intro-to-natural-language-processing-part-1-lexical-units-plurals.png](../Images/64fe1fa4313cf54542605a72ff6aa6a3.png)

另外，假设我们要总结以下产品评论：

A: “该产品存在一些连接问题。”

B: “该产品存在一个连接问题。”

在这里，“问题”和“问题”之间的区别并不相关。

虽然不完全详尽，但分词和规范化是从自然语言中解析词汇单元的两个常见步骤。[分词](https://www.ibm.com/developerworks/community/blogs/nlp/entry/tokenization?lang=en)是将字符序列分割成词素的过程，每个词素代表一个术语的实例。[规范化](http://nlp.stanford.edu/IR-book/html/htmledition/normalization-equivalence-classing-of-terms-1.html)是我们用来将术语压缩成词汇单元的一系列步骤。

分词算法可以很简单且确定性，例如每当字符不是字母数字时将字符分割成词素。非字母数字字符可能是空格、标点符号、井号等。我们可以使用[模式匹配](https://en.wikipedia.org/wiki/Pattern_matching)来实现这种方法，根据一些规则检查字符或字符序列的存在，其中我们将这些模式定义为[正则表达式](https://en.wikipedia.org/wiki/Regular_expression)（通常缩写为“regex”）。例如，我们可以用正则表达式字符串“*0-9*”或“*\d.*”来表示所有数字字符的集合。我们可以将带有修饰符的术语组合成复杂的搜索模式。我们可以通过每次匹配查询`[^A-Za-z0-9]+`来将字符分割在非字母数字字符上。正则表达式的应用非常广泛，几乎所有现代编程语言中都有相应的实现。

很容易找到这种策略失败的例子（“didn't ≠ ["didn", "t"]）。在一些应用中，研究人员使用多个复杂的正则表达式查询和特定于形态的规则来捕获这些模式，并通过[有限状态机](http://www.cs.rochester.edu/u/james/CSC248/Lec7.pdf)来确定正确的分词。编码一个详尽的规则集可能很困难，并且将取决于应用程序和分析的文本数据类型（尽管已有一些[部分规则集](https://github.com/explosion/spaCy/blob/master/spacy/en/tokenizer_exceptions.py)被汇编）。为了适应新的语料库，可以通过在手动分词的文本上训练统计模型来构建分词器，尽管由于确定性方法的成功，这种方法在实践中很少使用。这些模型可能会查看字符属性序列，例如是否连续的字母数字字符以大写字母开头，并将分词建模为这些属性的函数。模型学习使用的规则将取决于提供的训练语料库（如Twitter、医学文章等）。

另一个挑战是多项字符序列可以构成一个词汇单位。这些多项序列可能是像“New York”这样的命名实体，在这种情况下，我们进行[命名实体识别](https://en.wikipedia.org/wiki/Named-entity_recognition)或只是常见短语。解决这个问题的一种方法是通过将所有长度为n的项集（n-grams）作为标记来允许表示中的一些冗余。如果我们仅限于单个词和二元组，“New York”将被标记为“New”，“York”，“New York”。为了增加一些复杂性，而不是穷尽所有n-grams，我们可以选择一组术语的最高阶n-gram表示，依据某些条件，比如它是否存在于硬编码词典（称为地名词典）中，或在我们的数据集中是否常见。或者，我们可以使用机器学习模型。如果给定单词的概率，*p*(word[*i*] | word[*i*-1])，在前面的单词下足够高，我们可能会将这个序列视为一个二项词汇单位（或者，我们可以使用公式*p*(word[*i*] | word[*i*-1]) = bigram) 使用标记的示例）。虽然平滑可以帮助改善问题，但这些语言模型通常难以泛化，并且需要一定程度的迁移学习、特征工程、确定性或抽象。概率n-gram模型需要标记的示例、机器学习算法和特征提取器（后两者包含在[斯坦福NER软件](http://nlp.stanford.edu/software/CRF-NER.shtml)中）。如果这些模型不值得投资，高质量的预训练模型通常可以在新的数据集上成功使用。

在分词后，我们可能希望对我们的词元进行标准化。 [标准化](http://nlp.stanford.edu/IR-book/html/htmledition/normalization-equivalence-classing-of-terms-1.html)是一组规则，旨在将所有词汇等价类的实例减少到其规范形式。

这可能包括如下过程：

+   复数 -> 单数（例如 cats -> cat）

+   过去时态 -> 现在时态（例如 ran -> run）

+   副词形式 -> 形容词形式（quickly -> quick）

+   连字符 -> 连接

标准化的一种方法是通过一个称为[词形还原](https://en.wikipedia.org/wiki/Lemmatisation)的过程将单词减少到其词典形式。一个术语的词元具有词典条目中包含的所有元数据：词性、定义（词义）和词干。词形还原传统上需要一个[形态学解析器](http://cs.union.edu/%7Estriegnk/courses/nlp-with-prolog/html/node21.html)，在这个解析器中，我们根据词语的形态学元素（前缀、后缀等）完全特征化某个未经处理的词语（时态、复数形式、词性等）。为了构建一个解析器，我们创建一个已知词干和词缀的词典（词汇表），并包含有关它们的元数据，如可能的词性，列举规则（形态学语法）以规范词素如何组合在一起（例如复数修饰符“-s”必须跟在名词后面），最后列举规则（[正字法规则](https://en.wikipedia.org/wiki/Orthography)）以规范在不同形态状态下单词的变化（例如，一个以“-c”结尾的过去时动词必须添加一个“k”，如“picnic -> picnicked”）。这些规则和术语传递给一个有限状态机，该状态机遍历输入，维护一个状态或一组特征值，然后在检查规则和词汇表与文本匹配时更新（类似于正则表达式的工作方式）。

我们如何定义等价类的特异性（哪些形态学元素相关），以及我们使用的标准化程度和提取的形态学元数据，取决于我们的应用。在信息检索中，我们通常只关心文本的一些高层语义。所有的[词素](https://en.wikipedia.org/wiki/Morpheme)或传达意义的元素（除了词干）都可以与所有形态学元数据（如时态）一起丢弃。词干提取算法通常使用替换规则，如：

+   如果给定的词干包含一个元音，我们会从所有包含该词干的词元中移除“-ing”（如“barking”→“bark”，但“string”→“string”）。

Porter 词干提取器可能是最著名的替换类型词干提取算法，尽管还有其他算法。所有这些类型的词干提取器都容易出错，但简单且易于实现，并且是大多数应用工作的良好起点。

最终，构建预处理管道的目标包括：

+   所有相关信息被提取。

+   所有不相关的信息都被丢弃。

+   文本被表示为足够少的等价类。

+   数据应以有用的形式存在，如 ASCII 编码的字符串列表或抽象数据结构，如句子或文档。

相关性、充分性和有用性的定义取决于项目的要求、开发团队的实力以及时间和资源的可用性。此外，在许多情况下，预处理管道的强度可以决定项目的整体成功。因此，在构建管道时需要谨慎。

在将原始文本转换为字符串或词汇单元的分词数组之后，研究人员或开发者可能会采取步骤对其文本数据进行预处理，例如字符串编码、停用词和标点符号去除、拼写校正、词性标注、分块、句子分割和语法分析。下表展示了研究人员在给定任务和一些原始文本时可能使用的一些处理类型的示例：

| 原始文本 | 处理后 | 步骤 | 任务 | 管道如何适应任务 |
| --- | --- | --- | --- | --- |
| She sells seashells by the seashore. | ['she','sell','seashell','seashore'] | 分词，词形还原，停用词去除，标点去除 | 主题建模 | 我们只关注高级、主题性强和语义丰富的词 |
| John is capable. | John/PROPN is/VERB capable/ADJ ./PUNCT | 分词，词性标注 | 命名实体识别 | 我们关注每个词，但希望指明每个词所扮演的角色，以便构建命名实体识别候选列表 |
| Who won? I didn't check the scores. | [u'who', u'win'], [u'i',u'do',u'not',u'check',u'score'] | 分词，词形还原，句子分割，标点去除，字符串编码 | 情感分析 | 我们需要所有词，包括否定词，因为它们可以否定积极的陈述，但不关心时态或词形 |

预处理后，我们通常需要采取额外步骤将文本信息以量化形式表示。在本系列的第二部分中，我们将讨论各种抽象和数值文本表示方法。

本文中包含的依赖关系图是使用了令人惊叹的开源库 DisplaCy 构建的。点击[这里](https://github.com/explosion/displacy)查看。

*想要继续学习吗？下载我们的[Forrester新研究](https://www.datascience.com/resources/white-papers/forrester-data-science-platforms-create-business-value)，了解保持公司在数据科学前沿的工具和实践。*

[原文](https://www.datascience.com/blog/introduction-to-natural-language-processing-lexical-units-learn-data-science-tutorials)。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关主题

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [自然语言处理的温和介绍](https://www.kdnuggets.com/2022/06/gentle-introduction-natural-language-processing.html)

+   [自然语言处理简介](https://www.kdnuggets.com/introduction-to-natural-language-processing)

+   [自然语言处理关键术语解释](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [自然语言处理任务中的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [图像识别和自然语言处理中的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)
