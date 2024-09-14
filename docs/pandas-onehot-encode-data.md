# Pandas: 如何进行独热编码

> 原文：[https://www.kdnuggets.com/2023/07/pandas-onehot-encode-data.html](https://www.kdnuggets.com/2023/07/pandas-onehot-encode-data.html)

![Pandas: 如何进行独热编码](../Images/c0f8134b033e517c4d51e4349fe07d0c.png)

图片来自 [Pexels](https://www.pexels.com/photo/a-woman-looking-afar-5473955/)

# 什么是独热编码

* * *

## 我们的 Top 3 课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

独热编码是将类别值转换为兼容的数值表示形式的数据预处理步骤。

| categorical_column | bool_col | col_1 | col_2 | label |
| --- | --- | --- | --- | --- |
| value_A | True | 9 | 4 | 0 |
| value_B | False | 7 | 2 | 0 |
| value_D | True | 9 | 5 | 0 |
| value_D | False | 8 | 3 | 1 |
| value_D | False | 9 | 0 | 1 |
| value_D | False | 5 | 4 | 1 |
| value_B | True | 8 | 1 | 1 |
| value_D | True | 6 | 6 | 1 |
| value_C | True | 0 | 5 | 0 |

例如，对于这个虚拟数据集，类别列具有多个字符串值。许多机器学习算法要求输入数据为数值形式。因此，我们需要将这些数据属性转换为适合这些算法的形式。因此，我们将类别列拆分为多个二值列。

# 如何使用 Pandas 库进行独热编码

首先，将 .csv 文件或任何其他关联文件读入 Pandas 数据框。

```py
df = pd.read_csv("data.csv")
```

为了检查唯一值并更好地理解数据，我们可以使用以下 Panda 函数。

```py
df['categorical_column'].nunique()
df['categorical_column'].unique()
```

对于这些虚拟数据，这些函数返回以下输出：

```py
>>> 4
>>> array(['value_A', 'value_C', 'value_D', 'value_B'], dtype=object)
```

对于类别列，我们可以将其拆分为多个列。为此，我们使用 [pandas.get_dummies()](https://pandas.pydata.org/docs/reference/api/pandas.get_dummies.html) 方法。它接受以下参数：

| 参数 |  |
| --- | --- |
| **数据：** 类数组、Series 或 DataFrame | 原始的 pandas 数据框对象 |
| **列：** 类似列表，默认为 None | 类别列的列表，用于进行独热编码 |
| **drop_first：** 布尔值，默认为 False | 移除类别标签的第一个级别 |

为了更好地理解该函数，让我们对虚拟数据集进行独热编码。

## 类别列的独热编码

我们使用 `get_dummies` 方法并将原始数据框作为 **data** 输入。在 **columns** 中，我们传递一个仅包含 *categorical_column* 头的列表。

```py
df_encoded = pd.get_dummies(df, columns=['categorical_column', ])
```

以下命令删除了 categorical_column 并为每个唯一值创建了一个新列。因此，单个类别列被转换为 4 个新列，其中只有一个列的值为 1，其他 3 列的值均为 0。这就是为什么它被称为一热编码。

| categorical_column_value_A | categorical_column_value_B | categorical_column_value_C | categorical_column_value_D |
| --- | --- | --- | --- |
| 1 | 0 | 0 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 0 | 0 | 1 |
| 0 | 0 | 0 | 1 |
| 0 | 0 | 0 | 1 |
| 0 | 0 | 0 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 |
| 0 | 0 | 0 | 1 |

当我们想对布尔列进行一热编码时会出现问题。这也会创建两个新列。

## 热编码二进制列

```py
df_encoded = pd.get_dummies(df, columns=[bool_col, ])
```

| bool_col_False | bool_col_True |
| --- | --- |
| 0 | 1 |
| 1 | 0 |
| 0 | 1 |
| 1 | 0 |

当我们可以仅使用一列将 True 编码为 1，False 编码为 0 时，不必要地增加了一列。为解决此问题，我们使用 **drop_first** 参数。

```py
df_encoded = pd.get_dummies(df, columns=['bool_col'], drop_first=True)
```

| bool_col_True |
| --- |
| 1 |
| 0 |
| 1 |
| 0 |

# 结论

虚拟数据集经过一热编码后，最终结果如下

| col_1 | col_2 | bool | A | B | C | D | label |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 9 | 4 | 1 | 1 | 0 | 0 | 0 | 0 |
| 7 | 2 | 0 | 0 | 1 | 0 | 0 | 0 |
| 9 | 5 | 1 | 0 | 0 | 0 | 1 | 0 |
| 8 | 3 | 0 | 0 | 0 | 0 | 1 | 1 |
| 9 | 0 | 0 | 0 | 0 | 0 | 1 | 1 |
| 5 | 4 | 0 | 0 | 0 | 0 | 1 | 1 |
| 8 | 1 | 1 | 0 | 1 | 0 | 0 | 1 |
| 6 | 6 | 1 | 0 | 0 | 0 | 1 | 1 |
| 0 | 5 | 1 | 0 | 0 | 1 | 0 | 0 |
| 1 | 8 | 1 | 0 | 0 | 0 | 1 | 0 |

类别值和布尔值已经转换为可以作为机器学习算法输入的数值。

**[穆罕默德·阿赫姆](https://www.linkedin.com/in/muhammad-arham-a5b1b1237/)** 是一名深度学习工程师，专注于计算机视觉和自然语言处理。他曾参与多个生成式 AI 应用的部署和优化，这些应用在 Vyro.AI 全球排行榜上名列前茅。他对构建和优化智能系统的机器学习模型充满兴趣，并相信持续改进。

### 更多相关主题

+   [数据科学中的 Pandas 入门](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

+   [使用 Pandas 进行数据摄取：初学者教程](https://www.kdnuggets.com/2022/04/data-ingestion-pandas-beginner-tutorial.html)

+   [通过 Pandas 管道简化数据处理](https://www.kdnuggets.com/2022/08/simplify-data-processing-pandas-pipeline.html)

+   [10 个 Pandas 单行代码用于数据访问、操作和管理](https://www.kdnuggets.com/2023/01/pandas-one-liners-data-access-manipulation-management.html)

+   [使用 Pandas fillna() 输入缺失数据的最佳方法](https://www.kdnuggets.com/2023/02/optimal-way-input-missing-data-pandas-fillna.html)

+   [使用 Pandas 进行数据清理](https://www.kdnuggets.com/data-cleaning-with-pandas)
