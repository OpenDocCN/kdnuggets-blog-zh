# SQL 中的可扩展随机行选择

> 原文：[`www.kdnuggets.com/2018/04/scalable-select-random-rows-sql.html`](https://www.kdnuggets.com/2018/04/scalable-select-random-rows-sql.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Pavel Tiunov，[Statsbot](https://statsbot.co/)**

![标题图像](img/65a6d842e8e568f47b9eef4a27cf66c3.png)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 部门

* * *

如果您刚刚进入大数据领域，并且还从 Google Analytics 或 Mixpanel 等工具迁移过来进行网络分析，您可能注意到了性能差异。Google Analytics 可以在几秒钟内显示预定义的报告，而在您的数据仓库中对相同数据的相同查询可能需要几分钟甚至更长时间。这样的性能提升是通过选择随机行或[sampling technique](https://en.wikipedia.org/wiki/Sampling_(statistics))实现的。让我们学习如何在 SQL 中选择随机行。

### 样本与总体

在我们开始进行抽样实现之前，值得提及一些抽样基础知识。抽样是基于从某些总体中选择个体的子集来描述该总体的属性。因此，如果您有一些事件数据，您可以选择一个唯一用户及其事件的子集来计算描述所有用户行为的指标。另一方面，如果您选择一个事件子集，它不会准确描述总体的行为。

鉴于这一事实，我们假设所有应用了抽样的数据集都有唯一的用户标识符。此外，如果您同时拥有匿名用户标识符和授权用户标识符，我们建议使用后者，因为它提供更准确的结果。

### 在 SQL 中选择随机行

有很多不同的方法可以用来选择用户子集，您可以查看这些方法中的[多种选择](https://en.wikipedia.org/wiki/Sampling_(statistics)#Sampling_methods)。我们只考虑其中最明显和最简单实现的两种方法：简单随机抽样和系统抽样。

简单随机采样可以实现为给每个用户一个唯一的编号，范围从 0 到 N-1，然后从 0 到 N-1 中选择 X 个随机数。N 表示用户的总数，X 是样本大小。尽管这种方法易于理解，但在 SQL 环境中实现起来有点棘手，主要是因为随机数生成器输出在样本大小达到数十亿时不太稳定。此外，也不清楚如何在时间上获得均匀分布的样本。

考虑到这一点，我们将使用系统采样，它可以从 SQL 实现的角度克服这些障碍。简单的系统采样可以实现为在指定间隔内从每 M 个用户中选择一个用户。在这种情况下，样本大小将等于 N / M。选择 M 个用户中的一个，同时保持样本桶间的均匀分布，是这种方法的主要挑战。让我们看看如何在 SQL 中实现这一点。

### 序列生成的用户标识符

如果你的用户 ID 是生成的严格序列的整数且没有间隙（如由 `AUTO_INCREMENT` 主键字段生成的那样），你就很幸运。在这种情况下，你可以将系统采样实现得非常简单：

```py

select * from events where ABS(MOD(user_id, 10)) = 7

```

这个 SQL 查询和下面的所有 SQL 查询都采用标准 BigQuery SQL。在这个示例中，我们从 10 个用户中选择一个，这是一个 10% 的样本。7 是采样桶的随机数，它可以是 0 到 9 之间的任何数字。我们使用 MOD 操作来创建代表在这种情况下除以 10 余数的采样桶。很容易证明，如果 `user_id` 是严格的整数序列，则当用户数量足够高时，用户计数在所有采样桶中是均匀分布的。

要估计事件数量，例如，你可以写如下内容：

```py

select count(*) * 10 as events_count from events where ABS(MOD(user_id, 10)) = 7

```

请注意这个查询中的乘以 10。考虑到我们使用了 10% 的样本，所有估计的加性度量应按 1/10% 进行缩放，以匹配实际值。

我们可以质疑这种采样方法对特定数据集的准确性。你可以通过检查采样桶内分布的均匀性来估计。为此，你可以查询如下内容：

```py

select ABS(MOD(user_id, 10)), count(distinct user_id) from events GROUP BY 1

```

让我们考虑这个查询的一些示例结果：

![](img/c62f4fe96b983935965418997009139e.png)

我们可以计算这些采样桶大小的 alpha 系数为 0.01 的均值和置信区间。该 [置信区间](https://en.wikipedia.org/wiki/Confidence_interval#Basic_steps) 将等于采样桶平均大小的 0.01%。这意味着有 99% 的概率，采样桶大小的差异不超过 0.01%。不同的度量值与这些采样桶计算的统计数据相关联但并不继承它。因此，为了计算事件计数估计的精度，你可以计算每个样本的事件计数如下：

```py

select ABS(MOD(user_id, 10)), count(*) * 10 from events GROUP BY 1

```

然后计算这些事件计数的绝对和相对置信区间，如用户计数的情况，以获得事件计数估计的精度。

### 字符串标识符和其他用户标识符

如果您有字符串用户标识符或整数，但不是严格的整数序列标识符，您需要一种均匀地将所有用户 ID 分配到不同采样桶中的方法。 [这](https://en.wikipedia.org/wiki/Hash_function#Uniformity) 可以通过哈希函数完成。并非所有哈希函数在不同情况下都能实现均匀分布。您可以检查 [smhasher](https://github.com/rurban/smhasher/tree/master/doc) 测试套件的结果，以检查特定哈希函数的效果。

例如，在 BigQuery 中，您可以使用 `FARM_FINGERPRINT` 哈希函数来准备选择样本：

```py

select * from events where ABS(MOD(FARM_FINGERPRINT(string_user_id), 10)) = 7

```

`FARM_FINGERPRINT` 可以替换为任何合适的哈希函数，例如 Presto 中的 `xxhash64`，或者甚至是 Redshift 中的 `md5` 和 `strtol` 的组合。

如同序列生成的用户标识符一样，您可以检查均匀性统计数据：

```py

select ABS(MOD(FARM_FINGERPRINT(string_user_id), 10)), count(distinct string_user_id) from events GROUP BY 1

```

### 常见陷阱

通常，采样可以将您的 SQL 查询执行时间减少 5-10 倍而不会对精度造成危害，但仍然存在需要谨慎的情况。这主要是由于样本大小引起的采样偏差问题。当您感兴趣的指标在样本之间的离散度过高时，即使样本量足够大，也可能发生这种情况。

例如，您可能对某些稀有事件计数感兴趣，比如一个 B2C 网站上的企业演示请求，尽管该网站有大量流量。如果企业演示请求的数量约为每月 100 个事件，而您的月活跃用户约为 1M，那么采样可能会导致企业演示请求数量的估计误差显著。一般而言，在这种情况下应避免在 SQL 中随机采样行，或使用 [更复杂的方法](https://en.wikipedia.org/wiki/Sampling_(statistics)#Stratified_sampling)。

和往常一样，请随时留下您的评论或 [联系我们的团队](https://statsbot.co/form-trial?utm_source=statsbotblog&utm_medium=article&utm_campaign=random_rows) 以获取帮助。

[原文](https://statsbot.co/blog/select-random-rows-sql?utm_source=kdnuggets&utm_medium=post&utm_campaign=random-select)。经许可转载。

**相关：**

+   计算客户终生价值：SQL 示例

+   业务分析的 SQL 窗口函数教程

+   SQL 客户保留分析指南

### 更多相关主题

+   [如何使用 [ ], .loc, iloc, .at… 在 Pandas 中选择行和列](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [使用 SQL + Python 构建可扩展的 ETL](https://www.kdnuggets.com/2022/04/building-scalable-etl-sql-python.html)

+   [广义且可扩展的最优稀疏决策树 (GOSDT)](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [如何使用 Apache Kafka 构建可扩展的数据架构](https://www.kdnuggets.com/2023/04/build-scalable-data-architecture-apache-kafka.html)

+   [如何在秒级处理包含百万行的 DataFrame](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

+   [将行附加到 Pandas DataFrames 的 3 种方法](https://www.kdnuggets.com/2022/08/3-ways-append-rows-pandas-dataframes.html)
