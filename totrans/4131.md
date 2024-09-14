# 简化 Pandas DataFrame 的合并

> 原文：[https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)

![简化 Pandas DataFrame 的合并](../Images/7b9ab67e85f49434e4247a4e417d520a.png)

# 介绍

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

在许多现实场景中，我们的数据为了管理和简便被放置在不同的文件中。我们常常需要将它们合并为一个更大的 DataFrame 进行分析。Pandas 包通过提供各种合并 DataFrame 的方法，如 concat 和 merge，来帮助我们。此外，它还提供了用于比较的工具。

我们将通过示例来理解这两种方法的工作原理。对于本教程，我们假设你对 Python 和 Pandas 有基本了解。

# 连接 DataFrame

如果两个 DataFrame 的格式相同，我们选择 **.concat()** 方法。它将一个 DataFrame 的列或行附加到另一个 DataFrame 上。**.concat()** 负责在指定轴上执行连接操作。在我们深入了解其细节之前，先快速看一下函数语法及其参数。

```py
pd.concat(objs, axis=0, join="outer",ignore_index=False, keys=None,
    levels=None, names=None, verify_integrity=False, copy=True, )
```

虽然有很多内容，但我们将重点关注主要参数：

+   **objs:** 要连接的 DataFrame 对象或 Series 列表

+   **axis:** axis = 0 告诉 pandas 将第二个 DataFrame 堆叠在第一个 DataFrame 下方（按行连接），而 axis = 1 告诉 pandas 将第二个 DataFrame 堆叠在第一个 DataFrame 右侧（按列连接）。默认情况下，axis = 0。

+   **join:** 默认情况下，join=outer，outer 用于并集，inner 用于交集

+   **ignore_index:** 在连接过程中将忽略索引轴，默认情况下保持为 False，因为它们在连接中受到尊重。然而，如果索引轴没有有意义的信息，那么忽略它们是有用的。

+   **verify_integrity:** 它检查新连接轴中的重复项，但默认情况下保持为 False，因为相对于实际的连接操作，它可能非常昂贵。

## 示例

```py
import pandas as pd

# First Dict with two 2 and 4 columns
data_one = {'A' : ['A1','A2','A3','A4'] , 'B' : ['B1','B2','B3','B4']}

# Second Dict with two 2 and 4 columns
data_two = {'C' : ['C1','C2','C3','C4'] , 'D' : ['D1','D2','D3', 'D4']}

# Converting to DataFrames
df_one=pd.DataFrame(data_one)
df_two=pd.DataFrame(data_two)
```

![简化 Pandas DataFrame 的合并](../Images/3ab00aa88b932891643a4b09c5d9886c.png)

## 场景 1：按列连接

假设列 A、B、C 和 D 代表不同的功能，但索引号指的是相同的 ID，那么将 C 和 D 行合并到 df_one 的右侧更有意义。让我们来做一下，

```py
# Joining the columns
new_df = pd.concat([df_one,df_two],axis=1)
```

![结合 Pandas DataFrames 变得简单](../Images/e3e38ec873ee132212617dd711c1ece7.png)

## 场景 2：按行拼接

如果我们继续上述假设并按行拼接它们，那么结果 DataFrame 将会有很多 NaN 值缺失数据。原因很简单，假设在 df_two 中的索引 = 0，我们不能直接将其放置在列 A 和 B 下，因为我们认为它们是两个不同的函数，而值不存在。因此，pandas 会自动创建一个包含 A、B、C 和 D 的完整行，并在各自的列中填充缺失值。

```py
# Joining the rows
new_df1 = pd.concat([df_one,df_two],axis=0)
```

![结合 Pandas DataFrames 变得简单](../Images/9cb34e027f6e45cf4cecdca485a04b16.png)

请注意，我们的最终结果中有重复的索引。这是因为默认情况下 ignore_index 设置为 False，它保留了 DataFrames 的原始索引。这在我们的情况下可能不实用，所以我们将设置 **ignore_index = True**。

```py
# Joining the rows
new_df2 = pd.concat([df_one,df_two],axis=0 , ignore_index= True)
```

![结合 Pandas DataFrames 变得简单](../Images/bfab7bcbeb018fcf4139c5bf3010f220.png)

如果两个 DataFrame 中的列表示相同的内容，只是名称不同，那么我们可以在拼接之前重命名这些列。

```py
# Joining the rows
df_two.columns = df_one.columns
new_df3 = pd.concat([df_one,df_two],axis=0 , ignore_index= True)
```

![结合 Pandas DataFrames 变得简单](../Images/e101dd738e72dbe3132c768db927712a.png)

# 合并 DataFrames

合并或连接 DataFrames 与拼接不同。拼接仅仅是将一个 DataFrame 堆叠在另一个 DataFrame 上，沿着所需的轴。而连接就像 SQL 中的连接一样。我们可以基于一个唯一的列合并 DataFrames。这些方法具有高性能，并表现得显著更好。当一个 DataFrame 是包含额外数据的“查找表”，我们想要将其连接到另一个 DataFrame 时，这非常有用。让我们看看它的语法和参数：

```py
pd.merge( left, right, how="inner", on=None, left_on=None, right_on=None,
	left_index=False, right_index=False, sort=True, suffixes=("_x", "_y"),
	copy=True, indicator=False, validate=None)
```

让我们看看它的主要参数及其用途：

+   **left：** DataFrame 对象

+   **right：** DataFrame 对象

+   **how：** 连接类型，即 inner、outer、left、right 等。默认情况下 how = “inner”

+   **on：** 在两个 DataFrame 中都存在的列，并且构成连接操作的基础。

