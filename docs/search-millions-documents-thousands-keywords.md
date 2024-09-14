# 迅速搜索数百万文档中的数千个关键词

> 原文：[`www.kdnuggets.com/2017/09/search-millions-documents-thousands-keywords.html`](https://www.kdnuggets.com/2017/09/search-millions-documents-thousands-keywords.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png)评论

**由 [Vikash Singh](https://medium.com/@vi3k6i5), Belong.co**。

![](img/5241db5938fde65ba36e10d1a424d7e7.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

假设你有一个文档，你想知道它是否提到 python（一个你关心的术语）

```py

-------------------------------------------------------------------------
Document: I am a python developer.
Term: python
-------------------------------------------------------------------------

```

你想检查文档中是否包含 python 这个词。因此你打开文档，按 ctrl+f 并搜索 'python'。然后你找到了它 :)

现在假设你有 100 个这样的术语：[python, java, github, medium, 等等]

你将用一段简单的 python 代码打开文档。循环检查每个术语，看看术语是否存在。

```py

-------------------------------------------------------------------------
```

```py
Open (document) 
for each term in terms: 
    if term is present in document: print(term)
```

```py

-------------------------------------------------------------------------
```

现在假设你有 100 个文档。你可以在循环中打开每个文档。每个文档中你搜索每个术语。

```py

-------------------------------------------------------------------------

```

```py
for document in documents: 
    Open (document)
    for each term in terms:
        if term is present in document: print(term)
```

```py

-------------------------------------------------------------------------
```

现在假设 **java** 应该与 **Java** 匹配，但不包括 **javascript**。

更好的是，**java** 应该与 **j2ee** 和 **Java** 都匹配，但不包括 **java script**。

*(j2ee 和 java 是同义词，你注意到 java script 中的空格了吗？)*

现在变得有趣了。你怎么做呢？

我们去年遇到了这个问题 @[Belong.co](https://medium.com/@BelongCo)。我们注意到人们用多种方式谈论相同的术语。*Big apple* 可以指 *big apple* 或 *New York*。幸运的是，我们有一些上下文。当我们的文档提到 *Python* 时，它 99.99% 的情况下指的是编程语言，而不是动物。

但这并没有简化我们的问题。**Java** 和 **j2ee** 对我们来说是一样的，但不包括 **java script**。那么如何从数百万个文档中提取这些信息呢？

正如你所想的，我们编写了一段 *正则表达式* 基础的代码。对于 100 万个文档和 2K 个关键词，这段代码运行了 24 小时。生活变得美好了 :)

但很快我们扩展到几百万个文档和 10K+ 个关键词。于是相同的代码现在需要 10 多天才能运行。因此我们开始寻找更好的方法。

我在办公室问了问，[Vinay](https://www.linkedin.com/in/vinay-pande-54810813/) 建议我查看基于 Trie 字典的方法。[Suresh](https://www.linkedin.com/in/suresh-lakshmanan/) 建议使用[Aho Corasick 算法](https://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_algorithm)。在[Stack overflow](https://stackoverflow.com/questions/44178449/regex-replace-is-taking-time-for-millions-of-documents-how-to-make-it-faster)上得到了类似的建议。

事实证明，Aho Corasick 算法可以在对文档的一次扫描中同时搜索所有关键词。这真是太棒了。

[`youtu.be/NQ8GeVCEgBs`](https://youtu.be/NQ8GeVCEgBs)

在示例输入上的 flashtext 演示。

我编写了基于 Trie 数据结构的自定义实现以适应我们的用例。它运行得相当不错。使用这种算法的关键词提取过程需要 15 分钟。比起基于*正则表达式*的方法减少了 10 多天的时间。

```py

-------------------------------------------------------------------------
```

```py
Input: I love j2ee. Keyword: j2ee=>Java 
# *Which is basically saying j2ee means Java* Output: ['Java']
```

```py

-------------------------------------------------------------------------
```

现在关键词提取效果很好。因此，我还添加了在文档中用同义词替换关键词的功能。

```py
Say you want to replace ‘New Delhi’ with ‘NCR region’ in a document.
Input: I live in New Delhi.
Output: I live in NCR region.
```

我们能够在多个项目中利用这个库。这也是我们决定将其**开源**的原因。这里是代码的链接 :) [`github.com/vi3k6i5/flashtext`](https://github.com/vi3k6i5/flashtext)

使用起来非常简单：*[Python 代码即将发布]*

```py

-------------------------------------------------------------------------
```

```py
$ pip install flashtext
```

```py
>>> from flashtext.keyword import KeywordProcessor
>>> keyword_processor = KeywordProcessor()
>>> keyword_processor.add_keyword('j2ee', 'Java')
>>> keyword_processor.add_keyword('Python')
>>> keyword_processor.extract_keywords('I work on python and j2ee')
# output: ['Python', 'Java']
```

关键词替换：

```py
>>> keyword_processor.add_keyword('New Delhi', 'NCR region')
>>> keyword_processor.replace_keywords('I live in New Delhi.')
# output: 'I live in NCR region.'
```

```py

-------------------------------------------------------------------------
```

这非常有用，因为它有助于术语扩展。比如你想将***RC car***替换为***Remote Control car***在产品目录中。或者你想将***Electrocardiogram***提取为***ECG***。这两者都很容易做到。

* * *

如果你认识从事实体识别或 NER 或 NLP 或 Word2vec 的人，请将这篇博客分享给他们。这一库对我们在这些领域非常有用。我相信它对其他人也会有帮助。

干杯 :)

[原文](https://medium.com/@vi3k6i5/search-millions-of-documents-for-thousands-of-keywords-in-a-flash-b39e5d1e126a)。转载经许可。

**个人简介：[Vikash Singh](https://medium.com/@vi3k6i5)** 是 belong.co 的数据科学家，处理大量文本以及基于词嵌入的多个项目。

**相关：**

+   Python 超越 R，成为数据科学和机器学习平台的领导者

+   5 个免费的自然语言处理深度学习入门资源

+   文本挖掘 101：从简历中挖掘信息

### 更多相关主题

+   [如何在几秒钟内处理具有数百万行的数据框](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

+   [使用网格搜索和随机搜索进行超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过 Uplimit 的机器学习搜索课程提升您的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第二部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [使用 BERT 对长文本进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [GPT4All 是您文档的本地 ChatGPT，且是免费的！](https://www.kdnuggets.com/2023/06/gpt4all-local-chatgpt-documents-free.html)
