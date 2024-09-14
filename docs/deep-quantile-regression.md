# 深度分位回归

> 原文：[https://www.kdnuggets.com/2018/07/deep-quantile-regression.html](https://www.kdnuggets.com/2018/07/deep-quantile-regression.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Sachin Abeywardana](https://www.linkedin.com/in/sachinabeywardana/)，[DeepSchool.io](http://DeepSchool.io) 的创始人**

深度学习目前还没有广泛探讨估计中的不确定性。大多数深度学习框架目前专注于提供由损失函数定义的最佳估计。有时，除了点估计之外，还需要一些其他信息来做出决策。这时，分布将非常有用。贝叶斯统计非常适合这个问题，因为它推断了数据集上的分布。然而，迄今为止，贝叶斯方法的计算速度较慢，而且在大数据集上应用时会非常昂贵。

就决策而言，大多数人实际上需要的是分位数，而不是估计中的真正不确定性。例如，在测量一个特定年龄的孩子的体重时，个体的体重会有所不同。比较有趣的是（为了论证）第 10 和第 90 百分位数。请注意，不确定性不同于分位数，因为我可以要求第 90 分位数的置信区间。本文将纯粹关注推断分位数。

### 分位回归损失函数

在回归中，最常用的损失函数是均方误差函数。如果我们取这个损失的负值并进行指数运算，结果将对应于高斯分布。这个分布的模式（峰值）对应于均值参数。因此，当我们使用一个最小化该损失的神经网络进行预测时，我们是在预测训练集中可能存在噪声的输出均值。

分位回归中单个数据点的损失定义为：

![](../Images/40bfe34e63f6ca33ef68a47761ff042a.png)

单个数据点的损失

其中 alpha 是所需的分位数（一个介于 0 和 1 之间的值），

![](../Images/e2b5a8b41ef98af888d44ee4d2dd50bc.png)

其中 f(x) 是预测的（分位数）模型，y 是对应输入 x 的观测值。整个数据集的平均损失如下所示：

![](../Images/f1f0c04d829a5a7470942307398b8560.png)

损失函数

如果我们取单个损失的负值并进行指数运算，我们会得到下图所示的非对称拉普拉斯分布。**这个损失函数有效的原因是，如果我们找到图形左侧零点下的面积，它将是 alpha，即所需的分位数。**

![](../Images/d4b53cd78dff1dae2ba8d6546aa00ee3.png)

非对称拉普拉斯分布的概率密度函数（pdf）。

当 alpha=0.5 时，这种情况可能更为熟悉，因为它对应于平均绝对误差（MAE）。这个损失函数始终估计中位数（第50百分位数），而不是均值。

### 在 Keras 中建模

前向模型与进行 MSE 回归时没有不同。唯一改变的是损失函数。以下几行定义了上述部分中定义的损失函数。

```py

import keras.backend as K
def tilted_loss(q,y,f):
    e = (y-f)
    return K.mean(K.maximum(q*e, (q-1)*e), axis=-1)
```

在编译神经网络时，只需简单地执行：

```py

quantile = 0.5
model.compile(loss=lambda y,f: tilted_loss(quantile,y,f), optimizer='adagrad')
```

欲了解完整示例，请参见 [这个 Jupyter 笔记本](https://github.com/sachinruk/KerasQuantileModel/blob/master/Keras%20Quantile%20Model.ipynb)，我在其中查看了摩托车事故数据集随时间的变化。结果如下面所示，我展示了第10、第50和第90百分位数。

![](../Images/da4fd8f5e51325a415feff9c2ca70f14.png)

崩溃摩托车的加速度随时间的变化。

**最终说明**

1.  **注意，我必须对每个分位数重新运行训练。** 这是因为每个分位数的损失函数是不同的，因为分位数本身是损失函数的一个参数。

1.  由于每个模型都是简单的重新运行，存在分位数交叉的风险。即，第49百分位数在某些阶段可能超过第50百分位数。

1.  注意，分位数 0.5 与中位数相同，你可以通过最小化平均绝对误差（Mean Absolute Error）来获得中位数，而在 Keras 中，你可以使用 `loss='mae'` 来实现。

1.  不确定性和分位数 **不是** 同一回事。但大多数时候，你关心的是分位数，而不是不确定性。

1.  如果你真的想了解深度网络的不确定性，可以查看 [http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html](http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html)

**编辑 1**

正如 [Anders Christiansen](https://medium.com/@andersasac?source=post_info_responses---------0----------------)（在回应中）指出的那样，我们可以通过设置多个目标在一次运行中获得多个分位数。然而，Keras 通过 `loss_weights` 参数将所有损失函数结合在一起，如此处所示：[https://keras.io/getting-started/functional-api-guide/#multi-input-and-multi-output-models](https://keras.io/getting-started/functional-api-guide/#multi-input-and-multi-output-models)。在 TensorFlow 中实现这一点会更容易。如果有人比我更早完成这个任务，我愿意更新我的笔记本/帖子以反映这一点。作为一个粗略的指南，如果我们需要分位数 0.1、0.5 和 0.9，Keras 中的最后一层应为 `Dense(3)`，每个节点连接到一个损失函数。

**编辑 2**

感谢 [Jacob Zweig](https://medium.com/@jacob.zweig?source=post_info_responses---------1----------------) 实现了 TensorFlow 中的同时多分位数： [https://github.com/strongio/quantile-regression-tensorflow/blob/master/Quantile%20Loss.ipynb](https://github.com/strongio/quantile-regression-tensorflow/blob/master/Quantile%20Loss.ipynb)

如果你喜欢我的教程/博客文章，**考虑支持我** [https://www.patreon.com/deepschoolio](https://www.patreon.com/deepschoolio) 或订阅我的 YouTube 频道 [https://www.youtube.com/user/sachinabey](https://www.youtube.com/user/sachinabey)（或者两者都订阅！）。

**简历：[Sachin Abeywardana](https://www.linkedin.com/in/sachinabeywardana/)** 是机器学习博士及 [DeepSchool.io](http://DeepSchool.io) 创始人。

[原文](https://towardsdatascience.com/deep-quantile-regression-c85481548b5a)。经许可转载。

**相关：**

+   [DeepSchool.io：深度学习学习](/2017/12/deepschool-io-deep-learning-learning.html)

+   [数据科学的 Docker](/2018/01/docker-data-science.html)

+   [使用遗传算法优化递归神经网络](/2018/01/genetic-algorithm-optimizing-recurrent-neural-network.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关话题

+   [比较线性回归和逻辑回归](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [使用线性回归模型而不是…的 3 个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12, 3 月 23 日：最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [逻辑回归用于分类](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)
