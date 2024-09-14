# Python数据科学中的Pandas与Spark DataFrame：关键区别

> 原文：[https://www.kdnuggets.com/2016/01/python-data-science-pandas-spark-dataframe-differences.html](https://www.kdnuggets.com/2016/01/python-data-science-pandas-spark-dataframe-differences.html)

**作者：Christophe Bourguignat**。

*编辑者注：点击代码图片以放大。*

随着1.4版本的改进，[Spark DataFrames](https://databricks.com/blog/2015/02/17/introducing-dataframes-in-spark-for-large-scale-data-science.html)可能会成为新的Pandas，使*祖先* [RDDs看起来像字节码](https://ogirardot.wordpress.com/2015/05/29/rdds-are-the-new-bytecode-of-apache-spark/)。

我在Kaggle竞赛中大量使用Pandas（和Scikit-learn）。**迄今为止没有人用Spark赢得Kaggle挑战**，但我相信这会发生。因此，现在是准备未来并开始使用它的时候了。Spark DataFrames可以在[pyspark.sql](http://spark.apache.org/docs/latest/api/python/pyspark.sql.html)包中找到（名字奇怪且历史悠久：现在不仅仅是关于SQL！）。

我并不是一个Spark专家，但这是我初次尝试时注意到的一些事项。在我的GitHub上，你可以找到这篇文章的[IPython Notebook 伴侣](https://github.com/christophebourguignat/notebooks/blob/master/Spark-Pandas-Differences.ipynb)。

**读取**

使用Pandas，你可以轻松读取CSV文件，使用*read_csv()*。

默认情况下，Spark DataFrame支持从流行的*专业*格式中读取数据，如JSON文件、Parquet文件、Hive表——无论是从本地文件系统、分布式文件系统（HDFS）、云存储（S3）还是外部关系数据库系统。

![Spark DataFrames支持的格式](../Images/6c9e9ab0c93a7312d3ef5c50a3713940.png)

但**CSV在Spark中不被原生支持**。你需要使用一个单独的库：[spark-csv](https://github.com/databricks/spark-csv)。

```py
pandasDF = pd.read_csv('train.csv')
sparkDF = sqlContext.read.format('com.databricks.spark.csv').options(header='true').load('train.csv)  
```

**计数**

*sparkDF.count()* 和 *pandasDF.count()* 并不完全相同。

第一个返回**行数**，第二个返回每列的**非NA/null**观测值数量。

并不是说Spark还不支持*.shape*——在Pandas中经常使用。

[![Spark和Pandas dataframe count()的效果](../Images/e395afc8765755f63b48ded44e65a0ae.png)](https://cdn-images-1.medium.com/max/1200/1*WP-g74PW3FLVYBMBtIaB6g.png)

**查看**

在Pandas中，要查看DataFrame的表格内容，通常使用*pandasDF.head(5)*或*pandasDF.tail(5)*。在IPython Notebooks中，它会显示一个带有连续边框的漂亮数组。

在Spark中，你可以使用*sparkDF.head(5)*，但它有**难看的输出**。你应该更倾向于使用*sparkDF.show(5)*。注意你不能查看最后几行（*.tail()* 尚不存在，因为在分布式环境中难以实现）。

[![Spark和Pandas dataframe head()的效果](../Images/e1513d3549808d9d2c44b18718d49ab5.png)](https://cdn-images-1.medium.com/max/1200/1*12a6Jv_11QXHA2F96KkE2A.png)

**推断类型**

在 Pandas 中，你很少需要处理类型：**类型会被推断出来**。

使用从 CSV 文件加载的 Spark DataFrames 时，**默认假设类型为“字符串”**。

编辑：在 spark-csv 中，有一个 'inferSchema' 选项（默认禁用），但我没有设法让它工作。[它似乎在 1.1.0 版本中不起作用](https://github.com/databricks/spark-csv/issues/110)。

[![Spark and Pandas dataframe schema and dtypes comparison](../Images/6bc0a19ca0e12158c40509f70d5018ab.png)](https://cdn-images-1.medium.com/max/1200/1*PPjnVL7LfKI_afvJ8T-kJQ.png)

要在 Spark 中更改类型，你可以使用 *.cast()* 方法，或等效的 *.astype()* 方法，这[是为像我这样](https://issues.apache.org/jira/browse/SPARK-7394)来自 Pandas 世界的人创建的别名 ;)。注意，你必须**创建一个新列，并删除旧列**（[一些改进存在以允许类似“就地”更改](https://issues.apache.org/jira/browse/SPARK-6635)，但在 Python API 中尚不可用）。

[![Spark dataframe types](../Images/57c2c0df13aa50204958b910903fd0ff.png)](https://cdn-images-1.medium.com/max/1200/1*YZpC9yRc3_56iJF5aIdcSg.png)

**描述**

在 Pandas 和 Spark 中，*.describe()* 生成各种摘要统计信息。由于两个原因，它们给出的结果略有不同：

+   在 Pandas 中，NaN 值会被排除。在 Spark 中，NaN 值会导致均值和标准差的计算失败。

+   标准差的计算方式不同。Pandas 默认使用**无偏（或修正）标准差**，而 Spark 使用**未经修正的标准差**。区别在于分母上[使用 N-1 而不是 N](https://en.wikipedia.org/wiki/Standard_deviation)。

[![Dataframe describe() and show() in action](../Images/b0d1d6d67f33a8469787a9db4b253b91.png)](https://cdn-images-1.medium.com/max/1200/1*9zY5V2IiTQSvHEH2tkammw.png)

**整理**

在机器学习中，通常会创建新的列，这些列是基于已存在的列（特征工程）的计算结果。

在 Pandas 中，你可以使用‘[ ]’运算符。在 Spark 中则不行——DataFrames 是不可变的。你应该使用 *.withColumn()*。

[![Dataframe new columns](../Images/bee41a012ab249fa30a143e8a852af67.png)](https://cdn-images-1.medium.com/max/1200/1*sF5vRhyvFi0jGjDWqoyYRg.png)

**总结**

Spark 和 Pandas DataFrames 非常相似。不过，**Pandas API 仍然更方便和强大**——但差距在**迅速缩小**。

尽管 Spark 有其固有的设计限制（不可变性、分布式计算、惰性求值等），**Spark 仍然希望尽可能模仿 Pandas**（甚至方法名也不例外）。我猜这个目标很快就会实现。

并且使用[ Spark.ml](https://spark.apache.org/docs/latest/api/python/pyspark.ml.html)，模仿 scikit-learn，Spark 可能成为**工业化数据科学的完美一站式工具**。

感谢[Olivier Girardot](https://ogirardot.wordpress.com/)帮助改进这篇文章。

编辑 1：Olivier 刚发布了一篇新文章，提供了更多见解：[从 Pandas 到 Apache Spark DataFrames](https://ogirardot.wordpress.com/2015/07/31/from-pandas-to-apache-sparks-dataframe/)

编辑 2：这是另一篇相关主题的文章：[Pandarize Your Spark Dataframes](https://lab.getbase.com/pandarize-spark-dataframes/)

**简介：[Christophe Bourguignat](https://twitter.com/chris_bour)** 是数据爱好者、Kaggle 大师、博主以及 AXA 数据创新实验室的校友。

[原文](https://medium.com/@chris_bour/6-differences-between-pandas-and-spark-dataframes-1380cec394d2)。经许可转载。

**相关：**

+   [Spark 2015 年度回顾](/2016/01/spark-2015-year-in-review.html)

+   [初学者指南：使用 Apache Spark 进行大数据机器学习](/2015/11/petrov-apache-spark-machine-learning-large-data.html)

+   [掌握 Python 机器学习的 7 个步骤](/2015/11/seven-steps-machine-learning-python.html)

### 更多相关主题

+   [随机森林与决策树：关键区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [ChatGPT 与 Google Bard：技术差异对比](https://www.kdnuggets.com/2023/03/chatgpt-google-bard-comparison-technical-differences.html)

+   [Pandas 与 Polars：Python DataFrame 库的比较分析](https://www.kdnuggets.com/pandas-vs-polars-a-comparative-analysis-of-python-dataframe-libraries)

+   [如何使用 Pandas 将 JSON 数据转换为 DataFrame](https://www.kdnuggets.com/how-to-convert-json-data-into-a-dataframe-with-pandas)

+   [如何在几秒钟内处理包含百万行的 DataFrame](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

+   [解锁数据洞察：有效分析的关键 Pandas 函数](https://www.kdnuggets.com/unlocking-data-insights-key-pandas-functions-for-effective-analysis)
