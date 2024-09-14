# SQL中的数据清理：如何为分析准备混乱的数据

> 原文：[https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)

![SQL中的数据清理：如何为分析准备混乱的数据](../Images/cefee85bcf1824dbe72931f73db523ea.png)

使用Segmind SSD-1B模型生成的图像

想要开始使用SQL分析数据吗？那么，你可能需要稍等片刻。但为什么呢？

数据库表中的数据往往很混乱。你的数据可能包含缺失值、重复记录、异常值、不一致的数据条目等。因此，在使用SQL分析数据之前清理数据是非常重要的。

当你学习SQL时，你可以创建数据库表，修改它们，更新和删除记录。但在实际操作中，几乎不会出现这种情况。你可能没有权限修改表格、更新和删除记录。但你会有读取数据库的权限，并能够运行一系列SELECT查询。

在本教程中，我们将创建一个数据库表，填充记录，并查看如何使用SQL清理数据。让我们开始编码吧！

# 创建带有记录的数据库表

在本教程中，让我们创建一个如下的`employees`表：

```py
-- Create the employees table
CREATE TABLE employees (
	employee_id INT PRIMARY KEY,
	employee_name VARCHAR(50),
	salary DECIMAL(10, 2),
	hire_date VARCHAR(20),
	department VARCHAR(50)
);
```

接下来，让我们向表中插入一些虚构的样本记录：

```py
-- Insert 20 sample records 
INSERT INTO employees (employee_id, employee_name, salary, hire_date, department) VALUES
(1, 'Amy West', 60000.00, '2021-01-15', 'HR'),
(2, 'Ivy Lee', 75000.50, '2020-05-22', 'Sales'),
(3, 'joe smith', 80000.75, '2019-08-10', 'Marketing'), 
(4, 'John White', 90000.00, '2020-11-05', 'Finance'),
(5, 'Jane Hill', 55000.25, '2022-02-28', 'IT'),
(6, 'Dave West', 72000.00, '2020-03-12', 'Marketing'),
(7, 'Fanny Lee', 85000.50, '2018-06-25', 'Sales'),
(8, 'Amy Smith', 95000.25, '2019-11-30', 'Finance'),
(9, 'Ivy Hill', 62000.75, '2021-07-18', 'IT'),
(10, 'Joe White', 78000.00, '2022-04-05', 'Marketing'),
(11, 'John Lee', 68000.50, '2018-12-10', 'HR'),
(12, 'Jane West', 89000.25, '2017-09-15', 'Sales'),
(13, 'Dave Smith', 60000.75, '2022-01-08', NULL),
(14, 'Fanny White', 72000.00, '2019-04-22', 'IT'),
(15, 'Amy Hill', 84000.50, '2020-08-17', 'Marketing'),
(16, 'Ivy West', 92000.25, '2021-02-03', 'Finance'),
(17, 'Joe Lee', 58000.75, '2018-05-28', 'IT'),
(18, 'John Smith', 77000.00, '2019-10-10', 'HR'),
(19, 'Jane Hill', 81000.50, '2022-03-15', 'Sales'),
(20, 'Dave White', 70000.25, '2017-12-20', 'Marketing');
```

如果你能看出来，我使用了一小组名字来从中采样并构建记录的名字字段。不过，你可以在记录中更具创意。

