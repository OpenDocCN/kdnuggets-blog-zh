# Auto-Keras，或如何用 4 行代码创建深度学习模型

> 原文：[https://www.kdnuggets.com/2018/08/auto-keras-create-deep-learning-model-4-lines-code.html](https://www.kdnuggets.com/2018/08/auto-keras-create-deep-learning-model-4-lines-code.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

自动化机器学习是新兴的领域，并且它将继续存在。它帮助我们创建更好、更易用的模型。这里我将讨论 Auto-Keras，这是一个用于 AutoML 的新包。最后还有一个惊喜 ;）。

![](../Images/f159822cf162ee2840e3f2a23949e44e.png)

在开始之前，引用一段 [Matthew Mayo](https://medium.com/@mattmayo13) 对于 AutoML 的定义：

> AutoML 并不是自动化数据科学。虽然确实有重叠，但机器学习只是数据科学工具包中的众多工具之一，它的使用实际上并不涉及所有数据科学任务。例如，如果预测是某个数据科学任务的一部分，机器学习将是一个有用的组成部分；然而，机器学习可能与描述性分析任务完全无关。

那么，什么是自动化机器学习呢？简单来说，就是自动化以下任务的方式 ([https://www.automl.org/automl/](https://www.automl.org/automl/))：

+   预处理和清洗数据。

+   选择和构造合适的特征。

+   选择合适的模型家族。

+   优化模型超参数。

+   后处理机器学习模型。

+   关键性分析获得的结果。

现在我们已经弄清楚了什么是 AutoML，那么 Keras 是什么呢？

![](../Images/f22707ee9743cf46ab411f3379d6c3eb.png)

Keras 是一个高级神经网络 API，使用 Python 编写，可以运行在 [TensorFlow](https://github.com/tensorflow/tensorflow)、[CNTK](https://github.com/Microsoft/cntk) 或 [Theano](https://github.com/Theano/Theano) 之上。它的开发重点是实现快速实验。能够以尽可能少的延迟从想法到结果是做好研究的关键。

这是由 François Chollet 创建的，并且是让深度学习变得容易为大众使用的第一步。

TensorFlow 具有一个不太复杂的 Python API，但 Keras 使得许多人能够轻松进入深度学习。需要注意的是，Keras 现在已经正式成为 TensorFlow 的一部分：

[**模块: tf.contrib.keras | TensorFlow**](https://www.tensorflow.org/api_docs/python/tf/contrib/keras)

太好了。现在我们知道了 Keras 和 AutoML，那么让我们将它们结合起来。Auto-Keras 是一个用于自动化机器学习的开源软件库。Auto-Keras 提供了自动搜索深度学习模型架构和超参数的功能。

**安装**

```py
pip install autokeras
```

**使用**

对于使用方法，我将使用他们网站上的一个示例。但首先，让我们比较一下如何使用不同的工具实现相同的目标。我将使用著名且有时被诟病的 MNIST 数据集。

MNIST 是一个简单的计算机视觉数据集。它由手写数字的图像组成，如下所示：

![](../Images/f670d3b94b50fa70679d2829ff3cb185.png)

它还包括每张图像的标签，告诉我们它是哪个数字。

**使用急切执行的 TensorFlow 中的 MNIST：**

资源：

+   [**tensorflow/models** - 用 TensorFlow 构建的模型和示例](https://github.com/tensorflow/models/blob/master/official/mnist/mnist_eager.py)

+   [**Eager Execution | TensorFlow** - 尽管急切执行使得开发和调试更加互动，但 TensorFlow 图执行在…](https://www.tensorflow.org/guide/eager)

虽然不容易，但在示例中解释得非常清楚。TensorFlow 不是最简单的深度学习工具，但它是快速且可靠的。使用急切执行，代码更加可读。

**使用 PyTorch 的 MNIST：**

资源：

+   [**pytorch/examples** - 一组关于 pytorch 在视觉、文本、强化学习等方面的示例](https://github.com/pytorch/examples/blob/master/mnist/main.py)

**使用 Keras 的 MNIST：**

资源：

+   [**keras-team/keras** - 为人类打造的深度学习](https://github.com/keras-team/keras/blob/master/examples/mnist_cnn.py)

你可以看到，到目前为止，Keras 是运行此示例的更简单的包。它是一个出色的包，具有很棒的功能，可以在几分钟内从零开始得到一个模型。

但现在，画龙点睛之笔。

**MNIST 与 Auto-Keras：**

是的，就是这样。非常简单，对吧？你只需要一个 ImageClassifier，然后适配数据并评估它。你在这里也有一个 *final_fit*，它在找到最佳架构后进行最终训练。

目前你有 ImageClassifier、BayesianSearcher、Graph 模块、PreProcessor、LayerTransformer、NetTransformer、ClassifierGenerator 和一些实用工具。这是一个不断发展的包，请查看创作者的免责声明：

> 请注意，这是 Auto-Keras 的预发布版本，仍在最终测试阶段，尚未正式发布。网站、其软件及其上所有内容均按“现状”和“可用”基础提供。Auto-Keras 不对网站、其软件或任何内容的适用性或可用性作出任何明示或暗示的担保。Auto-Keras 不对因使用库或内容而导致的任何直接、间接、特殊或后果性损失承担责任。任何库的使用均由用户自行承担风险，用户将对因此类活动导致的计算机系统损坏或数据丢失承担全部责任。如果遇到任何错误、故障、功能缺失或其他问题，请立即告知我们，以便我们进行相应的修正。对此您的帮助将不胜感激。

无论哪种方式，这都是一个非常有用的惊人包，并且它在未来也会很有用。有关 AutoML 和包的更多信息，请参见：

+   [**AutoML** - BOHB 结合了贝叶斯优化和 HyperBand 的优点，以实现两者的最佳效果…](https://www.automl.org/)  

+   [**《自动化机器学习的现状》** - 自动化机器学习（AutoML）在过去一年里成为了相当感兴趣的话题...](/2017/01/current-state-automated-machine-learning.html)  

+   [**《使用 AutoML 生成带 TPOT 的机器学习管道》**  

    - 到目前为止，在这一系列文章中我们已经：这篇文章将采取不同的方法来构建管道](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html)  

哦！如果你想找一种更简单的方式来进行 AutoML 和深度学习，而无需编码，可以查看 [Deep Cognition](https://medium.com/@deepcognition) 和我关于它的文章：  

+   [**《使用 Deep Cognition 简化深度学习》** - 上个月我有幸见到了 DeepCognition.ai 的创始人。Deep Cognition 打破了显著的障碍…](https://becominghuman.ai/deep-learning-made-easy-with-deep-cognition-403fbe445351)  

+   [**《Deep Cognition 视频指南》** - 大家好！在这篇文章中，我将与大家分享几个视频，带你了解 Deep Cognition 的平台…](https://towardsdatascience.com/a-video-walkthrough-of-deep-cognition-fd0ca59d2f76)  

+   [**《在朋友的帮助下进行深度学习》** -  

    当你开始进入一个新领域时，最佳的方式是与优秀的公司、朋友或一个能…](https://towardsdatascience.com/deep-learning-with-a-little-help-from-my-friends-596ee10fd934)  

![](../Images/afc90ec947d6bb771e0ead5bb10d8f11.png)  

现在是最终惊喜时刻！如果你看到这里，我认为你对学习新事物感兴趣。我和我的朋友 [马修·丹乔](https://medium.com/@mdancho) 一起进入了为想成为数据科学家的人员创建课程的精彩领域。目前我们有一门完整的 R 课程，所以如果你是 R 爱好者，这就是为你准备的。现在我正在创建 Python 版，如果你想了解更多，请 [**点击这里**](https://university.business-science.io/)。  

[Business-Science University](https://university.business-science.io/) 将带你全面了解数据科学在商业中的应用，包括利用机器学习创建交互式应用程序，并在你的组织内分发解决方案。

**简介： [法维奥·瓦兹奎兹](https://www.linkedin.com/in/faviovazquez/)** 是一位物理学家和计算机工程师，专注于数据科学和计算宇宙学。他对科学、哲学、编程和音乐充满热情。目前，他在 Oxxo 担任首席数据科学家，专注于数据科学、机器学习和大数据。同时，他也是西班牙语数据科学出版物 Ciencia y Datos 的创始人。他喜欢新挑战、与优秀团队合作以及解决有趣的问题。他参与了 Apache Spark 的协作，帮助改进 MLlib、核心和文档。他热爱将自己的知识和专业技能应用于科学、数据分析、可视化和自动学习，以帮助世界变得更美好。

[原文](https://towardsdatascience.com/auto-keras-or-how-you-can-create-a-deep-learning-model-in-4-lines-of-code-b2ba448ccf5e). 经许可转载。

**相关资源：**

+   [自动化机器学习与自动化数据科学](https://www.kdnuggets.com/2018/07/automated-machine-learning-vs-automated-data-science.html)

+   [Keras 四步工作流程](https://www.kdnuggets.com/2018/06/keras-4-step-workflow.html)

+   [时间序列数据的自动特征工程](https://www.kdnuggets.com/2017/11/automated-feature-engineering-time-series-data.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

### 更多相关话题

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学来寻找目标，并寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一桩 90 亿美元的人工智能失败，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
