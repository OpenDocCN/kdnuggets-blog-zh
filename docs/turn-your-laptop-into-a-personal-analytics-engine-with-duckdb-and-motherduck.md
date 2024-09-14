# 将您的笔记本电脑变成一个个人分析引擎，使用 DuckDB 和 MotherDuck

> 原文：[https://www.kdnuggets.com/turn-your-laptop-into-a-personal-analytics-engine-with-duckdb-and-motherduck](https://www.kdnuggets.com/turn-your-laptop-into-a-personal-analytics-engine-with-duckdb-and-motherduck)

![将您的笔记本电脑变成一个个人分析引擎，使用 DuckDB 和 MotherDuck](../Images/dab672c2defb0da74ca87ccc8f5b6b4c.png)

图像由 DALL-E 生成

在数据分析处理成为成功商业与否的关键差异的时代，我们需要一个能够满足需求的工具栈。技术的进步帮助推进了我们所需的所有数据工具，即 DuckDB 和 MotherDuck。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

[DuckDB](https://duckdb.org/) 是一个开源的、进程内的 SQL 在线分析处理（OLAP）数据库管理系统。该数据库系统旨在快速处理数据分析查询，无论数据大小如何。该系统实现了内存处理和 OLAP 系统，能够有效提高我们的数据分析过程。

DuckDB 非常适合存储和处理涉及数据分析的表格数据（如表连接、数据汇总等），特别是当我们的工作流通常涉及表格的显著变化时。另一方面，DuckDB 不适合高容量数据活动和单个数据库中的多个并发进程。

[MotherDuck](https://motherduck.com/) 是一个托管的 DuckDB 云服务。它是免费的开源服务，由 DuckDB 社区维护。它是与 DuckDB Lab 合作建立的一个云服务平台，供公众使用。

结合 DuckDB 和 Motherduck，我们可以创建一个在各种场景中都能使用的分析引擎。我们怎么做呢？让我们深入了解一下。

# MotherDuck 界面

我们将使用原生的 MotherDuck 界面来演示服务如何工作，以及为什么 DuckDB 是一个强大的数据分析工具。如果你还没有，请在网站上注册并获取 MotherDuck 帐户。

一旦成功注册 MotherDuck 帐户，我们将进入 MotherDuck 界面。尝试熟悉这个界面，你会发现它与 Jupyter Notebook 非常相似（如果你曾使用过的话）。

我们将在 MotherDuck UI 中使用来自 [Kaggle](https://www.kaggle.com/datasets/henryshan/2023-data-scientists-salary) 的 DS Salary 数据实验 DBduck 功能。使用添加文件按钮上传数据，一个新的单元格将显示执行查询的内容。查询应如下所示。

```py
CREATE OR REPLACE TABLE ds_salaries AS SELECT * FROM read_csv_auto(['ds_salaries.csv']);
```

一旦创建了表格，请尝试使用以下`code`查询数据。

```py
select * from my_db.ds_salaries limit 10;
```

如你所见，MotherDuck 很像在 Notebook 中进行数据分析，只不过使用 SQL 查询。让我们尝试在 MotherDuck 中进行数据分析查询。

```py
select job_title, 
       avg(salary_in_usd) as average_salary_in_usd 
from my_db.ds_salaries
GROUP BY job_title
ORDER BY job_title
```

![将你的笔记本电脑转变为个人分析引擎，使用 DuckDB 和 MotherDuck](../Images/4aa1cc96d5c0b1fc3502fa59a49547af.png)

你可以在单元格中执行查询，表格结果将类似于下图所示。

![将你的笔记本电脑转变为个人分析引擎，使用 DuckDB 和 MotherDuck](../Images/609a640c93a5f33fb63e385349de723d.png)

你可以过滤数据、透视表格，或使用 UI 中的选择按钮下载结果。

# MotherDuck Python

MotherDuck 还允许用户通过 Notebook 上的 Python 访问数据库。我们需要使用以下代码安装 DuckDB 包。

```py
pip install duckdb==v0.9.2
```

MotherDuck 支持的当前版本是 DuckDB 0.9.2；这就是我们安装该版本的原因。

当安装成功后，我们需要将 DuckDB 连接到 Motherduck。虽然有几种方法可以验证连接，但我们将使用服务令牌。此令牌可以在你的 MotherDuck 设置中获取。

```py
import duckdb

token = "insert token here"
# initiate the MotherDuck connection
con = duckdb.connect(f'md:?motherduck_token={token}')
```

如果我们没有设置数据库名称，MotherDuck 将使用默认数据库，即 my_db。接下来，让我们使用之前在 Notebook 中使用的相同查询。

```py
q = """
select job_title,
       avg(salary_in_usd) as average_salary_in_usd
from my_db.ds_salaries
GROUP BY job_title
ORDER BY job_title
"""

con.sql(q).show()
```

你将看到类似于下表的输出。

```py
┌─────────────────────────────────────┬───────────────────────┐
│              job_title              │ average_salary_in_usd │
│               varchar               │        double         │
├─────────────────────────────────────┼───────────────────────┤
│ 3D Computer Vision Researcher       │              21352.25 │
│ AI Developer                        │     136666.0909090909 │
│ AI Programmer                       │               55000.0 │
│ AI Scientist                        │            110120.875 │
│ Analytics Engineer                  │    152368.63106796116 │
│ Applied Data Scientist              │              113726.3 │
│ Applied Machine Learning Engineer   │               99875.5 │
│ Applied Machine Learning Scientist  │    109452.83333333333 │
│ Applied Scientist                   │     190264.4827586207 │
│ Autonomous Vehicle Technician       │               26277.5 │
│            ·                        │                  ·    │
│            ·                        │                  ·    │
│            ·                        │                  ·    │
│ Principal Data Engineer             │              192500.0 │
│ Principal Data Scientist            │            198171.125 │
│ Principal Machine Learning Engineer │              190000.0 │
│ Product Data Analyst                │               56497.2 │
│ Product Data Scientist              │                8000.0 │
│ Research Engineer                   │    163108.37837837837 │
│ Research Scientist                  │    161214.19512195123 │
│ Software Data Engineer              │               62510.0 │
│ Staff Data Analyst                  │               15000.0 │
│ Staff Data Scientist                │              105000.0 │
├─────────────────────────────────────┴───────────────────────┤
│ 93 rows (20 shown)                                2 columns │
└─────────────────────────────────────────────────────────────┘
```

使用上述查询，你可以使用以下代码将其处理为 Pandas DataFrame。

```py
import pandas as pd

df = con.sql(q).fetchdf()
```

最后，你可以使用以下查询将另一个数据集加载到数据库中。

```py
con.sql("CREATE TABLE mytable AS SELECT * FROM '~/filepath.csv'")
```

上述查询假设你的数据是 CSV 文件。其他选项包括 S3 或本地 DuckDB 连接到 MotherDuck 数据库。

# 结论

DuckDB 是一个开源数据库系统，专门为数据分析而开发。该系统设计用于快速高效地处理数据。MotherDuck 是一个开源的托管云服务，用于 DuckDB。

通过结合 DuckDB 和 MotherDuck，我们可以将笔记本电脑变成个人分析引擎，将数据存储在云端，并使用 DuckDB 快速处理数据。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰写员。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 涉及各种 AI 和机器学习主题的写作。

### 更多相关内容

+   [构建视觉搜索引擎 - 第2部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [为什么DuckDB越来越受欢迎？](https://www.kdnuggets.com/2023/07/duckdb-getting-popular.html)

+   [通过openplayground轻松探索笔记本电脑上的LLMs](https://www.kdnuggets.com/2023/04/explore-llms-easily-laptop-openplayground.html)

+   [在笔记本电脑上使用LLMs的5种方法](https://www.kdnuggets.com/5-ways-to-use-llms-on-your-laptop)

+   [Inflection-1：个人AI的下一前沿](https://www.kdnuggets.com/2023/08/inflection1-next-frontier-personal-ai.html)

+   [通过Uplimit的“搜索与机器学习”课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)
