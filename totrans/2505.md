# 机器学习管道的设计模式

> 原文：[https://www.kdnuggets.com/2021/11/design-patterns-machine-learning-pipelines.html](https://www.kdnuggets.com/2021/11/design-patterns-machine-learning-pipelines.html)

[评论](#comments)

**由 [Davit Buniatyan](https://www.linkedin.com/in/davidbuniatyan/)，Activeloop 的首席执行官，一家 Y-Combinator 孵化器的初创公司**。

![机器学习管道的设计模式](../Images/ef7e6722925ec2460275b149d19f7894.png)

机器学习管道的设计模式在过去十年中经历了几次演变。这些变化通常是由于内存和 CPU 性能之间的不平衡所驱动的。它们也不同于传统的数据处理管道（如 MapReduce），因为它们需要支持与深度学习相关的长期运行的、有状态的任务的执行。

随着数据集规模的增长超过内存的可用性，我们看到更多的 ETL 管道采用分布式训练和分布式存储作为首要原则。这些管道不仅可以使用多个加速器以并行方式训练模型，还可以用云对象存储替代传统的分布式文件系统。

与来自 [AI 基础设施联盟](https://ai-infrastructure.org/) 的合作伙伴一起，我们在 Activeloop 积极构建工具，帮助研究人员在任意大的数据集上训练任意大的模型，例如 [用于 AI 的开源数据集格式](https://github.com/activeloopai/Hub)。

### 单机 + 单个 GPU

在 2012 年之前，训练机器学习模型是相对简单的操作。像 ImageNet 和 KITTI 这样的数据集存储在单台机器上，并由单个 GPU 访问。大多数情况下，研究人员可以在不进行预取的情况下获得不错的 GPU 利用率。

在这个时代生活更为轻松：几乎不需要考虑梯度共享、参数服务器、资源调度或多个 GPU 之间的同步。只要任务是 GPU 绑定而不是 IO 绑定，这种简单性意味着第一代深度学习框架如 Caffe 在单台机器上运行通常足以满足大多数机器学习项目的需求。

尽管训练一个模型需要数周时间并不罕见，但[研究人员](https://papers.nips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)开始探索使用多个 GPU 来并行化训练。

### 单机 + 多个 GPU

到 2012 年，更大的数据集如 COCO、更大的模型架构以及 [经验结果](https://papers.nips.cc/paper/2011/hash/218a0aefd1d1a4be65601cc6ddc1520e-Abstract.html) 证明了配置多 GPU 基础设施所需的努力是值得的^([1])。因此，我们看到了像 TensorFlow 和 PyTorch 这样的框架的出现，以及配备多个 GPU 的专用 [硬件设备](https://www.nvidia.com/en-us/data-center/dgx-systems/)，它们可以处理具有最小代码开销的分布式训练^([2])。

在数据并行的看似简单需求（参数共享后进行全局归约操作）背后存在着与并行计算相关的复杂问题，如容错和同步。因此，社区必须在多 GPU 训练能够可靠地投入实践之前，深入理解来自高性能计算（HPC）领域的全新设计模式和最佳实践（如[MPI](https://www.mpi-forum.org/)），正如我们在[Horovod](https://github.com/horovod/horovod)框架中看到的那样。

在此期间，一种流行的多 GPU 训练策略是将数据复制并本地放置到 GPU 上。模块如[tf.data](https://www.tensorflow.org/guide/data)通过结合预取和高效的数据格式，同时保持数据增广的灵活性，出色地提高了 GPU 的利用率。

然而，随着数据集的增加（如[Youtube8M](https://research.google.com/youtube8m/workshop2019/index.html)，其数据量超过 300 TB 且数据类型更为复杂），因此需要具有随机访问模式的云规模存储，像 HDF 这样的分布式文件系统格式在处理非结构化、多维数据时逐渐变得不那么有用。^([3])

这些格式不仅没有考虑云对象存储的设计需求，而且也没有针对模型训练独特的数据访问模式（洗牌后跟随顺序遍历）或读取类型（将多维数据作为块或分片访问，而不是读取整个文件）进行优化。

对于存储大规模数据集所需的云存储和最大化 GPU 利用率的需求之间的差距意味着机器学习管道不得不再次重新设计。

### 对象存储 + 多个 GPU

在 2018 年，我们开始看到更多利用云对象存储进行分布式训练的情况，出现了如[s3fs](https://github.com/dask/s3fs)和[AIStore](https://github.com/NVIDIA/AIStore)这样的库，以及 AWS SageMaker 的管道模式^([4])等服务。我们还看到了一些考虑云存储的 HDF 启发的数据存储格式的出现，如[Zarr](https://zarr.readthedocs.io/en/stable/)。也许最有趣的是，我们注意到许多行业机器学习团队（通常处理 100TB+ 的数据）正在开发内部解决方案，以将数据从 S3 流式传输到模型中。

尽管传输速度问题仍然存在（从云对象存储读取的速度可能比从 SSD 读取慢几个数量级），这种设计模式被广泛认为是处理 PB 级数据集的最可行技术。

虽然当前的 EBS 卷分片或将数据直接 [传输到模型](https://aws.amazon.com/blogs/machine-learning/using-pipe-input-mode-for-amazon-sagemaker-algorithms/) 以及像 [WebDataset](https://github.com/webdataset/webdataset) 等巧妙的变通方法已经足够，但吞吐量不匹配的根本问题仍然存在：云对象存储每请求可推送 ~30MB/sec，而 GPU 读取可达 140GB/sec，这意味着昂贵的加速器往往被闲置。

因此，任何针对大规模 ML 设计的新数据存储格式都需要解决几个关键问题：

+   **数据集大小**：数据集通常超出节点本地磁盘存储的容量，需要分布式存储系统和高效的网络访问。

+   **文件数量**：数据集通常包含数十亿个具有均匀随机访问模式的文件，这常常超出本地和网络文件系统的处理能力。从包含大量小文件的数据集中读取需要很长时间。

+   **数据速率**：在大型数据集上进行训练的工作通常使用多个 GPU，需要对数据集的总 I/O 带宽达到多 GB/s；这些带宽只能通过大规模并行 I/O 系统满足。

+   **洗牌和增强**：训练数据在训练前需要进行洗牌和增强。当数据集太大而无法缓存到内存中时，反复读取洗牌顺序的数据集是低效的。

+   **可扩展性**：用户通常希望在小数据集上进行开发和测试，然后迅速扩展到大数据集。

### 结论

在当前的千兆数据集环境下，研究人员应该能够在云端训练任意大型模型。正如分布式训练将模型架构与计算资源分离一样，云对象存储有潜力使 ML 训练独立于数据集大小。

^([1]) 安装和配置 [NCCL](https://developer.nvidia.com/nccl) 或 MPI 对胆量要求很高。

^([2]) 一个特别设计良好的 API 是 PyTorch 的 [DistributedDataParallel](https://pytorch.org/docs/stable/nn.html#torch.nn.parallel.DistributedDataParallel)。

^([3]) 虽然有诸如 Parquet 的创新，但这些主要处理结构化表格数据。

^([4]) AWS 的 Pipe Mode 也有助于优化 ENA 和多部分下载等底层细节。

**个人简介：** [Davit Buniatyan](https://www.linkedin.com/in/davidbuniatyan/) ([@DBuniatyan](https://twitter.com/DBuniatyan)) 在 20 岁时开始在普林斯顿大学攻读博士学位。他的研究涉及在 Sebastian Seung 的监督下重建小鼠大脑的连接组。为了克服他在神经科学实验室分析大数据集时遇到的障碍，David 成为 [Activeloop](https://www.activeloop.ai/) 的创始 CEO，该公司是 Y-Combinator 毕业的初创企业。他还获得了 Gordon Wu 奖学金和 AWS 机器学习研究奖。

**相关：**

+   [Prefect：自动化和编排数据管道的完美方式](https://www.kdnuggets.com/2021/09/prefect-way-automate-orchestrate-data-pipelines.html)

+   [Vaex：比 Pandas 快 1000 倍](https://www.kdnuggets.com/2021/05/vaex-pandas-1000x-faster.html)

+   [使用 pdpipe 构建 Pandas 数据管道](https://www.kdnuggets.com/2019/12/build-pipelines-pandas-pdpipe.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关内容

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [分析一个 90 亿美元的 AI 失败案例](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
