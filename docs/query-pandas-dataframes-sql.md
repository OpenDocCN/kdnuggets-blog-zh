# 使用 SQL 查询你的 Pandas DataFrames

> 原文：[`www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html`](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

![使用 SQL 查询你的 Pandas DataFrames](img/2ea84109c8d12ff5edd83ecd449d24b9.png)

图片由 Jeffrey Czum 提供，来源于 Pexels（作者编辑）

Pandas —— 或更具体地说，它的主要数据容器，[DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html) —— 早已巩固了自己作为 Python 数据生态系统中的标准表格数据存储结构的地位。使用 Pandas DataFrame 具有访问、操控和对复合数据进行计算的特定规范，这些规范可以通过时间和坚持掌握，因为它遵循 Python 语法；如果你对 Python 非常熟悉，你已经在熟练掌握标准 Pandas API 的路上了。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

有很多用户对 Python 的掌握并不是很熟练，但由于 Python 在数据科学和数据分析以及相关领域和职业中的突出地位，他们还是选择了这门语言。许多转向 Python 的人可能已经对另一种重量级数据科学语言 SQL 感到熟悉。那么，为什么我们不能使用 SQL 这种专为表格数据访问而编写的语言来查询 Pandas DataFrames 中的数据呢？

进入 Fugue。Fugue 是一个在设计时考虑了逻辑与执行解耦的项目，主要目的是简化计算并行化。

> Fugue 是一个抽象框架，它允许用户用原生 Python 或 Pandas 编写代码，然后将其移植到 Spark 和 Dask。

这项工作的核心组成部分是 FugueSQL。[FugueSQL](https://fugue-tutorials.readthedocs.io/en/latest/tutorials/beginner/beginner_sql.html) 不是**纯** SQL；它描述其语法为“标准 SQL、json 和 python 之间的混合”。然而，你会发现，对于基本的查询操作，它的表现或多或少符合你的预期。具体来说，它被设计为“完全兼容标准 SQL SELECT 语句”。因此，对于数据处理的新手来说，它通常比在 Python 中查询 Pandas 更直接。

FugueSQL 的好处可以在并行代码中实现，结合 Fugue 项目的附加目标，或单独使用，在单台计算机上查询 DataFrames。本文将介绍如何立即在自己的工作中开始使用 FugueSQL，并提供相应的示例。

## 安装与准备

如果你想安装完整的 Fugue 库，以利用它所有的并行化功能，你可以这样安装：

```py
pip install fugue
```

如果你只想要 FugueSQL API——这是本文示例所需的全部内容——可以这样安装：

```py
pip install fugue[sql]
```

安装后，导入所需的库：

```py
import pandas as pd
from fugue_sql import fsql
```

这样，只剩下创建一个可以在示例中查询的数据框。由于我需要这样做——并且我目前正在深入整理我的漫画书收藏——我们可以创建一个简单的玩具漫画书数据集。

简单的数据框将包含 4 列：某个漫画书标题的特定问题；漫画书出版商；专业且独立分配的[漫画书评级](https://www.mycomicshop.com/help/grading)（10 分制）；该漫画书在其分配等级中的价值。

以下是实现上述目标的代码：

```py
comics_df = pd.DataFrame({'book': ['Secret Wars 8',
                                   'Tomb of Dracula 10',
                                   'Amazing Spider-Man 252',
                                   'New Mutants 98',
                                   'Eternals 1',
                                   'Amazing Spider-Man 300',
                                   'Department of Truth 1'],
                          'publisher': ['Marvel', 'Marvel', 'Marvel', 'Marvel', 'Marvel', 'Marvel', 'Image'],
                          'grade': [9.6, 5.0, 7.5, 8.0, 9.2, 6.5, 9.8],
                          'value': [400, 2500, 300, 600, 400, 750, 175]})

print(comics_df)
```

```py
                     book publisher  grade  value
0           Secret Wars 8    Marvel    9.6    400
1      Tomb of Dracula 10    Marvel    5.0   2500
2  Amazing Spider-Man 252    Marvel    7.5    300
3          New Mutants 98    Marvel    8.0    600
4              Eternals 1    Marvel    9.2    400
5  Amazing Spider-Man 300    Marvel    6.5    750
6   Department of Truth 1     Image    9.8    175
```

你不需要理解（或关心）有关漫画书收藏的更多信息来理解以下示例，所以我们开始吧。

## 示例 1：我的书中哪些评级超过 8.0？

标题已经说明了一切。我们将使用 FugueSQL 和标准的 SELECT 语句来查询我们的 Pandas DataFrame，以确定我的哪些漫画书在 10 分制中评分超过 8.0。

为此，我们首先需要定义 SQL 语句：

```py
# which of my books are graded above 8.0?
query_1 = """
SELECT book, publisher, grade, value FROM comics_df
WHERE grade > 8.0
PRINT
"""
```

注意在我们的 SELECT 语句中使用 Pandas DataFrame 作为表名。

一旦查询定义完成，必须使用 Fugue 查询引擎执行它：

```py
fsql(query_1).run()
```

这是我们返回的查询结果：

```py
PandasDataFrame
book:str                                                      |publisher:str|grade:double|value:long
--------------------------------------------------------------+-------------+------------+----------
Secret Wars 8                                                 |Marvel       |9.6         |400       
Eternals 1                                                    |Marvel       |9.2         |400       
Department of Truth 1                                         |Image        |9.8         |175       
Total count: 3
```

在我们的数据框中，有 3 本书的评级超过 8.0：秘密战争 8；永恒族 1；和真相部 1。

如果你将这个结果与我们的原始数据框进行比较，你会发现我们的查询已被正确返回。成功了！非常酷，而且如果你熟悉 SQL 语法，这非常直接。

## 示例 2：我的书中哪些价值超过 500？

让我们看第二个示例，在这个示例中，我们将找出哪些书籍的价值超过 $500，考虑到它们的评级和本文发布时的市场情况。

再次，我们将首先定义 SQL 语句，然后使用 FugueSQL 引擎执行它。

```py
# which of my books are valued above 500?
query_2 = """
SELECT book, publisher, grade, value FROM comics_df
WHERE value > 500
PRINT
"""

fsql(query_2).run()
```

结果如下：

```py
PandasDataFrame
book:str                                                      |publisher:str|grade:double|value:long
--------------------------------------------------------------+-------------+------------+----------
Tomb of Dracula 10                                            |Marvel       |5.0         |2500      
New Mutants 98                                                |Marvel       |8.0         |600       
Amazing Spider-Man 300                                        |Marvel       |6.5         |750       
Total count: 3
```

在我们的数据框中，有 3 本书的价值超过 $500\。它们恰好是 3 个非常受欢迎角色的首次完整亮相，这导致了它们相对较高的价值：吸血鬼猎人布雷德；死侍；以及毒液，分别是。

## 示例 3：我的书中哪些是由 Image 出版的？

在我们的最后一个示例中，让我们看看这些漫画中哪些是由 Image Comics 出版的。

```py
# which of my books are published by Image?
query_3 = """
SELECT book, publisher, grade, value FROM comics_df
WHERE publisher = 'Image'
PRINT
"""

fsql(query_3).run()
```

```py
PandasDataFrame
book:str                                                      |publisher:str|grade:double|value:long
--------------------------------------------------------------+-------------+------------+----------
Department of Truth 1                                         |Image        |9.8         |175       
Total count: 1
```

只有这一本符合条件：《真相部》1，本书是一部精心创作的当代杰作，基于流行的阴谋论实际上改变现实的想法，是 Image 出版的唯一一本。

那么，返回的查询结果的数据类型是什么？让我们重新运行上述查询，将其存储到变量中，并检查其类型。

```py
result = fsql(query_3).run()
print(type(result))
```

```py
fugue.dataframe.dataframes.DataFrames
```

返回的结果是 Fugue dataframe 的形式。了解更多关于 Fugue dataframe 的信息，请点击 [这里](https://fugue.readthedocs.io/en/latest/api/fugue.dataframe.html)，以及如何进一步处理返回结果。

要获取更多示例，请 [查看这个 FugueSQL 教程](https://fugue-tutorials.readthedocs.io/en/latest/tutorials/beginner/index.html)。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是 KDnuggets 的数据科学家和主编，KDnuggets 是重要的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络和机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系。

### 进一步了解此主题

+   [SQL 查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [提高 SQL 查询性能的 5 个技巧](https://www.kdnuggets.com/5-tips-for-improving-sql-query-performance)

+   [如何合并 Pandas DataFrames](https://www.kdnuggets.com/2023/01/merge-pandas-dataframes.html)

+   [合并 Pandas DataFrames 的 3 种方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [将 JSON 转换为 Pandas DataFrames：正确解析的方法](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)

+   [如何使用 Pandas 高效地合并大型 DataFrames](https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas)
