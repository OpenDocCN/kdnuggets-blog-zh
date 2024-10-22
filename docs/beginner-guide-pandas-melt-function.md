# Pandas Melt 函数初学者指南

> 原文：[`www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html`](https://www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html)

![Pandas Melt 函数初学者指南](img/80237e1988db8ea9c1e495a23e2040a3.png)

图片来自 [catalyststuff](https://www.freepik.com/free-vector/cute-panda-reading-book-cartoon_11919389.htm#query=pandas&position=32&from_view=search&track=sph) 在 [Freepik](https://www.freepik.com/)

在处理现实生活中的数据集时，我们不能期望数据集完全符合我们的需求。有时，数据需要转换成另一种格式以便更容易处理。一种方法是将宽格式数据框转换成长格式。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

我们经常遇到宽格式数据；每行是数据，每列是数据特征。让我用数据集示例给你一个例子。我们将使用来自 Kaggle 的 [产品销售数据](https://www.kaggle.com/datasets/soumyadiptadas/products-sales-timeseries-data)（许可证：CC BY-NC-SA 4.0）由 Soumyadipta Das 提供。

```py
import pandas as pd
df = pd.read_csv('time series data.csv')
df.head()
```

![Pandas Melt 函数初学者指南](img/360060f8bcda075d189128bba9fa03c8.png)

在上述数据集中，每行表示销售发生的时间。另一方面，列是产品类型和其他支持类别（价格、温度）。

上面的数据集很好，但如果我们想在产品级别进行聚合可能会很困难。这就是为什么我们可以将数据转换成长格式以简化分析。为此，我们可以依靠 Pandas 的 melt 函数。

# Pandas Melt 函数

Pandas melt 函数用于将数据集从宽格式转换成长格式。什么是长格式数据集？它是一个每行包含变量及其值的数据集。技术上来说，我们是将数据集进行反透视，以获取具有较少列和较长行的数据集。让我们尝试 melt 函数以更好地理解。

```py
pd.melt(df)
```

![Pandas Melt 函数初学者指南](img/340da26a4d717140dda3b1bda971be6b.png)

我们从上面的输出中得到长格式数据集。数据集仅包含两列；‘variable’，它是宽格式数据集中的列名，以及‘value’，它是宽格式数据集中每行的数据值。

例如，‘t’列现在被视为数据观测，与原始数据集的行数相匹配，并带有相应的值。基本上，melt 函数从宽格式数据提供了键值对。

与宽格式相比，我们现在可以基于产品级别创建类别，而在宽格式数据中这并不可行，因为产品是列名。让我们尝试使用 melt 函数来做到这一点。

```py
pd.melt(
    df,
    id_vars=["t"],
    value_vars=["ProductP1", "ProductP2"],
    var_name="Product",
    value_name="Sales",
) 
```

![初学者 Pandas Melt 函数指南](img/83160460f23bc52fd97ed9b83a09812d.png)

在上述代码中，我们将‘t’列指定为数据标识符，将‘ProductP1’和‘ProductP2’指定为类别。为了便于阅读，我们将变量名称更改为‘Product’，值更改为‘Sales’。

现在，使用上述代码，对于每个时间框架（‘t’），我们获得了两个不同的产品类别及其值。这使得数据集的分析更加直观，因为组比较更为明确。

我们也可以使用 DataFrame 方法来融化数据集。当前代码的工作方式与上述示例完全相同。

```py
df.melt(
    id_vars=["t"],
    value_vars=["ProductP1", "ProductP2"],
    var_name="Product",
    value_name="Sales",
)
```

![初学者 Pandas Melt 函数指南](img/83160460f23bc52fd97ed9b83a09812d.png)

根据你的数据处理流程，你可以选择不同的数据融化方法。两种方法的结果完全相同。

我们也可以向融化后的数据集中添加更多标识符。为此，我们只需在‘id_vars’参数中指定所有预期的标识符。例如，我会将‘price’列作为额外的标识符。

```py
pd.melt(
    df,
    id_vars=["t", "price"],
    value_vars=["ProductP1", "ProductP2"],
    var_name="Product",
    value_name="Sales",
) 
```

![初学者 Pandas Melt 函数指南](img/3070b66250c7efbb7e3cb786be07687d.png)

结果将会是‘t’和‘price’列作为数据集标识符。上述方法在你的宽格式数据集中有多个键时尤其有用，而这些键你不想删除。

要进一步了解 Pandas melt 函数，你可以访问 [Pandas 文档](https://pandas.pydata.org/docs/reference/api/pandas.melt.html)。

# 结论

相比宽格式数据，有时长格式数据更为受欢迎。有时，我们需要分析的正是列，而唯一的方法是将数据反透视。通过使用 Pandas melt 函数，我们成功将宽格式数据转换为包含列名称和原始数据值的键值组合的长格式数据。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。

### 更多相关话题

+   [如何使用 pivot_table 函数进行高级数据汇总…](https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas)

+   [Python 函数参数：权威指南](https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html)

+   [你需要了解的关于梯度下降和成本函数的 5 个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [什么是函数？](https://www.kdnuggets.com/2022/11/function.html)

+   [数据科学中的 3 个 SQL 聚合函数面试问题](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)

+   [多标签自然语言处理：类别不平衡和损失函数的分析……](https://www.kdnuggets.com/2023/03/multilabel-nlp-analysis-class-imbalance-loss-function-approaches.html)
