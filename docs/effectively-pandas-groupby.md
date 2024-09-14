# 如何有效使用 Pandas GroupBy

> 原文：[`www.kdnuggets.com/2023/01/effectively-pandas-groupby.html`](https://www.kdnuggets.com/2023/01/effectively-pandas-groupby.html)

Pandas 是一个功能强大且广泛使用的开源库，用于使用 Python 进行数据操作和分析。它的一个关键特性是能够使用 groupby 函数通过基于一个或多个列将数据框分成组，然后对每个组应用各种聚合函数。

![如何有效使用 Pandas GroupBy](img/879f79de072a25cc8d7dcf4ed4c20a83.png)

图片来自 [Unsplash](https://unsplash.com/photos/_9a-3NO5KJE)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护

* * *

`groupby` 函数非常强大，因为它允许你快速总结和分析大数据集。例如，你可以根据特定列对数据集进行分组，并计算每个组中剩余列的均值、总和或计数。你还可以通过多个列进行分组，以获得对数据的更详细理解。此外，它还允许你应用自定义聚合函数，这对于复杂的数据分析任务是一个非常强大的工具。

在本教程中，你将学习如何使用 Pandas 的 groupby 函数来对不同类型的数据进行分组并执行不同的聚合操作。通过本教程，你应该能够使用这个函数以多种方式分析和总结数据。

# 实践代码示例

当概念经过充分练习后就会内化，这也是我们接下来要做的，即实际操作 Pandas 的 groupby 函数。建议使用 [Jupyter Notebook](https://jupyter.org/) 来完成本教程，因为你可以在每一步查看输出结果。

## 生成示例数据

导入以下库：

+   Pandas: 创建数据框并应用分组

+   Random - 用于生成随机数据

+   Pprint - 用于打印字典

```py
import pandas as pd
import random
import pprint
```

接下来，我们将初始化一个空的数据框，并填入每一列的值，如下所示：

```py
df = pd.DataFrame()
names = [
    "Sankepally",
    "Astitva",
    "Shagun",
    "SURAJ",
    "Amit",
    "RITAM",
    "Rishav",
    "Chandan",
    "Diganta",
    "Abhishek",
    "Arpit",
    "Salman",
    "Anup",
    "Santosh",
    "Richard",
]

major = [
    "Electrical Engineering",
    "Mechanical Engineering",
    "Electronic Engineering",
    "Computer Engineering",
    "Artificial Intelligence",
    "Biotechnology",
]

yr_adm = random.sample(list(range(2018, 2023)) * 100, 15)
marks = random.sample(range(40, 101), 15)
num_add_sbj = random.sample(list(range(2)) * 100, 15)

df["St_Name"] = names
df["Major"] = random.sample(major * 100, 15)
df["yr_adm"] = yr_adm
df["Marks"] = marks
df["num_add_sbj"] = num_add_sbj
df.head() 
```

额外提示 – 更简洁的方法是创建一个包含所有变量及其值的字典，然后将其转换为数据框。

```py
student_dict = {
    "St_Name": [
        "Sankepally",
        "Astitva",
        "Shagun",
        "SURAJ",
        "Amit",
        "RITAM",
        "Rishav",
        "Chandan",
        "Diganta",
        "Abhishek",
        "Arpit",
        "Salman",
        "Anup",
        "Santosh",
        "Richard",
    ],
    "Major": random.sample(
        [
            "Electrical Engineering",
            "Mechanical Engineering",
            "Electronic Engineering",
            "Computer Engineering",
            "Artificial Intelligence",
            "Biotechnology",
        ]
        * 100,
        15,
    ),
    "Year_adm": random.sample(list(range(2018, 2023)) * 100, 15),
    "Marks": random.sample(range(40, 101), 15),
    "num_add_sbj": random.sample(list(range(2)) * 100, 15),
}
df = pd.DataFrame(student_dict)
df.head() 
```

数据框的外观如下所示。在运行此代码时，由于我们使用的是随机样本，某些值可能不会匹配。

![如何有效使用 Pandas GroupBy](img/60567c4eba6d039fbeee0ec6f5df1ca1.png)

## 创建分组

让我们按“Major”科目分组数据，并应用组过滤以查看有多少记录落入此组。

```py
groups = df.groupby('Major')
groups.get_group('Electrical Engineering')
```

所以，四名学生属于电气工程专业。

![如何有效使用 Pandas GroupBy](img/1a5918b5b870497bb38c450cbeac5a5d.png)

你也可以按多个列分组（在此情况下为 Major 和 num_add_sbj）。

```py
groups = df.groupby(['Major', 'num_add_sbj'])
```

注意，所有可以应用于单列组的聚合函数都可以应用于多列组。在接下来的教程中，让我们以单列为例深入探讨不同类型的聚合。

让我们使用“Major”列上的`groupby`创建组。

```py
groups = df.groupby('Major')
```

## 应用直接函数

假设你想找到每个 Major 的平均分数。你会怎么做？

+   选择 Marks 列

+   应用均值函数

+   应用`round`函数将成绩四舍五入到两位小数（可选）

```py
groups['Marks'].mean().round(2)
```

```py
Major
Artificial Intelligence    63.6
Computer Engineering       45.5
Electrical Engineering     71.0
Electronic Engineering     92.0
Mechanical Engineering     64.5
Name: Marks, dtype: float64
```

## 聚合

另一种实现相同结果的方法是使用如下所示的聚合函数：

```py
groups['Marks'].aggregate('mean').round(2)
```

你也可以通过将函数作为字符串列表传递给组来应用多个聚合。

```py
groups['Marks'].aggregate(['mean', 'median', 'std']).round(2)
```

![如何有效使用 Pandas GroupBy](img/ab37326bb82a68c07d628410297a4895.png)

但如果你需要对不同的列应用不同的函数怎么办？不用担心。你也可以通过传递{column: function}对来做到这一点。

```py
groups.aggregate({'Year_adm': 'median', 'Marks': 'mean'})
```

![如何有效使用 Pandas GroupBy](img/2ed28413582b51776378cf5af557fc17.png)

## 变换

你可能需要对特定列执行自定义变换，这可以通过使用`groupby()`轻松实现。让我们定义一个类似于 sklearn 的预处理模块中的标准缩放器。你可以通过调用`transform`方法并传递自定义函数来变换所有列。

```py
def standard_scalar(x):
    return (x - x.mean())/x.std()
groups.transform(standard_scalar)
```

![如何有效使用 Pandas GroupBy](img/5b0d46f642b0c1b03290ae8809752001.png)

注意，“NaN”表示标准差为零的组。

## 过滤

你可能想检查哪个“Major”表现不佳，即平均“Marks”低于 60 的那个。这需要你对组应用一个带有函数的过滤方法。下面的代码使用一个 lambda 函数来实现过滤结果。

```py
groups.filter(lambda x: x['Marks'].mean() < 60)
```

![如何有效使用 Pandas GroupBy](img/fa2ab6e7d1ff79db689a9143e56d3f92.png)

## 首先

它给你按索引排序的第一个实例。

```py
groups.first()
```

![如何有效使用 Pandas GroupBy](img/6b33f5b854a68e6d80ac7805244e1a35.png)

## 描述

“describe”方法返回给定列的基本统计数据，如计数、均值、标准差、最小值、最大值等。

```py
groups['Marks'].describe()
```

![如何有效使用 Pandas GroupBy](img/ced1ca2d73e200609134a1ee14a3779b.png)

## 大小

Size，如名称所示，返回每组的大小，即记录数量。

```py
groups.size()
```

```py
Major
Artificial Intelligence    5
Computer Engineering       2
Electrical Engineering     4
Electronic Engineering     2
Mechanical Engineering     2
dtype: int64
```

## 计数和唯一值

“Count”返回所有值，而“Nunique”仅返回该组中的唯一值。

```py
groups.count()
```

![如何有效使用 Pandas GroupBy](img/603e4e537e29626e3efea386b1d5afef.png)

```py
groups.nunique()
```

![如何有效使用 Pandas GroupBy](img/ea473d0a224cf4a05b25cd20778995f9.png)

## 重命名

你也可以根据自己的偏好重命名聚合列的名称。

```py
groups.aggregate("median").rename(
    columns={
        "yr_adm": "median year of admission",
        "num_add_sbj": "median additional subject count",
    }
) 
```

![如何有效使用 Pandas GroupBy](img/bdfd7daf7a367cc9b644c1556a8454db.png)

# 充分利用 groupby 函数。

+   **明确 groupby 的目的：** 你是想通过一列对数据进行分组，以获取另一列的均值？还是通过多列对数据进行分组，以获取每组中的行数？

+   **了解数据框的索引：** groupby 函数使用索引来分组数据。如果你想通过某列对数据进行分组，确保该列被设置为索引，或者可以使用.set_index()

+   **使用适当的聚合函数**：可以与各种聚合函数一起使用，如 mean()、sum()、count()、min()、max()。

+   **使用 as_index 参数：** 当设置为 False 时，此参数告诉 pandas 将分组列用作常规列而不是索引。

你还可以将 groupby()与其他 pandas 函数（如 pivot_table()、crosstab()和 cut()）结合使用，以从数据中提取更多洞察。

# 摘要

groupby 函数是数据分析和处理的强大工具，因为它允许你根据一个或多个列对数据行进行分组，然后对这些组执行聚合计算。教程展示了如何通过代码示例使用 groupby 函数的各种方法。希望它能帮助你理解这些选项及其在数据分析中的作用。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位人工智能策略师和数字化转型领导者，她在产品、科学和工程交汇处工作，致力于构建可扩展的机器学习系统。她是一位获奖的创新领导者、作者和国际演讲者。她的使命是使机器学习民主化，并打破行话，让每个人都能参与这场变革。

### 了解更多相关话题

+   [数据分析：分析数据的四种方法及其有效应用](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [如何有效管理 Docker 镜像版本的标签](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

+   [数据可视化：有效呈现复杂信息](https://www.kdnuggets.com/data-visualization-presenting-complex-information-effectively)

+   [如何使用条件格式化增强 Pandas 中的数据可视化](https://www.kdnuggets.com/how-to-use-conditional-formatting-in-pandas-to-enhance-data-visualization)

+   [如何使用 pivot_table 函数进行高级数据汇总](https://www.kdnuggets.com/how-to-use-the-pivot_table-function-for-advanced-data-summarization-in-pandas)

+   [如何在 Pandas 中使用 MultiIndex 进行层次化数据组织](https://www.kdnuggets.com/how-to-use-multiindex-for-hierarchical-data-organization-in-pandas)