**注意**：本教程中的所有查询都适用于[MySQL](https://www.mysql.com/)。但你可以自由使用你选择的RDBMS。

# 1\. 缺失值

数据记录中的缺失值总是一个问题。因此，你必须相应地处理它们。

一种幼稚的方法是删除所有包含一个或多个字段缺失值的记录。然而，除非你确定没有其他更好的处理缺失值的方法，否则不应该这样做。

在`employees`表中，我们看到‘department’列中有一个NULL值（见employee_id 13的那一行），这表示该字段缺失：

```py
SELECT * FROM employees;
```

![SQL中的数据清理：如何为分析准备混乱的数据](../Images/182a6d2a11cfa7a62989c7966dae25e5.png)

你可以使用[COALESCE()函数](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#function_coalesce)将‘Unknown’字符串用于NULL值：

```py
SELECT
	employee_id,
	employee_name,
	salary,
	hire_date,
	COALESCE(department, 'Unknown') AS department
FROM employees;
```

运行上述查询应给你以下结果：

![SQL中的数据清理：如何为分析准备混乱的数据](../Images/0c9890c3c0762ab25515a03e3561d51e.png)

# 2\. 重复记录

数据库表中的重复记录可能会扭曲分析结果。我们选择了employee_id作为数据库表中的主键。因此，`employee_data`表中不会有重复的员工记录。

你仍然可以使用SELECT DISTINCT语句：

```py
SELECT DISTINCT * FROM employees;
```

如预期的那样，结果集包含了所有20条记录：

![SQL中的数据清洗：如何准备混乱的数据进行分析](../Images/e5d4d878576c2bc007f8781ef7f5a7ac.png)

# 3\. 数据类型转换

如果你注意到，‘hire_date’ 列当前是VARCHAR而不是日期类型。为了在处理日期时更方便，使用 [STR_TO_DATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_str-to-date) 函数会很有帮助，如下所示：

```py
SELECT
	employee_id,
	employee_name,
	salary,
	STR_TO_DATE(hire_date, '%Y-%m-%d') AS hire_date,
	department
FROM employees;
```

在这里，我们仅选择了‘hire_date’列，并未对日期值进行任何操作。因此，查询输出应与之前的查询相同。

但如果你想执行例如添加偏移日期到值的操作，这个函数可能会有帮助。

# 4\. 异常值

一个或多个数字字段中的异常值可能会影响分析。因此，我们应检查并移除异常值，以过滤掉不相关的数据。

但决定哪些值构成异常值需要领域知识和数据使用领域及历史数据的知识。

在我们的例子中，假设我们 *知道* ‘salary’ 列的上限为100000。因此，‘salary’ 列中的任何条目最多为100000。超过此值的条目为异常值。

我们可以通过运行以下查询来检查这些记录：

```py
SELECT *
FROM employees
WHERE salary > 100000;
```

如上所示，‘salary’ 列中的所有条目都是有效的。因此结果集为空：

![SQL中的数据清洗：如何准备混乱的数据进行分析](../Images/0c5dd3614ce2ebe99e7e9742d9e30e4e.png)

# 5\. 数据条目不一致

数据条目和格式不一致在日期和字符串列中尤为常见。

在 `employees` 表中，我们看到对应员工‘joe smith’的记录不是标题大小写。

但为了保持一致性，我们选择所有格式为标题大小写的名称。你需要使用 [CONCAT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_concat) 函数与 [UPPER()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_upper) 和 [SUBSTRING()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring) 函数一起使用，如下所示：

```py
SELECT
	employee_id,
	CONCAT(
    	UPPER(SUBSTRING(employee_name, 1, 1)), -- Capitalize the first letter of the first name
    	LOWER(SUBSTRING(employee_name, 2, LOCATE(' ', employee_name) - 2)), -- Make the rest of the first name lowercase
    	' ',
    	UPPER(SUBSTRING(employee_name, LOCATE(' ', employee_name) + 1, 1)), -- Capitalize the first letter of the last name
    	LOWER(SUBSTRING(employee_name, LOCATE(' ', employee_name) + 2)) -- Make the rest of the last name lowercase
	) AS employee_name_title_case,
	salary,
	hire_date,
	department
FROM employees;
```

![SQL中的数据清洗：如何准备混乱的数据进行分析](../Images/f20a9e444f3ac9c7ff43f46a632d57a9.png)

# 6\. 验证范围

在讨论异常值时，我们提到希望将‘salary’列的上限设为100000，并将任何超过100000的工资条目视为异常值。

但同样重要的是，你不希望‘salary’列中出现任何负值。因此，你可以运行以下查询来验证所有员工记录的值是否在0到100000之间：

```py
SELECT
	employee_id,
	employee_name,
	salary,
	hire_date,
	department
FROM employees
WHERE salary < 0 OR salary > 100000;
```

如上所示，结果集为空：

![SQL中的数据清洗：如何准备混乱的数据进行分析](../Images/b58b8b318272b4a2d110c21337adb4fc.png)

# 7\. 派生新列

派生新列不一定是数据清洗步骤。然而，在实际操作中，你可能需要使用现有列来派生对分析更有帮助的新列。

例如，`employees` 表包含一个 'hire_date' 列。一个更有帮助的字段可能是 'years_of_service' 列，表示员工在公司工作的时间长短。

以下查询计算当前年份与 'hire_date' 年份之间的差异，以计算 'years_of_service'：

```py
SELECT
	employee_id,
	employee_name,
	salary,
	hire_date,
	department,
	YEAR(CURDATE()) - YEAR(hire_date) AS years_of_service
FROM employees;
```

你应该看到以下输出：

![SQL 数据清理：如何为分析准备混乱数据](../Images/dcf584bdbc66099479f9feeac668d13f.png)

与我们运行的其他查询一样，这不会修改原始表。要向原始表中添加新列，你需要具有 ALTER 数据库表的权限。

# 总结

我希望你了解相关的数据清理任务如何提升数据质量并促进更相关的分析。你已经学会了如何检查缺失值、重复记录、不一致的格式、异常值等。

尝试启动你自己的关系数据库表，并运行一些查询以执行常见的数据清理任务。接下来，了解一下[数据可视化的 SQL](https://www.kdnuggets.com/sql-for-data-visualization-how-to-prepare-data-for-charts-and-graphs)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交集处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正在通过编写教程、操作指南、观点文章等与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。**

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多内容

+   [数据可视化的 SQL：如何为图表和图形准备数据](https://www.kdnuggets.com/sql-for-data-visualization-how-to-prepare-data-for-charts-and-graphs)

+   [通过 AI 和分析引擎更快地准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [为有效的 Tableau 和 Power BI 仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)

+   [如何准备数据科学面试](https://www.kdnuggets.com/2022/12/prepare-data-science-interview.html)

+   [掌握 SQL、Python、数据清洗、数据处理与探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据清洗在数据科学中的重要性](https://www.kdnuggets.com/2023/08/importance-data-cleaning-data-science.html)
