# 列导向数据库解析

> 原文：[https://www.kdnuggets.com/2021/02/understanding-nosql-database-types-column-oriented-databases.html](https://www.kdnuggets.com/2021/02/understanding-nosql-database-types-column-oriented-databases.html)

[评论](#comments)

NoSQL 已越来越受到欢迎，作为传统 SQL 数据库和数据库管理方法的补充工具。正如我们所知，NoSQL 不遵循 SQL 的关系模型，这使得它能够做很多强大的事情。更重要的是，它非常灵活和可扩展，这对于没有时间或预算设计 SQL 数据库的新项目非常有用。

因此，我们将深入了解不同的 [数据模型](https://hostingdata.co.uk/nosql-database/) 是如何工作的，本文将重点讨论列数据库。如果你想了解更一般的信息，应该查看我们的 [NoSQL 初学者指南](/2020/12/nosql-beginners.html)。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 列数据库是如何工作的？

从最表面上看，列存储数据库确实如其广告所述：即，它不是将信息组织成行，而是将其组织成列。这本质上使它们的功能与关系数据库中的表相同。当然，由于这是*NoSQL*数据库，这种数据模型使它们更加灵活。

![Image](../Images/8e7c96a2dd6f24ce7573a605fae19e33.png)

更具体地说，列数据库使用*键空间*的概念，这有点像关系模型中的模式。这个键空间包含所有列族，列族中包含行，行中再包含列。一开始理解起来有点棘手，但相对来说还是比较简单的。

![Image](../Images/420e731f7baa234c5e633fd8f5dabfa2.png)![Image](../Images/fbb86c445be95d40bbd68cce53eb7a2d.png)

通过快速查看，我们可以看到一个列族有多个行。在每一行中，可以有多个不同的列，具有不同的名称、链接，甚至大小（意味着它们不需要遵循标准）。此外，这些列只存在于它们自己的行中，并且可以包含值对、名称和时间戳。

以一个具体的行作为例子：

![Image](../Images/693cf6110d5044826ce975d87d07376d.png)

*行键* 就是那一行的具体标识符，并且总是唯一的。列中包含名称、值和时间戳，这些都是直接明了的。名称/值对也很简单，而时间戳是数据输入数据库的日期和时间。

一些列式存储数据库的例子包括 Casandra、CosmoDB、Bigtable 和 HBase。

### 列式数据库的好处

列式数据库有几个好处：

+   列式存储在压缩方面表现优异，因此在存储方面非常高效。这意味着你可以减少磁盘资源，同时在单列中保存大量信息。

+   由于大多数信息存储在列中，聚合查询非常快速，这对需要在短时间内进行大量查询的项目很重要。

+   列式存储数据库的可扩展性非常优秀。它们几乎可以无限扩展，通常分布在大规模的机器集群中，甚至达到几千台。这也意味着它们非常适合大规模并行处理。

+   加载时间同样优秀，你可以在几秒钟内轻松加载一个十亿行的表。这意味着你可以几乎瞬间完成加载和查询。

+   列式数据库具有很大的灵活性，因为列之间不一定要相同。这意味着你可以添加新的和不同的列而不会干扰整个数据库。也就是说，输入完全新的记录查询需要更改所有表。

总的来说，列式存储数据库非常适合分析和报告：快速的查询速度和处理大量数据而不增加过多开销的能力使其理想。

### 列式数据库的缺点

正如生活中的常态，没有什么是完美的，使用列式数据库也有一些缺点：

+   设计一个有效的索引模式既困难又耗时。即便如此，这种模式仍然不如简单的关系数据库模式有效。

+   虽然这对某些用户可能不是问题，但增量数据加载效果不佳，应该尽可能避免。

+   这适用于所有 NoSQL 数据库类型，而不仅仅是列式数据库。安全性 [web 应用中的漏洞](https://www.clouddefense.ai/blog/web-application-security) 始终存在，而 NoSQL 数据库缺乏内置的安全功能。如果安全是你的首要任务，你应该考虑使用关系数据库，或者在可能的情况下使用明确定义的模式。

+   由于数据存储的方式，在线事务处理 (OLTP) 应用程序也与列式数据库不兼容。

### 列式数据库总是 NoSQL 吗？

在结束之前，我们要指出，列式存储数据库并不一定仅仅是 NoSQL。通常的说法是，列式存储与关系数据库模型有很大不同，因此它完全归于 NoSQL 阵营。

这并不一定总是如此，[NoSQL 与 SQL](/2020/12/sql-vs-nosql-7-key-takeaways.html) 的争论总体上相当复杂。

在列存储数据库的情况下，它们与 SQL 方法论几乎相同。例如，关键空间充当模式，因此仍然存在某种形式的模式管理。另一个例子是元数据有时看起来完全像典型的关系型 DBMS。具有讽刺意味的是，列存储数据库也通常符合 SQL 和 ACID 标准。

更进一步，NoSQL 数据库通常是文档存储或键值存储，而列存储既不是。

因此，很难争论列存储完全是 NoSQL。

### 结论

尽管列存储数据库极其强大，但它们 *确实* 存在自己的一些问题。例如，它写入数据的方式意味着一致性稍有缺乏，因为列需要多次写入磁盘。这与关系型数据库中行数据的顺序写入相比。

尽管如此，列存储仍然是最常用的数据模型之一。

**简介：[亚历克斯·威廉姆斯](https://hostingdata.co.uk/author/alex-williams/)** 是一位经验丰富的全栈开发者和 [Hosting Data UK](https://hostingdata.co.uk/) 的拥有者。在伦敦大学 IT 专业毕业后，亚历克斯作为开发者领导了多个来自世界各地客户的项目，工作了近 10 年。最近，亚历克斯转型为独立 IT 顾问，并开始了自己的博客。在那里，他探讨了 web 开发、数据管理、数字营销以及为刚刚起步的在线业务主提供解决方案。

**相关：**

+   [SQL 与 NoSQL：7 个关键要点](/2020/12/sql-vs-nosql-7-key-takeaways.html)

+   [NoSQL 初学者指南](/2020/12/nosql-beginners.html)

+   [SQL 还是 NoSQL：这是个问题！](/2018/05/sql-not-sql-question.html)

### 更多相关主题

+   [键值数据库解析](https://www.kdnuggets.com/2021/04/nosql-explained-understanding-key-value-databases.html)

+   [从 Oracle 到 AI 数据库：数据存储的演变](https://www.kdnuggets.com/2022/02/oracle-databases-ai-evolution-data-storage.html)

+   [NoSQL 数据库及其用例](https://www.kdnuggets.com/2023/03/nosql-databases-cases.html)

+   [AI 和 LLM 用例中的向量数据库](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)

+   [使用向量数据库进行语义搜索](https://www.kdnuggets.com/semantic-search-with-vector-databases)

+   [Python 中 SQLite 数据库使用指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)
