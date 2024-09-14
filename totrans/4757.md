# 前13名Python深度学习库

> 原文：[https://www.kdnuggets.com/2018/11/top-python-deep-learning-libraries.html](https://www.kdnuggets.com/2018/11/top-python-deep-learning-libraries.html)

[评论](#comments)

Python在机器学习、人工智能、深度学习和数据科学任务中继续引领潮流。根据builtwith.com的数据，45%的科技公司更喜欢使用Python来实现人工智能和机器学习。

因此，我们决定开始一系列调查不同类别的[Python库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)：

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT

* * *

[**前8名Python机器学习库**](https://www.kdnuggets.com/2018/10/top-python-machine-learning-libraries.html) **✅**

**前13名Python深度学习库 ✅** - 本文

X大Python强化学习和进化计算库 – 敬请期待！

X大Python数据科学库 – 敬请期待！

当然，这些列表完全是主观的，因为许多库可以轻松地归入多个类别。例如，TensorFlow包含在此列表中，但Keras被省略，出现在[机器学习库集合](https://www.kdnuggets.com/2018/10/top-python-machine-learning-libraries.html)中。这是因为Keras更像是一个‘最终用户’库，如SKLearn，而TensorFlow更受研究人员和机器学习工程师类型的欢迎。

一如既往，请随时在下方评论区表达您的挫折/异议/烦恼！

![前Python深度学习](../Images/2b0972d63d92c72736ab85154f0e1459.png)

**图1：按提交和贡献者排名的前13名Python深度学习库。** 圆圈大小与星级数量成正比。

现在，让我们来看一下名单（截至2018年10月23日的GitHub数据）：

**1\. [TensorFlow](https://github.com/tensorflow/tensorflow)（贡献者 – 1700，提交 – 42256，星级 – 112591）**

**TensorFlow** 是一个用于数值计算的开源软件库，采用数据流图来进行计算。图中的节点代表数学运算，而图中的边代表在节点之间流动的多维数据数组（张量）。这种灵活的架构使你可以在桌面、服务器或移动设备上的一个或多个 CPU 或 GPU 上部署计算，而无需重写代码。

**2\. [PyTorch](https://github.com/pytorch/pytorch)（贡献者 – 806，提交 – 14022，星标 – 20243）**

PyTorch 是一个 Python 包，提供两个高级特性：

+   类似于 NumPy 的张量计算，具有强大的 GPU 加速

+   基于带有自动微分系统的深度神经网络

你可以在需要时重用你喜欢的 Python 包，如 NumPy、SciPy 和 Cython，以扩展 PyTorch。

**[3\. Apache MXNet](https://github.com/apache/incubator-mxnet)（贡献者 – 628，提交 – 8723，星标 – 15447）**

Apache MXNet（孵化中）是一个旨在兼顾 *效率* 和 *灵活性* 的深度学习框架。它允许你 ***混合*** [符号编程和命令式编程](https://mxnet.incubator.apache.org/architecture/index.html#deep-learning-system-design-concepts) 以 ***最大化*** 效率和生产力。其核心包含一个动态依赖调度器，可以在运行时自动对符号操作和命令式操作进行并行处理。

**4\. [Theano](https://github.com/Theano/Theano)（贡献者 – 329，提交 – 28033，星标 – 8536）**

Theano 是一个 Python 库，允许你高效地定义、优化和评估涉及多维数组的数学表达式。它可以使用 GPU 并执行高效的符号微分。

**5\. [Caffe](https://github.com/BVLC/caffe)（贡献者 – 270，提交 – 4152，星标 – 25927）**

Caffe 是一个深度学习框架，注重表达、速度和模块化。由伯克利人工智能研究所 ([BAIR](http://bair.berkeley.edu/)) / 伯克利视觉与学习中心（BVLC）及社区贡献者开发。

**6\. [fast.ai](https://github.com/fastai/fastai)（贡献者 – 226，提交 – 2237，星标 – 8872）**

fastai 库通过现代最佳实践简化了快速而准确的神经网络训练。查看 [fastai 网站](https://docs.fast.ai/) 以开始使用。该库基于 [fast.ai](http://www.fast.ai/) 进行的深度学习最佳实践研究，并包括对 [vision](https://docs.fast.ai/vision#vision)、[text](https://docs.fast.ai/text#text)、[tabular](https://docs.fast.ai/tabular#tabular) 和 [collab](https://docs.fast.ai/collab#collab)（协同过滤）模型的“开箱即用”支持。

**7\. [CNTK](https://github.com/Microsoft/CNTK)（贡献者 – 189，提交 – 15979，星标 – 15281）**

“微软认知工具包 ([https://cntk.ai](https://cntk.ai/)) 是一个统一的深度学习工具包，它通过有向图描述神经网络。在这个有向图中，叶节点表示输入值或网络参数，而其他节点表示对其输入的矩阵操作。CNTK 允许用户轻松实现和组合流行的模型类型，如前馈 DNN、卷积网络（CNN）和递归网络（RNN/LSTM）。”

**8\. [TFLearn](https://github.com/tflearn/tflearn)（贡献者 – 118，提交次数 – 599，星标 – 8632）**

“TFlearn 是一个建立在 Tensorflow 之上的模块化和透明的深度学习库。它旨在提供一个更高级的 TensorFlow API，以促进和加速实验，同时保持与 TensorFlow 的完全透明和兼容。”

**9\. [Lasagne](https://github.com/Lasagne/Lasagne)（贡献者 – 64，提交次数 – 1157，星标 – 3534）**

“Lasagne 是一个轻量级库，用于在 Theano 中构建和训练神经网络。它支持前馈网络，如卷积神经网络（CNN），递归网络包括长短期记忆（LSTM），以及它们的任何组合。”

**10\. [nolearn](https://github.com/dnouri/nolearn)（贡献者 – 14，提交次数 – 389，星标 – 909）**

“*nolearn* 包含了多个围绕现有神经网络库的包装器和抽象，尤其是 [Lasagne](http://lasagne.readthedocs.org/)，以及一些机器学习实用模块。所有代码均与 [scikit-learn](http://scikit-learn.org/) 兼容。”

**11\. [Elephas](https://github.com/maxpumperla/elephas)（贡献者 – 13，提交次数 – 249，星标 – 1046）**

“Elephas 是一个扩展的 [Keras](http://keras.io/)，允许你在大规模上运行分布式深度学习模型。Elephas 目前支持多种应用，包括：”

+   [数据并行训练深度学习模型](https://github.com/maxpumperla/elephas#basic-spark-integration)

+   [分布式超参数优化](https://github.com/maxpumperla/elephas#distributed-hyper-parameter-optimization)

+   [集成模型的分布式训练](https://github.com/maxpumperla/elephas#distributed-training-of-ensemble-models)

**12\. [spark-deep-learning](https://github.com/databricks/spark-deep-learning)（贡献者 – 12，提交次数 – 83，星标 – 1131）**

“Deep Learning Pipelines 提供了用于 Python 的可扩展深度学习的高级 API，结合了 Apache Spark。这一库源自 Databricks，并利用 Spark 的两个最强大的方面：”

+   “秉承 Spark 和 [Spark MLlib](https://spark.apache.org/mllib/) 的精神，它提供了易于使用的 API，使深度学习只需少量代码。”

+   “它利用 Spark 强大的分布式引擎来扩展大规模数据集上的深度学习。”

**13\. Distributed Keras（贡献者 – 5，提交次数 – 1125，星标 – 523）**

“Distributed Keras 是一个构建在 Apache Spark 和 Keras 之上的分布式深度学习框架，专注于“最先进”的分布式优化算法。我们设计了这个框架，使得新分布式优化器可以轻松实现，从而使研究人员能够专注于研究。”

请关注本系列的下一部分——重点讨论强化学习和进化计算库——将在接下来的几周内发布！

**相关：**

+   [顶级 8 款 Python 机器学习库](https://www.kdnuggets.com/2018/10/top-python-machine-learning-libraries.html)

+   [深度了解深度学习：理解其基本原理和原因](https://www.kdnuggets.com/2018/10/dataiku-deep-look-deep-learning-understanding-basics.html)

+   [使用 Keras 的深度学习简介](https://www.kdnuggets.com/2018/10/introduction-deep-learning-keras.html)

### 更多相关主题

+   [每个数据科学家都应知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [数据科学学习统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学，寻找目标，并寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一项 90 亿美元的 AI 失败，剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
