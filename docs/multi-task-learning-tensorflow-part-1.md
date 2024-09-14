# Tensorflow 中的多任务学习：第一部分

> 原文：[`www.kdnuggets.com/2016/07/multi-task-learning-tensorflow-part-1.html/2`](https://www.kdnuggets.com/2016/07/multi-task-learning-tensorflow-part-1.html/2)

### 交替训练

第一个解决方案特别适合于你会有一批任务 1 的数据，然后是一批任务 2 的数据的情况。

记住，Tensorflow 会自动确定操作所需的计算，并仅进行这些计算。这意味着**如果我们只在一个任务上定义优化器，它只会训练计算该任务所需的参数——并且不会动其他参数**。由于任务 1 仅依赖于任务 1 和共享层，任务 2 层将不会被触及。让我们画另一个图示，显示每个任务末尾的期望优化器。

![带有优化器的图表](img/5bd03318ff44c0d9530716319655d482.png)

```py
#  GRAPH CODE
# ============

# Import Tensorflow and Numpy
import Tensorflow as tf
import numpy as np

# ======================
# Define the Graph
# ======================

# Define the Placeholders
X = tf.placeholder("float", [10, 10], name="X")
Y1 = tf.placeholder("float", [10, 20], name="Y1")
Y2 = tf.placeholder("float", [10, 20], name="Y2")

# Define the weights for the layers

initial_shared_layer_weights = np.random.rand(10,20)
initial_Y1_layer_weights = np.random.rand(20,20)
initial_Y2_layer_weights = np.random.rand(20,20)

shared_layer_weights = tf.Variable(initial_shared_layer_weights, name="share_W", dtype="float32")
Y1_layer_weights = tf.Variable(initial_Y1_layer_weights, name="share_Y1", dtype="float32")
Y2_layer_weights = tf.Variable(initial_Y2_layer_weights, name="share_Y2", dtype="float32")

# Construct the Layers with RELU Activations
shared_layer = tf.nn.relu(tf.matmul(X,shared_layer_weights))
Y1_layer = tf.nn.relu(tf.matmul(shared_layer,Y1_layer_weights))
Y2_layer = tf.nn.relu(tf.matmul(shared_layer,Y2_layer_weights))

# Calculate Loss
Y1_Loss = tf.nn.l2_loss(Y1-Y1_layer)
Y2_Loss = tf.nn.l2_loss(Y2-Y2_layer)

# optimisers
Y1_op = tf.train.AdamOptimizer().minimize(Y1_Loss)
Y2_op = tf.train.AdamOptimizer().minimize(Y2_Loss)

```

我们可以通过交替调用每个任务的优化器来进行多任务学习，这意味着我们可以不断将一些信息从一个任务转移到另一个任务。从宽泛的意义上讲，我们正在发现任务之间的“共性”。以下代码实现了这一点，如果你在跟随教程，请将其粘贴到之前代码的底部：

```py
# Calculation (Session) Code
# ==========================

# open the session

with tf.Session() as session:
    session.run(tf.initialize_all_variables())
    for iters in range(10):
        if np.random.rand() < 0.5:
            _, Y1_loss = session.run([Y1_op, Y1_Loss],
                            {
                              X: np.random.rand(10,10)*10,
                              Y1: np.random.rand(10,20)*10,
                              Y2: np.random.rand(10,20)*10
                              })
            print(Y1_loss)
        else:
            _, Y2_loss = session.run([Y2_op, Y2_Loss],
                            {
                              X: np.random.rand(10,10)*10,
                              Y1: np.random.rand(10,20)*10,
                              Y2: np.random.rand(10,20)*10
                              })
            print(Y2_loss)

```

**提示：何时交替训练效果较好？**

当你为不同任务有两个不同的数据集时（例如，从英语翻译成法语和英语翻译成德语），交替训练是个好主意。通过这种方式设计网络，你可以在不需要找到更多任务特定训练数据的情况下提高每个任务的性能。

交替训练是你最常遇到的情况，因为没有多少数据集有两个或更多的输出。我们会讲一个例子，但最清晰的例子是你想在任务中建立层次结构。例如，在视觉任务中，你可能希望一个任务预测物体的旋转，另一个任务预测如果你改变相机角度物体会是什么样子。这两个任务显然是相关的——实际上，旋转可能发生在图像生成之前。

**提示：何时交替训练效果不佳？**

交替训练容易对特定任务产生偏差。第一种方式很明显——如果你的一个任务的数据集比另一个任务大得多，那么如果你按数据集大小的比例进行训练，你的共享层将包含关于更重要任务的更多信息。

第二种情况则不然。如果你交替训练，模型中的最终任务将对参数产生偏差。没有明显的方法可以克服这个问题，但这确实意味着在你不需要交替训练的情况下，你不应该这样做。

### 同时训练 - 联合训练

当你有一个包含多个标签的数据集时，你真正需要的是同时训练这些任务。问题是，如何保持任务特定函数的独立性？答案出奇简单 - 你只需将各个任务的损失函数相加，并在此基础上进行优化。下面是一个展示可以联合训练的网络的图示，附有相应的代码：

![联合训练](img/7e111a03e0943c04ebd6be7be88209f6.png)

```py
#  GRAPH CODE
# ============

# Import Tensorflow and Numpy
import Tensorflow as tf
import numpy as np

# ======================
# Define the Graph
# ======================

# Define the Placeholders
X = tf.placeholder("float", [10, 10], name="X")
Y1 = tf.placeholder("float", [10, 20], name="Y1")
Y2 = tf.placeholder("float", [10, 20], name="Y2")

# Define the weights for the layers

initial_shared_layer_weights = np.random.rand(10,20)
initial_Y1_layer_weights = np.random.rand(20,20)
initial_Y2_layer_weights = np.random.rand(20,20)

shared_layer_weights = tf.Variable(initial_shared_layer_weights, name="share_W", dtype="float32")
Y1_layer_weights = tf.Variable(initial_Y1_layer_weights, name="share_Y1", dtype="float32")
Y2_layer_weights = tf.Variable(initial_Y2_layer_weights, name="share_Y2", dtype="float32")

# Construct the Layers with RELU Activations
shared_layer = tf.nn.relu(tf.matmul(X,shared_layer_weights))
Y1_layer = tf.nn.relu(tf.matmul(shared_layer,Y1_layer_weights))
Y2_layer = tf.nn.relu(tf.matmul(shared_layer,Y2_layer_weights))

# Calculate Loss
Y1_Loss = tf.nn.l2_loss(Y1-Y1_layer)
Y2_Loss = tf.nn.l2_loss(Y2-Y2_layer)
Joint_Loss = Y1_Loss + Y2_Loss

# optimisers
Optimiser = tf.train.AdamOptimizer().minimize(Joint_Loss)
Y1_op = tf.train.AdamOptimizer().minimize(Y1_Loss)
Y2_op = tf.train.AdamOptimizer().minimize(Y2_Loss)

# Joint Training
# Calculation (Session) Code
# ==========================

# open the session

with tf.Session() as session:
    session.run(tf.initialize_all_variables())
    _, Joint_Loss = session.run([Optimiser, Joint_Loss],
                    {
                      X: np.random.rand(10,10)*10,
                      Y1: np.random.rand(10,20)*10,
                      Y2: np.random.rand(10,20)*10
                      })
    print(Joint_Loss)

```

### 结论和下一步

在这篇文章中，我们已经探讨了深度神经网络中多任务学习的基本原理。如果你之前使用过 Tensorflow，并且有自己的项目，希望这能为你提供足够的启示来开始。

对于那些想要更详细、具体的示例来展示如何提高多任务性能的人，请关注教程的第二部分，我们将*深入*探讨自然语言处理，构建一个用于浅层解析和词性标注的多任务模型。

**个人简介：[Jonathan Godwin](https://jg8610.github.io/)** 目前在 UCL 学习机器学习硕士，专攻深度多任务学习和自然语言处理。他将于九月完成学业，并寻求可以在有趣问题上运用这一技能的工作或研究岗位。

[原文](https://jg8610.github.io/Multi-Task/)。经授权转载。

**相关：**

+   TensorFlow 中的递归网络介绍

+   TensorFlow 的优缺点

+   Scikit Flow：使用 TensorFlow 和 Scikit-learn 轻松深度学习

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多此主题

+   [TensorFlow 计算机视觉 - 简化迁移学习](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [PyTorch 还是 TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [Tensorflow 的“Hello World”](https://www.kdnuggets.com/2022/05/hello-world-tensorflow.html)

+   [使用 Tensorflow 训练图像分类模型指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [使用 TensorFlow 和 Keras 构建并训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [免费 TensorFlow 2.0 完整课程](https://www.kdnuggets.com/2023/02/free-tensorflow-20-complete-course.html)
