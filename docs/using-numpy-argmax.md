# 使用 Numpy 的 argmax()

> 原文：[https://www.kdnuggets.com/2022/07/using-numpy-argmax.html](https://www.kdnuggets.com/2022/07/using-numpy-argmax.html)

![使用 Numpy 的 argmax()](../Images/8f28a3c503b099a667bf10b17d928f5b.png)

而 `argmax()` 返回... 9?!? 对，没错！

# 解释 `argmax()`

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

回想一下5个W：*谁，什么，何时，哪里，为什么*。

在处理问题时，用正确的“W”来框定问题可能意味着你能否获得所需的答案，或者陷入完全的困惑。

考虑以下内容：

+   **你的名字是什么时候？**

+   **你**是做什么的？

+   **你**住在哪条街上？

这些问题在提出时意义不大，因此你寻找的答案可能不会像预期的那样迅速出现。

当编写 Python 代码并使用 Numpy 的 `argmax()` 函数时，也可能会出现类似的困惑。`argmax` 在处理矩阵或任何维数的多维数组并查找最大值时非常有用。然而，`argmax` 返回的是沿某轴的最大值的索引，而不是最大值本身。

`argmax` 会告诉你**在哪里**，而不是**什么**。这是一个使用 Numpy 库的人经常误解的关键点，可能导致挫折。

# 理解 `argmax()`

让我们看看 Numpy 的 `argmax` 是如何设计的。

根据 [Numpy 文档](https://numpy.org/doc/stable/reference/generated/numpy.argmax.html)，`argmax` “[返回沿某轴的最大值的索引]”。

这意味着实际的最大值没有被返回，仅仅是这些最大值的位置。

其重要参数包括从输入数组中查找特定轴的最大值并返回其位置，以及一个特定的轴（这是可选的）。此外，还可以选择传递一个输出数组，以及一个布尔值以保留结果中的任何缩减轴。关于可选的轴参数，如果没有指定，返回的索引将指向展平后的输入数组。

`argmax` 返回一个原始数组索引的数组，其维度取决于函数的输入。

# 为什么 `argmax()`？

根据 Jason Brownlee 在 [机器学习大师](https://machinelearningmastery.com/argmax-in-machine-learning) 的一篇文章：

> `argmax`在机器学习中最常用于找到预测概率最大的类别。
> 
> [...]
> 
> 在应用机器学习中，你最常遇到的使用`argmax`的情况是寻找数组中结果为最大值的索引。

如果我们考虑类别成员预测的概率，`argmax`函数可以确定数组中包含最大值的索引位置，从而获得最高概率的预测。这对于机器学习显然是有用的。

让我们看看`argmax`在实践中的表现。

# 使用`argmax()`

让我们来看看`argmax`的实际应用。

## 单维度

在以通常的方式导入Numpy后，我们创建了一个单维度数组并将其传递给`argmax`。查看数组和代码，猜测一下你认为输出应该是什么。

```py
import numpy as np

a = [[ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]]
max_idx = np.argmax(a)
print(max_idx)
```

```py
9
```

这个输出符合你的预期吗？

如果你记得`argmax`返回的是索引而不是值，并且索引从0开始，我们可以看到我们得到的是索引9——实际上是第十个位置——它包含了'10'，即数组中的最大值。

明白了吗？

## 多维度

让我们看看`argmax`如何在多个维度的数组中工作。

```py
import numpy as np

a = [[ 1, 2, 3, 4, 5 ], [ 6, 7, 8, 9, 10 ]]
max_idx = np.argmax(a)
print(max_idx)
```

```py
9
```

现在我们有一个二维数组。我们没有传递轴参数，因此`argmax`的默认行为是将多维数组展平为一维，并返回该展平数组中最大值的索引。

因此，结果与前面单维度数组的结果相同，现在应该很明显为什么。

## 指定轴

但是如果我们指定一个轴呢？

首先，回顾一下轴0指的是*行*，而轴1指的是*列*。让我们看看当我们将`axis=0`传递给`argmax`时会发生什么。

```py
import numpy as np

a = [[ 1, 2, 3, 4, 5 ], [ 6, 7, 8, 9, 10 ]]
max_idx = np.argmax(a, axis=0)
print(max_idx)
```

```py
array([1, 1, 1, 1, 1)
```

这到底是怎么回事？

我们指定了`axis=0`，因此`argmax`将返回多维数组行上的最大值。

**最大值**是什么？如前所述，对于这个数组，它是'10'。

**最大值**在哪里？它在第二行的最后一列。

由于我们已经指定了我们想知道它在**行**中的位置（`axis=0`），对于数组中的每一列，Numpy报告了最大值出现的行。回顾一下索引从0开始，我们可以看到，在这个数组的每一列中，最大值出现在索引为1的行，即第二行。查看我们的代码，值'10'的位置似乎`argmax`是正确的。

如果我们指定`axis=1`，你期望输出是什么？我将这留给读者作为练习。

# 总结

在这篇文章中，我们了解了`argmax`是什么，为什么我们要使用它，并且介绍了几个使用Numpy的`argmax`函数的例子。

你现在应该知道`argmax`是如何工作的。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** （**[@mattmayo13](https://twitter.com/mattmayo13)**）是数据科学家以及 KDnuggets 的主编，KDnuggets 是一个开创性的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系。

### 更多相关内容

+   [使用 NumPy 执行日期和时间计算](https://www.kdnuggets.com/using-numpy-to-perform-date-and-time-calculations)

+   [使用 NumPy Linalg Norm 计算向量和矩阵范数](https://www.kdnuggets.com/2023/05/vector-matrix-norms-numpy-linalg-norm.html)

+   [超越 Numpy 和 Pandas：发掘鲜为人知的 Python 库的潜力](https://www.kdnuggets.com/2023/08/beyond-numpy-pandas-unlocking-potential-lesserknown-python-libraries.html)

+   [Numpy 和 Pandas 介绍](https://www.kdnuggets.com/introduction-to-numpy-and-pandas)

+   [提高数学效率：导航 NumPy 数组操作](https://www.kdnuggets.com/elevate-math-efficiency-navigating-numpy-array-operations)

+   [从头开始使用 NumPy 进行线性回归](https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy)
