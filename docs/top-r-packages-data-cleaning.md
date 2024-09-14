# 数据清理的顶级 R 包

> 原文：[`www.kdnuggets.com/2019/03/top-r-packages-data-cleaning.html`](https://www.kdnuggets.com/2019/03/top-r-packages-data-cleaning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Anna Kayfitz， [StrategicDB Corp](https://strategicdb.com)的首席执行官**

![数据清理](img/6e43452bbd82d735a0d17ed0a8e59259.png)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

随着每天有数百万或数十亿的数据元素进入你的业务，几乎不可避免地会有一些数据缺乏创建高效商业模型所需的质量。确保你的数据干净应该始终是数据科学工作流的第一步，也是最重要的一步，因为没有它，你将很难看到重要的信息，并可能因为重复、异常或缺失信息而做出错误的决策。

R 是最常用且功能强大的数据编程工具之一，它是一个用于统计计算和图形绘制的开源语言和环境。R 为用户提供了创建数据科学项目所需的所有工具，但无论如何，它的效果取决于输入的数据。因此，R 环境中有许多库可以帮助在项目开始之前进行数据清理和处理。

**探索数据**

你导入的数据集中的大多数探索工具已经存在于 R 平台中。

**总结（数据）**

这个实用的命令简要概述了所有的数据属性，显示每个属性的最小值、最大值、中位数、均值和类别分布。这是一种快速发现潜在数据异常的好方法。

紧接着，你可以使用直方图来更好地理解数据的分布。这将可视化显示数据集中或任何你特别希望观察的数值列中的异常值。

**plyr 包**

你需要安装 plyr 包来创建你的直方图，使用标准的 R 功能来安装库。

```py

Install.packages(“plyr”)

Library(plyr)

Hist(YOUR_DATASET_NAME)

```

这将创建一个数据的可视化图，以便快速发现任何异常。箱线图可视化使用相同的包，但将数据分为四分位数以进行异常值检测。这两者结合将迅速告诉你是否需要限制数据集或仅在算法或统计建模中使用某些数据段。

**纠正错误**

R 提供了一些内置方法来纠正数据错误，例如转换值，就像你在 Excel 或 SQL 中使用简单逻辑一样，例如 `as.character()` 将列转换为字符字符串。

然而，如果你想开始纠正你在直方图或箱线图中看到的错误，还有其他可以做到这一点的附加包。

**stringr 包**

`stringr` 包可以通过修剪空白和替换某些不必要的词来帮助清理数据。这些是相当标准的代码片段，如 `str_trim(YOUR_DATA_FIELD)`，它简单地移除空白。

然而，如何去除我们直方图告诉我们存在的异常值呢？这需要比这更复杂一点，但作为基本示例，我们可以告诉 R 用该字段的中位数替换所有的异常值。这将把所有数据聚合在一起，消除异常偏差。

**缺失值**

在 R 中检查不完整数据并对该字段执行操作非常简单。例如，这个函数将完全消除你选择的数据列中的缺失值。

```py

Na.omit(YOUR_DATA_COLUMN)

```

根据字段类型，有类似的选项可以用 0 或 N/A 替换空值，从而提高数据集的一致性。

**tidyr 包**

`tidyr` 包旨在整理你的数据。它通过识别数据集中的变量并使用提供的工具将它们移动到列中，有三个主要函数：`gather()`、`separate()` 和 `spread()`。

`gather()` 函数将多个列收集成键值对。例如，假设你有类似的考试分数数据。

| Name | Exam A | Exam B |
| --- | --- | --- |
| John | 55 | 80 |
| Mike | 76 | 90 |
| Sam | 45 | 75 |

`gather` 函数通过将数据转换成像这样可用的列来工作。

| Name | Exam | Score |
| --- | --- | --- |
| John | A | 55 |
| Mike | A | 76 |
| Sam | A | 45 |
| John | B | 80 |
| Mike | B | 90 |
| Sam | B | 75 |

现在我们真正能够分析考试分数了。`separate` 和 `spread` 函数执行类似的操作，你可以在安装了包后探索它们，但最终它们会根据需要调整你的数据。

**以下是一些其他可能对 R 中数据清理有用的包**

+   **purr 包**

`purr` 包旨在进行数据处理。它与 `plyr` 包非常相似，尽管较旧，一些用户发现它更易于使用，并且在功能上更为标准化。

+   **sqldf 包**

许多 R 用户在 SQL 语言中编写代码更为舒适，而不是 R。这个函数允许你在 R studio 中编写 SQL 代码来选择数据元素。

+   **janitor 包**

该包能够通过多个列找到重复项，并轻松从数据框中创建友好的列。它甚至具有一个 get_dupes()函数，用于在多个数据行中查找重复值。如果你希望以更高级的方式去重数据，例如，查找不同的组合或使用模糊逻辑，你可能需要查看一个[去重工具](https://strategicdb.com/data-cleansing-services/deduping-tool/)。

+   **splitstackshape 包**

这是一个较旧的包，可以处理数据框列中的逗号分隔值。对于调查或文本分析准备非常有用。

R 有大量的包，这篇文章仅触及了它的表面。随着新库不断出现，在开始任何新项目之前，进行研究并选择适合自己的包非常重要。

**简介**：Anna Kayfitz 是[StrategicDB Corp](https://strategicdb.com)的首席执行官，该公司专注于数据清理和分析。她拥有 Schulich 商学院的 MBA 学位，并在创办 StrategicDB 之前在数据分析和营销领域工作了 10 多年。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [用于分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [不要在孤立中进行分析](https://www.kdnuggets.com/2019/02/mode-dont-do-analysis-vacuum.html)

+   [在 Jupyter 中运行 R 和 Python](https://www.kdnuggets.com/2019/02/running-r-and-python-in-jupyter.html)

+   [2018 年数据科学和人工智能的 7 个顶级 R 包](https://www.kdnuggets.com/2019/01/vazquez-2018-top-7-r-packages.html)

### 更多相关话题

+   [2023 年需要了解的顶级数据 Python 包](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

+   [3 个用于数据可视化的 Julia 包](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [SQL、Python、数据清理、数据整理和探索性数据分析的指南集合](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据科学中数据清理的重要性](https://www.kdnuggets.com/2023/08/importance-data-cleaning-data-science.html)

+   [通过这本免费电子书学习数据清理和预处理](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)
