# 短期股票预测的 TensorFlow

> 原文：[`www.kdnuggets.com/2017/12/tensorflow-short-term-stocks-prediction.html`](https://www.kdnuggets.com/2017/12/tensorflow-short-term-stocks-prediction.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Mattia Brusamento](https://www.linkedin.com/in/mattia-brusamento/) 提供**

**总结**

在机器学习中，卷积神经网络（CNN 或 ConvNet）是一类成功应用于图像识别和分析的神经网络。在这个项目中，我尝试将这一类模型应用于股票市场预测，将股票价格与情感分析结合起来。网络的实现是使用 TensorFlow，从在线教程开始。在这篇文章中，我将描述以下步骤：数据集创建、CNN 训练和模型评估。

![](img/5387a7361754e11bcdcfa9ba4e58f555.png)

**数据集**

本节简要描述了构建数据集的过程、数据来源和进行的情感分析。

**股票代码**

为了构建数据集，我首先选择了一个行业和一个时间段来关注。我决定选择医疗保健行业，以及 2016 年 1 月 4 日到 2017 年 9 月 30 日的时间范围，并进一步将其拆分为训练集和评估集。具体来说，从 nasdaq.com 下载了股票列表，只保留了市值为 Mega、Large 或 Mid 的公司。从这个股票列表开始，分别使用 Google Finance 和 Intrinio API 检索了股票和新闻数据。

**股票数据**

如前所述，股票数据是通过 Google Finance 历史 API（"https://finance.google.com/finance/historical?q={tick}&startdate={startdate}&output=csv"，针对列表中的每个股票代码）检索的。

时间单位为天，我保留的值是收盘价。为了训练目的，使用线性插值填补了缺失的天数（pandas.DataFrame.interpolate）：

**新闻数据和情感分析**

为了检索新闻数据，我使用了来自 [intrinio](https://intrinio.com/) 的 API。对于每个股票代码，我从" https://api.intrinio.com/news.csv?ticker={tick}"下载了相关的新闻。数据为 csv 格式，包含以下列：

TICKER,FIGI_TICKER,FIGI,TITLE,PUBLICATION_DATE,URL,SUMMARY，以下是一个示例：

> "AAAP,AAAP:UW,BBG007K5CV53,"周四值得关注的 3 只股票：Advanced Accelerator Application SA(ADR) (AAAP)、Jabil Inc (JBL) 和 Medtronic Plc. (MDT)",2017-09-28 15:45:56 +0000,https://articlefeeds.nasdaq.com/~r/nasdaq/symbols/~3/ywZ6I5j5mIE/3-stocks-to-watch-on-thursday-advanced-accelerator-application-saadr-aaap-jabil-inc-jbl-and-medtronic-plc-mdt-cm852684,InvestorPlace 股市新闻、股票建议与交易技巧，大多数主要美国指数在周三上涨，金融股领涨，上涨 1.3%。标准普尔 500 指数上涨 0.4%，道琼斯工业平均指数上涨 0.3%。

新闻基于标题进行了去重。最后保留了 TICKER、PUBLICATION_DATE 和 SUMMARY 列。

对 SUMMARY 列进行了情感分析，使用了 Loughran 和 McDonald 金融情感词典进行金融情感分析，实施在[pysentiment](https://pypi.python.org/pypi/pysentiment) Python 库中。

这个库提供了一个分词器，能够进行词干提取和停用词去除，以及一个评分方法来评估分词文本。选择 get_score 方法中的值作为情感的代理是极性，计算方式如下：

***(#Positives - #Negatives)/(#Positives + #Negatives)***

```py
import pysentiment as ps

lm = ps.LM()
df_news['SUMMARY_SCORES'] = df_news.SUMMARY.map(lambda x: lm.get_score(lm.tokenize(str(x))))
df_news['POLARITY'] = df_news['SUMMARY_SCORES'].map(lambda x: x['Polarity'])

```

没有新闻的日期用 0 填充极性。

最后，数据按股票和日期分组，计算极性得分，对有多个新闻的日期进行求和。

**完整数据集**

通过合并股票和新闻数据，我们得到一个数据集，包含 2016-01-04 到 2017-09-30 的所有 154 个数据点，包含股票的收盘值和相应的极性值：

| 日期 | 股票 | 收盘 | 极性 |
| --- | --- | --- | --- |
| 2017-09-26 | ALXN | 139.700000 | 2.333332 |
| 2017-09-27 | ALXN | 139.450000 | 3.599997 |
| 2017-09-28 | ALXN | 138.340000 | 1.000000 |
| 2017-09-29 | ALXN | 140.290000 | -0.999999 |

**使用 TensorFlow 的 CNN**

为了开始使用 Tensorflow 中的卷积神经网络，我参考了[官方教程](https://www.tensorflow.org/tutorials/layers)。它展示了如何使用层来构建一个卷积神经网络模型，以识别 MNIST 数据集中的手写数字。为了使其适用于我们的目的，我们需要调整输入数据和网络。

**数据模型**

输入数据的模型化方式是单个特征元素为 154x100x2 的张量：

+   154 个数据点

+   100 连续天

+   2 个通道，一个用于股票价格，一个用于极性值

标签则被建模为长度为 154 的向量，其中每个元素为 1，如果相应的股票在次日上涨，否则为 0。

![](img/0ef64c9546571ab1be8c7b10d1c5b0fd.png)

这样，有一个 100 天的滑动时间窗口，因此前 100 天不能用作标签。训练集包含 435 条记录，而评估集包含 100 条记录。

**卷积神经网络**

CNN 的构建从 TensorFlow 的教程示例开始，然后适应了这个使用案例。前两个卷积和池化层的高度都等于 1，因此它们对单个股票进行卷积和池化，最后一层的高度等于 154，以学习股票之间的相关性。最后是全连接层，最后一层的长度为 154，每个股票一个。

网络的规模已经调整到可以在一台笔记本电脑上使用这个数据集训练几个小时。部分代码如下：

```py
def cnn_model_fn(features, labels, mode):  

"""Model function for CNN."""  

# Input Layer  

  input_layer = tf.reshape(tf.cast(features["x"], tf.float32), [-1, 154, 100, 2])

  # Convolutional Layer #1

  conv1 = tf.layers.conv2d(

      inputs=input_layer,

      filters=32,

      kernel_size=[1, 5],

      padding="same",

      activation=tf.nn.relu)

  # Pooling Layer #1

  pool1 = tf.layers.max_pooling2d(inputs=conv1, pool_size=[1, 2], strides=[1,2])

  # Convolutional Layer #2

  conv2 = tf.layers.conv2d(

      inputs=pool1,

      filters=8,

      kernel_size=[1, 5],

      padding="same",

      activation=tf.nn.relu)

  # Pooling Layer #2

  pool2 = tf.layers.max_pooling2d(inputs=conv2, pool_size=[1, 5], strides=[1,5])

  # Convolutional Layer #3

  conv3 = tf.layers.conv2d(

          inputs=pool2,

          filters=2,

          kernel_size=[154, 5],

          padding="same",

          activation=tf.nn.relu)

  # Pooling Layer #3

  pool3 = tf.layers.max_pooling2d(inputs=conv3, pool_size=[1, 2], strides=[1, 2])

  # Dense Layer

  pool3_flat = tf.reshape(pool3, [-1, 154 * 5 * 2])

  dense = tf.layers.dense(inputs=pool3_flat, units=512, activation=tf.nn.relu)

  dropout = tf.layers.dropout(

      inputs=dense, rate=0.4, training=mode == tf.estimator.ModeKeys.TRAIN)

  # Logits Layer

  logits = tf.layers.dense(inputs=dropout, units=154)

  predictions = {

      # Generate predictions (for PREDICT and EVAL mode)

      "classes": tf.argmax(input=logits, axis=1),

      "probabilities": tf.nn.softmax(logits, name="softmax_tensor")

  }

  if mode == tf.estimator.ModeKeys.PREDICT:

    return tf.estimator.EstimatorSpec(mode=mode, predictions=predictions)

  # Calculate Loss (for both TRAIN and EVAL modes)

  multiclass_labels = tf.reshape(tf.cast(labels, tf.int32), [-1, 154])

  loss = tf.losses.sigmoid_cross_entropy(

      multi_class_labels=multiclass_labels, logits=logits)

  # Configure the Training Op (for TRAIN mode)

  if mode == tf.estimator.ModeKeys.TRAIN:

    optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.001)

    train_op = optimizer.minimize(

        loss=loss,

        global_step=tf.train.get_global_step())

    return tf.estimator.EstimatorSpec(mode=mode, loss=loss, train_op=train_op)
```

**评估**

为了评估模型的性能，没有使用标准指标，而是构建了一个更接近实际使用的模型的模拟。

> 假设以初始资本 (**C**) 为 1，对于评估集中的每一天，我们将资本分为 ***N*** 个相等的部分，其中 N 从 1 到 154。
> 
> 我们在模型预测概率最高的前 N 只股票上放置 ***C/N***，其余的股票上放置 0。
> 
> 此时，我们有一个向量 ***A*** 表示我们的每日分配，我们可以计算每日的收益/损失，方法是将 A 乘以每只股票当天的百分比变化。
> 
> 我们得到一个新的资本 ***C = C + delta***，可以在第二天重新投资。
> 
> 最终，我们会得到一个大于或小于 1 的资本，取决于我们选择的好坏。

模型的一个良好基准已确定为 *N=154*：这代表了所有股票的通用性能，并模拟了将资本平均分配到所有股票上的情景。这产生了大约 **4.27%** 的收益。

为了评估目的，数据已被纠正，移除了市场关闭的天数。

不同 N 值下模型的性能在下面的图片中报告。

![](img/9c1a2b6f708c53059b12820c590b7509.png)

红色虚线是 0 基准线，而橙色线是基准线，*N=154*。

最佳性能是在 ***N=12*** 下获得的，*收益约为 **8.41%**，几乎是市场基准的两倍。

对于几乎所有大于 10 的 N 值，我们都有不错的表现，优于基准线，而 N 值过小则会降低性能。

**结论**

第一次尝试 Tensorflow 和 CNN 并将其应用于金融数据非常有趣。

这是一个玩具示例，使用了相当小的数据集和网络，但它展示了这些模型的潜力。

请随时提供反馈和建议，或者直接通过 LinkedIn 与我联系。

**简介：** Mattia 在米兰理工大学获得了计算机工程硕士学位（荣誉），在 TU Delft 期间研究了推荐系统的论文。Mattia 目前在一家意大利公司担任网络安全领域的数据科学家。

**相关**

+   [**通过实际应用案例深入理解深度卷积神经网络（Tensorflow 和 Keras）**](https://www.kdnuggets.com/2017/11/understanding-deep-convolutional-neural-networks-tensorflow-keras.html)

+   [**探索递归神经网络**](https://www.kdnuggets.com/2017/12/exploring-recurrent-neural-networks.html)

+   [**数据科学家：华尔街最炙手可热的职位**](https://www.kdnuggets.com/2017/11/data-scientist-hottest-job-wall-street.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业领域。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

### 更多相关话题

+   [数据科学项目：Rotten Tomatoes 电影评分预测：…](https://www.kdnuggets.com/2023/06/data-science-project-rotten-tomatoes-movie-rating-prediction-first-approach.html)

+   [数据科学项目：Rotten Tomatoes 电影评分预测：…](https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html)

+   [多变量时间序列预测与 BQML](https://www.kdnuggets.com/2023/07/multivariate-timeseries-prediction-bqml.html)

+   [TensorFlow 在计算机视觉中的应用 - 转移学习轻松实现](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [PyTorch 还是 TensorFlow？对比流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [Tensorflow 的 "Hello World"](https://www.kdnuggets.com/2022/05/hello-world-tensorflow.html)
