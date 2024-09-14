# SQL Group By 和 Partition By 场景：何时以及如何在数据科学中组合数据

> 原文：[https://www.kdnuggets.com/sql-group-by-and-partition-by-scenarios-when-and-how-to-combine-data-in-data-science](https://www.kdnuggets.com/sql-group-by-and-partition-by-scenarios-when-and-how-to-combine-data-in-data-science)

![SQL Group By 和 Partition By 场景：何时以及如何在数据科学中组合数据](../Images/b5b2d952dee44687f4b69129f0697533.png)

图片来源 [Freepik](https://www.freepik.com/free-vector/abstract-technology-sql-illustration_21743443.htm#query=sql&position=14&from_view=search&track=sph&uuid=27f89b21-e929-4fca-80ed-4c8c1cefdaa0)

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

SQL（结构化查询语言）是一种用于管理和操作数据的编程语言。这就是为什么SQL查询对于以结构化和高效的方式与数据库交互非常重要。

SQL中的分组作为组织和分析数据的强大工具。它有助于从复杂的数据集中提取有意义的见解和摘要。分组的最佳使用案例是总结和理解数据特征，从而帮助企业进行分析和报告任务。

我们通常有很多需求，需要通过共同的数据组合数据集记录，以计算组中的统计信息。这些实例大多数可以归纳为常见的场景。这些场景可以在类似需求出现时应用。

# SQL子句：Group By

SQL中的GROUP BY子句用于

1.  按某些列对数据进行分组

1.  将组缩减为一行

1.  对其他列进行聚合操作。

分组列 = 分组列中的值在组中的所有行应相同

聚合列 = 聚合列中的值通常是对应用了函数（如sum、max等）的不同值。

聚合列不应为分组列。

## 场景1：分组以查找总和

假设我们想要计算销售表中每个类别的总销售额。

因此，我们将按类别分组，并汇总每个类别的个体销售额。

```py
select category, 
sum(amount) as sales
from sales
group by category;
```

分组列 = 类别

聚合列 = 数量

聚合函数 = sum()

| **类别** | **销售额** |
| --- | --- |
| 玩具 | 10,700 |
| 书籍 | 4,200 |
| 健身器材 | 2,000 |
| 文具 | 1,400 |

## 场景2：分组以查找计数

假设我们想计算每个部门的员工数量。

在这种情况下，我们将按部门分组，并计算每个部门的员工数量。

```py
select department, 
count(empid) as emp_count
from employees
group by department;
```

分组列 = department

聚合列 = empid

聚合函数 = count

| **部门** | **员工数量** |
| --- | --- |
| finance | 7 |
| marketing | 12 |
| technology | 20 |

## 场景 3: 分组查找平均值

假设我们想计算每个部门员工的平均工资

类似地，我们将再次按部门分组，并分别计算每个部门员工的平均工资。

```py
select department, 
avg(salary) as avg_salary
from employees
group by department;
```

分组列 = department

聚合列 = salary

聚合函数 = avg

| **部门** | **平均工资** |
| --- | --- |
| finance | 2,500 |
| marketing | 4,700 |
| technology | 10,200 |

## 场景 4: 分组查找最大值 / 最小值

假设我们想计算每个部门员工的最高工资。

我们将按部门分组，并计算每个部门的最高工资。

```py
select department, 
max(salary) as max_salary
from employees
group by department;
```

分组列 = department

聚合列 = salary

聚合函数 = max

| **部门** | **最高工资** |
| --- | --- |
| finance | 4,000 |
| marketing | 9,000 |
| technology | 12,000 |

## 场景 5: 分组查找重复项

假设我们想找出数据库中重复或相同的客户姓名。

我们将按客户姓名分组，并使用 count 作为聚合函数。此外，我们将使用 having 子句对聚合函数进行筛选，只保留计数大于一的记录。

```py
select name, 
count(*) AS duplicate_count
from customers
group by name
having count(*) > 1;
```

分组列 = name

聚合列 = *

聚合函数 = count

Having = 应用于聚合函数的筛选条件

| **姓名** | **重复计数** |
| --- | --- |
| Jake Junning | 2 |
| Mary Moone | 3 |
| Peter Parker | 5 |
| Oliver Queen | 2 |

# SQL 子句: Partition By

SQL 中的 PARTITION BY 子句用于

1.  对某些列进行分组/分区数据

1.  单独的行被保留并**不**合并为一行

1.  在其他列上执行排名和聚合操作。

分区列 = 我们选择一个列来对数据进行分组。每个分组中的分区列的数据必须相同。如果未指定，则整个表被视为一个单一的分区。

排序列 = 对于基于分区列创建的每个组，我们将对组中的行进行排序/排序

排名函数 = 排名函数或聚合函数将应用于分区中的行

## 场景 6: 分区查找组中最高记录

假设我们想计算每个类别中销售最高的书籍 - 以及这本畅销书所带来的金额。

在这种情况下，我们不能使用分组子句 - 因为分组会将每个类别中的记录减少为一行。

然而，我们需要记录详细信息，如书名、金额等，以及类别，以查看每个类别中销售最高的书籍。

```py
select book_name, amount
row_number() over (partition by category order by amount) as sales_rank
from book_sales;
```

分区列 = category

排序列 = amount

排名函数 = row_number()

此查询返回 book_sales 表中的所有行，并按每个书籍类别中的销售量排序，最高销售书籍排在第 1 行。

现在我们需要筛选出只有行号 1 的行，以获取每个类别的畅销书

```py
select category, book_name, amount from (
select category, book_name, amount
row_number() over (partition by category order by amount) as sales_rank
from book_sales
) as book_ranked_sales
where sales_rank = 1;
```

上述筛选将仅给出每个类别中的畅销书及其销售额。

| **category** | **book_name** | **amount** |
| --- | --- | --- |
| science | The hidden messages in water | 20,700 |
| fiction | Harry Potter | 50,600 |
| spirituality | Autobiography of a Yogi | 30,800 |
| self-help | The 5 Love Languages | 12,700 |

## 场景 7：分区以查找组内累积总数

假设我们想计算销售的运行总数（累积总数）。我们需要为每个产品单独计算累积总数。

我们将按照 product_id 进行分区，并按日期对分区进行排序

```py
select product_id, date, amount,
sum(amount) over (partition by product_id order by date desc) as running_total
from sales_data;
```

分区列 = product_id

排序列 = date

排名函数 = sum()

| **product_id** | **date** | **amount** | **running_total** |
| --- | --- | --- | --- |
| 1 | 2023-12-25 | 3,900 | 3,900 |
| 1 | 2023-12-24 | 3,000 | 6,900 |
| 1 | 2023-12-23 | 2,700 | 9,600 |
| 1 | 2023-12-22 | 1,800 | 11,400 |
| 2 | 2023-12-25 | 2,000 | 2,000 |
| 2 | 2023-12-24 | 1,000 | 3,000 |
| 2 | 2023-12-23 | 7,00 | 3,700 |
| 3 | 2023-12-25 | 1,500 | 1,500 |
| 3 | 2023-12-24 | 4,00 | 1,900 |

## 场景 8：对比组内值的分区

假设我们想将每个员工的工资与其部门的平均工资进行比较。

我们将根据部门对员工进行分区，并计算每个部门的平均工资。

平均值还可以通过从员工个人工资中轻松减去来计算员工的工资是否高于或低于平均水平。

```py
select employee_id, salary, department,
avg(salary) over (partition by department) as avg_dept_sal
from employees;
```

分区列 = department

排序列 = 无排序

排名函数 = avg()

| **employee_id** | **salary** | **department** | **avg_dept_sal** |
| --- | --- | --- | --- |
| 1 | 7,200 | finance | 6,400 |
| 2 | 8,000 | finance | 6,400 |
| 3 | 4,000 | finance | 6,400 |
| 4 | 12,000 | technology | 11,300 |
| 5 | 15,000 | technology | 11,300 |
| 6 | 7,000 | technology | 11,300 |
| 7 | 4,000 | marketing | 5,000 |
| 8 | 6,000 | marketing | 5,000 |

## 场景 9：分区以将结果分成相等的组

假设我们想根据工资将员工分成 4 个相等（或近似相等）的组。

因此，我们将导出另一个逻辑列 tile_id，该列将包含每组员工的数字 ID。

组将根据工资创建——第一个 tile 组将拥有最高工资，以此类推。

```py
select employee_id, salary,
ntile(4) over (order by salary desc) as tile_id
from employees;
```

分区列 = 无分区 - 完整表格在同一分区中

排序列 = salary

排名函数 = ntile()

| **employee_id** | **salary** | **tile_id** |
| --- | --- | --- |
| 4 | 12,500 | 1 |
| 11 | 11,000 | 1 |
| 3 | 10,500 | 1 |
| 1 | 9,000 | 2 |
| 8 | 8,500 | 2 |
| 6 | 8,000 | 2 |
| 12 | 7,000 | 3 |
| 5 | 7,000 | 3 |
| 9 | 6,500 | 3 |
| 10 | 6,000 | 4 |
| 2 | 5,000 | 4 |
| 7 | 4,000 | 4 |

## 场景 10：分区以识别数据中的岛屿或间隙

假设我们有一个顺序的 product_id 列，我们想要识别其中的间隙。

所以我们将衍生出另一个逻辑列 island_id，如果 product_id 是顺序的，则具有相同的编号。当识别到 product_id 中的中断时， island_id 会递增。

```py
select product_id,
row_number() over (order by product_id) as row_num,
product_id - row_number() over (order by product_id) as island_id,
from products;
```

分区列 = 无分区 - 整个表在同一分区

排序列 = product_id

排序函数 = row_number()

| **product_id** | **row_num** | **island_id** |
| --- | --- | --- |
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 4 | 3 | 1 |
| 5 | 4 | 1 |
| 6 | 5 | 1 |
| 8 | 6 | 2 |
| 9 | 7 | 2 |

# 结论

Group By 和 Partition By 用于解决许多问题，例如：

**信息总结：** 分组允许你在每个组中汇总数据并总结信息。

**分析模式：** 有助于识别数据子集中的模式或趋势，为数据集的各个方面提供见解。

**统计分析：** 能够计算统计量，如平均值、计数、最大值、最小值和其他聚合函数在组内。

**数据清洗：** 有助于识别组内的重复项、不一致性或异常，使数据清洗和质量改进变得更加可控。

**队列分析：** 在基于队列的分析中非常有用，跟踪和比较随时间变化的实体组等。

**[Hanu](https://helpercodes.com/)** 运营着 [HelperCodes 博客](https://helpercodes.com/)，主要涉及 [SQL 备忘单](https://helpercodes.com/sql-query-cheatsheet-tutorial/)。我是一名全栈开发者，兴趣在于创建可重用的资产。

### 相关主题

+   [克服现实场景中数据不平衡的挑战](https://www.kdnuggets.com/2023/07/overcoming-imbalanced-data-challenges-realworld-scenarios.html)

+   [探索转移学习在小数据场景中的潜力](https://www.kdnuggets.com/exploring-the-potential-of-transfer-learning-in-small-data-scenarios)

+   [视觉 ChatGPT：微软结合 ChatGPT 和 VFM](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [KDnuggets 新闻，6月8日：21个数据科学备忘单…](https://www.kdnuggets.com/2022/n23.html)

+   [KDnuggets 新闻，12月7日：破解十大数据科学神话 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [KDnuggets™ 新闻 22:n03，1月19日：深入了解13个数据…](https://www.kdnuggets.com/2022/n03.html)
