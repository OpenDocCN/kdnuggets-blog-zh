# 使用 spaCy 开始自然语言处理

> 原文：[https://www.kdnuggets.com/2018/05/getting-started-spacy-natural-language-processing.html](https://www.kdnuggets.com/2018/05/getting-started-spacy-natural-language-processing.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

在之前的一系列文章中，我们探讨了一些与文本数据科学任务相关的一般性观点，无论是自然语言处理、文本挖掘，还是其他相关的主题。在[这些文章中最新的一篇](/2018/03/text-data-preprocessing-walkthrough-python.html)中，我们介绍了使用 Python 进行文本数据预处理的流程。更具体地说，我们查看了使用 Python *和 NLTK* 进行的文本预处理流程。虽然我们没有进一步探讨使用 NLTK 进行的数据预处理以外的内容，但理论上，该工具包可以用于进一步的分析任务。

尽管 NLTK 是一个很棒的自然语言... 好吧，工具包（所以名字叫做 NLTK），但它并不优化于构建生产系统。如果只是用 NLTK 来预处理数据，这可能无关紧要，但如果你计划构建一个端到端的 NLP 解决方案，并且在选择适当的工具来构建该系统，那么使用同样的工具来预处理数据可能是有意义的。

![](../Images/f259bf6da4be96051a36006ff848f1cc.png)

虽然 NLTK 是在 *学习* NLP 时构建的，[spaCy](https://spacy.io/) 则专门设计用于成为一个实现生产就绪系统的有用库。

> spaCy 旨在帮助你做实际的工作——构建实际的产品，或获取实际的见解。该库尊重你的时间，并试图避免浪费时间。它安装简单，API 简单高效。我们喜欢将 spaCy 视为自然语言处理的 Ruby on Rails。

spaCy 是有立场的，因为它不允许过多地混合和匹配可能被视为 NLP 流水线模块的内容，理由是特定的词形还原器，例如，并没有优化以良好地配合特定的分词器。尽管这种权衡在 **某些** 方面减少了灵活性，但最终应该会提高性能。

你可以通过[这个视频](https://www.youtube.com/watch?v=jB1-NukGZm0)快速了解 spaCy 及其设计哲学和选择，这是由 spaCy 开发者[Matthew Honnibal](https://twitter.com/honnibal)和[Ines Montani](https://twitter.com/_inesmontani)在美国旧金山的数据研究所联合主办的机器学习聚会上进行的讲座。

spaCy 宣称自己是“为深度学习准备文本的最佳方式。”由于 [之前的演练](/2018/03/text-data-preprocessing-walkthrough-python.html) 没有使用 NLTK（任务相关的噪声移除以及规范化过程中的一些步骤），我们不会在这里重复整个帖子，只是将 spaCy 替代 NLTK 的特定部分，因为那样会浪费大家的时间。相反，我们将探讨 **一些** 用于 NLTK 的相同功能如何通过 spaCy 实现，然后进行一些额外的任务，这些任务 **可能** 被认为是预处理，取决于你的最终目标，但也可能会渗透到后续的 [文本数据科学任务](/2017/11/framework-approaching-textual-data-tasks.html) 中。

### 开始使用

在执行任何操作之前，你需要先安装 spaCy 以及它的英语语言模型。

```py

$ pip3 install spacy
$ python3 -m spacy download en
```

我们需要一个文本样本来使用：

```py

sample = u"I can't imagine spending $3000 for a single bedroom apartment in N.Y.C."

```

现在让我们导入 spaCy，以及 displaCy（用于可视化 spaCy 的模型）和一份英语停用词列表（我们将在下面使用）。我们还加载英语语言模型作为 `Language` 对象（我们将按照 spaCy 的惯例称之为 'nlp'），然后对我们的样本文本调用 nlp 对象，这将返回一个处理过的 `Doc` 对象（我们巧妙地称之为 'doc'）。

```py

import spacy
from spacy import displacy
from spacy.lang.en.stop_words import STOP_WORDS

nlp = spacy.load('en')
doc = nlp(sample)

```

就这样。从 [spaCy 文档](https://spacy.io/usage/spacy-101)：

> 即使 `Doc` 已经被处理——例如，拆分成单独的单词并进行了注释——它仍然保留了**原始文本的所有信息**，例如空白字符。你始终可以获取一个词元在原始字符串中的偏移量，或者通过连接词元及其后面的空白字符来重建原始文本。这样，在使用 spaCy 处理文本时，你将不会丢失任何信息。

这是 spaCy 设计理念的核心；我鼓励你观看 [这个视频](https://www.youtube.com/watch?v=jB1-NukGZm0)。

现在让我们尝试一些操作。注意执行这些操作所需的代码量非常少。

### 分词

打印出我们样本的词元是很简单的：

```py

# Print out tokens
for token in doc:
    print(token)
```

```py

I
ca
n't
imagine
spending
$
3000
for
a
single
bedroom
apartment
in
N.Y.C.
```

从上面回顾一下，`Doc` 对象在传递给 `Language` 对象时就已经处理完毕，因此在这个阶段分词已经完成；我们只是访问那些现有的词元。从 [文档](https://spacy.io/api/doc)：

> `Doc` 是一系列 `Token` 对象。访问句子和命名实体，将注释导出为 numpy 数组，进行无损压缩序列化到二进制字符串中。`Doc` 对象持有一个 `TokenC` 结构体的数组。Python 层面的 `Token` 和 `Span` 对象是该数组的视图，即它们不拥有数据本身。

我们没有讨论 spaCy 实现的具体细节，但了解其底层代码的样子——尤其是它是如何编写的（以及为什么）——能帮助你理解它为何如此快速。我再一次鼓励你观看 [这个视频](https://www.youtube.com/watch?v=jB1-NukGZm0)。

虽然不是必要的，但考虑到设计上的差异，如果你想将这些 tokens 提取到自己的列表中（类似于我们在 NLTK 演练中所做的）：

```py

# Store tokens as list, print out
tokens = [token for token in doc]
print(tokens)

```

```py
[I, ca, n't, imagine, spending, $, 3000, for, a, single, bedroom, apartment, in, N.Y.C.]
```

### 识别停用词

让我们识别停用词。我们在上面导入了词汇表，所以只需迭代存储在 `Doc` 对象中的 tokens，并进行比较：

```py

for word in doc:
    if word.is_stop == True:
        print(word)

```

```py

ca
for
a
in
```

请注意，我们没有触及上面创建的 tokens 列表，而只是依赖于预处理的 `Doc` 对象。

### 词性标注

一个 `Doc` 对象包含 `Token` 对象，你可以查看 `Token` 的 [文档](https://spacy.io/api/token) 以了解每个 token 在我们请求之前已经包含的数据，以及下面显示的具体信息。

```py

for token in doc:
    print(token.text, token.lemma_, token.pos_, token.tag_, token.dep_,
          token.shape_, token.is_alpha, token.is_stop)
```

```py

I -PRON- PRON PRP nsubj X True False
ca can VERB MD aux xx True True
n't not ADV RB neg x'x False False
imagine imagine VERB VB ROOT xxxx True False
spending spend VERB VBG xcomp xxxx True False
$ $ SYM $ nmod $ False False
3000 3000 NUM CD dobj dddd False False
for for ADP IN prep xxx True True
a a DET DT det x True True
single single ADJ JJ amod xxxx True False
bedroom bedroom NOUN NN compound xxxx True False
apartment apartment NOUN NN pobj xxxx True False
in in ADP IN prep xx True True
N.Y.C. n.y.c. PROPN NNP pobj X.X.X. False False

```

虽然这本身信息量很大，并且对于特定的 NLP 目标可能以各种方式有用，但我们可以通过 [displaCy](https://spacy.io/usage/visualizers) 来进行可视化，以获得更简洁的视图：

```py
displacy.render(doc, style='dep', jupyter=True, options={'distance': 70})
```

[![POS 可视化](../Images/121e0dc7f6e5229b9afaab8f83a11158.png)](https://image.ibb.co/haQoeS/spacy_pos_visualized.jpg)

### 命名实体识别

一个 `Doc` 对象也已经处理了命名实体。用以下方法访问它们：

```py

# Print out named entities
for ent in doc.ents:
    print(ent.text, ent.start_char, ent.end_char, ent.label_)

```

```py

3000 26 30 MONEY
N.Y.C. 65 71 GPE

```

从上面的代码可以确定，首先输出命名实体，然后是其在文档中的起始字符索引，接着是结束字符索引，最后是实体类型（标签）。

displaCy 的可视化再次显得非常实用：

```py
displacy.render(doc, style='ent', jupyter=True)
```

![NER 可视化](../Images/e67a700939e14859796d357891c1e984.png)

我再强调一遍：回过头来看看这篇帖子，看看我们完成这些任务所需的代码是多么少。

这仅仅触及了我们可能希望通过 NLP 或像 spaCy 这样强大的工具实现的表面。显而易见，NLP 预处理任务以及其他数据形式的任务（包括文本和字符串操作）通常可以通过多种策略、工具和库来完成。在我看来，spaCy 因其易用性和 API 简单性而表现出色。我鼓励那些一直关注我撰写的 NLP 相关帖子的人，如果他们对自然语言处理认真，可以去了解一下 spaCy。

为了更好地了解 spaCy，我推荐这些资源：

+   [spaCy 101：你需要了解的一切](https://spacy.io/usage/spacy-101)

+   [语言处理管道](https://spacy.io/usage/processing-pipelines)

+   [提高数据科学生产力；spaCy 和 Prodigy 的创始人](https://www.youtube.com/watch?v=jB1-NukGZm0)（SF 机器学习见面会演讲）

+   [Matthew Honnibal - 设计 spaCy：工业级 NLP](https://www.youtube.com/watch?v=gJJQs47aUQ0)（PyData Berlin 2016 演讲）

**相关内容**：

+   [文本数据预处理：Python 中的操作流程](/2018/03/text-data-preprocessing-walkthrough-python.html)

+   [处理文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

+   [文本数据预处理的一般方法](/2017/12/general-approach-preprocessing-text-data.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 领域

* * *

### 了解更多相关话题

+   [使用 spaCy 进行自然语言处理](https://www.kdnuggets.com/2023/01/natural-language-processing-spacy.html)

+   [开始使用 spaCy 进行 NLP](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [自然语言处理关键术语解释](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)
