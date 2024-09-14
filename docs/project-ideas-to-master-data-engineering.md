# 掌握数据工程的项目建议

> 原文：[https://www.kdnuggets.com/project-ideas-to-master-data-engineering](https://www.kdnuggets.com/project-ideas-to-master-data-engineering)

![数据工程项目建议](../Images/476110dedc23dd90292c86bbef563b49.png)

图片由作者提供

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

对于任何数据领域的初学者来说，*真正*理解某一特定数据领域往往是困难的。你可以阅读理论解释和职位描述，观看解释它们的 YouTube 视频，但你的理解总停留在“明白了，但不完全”这一层次。

数据工程也是如此。当然，你需要了解什么是数据工程，以及 [数据工程师做什么](https://www.stratascratch.com/blog/what-does-a-data-engineer-do-and-what-they-dont-do/?utm_source=blog&utm_medium=click&utm_campaign=kdn+de+project+ideas)。我们将从这里开始。但你应该用实践来补充这些理论知识；在它们的交汇处存在真正的知识。

实际从事数据工程工作时，掌握数据工程是相当困难的。这主要是因为数据工程不仅仅涉及数据处理，还涉及数据架构和构建数据基础设施。

然而，有一种方法可以做到，那就是进行数据工程项目。了解数据工程师的工作将帮助我们选择适合的项目来掌握数据工程。

## **什么是数据工程？**

数据工程确保数据从多个不同的数据源流向数据存储，这些数据可以是批量的，也可以是实时的，供数据用户使用。在此过程中，数据还会被处理、分析，并转化为适合使用的格式。

这被称为数据管道，而数据工程师的工作是构建和维护它。

从这些描述中，我们可以提炼出数据工程的关键方面：

+   数据转化与处理

+   数据可视化

+   数据管道

+   数据存储

要掌握数据工程，你的项目应该专注于或包括以下一些主题。

由于数据工程的性质，想出一个只涉及其一个方面的项目几乎是不可能的；这就是数据工程师工作的全面性。真的不可能做一个仅处理数据的项目——好吧，这些数据来自哪里，又到哪里去呢？

所以，我选择的大多数项目都是端到端的数据工程项目，这些项目将教你如何构建数据管道——数据工程的核心。然而，这些项目采用了不同的方法和技术，因此你可以从一个项目中学到的方面，可能在另一个项目中学不到。

## **数据工程项目点子**

![掌握数据工程的项目点子](../Images/ceb2c73b6a484a1143bd332aa49999ad.png)

作者提供的图像

通过做项目，你能了解数据工程在实践中的实际情况。完成一个项目，你必须展示各种技术技能，对常见数据工程工具的熟悉程度，以及对整个过程的理解。

这使得这些项目非常适合学习。

## **1\. 数据管道开发项目**

没有比构建数据管道更能体现数据工程了。确保数据从源头流向数据用户，并通过扩展来支持数据驱动的决策，这正是数据工程的核心。

通过进行数据管道开发项目，你将学习到如何整合来自各种来源的数据以及整个ETL过程。

#### **项目建议**

链接: [CodeWith You (Yusuf Ganiyu) 的 AWS 端到端数据工程](https://www.youtube.com/watch?v=LSlt6iVI_9Y)

描述：这是一个优秀的项目，目标是构建一个数据管道，该管道将从 Reddit 中提取数据，进行转换，然后将其加载到 Redshift 数据仓库中。

视频引导你完成每一步，并且项目的 [源代码也可以在 GitHub 上获取](https://www.stratascratch.com/blog/19-data-science-project-ideas-for-beginners/?utm_source=blog&utm_medium=click&utm_campaign=kdn+de+project+ideas)。

使用的技术：

+   [Reddit API](https://www.reddit.com/dev/api/)

+   [Apache Airflow](https://airflow.apache.org) & [Celery](https://docs.celeryq.dev/en/stable/getting-started/introduction.html)

+   [PostgreSQL](https://www.postgresql.org)

+   [Amazon S3](https://aws.amazon.com/s3/)

+   [AWS Glue](https://aws.amazon.com/glue/)

+   [Amazon Athena](https://aws.amazon.com/athena/)

+   [Amazon Redshift](https://aws.amazon.com/redshift/)

## **2\. 数据转换项目**

数据转换意味着数据被改变为与分析工具兼容的标准化格式，适合于分析。

除了使数据分析和决策成为可能之外，数据转换还在提高数据质量方面发挥着重要作用，因为它涉及数据的清理和验证。

#### **项目建议**

链接: [StrataScratch 的 Chama 数据转换](https://platform.stratascratch.com/data-projects/data-transformation?utm_source=blog&utm_medium=click&utm_campaign=kdn+de+project+ideas)

描述：这里的任务是使用你选择的任何编程语言来转换位于三个 .csv 文件中的 Chama 数据，但需遵循特定的转换规则。

使用的技术：

+   [Python](https://www.python.org)（在官方解决方案中使用）

+   [Pandas](https://pandas.pydata.org)

## **3\. 数据湖实现项目**

数据湖是存储大量原始格式数据的中央存储库。它们在处理和分析大数据时至关重要。随着大数据在商业中的日益普及，数据工程师必须了解如何实施数据湖。

### **项目建议**

链接: [Kaviprakash Selvaraj 的端到端 Azure 数据工程项目](https://medium.com/@kaviprakash.2007/an-end-to-end-azure-data-engineering-project-using-sales-data-41a917fbcd70)

描述：这个 Azure 数据端到端的数据工程项目使用销售数据。它涵盖了数据摄取、处理和存储等主题。它的有趣之处在于，它概述了设置和管理数据湖的步骤，即 Azure 数据湖。

使用的技术：

+   [Azure 数据工厂](https://azure.microsoft.com/en-us/products/data-factory)

+   [Azure Databricks](https://azure.microsoft.com/en-us/products/databricks)

+   [Apache Spark](https://spark.apache.org)

+   [Azure Databricks SQL 分析](https://techcommunity.microsoft.com/t5/analytics-on-azure-blog/key-features-of-sql-analytics-in-azure-databricks/ba-p/1924410)

+   [Azure 数据湖存储](https://azure.microsoft.com/en-us/products/storage/data-lake-storage)

+   [Delta Lake](https://delta.io)

## **4\. 数据仓库项目**

数据从数据湖中提取，经过结构化处理后存储在数据仓库中。这些数据仓库作为商业智能的中央数据存储库。

实施数据仓库使得数据检索更高效，简化了数据管理，同时确保数据质量并提供数据洞察。

在数据仓库项目中，你将学习数据建模和数据库管理。

#### **项目建议**

链接: [Ahmed Ali 的 AWS 数据工程项目](https://github.com/Ahmedmagdy31/Data-Engineering-Project-over-AWS)

描述：这个端到端的项目使用 NYC 出租车数据，目标是在 AWS 中构建一个 ELT 管道。它适合学习数据仓库，因为数据被加载到数据仓库中，即 Amazon Redshift。

使用的技术：

+   [Amazon Redshift](https://aws.amazon.com/redshift/)

+   [AWS Step Functions](https://aws.amazon.com/step-functions/)

+   [AWS Glue](https://aws.amazon.com/glue/)

+   [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)

+   [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/)

## **5\. 实时数据处理项目**

实时处理数据变得越来越重要，以便企业能够做出及时和主动的决策。因此，数据工程师必须了解如何建立一个能够有效且高效地实时处理数据的系统。

#### **项目建议**

链接: [CodeWithYu (Yusuf Ganiyu) 的实时数据流](https://www.youtube.com/watch?v=GqAcTrqKcrY)

描述：这个 CodeWithYu 视频为你提供了关于构建数据流管道的详细指导。你将学习如何设置数据管道、实时流数据、分布式同步、数据处理、数据存储和容器化。

你将使用的数据是由[随机用户.me](http://randomuser.me) API生成的。正如我之前链接的其中一个视频，这个视频也有一个[在GitHub上的源码](https://github.com/airscholar/e2e-data-engineering)。

使用的技术：

+   [Apache Airflow](https://airflow.apache.org)

+   [Python](https://www.python.org)

+   [Apache Kafka](https://kafka.apache.org)

+   [Apache Zookeeper](https://zookeeper.apache.org)

+   [Apache Spark](https://spark.apache.org)

+   [Apache Cassandra](https://cassandra.apache.org/_/index.html)

+   [PostgreSQL](https://www.postgresql.org)

+   [Docker](https://www.docker.com)

## **6. 数据可视化项目**

虽然数据可视化可能不是你在考虑数据工程时首先想到的事情，但它是数据工程师的一项重要技能。

在数据工程的背景下，可视化数据通常意味着创建操作仪表板，展示数据管道的当前状态，例如处理速度或摄取的数据量。

数据工程师还可能会为存储在数据仓库中的数据创建仪表板，以帮助业务用户更轻松地获取所需信息。

#### **项目建议**

链接：[从原始数据到数据可视化 - Naufaldy Erianda 的数据工程项目](https://medium.com/@naufaldyer/end-to-end-project-data-engineering-from-raw-to-data-visualization-ed54aefb0322)

描述：这个项目的目标是从各种资源中提取数据，对其进行转换，并使其可用于数据可视化。最后，你将创建一个在Looker Studio中的仪表板。

使用的技术：

+   [MySQL](https://www.mysql.com)

+   [Airflow](https://airflow.apache.org)

+   [Google Cloud BigQuery](https://cloud.google.com/bigquery?hl=en)

+   [dbt](https://www.getdbt.com)

+   [Looker Studio](https://cloud.google.com/looker-studio?hl=en)

## **结论**

数据工程是一个复杂的领域，可能对初学者来说有些令人望而生畏。真正开始理解数据工程的最简单方法是通过做数据工程项目。

我建议了六个项目来教你：

+   构建管道

+   转换数据

+   实现数据湖

+   实现数据仓库

+   为实时数据处理构建一个管道

+   可视化数据

机器学习在自动化各种数据工程任务中越来越重要。因此，为了不被落在后头，可以查看一些[机器学习项目](https://www.stratascratch.com/blog/30-project-ideas-to-showcase-your-machine-learning-skills/?utm_source=blog&utm_medium=click&utm_campaign=kdn+de+project+ideas)和[数据科学项目](https://www.stratascratch.com/blog/19-data-science-project-ideas-for-beginners/?utm_source=blog&utm_medium=click&utm_campaign=kdn+de+project+ideas)，这些项目也可以用来练习数据工程技能。

[](https://twitter.com/StrataScratch)****[内特·罗西迪](https://twitter.com/StrataScratch)****是一位数据科学家，专注于产品策略。他还是一名兼职教授，教授分析学，并且是StrataScratch的创始人，该平台帮助数据科学家通过顶级公司提供的真实面试问题为面试做准备。内特撰写有关职业市场的最新趋势，提供面试建议，分享数据科学项目，并涵盖所有SQL相关内容。

### 相关话题

+   [19个初学者数据科学项目创意](https://www.kdnuggets.com/2021/11/19-data-science-project-ideas-beginners.html)

+   [作为数据科学家保持更新的5个项目创意](https://www.kdnuggets.com/2022/07/5-project-ideas-stay-uptodate-data-scientist.html)

+   [2022年人工智能项目创意](https://www.kdnuggets.com/2022/01/artificial-intelligence-project-ideas-2022.html)

+   [掌握数据科学、数据工程、机器学习…的25个免费课程](https://www.kdnuggets.com/25-free-courses-to-master-data-science-data-engineering-machine-learning-mlops-and-generative-ai)

+   [掌握数据工程的5个免费课程](https://www.kdnuggets.com/5-free-courses-to-master-data-engineering)

+   [掌握数据工程的10个GitHub库](https://www.kdnuggets.com/10-github-repositories-to-master-data-engineering)
