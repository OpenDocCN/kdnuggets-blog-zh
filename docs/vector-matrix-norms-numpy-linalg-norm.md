# 使用 NumPy Linalg 范数的向量和矩阵范数

> 原文：[`www.kdnuggets.com/2023/05/vector-matrix-norms-numpy-linalg-norm.html`](https://www.kdnuggets.com/2023/05/vector-matrix-norms-numpy-linalg-norm.html)

![使用 NumPy Linalg 范数的向量和矩阵范数](img/7358f625af3da5057e3573b28316189d.png)

作者提供的图片

**Num**erical **Py**thon 或 **NumPy** 是一个流行的 Python 科学计算库。NumPy 库有大量内置功能，用于创建 n 维数组并对其进行计算。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

如果你对数据科学、计算线性代数和相关领域感兴趣，学习如何计算向量和矩阵的范数会很有帮助。本教程将教你如何使用 NumPy 的**linalg**模块中的函数来做到这一点。

要进行代码编写，你需要在开发环境中安装 Python 和 NumPy。为了使 `print()` 语句中的 f-strings 正常工作，你需要安装 Python 3.8 或更高版本。

让我们开始吧！

# 什么是范数？

在本讨论中，我们将首先查看向量范数。稍后我们会讨论矩阵范数。从数学上讲，范数是一个从 n 维向量空间到实数集合的函数（或映射）：

![使用 NumPy Linalg 范数的向量和矩阵范数](img/5711822e157bdce711d26c52e5b9c19f.png)

> **注意**：范数也可以定义在复数向量空间上，**C^n → R** 也是一种有效的范数定义。但在本讨论中，我们将限制在实数向量空间。

## 范数的性质

对于 n 维向量**x** = (x1,x2,x3,...,xn)，**x** 的范数通常表示为 ||**x**||，应满足以下性质：

+   ||**x**|| 是**非负**量。对于向量**x**，范数 ||**x**|| 总是大于或等于零。当且仅当向量**x**是全零向量时，||**x**|| 才等于零。

+   对于两个向量**x** = (x1,x2,x3,...,xn) 和 **y** = (y1,y2,y3,...,yn)，它们的范数 ||**x**|| 和 ||**y**|| 应满足**三角不等式**：||**x** + **y**|| <= ||**x**|| + ||**y**||。

+   此外，所有范数都满足 ||α**x**|| = |α| ||**x**||，其中 α 为标量。

# 常见的向量范数：L1、L2 和 L∞ 范数

一般来说，n 维向量**x** = (x1,x2,x3,...,xn)的 Lp 范数（或 p-范数）对于 p >= 0 定义为：

![使用 NumPy Linalg Norm 的向量和矩阵范数](img/949698ab0104ea8f6bd3bbf5d3c5be9c.png)

让我们来看看常见的向量范数，即 L1、L2 和 L∞ 范数。

## L1 范数

L1 范数等于向量中所有元素绝对值的总和：

![使用 NumPy Linalg Norm 的向量和矩阵范数](img/334e1ceeaed5cb565d25ead443da73aa.png)

## L2 范数

将 p =2 代入一般的 Lp 范数方程，我们得到向量的 L2 范数的以下表达式：

![使用 NumPy Linalg Norm 的向量和矩阵范数](img/d072dbf3fd7da9f6804c893cc25d497c.png)

## L∞ 范数

对于给定的向量 **x**，L∞ 范数是 **x** 元素 *绝对* 值中的 **最大值**：

![使用 NumPy Linalg Norm 的向量和矩阵范数](img/db19cf650980003b693f8bcb9f0018ae.png)

验证这些范数是否满足前面列出的范数属性是相当简单的。

# 如何在 NumPy 中计算向量范数

NumPy 中的 **linalg** 模块包含我们可以用来计算范数的函数。

在开始之前，让我们初始化一个向量：

```py
import numpy as np
vector = np.arange(1,7)
print(vector)
```

```py
Output >> [1 2 3 4 5 6]
```

## NumPy 中的 L2 范数

让我们从 NumPy 中导入 **linalg** 模块：

```py
from numpy import linalg
```

`norm()` 函数用于计算矩阵和向量的范数。此函数需要一个必需的参数——我们需要计算范数的向量或矩阵。此外，它还接受以下 *可选* 参数：

+   `ord` 决定了计算的范数的阶数，以及

+   `axis` 指定了计算范数的轴。

当我们在函数调用中没有指定 `ord` 时，`norm()` 函数默认计算 L2 范数：

```py
l2_norm = linalg.norm(vector)
print(f"{l2_norm = :.2f}")
```

```py
Output >> l2_norm = 9.54
```

我们可以通过明确将 `ord` 设置为 2 来验证这一点：

```py
l2_norm = linalg.norm(vector, ord=2)
print(f"{l2_norm = :.2f}")
```

```py
Output >> l2_norm = 9.54
```

## NumPy 中的 L1 范数

要计算向量的 L1 范数，请调用 `norm()` 函数，设置 `ord = 1`：

```py
l1_norm = linalg.norm(vector, ord=1)
print(f"{l1_norm = :.2f}")
```

```py
Output >> l1_norm = 21.00
```

由于我们的示例 `vector` 仅包含正数，我们可以验证在这种情况下 L1 范数等于元素的总和：

```py
assert sum(vector) == l1_norm
```

## NumPy 中的 L∞ 范数

要计算 L∞ 范数，将 `ord` 设置为 'np.inf'：

```py
inf_norm = linalg.norm(vector, ord=np.inf)
print(f"{inf_norm = }")
```

在这个例子中，我们得到 6，向量中的 *最大* 元素（绝对值意义上）：

```py
Output >> inf_norm = 6.0
```

在 `norm()` 函数中，你还可以将 `ord` 设置为 '-np.inf'。

```py
neg_inf_norm = linalg.norm(vector, ord=-np.inf)
print(f"{neg_inf_norm = }")
```

如你所猜，负的 L∞ 范数返回向量中的 *最小* 元素（绝对值意义上）：

```py
Output >> neg_inf_norm = 1.0
```

## 关于 L0 范数的说明

L0 范数给出向量中非零元素的数量。从技术上讲，这不是一个范数。它实际上是一个伪范数，因为它违反了 ||α**x**|| = |α| ||**x**|| 的属性。这是因为，即使向量被一个标量乘以，非零元素的数量 *保持不变*。

要获取向量中非零元素的数量，将 `ord` 设置为 0：

```py
another_vector = np.array([1,2,0,5,0])
l0_norm = linalg.norm(another_vector,ord=0)
print(f"{l0_norm = }")
```

在这里，`another_vector` 有 3 个非零元素：

```py
Output >> l0_norm = 3.0
```

# 理解矩阵范数

到目前为止，我们已经看到如何计算向量范数。就像你可以将向量范数视为从 n 维向量空间到实数集合的映射一样，矩阵范数是从 m x n 矩阵空间到实数集合的映射。数学上，你可以表示为：

![NumPy Linalg Norm 的向量和矩阵范数](img/b90c3f12b0aaa8fe9441d83fe3429480.png)

常见的矩阵范数包括 Frobenius 范数和核范数。

## Frobenius 范数

对于一个 m x n 的矩阵 A，其中 m 行 n 列，Frobenius 范数由下式给出：

![NumPy Linalg Norm 的向量和矩阵范数](img/4f935b1f8583c4049e446e54a978b487.png)

## 核范数

奇异值分解（SVD）是一种矩阵分解技术，用于主题建模、图像压缩和协同过滤等应用。

SVD 将输入矩阵分解为一个左奇异向量矩阵（U）、一个奇异值矩阵（S）和一个右奇异向量矩阵（V_T）。而核范数是矩阵的最大奇异值。

![NumPy Linalg Norm 的向量和矩阵范数](img/65222385d97e70b2b4f9310c4fb704c1.png)

# 如何在 NumPy 中计算矩阵范数

继续讨论如何在 NumPy 中计算矩阵范数，让我们将 `vector` 重塑为一个 2 x 3 的矩阵：

```py
matrix = vector.reshape(2,3)
print(matrix)
```

```py
Output >> 
[[1 2 3]
 [4 5 6]]
```

## NumPy 中的矩阵 Frobenius 范数

如果你不指定 `ord` 参数，`norm()` 函数默认计算 Frobenius 范数。

让我们通过将 `ord` 设置为 `'fro'` 来验证这一点：

```py
frob_norm = linalg.norm(matrix,ord='fro')
print(f"{frob_norm = :.2f}")
```

```py
Output >> frob_norm = 9.54
```

当我们不传入可选的 `ord` 参数时，我们也得到 Frobenius 范数：

```py
frob_norm = linalg.norm(matrix)
print(f"{frob_norm = :.2f}")
```

```py
Output >> frob_norm = 9.54
```

总结来说，当 `norm()` 函数以矩阵作为输入时，默认返回矩阵的 Frobenius 范数。

## NumPy 中的矩阵核范数

要计算矩阵的核范数，你可以传入矩阵并在 `norm()` 函数调用中将 `ord` 设置为 'nuc'：

```py
nuc_norm = linalg.norm(matrix,ord='nuc')
print(f"{nuc_norm = :.2f}")
```

```py
Output >> nuc_norm = 10.28
```

## 沿特定轴的矩阵范数

我们通常不会在矩阵上计算 L1 和 L2 范数，但 NumPy 允许你计算任意 `ord` 的矩阵（2D 数组）和其他多维数组的范数。

让我们看看如何在特定轴上计算矩阵的 L1 范数——沿行和列。

类似地，我们可以设置 `axis = 1`。

`axis = 0` 表示矩阵的行。如果设置 `axis = 0`，则计算矩阵的 L1 范数是 **跨行**（或沿列）进行的，如下所示：

![NumPy Linalg Norm 的向量和矩阵范数](img/6c64acb724eb702509298b4bb66ac0fc.png)

图片由作者提供

让我们在 NumPy 中验证这一点：

```py
matrix_1_norm = linalg.norm(matrix,ord=1,axis=0)
print(f"{matrix_1_norm = }")
```

```py
Output >> matrix_1_norm = array([5., 7., 9.])
```

类似地，我们可以设置 `axis = 1`。

`axis = 1` 表示矩阵的列。因此，通过设置 `axis = 1` 计算矩阵的 L1 范数是 **跨列**（沿行）进行的。

![NumPy Linalg Norm 的向量和矩阵范数](img/ba423c79ba54c08deb408fe0fc13979f.png)

图片由作者提供

```py
matrix_1_norm = linalg.norm(matrix,ord=1,axis=1)
print(f"{matrix_1_norm = }")
```

```py
Output >> matrix_1_norm = array([ 6., 15.])
```

我建议你试验 `ord` 和 `axis` 参数，并尝试不同的矩阵，直到你掌握它。

# 结论

我希望你现在明白了如何使用 NumPy 计算向量和矩阵范数。然而，需要注意的是，Frobenius 范数和核范数仅对矩阵定义。因此，如果你计算向量或维度超过两个的多维数组，你会遇到错误。这就是本教程的全部内容！

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位技术作家，喜欢创作长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过编写教程、使用指南等方式与开发者社区分享她的学习成果。

### 更多相关主题

+   [Python 向量数据库与向量索引：LLM 应用架构](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [如何使用 NumPy 执行矩阵运算](https://www.kdnuggets.com/how-to-perform-matrix-operations-with-numpy)

+   [傻瓜指南：精度、召回率和混淆矩阵](https://www.kdnuggets.com/2020/01/guide-precision-recall-confusion-matrix.html)

+   [混淆矩阵、精度和召回率解析](https://www.kdnuggets.com/2022/11/confusion-matrix-precision-recall-explained.html)

+   [KDnuggets 新闻，11 月 16 日：LinkedIn 如何使用机器学习 •…](https://www.kdnuggets.com/2022/n45.html)

+   [AI 和 LLM 使用案例中的向量数据库](https://www.kdnuggets.com/vector-databases-in-ai-and-llm-use-cases)
