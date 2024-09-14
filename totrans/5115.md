# 逐步指南：阅读和理解 SQL 查询

> 原文：[https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)

![逐步指南：阅读和理解 SQL 查询](../Images/544a9ec01d27f3a9db6a4caadd97b5fa.png)

图片来源：[Freepik](https://www.freepik.com/free-vector/abstract-technology-sql-illustration_21743439.htm#query=sql%20query&position=0&from_view=search&track=ais&uuid=a7fa0c7e-00e4-42b2-9e13-3ea486e88a3e)

SQL，即结构化查询语言，是一种用于管理和操控关系型数据库管理系统（RDBMS）中的数据的编程语言。它是许多公司用来帮助业务顺利访问数据的标准语言。由于 SQL 的广泛使用，通常就业市场会将 SQL 作为必要技能之一。这就是为什么学习 SQL 非常重要。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

学习 SQL 时，一个常见的问题是理解查询，特别是当查询由其他人编写时。在公司中，我们通常需要团队合作，并且经常需要阅读和理解他们的 SQL 查询。因此，我们需要练习拆解 SQL 查询并理解它们。

本文将逐步介绍如何读取和理解 SQL 查询。我们怎么做呢？让我们开始吧。

# 1\. 理解一般 SQL 查询结构

遇到 SQL 查询时，我们需要做的第一件事是理解 SQL 查询的总体意图。总体意图并不意味着我们完全了解查询的结构；更多的是关于整体流程。

我们应该了解标准 SQL 查询，以便理解一般 SQL 查询。大多数 SQL 查询以 ***SELECT*** 子句开始，然后是 ***FROM*** 子句。接着，查询通常会跟随 ***JOIN***、***WHERE***、***GROUP BY***、***ORDER BY*** 和 ***HAVING*** 子句。

上述子句是我们需要理解的 SQL 查询的标准部分。每个子句的功能如下：

1.  ***SELECT***：从表中选择哪些列

1.  ***FROM***：数据来自哪个表

1.  ***JOIN***：通过指定的标识符合并表

1.  ***WHERE***：基于条件的数据过滤

1.  ***GROUP BY***：根据列的值组织数据，并允许执行聚合函数。

1.  ***ORDER BY***：根据特定列排列数据结果的顺序

1.  ***HAVING***：用于聚合函数的过滤条件，不能用***WHERE***指定

这些是标准子句，你在理解一般SQL查询结构时应首先找到它们。让我们使用示例代码进一步学习。

```py
SELECT 
  customers.name, 
  purchases.product, 
  SUM(price) as total_price 
FROM 
  purchases 
  JOIN customers ON purchases.cust_id = customers.id 
WHERE 
  purchases.category = 'kitchen' 
GROUP BY 
  customers.name, 
  purchases.product 
HAVING 
  total_price > 10000 
ORDER BY 
  total_price DESC; 
```

查看上述查询时，尝试识别标准子句。子句将帮助你理解选择了哪些数据（***SELECT***），数据来源于哪里（***FROM***和***JOIN***），以及条件（***WHERE***、***GROUP BY***、***ORDER BY***和***HAVING***）。

例如，阅读上面的查询可以帮助你理解以下内容：

1.  我们尝试获取三种不同的数据：来自`customers`表的`Name`，来自`purchases`表的`Product`，以及聚合的`price`列，未识别表的来源，并以别名`total_price`表示（信息来自于**SELECT**子句）。

1.  整体数据将来自于通过`cust_id`列连接的`purchases`表和通过`id`列连接的`customers`表（信息来自于***FROM***和***JOIN***子句）。

1.  我们只会选择`purchases`表中`category`列值为‘kitchen’的数据（信息来自于**WHERE**子句）。

1.  按`name`和`product`列进行分组，这些列来自于各自的表（信息来自于**GROUP BY**子句）。

1.  还需根据聚合函数结果的总和过滤，其中`total_price`大于10000（信息来自于***HAVING***子句），

1.  根据`total_price`降序排列数据（信息来自于**ORDER BY**子句）。

这是你需要了解和识别的一般SQL查询结构。从这里，我们可以深入探讨更高级的查询。让我们继续下一步。

# 2\. 理解最终的SELECT

有时你会遇到一个复杂的查询，其中包含许多***SELECT***子句。在这种情况下，我们应理解查询的最终结果或你在查询中看到的第一个（最终）***SELECT***。关键是要了解查询输出的期望结果。

我们使用下面的更复杂的代码示例。

```py
WITH customerspending AS (
  SELECT 
    customers.id, 
    SUM(purchases.price) as total_spending 
  FROM 
    purchases 
    JOIN customers ON purchases.cust_id = customers.id 
  GROUP BY 
    customers.id
) 
SELECT 
  c.name, 
  pd.product, 
  pd.total_product_price, 
  cs.total_spending 
FROM 
  (
    SELECT 
      purchases.cust_id, 
      purchases.product, 
      SUM(purchases.price) as total_product_price 
    FROM 
      purchases 
    WHERE 
      purchases.category = 'kitchen' 
    GROUP BY 
      purchases.cust_id, 
      purchases.product 
    HAVING 
      SUM(purchases.price) > 10000
  ) AS pd 
  JOIN customers c ON pd.cust_id = c.id 
  JOIN customerspending cs ON c.id = cs.id 
ORDER BY 
  pd.total_product_price DESC; 
```

查询现在看起来更复杂且更长，但初步关注应放在最终的***SELECT***上，它似乎试图生成客户的总支出和购买历史。尝试评估最终结果的期望，并从中分解。

# 3\. 理解最终条件子句

我们对结果有了初步了解，接下来的部分是查看最终***SELECT***的条件。条件子句，包括***WHERE***、***GROUP BY***、***ORDER BY***和***HAVING***，控制了整体数据结果。

尝试阅读和理解我们查询的条件，我们将更好地理解查询的最终结果。例如，在我们之前的SQL查询中，最终条件仅为***ORDER BY***。这意味着最终结果将按总产品价格的降序排列。

了解最终条件将帮助你理解查询的主要部分及整体意图。

# 4\. 理解最终连接

最后，我们需要了解数据的来源。在了解选择哪些数据以及获取这些数据的条件之后，我们需要理解数据来源。最终的***JOIN***子句将帮助我们理解表的交互和数据流动。

例如，之前的复杂查询显示我们进行了两次连接。这意味着我们至少使用了三个数据源来获取最终结果。这个信息在后续步骤中理解每个数据源的来源时将非常必要，特别是当数据源来自子查询时。

# 5\. 逆序阅读和重复

在了解最终结果应如何呈现及其来源后，我们需要更详细地查看细节。从这里开始，我们会回溯到每个子查询，了解它们为何以这种方式构造。

然而，我们不应尝试从顶到底地查看它们。相反，我们应尝试查看那些离最终结果较近的子查询，并向上移动到离最终结果最远的子查询。从上面的代码示例中，我们应该首先理解这段代码：

```py
SELECT 
  purchases.cust_id, 
  purchases.product, 
  SUM(purchases.price) as total_product_price 
FROM 
  purchases 
WHERE 
  purchases.category = 'kitchen' 
GROUP BY 
  purchases.cust_id, 
  purchases.product 
HAVING 
  SUM(purchases.price) > 10000
```

然后，我们将转向最远的代码，即这段：

```py
WITH customerspending AS (
  SELECT 
    customers.id, 
    SUM(purchases.price) as total_spending 
  FROM 
    purchases 
    JOIN customers ON purchases.cust_id = customers.id 
  GROUP BY 
    customers.id
)
```

当我们从最接近结果的子查询开始逐步拆解时，可以清晰地追踪到作者的思路。

如果你在理解每个子查询时遇到困难，可以尝试重复上述过程。通过一些练习，你将更好地阅读和理解查询。

# 结论

阅读和理解SQL查询是现代社会每个人都应该具备的技能，因为每家公司都在处理SQL查询。通过以下逐步指南，你将更好地理解复杂的SQL查询。这些步骤包括：

1.  理解一般的SQL查询结构

1.  理解最终选择

1.  理解最终条件子句

1.  理解最终连接

1.  逆序阅读和重复

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于Allianz Indonesia的同时，他喜欢通过社交媒体和写作媒体分享Python和数据技巧。Cornellius撰写了各种AI和机器学习主题的文章。

### 更多相关内容

+   [解析 DENSE_RANK()：SQL 爱好者的逐步指南](https://www.kdnuggets.com/breaking-down-denserank-a-step-by-step-guide-for-sql-enthusiasts)

+   [使用 Python 和 Beautiful Soup 进行网页抓取的逐步指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [成为数据科学家的指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)

+   [如何结构化数据科学项目：逐步指南](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)

+   [文本到视频生成：逐步指南](https://www.kdnuggets.com/2023/08/text2video-generation-stepbystep-guide.html)

+   [像专业人士一样测试：Python Mock 库的逐步指南](https://www.kdnuggets.com/testing-like-a-pro-a-step-by-step-guide-to-pythons-mock-library)
