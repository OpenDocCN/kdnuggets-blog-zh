# 通过Pandas管道简化数据处理

> 原文：[https://www.kdnuggets.com/2022/08/simplify-data-processing-pandas-pipeline.html](https://www.kdnuggets.com/2022/08/simplify-data-processing-pandas-pipeline.html)

![通过Pandas管道简化数据处理](../Images/dac07a1e2d0545f432c9878310efc11d.png)

图片由作者提供

# 介绍

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作

* * *

在R语言中，我们使用%>%创建管道，并对数据集执行多个操作。类似地，为了创建机器学习管道，我们使用scikit-learn的 [Pipeline](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) 处理数据、构建和评估模型。那么，在Python中创建数据管道时我们用什么？我们使用pandas的 [pipe](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.pipe.html) 来应用可链式函数。

# Pandas管道教程

在本教程中，我们将学习创建pandas管道，并添加多个链式函数以执行数据处理和可视化。

我们将使用 [Deepnote](https://deepnote.com/) 环境运行代码，并展示外观清晰的pandas数据框。

## 入门

我们将使用`read_csv()`从Kaggle加载并展示 [Mall Customer Segmentation](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) 数据集。

```py
import pandas as pd

data = pd.read_csv("Mall_Customers.csv")
data
```

它包含客户ID、年龄、性别、收入和消费评分。

![通过Pandas管道简化数据处理](../Images/da71e40975b21df97bc1f9dad0076c89.png)

## 创建数据处理函数

现在编写简单的Python函数，这些函数接受一个或多个参数。每个函数必须以数据框作为第一个参数，以创建链式管道。

+   **filter_male_income**: 该函数接受两个列，并筛选出年收入大于15的男性客户数据。

+   **mean_group**: 它按单列对数据框进行分组并计算均值，并删除CustomerID列。

+   **uppercase_column_name**: 它将列名转换为大写。

+   **bar_plot**: 该函数使用单列并绘制条形图。它在后台使用`matplotlib.pyplot`。

```py
*# filtering by Gender and Annual Income*
def filter_male_income(dataframe, col1,col2):
    return data[(data[col1] == "Male") & (data[col2] >= 15) ]

*# groupby mean and dropping ID columns*
def mean_group(dataframe, col):
    return dataframe.groupby(col).mean().drop("CustomerID",axis=1)

*# changing column names to uppercase*
def uppercase_column_name(dataframe):
    dataframe.columns = dataframe.columns.str.upper()
    return dataframe 

*# plot bar chart using pandas*
def bar_plot(dataframe, col):
    return dataframe.plot(kind="bar", y=col, figsize=(15,10))
```

## 带有一个函数的管道

在这一部分，我们将创建一个包含单一函数的简单管道。我们将在pandas dataframe（data）后添加`.pipe()`，并添加一个有两个参数的函数。在我们的案例中，这两个列是“Gender”和“Annual Income (k$)”。

```py
data.pipe(filter_male_income, col1="Gender", col2="Annual Income (k$)")
```

![使用Pandas管道简化数据处理](../Images/2a9097f1a48e73978843c5b1402ab086.png)

## 带有多个函数的管道

让我们尝试一个稍复杂的示例，向管道中添加2个函数。要添加其他函数，只需在第一个管道函数后添加`.pipe()`。我们可以在数据管道中添加任意多个带有多个参数的管道。结果是可重复的，代码可读且简洁。

在我们的案例中，我们已筛选了数据集，按“年龄”分组，并将列名转换为大写字母。

这很简单，就像启动一个Python函数一样。

```py
data.pipe(filter_male_income, col1="Gender", col2="Annual Income (k$)").pipe(
                mean_group, "Age"
                ).pipe(uppercase_column_name)
```

![使用Pandas管道简化数据处理](../Images/092e4c5a392d84645a3e41e1d38d9250.png)

## 一个完整的管道

一个完整的管道处理数据并显示一些分析结果。在我们的案例中，它是一个客户的**年收入**与**年龄**的简单条形图。我们已筛选数据框，按年龄分组，将列转换为大写字母，并绘制了条形图。

```py
data.pipe(filter_male_income, col1="Gender", col2="Annual Income (k$)").pipe(
                mean_group, "Age"
                ).pipe(uppercase_column_name).pipe(bar_plot,"ANNUAL INCOME (K$)")
```

![使用Pandas管道简化数据处理](../Images/d188b3a9a0daba7a0ce3edb355683486.png)

# 结论

管道可以应用于pandas数据框和系列。在数据处理和实验阶段，它非常有效。在这里，你可以轻松切换函数以获得最佳解决方案。管道允许我们以结构化和组织化的方式将多个函数合并为单一操作。它是干净、可读且可重复的。你可以用它来简化数据处理阶段。

在本教程中，我们学习了pandas管道函数及其使用案例。我们还创建了单个和多个函数的多个管道。管道函数还可以用于高级过程，如数据分析、数据可视化和机器学习任务。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些在心理健康方面挣扎的学生构建一个AI产品。

### 相关主题

+   [构建供应链管道所需的6种数据科学技术](https://www.kdnuggets.com/2022/01/6-data-science-technologies-need-build-supply-chain-pipeline.html)

+   [ETL与ELT：哪一种适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [使用Kafka和Risingwave构建F1流数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)

+   [使用Prefect构建数据管道](https://www.kdnuggets.com/building-data-pipeline-with-prefect)

+   [使用TPOT进行机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [为多变量时间序列构建一个可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)
