# 如何跟踪 Python 中的内存分配

> 原文：[https://www.kdnuggets.com/how-to-trace-memory-allocation-in-python](https://www.kdnuggets.com/how-to-trace-memory-allocation-in-python)

![如何跟踪 Python 中的内存分配](../Images/481bb88f07de623188f862389f04cc66.png)

图片由作者提供

在 Python 编码时，通常无需过多考虑内存分配的细节。但跟踪内存分配可能很有帮助，特别是当您处理内存密集型操作和大型数据集时。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

Python 的 [内置 tracemalloc 模块](https://docs.python.org/3/library/tracemalloc.html) 提供了一些功能，帮助您理解内存使用情况并调试应用程序。使用 tracemalloc，您可以获取内存分配的位置和数量，拍摄快照，比较快照之间的差异等。

我们将在本教程中查看一些这些内容。让我们开始吧。

## 开始之前

我们将使用一个简单的 Python 脚本进行数据处理。为此，我们将创建一个示例数据集并进行处理。除了最新版本的 Python，您还需要在工作环境中安装 pandas 和 NumPy。

创建一个虚拟环境并激活它：

```py
$ python3 -m venv v1
$ source v1/bin/activate
```

并安装所需的库：

```py
$ pip3 install numpy pandas
```

您可以在[GitHub](https://github.com/balapriyac/python-basics/tree/main/tracemalloc-tutorial)找到本教程的代码。

## 创建一个包含订单详情的示例数据集

我们将生成一个包含订单详情的示例 CSV 文件。您可以运行以下脚本来创建一个包含 100K 订单记录的 CSV 文件：

```py
# create_data.py
import pandas as pd
import numpy as np

# Create a sample dataset with order details
num_orders = 100000
data = {
	'OrderID': np.arange(1, num_orders + 1),
	'CustomerID': np.random.randint(1000, 5000, num_orders),
	'OrderAmount': np.random.uniform(10.0, 1000.0, num_orders).round(2),
	'OrderDate': pd.date_range(start='2023-01-01', periods=num_orders, freq='min')
}

df = pd.DataFrame(data)
df.to_csv('order_data.csv', index=False)
```

此脚本将 100K 条记录填充到 pandas 数据框中，具有以下四个特征，并将数据框导出为 CSV 文件：

+   **OrderID**: 每个订单的唯一标识符

+   **CustomerID**: 客户的 ID

+   **OrderAmount**: 每个订单的金额

+   **OrderDate**: 订单的日期和时间

## 使用 tracemalloc 跟踪内存分配

现在我们将创建一个 Python 脚本来加载和处理数据集。我们还将跟踪内存分配。

首先，我们定义 `load_data` 和 `process_data` 函数来加载和处理来自 CSV 文件的记录：

```py
# main.py
import pandas as pd

def load_data(file_path):
    print("Loading data...")
    df = pd.read_csv(file_path)
    return df

def process_data(df):
    print("Processing data...")
    df['DiscountedAmount'] = df['OrderAmount'] * 0.9  # Apply a 10% discount
    df['OrderYear'] = pd.to_datetime(df['OrderDate']).dt.year  # Extract the order year
    return df
```

然后我们可以通过以下步骤继续进行内存分配跟踪：

+   使用`tracemalloc.start()`初始化内存跟踪。

+   `load_data()` 函数将 CSV 文件读入数据框。我们在此步骤之后拍摄内存使用快照。

+   `process_data()` 函数向数据框添加了两个新列：'DiscountedAmount' 和 'OrderYear'。处理后我们再拍摄一个快照。

+   我们比较这两个快照以找出内存使用差异，并打印出最耗内存的行。

+   然后打印当前和峰值内存使用情况，以了解整体影响。

这是对应的代码：

```py
import tracemalloc

def main():
    # Start tracing memory allocations
    tracemalloc.start()

    # Load data
    df = load_data('order_data.csv')

    # Take a snapshot
    snapshot1 = tracemalloc.take_snapshot()

    # Process data
    df = process_data(df)

    # Take another snapshot
    snapshot2 = tracemalloc.take_snapshot()

    # Compare snapshots
    top_stats = snapshot2.compare_to(snapshot1, 'lineno')

    print("[ Top memory-consuming lines ]")
    for stat in top_stats[:10]:
        print(stat)

    # Current and peak memory usage
    current, peak = tracemalloc.get_traced_memory()
    print(f"Current memory usage: {current / 1024 / 1024:.1f} MB")
    print(f"Peak usage: {peak / 1024 / 1024:.1f} MB")

    tracemalloc.stop()

if __name__ == "__main__":
    main()
```

现在运行 Python 脚本：

```py
$ python3 main.py
```

这将输出最耗内存的行以及当前和峰值内存使用情况：

```py
Loading data...
Processing data...
[ Top 3 memory-consuming lines ]
/home/balapriya/trace_malloc/v1/lib/python3.11/site-packages/pandas/core/frame.py:12683: size=1172 KiB (+1172 KiB), count=4 (+4), average=293 KiB
/home/balapriya/trace_malloc/v1/lib/python3.11/site-packages/pandas/core/arrays/datetimelike.py:2354: size=781 KiB (+781 KiB), count=3 (+3), average=260 KiB
<frozen abc="">:123: size=34.6 KiB (+15.3 KiB), count=399 (+180), average=89 B
Current memory usage: 10.8 MB
Peak usage: 13.6 MB</frozen>
```

## 总结

使用 tracemalloc 跟踪内存分配有助于识别内存密集型操作，并通过内存跟踪和返回的统计信息潜在地优化性能。

你应该能看到是否可以使用更高效的数据结构和处理方法来减少内存使用。对于长时间运行的应用程序，你可以定期使用 tracemalloc 来跟踪内存使用情况。也就是说，你可以将 tracemalloc 与其他分析工具结合使用，以全面了解内存使用情况。

如果你有兴趣学习使用 memory-profiler 进行内存分析，请阅读 [Python 中的内存分析简介](https://www.kdnuggets.com/introduction-to-memory-profiling-in-python)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发人员和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和咖啡！目前，她正在通过编写教程、操作指南、观点文章等与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关主题

+   [Python 中的内存分析简介](https://www.kdnuggets.com/introduction-to-memory-profiling-in-python)

+   [如何在 Pandas 中对大型数据集执行内存高效操作](https://www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas)

+   [变压器的内存复杂性](https://www.kdnuggets.com/2022/12/memory-complexity-transformers.html)

+   [优化 Python 代码性能：深入探讨 Python 性能分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python Enum：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [通过快速 Python 提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)
