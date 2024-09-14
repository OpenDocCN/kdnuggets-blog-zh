# 在 Keras 中使用 Lambda 层

> 原文：[`www.kdnuggets.com/2021/01/working-lambda-layer-keras.html`](https://www.kdnuggets.com/2021/01/working-lambda-layer-keras.html)

评论

![在 Keras 中使用 Lambda 层](img/c3a523288bdaacf398ad685f673c0a8e.png)

Keras 是一个流行且易于使用的深度学习模型构建库。它支持所有已知类型的层：输入层、全连接层、卷积层、转置卷积层、重塑层、归一化层、丢弃层、展平层和激活层。每个层对数据执行特定操作。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

尽管如此，你可能希望对数据执行一些现有层未应用的操作，这时候现有的层类型可能不够用。举个简单的例子，假设你需要一个在模型架构的某个特定点添加固定数字的层。因为没有现有的层可以做到这一点，你可以自己构建一个。

在本教程中，我们将讨论在 Keras 中使用`Lambda`层。这使你能够将操作指定为函数。我们还将看到如何调试 Keras 加载功能，当构建一个包含 lambda 层的模型时。

本教程涵盖的部分如下：

+   使用`Functional API`构建 Keras 模型

+   添加一个`Lambda`层

+   向 lambda 层传递多个张量

+   保存和加载带有 lambda 层的模型

+   解决加载带有 lambda 层的模型时的 SystemError

让这个项目成为现实

[在 Gradient 上运行](https://www.paperspace.com/account/signup)

### **使用`Functional API`构建 Keras 模型**

在 Keras 中可以使用三种不同的 API 来构建模型：

1.  顺序 API

1.  功能 API

1.  模型子类化 API

你可以在[this post](https://www.pyimagesearch.com/2019/10/28/3-ways-to-create-a-keras-model-with-tensorflow-2-0-sequential-functional-and-model-subclassing)中找到有关这些内容的更多信息，但在本教程中我们将专注于使用 Keras `Functional API`构建自定义模型。由于我们希望专注于架构，我们将使用一个简单的问题示例来构建一个识别 MNIST 数据集图像的模型。

在 Keras 中构建模型时，你需要将层一个接一个地堆叠。这些层在`keras.layers`模块中可用（如下所示）。模块名称以`tensorflow`为前缀，因为我们使用 TensorFlow 作为 Keras 的后端。

```py
import tensorflow.keras.layers
```

创建的第一层是`Input`层。这是使用`tensorflow.keras.layers.Input()`类创建的。这个类的构造函数必须传递的一个必要参数是`shape`参数，它指定了将用于训练的数据中每个样本的形状。在本教程中，我们将从密集层开始，因此输入应为 1-D 向量。`shape`参数因此被赋值为一个包含一个值的元组（如下所示）。这个值是 784，因为 MNIST 数据集中每个图像的大小是 28 x 28 = 784。一个可选的`name`参数指定了该层的名称。

```py
input_layer = tensorflow.keras.layers.Input(shape=(784), name="input_layer")
```

下一层是使用`Dense`类创建的密集层，如下代码所示。它接受一个名为`units`的参数来指定这一层中的神经元数量。请注意这一层是通过在括号中指定该层的名称来连接到输入层的。这是因为功能 API 中的层实例是可以调用在张量上的，并且也返回一个张量。

```py
dense_layer_1 = tensorflow.keras.layers.Dense(units=500, name="dense_layer_1")(input_layer)
```

在密集层之后，根据下一行的代码，使用`ReLU`类创建了一个激活层。

```py
activ_layer_1 = tensorflow.keras.layers.ReLU(name="activ_layer_1")(dense_layer_1)
```

根据以下几行代码，添加了另几个密集-ReLu 层。

```py
dense_layer_2 = tensorflow.keras.layers.Dense(units=250, name="dense_layer_2")(activ_layer_1)
activ_layer_2 = tensorflow.keras.layers.ReLU(name="relu_layer_2")(dense_layer_2)

dense_layer_3 = tensorflow.keras.layers.Dense(units=20, name="dense_layer_3")(activ_layer_2)
activ_layer_3 = tensorflow.keras.layers.ReLU(name="relu_layer_3")(dense_layer_3)
```

下一行根据 MNIST 数据集中的类别数量向网络架构中添加了最后一层。由于 MNIST 数据集包含 10 个类别（每个数字一个），因此在这一层中使用的单位数量为 10。

```py
dense_layer_4 = tensorflow.keras.layers.Dense(units=10, name="dense_layer_4")(activ_layer_3)
```

为了返回每个类别的得分，根据下一行的代码，在之前的密集层后添加了一个`softmax`层。

```py
output_layer = tensorflow.keras.layers.Softmax(name="output_layer")(dense_layer_4)
```

我们现在已经连接了层，但模型尚未创建。要构建模型，我们现在必须使用`Model`类，如下所示。它接受的前两个参数代表输入和输出层。

```py
model = tensorflow.keras.models.Model(input_layer, output_layer, name="model")
```

在加载数据集和训练模型之前，我们必须使用`compile()`方法来编译模型。

```py
model.compile(optimizer=tensorflow.keras.optimizers.Adam(lr=0.0005), loss="categorical_crossentropy")
```

使用`model.summary()`我们可以查看模型架构的概述。输入层接受形状为(None, 784)的张量，这意味着每个样本必须被重塑为 784 个元素的向量。输出`Softmax`层返回 10 个数字，每个数字是 MNIST 数据集相应类别的得分。

```py
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_layer (InputLayer)     [(None, 784)]             0         
_________________________________________________________________
dense_layer_1 (Dense)        (None, 500)               392500    
_________________________________________________________________
relu_layer_1 (ReLU)          (None, 500)               0         
_________________________________________________________________
dense_layer_2 (Dense)        (None, 250)               125250    
_________________________________________________________________
relu_layer_2 (ReLU)          (None, 250)               0         
_________________________________________________________________
dense_layer_3 (Dense)        (None, 20)                12550     
_________________________________________________________________
relu_layer_3 (ReLU)          (None, 20)                0         
_________________________________________________________________
dense_layer_4 (Dense)        (None, 10)                510       
_________________________________________________________________
output_layer (Softmax)       (None, 10)                0         
=================================================================
Total params: 530,810
Trainable params: 530,810
Non-trainable params: 0
_________________________________________________________________
```

现在我们已经构建并编译了模型，让我们看看数据集是如何准备的。首先，我们将从`keras.datasets`模块加载 MNIST，将数据类型更改为`float64`，因为这比将其值保持在 0-255 范围内更容易训练网络，最后重新塑形，使每个样本成为 784 个元素的向量。

```py
(x_train, y_train), (x_test, y_test) = tensorflow.keras.datasets.mnist.load_data()

x_train = x_train.astype(numpy.float64) / 255.0
x_test = x_test.astype(numpy.float64) / 255.0

x_train = x_train.reshape((x_train.shape[0], numpy.prod(x_train.shape[1:])))
x_test = x_test.reshape((x_test.shape[0], numpy.prod(x_test.shape[1:])))
```

由于`compile()`方法中使用的损失函数是`categorical_crossentropy`，样本的标签应根据下一个代码进行热编码。

```py
y_test = tensorflow.keras.utils.to_categorical(y_test)
y_train = tensorflow.keras.utils.to_categorical(y_train)
```

最后，模型训练开始使用 `fit()` 方法。

```py
model.fit(x_train, y_train, epochs=20, batch_size=256, validation_data=(x_test, y_test))
```

到此为止，我们已经使用现有的层类型创建了模型架构。下一节讨论如何使用 `Lambda` 层来构建自定义操作。

### **使用 Lambda 层**

假设在名为 `dense_layer_3` 的全连接层之后，我们希望对张量进行某种操作，比如给每个元素加上 2。我们该如何做到这一点呢？现有的层都不提供这种功能，所以我们需要自己构建一个新的层。幸运的是，`Lambda` 层正是为了这个目的而存在的。我们来讨论一下如何使用它。

首先构建将执行所需操作的函数。在这种情况下，创建了一个名为 `custom_layer` 的函数。它只接受输入张量（或张量），并返回另一个张量作为输出。如果需要将多个张量传递给函数，则它们将作为列表传递。

在这个例子中，仅输入一个张量，并且将 2 添加到输入张量中的每个元素上。

```py
def custom_layer(tensor):
    return tensor + 2
```

在构建定义操作的函数之后，我们接下来需要使用 `Lambda` 类来创建 lambda 层，如下行所示。在这种情况下，只有一个张量被传递给 `custom_layer` 函数，因为 lambda 层是在名为 `dense_layer_3` 的全连接层返回的单个张量上可调用的。

```py
lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")(dense_layer_3)
```

以下是使用 lambda 层后构建完整网络的代码。

```py
input_layer = tensorflow.keras.layers.Input(shape=(784), name="input_layer")

dense_layer_1 = tensorflow.keras.layers.Dense(units=500, name="dense_layer_1")(input_layer)
activ_layer_1 = tensorflow.keras.layers.ReLU(name="relu_layer_1")(dense_layer_1)

dense_layer_2 = tensorflow.keras.layers.Dense(units=250, name="dense_layer_2")(activ_layer_1)
activ_layer_2 = tensorflow.keras.layers.ReLU(name="relu_layer_2")(dense_layer_2)

dense_layer_3 = tensorflow.keras.layers.Dense(units=20, name="dense_layer_3")(activ_layer_2)

def custom_layer(tensor):
    return tensor + 2

lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")(dense_layer_3)

activ_layer_3 = tensorflow.keras.layers.ReLU(name="relu_layer_3")(lambda_layer)

dense_layer_4 = tensorflow.keras.layers.Dense(units=10, name="dense_layer_4")(activ_layer_3)
output_layer = tensorflow.keras.layers.Softmax(name="output_layer")(dense_layer_4)

model = tensorflow.keras.models.Model(input_layer, output_layer, name="model")
```

为了查看张量在被传递给 lambda 层之前和之后的情况，我们将创建两个新的模型，除了之前的模型。我们将这两个模型称为 `before_lambda_model` 和 `after_lambda_model`。这两个模型都使用输入层作为它们的输入，但输出层有所不同。`before_lambda_model` 模型返回的是 `dense_layer_3` 的输出，即位于 lambda 层之前的层的输出。`after_lambda_model` 模型的输出是名为 `lambda_layer` 的 lambda 层的输出。通过这样做，我们可以看到应用 lambda 层前后的输入和输出。

```py
before_lambda_model = tensorflow.keras.models.Model(input_layer, dense_layer_3, name="before_lambda_model")
after_lambda_model = tensorflow.keras.models.Model(input_layer, lambda_layer, name="after_lambda_model")
```

构建和训练整个网络的完整代码如下所示。

```py
import tensorflow.keras.layers
import tensorflow.keras.models
import tensorflow.keras.optimizers
import tensorflow.keras.datasets
import tensorflow.keras.utils
import tensorflow.keras.backend
import numpy

input_layer = tensorflow.keras.layers.Input(shape=(784), name="input_layer")

dense_layer_1 = tensorflow.keras.layers.Dense(units=500, name="dense_layer_1")(input_layer)
activ_layer_1 = tensorflow.keras.layers.ReLU(name="relu_layer_1")(dense_layer_1)

dense_layer_2 = tensorflow.keras.layers.Dense(units=250, name="dense_layer_2")(activ_layer_1)
activ_layer_2 = tensorflow.keras.layers.ReLU(name="relu_layer_2")(dense_layer_2)

dense_layer_3 = tensorflow.keras.layers.Dense(units=20, name="dense_layer_3")(activ_layer_2)

before_lambda_model = tensorflow.keras.models.Model(input_layer, dense_layer_3, name="before_lambda_model")

def custom_layer(tensor):
    return tensor + 2

lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")(dense_layer_3)
after_lambda_model = tensorflow.keras.models.Model(input_layer, lambda_layer, name="after_lambda_model")

activ_layer_3 = tensorflow.keras.layers.ReLU(name="relu_layer_3")(lambda_layer)

dense_layer_4 = tensorflow.keras.layers.Dense(units=10, name="dense_layer_4")(activ_layer_3)
output_layer = tensorflow.keras.layers.Softmax(name="output_layer")(dense_layer_4)

model = tensorflow.keras.models.Model(input_layer, output_layer, name="model")

model.compile(optimizer=tensorflow.keras.optimizers.Adam(lr=0.0005), loss="categorical_crossentropy")
model.summary()

(x_train, y_train), (x_test, y_test) = tensorflow.keras.datasets.mnist.load_data()

x_train = x_train.astype(numpy.float64) / 255.0
x_test = x_test.astype(numpy.float64) / 255.0

x_train = x_train.reshape((x_train.shape[0], numpy.prod(x_train.shape[1:])))
x_test = x_test.reshape((x_test.shape[0], numpy.prod(x_test.shape[1:])))

y_test = tensorflow.keras.utils.to_categorical(y_test)
y_train = tensorflow.keras.utils.to_categorical(y_train)

model.fit(x_train, y_train, epochs=20, batch_size=256, validation_data=(x_test, y_test))
```

请注意，你不需要编译或训练这两个新创建的模型，因为它们的层实际上是从存在于 `model` 变量中的主模型中重用的。在训练好该模型之后，我们可以使用 `predict()` 方法来返回 `before_lambda_model` 和 `after_lambda_model` 模型的输出，以查看 lambda 层的结果。

```py
p = model.predict(x_train)

m1 = before_lambda_model.predict(x_train)
m2 = after_lambda_model.predict(x_train)
```

接下来的代码仅打印前两个样本的输出。正如你所见，从 `m2` 数组中返回的每个元素实际上是 `m1` 在加上 2 之后的结果。这正是我们在自定义 lambda 层中应用的操作。

```py
print(m1[0, :])
print(m2[0, :])

[ 14.420735    8.872794   25.369402    1.4622561   5.672293    2.5202641
 -14.753801   -3.8822086  -1.0581762  -6.4336205  13.342142   -3.0627508
  -5.694006   -6.557313   -1.6567478  -3.8457105  11.891999   20.581928
   2.669979   -8.092522 ]
[ 16.420734    10.872794    27.369402     3.462256     7.672293
   4.520264   -12.753801    -1.8822086    0.94182384  -4.4336205
  15.342142    -1.0627508   -3.694006    -4.557313     0.34325218
  -1.8457105   13.891999    22.581928     4.669979    -6.0925217 ]
```

在本节中，lambda 层用于对单个输入张量执行操作。在下一节中，我们将看到如何将两个输入张量传递给该层。

### **将多个张量传递给 Lambda 层**

假设我们要执行一个依赖于两个层`dense_layer_3`和`relu_layer_3`的操作。在这种情况下，我们必须在传递两个张量的同时调用 lambda 层。这可以通过创建一个包含所有这些张量的列表来完成，如下一行所示。

```py
lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")([dense_layer_3, activ_layer_3])
```

这个列表被传递给`custom_layer()`函数，我们可以根据下一段代码简单地提取各个层。它只是将这两个层加在一起。实际上，Keras 中有一个名为`Add`的层可以用来添加两个或更多层，但我们只是展示了如何在 Keras 不支持其他操作的情况下自己实现它。

```py
def custom_layer(tensor):
    tensor1 = tensor[0]
    tensor2 = tensor[1]
    return tensor1 + tensor2
```

下一段代码构建了三个模型：两个用于捕捉从`dense_layer_3`和`activ_layer_3`传递到 lambda 层的输出，另一个用于捕捉 lambda 层本身的输出。

```py
before_lambda_model1 = tensorflow.keras.models.Model(input_layer, dense_layer_3, name="before_lambda_model1")
before_lambda_model2 = tensorflow.keras.models.Model(input_layer, activ_layer_3, name="before_lambda_model2")

lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")([dense_layer_3, activ_layer_3])
after_lambda_model = tensorflow.keras.models.Model(input_layer, lambda_layer, name="after_lambda_model")
```

为了查看`dense_layer_3`、`activ_layer_3`和`lambda_layer`层的输出，下一段代码会预测它们的输出并打印出来。

```py
m1 = before_lambda_model1.predict(x_train)
m2 = before_lambda_model2.predict(x_train)
m3 = after_lambda_model.predict(x_train)

print(m1[0, :])
print(m2[0, :])
print(m3[0, :])

[ 1.773366   -3.4378722   0.22042789 11.220362    3.4020965  14.487111
  4.239182   -6.8589864  -6.428128   -5.477719   -8.799093    7.264849
 17.503246   -6.809489   -6.846208   16.094025   24.483786   -7.084775
 17.341183   20.311539  ]
[ 1.773366    0\.          0.22042789 11.220362    3.4020965  14.487111
  4.239182    0\.          0\.          0\.          0\.          7.264849
 17.503246    0\.          0\.         16.094025   24.483786    0.
 17.341183   20.311539  ]
[ 3.546732   -3.4378722   0.44085577 22.440723    6.804193   28.974222
  8.478364   -6.8589864  -6.428128   -5.477719   -8.799093   14.529698
 35.006493   -6.809489   -6.846208   32.18805    48.96757    -7.084775
 34.682365   40.623077  ]
```

使用 lambda 层现在已经很清楚了。下一节讨论了如何保存和加载使用 lambda 层的模型。

### **保存和加载带有 Lambda 层的模型**

为了保存模型（无论是否使用 lambda 层），使用`save()`方法。假设我们只对保存主模型感兴趣，下面是保存它的代码行。

```py
model.save("model.h5")
```

我们还可以使用`load_model()`方法加载保存的模型，如下一行所示。

```py
loaded_model = tensorflow.keras.models.load_model("model.h5")
```

希望模型能够成功加载。不幸的是，Keras 中存在一些问题，可能会导致在加载带有 lambda 层的模型时出现`SystemError: unknown opcode`。这可能是由于使用一个 Python 版本构建模型并在另一个版本中使用它。我们将在下一节中讨论解决方案。

### **解决带有 Lambda 层的模型加载时的 SystemError**

为了解决这个问题，我们不会使用上述讨论的方法保存模型。相反，我们将使用`save_weights()`方法保存模型权重。

现在我们只保存了权重。模型架构呢？模型架构将使用代码重新创建。为什么不将模型架构保存为 JSON 文件，然后再加载它？原因是加载架构后错误仍然存在。

总结来说，训练好的模型权重将被保存，模型架构将使用代码再现，最后权重将被加载到那个架构中。

模型的权重可以使用下一行代码保存。

```py
model.save_weights('model_weights.h5')
```

下面的代码再现了模型架构。`model`将不会被训练，但保存的权重将被重新分配给它。

```py
input_layer = tensorflow.keras.layers.Input(shape=(784), name="input_layer")

dense_layer_1 = tensorflow.keras.layers.Dense(units=500, name="dense_layer_1")(input_layer)
activ_layer_1 = tensorflow.keras.layers.ReLU(name="relu_layer_1")(dense_layer_1)

dense_layer_2 = tensorflow.keras.layers.Dense(units=250, name="dense_layer_2")(activ_layer_1)
activ_layer_2 = tensorflow.keras.layers.ReLU(name="relu_layer_2")(dense_layer_2)

dense_layer_3 = tensorflow.keras.layers.Dense(units=20, name="dense_layer_3")(activ_layer_2)
activ_layer_3 = tensorflow.keras.layers.ReLU(name="relu_layer_3")(dense_layer_3)

def custom_layer(tensor):
    tensor1 = tensor[0]
    tensor2 = tensor[1]

    epsilon = tensorflow.keras.backend.random_normal(shape=tensorflow.keras.backend.shape(tensor1), mean=0.0, stddev=1.0)
    random_sample = tensor1 + tensorflow.keras.backend.exp(tensor2/2) * epsilon
    return random_sample

lambda_layer = tensorflow.keras.layers.Lambda(custom_layer, name="lambda_layer")([dense_layer_3, activ_layer_3])

dense_layer_4 = tensorflow.keras.layers.Dense(units=10, name="dense_layer_4")(lambda_layer)
after_lambda_model = tensorflow.keras.models.Model(input_layer, dense_layer_4, name="after_lambda_model")

output_layer = tensorflow.keras.layers.Softmax(name="output_layer")(dense_layer_4)

model = tensorflow.keras.models.Model(input_layer, output_layer, name="model")

model.compile(optimizer=tensorflow.keras.optimizers.Adam(lr=0.0005), loss="categorical_crossentropy")
```

下面是如何使用`load_weights()`方法加载保存的权重，并将其分配给再现的架构。

```py
model.load_weights('model_weights.h5')
```

### **结论**

本教程讨论了如何使用`Lambda`层创建自定义层，以执行 Keras 中预定义层不支持的操作。`Lambda`类的构造函数接受一个函数，该函数指定了层的工作方式，并接受层调用时的张量。在函数内部，你可以执行任何操作，然后返回修改后的张量。

尽管 Keras 在加载使用 lambda 层的模型时存在问题，但我们也看到如何通过保存训练模型权重、使用代码重现模型架构，并将权重加载到该架构中来简单解决此问题。

**简历: [Ahmed Gad](https://www.linkedin.com/in/ahmedfgad/)** 于 2015 年 7 月获得埃及梅努非亚大学计算机与信息学院（FCI）信息技术学士学位，并以优异的成绩排名第一。由于在学院排名第一，他被推荐于 2015 年在埃及的某所学院担任助教，随后于 2016 年继续担任助教及研究员。他目前的研究兴趣包括深度学习、机器学习、人工智能、数字信号处理和计算机视觉。

[原文](https://blog.paperspace.com/working-with-the-lambda-layer-in-keras/)。经许可转载。

**相关内容:**

+   从 Y=X 到构建完整的人工神经网络

+   轻松使用 torchlayers 构建 PyTorch 模型

+   如何在深度学习中创建自定义实时图表

### 更多相关内容

+   [Python Lambda 函数详解](https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html)

+   [语义层的力量：数据工程师指南](https://www.kdnuggets.com/2023/10/cube-power-of-a-semantic-layer-a-data-engineers-guide)

+   [语义层：AI 驱动数据体验的骨干](https://www.kdnuggets.com/2023/10/cube-semantic-layer-backbone-aipowered-data-experiences)

+   [6 个理由说明通用语义层对你的数据栈有益](https://www.kdnuggets.com/2024/01/cube-6-reasons-why-a-universal-semantic-layer-is-beneficial)

+   [大数据处理：工具与技术](https://www.kdnuggets.com/working-with-big-data-tools-and-techniques)

+   [Python 中 SQLite 数据库使用指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)
