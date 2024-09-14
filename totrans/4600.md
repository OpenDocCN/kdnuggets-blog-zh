# Python的Datatable包概述

> 原文：[https://www.kdnuggets.com/2019/08/overview-python-datatable-package.html](https://www.kdnuggets.com/2019/08/overview-python-datatable-package.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[Parul Pandey](https://www.linkedin.com/in/parul-pandey-a5498975/)**

![figure-name](../Images/7481a0bbd700ed4cf33ab74b776fecc3.png)照片由[Johannes Groll](https://unsplash.com/@followhansi?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> * * *
> 
> ## 我们的前三大课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT
> 
> * * *
> 
> ***“在文明的曙光到2003年之间创建了5 Exabytes的信息，而现在每2天就会创建这么多信息”：Eric Schmidt***

如果你是R用户，你可能已经在使用`data.table`包。`[Data.table](https://cran.r-project.org/web/packages/data.table/data.table.pdf)`是R中的`[data.frame](https://www.rdocumentation.org/packages/base/versions/3.6.0/topics/data.frame)`包的扩展。它也是R用户在快速聚合大型数据（包括100GB内存）时的首选包。

R的`data.table`包因其易用性、便利性和编程速度而非常多功能且高性能。它在R社区中相当著名，每月下载量超过40万次，几乎有650个CRAN和Bioconductor包在使用它([source](https://github.com/Rdatatable/data.table/wiki))。

那么，对于Python用户来说有什么好处呢？好消息是，`data.table`包在Python中也有一个对应的包叫做`datatable`，它明确关注大数据支持、高性能、内存和外存数据集，以及多线程算法。从某种程度上来说，它可以被称为[***data.table***](https://github.com/Rdatatable/data.table)***的***“小弟弟”。

### Datatable

![figure-name](../Images/0763f680ee28628fdab89ac81a193692.png)

现代机器学习应用需要处理大量数据并生成多个特征。这是为了构建更高准确性的模型。Python的`datatable`模块就是为了解决这个问题而创建的。它是一个在单节点机器上以最大可能速度执行大数据（最多100GB）操作的工具包。datatable的开发得到了[H2O.ai](https://www.h2o.ai/)的赞助，`datatable`的首个用户是[Driverless.ai](https://www.h2o.ai/driverless-ai/)。

这个工具包与[pandas](https://github.com/pandas-dev/pandas)非常相似，但更注重速度和大数据支持。Python的`datatable`也致力于实现良好的用户体验、实用的错误信息和强大的API。在本文中，我们将探讨如何使用datatable，以及在处理大型数据集时它相较于pandas的优势。

### 安装

在MacOS上，可以通过pip轻松安装datatable：

```py
pip install datatable
```

在Linux上，安装可以通过以下二进制发行版完成：

```py
# If you have Python 3.5
pip install https://s3.amazonaws.com/h2o-release/datatable/stable/datatable-0.8.0/datatable-0.8.0-cp35-cp35m-linux_x86_64.whl# If you have Python 3.6
pip install https://s3.amazonaws.com/h2o-release/datatable/stable/datatable-0.8.0/datatable-0.8.0-cp36-cp36m-linux_x86_64.whl
```

*目前，datatable在Windows上不可用，但正在进行工作以添加对Windows的支持。*

欲了解更多信息，请参阅[构建说明](https://datatable.readthedocs.io/en/latest/install.html)。

本文的代码可以从关联的[Github存储库](https://github.com/parulnith/An-Overview-of-Python-s-Datatable-package)访问，或通过点击下面的图像在我的binder上查看。

![figure-name](../Images/983e69bc5a6820ce52e8678f39635e95.png)

### 读取数据

使用的数据集来自Kaggle，属于[*Lending Club Loan Data Dataset*](https://www.kaggle.com/wendykan/lending-club-loan-data#loan.csv)*。*数据集包含了2007-2015年间所有贷款的完整信息，包括当前的贷款状态（当前、逾期、已还清等）和最新的支付信息。文件包含**226万行**和**145列**。数据规模非常适合展示datatable库的能力。

```py
# Importing necessary Librariesimport numpy as np
import pandas as pd
import datatable as dt

```

让我们将数据加载到`Frame`对象中。在datatable中，`Frame`是基本的分析单元。这与pandas的DataFrame或SQL表相同：数据以二维数组的形式排列，具有行和列。

**使用datatable**

```py
%%time
datatable_df = dt.fread("data.csv")
____________________________________________________________________CPU times: user 30 s, sys: 3.39 s, total: 33.4 s
Wall time: 23.6 s
```

上面的`fread()`函数既强大又极快。它可以自动检测和解析大多数文本文件的参数，从.zip归档或URL加载数据，读取Excel文件，等等。

此外，datatable解析器：

+   可以自动检测分隔符、标题、列类型、引号规则等。

+   可以从多个来源读取数据，包括文件、URL、shell、原始文本、归档和通配符。

+   提供多线程文件读取以实现最大速度

+   读取大型文件时包含进度指示器

+   可以读取[RFC4180](https://tools.ietf.org/html/rfc4180)合规和不合规的文件。

**使用pandas**

现在，让我们计算 pandas 读取相同文件所花费的时间。

```py
%%time
pandas_df= pd.read_csv("data.csv")
___________________________________________________________CPU times: user 47.5 s, sys: 12.1 s, total: 59.6 s
Wall time: 1min 4s
```

结果显示，datatable 在读取大型数据集时明显优于 pandas。相比之下，pandas 需要超过一分钟，而 datatable 只需几秒钟。

### 数据框转换

现有数据框也可以转换为 numpy 或 pandas 数据框，如下所示：

```py
numpy_df = datatable_df.to_numpy()
pandas_df = datatable_df.to_pandas()
```

让我们将现有数据框转换为 pandas 数据框对象，并比较所需的时间。

```py
%%time
datatable_pandas = datatable_df.to_pandas()
___________________________________________________________________
CPU times: user 17.1 s, sys: 4 s, total: 21.1 s
Wall time: 21.4 s
```

似乎将文件作为 datatable 数据框读取，然后转换为 pandas 数据框所需的时间少于直接通过 pandas 数据框读取。 ***因此，将大型数据文件通过 datatable 导入，然后转换为 pandas 数据框可能是一个好主意。***

```py
type(datatable_pandas)
___________________________________________________________________
pandas.core.frame.DataFrame
```

### 基本数据框属性

让我们看看 datatable 数据框的一些基本属性，这些属性类似于 pandas 的属性：

```py
print(datatable_df.shape)       # (nrows, ncols)
print(datatable_df.names[:5])   # top 5 column names
print(datatable_df.stypes[:5])  # column types(top 5)
______________________________________________________________(2260668, 145)('id', 'member_id', 'loan_amnt', 'funded_amnt', 'funded_amnt_inv')(stype.bool8, stype.bool8, stype.int32, stype.int32, stype.float64)
```

我们还可以使用 `head` 命令输出前 'n' 行。

```py
datatable_df.head(10)
```

![figure-name](../Images/babf69d7f56dac36df2a78e6ec46e712.png)datatable 数据框前 10 行的快照

颜色表示数据类型，其中 **红色** 表示字符串，**绿色** 表示整数，**蓝色** 表示浮点数。

### 摘要统计量

在 pandas 中计算摘要统计量是一个内存消耗的过程，但使用 datatable 就不再如此。我们可以使用 datatable 计算每列的以下摘要统计量：

```py
datatable_df.sum()      datatable_df.nunique()
datatable_df.sd()       datatable_df.max()
datatable_df.mode()     datatable_df.min()
datatable_df.nmodal()   datatable_df.mean()
```

让我们使用 datatable 和 pandas 计算列的 **均值** 以测量时间差异。

**使用 datatable**

```py
%%time
datatable_df.mean()
_______________________________________________________________
CPU times: user 5.11 s, sys: 51.8 ms, total: 5.16 s
Wall time: 1.43 s
```

**使用 pandas**

```py
pandas_df.mean()
__________________________________________________________________
Throws memory error.
```

上述命令在 pandas 中无法完成，因为它开始抛出内存错误。

### 数据操作

数据表（如数据框）是列式数据结构。在 datatable 中，所有这些操作的主要工具是 **方括号符号**，灵感来自传统的矩阵索引，但具有更多功能。

![figure-name](../Images/980feab7a7cfe86abd29322e4b493f21.png)datatable 的 **方括号符号**

相同的 DT[i, j] 符号在数学中用于索引矩阵，在 C/C++、R、pandas、numpy 等中也使用。让我们看看如何使用 datatable 执行常见的数据操作活动：

### #选择行/列的子集

以下代码从数据集中选择所有行和 `funded_amnt` 列。

```py
datatable_df[:,'funded_amnt']
```

![figure-name](../Images/9e3b3de577f99c31b9929fa9cc65e524.png)

以下是选择前 5 行和 3 列的方法

```py
datatable_df[:5,:3]
```

![figure-name](../Images/6ca189ee0c88fce7688e515eae1ffdab.png)

### #排序数据框

**使用 datatable**

通过 `datatable` 可以按照特定列对数据框进行排序，如下所示：

```py
%%time
datatable_df.sort('funded_amnt_inv')
_________________________________________________________________
CPU times: user 534 ms, sys: 67.9 ms, total: 602 ms
Wall time: 179 ms
```

**使用 pandas：**

```py
%%time
pandas_df.sort_values(by = 'funded_amnt_inv')
___________________________________________________________________
CPU times: user 8.76 s, sys: 2.87 s, total: 11.6 s
Wall time: 12.4 s
```

注意 datatable 和 pandas 之间的显著时间差异。

### #删除行/列

以下是如何删除名为 `member_id` 的列：

```py
del datatable_df[:, 'member_id']
```

### #分组

与 pandas 类似，datatable 也具有 groupby 功能。让我们看看如何按 `grade` 列分组并获取 `funded_amount` 列的均值。

**使用 datatable**

```py
%%time
for i in range(100):
    datatable_df[:, dt.sum(dt.f.funded_amnt), dt.by(dt.f.grade)]
____________________________________________________________________
CPU times: user 6.41 s, sys: 1.34 s, total: 7.76 s
Wall time: 2.42 s
```

**使用 pandas**

```py
%%time
for i in range(100):
    pandas_df.groupby("grade")["funded_amnt"].sum()
____________________________________________________________________
CPU times: user 12.9 s, sys: 859 ms, total: 13.7 s
Wall time: 13.9 s
```

**.f 代表什么？**

`f` 代表 `frame proxy`，并提供了一种简单的方式来引用我们当前操作的 Frame。在我们的例子中，`dt.f` 仅代表 `dt_df`。

### #筛选行

筛选行的语法与 GroupBy 非常相似。让我们筛选出 `loan_amnt` 中那些值大于 `funded_amnt` 的行。

```py
datatable_df[dt.f.loan_amnt>dt.f.funded_amnt,"loan_amnt"]
```

### 保存 Frame

也可以将 Frame 的内容写入一个 `csv` 文件，以便将来使用。

```py
datatable_df.to_csv('output.csv')
```

有关更多数据处理函数，请参阅 [**文档**](https://datatable.readthedocs.io/en/latest/using-datatable.html) 页面。

### 结论

与默认的 pandas 相比，datatable 模块确实加快了执行速度，这在处理大数据集时无疑是一个福音。然而，datatable 在功能方面落后于 pandas。不过，由于 datatable 仍在积极开发中，我们可能会在未来看到一些重大更新。

### 参考文献

+   R 的 [data.table](https://github.com/Rdatatable/data.table/wiki)

+   [Datatable 文档](https://datatable.readthedocs.io/en/latest/index.html)

+   [Python datatable 入门](https://www.kaggle.com/sudalairajkumar/getting-started-with-python-datatable): 一篇关于 datatable 使用的精彩 Kaggle Kernel

**简介: [Parul Pandey](https://www.linkedin.com/in/parul-pandey-a5498975/)** 是一位数据科学爱好者，经常为数据科学出版物如 Towards Data Science 撰写文章。

[原文](https://towardsdatascience.com/an-overview-of-pythons-datatable-package-5d3a97394ee9)。经许可转载。

**相关内容:**

+   [25 个 Pandas 技巧](/2019/08/25-tricks-pandas.html)

+   [10 个简单技巧加速 Python 数据分析](/2019/07/10-simple-hacks-speed-data-analysis-python.html)

+   [成为 Pandas 专家，Python 的数据处理库](/2019/06/pro-pandas-python-library.html)

### 相关主题

+   [如何使用 MLFlow 打包和分发机器学习模型](https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html)

+   [文本摘要方法: 概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [机器学习的数据标注: 市场概述、方法和工具](https://www.kdnuggets.com/2021/12/data-labeling-ml-overview-and-tools.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [Mercury 概述: 创建数据科学作品集和……](https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html)

+   [线性机器学习算法概述](https://www.kdnuggets.com/2022/07/linear-machine-learning-algorithms-overview.html)
