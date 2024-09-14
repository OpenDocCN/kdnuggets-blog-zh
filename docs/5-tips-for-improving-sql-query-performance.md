# 提升 SQL 查询性能的 5 个技巧

> 原文：[`www.kdnuggets.com/5-tips-for-improving-sql-query-performance`](https://www.kdnuggets.com/5-tips-for-improving-sql-query-performance)

![sql-tips](img/240e20747f5fcbe8b0d0836f5b4b7b8f.png)

作者提供的图像

强大的数据库和 SQL 技能对所有数据角色都很重要。在实际工作中，你会查询超大数据库表——通常有几千甚至几百万行。这就是为什么 SQL 查询的性能成为决定应用程序整体性能的一个重要因素。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

未优化的查询通常会导致响应时间变慢、服务器负载增加以及用户体验不佳。因此，理解和应用 SQL 查询优化技术至关重要。

本教程介绍了优化 SQL 查询的实用技巧。让我们开始吧。

## 在开始之前：获取一个示例数据库表

在编写任何数据库的 SQL 查询时，可以使用以下技巧。但如果你希望使用示例数据库表来运行这些查询，可以使用[这个 Python 脚本](https://github.com/balapriyac/python-basics/blob/main/sql-tips/main.py)。

它连接到一个 SQLite 数据库：**employees.db**，创建了一个**employees**表，并填充了 10000 条记录。如前所述，你总是可以创建你自己的示例。

## 1\. 不要使用 SELECT *; 应选择特定列

初学者使用**SELECT ***来检索表中的所有列是很常见的。如果你只需要少数几列，这可能会低效——这几乎总是如此。

使用**SELECT ***可能会导致数据处理过多，特别是如果表有很多列或你在处理大型数据集时。

替代方法如下：

```py
SELECT * FROM employees;
```

这样做：

```py
SELECT employee_id, first_name, last_name FROM employees;
```

只读取必要的列可以使查询更加易读和易于维护。

## 2\. 避免使用 SELECT DISTINCT; 使用 GROUP BY 代替

**SELECT DISTINCT** 可能代价高昂，因为它需要对结果进行排序和过滤以移除重复项。最好通过设计上的唯一性来确保查询的数据是唯一的——使用主键或唯一约束。

替代方法如下：

```py
SELECT DISTINCT department FROM employees;
```

以下带有 GROUP BY 子句的查询要有帮助得多：

```py
SELECT department FROM employees GROUP BY department;
```

GROUP BY 可以更高效，特别是使用适当的索引时（我们稍后会讨论索引）。所以在编写查询时，确保你理解数据——不同的字段——在数据模型层面。

## 3\. 限制查询结果

通常你会查询包含数千行的大表，但你不总是需要（也无法）处理所有行。使用 **LIMIT** 子句（或其等效功能）有助于减少返回的行数，从而加快查询性能。

你可以将结果限制为 15 条记录：

```py
SELECT employee_id, first_name, last_name FROM employees LIMIT 15;
```

使用 LIMIT 子句减少结果集的大小，减少处理和传输的数据量。这也对应用程序中的分页结果很有用。

## 4\. 使用索引以加快检索速度

索引可以显著提高查询性能，通过使数据库比扫描整个表更快地找到行。它们对于经常在 WHERE、JOIN 和 ORDER BY 子句中使用的列特别有用。

这是一个在‘department’列上创建的示例索引：

```py
CREATE INDEX idx_employee_department ON employees(department);
```

你现在可以运行涉及‘department’列的过滤查询，并比较执行时间。你应该能够看到使用索引后结果速度大幅提高。要了解更多关于创建索引和性能改进的信息，请访问 [如何使用索引加速 SQL 查询 [Python 版]](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)。

如前所述，索引提高了对索引列的过滤查询的效率。但创建过多的索引可能会适得其反。这引出了下一个提示！

## 5\. 谨慎使用索引

虽然索引提高了读取性能，但它们可能会降低写入性能——INSERT、UPDATE 和 DELETE 查询——因为每次修改表时，索引必须更新。根据你经常运行的查询类型，平衡索引的数量和类型是很重要的。

作为通用规则：

+   仅索引经常查询的列。

+   避免对低基数（唯一值较少）的列进行过度索引。

+   定期检查索引，根据需要更新和删除它们。

总之，创建索引以加快对经常查询但很少更新的列的检索。这确保了索引的好处超过其维护成本。

## 总结

优化 SQL 查询涉及理解查询的具体需求和数据的结构。

通过避免使用 SELECT *，小心使用 SELECT DISTINCT，限制查询结果，创建适当的索引，并注意索引的权衡，你可以显著提高数据库操作的性能和效率。

祝你查询愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和咖啡！目前，她正致力于通过撰写教程、操作指南、观点文章等方式学习并与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编程教程。

### 相关主题

+   [使用 SQL 查询你的 Pandas 数据框](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [SQL 查询优化技巧](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [我们可以用 T5 查询表格吗？](https://www.kdnuggets.com/2022/05/query-table-t5.html)

+   [超越准确度：使用 NLP 测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [更多分类问题的性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [延迟性能的诅咒](https://www.kdnuggets.com/2022/05/nannyml-curse-delayed-performance.html)
