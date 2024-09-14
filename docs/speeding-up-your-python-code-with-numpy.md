# 使用 NumPy 加速你的 Python 代码

> 原文：[`www.kdnuggets.com/speeding-up-your-python-code-with-numpy`](https://www.kdnuggets.com/speeding-up-your-python-code-with-numpy)

![使用 NumPy 加速你的 Python 代码](img/b64f632158a37d94ca844c89dd824ab7.png)[图片由 storyset 提供，来源于 Freepik](https://www.freepik.com/free-vector/market-launch-concept-illustration_7171502.htm#fromView=search&page=1&position=3&uuid=715fea93-5839-4794-b955-1691b8cf44f5)

NumPy 是一个常用于数学和统计应用的 Python 包。然而，有些人仍然不知道 NumPy 可以帮助加速我们的 Python 代码执行。

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

NumPy 能够加速 Python 代码执行的原因有几个，包括：

+   NumPy 在循环中使用 C 代码而不是 Python

+   更好的 CPU 缓存过程

+   数学运算中的高效算法

+   能够使用并行操作

+   在大数据集和复杂计算中内存高效

出于多种原因，NumPy 在提高 Python 代码执行效率方面非常有效。本教程将展示 NumPy 如何加速代码处理的例子。让我们来看看。

## NumPy 在加速 Python 代码执行中的作用

第一个例子比较了 Python 列表和 NumPy 数组的数值运算，它们获取具有预期值结果的对象。

例如，我们希望从两个相加的列表中得到一个数字列表，所以我们执行了向量化操作。我们可以用以下代码进行实验：

```py
import numpy as np
import time

sample = 1000000

list_1 = range(sample)
list_2 = range(sample)
start_time = time.time()
result = [(x + y) for x, y in zip(list_1, list_2)]
print("Time taken using Python lists:", time.time() - start_time)

array_1 = np.arange(sample)
array_2 = np.arange(sample)
start_time = time.time()
result = array_1 + array_2
print("Time taken using NumPy arrays:", time.time() - start_time)
```

```py
Output>>
Time taken using Python lists: 0.18960118293762207
Time taken using NumPy arrays: 0.02495265007019043
```

正如你在上面的输出中看到的，NumPy 数组的执行速度比 Python 列表要快，从而获得相同的结果。

在整个例子中，你会发现 NumPy 的执行速度更快。让我们看看是否要进行聚合统计分析。

```py
array = np.arange(1000000)

start_time = time.time()
sum_rst = np.sum(array)
mean_rst = np.mean(array)
print("Time taken for aggregation functions:", time.time() - start_time)
```

```py
Output>> 
Time taken for aggregation functions: 0.0029935836791992188
```

NumPy 可以非常快速地处理聚合函数。如果我们与 Python 执行进行比较，可以看到执行时间的差异。

```py
list_1 = list(range(1000000))

start_time = time.time()
sum_rst = sum(list_1)
mean_rst = sum(list_1) / len(list_1)
print("Time taken for aggregation functions (Python):", time.time() - start_time)
```

```py
Output>>
Time taken for aggregation functions (Python): 0.09979510307312012
```

在相同结果下，Python 的内置函数会比 NumPy 花费更多时间。如果我们有更大的数据集，Python 完成这些操作的时间会比 NumPy 长得多。

另一个例子是当我们尝试进行原地操作时，我们可以看到 NumPy 会比 Python 示例快得多。

```py
array = np.arange(1000000)
start_time = time.time()
array += 1
print("Time taken for in-place operation:", time.time() - start_time)
```

```py
list_1 = list(range(1000000))
start_time = time.time()
for i in range(len(list_1)):
    list_1[i] += 1
print("Time taken for in-place list operation:", time.time() - start_time)
```

```py
Output>>
Time taken for in-place operation: 0.0010089874267578125
Time taken for in-place list operation: 0.1937870979309082
```

例子的关键点在于，如果你有机会使用 NumPy，那么效果会更好，因为处理速度会更快。

我们可以尝试更复杂的实现，使用矩阵乘法来观察 NumPy 相比于 Python 的速度。

```py
def python_matrix_multiply(A, B):
    result = [[0 for _ in range(len(B[0]))] for _ in range(len(A))]
    for i in range(len(A)):
        for j in range(len(B[0])):
            for k in range(len(B)):
                result[i][j] += A[i][k] * B[k][j]
    return result

def numpy_matrix_multiply(A, B):
    return np.dot(A, B)

n = 200
A = [[np.random.rand() for _ in range(n)] for _ in range(n)]
B = [[np.random.rand() for _ in range(n)] for _ in range(n)]

A_np = np.array(A)
B_np = np.array(B)

start_time = time.time()
python_result = python_matrix_multiply(A, B)
print("Time taken for Python matrix multiplication:", time.time() - start_time)

start_time = time.time()
numpy_result = numpy_matrix_multiply(A_np, B_np)
print("Time taken for NumPy matrix multiplication:", time.time() - start_time)
```

```py
Output>>
Time taken for Python matrix multiplication: 1.8010151386260986
Time taken for NumPy matrix multiplication: 0.008051872253417969
```

如你所见，NumPy 在更复杂的活动中也更快，比如使用标准 Python 代码的矩阵乘法。

我们可以尝试更多的例子，但 NumPy 应该比 Python 内置函数的执行时间更快。

## 结论

NumPy 是一个强大的数学和数值处理包。与标准 Python 内置函数相比，NumPy 的执行时间会更快。这就是为什么，如果适用，尝试使用 NumPy 来加速我们的 Python 代码。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在 Allianz Indonesia 全职工作期间，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 涉及各种 AI 和机器学习主题的写作。

### 更多相关话题

+   [作为数据科学家管理你的可重用 Python 代码](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

+   [追踪和可视化你的 Python 代码执行的 3 种工具](https://www.kdnuggets.com/2021/12/3-tools-track-visualize-execution-python-code.html)

+   [如何作为数据科学家注释你的 Python 代码](https://www.kdnuggets.com/how-to-comment-your-python-code-as-a-data-scientist)

+   [KDnuggets™ 新闻 22:n01, 1 月 5 日: 追踪和可视化的 3 种工具…](https://www.kdnuggets.com/2022/n01.html)

+   [加速你的 Python 代码的 3 种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [优化 Python 代码性能: 深入分析 Python 性能分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)
