# Pandas 的 5 个高级功能及其使用方法

> 原文：[`www.kdnuggets.com/2019/10/5-advanced-features-pandas.html`](https://www.kdnuggets.com/2019/10/5-advanced-features-pandas.html)

评论

![](img/fe9f215c9ed6ef36135e0104a0d1c0ae.png)

Pandas 是处理数据的黄金标准库。具备加载、过滤、操作和探索数据的功能，难怪它是 [数据科学家们的最爱](https://www.kdnuggets.com/2018/06/top-20-python-libraries-data-science-2018.html)。

我们大多数人自然会坚持使用 Pandas 的基础功能。从 CSV 文件中加载数据，过滤几个列，然后直接跳入 [数据可视化](https://towardsdatascience.com/5-quick-and-easy-data-visualizations-in-python-with-code-a2284bae952f)。然而，Pandas 实际上包含了许多不太为人知但非常有用的函数，这些函数可以使数据处理变得更加简单和清晰。

本教程将引导你了解这些更高级的功能 — 它们的作用及如何使用。与数据一起玩得更开心！

### (1) 配置选项和设置

Pandas 提供了一组用户可配置的选项和设置。这些设置是巨大的生产力提升工具，因为它们允许你根据自己的喜好精确调整 Pandas 环境。

例如，我们可以更改一些 Pandas 的显示设置，以改变显示的行和列数以及浮点数的显示精度。

上面的代码确保 Pandas 始终显示最多 10 行和 10 列，浮点值显示最多 2 位小数。这样，当我们尝试打印出一个大的 DataFrame 时，我们的终端或 Jupyter Notebook 就不会看起来一团糟！

这只是一个基本示例。除了简单的显示设置，还有很多东西可以探索。你可以查看所有选项的 [官方文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html)。

### (2) 合并 DataFrame

Pandas DataFrame 的一个相对不为人知的部分是，实际上有两种不同的方法来合并它们。每种方法产生不同的结果，因此根据你的需求选择合适的方法非常重要。此外，它们包含许多参数，可以进一步自定义合并。让我们来了解一下。

**拼接**

拼接是合并 DataFrame 的最常见方法，可以直观地理解为“堆叠”。这种堆叠可以是水平的，也可以是垂直的。

想象一下你有一个巨大的 CSV 格式数据集。将其拆分成多个文件以便于处理是有意义的（这在大数据集的处理中很常见，称为 *分片*）。

当你将数据加载到 Pandas 中时，你可以将每个 CSV 的 DataFrame 垂直堆叠，以创建一个包含所有数据的大 DataFrame。例如，如果我们有 3 个分片，每个分片有 500 万行，那么在将它们垂直堆叠后，我们的最终 DataFrame 将有 1500 万行。

下面的代码展示了如何在 Pandas 中垂直拼接 DataFrames。

你可以通过根据列而不是行来拆分数据集，来做类似的操作——为每个 CSV 文件提供几个列（包含数据集的所有行）。这就像是将数据集的特征分成不同的碎片。然后你可以水平堆叠它们来组合这些列/特征。

**合并**

合并（Merging）更复杂但更强大，采用类似 SQL 的风格将 Pandas DataFrames 组合在一起，即通过某些共同的属性来连接 DataFrames。

假设你有两个描述你的 YouTube 频道的 DataFrames。其中一个包含用户 ID 列表以及每个用户在你的频道上花费的总时间。另一个包含类似的用户 ID 列表以及每个用户观看的视频数量。合并允许我们通过匹配用户 ID 将这两个 DataFrames 组合成一个，通过将 ID、时间和视频数量放在每个用户的单独行中。

在 Pandas 中合并两个 DataFrames 是通过*merge*函数完成的。你可以在下面的代码中看到它是如何工作的。*left* 和 *right* 参数指的是你希望合并的两个 DataFrames，而 *on* 指定了用于匹配的列。

为了更进一步模拟 SQL 连接，*how* 参数允许你选择要执行的 SQL 风格连接类型：内连接、外连接、左连接或右连接。要了解更多关于 SQL 连接的信息，请参见 [W3Schools 教程](https://www.w3schools.com/sql/sql_join.asp)。

### (3) 重塑 DataFrames

有**几种**方法来重塑和重构 Pandas DataFrames。这些方法从简单易用到强大复杂。让我们来看看最常见的三种方法。在以下所有示例中，我们将使用这份超级英雄数据集！

**转置**

其中最简单的。转置将 DataFrame 的行和列互换。如果你有 5000 行和 10 列，然后转置你的 DataFrame，你将得到 10 行和 5000 列。

**分组**

分组（Groupby）的主要用途是根据某些键将 DataFrames 拆分成多个部分。一旦 DataFrame 被拆分成多个部分，你可以遍历并对每个部分独立应用一些操作。

例如，我们可以看到下面的代码中，我们创建了一个包含年份和得分的球员 DataFrame。然后我们进行了*groupby*操作，将 DataFrame 按照球员拆分成多个部分。因此，每个球员都有自己的组，显示该球员在每一年中获得的分数。

**堆叠**

堆叠（Stacking）将 DataFrame 转换为多层索引，即每一行具有多个子部分。这些子部分是通过 DataFrame 的列创建的，将它们压缩成多重索引。总体来说，堆叠可以看作是将列压缩成多重索引的行。

下面的例子最好地说明了这一点。

### (4) 处理时间数据

[Datetime 库](https://docs.python.org/3.6/library/datetime.html) 是 Python 的一个重要库。无论何时处理与现实世界日期和时间信息相关的内容，它都是你首选的库。幸运的是，Pandas 也提供了使用 Datetime 对象的功能。

让我们用一个例子来说明。在下面的代码中，我们首先创建一个包含 4 列的 DataFrame：Day、Month、Year 和 data，然后按年份和月份进行排序。正如你所看到的，这样非常杂乱；我们使用了 3 列来存储日期，但实际上我们知道一个日历日期只是一个值。

我们可以通过 datetime 来整理数据。

Pandas 方便地提供了一个名为 *to_datetime()* 的函数，可以将多个 DataFrame 列压缩并转换为一个单一的 Datetime 对象。一旦转化为这种格式，你就可以利用 Datetime 库的所有功能。

使用 *to_datetime()* 函数时，你需要将所有相关列中的“日期”数据传递给它。这包括“Day”、“Month”和“Year”列。一旦我们将数据转化为 Datetime 格式，我们就不再需要其他列，只需将其删除即可。查看下面的代码，了解具体的操作方法！

### (5) 将项目映射到组

映射是一个有用的技巧，有助于组织分类数据。举个例子，假设我们有一个包含数千行的大型 DataFrame，其中一列包含我们希望分类的项目。这样做可以大大简化机器学习模型的训练和数据的有效可视化。

查看下面的代码，了解我们如何将食品列表进行分类的迷你示例。

在上面的代码中，我们将列表放入了 pandas 系列中。我们还创建了一个字典，显示了我们想要的映射，将每个食品项分类为“Protein”或“Carb”。这是一个简单的示例，但如果这个系列的规模很大，比如有 1,000,000 项，那么遍历它就不再实际。

我们可以使用 Pandas 内置的 *.map()* 函数来以优化的方式执行映射，而不是基本的 for 循环。查看下面的代码，了解函数及其应用。

在函数中，我们首先遍历字典，创建一个新字典，其中键表示 pandas 系列中的每一个可能项，而值表示新映射项，“Protein” 或 “Carbs”。然后，我们简单地应用 Pandas 的内置映射函数来映射系列中的所有值。

查看下面的输出以查看结果！

```py
['Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Carbs', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein', 'Carbs', 'Carbs', 'Protein', 'Protein', 'Protein']

```

### 结论

所以就这样！这是 Pandas 的 5 个高级功能及其使用方法！

如果你渴望更多内容，不必担心！还有更多关于 Pandas 和数据科学的内容可以学习。作为推荐阅读，[KDNuggets 网站](https://www.kdnuggets.com/) 当然是这个主题的最佳资源！

**相关：**

+   [使用 Python 进行探索性数据分析](https://www.kdnuggets.com/2019/08/activestate-exploratory-data-analysis-python.html)

+   [Pandas 的 25 个技巧](https://www.kdnuggets.com/2019/08/25-tricks-pandas.html)

+   [成为 Pandas 的专家，Python 的数据处理库](https://www.kdnuggets.com/2019/06/pro-pandas-python-library.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

### 关于这个话题的更多内容

+   [如何使用 pivot_table 函数进行高级数据汇总…](https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas)

+   [数据科学编程语言及其使用时机](https://www.kdnuggets.com/2022/02/data-science-programming-languages.html)

+   [数据分析：四种数据分析方法及其应用…](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [KDnuggets™ 新闻 22:n06, 2 月 9 日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)

+   [将 JSON 转换为 Pandas DataFrames：正确解析的方式](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)

+   [MLOps：最佳实践及其应用方法](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)
