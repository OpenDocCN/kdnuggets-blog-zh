# 《最温和的 Tensorflow 入门 – 第二部分》

> 原文：[https://www.kdnuggets.com/2016/08/gentlest-introduction-tensorflow-part-2.html/2](https://www.kdnuggets.com/2016/08/gentlest-introduction-tensorflow-part-2.html/2)

### 训练变化。

**随机、小批量、批量**

在上述训练中，我们在每个时期喂入一个数据点。这被称为*随机*梯度下降。我们可以在每个时期喂入一批数据点，这被称为*小批量*梯度下降，甚至可以在每个时期喂入所有数据点，这被称为*批量*梯度下降。请参见下面的图形比较，并注意三张图之间的两个差异：

+   每个时期喂入 TF.Graph 的数据点数量（每个图的右上角）。

+   梯度下降优化器在调整 W、b 以降低成本时需要考虑的数据点数量（每个图的右下角）。

![Image](../Images/b79b5679f1ceeb28278aaab148182ea2.png)

*随机梯度下降*

![Image](../Images/33557bec9cb92d4fdb6e45778da7e05d.png)

*小批量梯度下降*

![Image](../Images/427edda0188c06a578eea475cdd2f31c.png)

*批量梯度下降*

每个时期使用的数据点数量有两个影响。使用更多的数据点：

+   计算资源（减法、平方和加法）用于计算成本和执行梯度下降的需求增加。

+   模型学习和泛化的速度增加。

随机、小批量、批量梯度下降的优缺点可以在下面的图表中总结：

![Image](../Images/4dd4d20bd311d1735880a43acfa54df3.png)

*随机、小批量和批量梯度下降的优缺点*

要在随机/小批量/批量梯度下降之间切换，我们只需要在将数据点输入训练步骤 [D] 之前准备不同的批量大小，即使用下面的代码片段用于 [C]：

```py
# * all_xs: All the feature values
# * all_ys: All the outcome values
# datapoint_size: Number of points/entries in all_xs/all_ys
# batch_size: Configure this to:
#             1: stochastic mode
#             integer < datapoint_size: mini-batch mode
#             datapoint_size: batch mode
# i: Current epoch number

if datapoint_size == batch_size:
  # Batch mode so select all points starting from index 0
  batch_start_idx = 0
elif datapoint_size < batch_size:
  # Not possible
  raise ValueError(“datapoint_size: %d, must be greater than         
                    batch_size: %d” % (datapoint_size, batch_size))
else:
  # stochastic/mini-batch mode: Select datapoints in batches
  #                             from all possible datapoints
  batch_start_idx = (i * batch_size) % (datapoint_size — batch_size)
  batch_end_idx = batch_start_idx + batch_size
  batch_xs = all_xs[batch_start_idx:batch_end_idx]
  batch_ys = all_ys[batch_start_idx:batch_end_idx]

# Get batched datapoints into xs, ys, which is fed into
# 'train_step'
xs = np.array(batch_xs)
ys = np.array(batch_ys)
```

**学习率变化**

学习率是指我们希望梯度下降调整 W、b 时增减的幅度。使用较小的学习率，我们会缓慢而稳妥地接近最小成本，但使用较大的学习率，我们可以更快地达到最小成本，但存在“超调”的风险，从而无法找到最小值。

为了克服这个问题，许多机器学习从业者在初始阶段使用较大的学习率（假设初始成本离最小值很远），然后在每个时期后逐渐降低学习率。

TF 提供了两种方法，如此 StackOverflow [讨论](http://stackoverflow.com/questions/33919948/how-to-set-adaptive-learning-rate-for-gradientdescentoptimizer) 中精彩解释，但这里是总结。

使用梯度下降优化器变体。

TF 提供了多种梯度下降优化器，支持学习率变化，例如 [tf.train.AdagradientOptimizer](http://www.tensorflow.org/api_docs/python/train.html#AdagradOptimizer) 和 [tf.train.AdamOptimizer](http://www.tensorflow.org/api_docs/python/train.html#AdamOptimizer)。

使用*tf.placeholder*来设置学习率

正如你之前学到的，如果我们声明一个*tf.placeholder*，在这种情况下用于学习率，并在*tf.train.GradientDescentOptimizer*中使用它，我们可以在每个训练epoch中向其提供不同的值，就像我们在每个epoch中向x、y_（它们也是*tf.placeholders*）提供不同的数据点一样。

我们需要2个小修改：

```py
# Modify [B] to make 'learn_rate' a 'tf.placeholder'
# and supply it to the 'learning_rate' parameter name of
# tf.train.GradientDescentOptimizer
learn_rate = tf.placeholder(tf.float32, shape=[])
train_step = tf.train.GradientDescentOptimizer(
    learning_rate=learn_rate).minimize(cost)

# Modify [D] to include feed a 'learn_rate' value,
# which is the 'initial_learn_rate' divided by
# 'i' (current epoch number)
# NOTE: Oversimplified. For example only.
feed = { x: xs, y_: ys, learn_rate: initial_learn_rate/i }
sess.run(train_step, feed_dict=feed)
```

### 总结

我们展示了什么是机器学习的“训练”，以及如何仅通过模型和成本定义，循环进行训练步骤来使用Tensorflow，这些步骤将数据点输入到梯度下降优化器中。我们还讨论了训练中的常见变体，即每个epoch中模型用于学习的数据点大小的变化，以及梯度下降优化器的学习率的变化。

**接下来**

+   设置Tensor Board以可视化TF执行，检测模型、成本函数或梯度下降中的问题

+   使用多个特征进行线性回归

**资源**

+   代码可以在 [Github](https://github.com/nethsix/gentle_tensorflow/blob/master/code/linear_regression_one_feature_using_mini_batch_with_tensorboard.py) 上找到

+   关于此主题的幻灯片可在 [SlideShare](http://www.slideshare.net/KhorSoonHin/gentlest-intro-to-tensorflow-part-2-62006222) 上查看

+   视频可在 [YouTube](https://www.youtube.com/watch?v=Trc52FvMLEg) 上观看

**简介: [Soon Hin Khor](https://twitter.com/neth_6)**，博士，致力于通过技术使世界更加关怀和负责任。ruby-tensorflow的贡献者。东京Tensorflow meetup的共同组织者。

[原文](https://medium.com/@khor/gentlest-introduction-to-tensorflow-part-2-ed2a0a7a624f). 经许可转载。

**相关内容:**

+   [TensorFlow的优点、缺点和不足](/2016/05/good-bad-ugly-tensorflow.html)

+   [Tensorflow中的多任务学习：第1部分](/2016/07/multi-task-learning-tensorflow-part-1.html)

+   [理解深度学习的7个步骤](/2016/01/seven-steps-deep-learning.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的IT需求

* * *

### 更多相关主题

+   [TensorFlow用于计算机视觉 - 转移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [PyTorch还是TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [Tensorflow的“Hello World”](https://www.kdnuggets.com/2022/05/hello-world-tensorflow.html)

+   [使用 TensorFlow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [使用 TensorFlow 和 Keras 构建并训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [免费 TensorFlow 2.0 完整课程](https://www.kdnuggets.com/2023/02/free-tensorflow-20-complete-course.html)
