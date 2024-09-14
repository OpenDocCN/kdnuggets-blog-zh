# 评估 HTAP 数据库在机器学习应用中的表现

> 原文：[`www.kdnuggets.com/2016/11/evaluating-htap-databases-machine-learning-applications.html/2`](https://www.kdnuggets.com/2016/11/evaluating-htap-databases-machine-learning-applications.html/2)

Oracle Exadata 是部署最广泛的高端 OLTP 系统，并且已经扩展了内存列式功能的新 OLAP 能力。Exadata 完全符合 ACID 标准，支持所有隔离级别，Oracle 使用混合表示，其中数据以元组（行式）插入，然后转换为内存中的列式表示。Oracle 严格来说是一个扩展式工程解决方案，性能优越但成本非常高。该系统不是开源的，Oracle 提供的数据库即服务模型。

MemSQL 主要用于分析工作负载。它符合 ACID 标准并处理事务更新，但这些更新并未针对大规模实时 OLTP 工作负载进行优化。MemSQL 通常不用于支持实时并发应用程序。MemSQL 使用混合表示，其中数据以元组（行式）插入，然后转换为列式。MemSQL 是一个扩展解决方案，通常部署在普通集群上，并且能够在集群中分布计算并扩展。该系统不是开源的，并且不通过 DBaaS 提供。

Splice Machine 是一个完全符合 ACID 标准的 MVCC，能够支持包括原生 Oracle PL/SQL 在内的应用程序。OLAP 计算在 Apache Spark 上进行，而事务查询则在 Apache HBase 上进行。原生数据持久化在 Apache HBase 中。数据也可以持久化在外部 Parquet 和 ORC 列式文件中。Splice Machine 是一个扩展解决方案，其基于成本的优化器通过 Apache Spark 或 Apache HBase 分配工作负载。该系统是开源的，并将在 2017 年第一季度作为 DBaaS 提供。

Apache Hive 主要用于分析工作负载。它符合 ACID 标准并处理事务更新，但这些更新并未针对大规模实时 OLTP 工作负载进行优化。Hive 通常不用于支持实时并发应用程序。一个 Hive 应用程序可以在另一个从同一分区读取的应用程序运行时添加行，而不会相互干扰。该系统使用 Hadoop 文件系统上的原生文件存储。较旧的系统将原始数据存储在 HDFS 上，但较新的系统使用 Apache Parquet 或 ORC 列式格式。这些列式存储系统压缩数据并表现出色。Hive 是一个扩展解决方案，通常部署在普通集群上。它依赖于 Hadoop 文件系统和 Map-Reduce 计算。该系统是开源的，通过 Quoble、Amazon 和 Google 提供 DBaaS。

Apache HAWQ 主要用于分析工作负载。它符合**ACID**规范，处理事务更新，但这些更新并未针对大规模实时 OLTP 工作负载进行优化。HAWQ 通常不用于支持实时并发应用。HAWQ 在 HDFS 上以多种格式存储数据，包括 Apache Parquet。HAWQ 是一种扩展型解决方案，通常部署在普通集群上。它将计算分布在集群中并能进行扩展。该系统是开源的，并通过 Pivotal 提供服务。

Apache Trafodion 是一种 SQL-on-HBase 解决方案，旨在支持具有两阶段提交协议的完整 OLTP 工作负载。Apache Trafodion 使用 HBase 作为持久化的行式存储。Apache Trafodion 是一种扩展型解决方案，将工作分布在执行代理的集群中，这些代理将工作分配给 HBase 区域服务器。该系统是开源的，但不作为 DBaaS 提供。

Apache Kudu/Impala 主要用于分析工作负载。它符合**ACID**规范，处理事务更新，但这些更新并未针对大规模实时 OLTP 工作负载进行优化。Kudu 通常不用于支持实时并发应用。Apache Kudu 是一种混合键值存储，具有元组型和列式存储。Kudu 使用混合表示法，其中数据以元组形式插入到写优化的 LSM 树中，然后使用 Apache Parquet 转换为列式存储。Apache Kudu/Impala 系统是扩展型系统，利用集群中的并行计算并尽可能向量化指令。该系统是开源的，但不作为 DBaaS 提供。

HTAP 系统结合了事务和分析能力。大多数 HTAP 系统能够接受事务更新并进行分析，而一些 HTAP 系统甚至能够在进行分析的同时支持并发应用。系统之间的多样性仍在继续，因为一些 HTAP 系统是扩展型的昂贵解决方案，而其他则是扩展型的较便宜解决方案，还有一些是开源的，而另一些是专有的。组织可以平衡这些需求以扩展传统应用并使其智能化。

**个人简介：** [Monte Zweben](https://www.linkedin.com/in/mzweben) 是一位技术行业资深人士。Monte 的早期职业生涯是在 NASA Ames Research Center 担任人工智能分部副主任，他因在航天飞机项目中的工作而获得了著名的太空行动奖。Monte 随后创办了 Red Pepper Software，并担任首席执行官，这是一家领先的供应链优化公司，1996 年与 PeopleSoft 合并，他在 PeopleSoft 担任制造业务部门的副总裁和总经理。1998 年，Monte 创办了 Blue Martini Software - 领先的零售电商和多渠道系统公司。Blue Martini 在 2000 年通过最成功的首次公开募股之一上市，并现在成为 JDA 的一部分。Blue Martini 之后，他担任了 SeeSaw Networks 的主席，这是一家数字化的、基于地点的媒体公司。Monte 还是《智能调度》的合著者，并在《哈佛商业评论》和各种计算机科学期刊及会议论文集中发表过文章。Zweben 目前担任 Rocket Fuel Inc.的主席，并在卡内基梅隆大学计算机科学学院的院长顾问委员会任职。

**相关：**

+   KDnuggets 顶级推文，8 月 8-10 日：忘记 SQL 与 NoSQL，新趋势是 HTAP：混合事务/分析处理

+   5 个理由说明机器学习应用需要更好的 Lambda 架构

+   访谈：Antonio Magnaghi，TicketMaster 谈通过 Lambda 架构统一异构分析

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

### 更多相关话题

+   [超越准确性：使用 NLP 测试库评估与改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [评估计算文档相似性的方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [键值数据库详解](https://www.kdnuggets.com/2021/04/nosql-explained-understanding-key-value-databases.html)

+   [从 Oracle 到人工智能数据库：数据存储的演变](https://www.kdnuggets.com/2022/02/oracle-databases-ai-evolution-data-storage.html)

+   [NoSQL 数据库及其使用案例](https://www.kdnuggets.com/2023/03/nosql-databases-cases.html)

+   [人工智能和大型语言模型中的向量数据库应用](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)
