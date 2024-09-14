# TensorFlow 教程，第 2 部分 – 开始使用

> 原文：[https://www.kdnuggets.com/2017/09/tensorflow-tutorial-part-2.html](https://www.kdnuggets.com/2017/09/tensorflow-tutorial-part-2.html)

**作者：Vivek Kalyanarangan，[专业网络联盟](https://unp.education/)。**

![](../Images/86cda183d7b242f18aab35d95c20b043.png)

在这个多部分系列中，我们将探索如何开始使用 TensorFlow。本 TensorFlow 教程将为这个人人谈论的热门工具奠定坚实的基础。第二部分是一个关于开始使用、安装和构建小型用例的 TensorFlow 教程。

本系列摘录自我作为[专业网络联盟](https://www.unp.education/)的一部分所进行的网络研讨会教程系列。我会时不时提及在演讲中使用的一些幻灯片，以便让内容更加清晰。

请不要错过我谈论所有我写的内容的在线研讨会。请注册我们的[即将举行的网络研讨会](https://machinelearningblogs.com/events/)，以了解我们将讨论的主题。

本文是关于完整 TensorFlow 教程的多部分系列的第一部分 –

+   [第 1 部分：介绍](https://machinelearningblogs.com/2017/09/07/tensorflow-tutorial-part-1-introduction/)

+   第 2 部分：开始使用

+   第 3 部分：构建第一个模型

### TensorFlow 教程：目标

![](../Images/804f3d9251b44bdd6af74beac705af36.png)

+   [安装 TensorFlow](#install_tensorflow)

+   [验证 TensorFlow 安装](#verify_install)

+   [在构建第一个 TensorFlow 模型之前](#build_model)

### 安装 TensorFlow

如果你已经安装了 TensorFlow，可以直接跳到下一部分。

#### 1\. 操作系统

不同操作系统有不同的安装 TensorFlow 的方法。你可以查阅[文档](https://www.tensorflow.org/)获取更多详细信息。我将只讨论开始使用所必需的内容。

+   点击安装选项卡

![](../Images/de8219546295b9dfd9bbe15d0c93c04f.png)

以下指南解释了如何安装一个版本的 TensorFlow，使你能够用 Python 编写应用程序。

+   [在 Ubuntu 上安装 TensorFlow](https://www.tensorflow.org/install/install_linux)

+   [在 Mac OS X 上安装 TensorFlow](https://www.tensorflow.org/install/install_mac)

+   [在 Windows 上安装 TensorFlow](https://www.tensorflow.org/install/install_windows)

+   [从源代码安装 TensorFlow](https://www.tensorflow.org/install/install_sources)

尽量遵循文档中已提到的最佳实践的基本结构。

#### 2\. GPU

使用 GPU 安装 TensorFlow 需要你有 NVIDIA GPU。AMD 显卡不受 TensorFlow 支持。NVIDIA 使用称为 CUDA 的低级 GPU 计算系统。这是 NVIDIA 专有的软件。

可以使用 OpenCL 与 AMD 配合，但目前它不支持 TensorFlow。

另外，并非所有NVIDIA设备都受支持。以下是[NVIDIA文档](https://developer.nvidia.com/cuda-gpus)中列出的受支持的GPU列表。

![](../Images/000cd280b377c392476ff8db745db974.png)

#### 3\. 环境

有三种环境可以用来设置tensorflow –

1.  直接安装 – 这只是安装任何软件库的典型方式。它直接与已安装的操作系统交互，并像其他库一样安装tensorflow。`pip`是直接安装的首选方式。

1.  虚拟环境 – 对于那些不知道python虚拟环境优势的人，`virtualenv`就像是一个与系统中已经安装的默认python平行的python安装。 在虚拟环境中安装库可以将库分开，你将不会与直接安装的其他库发生兼容性冲突。如果出现问题，你可以启动一个新的虚拟环境，重新开始。

1.  Docker容器 – 这是一种安装tensorflow的方式。可以将典型的VMware镜像想象成是一个超级强大的docker容器。Docker可以用来启动具有不同操作系统环境的容器，从而允许你拥有一个与主机系统完全分开的环境。所有细节在tensorflow文档中都明确标出。

#### 4\. Python版本

支持Python 2的2.7版本和Python 3的3.3或更高版本。所有操作系统均适用。

> 目前，Windows仅支持3.5版本。Python 2与Windows的组合是不受支持的。

### 验证安装

一旦tensorflow安装完成，无论操作系统、环境或python版本如何，你都应该运行以下脚本来验证tensorflow是否正常运行。

```py
 # import TensorFlow
import tensorflow as tf

sess = tf.Session()

# Verify we can print a string
hello = tf.constant("Hello UNP from TensorFlow")
print(sess.run(hello))

# Perform some simple math
a = tf.constant(20)
b = tf.constant(22)
print('a + b = {0}'.format(sess.run(a + b))) 
```

一旦这段代码成功运行并打印输出，恭喜你！你已经成功安装了tensorflow。接下来我们进入下一部分，构建我们的第一个应用程序。

### 在构建第一个tensorflow模型之前

#### 张量

下面是我们在开始之前需要学习的三种张量类型。

| **类型** | **描述** |
| --- | --- |
| 常量 | 常量值 |
| 变量 | 在图中调整的值 |
| 占位符 | 用于在图中传递数据 |

在动手操作之前，我只想介绍几个tensorflow术语及其含义。

![](../Images/0a104d3b06b34450c35480ed5b10b0ba.png)

1.  排名 – 张量的维度

1.  形状 – 张量的形状。与排名相关

上图应有助于理解。下面是Tensorflow支持的不同数据类型。

![](../Images/f6b2489ed45aed366e0d963cda105989.png)

**注意：**量化值[qint8、qint16 和 quint8]是 TensorFlow 的特殊值，有助于减少数据大小。事实上，Google 已经引入了 TensorFlow 处理单元（TPUs）来通过利用量化值加快计算速度*

#### 准备数据

我们将快速生成一些数据以便开始。在这个例子中，我们将生成房屋大小数据以预测房价。这里的目标不是建立一个复杂的房价预测器，而是以最简单的方式在 TensorFlow 中启动。

我们将使用下面的 Python 代码生成一些数据–

```py
 import tensorflow as tf
import numpy as np
import math
import matplotlib.pyplot as plt

#  generation some house sizes between 1000 and 3500 (typical sq ft of house)
num_house = 160
np.random.seed(42)
house_size = np.random.randint(low=1000, high=3500, size=num_house )

# Generate house prices from house size with a random noise added.
np.random.seed(42)
house_price = house_size * 100.0 + 
              np.random.randint(low=20000, high=70000, size=num_house)  

# Plot generated house and size 
plt.plot(house_size, house_price, "bx")  # bx = blue x
plt.ylabel("Price")
plt.xlabel("Size")
plt.show() 
```

这将生成如下输出[这是生成的数据的图示]

![](../Images/13efc08549f7833aae8f5cf050d6d3a0.png)

接下来，我们将对数据进行归一化。这有助于将数据调整到相同的尺度，从而可能导致更快的收敛。

我们还将其拆分为训练数据和测试数据，作为数据科学最佳实践的一部分。我们将用训练数据训练我们的模型，并用测试数据测试模型，以查看我们的预测有多准确。

```py
 # you need to normalize values to prevent under/overflows.
def normalize(array):
    return (array - array.mean()) / array.std()

# define number of training samples, 0.7 = 70%.  We can take the first 70% 
# since the values are randomized
num_train_samples = math.floor(num_house * 0.7)

# define training data
train_house_size = np.asarray(house_size[:num_train_samples])
train_price = np.asanyarray(house_price[:num_train_samples:])

train_house_size_norm = normalize(train_house_size)
train_price_norm = normalize(train_price)

# define test data
test_house_size = np.array(house_size[num_train_samples:])
test_house_price = np.array(house_price[num_train_samples:])

test_house_size_norm = normalize(test_house_size)
test_house_price_norm = normalize(test_house_price) 
```

### 结论

我希望这能设定对接下来内容的期望。在下一篇文章中，我们将构建我们的第一个TensorFlow模型。

请不要错过直播研讨会，在那里我会讨论我写的所有内容。注册我们的[即将到来的研讨会](https://machinelearningblogs.com/events/)以了解我们将讨论的主题。快乐编码！

在下一部分，我们终于准备好用房价数据训练我们的第一个 TensorFlow 模型。这将给我们提供第一次使用 TensorFlow 的亲身体验！

我在下面嵌入了原始演示文稿–

[原始](https://machinelearningblogs.com/2017/09/14/tensorflow-tutorial-part-2-getting-started/)。经许可转载。

**简介：** [**Vivek Kalyanarangan**](https://www.linkedin.com/in/%E2%9C%AA-vivek-kalyanarangan-149323a1/?ppe=1) 是一名数据科学家、博主、吉他手，对新技术充满热情。

**相关：**

+   [TensorFlow 教程：第 1 部分 – 介绍](/2017/09/tensorflow-tutorial-part-1.html)

+   [寻找最快的 Keras 深度学习后端](/2017/09/search-fastest-keras-deep-learning-backend.html)

+   [PyTorch 还是 TensorFlow?](/2017/08/pytorch-tensorflow.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

### 更多相关主题

+   [联邦学习：协作机器学习教程](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)

+   [开始自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [开始数据清理](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)

+   [开始使用 SQL 备忘单](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [开始使用 spaCy 进行自然语言处理](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)

+   [开始使用 PyCaret](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)
