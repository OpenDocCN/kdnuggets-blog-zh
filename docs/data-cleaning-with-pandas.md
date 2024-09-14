# 数据清洗与 Pandas

> 原文：[https://www.kdnuggets.com/data-cleaning-with-pandas](https://www.kdnuggets.com/data-cleaning-with-pandas)

![Data Cleaning with Pandas](../Images/4d7ef9a022460c8e9009883084da54ff.png)

作者提供的图片

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

如果你对数据科学感兴趣，那么数据清洗可能对你来说是一个熟悉的术语。如果不是，让我来解释一下。我们的数据通常来自多个资源，并且不干净。它可能包含缺失值、重复项、错误或不需要的格式等。对这些混乱的数据进行实验会导致不正确的结果。因此，在将数据输入到模型之前，准备数据是必要的。通过识别和解决潜在的错误、不准确和不一致性来准备数据，这个过程称为 **数据清洗**。

在本教程中，我将带你深入了解使用 Pandas 清理数据的过程。

# 数据集

我将使用著名的 [Iris](https://archive.ics.uci.edu/dataset/53/iris) 数据集。Iris 数据集包含了三种鸢尾花的四个特征的测量值：花萼长度、花萼宽度、花瓣长度和花瓣宽度。我们将使用以下库：

***   **Pandas:** 强大的数据处理和分析库

+   **Scikit-learn:** 提供数据预处理和机器学习的工具

# 数据清洗步骤

## 1\. 加载数据集

使用 Pandas 的 **read_csv()** 函数加载 Iris 数据集：

```py
column_names = ['id', 'sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'species']
iris_data = pd.read_csv('data/Iris.csv', names= column_names, header=0)
iris_data.head()
```

**输出：**

| **id** | **sepal_length** | **sepal_width** | **petal_length** | **petal_width** | **species** |
| --- | --- | --- | --- | --- | --- |
| 1 | 5.1 | 3.5 | 1.4 | 0.2 | Iris-setosa |
| 2 | 4.9 | 3.0 | 1.4 | 0.2 | Iris-setosa |
| 3 | 4.7 | 3.2 | 1.3 | 0.2 | Iris-setosa |
| 4 | 4.6 | 3.1 | 1.5 | 0.2 | Iris-setosa |
| 5 | 5.0 | 3.6 | 1.4 | 0.2 | Iris-setosa |

**header=0** 参数表示 CSV 文件的第一行包含列名（标题）。

## 2\. 探索数据集

为了获得关于我们数据集的见解，我们将使用 pandas 中的内置函数打印一些基本信息。

```py
print(iris_data.info())
print(iris_data.describe())
```

**输出：**

```py
 <class>RangeIndex: 150 entries, 0 to 149
Data columns (total 6 columns):
 #   Column        Non-Null Count  Dtype  
---  ------        --------------  -----  
 0   id            150 non-null    int64  
 1   sepal_length  150 non-null    float64
 2   sepal_width   150 non-null    float64
 3   petal_length  150 non-null    float64
 4   petal_width   150 non-null    float64
 5   species       150 non-null    object 
dtypes: float64(4), int64(1), object(1)
memory usage: 7.2+ KB
None</class>
```

![Data Cleaning with Pandas](../Images/2358828d44ba39e3f759d6dc9986b6e8.png)

iris_data.describe() 的输出

**info() 函数** 对于了解数据框的整体结构、每列中的非空值数量和内存使用情况非常有用。而总结统计提供了数据集中数值特征的概述。

## 3\. 检查类别分布

这是理解分类列中类别分布的重要步骤，这对于分类任务至关重要。你可以使用 pandas 中的 value_counts() 函数执行这一步。

```py
print(iris_data['species'].value_counts())
```

**输出：**

```py
Iris-setosa        50
Iris-versicolor    50
Iris-virginica     50
Name: species, dtype: int64
```

我们的结果显示数据集是平衡的，每个物种的表示数量相等。这为在所有 3 个类别之间进行公平评估和比较奠定了基础。

## 4\. 删除缺失值

由于从**info()**方法中可以明显看出我们有 5 列数据没有缺失值，因此我们将跳过这一步。但如果遇到任何缺失值，请使用以下命令处理它们：

```py
iris_data.dropna(inplace=True)
```

## 5\. 删除重复项

由于重复项可能扭曲我们的分析，因此我们从数据集中删除它们。我们将首先使用以下命令检查它们的存在：

```py
duplicate_rows = iris_data.duplicated()
print("Number of duplicate rows:", duplicate_rows.sum())
```

**输出：**

```py
Number of duplicate rows: 0
```

我们的这个数据集没有重复项。不过，可以通过**drop_duplicates()**函数删除重复项。

```py
iris_data.drop_duplicates(inplace=True)
```

## 6\. 独热编码

对于分类分析，我们将对物种列执行独热编码。进行这一步的原因是机器学习算法更倾向于使用数值数据。独热编码过程将分类变量转换为二进制（0 或 1）格式。

```py
encoded_species = pd.get_dummies(iris_data['species'], prefix='species', drop_first=False).astype('int')
iris_data = pd.concat([iris_data, encoded_species], axis=1)
iris_data.drop(columns=['species'], inplace=True)
```

![使用 Pandas 进行数据清洗](../Images/476d06368bc9b14541a405819bc3d427.png)

图片来源：作者

## 7\. 浮点值列的归一化

归一化是将数值特征缩放到均值为 0 和标准差为 1 的过程。进行此过程是为了确保特征对分析的贡献相等。我们将对浮点值列进行归一化，以实现一致的缩放。

```py
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
cols_to_normalize = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
scaled_data = scaler.fit(iris_data[cols_to_normalize])
iris_data[cols_to_normalize] = scaler.transform(iris_data[cols_to_normalize])
```

![使用 Pandas 进行数据清洗](../Images/69e35ac2ad4d8fff336dc543bc02288c.png)

归一化后的 iris_data.describe() 输出

## 8\. 保存清洗后的数据集

将清洗后的数据集保存到新的 CSV 文件中。

```py
iris_data.to_csv('cleaned_iris.csv', index=False)
```

# 总结

恭喜你！你已经成功使用 pandas 清洗了第一个数据集。在处理复杂数据集时，你可能会遇到额外的挑战。然而，这里提到的基本技术将帮助你入门并准备数据以进行分析。

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1)** 是一位有志于成为软件开发者的人员，对数据科学和 AI 在医学中的应用充满兴趣。Kanwal 被选为 2022 年 APAC 区域的 Google Generation 学者。Kanwal 喜欢通过撰写有关热门话题的文章分享技术知识，并热衷于提高女性在科技行业中的代表性。

### 更多相关内容

+   [掌握 Python 和 Pandas 数据清洗的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-data-cleaning-with-python-and-pandas)

+   [在 Pandas 中清洗和预处理文本数据以进行 NLP 任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

+   [掌握SQL、Python、数据清洗、数据整理及探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据科学中数据清洗的重要性](https://www.kdnuggets.com/2023/08/importance-data-cleaning-data-science.html)

+   [通过这本免费电子书学习数据清洗和数据预处理](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [SQL中的数据清洗：如何为分析准备混乱的数据](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)**
