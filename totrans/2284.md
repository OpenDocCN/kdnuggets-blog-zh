# 高斯朴素贝叶斯解释

> 原文：[https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html](https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html)

![高斯朴素贝叶斯解释](../Images/ee88ec80ea1eb5937228a0c928c54194.png)

高斯朴素贝叶斯分类器的决策区域。图像由作者提供。

我认为这在每个数据科学职业生涯的开始阶段都是一个经典：*朴素贝叶斯分类器*。或者我应该更准确地说是*朴素贝叶斯分类器家族*，因为它们有很多不同的变体。例如，有多项式朴素贝叶斯、伯努利朴素贝叶斯，以及高斯朴素贝叶斯分类器，它们在一个小细节上有所不同，正如我们将要发现的那样。朴素贝叶斯算法在设计上相当简单，但在许多复杂的现实世界场景中证明了其有用性。

在这篇文章中，你可以学习到

+   朴素贝叶斯分类器是如何工作的，

+   为什么这样定义它们是有意义的，以及

+   如何使用 NumPy 在 Python 中实现它们。

> 你可以在 [我的 GitHub](https://github.com/Garve/TDS/blob/main/TDS%20-%20Gaussian%20Naive%20Bayes.ipynb) 上找到代码。

查看我的贝叶斯统计入门文章 [贝叶斯推断的温柔介绍](https://towardsdatascience.com/a-gentle-introduction-to-bayesian-inference-6a7552e313cb) 可能会有所帮助，以便熟悉贝叶斯公式。由于我们将以符合 scikit-learn 的方式实现分类器，因此查看我的文章 [构建自定义 scikit-learn 回归器](https://towardsdatascience.com/build-your-own-custom-scikit-learn-regression-5d0d718f289) 也很有价值。不过，scikit-learn 的开销相当小，你仍然应该能够跟上。

我们将开始探索朴素贝叶斯分类的惊人简单理论，然后转向实现部分。

# 理论

我们在分类时真正感兴趣的是什么？我们实际在做什么，输入和输出是什么？答案很简单：

> 给定一个数据点 x，x 属于某个类别 c 的概率是多少？

这就是我们希望通过**任何**分类来回答的所有问题。你可以将这个陈述直接建模为条件概率：*p*(*c*|*x*)。

例如，如果有

+   3 个类别 *c₁*, *c₂*, *c₃*，以及

+   *x* 由 2 个特征 *x₁* 和 *x₂* 组成，

分类器的结果可能是这样的：*p*(*c₁*|*x₁*, *x₂*)=0.3，*p*(*c₂*|*x₁*, *x₂*)=0.5 和 *p*(*c₃*|*x₁*, *x₂*)=0.2。如果我们关心单一标签作为输出，我们会选择概率最高的那个，即这里的 *c₂*，概率为 50%。

> 朴素贝叶斯分类器尝试直接计算这些概率。

## 朴素贝叶斯

好的，给定一个数据点 *x*，我们想要计算所有类别 *c* 的 *p*(*c*|*x*)，然后输出概率最高的 *c*。在公式中你经常看到这样的表达

![高斯朴素贝叶斯解释](../Images/a52355ef1a080e26c5fd8aa72c7256cf.png)

图像由作者提供。

**注意：** max *p*(*c*|*x*) 返回最大概率，而 argmax *p*(*c*|*x*) 返回具有最高概率的 *c*。

但在我们能够优化 *p*(*c*|*x*)之前，我们必须能够计算它。为此，我们使用 [贝叶斯定理](https://en.wikipedia.org/wiki/Bayes%27_theorem)：

![高斯朴素贝叶斯解释](../Images/06709efebc3ddc7d032db27c132ad470.png)

贝叶斯定理。图片由作者提供。

这就是朴素贝叶斯中的贝叶斯部分。但是现在我们面临以下问题：*p*(*x*|*c*)和 *p*(*c*)是什么？

> **这就是朴素贝叶斯分类器训练的全部内容。**

## 训练

为了说明所有内容，以下我们使用一个包含 **两个实际特征** *x₁*、 *x₂*和 **三个类别** *c₁*、 *c₂*、 *c₃*的玩具数据集。

![高斯朴素贝叶斯解释](../Images/480de29d9f4a217537ee3d8085c70468.png)

数据的可视化。图片由作者提供。

你可以通过以下方式创建这个确切的数据集

```py
from sklearn.datasets import make_blobs

X, y = make_blobs(n_samples=20, centers=[(0,0), (5,5), (-5, 5)], random_state=0)
```

让我们从 **类别概率** *p*(*c*)开始，这是观察到某个类别 *c* 在标记数据集中的概率。估计这个概率的最简单方法是计算类别的相对频率并将其作为概率。我们可以用我们的数据集来看这到底意味着什么。

数据集中有20个点中有7个标记为类别 *c₁*（蓝色），因此我们说 *p*(*c₁*)=7/20。类别 *c₂*（红色）也有7个点，因此我们设定 *p*(*c₂*)=7/20。最后一个类别 *c₃*（黄色）只有6个点，因此 *p*(*c₃*)=6/20。

这种简单的类别概率计算类似于最大似然方法。然而，如果你愿意，也可以使用其他 *先验* 分布。例如，如果你知道这个数据集不代表真实的总体，因为类别 *c₃* 应该在50%的情况下出现，那么你可以设定 *p*(*c₁*)=0.25、 *p*(*c₂*)=0.25 和 *p*(*c₃*)=0.5。任何有助于提高测试集性能的方法都是可行的。

现在我们转向 **似然** *p*(*x*|*c*)=*p*(*x₁*, *x₂*|*c*)。计算这种似然的一种方法是过滤数据集中标签为 *c* 的样本，然后尝试找到一个能够捕捉特征 *x₁*、 *x₂*的分布（例如一个二维高斯分布）。

> 不幸的是，通常我们每个类别的样本不足以进行适当的似然估计。

为了建立一个更强健的模型，我们做出 **朴素假设**，即在给定 *c* 的情况下，特征 *x₁*、 *x₂* 是*随机独立*的。这只是通过贝叶斯定理使数学更简单的一种花哨方式。

![高斯朴素贝叶斯解释](../Images/9fb96ba70552e0566460c7be1094fbfb.png)

图片由作者提供

**对于每个类别 *c***。这就是朴素贝叶斯中 **朴素** 部分的来源，因为这个方程在一般情况下是不成立的。不过，即便如此，朴素贝叶斯在实践中仍能产生良好，有时甚至是出色的结果。特别是对于具有词袋特征的自然语言处理问题，多项式朴素贝叶斯表现尤为出色。

上面给出的参数对于任何朴素贝叶斯分类器都是相同的。现在只是取决于你如何建模*p(x₁|c₁), p(x₂|c₁), p(x₁|c₂), p(x₂|c₂), p(x₁|c₃)*和*p(x₂|c₃)*。

如果你的特征只有0和1，你可以使用一个[Bernoulli分布](https://en.wikipedia.org/wiki/Bernoulli_distribution)。如果它们是整数，则使用[多项分布](https://en.wikipedia.org/wiki/Multinomial_distribution)。然而，我们有实际的特征值，并决定使用**高斯**分布，因此称为高斯朴素贝叶斯。我们假设以下形式

![高斯朴素贝叶斯，解释](../Images/43e5b16b9a6b3eee4aba7934c23ede00.png)

图片由作者提供。

其中*μᵢ,ⱼ*是均值，*σᵢ,ⱼ*是标准差，我们需要从数据中估计。这意味着我们为每个特征*i*和一个类*cⱼ*获得一个均值，在我们的例子中是2*3=6个均值。标准差也是如此。**这需要一个例子。**

让我们尝试估计*μ₂,₁*和*σ₂,₁*。因为*j*=1，我们只对类*c₁*感兴趣，所以只保留此标签的样本。剩下的样本如下：

```py
# samples with label = c_1

array([[ 0.14404357,  1.45427351],

[ 0.97873798,  2.2408932 ],

[ 1.86755799, -0.97727788],

[ 1.76405235,  0.40015721],

[ 0.76103773,  0.12167502],

[-0.10321885,  0.4105985 ],

[ 0.95008842, -0.15135721]])
```

现在，由于*i*=2，我们只需考虑第二列。*μ₂*,₁是这一列的均值，*σ₂*,₁是这一列的标准差，即*μ₂*,₁ = 0.49985176和*σ₂*,₁ = 0.9789976。

如果你再次查看上面的散点图，这些数字是合理的。类*c₁*的样本的特征*x₂*大约在0.5左右，从图片中可以看出。

我们现在计算其他五种组合，就完成了！

在Python中，你可以这样做：

```py
from sklearn.datasets import make_blobs
import numpy as np

# Create the data. The classes are c_1=0, c_2=1 and c_3=2.
X, y = make_blobs(
    n_samples=20, centers=[(0, 0), (5, 5), (-5, 5)], random_state=0
)

# The class probabilities.
# np.bincounts counts the occurence of each label.
prior = np.bincount(y) / len(y)

# np.where(y==i) returns all indices where the y==i.
# This is the filtering step.
means = np.array([X[np.where(y == i)].mean(axis=0) for i in range(3)])
stds = np.array([X[np.where(y == i)].std(axis=0) for i in range(3)]) 
```

我们接收到

```py
# priors
array([0.35, 0.35, 0.3 ])
# means 
array([[ 0.90889988,  0.49985176],
       [ 5.4111385 ,  4.6491892 ],
       [-4.7841679 ,  5.15385848]])
# stds
array([[0.6853714 , 0.9789976 ],
       [1.40218915, 0.67078568],
       [0.88192625, 1.12879666]])
```

这是高斯朴素贝叶斯分类器训练的结果。

## 进行预测

完整的预测公式是

![高斯朴素贝叶斯，解释](../Images/4c2d5e28f9547e0dd95f9cb5c9a048eb.png)

图片由作者提供。

假设一个新的数据点*x*=*(-2, 5)进入。

![高斯朴素贝叶斯，解释](../Images/90959460a8c559b07e93067527e10527.png)

图片由作者提供。

为了确定它属于哪个类，让我们计算*p*(*c*|*x*)对于所有类。从图片来看，它应该属于类*c₃* = 2，但我们来看一下。让我们暂时忽略分母*p*(*x*)。使用以下循环计算*j* = 1, 2, 3的分子。

```py
x_new = np.array([-2, 5])

for j in range(3):
    print(
        f"Probability for class {j}: {(1/np.sqrt(2*np.pi*stds[j]**2)*np.exp(-0.5*((x_new-means[j])/stds[j])**2)).prod()*p[j]:.12f}"
    ) 
```

我们接收到

```py
Probability for class 0: 0.000000000263
Probability for class 1: 0.000000044359
Probability for class 2: 0.000325643718
```

当然，这些*概率*（我们不应该这样称呼）不加起来是1，因为我们忽略了分母。然而，这没有问题，因为我们可以将这些未标准化的概率除以它们的总和，这样它们的总和将加起来是1。因此，将这三个值除以约0.00032569的总和，我们得到

![高斯朴素贝叶斯，解释](../Images/e0a6e1d05ebe55271551267e08ace5e1.png)

图片由作者提供。

如我们所预期的那样，结果明显。现在，让我们实现它！

# 完整实现

这个实现目前还远远不够高效，也不够数值稳定，它只是用于教育目的。我们已经讨论了大部分内容，因此现在应该很容易跟进。你可以忽略所有的 `check` 函数，或者阅读我的文章 [构建你自己的自定义 scikit-learn](https://towardsdatascience.com/build-your-own-custom-scikit-learn-regression-5d0d718f289) 如果你对它们的具体作用感兴趣。

请注意，我首先实现了一个 `predict_proba` 方法来计算概率。`predict` 方法只是调用这个方法，并使用 argmax 函数返回具有最高概率的索引（类别）。类别从 0 到 *k*-1，其中 *k* 是类别的数量。

```py
import numpy as np
from sklearn.base import BaseEstimator, ClassifierMixin
from sklearn.utils.validation import check_X_y, check_array, check_is_fitted

class GaussianNaiveBayesClassifier(BaseEstimator, ClassifierMixin):
    def fit(self, X, y):
        X, y = check_X_y(X, y)
        self.priors_ = np.bincount(y) / len(y)
        self.n_classes_ = np.max(y) + 1

        self.means_ = np.array(
            [X[np.where(y == i)].mean(axis=0) for i in range(self.n_classes_)]
        )
        self.stds_ = np.array(
            [X[np.where(y == i)].std(axis=0) for i in range(self.n_classes_)]
        )

        return self

    def predict_proba(self, X):
        check_is_fitted(self)
        X = check_array(X)

        res = []
        for i in range(len(X)):
            probas = []
            for j in range(self.n_classes_):
                probas.append(
                    (
                        1
                        / np.sqrt(2 * np.pi * self.stds_[j] ** 2)
                        * np.exp(-0.5 * ((X[i] - self.means_[j]) / self.stds_[j]) ** 2)
                    ).prod()
                    * self.priors_[j]
                )
            probas = np.array(probas)
            res.append(probas / probas.sum())
        return np.array(res)

    def predict(self, X):
        check_is_fitted(self)
        X = check_array(X)

        res = self.predict_proba(X)

        return res.argmax(axis=1)
```

## 测试实现

尽管代码非常简短，但仍然较长，无法完全确保没有任何错误。所以，让我们检查一下它与 [scikit-learn 高斯NB分类器](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html) 的比较。

```py
my_gauss = GaussianNaiveBayesClassifier()
my_gauss.fit(X, y)
my_gauss.predict_proba([[-2, 5], [0,0], [6, -0.3]])
```

输出

```py
array([[8.06313823e-07, 1.36201957e-04, 9.99862992e-01],
       [1.00000000e+00, 4.23258691e-14, 1.92051255e-11],
       [4.30879705e-01, 5.69120295e-01, 9.66618838e-27]])
```

使用 `predict` 方法的预测结果是

```py
# my_gauss.predict([[-2, 5], [0,0], [6, -0.3]])
array([2, 0, 1])
```

现在，让我们使用 scikit-learn。插入一些代码

```py
from sklearn.naive_bayes import GaussianNB

gnb = GaussianNB()
gnb.fit(X, y)
gnb.predict_proba([[-2, 5], [0,0], [6, -0.3]])
```

结果

```py
array([[8.06314158e-07, 1.36201959e-04, 9.99862992e-01],
       [1.00000000e+00, 4.23259111e-14, 1.92051343e-11],
       [4.30879698e-01, 5.69120302e-01, 9.66619630e-27]])
```

数字看起来与我们的分类器的结果有点类似，但在最后几个显示的数字上有一些偏差。我们做错了什么吗？ **没有。** scikit-learn 版本只是使用了另一个超参数 `var_smoothing=1e-09`。如果我们将其设置为 **零**，我们就能得到完全相同的结果。完美！

看一下我们分类器的决策区域。我还标记了我们用于测试的三个点。你可以从 `predict_proba` 的输出中看到，那些接近边界的点只有 56.9% 的概率属于红色类别。其他两个点的分类信心要高得多。

![高斯朴素贝叶斯，解释](../Images/8aecf2b50a46459e9453ce1b0dcc4844.png)

带有三个新点的决策区域。图片来源：作者。

# 结论

在这篇文章中，我们了解了高斯朴素贝叶斯分类器的工作原理，并对其设计方式进行了直观的解释——这是一种直接建模感兴趣概率的方法。与逻辑回归相比，逻辑回归中概率是通过线性函数建模，并在其上应用了一个 sigmoid 函数。这仍然是一个简单的模型，但不像朴素贝叶斯分类器那样自然。

我们继续计算了几个例子，并在过程中收集了一些有用的代码片段。最后，我们实现了一个完整的高斯朴素贝叶斯分类器，并使其能够与 scikit-learn 良好配合。这意味着你可以在管道或网格搜索中使用它。

最后，我们通过导入 scikit-learn 自身的高斯朴素贝叶斯分类器，并测试我们的分类器和 scikit-learn 的分类器是否产生相同的结果，进行了一个小的正确性检查。这个测试是成功的。

**[罗伯特·库布勒博士](https://www.linkedin.com/in/robert-kuebler/)** 是 METRO.digital 的数据科学家以及 Towards Data Science 的作者。

[原文](https://towardsdatascience.com/learning-by-implementing-gaussian-naive-bayes-3f0e3d2c01b2)。经许可转载。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 更多相关话题

+   [朴素贝叶斯算法：你需要了解的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [KDnuggets 新闻，4月13日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [理解贝叶斯定理的三种方法将提升你的数据科学能力](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [数据库关键术语解释](https://www.kdnuggets.com/2016/07/database-key-terms-explained.html)

+   [描述统计学关键术语解释](https://www.kdnuggets.com/2017/05/descriptive-statistics-key-terms-explained.html)

+   [决策树算法解释](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)
