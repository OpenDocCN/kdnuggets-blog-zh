# 在 Azure Databricks 上使用 Spark、Python 或 SQL

> 原文：[https://www.kdnuggets.com/2020/08/spark-python-sql-azure-databricks.html](https://www.kdnuggets.com/2020/08/spark-python-sql-azure-databricks.html)

[评论](#comments)

**作者 [Ajay Ohri](http://linkedin.com/in/ajayohri)，数据科学经理**

**Azure Databricks** 是一个基于 Apache Spark 的大数据分析服务，旨在支持数据科学和数据工程，由微软提供。它支持协作工作以及 Python、Spark、R 和 SQL 等多种语言的使用。在 Databricks 上工作具有云计算的优势——可扩展、低成本、按需的数据处理和存储。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 领域

* * *

在这里，我们探讨了一些在 Python、PySpark 和 SQL 之间互换工作的方式。我们学习如何通过先上传 CSV 文件然后在笔记本中创建来导入数据。我们学习如何将 SQL 表转换为 Spark 数据框，并将 Spark 数据框转换为 Python Pandas 数据框。我们还学习如何将 Spark 数据框转换为永久或临时 SQL 表。

为什么我们需要学习如何在 SQL、Spark 和 Python Pandas 数据框之间互换代码？SQL 适合编写简洁易读的数据处理代码，Spark 适合处理大数据和机器学习，而 Python Pandas 可用于从数据处理、机器学习到 seaborn 或 matplotlib 库中的绘图等各种任务。

![图片](../Images/10e3236941f0b760eb80a0b99e6a2abe.png)

我们选择 SQL 笔记本以便于操作，然后选择适当的集群及其内存、核心、Spark 版本等。即使是 SQL 笔记本，我们也可以通过在单元格中的代码前面输入 %python 来编写 Python 代码。

![图片](../Images/f90682a67b15f98366a13a8280832397.png)

现在让我们开始数据输入、数据检查和数据互换的基础知识。

**步骤 1** 读取上传的数据

```py
%python

# Reading in Uploaded Data
# File location and type
file_location = "/FileStore/tables/inputdata.csv"
file_type = "csv"

# CSV options
infer_schema = "false"
first_row_is_header = "true"
delimiter = ","

# The applied options are for CSV files. For other file types, these will be ignored.
df = spark.read.format(file_type) \
  .option("inferSchema", infer_schema) \
  .option("header", first_row_is_header) \
  .option("sep", delimiter) \
  .load(file_location)

display(df)
```

**步骤 2** 从 SPARK 数据框创建一个临时视图或表

```py
%python
#Create a temporary view or table from SPARK Dataframe
temp_table_name = "temp_table"
df.createOrReplaceTempView(temp_table_name)
```

**步骤 3** 从 SPARK 数据框创建永久 SQL 表

```py
--Creating Permanent SQL Table from SPARK Dataframe
permanent_table_name = "cdp.perm_table"

df.write.format("parquet").saveAsTable(permanent_table_name)
```

**步骤 4** 检查 SQL 表

```py
--Inspecting SQL Table
select * from cdp.perm_table
```

**步骤 5** 将 SQL 表转换为 SPARK 数据框

```py
%python
#Converting SQL Table to SPARK Dataframe
sparkdf = spark.sql("select *  from cdp.perm_table")
```

**步骤 6** 检查 SPARK 数据框

```py
%python
#Inspecting Spark Dataframe 
sparkdf.take(10)
```

**步骤 7** 将 Spark 数据框转换为 Python Pandas 数据框

```py
%python
#Converting Spark Dataframe to Python Pandas Dataframe
%python
pandasdf=sparkdf.toPandas()
```

**步骤 8** 检查 Python 数据框

```py
%python
#Inspecting Python Dataframe 
pandasdf.head()
```

**参考文献**

1.  Azure Databricks 简介 - [https://www.slideshare.net/jamserra/introduction-to-azure-databricks-83448539](https://www.slideshare.net/jamserra/introduction-to-azure-databricks-83448539)

1.  Dataframes 和 Datasets - [https://docs.databricks.com/spark/latest/dataframes-datasets/index.html](https://docs.databricks.com/spark/latest/dataframes-datasets/index.html)

1.  优化 PySpark 与 pandas DataFrames 之间的转换 - [https://docs.databricks.com/spark/latest/spark-sql/spark-pandas.html](https://docs.databricks.com/spark/latest/spark-sql/spark-pandas.html)

1.  pyspark 包 - [https://spark.apache.org/docs/latest/api/python/pyspark.html](https://spark.apache.org/docs/latest/api/python/pyspark.html)

**个人简介： [Ajay Ohri](http://linkedin.com/in/ajayohri)** 是数据科学经理（Publicis Sapient），并且是《R for Cloud Computing》和《Python for R Users》以及其他 4 本数据科学书籍的作者。

**相关**：

+   [Dataproc 上的 Apache Spark 与 Google BigQuery 比较](/2020/07/apache-spark-dataproc-vs-google-bigquery.html)

+   [使用 Kubernetes 对 PySpark 进行容器化](/2020/08/containerization-pyspark-kubernetes.html)

+   [数据科学的 5 个 Apache Spark 最佳实践](/2020/08/5-spark-best-practices-data-science.html)

### 更多相关主题

+   [优化数据分析：在 Databricks 中集成 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

+   [在 Python 中使用 SQLite 数据库的指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [处理大数据：工具和技术](https://www.kdnuggets.com/working-with-big-data-tools-and-techniques)

+   [在实际环境中实现深度学习：以数据为中心的课程](https://www.kdnuggets.com/2022/04/corise-deep-learning-wild-data-centric-course.html)

+   [远程工作的数据科学家需要的 6 种软技能](https://www.kdnuggets.com/2022/05/6-soft-skills-data-scientists-working-remotely.html)
