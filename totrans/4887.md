# 构建神经网络的简单入门指南

> 原文：[https://www.kdnuggets.com/2018/02/simple-starter-guide-build-neural-network.html](https://www.kdnuggets.com/2018/02/simple-starter-guide-build-neural-network.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Jeff Hu](https://www.linkedin.com/in/yaochiehhu/)，机器学习爱好者**

![Header iamge](../Images/fc4c11c0a8e72faaf2ca753224789de7.png)

图片来源于 [https://media.scmagazine.com](https://media.scmagazine.com/)

从今天开始，你将能够通过 [PyTorch](http://pytorch.org/) 编程并构建一个原始的 [**前馈神经网络**](https://brilliant.org/wiki/feedforward-neural-networks/)（FNN）。这是 FNN 的 Python Jupyter 代码库：[https://github.com/yhuag/neural-network-lab](https://github.com/yhuag/neural-network-lab)

本指南作为基础实践工作，引导你从头开始构建神经网络。大多数数学概念和科学决策被省略。你可以自由研究更多内容。

### 入门

1. 请确保你的计算机上安装了 Python 和 PyTorch：

+   Python 3.6 ([安装](https://www.python.org/downloads/))

+   PyTorch ([安装](http://pytorch.org/))

2. 通过控制台命令检查 Python 安装的正确性：

```py
python -V
```

输出应为**Python 3.6.3**或更高版本

3. 打开一个存储库（文件夹）并创建你的第一个神经网络文件：

```py
mkdir fnn-tuto
cd fnn-tuto
touch fnn.py
```

### 开始编写代码

所有以下代码应写在**fnn.py**文件中

**导入 PyTorch**

它将 PyTorch 加载到代码中。太好了！好的开始是成功的一半。

**初始化超参数**

超参数是设置在前的强大参数，不会随神经网络训练而更新。

**下载 MNIST 数据集**

MNIST 是一个包含大量手写数字（即 0 到 9）的巨大数据库，旨在用于图像处理。

**加载数据集**

下载 MNIST 数据集后，我们将其加载到代码中。

***注意：****我们打乱了 train_dataset 的加载过程，以使学习过程独立于数据顺序，但 test_loader 的顺序保持不变，以检查我们是否能处理未指定的输入偏差顺序。*

### 构建前馈神经网络

现在我们已经准备好了数据集。我们将开始构建神经网络。概念性示意图如下：

![](../Images/5aa5e0ecf1e76a49324c6da5ec93a9c9.png)

FNN 图片来源于 [http://web.utk.edu/](http://web.utk.edu/)

**前馈神经网络模型结构**

FNN 包括两个全连接层（即 fc1 和 fc2）以及一个非线性 ReLU 层在中间。通常我们称这种结构为**1-hidden layer FNN**，不计算输出层（fc2）。

通过执行前向传播，输入图像（x）可以通过神经网络，生成一个输出（out），展示它属于 10 个类别中的每一个的可能性。*例如，一张猫的图像可能对狗类有 0.8 的可能性，对飞机类有 0.3 的可能性。*

**实例化 FNN**

我们现在根据我们的结构创建一个真正的 FNN。

```py
net = Net(input_size, hidden_size, num_classes)
```

**启用 GPU**

***注意：*** 你可以启用这一行以在 GPU 上运行代码

```py
# net.cuda()    # You can comment out this line to disable GPU
```

**选择损失函数和优化器**

损失函数（**criterion**）决定了如何将输出与类别进行比较，这决定了神经网络的表现好坏。而**优化器**选择了一种更新权重的方法，以便收敛并找到这个神经网络中最佳的权重。

```py
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(net.parameters(), lr=learning_rate)
```

### 训练 FNN 模型

这个过程可能需要 3 到 5 分钟，具体取决于你的机器。详细解释已在以下代码中的注释 (#) 中列出。

### 测试 FNN 模型

与训练神经网络类似，我们还需要加载批次的测试图像并收集输出。不同之处在于：

1.  无损失和权重计算

1.  无权重更新

1.  具有正确的预测计算

**保存训练好的 FNN 模型以备将来使用**

我们将训练好的模型保存为一个可以稍后加载和使用的[pickle](https://wiki.python.org/moin/UsingPickle)文件。

```py
torch.save(net.state_dict(), ‘fnn_model.pkl’)
```

恭喜！你已经完成了第一个前馈神经网络的构建！

### 接下来是什么

保存并关闭文件。开始在控制台运行文件：

```py
python fnn.py
```

你将看到训练过程如下：

![](../Images/1253e956ebdc7cdfcde3b4a47fa3af04.png)

感谢你的时间，希望你喜欢这个教程。所有代码可以在[这里](https://github.com/yhuag/neural-network-lab/blob/master/Feedforward%20Neural%20Network.ipynb)找到！

**致谢：这些代码主要基于**[**yunjey**](https://github.com/yunjey)**的** [**出色代码库**](https://github.com/yunjey/pytorch-tutorial/blob/master/tutorials/02-intermediate/generative_adversarial_network/main.py)**。❤**

**个人简介：[Jeff Hu](https://www.linkedin.com/in/yaochiehhu/)** (**[Github](https://yhuag.github.io/)**) 是一位台湾的机器学习爱好者和区块链开发者。他专注于自然语言处理和深度学习工作，同时在闲暇时是一名魔术师和诗人。

[原文](https://towardsdatascience.com/a-simple-starter-guide-to-build-a-neural-network-3c2cf07b8d7c)。转载已获许可。

**相关内容：**

+   [今天我在午休时用 Keras 构建了一个神经网络](/2017/12/today-built-neural-network-during-lunch-break-keras.html)

+   [PyTorch 还是 TensorFlow？](/2017/08/pytorch-tensorflow.html)

+   [用 Keras 精通深度学习的 7 个步骤](/2017/10/seven-steps-deep-learning-keras.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
