# NumPy 在线性代数应用中的应用

> 原文：[https://www.kdnuggets.com/numpy-for-linear-algebra-applications](https://www.kdnuggets.com/numpy-for-linear-algebra-applications)

![NumPy 在线性代数应用中的应用](../Images/a5af47dcabba34b86db0b04b7b88de86.png)

图片来源：编辑

NumPy 是一个高效的线性代数工具。它有助于矩阵操作和方程求解。本文描述了用于线性代数的 NumPy 函数。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

## 矩阵乘法

矩阵乘法从两个矩阵创建一个新矩阵。将第一个矩阵的每一行与第二个矩阵的每一列相乘，将乘积相加得到新矩阵中的每个元素。

```py
# Define matrices
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
# Matrix multiplication
C = np.dot(A, B)
print("Matrix Multiplication:\n", C)

# Output:
# [[19 22]
# [43 50]] 
```

## 矩阵逆

将一个矩阵与其逆矩阵相乘得到单位矩阵。这有助于解决线性方程组。只有方阵和非奇异矩阵才有逆矩阵。

```py
# Define a square matrix
A = np.array([[1, 2], [3, 4]])
# Matrix inversion
A_inv = np.linalg.inv(A)
print("Matrix Inversion:\n", A_inv)

# Output:
# [[-2\.   1.]
# [ 1.5 -0.5]] 
```

## 矩阵行列式

矩阵行列式是来自矩阵的一个数字。它告诉我们矩阵是否可以被逆转。我们使用特定规则根据矩阵大小进行计算。

```py
# Define a square matrix
A = np.array([[1, 2], [3, 4]])
# Compute the determinant
det_A = np.linalg.det(A)
print("Determinant of the Matrix:", det_A)

# Output: -2.0000000000000004 
```

## 矩阵迹

迹是对角元素的总和。它仅适用于方阵。我们得到一个数字作为迹。

```py
# Define a square matrix
A = np.array([[1, 2], [3, 4]])
# Compute the trace of the matrix
trace_A = np.trace(A)
print("Trace of the Matrix:", trace_A)

# Output: 5 
```

## 矩阵转置

矩阵转置将矩阵沿其对角线翻转，它交换行和列。

```py
# Define a matrix
A = np.array([[1, 2, 3], [4, 5, 6]])

# Compute the transpose of the matrix
A_T = np.transpose(A)
print("Transpose of the Matrix:\n", A_T)

# Output:
# [[1 4]
# [2 5]
# [3 6]] 
```

## 特征值和特征向量

特征值显示了特征向量在变换过程中被缩放的程度。特征向量在此变换下方向不变。

```py
# Define a square matrix
A = np.array([[1, 2], [3, 4]])
# Compute eigenvalues and eigenvectors
eigenvalues, eigenvectors = np.linalg.eig(A)
print("Eigenvalues:", eigenvalues)
print("Eigenvectors:\n", eigenvectors)

# Output:
# Eigenvalues: [-0.37228132  5.37228132]
# Eigenvectors:
# [[-0.82456484 -0.41597356]
# [ 0.56576746 -0.90937671]] 
```

## LU 分解

LU 分解将一个矩阵分成两部分。一部分是下三角矩阵（L）。另一部分是上三角矩阵（U）。它有助于解决线性最小二乘问题和求解特征值。

```py
import numpy as np
from scipy.linalg import lu

# Define a matrix
A = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
# LU Decomposition
P, L, U = lu(A)
# Display results
print("LU Decomposition:")
print("P matrix:\n", P)
print("L matrix:\n", L)
print("U matrix:\n", U)

# Output
# LU Decomposition:
# P matrix:
# [[0\. 1\. 0.]
#  [0\. 0\. 1.]
#  [1\. 0\. 0.]]
# L matrix:
# [[ 1\.          0\.          0\.        ]
#  [ 0.33333333  1\.          0\.        ]
#  [ 0.66666667 -0.5         1\.        ]]
# U matrix:
# [[ 7\.          8\.          9\.        ]
#  [ 0\.          0.33333333  0.66666667]
#  [ 0\.          0\.          0\.        ]] 
```

## QR 分解

QR 分解将一个矩阵分成两部分。一部分是正交矩阵（Q）。另一部分是上三角矩阵（R）。它有助于解决线性最小二乘问题和求解特征值。

```py
import numpy as np
from scipy.linalg import qr
# Define a matrix
A = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
# QR Decomposition
Q, R = qr(A)
# Display results
print("QR Decomposition:")
print("Q matrix:\n", Q)
print("R matrix:\n", R)

# Output
# QR Decomposition:
# Q matrix:
# [[-0.26726124 -0.78583024  0.55708601]
#  [-0.53452248 -0.08675134 -0.83125484]
#  [-0.80178373  0.6172134   0.08122978]]
# R matrix:
# [[-7.41619849 -8.48528137 -9.55445709]
#  [ 0\.         -0.90453403 -1.80906806]
#  [ 0\.          0\.          0\.        ]] 
```

## SVD（奇异值分解）

SVD 将一个矩阵分解为三个矩阵：U、Σ 和 V*。U 和 V* 是正交矩阵。Σ 是对角矩阵。它在数据降维和解决线性系统等许多应用中非常有用。

```py
import numpy as np
from scipy.linalg import svd
# Define a matrix
A = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
# Singular Value Decomposition
U, s, Vh = svd(A)
# Display results
print("SVD Decomposition:")
print("U matrix:\n", U)
print("Singular values:\n", s)
print("Vh matrix:\n", Vh)

# Output
# SVD Decomposition:
# U matrix:
# [[-0.21483724  0.88723069  0.40824829]
#  [-0.52058739  0.24964395 -0.61237224]
#  [-0.82633755 -0.38794279  0.61237224]]
# Singular values:
# [16.84810335  1.06836951  0\.        ]
# Vh matrix:
# [[-0.47967118 -0.57236779 -0.66506439]
#  [ 0.77669099  0.07568647 -0.62531812]
#  [-0.40824829  0.81649658 -0.40824829]] 
```

## 线性方程的直接解法

找到满足系统中方程的变量值。每个方程表示一条直线。解是这些直线相交的地方。

```py
# Define matrix A and vector B
A = np.array([[3, 1], [1, 2]])
B = np.array([9, 8])
# Solve the system of linear equations Ax = B
x = np.linalg.solve(A, B)
print("Solution to Ax = B:", x)

# Output: [2\. 3.] 
```

## 最小二乘拟合

最小二乘拟合找到数据点的最佳匹配。它降低了实际值与预测值之间的平方差异。

```py
# Define matrix A and vector B
A = np.array([[1, 1], [1, 2], [1, 3]])
B = np.array([1, 2, 2])
# Solve the linear least-squares problem
x, residuals, rank, s = np.linalg.lstsq(A, B, rcond=None)
print("Least Squares Solution:", x)
print("Residuals:", residuals)
print("Rank of the matrix:", rank)
print("Singular values:", s)

# Output:
# Least Squares Solution: [0.66666667 0.5]
# Residuals: [0.33333333]
# Rank of the matrix: 2
# Singular values: [4.07914333 0.60049122] 
```

## 矩阵范数

矩阵范数衡量矩阵的大小。范数对于检查数值稳定性和分析矩阵很有用。

```py
# Define a matrix
A = np.array([[1, 2], [3, 4]])
# Compute various norms
frobenius_norm = np.linalg.norm(A, 'fro')
one_norm = np.linalg.norm(A, 1)
infinity_norm = np.linalg.norm(A, np.inf)
print("Frobenius Norm:", frobenius_norm)
print("1-Norm:", one_norm)
print("Infinity Norm:", infinity_norm)

# Output:
# Frobenius Norm: 5.477225575051661
# 1-Norm: 6.0
# Infinity Norm: 7.0 
```

## 条件数

矩阵的条件数衡量对输入变化的敏感性。高条件数意味着解决方案可能不稳定。

```py
# Define a matrix
A = np.array([[1, 2], [3, 4]])
# Compute the condition number of the matrix
condition_number = np.linalg.cond(A)
print("Condition Number:", condition_number)

# Output: 14.933034373659268 
```

## 矩阵秩

矩阵的秩是独立行或列的数量。它显示了矩阵的大小及其覆盖向量空间的能力。

```py
# Define a matrix
A = np.array([[1, 2], [3, 4]])
# Compute the rank of the matrix
rank_A = np.linalg.matrix_rank(A)
print("Matrix Rank:", rank_A)

# Output: 2 
```

## 结论

NumPy 简化了诸如矩阵运算和线性方程等任务。你可以在这个[网站](https://numpy.org/doc/stable/reference/routines.linalg.html)上了解更多关于这些 NumPy 函数的信息。

**[Jayita Gulati](https://www.linkedin.com/in/jayitagulati1998/)** 是一位机器学习爱好者和技术写作人，她对构建机器学习模型充满激情。她拥有利物浦大学的计算机科学硕士学位。

### 相关话题

+   [用于机器学习的 3 个免费线性代数学习资源](https://www.kdnuggets.com/2022/03/top-3-free-resources-learn-linear-algebra-machine-learning.html)

+   [数据科学中的线性代数](https://www.kdnuggets.com/2022/07/linear-algebra-data-science.html)

+   [KDnuggets 新闻，7 月 13 日：数据科学中的线性代数；10 个现代…](https://www.kdnuggets.com/2022/n28.html)

+   [掌握线性代数的 5 门免费课程](https://www.kdnuggets.com/2022/10/5-free-courses-master-linear-algebra.html)

+   [用 NumPy 从头开始进行线性回归](https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy)

+   [比较线性回归和逻辑回归](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)
