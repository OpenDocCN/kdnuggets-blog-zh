# 加速算法：设计、算法选择和实现中的注意事项

> 原文：[https://www.kdnuggets.com/2017/12/accelerating-algorithms-design-choice-implementation.html](https://www.kdnuggets.com/2017/12/accelerating-algorithms-design-choice-implementation.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2017/12/accelerating-algorithms-design-choice-implementation.html/#comments)

![](../Images/546d443636f0c7cf6486c8f12e2311d8.png)

**由 [Tom Radcliffe](https://www.activestate.com/blog/authors/tomr)**。

追求速度是计算中的少数不变因素之一，这由两个因素驱动：数据量的不断增加和有限的硬件资源。在大数据和物联网时代，我们在这两个方向上都面临压力。幸运的是，我们在分布式和并行计算方面取得了很大进步，但算法层面对原始速度的需求永远不会消失。如果我们能让我们的算法本质上更快，我们将能更好地利用昂贵的硬件，这始终是件好事。

有许多加速算法的方法。一些方法对上下文依赖性强，有些则较为通用。我将探讨整体设计选择、算法选择和实现细节。我的背景和经验主要集中在优化问题和数值算法上，所以这将是主要关注的领域。

传统上，讨论优化时都会提到要谨慎。Knuth的第一条优化法则是：“不要做”而他的第二条类似：“还不要做。”我们还被告知“过早优化是万恶之源”，但什么算作“过早”？何时为时已晚？

我自己的优化法则旨在回答这个问题：“没有量化就没有优化。”我们不应该在没有先测量当前性能和对代码进行性能分析以确定时间消耗最多的地方之前尝试加速算法。正确推断算法花费大部分时间的地方是非常困难的。

比如，我写的一个光学蒙特卡罗模拟程序发现其运行时间有四分之一是用于对接近1的数字进行平方根运算。这是由于在散射事件后重新归一化方向向量的过程。通过为这种特殊情况编写一些高度优化的代码，我能够避免在数学协处理器中使用较慢的通用实现，从而在整体性能上提高了约20%，在运行时间长达数天的情况下，这并非小事。如果没有对代码进行性能分析，我不可能发现这个时间消耗点。

Python中的cProfile和profile模块（它们都是[ActivePython发行版](https://www.activestate.com/activepython "Download and install Python with ActivePython")的一部分）提供了确定性性能分析，使结果相对容易解读。其他语言可能支持基于程序计数器采样的性能分析，由于统计效应，这种方法可能会因运行而异。即使是确定性性能分析，程序的某些部分由于资源限制或代码中的随机数使用，可能会有性能波动。I/O和通信都是外部依赖可能影响性能的领域，因此如果可能的话，最好单独对这些进行性能分析。

这假设我们已有代码来进行性能分析，但算法加速可以在开发过程的多个不同层面进行：设计、算法选择和实现。

**设计**

设计优化是一个庞大的话题，我这里只能简单提及。根据应用领域，有许多不同的高效设计方法。其中最重要的一点是寻找可以代替最终结果的代理计算。

这在处理优化问题时特别有用。经常会看到这样的代码：

X = sqrt(some complicated expression)

取平方根——即便在如今高速数学硬件的时代，也是一个不合理昂贵的操作——除了在优化的最终阶段，表面在最小值附近的曲率可能对算法的效率至关重要外，其他地方不会对计算有任何帮助。这对于任何包裹最终结果的单调函数都是正确的：通常可以省略。

另一种相当通用的设计方法是对数据进行采样，而不是盲目使用每一个可用的数据点。这就是[伪相关](https://www.ncbi.nlm.nih.gov/pubmed/7935212 "https://www.ncbi.nlm.nih.gov/pubmed/7935212")的技巧，这是一种超高速图像配准算法：通过使用小的像素子集来估计交叉相关积分，可以将配准速度提高几个数量级。几年后，互信息算法中也使用了类似的方法。

总体设计问题应该是：“我真的需要计算那个吗？”可能有一些与您关心的结果强相关的东西，但可以更快地计算出来。

**算法选择**

算法的阶数获得了大量学术关注，但意外地并不重要。原因如下：

1.  问题通常具有一定的规模，因此算法的缩放行为并不相关。如果在一千万数据点上运行良好的算法，其在一亿数据点上的性能并不重要，前提是我们从未看到过如此大的数据集。

1.  领先因素很重要：一个 O(N**2) 的算法可能比等效的 O(N*log(N)) 算法具有更小的领先因素，因为其复杂度较低。

1.  说到复杂性，具有更好大 O 性能的算法通常更复杂，使它们更脆弱、难以调试和维护。

最著名的脆弱性案例可能是快速排序，当输入已经排序时，它的复杂度从 O(N*log(N)) 降低到 O(N**2)。这是一个比较容易发现的情况，但更复杂的算法虽然平均性能良好，在特殊情况下仍可能表现不佳，调试这种行为可能很复杂且耗时。因为这种失败通常比较少见，所以在部署后遇到这种“有时运行缓慢”的奇怪情况也是不 uncommon 的。

在选择算法时，最好先实现最简单的东西，然后在现实数据集上量化其性能，并决定是否需要更复杂的算法。通常情况下，可能不需要更复杂的算法。我曾经处理过一些使用 KD 树匹配点的网格配准代码，发现由于数据集相对较小，使用几个易于理解的启发式方法的简单线性搜索可以轻松超越标称上性能更好的 KD 树实现，后者是一个脆弱且复杂的烂摊子，其不可维护性使我不得不回退到更简单的东西，以便理解其运行情况。

**实现细节**

算法不是它的实现。即使是相对简单和高效的算法也可能实现不佳，从而导致性能变慢。不必要的函数调用是一个常见的情况。使用递归而不是循环可能会影响速度。

在许多情况下，高性能代码是用相对底层的语言编写，如 C、C++ 或者 Fortran，并从高层语言中调用。这些调用的顺序和组织方式对性能影响很大：通常来说，跨语言边界的调用是一个代价高昂的操作，因此建议以最小化这种调用的方式来使用接口。

高级语言通常包括一些加速技巧。例如，Python 的 sum() 和 map() 函数在处理数组时比循环快得多。对 100,000,000 个整数进行求和，在我的台式机上需要 4.4 秒，而使用 sum() 对数组求和只需 0.63 秒。此外，循环中的生成器（Python 2 中的 xrange 而不是 range，Python 3 中的 range()）通常比非懒加载的列表创建函数更快。

map() 函数比循环更快：对一个包含 100,000,000 个整数的数组进行平方根计算，在我的台式机上需要 10 秒，而使用 `map(math.sqrt, lstData)` 进行相同操作只需半倍的时间。

**底层方法**

在 Python 中还有更强大的技术，比如使用 [NumPy](https://ipython-books.github.io/featured-01/ "Getting the Best Performance out of NumPy") 和 [Cython](https://cython.org/ "https://cython.org/") 以及 Intel 的数学核心库（MKL），该库可以在 ActiveState 的 [ActivePython](https://www.activestate.com/activepython "Download and install Python with ActivePython") 中找到。虽然可能会有一些限制使得这些技术不能用于特定问题，但它们值得作为“效率武器库”的一部分加以考虑。只需确保首先量化你的算法性能！

要了解更多关于加速算法的信息，并深入了解 Intel 的 MKL 优化，请观看我的网络研讨会，["使用 Python 和 Intel MKL 加速你的算法"](https://www.activestate.com/webinars/accelerating-your-algorithms-production-python-and-intel-mkl "Webinar - Accelerating Your Algorithms with Python and Intel MKL")，由我和 Intel 的软件工程经理 Sergey Maidanov 共同主持。

[原文](https://www.activestate.com/blog/2017/11/accelerating-your-algorithms-considerations-design-algorithm-choice-and-implementation)。已获授权转载。

**相关内容**

+   [**机器学习的鲁棒算法**](https://www.kdnuggets.com/2017/12/robust-algorithms-machine-learning.html)

+   [**从创意到成功数据科学项目的 7 个超级简单步骤**](https://www.kdnuggets.com/2017/11/7-super-simple-steps-idea-successful-data-science-project.html)

+   [**2017年使用的顶级数据科学与机器学习方法**](https://www.kdnuggets.com/2017/12/top-data-science-machine-learning-methods.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 了解更多相关内容

+   [什么时候集成技术会是一个好选择？](https://www.kdnuggets.com/2022/07/would-ensemble-techniques-good-choice.html)

+   [开源工具在加速数据科学进步中的作用](https://www.kdnuggets.com/2023/05/role-open-source-tools-accelerating-data-science-progress.html)

+   [通过集成 Jupyter 和 KNIME 来缩短实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [DeepMind 的 AlphaTensor 的首个开源实现](https://www.kdnuggets.com/2023/03/first-open-source-implementation-deepmind-alphatensor.html)

+   [了解如何设计、测量和实施可靠的A/B测试…](https://www.kdnuggets.com/2023/01/sphere-design-measure-implement-trustworthy-ab-tests-ronny-kohavi.html)

+   [利用人工智能设计公平和公正的电动车充电网络](https://www.kdnuggets.com/leveraging-ai-to-design-fair-and-equitable-ev-charging-grids)
