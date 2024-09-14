# 使用 Pandas 构建数据科学管道

> 原文：[https://www.kdnuggets.com/building-data-science-pipelines-using-pandas](https://www.kdnuggets.com/building-data-science-pipelines-using-pandas)

![使用 Pandas 构建数据科学管道](../Images/05c0bb2b8602f22678bebd3cab880760.png)

使用 ChatGPT 生成的图像

Pandas 是最受欢迎的数据处理和分析工具之一，以其易用性和强大功能而著称。但您是否知道，您还可以使用它来创建和执行数据管道以处理和分析数据集？

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

在本教程中，我们将学习如何使用 Pandas 的 `pipe` 方法来构建端到端的数据科学管道。该管道包括数据摄取、数据清理、数据分析和数据可视化等多个步骤。为了突出这种方法的优点，我们还将比较基于管道的代码与非管道替代方案，以便让您清晰了解其差异和优势。

## 什么是 Pandas 管道？

Pandas `pipe` 方法是一种强大的工具，允许用户以清晰且可读的方式链接多个数据处理函数。此方法可以处理位置参数和关键字参数，使其适用于各种自定义函数。

简而言之，Pandas `pipe` 方法：

1.  增强代码可读性

1.  实现函数链式调用

1.  适应自定义函数

1.  改善代码组织

1.  高效处理复杂转换

这里是 `pipe` 函数的代码示例。我们将 `clean` 和 `analysis` Python 函数应用于 Pandas DataFrame。管道方法将首先清理数据，然后执行数据分析，最后返回结果。

```py
(
    df.pipe(clean)
    .pipe(analysis)
)
```

## Pandas 无管道代码

首先，我们将编写一段不使用管道的简单数据分析代码，以便在使用管道简化数据处理管道时进行清晰的比较。

在本教程中，我们将使用来自 Kaggle 的 [在线销售数据集 - 流行市场数据](https://www.kaggle.com/datasets/shreyanshverma27/online-sales-dataset-popular-marketplace-data)，该数据集包含不同产品类别的在线销售交易信息。

1.  我们将加载 CSV 文件并显示数据集中的前三行。

```py
import pandas as pd
df = pd.read_csv('/work/Online Sales Data.csv')
df.head(3)
```

![使用 Pandas 构建数据科学管道](../Images/86b5024a87c7367ea47cec510f86b87f.png)

1.  通过删除重复项和缺失值来清理数据集，并重置索引。

1.  转换列类型。我们将“产品类别”和“产品名称”转换为字符串类型，并将“日期”列转换为日期类型。

1.  为了进行分析，我们将从“日期”列中创建一个“月份”列。然后，计算每个月销售单位的平均值。

1.  可视化每月平均销售单位的柱状图。

```py
# data cleaning
df = df.drop_duplicates()
df = df.dropna()
df = df.reset_index(drop=True)

# convert types
df['Product Category'] = df['Product Category'].astype('str')
df['Product Name'] = df['Product Name'].astype('str')
df['Date'] = pd.to_datetime(df['Date'])

# data analysis
df['month'] = df['Date'].dt.month
new_df = df.groupby('month')['Units Sold'].mean()

# data visualization
new_df.plot(kind='bar', figsize=(10, 5), title='Average Units Sold by Month');
```

![使用Pandas构建数据科学管道](../Images/ee4399b659b959816f206b0fb511ee1e.png)

这非常简单，如果你是数据科学家或数据科学学生，你将知道如何执行大多数这些任务。

## 使用Pandas Pipe构建数据科学管道

要创建一个端到端的数据科学管道，我们首先需要使用Python函数将上述代码转换为适当的格式。

我们将为以下内容创建Python函数：

1.  **加载数据：** 这需要一个CSV文件的目录。

1.  **数据清理：** 这需要原始DataFrame并返回清理后的DataFrame。

1.  **转换列类型：** 这需要一个干净的DataFrame和数据类型，并返回一个具有正确数据类型的DataFrame。

1.  **数据分析：** 这需要来自前一步的DataFrame，并返回包含两列的修改后的DataFrame。

1.  **数据可视化：** 这需要一个经过修改的DataFrame和可视化类型来生成可视化图表。

```py
def load_data(path):
    return pd.read_csv(path)

def data_cleaning(data):
    data = data.drop_duplicates()
    data = data.dropna()
    data = data.reset_index(drop=True)
    return data

def convert_dtypes(data, types_dict=None):
    data = data.astype(dtype=types_dict)
    ## convert the date column to datetime
    data['Date'] = pd.to_datetime(data['Date'])
    return data

def data_analysis(data):
    data['month'] = data['Date'].dt.month
    new_df = data.groupby('month')['Units Sold'].mean()
    return new_df

def data_visualization(new_df,vis_type='bar'):
    new_df.plot(kind=vis_type, figsize=(10, 5), title='Average Units Sold by Month')
    return new_df
```

我们现在将使用 `pipe` 方法将所有上述Python函数串联在一起。如我们所见，我们已将文件路径提供给 `load_data` 函数，将数据类型提供给 `convert_dtypes` 函数，并将可视化类型提供给 `data_visualization` 函数。我们将使用可视化折线图，而不是柱状图。

构建数据管道使我们能够在不改变整体代码的情况下尝试不同的场景。你在标准化代码并使其更具可读性。

```py
path = "/work/Online Sales Data.csv"
df = (pd.DataFrame()
            .pipe(lambda x: load_data(path))
            .pipe(data_cleaning)
            .pipe(convert_dtypes,{'Product Category': 'str', 'Product Name': 'str'})
            .pipe(data_analysis)
            .pipe(data_visualization,'line')
           )
```

最终结果看起来非常棒。

![使用Pandas构建数据科学管道](../Images/56adff4a5b7c2aa4cf43f8d3e64934db.png)

## 结论

在这个简短的教程中，我们了解了Pandas `pipe` 方法以及如何使用它来构建和执行端到端的数据科学管道。这个管道使你的代码更加可读、可重复和更有组织。通过将pipe方法集成到你的工作流程中，你可以简化数据处理任务，提升项目的整体效率。此外，一些用户发现使用 `pipe` 而不是 `.apply()` 方法可以显著提高执行速度。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个AI产品，帮助那些在精神健康方面挣扎的学生。

### 更多相关话题

+   [构建数据流水线以创建大型语言模型应用](https://www.kdnuggets.com/building-data-pipelines-to-create-apps-with-large-language-models)

+   [使用 HuggingFace 流水线和 Streamlit 回答问题](https://www.kdnuggets.com/2021/10/simple-question-answering-web-app-hugging-face-pipelines.html)

+   [超越流水线：图作为 Scikit-Learn 的元估计器](https://www.kdnuggets.com/2022/09/graphs-scikitlearn-metaestimators.html)

+   [使用 HuggingFace Transformers 的简单 NLP 流水线](https://www.kdnuggets.com/2023/02/simple-nlp-pipelines-huggingface-transformers.html)

+   [通过特征/训练/推理流水线统一批处理和机器学习系统](https://www.kdnuggets.com/2023/09/hopsworks-unify-batch-ml-systems-feature-training-inference-pipelines)

+   [使用 Scikit-learn 流水线简化你的机器学习工作流](https://www.kdnuggets.com/streamline-your-machine-learning-workflow-with-scikit-learn-pipelines)
