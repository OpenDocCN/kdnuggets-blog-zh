# 使用 NumPy 执行日期和时间计算

> 原文：[`www.kdnuggets.com/using-numpy-to-perform-date-and-time-calculations`](https://www.kdnuggets.com/using-numpy-to-perform-date-and-time-calculations)

![NumPy 执行日期和时间计算](img/96bedcbc849f2fd2fcdf9c38ebe004de.png)

作者图片 | Canva

日期和时间是无数数据分析任务的核心，从追踪金融交易到实时监测传感器数据。然而，处理日期和时间的计算常常让人感觉像是在迷宫中探索。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

幸运的是，有了 NumPy，我们很幸运。NumPy 强大的日期和时间功能让这些任务变得轻松，提供了一系列简化过程的方法。

例如，NumPy 允许你轻松创建日期数组，对日期和时间执行算术运算，并且只需几行代码即可在不同的时间单位之间转换。你需要找出两个日期之间的差异吗？NumPy 可以轻松做到。你想将你的时间序列数据重新采样到不同的频率吗？NumPy 能满足你的需求。这种便利和强大使得 NumPy 成为处理日期和时间计算的宝贵工具，将曾经复杂的挑战转变为简单的任务。

本文将指导你使用 NumPy 执行日期和时间计算。我们将介绍**datetime**是什么以及如何表示，日期和时间通常用于何处，使用它时的常见困难和问题，以及最佳实践。

## 什么是 DateTime

DateTime 指的是以统一格式表示日期和时间。这包括特定的日历日期和时间，通常精确到秒的分数。这种组合对于准确记录和管理时间数据，如日志中的时间戳、事件调度和进行基于时间的分析，非常重要。

在一般编程和数据分析中，**DateTime** 通常由专门的数据类型或对象表示，这些对象提供了一种结构化的方式来处理日期和时间。这些对象允许对日期和时间进行轻松的操作、比较和算术运算。

NumPy 和其他库如 pandas 提供了对**DateTime**操作的强大支持，使得处理各种格式的时间数据以及执行复杂的计算变得轻松而精准。

在 NumPy 中，日期和时间的处理主要围绕 `datetime64` 数据类型及其相关函数。你可能会想知道为什么这个数据类型叫做 [datetime64](https://numpy.org/doc/stable/reference/arrays.scalars.html#numpy.datetime64)。这是因为 [datetime](https://docs.python.org/3/library/datetime.html#datetime.datetime) 已经被 Python 标准库使用了。

以下是它的工作原理：

**datetime64 数据类型**

+   **表示**：NumPy 的 `datetime64` 数据类型将日期和时间表示为 64 位整数，提供高效的时间数据存储和操作。

+   **格式**：`datetime64` 格式中的日期和时间由一个字符串指定，指示所需的精度，例如，`YYYY-MM-DD` 表示日期或 `YYYY-MM-DD HH:mm:ss` 表示到秒的时间戳。

例如：

```py
import numpy as np

# Creating a datetime64 array
dates = np.array(['2024-07-15', '2024-07-16', '2024-07-17'], dtype='datetime64')

# Performing arithmetic operations
next_day = dates + np.timedelta64(1, 'D')

print("Original Dates:", dates)
print("Next Day:", next_day) 
```

#### `datetime64`在 NumPy 中的特点

NumPy 的 `datetime64` 提供了强大的功能来简化多种操作。从灵活的分辨率处理到强大的算术能力，`datetime64` 使得处理时间数据变得简单高效。

1.  **分辨率灵活性**：`datetime64` 支持从纳秒到年的各种分辨率。例如，**ns**（纳秒），**us**（微秒），**ms**（毫秒），**s**（秒），**m**（分钟），**h**（小时），**D**（天），**W**（周），**M**（月），**Y**（年）。

```py
np.datetime64('2024-07-15T12:00', 'm')  # Minute resolution
np.datetime64('2024-07-15', 'D')        # Day resolution 
```

1.  **算术操作**：对 `datetime64` 对象执行直接的算术运算，例如，添加或减去时间单位，比如为日期添加天数。

```py
date = np.datetime64('2024-07-15')
next_week = date + np.timedelta64(7, 'D') 
```

1.  **索引和切片**：在 `datetime64` 数组上使用标准的 NumPy 索引和切片技术。例如，提取日期范围。

```py
dates = np.array(['2024-07-15', '2024-07-16', '2024-07-17'], dtype='datetime64')
subset = dates[1:3] 
```

1.  **比较操作**：比较 `datetime64` 对象以确定时间顺序。例如：检查一个日期是否在另一个日期之前。

```py
date1 = np.datetime64('2024-07-15')
date2 = np.datetime64('2024-07-16')
is_before = date1 < date2  # True 
```

1.  **转换函数**：在 `datetime64` 和其他日期/时间表示形式之间进行转换。例如：将 `datetime64` 对象转换为字符串。

```py
date = np.datetime64('2024-07-15')
date_str = date.astype('str') 
```

## 你倾向于在哪里使用日期和时间？

日期和时间可以在多个领域中使用，例如金融领域，用于跟踪股票价格、分析市场趋势、评估财务表现、计算回报、评估波动性以及识别时间序列数据中的模式。

你还可以在其他领域使用日期和时间，例如在医疗保健领域，管理带有时间戳的患者记录，用于医疗历史、治疗和药物计划。

#### 场景：分析电子商务销售数据

想象一下你是一名在电子商务公司工作的数据分析师。你有一个包含带有时间戳的销售交易的数据集，你需要分析过去一年的销售模式。以下是你如何利用 NumPy 中的 `datetime64`：

```py
# Loading and Converting Data
import numpy as np
import matplotlib.pyplot as plt

# Sample data: timestamps of sales transactions
sales_data = np.array(['2023-07-01T12:34:56', '2023-07-02T15:45:30', '2023-07-03T09:12:10'], dtype='datetime64')

# Extracting Specific Time Periods
# Extracting sales data for July 2023
july_sales = sales_data[(sales_data >= np.datetime64('2023-07-01')) & (sales_data < np.datetime64('2023-08-01'))]

# Calculating Daily Sales Counts
# Converting timestamps to dates
sales_dates = july_sales.astype('datetime64[D]')

# Counting sales per day
unique_dates, sales_counts = np.unique(sales_dates, return_counts=True)

# Analyzing Sales Trends
plt.plot(unique_dates, sales_counts, marker='o')
plt.xlabel('Date')
plt.ylabel('Number of Sales')
plt.title('Daily Sales Counts for July 2023')
plt.xticks(rotation=45)  # Rotates x-axis labels for better readability
plt.tight_layout()  # Adjusts layout to prevent clipping of labels
plt.show() 
```

在这种情况下，`datetime64` 允许你轻松操作和分析销售数据，提供对每日销售模式的洞察。

## 使用日期和时间时的常见困难

尽管 NumPy 的 `datetime64` 是处理日期和时间的强大工具，但它也有其挑战。从解析各种日期格式到管理时区，开发人员通常会遇到几个可能复杂化数据分析任务的障碍。本节突出了这些典型问题的一些例子。

1.  **解析和转换格式**：处理各种日期和时间格式可能很具挑战性，特别是当处理来自多个来源的数据时。

1.  **时区处理**：NumPy 的 `datetime64` 本身不支持时区。

1.  **分辨率不匹配**：数据集的不同部分可能具有不同分辨率的时间戳（例如，有些以天为单位，有些以秒为单位）。

## 如何进行日期和时间计算

让我们探讨 NumPy 中日期和时间计算的示例，从基本操作到更高级的场景，以帮助你充分利用 `datetime64` 来满足数据分析需求。

#### 向日期添加天数

这里的目标是演示如何将特定天数（在此情况下为**5**天）添加到给定日期（**2024-07-15**）

```py
import numpy as np

# Define a date
start_date = np.datetime64('2024-07-15')

# Add 5 days to the date
end_date = start_date + np.timedelta64(5, 'D')

print("Start Date:", start_date)
print("End Date after adding 5 days:", end_date) 
```

**输出：**

开始日期：2024-07-15

添加 5 天后的结束日期：2024-07-20

**解释**：

+   我们使用 `np.datetime64` 定义 `start_date`。

+   使用 `np.timedelta64`，我们将**5**天（**5**，**D**）添加到 `start_date` 以获得 `end_date`。

+   最后，我们打印 `start_date` 和 `end_date` 以观察加法的结果。

#### 计算两个日期之间的时间差

计算两个特定日期（**2024-07-15T12:00** 和 **2024-07-17T10:30**）之间的时间差（小时）

```py
import numpy as np

# Define two dates
date1 = np.datetime64('2024-07-15T12:00')
date2 = np.datetime64('2024-07-17T10:30')

# Calculate the time difference in hours
time_diff = (date2 - date1) / np.timedelta64(1, 'h')

print("Date 1:", date1)
print("Date 2:", date2)
print("Time difference in hours:", time_diff) 
```

**输出：**

日期 1：2024-07-15T12:00

日期 2：2024-07-17T10:30

时间差（小时）：46.5

**解释**：

+   使用 `np.datetime64` 定义 `date1` 和 `date2`，并指定特定的时间戳。

+   通过从 `date1` 中减去 `date2` 并除以 `np.timedelta64(1, 'h')` 来计算 `time_diff`，将差值转换为小时。

+   打印原始日期和计算出的时间差（小时）。

#### 处理时区和工作日

计算两个日期之间的工作日数，排除周末和假期。

```py
import numpy as np
import pandas as pd

# Define two dates
start_date = np.datetime64('2024-07-01')
end_date = np.datetime64('2024-07-15')

# Convert to pandas Timestamp for more complex calculations
start_date_ts = pd.Timestamp(start_date)
end_date_ts = pd.Timestamp(end_date)

# Calculate the number of business days between the two dates
business_days = pd.bdate_range(start=start_date_ts, end=end_date_ts).size

print("Start Date:", start_date)
print("End Date:", end_date)
print("Number of Business Days:", business_days) 
```

**输出：**

开始日期：2024-07-01

结束日期：2024-07-15

工作日数：11

**解释**：

+   **NumPy 和 Pandas 导入**：NumPy 被导入为 `np`，Pandas 被导入为 `pd`，以利用它们的日期和时间处理功能。

+   **日期定义**：使用 NumPy 的 `np.datetime64` 定义 `start_date` 和 `end_date` 来指定开始和结束日期（分别为 '**2024-07-01**' 和 '**2024-07-15**'）。

+   **转换为 pandas 时间戳**：此转换将 `start_date` 和 `end_date` 从 `np.datetime64` 转换为 pandas **时间戳** 对象（`start_date_ts` 和 `end_date_ts`），以兼容 pandas 更高级的日期处理功能。

+   **工作日计算**：使用`pd.bdate_range`生成`start_date_ts`和`end_date_ts`之间的工作日期范围（排除周末）。计算该工作日期范围的大小（元素数量）（`business_days`），表示两日期之间的工作日数量。

+   打印原始的`start_date`和`end_date`。

+   显示指定日期之间计算的工作日数量（`business_days`）。

## 使用 `datetime64` 的最佳实践

在 NumPy 中处理日期和时间数据时，遵循最佳实践可以确保你的分析准确、高效且可靠。正确处理`datetime64`可以防止常见问题并优化数据处理工作流程。以下是一些关键的最佳实践：

1.  在处理之前确保所有日期和时间数据都采用一致的格式。这有助于避免解析错误和不一致。

1.  选择与你的数据需求匹配的分辨率（‘**D**’，‘**h**’，‘**m**’等）。避免混合不同的分辨率，以防计算不准确。

1.  使用 `datetime64` 表示缺失或无效日期，并在分析前预处理数据以解决这些值。

1.  如果你的数据包含多个时区，请在处理工作流的早期阶段将所有时间戳标准化为一个通用时区。

1.  检查你的日期是否在`datetime64`的有效范围内，以避免溢出错误和意外结果。

## 结论

总结来说，NumPy 的 `datetime64` 数据类型提供了一个强大的框架，用于在数值计算中管理日期和时间数据。它为各种应用提供了灵活性和计算效率，例如数据分析、模拟等。

我们探讨了如何使用 NumPy 进行日期和时间计算，深入了解了核心概念及其使用 `datetime64` 数据类型的表现。我们讨论了日期和时间在数据分析中的常见应用。同时，我们还审视了在 NumPy 中处理日期和时间数据的常见困难，例如格式不一致、时区问题和分辨率不匹配。

遵循这些最佳实践可以确保你对`datetime64`的使用既准确又高效，从而为你的数据提供更可靠和有意义的见解。

[](https://www.linkedin.com/in/olumide-shittu)****[Shittu Olumide](https://www.linkedin.com/in/olumide-shittu/)**** 是一名软件工程师和技术作家，热衷于利用前沿技术来撰写引人入胜的叙述，对细节有敏锐的洞察力，并擅长简化复杂的概念。你还可以在[Twitter](https://twitter.com/Shittu_Olumide_)上找到 Shittu。

### 更多相关内容

+   [Tick-Tock: 使用 Pendulum 进行 Python 中简易日期和时间管理](https://www.kdnuggets.com/tick-tock-using-pendulum-for-easy-date-and-time-management-in-python)

+   [如何使用 NumPy 执行矩阵运算](https://www.kdnuggets.com/how-to-perform-matrix-operations-with-numpy)

+   [如何在 Python 中工程化日期特征](https://www.kdnuggets.com/2021/08/engineer-date-features-python.html)

+   [5 个项目想法帮助数据科学家保持最新](https://www.kdnuggets.com/2022/07/5-project-ideas-stay-uptodate-data-scientist.html)

+   [如何使用 Python 执行运动检测](https://www.kdnuggets.com/2022/08/perform-motion-detection-python.html)

+   [KDnuggets 新闻，8 月 17 日：如何执行运动检测…](https://www.kdnuggets.com/2022/n33.html)
