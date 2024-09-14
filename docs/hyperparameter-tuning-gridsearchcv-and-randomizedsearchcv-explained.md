# 超参数调优：GridSearchCV 和 RandomizedSearchCV 的解释

> 原文：[`www.kdnuggets.com/hyperparameter-tuning-gridsearchcv-and-randomizedsearchcv-explained`](https://www.kdnuggets.com/hyperparameter-tuning-gridsearchcv-and-randomizedsearchcv-explained)

![超参数调优：GridSearchCV 和 RandomizedSearchCV 的解释](img/97aa1d8c8affaf9e44ae26a81fe8b3ff.png)

作者提供的图像

每个你训练的机器学习模型都有一组参数或模型系数。机器学习算法的目标——作为一个优化问题——是学习这些参数的最佳值。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

此外，机器学习模型还有一组超参数。例如，K-最近邻算法中的 K 值，即邻居的数量，或者在训练深度神经网络时的批次大小等等。

这些超参数不是由模型学习的，而是由开发者指定的。它们影响模型性能，并且是可调节的。那么你如何找到这些超参数的最佳值呢？这个过程称为**超参数优化**或**超参数调优**。

两种最常见的超参数调优技术包括：

+   网格搜索

+   随机搜索

在本指南中，我们将学习这些技术的工作原理及其在 scikit-learn 中的实现。

# 训练基线 SVM 分类器

让我们开始在酒类数据集上训练一个简单的 支持向量机 (SVM) 分类器。

首先，导入所需的模块和类：

```py
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score
```

酒类数据集是 scikit-learn 中内置数据集的一部分。所以让我们按如下方式读取特征和目标标签：

```py
# Load the Wine dataset
wine = datasets.load_wine()
X = wine.data
y = wine.target
```

酒类数据集是一个简单的数据集，包含 13 个数值特征和三个输出类别标签。这是一个很好的数据集，用于了解多分类问题。你可以运行`wine.DESCR`来获取数据集的描述。

![超参数调优：GridSearchCV 和 RandomizedSearchCV 的解释](img/b96d32e12e7d2cdc4fdf54c6d1bbfe48.png)

wine.DESCR 的输出

接下来，将数据集分成训练集和测试集。在这里，我们使用了`test_size`为 0.2。所以 80%的数据进入训练数据集，20%进入测试数据集。

```py
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=24)
```

现在实例化一个支持向量分类器并将模型拟合到训练数据集。然后评估其在测试集上的性能。

```py
# Create a baseline SVM classifier
baseline_svm = SVC()
baseline_svm.fit(X_train, y_train)
y_pred = baseline_svm.predict(X_test)
```

因为这是一个简单的多分类问题，我们可以查看模型的准确率。

```py
# Evaluate the baseline model
accuracy = accuracy_score(y_test, y_pred)
print(f"Baseline SVM Accuracy: {accuracy:.2f}")
```

我们看到这个模型在默认超参数值下的准确率约为 0.78。

```py
Output >>>
Baseline SVM Accuracy: 0.78
```

这里我们使用了 `random_state` 为 24。对于不同的随机状态，你将得到不同的训练测试拆分，从而得到不同的准确率分数。

所以我们需要比单一训练-测试拆分更好的方法来评估模型性能。也许，可以在许多这样的拆分上训练模型，并考虑平均准确率，同时尝试不同的超参数组合？是的，这就是我们在模型评估和超参数搜索中使用交叉验证的原因。我们将在接下来的部分中了解更多。

接下来让我们确定可以为这个支持向量机分类器调整的超参数。

# SVM 超参数调整

在超参数调优中，我们的目标是找到 SVM 分类器的最佳超参数值组合。支持向量分类器常调的超参数包括：

+   **C**：正则化参数，控制最大化边界和最小化分类错误之间的权衡。

+   **kernel**：指定要使用的核函数类型（例如 'linear'，'rbf'，'poly'）。

+   **gamma**：'rbf' 和 'poly' 核的核系数。

# 理解交叉验证的作用

**交叉验证** 帮助评估模型 *对未见数据的泛化能力*，并减少对单一训练-测试拆分的过拟合风险。常用的 k 折交叉验证涉及将数据集拆分为 **k** 个相等大小的折叠。模型被训练 **k** 次，每个折叠 *一次* 作为验证集，其余折叠作为训练集。因此，对于每个折叠，我们将获得一个交叉验证准确率。

当我们运行网格和随机搜索以找到最佳超参数时，我们将基于最佳的平均交叉验证得分选择超参数。

# 什么是网格搜索？

**网格搜索** 是一种超参数调优技术，通过 **对指定的超参数空间进行全面搜索** 来找到产生最佳模型性能的超参数组合。

## 网格搜索如何工作

我们将超参数搜索空间定义为参数网格。**参数网格** 是一个字典，在其中你指定每个要调整的超参数及其要探索的值列表。

网格搜索系统地探索超参数网格中的每一个可能组合。它使用交叉验证对每个组合进行模型拟合和评估，并选择产生最佳性能的组合。

接下来，让我们在 scikit-learn 中实现网格搜索。

# Scikit-Learn 中的 GridSearchCV

首先，从 scikit-learn 的 [model_selection](https://scikit-learn.org/stable/model_selection.html) 模块中导入 `GridSearchCV` 类：

```py
from sklearn.model_selection import GridSearchCV
```

让我们定义 SVM 分类器的参数网格：

```py
# Define the hyperparameter grid
param_grid = {
    'C': [0.1, 1, 10],
    'kernel': ['linear', 'rbf', 'poly'],
    'gamma': [0.1, 1, 'scale', 'auto']
}
```

网格搜索则系统地探索参数网格中的每一个可能组合。在这个例子中，它使用以下参数评估模型的性能：

+   `C` 设置为 0.1, 1 和 10，

+   `kernel` 设置为 'linear', 'rbf' 和 'poly'，并且

+   `gamma` 设置为 0.1, 1, 'scale' 和 'auto'。

这将导致总共 36 种不同的组合需要评估。网格搜索对每个组合进行拟合和评估，并使用交叉验证选择出最佳表现的组合。

然后我们实例化`GridSearchCV`来调整`baseline_svm`的超参数：

```py
# Create the GridSearchCV object
grid_search = GridSearchCV(estimator=baseline_svm, param_grid=param_grid, cv=5)

# Fit the model with the grid of hyperparameters
grid_search.fit(X_train, y_train)
```

注意，我们使用了 5 折交叉验证。

最后，我们在测试数据上评估通过网格搜索找到的最佳模型——具有最优超参数的模型的性能：

```py
# Get the best hyperparameters and model
best_params = grid_search.best_params_
best_model = grid_search.best_estimator_

# Evaluate the best model
y_pred_best = best_model.predict(X_test)
accuracy_best = accuracy_score(y_test, y_pred_best)
print(f"Best SVM Accuracy: {accuracy_best:.2f}")
print(f"Best Hyperparameters: {best_params}")
```

如所见，模型在以下超参数下达到了 0.94 的准确率：

```py
Output >>>
Best SVM Accuracy: 0.94
Best Hyperparameters: {'C': 0.1, 'gamma': 0.1, 'kernel': 'poly'}
```

# 网格搜索的优缺点

使用网格搜索进行超参数调整具有以下优点：

+   网格搜索探索所有指定的组合，确保你不会错过定义的搜索空间中的最佳超参数。

+   它适合于探索较小的超参数空间。

然而，另一方面：

+   网格搜索可能计算开销较大，尤其是在处理大量超参数及其值时。对于非常复杂的模型或广泛的超参数搜索，可能不切实际。

现在让我们了解一下随机搜索。

# 什么是随机搜索？

**随机搜索**是另一种超参数调整技术，它**探索指定分布或范围内的随机超参数组合**。当处理较大的超参数搜索空间时，它特别有用。

## 随机搜索如何工作

在随机搜索中，你可以定义每个超参数的概率分布或范围，而不是指定一个值的网格。这会变成一个更大的超参数搜索空间。

随机搜索然后*随机抽样*从这些分布中选取固定数量的超参数组合。这使得随机搜索能够高效地探索多样的超参数组合。

# Scikit-Learn 中的 RandomizedSearchCV

现在让我们使用随机搜索来调整基线 SVM 分类器的参数。

我们导入`RandomizedSearchCV`类并定义`param_dist`，一个更大的超参数搜索空间：

```py
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import uniform

param_dist = {
    'C': uniform(0.1, 10),  # Uniform distribution between 0.1 and 10
    'kernel': ['linear', 'rbf', 'poly'],
    'gamma': ['scale', 'auto'] + list(np.logspace(-3, 3, 50))
}
```

类似于网格搜索，我们实例化随机搜索模型来寻找最佳超参数。在这里，我们将`n_iter`设置为 20；因此将采样 20 个随机的超参数组合。

```py
# Create the RandomizedSearchCV object
randomized_search = RandomizedSearchCV(estimator=baseline_svm, param_distributions=param_dist, n_iter=20, cv=5)

randomized_search.fit(X_train, y_train)
```

然后我们评估通过随机搜索找到的最佳超参数的模型性能：

```py
# Get the best hyperparameters and model
best_params_rand = randomized_search.best_params_
best_model_rand = randomized_search.best_estimator_

# Evaluate the best model
y_pred_best_rand = best_model_rand.predict(X_test)
accuracy_best_rand = accuracy_score(y_test, y_pred_best_rand)
print(f"Best SVM Accuracy: {accuracy_best_rand:.2f}")
print(f"Best Hyperparameters: {best_params_rand}")
```

最佳准确率和最优超参数为：

```py
Output >>>
Best SVM Accuracy: 0.94
Best Hyperparameters: {'C': 9.66495227534876, 'gamma': 6.25055192527397, 'kernel': 'poly'} 
```

通过随机搜索找到的参数与通过网格搜索找到的参数不同。具有这些超参数的模型也达到了 0.94 的准确率。

# 随机搜索的优缺点

让我们总结一下随机搜索的优点：

+   随机搜索在处理大量超参数或广泛范围的值时效率较高，因为它不需要进行详尽的搜索。

+   它可以处理各种参数类型，包括连续值和离散值。

以下是随机搜索的一些限制：

+   由于其随机特性，它可能不会总是找到最佳的超参数。但它通常能快速找到好的超参数。

+   与网格搜索不同，它不保证所有可能的组合都会被探索。

# 结论

我们学习了如何使用`RandomizedSearchCV`和`GridSearchCV`进行超参数调整。然后，我们用最佳超参数评估了模型的性能。

总结来说，网格搜索详尽地搜索参数网格中的所有可能组合，而随机搜索则是随机抽样超参数组合。

这两种技术都帮助你确定机器学习模型的最佳超参数，同时减少对特定训练-测试划分的过拟合风险。

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发人员和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和咖啡！目前，她正在通过撰写教程、操作指南、观点文章等，与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关话题

+   [使用网格搜索和随机搜索进行超参数调整](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [超参数优化：10 个顶级 Python 库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)

+   [数据治理与可观测性解释](https://www.kdnuggets.com/2022/08/data-governance-observability-explained.html)

+   [混淆矩阵、精确度和召回率解释](https://www.kdnuggets.com/2022/11/confusion-matrix-precision-recall-explained.html)

+   [KDnuggets 新闻，11 月 16 日：LinkedIn 如何使用机器学习 •…](https://www.kdnuggets.com/2022/n45.html)

+   [用 HuggingFace 微调 BERT 进行推文分类](https://www.kdnuggets.com/2022/01/finetuning-bert-tweets-classification-ft-hugging-face.html)
