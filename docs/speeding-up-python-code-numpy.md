# 使用 Numpy 加速你的 Python 代码的一个简单技巧

> 原文：[`www.kdnuggets.com/2019/06/speeding-up-python-code-numpy.html`](https://www.kdnuggets.com/2019/06/speeding-up-python-code-numpy.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论![figure-name](img/3dca2a2ec5dce62853868c8be29e6f66.png)

Python 非常庞大。

在过去几年中，Python 的受欢迎程度迅速增长。其重要原因之一是数据科学、机器学习和人工智能的兴起，这些领域都有高层次的 Python 库可供使用！

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在使用 Python 进行这些工作时，通常需要处理非常大的数据集。这些大型数据集直接读取到内存中，并作为 Python 数组、列表或字典进行存储和处理。

处理如此庞大的数组可能会很耗时；这实际上就是问题的本质。你有成千上万、数百万甚至数十亿的数据点。每个数据点处理过程中增加的微秒会因为数据规模的巨大而显著拖慢你的速度。

### 慢速方式

处理大型数据集的慢速方式是使用原始 Python。我们可以通过一个非常简单的例子来演示这一点。

下面的代码将 1.0000001 的值自乘 500 万次！

```py

import time

start_time = time.time()

num_multiplies = 5000000
data = range(num_multiplies)
number = 1

for i in data:
    number *= 1.0000001

end_time = time.time()

print(number)
print("Run time = {}".format(end_time - start_time))

```

我家里有一台相当不错的 CPU，Intel i7–8700k，加上 32GB 的 3000MHz RAM。尽管如此，将这 500 万个数据点相乘仍然需要 0.21367 秒。如果我将`num_multiplies`的值改为 10 亿次，处理时间变成了 43.24129 秒！

让我们再试一个带有数组的例子。

我们将构建一个大小为 1000x1000 的 Numpy 数组，每个元素的值为 1，再尝试将每个元素乘以一个浮点数 1.0000001。代码如下。

在同一台机器上，将这些数组值乘以 1.0000001 的常规浮点循环耗时 1.28507 秒。

```py

import time
import numpy as np

start_time = time.time()

data = np.ones(shape=(1000, 1000), dtype=np.float)

for i in range(1000):
    for j in range(1000):
        data[i][j] *= 1.0000001
        data[i][j] *= 1.0000001
        data[i][j] *= 1.0000001
        data[i][j] *= 1.0000001
        data[i][j] *= 1.0000001

end_time = time.time()

print("Run time = {}".format(end_time - start_time))

```

### 什么是矢量化？

Numpy 被设计成在矩阵操作中高效。更具体地说，大多数 Numpy 中的处理都是*矢量化的*。

矢量化涉及将数学运算，如我们在这里使用的乘法，表示为作用于整个数组，而不是它们的单个元素（如我们的 for 循环）。

通过矢量化，底层代码被并行化，以便操作可以在多个数组元素上同时运行，而不是一个一个地遍历它们。只要你应用的操作不依赖于其他数组元素，即“状态”，那么矢量化将给你带来一些良好的加速效果。

遍历 Python 数组、列表或字典可能会很慢。因此，Numpy 中的矢量化操作被映射到高度优化的 C 代码，使它们比标准 Python 对应操作快得多。

### 快速方法

这是做事的快速方法——按照 Numpy 被*设计*的方式来使用它。

在寻求加速的过程中，我们可以遵循几个要点：

+   如果对数组进行 for 循环，那么很有可能我们可以用一些内置的 Numpy 函数来替代它

+   如果我们看到任何类型的数学运算，很有可能我们可以用一些内置的 Numpy 函数来替代它

这两个要点都非常关注将非矢量化的 Python 代码替换为优化过的、矢量化的低级 C 代码。

查看我们之前第一个示例的快速版本，这次进行了 10 亿次乘法运算。

我们做了一件非常简单的事情：我们发现有一个 for 循环中重复了多次相同的数学操作。这应该立即提示我们去寻找可以替代它的 Numpy 函数。

我们找到了一种——`power` 函数，它简单地对输入值应用一定的幂。代码运行速度大幅提升至 7.6293e-6 秒——这就是一个

```py

import time
import numpy as np

start_time = time.time()

num_multiplies = 1000000000
data = range(num_multiplies)
number = 1

number *= np.power(1.0000001, num_multiplies)

end_time = time.time()

print(number)
print("Run time = {}".format(end_time - start_time))

```

将值乘入 Numpy 数组的想法非常类似。我们看到我们使用了双重 for 循环，并且应该立即意识到应该有更快的方法。

方便的是，如果我们直接将 1.0000001 标量相乘，Numpy 会自动对我们的代码进行矢量化。因此，我们可以像对待 Python 列表一样编写乘法操作。

以下代码演示了这一点，并在 0.003618 秒内运行——这是 355 倍的加速！

```py

import time
import numpy as np

start_time = time.time()

data = np.ones(shape=(1000, 1000), dtype=np.float)

for i in range(5):
    data *= 1.0000001

end_time = time.time()

print("Run time = {}".format(end_time - start_time))

```

### 喜欢学习吗？

在 [twitter](https://twitter.com/GeorgeSeif94) 上关注我，我会发布关于最新和最棒的 AI、技术和科学的内容！也可以在 [LinkedIn](https://www.linkedin.com/in/georgeseif/) 上与我联系！

### 推荐阅读

想了解更多关于 Python 编程的内容吗？[***Python Crash Course***](https://amzn.to/2H1JIDw) 书籍是学习 Python 编程的最佳资源！

另外提醒一下，我通过亚马逊联盟链接支持这个博客，因为分享好书对大家都有帮助！作为亚马逊会员，我从合格的购买中获得收益。

**简介: [George Seif](https://towardsdatascience.com/@george.seif94)** 是一位认证的极客和 AI / 机器学习工程师。

[原文](https://towardsdatascience.com/one-simple-trick-for-speeding-up-your-python-code-with-numpy-1afc846db418)。经许可转载。

**相关：**

+   为什么你应该更频繁地使用 .npy 文件

+   为什么你应该忘记数据科学代码中的‘for-loop’，而拥抱矢量化

+   处理 NumPy 矩阵：一个实用的初步参考

### 更多相关话题

+   [使用 NumPy 加速 Python 代码](https://www.kdnuggets.com/speeding-up-your-python-code-with-numpy)

+   [加速 Python 代码的 3 种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [个性化 AI 简单易懂：你的无代码 GPT 适配指南](https://www.kdnuggets.com/personalized-ai-made-simple-your-no-code-guide-to-adapting-gpts)

+   [机器学习不像你的大脑 第一部分：神经元很慢，……](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [ETL 与 ELT：哪一个适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [追踪和可视化 Python 代码执行的 3 个工具](https://www.kdnuggets.com/2021/12/3-tools-track-visualize-execution-python-code.html)
