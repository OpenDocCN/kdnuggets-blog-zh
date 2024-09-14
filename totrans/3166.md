# 使用机器学习的自动化文本分类

> 原文：[https://www.kdnuggets.com/2018/01/automated-text-classification-machine-learning.html](https://www.kdnuggets.com/2018/01/automated-text-classification-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 Shashank Gupta 和 [ParallelDots](https://paralleldots.xyz/)**。

![](../Images/f929f6efa5417b9d83e866dbcc38ac11.png)

数字化改变了我们处理和分析信息的方式。在线信息的可用性呈指数级增长。从网页到电子邮件、科学期刊、电子书、学习内容、新闻和社交媒体都充满了文本数据。目标是快速创建、分析和报告信息。这时，自动化文本分类发挥了作用。

文本分类是将文本智能分类到不同类别中的过程。使用机器学习来自动化这些任务，只会使整个过程变得极快且高效。人工智能和机器学习可以说是近年来最具益处的技术，它们的应用无处不在。正如杰夫·贝索斯在他的年度股东信中所说，

> 过去几十年，计算机已经广泛地自动化了那些程序员可以用明确规则和算法描述的任务。现代机器学习技术现在使我们能够对描述精确规则更为困难的任务做同样的事情。
> 
> – 杰夫·贝索斯

具体谈到自动化文本分类，我们已经写过关于[其背后的技术](https://blog.paralleldots.com/text-analytics/get-organized-with-automated-text-classification/)和[应用](https://blog.paralleldots.com/text-analytics/text-classification-applications-use-cases/)的内容。我们现在正在更新我们的文本分类器。在这篇文章中，我们讨论了与我们自动化文本分类[API](https://www.paralleldots.com/text-analysis-apis#text-classification)相关的技术、应用、定制和分段。

文本数据的意图、情感和情绪分析是文本分类中最重要的部分之一。这些用例在机器智能爱好者中引起了重大关注。我们为每个这样的类别开发了独立的分类器，因为它们的研究本身是一个庞大的主题。文本分类器可以在各种文本数据集上运行。你可以用标记数据训练分类器，也可以在原始非结构化文本上操作。这两类都有各自众多的应用。

**监督式文本分类**

当您定义了分类类别时，进行文本的监督分类。它基于训练和测试原理。我们将标记的数据输入机器学习算法进行处理。算法在标记数据集上进行训练，并给出期望的输出（预定义的类别）。在测试阶段，算法接受未观察到的数据，并根据训练阶段的内容将其分类到各个类别中。

电子邮件的垃圾邮件过滤是监督分类的一个例子。来信根据其内容自动分类。语言检测、意图、情感和情绪分析都基于监督系统。它可以用于特殊情况，例如通过分析数百万条在线信息来识别紧急情况。这是一个“针在大海捞针”的问题。我们提出了一个智能公共交通[系统](https://blog.paralleldots.com/technology/artificial-intelligence-can-make-public-transportation-safer/)来识别这种情况。为了在数百万条在线对话中识别紧急情况，分类器必须以高准确率进行训练。它需要特殊的损失函数、训练时的采样以及像构建多个分类器的堆栈这样的解决方法，每个分类器都细化前一个分类器的结果。

![](../Images/22c33d5d209d4076cb24063eafa8d3b0.png)

监督分类基本上是让计算机模仿人类。算法接受一组标记/分类文本（也称为训练集），基于这些文本生成AI模型，这些模型在进一步接收到新的未标记文本时，可以自动对其进行分类。我们的多个[API](https://www.paralleldots.com/text-analysis-apis)是基于监督系统开发的。[文本分类器](https://www.paralleldots.com/text-analysis-apis#text-classification)目前针对150个通用类别进行了训练。

**无监督文本分类**

无监督分类是在不提供外部信息的情况下进行的。在这种情况下，算法尝试发现数据中的自然结构。请注意，自然结构可能不完全是人类认为的逻辑分割。算法在数据点中寻找相似的模式和结构，并将其分组为集群。数据的分类是基于形成的集群。以网页搜索为例。算法根据搜索词进行集群，并将其作为结果呈现给用户。

每个数据点被嵌入到超空间中，您可以在TensorBoard上可视化它们。下面的图像基于我们对印度电信公司Reliance Jio的Twitter研究。

![](../Images/a7434a2b5c15a9592501fab655d96138.png)

数据探索的目的是基于文本相似性找到相似的数据点。这些相似的数据点形成一个最近邻的簇。下图展示了推文“*reliance jio prime membership at rs 99 : here’s how to get rs 100 cashback…*”的最近邻。

![](../Images/fcd0b79def51c6b525e3272d24efebf4.png)

如你所见，附带的推文与标记的推文类似。这个簇是类似推文的一类。无监督分类在从文本数据中生成洞察时非常有用。由于不需要标记，它具有很高的可定制性。它可以在任何文本数据上运行，无需训练和标记。因此，无监督分类具有语言无关性。

**自定义文本分类**

很多时候，使用机器学习的最大障碍是数据集的缺乏。许多人想要使用 AI 进行数据分类，但这需要创建一个数据集，从而产生类似于鸡蛋-鸡的问题。自定义文本分类是构建自己文本分类器而不需要数据集的最佳方法之一。

在 ParallelDots 的最新[研究工作](https://paralleldots.xyz/Zero-Shot-Learning-for-Text-Classification)中，我们提出了一种在文本上进行零样本学习的方法，其中在一个大型噪声数据集上训练的算法可以推广到新的类别甚至新的数据集。我们称这一范式为“训练一次，测试任何地方”。我们还提出了多种神经网络算法，这些算法可以利用这种训练方法，并在不同的数据集上获得良好结果。最佳方法使用 LSTM 模型来学习关系。我们的想法是，如果能够建模句子与类别之间的“归属感”概念，那么这些知识对于未见过的类别甚至未见过的数据集也很有用。

**如何构建自定义文本分类器？**

要构建自己的自定义文本分类器，你首先需要[注册](https://www.paralleldots.com/sign-up)一个 ParallelDots 账户，并[登录](https://user.apis.paralleldots.com/login)到你的仪表板。

你可以通过点击仪表板中的‘+’图标来创建你的第一个分类器。接下来，定义一些你希望将数据分类到的类别。请注意，为了获得最佳结果，请确保你的类别是相互排斥的。

![](../Images/25903b4cff5895456703dcd038e1b7a0.png)

你可以通过分析你的文本样本来检查分类的准确性，并在发布之前根据需要调整你的类别列表。一旦类别发布，你将获得一个应用程序 ID，这将允许你使用自定义分类器 API。

考虑到数据标注和准备可能是一个限制，自定义分类器可以是构建文本分类器的一个很好的工具，而不需要大量投资。我们还相信，这将降低构建实际机器学习模型的门槛，这些模型可以应用于各个行业，解决各种用例。

作为一个 AI 研究小组，我们不断开发前沿技术，以使过程更简单、更快捷。文本分类就是这样一种技术，在未来具有巨大的潜力。随着越来越多的信息被倾倒在互联网中，智能机器算法将负责使这些信息的分析和表示变得更容易。机器智能的未来无疑令人兴奋，订阅我们的新闻通讯，以便将更多这样的信息送到你的邮箱。

[ParallelDots AI APIs](https://www.paralleldots.com/)，是由[ParallelDots Inc](https://paralleldots.xyz/)提供的深度学习驱动的网络服务，能够理解大量非结构化的文本和视觉内容，从而增强你的产品。你可以查看我们的一些文本分析[API](https://www.paralleldots.com/text-analysis-apis)，通过填写此表单[这里](https://www.paralleldots.com/contact-us)联系我们，或者通过apis@paralleldots.com给我们写邮件。

[原文](https://blog.paralleldots.com/product/automated-text-classification-using-machine-learning/)。经许可转载。

**相关**

+   [**使用深度学习解决现实世界问题**](https://www.kdnuggets.com/2018/01/databricks-democratization-ai-deep-learning.html)

+   [**Plot2txt 进行定量图像分析**](https://www.kdnuggets.com/2018/01/plot2txt-quantitative-image-analysis.html)

+   [**错误分析来救援——来自 Andrew Ng 的经验教训，第 3 部分**](https://www.kdnuggets.com/2018/01/error-analysis-your-rescue.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [入门自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [什么是文本分类？](https://www.kdnuggets.com/2022/07/text-classification.html)

+   [适合你文本分类任务的最佳架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [使用 Streamlit 进行 DIY 自动化机器学习](https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html)

+   [使用 Python 的自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)
