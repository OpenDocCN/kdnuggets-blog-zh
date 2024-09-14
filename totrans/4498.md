# K-最近邻算法介绍及示例

> 原文：[https://www.kdnuggets.com/2020/04/introduction-k-nearest-neighbour-algorithm-using-examples.html](https://www.kdnuggets.com/2020/04/introduction-k-nearest-neighbour-algorithm-using-examples.html)

[评论](#comments)

**由 [Ranvir Singh](https://ranvir.xyz/) 提供，开源爱好者**

![KNN 算法 Python](../Images/9904c91821787cffd5c0365e7a03a07e.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

`KNN` 也称为 K-最近邻，是一种 [监督学习和模式分类算法](https://ranvir.xyz/blog/how-to-evaluate-your-machine-learning-model-like-a-pro-metrics/#supervised-learning-and-classification-problems)，它帮助我们找出新输入（测试值）属于哪个类别，当选择了 `k` 个最近邻并计算它们之间的距离时。

> 它尝试估计在给定 `X` 的情况下 `Y` 的条件分布，并将给定的观察值（测试值）分类到具有最高估计概率的类别中。

它首先识别训练数据中离 `测试值` 最近的 `k` 个点，并计算这些类别之间的距离。测试值将归属于距离最小的类别。![KNN 算法距离](../Images/bab96a13fc6221a835a48218c68dacf3.png)

### KNN 中测试值分类的概率

它使用此函数计算测试值属于类别 `j` 的概率

![图示](../Images/94c277b5f05dafe4a4924438759b7895.png)

### KNN 中计算距离的方法

距离可以通过不同的方法计算，这些方法包括：

+   欧几里得方法

+   曼哈顿方法

+   闵可夫斯基方法

+   等等……

有关可以使用的距离度量的更多信息，请阅读 [这篇关于 KNN 的文章](https://www.saedsayad.com/k_nearest_neighbors.htm)。你可以通过将 `metric` 参数传递给 KNN 对象来使用列表中的任何方法。这里有一个 [Stack Overflow 上的回答](https://stackoverflow.com/questions/21052509/sklearn-knn-usage-with-a-user-defined-metric) 可能对你有帮助。你甚至可以使用一些随机距离度量。如果你想使用自己的距离计算方法，也可以 [阅读这个答案](https://stackoverflow.com/questions/34408027/how-to-allow-sklearn-k-nearest-neighbors-to-take-custom-distance-metric)。

### KNN 的过程及示例

假设我们有一个数据集，其中包含标记明确的狗和马的身高和体重。我们将使用所有条目的体重和身高来创建一个图表。现在，每当有新的条目进来时，我们将选择一个`k`值。为了本示例的方便，假设我们选择`4`作为`k`的值。我们将找到最近的四个值的距离，距离最小的那个将具有更高的概率，并被认为是赢家。![KNN算法示例](../Images/d5ed5cee01cf2aad966a1e23d1318165.png)

### KneighborsClassifier: KNN Python 示例

GitHub 仓库: [KNN GitHub 仓库](https://github.com/singh1114/ml/blob/master/datascience/Machine%20learning/knn/knn.ipynb) 数据源使用: [数据源的GitHub](https://github.com/singh1114/ml/blob/master/datascience/Machine%20learning/knn/KNN_Project_Data) 在K-最近邻算法中，大多数时候你实际上并不知道输入参数或分类类别的含义。在面试的情况下，这样做是为了隐藏真实的客户数据。

```py
 # Import everything
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

# Create a DataFrame
df = pd.read_csv('KNN_Project_Data')

# Print the head of the data.
df.head() 
```

![KNN算法数据框头](../Images/943f8d030780af21e4937ec3bf668a36.png)

数据的头部清楚地表明我们有一些变量和一个包含不同类别的目标类。

### 为什么对KNN进行变量归一化/标准化

如我们所见，数据框中的数据尚未标准化。如果不对数据进行归一化，结果会大相径庭，且可能无法获得正确的结果。这是因为某些特征的偏差很大（值范围从1到1000）。这将导致非常糟糕的图表，并在模型中产生大量缺陷。有关归一化的更多信息，请查看[stack exchange](https://stats.stackexchange.com/a/287439)上的回答。Sklearn提供了一种非常简单的方式来标准化数据。

```py
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(df.drop('TARGET CLASS', axis=1))
sc_transform = scaler.transform(df.drop('TARGET CLASS', axis=1))
sc_df = pd.DataFrame(sc_transform)

# Now you can safely use sc_df as your input features.
sc_df.head() 
```

![KNN算法归一化数据框](../Images/8e89f347796245cfc68040ed56805082.png)

### 使用sklearn进行测试/训练拆分

我们可以简单地[使用sklearn拆分数据](https://ranvir.xyz/blog/how-to-evaluate-your-machine-learning-model-like-a-pro-metrics/#test-train-split-using-sklearn)。

```py
from sklearn.model_selection import train_test_split

X = sc_transform
y = df['TARGET CLASS']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3) 
```

### 使用KNN并找到最佳的k值

选择一个合适的`k`值可能是一项艰巨的任务。我们将使用Python来自动化这个任务。我们能够找到一个合适的`k`值，从而最小化模型中的错误率。

```py
# Initialize an array that stores the error rates.
from sklearn.neighbors import KNeighborsClassifier

error_rates = []

for a in range(1, 40):
    k = a
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train, y_train)
    preds = knn.predict(X_test)
    error_rates.append(np.mean(y_test - preds))

plt.figure(figsize=(10, 7))
plt.plot(range(1,40),error_rates,color='blue', linestyle='dashed', marker='o',
         markerfacecolor='red', markersize=10)
plt.title('Error Rate vs. K Value')
plt.xlabel('K')
plt.ylabel('Error Rate') 
```

![找到最佳k值](../Images/df8207c791896b95d54c50ca55e2e4b5.png) 从图表中可以看到，`k=30`给出了非常理想的错误率值。

```py
k = 30
knn = KNeighborsClassifier(n_neighbors=k)
knn.fit(X_train, y_train)
preds = knn.predict(X_test) 
```

### 评估KNN模型

阅读以下帖子以了解更多关于评估机器学习模型的内容。

```py
from sklearn.metrics import confusion_matrix, classification_report

print(confusion_matrix(y_test, preds))
print(classification_report(y_test, preds)) 
```

![混淆矩阵和分类报告](../Images/c6a18e3d0f4c354f208bcd472999ddcc.png)

### 使用KNN算法的好处

+   KNN算法因其简单易用而广泛用于各种学习任务。

+   在算法中只需提供两个指标：`k`的值和`distance metric`。

+   可以处理任意数量的类别，而不仅仅是二分类器。

+   向算法中添加新数据相对容易。

### KNN 算法的缺点。

+   预测 `k` 最近邻的成本非常高。

+   在处理大量特征/参数时表现不如预期。

+   处理分类特征较为困难。

一本很好的读物，[基于 sklearn 对 Knn 进行基准测试](https://jakevdp.github.io/blog/2013/04/29/benchmarking-nearest-neighbor-searches-in-python/)。

希望你喜欢这篇文章。如有任何问题或疑虑，欢迎在下面的评论中提出。

**简介：[Ranvir Singh](https://ranvir.xyz/)** （[**@ranvirsingh1114**](https://twitter.com/ranvirsingh1114)）是一名来自印度的网页开发者。他是一个编程和 Linux 爱好者，想尽可能多地学习新知识。作为 FOSS 爱好者和开源贡献者，他还曾参与 2017 年 Google Summer of Code 与 AboutCode 组织合作。他还在 2019 年担任了同一组织的 GSoC 导师。

[原始内容](https://ranvir.xyz/blog/k-nearest-neighbor-algorithm-using-sklearn-distance-metric/)。经许可转载。

**相关：**

+   [最新 Scikit-learn 版本中的 5 个新特性](/2019/12/5-features-scikit-learn-release-highlights.html)。

+   [掌握中级机器学习的 7 个步骤 — 2019 版](/2019/06/7-steps-mastering-intermediate-machine-learning-python.html)。

+   [为数据集选择合适的聚类算法](/2019/10/right-clustering-algorithm.html)。

### 相关主题更多。

+   [从理论到实践：构建 k-最近邻分类器](https://www.kdnuggets.com/2023/06/theory-practice-building-knearest-neighbors-classifier.html)。

+   [用于分类的最近邻](https://www.kdnuggets.com/2022/04/nearest-neighbors-classification.html)。

+   [Scikit-learn 中的 K 最近邻](https://www.kdnuggets.com/2022/07/knearest-neighbors-scikitlearn.html)。

+   [SQL LIKE 操作符示例](https://www.kdnuggets.com/2022/09/sql-like-operator-examples.html)。

+   [带示例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)。

+   [挑选示例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)。