+   **left_on：** 如果您使用 right_index 作为连接的列，则必须指定将用作键的列。

+   **right_on：** 左侧的反向操作

+   **left_index：** 如果设置为 True，则使用左侧 DataFrame 的行标签（索引）作为连接键。

+   **right_index：** 左侧索引的反向操作

+   **sort：** 按连接键的字典顺序对结果 DataFrame 进行排序，但不推荐这样做，因为这会显著影响性能。

+   **suffixes：** 如果我们有重叠的列，可以为它们指定后缀以区分它们。默认情况下，它设置为 ('_x', '_y')。

## 示例

以体育和图书馆数据框为例，假设两个 DataFrame 中的 **“name**” 列是唯一的，并可以用作主键。

```py
import pandas as pd

# First DataFrame
sports= pd.DataFrame({'sports_id':[1,2,3,4],'name':['Alex','David','James','Sara']})

# Second DataFrame
library= pd.DataFrame({'library_id':[1,2,3,4],'name':['David','James','Peter','Harry']})
```

![结合 Pandas DataFrames 变得简单](../Images/42b17b07355c081a2e6b047336d0ef21.png)

## 情景 1: 内连接和外连接

对于这两者，你在 **.merge()** 方法中放置数据框的顺序并不重要，因为最终结果仍然是相同的。如果我们将两个 DataFrame 视为两个集合，则内连接指的是它们的交集，而外连接指的是它们的并集。

![结合 Pandas DataFrames 变得简单](../Images/9bddf553892cc1c48d64a0f3597b4f0e.png)

```py
inner_join = pd.merge(sports,library,how="inner",on="name")
outer_join= pd.merge(library,sports,how="outer",on="name")
```

![结合 Pandas DataFrames 变得简单](../Images/b57c2f11ecedc5902abc368700e4b2a6.png)

## 情景 2: 左连接和右连接

在这里，我们放置 DataFrames 的顺序是重要的。例如，在左连接中，位于左侧位置的 DataFrame 对象的所有条目将显示出来，并且其匹配的条目来自右侧 DataFrame 对象。右连接则完全相反。让我们通过一个示例来理解这个概念，

![结合 Pandas DataFrames 变得简单](../Images/ad620c09165771ebd5597a7df4e07720.png)

```py
left_join = pd.merge(sports,library,how="left",on="name")

right_join= pd.merge(library,sports,how="right",on="name")
```

![结合 Pandas DataFrames 变得简单](../Images/d13f69f05a2609b00ee91fe799c182b6.png)

## 情景 3: 使用索引进行连接

首先，让我们通过使用 **set_index()** 方法将“name”列修改为索引。

```py
sports = sports.set_index("name")
```

![结合 Pandas DataFrames 变得简单](../Images/60eb93309b75987c40f2be1f2f7b0349.png)

现在我们将应用内连接操作，并将 **left_index= True** 设置为如果我们将体育作为左侧数据框。

```py
index_join = pd.merge(sports,library,how="inner",left_index = True , right_on="name")
```

![结合 Pandas DataFrames 变得简单](../Images/b568af6dd6fb9ed8d02ba5bc52caf5a5.png)

注意，它显示了与我们之前对内连接所做的结果相同。

## 情景 4: 后缀属性

让我们对原始数据框做一个小修改，将 sports_id 和 login_id 改为 id。现在虽然名称相同，但我们指的是不同的 id。Pandas 足够聪明，能够识别它，并自动添加后缀。稍后我们将看到如何自定义这些后缀。

```py
# First DataFrame
sports= pd.DataFrame({'id':[1,2,3,4],'name':['Alex','David','James','Sara']})

# Second DataFrame
library= pd.DataFrame({'id':[1,2,3,4],'name':['David','James','Peter','Harry']})
```

![结合 Pandas DataFrames 变得简单](../Images/42b17b07355c081a2e6b047336d0ef21.png)

```py
inner_join = pd.merge(sports,library,how="inner",on="name")
```

![结合 Pandas DataFrames 变得简单](../Images/f2e6ce7b393de613e1cc968f5c21c427.png)

这里，id_x 指代左侧数据框，而 id_y 指代右侧数据框。虽然它完成了工作，但为了提高可读性和代码的简洁性，我们通过使用 **suffixes** 属性来自定义这些后缀。

```py
suffix_ = pd.merge(sports,library,how="inner",on="name", suffixes=('_sport','_lib'))
```

![结合 Pandas DataFrames 变得简单](../Images/777258dd3015f367aa0128c63b1b029f.png)

# 结论

本文旨在简化在 Pandas 中合并行的概念。当你从大型 .csv 文件中提取数据时，建议在执行合并操作之前先分析数据的类型和格式。Pandas 在这方面也有一些优秀的方法。如果遇到问题，你应该参考 Pandas 的官方文档。最后但同样重要的是，一切都需要通过实践来掌握，因此在自己的时间里尝试不同的 DataFrames，以便更好地理解。

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)** 是一位有抱负的软件开发人员，她相信持续的努力和承诺。她是一位雄心勃勃的程序员，对数据科学和机器学习领域充满浓厚兴趣。

### 更多相关主题

+   [使用 SQL 查询 Pandas DataFrames](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [使用 apply() 方法处理 Pandas DataFrames](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [如何合并 Pandas DataFrames](https://www.kdnuggets.com/2023/01/merge-pandas-dataframes.html)

+   [合并 Pandas DataFrames 的 3 种方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [将 JSON 转换为 Pandas DataFrames：正确解析它们](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)

+   [如何高效地使用 Pandas 合并大型 DataFrames](https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas)
