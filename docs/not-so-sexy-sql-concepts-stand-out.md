# 让你脱颖而出的不那么炫酷的 SQL 概念

> 原文：[`www.kdnuggets.com/2022/02/not-so-sexy-sql-concepts-stand-out.html`](https://www.kdnuggets.com/2022/02/not-so-sexy-sql-concepts-stand-out.html)

SQL（结构化查询语言）是管理关系型数据库数据的编程语言。大多数数据科学家将使用关系型数据，而有一个较小的误解认为数据库操作的工作落在了伟大的“数据工程师”身上。从实际角度来看，作为数据科学家，每次需要提取一条数据时，你希望去找别人吗？

实际上，很多数据科学职位要求具备 SQL 经验/技能，但我们要么在学术中没有学习，要么不愿意自己学习，以便有更多时间学习 R 和 Python。是的，确实一天、一个星期、一个月等时间有限，但数据库是我们数据的家，数据科学家必须拥有一把钥匙！

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

当然，这不会是革命性的突破，但正因为这些概念在数据科学工具包中并不主流，才阻碍了人们提升技能，将数据科学水平提升到一个新层次。

![让你脱颖而出的不那么炫酷的 SQL 概念](img/de892b475bedeefa0235111dd9749785.png)

在这篇文章中，我讨论了一些数据科学家不熟悉的 SQL 决策概念。我将使用 SQL Server 作为我的 SQL 选择。请注意，如果你使用 MySQL、PostgreSQL 等，语法将会有所不同：

### 1\. SQL 视图

现在，简单总结一下你为何需要这种技术，视图（Views）将减少你重复运行相同脚本以访问相同数据的需求。我们经常会创建一个 SQL 脚本，一旦它正确，我们就将其保存到文件夹中，然后再次运行。既然我们可以将其保存为视图（View）并像使用 SQL 表一样使用它，何必这样做呢？

以下 SQL 脚本展示了一个普通的 SELECT 和 JOIN 语句，平均水平的新 SQL 用户或初学者 SQL 学术课程将会使用。

```py
SELECT
    p.customer_name, 
    p.customer_address, 
    b.order_item
    b.order_qty
FROM
    customers.purchases p
INNER JOIN purchases.orders b 
        ON b.order_id = p.order_id;

```

现在，假设你需要在工作中多次引用这个表，每次都发现自己在重新运行脚本。如果这是一个大表，你知道这可能需要非常长的时间。使用视图可以节省时间，让你随时引用它，并且几乎像正常表一样操作它。

```py
CREATE VIEW customers.orders
AS
SELECT
    p.customer_name, 
    p.customer_address, 
    b.order_item
    b.order_qty
FROM
    customers.purchases p
INNER JOIN purchases.orders b 
        ON b.order_id = p.order_id;

```

创建视图就是这么简单。这并不是存储或创建一个新表，而是让用户能够用一个简单的命令如‘*SELECT * FROM customers.orders*’来创建之前的表。你只需这样做，而不必保存文件并重新运行脚本。只需打开你的 SQL 应用程序并运行相关的选择语句即可。

### 2\. 存储过程

存储过程是一段你可以保存并在需要时执行的 SQL 代码。这是创建 SQL 数据处理工作流效率的另一种方法。

```py
CREATE PROCEDURE spCustomerOrders
AS
BEGIN
SELECT
    p.customer_name, 
    p.customer_address, 
    b.order_item
    b.order_qty
FROM
    customers.purchases p
INNER JOIN purchases.orders b 
        ON b.order_id = p.order_id;
END;

```

要执行上述过程，只需输入

```py
EXEC spCustomerOrders;
```

再次，这是一种创建小效率的简单方法。这确实类似于视图，但执行方法不同。从本质上讲，这是正确的，但区别在于你可以为存储过程指定**参数**。它的工作方式类似于 SQL 中的函数或过滤器。我认为这主要用作快速过滤方法，而不是其他任何东西。当然，现在你可以添加参数，还有许多其他可能性。

```py
CREATE PROCEDURE spCustomerOrders(@ord_qty AS Int)
AS
BEGIN
SELECT
    p.customer_name, 
    p.customer_address, 
    b.order_item
    b.order_qty
FROM
    customers.purchases p
INNER JOIN purchases.orders b 
        ON b.order_id = p.order_id;
WHERE b.order_qty >= @ord_qty
END;

```

在上述示例中，我们将‘*@ord_qty*’参数添加到了*spCustomerOrders*过程。它在 SQL Server 中总是以‘@’开头，*‘AS int’*用于指定期望的数据类型。在这种情况下，我们在 WHERE 子句中使用它来根据购买数量筛选客户和订单详情。

要执行存储过程，只需简单地输入

```py
EXEC spCustomerOrders 125;
```

上述命令将执行存储过程，返回订单数量超过 125 的客户和订单详情。

### 3\. 标量函数

作为一名统计学家，我认为很多人可以拼凑出标量函数的定义。但我还是给你一个定义。在 SQL 中，标量函数是一段代码，它接收一个或多个参数来计算并返回一个单一的值。我们经常使用 SQL 来创建数据汇总。

在我个人看来，可以把它看作是 GROUP BY 2.0。可能有点夸张，但本质上允许你对 SQL 默认情况下可能无法直接获取的数据进行更复杂的计算。

```py
CREATE FUNCTION customers.GrossProfit_per_Cust(
    @ord_qty INT,
    @price DEC(10,2),
    @cost_of_sales DEC(10,2)
)
RETURNS DEC(10,2)
AS 
BEGIN
    RETURN (@ord_qty * @price)-@cost_of_sales;
END;

```

上述函数用于通过将订单数量乘以价格并减去销售成本来查找每位客户的毛利润（这不是会计课，所以如果不正确请见谅！）。

以最简单的方式，你可以像这样使用函数：

```py
SELECT customers.GrossProfit_per_cust(12,50,200)
```

这看起来不错，但真正的价值在于在典型的 SELECT 语句中使用它，以将数据汇总提升到另一个层次。

```py
SELECT 
p.customer_name, SUM(customers.GrossProfit_per_cust(b.ord_qty, b.price, b.cost_of_sales) gross_profit
FROM customers.purchases p
INNER JOIN purchases.orders b 
ON b.order_id = p.order_id;
GROUP BY p.customer_name
ORDER BY gross_profit DESC;

```

### **结论**

这些是数据科学家可用但在学术层面未被充分利用的 3 个 SQL 概念，并且在许多初学者数据科学家的优先级列表中并不高。数据库现在正转向云端，全球生成的数据也在不断增加。

数据库技术需要跟上数据的步伐，以尽可能发挥其作用，而当今和未来的数据科学家需要能够以高于仅仅拥有最复杂的代码（如 GROUP BY）的水平来操作数据库。我们需要改进流程，更快、更灵活。

**[Asel Mendis](https://www.linkedin.com/in/asel-mendis/)** ([@aselmendis](https://twitter.com/aselmendis)) 是一位在澳大利亚的 数据科学家。他来自斯里兰卡，自 2019 年起进入数据领域。作为数据科学家，他的目标是利用相关技术和方法创造洞察和价值。他是 KDnuggets 的贡献编辑。他对位置智能、空间分析、人口统计学、机器学习、数据可视化以及使用 R 和 Python 的统计学感兴趣。他拥有皇家墨尔本理工大学的分析硕士学位，专攻应用统计学，并于 2021 年开始攻读应用统计学博士学位，研究主题为道路黑点。他还是澳大利亚统计学会的会员，并获得了统计学研究生（GStat）称号，未来期望成为认证统计学家（AStat）。

### 更多相关主题

+   [数据科学中你应该知道的 7 个 SQL 概念](https://www.kdnuggets.com/2022/11/7-sql-concepts-needed-data-science.html)

+   [回到基础第 2 周：数据库、SQL、数据管理及…](https://www.kdnuggets.com/back-to-basics-week-2-database-sql-data-management-and-statistical-concepts)

+   [你应该了解的 5 个关于梯度下降和成本函数的概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [在深入了解 Transformers 之前你应该知道的概念](https://www.kdnuggets.com/2023/01/concepts-know-getting-transformer.html)

+   [数据科学的 8 个基础统计概念](https://www.kdnuggets.com/2020/06/8-basic-statistics-concepts.html)

+   [程序员的 10 个数学概念](https://www.kdnuggets.com/10-math-concepts-for-programmers)
