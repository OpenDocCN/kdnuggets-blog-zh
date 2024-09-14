# 从头构建人工神经网络：第2部分

> 原文：[https://www.kdnuggets.com/2020/03/build-artificial-neural-network-scratch-part-2.html](https://www.kdnuggets.com/2020/03/build-artificial-neural-network-scratch-part-2.html)

[评论](#comments)![图](../Images/c9b03ee546c984cf6fada48aa5830875.png)

[来源](https://forum.novelupdates.com/threads/i-need-answers.16270/)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

在我之前的文章中，[**从头构建人工神经网络(ANN)：第1部分**](https://towardsdatascience.com/build-an-artificial-neural-network-ann-from-scratch-part-1-a21988497962)，我们开始讨论什么是人工神经网络；我们看到如何用Python从头创建一个简单的神经网络，具有一个输入层和一个输出层。这样的神经网络被称为感知器。然而，现实世界的神经网络能够执行复杂任务，如图像分类和股票市场分析，除了输入层和输出层，还包含多个隐藏层。

在上一篇文章中，我们总结了感知器能够找到线性决策边界。我们使用感知器预测一个人是否患糖尿病，使用的是一个虚拟数据集。然而，**感知器无法找到非线性决策边界。**

在本文中，我们将开发一个具有一个输入层、一个隐藏层和一个输出层的神经网络。我们将看到我们开发的神经网络将能够找到非线性边界。

### 生成数据集

让我们开始生成一个可以玩耍的数据集。幸运的是，[scikit-learn](http://scikit-learn.org/) 提供了一些有用的数据集生成器，因此我们不需要自己编写代码。我们将使用 [make_moons](http://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_moons.html) 函数。

```py
from sklearn import datasets
np.random.seed(0)
feature_set, labels = datasets.make_moons(300, noise=0.20)
plt.figure(figsize=(10,7))
plt.scatter(feature_set[:,0], feature_set[:,1], c=labels, cmap=plt.cm.Spectral)
```

![](../Images/29a9ae72755b310886ff687fc841e614.png)

我们生成的数据集有两个类别，以红色和蓝色点绘制。你可以将蓝色点视为男性患者，红色点视为女性患者，x轴和y轴表示医疗测量。

我们的目标是训练一个机器学习分类器，该分类器根据 `x` 和 `y` 坐标预测正确的类别（男性或女性）。请注意，数据是非线性可分的，我们无法绘制一条直线将两个类别分开。这意味着线性分类器，例如没有隐藏层的 ANN 或者逻辑回归，将无法拟合数据，除非你手动工程化对给定数据集有效的非线性特征（例如多项式）。

### 单隐藏层神经网络

这是我们的简单网络：

![](../Images/fdd87697154b2bd9784b0ceed74a5cf3.png)

我们有两个输入：**x1** 和 **x2**。有一个隐藏层，包含 3 个单元（节点）：**h1, h2** 和 **h3**。最后，有两个输出：**y1** 和 **y2**。连接它们的箭头是权重。共有两个权重矩阵：**w** 和 **u**。**w** 权重连接输入层和隐藏层。**u** 权重连接隐藏层和输出层。我们使用了字母 **w** 和 **u**，以便更容易跟踪计算。你还可以看到，我们将输出 **y1** 和 **y2** 与目标 **t1** 和 **t2** 进行比较。

在进行计算之前，我们需要引入最后一个字母。让 **a** 代表激活之前的线性组合。因此，我们有：

![](../Images/439b819aae373829b90d9174d522e237.png)

和

![](../Images/434fbfe985ff45b2fbaa8dae922c1fd7.png)

由于我们无法穷举所有激活函数和所有损失函数，我们将专注于两种最常见的：**sigmoid** 激活函数和 **L2-norm 损失**。有了这些新信息和新符号，输出 y 等于激活的线性组合。

因此，对于输出层，我们有：

![](../Images/facc4d63342964822889148b02a922a5.png)

而对于隐藏层：

![](../Images/956ac8ed8250ea3ce341d186d7f7a0ca.png)

我们将分别考察输出层和隐藏层的反向传播，因为方法有所不同。

我想提醒你：

![](../Images/93e86f051d737f9fd5382cf96b06aec6.png)

sigmoid 函数是：

![](../Images/4d8f2ba13f34e96007b11946575c8542.png)

其导数是：

![](../Images/c61e2215da96d344393a1cdb220e5f3f.png)

### 输出层的反向传播

为了得到更新规则：

![](../Images/6666261d5aa8814bbb88483ab35ed6f2.png)

我们必须计算

![](../Images/b4b1e6984fda749dcfb8222d7fd02cf4.png)

让我们考虑单个权重 *u*ij。损失函数关于 *u*ij 的偏导数等于：

![](../Images/fc37eeeedb738c64fc33a127448e33c0.png)

其中 i 对应于前一层（本次变换的输入层），j 对应于下一层（变换的输出层）。偏导数的计算是简单地按照链式法则进行的。

![](../Images/7d6d25ad485edee669bebe7feccd898e.png)

按照 L2-norm 损失的导数。

![](../Images/759bfb3e847d8aee144b39460e27ece6.png)

按照 sigmoid 导数进行。

最后，第三个偏导数只是以下的导数：

![](../Images/434fbfe985ff45b2fbaa8dae922c1fd7.png)

所以，

![](../Images/08850244595ca1cf164aeec7edd0adc0.png)

替换上述表达式中的偏导数，我们得到：

![](../Images/6c06ef7bc245aa3df9066a95aba67a9c.png)

因此，输出层单个权重的更新规则为：

![](../Images/8db95d12806ce2f99e5619d0ca7afd07.png)

### 5\. 隐藏层的反向传播

类似于输出层的反向传播，单个权重的更新规则，*wij* 将依赖于：

![](../Images/62ad4693fc62b76dbb9d9081b404152e.png)

按照链式法则。利用目前我们为使用 sigmoid 激活函数和线性模型变换得到的结果，我们得到：

![](../Images/7342f010519945bddc9f40d11aff893b.png)

和

![](../Images/ceb75107e4dc2c12135998a034fccb5b.png)

反向传播的实际问题来源于术语

![](../Images/55435ed94c0cd7c31c3885d2ca12b97e.png)

这是因为没有“隐藏”的目标。你可以参考下面对权重 *w11* 的解决方案。在进行计算时，建议查看上面显示的神经网络图。

![](../Images/4005feb701d1e03924701879e0e8ada8.png)

从这里，我们可以计算

![](../Images/2cb24bcd18194bd78b7ecbd8559f79f7.png)

这正是我们想要的。最终的表达式是：

![](../Images/baa55ff6d8cc63a8edf579dafb9c0e10.png)

这个方程的广义形式是：

![](../Images/2adb09a0fc5b9bc6319733aa1b92767f.png)

### 反向传播的广义化

使用输出层和隐藏层的反向传播结果，我们可以将它们合并为一个公式，总结反向传播，考虑 L2 范数损失和 sigmoid 激活。

![](../Images/b60b742945e59628928aa6b066ab43de.png)

对于隐藏层

![](../Images/6433c58e11c166fd2a334db235c2c67f.png)

### 带有一个隐藏层的神经网络代码

现在让我们从头开始在 Python 中实现刚才讨论的神经网络。我们将再次尝试分类我们上面创建的非线性数据。

我们从定义一些用于梯度下降的有用变量和参数开始，比如训练数据集的大小、输入层和输出层的维度。

```py
num_examples = len(X) # training set size
nn_input_dim = 2 # input layer dimensionality
nn_output_dim = 2 # output layer dimensionality
```

同时定义梯度下降的参数。

```py
epsilon = 0.01 # learning rate for gradient descent
reg_lambda = 0.01 # regularization strength
```

首先，让我们实现上述定义的损失函数。我们用它来评估模型的表现：

```py
# Helper function to evaluate the total loss on the dataset
def calculate_loss(model, X, y):
    num_examples = len(X)  # training set size
    W1, b1, W2, b2 = model['W1'], model['b1'], model['W2'], model['b2']
    # Forward propagation to calculate our predictions
    z1 = X.dot(W1) + b1
    a1 = np.tanh(z1)
    z2 = a1.dot(W2) + b2
    exp_scores = np.exp(z2)
    probs = exp_scores / np.sum(exp_scores, axis=1, keepdims=True)
    # Calculating the loss
    corect_logprobs = -np.log(probs[range(num_examples), y])
    data_loss = np.sum(corect_logprobs)
    # Add regulatization term to loss (optional)
    data_loss += Config.reg_lambda / 2 * (np.sum(np.square(W1)) + np.sum(np.square(W2)))
    return 1\. / num_examples * data_loss
```

我们还实现了一个辅助函数来计算网络的输出。它执行前向传播并返回概率最高的类别。

```py
def predict(model, x):
    W1, b1, W2, b2 = model['W1'], model['b1'], model['W2'], model['b2']
    # Forward propagation
    z1 = x.dot(W1) + b1
    a1 = np.tanh(z1)
    z2 = a1.dot(W2) + b2
    exp_scores = np.exp(z2)
    probs = exp_scores / np.sum(exp_scores, axis=1, keepdims=True)
    return np.argmax(probs, axis=1)
```

最后，这里是训练我们神经网络的函数。它实现了使用上述反向传播导数的批量梯度下降。

这个函数学习神经网络的参数并返回模型。

**nn_hdim**: 隐藏层中节点的数量

**num_passes**: 梯度下降训练数据的遍历次数

**print_loss**: 如果为 True，每1000次迭代打印一次损失

```py
def build_model(X, y, nn_hdim, num_passes=20000, print_loss=False):
    # Initialize the parameters to random values. We need to learn these.
    num_examples = len(X)
    np.random.seed(0)
    W1 = np.random.randn(Config.nn_input_dim, nn_hdim) / np.sqrt(Config.nn_input_dim)
    b1 = np.zeros((1, nn_hdim))
    W2 = np.random.randn(nn_hdim, Config.nn_output_dim) / np.sqrt(nn_hdim)
    b2 = np.zeros((1, Config.nn_output_dim))# This is what we return at the end
    model = {}# Gradient descent. For each batch...
    for i in range(0, num_passes):# Forward propagation
        z1 = X.dot(W1) + b1
        a1 = np.tanh(z1)
        z2 = a1.dot(W2) + b2
        exp_scores = np.exp(z2)
        probs = exp_scores / np.sum(exp_scores, axis=1, keepdims=True)# Backpropagation
        delta3 = probs
        delta3[range(num_examples), y] -= 1
        dW2 = (a1.T).dot(delta3)
        db2 = np.sum(delta3, axis=0, keepdims=True)
        delta2 = delta3.dot(W2.T) * (1 - np.power(a1, 2))
        dW1 = np.dot(X.T, delta2)
        db1 = np.sum(delta2, axis=0)# Add regularization terms (b1 and b2 don't have regularization terms)
        dW2 += Config.reg_lambda * W2
        dW1 += Config.reg_lambda * W1# Gradient descent parameter update
        W1 += -Config.epsilon * dW1
        b1 += -Config.epsilon * db1
        W2 += -Config.epsilon * dW2
        b2 += -Config.epsilon * db2# Assign new parameters to the model
        model = {'W1': W1, 'b1': b1, 'W2': W2, 'b2': b2}# Optionally print the loss.
        # This is expensive because it uses the whole dataset, so we don't want to do it too often.
        if print_loss and i % 1000 == 0:
            print("Loss after iteration %i: %f" % (i, calculate_loss(model, X, y)))return model
```

最后是主要方法：

```py
def main():
    X, y = generate_data()
    model = build_model(X, y, 3, print_loss=True)
    visualize(X, y, model)
```

每 1000 次迭代后打印损失：

![](../Images/2379742845310b45ea5aaaaba1dd09c4.png)

![](../Images/d8f8aa5b2fe29ab7c66fce9b731bcfc8.png)

当隐藏层节点数量为 3 时的分类

现在让我们了解一下隐藏层大小的变化如何影响结果。

```py
hidden_layer_dimensions = [1, 2, 3, 4, 5, 20, 50] 
for i, nn_hdim in enumerate(hidden_layer_dimensions): 
    plt.subplot(5, 2, i+1)
    plt.title('Hidden Layer size %d' % nn_hdim)
    model = build_model(X, y,nn_hdim, 20000, print_loss=False)
    plot_decision_boundary(lambda x:predict(model,x), X, y) 
    plt.show()
```

![图](../Images/720b6b3c0ad14b2f64ca3799a648f12a.png)

我们可以看到，低维度的隐藏层很好地捕捉了数据的一般趋势。较高的维度容易导致过拟合。它们是在“记忆”数据，而不是拟合一般的形状。

如果我们在单独的测试集上评估我们的模型，较小隐藏层大小的模型由于更好的泛化能力可能会表现得更好。我们可以通过更强的正则化来对抗过拟合，但选择合适的隐藏层大小是更“经济”的解决方案。

你可以在这个 GitHub 仓库中获取完整的代码。

[**nageshsinghc4/人工神经网络从零开始-python**](https://github.com/nageshsinghc4/Artificial-Neural-Network-from-scratch-python/tree/master)

### 结论

所以在这篇文章中，我们看到如何从数学上推导出一个具有一个隐藏层的神经网络，并且我们还用 NumPy 从零开始创建了一个具有 1 个隐藏层的神经网络。

好了，这就结束了关于从零开始构建人工神经网络的两篇文章系列**。** 希望你们喜欢阅读，欢迎在评论区分享你的评论/想法/反馈。

**简介：[Nagesh Singh Chauhan](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是 CirrusLabs 的大数据开发人员。他在电信、分析、销售、数据科学等各个领域拥有超过 4 年的工作经验，并在各种大数据组件方面具有专业知识。

[原文](https://towardsdatascience.com/build-an-artificial-neural-network-ann-from-scratch-part-2-a33c44eca56b)。 经授权转载。

**相关：**

+   [从零开始构建人工神经网络：第 1 部分](/2019/11/build-artificial-neural-network-scratch-part-1.html)

+   [只有 NumPy：从头理解和创建神经网络的计算图](/2019/08/numpy-neural-networks-computational-graphs.html)

+   [如何将图片转换为数字](/2020/01/convert-picture-numbers.html)

### 更多相关主题

+   [如何从零开始构建和训练 Transformer 模型…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [通过构建 15 个神经网络项目在 2022 年学习深度学习](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用 AIMET 优化神经网络](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [使用 TensorFlow 和 Keras 构建和训练您的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [排列在神经网络预测中的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)
