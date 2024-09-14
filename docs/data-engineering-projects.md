# 五个有趣的数据工程项目

> 原文：[https://www.kdnuggets.com/2020/03/data-engineering-projects.html](https://www.kdnuggets.com/2020/03/data-engineering-projects.html)

[评论](#comments)

**由[德米特里·里亚博伊](https://www.linkedin.com/in/dmitriy-ryaboy/)，Zymergen 的软件工程副总裁**。

最近数据工程领域活动频繁，过去几年涌现出了大量有趣的项目和想法。这篇文章是对我认为数据工程师需要了解的五个项目的介绍。

### DBT

![](../Images/3d4f5f9c0fe981793933a5ca46fb4a12.png)

DBT，或称“数据构建工具”，是对一个本质上简单的想法的清晰且执行良好的实现：用于执行重要工作的 SQL 语句应进行版本控制，并且最好能轻松参数化，可能相互引用。DBT 针对的是“数据分析师”，而不是数据工程师（尽管没有理由数据工程师不能使用它）。一切都在 SQL 中完成（嗯，还有 YAML）。

通过清晰地构建项目的布局、查询如何引用其他查询的工作方式以及配置中需要填充的字段，DBT 强制执行了许多优秀的实践，并大大改进了经常是混乱的工作流程。有了这些功能，它不仅可以运行你的工作流，还可以生成文档、帮助你运行验证和测试查询、通过包共享代码等。它提供了一个温和的上手方式，并为分析师提供了许多强大的工具，如果她选择利用这些工具，就可以充分发挥作用——如果不利用，就可以不干涉。

如果你或你生活中的某个人经常使用 SQL，他们需要了解一下 DBT。

### Prefect

![](../Images/426e5b49e7ef2f1bf56d29090406bd29.png)

Prefect 是一个新兴的 AirFlow 挑战者：[另一个数据管道管理器](https://github.com/pditommaso/awesome-pipeline)，它帮助设置 DAG 过程、参数化它们、适当响应错误条件、创建时间表和处理触发器等。如果你忽略它们稍显傲慢的营销（显然，Prefect 已经是“数据流自动化的全球领导者”，而 Airflow 只是一个“历史上重要的工具”），Prefect 有几个值得称道的地方，赢得了大量用户的赞誉：

+   它将实际的数据流与作业管理的调度/触发方面清晰分开，使得反向填充、临时运行、并行工作流实例等变得更容易实现。

+   它具有一个 neat 的功能性（以及类似 Airflow 的命令式风格）[API](https://docs.prefect.io/core/concepts/flows.html#apis)来创建 DAG。

+   它避免了 Airflow 的 XCom 陷阱，即通过一种奇怪的侧通道在任务之间传递数据，这种方法偶尔会出现问题，而是依赖于透明的（除了*在它出问题的时候*）序列化和个别任务的显式输入/输出。

+   它使得 [处理参数](https://docs.prefect.io/core/concepts/parameters.html)

你可以在 Airflow 中完成所有这些工作，但 Prefect 团队认为他们的 API 提供了更干净和直观的方式来解决这些及其他挑战。他们似乎赢得了不少赞同者。

### Dask

![](../Images/0b1d18ab2f26894dfe46c8345561261a.png)

人们还在忽视 Dask 吗？停止忽视 Dask 吧。

“[Dask](https://dask.org/) 是一个‘灵活的 Python 并行计算库’。如果你经常用 Python 进行数据工作，主要使用 NumPy / Scikit-learn / Pandas，你可能会发现引入 Dask 可以让事情变得非常顺畅。它轻量且快速，无论是在单机还是集群上都表现良好，它与 [RAPIDS](https://rapids.ai/dask.html) 协同工作，以获得 GPU 加速，并且相对于将你的 Python 代码迁移到 PySpark，可能会更容易进行扩展。他们有一份意外平衡的文档，讨论了 Dask 与 Spark 的优缺点，详细信息见 [https://docs.dask.org/en/latest/spark.html](https://docs.dask.org/en/latest/spark.html)。”

### DVC

![](../Images/91351f50fee592559d2d048429aa68aa.png)

DVC 代表“数据版本控制”。这个项目邀请数据科学家和工程师进入一个受 Git 启发的世界，在这里，所有工作流版本都被跟踪，以及所有数据工件和模型，还有相关的指标。

说实话，我对“数据的 git”以及各种自动化数据/工作流版本控制方案持有一些怀疑态度：我过去看到的各种方法要么过于局部而不够有用，要么需要数据科学家在工作方式上做出过于剧烈的改变，才有可能被实际采纳。因此，我忽略了，甚至有意回避了查看 DVC。现在我终于看了一下……看起来也许这个方案有前途？与分支/版本关联的指标是一个很棒的功能。将类似 git 的分支概念与训练多个模型结合，使得价值主张变得清晰。它的实现方式是使用 Git 进行代码和数据文件索引存储，同时利用可扩展的数据存储进行数据管理，通过聪明的重用来降低整体存储成本，看起来是合理的。他们在 [https://dvc.org/doc/understanding-dvc](https://dvc.org/doc/understanding-dvc) 上提到的很多内容都很真实。Thoughtworks 使用 DVC 作为他们的演示工具来 [讨论“CD4ML”](https://martinfowler.com/articles/cd4ml.html)。

另一方面，我并不特别热衷于将管道定义交给 DVC —— Airflow 或 Prefect 或其他许多工具似乎在这方面提供了更多。对互联网资源的随意浏览揭示了多次提到将 DVC 与 MLFlow 或其他工具一起使用，但尚不清楚这如何运作以及会放弃什么。

不过，DVC 是每当出现“数据的 git”或“ML 的 git”问题时总会被提到的技术。它绝对值得关注和留意。

### Great Expectations

![](../Images/a08bbfba2cd52652d4134822e076bac9.png)

Great Expectations 是一个非常好的 Python 库，允许你声明规则，期望某些数据集符合这些规则，并在遇到（生成或消费）这些数据集时进行验证。这些期望可能包括 *expect_colum_values_to_match_strftime_format 或 expect_column_distinct_values_to_be_in_set*。

将这些视为数据断言并没有错。期望可以使用多种常见的数据计算环境（Spark、SQL、Pandas）进行评估，并且可以与包括上述的 DBT 和 Prefect（当然还有 Airflow）在内的各种工作流引擎无缝集成。他们文档中的 [介绍](https://docs.greatexpectations.io/en/latest/intro.html) 和 [期望词汇表](https://docs.greatexpectations.io/en/latest/expectation_glossary.html) 部分相当自解释。

除了提供定义和验证这些断言的方法外，Great Expectations 还提供了自动化数据分析工具，*生成期望值并清理 HTML 数据文档*。这有多酷？

这不是一个完全新颖的想法，但它似乎执行得很好，且该库正在获得关注。

### 奖励环节

也许这是后 Hadoop 时代的影响，也许是云计算的原因，也许只是因为 Python 终于有了类型提示，但确实很难将有趣的项目列表缩小到五个。这里是我个人非常希望花时间研究的一些项目，按照字母顺序排列，希望你这个坚持到现在的读者也会喜欢：

+   [Amundsen](https://eng.lyft.com/open-sourcing-amundsen-a-data-discovery-and-metadata-platform-2282bb436234) 是来自 Lyft 的一个有趣的“数据发现和元数据平台”。每个自尊心强的科技独角兽似乎现在都有这样一个平台。我们能否停下来选出一个赢家？

+   [Cadence](http://cadenceworkflow.io/) 是一个“容错的有状态代码平台”，换句话说，就是一种将有关函数中长期状态的常见问题外包给其他人的方法。无论如何，抽时间观看这个视频，考虑一下它在你生活中的应用： [https://www.youtube.com/watch?v=llmsBGKOuWI](https://www.youtube.com/watch?v=llmsBGKOuWI)

+   [Calcite](https://calcite.apache.org/) 是解构数据库的核心，提供 SQL 解析器、数据库无关的查询执行规划器和优化器等功能。它 [可以在这里找到](https://calcite.apache.org/docs/powered_by.html)，出现在许多支持 SQL 的“大数据”项目中（Hive、Flink、Drill、Phoenix 等）。

+   [Dagster](https://github.com/dagster-io/dagster) 是 GraphQL 的创始人开发的数据工作流引擎，旨在以 GraphQL 对前端工程师的影响方式来改变数据工程师的开发体验。这是个好东西，可能值得单独写一篇文章。

+   [Json-Schema](https://json-schema.org/) 并不是什么新东西，但出于某种原因，人们似乎不知道它的存在。它存在，它在不断发展，你应该定义和验证你的架构。这里有规格，这里有工具，你可以将它应用于现有的JSON API中，而不必羡慕Avro/Thrift/Proto。

[原文](https://medium.com/@squarecog/five-interesting-data-engineering-projects-48ffb9c9c501)。已获授权转载。

**相关：**

+   [成为数据工程师的7个资源](https://www.kdnuggets.com/2020/01/resources-become-data-engineer.html)

+   [数据科学与数据工程之间的微妙界限](https://www.kdnuggets.com/2019/09/thin-line-between-data-science-data-engineering.html)

+   [数据工程初学者指南 – 第一部分](https://www.kdnuggets.com/2018/01/beginners-guide-data-engineering-1.html)

### 了解更多此主题

+   [Python上下文管理器的三个有趣用法](https://www.kdnuggets.com/3-interesting-uses-of-python-context-managers)

+   [KDnuggets™ 新闻 22:n03, 1月19日：深入探讨13个数据…](https://www.kdnuggets.com/2022/n03.html)

+   [五步成为数据科学专业人士](https://www.kdnuggets.com/2022/03/become-data-science-professional-five-steps.html)

+   [数据科学面试中你应该知道的五个SQL窗口函数](https://www.kdnuggets.com/2022/01/top-five-sql-window-functions-know-data-science-interviews.html)

+   [有效数据科学经理的五个标志](https://www.kdnuggets.com/2022/06/five-signs-effective-data-science-manager.html)

+   [在Pandas中进行条件筛选的五种方法](https://www.kdnuggets.com/2022/12/five-ways-conditional-filtering-pandas.html)
