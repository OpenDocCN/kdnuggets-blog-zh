# 2021年数据工程师最需求的技能

> 原文：[https://www.kdnuggets.com/2021/05/most-demand-skills-data-engineers-2021.html](https://www.kdnuggets.com/2021/05/most-demand-skills-data-engineers-2021.html)

[评论](#comments)

![数据工程](../Images/efe8223e7c92f882a8d32fd9e5e37cb1.png)

在撰写了[2021年数据科学家最需求技能](https://www.kdnuggets.com/2021/04/most-demand-skills-data-scientists.html)之后，我想对数据工程师进行相同的分析，因为该领域的需求正快速增长。根据[Interview Query的数据显示科学面试报告](https://www.interviewquery.com/blog-data-science-interview-report)，2019年到2020年之间，数据科学面试的数量仅增长了10%，而在同一时期内，**数据工程面试的数量增长了40%**！

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的IT需求

* * *

再次说明，我想预先声明，这受到Jeff Hale在2018/2019年所写文章的启发。我撰写这篇文章只是因为我想获取更符合当下的技能需求分析，并且我分享这篇文章是因为我假设也有其他人希望看到2021年数据工程师最需求技能的更新版本。

从这项分析中获取你想要的内容——显然，通过抓取招聘广告获得的见解并不能完美地与实际最受需求的数据科学技能相关联。然而，我认为这可以很好地指示出你应该更多关注哪些通用技能，同时避免哪些技能。

话虽如此，希望你喜欢这份分析，让我们开始吧！

### 方法论

在这项分析中，我从Indeed、Monster和SimplyHired上抓取并累计了超过17,000个招聘广告。我没有抓取LinkedIn，因为我在尝试抓取时遇到了验证码问题。

我接着检查了每个我搜索的术语在招聘广告中出现的频率。我搜索的术语列表如下：

+   Python, SQL, R, Java, Git, C, MATLAB, Excel, C++, JavaScript, C#, Julia, Scala, SAS

+   Scikit-learn, Pandas, NumPy, SciPy

+   Matplotlib, Looker, Tableau

+   TensorFlow, PyTorch, Keras

+   Spark, Hadoop, AWS, GCP, Hive, Azure, Google Cloud, MongoDB, BigQuery

+   Docker, Kubernetes, Airflow

+   NoSQL, MySQL, PostgreSQL

+   Caffe, Alteryx, Perl, Cassandra, Linux

在获取每个来源的数据后，我将它们相加，然后除以数据工程师职位总数以获得百分比。例如，Python 的值为 0.76 表示 76% 的职位中要求掌握 Python。

最后，我将数据工程师分析的结果与数据科学家分析的结果进行了比较，以查看这两个角色之间的差异。

### 结果

**数据工程师的前 25 项技能**

以下是 2021 年最受欢迎的数据工程师技能前 25 名，从高到低排名：

![](../Images/7628d27fb4782b0ca464d3b28c507ac4.png)

**数据工程师的顶级编程语言**

为了获得更详细的视图，下图显示了数据工程师的顶级编程语言：

![](../Images/3ce34e882750e9897a74988f85965a4f.png)

数据工程师与数据科学家在顶级编程语言上的两个主要区别：首先，要求 SQL 的职位百分比对于数据工程师来说要高得多——这是合理的，因为 SQL 是构建数据管道、表视图等所必需的。其次，要求 R 的职位百分比对于数据工程师则要小得多。因此，如果你想同时保持数据工程师和数据科学家的职业选择，那么我建议学习 Python 而不是 R。

**数据工程师和数据科学家之间正差异最大的前 10 项技能**

下图显示了数据工程师和数据科学家之间在技能百分比上的最大差异：

![](../Images/52a8d86f2cd706cfb6ac0840a34474bc.png)

不出所料，云计算技能和 Apache 的大数据产品如 Spark、Hive 和 Hadoop 对数据工程师而言比数据科学家更为重要，这很合理，因为数据工程师的工作重点是构建和维护组织的数据基础设施。Airflow 对数据工程师来说也更为重要，因为它正逐渐成为工作流调度的首选技术。

**数据工程师和数据科学家之间负差异最大的前 10 项技能**

![](../Images/c25733df518e6b4593b6f84ec090c26f.png)

同样，上图也不令人惊讶，因为这些技能大多集中在数据建模和数据分析上，这更符合数据科学家的领域而非数据工程师。

### 总体

总体而言，所有分析技能的差异在下图中提供：

![](../Images/79b3f594508ba1a17430584577bde3fd.png)

希望你发现这项分析有用。我不会仅凭这一资源完全决定学习某项技能而非另一项技能，但正如我之前所说的，我认为这能很好地展示出哪些技能在重要性上有所上升或下降。

**相关：**

+   [为什么你应该考虑成为数据工程师而不是数据科学家](https://www.kdnuggets.com/2021/04/consider-being-data-engineer-instead-data-scientist.html)

+   [成为数据工程师所需的9项技能](https://www.kdnuggets.com/2021/03/9-skills-become-data-engineer.html)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

### 更多相关主题

+   [2022年最受需求的人工智能技能](https://www.kdnuggets.com/2022/08/indemand-artificial-intelligence-skills-learn-2022.html)

+   [用热门数据技能快速推进你的下一步](https://www.kdnuggets.com/2023/01/datacamp-fast-track-next-move-indemand-data-skills.html)

+   [KDnuggets 新闻 3月30日：最受欢迎的编程入门…](https://www.kdnuggets.com/2022/n13.html)

+   [数据工程师和数据科学家都需要的高保真合成数据](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [数据科学家和数据工程师如何协同工作？](https://www.kdnuggets.com/2022/08/data-scientists-data-engineers-work-together.html)
