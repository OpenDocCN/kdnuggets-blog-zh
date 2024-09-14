# 深度多任务学习 – 3 个学到的经验

> 原文：[`www.kdnuggets.com/2019/02/deep-multi-task-learning.html`](https://www.kdnuggets.com/2019/02/deep-multi-task-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Zohar Komarovsky](https://www.linkedin.com/in/zohar-komarovsky-7773b9b/)，Taboola**。

在过去的一年里，我和我的团队一直在[Taboola feed](https://blog.taboola.com/taboola-feed/)上致力于个性化用户体验。我们使用了[多任务学习](http://ruder.io/multi-task)（MTL）来预测同一输入特征集上的多个关键绩效指标（KPI），并实现了一个在 TensorFlow 中进行深度学习（DL）的模型。刚开始时，MTL 对我们来说似乎复杂得多，因此我想分享一些学到的经验。

已经有不少关于在深度学习模型中实现 MTL 的帖子（[1](https://jg8610.github.io/Multi-Task)， [2](https://medium.com/@kajalgupta/multi-task-learning-with-deep-neural-networks-7544f8b7b4e3)， [3](https://hanxiao.github.io/2017/07/07/Get-10x-Speedup-in-Tensorflow-Multi-Task-Learning-using-Python-Multiprocessing)）。

### **分享即关怀**

我们想从[硬参数共享](http://ruder.io/multi-task/index.html#hardparametersharing)的基本方法开始。硬共享意味着我们有一个共享的子网络，随后是特定于任务的子网络。

![](img/80214669e0ea6d08cccad7cb7c411c68.png)

在 TensorFlow 中开始玩这样的模型的一种简单方法是使用[估算器](https://www.youtube.com/watch?v=4oNdaQk0Qv4)和[多个头](https://www.tensorflow.org/api_docs/python/tf/contrib/estimator/multi_head)。由于它看起来与其他神经网络架构没有太大不同，你可能会问什么会出错？

### **第 1 课 – 组合损失**

我们在 MTL 模型中遇到的第一个挑战是为多个任务定义一个单一的损失函数。虽然单个任务有一个明确的损失函数，但多个任务则有多个损失。

我们尝试的第一种方法是简单地将不同的损失进行求和。很快我们发现，虽然一个任务收敛到良好的结果，其他任务却表现得相当差。当我们仔细观察时，我们可以很容易地看到原因。损失的尺度差异如此之大，以至于一个任务主导了整体损失，而其他任务没有机会影响共享层的学习过程。

一个快速的解决方法是用加权和代替损失之和，这将所有损失调整到大致相同的尺度。然而，这个解决方案涉及到另一个可能需要不时调整的超参数。

幸运的是，我们找到了一篇[很棒的论文](https://arxiv.org/abs/1705.07115)，提出了在 MTL 中使用不确定性来加权损失。它的做法是，通过学习另一个噪声参数，并将其集成到每个任务的损失函数中。这允许处理多个任务，可能是回归和分类，并将所有损失带到相同的尺度。现在我们可以回到简单地求和我们的损失。

我们不仅得到了比加权和更好的结果，而且可以忽略额外的权重超参数。[这里](https://github.com/yaringal/multi-task-learning-example/blob/master/multi-task-learning-example.ipynb)是论文作者提供的 Keras 实现。

### **第 2 课 – 调整学习率**

学习率是[调整神经网络](https://engineering.taboola.com/hitchhikers-guide-hyperparameter-tuning/)中最重要的超参数之一，这是一个常见的约定。所以我们尝试了调整，并找到了一种对任务 A 非常有效的学习率，以及一种对任务 B 非常有效的学习率。选择较高的学习率会导致[死 ReLU 问题](https://www.quora.com/What-is-the-dying-ReLU-problem-in-neural-networks)出现在一个任务上，而使用较低的学习率则会导致另一个任务收敛缓慢。那么我们能做什么呢？我们可以为每个“头”（任务特定子网）调整一个单独的学习率，并为共享子网调整另一个学习率。

虽然听起来可能很复杂，但实际上非常简单。通常在 TensorFlow 中训练神经网络时，你会使用类似的东西：

```py
optimizer = tf.train.AdamOptimizer(learning_rate).minimize(loss)
```

*AdamOptimizer*定义了如何应用梯度，而*minimize*计算并应用它们。我们可以用自己的实现替代*minimize*，在应用梯度时使用计算图中每个变量的适当学习率：

```py
all_variables = shared_vars + a_vars + b_vars
all_gradients = tf.gradients(loss, all_variables)

shared_subnet_gradients = all_gradients[:len(shared_vars)]
a_gradients = all_gradients[len(shared_vars):len(shared_vars + a_vars)]
b_gradients = all_gradients[len(shared_vars + a_vars):]

shared_subnet_optimizer = tf.train.AdamOptimizer(shared_learning_rate)
a_optimizer = tf.train.AdamOptimizer(a_learning_rate)
b_optimizer = tf.train.AdamOptimizer(b_learning_rate)

train_shared_op = shared_subnet_optimizer.apply_gradients(zip(shared_subnet_gradients, shared_vars))
train_a_op = a_optimizer.apply_gradients(zip(a_gradients, a_vars))
train_b_op = b_optimizer.apply_gradients(zip(b_gradients, b_vars))

train_op = tf.group(train_shared_op, train_a_op, train_b_op)
```

顺便提一下，这个技巧实际上对单任务网络也有用。

### **第 3 课 – 将估计值用作特征**

一旦我们完成了创建一个预测多个任务的神经网络的第一阶段，我们可能想要将一个任务的估计值用作另一个任务的特征。在前向传播中这很简单。估计值是一个张量，因此我们可以像连接其他层的输出一样连接它。但反向传播中会发生什么？

假设任务 A 的估计值被作为特征传递给任务 B。我们可能不希望将任务 B 的梯度反向传播到任务 A，因为我们已经有了任务 A 的标签。

不用担心，TensorFlow 的 API 有[tf.stop_gradient](https://www.tensorflow.org/api_docs/python/tf/stop_gradient)正是为此目的。当计算梯度时，它允许你传递一个张量列表，你希望将其视为常量，这正是我们需要的。

```py
all_gradients = tf.gradients(loss, all_variables, stop_gradients=stop_tensors)
```

再次强调，这在 MTL 网络中很有用，但不仅限于此。这个技术可以用于任何时候你想用 TensorFlow 计算一个值，并且需要假装这个值是一个常量。例如，在训练生成对抗网络 (GANs) 时，你不希望通过对抗样本的生成过程进行反向传播。

### **那么，接下来是什么？**

我们的模型已上线运行，Taboola 的信息流正在个性化。然而，仍然有很多改进空间，还有许多有趣的架构可以探索。在我们的用例中，预测多个任务也意味着我们要基于多个 KPI 做出决策。这可能比使用单一 KPI 更具挑战性……但这已经是另一个全新的话题。

感谢阅读，希望你觉得这篇文章有用！

**个人简介**： [Zohar Komarovsky](https://www.linkedin.com/in/zohar-komarovsky-7773b9b/) 是 Taboola 的算法开发者，专注于推荐系统的机器学习应用。

[原文](https://engineering.taboola.com/deep-multi-task-learning-3-lessons-learned)。已获得许可重新发布。

**资源：**

+   [在线和基于网络的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [热门深度学习 GitHub 仓库](https://www.kdnuggets.com/2019/02/trending-top-deep-learning-github-repositories.html)

+   [2018 年最重要的机器学习/人工智能进展是什么？](https://www.kdnuggets.com/2019/01/machine-learning-ai-advances-2018.html)

+   [NLP 概述：应用于自然语言处理的现代深度学习技术](https://www.kdnuggets.com/2019/01/nlp-overview-modern-deep-learning-techniques.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [我从使用 ChatGPT 进行数据科学中学到了什么](https://www.kdnuggets.com/what-i-learned-from-using-chatgpt-for-data-science)

+   [实施推荐系统在商业中的十个关键经验](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [来自高级数据科学家的经验教训](https://www.kdnuggets.com/2022/09/lessons-senior-data-scientist.html)

+   [KDnuggets 新闻，9 月 28 日：Python 免费算法课程 •…](https://www.kdnuggets.com/2022/n38.html)

+   [4 个帮助我应对困难就业市场的职业教训](https://www.kdnuggets.com/2023/05/4-lessons-made-difference-navigating-current-job-market.html)

+   [学习数据科学、机器学习和深度学习的可靠计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)
