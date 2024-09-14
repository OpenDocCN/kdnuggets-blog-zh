# Spark SQL用于实时分析

> 原文：[https://www.kdnuggets.com/2015/09/spark-sql-real-time-analytics.html](https://www.kdnuggets.com/2015/09/spark-sql-real-time-analytics.html)

**由Sumit Pal和Ajit Jaokar编写，**（FutureText）。

本文是即将推出的**物联网数据科学从业者课程**在伦敦的一部分。如果你想成为物联网的数据科学家，这个密集课程非常适合你。我们涵盖了传感器融合、时间序列、深度学习等复杂领域。我们使用Apache Spark、R语言以及领先的物联网平台。有关更多信息，请联系[info@futuretext.com](mailto:info@futuretext.com)。

**概述**

这是讨论Spark在物联网实时分析中使用SQL的3部分系列文章的第一部分。第一部分讨论了使用Spark的SQL的技术基础。第二部分讨论了使用Spark SQL进行实时分析。最后，第三部分讨论了一个物联网实时分析的用例。

**介绍**

在第一部分中，我们讨论了Spark SQL以及为什么它是实时分析的首选方法。Spark SQL是Apache Spark中的一个模块，它将关系处理与Spark的函数式编程API集成。自1.0版本以来，Spark SQL就是Spark Core的一部分。它可以与现有的Hive部署并行运行HiveQL/SQL，或替代现有的Hive部署。它可以连接到现有的BI工具。它在Python、Scala和Java中都有绑定。它为框架提供了两个重要的补充。首先，它提供了关系和过程处理之间的紧密集成，具有与过程式Spark API集成的声明性DataFrame API。其次，它包括一个可扩展的优化器，使用Scala构建，利用其强大的模式匹配能力，使得添加可组合规则、控制代码生成和定义扩展变得容易。

**Spark SQL的目标和目标**

尽管关系方法已被应用于解决大数据问题，但它对许多大数据应用来说仍然不够充分。直到最近，关系和过程方法一直保持分离，迫使开发人员选择其中一种范式。Spark SQL框架结合了这两种模型。

正如他们所说，“读取数据的最快方式就是根本不读取它”。

Spark SQL支持在Spark程序（通过RDDs）和外部数据源上的关系处理。它可以轻松支持新的数据源，包括半结构化数据和适用于查询联合的外部数据库。

Spark SQL通过以下方式帮助这一理念

+   通过使用各种列式格式将数据转换为更高效的格式（从存储、网络和IO的角度来看）

+   使用数据分区

+   使用数据统计跳过数据读取

+   将谓词推送到存储系统

+   尽可能晚地优化，直到获取有关数据管道的所有信息

在内部，Spark SQL和DataFrame利用Catalyst查询优化器智能地规划查询的执行。

**Spark SQL**

Spark SQL 可以支持批处理或流处理 SQL。使用 RDDs，核心 Spark 框架支持批处理工作负载。RDD 可以指向静态数据集，Spark 的丰富 API 可用于操作在内存中以惰性求值处理的批数据集。

**流处理 + SQL 和 DataFrames**

让我们快速了解一下在 Spark 核心框架中什么是 RDD，什么是 DStream，这些是本文讨论的基本构建块。

Spark 在 RDDs（弹性分布式数据集）上操作，这是一种内存中的数据结构。每个 RDD 代表数据的一部分，这些数据被分区到集群中的数据节点。RDD 是不可变的，当应用变换时会创建新的 RDD。RDDs 在并行中操作，使用诸如映射、过滤等变换/操作。这些操作在所有分区上同时进行。RDDs 是弹性的，如果由于节点崩溃而丢失某个分区，可以从原始来源重建它。

![Spark RDD](../Images/76981d00d9f9634e21074e9cdd8f42fc.png)

Spark Streaming 提供了一种称为 DStream（离散流）的抽象，它是一个连续的数据流。DStreams 可以从输入数据流或 Kafka、Flume 等源创建，或通过对其他 DStreams 应用操作创建。DStream 本质上是 RDD 的序列。

![实时 RDD](../Images/58e5b962f873f68db85bccce6b212219.png)

由 DStreams 生成的 RDD 可以转换为 DataFrames 并使用 SQL 查询。该流可以通过 Spark 的 JDBC 驱动程序暴露给任何外部应用程序，进行 SQL 查询。流数据的批次存储在 Spark 的工作内存中，可以通过 SQL 或 Spark 的 API 进行交互式查询。

StreamSQL 是一个 Spark 组件，它结合了 Catalyst 和 Spark Streaming 来对 DStreams 执行 SQL 查询。StreamSQL 扩展了 SQL 以支持流，具有以下操作：

+   对流进行 SELECT 以计算函数或筛选不需要的数据（使用 WHERE 子句）

+   将流与一个或多个数据集进行 JOIN，以生成新的流。

+   窗口化和聚合 – 流可以被限制以创建有限的数据集。窗口化允许基于字段值的复杂消息选择。一旦创建了有限的批次，可以应用分析。

Spark SQL 具有以下组件

**组件**

Spark SQL 核心

+   以 RDD 执行查询

+   读取多种文件格式的数据集，如 Parquet、JSON、Avro 等。

+   读取 SQL 和 NOSQL 数据源

Hive 支持

+   HQL、MetaStore、SerDes、UDFs

Catalyst 优化器

+   它优化了关系代数 + 表达式

+   它进行查询优化。

**Spark SQL 解决的问题**

Spark SQL 提供了一个统一的框架，无需将数据移出集群。无需安装或集成额外的模块。它提供了一个统一的加载/保存接口，无论数据源和编程语言是什么。

下面的示例显示了从 avro 加载数据并将其转换为 parquet 是多么简单。

`val df = sqlContext.load("mydata.avro", "com.databricks.spark.avro") df.save("mydata.parquet", "parquet")`

Spark SQL 具有统一的框架来解决批处理和流处理中的相同分析问题，这一直是数据处理中的圣杯。已有框架可以很好地处理其中之一，并在可扩展性、性能和功能集方面表现良好，但在 Spark / Spark SQL 之前，拥有一个同时处理批处理和流处理的统一框架是不可行的。通过 Spark 框架，相同的代码（逻辑）可以用于批处理数据 – RDDs 或流处理数据集（DStreams – 离散化流）。DStream 只是一系列的 RDDs。这种表示方式允许批处理和流处理工作负载无缝工作。这大大减少了代码维护开销，并减少了对两种不同技能集的开发者的培训需求。

**从 JDBC 数据源读取**

从 JDBC 读取的数据源已作为 Spark SQL 的内置数据源添加。Spark SQL 可以从任何支持 JDBC 的现有关系数据库中提取数据。示例包括 mysql、postgres、H2 等。从这些系统中的一个读取数据就像创建指向外部表的虚拟表一样简单。然后，可以轻松地读取该表的数据，并与 Spark SQL 支持的任何其他数据源进行联接。

**Spark SQL 与数据框架**

数据框架是分布式的行集合，按命名列组织，是一种选择、过滤、聚合和绘制结构化数据的抽象 – 之前称为 SchemaRDD。

DataFrame API 可以对外部数据源和 Spark 的内置分布式集合执行关系操作。

DataFrame 在 Spark 程序中提供了丰富的关系/过程集成功能。DataFrames 是结构化记录的集合，可以使用 Spark 的过程 API 进行操作，或者使用新的关系 API 进行更丰富的优化。它们可以直接从 Spark 的内置分布式对象集合中创建，从而在现有的 Spark 中进行关系处理。

DataFrames 比 Spark 的过程 API 更方便和高效。它们使得使用 SQL 语句在一次操作中计算多个聚合变得容易，这在传统的函数式 API 中很难表达。

与 RDDs 不同，DataFrames 会跟踪其模式并支持各种关系操作，从而实现更优化的执行。它们通过反射推断模式。

Spark DataFrames 是惰性的，每个 DataFrame 对象表示一个计算数据集的逻辑计划，但在用户调用特殊的“输出操作”如保存之前不会执行。这使得所有操作能够进行丰富的优化。

DataFrames 使 Spark 的 RDD 模型得以演变，提供简化的方法来过滤、聚合和投影大数据集，从而使 Spark 开发者更快、更容易地处理结构化数据。DataFrames 可在 Spark 的 Java、Scala 和 Python API 中使用。

Spark SQL 的数据源 API 可以从各种数据源和数据格式中读取和写入 DataFrames——包括 Avro、parquet、ORC、JSON、H2。

*减少代码编写的示例——使用普通 RDDs 和 DataFrame API 的 SQL*

以下的 Scala 示例展示了等效的代码——一个使用 Spark 的 RDD API，另一个使用 Spark 的 DataFrame API。我们有一个对象——People——包括 firstname、lastname、age 作为属性，目标是获取按 firstname 分组的年龄基本统计数据。

`case class People(firstname: String, lastname: String, age: Intger) val people = rdd.map(p => (people.firstname, people.age)).cache()`

`// RDD 代码 val minAgeByFN = people.reduceByKey( scala.math.min(_, _) ) val maxAgeByFN = people.reduceByKey( scala.math.max(_, _) ) val avgAgeByFN = people.mapValues(x => (x, 1)) .reduceByKey((x, y) => (x._1 + y._1, x._2 + y._2)) val countByFN = people.mapValues(x => 1).reduceByKey(_ + _)`

`// Data Frame 代码 df = people.toDF people = df.groupBy("firstname").agg( min("age"), max("age"), avg("age"), count("*"))`

由于 Catalyst 优化器，DataFrames 比使用普通 RDDs 更快。DataFrames 提供与 SQL 和 Pig 等关系查询语言相同的操作。

**使用 Spark SQL 解决流式分析的方案**

下图展示了我们使用 Spark SQL 进行实时分析的方法。这遵循了构建实时分析系统的 Lambda 架构模式，适用于大规模流数据。

**Spark SQL 的缺点**

![lambda-architecture-spark-sql](../Images/f34f18a8fb3c7c2708686cefbea49f34.png)

与任何运行在 Hadoop 集群上的工具一样——服务水平协议（SLA）并不依赖于引擎的速度——而是依赖于系统上运行的其他并发用户的数量。

对于初次使用者来说，Spark SQL 代码有时过于简洁，难以理解正在做什么。需要一些经验和实践来习惯 Spark SQL 代码的编码风格和隐式性。

**结论**

Spark SQL 是核心 Spark API 的重要进化。尽管 Spark 的原始函数式编程 API 相当通用，但它仅提供了有限的自动优化机会。Spark SQL 使 Spark 更加易于使用。

本文旨在为下一组文章奠定基础，这些文章将探讨在物联网领域使用 Spark SQL 进行实时分析。

**参考文献：**

StreamSQL – [https://en.wikipedia.org/wiki/StreamSQL](https://en.wikipedia.org/wiki/StreamSQL)

StreamSQL – [https://github.com/thunderain-project/StreamSQL](https://github.com/thunderain-project/StreamSQL)

**简历：** **[Sumit Pal](https://www.linkedin.com/profile/view?id=AAIAAADShS4B8ypiJ59lZDz0YeioS2qlAuFkKtI&trk)** 是一位大数据、可视化和数据科学顾问。他还是一名软件架构师和大数据爱好者，构建端到端的数据驱动分析系统。Sumit 曾在微软（SQL 服务器开发团队）、甲骨文（OLAP 开发团队）和 Verizon（大数据分析团队）工作，拥有 22 年的职业生涯。目前，他为多个客户提供数据架构和大数据解决方案的建议，并进行 Spark、Scala、Java 和 Python 的实际编码。Sumit 在 [sumitpal.wordpress.com/](https://sumitpal.wordpress.com/) 撰写博客。

**[Ajit Jaokar](https://www.linkedin.com/in/ajitjaokar) ([@AjitJaokar](https://twitter.com/ajitjaokar))** 从事数据科学和物联网的研究与咨询。他的工作基于他在牛津大学和马德里理工大学的教学，涵盖了物联网、数据科学、智能城市和电信领域。

**相关内容：**

+   [使用 Apache Spark 介绍大数据](/2015/06/introduction-big-data-apache-spark.html)

+   [50+ 数据科学和机器学习备忘单](/2015/07/good-data-science-machine-learning-cheat-sheets.html)

+   [大数据的“重大”问题：Hadoop 还是 Spark？](/2015/08/big-data-question-hadoop-spark.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

### 更多相关内容

+   [数据库内分析：利用 SQL 的分析函数](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)

+   [用 SQL 查询你的 Pandas DataFrames](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [数据准备 SQL 备忘单](https://www.kdnuggets.com/2021/05/data-preparation-sql-cheat-sheet.html)

+   [SQL 入门备忘单](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [用 SQL 处理时间序列中的缺失值](https://www.kdnuggets.com/2022/09/handling-missing-values-timeseries-sql.html)

+   [数据科学的 4 个有用的中级 SQL 查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)
