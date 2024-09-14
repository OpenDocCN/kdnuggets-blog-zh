# 评估 Ray：用于大规模可扩展性的分布式 Python

> 原文：[https://www.kdnuggets.com/2020/03/domino-ray-distributed-python-massive-scalability.html](https://www.kdnuggets.com/2020/03/domino-ray-distributed-python-massive-scalability.html)

**作者 [Dean Wampler](https://twitter.com/deanwampler)，Domino Data Lab**

![图](../Images/c0247476e8e4562f2562f6155f97b3c2.png)

*[Dean](mailto:dean@anyscale.io) [Wampler](https://twitter.com/deanwampler) 提供了 Ray 的精华概述，这是一个将 Python 系统从单台机器扩展到大型集群的开源系统。如果你对更多见解感兴趣，请注册参加即将举行的 [Ray 峰会](https://events.linuxfoundation.org/ray-summit/)。*

### 介绍

本文面向做出技术决策的人，即数据科学团队负责人、架构师、开发团队负责人，甚至是参与组织中技术战略决策的经理。如果你的团队已经开始使用 ​[Ray](https://ray.io/)​，并且你想了解它是什么，这篇文章适合你。如果你想知道 Ray 是否应该成为你的 Python 应用程序，特别是 ML 和 AI 的技术战略的一部分，这篇文章也适合你。如果你想了解 Ray 的更深入技术介绍，请参阅 ​[Ray 项目博客](https://medium.com/distributed-computing-with-ray/ray-for-the-curious-fa0e019e17d3)​ 的这篇文章。

### Ray：扩展 Python 应用程序

Ray 是一个开源系统，用于将 Python 应用程序从单台机器扩展到大型集群。其设计受到下一代 ML 和 AI 系统独特挑战的驱动，但其功能使 Ray 成为所有需要在集群中扩展的基于 Python 的应用程序的绝佳选择，尤其是那些具有分布式状态的应用程序。Ray 还提供了一个最小侵入性和直观的 API，因此你可以在不需要大量分布式系统编程知识和努力的情况下获得这些好处。

开发人员在代码中指明哪些部分应该分布在集群中并异步运行，然后 Ray 会为你处理分布。若在本地运行，应用程序可以利用机器上的所有核心（你也可以指定限制）。当一台机器不够用时，可以很简单地在集群上运行 Ray，使应用程序利用集群。此时唯一需要改变的代码是初始化 Ray 时传递的选项。

使用 Ray 的 ML 库，例如用于强化学习（RL）的 ​[RLlib](https://ray.readthedocs.io/en/latest/rllib.html)​，用于超参数调优的 ​[Tune](https://ray.readthedocs.io/en/latest/tune.html)​，以及用于模型服务（实验性）的 ​[Serve](https://ray.readthedocs.io/en/latest/serve.html)​，都在内部实现了 Ray，以利用其可扩展的分布式计算和状态管理优势，同时提供了针对其服务目标的领域特定 API。

### Ray 的动机：训练强化学习（RL）模型

要了解 Ray 的动机，可以考虑训练强化学习（RL）模型的例子。RL 是一种机器学习方法，最近被用来[to ​击败世界顶级围棋选手](https://deepmind.com/research/case-studies/alphago-the-story-so-far)​，并​实现 Atari 和类似游戏的专家级游戏玩法​。

可扩展的 RL 需要 Ray 设计时提供的许多功能：

+   **高度并行化和高效执行的 *任务***（数百万个或更多） – 训练模型时，我们重复相同的计算以找到最佳模型 *方法*（“超参数”），一旦选择了最佳结构，就找到最佳的模型 *参数*。我们还需要在任务有依赖于其他任务结果的情况下进行适当的任务排序。

+   **自动故障容错** – 由于所有这些任务中可能有一部分因各种原因失败，我们需要一个支持任务监控和故障恢复的系统。

+   **多样化的计算模式** – 模型训练涉及大量的计算数学。特别是大多数 RL 模型训练，还需要高效地执行一个模拟器——例如，我们想要击败的游戏引擎或代表现实世界活动的模型，比如自动驾驶。这些计算模式（算法、内存访问模式等）更典型于通用计算系统，这些系统的计算模式与数据系统中的计算模式（通常以高吞吐量的记录变换和聚合为主）可能大相径庭。另一个区别是这些计算的动态特性。想象一下一个游戏玩家（或模拟器）如何适应游戏的演变状态，改善策略，尝试新战术等。这些多样化的要求出现在各种新型的基于 ML 的系统中，如机器人技术、自动驾驶车辆、计算机视觉系统、自动对话系统等。

+   **分布式状态管理** – 在 RL 中，需要在训练迭代之间跟踪当前模型参数和模拟器状态。由于任务是分布式的，这个状态也变得分布式。适当的状态管理也要求对有状态操作进行适当的排序。

当然，其他 ML/AI 系统也需要一些或所有这些功能。大规模操作的通用 Python 应用程序也是如此。

### Ray 的要点

像 ​RLlib​、​Tune​ 和 ​Serve​ 这样的 Ray 库使用了 Ray，但大多数情况下对用户隐藏了它。然而，使用 Ray API 本身是很简单的。假设你有一个需要在数据记录上重复运行的“昂贵”函数。如果它是 ​无状态的​，即它在调用之间不维护任何状态，并且你想要并行调用它，那么你只需通过添加 `​@ray.remote​` 注解将函数转化为 Ray ​任务​，如下所示：

```py
@ray.remote
def slow(record):
    new_record = expensive_process(record)
    return new_record
```

然后初始化 Ray 并按如下方式对数据集进行调用：

```py
ray.init() # Arguments can specify the cluster location, etc.
futures = [slow.remote(r) for r in records]
```

注意我们如何使用 `slow.remote` 调用 `slow` 函数。每次调用都会立即返回一个 future。我们有一个它们的集合。如果我们在集群中运行，Ray 会管理可用资源，并将任务放在具有运行函数所需资源的节点上。

我们现在可以要求 Ray 在完成每个结果时使用 `ray.wait` 返回结果。这是一种习惯用法：

```py
while len(futures) > 0:
     finished, rest = ray.wait(futures)
     # Do something with “finished”, which has 1 value:
     value = ray.get(finished[0]) # Get the value from the future
     print(value)
     futures = rest 
```

如上所述，我们将等待 `slow` 的调用之一完成，此时 `ray.wait` 将返回两个列表。第一个列表将有一个条目，即完成的 `slow` 调用的 *future* 的 id。我们传入的其他 futures 将在第二个列表中—`rest`。我们调用 `ray.get` 来检索已完成 future 的值。*(注意：这是一个阻塞调用，但由于我们已经知道它完成了，所以它会立即返回。)* 我们通过将列表重置为剩余的项目来完成循环，然后重复直到所有远程调用完成并且结果被处理。

你还可以向 `ray.wait` 传递参数，以便一次返回多个结果并设置超时。如果你不等待一组并发任务，你也可以通过调用 `ray.get(future_id)` 等待特定的未来对象。

没有参数的情况下，`ray.init` 默认本地执行，并使用所有可用的 CPU 核心。你可以提供参数以指定运行的集群、使用的 CPU 或 GPU 核心数量等。

假设一个远程函数传递了另一个远程函数调用的 future。Ray 会自动安排这些依赖关系，以便它们按所需的顺序进行评估。你不需要自己处理这种情况。

假设你有一个 *有状态* 的计算任务要完成。当我们上面使用 `ray.get` 时，我们实际上是从分布式对象存储中检索值。如果你愿意，可以使用 `ray.put` 明确将对象放入其中，它返回一个 id，你可以稍后传递给 `ray.get` 以再次检索。

### 使用演员模型处理有状态计算

Ray 支持一种更直观和灵活的方式来管理状态的设置和检索，采用了演员模型。它使用常规的 Python 类，并通过相同的`@ray.remote`注解将其转换为远程演员。为了简化起见，假设你需要统计 `slow` 被调用的次数。这里是一个实现该功能的类：

```py
@ray.remote
class CountedSlows:
    def __init__(self, initial_count = 0):
        self.count = initial_count
    def slow(self, record):
        self.count += 1
        new_record = expensive_process(record)
        return new_record
    def get_count(self):
        return self.count
```

除了注解之外，这看起来像是一个正常的 Python 类声明，尽管通常你不会单独定义 `get_count` 方法来检索 `count`。我稍后会再提到这一点。

现在以类似的方式使用它。注意类的实例是如何构造的，以及如何像以前一样使用 `remote` 调用实例上的方法：

```py
cs = CountedSlows.remote() # Note how actor construction works
futures = [cs.slow.remote(r) for r in records]

while len(futures) > 0:
     finished, rest = ray.wait(futures)
     value = ray.get(finished[0])
     print(value)
     futures = rest

count_future_id = cs.get_count.remote()
ray.get(count_future_id)
```

最后一行应该打印出一个等于原始集合大小的数字。注意，我调用了方法`​get_count`​来检索​`count`​属性的值。此时，Ray 不支持直接检索像​`count`​这样的实例​*属性*​，因此与常规 Python 类相比，添加检索它的方法是唯一的必要区别。

### Ray 统一了任务和演员

在上述两种情况下，Ray 跟踪任务和演员在集群中的位置，消除了在用户代码中明确知道和管理这些位置的需要。演员内部的状态变更以线程安全的方式处理，无需显式的并发原语。因此，Ray 为应用程序提供了直观的分布式状态管理，这意味着 Ray 是实现 *​有状态的* [serverless](https://en.wikipedia.org/wiki/Serverless_computing) 应用程序的绝佳平台。此外，在同一台机器上任务和演员之间的通信时，状态通过共享内存透明地管理，演员和任务之间进行零拷贝序列化，以实现最佳性能。

**注意：**​让我强调 Ray 在这里提供的一个重要好处。没有 Ray，当你需要在集群上扩展应用程序时，你必须决定创建多少个实例，将它们放置在集群中的哪个位置（或使用像 Kubernetes 这样的系统），如何管理它们的生命周期，它们如何通信和协调等。Ray 为你做了所有这些，几乎不需要你付出额外的努力。你主要只需编写正常的 Python 代码。它是简化微服务架构设计和管理的强大工具。

### 采用 Ray

如果你已经在使用其他并发 API，如 [multiprocessing](https://docs.python.org/3.8/library/multiprocessing.html)、[asyncio](https://docs.python.org/3.8/library/asyncio.html?highlight=asyncio#module-asyncio) 或 [joblib](https://joblib.readthedocs.io/en/latest/)，那么这些 API 在单台机器上扩展效果很好，但它们不支持集群扩展。Ray 最近引入了这些 API 的实验性实现，允许你的应用程序扩展到集群。你代码中唯一需要更改的就是导入语句。例如，如果你使用的是 ​`multiprocessing.Pool`​，这是通常的导入语句：

```py
from multiprocessing.pool import Pool
```

要使用 Ray 实现，请改用以下语句：

```py
from ray.experimental.multiprocessing.pool import Pool
```

这就是全部所需的。

那么 [Dask](https://dask.org/) 呢？它似乎提供了许多与 Ray 相同的功能。如果你需要分布式集合，如 numpy 数组和 Pandas DataFrames，Dask 是一个不错的选择。（一个使用 Ray 的研究项目 [Modin](https://github.com/modin-project/modin) 最终将满足这一需求。）Ray 设计用于更通用的场景，在这些场景中，需要分布式状态管理，并且在大规模上执行异构任务必须非常高效，例如我们在强化学习中需要的那样。

### 结论

我们已经看到 Ray 的抽象和功能使它成为一个易于使用的工具，同时提供强大的分布式计算和状态管理能力。尽管 Ray 的设计受到高性能、高要求 ML/AI 应用程序的特定需求的驱动，但它具有广泛的适用性，甚至提供了一种新的方法来处理基于微服务的架构。

我希望你发现这简短的 Ray 介绍很有趣。请 **试一试** 并告诉我你的想法！发送至： [dean@anyscale.io](mailto:dean@anyscale.io)

### 了解更多

有关 Ray 的更多信息，请查看以下内容：

+   [Ray Summit](https://events.linuxfoundation.org/ray-summit/) 将于 2020 年 5 月 27-28 日在旧金山举行。了解案例研究、研究项目和 Ray 的深入探讨，并聆听数据科学和 AI 社区领导者的晨间主题演讲！

+   [Ray 网站](https://ray.io/) 是所有与 Ray 相关内容的起点。

+   几个基于笔记本的 [Ray 教程](https://github.com/ray-project/tutorial) 让你可以尝试 Ray。

+   [Ray GitHub 页面](https://github.com/ray-project/tutorial) 是你可以找到所有 Ray 源代码的地方。

+   Ray 文档解释了一切： [主页](https://ray.readthedocs.io/en/latest/)， [安装说明](https://ray.readthedocs.io/en/latest/installation.html)。

+   直接向 [Ray Slack 工作区](https://docs.google.com/forms/d/e/1FAIpQLSfAcoiLCHOguOm8e7Jnn-JJdZaCxPGjgVCvFijHB5PLaQLeig/viewform?usp=send_form) 或 [ray-dev Google 组](https://groups.google.com/forum/?nomobile=true#!forum/ray-dev) 提问有关 Ray 的问题。

+   [Ray 的 Twitter 账户](https://twitter.com/raydistributed)。

+   一些 Ray 项目：

    +   [RLlib](https://ray.readthedocs.io/en/latest/rllib.html): 使用 Ray 进行可扩展的强化学习（以及这篇 [RLlib 研究论文](https://arxiv.org/abs/1712.09381)）

    +   [Tune](https://ray.readthedocs.io/en/latest/tune.html): 使用 Ray 进行高效的超参数调优

    +   [Serve](https://ray.readthedocs.io/en/latest/serve.html): 使用 Ray 进行灵活、可扩展的模型服务

    +   [Modin](https://github.com/modin-project/modin): 一个旨在加速 Pandas 的 Ray 研究项目

    +   [FLOW](https://flow-project.github.io/): 一个使用强化学习进行交通控制建模的计算框架

    +   [Anyscale](https://anyscale.io/): Ray 背后的公司

+   若需更多技术细节：

    +   一篇[详细描述Ray系统的研究论文](https://arxiv.org/abs/1712.05889)

    +   一篇[描述Ray内部灵活原语用于深度学习的研究论文](https://pdfs.semanticscholar.org/0e8f/5cd8d8dbbe4a55427e90ed35977e238b1eed.pdf?_ga=2.179761508.1978760042.1576357334-1293756462.1576357334)

    +   [使用Ray的快速序列化](https://ray-project.github.io/2017/10/15/fast-python-serialization-with-ray-and-arrow.html) 和 [Apache Arrow](https://arrow.apache.org/)

[最初发布在Domino数据科学博客上](https://blog.dominodatalab.com/evaluating-ray-distributed-python-for-massive-scalability/)

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

### 更多相关话题

+   [超越准确性：使用NLP测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [评估文档相似性计算方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [数据网格及其分布式数据架构](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)

+   [KDnuggets™ 新闻 22:n07, 2月16日：如何学习机器数学…](https://www.kdnuggets.com/2022/n07.html)

+   [优化LLM的性能和可扩展性](https://www.kdnuggets.com/optimizing-your-llm-for-performance-and-scalability)

+   [优化Python代码性能：深入探讨Python分析工具](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)
