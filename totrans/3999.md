# 使用 SQL 与 Python: SQLAlchemy 和 Pandas

> 原文：[https://www.kdnuggets.com/using-sql-with-python-sqlalchemy-and-pandas](https://www.kdnuggets.com/using-sql-with-python-sqlalchemy-and-pandas)

![使用 SQL 与 Python: SQLAlchemy 和 Pandas 封面图](../Images/514e8ddb11081ab64e092520e5706977.png)

作者提供的图像

作为数据科学家，你需要 Python 来进行详细的数据分析、数据可视化和建模。然而，当你的数据存储在关系数据库中时，你需要使用 SQL（结构化查询语言）来提取和操作数据。但你如何将 SQL 与 Python 集成以发挥数据的最终潜力呢？

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

在本教程中，我们将学习结合 SQL 的强大功能与 Python 的灵活性，使用 SQLAlchemy 和 Pandas。我们将学习如何连接数据库、使用 SQLAlchemy 执行 SQL 查询，以及使用 Pandas 分析和可视化数据。

使用以下命令安装 Pandas 和 SQLAlchemy：

```py
pip install pandas sqlalchemy
```

## 1\. 将 Pandas DataFrame 保存为 SQL 表

为了使用 CSV 数据集创建 SQL 表，我们将：

1.  使用 SQLAlchemy 创建一个 SQLite 数据库。

1.  使用 Pandas 加载 CSV 数据集。[countries_poluation](https://www.kaggle.com/datasets/willianoliveiragibin/countries-poluation) 数据集包含了 2017 年至 2023 年间全球所有国家的空气质量指数 (AQI)。

1.  将所有 AQI 列从对象类型转换为数值类型，并删除包含缺失值的行。

```py
# Import necessary packages
import pandas as pd
import psycopg2
from sqlalchemy import create_engine

# creating the new db
engine = create_engine(
    "sqlite:///kdnuggets.db")

# read the CSV dataset
data = pd.read_csv("/work/air_pollution new.csv")

col = ['2017', '2018', '2019', '2020', '2021', '2022', '2023']

for s in col:
    data[s] = pd.to_numeric(data[s], errors='coerce')

    data = data.dropna(subset=[s])
```

1.  将 Pandas dataframe 保存为 SQL 表。`to_sql` 函数需要一个表名和引擎对象。

```py
# save the dataframe as a SQLite table
data.to_sql('countries_poluation', engine, if_exists='replace')
```

结果是，你的 SQLite 数据库保存在你的文件目录中。

![Deepnote 文件管理器](../Images/421fd28094b44ace698fb0faef8e6223.png)

> **注意：** 我在本教程中使用 Deepnote 来无缝运行 Python 代码。Deepnote 是一个免费的 AI 云笔记本，它将帮助你快速运行任何数据科学代码。

## 2\. 使用 Pandas 加载 SQL 表

为了将整个 SQL 数据库表加载为 Pandas dataframe，我们将：

1.  通过提供数据库 URL 来建立与我们的数据库的连接。

1.  使用 `pd.read_sql_table` 函数加载整个表并将其转换为 Pandas dataframe。该函数需要表 anime、引擎对象和列名。

1.  显示前 5 行。

```py
import pandas as pd
import psycopg2
from sqlalchemy import create_engine

# establish a connection with the database
engine = create_engine("sqlite:///kdnuggets.db")

# read the sqlite table
table_df = pd.read_sql_table(
    "countries_poluation",
    con=engine,
    columns=['city', 'country', '2017', '2018', '2019', '2020', '2021', '2022',
       '2023']
)

table_df.head()
```

SQL 表已成功加载为数据框。这意味着你现在可以使用流行的 Python 包，如 Seaborn、Matplotlib、Scipy、Numpy 等来进行数据分析和可视化。

![国家空气污染 pandas 数据框](../Images/334a7809a6841a54b9db56493b4f5a3e.png)

## 3\. 使用 Pandas 运行 SQL 查询

我们可以使用 `pd.read_sql` 函数访问整个数据库，而不仅仅是限制在一个表中。只需编写简单的 SQL 查询并提供引擎对象。

SQL 查询将显示“countries_population”表中的两列，按“2023”列排序，并显示前 5 个结果。

```py
# read table data using sql query
sql_df = pd.read_sql(
    "SELECT city,[2023] FROM countries_poluation ORDER BY [2023] DESC LIMIT 5",
    con=engine
)

print(sql_df)
```

我们找到了世界上空气质量最差的前五个城市。

```py
 city  2023
0       Lahore  97.4
1        Hotan  95.0
2      Bhiwadi  93.3
3  Delhi (NCT)  92.7
4     Peshawar  91.9
```

## 4\. 使用 Pandas 处理 SQL 查询结果

我们还可以使用 SQL 查询结果进行进一步分析。例如，使用 Pandas 计算前五个城市的平均值。

```py
average_air = sql_df['2023'].mean()
print(f"The average of top 5 cities: {average_air:.2f}")
```

输出：

```py
The average of top 5 cities: 94.06
```

或者，通过指定 x 和 y 参数以及绘图类型来创建条形图。

```py
sql_df.plot(x="city",y="2023",kind = "barh");
```

![使用 pandas 进行数据可视化](../Images/641625a7e2204e9360c907cfbf20ba18.png)

## 结论

使用 SQLAlchemy 与 Pandas 的可能性是无限的。你可以使用 SQL 查询进行简单的数据分析，但要可视化结果或甚至训练机器学习模型，你必须将其转换为 Pandas 数据框。

在本教程中，我们学习了如何将 SQL 数据库加载到 Python 中，进行数据分析，并创建可视化。如果你喜欢本指南，你还会喜欢 '[在 Python 中使用 SQLite 数据库的指南](/a-guide-to-working-with-sqlite-databases-in-python)'，该指南深入探讨了如何使用 Python 的内置 sqlite3 模块。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证数据科学专家，热衷于构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为面临心理健康问题的学生开发人工智能产品。

### 更多相关主题

+   [使用 SQL 查询 Pandas 数据框](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [在 Pandas 中使用 Pandasql 执行 SQL](https://www.kdnuggets.com/sql-in-pandas-with-pandasql)

+   [如何在 Pandas 中选择行和列使用 [ ], .loc, iloc, .at…](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [如何通过索引加速 SQL 查询 [Python 版]](https://www.kdnuggets.com/2023/08/speed-sql-queries-indexes-python-edition.html)

+   [在 Pandas 数据框中使用 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [如何使用插值技术处理Pandas中的缺失数据](https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas)
