# Python 中计时代码的一种简单方法

> 原文：[`www.kdnuggets.com/2021/03/simple-way-time-code-python.html`](https://www.kdnuggets.com/2021/03/simple-way-time-code-python.html)

评论

**由 [Edward Krueger](https://www.linkedin.com/in/edkrueger/)，高级数据科学家和技术负责人 & [Douglas Franklin](https://www.linkedin.com/in/dougaf/)，有志数据科学家和教学助理**

![图示](img/c0d0bbae1f9f5187046476a61b4abe60.png)

Brad Neathery 的照片，来源于 Unsplash

### 介绍

我们的目标是创建一种简单的方法来计时 Python 中的函数。我们通过使用 Python 的库 `functools` 和 `time` 编写一个装饰器来实现。然后将此装饰器应用于我们感兴趣的函数。

### 计时装饰器：@timefunc

下面的代码代表了一个常见的装饰器模式，具有可重用和灵活的结构。注意 `functool.wraps` 的位置。它是我们闭包的装饰器。这个装饰器保留了 `func` 的元数据，因为它传递给了闭包。

在第 16 行，`functools` 在我们的打印语句中变得重要，我们访问 `func.__name__`。如果我们没有使用 `functools.wraps` 来装饰我们的闭包，就会返回错误的名称。

这个装饰器返回传递给 `timefunc()` 的函数的运行时间。在第 13 行，`start` 启动计时。然后，第 14 行的 `result` 存储 `func(*args, **kwargs)` 的值。之后，计算 `time_elapsed`。打印语句报告 `func` 的名称和执行时间。

### 使用 @ 符号应用 timefunc

在 Python 中，装饰器可以很容易地通过 `@` 符号应用。并非所有装饰器的应用都使用这种语法，但所有 `@` 符号都是装饰器的应用。

我们使用 `@` 符号将 `single_thread` 装饰为 `timefunc`。

现在 `single_thread` 被装饰后，当它在第 13 行被调用时，我们将看到它的 `func.__name__` 和运行时间。

![](img/e8a5bbe7e0d544055759eacbba2ba4cf.png)

`timefunc` 装饰的 `single_thread` 的输出

如果你想了解它是如何工作的，下面我们将深入探讨编写一个用于计时函数的装饰器的原因和方法。

### 为什么有人可能会对一个函数进行计时

原因相对简单。更快的函数就是更好的函数。

> 时间就是金钱，朋友。—— Gazlowe

计时装饰器向我们展示函数的运行时间。我们可以将装饰器应用于多个版本的函数，对它们进行基准测试，并选择最快的一个。此外，在测试代码时，了解执行所需的时间也很有用。计划有五分钟的运行时间？这是一个不错的时间段，可以起身活动一下腿部，或者重新填充咖啡！

在 Python 中编写装饰器函数时，我们依赖于 `functools` 和对作用域的了解。让我们深入了解作用域和装饰。

### 装饰、闭包和作用域

装饰是 Python 中的一种设计模式，它允许你修改函数的行为。装饰器是一个接收函数并返回修改后函数的函数。

在编写闭包和装饰器时，你必须牢记每个函数的作用域。在 Python 中，函数定义作用域。闭包可以访问返回它们的函数的作用域；装饰器的作用域。

在传递给闭包时，保存装饰函数的元数据非常重要。了解我们的作用域可以让我们正确地使用`functools.wraps`装饰我们的闭包。

*有关这些概念的更多信息，请阅读这篇三分钟的文章。*

[**Python 中的装饰器和闭包实例**](https://towardsdatascience.com/decorators-and-closures-by-example-in-python-382758321164)

如何使用装饰器增强函数的行为

### 关于这个装饰器的重用性

请注意，`func`作为参数在第 7 行被传入。然后在第 11 行，我们将`*args, **kwargs`传递给闭包。这些`*args, **kwargs`用于计算第 10 行中`func(*args, **kwargs)`的`result`。

`*args`和`**kwargs`的灵活性使得`timefunc`能够在几乎任何函数上工作。我们闭包的打印语句旨在访问函数的`__name__`、`args`、`kwargs`和`result`，以创建一个有用的计时输出。

### 结论

装饰是一种增强函数行为的强大工具。通过编写一个计时装饰器，你可以获得一个优雅、可重用的模式来跟踪函数的运行时间。

随意将`timefunc`复制到你的代码库中，或者你可以尝试编写自己的计时装饰器！

**[爱德华·克鲁格](https://www.linkedin.com/in/edkrueger/)** 是 Business Laboratory 的高级数据科学家和技术主管，并且是德克萨斯大学奥斯汀分校麦库姆斯商学院的讲师。

**[道格拉斯·弗兰克林](https://www.linkedin.com/in/dougaf/)** 是德克萨斯大学奥斯汀分校麦库姆斯商学院的教学助理。

[原文](https://towardsdatascience.com/a-simple-way-to-time-code-in-python-a9a175eb0172)。经许可转载。

**相关：**

+   数据科学家在 Python 中常犯的 15 个错误（及如何修复）

+   如何使用 Modin 加速 Pandas

+   11 个完整 EDA（探索性数据分析）必备代码块

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关话题

+   [使用 AI 和分析引擎加快时间序列数据准备的方法](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [加速 Python 代码的三种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)

+   [个性化 AI 简单实现：无代码适配 GPTs 指南](https://www.kdnuggets.com/personalized-ai-made-simple-your-no-code-guide-to-adapting-gpts)

+   [在 Python 中正确访问字典的方法](https://www.kdnuggets.com/the-right-way-to-access-dictionaries-in-python)

+   [使用 Pandas 制作美丽互动可视化的最简单方法](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

+   [管理深度学习数据集的新方法](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)
