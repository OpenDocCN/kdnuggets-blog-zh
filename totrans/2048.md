# 数据工程简介

> 原文：[https://www.kdnuggets.com/2020/12/introduction-data-engineering.html](https://www.kdnuggets.com/2020/12/introduction-data-engineering.html)

[评论](#comments)

**作者：[Xinran Waibel](https://medium.com/@xinran.waibel)，Netflix的数据工程师**。

根据最近发布的[Dice 2020科技职位报告](https://techhub.dice.com/Dice-2020-Tech-Job-Report.html)，数据工程师是2019年增长最快的科技职业，开放职位数量同比增长了50%。由于数据工程是一个相对较新的职业类别，我经常收到有意从事这一职业的人的问题。在这篇博客文章中，我将分享我成为数据工程师的经历，并回答一些关于数据工程的常见问题。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

![](../Images/b9caac07ae2eb5fbb9bce2a52a92bc48.png)

*来源：[Dice 2020科技职位报告](https://techhub.dice.com/Dice-2020-Tech-Job-Report.html)。*

### 我的数据工程师之旅

几年前，在成为数据工程师之前，我主要从事数据库和应用程序开发（以及规模和性能测试）。我非常喜欢使用[RDBMS](https://en.wikipedia.org/wiki/Relational_database)和数据，因此决定从事专注于大数据的工程职业。在网上研究后，我了解到一种我从未听说过的工作，但认为可能是适合我的“白马王子”：数据工程师。当时我感到迷茫，因为不知道从何开始，也不确定这是否适合我。然而，我幸运地有一位导师，他指导我入门并帮助我获得了第一份数据工程工作。从那时起，我一直在这个领域全职工作，仍然非常热爱！

### 随便问我什么

![](../Images/250cb48d98df446d74b2c06f089aa43e.png)

*图片由[Camylla Battani](https://unsplash.com/@camylla93?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，发布在[Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上。*

**问：数据工程师做什么？**

简而言之，数据工程师负责设计、开发和维护数据平台，包括数据基础设施、数据应用、数据仓库和数据管道。

在大公司中，数据工程师通常被分成不同的团队，各自负责数据平台的特定部分。

+   **数据仓库与管道**：数据仓库工程师构建批处理和/或实时数据管道，以整合系统之间的数据并支持[data warehouse](https://en.wikipedia.org/wiki/Data_warehouse)。由于数据仓库旨在解决业务问题，因此数据仓库工程师通常与数据分析师、科学家或服务于特定业务职能的业务团队密切合作。

+   **数据基础设施**：数据基础设施工程师构建和维护数据平台的基础：所有运行其上的分布式系统。例如，在Target，数据基础设施团队维护整个组织使用的[Hadoop](https://hadoop.apache.org/)集群。

+   **数据应用**：数据应用工程师是构建内部数据工具和API的软件工程师。有时，一个出色的内部工具后来可能会成为公司的开源产品。例如，Lyft的一个数据产品团队构建了一个名为[Amundsen](https://github.com/lyft/amundsen)的数据发现工具，并于2019年开源。

**问：什么是数据仓库？**

数据仓库是一个数据存储系统，存储来自各种来源的数据，主要用于数据分析。公司的数据通常存储在不同的事务系统中（甚至更糟的是作为文本文件），而事务数据高度规范化，不适合分析。建立数据仓库的主要原因是将所有类型的数据以优化的格式存储在一个集中位置，以便数据科学家可以一起分析这些数据。许多数据库适合作为数据仓库，例如[Apache Hive](https://hive.apache.org/)、[BigQuery](https://cloud.google.com/bigquery)（GCP）和[RedShift](https://aws.amazon.com/redshift/)（AWS）。

**问：什么是数据管道？**

数据管道是一系列数据处理过程，用于在不同系统之间提取、处理和加载数据。数据管道主要有两种类型：批处理驱动和实时驱动：

+   **批处理驱动**：批处理数据管道仅在特定频率下处理数据，通常由数据编排工具调度，如[Airflow](https://towardsdatascience.com/https-medium-com-xinran-waibel-build-data-pipelines-with-apache-airflow-808a4de79047)、[Oozie](https://towardsdatascience.com/lesser-known-tips-on-apache-oozie-1e9bee9169da)或[Cron](https://en.wikipedia.org/wiki/Cron)。它们通常一次处理大量历史数据，因此需要较长时间才能完成，并且在最终系统中引入更多数据延迟。例如，一个基于批处理的数据管道每天12 AM从API下载前一天的数据，转换数据，然后将其加载到数据仓库中。

+   **实时**：实时数据管道会在新数据可用时立即处理，源系统与终端系统之间几乎没有延迟。实时数据处理的架构与批处理管道非常不同，因为数据被视为事件流，而不是记录块。例如，为了将上述管道重建为实时管道，需要一个像 [Kafka](https://www.confluent.io/what-is-apache-kafka/) 这样的事件流工具：[Kafka Connector](https://www.confluent.io/connectors/) 将数据从 API 流式传输到 Kafka 主题，[Kafka Streams](https://docs.confluent.io/current/streams/index.html)（或 [Kafka Producer](https://docs.confluent.io/current/clients/producer.html)）处理 Kafka 主题中的原始数据，并将转换后的数据加载到另一个 Kafka 主题中。源 API 和目标 Kafka 主题之间的延迟可能在一秒内！

![](../Images/9cf486eebac5af88cf75e35e9821685d.png)

*来源：[数据工程师与数据科学家](https://www.oreilly.com/radar/data-engineers-vs-data-scientists/)。*

**问：数据工程师与数据科学家的区别是什么？**

数据工程师构建数据平台，使数据科学家能够分析数据和训练[机器学习 (ML)](https://en.wikipedia.org/wiki/Machine_learning)模型。有时，数据工程师也需要进行数据分析，并帮助数据科学家将ML模型集成到数据管道中。在一些数据团队中，你可能会发现数据科学家从事数据工程工作。数据工程师和数据科学家之间有几个重叠的技能：编程、数据管道建设和数据分析。

有一个新兴角色叫做 ML 工程师，负责在两个领域之间搭建桥梁。ML 工程师在工程和机器学习方面拥有强大的技能，负责优化和生产化 ML 模型。

![](../Images/30e37f5f66af72c3e269c6c9e7fb5c3b.png)

*照片由 [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。*

**问：要成为数据工程师，我应该学习哪些编程语言？**

简短的答案是：Python、Java/Scala 和 SQL。

对于数据应用轨道，你还需要学习用于 [全栈](https://skillcrush.com/blog/front-end-back-end-full-stack/) 开发的常见编程语言，例如 HTML、CSS 和 JavaScript。

**问：我应该学习哪些技能和工具？**

以下是核心技能列表以及一个流行的框架：

+   分布式系统: [Hadoop](https://hadoop.apache.org/)

+   数据库: [MySQL](https://www.mysql.com/)

+   数据处理: [Spark](https://spark.apache.org/)

+   实时数据生态系统: [Kafka](https://www.confluent.io/what-is-apache-kafka/)

+   数据编排：[Airflow](https://towardsdatascience.com/https-medium-com-xinran-waibel-build-data-pipelines-with-apache-airflow-808a4de79047)

+   数据科学和机器学习：[pandas](https://pandas.pydata.org/)(Python库)

+   全栈开发：[React](https://reactjs.org/)

由于[大数据世界中不再存在一刀切的解决方案](https://www.allthingsdistributed.com/2018/06/purpose-built-databases-in-aws.html)，每家公司都在为其数据平台利用不同的工具。因此，我建议你首先学习每个核心技能的基础知识，然后选择一个流行的工具深入学习，并理解你选择的工具与其他工具之间的权衡。要达到下一个层次，你还需要了解所有工具如何在数据架构中协同工作。

*(对高效学习技术栈感兴趣？查看一下*[*系统化学习方法*](https://towardsdatascience.com/systematic-learning-matters-for-engineers-38045f082293)*。)*

今天，地球上每个人每秒钟生成大约1.7MB的新数据，这些数据包含巨大的价值，没有数据工程无法收获。这就是为什么我如此热爱我的工作的原因。 :)

[原文](https://towardsdatascience.com/introduction-to-data-engineering-e16c9942dc2c)。转载已获许可。

**简介：** [Xinran Waibel](https://medium.com/@xinran.waibel) 是一位经验丰富的数据工程师，现居旧金山湾区，目前在Netflix工作。她还是《Towards Data Science》、《Google Cloud》和Medium上的《The Startup》的技术作家。

**相关：**

+   [数据科学家缺失的团队](https://www.kdnuggets.com/2020/11/missing-teams-data-scientists.html)

+   [数据工程所需的技能](https://www.kdnuggets.com/2020/06/skills-build-data-engineering.html)

+   [五个有趣的数据工程项目](https://www.kdnuggets.com/2020/03/data-engineering-projects.html)

### 更多相关内容

+   [成为一名优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，然后再找到目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
