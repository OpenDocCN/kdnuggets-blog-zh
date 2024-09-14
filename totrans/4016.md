# 每个数据工程师都应该知道的 7 个 Python 库

> 原文：[https://www.kdnuggets.com/7-python-libraries-every-data-engineer-should-know](https://www.kdnuggets.com/7-python-libraries-every-data-engineer-should-know)

![每个数据工程师都应该知道的 7 个 Python 库](../Images/57686075a802394bd08409e9293398cc.png)

图片由作者提供

作为数据工程师，你需要掌握的工具和框架列表可能会让人望而却步。但至少，你应该精通 SQL、Python 和 Bash 脚本。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

除了熟悉核心的 Python 特性和内置模块外，你还应该能够熟练使用 Python 库来完成你作为数据工程师经常要做的任务。在这里，我们将探讨一些这样的库，帮助你完成以下任务：

+   使用 API

+   网络爬取

+   连接数据库

+   工作流编排

+   批处理和流处理

让我们开始吧。

## 1\. Requests

作为数据工程师，你经常需要使用 API 来提取数据。[Requests](https://requests.readthedocs.io/en/latest/) 是一个 Python 库，可以让你从 Python 脚本中发出 HTTP 请求。使用 Requests，你可以从 RESTful API 获取数据，抓取网页，向服务器端点发送数据，等等。

这里是为什么 Requests 在数据专业人士和开发者中如此受欢迎的原因：

+   Requests 提供了一个简单直观的 API，用于发出 HTTP 请求，支持 GET、POST、PUT 和 DELETE 等各种 HTTP 方法。

+   它处理诸如身份验证、cookies 和会话等功能。

+   它还支持 SSL 验证、超时和连接池等功能，以实现与 web 服务器的稳健高效的通信。

要开始使用 Requests，请查看 [快速入门](https://requests.readthedocs.io/en/latest/user/quickstart/) 页面和官方文档中的 [高级用法](https://requests.readthedocs.io/en/latest/user/advanced/) 指南。

## 2\. BeautifulSoup

作为数据专业人士（无论是数据科学家还是数据工程师），你应该能够通过编程方式爬取网页以收集数据。[BeautifulSoup](https://beautiful-soup-4.readthedocs.io/en/latest/) 是最广泛使用的 Python 库之一，可用于解析和浏览 HTML 和 XML 文档。

让我们列出 BeautifulSoup 的一些特性，这些特性使它成为网络爬取任务的绝佳选择：

+   BeautifulSoup 提供了一个简单的 API 来解析 HTML 文档。你可以根据标签、属性和内容进行搜索、过滤和提取数据。

+   它支持各种解析器，包括 lxml 和 html5lib——为不同的使用场景提供性能和兼容性选项。

从导航解析树到仅解析文档的一部分，[docs](https://beautiful-soup-4.readthedocs.io/en/latest/) 提供了使用 BeautifulSoup 时可能需要执行的所有任务的详细指南。

一旦你对 BeautifulSoup 熟悉后，你也可以探索 [Scrapy](https://scrapy.org/) 进行网页抓取。对于大多数网页抓取任务，你通常会将 Requests 与 BeautifulSoup 或 Scrapy 一起使用。

## 3\. Pandas

作为数据工程师，你将定期处理数据操作和转换任务。[Pandas](https://pandas.pydata.org/) 是一个流行的 Python 库，用于数据操作和分析。它提供了数据结构和一套必要的功能，用于高效地清理、转换和分析数据。

这就是为什么 pandas 在数据专业人员中受欢迎的原因：

+   它支持读取和写入多种格式的数据，如 CSV、Excel、SQL 数据库等。

+   如前所述，pandas 还提供了用于过滤、分组、合并和重塑数据的函数。

Derek Banas 在 YouTube 上的 [Pandas Tutorial: Pandas Full Course](https://www.youtube.com/watch?v=PcvsOaixUh8) 是一个全面的教程，可以帮助你熟悉 pandas。你还可以查看 [7 Steps to Mastering Data Wrangling with Python and Pandas](/7-steps-to-mastering-data-wrangling-with-pandas-and-python) ，了解如何掌握 pandas 的数据操作技巧。

一旦你对 pandas 熟悉后，根据需要扩展数据处理任务，你可以探索 [Dask](https://www.dask.org/)。Dask 是一个灵活的并行计算库，在 Python 中，支持在集群上进行并行计算。

## 4\. SQLAlchemy

在作为数据工程师的工作日中，与数据库打交道是最常见的任务之一。[SQLAlchemy](https://www.sqlalchemy.org/) 是一个 SQL 工具包和一个 Python 的对象关系映射（ORM）库，使得与数据库的操作变得简单。

SQLAlchemy 的一些关键功能包括：

+   一个强大的 ORM 层，允许将数据库模型定义为 Python 类，属性映射到数据库列。

+   允许从 Python 中编写和运行 SQL 查询。

+   支持多种数据库后端，包括 PostgreSQL、MySQL 和 SQLite——在不同数据库间提供一致的 API。

你可以查看 SQLAlchemy 文档中的详细参考指南，了解 [ORM](https://docs.sqlalchemy.org/en/20/orm/) 和 [连接和模式管理](https://docs.sqlalchemy.org/en/20/core/) 等功能。

如果你主要使用 PostgreSQL 数据库，你可能想学习使用 [Psycopg2](https://www.psycopg.org/docs/)，这是 Python 的 Postgres 适配器。Psycopg2 提供了一个低级接口，以便直接从 Python 代码中处理 PostgreSQL 数据库。

## 5\. Airflow

数据工程师经常处理工作流编排和自动化任务。使用 [Apache Airflow](https://airflow.apache.org/)，你可以创建、调度和监控工作流。因此，你可以用它来协调批处理作业、编排 ETL 工作流或管理任务之间的依赖关系等。

让我们回顾一下 Airflow 的一些功能：

+   使用 Airflow，你可以将工作流定义为 DAGs，调度任务，管理依赖关系，并监控工作流执行。

+   它提供了一组用于与各种系统和服务（包括数据库、云平台和数据处理框架）交互的操作符。

+   它非常可扩展，因此你可以根据需要定义自定义操作符和钩子。

[Marc Lamberti 的教程](https://marclamberti.com/blog-list/)和课程是开始学习 Airflow 的极好资源。虽然 Airflow 被广泛使用，但也有一些替代品，如 Prefect 和 Mage，你也可以进行探索。要了解更多关于 Airflow 的编排替代品的信息，请阅读 [5 Airflow Alternatives for Data Orchestration](/5-airflow-alternatives-for-data-orchestration)。

## 6\. PySpark

作为数据工程师，你需要处理需要分布式计算能力的大数据处理任务。[PySpark](https://spark.apache.org/docs/latest/api/python/index.html) 是 Apache Spark 的 Python API，Apache Spark 是一个用于处理大规模数据的分布式计算框架。

PySpark 的一些功能如下：

+   它提供了用于批处理、机器学习和图形处理等的 API。

+   它提供了像 DataFrame 和 Dataset 这样的高级抽象，用于处理结构化数据，以及 RDDs 进行更低级别的数据操作。

[PySpark 教程](https://youtu.be/_C8kWso4ne4?feature=shared) 在 freeCodeCamp 的社区 YouTube 频道是开始学习 PySpark 的一个好资源。

## 7\. Kafka-Python

Kafka 是一个流行的分布式流平台，而 [Kafka-Python](https://kafka-python.readthedocs.io/en/master/) 是一个用于从 Python 与 Kafka 交互的库。因此，当你需要处理实时数据处理和消息系统时，可以使用 Kafka-Python。

Kafka-Python 的一些功能如下：

+   提供了高级的生产者和消费者 API 用于发布和消费 Kafka 主题中的消息。

+   支持诸如消息批处理、压缩和分区等功能。

你可能不会在所有项目中都使用 Kafka。但如果你想了解更多，[docs](https://kafka-python.readthedocs.io/en/master/) 页面有有用的使用示例。

## 总结

到此为止！我们已经涵盖了一些最常用的 Python 库用于数据工程。如果你想探索数据工程，可以尝试构建端到端的数据工程项目，看看这些库实际是如何工作的。

这里有几个资源可以帮助你入门：

+   [数据工程初学者指南](/2023/07/beginner-guide-data-engineering.html)

+   [初学者免费数据工程课程](/free-data-engineering-course-for-beginners)

祝学习愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正致力于通过编写教程、操作指南、观点文章等方式学习和分享知识。Bala 还创建了引人入胜的资源概述和编程教程。

### 相关主题

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [每个数据科学家都应该知道的 10 个 Python 库](https://www.kdnuggets.com/10-python-libraries-every-data-scientist-should-know)

+   [KDnuggets 新闻，4月13日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [每个机器学习工程师都应该掌握的 5 个机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [每个 AI 工程师都应该知道的工具：实用指南](https://www.kdnuggets.com/tools-every-ai-engineer-should-know-a-practical-guide)

+   [2022 年数据科学家应该知道的 Python 库](https://www.kdnuggets.com/2022/04/python-libraries-data-scientists-know-2022.html)
