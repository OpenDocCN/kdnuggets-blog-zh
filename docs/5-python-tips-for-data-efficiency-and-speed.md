# 提升数据效率和速度的 5 个 Python 小技巧

> 原文：[`www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed`](https://www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed)

![f-img](img/b9aace080bf31cb205834a05d2f462c2.png)

图片由作者提供

[编写高效的 Python 代码](https://www.kdnuggets.com/how-to-write-efficient-python-code-a-tutorial-for-beginners) 对于优化性能和资源使用非常重要，无论你是在进行数据科学项目、构建 web 应用程序，还是在进行其他编程任务。

* * *

## 我们的 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

利用 Python 强大的功能和最佳实践，你可以减少计算时间，提高应用程序的响应速度和可维护性。

在本教程中，我们将探讨五个重要的技巧，通过每个例子的编码来帮助你编写更高效的 Python 代码。让我们开始吧。

## 1\. 使用列表推导式代替循环

你可以使用列表推导式从现有列表和其他可迭代对象（如字符串和元组）中创建列表。它们通常比普通循环在列表操作中更简洁和更快。

假设我们有一个用户信息的数据集，我们想提取那些分数大于 85 的用户的姓名。

### 使用循环

首先，让我们使用 for 循环和 if 语句来完成这个任务：

```py
data = [{'name': 'Alice', 'age': 25, 'score': 90},
    	{'name': 'Bob', 'age': 30, 'score': 85},
    	{'name': 'Charlie', 'age': 22, 'score': 95}]

# Using a loop
result = []
for row in data:
    if row['score'] > 85:
        result.append(row['name'])

print(result)
```

你应该得到以下输出：

```py
Output  >>> ['Alice', 'Charlie']
```

### 使用列表推导式

现在，让我们使用列表推导式重新编写。你可以使用通用语法 `[output for input in iterable if condition]` 来实现：

```py
data = [{'name': 'Alice', 'age': 25, 'score': 90},
    	{'name': 'Bob', 'age': 30, 'score': 85},
    	{'name': 'Charlie', 'age': 22, 'score': 95}]

# Using a list comprehension
result = [row['name'] for row in data if row['score'] > 85]

print(result)
```

这应该会给你相同的输出：

```py
Output >>> ['Alice', 'Charlie']
```

如所见，列表推导式版本更简洁，更易于维护。你可以尝试其他示例并 [对你的代码进行性能分析](https://www.kdnuggets.com/profiling-python-code-using-timeit-and-cprofile) 使用 timeit 比较循环和列表推导式的执行时间。

因此，列表推导式让你编写更具可读性和高效的 Python 代码，特别是在转换列表和过滤操作时。但要小心不要过度使用它们。阅读 [为什么你不应该过度使用 Python 中的列表推导式](https://www.kdnuggets.com/why-you-should-not-overuse-list-comprehensions-in-python) 来了解过度使用它们可能会变成一种过度的好事。

## 2\. 使用生成器进行高效的数据处理

你可以在 Python 中使用生成器来迭代大型数据集和序列，而无需一开始就将它们全部存储在内存中。这在内存效率重要的应用程序中特别有用。

与常规 Python 函数使用 `return` 关键字返回整个序列不同，生成器函数返回一个生成器对象。你可以对其进行循环，以按需逐个获取单独的项。

假设我们有一个大型 CSV 文件，其中包含用户数据，我们想逐行处理每一行——一次处理一行——而不是一次将整个文件加载到内存中。

这是生成器函数的示例：

```py
import csv
from typing import Generator, Dict

def read_large_csv_with_generator(file_path: str) -> Generator[Dict[str, str], None, None]:
    with open(file_path, 'r') as file:
        reader = csv.DictReader(file)
        for row in reader:
            yield row

# Path to a sample CSV file
file_path = 'large_data.csv'

for row in read_large_csv_with_generator(file_path):
    print(row)
```

**注意**：请记得在上述代码片段中将‘large_data.csv’替换为你文件的路径。

如你所见，在处理流数据或数据集大小超出可用内存时，使用生成器尤其有用。

要详细了解生成器，请阅读 [开始使用 Python 生成器](https://www.kdnuggets.com/2023/02/getting-started-python-generators.html)。

## 3\. 缓存昂贵的函数调用

缓存可以通过存储昂贵函数调用的结果并在函数再次调用相同输入时重用这些结果，从而显著提高性能。

假设你正在从头开始编写 k-means 聚类算法并希望缓存计算出的欧几里得距离。以下是如何使用 `@cache` 装饰器缓存函数调用：

```py
 from functools import cache
from typing import Tuple
import numpy as np

@cache
def euclidean_distance(pt1: Tuple[float, float], pt2: Tuple[float, float]) -> float:
    return np.sqrt((pt1[0] - pt2[0]) ** 2 + (pt1[1] - pt2[1]) ** 2)

def assign_clusters(data: np.ndarray, centroids: np.ndarray) -> np.ndarray:
    clusters = np.zeros(data.shape[0])
    for i, point in enumerate(data):
        distances = [euclidean_distance(tuple(point), tuple(centroid)) for centroid in centroids]
        clusters[i] = np.argmin(distances)
    return clusters
```

让我们来看以下示例函数调用：

```py
data = np.array([[1.0, 2.0], [2.0, 3.0], [3.0, 4.0], [8.0, 9.0], [9.0, 10.0]])
centroids = np.array([[2.0, 3.0], [8.0, 9.0]])

print(assign_clusters(data, centroids))
```

其输出为：

```py
Outputs >>> [0\. 0\. 0\. 1\. 1.]
```

要了解更多信息，请阅读 [如何通过缓存加速 Python 代码](https://www.kdnuggets.com/how-to-speed-up-python-code-with-caching)。

## 4\. 使用上下文管理器进行资源处理

在 Python 中，[上下文管理器](https://www.kdnuggets.com/how-to-create-custom-context-managers-in-python) 确保资源——如文件、数据库连接和子进程——在使用后得到适当管理。

假设你需要查询数据库并希望在使用后确保连接被正确关闭：

```py
import sqlite3

def query_db(db_path):
    with sqlite3.connect(db_path) as conn:
        cursor = conn.cursor()
        cursor.execute(query)
        for row in cursor.fetchall():
            yield row
```

你现在可以尝试对数据库运行查询：

```py
query = "SELECT * FROM users"
for row in query_database('people.db', query):
    print(row)
```

要了解更多关于上下文管理器的使用，请阅读 [Python 上下文管理器的 3 种有趣用途](https://www.kdnuggets.com/3-interesting-uses-of-python-context-managers)。

## 5\. 使用 NumPy 向量化操作

NumPy 允许你对数组执行逐元素操作——如对向量的操作——而无需显式循环。这通常比循环要快得多，因为 NumPy 在底层使用 C。

假设我们有两个大型数组，分别表示来自两个不同测试的分数，我们想为每个学生计算平均分。让我们使用循环来完成这个任务：

```py
import numpy as np

# Sample data
scores_test1 = np.random.randint(0, 100, size=1000000)
scores_test2 = np.random.randint(0, 100, size=1000000)

# Using a loop
average_scores_loop = []
for i in range(len(scores_test1)):
    average_scores_loop.append((scores_test1[i] + scores_test2[i]) / 2)

print(average_scores_loop[:10])
```

下面是如何使用 NumPy 的向量化操作重写它们的示例：

```py
# Using NumPy vectorized operations
average_scores_vectorized = (scores_test1 + scores_test2) / 2

print(average_scores_vectorized[:10])
```

### 循环与向量化操作

让我们使用 timeit 测量循环和 NumPy 版本的执行时间：

```py
setup = """
import numpy as np

scores_test1 = np.random.randint(0, 100, size=1000000)
scores_test2 = np.random.randint(0, 100, size=1000000)
"""

loop_code = """
average_scores_loop = []
for i in range(len(scores_test1)):
    average_scores_loop.append((scores_test1[i] + scores_test2[i]) / 2)
"""

vectorized_code = """
average_scores_vectorized = (scores_test1 + scores_test2) / 2
"""

loop_time = timeit.timeit(stmt=loop_code, setup=setup, number=10)
vectorized_time = timeit.timeit(stmt=vectorized_code, setup=setup, number=10)

print(f"Loop time: {loop_time:.6f} seconds")
print(f"Vectorized time: {vectorized_time:.6f} seconds")
```

如所见，使用 Numpy 的向量化操作比循环版本要快得多：

```py
Output >>>
Loop time: 4.212010 seconds
Vectorized time: 0.047994 seconds
```

## 总结

本教程到此为止！

我们回顾了以下技巧——使用列表推导代替循环、利用生成器进行高效处理、缓存昂贵的函数调用、使用上下文管理器管理资源以及利用 NumPy 进行向量化操作——这些都能帮助优化代码性能。

如果你在寻找针对数据科学项目的技巧，请阅读 [5 个 Python 数据科学最佳实践](https://www.kdnuggets.com/5-python-best-practices-for-data-science)。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正在通过编写教程、操作指南、意见文章等方式，学习并与开发者社区分享她的知识。Bala 还制作了引人入胜的资源概述和编码教程。

### 相关话题

+   [3 种基于研究的高级提示技术提升 LLM 效率……](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [效率决定生物神经元与……的区别](https://www.kdnuggets.com/2022/11/efficiency-spells-difference-biological-neurons-artificial-counterparts.html)

+   [计算深度学习模型的计算效率……](https://www.kdnuggets.com/2023/06/calculate-computational-efficiency-deep-learning-models-flops-macs.html)

+   [利用 ChatGPT 最大化数据分析效率](https://www.kdnuggets.com/maximizing-efficiency-in-data-analysis-with-chatgpt)

+   [如何计算算法效率](https://www.kdnuggets.com/2022/09/calculate-algorithm-efficiency.html)

+   [提升数学效率：驾驭 Numpy 数组操作](https://www.kdnuggets.com/elevate-math-efficiency-navigating-numpy-array-operations)
