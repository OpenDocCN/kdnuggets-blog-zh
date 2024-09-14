# 使用 NumPy 从头开始线性回归

> 原文：[https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy](https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy)

![使用 NumPy 从头开始线性回归](../Images/1951ea4b9d9ce604ed154efff3629c4f.png)

作者提供的图像

# 动机

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的信息技术。

* * *

线性回归是机器学习中最基础的工具之一。它用于找到一条适合我们数据的直线。尽管它仅适用于简单的直线模式，但理解其背后的数学帮助我们理解梯度下降和损失最小化方法。这些对于所有机器学习和深度学习任务中的更复杂模型都是重要的。

在本文中，我们将动手使用 NumPy 从头开始构建线性回归。我们将从基础开始，而不是使用 Scikit-Learn 提供的抽象实现。

# 数据集

我们使用 Scikit-Learn 方法生成一个虚拟数据集。目前我们只使用一个变量，但该实现将是通用的，可以在任意数量的特征上进行训练。

Scikit-Learn 提供的 make_regression 方法生成随机的线性回归数据集，并加入了高斯噪声以增加一些随机性。

```py
X, y = datasets.make_regression(
        n_samples=500, n_features=1, noise=15, random_state=4)
```

我们生成 500 个随机值，每个值具有 1 个特征。因此，X 的形状是 (500, 1)，每个 500 个独立的 X 值都有一个对应的 y 值。因此，y 的形状也是 (500, )。

可视化的数据集如下所示：

![使用 NumPy 从头开始线性回归](../Images/a1f435f09880ff61db42c639fa6d2b15.png)

作者提供的图像

**我们的目标是找到一条最适合的直线，使其通过数据中心，最小化预测值与原始 y 值之间的平均差异。**

# 直观

线性直线的一般方程是：

y = m*X + b

X 是数值型的，单一值。这里 m 和 b 代表梯度和 y 截距（或偏置）。这些是未知的，改变这些值可以生成不同的直线。在机器学习中，X 依赖于数据，y 值也是如此。**我们只控制 m 和 b，它们作为我们的模型参数。** 我们的目标是找到这两个参数的最佳值，以生成一条最小化预测值与实际 y 值之间差异的直线。

这扩展到 X 是多维的情况。在这种情况下，m 值的数量将等于数据中的维度数量。例如，如果我们的数据有三个不同的特征，我们将有三个不同的 m 值，称为**权重**。

方程现在变为：

y = w1*X1 + w2*X2 + w3*X3 + b

这可以扩展到任意数量的特征。

但我们如何知道偏置和权重的最优值？嗯，我们不知道。但是我们可以使用梯度下降方法逐步找出。我们从随机值开始，并在多个步骤中稍微调整它们，直到接近最优值。

首先，让我们初始化线性回归，稍后我们会更详细地讲解优化过程。

# 初始化线性回归类

```py
import numpy as np

class LinearRegression:
    def __init__(self, lr: int = 0.01, n_iters: int = 1000) -> None:
        self.lr = lr
        self.n_iters = n_iters
        self.weights = None
        self.bias = None
```

我们使用学习率和迭代次数这两个超参数，稍后会详细解释。权重和偏置被设置为 None，因为权重参数的数量取决于数据中的输入特征。由于我们还没有数据，因此目前将它们初始化为 None。

# 拟合方法

在 fit 方法中，我们提供了数据及其相关值。现在我们可以利用这些数据来初始化权重，然后训练模型以找到最优权重。

```py
def fit(self, X, y):
        num_samples, num_features = X.shape     # X shape [N, f]
        self.weights = np.random.rand(num_features)  # W shape [f, 1]
        self.bias = 0
```

自变量 X 将是一个形状为 (num_samples, num_features) 的 NumPy 数组。在我们的例子中，X 的形状是 (500, 1)。我们数据中的每一行都有一个相关的目标值，因此 y 的形状也是 (500,) 或 (num_samples)。

我们提取这些，并根据输入特征的数量随机初始化权重。因此，现在我们的权重也是一个大小为 (num_features, ) 的 NumPy 数组。偏置是初始化为零的单个值。

# 预测 Y 值

我们使用上述讨论的直线方程来计算预测的 y 值。然而，我们可以采用向量化的方法进行更快的计算，而不是通过迭代方法来求和所有值。由于权重和 X 值都是 NumPy 数组，我们可以使用矩阵乘法来获得预测值。

X 的形状是 (num_samples, num_features)，而权重的形状是 (num_features, )。我们希望预测值的形状是 (num_samples, )，以匹配原始 y 值。因此，我们可以将 X 与权重相乘，即 (num_samples, num_features) x (num_features, ) 来获得形状为 (num_samples, ) 的预测值。

偏置值在每个预测的末尾添加。这可以简单地在一行中实现。

```py
# y_pred shape should be N, 1
y_pred = np.dot(X, self.weights) + self.bias
```

然而，这些预测正确吗？显然不是。我们使用随机初始化的权重和偏置值，因此预测也会是随机的。

我们如何获得最优值？**梯度下降**。

# 损失函数和梯度下降

现在我们有了预测的 y 值和目标 y 值，我们可以找到这两者之间的差异。均方误差（MSE）用于比较实际值。方程如下：

![从零开始的线性回归与 NumPy](../Images/d5f03ab0e7f212765a705a6189468d4d.png)

