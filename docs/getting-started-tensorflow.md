# TensorFlow 入门：机器学习教程

> 原文：[https://www.kdnuggets.com/2017/12/getting-started-tensorflow.html/2](https://www.kdnuggets.com/2017/12/getting-started-tensorflow.html/2)

**数据转换**

**减少**

TensorFlow 支持不同类型的减少。减少是一种通过在这些维度上执行某些操作来从张量中移除一个或多个维度的操作。当前版本 TensorFlow 支持的减少操作列表可以在这里找到。我们将在下面的示例中介绍其中的一些。

```py
import tensorflow as tf
import numpy as np

def convert(v, t=tf.float32):
    return tf.convert_to_tensor(v, dtype=t)

x = convert(
    np.array(
        [
            (1, 2, 3),
            (4, 5, 6),
            (7, 8, 9)
        ]), tf.int32)

bool_tensor = convert([(True, False, True), (False, False, True), (True, False, False)], tf.bool)

red_sum_0 = tf.reduce_sum(x)
red_sum = tf.reduce_sum(x, axis=1)

red_prod_0 = tf.reduce_prod(x)
red_prod = tf.reduce_prod(x, axis=1)

red_min_0 = tf.reduce_min(x)
red_min = tf.reduce_min(x, axis=1)

red_max_0 = tf.reduce_max(x)
red_max = tf.reduce_max(x, axis=1)

red_mean_0 = tf.reduce_mean(x)
red_mean = tf.reduce_mean(x, axis=1)

red_bool_all_0 = tf.reduce_all(bool_tensor)
red_bool_all = tf.reduce_all(bool_tensor, axis=1)

red_bool_any_0 = tf.reduce_any(bool_tensor)
red_bool_any = tf.reduce_any(bool_tensor, axis=1)

with tf.Session() as session:
    print "Reduce sum without passed axis parameter: ", session.run(red_sum_0)
    print "Reduce sum with passed axis=1: ", session.run(red_sum)

    print "Reduce product without passed axis parameter: ", session.run(red_prod_0)
    print "Reduce product with passed axis=1: ", session.run(red_prod)

    print "Reduce min without passed axis parameter: ", session.run(red_min_0)
    print "Reduce min with passed axis=1: ", session.run(red_min)

    print "Reduce max without passed axis parameter: ", session.run(red_max_0)
    print "Reduce max with passed axis=1: ", session.run(red_max)

    print "Reduce mean without passed axis parameter: ", session.run(red_mean_0)
    print "Reduce mean with passed axis=1: ", session.run(red_mean)

    print "Reduce bool all without passed axis parameter: ", session.run(red_bool_all_0)
    print "Reduce bool all with passed axis=1: ", session.run(red_bool_all)

    print "Reduce bool any without passed axis parameter: ", session.run(red_bool_any_0)
    print "Reduce bool any with passed axis=1: ", session.run(red_bool_any)

```

输出：

```py
Reduce sum without passed axis parameter:  45
Reduce sum with passed axis=1:  [ 6 15 24]
Reduce product without passed axis parameter:  362880
Reduce product with passed axis=1:  [  6 120 504]
Reduce min without passed axis parameter:  1
Reduce min with passed axis=1:  [1 4 7]
Reduce max without passed axis parameter:  9
Reduce max with passed axis=1:  [3 6 9]
Reduce mean without passed axis parameter:  5
Reduce mean with passed axis=1:  [2 5 8]
Reduce bool all without passed axis parameter:  False
Reduce bool all with passed axis=1:  [False False False]
Reduce bool any without passed axis parameter:  True
Reduce bool any with passed axis=1:  [ True  True  True]

```

减少操作的第一个参数是我们想要减少的张量。第二个参数是我们希望在其上执行减少操作的维度索引。该参数是可选的，如果未传递，减少操作将沿所有维度进行。

我们可以查看 [reduce_sum](https://www.tensorflow.org/versions/master/api_docs/python/tf/reduce_sum) 操作。我们传递一个 2 维张量，并希望沿维度 1 进行减少。

在我们的例子中，结果的总和将是：

```py
[1 + 2 + 3 = 6, 4 + 5 + 6 = 15, 7 + 8 + 9 = 24]

```

如果我们传递了维度 0，结果将是：

```py
[1 + 4 + 7 = 12, 2 + 5 + 8 = 15, 3 + 6 + 9 = 18]

```

如果我们不传递任何轴，结果只是整体总和：

```py
1 + 4 + 7 = 12, 2 + 5 + 8 = 15, 3 + 6 + 9 = 45

```

所有减少函数具有类似的接口，并在 TensorFlow [减少文档](https://www.tensorflow.org/versions/r0.12/api_docs/python/math_ops/reduction) 中列出。

**分割**

分割是一个过程，其中一个维度是将维度映射到提供的段索引的过程，结果元素由索引行决定。

分割实际上是将元素按重复索引分组，例如，在我们的例子中，我们对张量 `tens1` 应用分段 ids `[0, 0, 1, 2, 2]`，这意味着第一个和第二个数组将按照分割操作（在我们的例子中是求和）进行变换，并得到一个新的数组，形式如 `(2, 8, 1, 0) = (2+0, 5+3, 3-2, -5+5)`。张量 `tens1` 中的第三个元素没有被分组，因为它没有重复的索引，最后两个数组以与第一个组相同的方式相加。除了求和，TensorFlow 还支持 [product](https://www.tensorflow.org/versions/master/api_docs/python/tf/segment_prod)、[mean](https://www.tensorflow.org/versions/master/api_docs/python/tf/segment_mean)、[max](https://www.tensorflow.org/versions/master/api_docs/python/tf/segment_max) 和 [min](https://www.tensorflow.org/versions/master/api_docs/python/tf/segment_min)。

![](../Images/8626a778161812ae5f56cd7be77da7ed.png)

```py
import tensorflow as tf
import numpy as np

def convert(v, t=tf.float32):
    return tf.convert_to_tensor(v, dtype=t)

seg_ids = tf.constant([0, 0, 1, 2, 2])
tens1 = convert(np.array([(2, 5, 3, -5), (0, 3, -2, 5), (4, 3, 5, 3), (6, 1, 4, 0), (6, 1, 4, 0)]), tf.int32)
tens2 = convert(np.array([1, 2, 3, 4, 5]), tf.int32)

seg_sum = tf.segment_sum(tens1, seg_ids)
seg_sum_1 = tf.segment_sum(tens2, seg_ids)

with tf.Session() as session:
    print "Segmentation sum tens1: ", session.run(seg_sum)
    print "Segmentation sum tens2: ", session.run(seg_sum_1)

```

```py
Segmentation sum tens1:  
[[ 2  8  1  0]
 [ 4  3  5  3]
 [12  2  8  0]]

Segmentation sum tens2: [3 3 9]

```

**序列实用工具**

序列实用工具包括如下方法：

+   [argmin](https://www.tensorflow.org/versions/master/api_docs/python/tf/argmin) 函数返回输入张量在各轴上的最小值的索引，

+   [argmax](https://www.tensorflow.org/versions/master/api_docs/python/tf/argmax) 函数返回输入张量在各轴上的最大值的索引，

+   [setdiff](https://www.tensorflow.org/versions/master/api_docs/python/tf/setdiff1d)，它计算两个数字或字符串列表之间的差异，

+   [where](https://www.tensorflow.org/api_docs/python/tf/where)函数，它将根据传递的条件从两个传递的元素x或y中返回元素，或者

+   [unique](https://www.tensorflow.org/versions/master/api_docs/python/tf/unique)函数，它将返回1-D张量中的唯一元素。

我们下面演示了几个执行示例：

```py
import numpy as np
import tensorflow as tf

def convert(v, t=tf.float32):
    return tf.convert_to_tensor(v, dtype=t)

x = convert(np.array([
    [2, 2, 1, 3],
    [4, 5, 6, -1],
    [0, 1, 1, -2],
    [6, 2, 3, 0]
]))

y = convert(np.array([1, 2, 5, 3, 7]))
z = convert(np.array([1, 0, 4, 6, 2]))

arg_min = tf.argmin(x, 1)
arg_max = tf.argmax(x, 1)
unique = tf.unique(y)
diff = tf.setdiff1d(y, z)

with tf.Session() as session:
    print "Argmin = ", session.run(arg_min)
    print "Argmax = ", session.run(arg_max)

    print "Unique_values = ", session.run(unique)[0]
    print "Unique_idx = ", session.run(unique)[1]

    print "Setdiff_values = ", session.run(diff)[0]
    print "Setdiff_idx = ", session.run(diff)[1]

    print session.run(diff)[1]

```

输出：

```py
Argmin = [2 3 3 3]
Argmax =  [3 2 1 0]
Unique_values =  [ 1\.  2\.  5\.  3\.  7.]
Unique_idx =  [0 1 2 3 4]
Setdiff_values =  [ 5\.  3\.  7.]
Setdiff_idx =  [2 3 4]

```

**使用TensorFlow进行机器学习**

在这一部分，我们将展示一个使用TensorFlow的机器学习用例。第一个示例将是一个使用[kNN](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)方法进行数据分类的算法，第二个示例将使用[线性回归算法](https://en.wikipedia.org/wiki/Linear_regression)。

**kNN**

第一个算法是k-最近邻（kNN）。这是一个监督学习算法，它使用距离度量（例如欧几里得距离）来将数据分类为训练数据。它是最简单的算法之一，但仍然非常强大，适合数据分类。该算法的优点：

+   当训练模型足够大时，准确率很高，并且

+   通常不对异常值敏感，我们不需要对数据做任何假设。

该算法的缺点：

+   计算开销大，并且

+   需要大量内存，其中新的分类数据需要添加到所有初始训练实例中。

![](../Images/61b097d4eeb897b627e93adfa030aa28.png)

在这个代码示例中，我们将使用的距离是欧几里得距离，它定义了两个点之间的距离，如下所示：

![](../Images/6379458d1999e254d668ef45cbb393d3.png)

在这个公式中，`n`是空间的维度数量，`x`是训练数据的向量，`y`是我们想要分类的新数据点。

```py
import os
import numpy as np
import tensorflow as tf

ccf_train_data = "train_dataset.csv"
ccf_test_data = "test_dataset.csv"

dataset_dir = os.path.abspath(os.path.join(os.path.dirname(__file__), '../datasets'))

ccf_train_filepath = os.path.join(dataset_dir, ccf_train_data)
ccf_test_filepath = os.path.join(dataset_dir, ccf_test_data)

def load_data(filepath):
    from numpy import genfromtxt

    csv_data = genfromtxt(filepath, delimiter=",", skip_header=1)
    data = []
    labels = []

    for d in csv_data:
        data.append(d[:-1])
        labels.append(d[-1])

    return np.array(data), np.array(labels)

train_dataset, train_labels = load_data(ccf_train_filepath)
test_dataset, test_labels = load_data(ccf_test_filepath)

train_pl = tf.placeholder("float", [None, 28])
test_pl = tf.placeholder("float", [28])

knn_prediction = tf.reduce_sum(tf.abs(tf.add(train_pl, tf.negative(test_pl))), axis=1)

pred = tf.argmin(knn_prediction, 0)

with tf.Session() as tf_session:
    missed = 0

    for i in xrange(len(test_dataset)):
        knn_index = tf_session.run(pred, feed_dict={train_pl: train_dataset, test_pl: test_dataset[i]})

        print "Predicted class {} -- True class {}".format(train_labels[knn_index], test_labels[i])

        if train_labels[knn_index] != test_labels[i]:
            missed += 1

    tf.summary.FileWriter("../samples/article/logs", tf_session.graph)

print "Missed: {} -- Total: {}".format(missed, len(test_dataset))

```

我们在上述示例中使用的数据集可以在[Kaggle数据集](https://www.kaggle.com/datasets)部分找到。我们使用了[这个](https://www.kaggle.com/dalpozz/creditcardfraud)数据集，它包含了欧洲持卡人的信用卡交易数据。我们使用的数据没有进行任何清理或过滤，按照Kaggle对这个数据集的描述，它是高度不平衡的。数据集包含31个变量：Time、V1、…、V28、Amount和Class。在这个代码示例中，我们仅使用V1、…、V28和Class。Class用1标记欺诈交易，用0标记非欺诈交易。

代码示例主要包含我们在之前部分描述的内容，唯一的例外是我们引入了一个用于加载数据集的函数。函数`load_data(filepath)`将以CSV文件作为参数，并返回一个包含数据和标签的元组，这些数据和标签在CSV中定义。

在那个函数下方，我们定义了测试和训练数据的占位符。训练数据用于预测模型，以解决需要分类的输入数据的标签。在我们的例子中，kNN 使用欧几里得距离来获取最近的标签。

错误率可以通过将分类器错过的样本数量除以总样本数量来计算，对于我们这个数据集来说是 0.2（即分类器给出 20% 测试数据的错误数据标签）。

### 线性回归

线性回归算法寻求两个变量之间的线性关系。如果我们将因变量标记为 y，自变量标记为 x，那么我们试图估计函数`y = Wx + b`的参数。

线性回归是一种在应用科学领域广泛使用的算法。这个算法在实现中可以结合两个重要的机器学习概念： [成本函数](https://en.wikipedia.org/wiki/Loss_function) 和 [梯度下降法](https://en.wikipedia.org/wiki/Gradient_descent) 来找到函数的最小值。

使用这种方法实现的机器学习算法必须预测 `y` 作为 `x` 的函数，其中线性回归算法将确定 `W` 和 `b` 的值，这些值实际上是未知的，并且在训练过程中确定。选择一个成本函数，通常使用均方误差，梯度下降是用于找到成本函数局部最小值的优化算法。

梯度下降法只能找到局部最小值，但可以通过在找到局部最小值后随机选择新的起点并重复这一过程来搜索全局最小值。如果函数的极小值数量有限，并且尝试次数非常多，那么在某些时刻找到全局最小值的机会就会很大。有关这种技术的更多细节，我们将留给[文章](https://www.toptal.com/machine-learning/machine-learning-theory-an-introductory-primer)，我们在引言部分提到过。

```py
import tensorflow as tf
import numpy as np

test_data_size = 2000
iterations = 10000
learn_rate = 0.005

def generate_test_values():
    train_x = []
    train_y = []

    for _ in xrange(test_data_size):
        x1 = np.random.rand()
        x2 = np.random.rand()
        x3 = np.random.rand()
        y_f = 2 * x1 + 3 * x2 + 7 * x3 + 4
        train_x.append([x1, x2, x3])
        train_y.append(y_f)

    return np.array(train_x), np.transpose([train_y])

x = tf.placeholder(tf.float32, [None, 3], name="x")
W = tf.Variable(tf.zeros([3, 1]), name="W")
b = tf.Variable(tf.zeros([1]), name="b")
y = tf.placeholder(tf.float32, [None, 1])

model = tf.add(tf.matmul(x, W), b)

cost = tf.reduce_mean(tf.square(y - model))
train = tf.train.GradientDescentOptimizer(learn_rate).minimize(cost)

train_dataset, train_values = generate_test_values()

init = tf.global_variables_initializer()

with tf.Session() as session:
    session.run(init)

    for _ in xrange(iterations):

        session.run(train, feed_dict={
            x: train_dataset,
            y: train_values
        })

    print "cost = {}".format(session.run(cost, feed_dict={
        x: train_dataset,
        y: train_values
    }))

    print "W = {}".format(session.run(W))
    print "b = {}".format(session.run(b))

```

输出：

```py
cost = 3.1083032809e-05
W = [[ 1.99049103]
 [ 2.9887135 ]
 [ 6.98754263]]
b = [ 4.01742554]

```

在上述示例中，我们有两个新变量，分别称为 `cost` 和 `train`。通过这两个变量，我们定义了一个优化器，用于我们的训练模型以及我们希望最小化的函数。

最终，`W` 和 `b` 的输出参数应与 `generate_test_values` 函数中定义的参数相同。在第 17 行，我们实际上定义了一个函数，用于生成线性数据点进行训练，其中 `w1=2`，`w2=3`，`w3=7` 和 `b=4`。上述示例中的线性回归是多变量的，其中使用了多个自变量。

**结论**

从这个 TensorFlow 教程中可以看出，TensorFlow 是一个强大的框架，使得处理数学表达式和多维数组变得轻松——这是机器学习中根本上必要的。它还抽象化了执行数据图和扩展的复杂性。

随着时间的推移，TensorFlow 的受欢迎程度不断上升，现在开发人员利用深度学习方法解决图像识别、视频检测、情感分析等问题。像其他库一样，您可能需要一些时间来熟悉 TensorFlow 的基本概念。一旦您掌握了这些概念，通过文档和社区支持，利用 TensorFlow 将问题表示为数据图并加以解决，可以使大规模机器学习变得不那么繁琐。

[原始](https://www.toptal.com/machine-learning/tensorflow-machine-learning-tutorial)。经许可转载。

**个人简介： [Dino](https://www.toptal.com/resume/dino-causevic)** 拥有五年以上软件开发经验。在过去的两年中，他主要从事 Java 和相关技术的工作，涉及使用 NoSQL 技术实现大数据解决方案和实现 REST 服务。他常驻萨拉热窝，是 Toptal 的成员。

**相关**

+   [**NIPS 2017 重点 & 总结笔记**](https://www.kdnuggets.com/2017/12/nips-2017-key-points-summary-notes.html)

+   [**TensorFlow 短期股票预测**](https://www.kdnuggets.com/2017/12/tensorflow-short-term-stocks-prediction.html)

+   [**加速算法：设计、算法选择和实现中的考虑**](https://www.kdnuggets.com/2017/12/accelerating-algorithms-design-choice-implementation.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关内容

+   [联邦学习：协作机器学习教程……](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)

+   [开始使用 Scikit-learn 进行机器学习分类](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [开始自动化文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [开始清理数据](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)

+   [SQL 入门速查表](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [spaCy 入门指南](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)
ng-tutorial-get-started.html)

+   [入门 Scikit-learn 用于机器学习中的分类](https://www.kdnuggets.com/getting-started-with-scikit-learn-for-classification-in-machine-learning)

+   [入门自动化文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [入门数据清理](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)

+   [入门 SQL 备忘单](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)

+   [入门 spaCy 用于自然语言处理](https://www.kdnuggets.com/2022/11/getting-started-spacy-nlp.html)
