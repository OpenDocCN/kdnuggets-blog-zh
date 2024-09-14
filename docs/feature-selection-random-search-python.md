# Python中的随机搜索特征选择

> 原文：[https://www.kdnuggets.com/2019/08/feature-selection-random-search-python.html](https://www.kdnuggets.com/2019/08/feature-selection-random-search-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[吉安卢卡·马拉托](http://www.gianlucamalato.it/)，数据科学家、小说作者和软件开发者。**

![](../Images/2fa60c200e38bb2510ca3c1de6181396.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

**特征选择**一直是机器学习中的一项重要任务。根据我的经验，我可以肯定地说，特征选择**比模型选择更为重要**。

### 特征选择与多重共线性

我已经写了一篇关于特征选择的[文章](https://medium.com/data-science-journal/how-to-measure-feature-importance-in-a-binary-classification-model-d284b8c9a301)。这是一种**无监督**的方式来测量**二分类**模型中特征的重要性，使用了皮尔逊卡方检验和相关系数。

一般来说，无监督的方法通常足以用于简单的特征选择。然而，每个模型都有**自己独特的方式**来“思考”特征以及处理特征与目标变量的相关性。此外，有些模型不太关心**多重共线性**（即特征之间的相关性），而有些模型在发生这种情况时会显示出**非常大的问题**（例如线性模型）。

尽管可以通过模型引入的一些**相关性**指标来**排序**特征（例如，对线性回归系数执行的t检验的p值），但仅仅选择最相关的变量可能还不够。想象一下一个特征，它等于另一个特征，只是乘以二。这些特征之间的线性相关性为1，这种简单的乘法不会影响与目标变量的相关性，因此如果我们只选择最相关的变量，我们将得到原始特征和被乘以二的特征。这会导致**多重共线性**，这对我们的模型可能非常危险。

这就是为什么我们必须引入某种方法来更好地选择特征。

### 随机搜索

随机搜索是数据科学家工具箱中的一个非常有用的工具。这是一种非常简单的技术，通常用于交叉验证和**超参数优化**等场景。

这非常简单。如果你有一个多维网格并且想在这个网格上寻找一个最大化（或最小化）某个**目标函数**的点，随机搜索的工作原理如下：

1.  在网格上取一个随机点并测量目标函数值。

1.  如果该值优于迄今为止取得的最佳值，则将该点保存在内存中。

1.  重复一个预定义的次数。

就这样。只需生成随机点并寻找最佳点。

这是一种**找到全局最小值（或最大值）**的**好方法**吗？当然不是。我们所寻找的点只有一个（如果我们运气好的话），在一个非常大的空间中，而且我们只有有限的迭代次数。在一个*N*点网格中获取该单个点的概率是*1/N*。

那么，为什么随机搜索如此常用？因为我们**并不真正想要**最大化我们的性能指标；我们想要一个好的、**合理高的值**，它不是最高的值，以避免过拟合。

这就是为什么随机搜索有效，并且可以用于特征选择的原因。

### 如何使用随机搜索进行特征选择

随机搜索可以用于特征选择，效果**相当好**。类似于随机搜索的一个例子是**随机森林**模型，它对每棵树进行特征的随机选择。

这个想法很简单：**随机**选择特征，通过**k折交叉验证**来测量模型性能，并重复多次。给出最佳性能的特征组合就是我们所寻找的。

更准确地说，以下是需要遵循的步骤：

1.  生成一个随机整数*N*，范围在1到特征数量之间。

1.  生成一个随机的*N*整数序列，范围在0到*N-1*之间，且不重复。这个序列表示我们的特征数组。请记住，Python数组是从0开始的。

1.  在这些特征上训练模型，并使用k折交叉验证进行交叉验证，保存**某些性能指标的平均值**。

1.  从第1点开始重复，次数可以随意。

1.  最后，获取根据所选择的性能指标提供最佳性能的特征数组。

### Python中的一个实际示例

对于这个示例，我将使用**乳腺癌数据集**，它包含在**sklearn**模块中。我们的模型将是**逻辑回归**，并且我们将使用**准确率**作为性能指标进行5折交叉验证。

首先，我们必须导入必要的模块。

```py
import sklearn.datasets
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score
import numpy as np

```

然后我们可以导入乳腺癌数据并将其拆分为输入和目标。

```py
dataset= sklearn.datasets.load_breast_cancer()
data = dataset.data
target = dataset.target

```

现在，我们可以创建一个逻辑回归对象。

```py
lr = LogisticRegression()

```

然后，我们可以在k折交叉验证中测量所有特征的平均准确率。

```py
# Model accuracy using all the features
np.mean(cross_val_score(lr,data,target,cv=5,scoring="accuracy"))
# 0.9509041939207385

```

这是95%。我们要记住这一点。

现在，我们可以实现一个例如300次迭代的随机搜索。

```py
result = []

# Number of iterations
N_search = 300

# Random seed initialization
np.random.seed(1)

for i in range(N_search):
    # Generate a random number of features
    N_columns =  list(np.random.choice(range(data.shape[1]),1)+1)

    # Given the number of features, generate features without replacement
    columns = list(np.random.choice(range(data.shape[1]), N_columns, replace=False))

    # Perform k-fold cross validation
    scores = cross_val_score(lr,data[:,columns], target, cv=5, scoring="accuracy")

    # Store the result
    result.append({'columns':columns,'performance':np.mean(scores)})

# Sort the result array in descending order for performance measure
result.sort(key=lambda x : -x['performance'])

```

在循环和排序函数结束时，*result*列表的第一个元素就是我们所寻找的对象。

我们可以使用这个值来计算该特征子集的新性能指标。

```py
np.mean(cross_val_score(lr, data[:,result[0][‘columns’]], target, cv=5, scoring=”accuracy”))
# 0.9526741054251634

```

如你所见，准确率有所提高。

### 结论

随机搜索可以成为执行特征选择的强大工具。它并不是用来解释某些特征为何比其他特征更有用（与递归特征消除等其他特征选择程序相对），但它可以作为一个有用的工具，以更少的时间获得**良好的结果**。

[原文](https://towardsdatascience.com/feature-selection-by-random-search-in-python-730ffd2912e9)。经许可转载。

**相关：**

+   [特征提取的搭车指南](https://www.kdnuggets.com/2019/06/hitchhikers-guide-feature-extraction.html)

+   [前向特征选择：Python 中的实际示例](https://www.kdnuggets.com/2018/06/step-forward-feature-selection-python.html)

+   [多目标优化特征选择](https://www.kdnuggets.com/2017/12/rapidminer-multi-objective-optimization-feature-selection.html)

### 相关主题

+   [使用网格搜索和随机搜索进行超参数调优的 Python 实践](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [特征选择：科学与艺术的交汇](https://www.kdnuggets.com/2021/12/feature-selection-science-meets-art.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [通过 Uplimit 的机器学习课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)
