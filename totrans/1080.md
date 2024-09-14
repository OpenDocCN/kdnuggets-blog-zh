# 从Oracle到AI数据库：数据存储的演变

> 原文：[https://www.kdnuggets.com/2022/02/oracle-databases-ai-evolution-data-storage.html](https://www.kdnuggets.com/2022/02/oracle-databases-ai-evolution-data-storage.html)

![从Oracle到AI数据库：数据存储的演变](../Images/6c896d3ad813766531a27e53c5468052.png)

[技术矢量图由fullvector创作 - www.freepik.com](https://www.freepik.com/vectors/technology)

尽管机器学习已经变得商品化，但它仍然是西部荒野。各个行业的ML团队正在开发自己的数据处理、模型训练和生产使用的技术。这显然不是一个可持续的机器学习方法。随着时间的推移，这些多样化的方法将会标准化。为了加快这一过程，行业需要专门为AI设计的开发者工具。在本文中，你将看到传统的数据存储解决方案与专为解决AI用例而构建的数据库之间的区别。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

任何与企业打过交道的人都见过一个具有行和列的界面，在那里你认真地添加与工作相关的信息。

Oracle数据库在40多年前作为这种数据输入的目标被引入。尽管许多企业将其作为默认数据库使用，但它对于普通用户来说相当复杂。它也是专有的，因此对开发者不可访问。Oracle甚至启动了DeWitt条款，禁止发布未经数据库创建者授权的数据库基准测试。

快进15年，我们见证了像MySQL和PostgreSQL这样的新一代数据库的出现。与Oracle的产品相比，主要的区别在于可访问性——这些新型数据库是开源产品，可以在本地计算机上运行，这使得开发者更容易进行设置和使用。它们在互联网泡沫期间得到了广泛采用也就不足为奇了。

## **Web 2.0和开源数据库的崛起**

随着时间的推移，我们见证了2000年代初“Web 2.0”的兴起。Google 的 Jeff Dean 和 Sanjay Ghemawat 提出了 MapReduce，一种根本性的新数据处理方法。该方法的逻辑是对数据集执行映射和归约操作。映射操作包括对数据行和列的过滤和排序。归约操作涉及对表格数据的汇总和聚合。该基础设施被设计为并行运行。这种 MapReduce 方法是 Hadoop 的核心，Hadoop 是2000年代最成功的开源项目之一。

2007-2012年间，世界迎来了各种开源 NoSQL 数据库。最受欢迎的有：Apache Cassandra、Google 的 BigTable、MongoDB、Redis 和 Amazon 的 DynamoDB。

这些数据库的关键区分点在于它们水平扩展良好。许多机器可以并行启动并协作服务一个能够容纳真正大规模数据集的数据库。NoSQL 数据库不使用关系型的方法将数据建模为行和列，且可以轻松接受非结构化数据，而不需要复杂的数据模型。

还需要提到的是，有两种类型的数据库：事务型和分析型。事务型数据库是操作性的，存储关键数据，如用户日志、活动、IP 地址、采购订单等。它们被设计为能够快速接受新数据，并以最小的延迟响应数据请求。分析型数据库工作速度较慢，可以托管在更便宜的存储上，并允许对数据进行比在事务型数据库中更深入的分析。

## **大数据：分析与人工智能炒作周期的开端**

“大数据”这一术语最初在2005年引入，但在应用程序变得更加复杂后才被广泛接受。当时，适当的分析涉及从不同地方提取大量数据。为了应对不断上升的挑战，市场上出现了新产品——Amazon RedShift、Snowflake、Google BigQuery、Clickhouse。这些数据仓库已经成为现代企业的主流。

2020年引入了一种新的数据协调解决方案——‘湖屋’。湖屋帮助用户以低成本查询历史数据，但在分析和机器学习工作负载方面几乎没有性能下降。这转变成了[Snowflake和Databricks之间](https://blocksandfiles.com/2021/11/15/snowflake-rebuts-databricks-snowflake-performance-comparison/)的一场军备竞赛。虽然它们在表格数据的世界中继续争斗，但一些重大变化正在深处酝酿。

在2021年，深度学习（DL）已经成为主流。这是非结构化数据的时代。与以行和列表示的表格数据不同，非结构化数据包括图像、视频流、音频和文本。这些数据无法以电子表格形式表示。

由于去年的封锁，Netflix、TikTok以及其他社交媒体的消费量飙升至前所未有的水平。这些产品都处理和生成大量的非结构化数据。公司应该如何管理这些数据，尤其是当他们希望通过AI来挖掘深层次的见解时？AI团队在处理这些数据时是否可以使用数据库解决方案？

你可能会感到惊讶，但我们讨论的技术没有一个能完全满足深度学习从业者的需求。像特斯拉这样的公司，处理大量非结构化数据，[承认他们不得不构建自己的基础设施](https://youtu.be/hx7BXih7zx8)。

很少有企业拥有足够的内部带宽从头开始构建数据基础设施。AI行业需要一个坚实的数据基础和专门为AI设计的数据库。

## **AI数据库是深度学习创新的支柱**

长期以来，行业一直需要[一个简单的API来创建](https://github.com/activeloopai/Hub)、存储和协作处理任何大小的AI数据集。更重要的是，这个工具应该专门为深度学习相关任务创建。

传统数据库将数据存储在内存中以快速执行查询，但内存存储对ML数据集而言极其昂贵，因为它们非常大。即便是使用像AWS EBS这样的附加磁盘也非常昂贵。最具成本效益的存储层是对象存储，例如S3、Google Cloud Storage和MinIO，但这些工具在快速查询或计算方面较慢。

专门为深度学习构建的数据库应该支持[在这些云存储系统上托管数据](/2021/11/design-patterns-machine-learning-pipelines.html)并以[深度学习模型原生格式存储数据](/2021/11/after-hdf5-data-storage-format-deep-learning.html)。这种功能将允许更快的计算，并且更具成本效益，因为从这种框架中访问数据不应产生任何额外的预处理成本。这样的框架将帮助现代的AI驱动企业[节省多达30%的基础设施成本。](https://activeloop-forbes.s3.us-west-2.amazonaws.com/forbes_article_-_better_aerial_data_pipelines.pdf)

该框架应优化数据从云存储传输到机器学习模型的过程，以尽量减少延迟。这将允许AI即时探索和可视化无法存放在其他地方的大规模数据集。

更重要的是，现代AI数据库应该非常易于使用，只需几行代码即可实现。[根据Kaggle最近的调查](https://www.kaggle.com/kaggle-survey-2021)，超过20%的数据科学家在行业经验上少于3年，另有14%的人经验不足一年。机器学习仍然是一个年轻的领域，简单性至关重要。

我们已经开始看到专门为 AI 使用案例构建的数据库的出现。在未来五年，我们将见证对 [机器学习基础设施堆栈的标准化](https://www.forbes.com/sites/forbestechcouncil/2021/10/08/how-to-enable-data-centric-ai/?sh=5e570ea020f8)。更快采用 AI 数据库的组织将站在机器学习革命的前沿。

**[Davit Buniatyan](https://www.linkedin.com/in/davidbuniatyan/)** (**[@DBuniatyan](https://twitter.com/dbuniatyan)**) 在20岁时开始了普林斯顿大学的博士学位。他的研究涉及在Sebastian Seung的指导下重建小鼠大脑的连接组。为了克服在神经科学实验室分析大型数据集时遇到的困难，David 成为 Activeloop 的创始首席执行官，该公司是 Y-Combinator 的毕业生初创企业。他还是戈登·吴奖学金和 AWS 机器学习研究奖的获得者。Davit 是 [Activeloop](https://www.activeloop.ai/) 的创始人，这是一个为 AI 设计的数据库。

### 更多相关话题

+   [从人工智能到机器学习的演变](https://www.kdnuggets.com/2022/08/evolution-artificial-intelligence-machine-learning-data-science.html)

+   [数据景观的演变](https://www.kdnuggets.com/2023/06/evolution-data-landscape.html)

+   [ETL中的演变：跳过转换如何提升数据管理](https://www.kdnuggets.com/evolution-in-etl-how-skipping-transformation-enhances-data-management)

+   [分析未来成功的概率与智能…](https://www.kdnuggets.com/2022/02/analyzing-probability-future-success-intelligence-node-attributes-evolution-model.html)

+   [Apache Druid 的演变](https://www.kdnuggets.com/2022/07/evolution-apache-druid.html)

+   [语音识别指标的演变](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)
