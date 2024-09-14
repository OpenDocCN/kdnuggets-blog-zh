# 构建一个基本的 Keras 神经网络 Sequential 模型

> 原文：[https://www.kdnuggets.com/2018/06/basic-keras-neural-network-sequential-model.html](https://www.kdnuggets.com/2018/06/basic-keras-neural-network-sequential-model.html)

[评论](#comments)

正如标题所示，本篇文章介绍了如何使用 Sequential 模型 API 构建一个基本的 Keras 神经网络。这里的具体任务是一个常见任务（在 MNIST 数据集上训练分类器），但这可以被视为处理任何类似任务的模板示例。

这种方法基本上与 Chollet 的 [Keras 4 步工作流程](/2018/06/keras-4-step-workflow.html) 相符，他在他的书 "[Python 深度学习](https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438)" 中概述了这一流程，实际上无非就是在书的早期章节或官方 Keras 教程中可以找到的示例。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT

* * *

制作这样一篇文章的动机是将其作为一系列即将发布的文章的基础参考，这些文章将以不同的方式配置 Keras。这似乎比在每篇文章开头一遍又一遍地覆盖相同的内容要更好。对于那些没有在 Keras 中构建神经网络经验的人来说，内容应该本身就很有用。

![标题图片](../Images/2f500fab517e06ac7164e13b054b02a1.png)

图片来自 [Keras 文档网站](https://keras.io/models/sequential/) 的截图

使用的数据集是 MNIST，构建的模型是一个密集层的 Sequential 网络，暂时故意避免使用 CNN。

首先是导入库和一些超参数以及数据调整大小的变量。

```py
from keras import models
from keras.layers import Dense, Dropout
from keras.utils import to_categorical
from keras.datasets import mnist
from keras.utils.vis_utils import model_to_dot
from IPython.display import SVG

import livelossplot
plot_losses = livelossplot.PlotLossesKeras()

NUM_ROWS = 28
NUM_COLS = 28
NUM_CLASSES = 10
BATCH_SIZE = 128
EPOCHS = 10

```

接下来是一个用于输出我们数据集的简单（但有用的）元数据的函数。由于我们会使用几次，将这些几行代码放在一个可调用的函数中是有意义的。可重用的代码本身就是一个目标 :)

```py
def data_summary(X_train, y_train, X_test, y_test):
    """Summarize current state of dataset"""
    print('Train images shape:', X_train.shape)
    print('Train labels shape:', y_train.shape)
    print('Test images shape:', X_test.shape)
    print('Test labels shape:', y_test.shape)
    print('Train labels:', y_train)
    print('Test labels:', y_test)

```

接下来我们加载我们的数据集（MNIST，使用 Keras 的数据集工具），然后使用上述函数获取一些数据集元数据。

```py
# Load data
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# Check state of dataset
data_summary(X_train, y_train, X_test, y_test)

```

```py

Train images shape: (60000, 28, 28)
Train labels shape: (60000,)
Test images shape: (10000, 28, 28)
Test labels shape: (10000,)
Train labels: [5 0 4 ... 5 6 8]
Test labels: [7 2 1 ... 4 5 6]
```

为了将 MNIST 实例输入神经网络，它们需要从二维图像表示转换为一维序列。我们还将类向量转换为二进制矩阵（使用 `to_categorical`）。下面完成了这一转换，然后再次调用之前定义的相同函数以展示数据重塑的效果。

```py
# Reshape data
X_train = X_train.reshape((X_train.shape[0], NUM_ROWS * NUM_COLS))
X_train = X_train.astype('float32') / 255
X_test = X_test.reshape((X_test.shape[0], NUM_ROWS * NUM_COLS))
X_test = X_test.astype('float32') / 255

# Categorically encode labels
y_train = to_categorical(y_train, NUM_CLASSES)
y_test = to_categorical(y_test, NUM_CLASSES)

# Check state of dataset
data_summary(X_train, y_train, X_test, y_test)

```

```py

Train images shape: (60000, 784)
Train labels shape: (60000, 10)
Test images shape: (10000, 784)
Test labels shape: (10000, 10)
Train labels: [[0\. 0\. 0\. ... 0\. 0\. 0.]
 [1\. 0\. 0\. ... 0\. 0\. 0.]
 [0\. 0\. 0\. ... 0\. 0\. 0.]
 ...
 [0\. 0\. 0\. ... 0\. 0\. 0.]
 [0\. 0\. 0\. ... 0\. 0\. 0.]
 [0\. 0\. 0\. ... 0\. 1\. 0.]]
Test labels: [[0\. 0\. 0\. ... 1\. 0\. 0.]
 [0\. 0\. 1\. ... 0\. 0\. 0.]
 [0\. 1\. 0\. ... 0\. 0\. 0.]
 ...
 [0\. 0\. 0\. ... 0\. 0\. 0.]
 [0\. 0\. 0\. ... 0\. 0\. 0.]
 [0\. 0\. 0\. ... 0\. 0\. 0.]]
```

所有所需的数据转换都已完成。现在是时候构建、编译和训练神经网络了。你可以在 [这篇文章](/2018/06/keras-4-step-workflow.html) 中查看更多关于此过程的内容。

```py
# Build neural network
model = models.Sequential()
model.add(Dense(512, activation='relu', input_shape=(NUM_ROWS * NUM_COLS,)))
model.add(Dropout(0.5))
model.add(Dense(256, activation='relu'))
model.add(Dropout(0.25))
model.add(Dense(10, activation='softmax'))

# Compile model
model.compile(optimizer='rmsprop',
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Train model
model.fit(X_train, y_train,
          batch_size=BATCH_SIZE,
          epochs=EPOCHS,
          callbacks=[plot_losses],
          verbose=1,
          validation_data=(X_test, y_test))

score = model.evaluate(X_test, y_test, verbose=0)
print('Test loss:', score[0])
print('Test accuracy:', score[1])

```

唯一的不传统步骤（就使用 Keras 库而言）是使用 [Live Loss Plot 回调](https://github.com/stared/livelossplot)，该回调在每个训练周期结束时输出逐周期的损失函数和准确率。确保在运行上述代码之前已安装 Live Loss Plot。我们还给出了测试数据集上的最终损失和准确率。

![图片](../Images/a2adbfb18cdab3aa422284c8325f9b93.png)

```py

Test loss: 0.09116139834111646
Test accuracy: 0.9803
```

几乎完成了，但首先让我们输出我们构建的神经网络的总结。

```py
# Summary of neural network
model.summary()

```

```py

Layer (type)                 Output Shape              Param #   
=================================================================
dense_1 (Dense)              (None, 512)               401920    
_________________________________________________________________
dropout_1 (Dropout)          (None, 512)               0         
_________________________________________________________________
dense_2 (Dense)              (None, 256)               131328    
_________________________________________________________________
dropout_2 (Dropout)          (None, 256)               0         
_________________________________________________________________
dense_3 (Dense)              (None, 10)                2570      
=================================================================
Total params: 535,818
Trainable params: 535,818
Non-trainable params: 0

```

最后，视觉化模型：

```py
# Output network visualization
SVG(model_to_dot(model).create(prog='dot', format='svg'))

```

![图片](../Images/18b8a2587d440a56daa34f40129a7bfb.png)

完整代码如下：

如前所述，这篇文章并没有涉及任何创新内容，但我们将发布一系列使用 Keras 的后续文章，希望能更吸引读者，并且这个常见的起点对参考应该是有益的。

此外，对于那些寻找使用 Keras Sequential 模型构建神经网络的简化方法的读者，这篇文章应该能作为一个基础指南，涵盖沿途所有重要的点。训练后的工作由你决定（目前），但我们未来也会再次讨论这个话题。

**相关**：

+   [Keras 4 步工作流](/2018/06/keras-4-step-workflow.html)

+   [掌握 Keras 深度学习的 7 个步骤](/2017/10/seven-steps-deep-learning-keras.html)

+   [今天我在午休时使用 Keras 构建了一个神经网络](/2017/12/today-built-neural-network-during-lunch-break-keras.html)

### 更多相关主题

+   [使用 TensorFlow 和 Keras 构建并训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [通过构建 15 个神经网络项目在 2022 年学习深度学习](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [神经网络预测中排列的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)

+   [Keras 3.0：你需要知道的一切](https://www.kdnuggets.com/2023/07/keras-30-everything-need-know.html)
