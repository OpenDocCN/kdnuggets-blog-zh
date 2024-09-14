# 如何高效合并大型 DataFrames

> 原文：[https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas](https://www.kdnuggets.com/how-to-merge-large-dataframes-efficiently-with-pandas)

![如何高效合并大型 DataFrames](../Images/81ec6527fc5abdff19a63f285a14f816.png)

图片由编辑 | Midjourney & Canva 提供 让我们学习如何高效合并大型 DataFrames。

## 准备工作

* * *

## 我们的 top 3 课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

确保你的环境中已安装 Pandas 包。如果没有，你可以通过以下代码使用 pip 安装：

```py
pip install pandas
```

在安装了 Pandas 包之后，我们将在下一部分中学习更多内容。

## 高效合并 Pandas DataFrames

Pandas 是一个开源数据处理包，数据社区中的许多人都在使用。它是一个灵活的包，可以处理许多数据任务，包括数据合并。合并，另一方面，是指基于公共列或索引将两个或更多数据集组合在一起的活动。它主要用于当我们有多个数据集并希望合并其信息时。

在现实世界中，我们往往会看到多个大型表格。当我们将表格转化为 Pandas DataFrames 时，我们可以操作和合并它们。然而，更大的尺寸意味着计算负担更重，并且需要更多的资源。

这就是为什么有少数方法可以提高合并大型 Pandas DataFrames 的效率。

首先，如适用，让我们使用更节省内存的类型，例如类别类型和较小的浮点类型。

```py
df1['object1'] = df1['object1'].astype('category')
df2['object2'] = df2['object2'].astype('category')

df1['numeric1'] = df1['numeric1'].astype('float32')
df2['numeric2'] = df2['numeric2'].astype('float32')
```

然后，尝试将要合并的关键列设置为索引。这是因为基于索引的合并更快。

```py
df1.set_index('key', inplace=True) 
df2.set_index('key', inplace=True)
```

接下来，我们使用 DataFrame 的 `.merge` 方法，而不是 `pd.merge` 函数，因为它在性能上更高效和优化。

```py
merged_df = df1.merge(df2, left_index=True, right_index=True, how='inner') 
```

最后，你可以调试整个过程以了解哪些行来自哪个 DataFrame。

```py
merged_df_debug = pd.merge(df1.reset_index(), df2.reset_index(), on='key', how='outer', indicator=True)
```

使用这种方法，你可以提高合并大型 DataFrames 的效率。

## 额外资源

+   [合并 Pandas DataFrames 的 3 种方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [如何合并 Pandas DataFrames](kdnuggets.com/2023/01/merge-pandas-dataframes.html/)

+   [掌握 Python 数据科学：超越基础](https://www.kdnuggets.com/mastering-python-for-data-science-beyond-the-basics)

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一位数据科学助理经理和数据撰写者。在全职工作于 Allianz Indonesia 时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉猎各种 AI 和机器学习主题。

### 更多相关话题

+   [如何合并 Pandas DataFrame](https://www.kdnuggets.com/2023/01/merge-pandas-dataframes.html)

+   [合并 Pandas DataFrame 的 3 种方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [用 SQL 查询 Pandas DataFrame](https://www.kdnuggets.com/2021/10/query-pandas-dataframes-sql.html)

+   [使用 Pandas Dataframe 的 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [简化 Pandas DataFrame 的合并](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)

+   [将 JSON 转换为 Pandas DataFrame：正确解析的方法](https://www.kdnuggets.com/converting-jsons-to-pandas-dataframes-parsing-them-the-right-way)
