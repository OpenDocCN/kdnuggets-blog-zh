# 如何在 R 中导入数据

> 原文：[`www.kdnuggets.com/how-to-import-data-in-r`](https://www.kdnuggets.com/how-to-import-data-in-r)

![如何在 R 中导入数据](img/aee4f931fce6e560b423f3be3834b030.png)

编辑者提供的图像 | Midjourney

数据导入是使用 R 的第一步。你可以从 CSV 文件、文本文件和数据库等来源加载数据。每种来源都有其自己的导入方法。本文将解释如何将数据从几个来源导入到 R 中。

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求。

* * *

## 导入 CSV 文件

CSV 文件是一种常见的数据存储方式。R 提供了几种导入 CSV 文件的函数。最常用的是`read.csv()`和`readr::read_csv()`。`readr`包比 read.csv()更快，并且对数据类型的处理更好。

```py
library(readr)
data <- read_csv("path/to/your/file.csv") 
```

## 导入 Excel 文件

Excel 文件常用于存储电子表格中的数据。要将它们导入 R 中，使用`readxl`或`openxlsx`包。`readxl`包使读取 Excel 文件变得容易。使用`read_excel()`函数来加载你的数据。

```py
library(readxl)
data <- read_excel("path/to/your/file.xlsx") 
```

`openxlsx`包用于导入 Excel 文件。使用`read.xlsx()`函数。它提供了处理 Excel 文件的额外功能，如创建或修改文件。

```py
library(openxlsx)
data <- read.xlsx("path/to/your/file.xlsx", sheet = 1) 
```

## 导入文本文件

文本文件通常由分隔符（如制表符或自定义字符）分隔数据。R 可以使用如`read.table()`和`readr::read_delim()`这样的函数来处理这些文件。`readr`包的`read_delim()`通常在处理不同的分隔符时更快且更灵活。

```py
library(readr)
data <- read_delim("path/to/your/file.txt", delim = "\t") 
```

## 从在线来源导入数据

数据可以直接从 URL 导入到 R 中。这包括来自 URL、API 和在线数据库的数据。使用如`read.csv()`的函数来处理直接 URL，或使用`httr`等包处理 API 请求。

```py
data <- read.csv("http://example.com/data.csv") 
```

对于 JSON 和 XML 数据，使用如`jsonlite`和`xml2`的包。

```py
library(jsonlite)
data <- fromJSON("http://example.com/data.json") 
```

## 从数据库导入数据

要从数据库中导入数据到 R 中，安装并加载相关的包，如 RSQLite 或 RMySQL。使用`dbConnect()`连接到数据库。使用`dbGetQuery()`运行查询以获取数据。最后，使用`dbDisconnect()`关闭连接。

```py
library(DBI)
library(RSQLite)

con <- dbConnect(RSQLite::SQLite(), "path/to/your/database.sqlite")
data <- dbGetQuery(con, "SELECT * FROM your_table")
dbDisconnect(con) 
```

## 从 JSON 文件导入数据

首先，安装并加载`jsonlite`包。然后，使用`fromJSON()`函数读取你的 JSON 文件。该函数将 JSON 数据转换为 R 的数据框。

```py
library(jsonlite)
data <- fromJSON("path/to/your/file.json") 
```

## 从 API 导入数据

要从 API 导入数据到 R 中，首先需要安装并加载 `httr` 包。使用 `GET()` 函数向 API 发送请求，然后使用 content() 提取响应中的内容。

```py
library(httr)
response <- GET("https://api.example.com/data")
data <- content(response, "parsed") 
```

## 从 SAS 文件导入数据

SAS 文件在统计分析中很常见。要将它们导入到 R 中，可以使用 `haven` 或 `sas7bdat` 包。`haven` 包可以直接将 SAS 文件导入数据框，并保持变量标签和类型不变。

```py
library(haven)
data <- read_sas("path/to/your/file.sas7bdat") 
```

`sas7bdat` 包将 SAS 文件导入数据框，注重简单性和效率。

```py
library(sas7bdat)
data <- read.sas7bdat("path/to/your/file.sas7bdat") 
```

## 从 SPSS 文件导入数据

SPSS 文件在社会科学中常被使用。要将它们导入到 R 中，可以使用 `haven` 或 `foreign` 包。`haven` 包读取 SPSS 文件并保留变量和数值标签，这有助于更好地理解数据。

```py
library(haven)
data <- read_sav("path/to/your/file.sav") 
```

`foreign` 包还提供读取 SPSS 文件的功能，但可能无法保留太多的元数据。

```py
library(foreign)
data <- read.spss("path/to/your/file.sav", to.data.frame = TRUE) 
```

## 结论

将数据导入 R 对于开始任何分析都很重要。你可以使用合适的函数将来自不同来源的数据导入 R。这使你的工作更加轻松，并让你能更早地开始分析。

**[Jayita Gulati](https://www.linkedin.com/in/jayitagulati1998/)** 是一位机器学习爱好者和技术作家，她的热情驱动着她建立机器学习模型。她拥有利物浦大学计算机科学硕士学位。

### 更多相关话题

+   [数据科学职位导航：数据分析师 vs. 数据科学家…](https://www.kdnuggets.com/navigating-data-science-job-titles-data-analyst-vs-data-scientist-vs-data-engineer)

+   [数据科学家、数据工程师及其他数据职业解析](https://www.kdnuggets.com/2021/05/data-scientist-data-engineer-data-careers-explained.html)

+   [高保真合成数据适用于数据工程师和数据科学家](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [数据科学家 vs 数据分析师 vs 数据工程师](https://www.kdnuggets.com/2022/01/data-scientist-data-analyst-data-engineer.html)

+   [数据仓库 vs. 数据湖 vs. 数据集市：需要帮助决定吗？](https://www.kdnuggets.com/data-warehouses-vs-data-lakes-vs-data-marts-need-help-deciding)

+   [掌握 SQL、Python、数据清理、数据处理等的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
