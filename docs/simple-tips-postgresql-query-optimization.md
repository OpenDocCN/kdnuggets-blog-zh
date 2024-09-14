# PostgreSQL 查询优化的简单技巧

> 原文：[`www.kdnuggets.com/2018/06/simple-tips-postgresql-query-optimization.html`](https://www.kdnuggets.com/2018/06/simple-tips-postgresql-query-optimization.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 Pavel Tiunov 撰写，[Statsbot](https://statsbot.co/)**

![图片](img/96ce8e920837a63f0a217c54b00279dd.png)

单个查询优化技巧可以将你的数据库性能提升 100 倍。曾经我们建议一位拥有 10TB 数据库的客户使用基于日期的多列索引。结果，他们的日期范围查询速度提升了 112 倍。在这篇文章中，我们分享了五个简单但仍然强大的 PostgreSQL 查询优化技巧。

虽然我们通常建议客户使用这些技巧来优化分析查询（例如聚合查询），但本文对于任何其他类型的查询仍然非常有帮助。

为了简化起见，我们在测试数据集上运行了本文的示例。虽然这不会显示实际的性能改进，但你会发现我们的建议能解决大量的优化问题，并在实际情况中表现良好。

### Explain analyze

Postgres 有一个很酷的扩展功能，称为[`EXPLAIN ANALYZE`](https://www.postgresql.org/docs/9.3/static/using-explain.html#USING-EXPLAIN-ANALYZE)，扩展了著名的`EXPLAIN`命令。不同之处在于，`EXPLAIN` 根据收集的数据库统计信息显示查询成本，而`EXPLAIN ANALYZE` 实际运行它以显示每个阶段处理的时间。

我们强烈建议使用`EXPLAIN ANALYZE`，因为有很多情况下`EXPLAIN`显示的查询成本较高，而实际执行时间则较短，反之亦然。最重要的是，EXPLAIN 命令将帮助你了解是否使用了特定的索引以及如何使用。

查看索引的能力是学习 PostgreSQL 查询优化的第一步。

### 每个查询一个索引

索引是你表的物化副本。它们仅包含表的特定列，因此你可以快速根据这些列中的值查找数据。Postgres 中的索引还存储行标识符或行地址，以加速原始表扫描。

这总是存储空间和查询时间之间的权衡，许多索引可能会引入 DML 操作的开销。然而，当读取查询性能是优先考虑的（如商业分析中的情况），这种方法通常是有效的。

我们建议为每个独特的查询创建一个索引以获得更好的性能。请继续阅读本文，了解如何为特定查询创建索引。

### 使用多个列的索引

让我们查看以下简单查询的 explain analyze 计划，未使用索引：

```py

EXPLAIN ANALYZE SELECT line_items.product_id, SUM(line_items.price)
FROM line_items
WHERE product_id > 80
GROUP BY 1
```

Explain analyze 返回：

```py

HashAggregate (cost=13.81..14.52 rows=71 width=12) (actual time=0.137..0.141 rows=20 loops=1)
Group Key: product_id
-> Seq Scan on line_items (cost=0.00..13.25 rows=112 width=8) (actual time=0.017..0.082 rows=112 loops=1)
Filter: (product_id > 80)
Rows Removed by Filter: 388
Planning time: 0.082 ms
Execution time: 0.187 ms
```

这个查询扫描所有行项，找到 id 大于 80 的产品，然后按该产品 id 对所有值进行求和。

现在我们将为这个表添加索引：

```py

CREATE INDEX items_product_id ON line_items(product_id)
```

我们创建了一个[B-树索引](https://en.wikipedia.org/wiki/B-tree)，该索引只包含一个列：`product_id`。在阅读了大量关于使用索引的好处的文章后，人们可能会期待这样的操作能提升查询性能。抱歉，坏消息。

由于我们需要在上述查询中对价格列进行求和，我们仍然需要扫描原始表。根据表的统计信息，Postgres 将选择扫描原始表而不是索引。问题在于，索引缺少`price`列。

我们可以通过添加价格列来调整此索引，如下所示：

```py

CREATE INDEX items_product_id_price ON line_items(product_id, price)
```

如果我们重新运行解释计划，我们会看到我们的索引在第四行：

```py

GroupAggregate (cost=0.27..7.50 rows=71 width=12) (actual time=0.034..0.090 rows=20 loops=1)
Group Key: product_id
-> Index Only Scan using items_product_id_price on line_items (cost=0.27..6.23 rows=112 width=8) (actual time=0.024..0.049 rows=112 loops=1)
Index Cond: (product_id > 80)
Heap Fetches: 0
Planning time: 0.271 ms
Execution time: 0.136 ms
```

但是，将价格列放在首位会如何影响 PostgreSQL 查询优化呢？

### 多列索引中的列顺序

好吧，我们发现前一个查询中使用了多列索引，因为我们包含了这两列。有趣的是，我们在定义索引时可以使用这些列的另一种顺序：

```py

CREATE INDEX items_product_id_price_reversed ON line_items(price, product_id)
```

如果我们重新运行解释分析，我们将看到`items_product_id_price_reversed`没有被使用。这是因为该索引首先按`price`排序，然后按`product_id`排序。使用这个索引将导致全索引扫描，**这几乎等同于扫描整个表**。这就是为什么 Postgres 选择对原始表进行扫描的原因。

将用于过滤器中具有最大唯一值数量的列放在第一位是一种良好的实践。

### 过滤器 + 连接

现在是时候找出特定连接查询的最佳索引集合了，该查询还有一些过滤条件。通常，你可以通过反复试验来获得最佳结果。

就像在简单过滤的情况下，选择最具限制性的过滤条件并为其添加索引。

让我们考虑一个示例：

```py

SELECT orders.product_id, SUM(line_items.price)
FROM line_items
LEFT JOIN orders ON line_items.order_id = orders.id
WHERE line_items.created_at BETWEEN '2018-01-01' and '2018-01-02'
GROUP BY 1
```

在这里，我们基于`order_id`进行连接，并在`created_at`上进行过滤。这样，我们可以创建一个多列索引，其中`created_at`排在第一位，`order_id`排在第二位，`price`排在第三位：

```py

CREATE INDEX line_items_created_at_order_id_price ON line_items(created_at, order_id, price)
```

我们将得到以下解释计划：

```py

GroupAggregate (cost=12.62..12.64 rows=1 width=12) (actual time=0.029..0.029 rows=1 loops=1)
Group Key: orders.product_id
-> Sort (cost=12.62..12.62 rows=1 width=8) (actual time=0.025..0.026 rows=1 loops=1)
Sort Key: orders.product_id
Sort Method: quicksort Memory: 25kB
-> Nested Loop Left Join (cost=0.56..12.61 rows=1 width=8) (actual time=0.015..0.017 rows=1 loops=1)
-> Index Only Scan using line_items_created_at_order_id_price on line_items (cost=0.27..4.29 rows=1 width=8) (actual time=0.009..0.010 rows=1 loops=1)
Index Cond: ((created_at >= '2018-01-01 00:00:00'::timestamp without time zone) AND (created_at <= '2018-01-02 00:00:00'::timestamp without time zone))
Heap Fetches: 0
-> Index Scan using orders_pkey on orders (cost=0.29..8.30 rows=1 width=8) (actual time=0.004..0.005 rows=1 loops=1)
Index Cond: (line_items.order_id = id)
Planning time: 0.303 ms
Execution time: 0.072 ms
```

如你所见，`line_items_created_at_order_id_price`用于通过日期条件减少扫描。之后，它使用`orders_pkey`索引扫描与订单连接。

日期列通常是多列索引中最适合作为第一列的候选者，因为它以可预测的方式减少了扫描吞吐量。

### 结论

我们的 PostgreSQL 查询优化技巧将帮助你将查询速度提高 10-100 倍，适用于多 GB 数据库。它们可以以 80/20 的方式解决大多数性能瓶颈。这并不意味着你不应该使用`EXPLAIN`来双重检查你的查询，以适应实际的场景。

[原文](https://statsbot.co/blog/postgresql-query-optimization/?utm_source=kdnuggets&utm_medium=post&utm_campaign=postgres-sql)。经许可转载。

**相关：**

+   SQL 中的随机行可扩展选择

+   SQL 窗口函数商业分析教程

+   计算客户生命周期价值：SQL 示例

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求。

* * *

### 相关话题

+   [SQL 查询优化技巧](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [提高 SQL 查询性能的 5 个技巧](https://www.kdnuggets.com/5-tips-for-improving-sql-query-performance)

+   [用 SQL 查询你的 Pandas 数据框](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [我们能用 T5 查询表格吗？](https://www.kdnuggets.com/2022/05/query-table-t5.html)

+   [使用 TPOT 进行机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)
