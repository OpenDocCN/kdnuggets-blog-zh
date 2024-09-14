# 顶级 6 个 Python NLP 库的比较

> 原文：[https://www.kdnuggets.com/2018/07/comparison-top-6-python-nlp-libraries.html](https://www.kdnuggets.com/2018/07/comparison-top-6-python-nlp-libraries.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [comments](#comments)

**由 [ActiveWizards](https://activewizards.com/) 提供**

![Image](../Images/26cfab0a789d1f6f563bb187fbdfab0c.png)

自然语言处理（NLP）在今天变得非常流行，这在深度学习的发展背景下尤为显著。NLP 是一个人工智能领域，旨在从文本中理解和提取重要信息，并基于文本数据进一步训练。主要任务包括语音识别和生成、文本分析、情感分析、机器翻译等。

在过去几十年中，只有具有适当语言学教育的专家才能从事自然语言处理工作。除了数学和机器学习，他们还应该熟悉一些关键语言学概念。现在，我们可以直接使用已经编写好的 NLP 库。它们的主要目的是简化文本预处理。我们可以专注于构建机器学习模型和调整超参数。

有许多工具和库被设计用于解决 NLP 问题。今天，我们希望基于我们的经验概述并比较最受欢迎和最有用的自然语言处理库。你应该理解，我们查看的所有库的任务仅部分重叠。因此，有时直接比较它们是困难的。我们将绕过一些功能，仅比较那些可以直接比较的库。

### 总体概述

[NLTK](http://www.nltk.org/)（自然语言工具包）用于如分词、词形还原、词干提取、解析、词性标注等任务。这个库几乎涵盖了所有 NLP 任务的工具。

[Spacy](https://spacy.io/) 是 NLTK 的主要竞争对手。这两个库可以用于相同的任务。

[Scikit-learn](http://scikit-learn.org/stable/) 提供了一个大型机器学习库。这里也提供了文本预处理的工具。

[Gensim](https://radimrehurek.com/gensim/) 是用于主题和向量空间建模、文档相似性的包。

[Pattern](https://www.clips.uantwerpen.be/pattern) 库的总体任务是作为网页挖掘模块。因此，它只将 NLP 作为附加任务支持。

[Polyglot](https://pypi.python.org/pypi/polyglot) 是另一个用于 NLP 的 Python 包。它虽然不太流行，但也可以用于广泛的 NLP 任务。

为了使比较更生动，我们准备了一个表格，显示库的优缺点。

[![](../Images/14c96a87552517337ba8638d153f17dc.png)](https://activewizards.com/content/blog/Comparison_of_Python_NLP_libraries/nlp-librares-python-prs-and-cons01.png)

更新日期：2018 年 7 月

### 结论

在这篇文章中，我们比较了几种流行的自然语言处理库的功能。尽管大多数库提供了重叠任务的工具，但有些库对特定问题采用了独特的方法。显然，当前最受欢迎的NLP包是NLTK和Spacy。它们是NLP领域的主要竞争者。在我们看来，它们之间的区别在于解决问题的方法的总体哲学。

NLTK 更加学术。你可以用它尝试不同的方法和算法，进行组合等。而 Spacy 则提供了针对每个问题的现成解决方案。你不必考虑哪个方法更好：Spacy 的作者已经解决了这个问题。此外，Spacy 的速度非常快（比 NLTK 快几倍）。一个缺点是 Spacy 支持的语言数量有限。不过，支持的语言数量在不断增加。因此，我们认为 Spacy 在大多数情况下是最佳选择，但如果你想尝试一些特别的东西，可以使用 NLTK。

尽管这两个库很受欢迎，但还有许多不同的选项，选择哪个NLP包取决于你需要解决的具体问题。因此，如果你知道其他有用的NLP库，请在评论区告知我们的读者。

**[ActiveWizards](https://activewizards.com/)** 是一个专注于数据项目（大数据、数据科学、机器学习、数据可视化）的数据科学家和工程师团队。核心专业领域包括数据科学（研究、机器学习算法、可视化和工程）、数据可视化（d3.js、Tableau等）、大数据工程（Hadoop、Spark、Kafka、Cassandra、HBase、MongoDB等），以及数据密集型Web应用开发（RESTful API、Flask、Django、Meteor）。

[原文](https://activewizards.com/blog/comparison-of-python-nlp-libraries/)。经授权转载。

**相关：**

+   [2017 年数据科学领域的15个Python库](/2017/06/top-15-python-libraries-data-science.html)

+   [2018年数据科学领域的15个Scala库](/2018/02/top-15-scala-libraries-data-science-2018.html)

+   [金融领域数据科学的7大应用案例](/2018/05/top-7-data-science-use-cases-finance.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

### 更多相关内容

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让 Python 成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
