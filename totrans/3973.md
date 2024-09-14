# NumPy 中处理缺失数据的掩码数组

> 原文：[https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)

![NumPy 中处理缺失数据的掩码数组](../Images/3e6cefa0a51bdb948693bd090ff75e54.png)

作者提供的图像

想象一下试图解决一个缺少拼图块的谜题。这会很令人沮丧，对吧？这是处理不完整数据集时的常见情境。NumPy 中的掩码数组是专门的数组结构，允许你高效地处理缺失或无效的数据。在必须对包含不可靠条目的数据集进行计算的情境下，它们特别有用。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供 IT 支持

* * *

掩码数组本质上是两个数组的组合：

+   **数据数组**：包含实际数据值的主要数组。

+   **掩码数组**：一个与数据数组形状相同的布尔数组，其中每个元素指示对应的数据元素是有效还是被掩码（无效/缺失）。

## 数据数组

数据数组是掩码数组的核心组件，保存了你想分析或处理的实际数据值。这个数组可以包含任何数值或分类数据，就像标准的 NumPy 数组一样。以下是一些重要的考虑点：

+   **存储**：数据数组存储了你需要处理的值，包括有效和无效的条目（如 `NaN` 或代表缺失数据的特定值）。

+   **操作**：在执行操作时，NumPy 使用数据数组来计算结果，但会考虑掩码数组以确定包含或排除哪些元素。

+   **兼容性**：掩码数组中的数据数组支持所有标准的 NumPy 功能，使得在常规数组和掩码数组之间切换变得容易，而不会显著改变现有代码库。

**示例**：

```py
import numpy as np

data = np.array([1.0, 2.0, np.nan, 4.0, 5.0])
masked_array = np.ma.array(data)
print(masked_array.data)  # Output: [ 1\.  2\. nan  4\.  5.] 
```

## 掩码数组

掩码数组是一个与数据数组形状相同的布尔数组。掩码数组中的每个元素对应于数据数组中的一个元素，并指示该元素是有效（**False**）还是被掩码（**True**）。以下是一些详细的点：

+   **结构**：掩码数组的创建形状与数据数组相同，以确保每个数据点都有一个对应的掩码值。

+   **标识无效数据**：掩码数组中的 **True** 值将相应的数据点标记为无效或缺失，而 **False** 值则表示有效数据。这允许 NumPy 在计算过程中忽略或排除无效数据点。

+   **自动掩码**：NumPy 提供了根据特定条件自动创建掩码数组的函数（例如，`np.ma.masked_invalid()` 用于掩盖 **NaN** 值）。

**示例**：

```py
import numpy as np

data = np.array([1.0, 2.0, np.nan, 4.0, 5.0])
mask = np.isnan(data)  # Create a mask where NaN values are True
masked_array = np.ma.array(data, mask=mask)
print(masked_array.mask)  # Output: [False False  True False False] 
```

掩码数组的强大之处在于数据与掩码数组之间的关系。当你对掩码数组执行操作时，NumPy 会考虑这两个数组，确保计算仅基于有效数据。

## 掩码数组的好处

NumPy 中的掩码数组提供了多种优势，尤其是在处理包含缺失或无效数据的数据集时，其中包括：

1.  **缺失数据的高效处理**：掩码数组允许你轻松标记无效或缺失的数据，例如 NaN，并在计算中自动处理它们。操作仅在有效数据上执行，确保缺失或无效的条目不会扭曲结果。

1.  **简化数据清理**：像 `numpy.ma.masked_invalid()` 这样的函数可以自动掩盖常见的无效值（例如 NaN 或无穷大），而无需额外的代码来手动识别和处理这些值。你可以根据特定标准定义自定义掩码，允许灵活的数据清理策略。

1.  **与 NumPy 函数的无缝集成**：掩码数组与大多数标准 NumPy 函数和操作兼容。这意味着你可以使用熟悉的 NumPy 方法，而无需手动排除或预处理掩码值。

1.  **提高计算准确性**：在执行计算（例如，均值、总和、标准差）时，掩码值会自动从计算中排除，从而得到更准确和有意义的结果。

1.  **增强数据可视化**：在可视化数据时，掩码数组确保无效或缺失值不会被绘制，从而产生更清晰和准确的视觉表示。你可以仅绘制有效数据，避免杂乱，提高图表的可解释性。

## 使用掩码数组处理 NumPy 中的缺失数据

本节将演示如何使用掩码数组处理 NumPy 中的缺失数据。首先，让我们看一个简单的示例：

```py
import numpy as np

# Data with some missing values represented by -999
data = np.array([10, 20, -999, 30, -999, 40])

# Create a mask where -999 is considered as missing data
mask = (data == -999)

# Create a masked array using the data and mask
masked_array = np.ma.array(data, mask=mask)

# Calculate the mean, ignoring masked values
mean_value = masked_array.mean()
print(mean_value) 
```

**输出**：

25.0

**说明**：

+   **数据创建**：`data` 是一个整数数组，其中 **-999** 代表缺失值。

+   **掩码创建**：`mask` 是一个布尔数组，将 **-999** 位置标记为 **True**（表示缺失数据）。

+   **掩码数组创建**：`np.ma.array(data, mask=mask)` 创建一个掩码数组，将掩码应用于 `data`。

+   **计算**：`masked_array.mean()`。

计算平均值时忽略掩码值（即**-999**），结果是剩余有效值的平均值。

在此示例中，平均值仅从 **[10, 20, 30, 40]** 计算，不包括 **-999** 值。

