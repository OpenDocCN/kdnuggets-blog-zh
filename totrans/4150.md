# 数据导入与 Pandas：初学者教程

> 原文：[https://www.kdnuggets.com/2022/04/data-ingestion-pandas-beginner-tutorial.html](https://www.kdnuggets.com/2022/04/data-ingestion-pandas-beginner-tutorial.html)

![数据导入与 Pandas：初学者教程](../Images/1ee6bcf1b4fdde28bdbc82763da6ffd9.png)

图片由作者提供

[Pandas](https://pandas.pydata.org/pandas-docs/stable/index.html) 是一个易于使用的开源数据分析工具，广泛用于数据分析、数据工程、数据科学和机器学习工程。它具有强大的功能，如数据清理与操作、支持流行的数据格式以及使用 matplotlib 的数据可视化。大多数数据科学学生只学习导入 CSV，但在工作中，你必须处理多种数据格式，如果这是你第一次做，事情可能会变得复杂。在本指南中，我们将重点介绍导入 CSV、Excel、SQL、HTML 和 JSON 数据集。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

# SQL

要运行 SQL 查询，我们需要下载用于 Kaggle [科技行业心理健康](https://www.kaggle.com/anth7310/mental-health-in-the-tech-industry) 的 SQLite 数据库，许可证为 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)。该数据库包含三个表：Questions、Answer 和 Survey。

![数据导入与 Pandas：初学者教程](../Images/fcc3a630e7e5303231d299d03ec017af.png)

SQL Schema | [Kaggle](https://www.kaggle.com/anth7310/mental-health-in-the-tech-industry)

要从任何 SQL 服务器导入数据，我们需要创建连接（SQLAlchemy 可连接对象 / sqlite3），编写 SQL 查询，并使用 Pandas 的 **read_sql_query()** 函数将输出转换为数据框。在我们的案例中，我们将首先使用 sqlite3 包连接 *mental_health.sqlite*，然后将对象传递给 **read_sql_query()** 函数。最后一步是编写查询以从 *Question* 表中导入所有列。如果你是 SQL 新手，我建议你通过参加一个免费的课程来学习基础知识：[Learn SQL | Codecademy](https://www.codecademy.com/learn/learn-sql)。

```py
import pandas as pd

import sqlite3

# Prepare a connection object
# Pass the Database name as a parameter
conn = sqlite3.connect("mental_health.sqlite")

# Use read_sql_query method
# Pass SELECT query and connection object as parameter
pdSql = pd.read_sql_query("SELECT * FROM Question", conn)
# display top 5 rows
pdSql.head()
```

我们已经成功将 SQL 查询转换为 Pandas 数据框。就是这么简单。

![数据导入与 Pandas：初学者教程](../Images/c5ba35f1464ccb7fe1bfe0724bf589ab.png)

# HTML

网络抓取在技术世界中是一项复杂且耗时的工作。你将使用 [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/)， [Selenium](https://www.selenium.dev/)，和 [Scrapy](https://scrapy.org/) 来提取和清理HTML数据。使用Pandas **read_html()**，你可以跳过所有步骤，直接将网站上的表格数据导入数据框。**这就是简单**。在我们的案例中，我们将抓取 [COVID-19 疫苗接种追踪器](https://www.pharmaceutical-technology.com/covid-19-vaccination-tracker/) 网站，以提取包含COVID19疫苗接种数据的表格。

![使用Pandas的数据导入：初学者教程](../Images/5a7258252c504ceb3c88b7340462c8eb.png)

COVID19 疫苗接种数据 | [制药技术](https://www.pharmaceutical-technology.com/covid-19-vaccination-tracker/)

仅使用**pd.read_html()**我们就能够从网站中提取数据。

```py
df_html = pd.read_html(
"https://www.pharmaceutical-technology.com/covid-19-vaccination-tracker/"
)[0]

df_html.head()
```

我们的初始输出是列表，若要将列表转换为数据框，我们在末尾使用了**[0]**。这只会显示列表中的第一个值。

**注意：** 你需要对初始结果进行实验，以获得最终的结果。

![使用Pandas的数据导入：初学者教程](../Images/34bebd7afd402c10af4a31cdcfd053d2.png)

# CSV

CSV 是数据科学中最常见的文件格式。它简单易用，可被多个Python包访问。你在数据科学课程中学到的第一件事就是导入CSV文件。在我们的案例中，我们使用的是Kaggle的 [共享单车数据集](https://www.kaggle.com/yasserh/bike-sharing-dataset)，其遵循 [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/) 许可证。CSV中的值由逗号分隔，如下所示。

![使用Pandas的数据导入：初学者教程](../Images/b84155762108f436c8280b8294bdbd73.png)

作者提供的图片

我们将使用**read_csv()**函数将数据集导入Pandas数据框。这个函数非常强大，因为我们可以解析日期、删除缺失值，并且只用一行代码就能进行大量数据清理。

```py
data_csv = pd.read_csv("day.csv")
data_csv.head()
```

我们成功加载了CSV文件并显示了前五行。

![使用Pandas的数据导入：初学者教程](../Images/19ba1f5d0bb8d671b6cc8f61cc4e6eb8.png)

# Excel

Excel表格在数据和业务分析专业人员中仍然很受欢迎。在我们的案例中，我们将使用Microsoft Excel将 [美国总统与债务](https://data.world/kevinnayar/us-presidents-and-debt) 数据集（由kevinnayar提供，遵循 [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/) 许可证）转换为**.xlsx**格式。我们的Excel文件包含两个工作表，但Pandas数据框是一个平面表，我们将使用**sheet_name**将选定的工作表导入Pandas数据框。

![使用Pandas的数据导入：初学者教程](../Images/821a0b955235e2d4cbe7152a69d2f3fe.png)

作者提供的图片

我们将使用**read_excel()**导入数据集：

+   第一个参数是文件路径。

+   第二是 sheet_name：在我们的案例中，我们正在导入第二个工作表。工作表编号从 0 开始。

+   第三是 index_col：由于我们的数据集包含索引列，为了避免重复，我们将提供**index_col=<column_name>**。

```py
data_excel = pd.read_excel("US_Presidents.xlsx",sheet_name = 1, index_col = "index")
data_excel.head()
```

![使用 Pandas 进行数据导入：初学者教程](../Images/1418e9de94d6d65f9bc3ddc20a8eb4c5.png)

# JSON

读取 JSON 文件相当棘手，因为有多种格式需要理解。有时，Pandas 无法导入嵌套 JSON 文件，因此我们需要执行手动步骤以完美导入文件。JSON 是科技行业最常见的文件格式。它受到网页开发者和数据工程师的青睐。在我们的案例中，我们将下载[Spotify 推荐](https://www.kaggle.com/bricevergnou/spotify-recommendation)数据集，许可证为[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)。该数据集包含好歌曲和坏歌曲的 JSON 文件。对于这个例子，我们将只使用*good.json* 文件。正如我们所见，我们正在处理一个嵌套的数据集。

![使用 Pandas 进行数据导入：初学者教程](../Images/7ab0d0abb065bac0dd87d52b6f0af72e.png)

作者提供的图片

在进行任何数据处理之前，让我们使用**read_json()**函数在不带参数的情况下导入数据集。

```py
df_json = pd.read_json("good.json")
df_json.head()
```

如我们所见，数据框只包含一列，而且数据杂乱无章。要调试此问题，我们需要导入原始数据集，然后进行解析。

![使用 Pandas 进行数据导入：初学者教程](../Images/2a3a237fa062aecb33d1cf6604080de6.png)

首先，我们将使用**json**包导入原始 JSON 文件，并仅选择*audio_features*子集。最后，我们将通过使用**json_normalize()**函数将 JSON 转换为 Pandas 数据框。

这是成功的，我们终于将 JSON 解析为数据框。如果你处理的是多层嵌套 JSON 文件，尝试先导入原始数据，然后处理数据，以便最终输出为平面表格。

```py
import json

with open('good.json') as data_file:    
    data = json.load(data_file) 

df = pd.json_normalize(data["audio_features"])
df.head()
```

![使用 Pandas 进行数据导入：初学者教程](../Images/8506a1eb58677611a614320fa5e275ff.png)

代码和所有数据集可以在 [**Deepnote**](https://deepnote.com/@abid/Data-Ingestion-with-Pandas-04Jajb5RSDWQY_RbkaJxjA)**.**

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个 AI 产品，帮助那些在精神健康方面遇到困难的学生。

### 更多相关话题

+   [初学者 Pandas 教程](https://www.kdnuggets.com/2022/03/introductory-pandas-tutorial.html)

+   [初学者的 Pandas Melt 函数指南](https://www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html)

+   [数据科学家的 Docker 教程](https://www.kdnuggets.com/2023/07/docker-tutorial-data-scientists.html)

+   [Pydantic 教程：简化 Python 数据验证](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)

+   [联邦学习：协作机器学习教程…](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)

+   [YOLOv5 PyTorch 教程](https://www.kdnuggets.com/2022/12/yolov5-pytorch-tutorial.html)
