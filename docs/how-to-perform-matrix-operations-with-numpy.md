# 如何使用 NumPy 执行矩阵操作

> 原文：[https://www.kdnuggets.com/how-to-perform-matrix-operations-with-numpy](https://www.kdnuggets.com/how-to-perform-matrix-operations-with-numpy)

![如何使用 NumPy 执行矩阵操作](../Images/9bd1f47928182ebd9c497268f612cfd6.png)

图片由 [Vlado Paunovic](https://unsplash.com/photos/black-and-white-checkered-illustration-iBG594vhR1k) 提供

NumPy 是一个强大的 Python 库，包含大量数学函数，并支持创建可以应用这些数学函数的矩阵和多维数组。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在这个简短的教程中，你将学习如何使用 NumPy 执行几种最基本的矩阵操作。

## NumPy 中的矩阵和数组

在 NumPy 中，矩阵被定义为一种专门的数组，严格为二维，并且在应用数学操作后保持其二维性。此类型的矩阵可以使用 `np.matrix` 类实现，但 NumPy 不再推荐使用此类，因为它可能在将来被移除。NumPy 推荐的替代选项是使用 N 维数组类型 `ndarray`。

在 NumPy 中，ndarray 和矩阵的主要区别在于前者可以是任何维度的，并且其使用不局限于二维操作。

因此，在本教程中，我们将重点实施对二维数组的几种基本矩阵操作，这些数组是使用 `np.ndarray` 创建的。

## 创建 NumPy 数组

首先导入 NumPy 包，然后创建两个由两行三列组成的二维数组。这些数组将在本教程的后续示例中使用：

```py
# Import NumPy package
import numpy as np

# Create arrays
a1 = np.array([[0, 1, 0], [2, 3, 2]])
a2 = np.array([[3, 4, 3], [5, 6, 5]]) 
```

`shape` 属性让我们确认数组的维度：

```py
# Print one of the arrays
print('Array 1:', '\n', a1, '\n Shape: \n’, a1.shape) 
```

输出：

```py
Array 1: 
[[0 1 0]
[2 3 2]]

Shape: (2, 3) 
```

## 基本数组操作

NumPy 提供了自己的函数来执行数组的逐元素加法、减法、除法和乘法。此外，NumPy 还通过扩展 Python 的算术运算符的功能来处理逐元素数组操作。

以数组 `a1` 和 `a2` 之间的逐元素加法为例开始。

通过使用 `np.add` 函数或重载的 `+` 运算符，可以实现两个数组的逐元素加法：

```py
# Using np.add
func_add = np.add(a1, a2)

# Using the + operator
op_add = a1 + a2 
```

通过打印结果，可以确认它们都产生了相同的输出：

```py
# Print results
print('Function: \n', func_add, '\n\n', 'Operator: \n', op_add) 
```

输出：

```py
Function: 
[[3 5 3]
[7 9 7]]

Operator: 
[[3 5 3]
[7 9 7]] 
```

然而，如果我们对它们进行计时，会发现一点小差异：

```py
import numpy as np
import timeit

def func():

a1 = np.array([[0, 1, 0], [2, 3, 2]])
a2 = np.array([[3, 4, 3], [5, 6, 5]])
np.add(a1, a2)

def op():

a1 = np.array([[0, 1, 0], [2, 3, 2]])
a2 = np.array([[3, 4, 3], [5, 6, 5]])
a1 + a2

# Timing the functions over 100000 iterations
func_time = timeit.timeit(func, number=100000)
op_time = timeit.timeit(op, number=100000)

# Print timing results
print('Function:', func_time, '\n', 'Operator:', op_time) 
```

输出：

```py
Function: 0.2588757239282131 
Operator: 0.24321464297827333 
```

在这里可以看到，NumPy的`np.add`函数的性能略低于`+`运算符。这主要是因为add函数引入了类型检查，将任何*类似数组*的输入（如列表）转换为数组，然后再进行加法操作。这也就带来了额外的计算开销，相较于`+`运算符。

然而，这种措施也使得`np.add`函数更不容易出错。例如，将`np.add`应用于`list`类型的输入仍然有效（例如`np.add([1, 1], [2, 2])`），而使用`+`运算符则会导致列表连接。

同样，对于逐元素减法（使用`np.subtract`或`-`），除法（使用`np.divide`或`/`）和乘法（使用`np.multiply`或`*`），NumPy函数会进行类型检查，引入少量计算开销。

其他几个可能有用的操作包括转置和乘法数组。

矩阵转置会导致矩阵的正交旋转，可以使用`np.transpose`函数（它包含类型检查）或`.T`属性来实现：

```py
# Using np.transpose
func_a1_T = np.transpose(a1)

# Using the .T attribute
att_a1_T = a1.T 
```

矩阵乘法可以使用`np.dot`函数或`@`运算符（后者从Python 3.5开始实现了`np.matmul`函数）：

```py
# Using np.dot
func_dot = np.dot(func_a1_T, a2)

# Using the @ operator
op_dot = func_a1_T @ a2 
```

在处理二维数组时，`np.dot`和`np.matmul`的表现是一样的，并且都包含类型检查。

## 额外资源

+   [NumPy数组对象](https://numpy.org/doc/stable/reference/arrays.html)

+   [Python中NumPy数组的温和介绍](https://machinelearningmastery.com/gentle-introduction-n-dimensional-arrays-python-numpy/)

**[Stefania Cristina](https://www.linkedin.com/in/stefania-cristina-b3b7aa57)**，博士，是马耳他大学系统与控制工程系的高级讲师。她的研究兴趣集中在计算机视觉和机器学习领域。

### 更多相关主题

+   [如何在大数据集上进行内存高效的操作](https://www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas)

+   [NumPy Linalg Norm中的向量和矩阵范数](https://www.kdnuggets.com/2023/05/vector-matrix-norms-numpy-linalg-norm.html)

+   [提升数学效率：驾驭NumPy数组操作](https://www.kdnuggets.com/elevate-math-efficiency-navigating-numpy-array-operations)

+   [使用NumPy进行日期和时间计算](https://www.kdnuggets.com/using-numpy-to-perform-date-and-time-calculations)

+   [Python中的稀疏矩阵表示](https://www.kdnuggets.com/2020/05/sparse-matrix-representation-python.html)

+   [傻瓜指南：精准率、召回率和混淆矩阵](https://www.kdnuggets.com/2020/01/guide-precision-recall-confusion-matrix.html)