我们只关心值之间的绝对差异。预测值高于原始值和低于原始值同样糟糕。因此，我们对目标值和预测值之间的差异进行平方处理，以将负差异转换为正值。此外，这也惩罚了目标和预测之间的较大差异，因为较大的差异平方后会对最终损失贡献更多。

为了使我们的预测尽可能接近原始目标，我们现在尝试最小化这个函数。损失函数在梯度为零时达到最小值。由于我们只能优化权重和偏置值，我们对 MSE 函数关于权重和偏置值的偏导数进行计算。

![使用 NumPy 从头开始进行线性回归](../Images/83e89da79a360a5709bf54902e4a76cb.png)

然后，我们使用梯度下降法根据梯度值优化我们的权重。

![使用 NumPy 从头开始进行线性回归](../Images/19a05045d5c5faa8874379aad3328eda.png)

图片来自 [Sebasitan Raschka](https://sebastianraschka.com/faq/docs/gradient-optimization.html)

我们对每个权重值取梯度，然后将其移动到梯度的相反方向。这将使损失值趋向最小值。根据图片，梯度为正，因此我们减少权重。这将使 J(W) 或损失值趋向最小值。因此，优化方程如下：

![使用 NumPy 从头开始进行线性回归](../Images/87a11aefee51baeef82929e742c74a05.png)

学习率（或 alpha）控制图片中显示的增量步长。我们只对值进行小幅度的调整，以稳定地向最小值移动。

## 实现

如果我们使用基本的代数运算简化导数方程，这将变得非常简单。

![使用 NumPy 从头开始进行线性回归](../Images/d05edcda87c786e8fe178a475a1d8375.png)

对于导数，我们使用两行代码来实现：

```py
# X -> [ N, f ]
# y_pred -> [ N ]
# dw -> [ f ]
dw = (1 / num_samples) * np.dot(X.T, y_pred - y)
db = (1 / num_samples) * np.sum(y_pred - y)
```

dw 的形状仍为 (num_features, )，因此我们为每个权重有一个单独的导数值。我们分别优化这些权重。db 只有一个值。

现在，为了优化这些值，我们使用基本的减法将值移动到梯度的相反方向。

```py
self.weights = self.weights - self.lr * dw
self.bias = self.bias - self.lr * db
```

同样，这只是一个单一的步骤。我们仅对随机初始化的值进行小幅度调整。现在我们重复相同的步骤，以趋近于最小值。

完整的循环如下：

```py
for i in range(self.n_iters):

            # y_pred shape should be N, 1
            y_pred = np.dot(X, self.weights) + self.bias

            # X -> [N,f]
            # y_pred -> [N]
            # dw -> [f]
            dw = (1 / num_samples) * np.dot(X.T, y_pred - y)
            db = (1 / num_samples) * np.sum(y_pred - y)

            self.weights = self.weights - self.lr * dw
            self.bias = self.bias - self.lr * db
```

# 预测

我们以与训练时相同的方式进行预测。然而，现在我们拥有了最佳的权重和偏置集合。预测值现在应该接近原始值。

```py
def predict(self, X):
        return np.dot(X, self.weights) + self.bias
```

# 结果

使用随机初始化的权重和偏置，我们的预测结果如下：

![使用 NumPy 从头开始进行线性回归](../Images/30983147e20a744358c6845a763a3492.png)

作者提供的图片

权重和偏差初始化非常接近 0，因此我们得到了一条水平线。经过 1000 次迭代训练后，我们得到了这个：![从零开始的线性回归与 NumPy](../Images/ff5eda8a6ccf432c7b79a8a6d371f38d.png)

图片来源：作者

预测的线正好穿过我们数据的中心，似乎是可能的最佳拟合线。

# 结论

你现在已经从零开始实现了线性回归。完整代码也可以在[GitHub](https://github.com/MuhammadArham-43/ML_Algos)上找到。

```py
import numpy as np

class LinearRegression:
    def __init__(self, lr: int = 0.01, n_iters: int = 1000) -> None:
        self.lr = lr
        self.n_iters = n_iters
        self.weights = None
        self.bias = None

    def fit(self, X, y):
        num_samples, num_features = X.shape     # X shape [N, f]
        self.weights = np.random.rand(num_features)  # W shape [f, 1]
        self.bias = 0

        for i in range(self.n_iters):

            # y_pred shape should be N, 1
            y_pred = np.dot(X, self.weights) + self.bias

            # X -> [N,f]
            # y_pred -> [N]
            # dw -> [f]
            dw = (1 / num_samples) * np.dot(X.T, y_pred - y)
            db = (1 / num_samples) * np.sum(y_pred - y)

            self.weights = self.weights - self.lr * dw
            self.bias = self.bias - self.lr * db

        return self

    def predict(self, X):
        return np.dot(X, self.weights) + self.bias
```

**[穆罕默德·阿赫曼](https://www.linkedin.com/in/muhammad-arham-a5b1b1237/)** 是一名深度学习工程师，专注于计算机视觉和自然语言处理。他曾在 Vyro.AI 上工作，负责多个生成型 AI 应用的部署和优化，这些应用在全球排行榜上名列前茅。他对构建和优化智能系统的机器学习模型充满兴趣，并相信持续改进。

### 更多相关话题

+   [线性回归与逻辑回归比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [使用线性回归模型而非…的 3 个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12, 3 月 23 日：最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [数据科学中的线性回归](https://www.kdnuggets.com/2022/07/linear-regression-data-science.html)

+   [预测指南：Python 中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)
