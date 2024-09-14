# 使用NumExpr包加速你的Numpy和Pandas

> 原文：[https://www.kdnuggets.com/2020/07/speed-up-numpy-pandas-numexpr-package.html](https://www.kdnuggets.com/2020/07/speed-up-numpy-pandas-numexpr-package.html)

[评论](#comments)![图示](../Images/07d9b21986edd14fe9e3deee4350911d.png)

图片来源：[Pixabay](https://pixabay.com/vectors/cheetah-wildcat-fast-speed-spotted-40986/)和作者制作的拼贴图

### 介绍

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT工作

* * *

Numpy和Pandas可能是数据科学（DS）和机器学习（ML）任务中最广泛使用的两个核心Python库。不用多说，评估数值表达式的速度对这些DS/ML任务至关重要，这两个库在这方面表现也不负众望。

在底层，它们使用快速且优化的[向量化](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/VectorizedOperations.html)操作（尽可能多）来加速数学运算。许多文章已经讨论了Numpy如何比普通的Python循环或基于列表的操作要优越得多（尤其是当你可以向量化你的计算时）。

[**Numpy到底有多快？为何如此？**](https://towardsdatascience.com/how-fast-numpy-really-is-e9111df44347)

与标准Python列表的比较

[**使用Python进行数据科学：将你的条件循环转为Numpy向量**](https://towardsdatascience.com/data-science-with-python-turn-your-conditional-loops-to-numpy-vectors-9484ff9c622e)

甚至对条件循环进行向量化，以加速整体数据转换，是值得的。

在这篇文章中，我们展示了如何使用一个名为‘**NumExpr**’的简单扩展库来提升数学运算的速度，这些运算由核心的Numpy和Pandas生成。

### 让我们看看它的实际效果

示例Jupyter笔记本可以在[我的Github仓库中找到](https://github.com/tirthajyoti/Machine-Learning-with-Python/blob/master/Pandas%20and%20Numpy/Speed-up-Numpy-Pandas-with-Numexpr.ipynb)。

### 安装NumExpr库

首先，我们需要确保我们拥有`numexpr`库。所以，正如预期的那样，

```py
pip install numexpr
```

该项目托管在[这里](https://github.com/pydata/numexpr)的Github上。它来自[PyData](https://pydata.org/)稳定版，这是一个隶属于NumFocus的组织，也促成了Numpy和Pandas的诞生。

根据源信息，“*NumExpr 是一个快速的****数值表达式求值器****，用于 NumPy。使用它，操作数组的表达式会得到****加速****，并且使用的****内存更少****，相比于在 Python 中进行相同的计算。此外，它的****多线程能力****可以利用所有核心——这通常会导致相比于 NumPy 有显著的性能提升。*” ([source](https://github.com/pydata/numexpr))

这是[详细文档](https://numexpr.readthedocs.io/projects/NumExpr3/en/latest/)和各种用例示例。

### 简单的向量-标量操作

我们从简单的数学操作开始——将标量数字（比如 1）添加到 Numpy 数组。为了使用 NumExpr 包，我们只需将相同的计算包裹在符号表达式中的特殊方法 `evaluate` 下。以下代码将清楚地说明使用方法，

![](../Images/9151358f63997517985321a60910702c.png)

哇！这真是太神奇了！我们只需将熟悉的 `a+1` Numpy 代码写成符号表达式 `"a+1"` 的形式，并传递给 `ne.evaluate()` 函数。我们得到了显著的速度提升——**从 3.55 毫秒到 1.94 毫秒，平均值**。

请注意，我们在 10 次循环测试中运行了相同的计算 200 次，以计算执行时间。当然，**结果的准确性在一定程度上取决于底层硬件**。欢迎你在你的机器上进行评估，看看你得到了什么改进。

### 涉及两个数组的算术运算

让我们再提升一点，涉及两个数组，可以吗？以下是使用两个数组评估简单线性表达式的代码，

![](../Images/2fb3d0e17369e442cfc07efc77c58aa7.png)

这是计算时间的显著提升，从**11.7 毫秒到 2.14 毫秒，平均值**。

### 涉及更多数组的稍微复杂的操作

现在，让我们进一步提升，涉及更多数组在稍微复杂的有理函数表达式中。假设，我们想评估以下表达式，涉及**五个 Numpy 数组，每个数组有一百万个随机数**（从正态分布中抽取），

![](../Images/4f5a0970dab28c7eb22af18247ceb198.png)

这是代码。我们创建了一个形状为（1000000, 5）的 Numpy 数组，并从中提取了五个（1000000,1）向量以用于有理函数。此外，请注意 NumExpr 方法中的符号表达式如何本地理解‘**sqrt**’（我们只需写`sqrt`）。

![](../Images/f5b8b997d60404918e425a18c47cce92.png)

哇！这显示了一个巨大的速度提升**从 47 毫秒到 ~ 4 毫秒**，平均值。事实上，你会发现**表达式越复杂，涉及的数组越多，Numexpr 的速度提升越显著**！

### 逻辑表达式/布尔过滤

事实证明，我们不仅限于简单的算术表达式，如上所示。Numpy 数组的一个最有用的特性是将它们直接用于涉及逻辑运算符如 `>` 或 `<` 的表达式中，以创建布尔过滤器或掩码。

我们可以使用 NumExpr 来加速筛选过程。这里是一个示例，我们检查涉及 4 个向量的欧几里得距离度量是否大于某个阈值。

![](../Images/a859e7ef111b52dcd51605795a8530e9.png)

这种筛选操作在数据科学/机器学习管道中随处可见，你可以想象通过战略性地将 Numpy 评估替换为 NumExpr 表达式可以节省多少计算时间。

### 复数！

我们可以很容易地从实数域跳到虚数域。NumExpr 对复数同样有效，复数是 Python 和 Numpy 原生支持的。这里是一个示例，也展示了对数等超越操作的使用。

![](../Images/96ff385600bb92277827d427a71f5798.png)

### 数组大小的影响

接下来，我们检查 Numpy 数组大小对速度提升的影响。为此，我们选择一个简单的条件表达式，如 `2*a+3*b < 3.5` 并绘制各种大小下的相对执行时间（在 10 次运行后取平均值）。代码在 Notebook 中，最终结果如下所示，

![图示](../Images/8857e6ead9bf9dc48c62d85461792e88.png)

图像来源：作者代码生成

### Pandas “eval” 方法

这是一个 Pandas 方法，它评估一个 Python 符号表达式（作为字符串）。**默认情况下，它使用 NumExpr 引擎**以实现显著的加速。以下是来自 [官方文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.eval.html)的摘录，

![](../Images/1f6511b840664f4a696b3fc9a2be3994.png)

我们用以下代码展示一个简单示例，其中我们构造了四个各有 50000 行和 100 列的 DataFrame（填充了均匀随机数），并对这些 DataFrame 进行非线性变换——一种使用原生 Pandas 表达式，另一种使用 `pd.eval()` 方法。

![](../Images/09bde7b98a72d185e11a17f783413f79.png)

### DataFrame 大小的影响

我们对 DataFrame 的大小（行数，列数固定为 100）对速度改进的影响进行了类似的分析。结果如下所示，

![图示](../Images/70354a3d16de2fd2df65fb2ce214c53c.png)

图像来源：作者代码生成

### 它的工作原理和支持的运算符

NumExpr 的工作方式的细节有些复杂，涉及到对底层计算架构的优化使用。你可以 [**在这里阅读**](https://numexpr.readthedocs.io/projects/NumExpr3/en/latest/intro.html)。

### 简短说明

基本上，表达式是使用Python的`compile`函数编译的，变量被提取并建立了一个解析树结构。然后将该树编译成一个字节码程序，描述了使用所谓的‘**向量寄存器**’（每个寄存器宽4096个元素）的逐元素操作流。速度提升的关键是Numexpr的**一次处理一块元素的能力**。

![图示](../Images/60845f55358c0a70b6c5736506a98e8d.png)

NumExpr评估流程，来源：作者使用Google Drawing制作

它**跳过了Numpy使用临时数组的做法**，这会浪费内存，并且对于大数组无法放入缓存内存。

此外，**虚拟机完全用C语言编写**，使其比原生Python更快。它还**支持多线程**，允许在合适的硬件上更快地并行处理操作。

### 支持的操作符

NumExpr支持广泛的数学操作符，但不支持条件操作符，如`if`或`else`。[**完整的操作符列表可以在这里找到**](https://numexpr.readthedocs.io/projects/NumExpr3/en/latest/user_guide.html#supported-operators)。

### 线程池配置

你还可以通过设置环境变量`NUMEXPR_MAX_THREAD`来控制用于大数组并行操作的线程数量。目前，最大可能的线程数为64，但超过底层CPU节点上可用的虚拟核心数并没有实际的好处。

### 总结

在本文中，我们展示了如何利用基于虚拟机的特殊表达式评估范式来加速Numpy和Pandas中的数学计算。虽然这种方法可能不适用于所有任务，但数据科学、数据处理和统计建模管道的很大一部分可以在代码变化最小的情况下受益于此。

另外，你可以查看作者的[**GitHub**](https://github.com/tirthajyoti?tab=repositories)** 代码库**以获取机器学习和数据科学的代码、想法和资源。如果你像我一样，对AI/机器学习/数据科学充满热情，请随时[在LinkedIn上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)或[在Twitter上关注我](https://twitter.com/tirthajyotiS)。

**简介: [Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)** 是ON Semiconductor的高级首席工程师。

[原文](https://towardsdatascience.com/speed-up-your-numpy-and-pandas-with-numexpr-package-25bd1ab0836b)。已获许可转载。

**相关:**

+   [使用pdpipe构建Pipelines](/2019/12/build-pipelines-pandas-pdpipe.html)

+   [Dask中的机器学习](/2020/06/machine-learning-dask.html)

+   [Google的新解释性AI服务](/2019/12/googles-new-explainable-ai-service.html)

### 更多相关话题

+   [成为一名出色数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并找到目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的人工智能失败案例，深度剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
