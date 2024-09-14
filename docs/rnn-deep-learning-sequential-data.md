# 循环神经网络（RNN）：用于序列数据的深度学习

> 原文：[https://www.kdnuggets.com/2020/07/rnn-deep-learning-sequential-data.html](https://www.kdnuggets.com/2020/07/rnn-deep-learning-sequential-data.html)

[评论](#comments)

循环神经网络（RNN）是一类人工神经网络，可以在[深度学习](https://blog.exxactcorp.com/category/deep-learning/)中处理一系列输入，并在处理下一序列的输入时保留其状态。传统神经网络会处理一个输入并移动到下一个，忽略其序列。诸如时间序列的数据具有需要遵循的顺序以便理解。传统的前馈网络无法理解这一点，因为每个输入被假定为彼此独立，而在时间序列设置中，每个输入依赖于先前的输入。

![图像](../Images/ebdc0da1eb3f72438224bb2b01b4b3f4.png)

[插图 1: 来源](https://www.dummies.com/programming/big-data/data-science/deep-learning-and-recurrent-neural-networks/)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在插图 1 中，我们看到神经网络（隐藏状态）*A*接收一个*x**t*并输出一个值*h**t*。循环展示了信息是如何从一个步骤传递到下一个步骤的。输入是“JAZZ”的各个字母，每个字母按顺序传递给网络 A（即顺序处理）。

循环神经网络可以用于多种方式，例如：

+   检测下一个单词/字母

+   在时间空间中预测金融资产价格

+   体育中的动作建模（预测体育赛事中的下一动作，例如足球、橄榄球、网球等）

+   音乐创作

+   图像生成

### **RNN 与自回归模型**

自回归模型是指将具有时间维度的数据的值回归到先前的值，直到用户指定的某个点。RNN的工作方式相同，但明显的区别在于RNN查看所有数据（即不需要用户指定特定的时间段）。

***Y******t ******= β******0****** + β******1******y******t-1****** + Ɛ******t***

上述 AR 模型是一个阶数为 1 的 AR(1) 模型，它以直接前值来预测下一时间段的值 (yt)。由于这是一个线性模型，它需要某些线性回归的假设，特别是因变量和自变量之间的线性假设。在这种情况下，***Y******t ***和 ***y******t-1 ***必须具有线性关系。此外，还有其他检查，例如自相关检查，需要确定预测 ***Y******t******的适当阶数。***

循环神经网络无需线性或模型阶数检查。它可以自动检查整个数据集以预测下一序列。如下面的图像所示，一个神经网络包含3个隐藏层，具有相等的权重、偏置和激活函数，并用于预测输出。

![图像](../Images/17cf506bfd51128920b1b80d5e555bb9.png)

[来源](https://hackernoon.com/rnn-or-recurrent-neural-network-for-noobs-a9afbb00e860)

这些隐藏层可以合并成一个单一的递归隐藏层。递归神经元现在存储所有之前步骤的输入，并将这些信息与当前步骤的输入合并。

![图像](../Images/13f5e5872ab6f20539c8e81982e67afb.png)

[来源](https://hackernoon.com/rnn-or-recurrent-neural-network-for-noobs-a9afbb00e860)

****循环神经网络的优点****

+   它可以建模非线性时间/序列关系。

+   相比于自回归过程，无需指定滞后值来预测下一个值。

****循环神经网络的缺点****

+   消失梯度问题

+   不适合预测长时间跨度

### **消失梯度问题**

随着更多含激活函数的层被添加，损失函数的梯度趋近于零。梯度下降算法找到网络成本函数的全局最小值。浅层网络不应该受到过小梯度的影响，但随着网络的增大及隐藏层的增加，这可能导致梯度过小，影响模型训练。

神经网络的梯度通过反向传播算法计算，其中你需要找到网络的导数。通过链式法则，逐层计算导数并将其相乘。这就是问题所在。使用像 sigmoid 函数这样的激活函数时，梯度有可能随着隐藏层的增加而减小。

这个问题可能导致模型编译后的结果很糟糕。解决这个问题的简单方法是使用具有 ReLU 激活函数的长短期记忆模型。

### **长短期记忆模型**

长短期记忆网络是一种特殊的递归神经网络，能够处理长期依赖问题，而不会受到梯度不稳定的影响。

![图像](../Images/b60656880e1eccebad51931e5ae7c94b.png)

[来源](https://www.superdatascience.com/blogs/recurrent-neural-networks-rnn-lstm-variation/)

上面的图是一个典型的 RNN，只是重复模块包含额外的层，使其与 RNN 区别开来。

这里的区分是水平线称为“单元状态”，它充当信息的传送带。LSTM 将通过使用如上所示的 3 个门来删除或添加单元状态中的信息。这些门由一个 sigmoid 函数和一个逐点乘法操作组成，输出值在 1 和 0 之间，用于描述允许单元状态通过多少成分。值为 1 表示允许所有信息通过，而 0 则表示完全忽略它。

**3 个门是：**

**1\. 输入门**

+   这个门用于发现哪些值将用于修改记忆，通过使用 sigmoid 函数（分配介于 0 和 1 之间的值），然后使用 tanh 函数为值赋予 -1 到 1 之间的权重。

**2\. 忘记门**

+   忘记门使用 sigmoid 函数来决定哪些值被忽略，通过使用前一个状态 (*h**t-1*) 和输入 (*x**t*)，为 *c**t-1* 中的每个值分配一个介于 0 和 1 之间的值。

**3\. 输出门**

+   该块的输入用于决定输出，通过使用 sigmoid 函数分配介于 0 和 1 之间的值，然后乘以 tanh 函数来决定其重要性级别，分配一个介于 -1 和 1 之间的值。

### **编写 RNN – LSTM**

```py
## Libraries

import tensorflow as tf

model = tf.keras.models.Sequential()
Dense = tf.keras.layers.Dense
Dropout = tf.keras.layers.Dropout
LSTM = tf.keras.layers.LSTM

## Dataset
mnist_data = tf.keras.datasets.mnist # mnist is a dataset of 28x28 images of handwritten digits and their labels with 60,000 rows of data

## Create train and test data
(x_train, y_train),(x_test, y_test) = mnist_data.load_data()

x_train = x_train/255.0 # Normalize training data features
x_test = x_test/255.0 # Normalize training data labels

# The images are 28x28 pixels of unassigned integers in the range of 0 to 255\. The above #normalization code is not necessary and can still be passed on to compile. 
# However, the #accuracy will be much worse of at around 20% best case scenario and loss at over 90%. The #training time will also increase.

model.add(LSTM(256, activation='relu', return_sequences=True))
model.add(Dropout(0.2))
model.add(LSTM(256, activation='relu'))
model.add(Dropout(0.1))
model.add(Dense(32, activation='relu'))
model.add(Dropout(0.2))
model.add(Dense(10, activation='softmax'))

optimizer = tf.keras.optimizers.Adam(lr=1e-4, decay=1e-6)

# Compile model
model.compile(
   loss='sparse_categorical_crossentropy',
   optimizer=optimizer,
   metrics=['accuracy'],
)

# The specification of loss=’sparse_categorical_crossentropy’ is very important here as our targets are # integers and not one-hot encoded categories.
model.fit(x_train,
   y_train,
   epochs=4,
   validation_data=(x_test, y_test))
```

```py
Epoch 1/4
60000/60000 [==============================] - 278s 5ms/sample - loss: 0.9960 - acc: 0.6611 - val_loss: 0.2939 - val_acc: 0.9013

Epoch 2/4
60000/60000 [==============================] - 276s 5ms/sample - loss: 0.2955 - acc: 0.9107 - val_loss: 0.1523 - val_acc: 0.9504

Epoch 3/4
60000/60000 [==============================] - 273s 5ms/sample - loss: 0.1931 - acc: 0.9452 - val_loss: 0.1153 - val_acc: 0.9641

Epoch 4/4
60000/60000 [==============================] - 270s 4ms/sample - loss: 0.1489 - acc: 0.9581 - val_loss: 0.1076 - val_acc: 0.9696
```

[原文](https://blog.exxactcorp.com/recurrent-neural-networks-rnn-deep-learning-for-sequential-data/)。经许可转载。

**相关内容：**

+   [LSTM 用于时间序列预测](/2020/04/lstm-time-series-prediction.html)

+   [NLP 中的深度学习：ANNs、RNNs 和 LSTMs 解析！](/2019/08/deep-learning-nlp-explained.html)

+   [理解应用于 LSTM 的反向传播](/2019/05/understanding-backpropagation-applied-lstm.html)

### 更多相关内容

+   [神经网络与深度学习：教科书（第2版）](https://www.kdnuggets.com/2023/07/aggarwal-neural-networks-deep-learning-textbook-2nd-edition.html)

+   [深度神经网络不会引导我们走向AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [使用最先进的深度学习进行可解释的预测和即时预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [在神经网络之前尝试的 10 件简单事物](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [使用 PyTorch 解释性神经网络](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

+   [使用卷积神经网络（CNNs）的图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)
