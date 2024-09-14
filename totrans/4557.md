# 如何通过一行代码将 Pandas 提速 4 倍

> 原文：[https://www.kdnuggets.com/2019/11/speed-up-pandas-4x.html](https://www.kdnuggets.com/2019/11/speed-up-pandas-4x.html)

[评论](#comments)

[Pandas](https://pandas.pydata.org/) 是处理 Python 数据的首选库。它易于使用，并且在处理不同类型和大小的数据时相当灵活。它拥有大量的[不同函数](https://dev.pandas.io/docs/user_guide/index.html)，使得数据操作变得轻而易举。

![](../Images/637b258d0b1a651f13343b312131a6fb.png)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

*各种 Python 包的受欢迎程度随时间变化。 [来源](https://stackoverflow.blog/2017/09/14/python-growing-quickly/)*

但有一个缺点：Pandas 对于较大的数据集是*慢*的。

默认情况下，Pandas 以单个进程使用单个 CPU 核心来执行其函数。这对于较小的数据集效果很好，因为你可能不会注意到速度上的太大差异。但对于更大的数据集和更多的计算，当仅使用单个核心时，速度会大幅下降。它一次只能对一个数据集进行计算，而这些数据集可能有*数百万*甚至*数十亿*行。

然而，大多数现代数据科学机器至少有*2 个* CPU 核心。这意味着，在两个 CPU 核心的示例中，默认情况下，计算机的 50% 或更多处理能力在使用 Pandas 时不会被使用。当你使用到 4 核心（现代 Intel i5）或 6 核心（现代 Intel i7）时，情况变得更糟。Pandas 本身并没有设计成能有效利用这些计算能力。

[Modin](https://github.com/modin-project/modin) 是一个新的库，旨在通过自动将计算分配到**系统上所有可用的 CPU 核心**来加速 Pandas。凭借这一点，Modin 声称能够在任何大小的 Pandas DataFrame 上获得[几乎线性的加速](https://modin.readthedocs.io/en/latest/#faster-pandas-even-on-your-laptop)。

让我们看看它是如何工作的，并通过一些代码示例进行演示。

### Modin 如何与 Pandas 一起进行并行处理

给定一个 Pandas DataFrame，我们的目标是以最快的方式对其执行某种计算或处理。这可能是对每一列计算均值，使用*.mean()*，通过*groupby*对数据进行分组，使用*drop_duplicates()*去除所有重复项，或使用其他内置的 Pandas 函数。

在前一部分中，我们提到 Pandas 仅使用一个 CPU 核心进行处理。自然，这成为了一个重大瓶颈，特别是对于较大的 DataFrame，资源不足的问题尤为明显。

理论上，将计算并行化就像在每个可用的 CPU 核心上对不同的数据点应用该计算一样简单。对于 Pandas DataFrame，一个基本的想法是将 DataFrame 划分为几块，块数等于 CPU 核心的数量，然后让每个 CPU 核心在其块上运行计算。最后，我们可以汇总结果，这是一项计算上便宜的操作。

![](../Images/3fa16195a5acf43a3969a8c66c906475.png)

*多核系统如何更快地处理数据。对于单核处理（左），所有 10 个任务都去一个节点。对于双核处理（右），每个节点承担 5 个任务，从而将处理速度加倍。*

这正是 Modin 所做的。它将你的 DataFrame 切分成不同的部分，使每个部分可以发送到不同的 CPU 核心。Modin 在*行*和*列*之间对 DataFrame 进行分区。这使得 Modin 的并行处理[可扩展到任意形状的 DataFrame](https://modin.readthedocs.io/en/latest/architecture.html#dataframe-partitioning)。

想象一下，如果你有一个列很多但行较少的 DataFrame。一些库只在行之间进行分区，这在这种情况下效率较低，因为我们有更多的列而不是行。但使用 Modin，由于分区是在两个维度上进行的，因此并行处理在各种形状的 DataFrame 中都保持高效，无论它们是较宽（列很多）、较长（行很多）还是两者兼具。

![](../Images/5b89c639a28c62e3f40bef41799c69fd.png)

*Pandas DataFrame（左）作为一个块存储，只发送到一个 CPU 核心。Modin DataFrame（右）在行和列之间进行分区，每个分区可以发送到系统中不同的 CPU 核心，最多可达到系统的核心数。*

上图是一个简单的示例。Modin 实际上使用一个*分区管理器*，它可以根据操作的类型改变分区的大小和形状。例如，可能有一个操作需要整个行或整个列。在这种情况下，[分区管理器](https://modin.readthedocs.io/en/latest/architecture.html#partition-manager)会以其能找到的最优方式进行分区和分配到 CPU 核心。它具有灵活性。

为了进行并行处理的繁重工作，Modin可以使用[Dask](https://dask.org/)或[Ray](https://github.com/ray-project/ray/)。它们都是具有Python API的并行计算库，你可以在运行时选择使用其中一个。Ray目前是更安全的选择，因为它更稳定——Dask后端是实验性的。

不过，理论说够了。让我们来看看代码和速度基准测试吧！

### 测试Modin速度

安装和使用Modin的最简单方法是通过pip。以下命令安装Modin、Ray以及所有相关的依赖：

```py
pip install modin[ray]

```

在接下来的示例和基准测试中，我们将使用[*CS:GO Competitive Matchmaking Data*](https://www.kaggle.com/skihikingkevin/csgo-matchmaking-damage)来自Kaggle。CSV的每一行包含了CS:GO竞技比赛中一轮的数据。

目前我们将专注于实验最大的CSV文件（有几个），叫做*esea_master_dmg_demos.part1.csv*，大小为1.2GB。如此大的文件，我们应该能够看到Pandas的性能下降情况以及Modin如何帮助我们。测试时，我将使用一款[i7–8700k CPU](https://ark.intel.com/content/www/us/en/ark/products/126684/intel-core-i7-8700k-processor-12m-cache-up-to-4-70-ghz.html)，它有6个物理核心和12个线程。

我们进行的第一个测试是用我们熟悉的*read_csv()*读取数据。Pandas和Modin的代码完全一样。

为了测量速度，我导入了*time*模块，并在*read_csv()*前后加了*time.time()*。结果是Pandas加载数据从CSV到内存花费了8.38秒，而Modin只用了3.22秒。这是2.6倍的加速。仅仅通过更改导入语句就取得这样的速度提升，算是不小的进步！

让我们对DataFrame进行几个较重的处理。连接多个DataFrame是Pandas中常见的操作——我们可能有几个或更多的CSV文件包含数据，需要逐个读取并连接。我们可以轻松地使用*pd.concat()*函数来完成，Pandas和Modin都能做到。

我们预计Modin在这种操作中表现会很好，因为它处理大量数据。代码如下所示。

在上面的代码中，我们将DataFrame自身连接了5次。Pandas在3.56秒内完成了连接操作，而Modin在0.041秒内完成，速度提升了86.83倍！即使我们只有6个CPU核心，DataFrame的分区也大大提高了速度。

Pandas中常用来清理DataFrame的一个函数是*.fillna()*函数。这个函数查找DataFrame中的所有NaN值，并用你选择的值替换它们。这里有很多操作。Pandas必须遍历每一行每一列来查找NaN值并进行替换。这是应用Modin的绝佳机会，因为我们重复执行了很多简单操作。

这次，Pandas 执行* .fillna()* 用了 1.8 秒，而 Modin 只用了 0.21 秒，实现了 8.57 倍的加速！

### 警告和最终基准

那么，Modin 总是这么快吗？

嗯，不总是这样。

有些情况下，Pandas 实际上比 Modin 更快，即使是在这个有 5,992,097（几乎 600 万）行的大数据集上。下表显示了我运行的一些实验中 Pandas 和 Modin 的运行时间。

正如你所见，有些操作中 Modin 明显更快，通常是在读取数据和查找值时。其他操作，如进行统计计算，在 Pandas 中要快得多。

### 使用 Modin 的实用技巧

Modin 仍然是一个相对年轻的库，正在不断开发和扩展。因此，并不是所有 Pandas 函数都已完全加速。如果你尝试使用一个尚未加速的 Modin 函数，它将默认回退到 Pandas，因此不会有代码错误或故障。有关 Modin 支持的 Pandas 方法的完整列表，请参见 [此页面](https://modin.readthedocs.io/en/latest/UsingPandasonRay/dataframe_supported.html)。

默认情况下，Modin 将使用你机器上的所有 CPU 核心。在某些情况下，你可能希望限制 Modin 可以使用的 CPU 核心数量，尤其是当你想将计算能力用于其他地方时。我们可以通过 Ray 的初始化设置限制 Modin 可以访问的 CPU 核心数量，因为 Modin 在后台使用了 Ray。

```py
import ray
ray.init(num_cpus=4)
import modin.pandas as pd

```

在处理大数据时，数据集的大小超过系统内存（RAM）的情况并不少见。Modin 有一个特定的标志，我们可以将其设置为*true*，以启用其 *out of core* 模式。Out of core 基本上意味着 Modin 会将你的磁盘用作内存的溢出存储，允许你处理比 RAM 大得多的数据集。我们可以设置以下环境变量来启用此功能：

```py
export MODIN_OUT_OF_CORE=true

```

### 结论

这样你就了解了！这是使用 Modin 加速 Pandas 函数的指南。只需更改导入语句即可完成，操作非常简单。希望你在至少一些情况下发现 Modin 对加速你的 Pandas 函数有用。

**相关：**

+   [初学者的数据清理和预处理](https://www.kdnuggets.com/2019/11/data-cleaning-preprocessing-beginners.html)

+   [Pandas 的 5 个高级功能及其使用方法](https://www.kdnuggets.com/2019/10/5-advanced-features-pandas.html)

+   [10 个简单技巧加速你的 Python 数据分析](https://www.kdnuggets.com/2019/07/10-simple-hacks-speed-data-analysis-python.html)

### 更多相关话题

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [成为伟大数据科学家所需的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [加速你的 Python 代码的 3 种简单方法](https://www.kdnuggets.com/2022/10/3-simple-ways-speed-python-code.html)
