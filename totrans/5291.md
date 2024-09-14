# 实时流处理分析的类似SQL查询语言

> 原文：[https://www.kdnuggets.com/2015/03/sql-query-language-realtime-streaming-analytics.html](https://www.kdnuggets.com/2015/03/sql-query-language-realtime-streaming-analytics.html)

作者：Srinath Perera（[@srinath_perera](https://twitter.com/srinath_perera)）

我上周在[Strata+Hadoop World 2015](http://strataconf.com/big-data-conference-ca-2015)参加了会议，确实对实时分析的兴趣达到了顶峰。

实时分析，或者人们称之为实时分析，有两种类型。

1.  实时流处理分析（静态查询一次给出，不会改变，它们处理数据时无需存储。CEP、Apache Storm、Apache Samza等是这方面的例子）

1.  实时交互/临时分析（用户发出临时动态查询，系统响应）。Druid、SAP Hana、VoltDB、MemSQL、Apache Drill是这方面的例子。

在这篇文章中，我专注于实时流处理分析。（临时分析无论如何都会使用类似SQL的查询语言。）

![数据库大小时间分析](../Images/df8caa7ba3c18e86df3a5eb6f177b447.png)

当谈到实时分析时，人们仍然只考虑计数用例。然而，这只是冰山一角。由于实时用例中数据的时间维度，您可以做的还有很多。让我们看看一些常见的模式。

1.  简单计数（例如，故障计数）

1.  使用窗口进行计数（例如，每小时的故障计数）

1.  预处理：过滤、转换（例如，数据清理）

1.  警报，阈值（例如，高温报警）

1.  数据关联，检测丢失事件，检测错误数据（例如，检测失败的传感器）

1.  连接事件流（例如，检测到足球的击打）

1.  与数据库中的数据合并，收集，按条件更新数据，检测事件序列模式（例如，小交易后跟随大交易）

1.  跟踪——在空间、时间等方面跟踪相关实体的状态（例如，航空公司行李的位置、车辆、野生动物追踪）

1.  检测趋势——上升、转折、下降、异常值、复杂趋势如三重底等（例如，算法交易、SLA、负载均衡）

1.  学习模型（例如，预测性维护）

1.  预测下一个值和纠正措施（例如，自动驾驶汽车）

为什么我们需要类似SQL的查询语言用于实时流处理分析？

上述每一个都在用例中出现过，我们已经使用类似SQL的CEP查询语言实现了它们。了解实现CEP核心概念（如滑动窗口、时间查询模式）的内部细节，我认为每个流处理用例开发者都不应该重新编写这些。算法并非简单，而且很难做到准确！

相反，我们需要更高层次的抽象。我们应该一次性实现这些抽象，并重复使用它们。我们可以从 Hive 和 Hadoop 中学到的最佳教训就是它们正好为批量分析做了这些工作。我多次解释了 Hive 中的大数据，大多数人很快就能理解。Hive 已成为大多数大数据用例的主要编程 API。

以下是 SQL 类查询语言的一些理由。

1.  实时分析很困难。每个开发者都不愿意手动实现滑动窗口和时间事件模式等。

1.  对于熟悉 SQL 的人（几乎每个人都知道），它易于跟随和学习。

1.  SQL 类语言表达力强，简洁、甜美且快速！！

1.  SQL 类语言定义了涵盖 90% 问题的核心操作

1.  专家们可以随时深入研究！

1.  实时分析运行时可以更好地优化 SQL 类模型的执行。大多数优化已经被研究过，还有很多可以借鉴数据库优化的经验。

那么这些语言是什么呢？在复杂事件处理的世界里定义了很多（例如 WSO2 Siddhi、Esper、Tibco StreamBase、IBM Infosphere Streams 等）。SQL 流有一个完全符合 ANSI SQL 的版本。上周我在 Strata 会议上详细讨论了这个问题以及 CEP 如何满足要求。这里是幻灯片

**[可扩展的实时分析与声明式 SQL 类复杂事件处理脚本](http://www.slideshare.net/hemapani/scalable-realtime-analytics-with-declarative-sql-like-complex-event-processing-scripts "可扩展的实时分析与声明式 SQL 类复杂事件处理脚本")** 来自 **[Srinath Perera](http://www.slideshare.net/hemapani)**

个人简介：[Srinath Perera](http://people.apache.org/~hemapani/) 是一位科学家、软件架构师和程序员，专注于分布式系统。

原文：[srinathsview.blogspot.com/2015/02/why-we-need-sql-like-query-language-for.html](https://srinathsview.blogspot.com/2015/02/why-we-need-sql-like-query-language-for.html)

**相关：**

+   [采访：Ted Dunning，MapR 讲述大数据中实时性的真正含义](/2015/03/interview-ted-dunning-mapr-real-time-big-data.html)

+   [2015 年大数据的 8 大趋势](/2015/01/recruiter-8-trends-big-data-2015.html)

+   [即将举行的分析、大数据、数据科学网络广播 – 3 月 10 日及以后](/2015/03/upcoming-webcasts-mar10-analytics-big-data-science.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [用 SQL 查询你的 Pandas DataFrames](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [我们能用 T5 查询表格吗？](https://www.kdnuggets.com/2022/05/query-table-t5.html)

+   [SQL 查询优化技巧](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [提高 SQL 查询性能的 5 个技巧](https://www.kdnuggets.com/5-tips-for-improving-sql-query-performance)

+   [如何在 Snowflake 上构建流式半结构化分析平台](https://www.kdnuggets.com/2023/07/build-streaming-semistructured-analytics-platform-snowflake.html)

+   [为机器学习项目提供简单快速的数据流](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)
