# 机器学习不像你的大脑 第四部分：神经元表示精确值的能力有限

> 原文：[`www.kdnuggets.com/2022/06/machine-learning-like-brain-part-4-neuron-limited-ability-represent-precise-values.html`](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-4-neuron-limited-ability-represent-precise-values.html)

![机器学习不像你的大脑 第四部分：神经元表示精确值的能力有限](img/dd22cb4b28766ee141a8fb9838e52253.png)

图片来源：[camilo jimenez](https://unsplash.com/@camstejim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 通过 [Unsplash](https://unsplash.com/s/photos/neuron%E2%80%99s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

机器学习算法依赖于它们以高分辨率和准确性表示数字的能力。这在生物神经元中很困难或不可能实现。此外，所需的准确性越高，基于神经元的系统运行速度就会越慢。任何生物大脑要实现机器学习所需的数值精度都会太慢，无法发挥作用。

在计算机或你的大脑中，神经元和晶体管都以数字方式表示信息——神经元通过发射脉冲，晶体管通过接受两种定义的电压状态之一来表示。鉴于这些数字组件，有多种方式来编码数值：

1.  单个串行信号中的脉冲频率（位数）；

1.  单个信号中的脉冲时序；

1.  一些并行信号的编码；

1.  一些更复杂的编码方案，如二进制整数、浮点数或 Unicode。虽然从理论上讲，神经脉冲流可能编码二进制数字或 Unicode 字符串，但这种可能性极其微小。

前三种方法确实出现在你的神经系统中，它们在计算机系统中也存在。

# 方法 1

在大脑中，数值可以通过在给定时间段内的神经脉冲数量来表示。但要记住，神经元的速度非常慢，脉冲的最大频率约为 250Hz 或 4 毫秒。如果我们想表示 0-10，我们可以分配一个 40 毫秒的时间段。如果在该时间段内出现 10 个脉冲，这可以表示 10，而没有脉冲可以表示零，等等。

这种方法的一些问题：

1.  你不能有分数脉冲，因此在 40 毫秒内，你永远无法表示超过 11 个不同的值；

1.  如果你想表示 100 个不同的值，你必须等待 400 毫秒（将近半秒钟）来知道你表示了哪个值——这太慢，无法有效使用，因为任何复杂的神经过程都需要多个处理层级；

1.  要处理更大的值数量，你需要逐渐更小的突触权重，且大脑内部的电子噪声水平会成为一个问题；

1.  对于一个波动的值，你只能在时间段结束时知道新的值。换句话说，一个大部分时间为零而 20ms 为 10 的信号将被记录为 5，因为在 40ms 的时间段内只会有 5 次脉冲。

# 方法 2

在这种方法中，我们不再计数给定时间段内的脉冲数量，而是检查相邻脉冲之间的时间。许多外周神经在更大的刺激下发射速度更快。例如，一些视网膜神经在更亮的光线下发射速度更快。由于最快的发射速率是每 4ms，我们可以让它代表 10，每 5ms 发射可以代表 9，等等。现在我们可以在 14ms 内表示相同的 11 个值，而不是 40ms。神经元实际上在这种区分上非常出色。例如，大脑以亚毫秒精度检测声音方向的能力依赖于信号到达时间差的精确区分。

你可能会认为可以说`4ms`代表一个值，而`4.01ms`（例如）代表另一个值，并且可以获得任何期望的精度。不幸的是，这并不起作用，因为老问题——噪音。大脑是一个电气噪声环境，神经信号可能会在毫秒级别上抖动。

让我们看看这种信号的接收端。通过调整神经元模型的各种参数，单个神经元可以响应任何特定的发射时机。这意味着要区分 10 种不同的信号时机，你需要 10 个神经元。然而，神经元对信号的相对速度差异检测具有相当高的准确性。这意味着，虽然这种信号对于检测两个不同亮度级别之间的边界等相对值非常有用，但不能用于检测绝对信号值。

另一个问题是，尽管个别大脑神经元可以检测这种类型的信号，但没有办法生成这种信号。神经元生成`6ms`与`7ms`脉冲间隙的唯一方法是接收这种输入信号。这使得对这种类型的信号进行大脑计算变得极其复杂。

# 方法 3

考虑一个神经元簇来表示一个信号。发射的神经元数量越多，值越高。有趣的是，人类触觉敏感性使用了这种机制。因此，你的指尖按压得越用力，感觉神经的发射数量就越多。这有利于在一个发射时间内表示任何数量的值，但实际的限制是它使用了大量神经元。要表示一个 1000x1000 的视觉数组，其中每个可能是 1000 种颜色之一，这将需要十亿个神经元。由于人类视觉皮层中只有 1.4 亿个神经元，这种机制的实用性有限。

# 结论

神经元放电所能表示的最大值数量在 10 到 100 个独特值之间。机器学习算法需要比这更高的精度，因为梯度下降的基本概念假设存在连续的梯度面。你需要在大脑中表示的信息值越多，运行速度就越慢。

> 在本系列的第五部分中，我们将探讨为什么神经元无法执行机器学习所需的最简单求和操作。

**[查尔斯·西蒙](https://futureai.guru/Founder.aspx)** 是一位全国知名的企业家和软件开发者，同时也是 FutureAI 的首席执行官。西蒙是《计算机会反叛吗？：为人工智能的未来做准备》的作者，并且是 Brain Simulator II 的开发者，这是一个 AGI 研究软件平台。欲了解更多信息，[请访问这里](https://futureai.guru/Founder.aspx)。

### 主题更多内容

+   [机器学习并不像你的大脑 第六部分：……精确突触权重的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [机器学习并不像你的大脑 第四部分：神经元很慢……](https://www.kdnuggets.com/2022/04/machine-learning-like-brain-part-one-neurons-slow-slow-slow.html)

+   [机器学习并不像你的大脑 第二部分：感知机与神经元](https://www.kdnuggets.com/2022/05/machine-learning-like-brain-part-two-perceptrons-neurons.html)

+   [机器学习并不像你的大脑 第三部分：基本架构](https://www.kdnuggets.com/2022/06/machine-learning-like-brain-part-3-fundamental-architecture.html)

+   [机器学习并不像你的大脑 第五部分：生物神经元……](https://www.kdnuggets.com/2022/07/machine-learning-like-brain-part-5-biological-neurons-cant-summation-inputs.html)

+   [机器学习并不像你的大脑 第七部分：神经元……](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-seven-neurons-good.html)
