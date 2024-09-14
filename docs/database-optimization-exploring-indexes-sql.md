# 数据库优化：探索SQL中的索引

> 原文：[https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)

![数据库优化：探索SQL中的索引](../Images/8f24e8e8ded8676df58f2a7ab39f3311.png)

图片由作者提供

在书中寻找特定主题时，我们会先查看索引页（位于书的开头），找出包含我们感兴趣主题的页码。现在，想象一下没有索引页的情况下查找书中的特定主题是多么不便。为此，我们不得不逐页搜索，这既耗时又令人沮丧。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT工作。

* * *

在SQL Server检索数据时，也会出现类似的问题。为了解决这个问题，SQL Server也使用索引来加快数据检索过程，在这篇文章中我们将讨论这一部分。我们将讨论为什么需要索引以及如何有效地创建和删除索引。本教程的前提是对SQL命令的基本了解。

# 什么是索引？

索引是一种模式对象，使用指针从行中检索数据，从而减少定位数据的I/O（输入/输出）时间。索引可以应用于一个或多个我们希望搜索的列。它们将列存储在一个称为[**B-Tree**](https://en.wikipedia.org/wiki/B-tree#:~:text=A%20B%2Dtree%20index%20creates,pages%20at%20the%20lowest%20level.)的单独数据结构中。B-Tree的主要优点之一是它将数据按排序顺序存储。

如果你想知道为什么排序后的数据可以更快地检索，你必须阅读[线性搜索与二分搜索](https://www.javatpoint.com/ds-linear-search-vs-binary-search)。

索引是提升SQL查询性能的最著名方法之一。它们小巧、快速，并且针对关系表进行了显著优化。当我们希望在没有索引的情况下搜索一行时，SQL会执行全表扫描，逐行扫描每一行以找到匹配条件，这非常耗时。另一方面，如前所述，索引保持数据排序。

但是我们也应该小心，索引创建了一个单独的数据结构，这需要额外的空间，当数据库很大时，这可能会成为一个问题。为了良好的实践，索引仅对经常使用的列有效，而对于很少使用的列可以避免使用。以下是一些索引可能有用的场景，

1.  行数必须大于（>10000）。

1.  所需的列包含大量值。

1.  所需的列不能包含大量的NULL值。

1.  如果我们经常基于特定列对数据进行排序或分组，索引可以快速检索排序后的数据，而不是进行全表扫描。

并且在以下情况下可以避免使用索引，

1.  表很小。

1.  或者当列的值很少使用时。

1.  或者当列的值经常变化时。

也可能有一种情况，当优化器检测到全表扫描比索引表花费的时间更少时，即使存在索引，索引也可能不会被使用。这种情况可能发生在表很小或者列经常更新时。

# 创建示例数据库

在开始之前，你必须在PC上设置MySQL Workbench，以便轻松跟随教程。你可以参考[这个](https://www.youtube.com/watch?v=GwHpIl0vqY4&ab_channel=AmitThinks) YouTube视频来设置你的工作台。

设置好工作台后，我们将创建一些随机数据，从中执行查询。

**创建表：**

```py
-- Create a table to hold the random data

CREATE TABLE employee_info (id INT PRIMARY KEY AUTO_INCREMENT,
                                               name VARCHAR(100),
                                                    age INT, email VARCHAR(100));
```

**插入数据：**

```py
-- Insert random data into the table

INSERT INTO employee_info (name, age, email)
SELECT CONCAT('User', LPAD(ROW_NUMBER() OVER (), 5, '0')),
       FLOOR(RAND() * 50) + 20,
       CONCAT('user', LPAD(ROW_NUMBER() OVER (), 5, '0'), '@xyz.com')
FROM information_schema.tables
LIMIT 100;
```

它将创建一个名为`employee_info`的表，具有名称、年龄和电子邮件等属性。

**显示数据：**

```py
SELECT *
FROM employee_info;
```

输出：

![数据库优化：探索SQL中的索引](../Images/23f9cc00fa11f0261497c54e161cf30b.png)

图 1 示例数据库 | 图片来源：作者

# 创建和删除索引

为了创建索引，我们可以使用如下的CREATE命令，

语法：

```py
CREATE INDEX index_name ON TABLE_NAME (COLUMN_NAME);
```

在上述查询中，`index_name`是索引的名称，`table_name`是表的名称，`column_name`是我们要应用索引的列名称。

例如

```py
CREATE INDEX age_index ON employee_info (age);
```

我们还可以为同一表中的多个列创建索引，

```py
CREATE INDEX index_name ON TABLE_NAME (col1,
                                       col2,
                                       col3, ....);
```

**唯一索引：** 我们还可以为特定列创建唯一索引，这不允许在该列中存储重复值。这可以维护数据的完整性，并进一步提高性能。

```py
CREATE UNIQUE INDEX index_name ON TABLE_NAME (COLUMN_NAME);
```

**注意：** 索引可以自动为PRIMARY_KEY和UNIQUE列创建。我们不需要手动创建它们。

**删除索引：**

我们可以使用DROP命令从表中删除特定的索引。

```py
DROP INDEX index_name ON TABLE_NAME;
```

我们需要指定索引和表的名称以删除索引。

**显示索引：**

你还可以查看表中存在的所有索引。

语法：

```py
SHOW INDEX
FROM TABLE_NAME;
```

例如

```py
SHOW INDEX
FROM employee_info;
```

输出：

![数据库优化：探索SQL中的索引](../Images/cf61de5a41b2b6660af55c2f95f35b27.png)

# 更新索引

以下命令在现有表中创建一个新索引。

语法：

```py
ALTER TABLE TABLE_NAME ADD INDEX index_name (col1, col2, col3, ...);
```

**注意：** ALTER 不是ANSI SQL的标准命令。因此，它可能在其他数据库中有所不同。

例如

```py
ALTER TABLE employee_info ADD INDEX name_index (name);

SHOW INDEX
FROM employee_info;
```

输出：

![数据库优化：探索 SQL 索引](../Images/c82b61b7132d7ee034165739dbaaa2d4.png)

在上面的示例中，我们在现有表中创建了一个新索引。但我们无法修改现有索引。为此，我们必须首先删除旧索引，然后创建一个新的修改版索引。

例如：

```py
DROP INDEX name_index ON employee_info;

CREATE INDEX name_index ON employee_info (name, email);

SHOW INDEX
FROM employee_info ;
```

输出：

![数据库优化：探索 SQL 索引](../Images/3686af726168caf139980d8578c871dc.png)

# 总结一下

在这篇文章中，我们涵盖了 SQL 索引的基本理解。还建议将索引保持狭窄，即仅限于少数几列，因为过多的索引可能会对性能产生负面影响。索引可以加快`SELECT`查询和`WHERE`子句，但会减慢插入和更新语句。因此，只对经常使用的列应用索引是一个好的做法。

在此之前，请继续阅读和学习。

**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)** 是一名 B.Tech 电气工程专业的学生，目前在本科最后一年。他的兴趣领域包括网页开发和机器学习。他一直在追求这些兴趣，并渴望在这些方向上进一步发展。

### 更多相关主题

+   [如何使用索引加速 SQL 查询 [Python 版]](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)

+   [Python 向量数据库和向量索引：架构 LLM 应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [SQL 查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [免费 SQL 和数据库课程](https://www.kdnuggets.com/2022/09/free-sql-database-course.html)

+   [数据库内分析：利用 SQL 的分析功能](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)

+   [KDnuggets 新闻，9月21日：7个机器学习项目集…](https://www.kdnuggets.com/2022/n37.html)
