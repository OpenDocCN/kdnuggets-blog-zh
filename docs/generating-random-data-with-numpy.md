# 生成随机数据与 NumPy

> 原文：[https://www.kdnuggets.com/generating-random-data-with-numpy](https://www.kdnuggets.com/generating-random-data-with-numpy)

![使用 NumPy 生成随机数据](../Images/b2812e23ca1b57b27f25f46ef1875f2d.png)

图片由编辑提供 | Ideogram

随机数据由通过各种工具生成的值组成，这些值没有可预测的模式。值的出现取决于它们所抽取的概率分布，因为它们是不可预测的。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在我们的实验中使用随机数据有很多好处，包括现实世界数据模拟、机器学习训练的合成数据或统计采样目的。

NumPy 是一个强大的包，支持许多数学和统计计算，包括随机数据生成。从简单的数据到复杂的多维数组和矩阵，NumPy 可以帮助我们满足随机数据生成的需求。

本文将进一步讨论如何使用 NumPy 生成随机数据。所以，让我们深入了解吧。

## 使用 NumPy 生成随机数据

你需要在环境中安装 NumPy 包。如果你还没有安装，可以使用 pip 来安装。

```py
pip install numpy 
```

当包成功安装后，我们将进入文章的主要部分。

首先，我们将设置种子号以便可重复性。当我们使用计算机进行随机事件时，我们必须记住我们所做的只是伪随机的。伪随机的概念是数据看起来随机，但如果我们知道起点（我们称之为种子），它是确定性的。

要在 NumPy 中设置种子，我们将使用以下代码：

```py
import numpy as np

np.random.seed(101)
```

你可以将任何正整数作为种子，这将成为我们的起点。此外，NumPy 的 `.random` 方法将成为本文的主要函数。

一旦设置了种子，我们将尝试使用 NumPy 生成随机数数据。让我们尝试随机生成五个不同的浮点数。

```py
np.random.rand(5)
```

```py
Output>>
array([0.51639863, 0.57066759, 0.02847423, 0.17152166, 0.68527698])
```

使用 NumPy 可以获得多维数组。例如，以下代码将生成一个填充随机浮点数的 3x3 数组。

```py
np.random.rand(3, 3)
```

```py
Output>>
array([[0.26618856, 0.77888791, 0.89206388],
       [0.0756819 , 0.82565261, 0.02549692],
       [0.5902313 , 0.5342532 , 0.58125755]])
```

接下来，我们可以从某个范围生成一个整数随机数。我们可以使用以下代码来实现：

```py
np.random.randint(1, 1000, size=5)
```

```py
Output>>
array([974, 553, 645, 576, 937])
```

之前通过随机采样生成的所有数据都遵循均匀分布。这意味着所有数据发生的机会相似。如果我们将数据生成过程迭代到无限次，所有数字的频率将接近相等。

我们可以从各种分布生成随机数据。在这里，我们尝试从标准正态分布生成十个随机数据。

```py
np.random.normal(0, 1, 10)
```

```py
Output>>
array([-1.31984116,  1.73778011,  0.25983863, -0.317497  ,  0.0185246 ,
       -0.42062671,  1.02851771, -0.7226102 , -1.17349046,  1.05557983])
```

上述代码获取了均值为零和标准差为一的正态分布的 Z 分数值。

我们可以生成遵循其他分布的随机数据。下面是我们如何使用泊松分布生成随机数据。

```py
np.random.poisson(5, 10)
```

```py
Output>>
array([10,  6,  3,  3,  8,  3,  6,  8,  3,  3])
```

上述代码中的泊松分布随机样本数据会模拟特定平均率（5）的随机事件，但生成的数字可能会有所不同。

我们可以生成遵循二项分布的随机数据。

```py
np.random.binomial(10, 0.5, 10)
```

```py
Output>>
array([5, 7, 5, 4, 5, 6, 5, 7, 4, 7])
```

上述代码模拟了我们执行的基于二项分布的实验。假设我们进行十次掷硬币实验（第一个参数为十，第二个参数为概率 0.5）；多少次会出现正面？如上面的输出所示，我们进行了十次实验（第三个参数）。

让我们尝试指数分布。使用这段代码，我们可以生成遵循指数分布的数据。

```py
np.random.exponential(1, 10)
```

```py
Output>>
array([0.7916478 , 0.59574388, 0.1622387 , 0.99915554, 0.10660882,
       0.3713874 , 0.3766358 , 1.53743068, 1.82033544, 1.20722031])
```

指数分布解释了事件之间的时间。例如，上述代码可以表示等待公交车进入车站，这需要随机的时间，但平均需要 1 分钟。

对于更高级的生成，你可以随时结合分布结果来创建遵循自定义分布的样本数据。例如，下面生成的随机数据中有 70% 遵循正态分布，而其余部分遵循指数分布。

```py
def combined_distribution(size=10):
    # normal distribution
    normal_samples = np.random.normal(loc=0, scale=1, size=int(0.7 * size))

    #exponential distribution
    exponential_samples = np.random.exponential(scale=1, size=int(0.3 * size))

    # Combine the samples
    combined_samples = np.concatenate([normal_samples, exponential_samples])

    # Shuffle thes samples
    np.random.shuffle(combined_samples)

    return combined_samples

samples = combined_distribution()
samples
```

```py
Output>>
array([-1.42085224, -0.04597935, -1.22524869,  0.22023681,  1.13025524,
        0.74561453,  1.35293768,  1.20491792, -0.7179921 , -0.16645063])
```

这些自定义分布更为强大，特别是当我们想要模拟数据以符合实际情况数据（通常更为复杂）时。

## 结论

NumPy 是一个强大的 Python 包，用于数学和统计计算。它生成的随机数据可以用于许多事件，例如数据模拟、机器学习的合成数据等。

在本文中，我们讨论了如何使用 NumPy 生成随机数据，包括可以改善数据生成体验的方法。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一位数据科学助理经理和数据编写员。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 撰写了各种人工智能和机器学习主题的文章。

### 了解更多主题

+   [假装直到成功：生成真实的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)

+   [什么是矩生成函数？](https://www.kdnuggets.com/2022/12/momentgenerating-functions.html)

+   [如何使用GPT通过Hugging Face生成创意内容…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [随机森林与决策树：主要区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [随机森林算法是否需要归一化？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [决策树与随机森林，解释](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)
