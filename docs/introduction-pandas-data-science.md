# 数据科学的 Pandas 介绍

> 原文：[https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

![数据科学的 Pandas 介绍](../Images/7c932944c4b641a632fd107fd28bf1f3.png)

[由 benzoix 提供的图像](https://www.freepik.com/free-vector/panda-mascot-logo-esport-gaming_11760508.htm#query=pandas&position=23&from_view=search&track=sph) 来自 Freepik

Pandas 实际上是什么？为什么它如此著名？把 Pandas 想象成一个 Excel 表，但它比 Excel 更高级，具有更多的功能和灵活性。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

# 为什么选择 Pandas

选择 pandas 的原因有很多，其中一些包括

+   开源

+   容易学习

+   优秀的社区

+   基于 Numpy 之上

+   容易分析和预处理数据

+   内置数据可视化

+   许多内置函数帮助进行探索性数据分析

+   内置支持 CSV、SQL、HTML、JSON、pickle、excel、剪贴板等

+   还有很多其他功能

# 安装 Pandas

如果你使用 Anaconda，你将自动拥有 pandas，但如果由于某种原因没有它，只需运行以下命令

```py
conda install pandas
```

如果你没有使用 Anaconda，通过 pip 安装

```py
pip install pandas
```

## 导入

要导入 pandas，请使用

```py
import pandas as pd
import numpy as np
```

最好与 pandas 一起导入 numpy，以便访问更多 numpy 功能，这有助于我们进行探索性数据分析（EDA）。

# Pandas 数据结构

Pandas 有两个主要的数据结构。

+   Series

+   数据框

## Series

将 Series 视为 Excel 表中的一列。你也可以将其视为一个一维的 Numpy 数组。唯一的不同之处在于，我们可以为其设置索引名称。

创建 pandas Series 的基本语法如下：

```py
newSeries = pd.Series(data , index)
```

数据可以是从 Python 字典到列表或元组的任何类型。它也可以是一个 numpy 数组。

让我们从 Python 列表中创建一个 Series：

```py
mylist = ['Ahmad','Machine Learning', 20, 'Pakistan']
labels = ['Name', 'Career', 'Age', 'Country']
newSeries = pd.Series(mylist,labels)
print(newSeries)
```

![](../Images/314cccf6adc18392f9bf86ed3f9b5237.png)

newSeries 的输出。

在 pandas Series 中添加索引不是必须的。在这种情况下，它会自动从 0 开始索引。

```py
mylist = ['Ahmad','Machine Learning', 20, 'Pakistan']
newSeries = pd.Series(mylist)
print(newSeries)
```

![](../Images/db6ab16b2ad6fb7298c5e767599f1d2b.png)

在这里，我们可以看到索引从 0 开始，一直到 Series 结束。现在让我们看看如何使用 [Python 字典](https://www.kdnuggets.com/2019/12/python-dictionary-methods.html) 创建 Series，

```py
myDict = {'Name': 'Ahmad',
         'Career': 'Machine Learning',
         'Age': 20,
          'Country': 'Pakistan'}
mySeries = pd.Series(myDict)
print(mySeries)
```

![](../Images/8cc86f7cf20db2ad2512282c3c880f1c.png)

在这里我们可以看到，我们不需要显式地传递索引值，因为它们会从字典的键中自动分配。

## 访问Series中的数据

访问Pandas Series中的数据的常见模式是：

```py
seriesName['IndexName']
```

让我们以之前创建的mySeries为例。要获取Name、Age和Career的值，我们只需：

```py
print(mySeries['Name'])
print(mySeries['Age'])
print(mySeries['Career'])
```

![](../Images/731668986bd3ec59bc1b4ffe05466651.png)

## Pandas Series 的基本操作

让我们创建两个新系列来对它们进行操作。

```py
newSeries = pd.Series([10,20,30,40],index=['LONDON','NEWYORK','Washington','Manchester'])
newSeries1 = pd.Series([10,20,35,46],index=['LONDON','NEWYORK','Istanbul','Karachi'])
print(newSeries,newSeries1,sep='\n\n')
```

![](../Images/66bad4155f568f5d8de191d9271209a6.png)

基本的算术操作包括+-*/操作。这些操作是针对索引的，因此我们来执行它们。

```py
newSeries + newSeries1
```

![](../Images/c360748cc9c2a747e56e176141fe6bdf.png)

在这里我们可以看到，由于伦敦和NEWYORK索引在两个Series中都存在，因此它将两个值相加，其他的输出为NaN（不是一个数字）。

```py
newSeries * newSeries1
```

![](../Images/41100455557a3b9362ef33bdaf33a8b8.png)

```py
newSeries / newSeries1
```

![](../Images/c9f8567d040d3e456cd94f1999196aea.png)

## 元素级操作/广播

如果你对Numpy很熟悉，你一定听说过广播的概念。如果你不熟悉广播的概念，请参考 [this link](https://www.geeksforgeeks.org/python-broadcasting-with-numpy-arrays/)。

现在，使用我们的newSeries Series，我们将看到使用广播概念进行的操作。

```py
newSeries + 5
```

![](../Images/bb9f77aafeade733e05820e62036069e.png)

在这里，它给Series newSeries中的每个元素加上了5。这也被称为逐元素操作。类似地，其他操作如*、/、-、**及其他运算符也是如此。我们只会看到**运算符，你也应该尝试其他运算符。

```py
newSeries ** 1/2
```

![](../Images/e9f736d264dc48221607416762b416d5.png)

在这里，我们对每个数字进行逐元素的平方根计算。记住，平方根是任何数字的1/2次方。

## Pandas 数据框

数据框确实是Pandas中最常用和最重要的数据结构。可以将数据框看作是一个Excel表格。

创建数据框的主要方法有：

+   读取 CSV/Excel 文件

+   Python 字典

+   ndarray

让我们举个例子，看看如何使用字典创建数据框。

我们可以通过传递一个字典来创建数据框，其中字典的每个值都是一个列表。

```py
df1 = {"Name":["Ahmad","Ali",'Ismail',"John"],"Age":  [20,21,19,17],"Height":[5.1,5.6,6.1,5.7]} 
```

要将这个字典转换为数据框，我们只需对这个字典调用dataframe函数即可。

```py
df1 = pd.DataFrame(df1)
df1
```

![](../Images/d872e788d8650798f50112d4cd77d76f.png)df1

## 从列中获取值

要从列中获取值，我们可以使用这种语法：

```py
#df1['columnname']
#df1.columnname
```

这两种语法都是正确的，但我们需要小心选择。如果我们的列名中有空格，那么肯定不能使用第二种方法。我们必须使用第一种方法。只有当列名中没有空格时，我们才可以使用第二种方法。

```py
df1.Name
```

![](../Images/68ef167ed2e8a38a5e2ad5ebc29e806d.png)df1.Name

在这里我们可以看到列的值、它们的索引号、列名和列的数据类型。

```py
df1['Age']
```

![](../Images/51feecdbad37e6d507cbf7501fd8db96.png)df1[‘Age’]

我们可以看到，使用这两种语法都会返回数据框的列，我们可以对其进行分析。

## 多列的值

要获取数据框中多个列的值，可以将列名作为列表传递。

```py
df1[["Name","Age"]]
```

![](../Images/f3e2df68cdcf0a5fe482caf94d6c0b4c.png)df1[[“Name”,”Age”]]

我们可以看到它返回了包含 Name 和 Age 两列的数据框。

# Pandas 中 DataFrame 的重要函数

让我们通过使用一个名为‘Titanic’的数据集来深入探讨 DataFrame 的重要函数。这个数据集通常可以在线获得，或者你可以在 [Kaggle](https://www.kaggle.com/c/titanic/data) 找到它。

## 读取数据

Pandas 对读取各种类型的数据提供了良好的内置支持，包括 CSV、fether、excel、HTML、JSON、pickle、SAS、SQL 等。

读取数据的常用语法是

```py
# pd.read_fileExtensionName("File Path")
```

## CSV

要从 CSV 文件中读取数据，你只需使用 pandas 的 read_csv 函数。

```py
df = pd.read_csv('Path_To_CSVFile')
```

由于 Titanic 数据集也以 CSV 格式提供，所以我们将使用 read_csv 函数读取它。当你下载数据集时，你会得到两个名为 train.csv 和 test.csv 的文件，这些文件将帮助测试机器学习模型，所以我们将只关注 train_csv 文件。

```py
df = pd.read_csv('train.csv')
```

现在 df 自动成为一个数据框。让我们探索它的一些函数。

## head()

如果你正常打印你的数据集，它会显示一个完整的数据集，可能有百万行或列，这很难查看和分析。df.head() 函数允许我们查看数据集的前 ’n’ 行（默认 5 行），以便我们可以对数据集做一个粗略估计，并确定接下来要应用的关键函数。

```py
df.head()
```

![](../Images/6809138a901745575a553a2a32df58e9.png)df.head()

现在我们可以看到数据集中的列及其前 5 行的值。由于我们没有传递任何值，因此它显示的是前 5 行。

## tail()

类似于 head 函数，我们还有一个 tail 函数，它显示最后 *n* 个值。

```py
df.tail(3)
```

![](../Images/21eb4f5febca316bff45b34bfbd128d4.png)df.tail(3)

我们可以看到数据集的最后 3 行，因为我们传递了 df.tail(3)。

## shape()

shape() 是另一个重要的函数，用于分析数据集的形状，这在我们制作机器学习模型时非常有用，并且我们希望我们的维度是精确的。

```py
df.shape()
```

![](../Images/82749010f211e57a998c104bfa3c5599.png)df.shape()

在这里我们可以看到我们的输出是 (891,12)，这等于 891 行和 12 列，这意味着我们总共有 12 个特征或 12 列和 891 行或 891 个样本。

之前我们使用 df.tail() 函数时，最后一列的索引号是 890，因为我们的索引是从 0 开始的，而不是从 1 开始的。如果索引号是从 1 开始的，那么最后一列的索引号将是 891。

## isnull()

这是另一个重要的函数，用于查找数据集中为空的值。我们可以在之前的输出中看到一些值是 NaN，这意味着“不是数字”，我们必须处理这些缺失值以获得良好的结果。isnull() 是处理这些空值的重要函数。

```py
df.isnull().head()
```

我正在使用head函数，以便我们可以看到前5个示例，而不是整个数据集。

![](../Images/83de576c429ae703321b77b4484fa58e.png)

df.isnull().head()

在这里，我们可以看到“Cabin”列中的一些值为True。True表示该值为NaN或缺失。我们可以看到这不容易理解，因此我们可以使用sum()函数来获取更详细的信息。

## sum()

sum函数用于计算数据框中所有值的总和。记住True表示1，False表示0，因此为了获取isnull()函数返回的所有True值，我们可以使用sum()函数。让我们检查一下。

```py
df.isnull().sum()
```

![](../Images/a56bed68de5735aba3c0809c8b4146bf.png)df.isnull().sum()

在这里，我们可以看到只有“Age”、“Cabin”和“Embarked”列中有缺失值。

## info()

info函数也是一个常用的pandas函数，它“打印DataFrame的简洁摘要。”

```py
df.info()
```

![](../Images/c20d616c8ff237b204f0ba62b0a457d1.png)df.info()

在这里，我们可以看到它告诉我们有多少个非空实体，例如在年龄字段中，我们有714个非空的float64类型实体。它还告诉我们内存使用情况，在这个例子中是83.6 KB。

## describe()

describe()也是一个非常有用的函数来分析数据。它告诉我们数据框的描述统计信息，包括那些总结数据集分布的中心趋势、离散程度和形状的统计信息，排除值。

```py
df.describe()
```

![](../Images/3aa801ab8f71ceab689c9250d04e3708.png)df.describe()

在这里，我们可以看到每一列的一些重要统计分析，包括均值、标准差、最小值等。更多信息请参见其[文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html)。

# 布尔索引

这也是Numpy和Pandas中一个重要且广泛使用的概念。

正如名称所示，我们使用布尔变量进行索引，即True和False。如果索引为True，则显示该行；如果索引为False，则不显示该行。

当我们尝试从数据集中提取重要特征时，它对我们很有帮助。让我们以“Sex”为“male”的条目为例，看看我们如何解决这个问题。

```py
df["Sex"]=="male"
```

这将返回一个布尔值为True和False的Series，其中True表示“Sex”为“male”的行，否则为False。

为了只查看前10个结果，我可以使用head函数，如下所示。

```py
(df["Sex"]=="male").head(10)
```

![](../Images/508840d2a0fe27bcd0c66b643c8dfb88.png)(df[“sex”]==”male”).head(10)

现在，为了查看完整的数据框，只包含“Sex”为“male”的行，我们应该在数据框中传递df[“Sex”]==”male”以获取所有“Sex”为“male”的结果。

```py
df[df["Sex"]=="male"].head(5)
```

![](../Images/63c60b477e7170ef67fba61dc0a8a9e5.png)

在这里，我们可以看到所有的结果都是“Sex”为Male的行。

现在让我们通过布尔索引提取一些有用的信息。

为了基于多个条件获取行，我们使用括号“()”和“&,|,!=”符号来连接多个条件。

让我们找出所有幸存男性乘客的百分比。

```py
df[(df["Sex"]=="male") & (df["Survived"]==1)]
```

![](../Images/a6d01eb337b12de53bb73107aafbf734.png)

给这段代码一点时间，理解发生了什么。我们正在收集所有行，其中df[“Sex”] == “male”并且df[“Survived”]==1\. 返回值是一个包含所有幸存男性乘客的dataframe。

让我们找出幸存的男性乘客的百分比。

现在，幸存男性乘客的百分比公式是：幸存男性总数 / 男性乘客总数

![](../Images/a3309603d8f6718e8642122df0f8de1a.png)

在pandas中，我们可以这样写

```py
men = df[df['Sex']=='male']['Survived']
perc = sum(men) / len(men) * 100
```

让我们逐步解析这段代码。

df[‘Sex’]==’male’将返回一个布尔Series，其中性别为男性。

df[df[“Sex”]==”male”]将返回所有“Sex”为“male”的示例的完整数据框。

men = df[df[‘Sex’]==’male’][‘Survived’]将返回所有男性乘客的“Survived”列。

sum(men)将计算所有幸存男性的总和。由于这是一个由0和1组成的Series，len(men)将返回男性的总数。

现在将这些放入上面的公式中，我们将找到在Titanic上幸存的所有男性的百分比。

![](../Images/ff899cef506259b24ce2800648fb5efd.png)

18%!!!!! 是的，我们的数据集中只有18%的男性幸存者。

类似地，我们可以为女性编写代码，我不这样做，但这是你的任务，我们发现74%的女性乘客幸存于这次灾难。

这篇文章到此结束。显然，Pandas中还有许多其他重要功能，如groupby、apply、iloc、rename、replace等。我建议你查看Wes编写的[《Python 数据分析》](https://www.amazon.com/Python-Data-Analysis-Wrangling-IPython/dp/1491957662/ref=sr_1_1?crid=2YVHIUW4AOLTD&dchild=1&keywords=python+for+data+analysis&qid=1590067346&sprefix=Python+for+Data+ANa%2Caps%2C348&sr=8-1)书籍，他是这个Pandas库的创始人。

另外，查看一下[这个备忘单](https://www.dataquest.io/blog/pandas-cheat-sheet/)以便快速参考。

**[Ahmad](https://twitter.com/AhmadMustafaAn1)** 对机器学习、深度学习和计算机视觉感兴趣。目前担任Redbuffer的初级机器学习工程师。

### 相关阅读

+   [Numpy和Pandas入门](https://www.kdnuggets.com/introduction-to-numpy-and-pandas)

+   [使用Pandas构建数据科学管道](https://www.kdnuggets.com/building-data-science-pipelines-using-pandas)

+   [数据科学的基本数学：奇异值分解的可视化介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [数据科学入门：初学者指南](https://www.kdnuggets.com/2023/07/introduction-data-science-beginner-guide.html)

+   [数据科学中的数据库简介](https://www.kdnuggets.com/introduction-to-databases-in-data-science)

+   [数据科学中的云计算简介](https://www.kdnuggets.com/introduction-to-cloud-computing-for-data-science)
