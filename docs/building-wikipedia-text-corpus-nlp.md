# 构建一个用于自然语言处理的维基百科文本语料库

> 原文：[`www.kdnuggets.com/2017/11/building-wikipedia-text-corpus-nlp.html`](https://www.kdnuggets.com/2017/11/building-wikipedia-text-corpus-nlp.html)

对于自然语言处理 (NLP) 任务，所需的第一件事之一是一个语料库。在语言学和 NLP 中，**语料库**（字面意思是拉丁语中的“身体”）指的是一组文本。这些集合可以由单一语言的文本组成，也可以跨越多种语言——多语言语料库（语料库的复数形式）可能有许多用途。语料库还可以由主题文本（历史的、圣经的等）组成。语料库通常仅用于统计语言分析和假设检验。

好消息是，互联网充满了文本，而且在许多情况下，这些文本已经被收集和组织得很好，即使它需要一些调整以适应更可用、更精确定义的格式。特别是维基百科，是一个组织良好的丰富文本数据源。它也是一个庞大的知识集合，开放的头脑可以为这样的文本集合构思各种用途。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT 需求

* * *

我们将在这里从一组英文维基百科文章中构建一个语料库，这些文章可以在网上自由和方便地获取。

![维基百科](img/8f2962b58d464bf97d168a4617f22220.png)

**安装 gensim**

为了轻松构建一个没有维基百科文章标记的文本语料库，我们将使用 [gensim](https://radimrehurek.com/gensim/index.html)，这是一个用于 Python 的 [主题建模](https://en.wikipedia.org/wiki/Topic_model) 库。具体来说，`gensim.corpora.wikicorpus.WikiCorpus` 类正是为这个任务而设计的：

> 从维基百科（或其他基于 MediaWiki 的）数据库转储中构建一个语料库。

为了正确完成以下步骤，你需要 [安装 gensim](https://radimrehurek.com/gensim/install.html)。这是一个相对简单的过程；使用 pip：

```py
$ pip install gensim
```

继续...

**下载维基百科转储文件**

显然，此过程还需要一个维基百科转储文件。最新的文件可以在 [这里](https://dumps.wikimedia.org/enwiki/latest/) 找到。

提个警告：最新的英文维基百科数据库转储文件大小约为 14 GB，因此下载、存储和处理该文件并非易事。

我获得并用于此任务的文件是`enwiki-latest-pages-articles.xml.bz2`。下载它或其他类似文件以用于接下来的步骤。

**创建语料库**

我写了一个简单的 Python 脚本（灵感来自于[这里](https://github.com/panyang/Wikipedia_Word2vec/blob/master/v1/process_wiki.py)），通过去除文章中的所有维基百科标记来构建语料库，使用 gensim 库。你可以在[这里](https://radimrehurek.com/gensim/install.html)阅读关于 WikiCorpus 类的更多信息（如上所述）。

代码非常简单明了：使用`WikiCorpus`类的`get_texts()`方法逐篇打开和读取维基百科的转储文件，所有这些最终都写入一个文本文件中。维基百科转储文件和最终的语料库文件都必须在命令行中指定。

```py
$ python make_wiki_corpus enwiki-latest-pages-articles.xml.bz2 wiki_en.txt
```

```py
Processed 10000 articles
Processed 20000 articles
Processed 30000 articles
Processed 40000 articles
Processed 50000 articles
Processed 60000 articles
Processed 70000 articles
Processed 80000 articles
Processed 90000 articles
Processed 100000 articles
...
```

几小时后，上述代码生成了一个名为`wiki_en.txt`的语料库文件。

**检查语料库**

第二个脚本检查我们刚刚构建的语料库文本文件。

现在，请记住，这个大型的维基百科转储文件最终会生成一个非常大的语料库文件。由于其巨大的尺寸，你可能会在一次性将整个文件读入内存时遇到困难。

这个脚本开始时会从文本文件中读取 50 行——相当于 50 篇完整的文章——并将它们输出到终端，然后你可以按一个键输出另外 50 行，或者输入‘STOP’以退出。如果你*确实*停止，脚本将继续将整个文件加载到内存中。这可能会对你造成问题。然而，你可以通过批量检查文本，以满足你对运行第一个脚本后有所收获的好奇心。

如果你计划处理这样一个大的文本文件，可能需要一些变通方法来应对其相对于你计算机内存的巨大尺寸。

必须在命令行中指定语料库文件以执行。

```py
$ python check_wiki_corpus.py wiki_en.txt
```

```py
...
best loved patriotic songs harperresource external links mp and realaudio 
recordings available at the united states library of congress words sheet 
music midi file at the cyber hymnal america the beautiful park in colorado 
springs named for katharine lee bates words archival collection of america 
the beautiful lantern slides from the another free sheet music 

>>> Type 'STOP' to quit or hit Enter key for more <<<
```

就这样。一些简单的代码实现了 gensim 使之变得简单的任务。现在你拥有了一个充足的语料库，自然语言处理的世界就是你的舞台。是时候做些有趣的事情了。

**相关**：

+   处理文本数据科学任务的框架

+   自然语言处理关键术语解释

+   5 个免费资源帮助你开始深度学习自然语言处理

### 更多相关话题

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [自然语言处理关键术语解释](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)

+   [自然语言处理的温和入门](https://www.kdnuggets.com/2022/06/gentle-introduction-natural-language-processing.html)
