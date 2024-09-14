# 使用 Numpy 矩阵：一个便捷的初学者参考

> 原文：[`www.kdnuggets.com/2017/03/working-numpy-matrices.html`](https://www.kdnuggets.com/2017/03/working-numpy-matrices.html)

**作者：Ieva Zarina，Nordigen 软件开发工程师。**

在我开始处理自然语言处理时，我使用了默认的 Python 列表。但随着实验规模的扩大和数据量的增加，我很快就耗尽了内存。Python 列表在内存空间上不是优化的，因此转向了 [Numpy](http://www.numpy.org/)。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

![Numpy](img/d021f4f813085ed85b4cb6a6984b9721.png)

Numpy 是 Python 科学生态系统中*事实上的*ndarray 工具。

Numpy 数组很像 C 中的数组 – 通常你会先创建所需大小的数组，然后填充它。不推荐进行合并、附加，因为 Numpy 会创建一个与被合并数组相同大小的空数组，然后将内容复制到其中。

下面是一些可以操作 Numpy 数组（[ndarray](http://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.html)）的方法：

### 创建 ndarray

创建 Numpy 矩阵的一些方法是：

+   **从 Python 列表转换**使用 [numpy.asarray()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.asarray.html)：

```py
import numpy as np

list = [1, 2, 3]
c = np.asarray(list)

```

+   **创建一个大小为**所需的 ndarray，并填充为 1、0 或随机值：

```py
# Array items as ndarray 
c = np.array([1, 2, 3])

# A 2x2 2d array shape for the arrays in the format (rows, columns)
shape = (2, 2)

# Random values
c = np.empty(shape)

d = np.ones(shape)
e = np.zeros(shape)

```

+   你也可以使用 [numpy.empty_like()](http://docs.scipy.org/doc/numpy/reference/generated/numpy.empty_like.html#numpy.empty_like) 创建一个**形状与另一个数组相同**的数组：

```py
# Creating ndarray from list
c = np.array([[1., 2.,],[1., 2.]])

# Creating new array in the shape of c, filled with 0
d = np.empty_like(c)

```

### 切片

有时候我需要在二维矩阵中**选择**所有**列**或**行**的某一部分。例如，矩阵：

```py
a = np.asarray([[1,1,2,3,4], # 1st row
                [2,6,7,8,9], # 2nd row
                [3,6,7,8,9], # 3rd row
                [4,6,7,8,9], # 4th row
                [5,6,7,8,9]  # 5th row
              ])

b = np.asarray([[1,1],
                [1,1]])

# Select row in the format a[start:end], if start or end omitted it means all range.
y = a[:1]  # 1st row
y = a[0:1] # 1st row
y = a[2:5] # select rows from 3rd to 5th row

# Select column in the format a[start:end, column_number]
x = a[:, -1] # -1 means first from the end
x = a[:,1:3] # select cols from 2nd col until 3rd

```

### 合并数组

不建议合并 Numpy 数组，因为 Numpy 会在内部创建一个大的空数组，然后将内容复制进去。最好是在一开始就创建所需大小的数组，然后直接填充。然而，有时候你无法避免合并。在这种情况下，Numpy 提供了一些**内置函数：**

**[连接](http://docs.scipy.org/doc/numpy/reference/generated/numpy.concatenate.html)**

一维数组：

```py
a = np.array([1, 2, 3])
b = np.array([5, 6])
print np.concatenate([a, b, b])  
# >>  [1 2 3 5 6 5 6]

```

二维数组：

```py
a2 = np.array([[1, 2], [3, 4]])

# axis=0 - concatenate along rows
print np.concatenate((a2, b), axis=0)
# >>   [[1 2]
#       [3 4]
#       [5 6]]

# axis=1 - concatenate along columns, but first b needs to be transposed:
b.T
#>> [[5]
#    [6]]
np.concatenate((a2, b.T), axis=1)
#>> [[1 2 5]
#    [3 4 6]]

```

**[附加](http://docs.scipy.org/doc/numpy/reference/generated/numpy.append.html) – 将值附加到数组的末尾**

一维数组：

```py
# 1d arrays
print np.append(a, a2)
# >> [1 2 3 1 2 3 4]

print np.append(a, a)
# >> [1 2 3 1 2 3]

```

二维数组 – 两个数组必须匹配行的形状：

```py
print np.append(a2, b, axis=0)
# >> [[1 2]
#     [3 4]
#     [5 6]]

print np.append(a2, b.T, axis=1)
# >> [[1 2 5]
#     [3 4 6]]

```

**[Hstack](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.hstack.html)（水平堆叠）和 [vstack](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.vstack.html#numpy.vstack)（垂直堆叠）**

一维数组：

```py
print np.hstack([a, b])
# >> [1 2 3 5 6]

print np.vstack([a, a])
# >> [[1 2 3]
#     [1 2 3]]

```

二维数组：

```py
print np.hstack([a2,a2]) # arrays must match shape
# >> [[1 2 1 2]
#     [3 4 3 4]]

print np.vstack([a2, b])
# >> [[1 2]
#     [3 4]
#     [5 6]]

```

### ndarray 中的类型

不使用默认的浮点数，numpy 可以容纳所有常见的**[类型](http://docs.scipy.org/doc/numpy/user/basics.types.html)**。如果数组中的任何一个数字是浮点数，所有数字将会被转换为浮点数：

```py
a = np.array([1, 2, 3.3])
print a
# >> [ 1\.   2\.   3.3]

```

但你可以轻松地**将类型转换**为 int、float 或其他类型：

```py
a = np.array([1, 2, 3.3])
print a
# >> [ 1\.   2\.   3.3]print a.astype(int)
# >> [1 2 3]

```

**字符串数组**需要以类型 S1 创建表示长度为 1 的字符串，S2 表示长度为 2，以此类推。 [`numpy.``chararray()`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.chararray.html) 创建这种类型的数组。你需要指定数组的形状和 itemsize —— 每个字符串的长度。

```py
chararray = np.chararray([3,3], itemsize=3)
chararray[:] = 'abc' # assing value to all fields 
print chararray
#>> [['abc' 'abc' 'abc']
#    ['abc' 'abc' 'abc']
#    ['abc' 'abc' 'abc']]

```

### 读/写文件

使用 [numpy.savetext()](http://docs.scipy.org/doc/numpy/reference/generated/numpy.savetxt.html) 以纯文本形式将 numpy 数组写入文件，并使用 [numpy.loadtext()](http://docs.scipy.org/doc/numpy/reference/generated/numpy.loadtxt.html) 加载它：

```py
a2 = np.array([
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1],
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1],
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1],
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1],
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1],
    [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1]
])

np.savetxt('test.txt', a2, delimiter=',')
a2_new = np.loadtxt('test.txt', delimiter=',')

```

**编写稀疏矩阵**

然而，在机器学习中，如果你有一个大型的**[稀疏](https://en.wikipedia.org/wiki/Sparse_matrix)矩阵**（包含大量为 0 的值），使用**svmlight 格式**可以更快地读取和写入大型矩阵，同时文件也更小：

```py
from sklearn.datasets import dump_svmlight_file, load_svmlight_file

matrix = [
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 3, 0, 0, 0, 0, 0, 0, 0, 1, 2],
    [1, 0, 0, 0, 0, 3, 0, 0, 0, 0, 0, 0, 0, 1, 2]
]

labels = [1,1,1,1,1,2,2]

dump_svmlight_file(matrix, labels, 'svmlight.txt', zero_based=True)

# The file looks like this:

# 1 0:1 13:1 14:2
# 1 0:1 13:1 14:2
# 1 0:1 13:1 14:2
# 1 0:1 13:1 14:2
# 1 0:1 13:1 14:2
# 2 0:1 5:3 13:1 14:2
# 2 0:1 5:3 13:1 14:2

svm_loaded = load_svmlight_file('svmlight.txt', zero_based=True)

```

使用 .toarray() 从 svmlight 压缩稀疏行格式中获取矩阵：

```py
svm_loaded[0].toarray() # matrix element at index 0
svm_loaded[1] # labels at index 1

```

总结来说，本文探讨了如何：

+   创建 numpy 数组，

+   切片数组，

+   合并数组，

+   numpy 数组的基本类型，

+   读取和写入数组到文件，

+   读取和写入稀疏矩阵到 svmlight 格式。

这只是 numpy 矩阵的一个介绍，讲解了如何入门和进行基本操作。更多信息可以在 [MIT 指南书](http://web.mit.edu/dvp/Public/numpybook.pdf) 和官方 [文档](http://docs.scipy.org/doc/) 中找到。

**个人简介： [Ieva Zarina](https://www.linkedin.com/in/ieva-zarina-60515a72/)** 是 Nordigen 的软件开发人员。

[原文](http://ieva.rocks/2016/08/24/working-with-numpy-matrices/)。经许可转载。

**相关：**

+   掌握 Python 机器学习的 7 个额外步骤

+   K-Means & 其他聚类算法：Python 快速入门

+   使用 Iris 数据集的简单 XGBoost 教程

### 更多相关话题

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [处理大数据：工具和技术](https://www.kdnuggets.com/working-with-big-data-tools-and-techniques)

+   [在 Python 中操作 SQLite 数据库的指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)

+   [让深度学习在实际环境中发挥作用：一个以数据为中心的课程](https://www.kdnuggets.com/2022/04/corise-deep-learning-wild-data-centric-course.html)

+   [远程工作的数据科学家需要的 6 种软技能](https://www.kdnuggets.com/2022/05/6-soft-skills-data-scientists-working-remotely.html)

+   [让深度学习在实际环境中发挥作用：一个以数据为中心的课程](https://www.kdnuggets.com/2022/11/corise-deep-learning-wild-data-centric-course.html)
