# 掌握 Python 数据清理的艺术

> 原文：[https://www.kdnuggets.com/mastering-the-art-of-data-cleaning-in-python](https://www.kdnuggets.com/mastering-the-art-of-data-cleaning-in-python)

![掌握 Python 数据清理的艺术](../Images/8962cd78b382626bafaea7ad8d538500.png)

作者提供的图片

数据清理是任何数据分析过程中的关键部分。这一步骤中你会去除错误，处理缺失数据，并确保数据以可处理的格式存在。没有经过良好清理的数据集，任何后续分析都可能会产生偏差或错误。

* * *

## 我们的三大推荐课程

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 部门

* * *

本文介绍了几种在 Python 中进行数据清理的关键技术，使用了 pandas、numpy、seaborn 和 matplotlib 等强大库。

# 理解数据清理的重要性

在深入数据清理的机制之前，让我们了解其重要性。现实世界中的数据通常是混乱的。它可能包含重复条目、错误或不一致的数据类型、缺失值、无关特征和异常值。所有这些因素都可能导致分析数据时得出误导性结论。这使得数据清理成为数据科学生命周期中不可或缺的一部分。

我们将涵盖以下数据清理任务。

![掌握 Python 数据清理的艺术](../Images/9705a754315fcc1b9d070e034988a2f3.png)

作者提供的图片

# Python 数据清理的设置

在开始之前，让我们导入必要的库。我们将使用 pandas 进行数据处理，使用 seaborn 和 matplotlib 进行可视化。

我们还将导入 datetime Python 模块以便处理日期。

```py
import pandas as pd
import seaborn as sns
import datetime as dt
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
```

# 加载和检查您的数据

首先，我们需要加载数据。在这个例子中，我们将使用 pandas 加载一个 CSV 文件。我们还添加了分隔符参数。

```py
df = pd.read_csv('F:\\KDNuggets\\KDN Mastering the Art of Data Cleaning in Python\\property.csv', delimiter= ';')
```

接下来，检查数据以了解其结构、我们正在处理的变量类型以及是否有缺失值是很重要的。由于我们导入的数据量不大，让我们查看整个数据集。

```py
# Look at all the rows of the dataframe
display(df)
```

数据集的样子如下。

![掌握 Python 数据清理的艺术](../Images/14b819ad4461b06f0037afbd0fb31cc0.png)

你可以立即看到有一些缺失值。此外，日期格式不一致。

现在，让我们使用 info() 方法查看 DataFrame 概况。

```py
# Get a concise summary of the dataframe
print(df.info())
```

这是代码输出。

![掌握 Python 数据清理的艺术](../Images/e02bf810733535c1ba1e9252f7922309.png)

我们可以看到，只有列 square_feet 没有任何 NULL 值，因此我们需要处理这一点。此外，列 advertisement_date 和 sale_date 的数据类型是对象类型，尽管它们应该是日期。

列的位置完全为空。我们需要它吗？

我们将向你展示如何处理这些问题。我们首先学习如何删除不必要的列。

# 删除不必要的列

数据集中有两列在数据分析中不需要，因此我们将删除它们。

第一列是 buyer。我们不需要它，因为买方的名字不会影响分析。

我们使用了 `drop()` 方法，并指定了列名。我们将 `axis` 设置为 1，以指定要删除的是列。另外，将 `inplace` 参数设置为 True，以便我们修改现有的 DataFrame，而不是创建一个没有被删除列的新 DataFrame。

```py
df.drop('buyer', axis = 1, inplace = True)
```

我们要删除的第二列是 location。虽然这些信息可能有用，但这是一列完全为空的列，所以我们直接删除它。

我们采用与第一列相同的方法。

```py
df.drop('location', axis = 1, inplace = True)
```

当然，你可以同时删除这两列。

```py
df = df.drop(['buyer', 'location'], axis=1)
```

这两种方法都返回了以下数据框。

![掌握 Python 数据清理的艺术](../Images/d4b19f806cfe3564dd619a9df95ffe74.png)

# 处理重复数据

数据集中的重复数据可能由于各种原因发生，并且可能会影响你的分析。

让我们检测数据集中的重复项。以下是如何操作。

以下代码使用了 **duplicated()** 方法来检查整个数据集中的重复项。默认情况下，它将值的第一次出现视为唯一，之后的出现视为重复。你可以使用 **keep** 参数来修改这一行为。例如，df.duplicated(keep=False) 会将所有重复项标记为 True，包括第一次出现。

```py
# Detecting duplicates
duplicates = df[df.duplicated()]
duplicates
```

这是输出结果。

![掌握 Python 数据清理的艺术](../Images/d4fe70038e3767a4460653a2ef241088.png)

索引为 3 的行被标记为重复，因为值相同的第 2 行是它的第一次出现。

现在我们需要去除重复项，这里使用了以下代码。

```py
# Detecting duplicates
duplicates = df[df.duplicated()]
duplicates
```

**drop_duplicates()** 函数在识别重复项时会考虑所有列。如果你只想考虑某些列，可以将它们作为列表传递给此函数，例如：df.drop_duplicates(subset=['column1', 'column2'])。

![掌握 Python 数据清理的艺术](../Images/0c802373e36ec7124b65ee9e9cb0cc8b.png)

如你所见，重复的行已经被删除。然而，索引保持不变，索引 3 丢失了。我们将通过重置索引来整理这一点。

```py
df = df.reset_index(drop=True)
```

这个任务是通过使用**reset_index()**函数来完成的。`drop=True`参数用于丢弃原始索引。如果不包括此参数，旧索引将作为新列添加到你的DataFrame中。通过设置`drop=True`，你告诉pandas忘记旧索引并将其重置为默认的整数索引。

练习一下，尝试从这个[Microsoft数据集中删除重复项](https://platform.stratascratch.com/coding/9849-find-the-duplicate-records-in-the-dataset?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)。

# 数据类型转换

有时，数据类型可能设置不正确。例如，日期列可能被解释为字符串。你需要将其转换为适当的类型。

在我们的数据集中，我们将对`advertisement_date`和`sale_date`列进行此操作，因为它们显示为对象数据类型。此外，日期格式在各行之间不同。我们需要使其一致，同时转换为日期。

最简单的方法是使用**to_datetime()**方法。你可以像下面所示，一列一列地进行转换。

在这样做时，我们将`dayfirst`参数设置为True，因为某些日期以天为首。

```py
# Converting advertisement_date column to datetime
df['advertisement_date'] = pd.to_datetime(df['advertisement_date'], dayfirst = True)

# Converting sale_date column to datetime
df['sale_date'] = pd.to_datetime(df['sale_date'], dayfirst = True)
```

你也可以使用**apply()**方法结合**to_datetime()**同时转换两列。

```py
# Converting advertisement_date and sale_date columns to datetime
df[['advertisement_date', 'sale_date']] = df[['advertisement_date', 'sale_date']].apply(pd.to_datetime, dayfirst =  True)
```

两种方法都会给你相同的结果。

![掌握Python数据清洗的艺术](../Images/19dc91cbaf4dbb488a3593d95ad7a444.png)

现在日期格式一致了。我们看到并非所有数据都已转换。`advertisement_date`中有一个NaT值，`sale_date`中有两个。这意味着日期缺失了。

让我们使用**info()**方法检查列是否已转换为日期。

```py
# Get a concise summary of the dataframe
print(df.info())
```

![掌握Python数据清洗的艺术](../Images/e6ec0eb9378e4f56d55550dcb47a8a4d.png)

正如你所见，两列都不是datetime64[ns]格式。

现在，尝试在这个[Airbnb数据集中将数据从文本转换为数字](https://platform.stratascratch.com/coding/9634-host-response-rates-with-cleaning-fees?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)。

# 处理缺失数据

现实世界的数据集通常会有缺失值。处理缺失数据是至关重要的，因为某些算法不能处理这些值。

我们的示例中也有一些缺失值，所以我们来看一下处理缺失数据的两种最常见的方法。

## 删除缺失值的行

如果缺失数据的行数与总观察数相比不重要，你可以考虑删除这些行。

在我们的示例中，最后一行除了平方英尺和广告日期外没有值。我们不能使用这些数据，所以我们来删除这一行。

这是我们指明行索引的代码。

```py
df = df.drop(8)
```

DataFrame现在看起来是这样的。

![掌握Python数据清洗的艺术](../Images/9f2a0e5598824db43db0c0347be9f221.png)

最后一行已经被删除，我们的DataFrame现在看起来更好了。不过，仍然有一些缺失的数据，我们将使用另一种方法处理这些数据。

## 插补缺失值

如果你有显著缺失的数据，除了删除，另一种更好的策略是插补。这个过程涉及根据其他数据填充缺失值。对于数值数据，常见的插补方法包括使用集中趋势的度量（均值、中位数、众数）。

在我们已经更改的DataFrame中，advertisement_date和sale_date列中有NaT（非时间）值。我们将使用**mean()**方法插补这些缺失值。

代码使用**fillna()**方法查找并用均值填充空值。

```py
# Imputing values for numerical columns
df['advertisement_date'] = df['advertisement_date'].fillna(df['advertisement_date'].mean())
df['sale_date'] = df['sale_date'].fillna(df['sale_date'].mean())
```

你也可以用一行代码完成相同的操作。我们使用**apply()**来应用用**lambda**定义的函数。与上述相同，这个函数使用**fillna()**和**mean()**方法填充缺失值。

```py
# Imputing values for multiple numerical columns
df[['advertisement_date', 'sale_date']] = df[['advertisement_date', 'sale_date']].apply(lambda x: x.fillna(x.mean()))
```

两种情况下的输出如下所示。

![掌握 Python 中的数据清理艺术](../Images/96cc85a9edbd5b5dc67ab39ab9774fab.png)

我们的sale_date列现在有一些我们不需要的时间。让我们把它们移除。

我们将使用**strftime()**方法，它将日期转换为其字符串表示形式和特定格式。

```py
df['sale_date'] = df['sale_date'].dt.strftime('%Y-%m-%d')
```

![掌握 Python 中的数据清理艺术](../Images/e2b413dcdf2794b2dfce981d3964e274.png)

现在日期看起来都很整齐了。

如果你需要在多个列上使用**strftime()**，你可以再次使用**lambda**，方法如下。

```py
df[['date1_formatted', 'date2_formatted']] = df[['date1', 'date2']].apply(lambda x: x.dt.strftime('%Y-%m-%d'))
```

现在，让我们看看如何插补缺失的分类值。

分类数据是一种用于将信息分组的类型，每个组是一个类别。分类数据可以取数值（例如“1”表示“男”和“2”表示“女”），但这些数字没有数学意义。例如，你不能将它们相加。

分类数据通常分为两类：

1.  **名义数据：** 这是指类别仅仅被标记，不能按特定顺序排列。例如性别（男、女）、血型（A、B、AB、O）或颜色（红、绿、蓝）。

1.  **顺序数据：** 这是指类别可以被排序或排名。虽然类别之间的间隔不均等，但类别的顺序具有意义。例如评分尺度（电影的1到5评分）、教育水平（高中、本科、研究生）或癌症阶段（I期、II期、III期）。

对于插补缺失的分类数据，通常使用众数。在我们的例子中，property_category列是分类（名义）数据，并且有两个行数据缺失。

我们将用众数替换缺失值。

```py
# For categorical columns
df['property_category'] = df['property_category'].fillna(df['property_category'].mode()[0])
```

这段代码使用**fillna()**函数来替换property_category列中的所有NaN值。它用众数替代这些值。

此外，[0] 部分用于从此系列中提取第一个值。如果存在多个众数，它将选择第一个。如果只有一个众数，它也会正常工作。

这是输出结果。

![掌握 Python 中的数据清洗艺术](../Images/d5a4dc6cfbcae664ee2ba044bfc1cf82.png)

现在数据看起来相当不错。剩下的唯一任务是检查是否有异常值。

你可以在这个 [Meta 面试问题](https://platform.stratascratch.com/coding/9790-find-the-number-of-processed-and-not-processed-complaints-of-each-type?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning) 上练习处理空值，其中你需要将 NULL 替换为零。

# 处理异常值

异常值是数据集中明显不同于其他观察值的数据点。它们可能远离数据集中其他值，位于整体模式之外。由于其值显著高于或低于其他数据点，它们被认为是不寻常的。

异常值可能由于各种原因产生，例如：

+   测量或输入错误

+   数据损坏

+   真实的统计异常

异常值可能会显著影响数据分析和统计建模的结果。它们可能导致分布偏斜、偏差，或使基本的统计假设无效，扭曲估计的模型拟合，降低预测模型的预测准确性，并导致不正确的结论。

一些常用的检测异常值的方法包括 Z-score、IQR（四分位距）、箱线图、散点图和数据可视化技术。在一些高级情况下，也会使用机器学习方法。

数据可视化可以帮助识别异常值。Seaborn 的箱线图在这方面很有用。

```py
plt.figure(figsize=(10, 6))
sns.boxplot(data=df[['advertised_price', 'sale_price']])
```

我们使用 plt.figure() 设置图形的宽度和高度（以英寸为单位）。

然后我们为广告价格和销售价格列创建箱线图，如下所示。

![掌握 Python 中的数据清洗艺术](../Images/eab301802d2a19402b4de84b5a41005f.png)

可以通过将此添加到上述代码来改进图表，以便更容易使用。

```py
plt.xlabel('Prices')
plt.ylabel('USD')
plt.ticklabel_format(style='plain', axis='y')
formatter = ticker.FuncFormatter(lambda x, p: format(x, ',.2f'))
plt.gca().yaxis.set_major_formatter(formatter)
```

我们使用上述代码设置两个轴的标签。我们还注意到 y 轴上的值以科学记数法显示，而我们不能用这种方式显示价格值。因此，我们使用 plt.ticklabel_format() 函数将其更改为普通样式。

然后我们创建格式化器，将 y 轴上的值显示为用逗号分隔的千位数和小数点。最后一行代码将其应用到轴上。

现在输出结果如下。

![掌握 Python 中的数据清洗艺术](../Images/484a574270a0fdf86aa68c9650010659.png)

那么，我们如何识别和去除异常值呢？

其中一种方法是使用 IQR 方法。

IQR，即四分位距，是一种统计方法，用于通过将数据集划分为四分位数来测量变异性。四分位数将按顺序排列的数据集分为四个相等的部分，第一个四分位数（25百分位数）和第三个四分位数（75百分位数）之间的值构成了四分位距。

四分位距用于识别数据中的异常值。其工作原理如下：

1.  首先，计算第一个四分位数（Q1），第三个四分位数（Q3），然后确定IQR。IQR的计算公式是Q3 - Q1。

1.  任何低于Q1 - 1.5IQR或高于Q3 + 1.5IQR的值都被视为异常值。

在我们的箱线图上，箱体实际上代表了IQR。箱体内的线是中位数（或第二四分位数）。箱线图的“胡须”表示距离Q1和Q3的1.5*IQR范围内的范围。

任何超出这些“胡须”的数据点都可以被视为异常值。在我们的案例中，就是$12,000,000的值。如果你查看箱线图，你会清楚地看到这一点，这显示了数据可视化在检测异常值中的重要性。

现在，让我们通过使用Python代码中的IQR方法来去除异常值。首先，我们将去除advertised price的异常值。

```py
Q1 = df['advertised_price'].quantile(0.25)
Q3 = df['advertised_price'].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df['advertised_price'] < (Q1 - 1.5 * IQR)) |(df['advertised_price'] > (Q3 + 1.5 * IQR)))]
```

我们首先使用**quantile()**函数计算第一个四分位数（即第25百分位数）。对第三个四分位数或第75百分位数也进行相同操作。

它们显示了25%和75%的数据分别落在的值。

然后我们计算四分位数之间的差异。到目前为止，一切都是将IQR步骤转化为Python代码。

最后一步，我们去除异常值。换句话说，去除所有小于Q1 - 1.5 * IQR或大于Q3 + 1.5 * IQR的数据。

'~'运算符否定条件，因此我们只保留不是异常值的数据。

然后我们可以对sale price做同样的处理。

```py
Q1 = df['sale_price'].quantile(0.25)
Q3 = df['sale_price'].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df['sale_price'] < (Q1 - 1.5 * IQR)) |(df['sale_price'] > (Q3 + 1.5 * IQR)))]
```

当然，你可以使用**for loop**以更简洁的方式实现。

```py
for column in ['advertised_price', 'sale_price']:
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    df = df[~((df[column] < (Q1 - 1.5 * IQR)) |(df[column] > (Q3 + 1.5 * IQR)))]
```

循环遍历两列。对于每一列，它计算IQR，然后从数据框中删除相应的行。

请注意，这一操作是顺序进行的，首先处理advertised_price，然后处理sale_price。因此，数据框在每一列中都会就地修改，行可能会因为在任一列中是异常值而被删除。因此，这一操作可能导致行数少于分别去除advertised_price和sale_price中的异常值后再合并结果的行数。

在我们的例子中，两种情况的输出将是相同的。要查看箱线图的变化，我们需要使用之前相同的代码再次绘制。

```py
plt.figure(figsize=(10, 6))
sns.boxplot(data=df[['advertised_price', 'sale_price']])
plt.xlabel('Prices')
plt.ylabel('USD')
plt.ticklabel_format(style='plain', axis='y')
formatter = ticker.FuncFormatter(lambda x, p: format(x, ',.2f'))
plt.gca().yaxis.set_major_formatter(formatter)
```

这是输出结果。

![掌握 Python 数据清理的艺术](../Images/02a67feb20614804d7f213806b8ba6c4.png)

你可以通过解决[General Assembly 面试题](https://platform.stratascratch.com/coding/9611-find-the-80th-percentile-of-hours-studied?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)来练习在Python中计算百分位数。

# 结论

数据清理是数据分析过程中的一个关键步骤。尽管这可能很耗时，但确保发现的准确性至关重要。

幸运的是，Python 丰富的库生态系统使这一过程更加可控。我们学习了如何删除不必要的行和列，重新格式化数据，并处理缺失值和异常值。这些是对大多数数据必须执行的常规步骤。然而，有时你还需要[将两列合并为一列](https://platform.stratascratch.com/coding/9834-combine-the-first-and-last-names-of-workers?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)、[验证现有数据](https://platform.stratascratch.com/coding/9737-verify-that-the-first-4-digits-are-equal-to-1415-for-all-phone-numbers?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)、[给数据分配标签](https://platform.stratascratch.com/coding/9628-reviews-bins-on-reviews-number?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)或[去除空白字符](https://platform.stratascratch.com/coding/9818-file-contents-shuffle?code_type=2&utm_source=blog&utm_medium=click&utm_campaign=kdn+data+cleaning)。

这一切都是数据清理，因为它使你能够将混乱的现实世界数据转化为一个结构良好的数据集，从而可以自信地进行分析。只需比较我们开始时的数据集和最终的数据集。

如果你对这个结果不满意，干净的数据没有让你感到异常兴奋，那你在数据科学领域到底在做什么！？

[](https://twitter.com/StrataScratch)****[内特·罗西迪](https://twitter.com/StrataScratch)**** 是一位数据科学家，专注于产品战略。他还是一名兼职教授，教授分析学，并且是 StrataScratch 的创始人，该平台帮助数据科学家通过顶级公司提供的真实面试问题准备面试。内特撰写有关职业市场的最新趋势，提供面试建议，分享数据科学项目，并涵盖所有 SQL 相关内容。

### 更多相关话题

+   [掌握数据讲述的艺术：数据科学家的指南](https://www.kdnuggets.com/2023/06/mastering-art-data-storytelling-guide-data-scientists.html)

+   [掌握 SQL、Python、数据清理、数据整理与探索性数据分析的指南集合](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [掌握 Python 和 Pandas 的数据清理的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

+   [掌握数据清理和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [数据讲述 - 通过数据讲故事的艺术](https://www.kdnuggets.com/2023/07/manning-data-storytelling-the-art-telling-stories-data.html)

+   [掌握数据故事讲述艺术的7个步骤](https://www.kdnuggets.com/7-steps-to-master-the-art-of-data-storytelling)
