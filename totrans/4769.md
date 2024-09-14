# 使用 Python 对你的数据预处理进行 2–6 倍的加速

> 原文：[https://www.kdnuggets.com/2018/10/get-speed-up-data-pre-processing-python.html](https://www.kdnuggets.com/2018/10/get-speed-up-data-pre-processing-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

Python 是进行所有机器学习任务的首选编程语言。它易于使用，并且拥有许多出色的库，使得数据处理变得轻而易举！但当我们处理大量数据时，情况就会变得有些复杂……

如今，“大数据”一词通常指的是至少有数十万甚至 *数百万* 数据点的数据集！在如此规模下，每一个小计算都会累积起来，我们需要在编写每一步骤的代码时保持效率。考虑到机器学习系统效率时，一个常常被忽视的关键步骤是 *预处理* 阶段，我们必须对所有数据点应用某种操作。

默认情况下，Python 程序作为一个单一进程使用单个 CPU 执行。大多数现代机器用于机器学习时，*至少* 有 2 个 CPU 核心。这意味着，对于 2 个 CPU 核心的例子来说，当你运行预处理时，50% 或更多的计算机处理能力默认不会被使用！当你达到 4 核心（现代 Intel i5）或 6 核心（现代 Intel i7）时，情况会更糟。

但幸运的是，Python 标准库中有一个稍微隐蔽的功能，可以让我们利用所有的 CPU 核心！得益于 Python 的 `concurrent.futures` 模块，只需 3 行代码即可将普通程序转换为可以在 CPU 核心间并行处理数据的程序。

### 标准方法

让我们以一个简单的例子来说明，我们有一个文件夹中的图像数据集；也许我们甚至有成千上万或数百万张图像！为了处理时间，我们这里用 1000 张。我们希望将所有图像调整为 600x600 的大小，然后再传递给我们的深度神经网络。这就是在 GitHub 上你经常会看到的一些标准 Python 代码的样子。

这个程序遵循了你在数据处理脚本中经常会看到的简单模式：

1.  你从一份你想要处理的文件列表（或其他数据）开始。

1.  你使用 `for` 循环逐个处理每一条数据，然后在每次循环迭代中运行预处理

我们来在一个包含 1000 个 jpeg 文件的文件夹上测试这个程序，看看运行需要多长时间：

```py
time python standard_res_conversion.py
```

在我的 i7–8700k 六核 CPU 上，这给我带来了 **7.9864 秒** 的运行时间！对于这样一个高端 CPU 来说，这似乎有点慢。我们来看看如何加快速度。

### 快速方法

要理解我们如何希望 Python 并行处理事务，直观地思考并行处理本身是有帮助的。假设我们要完成同一个单一任务——将钉子钉入一块木头，我们有 1000 个钉子。如果每个钉子需要 1 秒，那么一个人完成这个工作需要 1000 秒。但如果我们有 4 个人在团队中，我们可以将桶分成 4 份，每个人处理自己的一份钉子。使用这种方法，我们只需 250 秒就能完成工作！

在我们的例子中，我们可以让 Python 对 1000 张图片做类似的处理：

1.  将 jpg 文件列表分成 4 个较小的组。

1.  运行 4 个独立的 Python 解释器实例。

1.  让每个 Python 实例处理 4 个较小的数据组之一。

1.  将 4 个进程的结果合并以获得最终的结果列表

最棒的是，Python 为我们处理了所有的繁重工作。我们只需告诉它要运行哪个函数以及使用多少个 Python 实例，它就会完成剩下的工作！我们只需更改**3 行代码**。

从上述代码：

```py
with concurrent.futures.ProcessPoolExecutor() as executor:
```

启动与 CPU 核心数量相同的 Python 进程，在我的情况下是 6 个。实际的处理行是这一行：

```py
executor.map(load_and_resize, image_files)
```

**executor.map()** 接受你希望运行的函数和一个列表作为输入，其中列表的每个元素都是**函数的单个输入**。由于我们有 6 个核心，我们将同时处理列表中的 6 个项目！

如果我们再次使用以下方法运行程序：

```py
time python fast_res_conversion.py
```

我们获得了**1.14265 秒**的运行时间，几乎是 x6 的加速！

*注意：启动更多 Python 进程并在它们之间传输数据会有一定的开销，因此你不会总是获得如此大的速度提升。但总体而言，你的加速通常会非常显著*

### 它总是超级快速的吗？

使用 Python 并行池是处理数据列表并对每个数据点执行类似计算的绝佳解决方案。但这并不总是完美的解决方案。通过并行池处理的数据不会以任何可预测的顺序处理。如果你需要处理结果按特定顺序排列，那么这个方法可能不适合你。

你处理的数据也需要是 Python 知道如何“序列化”的类型。幸运的是，这些类型是相当常见的。来自官方 Python 文档：

+   `None`、`True` 和 `False`

+   整数、浮点数、复数

+   字符串、字节、字节数组

+   包含仅可序列化对象的元组、列表、集合和字典

+   在模块顶层定义的函数（使用`[def](https://docs.python.org/3/reference/compound_stmts.html#def)`，而不是`[lambda](https://docs.python.org/3/reference/expressions.html#lambda)`）

+   在模块顶层定义的内置函数

+   定义在模块顶层的类

+   类的实例，其`[__dict__](https://docs.python.org/3/library/stdtypes.html#object.__dict__)`或调用`[__getstate__()](https://docs.python.org/3/library/pickle.html#object.__getstate__)`的结果是可序列化的（有关详细信息，请参见[序列化类实例](https://docs.python.org/3/library/pickle.html#pickle-inst)部分）。

### 喜欢阅读关于技术的内容吗？

在[twitter](https://twitter.com/GeorgeSeif94)上关注我，了解最新最酷的AI、技术和科学！

**简介： [George Seif](https://towardsdatascience.com/@george.seif94)** 是一名认证技术专家和AI/机器学习工程师。

[原文](https://towardsdatascience.com/heres-how-you-can-get-a-2-6x-speed-up-on-your-data-pre-processing-with-python-847887e63be5). 经授权转载。

**相关：**

+   [5个“干净代码”技巧将显著提升你的生产力](/2018/10/5-clean-code-tips-dramatically-improve-productivity.html)

+   [数据科学家需要了解的5种聚类算法](/2018/06/5-clustering-algorithms-data-scientists-need-know.html)

+   [为你的回归问题选择最佳机器学习算法](/2018/08/selecting-best-machine-learning-algorithm-regression-problem.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关内容

+   [Python数据预处理简单指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)

+   [通过这本免费电子书学习数据清理和预处理以应用于数据科学](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [掌握数据清理和预处理技术的7个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [利用ChatGPT进行自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [在Pandas中清理和预处理文本数据以进行NLP任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

+   [如何在没有工作经验的情况下获得第一份数据科学工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)
