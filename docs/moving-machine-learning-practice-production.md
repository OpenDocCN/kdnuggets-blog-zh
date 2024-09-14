# 从实践到生产中迁移机器学习时需要问的问题

> 原文：[`www.kdnuggets.com/2016/11/moving-machine-learning-practice-production.html`](https://www.kdnuggets.com/2016/11/moving-machine-learning-practice-production.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Ramanan Balakrishnan，Semantics3**

随着对神经网络和深度学习的兴趣不断增长，个人和公司声称将人工智能越来越多地融入到他们的日常工作流程和产品提供中。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

再加上人工智能研究的飞速发展，新一波的热潮显示了在解决一些难题方面的巨大潜力。

尽管如此，我感觉这个领域在*欣赏*这些发展和随后*部署*它们以解决“现实世界”任务之间存在差距。

许多框架、教程和指南已经出现，以普及机器学习，但它们规定的步骤往往与需要解决的模糊问题不一致。

这篇文章是一些值得思考的问题（以及一些（可能甚至是错误的）答案）的集合，在将机器学习应用于生产环境时值得考虑。

### 垃圾进，垃圾出

> 我是否有可靠的数据来源？我从哪里获得我的数据集？

在开始时，大多数教程通常包含定义良好的数据集。无论是[MNIST](http://yann.lecun.com/exdb/mnist/)、[维基百科语料库](http://corpus.byu.edu/wikipedia.asp)还是来自[UCI 机器学习库](https://archive.ics.uci.edu/ml/)的任何优秀选项，这些数据集通常无法代表你希望解决的问题。

对于你的具体使用案例，可能根本不存在合适的数据集，构建数据集可能比你预期的要花费更长的时间。

例如，在 Semantics3，我们处理了许多电子商务特定的问题，从*产品分类*到*产品匹配*再到*搜索相关性*。针对这些问题，我们不得不内部审视并付出相当的努力来生成高保真的产品数据集。

在许多情况下，即使你拥有所需的数据，仍可能需要大量（且*昂贵*）的人工劳动来对数据进行分类、注释和标记以供训练使用。

### 数据转换为输入

> 需要哪些预处理步骤？在使用算法之前，我如何对数据进行归一化？

这是另一个步骤，通常与实际模型无关，在大多数教程中被忽略。特别是在探索深度神经网络时，这种遗漏显得更加明显，因为将数据转化为可用的“输入”至关重要。

虽然存在一些图像的标准技术，如裁剪、缩放、零中心化和白化——但最终的决定仍然取决于个人对每个任务所需归一化水平的判断。

在处理文本时，领域变得更加混乱。*大写字母重要吗？我应该使用分词器吗？词嵌入呢？我的词汇量和维度应该有多大？我应该使用预训练向量，还是从头开始或叠加它们？*

没有适用于所有情况的正确答案，但了解可用的选项通常是战斗的一半。最近，来自[spaCy](https://spacy.io/)创建者的[帖子](https://explosion.ai/blog/deep-learning-formula-nlp)详细介绍了一种标准化文本深度学习的有趣策略。

### 现在，我们开始吧？

> 我应该使用哪种语言/框架？Python、R、Java、C++？Caffe、Torch、Theano、Tensorflow、DL4J？

这可能是拥有最多意见的问提。我将此部分包含在此仅为完整性，并乐于指出[各种](http://blog.udacity.com/2016/04/languages-and-libraries-for-machine-learning.html) [其他](https://github.com/zer0n/deepframeworks) [资源](https://deeplearning4j.org/compare-dl4j-torch7-pylearn) [可用的](https://www.oreilly.com/ideas/six-reasons-why-i-recommend-scikit-learn)。

虽然每个人可能有不同的评估标准，但我主要关注的是自定义、原型设计和测试的便利性。在这一方面，我倾向于在可能的情况下从[scikit-learn](http://scikit-learn.org/)开始，并使用[Keras](https://keras.io/)进行我的深度学习项目。

进一步的问题如*我应该使用哪种技术？我应该使用深度模型还是浅层模型，CNN/RNN/LSTM 呢？* 再次，有许多资源可以帮助做出决策，这可能是人们谈论“使用”机器学习时讨论最多的方面。

### 训练模型

> 我如何训练我的模型？我应该购买 GPUs、定制硬件，还是使用 ec2（spot?）实例？我可以为提高速度进行并行化吗？

随着模型复杂性的不断上升以及对处理能力的需求增加，这在迁移到生产环境时是一个不可避免的问题。

一个亿级参数的网络可能会承诺在其数据集达到 TB 级时表现出色，但大多数人无法承受训练仍在进行中时等待数周的情况。

即使是较简单的模型，构建、训练、汇总和拆除任务所需的基础设施和工具也可能让人感到相当艰巨。

花时间规划你的[infrastructure](https://openai.com/blog/infrastructure-for-deep-learning/)、标准化[setup](https://engineering.semantics3.com/2016/09/24/gpu-enabled-instance-deep-learning/)并尽早定义工作流程，可以在你构建每个新模型时节省宝贵的时间。

### 没有哪个系统是孤立存在的

> 我需要进行批处理还是实时预测？嵌入式模型还是接口？RPC 还是 REST？

你的*99%-验证准确率*模型除非与生产系统的其他部分接口，否则用处不大[.](https://xkcd.com/1312/)这个决策至少部分取决于你的使用案例。

执行简单任务的模型可能可以将其权重直接打包到应用程序中，而更复杂的模型可能需要与集中式的重型服务器进行通信。

在我们的案例中，大多数生产系统离线批处理任务，而少数通过 HTTP 上的 JSON-RPC 提供实时预测。

知道这些问题的答案也可能会限制你在构建模型时应该考虑的架构类型。构建一个复杂的模型，结果发现它无法在你的移动应用中部署，是一个可以轻松避免的灾难。

### 监控性能

> 我如何跟踪我的预测？我是否将结果记录到数据库中？在线学习怎么办？

在构建、训练并将模型部署到生产环境后，任务仍未完成，除非你有监控系统到位。确保模型成功的一个关键组件是能够衡量和量化其性能。在这方面值得回答很多问题。

*我的模型如何影响整体系统性能？我应该测量哪些数据？模型是否正确处理了所有可能的输入和场景？*

由于曾在[过去](https://engineering.semantics3.com/2016/07/20/an-unexpected-dba-journey/)使用过 Postgres，我倾向于用它来监控我的模型。定期保存生产统计数据（数据样本、预测结果、异常数据细节）在执行分析（和错误追踪）时证明了其无价之宝。

另一个需要考虑的重要方面是模型的在线学习要求。*你的模型是否应该即时学习新特性？*当滑板成为现实[,](http://www.slate.com/articles/technology/future_tense/2012/11/where_s_my_hoverboard_sorry_you_re_probably_never_getting_one.html)产品分类器应该将其归入*车辆*、*玩具*还是保持*未分类*？这些都是在构建系统时值得讨论的重要问题。

### 总结一下

![这不仅仅是秘密调料](img/dccfceb5534b4afa45333e29828b1d44.png)

*这不仅仅是秘密调料*

本文提出了更多问题而非答案，但这正是其真正的目的。随着新技术、*细胞*、*层* 和网络架构的诸多进展，比以往更容易因关注细节而忽视整体。

从业者之间需要更多关于端到端部署的讨论，以推动这一领域向前发展，并真正使机器学习普及到大众。

**简历: [拉马南·巴拉克里希南](https://www.linkedin.com/in/ramananbalakrishnan)** 是 [Semantics3](https://www.semantics3.com/) 的数据科学家。当他不在深度学习、信息理论或现代物理领域钻研时，他也会参与偶尔的开锁挑战。

[原文](https://engineering.semantics3.com/2016/11/13/machine-learning-practice-to-production/)。经许可转载。

**相关:**

+   机器学习：完整而详细的概述

+   深度学习研究综述：生成对抗网络

+   将更智能的决策深嵌入运营 IT 结构

### 更多相关话题

+   [将机器学习算法全面部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [从 PoC 到生产的机器学习操作化](https://www.kdnuggets.com/2022/05/operationalizing-machine-learning-poc-production.html)

+   [将机器学习模型部署到云中的生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [为生产优先考虑数据科学模型](https://www.kdnuggets.com/2022/04/prioritizing-data-science-models-production.html)

+   [Feature Store 峰会 2023：部署 ML 模型的实用策略…](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)
