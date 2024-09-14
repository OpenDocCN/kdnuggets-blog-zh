# 使用 NumPy 和图像分类的人工神经网络实现

> 原文：[`www.kdnuggets.com/2019/02/artificial-neural-network-implementation-using-numpy-and-image-classification.html/2`](https://www.kdnuggets.com/2019/02/artificial-neural-network-implementation-using-numpy-and-image-classification.html/2)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

### ANN 实现

下一图展示了目标 ANN 结构。输入层有 102 个输入，2 个隐藏层分别有 150 和 60 个神经元，输出层有 4 个输出（每个水果类别一个）。

![](img/713689ba5ab8a28eaf6a6aad078bdfe1.png)

在任何层中，输入向量通过与下一层连接的权重矩阵（矩阵乘法）进行乘法操作，产生一个输出向量。这个输出向量再次通过与下一层连接的权重矩阵进行乘法操作。该过程持续到达输出层。矩阵乘法的总结见下图。

![](img/5877059b505795d9234a289831d0fbb4.png)

输入向量的大小为 1x102，需与第一隐藏层的权重矩阵（大小为 102x150）相乘。记住这是矩阵乘法。因此，输出数组的形状为 1x150。该输出用作第二隐藏层的输入，再与大小为 150x60 的权重矩阵相乘。结果大小为 1x60。最后，这样的输出与第二隐藏层和输出层之间的权重（大小为 60x4）相乘。最终结果大小为 1x4。每个结果向量中的元素表示一个输出类别。根据最高分的类别对输入样本进行标记。

实现这些乘法的 Python 代码如下所示。

```py

import numpy
import pickle

def sigmoid(inpt):
  return 1.0 / (1 + numpy.exp(-1 * inpt))

f = open("dataset_features.pkl", "rb")
data_inputs2 = pickle.load(f)
f.close()

features_STDs = numpy.std(a=data_inputs2, axis=0)
data_inputs = data_inputs2[:, features_STDs > 50]

f = open("outputs.pkl", "rb")
data_outputs = pickle.load(f)
f.close()

HL1_neurons = 150
input_HL1_weights = numpy.random.uniform(low=-0.1, high=0.1,
    size=(data_inputs.shape[1], HL1_neurons))

HL2_neurons = 60
HL1_HL2_weights = numpy.random.uniform(low=-0.1, high=0.1,
    size=(HL1_neurons, HL2_neurons))

output_neurons = 4
HL2_output_weights = numpy.random.uniform(low=-0.1, high=0.1,
    size=(HL2_neurons, output_neurons))

H1_outputs = numpy.matmul(a=data_inputs[0, :], b=input_HL1_weights)
H1_outputs = sigmoid(H1_outputs)
H2_outputs = numpy.matmul(a=H1_outputs, b=HL1_HL2_weights)
H2_outputs = sigmoid(H2_outputs)
out_otuputs = numpy.matmul(a=H2_outputs, b=HL2_output_weights)

predicted_label = numpy.where(out_otuputs == numpy.max(out_otuputs))[0][0]
print("Predicted class : ", predicted_label)

```

在读取之前保存的特征及其输出标签并过滤特征后，定义层的权重矩阵。它们被随机赋值在-0.1 到 0.1 之间。例如，变量**"input_HL1_weights"**保存了输入层和第一隐藏层之间的权重矩阵。该矩阵的大小根据特征元素的数量和隐藏层中神经元的数量定义。

创建权重矩阵后，接下来是应用矩阵乘法。例如，变量**"H1_outputs"**保存了将给定样本的特征向量与输入层和第一隐藏层之间的权重矩阵相乘后的输出。

通常，对每个隐藏层的输出应用激活函数，以在输入和输出之间创建非线性关系。例如，将矩阵乘法的输出应用于 sigmoid 激活函数。

生成输出层输出后，进行预测。预测的类别标签被保存到 **"predicted_label"** 变量中。这些步骤对每个输入样本重复。下面是适用于所有样本的完整代码。

```py

import numpy
import pickle

def sigmoid(inpt):
  return 1.0 / (1 + numpy.exp(-1 * inpt))

def relu(inpt):
  result = inpt
  result[inpt < 0] = 0
  return result

def update_weights(weights, learning_rate):
  new_weights = weights - learning_rate * weights
  return new_weights

def train_network(num_iterations, weights, data_inputs, data_outputs, learning_rate, activation="relu"):
  for iteration in range(num_iterations):
    print("Itreation ", iteration)
    for sample_idx in range(data_inputs.shape[0]):
      r1 = data_inputs[sample_idx, :]
      for idx in range(len(weights) - 1):
       curr_weights = weights[idx]
       r1 = numpy.matmul(a=r1, b=curr_weights)
       if activation == "relu":
         r1 = relu(r1)
       elif activation == "sigmoid":
         r1 = sigmoid(r1)
    curr_weights = weights[-1]
    r1 = numpy.matmul(a=r1, b=curr_weights)
    predicted_label = numpy.where(r1 == numpy.max(r1))[0][0]
    desired_label = data_outputs[sample_idx]
    if predicted_label != desired_label:
      weights = update_weights(weights,
        learning_rate=0.001)
  return weights

def predict_outputs(weights, data_inputs, activation="relu"):
  predictions = numpy.zeros(shape=(data_inputs.shape[0]))
  for sample_idx in range(data_inputs.shape[0]):
    r1 = data_inputs[sample_idx, :]
      for curr_weights in weights:
        r1 = numpy.matmul(a=r1, b=curr_weights)
      if activation == "relu":
        r1 = relu(r1)
      elif activation == "sigmoid":
        r1 = sigmoid(r1)
    predicted_label = numpy.where(r1 == numpy.max(r1))[0][0]
    predictions[sample_idx] = predicted_label
  return predictions

f = open("dataset_features.pkl", "rb")
data_inputs2 = pickle.load(f)
f.close()

features_STDs = numpy.std(a=data_inputs2, axis=0)
data_inputs = data_inputs2[:, features_STDs > 50]

f = open("outputs.pkl", "rb")
data_outputs = pickle.load(f)
f.close()

HL1_neurons = 150
input_HL1_weights = numpy.random.uniform(low=-0.1, high=0.1,
size=(data_inputs.shape[1], HL1_neurons))

HL2_neurons = 60
HL1_HL2_weights = numpy.random.uniform(low=-0.1, high=0.1,
size=(HL1_neurons, HL2_neurons))

output_neurons = 4
HL2_output_weights = numpy.random.uniform(low=-0.1, high=0.1,
size=(HL2_neurons, output_neurons))

weights = numpy.array([input_HL1_weights,
  HL1_HL2_weights,
  HL2_output_weights])

weights = train_network(num_iterations=10,
  weights=weights,
  data_inputs=data_inputs,
  data_outputs=data_outputs,
  learning_rate=0.01,
  activation="relu")

predictions = predict_outputs(weights, data_inputs)
num_flase = numpy.where(predictions != data_outputs)[0]
print("num_flase ", num_flase.size)

```

**"weights"** 变量保存了整个网络中的所有权重。根据每个权重矩阵的大小，网络结构被动态指定。例如，如果 **"input_HL1_weights"** 变量的大小是 102x80，那么我们可以推断出第一个隐藏层有 80 个神经元。

**"train_network"** 是核心功能，它通过遍历所有样本来训练网络。对于每个样本，应用了在列表 3-6 中讨论的步骤。它接受训练迭代次数、特征、输出标签、权重、学习率和激活函数。激活函数有两个选项，即 ReLU 或 sigmoid。ReLU 是一个阈值函数，只要输入大于零，就返回相同的输入。否则，它返回零。

如果网络对给定样本做出了错误预测，则使用 **"update_weights"** 函数更新权重。没有使用优化算法来更新权重。权重根据学习率简单地更新。准确率不会超过 45%。为了获得更好的准确率，可以使用优化算法来更新权重。例如，你可以在 scikit-learn 库的 ANN 实现中找到梯度下降技术。

在我的书中，你可以找到使用遗传算法（GA）优化技术来优化 ANN 权重的指南，这种技术提高了分类准确率。你可以从我准备的以下资源中了解更多关于 GA 的内容：

### 遗传算法优化介绍

+   [`www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/`](https://www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/)

+   `www.kdnuggets.com/2018/03/introduction-optimization-with-genetic-algorithm.html`

+   [`towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b`](https://towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b)

+   [`www.springer.com/us/book/9781484241660`](https://www.springer.com/us/book/9781484241660)

### 遗传算法（GA）优化 - 步骤示例

+   [`www.slideshare.net/AhmedGadFCIT/genetic-algorithm-ga-optimization-stepbystep-example`](https://www.slideshare.net/AhmedGadFCIT/genetic-algorithm-ga-optimization-stepbystep-example)

### 遗传算法在 Python 中的实现

+   [`www.linkedin.com/pulse/genetic-algorithm-implementation-python-ahmed-gad/`](https://www.linkedin.com/pulse/genetic-algorithm-implementation-python-ahmed-gad/)

+   `www.kdnuggets.com/2018/07/genetic-algorithm-implementation-python.html`

+   [`towardsdatascience.com/genetic-algorithm-implementation-in-python-5ab67bb124a6`](https://towardsdatascience.com/genetic-algorithm-implementation-in-python-5ab67bb124a6)

+   [`github.com/ahmedfgad/GeneticAlgorithmPython`](https://github.com/ahmedfgad/GeneticAlgorithmPython)

### 联系作者

+   领英：[`www.linkedin.com/in/ahmedfgad`](https://www.linkedin.com/in/ahmedfgad)

+   脸书：[`www.facebook.com/ahmed.f.gadd`](https://www.facebook.com/ahmed.f.gadd)

+   推特：[`twitter.com/ahmedfgad`](https://twitter.com/ahmedfgad)

+   Towards Data Science：[`towardsdatascience.com/@ahmedfgad`](https://towardsdatascience.com/@ahmedfgad)

+   KDnuggets：`www.kdnuggets.com/author/ahmed-gad`

+   电子邮件：ahmed.f.gad@gmail.com

[原文](https://www.linkedin.com/pulse/artificial-neural-network-implementation-using-numpy-fruits360-gad/)。已获许可转载。

**相关：**

+   [神经网络 - 直观理解](https://www.kdnuggets.com/2019/02/neural-networks-intuition.html)

+   [如何在 Python 中创建简单的神经网络](https://www.kdnuggets.com/2018/10/simple-neural-network-python.html)

+   [从零开始使用 NumPy 构建卷积神经网络](https://www.kdnuggets.com/2018/04/building-convolutional-neural-network-numpy-scratch.html)

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [成为伟大数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [使用卷积神经网络（CNNs）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [停止学习数据科学，找到目标并……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
