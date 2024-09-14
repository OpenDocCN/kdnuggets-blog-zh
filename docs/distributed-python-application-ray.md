# 使用 Ray 编写你的第一个分布式 Python 应用程序

> 原文：[https://www.kdnuggets.com/2021/08/distributed-python-application-ray.html](https://www.kdnuggets.com/2021/08/distributed-python-application-ray.html)

[评论](#comments)

**由 [Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/)，数据科学专家**

![Ray 使并行和分布式计算的工作更像你所期望的那样](../Images/538062b0a78f2d34aef3c6188ad7cecb.png)

Ray 使并行和分布式计算的工作更像你所期望的那样 ([图片来源](https://www.reddit.com/r/aww/comments/2oagj8/multithreaded_programming_theory_and_practice/))

[Ray](https://docs.ray.io/en/master/) 是一个快速、简单的分布式执行框架，便于扩展应用程序并利用最先进的机器学习库。使用 Ray，你可以将顺序运行的 Python 代码转变为分布式应用程序，几乎不需要修改代码。

本教程的目标是探讨以下内容：

+   为什么你应该使用 Ray 进行并行化和分布式

+   如何开始使用 Ray

+   分布式计算中的权衡（计算成本、内存、I/O 等）

## 为什么你应该使用 Ray 进行并行化和分布式？

正如 [之前的文章指出](https://towardsdatascience.com/modern-parallel-and-distributed-python-a-quick-tutorial-on-ray-99f8d70369b8)，并行和分布式计算是现代应用程序的基础。问题是，将现有 Python 代码并行化或分布式化可能意味着重写现有代码，有时甚至是从头开始。此外，现代应用程序有现有模块如 [multiprocessing](https://docs.python.org/3/library/multiprocessing.html) 缺乏的要求。这些要求包括：

+   在多台机器上运行相同的代码

+   构建具有状态并能够通信的微服务和角色

+   机器故障和抢占的优雅处理

+   大对象和数值数据的高效处理

Ray 库满足这些需求，并允许你在不重写应用程序的情况下进行扩展。为了简化并行和分布式计算，Ray 将函数和类转化为分布式环境中的任务和角色。这个教程的其余部分将深入探讨这些概念以及在构建并行和分布式应用程序时需要考虑的一些重要事项。

![Ray 生态系统](../Images/a219e5df972dc5e1ae9dd250a6ee0a3c.png)

虽然本教程探讨了 Ray 如何使并行化普通 Python 代码变得容易，但需要注意的是，Ray 及其生态系统也使得并行化现有库变得容易，例如 [scikit-learn](https://medium.com/distributed-computing-with-ray/how-to-speed-up-scikit-learn-model-training-aaf17e2d1e1)、 [XGBoost](https://www.anyscale.com/blog/distributed-xgboost-training-with-ray)、 [LightGBM](https://www.anyscale.com/blog/introducing-distributed-lightgbm-training-with-ray)、 [PyTorch](https://medium.com/pytorch/getting-started-with-distributed-machine-learning-with-pytorch-and-ray-fd83c98fdead)等。

## 如何开始使用 Ray

### 将 Python 函数转换为远程函数（Ray 任务）

Ray 可以通过 pip 安装。

```py
pip install 'ray[default]'
```

让我们通过创建 Ray 任务来开始 Ray 之旅。这可以通过用 @ray.remote 装饰一个普通的 Python 函数来完成。这将创建一个可以在你的笔记本电脑的 CPU 核心（或 Ray 集群）上调度的任务。

考虑下面的两个函数，它们生成斐波那契数列（一个整数序列，其特征是每个数都是前两个数的和）。第一个是普通的 Python 函数，第二个是 Ray 任务。

```py
import os
import time
import ray

# Normal Python
def fibonacci_local(sequence_size):
    fibonacci = []
    for i in range(0, sequence_size):
        if i < 2:
            fibonacci.append(i)
            continue
        fibonacci.append(fibonacci[i-1]+fibonacci[i-2])
    return sequence_size

# Ray task
@ray.remote
def fibonacci_distributed(sequence_size):
    fibonacci = []
    for i in range(0, sequence_size):
        if i < 2:
            fibonacci.append(i)
            continue
        fibonacci.append(fibonacci[i-1]+fibonacci[i-2])
    return sequence_size
```

关于这两个函数，有几点需要注意。首先，它们是相同的，只是在 fibonacci_distributed 函数上有 @ray.remote 装饰器。

第二点需要注意的是返回值较小。它们返回的不是斐波那契数列本身，而是序列大小，这是一个整数。这一点很重要，因为这可能会降低分布式函数的价值，设计时需要考虑它是否需要或返回大量数据（参数）。工程师通常将此称为分布式函数的输入/输出（IO）。

### 比较本地与远程性能

本节中的函数将帮助我们比较生成多个长斐波那契数列所需的时间，无论是在本地还是并行处理。值得注意的是，下面的两个函数都使用了 os.cpu_count()，它返回系统中的 CPU 数量。

```py
os.cpu_count()
```

![os cpucount](../Images/e670d8f5a15c37da40e802f8614feb32.png)

本教程中使用的机器有八个 CPU，这意味着下面的每个函数将生成 8 个斐波那契数列。

```py
# Normal Python
def run_local(sequence_size):
    start_time = time.time()
    results = [fibonacci_local(sequence_size) for _ in range(os.cpu_count())]
    duration = time.time() - start_time
    print('Sequence size: {}, Local execution time: {}'.format(sequence_size, duration))

# Ray
def run_remote(sequence_size):
    # Starting Ray
    ray.init()
    start_time = time.time()
    results = ray.get([fibonacci_distributed.remote(sequence_size) for _ in range(os.cpu_count())])
    duration = time.time() - start_time
    print('Sequence size: {}, Remote execution time: {}'.format(sequence_size, duration))
```

在深入了解 run_local 和 run_remote 的代码之前，让我们先运行这两个函数，看看生成多个 100000 数字的斐波那契数列在本地和远程的时间差异。

```py
run_local(100000)
run_remote(100000)
```

![first distributed run_local run_remote](../Images/9005332b15106e8cb059a1e24ef9f1db.png)

run_remote 函数将计算并行化到多个 CPU 上，从而减少了处理时间（1.76 秒对比 4.20 秒）。

### Ray API

为了更好地理解为何 run_remote 更快，我们简要回顾一下代码，并解释 Ray API 的工作原理。

![run_remote yellow](../Images/88beaa9a9ad3a237ea935e57ceabfea0.png)

`ray.init()` 命令启动所有相关的 Ray 进程。默认情况下，Ray 每个 CPU 核心创建一个工作进程。如果你想在集群上运行 Ray，你需要传入一个集群地址，例如 `ray.init(address= 'InsertAddressHere')`。

![run_remote remote fibonacci_distributed.remote](../Images/bda15b087909e40f3071fa0c9c29480d.png)

`fibonacci_distributed.remote(100000)`

![fibonacci_distributed.remote(100000)](../Images/79635aab3b742035a45bb27dcd41d70a.png)

调用 `fibonacci_distributed.remote(sequence_size)` 会立即返回一个未来对象，而不是函数的返回值。实际的函数执行将会在后台进行。由于它立即返回，每个函数调用可以并行执行。这使得生成那些多个 100000 长的 Fibonacci 序列所需的时间更少。

![ray.get](../Images/946c3b656a4b0cea11163461bf11387c.png)

![ray get results](../Images/98aa587fb84451107d8d06cc0266bef5.png)

`ray.get` 在任务完成时检索结果值。

最后，重要的是要注意，当调用 `ray.init()` 的进程终止时，Ray 运行时也将终止。注意，如果你尝试多次运行 `ray.init()`，你可能会遇到 RuntimeError（也许你意外地调用了 `ray.init` 两次？）。可以通过使用 `ray.shutdown()` 来解决这个问题。

```py
# To explicitly stop or restart Ray, use the shutdown API
ray.shutdown()
```

### Ray 仪表盘

Ray 附带一个仪表盘，你可以在调用 `ray.init` 函数后通过 http://127.0.0.1:8265 访问。

在 [其他内容](https://docs.ray.io/en/master/ray-dashboard.html#ray-dashboard) 中，仪表盘允许你：

+   了解 Ray 内存利用情况并调试内存错误。

+   查看每个演员的资源使用情况、已执行的任务、日志等。

+   查看集群指标。

+   终止演员并分析你的 Ray 作业。

+   一目了然地查看错误和异常。

+   在单一界面中查看多个机器的日志。

+   查看 [Ray Tune](https://docs.ray.io/en/master/tune/index.html) 作业和试验信息。

下面的仪表盘显示了在运行 `run_remote(200000)` 后每个节点和每个工作者的资源利用情况。注意仪表盘显示了每个工作者正在运行的 `fibonacci_distributed` 函数。观察你的分布式函数运行的情况是个好主意。这样，如果你发现一个工作者在做所有的工作，你可能是不正确使用了 `ray.get` 函数。另外，如果你发现你的总 CPU 利用率接近 100%，你可能做得有些过头了。

[![ray dashboard 8 core](../Images/ceb59b8475e95e39799f0d5ec6b35100.png)](https://images.ctfassets.net/xjan103pcp94/7slD3bTCoh7Wsd0LIScROV/29127717361206b65fab68981b56e59d/8CoreMichael.png)

## 分布式计算中的权衡

本教程使用了斐波那契序列，因为它们提供了调整计算和IO的多种选项。你可以通过增加或减少序列大小来改变每个函数调用所需的计算量。序列大小越大，生成序列所需的计算量就越多，而序列大小越小，则所需的计算量越少。如果你分布的计算量过小，Ray的开销将主导总处理时间，你将无法从分布函数中获得任何价值。

在分布函数时，IO也至关重要。如果你修改这些函数以返回它们计算的序列，随着序列大小的增加，IO也会增加。在某些时候，传输数据所需的时间将主导完成多个分布函数调用所需的总时间。如果你在集群中分布函数，这一点尤其重要。这将需要使用网络，而网络调用比本教程中使用的进程间通信更昂贵。

因此，建议你尝试实验分布式斐波那契函数和本地斐波那契函数。尝试确定从远程函数中获益所需的最小序列大小。一旦你搞清楚了计算量，就可以调整IO以观察整体性能的变化。无论你使用什么工具，分布式架构在不需要移动大量数据时效果最佳。

幸运的是，Ray的一个主要优点是能够远程维护整个对象。这有助于缓解IO问题。接下来我们来看看这个问题。

## 远程对象作为Actors

正如Ray将Python函数转化为分布式任务一样，Ray将Python类转化为分布式的actor。Ray提供了actor，以便你可以并行化一个类的实例。从代码角度看，你只需在Python类中添加`@ray.remote`装饰器，即可将其转化为一个actor。当你创建该类的一个实例时，Ray会创建一个新的actor，这是一个在集群中运行的进程，并持有对象的副本。

由于它们是远程对象，它们可以持有数据，并且它们的方法可以操作这些数据。这有助于减少进程间通信。如果你发现自己编写了太多返回数据的任务，而这些数据又被发送到其他任务中，请考虑使用actor。

现在，让我们看一下下面的actor。

```py
from collections import namedtuple
import csv
import tarfile
import time

import ray

@ray.remote
class GSODActor():

    def __init__(self, year, high_temp):
        self.high_temp = float(high_temp)
        self.high_temp_count = None
        self.rows = []
        self.stations = None
        self.year = year

    def get_row_count(self):
        return len(self.rows)

    def get_high_temp_count(self):
        if self.high_temp_count is None:
            filtered = [l for l in self.rows if float(l.TEMP) >= self.high_temp]
            self.high_temp_count = len(filtered)
        return self.high_temp_count

    def get_station_count(self):
        return len(self.stations)

    def get_stations(self):
        return self.stations

    def get_high_temp_count(self, stations):
        filtered_rows = [l for l in self.rows if float(l.TEMP) >= self.high_temp and l.STATION in stations]
        return len(filtered_rows)

    def load_data(self):
        file_name = self.year + '.tar.gz'
        row = namedtuple('Row', ('STATION', 'DATE', 'LATITUDE', 'LONGITUDE', 'ELEVATION', 'NAME', 'TEMP', 'TEMP_ATTRIBUTES', 'DEWP',
                                 'DEWP_ATTRIBUTES', 'SLP', 'SLP_ATTRIBUTES', 'STP', 'STP_ATTRIBUTES', 'VISIB', 'VISIB_ATTRIBUTES',
                                 'WDSP', 'WDSP_ATTRIBUTES', 'MXSPD', 
                                 'GUST', 'MAX', 'MAX_ATTRIBUTES', 'MIN', 'MIN_ATTRIBUTES', 'PRCP',
                                 'PRCP_ATTRIBUTES', 'SNDP', 'FRSHTT'))

        tar = tarfile.open(file_name, 'r:gz')
        for member in tar.getmembers():
            member_handle = tar.extractfile(member)
            byte_data = member_handle.read()
            decoded_string = byte_data.decode()
            lines = decoded_string.splitlines()
            reader = csv.reader(lines, delimiter=',')

            # Get all the rows in the member. Skip the header.
            _ = next(reader)
            file_rows = [row(*l) for l in reader]
            self.rows += file_rows

        self.stations = {l.STATION for l in self.rows}
```

上述代码可用于加载和操作一个名为全球表面日总结（GSOD）的公共数据集。该数据集由国家海洋和大气管理局（NOAA）管理，并且可以在其 [网站](https://www.ncei.noaa.gov/data/global-summary-of-the-day/archive/) 上免费获取。NOAA目前维护来自全球超过9000个站点的数据，GSOD数据集包含这些站点的每日总结信息。从1929年到2020年，每年都有一个gzip文件。对于本教程，你只需下载 [1980](https://www.ncei.noaa.gov/data/global-summary-of-the-day/archive/1980.tar.gz) 和 [2020](https://www.ncei.noaa.gov/data/global-summary-of-the-day/archive/2020.tar.gz) 的文件。

这次actor实验的目标是计算1980年和2020年中有多少次读数达到100度或更高，并确定2020年是否有比1980年更多的极端温度。为了实现公平比较，只应考虑1980年和2020年都存在的站点。因此，这次实验的逻辑如下：

+   加载1980年的数据。

+   加载2020年的数据。

+   获取1980年存在的站点列表。

+   获取2020年存在的站点列表。

+   确定站点的交集。

+   获取1980年期间交集站点中100度或以上的读数数量。

+   获取2020年期间交集站点中100度或以上的读数数量。

+   打印结果。

问题在于，这种逻辑完全是顺序的；一个事情只能在另一个事情之后发生。使用Ray后，这种逻辑可以在并行中完成。

下表展示了更具并行性的逻辑。

![Ray Actor Logic](../Images/2cd485e5f4ded37e80278cefebff12fe.png)

以这种方式编写逻辑是确保你以可并行的方式执行所有操作的绝佳方法。以下代码实现了这一逻辑。

```py
# Code assumes you have the 1980.tar.gz and 2020.tar.gz files in your current working directory.
def compare_years(year1, year2, high_temp):

    # if you know that you need fewer than the default number of workers,
    # you can modify the num_cpus parameter
    ray.init(num_cpus=2)

    # Create actor processes
    gsod_y1 = GSODActor.remote(year1, high_temp)
    gsod_y2 = GSODActor.remote(year2, high_temp)

    ray.get([gsod_y1.load_data.remote(), gsod_y2.load_data.remote()])

    y1_stations, y2_stations = ray.get([gsod_y1.get_stations.remote(),
               	                    gsod_y2.get_stations.remote()])

    intersection = set.intersection(y1_stations, y2_stations)

    y1_count, y2_count = ray.get([gsod_y1.get_high_temp_count.remote(intersection),
                                  gsod_y2.get_high_temp_count.remote(intersection)])

    print('Number of stations in common: {}'.format(len(intersection)))
    print('{} - High temp count for common stations: {}'.format(year1, y1_count))
    print('{} - High temp count for common stations: {}'.format(year2, y2_count))

#Running the code below will output which year had more extreme temperatures
compare_years('1980', '2020', 100)
```

![compare years](../Images/5776d72c988f49612a47db2d6ea76343.png)

关于上面的代码有几点重要事项需要提及。首先，将`@ray.remote`装饰器放置在类级别，使所有类方法都可以被远程调用。其次，上面的代码使用了两个actor进程（gsod_y1和gsod_y2），这些进程可以并行执行方法（尽管每个actor每次只能执行一个方法）。这使得1980年和2020年的数据能够同时加载和处理。

## 结论

[Ray](https://github.com/ray-project/ray) 是一个快速、简单的分布式执行框架，使得扩展应用程序并利用最先进的机器学习库变得容易。这个教程展示了如何使用 Ray 轻松将现有的顺序 Python 代码转变为分布式应用程序，几乎无需更改代码。虽然这里的实验都在同一台机器上进行，但 [Ray 也使得在每个主要云服务提供商上扩展 Python 代码变得容易](https://towardsdatascience.com/how-to-scale-python-on-every-major-cloud-provider-5e5df3e88274)。如果你对 Ray 感兴趣，可以查看 [Ray 项目在 GitHub 上](https://github.com/ray-project/ray)，关注 [@raydistributed 的推特](https://twitter.com/raydistributed)，并注册 [Ray 新闻通讯](https://anyscale.us5.list-manage.com/subscribe?u=524b25758d03ad7ec4f64105f&id=d94e960a03)。

**简介: [Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/)** 是一名数据科学专家，现任 Anyscale 开发者关系专员。

[原文](https://www.anyscale.com/blog/writing-your-first-distributed-python-application-with-ray)。经许可转载。

**相关：**

+   [成功的数据科学职业建议](./2020/03/advice-successful-data-science-career.html)

+   [Dask 和 Pandas：没有过多的数据](./2021/03/dask-pandas-data.html)

+   [加速 Scikit-Learn 模型训练](./2021/03/speed-up-scikit-learn-model-training.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 工作

* * *

### 更多相关内容

+   [本周提升搜索应用的8种方法](https://www.kdnuggets.com/2022/09/corise-8-ways-improve-search-application-week.html)

+   [RAG vs 微调：哪种工具最能提升您的 LLM 应用？](https://www.kdnuggets.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application)

+   [创建一个用于从音频中提取主题的 Python 网络应用](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [使用 Google Earth 在 Python 中构建地理空间应用](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [用 Python 轻松构建 AI 应用的10个步骤](https://www.kdnuggets.com/build-an-ai-application-with-python-in-10-easy-steps)

+   [数据网格及其分布式数据架构](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)
