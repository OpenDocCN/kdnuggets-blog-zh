# 令人惊叹的低代码机器学习功能与新的 Ludwig 更新

> 原文：[https://www.kdnuggets.com/2021/06/ludwig-update-includes-low-code-machine-learning-capabilities.html](https://www.kdnuggets.com/2021/06/ludwig-update-includes-low-code-machine-learning-capabilities.html)

[评论](#comments)

![](../Images/6f2248f00bd3292b0311bb20c4c81a80.png)

**图片来源：Ludwig**

> 我最近启动了一个新的新闻通讯，专注于 AI 教育，**已经有超过 50,000 名订阅者**。TheSequence 是一个不含废话（即没有炒作，没有新闻等）的 AI 专注型新闻通讯，阅读时间为 5 分钟。目标是让你了解最新的机器学习项目、研究论文和概念。请通过下面的订阅试试看：

[![图片](../Images/f2aed90f956dea213be7c9bbf9cd7072.png)](https://thesequence.substack.com/)

如果你关注这个博客，你会知道我非常喜欢 Ludwig 开源项目。最初由 Uber 孵化，现在成为 Linux AI Foundation 的一部分，Ludwig 提供了当前市场上最好的低代码机器学习(ML)堆栈之一。上周，Ludwig 的 0.4 版本已开源，并包含一组酷炫的功能，这些功能可能使其在实际机器学习解决方案中更具优势。

### 什么是 Uber Ludwig？

从功能上讲，Ludwig 是一个框架，用于简化选择、训练和评估给定场景的机器学习模型的过程。考虑配置而不是编码机器学习模型。Ludwig 提供了一组可以组合在一起创建针对特定需求优化的端到端模型的模型架构。概念上，Ludwig 是基于一系列原则设计的：

+   ***无需编码：*** 不需要编码技能即可训练模型并使用其进行预测。

+   ***通用性：*** 一种基于数据类型的深度学习模型设计的新方法，使工具可以用于许多不同的用例。

+   ***灵活性：*** 经验丰富的用户对模型构建和训练有广泛的控制，而新手会发现它易于使用。

+   ***可扩展性：*** 易于添加新的模型架构和新的特征数据类型。

+   ***可理解性：*** 深度学习模型的内部结构通常被认为是黑箱，但我们提供标准可视化来理解其性能并比较其预测。

### 一种适用于一切机器学习的声明性体验

Ludwig 的发展轨迹专注于使基于配置的声明性模型能够与当前市场上的顶级机器学习堆栈互动。从这个角度来看，Ludwig 为希望在解决方案中利用最佳机器学习框架的数据科学团队提供了一个简单且一致的体验。

![](../Images/bae0d11b0a17a7239ebcfeb4bb5d746d.png)

图片来源：Ludwig

### Ludwig 0.4

Ludwig 新版的重点是简化 MLOps 实践中的声明式模型。从这一角度来看，Ludwig 0.4 包含了一系列功能，可能会简化在实际解决方案中实现 MLOps 管道的过程。让我们来回顾几个：

### 1) Ludwig 与 Ray

到目前为止，我最喜欢的功能是与 Ray 平台的集成。Ray 是一个高度可扩展的机器学习训练和优化过程的完整堆栈之一。在 Ludwig 0.4 中，数据科学家可以通过几行配置代码，将训练工作负载从单个笔记本电脑扩展到大型 Ray 集群。

### 2) 使用 Ray Tune 进行超参数搜索

Ray Tune 是 Ray 平台的一个组件，它允许在大型节点集群中进行分布式超参数搜索。Ludwig 0.4 集成了 Ray Tune，支持如 [基于种群的训练](https://docs.ray.io/en/master/tune/api_docs/schedulers.html#tune-scheduler-pbt)、[贝叶斯优化](https://docs.ray.io/en/master/tune/api_docs/suggestion.html#bayesopt) 和 [HyperBand](https://docs.ray.io/en/master/tune/api_docs/schedulers.html#hyperband-tune-schedulers-hyperbandscheduler) 等分布式超参数搜索算法。

### 3) 使用 TabNet 进行声明式表格模型

TabNet 是用于表格数据的顶级深度学习框架之一，具有前沿的注意力架构。Ludwig 的新版本通过添加新的 TabNet 组合器，支持表格特征转换和注意力机制，提供了一种声明式的表格模型体验，以实现最先进的性能。

### 4) 使用 MLflow 进行实验跟踪和模型服务

MLflow 正迅速成为最受欢迎的机器学习实验跟踪和模型服务平台之一。Ludwig 0.4 通过单一命令行实现基于 MLflow 的实验跟踪。此外，新版本的 Ludwig 可以通过简单的命令行语句将 ML 模型部署到 MLflow 注册表中。

[原文](https://jrodthoughts.medium.com/ludwig-0-4-is-here-and-includes-some-amazing-low-code-machine-learning-capabilities-44cc66c61daa)。经许可转载。

**相关内容：**

+   [DeepMind 想要重新构想机器学习中最重要的算法之一](/2021/05/deepmind-reimagine-important-algorithms-machine-learning.html)

+   [用于解释和比较机器学习模型的仪表板](/2021/06/dashboards-interpreting-comparing-machine-learning-models.html)

+   [机器学习数据集选择的九大致命错误](/2021/06/9-deadly-sins-ml-dataset-selection.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

### 相关话题

+   [预测未来事件：AI和ML的能力与局限](https://www.kdnuggets.com/2023/06/forecasting-future-events-capabilities-limitations-ai-ml.html)

+   [2023年你应该了解的10个惊人机器学习可视化](https://www.kdnuggets.com/2022/11/10-amazing-machine-learning-visualizations-know-2023.html)

+   [通过Python图形库制作惊人的可视化](https://www.kdnuggets.com/2022/12/make-amazing-visualizations-python-graph-gallery.html)

+   [获取数据科学项目的10个惊人网站](https://www.kdnuggets.com/2023/04/10-websites-get-amazing-data-data-science-projects.html)

+   [2023年你需要尝试的5个惊人且免费的LLM游乐场](https://www.kdnuggets.com/5-amazing-free-llms-playgrounds-you-need-to-try-in-2023)

+   [如何更新Python字典](https://www.kdnuggets.com/2023/02/update-python-dictionary.html)
