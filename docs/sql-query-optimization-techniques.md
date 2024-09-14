# SQL 查询优化技术

> 原文：[`www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html`](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

![SQL 查询优化技术](img/41f30feb0f08d02cc4e3508fc8274d43.png)

作者提供的图片

在初级阶段，我们只关注编写和运行 SQL 查询。我们不在乎执行需要多长时间或它是否能处理数百万条记录。但在中级阶段，人们期望你的查询已优化并且执行时间最小。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

在大型应用程序中编写优化的查询，如电子商务平台或银行系统，是至关重要的。假设你拥有一个超过一百万个产品的电子商务公司，而客户希望搜索某个产品。如果你在后端编写的查询需要超过一分钟来从数据库中获取该产品，你认为客户还会在你的网站上购买产品吗？

你必须了解 SQL 查询优化的重要性。在本教程中，我将向你展示一些优化 SQL 查询的技巧和窍门，从而使它们执行得更快。主要前提是你必须具备基本的 SQL 知识。

# 1\. 使用 EXIST() 替代 COUNT() 来查找表中的特定元素

要检查表中是否存在特定元素，请使用 `EXIST()` 关键字，代替 `COUNT()` 将以更优化的方式运行查询。

使用 `COUNT()`，查询需要计算特定元素的所有出现次数，这在数据库庞大时可能效率较低。另一方面，`EXIST()` 只会检查该元素的第一次出现，然后在找到第一次出现时停止。这节省了很多时间。

另外，你只关心检查特定元素是否存在，而不关心出现次数。这就是为什么 `EXIST()` 更好的原因。

```py
SELECT 
  EXISTS(
    SELECT 
      * 
    FROM 
      table 
    WHERE 
      myColumn = 'val'
  );
```

上述查询会返回**1**，如果至少有一行表记录包含名为 `myColumn` 的列，其值等于**val**。否则，它会返回**0**。

# 2\. 使用 Varchar 而不是 Char

`char` 和 `varchar` 数据类型都用于在表中存储字符字符串。但 `varchar` 比 `char` 更加节省内存。

char 数据类型只能存储固定长度的字符字符串。如果字符串的长度小于固定长度，则会填充空格以使其长度等于设置的长度。这会在填充时不必要地浪费内存。例如，`CHAR(100)` 即使存储一个字符也会占用 100 字节的内存。

另一方面，varchar 数据类型存储可变长度的字符字符串，其长度小于指定的最大长度。它不会填充空格，只占用与字符串实际长度相等的内存。例如，`VARCHAR(100)` 存储一个字符时仅占用 1 字节的内存。

```py
CREATE TABLE myTable (
  id INT PRIMARY KEY, 
  charCol CHAR(10), 
  varcharCol VARCHAR(10)
);
```

在上面的示例中，创建了一个名为 `myTable` 的表，该表有两个列，分别是 `charCol` 和 `varcharCol`，它们的类型分别是 char 和 varchar。`charCol` 将始终占用 10 字节的内存。相比之下，`varcharCol` 占用的内存等于存储其中的字符字符串的实际大小。

# 3\. 避免在 WHERE 子句中使用子查询

我们必须避免在 WHERE 子句中使用子查询以优化 SQL 查询，因为子查询可能会非常昂贵，并且在返回大量行时执行起来很困难。

你可以通过使用连接操作或编写相关子查询来获得相同的结果，而不是使用子查询。相关子查询是一种内部查询依赖于外部查询的子查询。与非相关子查询相比，它们非常高效。

下面是一个例子，以理解这两者之间的区别。

```py
# Using a subquery
SELECT 
  * 
FROM 
  orders 
WHERE 
  customer_id IN (
    SELECT 
      id 
    FROM 
      customers 
    WHERE 
      country = 'INDIA'
  );

# Using a join operation
SELECT 
  orders.* 
FROM 
  orders 
  JOIN customers ON orders.customer_id = customers.id 
WHERE 
  customers.country = 'INDIA'; 
```

在第一个示例中，子查询首先收集所有属于印度的客户 ID，然后外部查询将获取所选客户 ID 的所有订单。而在第二个示例中，我们通过连接`customers`和`orders`表来实现相同的结果，然后仅选择来自印度的客户的订单。

通过避免在 WHERE 子句中使用子查询并使其更易于阅读和理解，我们可以优化查询。

# 4\. 将 JOIN 从较大表应用到较小表

将 `JOIN` 操作从较大表应用到较小表是一种常见的 SQL 优化技术。因为从较大表连接到较小表会使你的查询执行得更快。如果我们将 `JOIN` 操作从较小表应用到较大表，SQL 引擎必须在较大表中查找匹配的行。这是更为耗费资源和时间的。另一方面，如果 `JOIN` 从较大表应用到较小表，SQL 引擎只需在较小表中查找匹配的行。

这里有一个例子帮助你更好地理解。

```py
# Order table is larger than the Customer table

# Join from a larger table to a smaller table
SELECT 
  * 
FROM 
  Order 
  JOIN Customer ON Customer.id = Order.id

# Join from a smaller table to a larger table
SELECT 
  * 
FROM 
  Customer 
  JOIN Order ON Customer.id = Order.id 
```

# 5\. 使用 `regexp_like` 代替 `LIKE` 子句

与 `LIKE` 子句不同，`regexp_like` 也用于模式搜索。`LIKE` 子句是一个基本的模式匹配操作符，只能执行如 **_** 或 **%** 这样的基本操作，用于匹配单个字符或任意数量的字符。`LIKE` 子句必须扫描整个数据库以找到特定模式，这对于大型表来说速度较慢。

另一方面，`regexp_like` 是一种更高效、优化和强大的模式搜索技术。它使用更复杂的正则表达式来查找字符字符串中的特定模式。这些正则表达式比简单的通配符匹配更具体，因为它们允许你搜索我们所寻找的确切模式。由于这个原因，需要搜索的数据量减少，查询执行速度更快。

请注意，`regexp_like` 可能并非所有数据库管理系统中都存在。其语法和功能在其他系统中可能有所不同。

这里有一个示例帮助你更好地理解。

```py
# Query using the LIKE clause
SELECT 
  * 
FROM 
  mytable 
WHERE 
  (
    name LIKE 'A%' 
    OR name LIKE 'B%'
  );

# Query using regexp_like clause
SELECT 
  * 
FROM 
  mytable 
WHERE 
  regexp_like(name, '^[AB].*'); 
```

上述查询用于查找名称以 A 或 B 开头的元素。在第一个示例中，使用 `LIKE` 搜索所有以 A 或 B 开头的名称。`A%` 表示第一个字符是 A；之后可以有任意数量的字符。在第二个示例中，使用了 `regexp_like`。在 `^[AB]` 内部，`^` 表示符号将匹配字符串的开头，`[AB]` 表示开头字符可以是 A 或 B，`.*` 表示之后的所有字符。

使用 `regexp_like`，数据库可以快速过滤掉不匹配模式的行，从而提高性能并减少资源使用。

# 结论

在这篇文章中，我们讨论了优化 SQL 查询的各种方法和技巧。本文让你清晰地了解了如何编写高效的 SQL 查询以及优化它们的重要性。优化查询的方法还有很多，例如偏好使用整数值而不是字符，或者在表中没有重复项时使用 Union All 而不是 Union 等。

**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)** 是一名电气工程专业的 B.Tech. 学生，目前处于本科最后一年。他的兴趣领域包括网页开发和机器学习。他已经在这些方向上有所追求，并渴望进一步深入工作。

### 了解更多相关内容

+   [用 SQL 查询你的 Pandas DataFrame](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [提升 SQL 查询性能的 5 个技巧](https://www.kdnuggets.com/5-tips-for-improving-sql-query-performance)

+   [3 种基于研究的高级提示技术提升 LLM 效率…](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [我们可以用 T5 查询表格吗？](https://www.kdnuggets.com/2022/05/query-table-t5.html)

+   [数据库优化：探索 SQL 中的索引](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)

+   [学习高级 SQL 技巧的前 5 个免费资源](https://www.kdnuggets.com/top-5-free-resources-for-learning-advanced-sql-techniques)
