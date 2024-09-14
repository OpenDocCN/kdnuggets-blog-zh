# 逻辑回归用于分类

> 原文：[https://www.kdnuggets.com/2022/04/logistic-regression-classification.html](https://www.kdnuggets.com/2022/04/logistic-regression-classification.html)

在我们更深入了解逻辑回归之前，让我们先回顾一些重要的定义，这将帮助我们更好地理解这个主题。

逻辑回归属于监督学习。**监督学习**是指算法在带标签的数据集上进行学习，并分析训练数据。这些带标签的数据集有输入和预期输出。

监督学习可以进一步分为分类和回归。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

**分类**是关于预测标签，通过识别一个对象基于不同参数属于哪个类别。

**回归**是关于预测连续输出，通过找出因变量和自变量之间的相关性。

![logistic-regression-classification](../Images/ca7b5bcd2d0054c7e4111ba309667d34.png)

来源：[Javatpoint](https://www.javatpoint.com/regression-vs-classification-in-machine-learning)

# 什么是逻辑回归？

**逻辑回归**是一种统计方法和机器学习算法，用于分类问题，基于概率的概念。它用于当因变量（目标）是分类变量时。

当分类问题是二元的，例如是或否、对或错时，它被广泛使用。例如，它可以用于预测一封邮件是否是垃圾邮件（1）或不是（0）。

逻辑回归使用 Sigmoid 函数来返回标签的概率。

## Sigmoid 函数

Sigmoid 函数是一个数学函数，用于将预测值映射到概率值。该函数具有将任何实数值映射到 0 和 1 之间的另一个值的能力。

代码：

```py
def sigmoid(z):
  return 1.0 / (1 + np.exp(-z))
```

![Sigmoid_function](../Images/75b141512a120c90f61e7040833099ed.png)

来源：[维基百科](https://en.wikipedia.org/wiki/Sigmoid_function)

规则是逻辑回归的值必须在 0 和 1 之间。由于不能超过值 1 的限制，图形上形成一个“S”形的曲线。这是识别 Sigmoid 函数或逻辑函数的简单方法。

关于逻辑回归，使用的概念是阈值。阈值有助于定义0或1的概率。例如，阈值以上的值倾向于1，而阈值以下的值倾向于0。

## 逻辑回归的类型

1.  **二项型：** 这意味着因变量只能有两种可能的类型，如0或1，或者是“是”或“否”等。

1.  **多项型：** 这意味着因变量可以有3种或更多种可能的无序类型，例如“猫”，“狗”或“羊”。

1.  **有序型：** 这意味着因变量可以有3种或更多种可能的有序类型，例如“低”，“中”等。

# 线性回归和逻辑回归

线性回归与逻辑回归相似但有所不同。

线性回归假设因变量和自变量之间存在线性关系。它使用描述两个或多个变量的最佳拟合线。线性回归的目标是准确预测连续因变量的输出。

然而，逻辑回归预测的是依赖于其他因素的事件或类别的概率，因此逻辑回归的输出总是介于0和1之间。

要了解更多关于线性回归和逻辑回归之间的差异，可以在这个链接中阅读更多信息。

![线性回归和逻辑回归](../Images/4f141643a771af334341c451bc56eff2.png)

来源：[gfycat](https://gfycat.com/)

# 成本函数

成本函数是用于计算误差的数学公式，它是预测值和实际值之间的差异。它简单地衡量模型在估计 x 和 y 之间关系的能力方面有多么错误。

成本函数的值也可以称为成本、损失或误差。在逻辑回归中，我们使用的成本函数称为 [交叉熵](https://ml-cheatsheet.readthedocs.io/en/latest/loss_functions.html#loss-cross-entropy)，也称为对数损失。其公式为：

如果你想了解更多不同类型的成本函数，请点击这个链接。

# 梯度下降法

为了最小化我们的成本，我们使用梯度下降法来估计模型的参数或权重。

![初学者如何将逻辑回归与线性回归相关联](../Images/0368b97b7427af5701511ec416272356.png)来源：[analyticsvidhya](https://www.analyticsvidhya.com/blog/2020/12/beginners-take-how-logistic-regression-is-related-to-linear-regression/)

# 逻辑回归的实现

为了举一个实现逻辑回归的简单例子，我将使用一个来自 [kaggle](https://www.kaggle.com/rakeshrau/social-network-ads) 的数据集，该数据集探讨了通过社交媒体广告购买产品的信息。

数据集来源 - [https://www.kaggle.com/rakeshrau/social-network-ads](https://www.kaggle.com/rakeshrau/social-network-ads)

## 1. 加载数据集和库

```py
# Import Libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from math import exp
plt.rcParams["figure.figsize"] = (10, 6)# Download your chosen dataset
# Source of dataset - https://www.kaggle.com/rakeshrau/social-network-ads
# !wget "https://drive.google.com/uc?id=15WAD9_4CpUK6EWmgWVXU8YMnyYLKQvW8&export=download" -O data.csv -q# Load the dataset
data = pd.read_csv("data.csv")
data.head(10)
```

![](../Images/b30ff09c4e12e409c8a94e9863e7c0ef.png)

## 2\. 训练/测试数据

```py
# Visualizing the dataset by Age and Purchased
plt.scatter(data['Age'], data['Purchased'])
plt.show()# Divide the Data into training set and test set
X_train, X_test, y_train, y_test = train_test_split(data['Age'], data['Purchased'], test_size=0.20)
```

![](../Images/370c0df80c5ed826f4654a76087982e7.png)

## 3\. 构建模型

我们需要对数据进行归一化，并将均值移至原点。这是由于逻辑方程的性质，我们希望获得准确的结果。

然后我们创建一个方法来帮助我们进行预测，该方法将返回一个概率。接下来我们可以开始训练模型。模型训练了300个周期，在这300个周期中计算了偏导数，并更新了权重。

```py
# Building the Logistic Regression model

# Normalising the data
def normalize(X):
  return X - X.mean()

# Make predictions
def predict(X, b0, b1):
  return np.array([1 / (1 + exp(-1*b0 + -1*b1*x)) for x in X])

# The model
def logistic_regression(X, Y):

   X = normalize(X)

   # Initializing variables
   b0 = 0
   b1 = 0
   L = 0.001
   epochs = 300

   for epoch in range(epochs):
      y_pred = predict(X, b0, b1)
      D_b0 = -2 * sum((Y - y_pred) * y_pred * (1 - y_pred)) # Loss wrt b0
      D_b1 = -2 * sum(X * (Y - y_pred) * y_pred * (1 - y_pred)) # Loss wrt b1
      # Update b0 and b1
      b0 = b0 - L * D_b0
      b1 = b1 - L * D_b1

      return b0, b1
```

## 4\. 训练模型

如上所述，预测方程将返回一个概率。由于这是一个分类任务，我们需要将其转换为二进制值。为此，我们需要选择一个阈值。在这个例子中，我们选择阈值0.5，这意味着所有预测值大于0.5的将被视为1，而小于0.5的将被视为0。

你还可以通过检查我们做了多少正确预测并将其除以测试案例的总数来计算准确率。

```py
# Training the Model
b0, b1 = logistic_regression(X_train, y_train)# Making predictions and setting a thresholdX_test_norm = normalize(X_test)
y_pred = predict(X_test_norm, b0, b1)
y_pred = [1 if p >= 0.5 else 0 for p in y_pred]
# Plotting the data
plt.scatter(X_test, y_test)
plt.scatter(X_test, y_pred, c="red")
plt.show()# Calculating the accuracy
accuracy = 0
for i in range(len(y_pred)):
if y_pred[i] == y_test.iloc[i]:
accuracy += 1
print(f"Accuracy = {accuracy / len(y_pred)}")
```

绘图和准确率：

![](../Images/49ebae219ed0926a06128398dec4da66.png)

```py
Accuracy = 0.85
```

使用下面的gify可以帮助我们直观地看到每次迭代中权重b0和b1的更新情况。

![绘图和准确率](../Images/834221b2c63eef0fa4f876fd92d6ffc3.png)

来源：[gfycat](https://gfycat.com/)

# 结论

逻辑回归是一种广泛使用的技术，因为它非常高效且不需要大量计算资源。当你移除与输出变量无关或关系很小的变量时，逻辑回归的效果会更好。因此，特征工程在逻辑回归的性能中是一个重要因素。

逻辑回归非常适合分类任务，但它并不是最强大的算法之一。它很容易被其他更复杂的算法超越，不过它使用起来简单且方便。然而，由于其简单性，它可以作为一个良好的基准，与其他更复杂算法的性能进行比较。

**[尼莎·阿亚](https://www.linkedin.com/in/nisha-arya-ahmed/)**是一名数据科学家和自由技术写作人员。她特别关注提供数据科学职业建议或教程以及数据科学相关的理论知识。她还希望探索人工智能在促进人类寿命方面的不同方式。作为一个热衷学习者，她希望拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [分类指标讲解：逻辑回归与…](https://www.kdnuggets.com/2022/10/classification-metrics-walkthrough-logistic-regression-accuracy-precision-recall-roc.html)

+   [线性回归与逻辑回归比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12, 3月23日: 最佳数据科学书籍推荐…](https://www.kdnuggets.com/2022/n12.html)

+   [逻辑回归是如何工作的？](https://www.kdnuggets.com/2022/07/logistic-regression-work.html)
