# 每一种复杂 DataFrame 操作，直观地解释与可视化

> 原文：[https://www.kdnuggets.com/2020/11/dataframe-manipulation-explained-visualized.html](https://www.kdnuggets.com/2020/11/dataframe-manipulation-explained-visualized.html)

[评论](#comments)

**由 [Andre Ye](https://www.linkedin.com/in/andre-ye-501746150/)，Critiq 的联合创始人，Medium 的编辑和顶级作者**。

![](../Images/192e134ecba0ea1bad5e37024965a72f.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

Pandas 提供了广泛的 DataFrame 操作，但其中许多操作复杂且不易上手。本文介绍了 8 种基本的 DataFrame 操作方法，涵盖了数据科学家需要了解的几乎所有操作函数。每种方法将包括解释、可视化、代码以及记忆技巧。

### Pivot

旋转一个表格会创建一个新的“旋转表”，将现有的数据列投影为新表中的元素，包括索引、列和值。初始 DataFrame 中将成为索引和列的列显示为唯一值，这两个列的组合将显示为值。这意味着旋转不能处理重复值。

![](../Images/367020d75f24e109d31f8a72808fdd0e.png)

代码以旋转名为 *df* 的 DataFrame 如下：

```py
df.pivot(index='foo', columns='bar', values='baz')

```

*记忆技巧*：在数据操作的领域之外，旋转是围绕某种对象进行的转动。在体育中，人们可以通过脚来“旋转”：pandas 中的旋转也是类似的。原始 DataFrame 的状态围绕 DataFrame 的中心元素旋转成一个新的 DataFrame。有些元素非常字面意义上旋转或转换（如列 *bar*）。

### Melt

Melt 可以被视为一种“反旋转”，因为它将基于矩阵的数据（具有两个维度）转换为基于列表的数据（列表示值，行表示唯一数据点），而旋转则相反。考虑一个二维矩阵，其中一个维度为 *B* 和 *C*（列名），另一个维度为 *a*、*b* 和 *c*（行索引）。

我们选择一个 ID、一个维度以及一个或多个列来包含值。包含值的列被转换为两列：一列用于变量（值列的名称），另一列用于值（包含的数字）。

![](../Images/3bc36709af67c589c9d6ed6bef85e4d2.png)

结果是 ID 列值（*a, b, c*）和值列（*B, C*）的每种组合，以及其对应的值，以列表格式组织。

melt 操作可以这样在 DataFrame *df* 上执行：

```py
df.melt(id_vars=['A'], value_vars=['B','C'])

```

*记住*: 将某物像蜡烛一样融化，是将一个固化的复合物转变为几个更小的独立元素（蜡滴）。融化一个二维 DataFrame 会解开其固化的结构，并将其各部分记录为列表中的独立条目。

### 爆炸

爆炸是一个有用的方法，可以清除数据中的列表。当某列被爆炸时，列中的所有列表都作为新行列出在相同的索引下（为避免此情况，只需在之后调用 *.reset_index()*）。非列表项如字符串或数字不会受到影响，空列表是 NaN 值（可以使用 *.dropna()* 清理这些）。

![](../Images/8ccb6230eae6b6cab6ea6310ac933642.png)

爆炸 DataFrame *df* 中的列 ‘*A*’ 非常简单：

```py
df.explode(‘A’)

```

*记住*: 爆炸某物会释放其所有内部内容——爆炸一个列表会将其元素分开。

### 堆叠

堆叠将任何大小的 DataFrame 进行‘堆叠’，将列作为现有索引的子索引。因此，结果 DataFrame 只有一列和两个层级的索引。

![](../Images/496b8b6fade8aebd1a78f684b34a1a3a.png)

对名为 *df* 的表格进行堆叠非常简单，使用 *df.stack()*。

为了访问，比如说，狗的身高，只需调用基于索引的检索两次，如 *df.loc[‘dog’].loc[‘height’]*。

*记住*: 从视觉上看，*stack* 将表格的二维性转换为将列堆叠成多层索引。

### Unstack

Unstacking 将一个多层索引的 DataFrame 进行“展开”，将指定层级的索引转换为新 DataFrame 的列，并附上其对应的值。对一个表格执行一次 stack 再执行一次 unstack 不会改变它（不考虑存在的‘*0*’）。

![](../Images/9ee5f14a02aebb804806f543ad924317.png)

Unstacking 的一个参数是层级。在列表索引中，-1 索引会返回最后一个元素；层级也是如此。层级为 -1 表示将最后一个索引层级（最右边的那个）展开。举个进一步的例子，当层级设置为 0（第一个索引层级）时，它中的值会变成列，而接下来的索引层级（第二个）则会变成变换后 DataFrame 的索引。

![](../Images/fe321ae0b445c7229f94b09c3d0c05ed.png)

Unstacking 的操作方式与 stacking 相同，但需要层级参数：*df.unstack(level=-1)*。

*记住*: Unstack 意思是“撤销堆叠”。

### 合并

合并两个 DataFrame 是在共享的‘键’上将它们按列（水平）组合起来。这个键允许将表格合并，即使它们的顺序不同。完成合并的 DataFrame 将默认在值列上添加后缀 *x* 和 *y*。

![](../Images/4b7c9f494929a29c1e248ba37f54c193.png)

要合并两个DataFrames *df1* 和 *df2*（其中 *df1* 包含 *leftkey*，*df2* 包含 *rightkey*），调用：

```py
df1.merge(df2, left_on='leftkey', right_on='rightkey')

```

合并不是pandas的函数，而是附加到DataFrame上的。总是假定合并附加到的DataFrame是‘左表’，而在函数中作为参数调用的DataFrame是‘右表’，并具有对应的键。

合并函数默认执行的是所谓的内连接（inner join）：如果每个DataFrame中有一个在另一个DataFrame中未列出的键，则该键不会包含在合并后的DataFrame中。另一方面，如果一个键在同一个DataFrame中列出两次，那么所有相同键的值组合都会在合并后的表中列出。例如，如果 *df1* 中的键 *foo* 有 3 个值，而 *df2* 中有 2 个相同键的值，则最终DataFrame中会有 6 个条目，其中 *leftkey=foo* 和 *rightkey=foo*。

![](../Images/962c8295ba621a22e5715c85fc8da0d5.png)

*记住*：合并DataFrames 就像在驾驶时合并车道 —— 水平地进行。想象每一列都是高速公路上的一个车道；为了合并，它们必须水平结合。

### 连接（Join）

连接（Joins）通常比合并（merge）更受青睐，因为它具有更简洁的语法和更广泛的水平合并DataFrames的可能性。连接的语法如下：

```py
df1.join(other=df2, on='common_key', how='join_method')

```

使用连接时，公共键列（类似于合并中的 *right_on* 和 *left_on*）必须命名相同。 *how* 参数是一个字符串，指代 *join* 可以将两个DataFrame组合在一起的四种方法之一：

+   ‘*left*’：包括所有 *df1* 的元素，仅当 *df2* 的键是 *df1* 的键时才附加 *df2* 的元素。否则，合并后的DataFrame中 *df2* 的缺失部分将标记为 NaN。

+   ‘*right*’：‘*left*’，但在另一个 DataFrame 上。包括所有 *df2* 的元素，仅当 *df1* 的键是 *df2* 的键时。

+   ‘*outer*’：包括两个DataFrame中的所有元素，即使一个键在另一个DataFrame中不存在 —— 缺失的元素标记为 NaN。

+   ‘*inner*’：仅包括键在两个DataFrame键中都存在的元素（交集）。合并的默认设置。

*记住*：如果你曾使用SQL，那么‘join’一词应该立即与你的列级添加关联。如果没有，那么‘join’和‘merge’在定义上非常相似。

### 连接（Concat）

合并（merges）和连接（joins）是水平操作，而连接（concatenations）或简称为concats，则是按行（垂直）附加DataFrames。例如，考虑两个具有相同列名的DataFrames *df1* 和 *df2*，使用 *pandas.concat([df1, df2])* 进行连接：

![](../Images/1cda4fbd10fe600a5fa28427292c4649.png)

虽然你可以通过将轴参数设置为 *1* 使用 concat 进行列级连接，但直接使用 join 会更方便。

请注意，concat 是 pandas 的函数，而不是 DataFrame 的函数。因此，它接收一个需要连接的 DataFrame 列表。

如果DataFrame有一个不包含在另一个DataFrame中的列，默认情况下，该列会被包括在内，缺失值列为NaN。为了避免这种情况，添加一个额外的参数，*join=’inner’*，这将只连接两个DataFrame共有的列。

![](../Images/66b0963eca891dec54a8f3e9bebe01bf.png)

*记住*: 在列表和字符串中，可以追加额外的项目。连接是将额外的元素附加到现有内容中，而不是添加新信息（如按列连接）。由于每个索引/行是一个单独的项，连接会将额外的项添加到DataFrame中，可以将其视为行的列表。

Append是另一种合并两个DataFrame的方法，但它执行与concat相同的功能，效率和灵活性较低。

有时内置函数不够用。

尽管这些函数覆盖了你可能需要的数据操作范围，但有时所需的数据操作过于复杂，一个或一系列函数无法完成。探索复杂的数据操作方法，如解析函数、迭代投影、高效解析等，更多内容请见[这里](https://medium.com/analytics-vidhya/tips-tricks-techniques-to-take-your-data-wrangling-skills-to-the-next-level-c912c23b20cb)。

[原始文档](https://medium.com/analytics-vidhya/every-dataframe-manipulation-explained-visualized-intuitively-dbeea7a5529e)。经许可转载。

**相关：**

+   [强化版探索性数据分析](https://www.kdnuggets.com/2020/07/exploratory-data-analysis-steroids.html)

+   [如何准备你的数据](https://www.kdnuggets.com/2020/06/how-prepare-your-data.html)

+   [数据科学中的Pandas简介](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

### 更多相关话题

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并以目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的AI失败，进行检讨](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