让我们通过一个更全面的示例来探索使用掩码数组处理较大数据集中缺失数据的情况。我们将使用涉及多个传感器跨多个日期的温度读数数据集的场景。数据集中包含一些由于传感器故障而缺失的值。

#### 用例：分析来自多个传感器的温度数据

**场景**：你有来自五个传感器的十天温度读数。由于传感器问题，一些读数缺失。我们需要计算平均每日温度，同时忽略缺失数据。

**数据集**：数据集表示为二维NumPy数组，行表示日期，列表示传感器。缺失值由`np.nan`表示。

步骤：

1.  **导入NumPy**：用于数组操作和处理掩码数组。

1.  **定义数据**：创建一个包含一些缺失值的二维温度读数数组。

1.  **创建掩码**：识别数据集中的缺失值（NaNs）。

1.  **创建掩码数组**：应用掩码以处理缺失值。

1.  计算每日平均温度：计算每一天的平均温度，忽略缺失值。

1.  **输出结果**：显示分析结果。

**代码**：

```py
import numpy as np

# Example temperature readings from 5 sensors over 10 days
# Rows: days, Columns: sensors
temperature_data = np.array([
    [22.1, 21.5, np.nan, 23.0, 22.8],  # Day 1
    [20.3, np.nan, 22.0, 21.8, 23.1],  # Day 2
    [np.nan, 23.2, 21.7, 22.5, 22.0],  # Day 3
    [21.8, 22.0, np.nan, 21.5, np.nan],  # Day 4
    [22.5, 22.1, 21.9, 22.8, 23.0],  # Day 5
    [np.nan, 21.5, 22.0, np.nan, 22.7],  # Day 6
    [22.0, 22.5, 23.0, np.nan, 22.9],  # Day 7
    [21.7, np.nan, 22.3, 22.1, 21.8],  # Day 8
    [22.4, 21.9, np.nan, 22.6, 22.2],  # Day 9
    [23.0, 22.5, 21.8, np.nan, 22.0]   # Day 10
])

# Create a mask for missing values (NaNs)
mask = np.isnan(temperature_data)

# Create a masked array
masked_data = np.ma.masked_array(temperature_data, mask=mask)

# Calculate the average temperature for each day, ignoring missing values
daily_averages = masked_data.mean(axis=1)  # Axis 1 represents days

# Print the results
for day, avg_temp in enumerate(daily_averages, start=1):
    print(f"Day {day}: Average Temperature = {avg_temp:.2f} °C") 
```

**输出：**

![掩码数组示例-III](../Images/2f82b19dc3d74d02be449c6b5010d167.png)

**解释**：

+   **导入NumPy**：导入**NumPy**库以利用其功能。

+   **定义数据**：创建一个二维数组`temperature_data`，其中每行代表特定日期传感器的温度，且有些值缺失（`np.nan`）。

+   **创建掩码**：使用`np.isnan(temperature_data)`生成布尔掩码，以识别缺失值（**True**表示值为`np.nan`）。

+   **创建掩码数组**：使用`np.ma.masked_array(temperature_data, mask=mask)`创建`masked_data`。这个数组屏蔽掉缺失值，使操作忽略它们。

+   **计算每日平均温度**：使用`.mean(axis=1)`计算每天的平均温度。这里，`axis=1`表示在每一天的传感器之间计算均值。

+   **输出结果**：打印每天的平均温度。掩码值被排除在计算之外，提供准确的每日平均值。

## 结论

在这篇文章中，我们探讨了掩码数组的概念以及如何利用它们处理缺失数据。我们讨论了掩码数组的两个关键组成部分：数据数组，它包含实际值，和掩码数组，它指示哪些值是有效的或缺失的。我们还考察了它们的好处，包括高效处理缺失数据、与NumPy函数的无缝集成以及提高计算准确性。

我们通过简单和复杂的示例演示了掩码数组的使用。初始示例展示了如何处理由特定标记如 **-999** 表示的缺失值，而更全面的示例则展示了如何分析来自多个传感器的温度数据，其中缺失值由 `np.nan` 表示。这两个示例都突出了掩码数组通过忽略无效数据来准确计算结果的能力。

进一步阅读请查看以下两个资源：

+   [numpy.ma模块](https://numpy.org/doc/stable/reference/maskedarray.generic.html)

+   [掩码数组](https://numpy.org/doc/1.21/user/tutorial-ma.html)

[](https://www.linkedin.com/in/olumide-shittu)****[Shittu Olumide](https://www.linkedin.com/in/olumide-shittu/)**** 是一位软件工程师和技术写作者，热衷于利用前沿技术来编写引人入胜的叙述，具有敏锐的细节观察力和简化复杂概念的天赋。你也可以在 [Twitter](https://twitter.com/Shittu_Olumide_) 上找到Shittu。

### 更多相关话题

+   [如何使用Scikit-learn的Imputer模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)

+   [如何使用NumPy对数组进行填充](https://www.kdnuggets.com/how-to-apply-padding-to-arrays-with-numpy)

+   [如何计算两个NumPy数组之间的互相关](https://www.kdnuggets.com/how-to-compute-the-cross-correlation-between-two-numpy-arrays)

+   [KDnuggets 新闻，8月31日：完整的数据科学学习路线图…](https://www.kdnuggets.com/2022/n35.html)

+   [处理不平衡数据的7种技术](https://www.kdnuggets.com/2017/06/7-techniques-handle-imbalanced-data.html)

+   [如何使用Pandas处理数据集中的异常值](https://www.kdnuggets.com/how-to-handle-outliers-in-dataset-with-pandas)
