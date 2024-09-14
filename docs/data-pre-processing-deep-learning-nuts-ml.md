# 数据预处理在深度学习中的作用

> 原文：[`www.kdnuggets.com/2017/05/data-pre-processing-deep-learning-nuts-ml.html`](https://www.kdnuggets.com/2017/05/data-pre-processing-deep-learning-nuts-ml.html)

**由 Stefan Maetschke 博士撰写。**

数据预处理是任何机器学习项目的基础部分，通常在数据准备上花费的时间比实际的机器学习更多。虽然一些预处理任务是特定问题的，许多其他任务，如将数据划分为训练集和测试集、样本分层或构建小批量是通用的。以下的*标准管道*展示了视觉领域深度学习的常见处理步骤。

![](img/2b264a4fa4eb7d575994203692befe3c.png)

**读取器**从文本文件、Excel 或 Pandas 表格中读取样本数据。然后，**拆分器**将数据分割成训练集、验证集和测试集，并在需要时进行分层抽样。通常，所有的图像数据无法一次性加载到内存中，**加载器**根据需求加载图像。这些图像通常会由**变换器**处理，如调整大小、裁剪或其他调整。此外，为了增加训练集，**增强器**通过随机增强（翻转、旋转等）图像来合成额外的图像。高效的基于 GPU 的机器学习要求通过**批处理器**将图像和标签数据分组到小批量中，然后传递给**网络**进行训练或推断。最后，为了跟踪训练进度，通常会使用**记录器**将训练损失或准确率写入日志文件。

一些机器学习框架，如 [Keras](https://keras.io/)，提供了这些预处理组件中的部分功能，并隐藏在 API 后面，如果适合当前任务，这会大大简化网络训练。请参阅以下 [Keras 示例](https://github.com/fchollet/keras/blob/master/examples/cifar10_cnn.py) 来训练一个带增强的模型。

```py
datagen = ImageDataGenerator(  # augment images
    width_shift_range=0.1,  
    height_shift_range=0.1, 
    horizontal_flip=True)   

datagen.fit(x_train)

model.fit_generator(datagen.flow(x_train, y_train, batch_size=batch_size),
    steps_per_epoch=x_train.shape[0] 
    epochs=epochs,
    validation_data=(x_test, y_test))
```

然而，如果需要一个 API 未提供的图像格式、增强或其他预处理功能怎么办？扩展像 Keras 这样的库并非易事，通常的做法是（重新）实现所需的功能——通常是以快速而粗糙的方式。但实现一个稳健的数据管道，按需加载、变换、增强和处理图像是具有挑战性和耗时的。

[nuts-ml](https://maet3608.github.io/nuts-ml/index.html) 是一个 Python 库，提供了称为*nuts*的常见预处理函数，这些函数可以自由排列并轻松扩展，以构建高效的数据处理管道。以下摘录自 [nuts-ml 示例](https://github.com/maet3608/nuts-ml/blob/master/nutsml/examples/cifar/cnn_train.py) 显示了一个网络训练管道，其中 `>>` 操作符定义了数据的流动。

```py
t_loss = (train_samples >> augment >> rerange >> Shuffle(100) >> 
          build_batch >> network.train() >> Mean())
print "training loss  :", t_loss
```

在上述示例中，训练图像被增强，像素值被重新调整，并且样本在构建网络训练批次之前被打乱。最后，计算并打印每批次训练损失的均值。这个数据流的组成部分可以定义如下

```py
rerange = TransformImage(0).by('rerange', 0, 255, 0, 1, 'float32')

augment = (AugmentImage(0)
           .by('identical', 1.0)
           .by('brightness', 0.1, [0.7, 1.3])
           .by('fliplr', 0.1)))

build_batch = (BuildBatch(BATCH_SIZE)
               .by(0, 'image', 'float32')
               .by(1, 'one_hot', 'uint8', NUM_CLASSES))           

network = KerasNetwork(model)
```

其中，rerange 是一种图像转换，它将像素值从范围 [0, 255] 转换到范围 [0, 1]，augment 通过随机水平翻转和改变图像亮度来生成额外的训练图像，build_batch 构建由图像和一-hot 编码类标签组成的批次，而 network 将现有的 Keras 模型包装成可以插入管道的 nut。这个示例的完整代码可以在这里找到

nuts-ml 帮助更快速地构建深度学习的数据预处理管道。生成的代码更具可读性，并且可以很容易地修改以尝试不同的预处理方案。任务特定的函数可以轻松地作为 nuts 实现并添加到数据流中。例如，这里是一个调整图像亮度的简单 nut

```py
@nut_function
def AdjustBrightness(image, c):
  return image * c

... images >> AdjustBrightness(1.1) >> ...  
```

nuts-ml 不进行网络训练，而是使用现有的库，如 Keras 或 Theano。任何接受 Numpy 数组的小批量用于训练或推理的机器学习库都是兼容的。有关 nuts-ml 的更多信息，请参见介绍部分并查看教程。

**简历：[Stefan Maetschke (PhD)](http://researcher.ibm.com/researcher/view.php?person=au1-stefanrm)** 是 IBM Research Australia 的研究科学家，他在这里开发用于医学图像分析的机器学习基础设施和模型。

**相关内容：**

+   使用深度学习进行医学图像分析，第二部分

+   使用深度学习进行医学图像分析

+   负面图像上的负面结果：深度学习的主要缺陷？

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

### 更多相关内容

+   [通过这本免费电子书学习数据清洗和预处理](https://www.kdnuggets.com/2023/08/learn-data-cleaning-preprocessing-data-science-free-ebook.html)

+   [Python 中数据预处理的简易指南](https://www.kdnuggets.com/2020/07/easy-guide-data-preprocessing-python.html)

+   [掌握数据清洗和预处理技术的 7 个步骤](https://www.kdnuggets.com/2023/08/7-steps-mastering-data-cleaning-preprocessing-techniques.html)

+   [利用 ChatGPT 实现自动化的数据清洗和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [在 Pandas 中清洗和预处理文本数据以用于 NLP 任务](https://www.kdnuggets.com/cleaning-and-preprocessing-text-data-in-pandas-for-nlp-tasks)

+   [学习数据科学、机器学习和深度学习的扎实计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)
