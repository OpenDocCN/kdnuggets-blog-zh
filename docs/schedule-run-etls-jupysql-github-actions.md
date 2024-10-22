# 使用 Jupysql 和 GitHub Actions 安排和运行 ETL

> 原文：[`www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html`](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

![使用 Jupysql 和 GitHub Actions 安排和运行 ETL](img/4f0d1e0ad6d0308517fba43087bd543c.png)

作者提供的图片

在本博客中你将实现：

1.  了解 ETL 和 JupySQL 的基本概念

1.  使用公共企鹅数据集并执行 ETL。

1.  在 GitHub Actions 上安排我们构建的 ETL。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

# 介绍

在这份简短而信息丰富的指南中，我们旨在为你提供对 ETL（提取、转换、加载）和 JupySQL 这一灵活且多功能工具的基本概念的全面理解，该工具允许从 Jupyter 中无缝地进行基于 SQL 的 ETL 操作。

我们的主要关注点将是演示如何通过 JupySQL 这一流行且强大的 Python 库有效地执行 ETL 操作，同时强调通过 GitHub Actions 安排完整的 ETL 笔记本的自动化 ETL 过程的好处。

## 但首先，什么是 ETL？

现在，让我们深入了解细节。`ETL`（提取、转换、加载）是数据管理中至关重要的过程，它涉及从各种来源提取数据，将提取的数据转换为可用格式，并将转换后的数据加载到目标数据库或数据仓库中。它是数据分析、数据科学、数据集成和数据迁移等多个目的的关键过程。另一方面，JupySQL 是一个广泛使用的 Python 库，它通过 SQL 查询的强大功能简化了与数据库的交互。通过使用 JupySQL，数据科学家和分析师可以轻松执行 SQL 查询，操作数据框，并在 Jupyter 笔记本中与数据库交互。

## 为什么 ETL 很重要？

ETL 在数据分析和商业智能中扮演着重要角色。它们帮助企业从各种来源收集数据，包括社交媒体、网页、传感器以及其他内部和外部系统。通过这样做，企业可以获得其运营、客户和市场趋势的整体视图。

提取数据后，ETL 将其转换为结构化格式，例如关系数据库，这使企业能够轻松分析和操作数据。通过转换数据，ETL 可以清理、验证和标准化数据，使其更易于理解和分析。

最终，ETL 将数据加载到数据库或数据仓库中，企业可以轻松访问这些数据。通过这样做，ETL 使企业能够访问准确和最新的信息，从而做出明智的决策。

## 什么是 JupySQL？

[JupySQL](https://github.com/ploomber/jupysql) 是 Jupyter 笔记本的一个扩展，允许您使用 SQL 查询与数据库进行交互。它提供了一种方便的方式，直接从 Jupyter 笔记本访问数据库和数据仓库，使您能够执行复杂的数据操作和分析。

JupySQL 支持多种数据库管理系统，包括 SQLite、MySQL、PostgreSQL、DuckDB、Oracle、Snowflake 等（查看左侧的集成部分了解更多信息）。您可以使用标准连接字符串或通过环境变量连接到数据库。

## 为什么选择 JupySQL？

JupySQL 是一个强大的工具，便于在 Jupyter 笔记本中直接使用 SQL 查询与数据库进行交互。为了高效和准确地执行数据提取和转换过程，在通过 JupySQL 执行 ETL 时有几个关键因素需要考虑。JupySQL 为用户提供了与数据源交互并轻松进行数据转换的必要工具。为了节省宝贵的时间和精力，同时确保一致性和可靠性，通过 GitHub Actions 调度完整的 ETL 笔记本来自动化 ETL 过程可能是一个改变游戏规则的选择。通过利用 JupySQL，用户可以实现数据交互性（Jupyter）和易用性及 SQL 连接性（JupySQL）的最佳结合，从而简化数据管理过程，使数据科学家和分析师能够专注于他们的核心能力——生成有价值的见解和报告。

## 开始使用 JupySQL

要使用 JupySQL，您需要使用 pip 安装它。您可以运行以下命令：

```py
!pip install jupysql --quiet
```

安装后，您可以使用以下命令在 Jupyter 笔记本中加载扩展：

```py
%load_ext sql
```

加载扩展后，您可以使用以下命令连接到数据库：

```py
%sql dialect://username:password@host:port/database
```

例如，要连接到本地 DuckDB 数据库，您可以使用以下命令：

```py
%sql duckdb://
```

# 使用 JupySQL 执行 ETL

使用 JupySQL 执行 ETL 时，我们将遵循标准 ETL 流程，其中包括以下步骤：

1.  提取数据

1.  转换数据

1.  加载数据

1.  提取数据

## 提取数据

要使用 JupySQL 提取数据，我们需要连接到源数据库并执行查询以检索数据。例如，要从 MySQL 数据库中提取数据，我们可以使用以下命令：

```py
%sql mysql://username:password@host:port/database
data = %sql SELECT * FROM mytable
```

该命令使用指定的连接字符串连接到 MySQL 数据库，并从“mytable”表中检索所有数据。数据存储在名为“data”的变量中，作为 Pandas DataFrame。

**注意**：我们还可以使用 `%%sql df <<` 将数据保存到 df 变量中

由于我们将通过 DuckDB 在本地运行，我们可以直接提取一个公共数据集并立即开始工作。我们将获取我们的样本数据集（我们将通过 csv 文件处理企鹅数据集）：

```py
from urllib.request import urlretrieve

_ = urlretrieve(
   "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/penguins.csv",
   "penguins.csv",
)
```

我们可以获取数据的样本，以检查我们是否已连接并且可以查询数据：

```py
SELECT *
FROM penguins.csv
LIMIT 3
```

## 转换数据

提取数据后，通常需要将其转换为更适合分析的格式。此步骤可能包括数据清理、数据过滤、数据聚合和合并来自多个来源的数据。以下是一些常见的数据转换技术：

+   **清理数据**：数据清理涉及删除或修复数据中的错误、不一致性或缺失值。例如，你可能会删除缺失值的行，将缺失值替换为均值或中位数，或修正错别字或格式错误。

+   **过滤数据**：数据过滤涉及选择符合特定标准的数据子集。例如，你可以过滤数据，仅包含特定日期范围的记录，或符合某一阈值的记录。

+   **聚合数据**：数据聚合涉及通过计算统计数据（如某个变量的总和、均值、中位数或计数）来总结数据。例如，你可以按月或按产品类别聚合销售数据。

+   **合并数据**：数据合并涉及将来自多个来源的数据合并为一个数据集。例如，你可以合并关系数据库中不同表的数据，或合并不同文件中的数据。

在 JupySQL 中，你可以使用 Pandas DataFrame 方法进行数据转换，也可以使用原生 SQL。例如，你可以使用 rename 方法重命名列，使用 dropna 方法删除缺失值，使用 astype 方法转换数据类型。我将演示如何使用 pandas 或 SQL 来完成这些操作。

+   注意：你可以使用 `%sql` 或 `%%sql`，了解它们之间的区别，请查看 [这里](https://jupysql.ploomber.io/en/latest/community/developer-guide.html?highlight=%25sql%20vs%20%25%25sql#magics-e-g-sql-sql-etc)

这是一个如何使用 Pandas 和 JupySQL 替代方案进行数据转换的示例：

```py
# Rename columns
df = data.rename(columns={'old_column_name': 'new_column_name'})  # Pandas
%%sql df <<
SELECT *, old_column_name
AS new_column_name
FROM data;  # JupySQL
```

```py
# Remove missing values
data = data.dropna()  # Pandas
%%sql df <<
SELECT *
FROM data
WHERE column_name IS NOT NULL;  # JupySQL single column, can add conditions to all columns as needed.
```

```py
# Convert data types
data['date_column'] = data['date_column'].astype('datetime64[ns]')  # Pandas
%sql df <<
SELECT *,
CAST(date_column AS timestamp) AS date_column
FROM data  # Jupysql
```

```py
# Filter data
filtered_data = data[data['sales'] > 1000]  # Pandas
%%sql df <<
SELECT * FROM data
WHERE sales > 1000;  # JupySQL
```

```py
# Aggregate data
monthly_sales = data.groupby(['year', 'month'])['sales'].sum()  # Pandas
%%sql df <<
SELECT year, month,
SUM(sales) as monthly_sales
FROM data
GROUP BY year, month  # JupySQL
```

```py
# Combine data
merged_data = pd.merge(data1, data2, on='key_column')  # Pandas
%%sql df <<
SELECT * FROM data1
JOIN data2
ON data1.key_column = data2.key_column;  # JupySQL
```

在我们的示例中，我们将使用简单的转换方式，类似于上述代码。我们将清理数据中的 NA，并将一列（species）拆分成 3 列（分别命名为每种物种）：

```py
# Combine data
merged_data = pd.merge(data1, data2, on='key_column')  # Pandas
%%sql df <<
SELECT * FROM data1
JOIN data2
ON data1.key_column = data2.key_column;  # JupySQL
```

```py
SELECT *
FROM penguins.csv
WHERE species IS NOT NULL AND island IS NOT NULL AND bill_length_mm IS NOT NULL AND bill_depth_mm IS NOT NULL
AND flipper_length_mm IS NOT NULL AND body_mass_g IS NOT NULL AND sex IS NOT NULL;
```

```py
# Map the species column into classifiers
transformed_df = transformed_df.DataFrame().dropna()
transformed_df["mapped_species"] = transformed_df.species.map(
   {"Adelie": 0, "Chinstrap": 1, "Gentoo": 2}
)
transformed_df.drop("species", inplace=True, axis=1)

# Checking our transformed data
transformed_df.head() 
```

## 加载数据

转换数据后，我们需要将数据加载到目标数据库或数据仓库中。我们可以使用 `ipython-sql` 连接到目标数据库并执行 SQL 查询以加载数据。例如，要将数据加载到 PostgreSQL 数据库中，我们可以使用以下命令：

```py
%sql postgresql://username:password@host:port/database
%sql DROP TABLE IF EXISTS mytable;
%sql CREATE TABLE mytable (column1 datatype1, column2 datatype2, ...);
%sql COPY mytable FROM '/path/to/datafile.csv' DELIMITER ',' CSV HEADER;
```

此命令使用指定的连接字符串连接到 PostgreSQL 数据库，删除“mytable”表（如果存在），创建一个具有指定列和数据类型的新表，并从 CSV 文件中加载数据。

由于我们的用例是本地使用 DuckDB，我们可以简单地将新创建的 `transformed_df` 保存为 CSV 文件，但我们也可以使用上面的代码片段根据我们的用例将其保存到数据库或数据仓库中。

运行以下步骤以将新数据保存为 CSV 文件：

```py
transformed_df.to_csv("transformed_data.csv")
```

我们可以看到一个名为 `transformed_data.csv` 的新文件被创建了。在下一步中，我们将看到如何自动化这个过程并通过 GitHub 消耗最终文件。

# GitHub actions 上的调度

我们流程中的最后一步是通过 GitHub actions 执行完整的 notebook。为此，我们可以使用 `ploomber-engine`，它允许你调度 notebooks，以及其他 notebook 功能，如分析、调试等。如果需要，我们可以向 notebook 传递外部参数，并将其制作成通用模板。

+   注意：我们的 notebook 文件正在加载一个公共数据集，并在本地 ETL 后保存，我们可以轻松地更改为使用任何数据集，并将其加载到 S3，作为仪表盘可视化数据等。

对于我们的示例，我们可以使用这个示例 ci.yml 文件（这是设置 GitHub 工作流的文件），并将其放入我们的代码库中，最终文件应位于 `.github/workflows/ci.yml` 下。

`ci.yml` 文件的内容：

```py
name: CI

on:
 push:
 pull_request:
 schedule:
   - cron: '0 0 4 * *'

# These permissions are needed to interact with GitHub's OIDC Token endpoint.
permissions:
 id-token: write
 contents: read

jobs:
 report:
   runs-on: ubuntu-latest
   steps:
   - uses: actions/checkout@v3
   - name: Set up Python ${{ matrix.python-version }}
     uses: conda-incubator/setup-miniconda@v2
     with:
       python-version: '3.10'
       miniconda-version: latest
       activate-environment: conda-env
       channels: conda-forge, defaults

   - name: Run notebook
     env:
       PLOOMBER_STATS_ENABLED: false
       PYTHON_VERSION: '3.10'
     shell: bash -l {0}
     run: |
       eval "$(conda shell.bash hook)"

       # pip install -r requirements.txt
       pip install jupysql pandas ploomber-engine --quiet
       ploomber-engine --log-output posthog.ipynb report.ipynb

   - uses: actions/upload-artifact@v3
     if: always()
     with:
       name: Transformed_data
       path: transformed_data.csv
```

在这个 CI 示例中，我还添加了一个计划触发器，这个任务将每晚凌晨 4 点运行。

# 结论

ETL 是数据分析和商业智能的重要过程。它们帮助企业从各种来源收集、转换和加载数据，使分析和做出明智决策变得更加容易。JupySQL 是一个强大的工具，允许你直接在 Jupyter notebooks 中使用 SQL 查询与数据库交互。结合 GitHub actions，我们可以创建强大的工作流，这些工作流可以被调度并帮助我们将数据处理到最终阶段。

使用 JupySQL，你可以轻松高效地执行 ETL，允许你以结构化格式提取、转换和加载数据，同时 GitHub actions 分配计算资源并设置环境。

**[Ido Michael](https://www.linkedin.com/in/ido-michael/)** 共同创办了 Ploomber，旨在帮助数据科学家更快地构建。他曾在 AWS 领导数据工程/科学团队。在这些客户互动中，他和团队一起单独构建了数百个数据管道。来自以色列的他来到纽约攻读哥伦比亚大学的硕士学位。他专注于构建 Ploomber，因为他发现项目通常将大约 30% 的时间用于将开发工作（原型）重构为生产管道。

### 更多相关主题

+   [GitHub Actions For Machine Learning Beginners](https://www.kdnuggets.com/github-actions-for-machine-learning-beginners)

+   [为 ML 构建 ETL 的最佳实践](https://www.kdnuggets.com/best-practices-for-building-etls-for-ml)

+   [开始使用 PyTest：轻松编写和运行 Python 测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [用 llamafile 以 5 个简单步骤分发和运行 LLM](https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps)

+   [学习如何在您的设备上仅需几个步骤运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [使用 LM Studio 在本地运行 LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)
