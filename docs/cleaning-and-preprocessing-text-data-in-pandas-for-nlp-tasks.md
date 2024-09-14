# 在Pandas中清理和预处理文本数据以用于NLP任务

> 原文：[https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

![NLP（自然语言处理）](../Images/5ae96669735b9ef013b0b884100fcb7a.png)

图片由作者提供

清理和预处理数据通常是构建由数据驱动的AI和机器学习解决方案中最令人畏惧但又至关重要的阶段，而文本数据也不例外。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

本教程破冰地解决了为NLP任务（如语言模型（LMs）可以解决的任务）准备文本数据的挑战。通过将你的文本数据封装在pandas DataFrames中，以下步骤将帮助你准备文本数据，以便被NLP模型和算法处理。

## 将数据加载到Pandas DataFrame中

为了保持本教程简单明了并专注于理解必要的文本清理和预处理步骤，让我们考虑一个包含四个单属性文本数据实例的小样本，这些实例将被移动到一个pandas DataFrame实例中。从现在起，我们将在这个DataFrame对象上应用每个预处理步骤。

```py
import pandas as pd
data = {'text': ["I love cooking!", "Baking is fun", None, "Japanese cuisine is great!"]}
df = pd.DataFrame(data)
print(df)
```

输出：

```py
 text
0   I love cooking!
1   Baking is fun
2   None
3   Japanese cuisine is great!
```

## 处理缺失值

你注意到示例数据实例中的'None'值了吗？这被称为缺失值。缺失值由于各种原因通常会被收集，往往是偶然的。关键点是：你需要处理这些值。最简单的方法是检测并删除包含缺失值的实例，如下代码所示：

```py
df.dropna(subset=['text'], inplace=True)
print(df)
```

输出：

```py
 text
0    I love cooking!
1    Baking is fun
3    Japanese cuisine is great!
```

## 规范化文本以保持一致

规范化文本意味着标准化或统一那些在不同实例中可能以不同格式出现的元素，例如日期格式、全名或大小写敏感性。规范化文本的最简单方法是将所有文本转换为小写，如下所示。

```py
df['text'] = df['text'].str.lower()
print(df)
```

输出：

```py
 text
0             i love cooking!
1               baking is fun
3  japanese cuisine is great!
```

## 去除噪音

噪音是指不必要或意外收集的数据，如果处理不当，可能会妨碍后续的建模或预测过程。在我们的示例中，我们假设像“！”这样的标点符号在后续的NLP任务中是不需要的，因此我们通过使用正则表达式检测文本中的标点符号来进行噪音去除。Python的're'包用于基于正则表达式匹配进行文本操作。

```py
import re
df['text'] = df['text'].apply(lambda x: re.sub(r'[^\w\s]', '', x))
print(df)
```

输出：

```py
 text
0             i love cooking
1              baking is fun
3  japanese cuisine is great
```

## 对文本进行分词

分词可以说是进行 NLP 和语言模型使用前最重要的文本预处理步骤之一 - 以及将文本编码为数字表示 - 它包括将每个文本输入拆分成一系列片段或标记。在最简单的情况下，标记通常与单词相关，但在复合词等某些情况下，一个单词可能会导致多个标记。某些标点符号（如果它们之前没有被作为噪音去除）有时也会被识别为独立的标记。

这段代码将我们三个文本条目中的每一个分割成单独的单词（标记），并将其添加为 DataFrame 中的新列，然后显示更新后的数据结构及其两列。应用的简化分词方法被称为简单的空格分词：它仅使用空格作为检测和分隔标记的标准。

```py
df['tokens'] = df['text'].str.split()
print(df)
```

输出：

```py
 text                          tokens
0             i love cooking              [i, love, cooking]
1              baking is fun               [baking, is, fun]
3  japanese cuisine is great  [japanese, cuisine, is, great]
```

## 移除停用词

一旦文本被分词，我们会过滤掉不必要的标记。这通常是停用词的情况，比如冠词“a/an, the”或连词，这些词对文本没有实际语义贡献，应被移除以便后续有效处理。这个过程依赖于语言：下面的代码使用 NLTK 库下载英语停用词字典，并从标记向量中过滤掉它们。

```py
import nltk
nltk.download('stopwords')
stop_words = set(stopwords.words('english'))
df['tokens'] = df['tokens'].apply(lambda x: [word for word in x if word not in stop_words])
print(df['tokens'])
```

输出：

```py
0               [love, cooking]
1                 [baking, fun]
3    [japanese, cuisine, great]
```

## 词干提取和词形还原

快完成了！词干提取和词形还原是可能根据具体任务使用的附加文本预处理步骤。词干提取将每个标记（单词）还原为其基础或根形式，而词形还原则进一步将其还原为词形或基础词典形式，取决于上下文，例如“best” -> “good”。为了简化起见，我们在这个示例中将只应用词干提取，使用 NLTK 库中实现的 PorterStemmer，借助 wordnet 数据集的词根关联。得到的词干词保存在 DataFrame 中的新列中。

```py
from nltk.stem import PorterStemmer
nltk.download('wordnet')
stemmer = PorterStemmer()
df['stemmed'] = df['tokens'].apply(lambda x: [stemmer.stem(word) for word in x])
print(df[['tokens','stemmed']])
```

输出：

```py
 tokens                   stemmed
0             [love, cooking]              [love, cook]
1               [baking, fun]               [bake, fun]
3  [japanese, cuisine, great]  [japanes, cuisin, great]
```

## 将文本转换为数字表示

最后但同样重要的是，计算机算法包括 AI/ML 模型并不理解人类语言，而是数字，因此我们需要将我们的词向量映射到数字表示中，通常称为嵌入向量，或简称为嵌入。下面的示例将“tokens”列中的分词文本转换为 TF-IDF 向量化方法（这是经典 NLP 的黄金时代中最受欢迎的方法之一）来将文本转换为数字表示。

```py
from sklearn.feature_extraction.text import TfidfVectorizer
df['text'] = df['tokens'].apply(lambda x: ' '.join(x))
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(df['text'])
print(X.toarray())
```

输出：

```py
[[0\.         0.70710678 0\.         0\.         0\.         0\.       0.70710678]
[0.70710678 0\.         0\.         0.70710678 0\.         0\.        0\.        ]
[0\.         0\.         0.57735027 0\.         0.57735027 0.57735027        0\.        ]]
```

就这样！尽管对我们来说可能显得难以理解，这种我们预处理文本的数字表示就是智能系统，包括 NLP 模型，能够理解并且在挑战性语言任务如文本情感分类、总结或甚至翻译成另一种语言时表现得异常出色的内容。

下一步是将这些数字表示输入到我们的 NLP 模型中，让它施展魔法。

[](https://www.linkedin.com/in/ivanpc/)****[伊万·帕洛马雷斯·卡拉斯科萨](https://www.linkedin.com/in/ivanpc/)**** 是一位在人工智能、机器学习、深度学习和大型语言模型方面的领导者、作家、演讲者和顾问。他培训和指导他人在实际应用中利用人工智能。

### 更多相关内容

+   [学习数据清理和预处理以进行数据科学，这本免费电子书](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [掌握数据清理和预处理技术的7个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [利用ChatGPT进行自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [用Python自动化的5个任务](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

+   [HuggingGPT：解决复杂AI任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)
