# Sibyl：Google 的大规模机器学习系统

> 原文：[https://www.kdnuggets.com/2014/08/sibyl-google-system-large-scale-machine-learning.html](https://www.kdnuggets.com/2014/08/sibyl-google-system-large-scale-machine-learning.html)

我们的生活正越来越多地受益于机器学习。以 Google 为例，Google 的搜索自动补全总是能节省我的时间。YouTube 推荐我们可能会稍后观看的视频列表，而 Gmail 的垃圾邮件检测系统帮助我们过滤垃圾邮件，尽管它有时会出错。

我一直很好奇 Google 是如何处理如此大量的数据的。

Sibyl 是 Google 最大的机器学习平台之一，广泛应用于 Google 产品的排名和评分，如 YouTube、Android 应用、Gmail 和 Google+。上个月，Google Research 的首席工程师 Tushar Chandra 在 IEEE/IFIP 可靠系统与网络国际会议（DSN 2014）上讲述了 Sibyl 的应用情况。

[![Sibyl-data](../Images/8c1ea02fbb7fcf90f7f7cac3c82ad3ec.png)](/wp-content/uploads/Sibyl-data.jpg)上表可能给你一个关于 Sibyl 处理问题规模的概念。表中显示的“每个示例的特征数量”实际上是“每个示例的活动特征数量”。每个示例的原始特征数量将接近 10 亿。只要每个示例是稀疏的，Sibyl 可以处理超过 1000 亿个示例和 1000 亿维度的数据。然而，如果使用像随机梯度下降这样的简单迭代算法，甚至 Vowpal Wabbit，将需要多年才能处理整个数据。

另一方面，准确性非常重要。机器学习通常会带来 10%以上的改进，这正在成为行业的“最佳实践”。对于 Google 来说，1%的准确性提升意味着数百万美元。因此，人们在改进机器学习解决方案上投入了大量的精力。事实上，Sibyl 使用了在多台机器上运行的 Parallel Boosting Algorithm。

[![Sibyl-scheme](../Images/d315b34e84fe420e6e4849fd3cf57d23.png)](/wp-content/uploads/Sibyl-scheme3.jpg)上面的示意图是 Sibyl 系统的核心，其中每一步都可以并行完成。我将跳过细节。对算法感兴趣的人可以阅读 [Collins, Schapire 和 Singer 的论文。](http://www.recognition.mccme.ru/pub/papers/boosting/collins00logistic.pdf)

正如 Tushar 所说，Sibyl 是唯一一个没有内部分布式系统的大规模机器学习系统。MapReduce 和 Google 文件系统提供了所需的分布式系统支持。Parallel Boosting Algorithm 使其成为可能，因为它非常适合 MapReduce 和 GFS。

[![Sybil-MapReduce](../Images/103c34b096b61c5ff9a575ca87436ea3.png)](/wp-content/uploads/Sybil-MapReduce.jpg)

他们将实例输入到 Mapper 中，每个特征被分配给一个 Reducer。在 Mapper 和 Reducer 之间，使用了 Parallel Boosting Algorithm。

通常需要 10-50 次迭代才能收敛。

Tushar 还介绍了一些设计原则，如使用 GFS 进行通信、列式数据存储、使用特征字典、将模型和统计信息存储在 RAM 中以及优化多核处理等，这些原则我在我的文章中不会一一列举。

Sibyl 将会更加快速高效，如果设计时采用这些原则。例如，正如第一个表格所示，系统通过使用列式数据存储将训练数据压缩多达 80％，从而提高效率。通过特征整数化，每个特征的字节数可以小至 0.67。

你可以在视频中找到更多细节

[DSN 2014 主旨演讲：“Sibyl：Google 大规模机器学习系统”](https://www.youtube.com/watch?v=QoUVwGZb9tA#t=2002)

**Ran Bi** 是纽约大学数据科学项目的硕士生。在她的 NYU 学习期间，她参与了多个机器学习、深度学习以及大数据分析项目。

**相关内容：**

+   [OpenML：分享、发现和执行机器学习](/2014/08/openml-share-discover-do-machine-learning.html)

+   [当 Watson 遇上机器学习](/2014/07/watson-meets-machine-learning.html)

+   [Vowpal Wabbit：大数据上的快速学习](/2014/05/vowpal-wabbit-fast-learning-on-big-data.html)

+   [PredictionIO 为开源机器学习服务器融资250万美元](/2014/07/predictionio-open-source-machine-learning-server.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关话题

+   [从 Google Colab 到 Ploomber 管道：使用 GPU 扩展机器学习](https://www.kdnuggets.com/2022/03/google-colab-ploomber-pipeline-ml-scale-gpus.html)

+   [如何高效地利用云计算扩展数据科学项目](https://www.kdnuggets.com/2023/05/efficiently-scale-data-science-projects-cloud-computing.html)

+   [扩展 MLOps 的操作手册](https://www.kdnuggets.com/2023/06/playbook-scale-mlops.html)

+   [了解数据隐私，学习实施技术隐私解决方案……](https://www.kdnuggets.com/2022/04/manning-data-privacy-learn-implement-technical-privacy-solutions-tools-scale.html)

+   [学习系统设计：五本必读书籍](https://www.kdnuggets.com/learning-system-design-top-5-essential-reads)

+   [使用 Python 为 Amazon 产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)
