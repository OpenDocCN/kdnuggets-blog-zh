# 建立数据科学作品集：机器学习项目第 1 部分

> 原文：[https://www.kdnuggets.com/2016/07/building-data-science-portfolio-machine-learning-project-part-1.html/2](https://www.kdnuggets.com/2016/07/building-data-science-portfolio-machine-learning-project-part-1.html/2)

### 选择角度

我们可以在 Fannie Mae 数据集上采取几种方向。我们可以：

+   尝试预测房屋止赎后的销售价格。

+   预测借款人的还款历史。

+   在贷款获取时确定每笔贷款的评分。

重要的是要坚持一个角度。试图同时关注太多事物会使项目难以有效实施。选择一个具有足够细微差别的角度也很重要。以下是一些没有太多细微差别的角度示例：

+   确定哪些银行向 Fannie Mae 出售的贷款被止赎的次数最多。

+   确定借款人信用评分的趋势。

+   探索哪些类型的房屋最常被止赎。

+   探索贷款金额与止赎销售价格之间的关系

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

上述所有角度都很有趣，如果我们专注于讲故事的话会很棒，但对于一个运营项目来说并不太合适。

使用 Fannie Mae 数据集，我们将尝试通过仅使用在贷款获取时可用的信息来预测贷款是否会在未来被止赎。实际上，我们将为任何抵押贷款创建一个“评分”，以告诉我们 Fannie Mae 是否应该购买它。这将为我们提供一个很好的基础，并成为一个很棒的作品集项目。

### 理解数据

我们快速查看一下原始数据文件。以下是 2012 年第 `1` 季度获取数据的前几行：

```py
100000853384|R|OTHER|4.625|280000|360|02/2012|04/2012|31|31|1|23|801|N|C|SF|1|I|CA|945||FRM|
100003735682|R|SUNTRUST MORTGAGE INC.|3.99|466000|360|01/2012|03/2012|80|80|2|30|794|N|P|SF|1|P|MD|208||FRM|788
100006367485|C|PHH MORTGAGE CORPORATION|4|229000|360|02/2012|04/2012|67|67|2|36|802|N|R|SF|1|P|CA|959||FRM|794

```

以下是 2012 年第 `1` 季度性能数据的前几行：

```py
100000853384|03/01/2012|OTHER|4.625||0|360|359|03/2042|41860|0|N||||||||||||||||
100000853384|04/01/2012||4.625||1|359|358|03/2042|41860|0|N||||||||||||||||
100000853384|05/01/2012||4.625||2|358|357|03/2042|41860|0|N||||||||||||||||

```

在开始编写代码之前，花些时间真正理解数据是非常有用的。这在运营项目中尤为关键——因为我们不能互动地探索数据，所以如果我们事先不发现某些细微之处，它们可能更难被察觉。在这种情况下，第一步是阅读 Fannie Mae 网站上的材料：

+   [概述](http://www.fanniemae.com/portal/funding-the-market/data/loan-performance-data.html)

+   [有用术语词汇表](https://loanperformancedata.fanniemae.com/lppub-docs/lppub_glossary.pdf)

+   [常见问题解答](https://loanperformancedata.fanniemae.com/lppub-docs/lppub_faq.pdf)

+   [获取与绩效文件中的列](https://loanperformancedata.fanniemae.com/lppub-docs/lppub_file_layout.pdf)

+   [样本获取数据文件](https://loanperformancedata.fanniemae.com/lppub-docs/acquisition-sample-file.txt)

+   [样本绩效数据文件](https://loanperformancedata.fanniemae.com/lppub-docs/performance-sample-file.txt)

阅读这些文件后，我们知道一些关键事实，这些事实将帮助我们：

+   每个季度都有一个获取文件和一个绩效文件，从`2000`年开始到现在。数据存在一年的滞后，所以截至本文编写时，最新的数据来自`2015`年。

+   文件采用文本格式，以管道符号 (`|`) 作为分隔符。

+   文件没有标题，但我们有每列内容的列表。

+   所有文件加在一起包含`22`百万笔贷款的数据。

+   因为绩效文件包含以前年份获取的贷款信息，所以早期年份获取的贷款将有更多的绩效数据（即`2014`年获取的贷款不会有太多绩效历史）。

这些小的信息片段将为我们节省大量时间，因为我们在规划项目结构和处理数据时。

### 结构化项目

在我们开始下载和探索数据之前，重要的是要考虑我们将如何结构化项目。在构建端到端项目时，我们的主要目标是：

+   创建有效的解决方案

+   拥有一个快速运行且资源消耗最小的解决方案

+   使其他人能够轻松扩展我们的工作

+   让其他人更容易理解我们的代码

+   尽可能少写代码

为了实现这些目标，我们需要良好地结构化项目。结构良好的项目遵循几个原则：

+   将数据文件与代码文件分开。

+   将原始数据与生成的数据分开。

+   拥有一个`README.md`文件，引导人们安装和使用项目。

+   拥有一个`requirements.txt`文件，其中包含运行项目所需的所有包。

+   拥有一个`settings.py`文件，其中包含在其他文件中使用的任何设置。

    +   例如，如果你在多个 Python 脚本中读取相同的文件，最好让它们都导入`settings`并从集中位置获取文件名。

+   拥有一个`.gitignore`文件，防止大文件或秘密文件被提交。

+   将任务的每一步拆分成可以单独执行的文件。

    +   例如，我们可能有一个文件用于读取数据，一个用于创建特征，另一个用于进行预测。

+   存储中间值。例如，一个脚本可能会输出一个文件，供下一个脚本读取。

    +   这使我们能够在不重新计算所有内容的情况下更改数据处理流程。

我们的文件结构很快将如下所示：

```py

loan-prediction
├── data
├── processed
├── .gitignore
├── README.md
├── requirements.txt
├── settings.py

```

### 创建初始文件

首先，我们需要创建一个`loan-prediction`文件夹。在该文件夹内，我们需要创建一个`data`文件夹和一个`processed`文件夹。第一个文件夹用于存储我们的原始数据，第二个文件夹用于存储任何中间计算值。

接下来，我们将创建一个`.gitignore`文件。`.gitignore`文件将确保某些文件被 git 忽略，不会推送到 GitHub。一个很好的示例是 OSX 在每个文件夹中创建的`.DS_Store`文件。一个好的`.gitignore`文件起点可以在[这里](https://github.com/github/gitignore/blob/master/Python.gitignore)。我们还希望忽略数据文件，因为它们非常大，且 Fannie Mae 的条款阻止我们重新分发，因此我们应在文件末尾添加两行：

```py
data
processed

```

[这里是](https://github.com/dataquestio/loan-prediction/blob/master/.gitignore)该项目的一个`.gitignore`文件示例。

接下来，我们需要创建`README.md`，这将帮助人们理解项目。`.md` 表示文件使用了 Markdown 格式。Markdown 允许你写纯文本，同时可以根据需要添加一些花哨的格式。[这里是](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)一个关于 Markdown 的指南。如果你将名为`README.md`的文件上传到 GitHub，GitHub 会自动处理 Markdown，并展示给查看该项目的任何人。[这里是](https://github.com/dataquestio/loan-prediction)一个示例。

目前，我们只需在`README.md`中写一个简单的描述：

```py
Loan Prediction
-----------------------

Predict whether or not loans acquired by Fannie Mae will go into foreclosure.  Fannie Mae acquires loans from other lenders as a way of inducing them to lend more.  Fannie Mae releases data on the loans it has acquired and their performance afterwards [here](http://www.fanniemae.com/portal/funding-the-market/data/loan-performance-data.html).

```

现在，我们可以创建一个`requirements.txt`文件。这将使其他人更容易安装我们的项目。我们还不完全知道将使用哪些库，但这是一个好的起点：

```py
pandas
matplotlib
scikit-learn
numpy
ipython
scipy

```

上述库是 Python 中用于数据分析任务的最常用库，可以合理地假设我们将使用大多数这些库。[这里是](https://github.com/dataquestio/loan-prediction/blob/master/requirements.txt)该项目的一个要求文件示例。

在创建`requirements.txt`之后，你应该安装这些包。对于这篇文章，我们将使用`Python 3`。如果你还没有安装 Python，你可以考虑使用[Anaconda](https://www.continuum.io/downloads)，这是一个 Python 安装器，它同时安装上述所有包。

最后，我们可以创建一个空的`settings.py`文件，因为我们还没有任何项目设置。

![Vik Paruchuri](../Images/e73e7cfa76d58e480c067a60ded9e910.png)**简历：[Vik Paruchuri](https://www.linkedin.com/in/vikparuchuri)** 是一位驻旧金山的数据科学家和开发者。他是 [Dataquest](https://www.dataquest.io/) 的创始人，你可以在舒适的浏览器中学习数据科学。

*如果你喜欢这个，你可能也会喜欢阅读我们“构建数据科学作品集”系列中的其他文章：*

+   *[数据讲述](https://www.dataquest.io/blog/data-science-portfolio-project/)*。

+   *[如何建立一个数据科学博客](https://www.dataquest.io/blog/how-to-setup-a-data-science-blog/)。*

[原文](https://www.dataquest.io/blog/data-science-portfolio-machine-learning/)。已获得许可转载。

**相关：**

+   [实际学习数据科学的 5 个步骤](/2015/10/5-steps-learn-data-science.html)

+   [Python 中的统计数据分析](/2016/07/statistical-data-analysis-python.html)

+   [用 Python 矿工 Twitter 数据 第 1 部分：收集数据](/2016/06/mining-twitter-data-python-part-1.html)

### 更多相关内容

+   [7 个免费平台，帮助你打造强大的数据科学作品集](https://www.kdnuggets.com/2022/10/7-free-platforms-building-strong-data-science-portfolio.html)

+   [5 个免费平台，帮助你打造强大的数据科学作品集](https://www.kdnuggets.com/5-free-platforms-for-building-a-strong-data-science-portfolio)

+   [7 个机器学习作品集项目，提升简历竞争力](https://www.kdnuggets.com/2022/09/7-machine-learning-portfolio-projects-boost-resume.html)

+   [KDnuggets 新闻，9月21日：7 个机器学习作品集项目…](https://www.kdnuggets.com/2022/n37.html)

+   [如何作为初学者打造强大的数据科学作品集](https://www.kdnuggets.com/2021/10/strong-data-science-portfolio-as-beginner.html)

+   [如何设计你的数据科学作品集](https://www.kdnuggets.com/2022/01/design-data-science-portfolio.html)
