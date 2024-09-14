# 如何使用 NumPy 对数组进行填充

> 原文：[`www.kdnuggets.com/how-to-apply-padding-to-arrays-with-numpy`](https://www.kdnuggets.com/how-to-apply-padding-to-arrays-with-numpy)

![如何使用 NumPy 对数组进行填充](img/82adb7c73e63d66b5db719b315874990.png)

图片来自 [freepik](https://www.freepik.com/free-vector/futuristic-code-zoom-background_29807378.htm#fromView=search&page=1&position=2&uuid=4a381e3b-a6c2-49c7-bfff-d0a4415fead3)

填充是向数组的边缘添加额外元素的过程。这可能听起来很简单，但它有多种应用，可以显著提升数据处理任务的功能和性能。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

假设你正在处理图像数据。通常，在应用滤镜或执行卷积操作时，图像的边缘可能会成为问题，因为周围没有足够的像素来一致地应用操作。对图像进行填充（在原始图像周围添加行和列的像素）可以确保每个像素都得到均等处理，从而产生更准确、更具视觉吸引力的输出。

你可能会想知道填充是否仅限于图像处理。答案是否定的。在深度学习中，填充在处理卷积神经网络（CNN）时至关重要。它允许你在网络的连续层中保持数据的空间维度，防止数据在每次操作后缩小。这在保留输入数据的原始特征和结构时尤为重要。

在时间序列分析中，填充可以帮助对齐不同长度的序列。这种对齐对于将数据输入到机器学习模型中至关重要，因为输入大小的一致性通常是必要的。

在本文中，你将学习如何使用 NumPy 对数组进行填充，以及填充的不同类型和使用 NumPy 填充数组时的最佳实践。

## Numpy.pad

[numpy.pad](https://numpy.org/doc/stable/reference/generated/numpy.pad.html) 函数是 NumPy 中添加数组填充的首选工具。该函数的语法如下所示：

numpy.pad(array, pad_width, mode='constant', **kwargs)

其中：

+   **array**：要添加填充的输入数组。

+   **pad_width**：这是填充到每个轴边缘的值的数量。它指定了在数组的每个轴的两端添加的元素数量。它可以是一个整数（所有轴相同填充），一个包含两个整数的元组（每个轴的两端填充不同），或者是不同轴的这种元组的序列。

+   **mode**：这是用于填充的方法，决定了应用哪种类型的填充。常见的模式包括：零填充、边缘填充、对称填充等。

+   **kwargs**：这些是根据模式的不同而额外的关键字参数。

让我们检查一个数组示例，看看如何使用 NumPy 为其添加填充。为了简单起见，我们将专注于一种填充类型：零填充，它是最常见且直接的。

#### 步骤 1：创建数组

首先，让我们创建一个简单的 2D 数组来进行操作：

```py
import numpy as np
# Create a 2D array
array = np.array([[1, 2], [3, 4]])
print("Original Array:")
print(array) 
```

输出：

```py
Original Array:
[[1 2]
 [3 4]] 
```

#### 步骤 2：添加零填充

接下来，我们将为该数组添加零填充。我们使用 `np.pad` 函数来实现这一点。我们将指定填充宽度为 1，围绕整个数组添加一行/列的零。

```py
# Add zero padding
padded_array = np.pad(array, pad_width=1, mode='constant', constant_values=0)
print("Padded Array with Zero Padding:")
print(padded_array) 
```

输出：

```py
Padded Array with Zero Padding:
[[0 0 0 0]
 [0 1 2 0]
 [0 3 4 0]
 [0 0 0 0]] 
```

**说明**

+   **原始数组**：我们的起始数组是一个简单的 2x2 数组，值为 **[[1, 2], [3, 4]]**。

+   **零填充**：通过使用 `np.pad`，我们在原始数组周围添加了一层零。`pad_width=1` 参数指定在每一侧添加一行/列的填充。`mode='constant'` 参数表示填充应为常数值，我们通过 `constant_values=0` 将其设置为零。

## 填充类型

不同类型的填充方式中，零填充（zero padding），如上例所示，是其中之一；其他示例包括常数填充（constant padding）、边缘填充（edge padding）、反射填充（reflect padding）和对称填充（symmetric padding）。让我们详细讨论这些填充类型，并了解如何使用它们。

#### 零填充

零填充是添加额外值到数组边缘的最简单和最常用的方法。这种技术涉及用零填充数组，在各种应用中非常有用，例如图像处理。

零填充涉及在数组的边缘添加填充零的行和列。这有助于在执行可能会缩小数组的操作时保持数据的大小。

示例：

```py
import numpy as np

array = np.array([[1, 2], [3, 4]])
padded_array = np.pad(array, pad_width=1, mode='constant', constant_values=0)
print(padded_array) 
```

输出：

```py
[[0 0 0 0]
 [0 1 2 0]
 [0 3 4 0]
 [0 0 0 0]] 
```

#### 常数填充

常数填充允许你使用自己选择的常数值来填充数组，而不仅仅是零。这个值可以是你选择的任何东西，比如 0、1 或其他数字。当你希望保持特定的边界条件或零填充可能不适合你的分析时，它特别有用。

示例：

```py
array = np.array([[1, 2], [3, 4]])
padded_array = np.pad(array, pad_width=1, mode='constant', constant_values=5)
print(padded_array) 
```

输出：

```py
[[5 5 5 5]
 [5 1 2 5]
 [5 3 4 5]
 [5 5 5 5]] 
```

#### 边缘填充

边缘填充用边缘的值填充数组。不是添加零或某个常数值，而是使用最近的边缘值来填补空白。这种方法有助于保持原始数据模式，并且在你希望避免将新的或任意值引入数据时非常有用。

示例：

```py
array = np.array([[1, 2], [3, 4]])
padded_array = np.pad(array, pad_width=1, mode='edge')
print(padded_array) 
```

输出：

```py
[[1 1 2 2]
 [1 1 2 2]
 [3 3 4 4]
 [3 3 4 4]] 
```

#### 反射填充

反射填充是一种通过从原始数组的边缘镜像值来填充数组的技术。这意味着边界值在边缘处反射，这有助于在数据中保持模式和连续性，而不会引入任何新的或任意的值。

示例：

```py
array = np.array([[1, 2], [3, 4]])
padded_array = np.pad(array, pad_width=1, mode='reflect')
print(padded_array) 
```

输出：

```py
[[4 3 4 3]
 [2 1 2 1]
 [4 3 4 3]
 [2 1 2 1]] 
```

#### 对称填充

对称填充是一种操作数组的技术，有助于保持原始数据的平衡和自然延展。它类似于反射填充，但在反射中包括了边缘值本身。这种方法有助于在填充数组中保持对称性。

示例：

```py
array = np.array([[1, 2], [3, 4]])
padded_array = np.pad(array, pad_width=1, mode='symmetric')
print(padded_array) 
```

输出：

```py
[[1 1 2 2]
 [1 1 2 2]
 [3 3 4 4]
 [3 3 4 4]] 
```

## 应用 NumPy 填充到数组的常见最佳实践

1.  选择正确的填充类型

1.  确保填充值与数据的性质一致。例如，二进制数据应使用零填充，但在图像处理任务中，边缘填充或反射填充可能更为适合，应避免使用零填充。

1.  考虑填充如何影响数据分析或处理任务。填充可能会引入伪影，特别是在图像或信号处理领域，因此请选择一种能够最小化这种影响的填充类型。

1.  填充多维数组时，确保正确指定填充维度。不对齐的维度可能会导致错误或意外结果。

1.  清楚地记录为何以及如何在代码中应用填充。这有助于保持清晰性，并确保其他用户（或未来的你）理解填充的目的和方法。

## 结论

在这篇文章中，你学习了填充数组的概念，这是一种广泛应用于图像处理和时间序列分析等领域的基本技术。我们探讨了填充如何帮助扩展数组的大小，使其适用于不同的计算任务。

我们介绍了 `numpy.pad` 函数，该函数简化了在 NumPy 中向数组添加填充的过程。通过清晰简明的示例，我们展示了如何使用 `numpy.pad` 向数组添加填充，并展示了各种填充类型，如零填充、常数填充、边缘填充、反射填充和对称填充。

遵循这些最佳实践，你可以使用 NumPy 对数组进行填充，确保你的数据操作准确、高效，并适合你的特定应用。

[](https://www.linkedin.com/in/olumide-shittu)****[Shittu Olumide](https://www.linkedin.com/in/olumide-shittu/)**** 是一位软件工程师和技术作家，他热衷于利用前沿技术创作引人入胜的叙述，注重细节，并擅长简化复杂概念。你还可以在 [Twitter](https://twitter.com/Shittu_Olumide_) 上找到 Shittu。

### 更多相关主题

+   [NumPy 中的遮罩数组处理缺失数据](https://www.kdnuggets.com/masked-arrays-in-numpy-to-handle-missing-data)

+   [将 AI 应用于小数据集的 5 种方法](https://www.kdnuggets.com/2022/02/5-ways-apply-ai-small-data-sets.html)

+   [MLOps：最佳实践及如何应用](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

+   [在 Pandas 数据框中使用 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [什么是切比雪夫定理，它如何应用于数据科学？](https://www.kdnuggets.com/2022/11/chebychev-theorem-apply-data-science.html)

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理，它如何…](https://www.kdnuggets.com/2022/n46.html)
