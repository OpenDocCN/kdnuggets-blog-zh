# SQL入门五步骤

> 原文：[https://www.kdnuggets.com/5-steps-getting-started-with-sql](https://www.kdnuggets.com/5-steps-getting-started-with-sql)

![SQL入门五步骤](../Images/fa59cd914bdc8e4ddf8c87a652f631d5.png)

# 结构化查询语言简介

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

在关系型数据库中管理和操作数据时，结构化查询语言（SQL）是最重要的工具。SQL是一个主要的领域特定语言，它是数据库管理的基石，并提供了一种与数据库交互的标准化方式。随着数据驱动决策和创新的力量，SQL仍然是一个至关重要的技术，需要数据分析师、开发人员和数据科学家给予高度关注。

SQL最初由IBM于1970年代开发，并在1980年代末由ANSI和ISO标准化。各种类型的组织——从小型企业到大学，再到大型公司——都依赖于如MySQL、SQL Server和PostgreSQL等SQL数据库来处理大规模数据。随着数据驱动行业的扩展，SQL的重要性不断增长。其普遍应用使其成为各种专业人士在数据领域及其他领域的关键技能。

SQL允许用户执行各种数据相关任务，包括：

+   查询数据

+   插入新记录

+   更新现有记录

+   删除记录

+   创建和修改表

本教程将提供SQL的逐步讲解，重点是通过大量的实操示例开始学习。

# 第一步：设置你的SQL环境

## 选择SQL数据库管理系统（DBMS）

在深入SQL查询之前，你需要选择一个适合你项目需求的数据库管理系统（DBMS）。DBMS是你SQL活动的支柱，提供不同的功能、性能优化和定价模型。你对DBMS的选择会显著影响你与数据的互动方式。

+   **MySQL**：开源，广泛采用，被Facebook和Google使用。适用于从小项目到企业级应用的各种应用。

+   **PostgreSQL**：开源，功能强大，被Apple使用。以其性能和标准遵从性而闻名。

+   **SQL Server Express**：微软的入门级选项。适用于具有有限扩展需求的小型到中型应用程序。

+   **SQLite**：轻量级、无服务器、自包含。非常适合移动应用和小型项目。

## MySQL 安装指南

为了本教程的目的，我们将重点介绍 MySQL，因为它的使用广泛且功能全面。安装 MySQL 是一个简单的过程：

1.  访问 [MySQL 的网站](https://dev.mysql.com/downloads/mysql/) 并下载适合你操作系统的安装程序。

1.  运行安装程序，按照屏幕上的指示操作。

1.  在设置过程中，你会被要求创建一个 root 帐户。请确保记住或安全存储 root 密码。

1.  安装完成后，你可以通过打开终端并输入 `mysql -u root -p` 来访问 MySQL shell。系统会提示你输入 root 密码。

1.  成功登录后，你将看到 MySQL 提示符，表示你的 MySQL 服务器正在运行。

## 设置 SQL IDE

集成开发环境 (IDE) 可以显著提升你的 SQL 编程体验，提供如自动补全、语法高亮和数据库可视化等功能。虽然运行 SQL 查询不一定需要 IDE，但对于更复杂的任务和大型项目，IDE 是强烈推荐的。

+   **[DBeaver](https://dbeaver.io/)**：开源，并支持广泛的 DBMS，包括 MySQL、PostgreSQL、SQLite 和 SQL Server。

+   **[MySQL Workbench](https://www.mysql.com/products/workbench/)**：由 Oracle 开发，这是 MySQL 的官方 IDE，提供了专门为 MySQL 量身定制的全面工具。

下载并安装你选择的 IDE 后，你需要将其连接到你的 MySQL 服务器。这通常涉及指定服务器的 IP 地址（如果服务器在你的机器上则为 `localhost`）、端口号（通常 MySQL 的端口号为 3306）以及授权数据库用户的凭据。

## 测试你的设置

让我们确保一切正常工作。你可以通过运行一个简单的 SQL 查询来显示所有现有的数据库来做到这一点：

```py
SHOW DATABASES;
```

如果这个查询返回一个数据库列表，并且没有错误，那么恭喜你！你的 SQL 环境已经成功设置好，你可以开始 SQL 编程了。

# 步骤 2：基本 SQL 语法和命令

## 创建数据库和表

在添加或操作数据之前，你首先需要一个数据库和至少一个表。创建数据库和表的方法如下：

```py
CREATE DATABASE sql_tutorial;
USE sql_tutorial;
CREATE TABLE customers (
  id INT PRIMARY KEY AUTO_INCREMENT, 
  name VARCHAR(50),
  email VARCHAR(50)
);
```

## 操作数据

现在你已经准备好进行数据操作。让我们看看基本的 [CRUD 操作](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)：

+   插入：`INSERT INTO customers (name, email) VALUES ('John Doe', 'john@email.com');`

+   查询：`SELECT * FROM customers;`

+   更新：`UPDATE customers SET email = 'john@newemail.com' WHERE id = 1;`

+   删除：`DELETE FROM customers WHERE id = 1;`

## 过滤和排序

SQL 中的过滤涉及使用条件选择性地检索表中的行，通常使用 `WHERE` 子句。SQL 中的排序将检索到的数据按特定顺序排列，通常使用 `ORDER BY` 子句。SQL 中的分页将结果集划分为更小的块，每页显示有限数量的行。

+   过滤：`SELECT * FROM customers WHERE name = 'John Doe';`

+   排序：`SELECT * FROM customers ORDER BY name ASC;`

+   分页：`SELECT * FROM customers LIMIT 10 OFFSET 20;`

## 数据类型与约束

理解数据类型和约束对于定义表的结构至关重要。数据类型指定列可以包含的数据种类，如整数、文本或日期。约束强制实施限制以确保数据的完整性。

+   **整数类型：** INT、SMALLINT、TINYINT 等。用于存储整数。

+   **十进制类型：** FLOAT、DOUBLE、DECIMAL。适合存储带小数的数字。

+   **字符类型：** CHAR、VARCHAR、TEXT。用于文本数据。

+   **日期和时间：** DATE、TIME、DATETIME、TIMESTAMP。用于存储日期和时间信息。

```py
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE,
    email VARCHAR(50) UNIQUE,
    salary FLOAT CHECK (salary > 0)
  );
```

在上述示例中，`NOT NULL` 约束确保列不能有 NULL 值。`UNIQUE` 约束保证列中的所有值都是唯一的。`CHECK` 约束验证工资必须大于零。

# 第 3 步：更高级的 SQL 概念

## 表连接

连接用于基于相关列将两张或多张表的行组合在一起。当你想检索分布在多个表中的数据时，它们是必不可少的。理解连接对复杂的 SQL 查询至关重要。

+   INNER JOIN：`SELECT * FROM orders JOIN customers ON orders.customer_id = customers.id;`

+   LEFT JOIN：`SELECT * FROM orders LEFT JOIN customers ON orders.customer_id = customers.id;`

+   RIGHT JOIN：`SELECT * FROM orders RIGHT JOIN customers ON orders.customer_id = customers.id;`

连接可能很复杂，但在需要从多个表中提取数据时，它们非常强大。让我们通过详细的示例来深入了解不同类型的连接如何工作。

考虑两个表：**员工** 和 **部门**。

```py
-- Employees Table
CREATE TABLE Employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  department_id INT
);

INSERT INTO Employees (id, name, department_id) VALUES
(1, 'Winifred', 1),
(2, 'Francisco', 2),
(3, 'Englebert', NULL);

-- Departments Table
CREATE TABLE Departments (
  id INT PRIMARY KEY,
  name VARCHAR(50)
);

INSERT INTO Departments (id, name) VALUES
(1, 'R&D'),
(2, 'Engineering'),
(3, 'Sales'); 
```

让我们探索不同类型的连接：

```py
-- INNER JOIN
-- Returns records that have matching values in both tables

SELECT E.name, D.name 
FROM Employees E
INNER JOIN Departments D ON E.department_id = D.id;

-- LEFT JOIN (or LEFT OUTER JOIN)
-- Returns all records from the left table,
-- and the matched records from the right table

SELECT E.name, D.name 
FROM Employees E
LEFT JOIN Departments D ON E.department_id = D.id;

-- RIGHT JOIN (or RIGHT OUTER JOIN)
-- Returns all records from the right table
-- and the matched records from the left table

SELECT E.name, D.name 
FROM Employees E
RIGHT JOIN Departments D ON E.department_id = D.id; 
```

在上述示例中，INNER JOIN 只返回两个表中都有匹配的行。LEFT JOIN 返回左表中的所有行，以及右表中匹配的行，如果没有匹配，则填充 NULL。RIGHT JOIN 则相反，返回右表中的所有行，以及左表中匹配的行。

## 分组与聚合

聚合函数对一组值进行计算，并返回单个值。聚合通常与 GROUP BY 子句一起使用，将数据分组并对每个组进行计算。

+   计数：`SELECT customer_id, COUNT(id) AS total_orders FROM orders GROUP BY customer_id;`

+   求和：`SELECT customer_id, SUM(order_amount) AS total_spent FROM orders GROUP BY customer_id;`

+   筛选组：`SELECT customer_id, SUM(order_amount) AS total_spent FROM orders GROUP BY customer_id HAVING total_spent > 100;`

## 子查询和嵌套查询

子查询允许你在查询中执行查询，为主查询提供一种方法，以便根据条件进一步限制检索到的数据。

```py
SELECT *
  FROM customers
  WHERE id IN (
    SELECT customer_id
    FROM orders
    WHERE orderdate > '2023-01-01'
  );
```

## 事务

事务是作为一个工作单元执行的一系列 SQL 操作。它们对维护数据库操作的完整性非常重要，特别是在多用户系统中。事务遵循 ACID 原则：原子性、一致性、隔离性和持久性。

```py
BEGIN;
  UPDATE accounts SET balance = balance - 500 WHERE id = 1;
  UPDATE accounts SET balance = balance + 500 WHERE id = 2;
  COMMIT;
```

在上面的示例中，两个 UPDATE 语句都被包含在一个事务中。要么两者都成功执行，要么出现错误时两者都不执行，从而确保数据的完整性。

# 第4步：优化和性能调优

## 理解查询性能

查询性能对维持一个响应快速的数据库系统至关重要。一个低效的查询可能导致延迟，影响整体用户体验。以下是一些关键概念：

+   **执行计划：** 这些计划提供了查询将如何执行的路线图，允许分析和优化。

+   **瓶颈：** 识别查询中缓慢的部分可以指导优化工作。像 SQL Server Profiler 这样的工具可以帮助进行这项工作。

## 索引策略

索引是提高数据检索速度的数据结构。在大型数据库中至关重要。它们的工作原理如下：

+   **单列索引：** 单列上的索引，通常用于 WHERE 子句；`CREATE INDEX idx_name ON customers (name);`

+   **复合索引：** 多列上的索引，用于根据多个字段过滤的查询；`CREATE INDEX idx_name_age ON customers (name, age);`

+   **了解何时索引：** 索引提高读取速度，但可能会降低插入和更新的速度。需要仔细考虑以平衡这些因素。

## 优化连接和子查询

连接和子查询可能会占用大量资源。优化策略包括：

+   **使用索引：** 在连接字段上应用索引可以提高连接性能。

+   **减少复杂性：** 尽量减少连接的表和选择的行数。

```py
SELECT customers.name, COUNT(orders.id) AS total_orders
  FROM customers
  JOIN orders ON customers.id = orders.customer_id
  GROUP BY customers.name
  HAVING orders > 2;
```

## 数据库规范化和反规范化

数据库设计在性能中扮演着重要角色：

+   **规范化：** 通过将数据组织到相关表中减少冗余。这可以使查询变得更复杂，但确保数据一致性。

+   **反规范化：** 合并表以提高读取性能，但可能会导致潜在的不一致性。用于读取速度优先的场景。

## 监控和分析工具

利用工具监控性能，确保数据库平稳运行：

+   **MySQL 性能模式：** 提供有关查询执行和性能的见解。

+   **SQL Server Profiler：** 允许跟踪和捕捉 SQL Server 事件，帮助分析性能。

## 编写高效 SQL 的最佳实践

遵循最佳实践使 SQL 代码更具可维护性和效率：

+   **避免使用 SELECT *：** 仅选择所需的列以减少负载。

+   **最小化通配符：** 在 LIKE 查询中谨慎使用通配符。

+   **使用 EXISTS 而不是 COUNT：** 检查存在时，EXISTS 更有效。

```py
SELECT id, name 
FROM customers 
WHERE EXISTS (
    SELECT 1 
    FROM orders 
    WHERE customer_id = customers.id
);
```

## 数据库维护

定期维护确保最佳性能：

+   **更新统计信息：** 帮助数据库引擎做出优化决策。

+   **重建索引：** 随着时间推移，索引会变得碎片化。定期重建可以提高性能。

+   **备份：** 定期备份对数据完整性和恢复至关重要。

# 第 5 步：性能和安全最佳实践

## 性能最佳实践

优化 SQL 查询和数据库的性能对于维持响应和高效的系统至关重要。以下是一些性能最佳实践：

+   **明智地使用索引：** 索引加速数据检索，但可能会降低数据修改操作（如插入、更新和删除）的速度。

+   **限制结果：** 使用 `LIMIT` 子句仅检索所需的数据。

+   **优化连接：** 始终在索引或主键列上连接表。

+   **分析查询计划：** 理解查询执行计划可以帮助你优化查询。

## 安全最佳实践

在处理数据库时，安全至关重要，因为它们通常包含敏感信息。以下是增强 SQL 安全性的最佳实践：

+   **数据加密：** 存储敏感数据前始终进行加密。

+   **用户权限：** 只授予用户完成任务所需的最小权限。

+   **防止 SQL 注入：** 使用参数化查询防护 SQL 注入攻击。

+   **定期审计：** 进行定期安全审计以识别漏洞。

## 结合性能和安全

在性能和安全之间找到正确的平衡通常具有挑战性但却是必要的。例如，虽然索引可以加速数据检索，但也可能使敏感数据更易获取。因此，始终考虑性能优化策略的安全影响。

## 示例：安全高效的查询

```py
-- Using a parameterized query to both optimize
-- performance and prevent SQL injection

PREPARE secureQuery FROM 'SELECT * FROM users WHERE age > ? AND age < ?';
SET @min_age = 18, @max_age = 35;
EXECUTE secureQuery USING @min_age, @max_age; 
```

这个示例使用了参数化查询，不仅可以防止 SQL 注入，还允许 MySQL 缓存查询，提高性能。

# 前进的方向

本入门指南涵盖了 SQL 的基本概念和流行的实际应用。从启动到掌握复杂查询，这本指南应该已经为你提供了通过详细示例和实际方法来驾驭数据管理的技能。随着数据继续塑造我们的世界，掌握 SQL 为各种领域打开了大门，包括数据分析、机器学习和软件开发。

随着你的进步，可以考虑通过额外资源扩展你的 SQL 技能。像 [w3schools SQL 教程](https://www.w3schools.com/sql/) 和 [SQLBolt 的 SQL 练习](https://sqlbolt.com/) 提供了额外的学习材料和练习。此外，[HackerRank 的 SQL 问题](https://www.hackerrank.com/domains/sql) 提供了目标导向的查询练习。无论你是在构建复杂的数据分析平台，还是开发下一代网络应用程序，SQL 都是你必定会经常使用的技能。记住，掌握 SQL 的过程是一条漫长的道路，这条道路因持续的实践和学习而更加丰富。

[**Matthew Mayo**](https://www.linkedin.com/in/mattmayo13/) ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 KDnuggets 的主编，Matthew 致力于使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、机器学习算法以及探索新兴的人工智能。他的使命是让数据科学社区中的知识变得更具普及性。Matthew 从 6 岁起就开始编程。

### 更多相关主题

+   [开始使用 SQL 快捷指南](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [5 步骤开始使用 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [5 步骤开始使用 Scikit-learn](https://www.kdnuggets.com/5-steps-getting-started-scikit-learn)

+   [5 步骤开始使用 Google Cloud Platform](https://www.kdnuggets.com/5-steps-google-cloud-platform)

+   [5 步骤开始使用 PyTorch](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

+   [开始使用自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)
