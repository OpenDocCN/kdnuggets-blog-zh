# 成为数据科学家你需要了解的十种机器学习算法

> 原文：[https://www.kdnuggets.com/2018/04/10-machine-learning-algorithms-data-scientist.html/2](https://www.kdnuggets.com/2018/04/10-machine-learning-algorithms-data-scientist.html/2)

### 6\. 前馈神经网络

这些基本上是多层逻辑回归分类器。多个由非线性激活函数（sigmoid、tanh、relu + softmax 和新的 selu）分隔的权重层。它们的另一个常见名称是多层感知机。FFNN 可以用于分类和作为自编码器进行无监督特征学习。

![machine learning algorithms](../Images/b73e7922fae61090203dc0ec473f9f81.png)

多层感知机

![machine learning algorithms](../Images/1e53aeab5704a84516b7e95581213348.png)

FFNN 作为自编码器

FFNN 可以用于训练分类器或作为自编码器提取特征

**库：**

[http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html#sklearn.neural_network.MLPClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html#sklearn.neural_network.MLPClassifier)

[http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPRegressor.html](http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPRegressor.html)

[https://github.com/keras-team/keras/blob/master/examples/reuters_mlp_relu_vs_selu.py](https://github.com/keras-team/keras/blob/master/examples/reuters_mlp_relu_vs_selu.py)

**入门教程：**

[http://www.deeplearningbook.org/contents/mlp.html](http://www.deeplearningbook.org/contents/mlp.html)

[http://www.deeplearningbook.org/contents/autoencoders.html](http://www.deeplearningbook.org/contents/autoencoders.html)

[http://www.deeplearningbook.org/contents/representation.html](http://www.deeplearningbook.org/contents/representation.html)

### 7\. 卷积神经网络（Convnets）

几乎所有世界上最先进的基于视觉的机器学习结果都是通过卷积神经网络实现的。它们可以用于图像分类、目标检测或图像分割。由 Yann Lecun 在80年代末90年代初发明，卷积神经网络具有作为层级特征提取器的卷积层。你也可以在文本中使用它们（甚至在图形中）。

![](../Images/4b9d7421c900673e7f954c1822d35b4d.png)

使用卷积神经网络进行最先进的图像和文本分类、目标检测、图像分割。

**库：**

[https://developer.nvidia.com/digits](https://developer.nvidia.com/digits)

[https://github.com/kuangliu/torchcv](https://github.com/kuangliu/torchcv)

[https://github.com/chainer/chainercv](https://github.com/chainer/chainercv)

[https://keras.io/applications/](https://keras.io/applications/)

**入门教程：**

[http://cs231n.github.io/](http://cs231n.github.io/)

[https://adeshpande3.github.io/A-Beginner%27s-Guide-To-Understanding-Convolutional-Neural-Networks/](https://adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/)

### 8. 循环神经网络（RNNs）：

RNN通过在时间t应用相同的权重集合到汇聚状态和输入上来对序列建模（给定一个序列在时间0..t..T有输入，并且在每个时间t都有一个隐藏状态，该状态是RNN的t-1步骤的输出）。纯RNN现在很少使用，但像LSTM和GRU这样的对比模型在大多数序列建模任务中处于最前沿。

![机器学习算法](../Images/9df42b9af4a17bfb507f092586a6b63f.png)

RNN（如果这里有一个密集连接单元和非线性，现代的f通常是LSTM或GRU）。LSTM单元用来代替纯RNN中的普通密集层。

![机器学习算法](../Images/a2e8504e97e3117fc1659842b6a95898.png)

使用RNN进行任何序列建模任务，特别是文本分类、机器翻译、语言建模

**库：**

[https://github.com/tensorflow/models](https://github.com/tensorflow/models)（这里有很多来自Google的酷炫NLP研究论文）

[https://github.com/wabyking/TextClassificationBenchmark](https://github.com/wabyking/TextClassificationBenchmark)

[http://opennmt.net/](http://opennmt.net/)

**入门教程：**

[http://cs224d.stanford.edu/](http://cs224d.stanford.edu/)

[http://www.wildml.com/category/neural-networks/recurrent-neural-networks/](http://www.wildml.com/category/neural-networks/recurrent-neural-networks/)

[http://colah.github.io/posts/2015-08-Understanding-LSTMs/](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)

### 9. 条件随机场（CRFs）

CRF可能是概率图模型（PGMs）家族中最常用的模型。它们用于像RNN这样的序列建模，也可以与RNN结合使用。在神经机器翻译系统出现之前，CRF是最先进的技术，在许多小数据集的序列标记任务中，它们仍然比RNN表现更好，因为RNN需要更多的数据来进行泛化。CRF也可以用于其他结构化预测任务，如图像分割等。CRF对序列中的每个元素（比如一个句子）进行建模，使得邻近元素影响序列中某个组件的标签，而不是所有标签彼此独立。

使用CRF对序列进行标记（在文本、图像、时间序列、DNA等中）

**库：**

[https://sklearn-crfsuite.readthedocs.io/en/latest/](https://sklearn-crfsuite.readthedocs.io/en/latest/)

**入门教程：**

[http://blog.echen.me/2012/01/03/introduction-to-conditional-random-fields/](http://blog.echen.me/2012/01/03/introduction-to-conditional-random-fields/)

Hugo Larochelle在YouTube上的7部分讲座系列：[https://www.youtube.com/watch?v=GF3iSJkgPbA](https://www.youtube.com/watch?v=GF3iSJkgPbA)

### 10. 决策树

假设我拿到一个包含各种水果数据的 Excel 表格，我需要判断哪些看起来像苹果。我会问一个问题：“哪些水果是红色且圆形的？”然后将所有水果根据是否回答“是”或“否”来分类。现在，所有红色且圆形的水果可能不是苹果，而所有苹果也不一定是红色和圆形的。所以，我会对红色和圆形的水果问一个问题：“哪些水果有红色或黄色的色调？”对非红色和圆形的水果问：“哪些水果是绿色且圆形的？”根据这些问题，我可以较为准确地判断哪些是苹果。这种问题级联就是决策树。然而，这只是基于我的直觉的决策树。直觉无法处理高维和复杂的数据。我们必须通过查看标记的数据自动生成问题级联。这就是基于机器学习的决策树所做的。早期版本如 CART 树曾用于简单数据，但随着数据集的增大和复杂化，需要用更好的算法来解决偏差-方差权衡。现在常用的两种决策树算法是随机森林（在属性的随机子集上构建不同的分类器并将它们结合输出）和提升树（训练一系列级联的树，每棵树都纠正其下方树的错误）。

决策树可用于分类数据点（甚至回归）

**库**

[http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

[http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html)

[http://xgboost.readthedocs.io/en/latest/](http://xgboost.readthedocs.io/en/latest/)

[https://catboost.yandex/](https://catboost.yandex/)

### 入门教程

[http://xgboost.readthedocs.io/en/latest/model.html](http://xgboost.readthedocs.io/en/latest/model.html)

[https://arxiv.org/abs/1511.05741](https://arxiv.org/abs/1511.05741)

[https://arxiv.org/abs/1407.7502](https://arxiv.org/abs/1407.7502)

[http://education.parrotprediction.teachable.com/p/practical-xgboost-in-python](http://education.parrotprediction.teachable.com/p/practical-xgboost-in-python)

**TD 算法（推荐使用）**

如果你仍在疑惑这些方法如何解决像DeepMind那样击败围棋世界冠军的任务，其实它们不能。我们之前讨论的10种算法都是模式识别算法，而不是策略学习算法。要学习解决多步骤问题的策略，比如赢得棋局或玩Atari游戏，我们需要让一个智能体在世界中自由探索，并从它所面临的奖励/惩罚中学习。这种机器学习方法被称为强化学习。最近很多（但不是全部）领域的成功都是将卷积神经网络（Convnet）或长短期记忆网络（LSTM）的感知能力与一种称为时间差分学习（Temporal Difference Learning）的算法集合结合的结果。这些算法包括Q-Learning、SARSA及其他变种。这些算法聪明地运用了Bellman方程，以获得一个可以通过智能体从环境中获得的奖励进行训练的损失函数。

这些算法主要用于自动玩游戏 :D，也用于语言生成和目标检测的其他应用。

**库：**

[https://github.com/keras-rl/keras-rl](https://github.com/keras-rl/keras-rl)

[https://github.com/tensorflow/minigo](https://github.com/tensorflow/minigo)

**入门教程：**

获取免费的Sutton和Barto书籍： [https://web2.qatar.cmu.edu/~gdicaro/15381/additional/SuttonBarto-RL-5Nov17.pdf](https://web2.qatar.cmu.edu/~gdicaro/15381/additional/SuttonBarto-RL-5Nov17.pdf)

观看David Silver的课程： [https://www.youtube.com/watch?v=2pWv7GOvuf0](https://www.youtube.com/watch?v=2pWv7GOvuf0)

这些是你可以学习的10种机器学习算法，以成为数据科学家。

你还可以在 [这里](https://blog.paralleldots.com/data-science/lesser-known-machine-learning-libraries-part-ii/) 阅读有关机器学习库的文章。

我们希望你喜欢这篇文章。请 [注册](http://user.apis.paralleldots.com/signing-up?utm_source=blog&utm_medium=chat&utm_campaign=paralleldots_blog) 一个免费的ParallelDots账户，开始你的AI之旅。你还可以在 [这里](https://www.paralleldots.com/ai-apis) 查看ParallelDots AI API的演示。

[原文](https://blog.paralleldots.com/data-science/machine-learning/ten-machine-learning-algorithms-know-become-data-scientist/)。经授权转载。

**相关：**

+   [2018年深度学习前20篇论文](https://www.kdnuggets.com/2018/03/top-20-deep-learning-papers-2018.html)

+   [层次分类 – 一种预测数千种可能类别的有用方法](https://www.kdnuggets.com/2018/03/hierarchical-classification.html)

+   [AI从业者需要应用的10种深度学习方法](https://www.kdnuggets.com/2017/12/10-deep-learning-methods-ai-practitioners-need-apply.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全认证](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

### 更多相关话题

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成为一名伟大的数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [100亿美元的AI失败，剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
