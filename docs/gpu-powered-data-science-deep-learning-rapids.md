# GPU驱动的数据科学（非深度学习）与RAPIDS

> 原文：[https://www.kdnuggets.com/2021/08/gpu-powered-data-science-deep-learning-rapids.html](https://www.kdnuggets.com/2021/08/gpu-powered-data-science-deep-learning-rapids.html)

[评论](#comments)

![头图](../Images/d3c4ef08dfa0e0885360d379295720aa.png)

**图片来源**: [Pixabay](https://pixabay.com/photos/pc-hardware-geforce-radeon-6113265/) (免费图片)

## 你在寻找“GPU驱动的数据科学”吗？

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 加速你的网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

想象你自己是一名数据科学家、商业分析师或物理学/经济学/神经科学的学术研究员…

你经常进行**数据处理、清洗、统计测试、可视化**。你还经常处理**线性模型**的拟合数据，并偶尔涉足**随机森林**。你还进行**大数据集的聚类**。听起来很熟悉吗？

然而，鉴于你处理的数据集的性质（主要是表格和结构化数据），你不会过多涉足深度学习。你宁愿将所有的硬件资源投入到你日常实际工作的事务中，而不是花费在一些花哨的深度学习模型上。再说一次，熟悉吗？

你听说过[GPU系统，如NVidia提供的各种工业和科学应用的系统](https://www.nvidia.com/en-us/deep-learning-ai/products/solutions/)的强大能力和飞快的计算能力。

然后，你不断思考——“***这对我来说有什么好处？我如何在特定的工作流程中利用这些强大的半导体组件***？”

你在寻找GPU驱动的数据科学。

评估这种方法的最佳（且最快）选项之一是使用[**Saturn Cloud**](https://saturncloud.io/?utm_source=Tirtha&utm_medium=GPU-powered%20Data%20Science%20Article)** + **[**RAPIDS**](https://rapids.ai/)**。**让我详细解释一下…

## 在AI/ML的传说中，GPU主要用于深度学习

尽管在学术界和商业界对核心 AI/ML 任务中（例如运行 [1000 层深度神经网络](https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035) 进行图像分类或 [十亿参数的 BERT](https://arxiv.org/abs/1909.08053) 语音合成模型）的 GPU 和分布式计算的使用有广泛讨论，但它们在常规数据科学和数据工程任务中的实用性却得到的关注较少。

尽管如此，**数据相关任务是 AI 流水线中任何 ML 负载的关键前置条件**，它们通常占据了数据科学家或甚至 ML 工程师的 **[大部分时间和智力投入](https://www.infoworld.com/article/3228245/the-80-20-data-science-dilemma.html)**。最近，这位著名的 AI 先锋

[安德鲁·吴](https://medium.com/u/592ce2a67248?source=post_page-----29f9ed8d51f3--------------------------------) 讨论了 [**从模型中心化转向数据中心化的 AI**](https://www.youtube.com/watch?v=06-AZXmwHjo) 工具开发方法。这意味着在实际 AI 负载在你的流水线中执行之前，需要花费更多的时间处理原始数据和预处理数据。

所以，重要的问题是：***我们能否利用 GPU 和分布式计算的力量来处理常规数据处理工作***？

![](../Images/cfe568cb012bc33d8646f653e9cad210.png)

**图片来源**：作者从免费图片（[Pixabay](https://pixabay.com/vectors/squirrel-reading-books-surprise-304021/)）创建的拼贴

> 尽管在学术界和商业界对核心 AI/ML 任务中 GPU 和分布式计算的使用有广泛讨论，但它们在常规数据科学和数据工程任务中的实用性却得到的关注较少。

## 奇妙的 RAPIDS 生态系统

[RAPIDS 软件库和 API 套件](https://rapids.ai/) 使你——一个普通数据科学家（而不一定是深度学习从业者）——有选择和灵活性来在 GPU 上 **完全执行端到端的数据科学和分析流程**。

这个开源项目由 Nvidia 孵化，通过构建工具来利用 CUDA 原语。它特别关注于 **通过数据科学友好的 Python 语言暴露 GPU 并行性和高带宽内存速度特性**。

**常见的数据准备和处理任务** 在 RAPIDS 生态系统中备受重视。它还提供了显著的 **对多节点、多 GPU 部署和分布式处理的支持**。在可能的情况下，它与其他库集成，使 **超大内存**（即数据集大小大于单个计算机 RAM）数据处理对个体数据科学家变得简单易用。

![](../Images/0829816c3ae29c222246fdfa91aae184.png)

**图片来源**：作者创建的拼贴

三个最突出的（并且 Pythonic）组件——对普通数据科学家特别感兴趣——是，

+   [**CuPy**](https://docs.cupy.dev/en/stable/reference/comparison.html)：一个 CUDA 驱动的数组库，其外观和感觉与 Numpy 一致，同时使用各种 CUDA 库，例如 cuBLAS、cuDNN、cuRand、cuSolver、cuSPARSE、cuFFT 和 NCCL，以充分利用底层的 GPU 架构。

+   **CuDF**：这是一个 GPU DataFrame 库，用于加载、汇总、连接、过滤和处理数据，具有**类似 pandas 的 API**。数据工程师和数据科学家可以使用它来轻松加速他们的任务流程，无需深入了解 CUDA 编程的细节。

+   [**CuML**](https://github.com/rapidsai/cuml)：这个库使数据科学家、分析师和研究人员能够运行传统/经典的机器学习算法和相关处理任务，充分利用 GPU 的力量。自然，这主要用于表格数据集。想象一下 Scikit-learn 以及它可以用你 GPU 卡上的所有 CUDA 和 Tensor Cores 做什么！与之相匹配的是，大多数情况下，cuML 的 Python API 与 Scikit-learn 一致。此外，它尝试通过与 [**Dask**](https://docs.dask.org/en/latest/) 优雅地集成，提供**多 GPU 和多节点 GPU 支持**，以利用真正的分布式处理/集群计算。

> ***我们是否可以利用 GPU 和分布式计算的力量来处理常规数据处理任务和结构化数据的机器学习？***

## 这与使用 Apache Spark 有什么不同吗？

你可能会问，这种 GPU 驱动的数据处理与使用 Apache Spark 有什么不同。实际上，有一些微妙的区别，只有最近，随着 Spark 3.0 的发布，GPU 才成为 Spark 工作负载的主流资源。

[**使用 GPU 和 RAPIDS 加速 Apache Spark 3.0 | NVIDIA 开发者博客**](https://developer.nvidia.com/blog/accelerating-apache-spark-3-0-with-gpus-and-rapids/)

我们没有时间或空间讨论这种 GPU 驱动的数据科学方法与特别适合 Apache Spark 的大数据任务之间的独特差异。但问问自己这些问题，你可能会理解微妙的不同。

“*作为一个建模经济交易和投资组合管理的数据科学家，我想解决一个*[*线性方程组*](https://en.wikipedia.org/wiki/System_of_linear_equations)*，其中有 100,000 个变量。我应该使用纯线性代数库还是 Apache Spark*？”

“*作为图像压缩管道的一部分，我想在一个包含数百万条目的大矩阵上使用*[*奇异值分解*](https://towardsdatascience.com/understanding-singular-value-decomposition-and-its-application-in-data-science-388a54be95d)*。Apache Spark 是一个好的选择吗*？”

大问题规模并不总是意味着 Apache Spark 或 Hadoop 生态系统。大计算并不等于大数据。作为一个全面的数据科学家，你需要了解这两者，以应对各种问题。

> RAPIDS 专注于**通过 Python API 显示 GPU 并行性和高带宽内存速度特性。**

## 我们在这篇文章中展示了什么？

### 仅 CuPy 和 CuML 的简洁示例

因此，在这篇文章中，我们将仅展示 CuPy 和 CuML 的简洁示例，

+   它们与对应的 Numpy 和 Scikit-learn 函数/估计器（速度方面）的比较

+   数据/问题规模在这种速度比较中的重要性。

### 后续文章中的 CuDF 示例

尽管类似 Pandas 数据处理的数据工程示例对许多数据科学家非常感兴趣，我们将在后续文章中介绍 CuDF 示例。

### 我的 GPU 基础硬件平台是什么？

我使用的是[**Saturn Cloud**](https://saturncloud.io/?utm_source=Tirtha&utm_medium=GPU-powered%20Data%20Science%20Article) 的 Tesla T4 GPU 实例，因为创建一个[功能齐全并加载了（数据科学和 AI 库）计算资源的云实例](https://www.saturncloud.io/s/nvidia/) 只需 5 分钟时间。**只要我每月不超过 10 小时的 Jupyter Notebook 使用时间，它就是免费的**！如果你想了解更多关于他们服务的信息，

## Saturn Cloud 托管现已上线：人人都能使用的 GPU 数据科学！

[**GPU 计算是数据科学的未来。像 RAPIDS、TensorFlow 和 PyTorch 这样的包使计算变得迅速…**](https://www.saturncloud.io/blog/saturn-cloud-hosted-has-launched-gpu-data-science-for-everyone/)

除了拥有[Tesla T4 GPU](https://www.nvidia.com/en-us/data-center/tesla-t4/)，它还是一台4核 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz 机器，配有 16 GB RAM 和 10 GB 持久磁盘。因此，从硬件配置的角度来看，这是一个相当普通的设置（由于免费层，硬盘有限），即任何数据科学家都可能拥有这种硬件。唯一的区别在于 GPU 的存在，以及正确设置所有 CUDA 和 Python 库，以确保 RAPIDS 套件正常运行。

> 大问题规模并不总意味着 Apache Spark 或 Hadoop 生态系统。大计算不等同于大数据。作为一个全面的数据科学家，你需要了解这两者，以解决各种问题。

### 解线性方程组

我们创建了不同大小的线性方程组，并使用 Numpy（和 CuPy）的`linalg.solve`例程通过以下代码解决它，

![](../Images/da82dcb41575536341d1705d43609761.png)

并且，CuPy 实现的代码在多次调用中仅通过一个字母的变化就能完成！

![](../Images/a1bac98a78faba8d375bbea8b9339f76.png)

还请注意，我们如何从 Numpy 数组创建 CuPy 数组作为参数。

结果却很显著。CuPy 开始时较慢或与 Numpy 的速度相似，但对于大规模问题（方程数量）表现得更为出色。

![](../Images/3eef97e201daaefe710505532167abe3.png)

### 奇异值分解

接下来，我们使用一个从正态分布中生成的随机方阵（具有不同大小）来解决奇异值分解问题。我们这里不重复代码块，仅展示结果以简洁起见。

![](../Images/c9e56c1ab31bb1185a08a915d28e1175.png)

需要注意的是，CuPy 算法在此问题类别中并未显著优于 Numpy 算法。也许，这是 CuPy 开发者需要改进的地方。

### 回归基础：矩阵求逆

最后，我们回到基础，考虑矩阵求逆这一基本问题（几乎在所有机器学习算法中都有使用）。结果再次显示 CuPy 算法在性能上明显优于 Numpy 包。

![](../Images/520e51bcb56b1e8fec32e0f8f078365b.png)

### 处理 K-means 聚类问题

接下来，我们考虑一个无监督学习问题，使用众所周知的 k-means 算法进行聚类。在这里，我们比较了 CuML 函数与 Scikit-learn 包中的等效估计器。

仅供参考，这里是这两个估计器的 API 比较。

![](../Images/facdccdbfe76522bafd1a3c7a968a4b3.png)

**图片来源**： [Scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) 和 [CuML 网站](https://docs.rapids.ai/api/cuml/stable/api.html#k-means-clustering) （开源项目）

这是一个具有 10 个特征/维度的数据集的结果。

![](../Images/6e741bf16eb344f368b8968ea9a58538.png)

这是另一个具有 100 个特征的数据集的实验结果。

![](../Images/c49a3e4ddf1e024f4f98159babf87482.png)

显然，样本数量（行数）和维度（列数）在 GPU 加速的性能表现中都起着重要作用。

### 众所周知的线性回归问题

在处理表格数据集时，谁能忽视线性回归问题的速度比较呢？按照以前的节奏，我们改变了问题的规模——这一次同时改变样本数量和维度——并比较了 CuML `LinearRegression` 估计器与 Scikit-learn 稳定版的性能。

下图中的 X 轴表示问题规模——从 1,000 个样本/50 个特征到 20,000 个样本/1,000 个特征。

再次强调，随着问题复杂性的增加（样本数量和维度），CuML 估计器的表现显著提升。

![](../Images/6e95f098dc9fe9c215df7cef5e96d97f.png)

## 总结

我们专注于 RAPIDS 框架的两个最基本组件，该框架旨在将 GPU 的强大计算能力带到数据分析和机器学习的日常任务中，即使数据科学家不进行任何深度学习任务。

![](../Images/99e2cd77dfaf96b3a37ddccb0662cae7.png)

**图片来源**：由作者使用免费Pixabay图片制作（[链接-1](https://pixabay.com/photos/nvidia-graphic-card-bitcoin-gpu-5264921/), [链接-2](https://pixabay.com/vectors/cube-hexagon-stairs-152932/), [链接-3](https://pixabay.com/vectors/statistic-analytic-diagram-1564428/))

我们使用了一个 [**Saturn Cloud**](https://saturncloud.io/?utm_source=Tirtha&utm_medium=GPU-powered%20Data%20Science%20Article)** 基于Tesla T4的实例，进行 **[**简单、免费的快速设置**](https://www.saturncloud.io/s/freehosted/)**，并展示了一些CuPy和CuML库的功能以及广泛使用的算法性能比较。

+   并非所有RAPIDS库中的算法都极为出色，但大多数都是。

+   一般来说，随着问题复杂性（样本大小和维度）的增加，性能提升迅速。

+   如果你有GPU，始终尝试使用RAPIDS，比较和测试是否获得了性能提升，并使其成为你数据科学管道中值得信赖的工作马。

+   代码变更最小，几乎没有，切换过来几乎没有成本。

**让GPU的力量启动你的分析和数据科学工作流程**。

你可以查看作者的 [**GitHub**](https://github.com/tirthajyoti?tab=repositories)** 代码库**，获取机器学习和数据科学的代码、创意和资源。如果你像我一样，对AI/机器学习/数据科学充满热情，请随时 [在LinkedIn上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 或 [关注我在Twitter上的账号](https://twitter.com/tirthajyotiS)。

感谢Mel。

[原文](https://medium.com/dataseries/gpu-powered-data-science-not-deep-learning-with-rapids-29f9ed8d51f3)。经许可转载。

**相关：**

+   [如何使用NVIDIA GPU加速库](/2021/07/nvidia-gpu-accelerated-libraries.html)

+   [你为什么以及如何学习“高效数据科学”？](/2021/07/learn-productive-data-science.html)

+   [不仅仅是深度学习：GPU如何加速数据科学和数据分析](/2021/07/deep-learning-gpu-accelerate-data-science-data-analytics.html)

### 更多相关内容

+   [成为出色数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目的，并通过目的来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
