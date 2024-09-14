# 如何使用索引加速 SQL 查询 [Python 版]

> 原文：[https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)

![如何使用索引加速 SQL 查询 [Python 版]](../Images/6dd7f74dcf25f10a17a48f0c17ae157a.png)

图片由作者提供

假设你在翻阅一本书的页面。你想更快地找到你正在寻找的信息。你会怎么做？你可能会查找术语的索引，然后跳到引用特定术语的页面。**SQL 中的索引** 与书籍中的索引类似。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织 IT

* * *

在大多数实际系统中，你会对包含大量行（比如数百万行）的数据库表运行查询。需要对所有行进行全表扫描以检索结果的查询会非常慢。如果你知道经常需要基于某些列查询信息，你可以在这些列上创建数据库索引。这将显著加快查询速度。

那么今天我们将学习什么？我们将学习如何在 Python 中连接和查询 SQLite 数据库—使用 sqlite3 模块。我们还将学习如何添加索引以及如何提升性能。

要跟随本教程进行编码，你的工作环境中应安装 Python 3.7+ 和 SQLite。

**注意**：本教程中的示例和输出样本适用于 Ubuntu LTS 22.04 上的 Python 3.10 和 SQLite3（版本 3.37.2）。

# 在 Python 中连接到数据库

我们将使用 [内置的 sqlite3 模块](https://docs.python.org/3/library/sqlite3.html)。在开始运行查询之前，我们需要：

+   连接到数据库

+   创建一个数据库游标来运行查询

连接到数据库时，我们将使用来自 sqlite3 模块的 `connect()` 函数。一旦建立连接，我们可以在连接对象上调用 `cursor()` 来创建一个数据库游标，如下所示：

```py
import sqlite3

# connect to the db
db_conn = sqlite3.connect('people_db.db')
db_cursor = db_conn.cursor()
```

在这里我们尝试连接到数据库 `people_db`。如果数据库不存在，运行上述代码片段将为我们创建 SQLite 数据库。

# 创建表格并插入记录

现在，我们将在数据库中创建一个表格，并填充记录。

让我们在 `people_db` 数据库中创建一个名为 **people** 的表格，包含以下字段：

+   name

+   email

+   job

```py
# main.py
...
# create table
db_cursor.execute('''CREATE TABLE people (
                  id INTEGER PRIMARY KEY,
                  name TEXT,
                  email TEXT,
                  job TEXT)''')

...

# commit the transaction and close the cursor and db connection
db_conn.commit()
db_cursor.close()
db_conn.close()
```

## 使用 Faker 生成合成数据

现在我们必须将记录插入到表中。为此，我们将使用 [Faker](https://faker.readthedocs.io/)——一个用于合成数据生成的 Python 包——通过 [pip](https://pypi.org/project/pip/) 安装：

```py
$ pip install faker
```

安装 faker 后，你可以将 `Faker` 类导入到 Python 脚本中：

```py
# main.py
...
from faker import Faker
...
```

下一步是生成并插入记录到 people 表中。为了让我们了解索引如何加速查询，让我们插入大量记录。在这里，我们将插入 100K 条记录；将 `num_records` 变量设置为 100000。

然后，我们做以下操作：

+   实例化一个 `Faker` 对象 `fake` 并设置种子，以确保结果的可重现性。

+   使用名字和姓氏——通过在 `fake` 对象上调用 `first_name()` 和 `last_name()`——获取一个姓名字符串。

+   通过调用 `domain_name()` 生成一个虚假的域名。

+   使用名字和姓氏以及域名生成电子邮件字段。

+   使用 `job()` 为每个记录获取一个工作。

我们生成并插入记录到 `people` 表中：

```py
# create and insert records
fake = Faker() # be sure to import: from faker import Faker 
Faker.seed(42)

num_records = 100000

for _ in range(num_records):
    first = fake.first_name()
    last = fake.last_name()
    name = f"{first} {last}"
    domain = fake.domain_name()
    email = f"{first}.{last}@{domain}"
    job = fake.job()
    db_cursor.execute('INSERT INTO people (name, email, job) VALUES (?,?,?)', (name,email,job))

# commit the transaction and close the cursor and db connection
db_conn.commit()
db_cursor.close()
db_conn.close() 
```

现在 main.py 文件包含以下代码：

```py
# main.py
# imports
import sqlite3
from faker import Faker

# connect to the db
db_conn = sqlite3.connect('people_db.db')
db_cursor = db_conn.cursor()

# create table
db_cursor.execute('''CREATE TABLE people (
                  id INTEGER PRIMARY KEY,
                  name TEXT,
                  email TEXT,
                  job TEXT)''')

# create and insert records
fake = Faker()
Faker.seed(42)

num_records = 100000

for _ in range(num_records):
    first = fake.first_name()
    last = fake.last_name()
    name = f"{first} {last}"
    domain = fake.domain_name()
    email = f"{first}.{last}@{domain}"
    job = fake.job()
    db_cursor.execute('INSERT INTO people (name, email, job) VALUES (?,?,?)', (name,email,job))

# commit the transaction and close the cursor and db connection
db_conn.commit()
db_cursor.close()
db_conn.close()
```

运行这个脚本——一次——以用 `num_records` 数量的记录填充表格。

# 查询数据库

现在我们有了包含 100K 记录的表格，让我们对 `people` 表运行一个示例查询。

让我们运行一个查询来：

+   获取工作职位为 ‘Product manager’ 的记录的姓名和电子邮件，

+   将查询结果限制为 10 条记录。

我们将使用 time 模块的默认计时器来获取查询的大致执行时间。

```py
# sample_query.py

import sqlite3
import time

db_conn = sqlite3.connect("people_db.db")
db_cursor = db_conn.cursor()

t1 = time.perf_counter_ns()

db_cursor.execute("SELECT name, email FROM people WHERE job='Product manager' LIMIT 10;")

res = db_cursor.fetchall()
t2 = time.perf_counter_ns()

print(res)
print(f"Query time without index: {(t2-t1)/1000} us")
```

这是输出结果：

```py
Output >>
[
    ("Tina Woods", "Tina.Woods@smith.com"),
    ("Toni Jackson", "Toni.Jackson@underwood.com"),
    ("Lisa Miller", "Lisa.Miller@solis-west.info"),
    ("Katherine Guerrero", "Katherine.Guerrero@schmidt-price.org"),
    ("Michelle Lane", "Michelle.Lane@carr-hardy.com"),
    ("Jane Johnson", "Jane.Johnson@graham.com"),
    ("Matthew Odom", "Matthew.Odom@willis.biz"),
    ("Isaac Daniel", "Isaac.Daniel@peck.com"),
    ("Jay Byrd", "Jay.Byrd@bailey.info"),
    ("Thomas Kirby", "Thomas.Kirby@west.com"),
]

Query time without index: 448.275 us
```

你还可以通过在命令行中运行 `sqlite3 db_name` 来调用 SQLite 命令行客户端：

```py
$ sqlite3 people_db.db
SQLite version 3.37.2 2022-01-06 13:25:41
Enter ".help" for usage hints.
```

要获取索引列表，你可以运行 `.index`：

```py
sqlite> .index
```

由于当前没有索引，所以不会列出任何索引。

你还可以这样检查查询计划：

```py
sqlite> EXPLAIN QUERY PLAN SELECT name, email FROM people WHERE job='Product Manager' LIMIT 10;
QUERY PLAN
`--SCAN people
```

这里的查询计划是 *扫描所有行*，这效率较低。

# 在特定列上创建索引

要在特定列上创建数据库索引，你可以使用以下语法：

```py
CREATE INDEX index-name on table (column(s))
```

假设我们需要频繁查找具有特定职位名称的个人记录。创建一个 `people_job_index` 索引在 job 列上会有所帮助：

```py
# create_index.py

import time
import sqlite3

db_conn = sqlite3.connect('people_db.db')

db_cursor =db_conn.cursor()

t1 = time.perf_counter_ns()

db_cursor.execute("CREATE INDEX people_job_index ON people (job)")

t2 = time.perf_counter_ns()

db_conn.commit()

print(f"Time to create index: {(t2 - t1)/1000} us")

Output >>
Time to create index: 338298.6 us
```

尽管创建索引需要这么长时间，但这只是一次性操作。当运行多个查询时，你仍然会获得显著的加速。

现在如果你在 SQLite 命令行客户端运行 `.index`，你将得到：

```py
sqlite> .index
people_job_index
```

# 使用索引查询数据库

如果你现在查看查询计划，你应该能够看到我们现在使用 **job** 列上的索引 `people_job_index` 来搜索 `people` 表：

```py
sqlite> EXPLAIN QUERY PLAN SELECT name, email FROM people WHERE job='Product manager' LIMIT 10;
QUERY PLAN
`--SEARCH people USING INDEX people_job_index (job=?)
```

你可以重新运行 sample_query.py。只需修改 `print()` 语句，看看查询现在需要多长时间：

```py
# sample_query.py

import sqlite3
import time

db_conn = sqlite3.connect("people_db.db")
db_cursor = db_conn.cursor()

t1 = time.perf_counter_ns()

db_cursor.execute("SELECT name, email FROM people WHERE job='Product manager' LIMIT 10;")

res = db_cursor.fetchall()
t2 = time.perf_counter_ns()

print(res)
print(f"Query time with index: {(t2-t1)/1000} us")
```

这是输出结果：

```py
Output >>
[
    ("Tina Woods", "Tina.Woods@smith.com"),
    ("Toni Jackson", "Toni.Jackson@underwood.com"),
    ("Lisa Miller", "Lisa.Miller@solis-west.info"),
    ("Katherine Guerrero", "Katherine.Guerrero@schmidt-price.org"),
    ("Michelle Lane", "Michelle.Lane@carr-hardy.com"),
    ("Jane Johnson", "Jane.Johnson@graham.com"),
    ("Matthew Odom", "Matthew.Odom@willis.biz"),
    ("Isaac Daniel", "Isaac.Daniel@peck.com"),
    ("Jay Byrd", "Jay.Byrd@bailey.info"),
    ("Thomas Kirby", "Thomas.Kirby@west.com"),
]

Query time with index: 167.179 us
```

我们看到查询现在执行大约需要 167.179 微秒。

## 性能改进

对于我们的示例查询，使用索引查询大约快 2.68 倍。我们在执行时间上获得了 62.71% 的百分比加速。

你还可以尝试运行更多查询：涉及在 job 列上进行过滤的查询，并查看性能改进。

另外请注意：由于我们仅在工作列上创建了索引，如果你运行涉及其他列的查询，查询的速度*不会*比没有索引时快。

# 总结与下一步

我希望本指南帮助你了解如何通过在**频繁查询的列**上创建数据库索引来显著提高查询速度。这是关于数据库索引的介绍。你还可以创建多列索引、同一列的多个索引等。

你可以在[这个GitHub仓库](https://github.com/balapriyac/sql-index-intro)找到本教程中使用的所有代码。祝编程愉快！

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、观点文章等与开发者社区分享她的知识。

### 更多相关主题

+   [数据库优化：探索SQL中的索引](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)

+   [Python向量数据库和向量索引：架构LLM应用](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [数据科学中4个有用的中级SQL查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [5个棘手的SQL查询解决方案](https://www.kdnuggets.com/2020/11/5-tricky-sql-queries-solved.html)

+   [解决5个复杂SQL问题：棘手查询解释](https://www.kdnuggets.com/2022/07/5-hardest-things-sql.html)

+   [KDnuggets 新闻，12月7日：揭示数据科学的十大误区 • 4…](https://www.kdnuggets.com/2022/n47.html)
