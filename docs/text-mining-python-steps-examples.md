# Python中的文本挖掘：步骤和示例

> 原文：[https://www.kdnuggets.com/2020/05/text-mining-python-steps-examples.html](https://www.kdnuggets.com/2020/05/text-mining-python-steps-examples.html)

[评论](#comments)

**作者：[Dhilip Subramanian](https://medium.com/@sdhilip)，数据科学家和人工智能爱好者**

![Header image](../Images/c3cf17396251256b2cdd50b7e755ab3e.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

在今天的场景中，人们的成功方式之一是通过他们如何与他人沟通和分享信息来确定的。这就是语言概念发挥作用的地方。然而，世界上有许多语言，每种语言都有不同的标准和字母，这些单词有意义地排列组合形成句子。每种语言在构建这些句子时都有自己的规则，这些规则也称为语法。

![](../Images/c5f6849132542f8ff523affcaf749f20.png)

在当今世界，根据行业估算，只有20%的数据是以结构化格式生成的，无论是我们发推文、发送WhatsApp消息、电子邮件、Facebook、Instagram还是任何文本消息。大多数数据存在于文本形式中，这是一种高度非结构化的格式。为了从文本数据中提取有意义的见解，我们需要遵循一种叫做文本分析的方法。

### 什么是文本挖掘？

> **文本挖掘是从自然语言文本中提取有意义信息的过程。**

![](../Images/e9b57d93f95c884eb9b1ee9923b3b2ed.png)

### **什么是自然语言处理（NLP）？**

> **自然语言处理（NLP）是计算机科学和人工智能的一部分，处理人类语言。**

换句话说，NLP是文本挖掘的一个组成部分，执行一种特殊的语言分析，**帮助机器“阅读”文本**。它使用不同的方法来**解码人类语言中的模糊性**，包括以下内容：自动摘要、词性标注、消歧义、分块以及消歧义和自然语言理解与识别。我们将通过Python逐步了解所有这些过程。

![](../Images/74d22d2e6faa0c42f09efa9b50e4cff2.png)

首先，我们需要安装NLTK库，它是一个自然语言工具包，用于构建Python程序以处理人类语言数据，并提供易于使用的接口。

### 自然语言处理术语

### 分词

分词是自然语言处理中的第一步。它是将字符串分解成标记的过程，而标记又是较小的结构或单元。分词涉及三个步骤：将复杂句子拆分为单词，理解每个单词在句子中的重要性，最后对输入句子生成结构描述。

**代码：**

```py
# Importing necessary library
import pandas as pd
import numpy as np
import nltk
import os
import nltk.corpus# sample text for performing tokenization
text = “In Brazil they drive on the right-hand side of the road. Brazil has a large coastline on the eastern
side of South America"# importing word_tokenize from nltk
from nltk.tokenize import word_tokenize# Passing the string text into word tokenize for breaking the sentences
token = word_tokenize(text)
token
```

**输出**

```py
['In','Brazil','they','drive', 'on','the', 'right-hand', 'side', 'of', 'the', 'road', '.', 'Brazil', 'has', 'a', 'large', 'coastline', 'on', 'the', 'eastern', 'side', 'of', 'South', 'America']
```

从上述输出可以看出，文本被拆分成了标记。单词、逗号、标点符号都被称为标记。

### 在文本中查找频率差异

**代码 1**

```py
# finding the frequency distinct in the tokens
# Importing FreqDist library from nltk and passing token into FreqDist
from nltk.probability import FreqDist
fdist = FreqDist(token)
fdist
```

**输出**

```py
FreqDist({'the': 3, 'Brazil': 2, 'on': 2, 'side': 2, 'of': 2, 'In': 1, 'they': 1, 'drive': 1, 'right-hand': 1, 'road': 1, ...})
```

‘the’在文本中出现了3次，‘Brazil’出现了2次，等等。

**代码 2**

```py
# To find the frequency of top 10 words
fdist1 = fdist.most_common(10)
fdist1
```

**输出**

```py
[('the', 3),
 ('Brazil', 2),
 ('on', 2),
 ('side', 2),
 ('of', 2),
 ('In', 1),
 ('they', 1),
 ('drive', 1),
 ('right-hand', 1),
 ('road', 1)]
```

### 词干提取

> 词干提取通常指将单词规范化为其基本形式或根形式。

![](../Images/e754c6c3775f95075828815ef3e9d82d.png)

在这里，我们有单词waited、waiting和waits。根词是‘wait’。词干提取有两种方法，即Porter词干提取（去除单词的常见形态和屈折结尾）和Lancaster词干提取（更激进的词干提取算法）。

**代码 1**

```py
# Importing Porterstemmer from nltk library
# Checking for the word ‘giving’ 
from nltk.stem import PorterStemmer
pst = PorterStemmer()
pst.stem(“waiting”)
```

**输出**

```py
'wait'
```

**代码 2**

```py
# Checking for the list of words
stm = ["waited", "waiting", "waits"]
for word in stm :
   print(word+ ":" +pst.stem(word))
```

**输出**

```py
waited:wait
waiting:wait
waits:wait
```

**代码 3**

```py
# Importing LancasterStemmer from nltk
from nltk.stem import LancasterStemmer
lst = LancasterStemmer()
stm = [“giving”, “given”, “given”, “gave”]
for word in stm :
 print(word+ “:” +lst.stem(word))
```

**输出**

```py
giving:giv
given:giv
given:giv
gave:gav
```

Lancaster比Porter词干提取器更激进

### 词形还原

![](../Images/d690d47d94309e2d45e733a64aa0cea6.png)

简单来说，这是一种将单词转换为其基本形式的过程。词干提取和词形还原的区别在于，词形还原考虑上下文，将单词转换为其有意义的基本形式，而词干提取只是去掉最后几个字符，通常导致错误的含义和拼写错误。

例如，词形还原会正确地将‘caring’的基本形式识别为‘care’，而词干提取则会去掉‘ing’部分，将其转换为car。

词形还原可以通过使用Wordnet Lemmatizer、Spacy Lemmatizer、TextBlob、Stanford CoreNLP在Python中实现。

**代码**

```py
# Importing Lemmatizer library from nltk
from nltk.stem import WordNetLemmatizer
lemmatizer = WordNetLemmatizer() 

print(“rocks :”, lemmatizer.lemmatize(“rocks”)) 
print(“corpora :”, lemmatizer.lemmatize(“corpora”))
```

**输出**

```py
rocks : rock
corpora : corpus
```

### 停用词

“停用词”是语言中最常见的词汇，如“the”、“a”、“at”、“for”、“above”、“on”、“is”、“all”。这些词没有提供任何实际意义，通常会从文本中删除。我们可以使用nltk库来删除这些停用词。

**代码**

```py
# importing stopwors from nltk library
from nltk import word_tokenize
from nltk.corpus import stopwords
a = set(stopwords.words(‘english’))text = “Cristiano Ronaldo was born on February 5, 1985, in Funchal, Madeira, Portugal.”
text1 = word_tokenize(text.lower())
print(text1)stopwords = [x for x in text1 if x not in a]
print(stopwords)
```

**输出**

```py
Output of text:
['cristiano', 'ronaldo', 'was', 'born', 'on', 'february', '5', ',', '1985', ',', 'in', 'funchal', ',', 'madeira', ',', 'portugal', '.']Output of stopwords:
['cristiano', 'ronaldo', 'born', 'february', '5', ',', '1985', ',', 'funchal', ',', 'madeira', ',', 'portugal', '.']
```

### 词性标注（POS）

![](../Images/7bc7db51d2c88556da7a5d29f2c05ef8.png)

词性标注用于为给定文本中的每个单词分配词性（如名词、动词、代词、副词、连词、形容词、感叹词），根据其定义和上下文。有许多可用的POS标注工具，其中一些广泛使用的标注工具有NLTK、Spacy、TextBlob、Standford CoreNLP等。

**代码**

```py
text = “vote to choose a particular man or a group (party) to represent them in parliament”
#Tokenize the text
tex = word_tokenize(text)
for token in tex:
print(nltk.pos_tag([token]))
```

**输出**

```py
[('vote', 'NN')]
[('to', 'TO')]
[('choose', 'NN')]
[('a', 'DT')]
[('particular', 'JJ')]
[('man', 'NN')]
[('or', 'CC')]
[('a', 'DT')]
[('group', 'NN')]
[('(', '(')]
[('party', 'NN')]
[(')', ')')]
[('to', 'TO')]
[('represent', 'NN')]
[('them', 'PRP')]
[('in', 'IN')]
[('parliament', 'NN')]
```

### 命名实体识别

这是检测命名实体的过程，如人名、地点名、公司名、数量和货币值。

![图](../Images/c4655974cf144518f570af57ebcd0416.png)

参考：[Sujit Pal](https://www.slideshare.net/sujitpal/soda-v2-named-entity-recognition-from-streaming-test-106598233)

**代码**

```py
text = “Google’s CEO Sundar Pichai introduced the new Pixel at Minnesota Roi Centre Event”#importing chunk library from nltk
from nltk import ne_chunk# tokenize and POS Tagging before doing chunk
token = word_tokenize(text)
tags = nltk.pos_tag(token)
chunk = ne_chunk(tags)
chunk
```

**输出**

```py
Tree('S', [Tree('GPE', [('Google', 'NNP')]), ("'s", 'POS'), Tree('ORGANIZATION', [('CEO', 'NNP'), ('Sundar', 'NNP'), ('Pichai', 'NNP')]), ('introduced', 'VBD'), ('the', 'DT'), ('new', 'JJ'), ('Pixel', 'NNP'), ('at', 'IN'), Tree('ORGANIZATION', [('Minnesota', 'NNP'), ('Roi', 'NNP'), ('Centre', 'NNP')]), ('Event', 'NNP')])
```

### 词组分析

词块化是指提取单独的信息片段并将其分组为更大的块。在自然语言处理（NLP）和文本挖掘的背景下，词块化是将词语或标记分组为块。

![图片](../Images/bafbf88d5ed750b51f406187fe96c82b.png)

参考: [nltk.org](https://www.nltk.org/book/ch07.html)

**代码**

```py
text = “We saw the yellow dog”
token = word_tokenize(text)
tags = nltk.pos_tag(token)reg = “NP: {<DT>?<JJ>*<NN>}” 
a = nltk.RegexpParser(reg)
result = a.parse(tags)
print(result)
```

**输出**

```py
(S We/PRP saw/VBD (NP the/DT yellow/JJ dog/NN))
```

本博客总结了文本预处理，并涵盖了包括分词、词干提取、词形还原、词性标注、命名实体识别和词块化在内的NLTK步骤。

感谢阅读。继续学习，敬请关注更多内容！

### 参考：

1.  [https://www.expertsystem.com/natural-language-processing-and-text-mining/](https://www.expertsystem.com/natural-language-processing-and-text-mining/)

1.  [https://www.nltk.org](https://www.nltk.org/)

1.  [https://www.edureka.co](https://www.geeksforgeeks.org/nlp-chunk-tree-to-text-and-chaining-chunk-transformation/)

1.  [https://www.geeksforgeeks.org/nlp-chunk-tree-to-text-and-chaining-chunk-transformation/](https://www.geeksforgeeks.org/nlp-chunk-tree-to-text-and-chaining-chunk-transformation/)

1.  [https://www.geeksforgeeks.org/part-speech-tagging-stop-words-using-nltk-python/](https://www.learntek.org/blog/categorizing-pos-tagging-nltk-python/)

**简介: [Dhilip Subramanian](https://medium.com/@sdhilip)** 是一名机械工程师，已获得分析学硕士学位。他拥有9年的数据相关领域经验，专注于IT、市场营销、银行、电力和制造业。他对自然语言处理（NLP）和机器学习充满热情。他是[SAS社区](https://communities.sas.com/t5/user/viewprofilepage/user-id/271305)的贡献者，并在Medium平台上撰写有关数据科学各个方面的技术文章。

[原文](https://medium.com/towards-artificial-intelligence/text-mining-in-python-steps-and-examples-78b3f8fd913b)。经许可转载。

**相关：**

+   [使用TensorFlow和Keras进行分词和文本数据准备](/2020/03/tensorflow-keras-tokenization-text-data-prep.html)

+   [五款酷炫的Python数据科学库](/2020/04/five-cool-python-libraries-data-science.html)

+   [自然语言处理食谱：最佳实践和示例](/2020/05/natural-language-processing-recipes-best-practices-examples.html)

### 更多相关话题

+   [SQL LIKE 操作符示例](https://www.kdnuggets.com/2022/09/sql-like-operator-examples.html)

+   [示例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)

+   [选择示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [梦想与现实之间：生成文本与幻觉](https://www.kdnuggets.com/between-dreams-and-reality-generative-text-and-hallucinations)

+   [用Python在5分钟内构建一个文本转语音转换器](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

+   [文本总结开发：使用 GPT-3.5 的 Python 教程](https://www.kdnuggets.com/2023/04/text-summarization-development-python-tutorial-gpt35.html)
