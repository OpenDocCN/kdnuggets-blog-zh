# 如何开始你的NLP之旅

> 原文：[https://www.kdnuggets.com/2021/03/begin-nlp-journey.html](https://www.kdnuggets.com/2021/03/begin-nlp-journey.html)

[评论](#comments)

**由 [Diego Lopez Yse](https://www.linkedin.com/in/lopezyse/)，数据科学家**

![](../Images/f528880f08e3fdfecac0452c85796b63.png)

图片由[PNG Design](https://unsplash.com/@_pngdesign?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

**自然语言处理（NLP）**是人工智能领域中最令人兴奋的领域之一。它允许机器以多种方式处理和理解人类语言，并引发了我们与系统和技术互动方式的革命。

在[上一篇文章](https://towardsdatascience.com/your-guide-to-natural-language-processing-nlp-48ea2511f6e1)中，我讲述了NLP、它的实际应用和一些核心概念。现在我想向你展示NLP有多真实，任何人都可以开始学习它。怎么做呢？让我们从一个简单的文本开始，使用一些NLP技术进行一些探索性数据分析（EDA）。这样，我们可以使用简单而强大的工具理解数据，然后再忙于模型或更复杂的任务。

### 定义你的文本

史蒂芬·霍金曾说：

> “人工智能（AI）可能是人类发生的最好或最糟糕的事情”

我完全同意他的观点，时间会证明实际发生的事情。然而，这句话是测试一些NLP技术的合适句子。为此，让我们首先将这句话保存为一个名为“text”的变量：

```py
text = “Artificial Intelligence (AI) is likely to be either the best or the worst thing to happen to humanity.”
```

使用**langdetect**库，我们可以检查其语言，并找出其用该语言写的概率：

```py
import langdetect
from langdetect import detect_langs
print(detect_langs(text))
```

![](../Images/ced664c8874856d6715a505f8812cc2f.png)

我们可以以超过99.9%的确定性声明，这句话是用英语写的。你还应该考虑使用[**拼写检查功能**](https://textblob.readthedocs.io/en/dev/quickstart.html)来纠正任何语法错误。

那么字符的数量呢？

```py
len(text)
```

![](../Images/662ca47f2ad17ff6af58344750b3298c.png)

我们有102个字符，包括空格。不同字符的数量是多少？

```py
len(set(text))
```

![](../Images/a825379189ff772e1698d72c6a8aef08.png)

让我们来看看这些：

```py
print(sorted(set(text)))
```

![](../Images/edb7edf6d11613f5ec1f7d159abe9b03.png)

这里有一些有趣的地方。我们不仅在计算像‘(’和‘.’这样的非字母数字字符，还包括空格，更重要的是，大小写字母被视为不同的字符。

### 词汇化

词汇化是将连续文本分割成句子和单词的过程。实质上，这是将文本切割成称为*tokens*的片段的任务。我们使用**NLTK**库来执行此任务：

```py
import nltk
from nltk.tokenize import word_tokenize
tokenized_word = word_tokenize(text)
print(tokenized_word)
```

![](../Images/1ecfc8c6d66f3cd3950c478327ab10cb.png)

我们可以看到，词汇化产生了一个词汇列表：

```py
type(tokenized_word)
```

![](../Images/61d9fdc5bca2c694e541f2b9ebc7fdae.png)

这意味着我们可以调用其中的元素。

```py
tokenized_word[2:9]
```

![](../Images/eddf2e0504f2b5d917589a0d1d4c76b4.png)

我们有多少个token？

```py
len(tokenized_word)
```

![](../Images/3a1dde1027f391f13211aa5d5445b546.png)

那么独特的tokens呢？

```py
len(set(tokenized_word))
```

![](../Images/77e2dc39e50df31761e5a44507199202.png)

现在我们可以计算一个与文本的*词汇丰富性*相关的指标：

```py
len(set(tokenized_word)) / len(tokenized_word)
```

![](../Images/979fe66bff745f3af338d3e07b14ee26.png)

这表明不同词汇的数量占总词汇数量的85.7%。

### 小写和标点

现在，让我们将文本转换为小写，以标准化字符，并为未来的停用词移除做准备：

```py
tk_low = [w.lower() for w in tokenized_word]
print(tk_low)
```

![](../Images/f8673a933d9289a62128043792ce60da.png)

接下来，我们去除非字母数字字符：

```py
nltk.download(“punkt”)
tk_low_np = remove_punct(tk_low)
print(tk_low_np)
```

![](../Images/68e85988a65ac9f17530ba8e14aac90a.png)

让我们可视化词汇的累积频率分布：

```py
from nltk.probability import FreqDist
fdist = FreqDist(tk_low_np)
fdist.plot(title = ‘Word frequency distribution’, cumulative = True)
```

![](../Images/62ebff0e15f54dfd2196300045eac5a1.png)

我们可以看到，“to”和“the”这些词出现得最频繁，但它们实际上并未向文本添加信息。它们被称为*停用词*。

### 停用词移除

这一过程包括去除常见的语言文章、代词和介词，如英语中的“and”、“the”或“to”。在此过程中，一些非常常见的词被认为对NLP目标贡献甚微或没有价值，因此从待处理的文本中被过滤和排除，从而去除那些没有提供有用信息的广泛和频繁出现的术语。

首先，我们需要创建一个停用词列表，并将其从我们的词汇列表中过滤掉：

```py
from nltk.corpus import stopwords
stop_words = set(stopwords.words(“english”))
print(stop_words)
```

![](../Images/8c928a8d5842671ad506ac7ae8ee63a5.png)

我们将使用NLTK库中的这个列表，但请记住，你可以创建自己的停用词集。让我们在列表中查找“the”这个词：

```py
print(‘the’ in stop_words)
```

![](../Images/9382528bc581caf9b3b88402b63e1d57.png)

现在，让我们从这些停用词中清理文本：

```py
filtered_text = []
for w in tk_low_np:
   if w not in stop_words:
      filtered_text.append(w)
print(filtered_text)
```

![](../Images/c9b829e69f0f7c887cf309dc2573fe83.png)

我们可以看到，词汇“is”、“to”、“be”、“the”和“or”已从文本中删除。让我们更新词汇的累积频率分布：

![](../Images/a46199b961de03dcd9e2161bae951c3f.png)

停用词移除应以非常谨慎的方式进行，因为它可能在执行其他任务时带来巨大问题，例如情感分析。如果一个词的上下文受到影响（例如，去除“not”这样的词，这是一种否定成分），这种操作可能会改变段落的意义。

除了这个例子，可能还需要处理其他类型的特征，例如 [**缩写**（如“doesn’t”，应展开](https://www.analyticsvidhya.com/blog/2020/04/beginners-guide-exploratory-data-analysis-text-data/)）或 [**重音和变音符号**（如“cliché”或“naïve”，应通过去除变音符号来规范化）](https://nlp.stanford.edu/IR-book/html/htmledition/accents-and-diacritics-1.html)。

### 正则表达式

[正则表达式](https://docs.python.org/3/howto/regex.html)（简称 REs 或 RegExes）是嵌入在 Python 中的一个微小且高度专业化的编程语言，通过**re**模块提供。使用它们，你可以指定要匹配的字符串集的规则。你可以提出诸如“这个字符串是否符合模式？”或“这个字符串中是否有符合模式的匹配项？”等问题。

例如，让我们搜索以“st”结尾的单词：

```py
import re
[w for w in filtered_text if re.search(‘st$’, w)]
```

![](../Images/a0f52b5a704a95407fcc1eb4fc132e55.png)

或计算第一个单词（“artificial”）中的元音字母数量：

```py
len(re.findall(r’[aeiou]’, filtered_text[0]))
```

![](../Images/87cb03271ef06cde7b57f7147edb2a9f.png)

你甚至可以根据条件修改文本。例如，将第二个单词（“intelligence”）中的字母“ce”替换为字母“t”：

```py
x = re.sub('ce', 't', filtered_text[1])
print(x)
```

![](../Images/f005edc4ceacb22e5a7e2d7bb584f86a.png)

你可以通过 [这个链接](https://www.w3schools.com/python/python_regex.asp) 找到更多正则表达式的例子。

### 结论

我们仅仅触及了所有可能和更复杂的 NLP 技术的表面。你可能不仅想分析结构化文本，还要处理从对话、声明甚至推文中生成的所有数据，这些都是非结构化数据的例子。**非结构化数据**无法 neatly 放入传统的行列结构的关系数据库中，代表了现实世界中绝大多数的数据。它是混乱的且难以操作。

NLP因数据访问的巨大改进和计算能力的提升而迅速发展，这使我们能够在医疗、媒体、金融和人力资源等领域取得有意义的成果。

> 我的建议是：**学习 NLP。** 尝试不同的数据源和技术。实验、失败并提升自己。这一学科将影响所有可能的行业，我们很可能在未来几年达到一种令人惊叹的进展水平。

**对这些话题感兴趣？关注我在**[**Linkedin**](https://www.linkedin.com/in/lopezyse/)**或**[**Twitter**](https://twitter.com/lopezyse)

**个人简介：[Diego Lopez Yse](https://www.linkedin.com/in/lopezyse/)** 是一位经验丰富的专业人士，拥有在不同领域（资本市场、生物技术、软件、咨询、政府、农业）获得的国际背景。始终是团队成员。擅长商业管理、分析、金融、风险、项目管理和商业运营。拥有数据科学和公司金融硕士学位。

[原文](https://towardsdatascience.com/how-to-begin-your-nlp-journey-5dc6734dfb43)。经许可转载。

**相关内容：**

+   [强化版探索性数据分析](/2020/07/exploratory-data-analysis-steroids.html)

+   [开始使用5个必备的自然语言处理库](/2021/02/getting-started-5-essential-nlp-libraries.html)

+   [自然语言处理管道解析](/2021/03/natural-language-processing-pipelines-explained.html)

### 更多相关内容

+   [通过这5个免费课程启动你的NLP之旅](https://www.kdnuggets.com/kickstart-your-nlp-journey-with-these-5-free-courses)

+   [绘制通向SAS认证的路线图](https://www.kdnuggets.com/2022/11/sas-map-journey-towards-sas-certification.html)

+   [在你的数据科学之旅中取得突破性进展](https://www.kdnuggets.com/2023/02/make-quantum-leaps-data-science-journey.html)

+   [通过Uplimit的Metaflow加速你的机器学习之旅](https://www.kdnuggets.com/2023/10/uplimit-accelerate-your-machine-learning-journey-metaflow-mastery-course)

+   [超级充能你的AI之旅！加入Uplimit的免费AI构建课程](https://www.kdnuggets.com/2024/01/uplimit-supercharge-your-ai-journey-openai-course)

+   [机器学习模型解释性如何加速AI采纳之旅](https://www.kdnuggets.com/2022/07/ml-model-explainability-accelerates-ai-adoption-journey-financial-services.html)
