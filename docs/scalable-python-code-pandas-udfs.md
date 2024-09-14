# 使用Pandas UDFs的可扩展Python代码：数据科学应用

> 原文：[https://www.kdnuggets.com/2019/06/scalable-python-code-pandas-udfs.html](https://www.kdnuggets.com/2019/06/scalable-python-code-pandas-udfs.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Ben Weber](https://www.linkedin.com/in/ben-weber-3b87482/)，Zynga的数据科学家及Mischief顾问**

![figure-name](../Images/9c97c2e4ab5c7ae71e54f8cb66b86416.png)来源：[https://pxhere.com/en/photo/1417846](https://pxhere.com/en/photo/1417846)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织进行IT管理

* * *

PySpark是一个非常强大的工具，因为它能够让Python代码从单台机器扩展到大型集群。虽然像MLlib这样的库提供了对数据科学家在这种环境中可能想要执行的标准任务的良好支持，但Python库提供了在分布式环境中未配置的广泛功能。虽然像 [Koalas](https://github.com/databricks/koalas) 这样的库应该可以更容易地将Python库移植到PySpark中，但在可扩展运行时所需的库集合与支持分布式执行的库之间仍然存在差距。这篇文章讨论了如何使用Spark 2.3+中的 [Pandas UDFs](https://databricks.com/blog/2017/10/30/introducing-vectorized-udfs-for-pyspark.html) 功能来弥合这一差距。

我遇到了Pandas UDFs，因为我需要一种方法来扩展我在Zynga开发的项目的自动特征工程。我们有几十款游戏，事件分类各异，需要一种自动化方法来为不同模型生成特征。计划是使用 [Featuretools](https://github.com/Featuretools/featuretools) 库来完成这一任务，但面临的挑战是它仅支持在单台机器上的Pandas。我们的用例需要扩展到大型集群，并且我们需要以并行和分布式模式运行Python库。我在2019年Spark峰会上展示了我们实现这一规模的方法。

我们采取的方法是首先在Spark集群中的驱动节点上使用数据样本执行任务，然后使用Pandas UDFs扩展到完整的数据集，以处理数十亿条记录。我们在建模管道的特征生成步骤中使用了这种方法。这种方法也可以应用于数据科学工作流中的不同步骤，并且可以用于数据科学之外的领域。我们在以下Medium文章中对我们的方法进行了深入探讨：

[**Zynga的Portfolio-Scale机器学习**

*自动化处理数十款游戏的预测建模*medium.com](https://medium.com/zynga-engineering/portfolio-scale-machine-learning-at-zynga-bda8e29ee561)

这篇文章通过一个例子展示了如何使用Pandas UDFs扩展批量预测管道的模型应用步骤，但UDFs的使用案例远比博客中涵盖的要广泛。

### 数据科学应用

Pandas UDFs可以用于数据科学中的各种应用，从特征生成到统计测试再到分布式模型应用。然而，这种扩展Python的方法并不限于数据科学，只要你可以将数据编码为数据框并将任务分解为子问题，它可以应用于各种领域。为了展示Pandas UDFs如何用于扩展Python代码，我们将通过一个示例演示，其中批处理过程用于创建购买可能性模型，首先使用单台机器，然后使用集群扩展到可能的数十亿条记录。此帖子的完整源代码可在[github](https://github.com/bgweber/StartupDataScience/blob/master/python/SK_Scale.ipynb)上获取，我们将使用的库已预安装在Databricks社区版中。

我们笔记本中的第一步是加载我们将用于执行分布式模型应用的库。我们需要Pandas来加载数据集和实现用户定义的函数，需要sklearn来构建分类模型，以及用于定义UDF的pyspark库。

接下来，我们将加载一个数据集以构建分类模型。在这个代码片段中，一个CSV文件被急切地加载到内存中，使用Pandas的`read_csv`函数，然后转换为Spark数据框。代码还为每条记录附加了一个唯一ID和一个用于分发数据框的分区ID。

该步骤的输出见下表。Spark数据框是记录的集合，每条记录指定用户是否以前购买过目录中的一组游戏，标签指定用户是否购买了新游戏发行，user_id和partition_id字段是使用上面代码片段中的Spark SQL语句生成的。

![figure-name](../Images/5fb756e02f53f580bd1a8f9fca103b6a.png)

现在我们有一个 Spark 数据框，可以用来执行建模任务。然而，对于这个例子，我们将专注于当将数据集的样本拉取到驱动节点时可以执行的任务。运行 `toPandas()` 命令时，整个数据框会被急切地提取到驱动节点的内存中。由于我们处理的是一个小数据集，这种做法没问题。但在使用 toPandas 函数之前，最好先对数据集进行采样。一旦将数据框拉取到驱动节点后，我们可以使用 sklearn 来构建逻辑回归模型。

只要你的完整数据集能够适应内存，你可以使用下面展示的单机方法将 sklearn 模型应用于新的数据框。然而，如果需要对数百万或数十亿条记录进行评分，这种单机方法可能会失败。

这一阶段的结果是一个包含用户 ID 和模型预测的数据框。

![figure-name](../Images/eb8ffafef48ebd22b3285de170ac448f.png)

在笔记本的最后一步，我们将使用 Pandas UDF 来扩展模型应用过程。我们可以使用 Pandas UDF 将数据集分布到 Spark 集群中，而不是将整个数据集拉取到驱动节点的内存中，并使用 pyarrow 在 Spark 和 Pandas 数据框表示之间进行转换。结果与上面的代码片段相同，但在这种情况下，数据框分布在集群的工作节点上，任务在集群上并行执行。

结果与之前相同，但计算现在已从驱动节点转移到工作节点集群。这个过程的输入和输出是一个 Spark 数据框，即使我们使用 Pandas 在 UDF 内执行任务。

![figure-name](../Images/b1c4e6103b662858295fbdb9769d5e07.png)

有关设置 Pandas UDF 的更多详细信息，请查看我之前关于如何开始使用 PySpark 的帖子。

[**PySpark 简介**](https://towardsdatascience.com/a-brief-introduction-to-pyspark-ff4284701873)

*PySpark 是一种用于大规模执行探索性数据分析、构建机器学习管道的优秀语言，等等* [towardsdatascience.com](https://towardsdatascience.com/a-brief-introduction-to-pyspark-ff4284701873)

这是一个介绍，展示了如何将 sklearn 处理从 Spark 集群的驱动节点移动到工作节点。我还使用了这个功能，将 [Featuretools](https://github.com/Featuretools/featuretools) 库扩展到处理数十亿条记录并创建数百个预测模型。

### 结论

Pandas UDFs 是一种功能，允许 Python 代码在分布式环境中运行，即使该库最初是为单节点执行开发的。数据科学家在构建可扩展的数据管道时可以受益于这一功能，但许多不同领域也可以从这一新功能中获益。我提供了一个批处理模型应用的示例，并链接到一个使用 Pandas UDFs 进行自动化特征生成的项目。UDFs 的许多应用尚未被探索，现在为 Python 开发者提供了新的计算规模。

**简介：[Ben Weber](https://www.linkedin.com/in/ben-weber-3b87482/)** 是 Zynga 的杰出数据科学家以及 Mischief 的顾问。

[原文](https://towardsdatascience.com/scalable-python-code-with-pandas-udfs-a-data-science-application-dd515a628896)。已获得许可转载。

**相关：**

+   [Optimus v2：简化的数据科学工作流](/2018/08/optimus-v2-agile-data-science-workflows-made-easy.html)

+   [Swiftapply  – 自动化高效的 pandas apply 操作](/2018/04/swiftapply-automatically-efficient-pandas-apply-operations.html)

+   [使用 Apache Spark 进行深度学习：第 1 部分](/2018/04/deep-learning-apache-spark-part-1.html)

### 更多相关主题

+   [使用 SQL + Python 构建可扩展的 ETL](https://www.kdnuggets.com/2022/04/building-scalable-etl-sql-python.html)

+   [如何利用 Apache Kafka 构建可扩展的数据架构](https://www.kdnuggets.com/2023/04/build-scalable-data-architecture-apache-kafka.html)

+   [通用且可扩展的最优稀疏决策树（GOSDT）](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [使用 Python 创建从音频中提取主题的 Web 应用](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [使用 Google Earth 在 Python 中构建地理空间应用…](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [用 Python 在 10 个简单步骤中构建 AI 应用](https://www.kdnuggets.com/build-an-ai-application-with-python-in-10-easy-steps)
