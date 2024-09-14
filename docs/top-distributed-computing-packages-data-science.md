# 数据科学领域受欢迎的分布式计算包排名

> 原文：[https://www.kdnuggets.com/2018/03/top-distributed-computing-packages-data-science.html](https://www.kdnuggets.com/2018/03/top-distributed-computing-packages-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Rachel Allen](https://github.com/raykallen/) 和 [Michael Li](https://github.com/tianhuil/)**

***在[数据孵化器](https://www.thedataincubator.com/)我们致力于提供最新的数据科学课程。通过来自企业和政府合作伙伴的反馈，我们提供行业内最受欢迎的数据科学工具和技术培训。我们希望将更多基于数据的方法融入到我们[企业数据科学培训](https://www.thedataincubator.com/training.html)和我们为希望成为数据科学家的博士及硕士毕业生提供的[免费数据科学奖学金项目](https://www.thedataincubator.com/fellowship.html)的课程开发中。为实现这一目标，我们首先查看并[排名了受欢迎的深度学习库](http://testtdi.com/2017/10/ranking-popular-deep-learning-libraries-for-data-science/)。接下来，我们希望分析数据科学领域分布式计算包的受欢迎程度。以下是结果。***

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

**排名**

以下是基于 Github 和 Stack Overflow 活动以及 Google 搜索结果的前 20 名中的 140 个分布式计算包排名。表格显示了标准化分数，其中值为 1 表示高于平均水平一个标准差（平均 = 0）。例如，*Apache Hadoop* 在 Stack Overflow 活动中高于平均水平 6.6 个标准差，而 *Apache Flink* 接近平均水平。具体方法见下文。

**结果与讨论**

[![排名受欢迎的分布式计算包数据科学](../Images/ed41a380b85d40efe98c8a6160ef8069.png)](http://blog.thedataincubator.com/wp-content/uploads/2018/02/Ranking-Distributed-Computing-Packages.png)

包排名是基于等权重的三个组件：GitHub（stars 和 forks）、Stack Overflow（标签和问题）以及 Google 搜索结果数量。这些数据是通过可用的 API 获取的。制定一个全面的分布式计算包列表是棘手的——最终，我们抓取了我们认为具有代表性的三个不同列表。我们选择专注于 140 个框架和分布式编程包（详见下文方法）。计算每个指标的标准化分数使我们能够查看哪些包在每个类别中脱颖而出。[完整排名在这里](https://github.com/thedataincubator/data-science-blogs/blob/master/output/DL_libraries_final_Rankings.csv)，而[原始数据在这里](https://github.com/thedataincubator/data-science-blogs/blob/master/output/distributed_computing_data.csv)。

**Apache Spark 和 Apache Hadoop 各自拥有独特的地位**

Apache Spark (1) 是一个极其流行的开源分布式计算框架。Apache Spark 在 GitHub 活动指标中表现突出，其 forks 和 stars 的数量超过均值八个标准差以上。Apache Spark 利用内存数据处理，这使得它比其前身更快，并且能够进行机器学习。它还提供了 Scala 或 Python 的交互式控制台，后者在数据科学家中更受欢迎。尽管 Apache Spark 最初是为 Hadoop 生态系统设计的，但它可以使用多种不同的文件管理系统独立运行。Apache Hadoop (2) 在 Stack Overflow 活动中超越了 Apache Spark。Hadoop 的 Stack Overflow 活动与其他两个指标的脱节，很可能是因为 Apache Hadoop 的含义随着时间的推移而发展。术语“Hadoop”不仅指代框架，还可以指代构成生态系统的所有 Hadoop 相关项目。这导致了 Stack Overflow 分数的某种程度的膨胀。尽管如此，我们名单上的大多数框架和引擎都有 Apache Hadoop 集成。而且它在所有指标中都至少超过了均值两个标准差，巩固了其第二的位置。

**Apache Storm 和 Apache Flink 是流行的替代框架，尤其适用于流处理**

Apache Storm (4)，最初被誉为实时的 Apache Hadoop，是一个仅用于流处理的框架，最适合近实时的分布式计算。它在所有指标上的表现均优于平均水平。虽然 Apache Storm 在大规模处理流数据时表现良好，但它通常与 Apache Kafka (3) 一起使用，后者是一个处理来自实时数据源的原始消息的平台。类似于 Apache Spark，Apache Flink (8) 也是一个既能进行批处理又能进行流处理的框架。然而，Apache Spark 自称为一个可以处理流的批处理器，而 Apache Flink 则适合重型流处理和一些批处理任务。

**Stratio Crossdata 是排名最高的数据中心和增长最快的包**

Stratio Crossdata (6)通过提供统一的方式访问多个数据存储，扩展了Apache Spark的功能。

Stratio Crossdata使用类似SQL的语言和一个API来访问多个具有不同性质的数据存储，如Apache Cassandra、ElasticSearch、Arvo或MongoDB。Stratio Crossdata的Google搜索结果数量较上个季度增长了400%，这是我们列表中140个包中增长率最大的。

**前10名中的两个由Twitter开发**

在我们列表中的两个Twitter项目中，最受欢迎的是Apache Storm (4)，它于2011年由Twitter捐赠给Apache软件基金会。Twitter Heron (9)是Apache Storm的直接继任者，于2016年6月发布。Twitter Heron提供了改进的实时、容错流处理，其吞吐量高于Storm。Twitter Heron的季度增长率达到了180%，是第五高的。我们将有兴趣看到Twitter Heron是否能随着时间的推移进一步攀升。

**Hadoop生态系统主导**

Hadoop生态系统项目是最普遍和广泛采用的分布式计算框架和接口。我们排名前20的包中有17个是Hadoop生态系统的一部分或设计用于与Apache Spark或Apache Hadoop（包括HDFS）集成的。在Hadoop生态系统之外，Hazelcast (10)（一个内存数据网格）、Google BigQuery (12)（一个基于云的大数据分析Web服务，使用类似SQL的语法）和Metamarkets Druid (15)（一个实时分析大数据集的框架）在我们的指标中表现良好。

**限制**

与[任何分析](https://twitter.com/benhamner/status/732392995610198016)一样，在过程中做出了决定。所有源代码和数据都在[我们的Github页面](https://github.com/thedataincubator/data-science-blogs)上。分布式计算包的完整列表来源于几个渠道。

自然，一些存在较长时间的库将有更高的指标，因此排名也更高。唯一考虑这一点的指标是Google搜索季度增长率。

数据展示了一些困难：

+   一些库的名称是常见词（onyx、drools、disco），因此确定Google搜索结果数量的搜索词包括了额外的描述性词汇（“onyx平台”，“kiegroup drools”）或别名（“discoproject”）。所有搜索词可以在[这里](https://github.com/thedataincubator/data-science-blogs/blob/master/data/DC_packages_results_google.csv)找到。

+   手动检查确认了Stack Overflow标签和Github仓库位置。

+   Stack Overflow标签可以在[这里](https://github.com/thedataincubator/data-science-blogs/blob/master/data/DC_packages_results_stackoverflow.csv)找到。

+   Github仓库名称可以在[这里](https://github.com/thedataincubator/data-science-blogs/blob/master/data/DC_packages_results_github.csv)找到。

**方法**

所有源代码和数据在 [我们的 GitHub 页面](https://github.com/thedataincubator/data-science-blogs) 上。

我们首先从 [这些](https://github.com/onurakpolat/awesome-bigdata) [四个](https://projects.apache.org/projects.html?category) [来源](http://analyticsindiamag.com/10-hadoop-alternatives-consider-big-data/) [中](http://bigdata.andreamostosi.name/) 生成了一个140个分布式计算包的列表，然后收集了所有的指标，得出了一个用于排名的索引值。“Github”索引分数基于stars 和 forks，“Stack Overflow”索引分数基于包含包名的标签和问题，“搜索结果”基于过去五年的谷歌搜索总结果数以及过去三个月相对于前三个月计算的季度增长率。选择谷歌搜索结果数据作为指标而非谷歌趋势数据，因为某一关键词的索引网站数量比搜索该关键词的人数更能可靠地指示该包的使用流行度。此索引排名的计算可以在源代码中找到。

其他一些说明：

+   任何不可用的 Stack Overflow 或计数被转换为零计数。

+   如果不存在 GitHub 存储库，forks 和 stars 记录为零。

+   计数被标准化为均值0和偏差1，然后计算出“Github”和“Stack Overflow”索引分数，再与“搜索结果”结合得出总体索引分数。

所有数据已于2017年9月19日下载。

**资源**

源代码可在 [The Data Incubator](https://www.thedataincubator.com/)'s [GitHub](https://github.com/thedataincubator/data-science-blogs/) 上获取。

访问我们的网站了解更多信息：

1.  [数据科学奖学金](https://www.thedataincubator.com/fellowship.html) - 一个免费的、全职的为期八周的训练营计划，针对希望在纽约市、华盛顿特区、旧金山和波士顿成为专业数据科学家的博士和硕士毕业生。

1.  [招聘数据科学家](https://www.thedataincubator.com/hiring.html)

1.  [企业数据科学培训](https://www.thedataincubator.com/training.html)

1.  在线数据科学课程：由我们驻场数据科学专家教授的入门级兼职训练营，基于我们的奖学金课程，为繁忙的专业人士在空闲时间提升数据科学技能。

    +   [数据科学基础](https://www.thedataincubator.com/foundations.html)

    +   [机器学习简介](https://www.thedataincubator.com/machine-learning.html)

    +   [TensorFlow 人工智能简介](https://www.thedataincubator.com/artificial-intelligence-tensorflow.html)

    +   [Spark 分布式计算简介](https://www.thedataincubator.com/spark.html)

    +   [比特币与区块链简介](https://www.thedataincubator.com/bitcoin-and-blockchain.html)

个人简介： [Michael Li](https://www.linkedin.com/in/tianhuili) 是 The Data Incubator 的创始人兼首席执行官。他曾在 Foursquare 担任数据科学家，在 D.E. Shaw 和 J.P. Morgan 担任量化分析师，也曾在 NASA 担任火箭科学家。他在普林斯顿大学获得博士学位，并作为 Hertz 奖学金获得者完成学业，还在剑桥大学作为 Marshall 奖学金获得者学习了数学 Part III。在 Foursquare，Michael 发现他最喜欢的工作部分是教授和指导聪明的人关于数据科学。他决定创办一家初创公司，让他能够专注于自己真正热爱的事物。

[Rachel Kay Allen](https://www.linkedin.com/in/rachel-kay-allen-913b5696/) 是 Booz Allen Hamilton 的首席科学家，曾在 The Data Incubator 担任讲师。

**相关：**

+   [2018年数据科学领域的15个顶级 Scala 库](https://www.kdnuggets.com/2018/02/top-15-scala-libraries-data-science-2018.html)

+   [使用 Optimus 在 Apache Spark 上进行机器学习](https://www.kdnuggets.com/2017/11/machine-learning-with-optimus.html)

+   [在数据科学领域开局前需要了解的5件事](https://www.kdnuggets.com/2018/03/5-things-before-rushing-data-science.html)

### 更多主题

+   [2023年值得了解的顶级数据 Python 包](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

+   [3 个 Julia 包用于数据可视化](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [数据网格及其分布式数据架构](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)

+   [KDnuggets™ 新闻 22:n07, 2月16日：如何学习机器数学…](https://www.kdnuggets.com/2022/n07.html)

+   [如何通过云计算高效扩展数据科学项目](https://www.kdnuggets.com/2023/05/efficiently-scale-data-science-projects-cloud-computing.html)
