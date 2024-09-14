# 使用 Pandas 管道函数进行更清晰的数据分析

> 原文：[https://www.kdnuggets.com/2021/01/cleaner-data-analysis-pandas-pipes.html](https://www.kdnuggets.com/2021/01/cleaner-data-analysis-pandas-pipes.html)

[comments](#comments)![Figure](../Images/dae67c23202b0f5e3c5ee224a97d389d.png)

由 [Candid](https://unsplash.com/@candid?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/s/photos/clean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

Pandas 是一个广泛使用的数据分析和处理库，提供了众多功能和方法，以实现强大且高效的数据分析过程。

在典型的数据分析或清理过程中，我们可能会执行许多操作。随着操作数量的增加，代码开始变得杂乱且难以维护。

克服此问题的一种方法是使用 Pandas 的管道函数。管道函数的作用是允许以链式方式组合多个操作。

在本文中，我们将通过示例了解如何使用管道函数来生成更清晰、更易维护的代码。

我们首先将在单独的步骤中对样本数据框进行数据清理和处理。之后，我们将使用管道函数将这些步骤合并。

首先导入库并创建数据框。

```py
import numpy as np
import pandas as pd

marketing = pd.read_csv("/content/DirectMarketing.csv")
marketing.head()
```

![Figure](../Images/61b038414c75427fc36344ea1c72a710.png)

（图片由作者提供）

数据集包含有关营销活动的信息。可在 Kaggle 上 [这里](https://www.kaggle.com/yoghurtpatil/direct-marketing) 获取。

我想做的第一个操作是删除缺失值很多的列。

```py
thresh = len(marketing) * 0.6
marketing.dropna(axis=1, thresh=thresh, inplace=True)
```

上面的代码删除了缺失值占 40% 或更多的列。我们传递给 dropna 函数的 thresh 参数值表示所需的最小非缺失值数量。

我还想删除一些异常值。在工资列中，我希望保留在第 5 百分位数和第 95 百分位数之间的值。

```py
low = np.quantile(marketing.Salary, 0.05)
high = np.quantile(marketing.Salary, 0.95)

marketing = marketing[marketing.Salary.between(low, high)]
```

我们通过使用 numpy 的 quantile 函数找到所需范围的下限和上限。这些值随后用于过滤数据框。

需要注意的是，检测异常值的方法有很多种。实际上，我们使用的方法有些表面化。还有更现实的替代方案。然而，这里重点是管道函数。因此，你可以实现最适合你任务的操作。

数据框包含许多分类变量。如果类别的数量相对于总数值比较少，最好使用类别数据类型而不是对象。这可以根据数据大小节省大量内存。

以下代码将遍历对象数据类型的列。如果类别的数量少于总值的 5%，则该列的数据类型将更改为类别。

```py
cols = marketing.select_dtypes(include='object').columns
for col in cols:
    ratio = len(marketing[col].value_counts()) / len(marketing)
    if ratio < 0.05:
    marketing[col] = marketing[col].astype('category')
```

我们完成了三步数据清洗和处理。根据任务的不同，步骤的数量可能更多。

让我们创建一个管道来完成所有这些任务。

管道函数接受函数作为输入。这些函数需要以数据框作为输入，并返回一个数据框。因此，我们需要为每个任务定义函数。

```py
def drop_missing(df):
    thresh = len(df) * 0.6
    df.dropna(axis=1, thresh=thresh, inplace=True)
    return df

def remove_outliers(df, column_name):
    low = np.quantile(df[column_name], 0.05)
    high = np.quantile(df[column_name], 0.95)
    return df[df[column_name].between(low, high, inclusive=True)]

def to_category(df):
    cols = df.select_dtypes(include='object').columns
    for col in cols:
        ratio = len(df[col].value_counts()) / len(df)
        if ratio < 0.05:
            df[col] = df[col].astype('category')
    return df
```

你可能会争辩说，如果我们需要定义函数，那有什么意义？这似乎并没有简化工作流程。对于一个特定的任务你是对的，但我们需要从更广泛的角度思考。考虑你多次进行相同的操作。在这种情况下，创建一个管道可以使过程更简单，并提供更清晰的代码。

我们提到过管道函数接受一个函数作为输入。如果我们传递给管道函数的函数有任何参数，我们可以将这些参数与函数一起传递给管道函数。这使得管道函数更加高效。

例如，`remove_outliers` 函数接受一个列名作为参数。该函数会去除该列中的异常值。

我们现在可以创建我们的管道。

```py
marketing_cleaned = (marketing.
                       pipe(drop_missing).
                       pipe(remove_outliers, 'Salary').
                       pipe(to_category))
```

它看起来整洁而干净。我们可以根据需要添加任意多的步骤。唯一的标准是管道中的函数应该接受一个数据框作为参数并返回一个数据框。就像使用 `remove_outliers` 函数一样，我们可以将函数的参数作为参数传递给管道函数。这种灵活性使管道更加有用。

一个重要的事情是，管道函数会修改原始数据框。如果可能，我们应该避免更改原始数据集。

为了克服这个问题，我们可以在管道中使用原始数据框的副本。此外，我们可以在管道的开始阶段添加一个步骤，以创建数据框的副本。

```py
def copy_df(df):
   return df.copy()

marketing_cleaned = (marketing.
                       pipe(copy_df).
                       pipe(drop_missing).
                       pipe(remove_outliers, 'Salary').
                       pipe(to_category))
```

我们的管道现在完成了。让我们比较原始数据框和清洗后的数据框，以确认它正在正常工作。

```py
marketing.shape
(1000,10)

marketing.dtypes
Age            object
Gender         object
OwnHome        object
Married        object
Location       object
Salary          int64
Children        int64
History        object
Catalogs        int64
AmountSpent     int64

marketing_cleaned.dtypes
(900,10)

marketing_cleaned.dtypes
Age            category
Gender         category
OwnHome        category
Married        category
Location       category
Salary            int64
Children          int64
History        category
Catalogs          int64
AmountSpent       int64
```

管道如预期般工作。

### 结论

管道提供了更清晰、更易于维护的数据分析语法。另一个优点是它们自动化了数据清洗和处理的步骤。

如果你反复进行相同的操作，你应该考虑创建一个管道。

感谢阅读。如果你有任何反馈，请告诉我。

**个人简介： [Soner Yıldırım](https://www.linkedin.com/in/soneryildirim/)** 是一位数据科学爱好者。[查看他的作品集](https://soneryldrm.github.io/Portfolio/)。

[原文](https://towardsdatascience.com/cleaner-data-analysis-with-pandas-using-pipes-4d73770fbf3c)。经授权转载。

**相关内容：**

+   [数据清理：任何数据科学项目成功的秘密成分](/2020/07/data-cleaning-secret-ingredient-success-data-science-project.html)

+   [SQL 数据清理与整理](/2021/01/data-cleaning-wrangling-sql.html)

+   [在 Python 中合并 Pandas 数据框](/2020/12/merging-pandas-dataframes-python.html)

### 更多相关内容

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [停止学习数据科学来寻找目标，并找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每位数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [90 亿美元的 AI 失败，解析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
