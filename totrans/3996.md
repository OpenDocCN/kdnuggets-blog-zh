# Python NLP库概览

> 原文：[https://www.kdnuggets.com/a-tour-of-python-nlp-libraries](https://www.kdnuggets.com/a-tour-of-python-nlp-libraries)

![Python NLP库概览](../Images/58939d9717841e882b2724e949acdeaf.png)

使用DALL·E 3生成的图像

NLP，即自然语言处理，是人工智能领域中的一个领域，专注于人类语言与计算机之间的互动。它试图探索和应用文本数据，使计算机能够有意义地理解文本。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

随着NLP领域研究的进展，我们处理计算机中文本数据的方式也在不断发展。在现代，我们使用Python来帮助轻松探索和处理数据。

随着Python成为探索文本数据的首选语言，许多库被专门为NLP领域开发。在这篇文章中，我们将探索各种令人惊叹和实用的NLP库。

那么，让我们开始吧。

## NLTK

[NLTK](https://www.nltk.org/)，或自然语言工具包，是一个具有许多文本处理API和工业级封装的NLP Python库。它是研究人员、数据科学家、工程师等使用的最大NLP Python库之一。它是进行NLP任务的标准Python库。

让我们尝试探索一下NLTK能做什么。首先，我们需要使用以下代码安装该库。

```py
pip install -U nltk 
```

接下来，我们将看看NLTK能做什么。首先，NLTK可以使用以下代码进行分词处理：

```py
import nltk from nltk.tokenize
import word_tokenize

# Download the necessary resources
nltk.download('punkt')

text = "The fruit in the table is a banana"
tokens = word_tokenize(text)

print(tokens) 
```

```py
Output>> 
['The', 'fruit', 'in', 'the', 'table', 'is', 'a', 'banana'] 
```

分词基本上将句子中的每个单词划分为单独的数据。

使用NLTK，我们还可以对文本样本进行词性标注（POS）。

```py
from nltk.tag import pos_tag

nltk.download('averaged_perceptron_tagger')

text = "The fruit in the table is a banana"
pos_tags = pos_tag(tokens)

print(pos_tags) 
```

```py
Output>>
[('The', 'DT'), ('fruit', 'NN'), ('in', 'IN'), ('the', 'DT'), ('table', 'NN'), ('is', 'VBZ'), ('a', 'DT'), ('banana', 'NN')] 
```

NLTK的词性标注器的输出是每个词元及其预期的词性标记。例如，单词“Fruit”是名词（NN），而单词“a”是限定词（DT）。

使用NLTK也可以进行词干提取和词形还原。词干提取是通过剪切前缀和后缀将单词还原为其基本形式，而词形还原则通过考虑单词的词性和形态分析来转换为基本形式。

```py
from nltk.stem import PorterStemmer, WordNetLemmatizer
nltk.download('wordnet')
nltk.download('punkt')

text = "The striped bats are hanging on their feet for best"
tokens = word_tokenize(text)

# Stemming
stemmer = PorterStemmer()
stems = [stemmer.stem(token) for token in tokens]
print("Stems:", stems)

# Lemmatization
lemmatizer = WordNetLemmatizer()
lemmas = [lemmatizer.lemmatize(token) for token in tokens]
print("Lemmas:", lemmas) 
```

```py
Output>> 
Stems: ['the', 'stripe', 'bat', 'are', 'hang', 'on', 'their', 'feet', 'for', 'best']
Lemmas: ['The', 'striped', 'bat', 'are', 'hanging', 'on', 'their', 'foot', 'for', 'best'] 
```

你可以看到词干提取和词形还原的过程对单词的结果略有不同。

这就是NLTK的简单使用方法。你仍然可以做很多事情，但上述API是最常用的。

## SpaCy

[SpaCy](https://spacy.io/) 是一个专门为生产环境设计的 NLP Python 库。它是一个高级库，因其性能和处理大量文本数据的能力而闻名。它是许多 NLP 应用中首选的工业用库。

要安装 SpaCy，你可以查看他们的 [使用页面](https://spacy.io/usage)。根据你的需求，有许多组合可供选择。

让我们尝试使用 SpaCy 进行 NLP 任务。首先，我们将尝试使用库进行命名实体识别 (NER)。NER 是一种将文本中的命名实体识别并分类到预定义类别（如人、地址、地点等）的过程。

```py
import spacy

nlp = spacy.load("en_core_web_sm")

text = "Brad is working in the U.K. Startup called AIForLife for 7 Months."
doc = nlp(text)
#Perform the NER
for ent in doc.ents:
    print(ent.text, ent.label_) 
```

```py
Output>>
Brad PERSON
the U.K. Startup ORG
7 Months DATE 
```

正如你所见，SpaCy 预训练模型了解文档中的哪个词可以被分类。

接下来，我们可以使用 SpaCy 进行依存关系解析并进行可视化。依存关系解析是理解每个词如何通过形成树形结构与其他词相关的过程。

```py
import spacy
from spacy import displacy

nlp = spacy.load("en_core_web_sm")

text = "SpaCy excels at dependency parsing."
doc = nlp(text)
for token in doc:
    print(f"{token.text}: {token.dep_}, {token.head.text}")

displacy.render(doc, jupyter=True) 
```

```py
Output>> 
Brad: nsubj, working
is: aux, working
working: ROOT, working
in: prep, working
the: det, Startup
U.K.: compound, Startup
Startup: pobj, in
called: advcl, working
AIForLife: oprd, called
for: prep, called
7: nummod, Months
Months: pobj, for
.: punct, working 
```

输出应包括所有单词及其词性和相关位置。上述代码还会在你的 Jupyter Notebook 中提供树形可视化。

最后，让我们尝试使用 SpaCy 进行文本相似性分析。文本相似性测量两个文本片段的相似程度或相关性。它有许多技术和测量方法，但我们将尝试最简单的一种。

```py
import spacy

nlp = spacy.load("en_core_web_sm")

doc1 = nlp("I like pizza")
doc2 = nlp("I love hamburger")

# Calculate similarity
similarity = doc1.similarity(doc2)
print("Similarity:", similarity) 
```

```py
Output>>
Similarity: 0.6159097609586724 
```

相似度度量通过提供一个输出分数来测量文本之间的相似性，通常在 0 和 1 之间。分数越接近 1，两篇文本的相似度越高。

你还可以用 SpaCy 做很多事情。探索文档以找到对你工作有用的内容。

## TextBlob

[TextBlob](https://textblob.readthedocs.io/en/dev/) 是一个建立在 NLTK 之上的 NLP Python 库，用于处理文本数据。它简化了许多 NLTK 的用法，并可以简化文本处理任务。

你可以使用以下代码安装 TextBlob：

```py
pip install -U textblob
python -m textblob.download_corpora 
```

首先，让我们尝试使用 TextBlob 进行 NLP 任务。我们首先尝试的是使用 TextBlob 进行情感分析。我们可以通过下面的代码实现这一点。

```py
from textblob import TextBlob

text = "I am in the top of the world"
blob = TextBlob(text)
sentiment = blob.sentiment

print(sentiment) 
```

```py
Output>>
Sentiment(polarity=0.5, subjectivity=0.5) 
```

输出是极性和主观性分数。极性是文本的情感，分数范围从 -1（负面）到 1（正面）。同时，主观性分数范围从 0（客观）到 1（主观）。

我们还可以使用 TextBlob 进行文本校正任务。你可以使用以下代码来完成这一点。

```py
from textblob import TextBlob

text = "I havv goood speling."
blob = TextBlob(text)

# Spelling Correction
corrected_blob = blob.correct()
print("Corrected Text:", corrected_blob) 
```

```py
Output>>
Corrected Text: I have good spelling. 
```

尝试探索 TextBlob 包，以找到适合你文本任务的 API。

## Gensim

[Gensim](https://radimrehurek.com/gensim/) 是一个开源的 Python NLP 库，专注于主题建模和文档相似性分析，特别适用于大数据和流数据。它更关注工业实时应用。

让我们尝试这个库。首先，我们可以使用以下代码进行安装：

```py
pip install gensim 
```

安装完成后，我们可以尝试 Gensim 的功能。让我们尝试使用 Gensim 进行 LDA 主题建模。

```py
import gensim
from gensim import corpora
from gensim.models import LdaModel

# Sample documents
documents = [
    "Tennis is my favorite sport to play.",
    "Football is a popular competition in certain country.",
    "There are many athletes currently training for the olympic."
]

# Preprocess documents
texts = [[word for word in document.lower().split()] for document in documents]

dictionary = corpora.Dictionary(texts)
corpus = [dictionary.doc2bow(text) for text in texts]

#The LDA model
lda_model = LdaModel(corpus, num_topics=2, id2word=dictionary, passes=15)

topics = lda_model.print_topics()
for topic in topics:
    print(topic) 
```

```py
Output>>
(0, '0.073*"there" + 0.073*"currently" + 0.073*"olympic." + 0.073*"the" + 0.073*"athletes" + 0.073*"for" + 0.073*"training" + 0.073*"many" + 0.073*"are" + 0.025*"is"')
(1, '0.094*"is" + 0.057*"football" + 0.057*"certain" + 0.057*"popular" + 0.057*"a" + 0.057*"competition" + 0.057*"country." + 0.057*"in" + 0.057*"favorite" + 0.057*"tennis"') 
```

输出结果是文档样本中词语的组合，形成一个有机的话题。你可以评估结果是否有意义。

Gensim还提供了一种用户嵌入内容的方式。例如，我们使用Word2Vec从词语中创建嵌入。

```py
import gensim
from gensim.models import Word2Vec

# Sample sentences
sentences = [
    ['machine', 'learning'],
    ['deep', 'learning', 'models'],
    ['natural', 'language', 'processing']
]

# Train Word2Vec model
model = Word2Vec(sentences, vector_size=20, window=5, min_count=1, workers=4)

vector = model.wv['machine']
print(vector) 
```

```py
 Output>>
[ 0.01174188 -0.02259516  0.04194366 -0.04929082  0.0338232   0.01457208
 -0.02466416  0.02199094 -0.00869787  0.03355692  0.04982425 -0.02181222
 -0.00299669 -0.02847819  0.01925411  0.01393313  0.03445538  0.03050548
  0.04769249  0.04636709] 
```

Gensim还有许多应用场景。尝试查看文档并评估你的需求。

## 结论

在这篇文章中，我们探讨了几种对许多文本任务至关重要的Python NLP库。所有这些库都对你的工作有用，从文本分词到词嵌入。我们讨论的库包括：

1.  NLTK

1.  SpaCy

1.  TextBlob

1.  Gensim

希望这对你有帮助

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在全职工作于安联印尼期间，他热衷于通过社交媒体和写作媒体分享Python和数据技巧。Cornellius涉猎各种AI和机器学习主题。

### 更多相关主题

+   [数据科学、数据可视化等领域的前38个Python库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [2022年数据科学家应了解的Python库](https://www.kdnuggets.com/2022/04/python-libraries-data-scientists-know-2022.html)

+   [可解释的人工智能：揭开模型决策的面纱的10个Python库](https://www.kdnuggets.com/2023/01/explainable-ai-10-python-libraries-demystifying-decisions.html)

+   [Python数据清洗库简介](https://www.kdnuggets.com/2023/03/introduction-python-libraries-data-cleaning.html)

+   [超越Numpy和Pandas：挖掘鲜为人知的潜力…](https://www.kdnuggets.com/2023/08/beyond-numpy-pandas-unlocking-potential-lesserknown-python-libraries.html)

+   [Level 50数据科学家：必知的Python库](https://www.kdnuggets.com/level-50-data-scientist-python-libraries-to-know)
