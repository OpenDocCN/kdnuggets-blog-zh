# 如何计算算法效率

> 原文：[https://www.kdnuggets.com/2022/09/calculate-algorithm-efficiency.html](https://www.kdnuggets.com/2022/09/calculate-algorithm-efficiency.html)

![如何计算算法效率](../Images/81f2044792ac245d60f88e2c0c319390.png)

图片由[DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/ai?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

几乎所有形式的技术都使用算法来执行复杂的功能，从搜索引擎到移动应用程序，再到视频游戏和社交媒体。然而，在[创建算法](/2019/03/designing-ethical-algorithms.html)时，重要的是使其尽可能简化，以免对运行它的软件或设备造成负担。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

在这篇文章中，我们将讨论如何计算算法效率，重点介绍两种主要的测量方法，并提供计算过程的概述。

# 什么是算法效率，为什么它很重要？

算法效率与计算机处理算法所需的资源量有关。需要确定算法的效率，以确保它在不发生崩溃或严重延迟的风险下能够运行。如果算法效率不高，它不太可能适合其目的。

我们通过处理算法所使用的资源量来衡量算法的效率。高效的算法使用最少的资源来执行其功能。

算法可以用于各种功能，包括机器学习算法、对抗网络犯罪和使互联网更安全。当你浏览网页时，也应该始终了解[如何保护自己免受身份盗窃](https://www.aura.com/learn/how-to-protect-yourself-from-identity-theft)及其他犯罪活动。

# 测量算法效率的主要方法是什么？

本节将讨论计算算法效率的两种主要衡量标准。这些是时间复杂度和空间复杂度。然而，它们之间无法直接比较，因此我们需要考虑两者的组合。

## 空间复杂度

简而言之，空间复杂度指的是在程序执行过程中与函数输入相关的所需内存量，即计算机或设备上所需的内存量。内存类型可能包括寄存器、缓存、RAM、虚拟内存和辅助内存。

在[分析空间复杂度](https://www.baeldung.com/cs/space-complexity)时，你需要考虑四个关键因素：

+   存储算法代码所需的内存

+   输入数据所需的内存

+   输出数据所需的内存（某些算法，例如排序算法，不需要内存来输出数据，通常只是重新排列输入数据）。

+   算法计算时所需的工作空间内存。这可以包括局部变量和任何需要的栈空间。

从数学上讲，空间等于下面两个组件的总和：

+   **变量部分**包括依赖于算法试图解决的问题的结构化变量。

+   **固定部分**指的是与问题无关的部分，包括指令空间、常量空间、固定大小的结构变量和简单变量。

因此，任何算法的空间复杂度 S(a) 计算如下：

S(a) = c（固定部分）+ v(i)（依赖于实例特征 i 的变量部分）

## 时间复杂度

时间复杂度是[执行算法所需的总时间](/2020/06/time-complexity-measure-efficiency-algorithms.html)，它依赖于与空间复杂度相同的所有因素，但这些因素被分解成数值函数。

当比较不同算法时，这一度量方法非常有用，特别是在处理大量数据时。然而，如果数据量较小，比较算法性能时需要更详细的估算。当算法使用并行处理时，时间复杂度的效果较差。

时间复杂度定义为 T(num)。它通过步骤数量来衡量，只要每一步等同于常量时间。

## 算法时间复杂度情况

算法的复杂度由一些关键标准定义，即数据的移动和键的比较——例如，[数据的移动次数](/2021/06/overcoming-simplicity-illusion-data-migration.html)和键的比较次数。测量算法复杂度时，我们使用三种情况：

+   最佳情况的时间复杂度

+   平均情况下的时间复杂度

+   最坏情况的时间复杂度

# 计算算法效率的过程

准确计算算法效率时需要完成两个阶段——理论分析和基准测试（性能测量）。我们将下面提供每个阶段的总结。

## 理论分析

与算法相关的理论分析通常是以渐近方式估计算法复杂度的过程（接近一个值或曲线）。描述算法使用的资源数量最常用的方法是使用Donald Knuth的“大O符号”。

使用[大O符号](https://triplebyte.com/blog/counterpoint-stop-trying-to-force-big-o-into-software-development)，程序员可以测量算法以确保其在不同输入数据规模下都能高效扩展。

## 基准测试（性能测量）

在分析和测试新的算法或软件时，会使用基准测试来衡量其性能。这有助于评估算法与其他表现良好的算法相比的效率。

以排序算法为例。使用先前版本排序算法设置的基准测试可以确定当前算法的效率，利用已知数据同时考虑其功能改进。

基准测试还允许分析团队将算法速度与各种编程语言进行比较，以确定是否可以改进。实际上，基准测试可以以各种方式实施，以衡量性能与任何前身或类似软件的对比。

# 实施挑战

实现新算法有时可能会影响其整体效率。这可能与选择的[编程语言](/2021/05/top-programming-languages.html)、使用的编译器选项、操作系统或算法的编写方式有关。特别是，特定语言使用的编译器可能会对速度产生重大影响。

在测量空间和时间复杂度时，并非所有因素都由程序员决定。诸如缓存局部性和一致性、数据对齐和粒度、多线程以及同时多任务等问题都会影响性能，无论使用何种编程语言或算法编写方式。

运行软件的处理器也可能引发问题；一些处理器可能支持向量或并行处理，而另一些则不支持。在某些情况下，利用这些能力可能并不总是可行，这使得算法效率降低，并需要一些重新配置。

最后，[处理器使用的指令集](/2020/05/tops-just-hype-dark-ai-silicon-disguise.html)（例如，ARM或x86-64）可能会影响指令在各种设备上的处理速度。这使得优化编译器变得困难，因为需要考虑的硬件组合非常多。

# 结论

一个算法需要非常迅速地处理，因此它必须尽可能完美地执行。这就是为什么每个新算法都要经过测试来计算其效率，通过理论分析和基准测试将其与其他算法进行比较。

时间复杂度和空间复杂度是计算算法效率的两个主要指标，决定了在机器上处理算法所需的资源数量。时间复杂度衡量处理算法所需的时间，而空间复杂度衡量所使用的内存量。

不幸的是，在实施过程中可能会遇到许多挑战，例如使用的编程语言、处理器、指令集等，这些都会给程序员带来头疼的问题。

尽管如此，时间复杂度和空间复杂度已被证明是非常[有效的测量方法](https://byjusexamprep.com/efficiency-of-an-algorithm-i)。

**[Nahla Davies](http://nahlawrites.com/)** 是一位软件开发者和技术作家。在全职从事技术写作之前，她曾担任 Inc. 5,000 的一家体验品牌机构的首席程序员，该机构的客户包括三星、时代华纳、Netflix 和索尼。

### 更多相关话题

+   [计算深度学习模型的计算效率…](https://www.kdnuggets.com/2023/06/calculate-computational-efficiency-deep-learning-models-flops-macs.html)

+   [效率是生物神经元与…的区别所在](https://www.kdnuggets.com/2022/11/efficiency-spells-difference-biological-neurons-artificial-counterparts.html)

+   [提升数学效率：如何处理 Numpy 数组操作](https://www.kdnuggets.com/elevate-math-efficiency-navigating-numpy-array-operations)

+   [通过 ChatGPT 最大化数据分析效率](https://www.kdnuggets.com/maximizing-efficiency-in-data-analysis-with-chatgpt)

+   [3 种基于研究的高级提示技术提升 LLM 效率…](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [5 个提升 Python 数据效率和速度的小贴士](https://www.kdnuggets.com/5-python-tips-for-data-efficiency-and-speed)
