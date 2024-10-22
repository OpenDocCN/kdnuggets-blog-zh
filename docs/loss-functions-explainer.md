# 损失函数：解释

> 原文：[`www.kdnuggets.com/2022/03/loss-functions-explainer.html`](https://www.kdnuggets.com/2022/03/loss-functions-explainer.html)

**损失函数**是一种评估算法学习数据并产生正确输出的方法。它使用数学公式计算预测值与实际值之间的距离。

通俗来说，损失函数衡量模型在估计 x 和 y 之间关系的能力上有多么错误。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

损失函数可以分为两类：

1.  **分类** - 预测标签，通过根据不同的参数识别对象属于哪个类别。

1.  **回归** - 预测连续输出，通过找出依赖变量和自变量之间的相关性。

以下是分类和回归任务中损失函数的类型列表。

# 交叉熵

**交叉熵**和对数损失测量的是相同的内容，但它们并不相同，用于分类任务。交叉熵衡量给定随机变量或事件集的两个概率分布之间的差异。

## 交叉熵的类型：

二分类交叉熵 - 用于二分类任务

+   分类交叉熵 - 用于二分类和多分类任务。这种交叉熵要求标签被编码为分类。例如；3 个类别的独热编码将使用以下表示：[0, 1, 0], [1,0,0]…)

+   稀疏交叉熵 - 用于二分类和多分类任务。这种交叉熵要求标签为整数；0 或 1 或 n

## 交叉熵的代码：

```py
def CrossEntropy(yHat, y):
    if y == 1:
      return -log(yHat)
    else:
      return -log(1 - yHat)
```

# 对数损失

**对数损失**，即交叉熵的二分类形式，用于分类任务。它衡量分类模型的性能，其中输出是一个介于 0 和 1 之间的概率值。

随着预测概率与实际标签的偏差增大，对数损失增加。在理想情况下，完美的模型对数损失为 0。

![损失函数：解释](img/f739f48a00ac473c7aa2f07e97123a57.png)

来源：[ml-cheatsheet](https://ml-cheatsheet.readthedocs.io/en/latest/loss_functions.html)

在上图中，你可以看到当预测概率达到 1 时，Log Loss 减少。另一方面，你可以看到当预测概率减少时，Log Loss 快速增加。

## 使用 Scikit-learn 计算 Log Loss

```py
from sklearn.metrics import log_loss:

LogLoss = log_loss(y_true, y_pred, eps = 1e-15,
    normalize = True, sample_weight = None, labels = None)
```

# Hinge

**Hinge** 是一种用于分类任务的损失函数。这种类型的损失函数在分类边界上引入了一个边际，并对损失进行惩罚。Hinge 函数对错误分类的样本进行惩罚，同时对那些在决策边界定义的边际内正确分类的样本进行惩罚。

![损失函数解释](img/6f2758eea8eb6e7e710645cff3cb3599.png)

来源: [StackOverflow](https://math.stackexchange.com/questions/782586/how-do-you-minimize-hinge-loss)

+   x 轴表示距离边界的距离

+   y 轴表示损失的大小，或者函数将会承受的惩罚。

轴上的虚线表示 1，这意味着当实例与边界的距离大于 1 时，损失大小为 0。如果与边界的距离为 0，损失大小为 1。

正确分类的数据点将具有低或 0 的损失大小；低 Hinge 损失。而错误分类的数据点将具有高或 1 的损失大小；高 Hinge 损失。

## Hinge 损失代码

```py
def Hinge(y_pred, y_true):
   return np.max([0., 1\. - y_pred * y_true])
```

# 均方误差损失

**Mean Square Error Loss** 也被称为 L2 正则化，用于回归任务。它告诉你回归线与一组数据点的接近程度。

它计算当前输出与预期输出之间的平方差，再除以输出的数量。然而，由于使用平方差，均方误差损失对异常值更敏感。

![损失函数解释](img/e087f1e6b8e997ea4958685404ba8c6f.png)

来源: [freecodecamp](https://www.freecodecamp.org/news/machine-learning-mean-squared-error-regression-line-c7dde9a26b93/)

## 均方误差损失代码

```py
def mean_square_error(y_true, y_pred):
   return K.mean(K.square(y_true-y_pred), axis=-1)
```

# 均绝对误差损失

**Mean Absolute Error** 也被称为 L1 正则化，用于回归任务。它计算标签数据和预测数据之间的误差平方的平均值。

它计算当前输出与预期输出之间的绝对差，再除以输出的数量。其目的是最小化这些绝对差异。均绝对误差对异常值不敏感，因为它基于绝对值，而不是像均方误差那样。

为了计算均绝对误差，你需要计算模型预测与实际标签输出之间的差异。然后对差异应用绝对值，并在整个数据集上取平均值。

![损失函数解释](img/dcd53ca57a2d1fc99c851abd9fb8fee8.png)

来源: [TowardDataScience](https://medium.com/analytics-vidhya/a-comprehensive-guide-to-loss-functions-part-1-regression-ff8b847675d6)

## 均绝对误差代码

```py
def mean_abc_error(y_true, y_pred):
   return K.mean(K.abs(y_true-y_pred), axis=-1)
```

# Huber 损失

**Huber 损失** 是均方误差和平均绝对误差的结合。然而，区别在于它受到一个额外参数 delta (δ) 的影响。

这是一种结合了两者的方法，它告诉我们对于小于 delta 的损失值，使用均方误差；对于大于 delta 的损失值，使用平均绝对误差。

在下面的图像中，均方误差用红色表示，平均绝对误差用蓝色表示，Huber 损失用绿色表示。

![损失函数：解释图](img/dde530e71bd8f26927c7761b0e416f7a.png)

来源: [TowardsDataScience](https://towardsdatascience.com/understanding-the-3-most-common-loss-functions-for-machine-learning-regression-23e0ef3e14d3)

## Huber 损失代码

```py
def Huber(yHat, y, delta=1.):
    return np.where(np.abs(y-yHat) < delta,.5*(y-yHat)**2 , delta*(np.abs(y-yHat)-0.5*delta))
```

# 结论

我简要介绍了分类和回归的 3 种损失函数。然而，还有许多其他损失函数可以探索。

以下是一个图像，展示了分类和回归任务的不同损失函数列表。

![损失函数：解释图](img/0c0647892e2e2b528e5aa732c98ddbc7.png)

来源: [Heartbeat](https://heartbeat.comet.ml/5-regression-loss-functions-all-machine-learners-should-know-4fb140e9d4b0)

**[尼莎·阿雅](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由撰稿人。她特别感兴趣于提供数据科学职业建议或教程及理论知识。她还希望探索人工智能如何能够促进人类寿命的延续。作为一个热心学习者，她寻求拓宽技术知识和写作技能，同时帮助指导他人。

### 相关主题

+   [多标签自然语言处理：类不平衡和损失函数的分析…](https://www.kdnuggets.com/2023/03/multilabel-nlp-analysis-class-imbalance-loss-function-approaches.html)

+   [什么是矩生成函数？](https://www.kdnuggets.com/2022/12/momentgenerating-functions.html)

+   [Python Lambda 函数解析](https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html)

+   [你可能不知道的 5 个 Pandas 绘图函数](https://www.kdnuggets.com/2023/02/5-pandas-plotting-functions-might-know.html)

+   [数据库内分析：利用 SQL 的分析函数](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)

+   [你可能不知道的 4 个 Python Itertools 过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)
