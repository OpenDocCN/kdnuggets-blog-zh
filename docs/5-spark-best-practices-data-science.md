# 5 个 Apache Spark 数据科学最佳实践

> 原文：[https://www.kdnuggets.com/2020/08/5-spark-best-practices-data-science.html](https://www.kdnuggets.com/2020/08/5-spark-best-practices-data-science.html)

[评论](#comments)

**由[Zion Badash](https://www.linkedin.com/in/zion-badash-a73826a1/?originalSubdomain=il)提供，Wix.com的数据科学家**

![图](../Images/8ed183f322d32458e9f9effc15e95c27.png)

图片由[chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 为什么转向 Spark？

尽管我们都在[talk about Big Data](https://www.facebook.com/dan.ariely/posts/904383595868)，但通常在你的职业生涯中需要一些时间才能遇到它。对我来说，在[Wix.com](https://www.wix.com/)这个过程比我想象的要快，拥有超过 1.6 亿用户会生成大量数据——这也带来了扩展我们数据处理的需求。

虽然还有其他选项（例如，[Dask](https://dask.org/)），但我们决定选择 Spark，主要有两个原因——（1）它是目前的技术前沿，并广泛用于大数据。（2）我们已有了需要的 Spark 基础设施。

### 如何为熟悉 pandas 的人编写 PySpark

你可能对 pandas 很熟悉，当我说熟悉时，我的意思是非常流利，你的母语 :)

接下来讲座的标题一语道破——[Data Wrangling with PySpark for Data Scientists Who Know Pandas](https://databricks.com/session/data-wrangling-with-pyspark-for-data-scientists-who-know-pandas)，这是一个很棒的讲座。

现在正是指出，仅仅把语法弄对可能是个好的开始，但要成功完成 PySpark 项目，你需要更多的知识，你需要了解 Spark 是如何工作的。

> 让 Spark 正常工作是很困难的，但当它工作时——它表现非常好！

### Spark概述

我这里只会讲到一部分，但我建议访问以下文章，阅读 MapReduce 解释，以获得更全面的解释——[The Hitchhikers guide to handle Big Data using Spark](https://towardsdatascience.com/the-hitchhikers-guide-to-handle-big-data-using-spark-90b9be0fe89a)。

我们在这里要理解的概念是**水平扩展**。

**垂直扩展**更容易。如果我们有一个运行良好的 pandas 代码，但数据变得太大，我们可以考虑迁移到具有更多内存的更强大机器上，希望它能处理。这意味着我们仍然有一台机器处理所有数据——我们进行了**垂直扩展**。

如果我们决定使用 MapReduce，将数据拆分成块，并让不同的机器处理每个块——我们就是在进行**水平扩展**。

### 5 个 Spark 最佳实践

这些是帮助我将运行时间减少 10 倍并扩展项目的 5 个 Spark 最佳实践。

### 1 - 从小做起 —— 采样数据

如果我们想让大数据发挥作用，首先要使用小块数据来确认我们在正确的方向上。在我的项目中，我对 10% 的数据进行了采样，并确保管道正常工作，这使我能够在 Spark UI 中使用 SQL 部分，并观察整个流程中的数字增长，同时不会等待太长时间。

根据我的经验，如果你在小样本上达到了所需的运行时间，通常可以比较容易地进行扩展。

### 2 - 理解基础知识 —— 任务、分区、核心

这可能是使用 Spark 时最重要的事情：

> **1 个分区对应 1 个任务，运行在 1 个核心上**

你必须始终注意分区的数量——跟踪每个阶段的任务数量，并将其与 Spark 连接中的核心数量匹配。以下是一些提示和经验法则来帮助你做到这一点（所有这些都需要用你的情况进行测试）：

+   任务与核心的比例应为每个核心 2–4 个任务。

+   每个分区的大小应为 200MB–400MB，这取决于每个工作节点的内存，按需进行调整。

### 3 - 调试 Spark

Spark 使用延迟计算，这意味着它在执行计算指令图之前会等待操作的调用。操作的例子有 `show(), count(),...`

这使得很难理解代码中哪里有错误或需要优化的地方。我发现有用的一种实践是通过使用 `df.cache()` 将代码分成几个部分，然后使用 `df.count()` 强制 Spark 计算每个部分的 df。

现在，使用 Spark UI 你可以查看每个部分的计算并发现问题。需要注意的是，如果没有使用我们在（1）中提到的采样，这种做法可能会导致非常长的运行时间，这将很难调试。

### 4 - 查找和解决倾斜

让我们从定义数据倾斜开始。正如我们提到的，我们的数据被分成多个分区，并且在转换过程中每个分区的大小可能会发生变化。这可能导致分区之间的大小差异很大，这意味着我们的数据存在倾斜。

查找倾斜可以通过查看 Spark UI 中的阶段详细信息，并寻找最大值和中位数之间的显著差异来完成：

![图像](../Images/4733ff757dca1d055ee94dac6d0ec4d2.png)

大的方差（中位数=3秒，最大值=7.5分钟）可能暗示数据中的倾斜

这意味着我们有一些任务显著比其他任务慢。

为什么这不好——这可能导致其他阶段等待这些少量任务，而让核心空闲等待而无所事事。

如果你知道倾斜的来源，最好直接解决并更改分区。如果不知道 / 没有直接解决的选项，可以尝试以下方法：

**调整任务和核心之间的比例**

如我们所提到的，通过让任务数量多于核心数量，我们希望在较长任务运行时，其他核心能继续忙于其他任务。虽然这是真的，但前面提到的比例（2-4:1）无法真正解决任务持续时间之间的巨大差异。我们可以尝试将比例提高到 10:1 看看是否有效，但这种方法可能还有其他缺点。

**对数据进行 Salting**

Salting 是使用随机键重新分区数据，以使新的分区平衡。以下是 PySpark 的代码示例（使用 groupby，这通常是导致倾斜的罪魁祸首）：

### 5 - Spark 中迭代代码的问题

这是一个真正棘手的问题。正如我们提到的，Spark 使用延迟计算，因此在运行代码时——它只是构建了一个计算图，一个 DAG。但当你有一个迭代过程时，这种方法可能非常有问题，因为 DAG 重新打开先前的迭代并变得非常大，我是说非常非常大。这可能对驱动程序来说太大，无法保存在内存中。这个问题很难定位，因为应用程序被卡住了，但它在 Spark UI 中显示为没有作业运行（这是真的）很长时间——直到驱动程序最终崩溃。

目前这是 Spark 的一个固有问题，我使用的解决方法是每 5-6 次迭代使用 `df.checkpoint() / df.localCheckpoint()`（通过实验找到你的数字）。之所以有效，是因为 `checkpoint()` 打破了血统和 DAG（不同于 `cache()`），保存结果并从新的检查点开始。缺点是，如果发生了不好的情况，你没有完整的 DAG 来重新创建 df。

### 总结

正如我之前说的，学习如何让 Spark 发挥魔力需要时间，但这 5 个实践确实推动了我的项目，并在我的代码中洒上了一些 Spark 魔力。

总之，这是我在开始项目时寻找的（但未找到）的文章——希望你正好找到了。

### 参考文献

+   [针对掌握 Pandas 的数据科学家的 PySpark 数据清理](https://databricks.com/session/data-wrangling-with-pyspark-for-data-scientists-who-know-pandas)

+   [使用 Spark 处理大数据的指南](https://towardsdatascience.com/the-hitchhikers-guide-to-handle-big-data-using-spark-90b9be0fe89a)

+   [Spark: 权威指南](https://www.oreilly.com/library/view/spark-the-definitive/9781491912201/) — 第18章关于监控和调试的内容非常精彩。

**简介: [Zion Badash](https://www.linkedin.com/in/zion-badash-a73826a1/?originalSubdomain=il)** 是 Wix.com 的预测团队数据科学家。他的兴趣包括机器学习、时间序列、Spark 以及相关领域的一切。

[原文](https://towardsdatascience.com/5-spark-best-practices-61587a35ac15)。经授权转载。

**相关:**

+   [使用 Apache Spark 和 PySpark 的好处及示例](/2020/04/benefits-apache-spark-pyspark.html)

+   [Apache Spark 集群在 Docker 上](/2020/07/apache-spark-cluster-docker.html)

+   [Dataproc 上的 Apache Spark 与 Google BigQuery](/2020/07/apache-spark-dataproc-vs-google-bigquery.html)

### 了解更多相关话题

+   [将 ChatGPT 融入数据科学工作流程: 提示和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [数据科学团队协作的 5 个最佳实践](https://www.kdnuggets.com/2023/06/5-best-practices-data-science-team-collaboration.html)

+   [数据科学中的 5 个 Python 最佳实践](https://www.kdnuggets.com/5-python-best-practices-for-data-science)

+   [迁移到 AWS 云的 11 个最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [数据仓储和 ETL 最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [数据可视化最佳实践与有效沟通资源](https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html)
