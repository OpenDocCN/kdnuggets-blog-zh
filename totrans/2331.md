# SMOTE简介

> 原文：[https://www.kdnuggets.com/2022/11/introduction-smote.html](https://www.kdnuggets.com/2022/11/introduction-smote.html)

![SMOTE简介](../Images/8b829ac989ef7e8c411cf74f02fca11b.png)

图片由作者提供

当我们拥有不平衡的分类数据集时，模型学习决策边界的少数类样本很少。这也会整体影响模型的表现。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

你可以通过过采样少数类来解决这个问题，你可以通过复制训练数据集中少数类的样本来实现。这将平衡类别分布，但不会提高模型性能，因为它没有为模型提供额外的信息。

那么，你如何同时平衡类别分布并提高模型性能呢？通过使用SMOTE（合成少数类过采样技术）从少数类中合成新的样本。

# 什么是SMOTE？

SMOTE（合成少数类过采样技术）是一种平衡数据集中类别分布的过采样方法。它选择接近特征空间的少数样本。然后，它在特征空间中的样本之间绘制一条线，并在这条线上生成一个新的样本。

简单来说，算法从少数类中选择一个随机样本，并使用K最近邻选择一个随机邻居。在特征空间中，这个合成样本在两个样本之间创建。

使用SMOTE有一个缺点，因为在创建合成样本时，它没有考虑多数类。这可能会导致类别之间有很强的重叠。

让我们通过使用Imbalanced-Learn库来观察SMOTE的实际效果。

```py
%pip install imbalanced-learn
```

> **注意：**我们使用Deepnote笔记本来运行实验。

# 不平衡数据集

我们将使用sci-kit learn的数据集模块中的make_classification来创建一个不平衡的分类数据集。

```py
from collections import Counter
from sklearn.datasets import make_classification
from imblearn.over_sampling import SMOTE
import matplotlib.pyplot as plt

# create a binary classification dataset
X, y = make_classification(
    n_samples=1000,
    n_features=2,
    n_redundant=0,
    n_clusters_per_class=1,
    weights=[0.98],
    random_state=125,
)

labels = Counter(y)
print("y labels after oversampling")
print(labels)
```

正如我们观察到的，样本总数为1K。970个属于**0**标签，只有30个属于**1**。

```py
y labels after oversampling
Counter({0: 970, 1: 30})
```

然后我们将使用matplotlib的pyplot来可视化数据集。

正如我们所见，图表上只有少量的黄色点（1），而紫色点则更多。这是一个明显的不平衡数据集的例子。

```py
plt.scatter(X[:, 0], X[:, 1], marker="o", c=y, s=50, edgecolor="k");
```

![SMOTE简介](../Images/b0df5995c9c02ac685d26f27b7cfdc01.png)

## 模型训练与评估

在我们使用过采样平衡数据集之前，我们需要为模型性能设定基准。

我们将使用决策树分类模型在数据集上进行10折3次交叉验证来进行训练和评估。简而言之，我们将在数据集上训练和评估30个模型。

RepeatedStratifiedKFold中的分层意味着每次交叉验证的划分都具有与原始数据集相同的类别分布。

```py
import numpy as np
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import RepeatedStratifiedKFold
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier()

cv = RepeatedStratifiedKFold(n_splits=10, n_repeats=3, random_state=1)
result = cross_val_score(model, X, y, scoring="roc_auc", cv=cv, n_jobs=-1)
print("Mean AUC: %.3f" % np.mean(result))
```

我们得到了**ROC AUC**平均得分为**0.626**，这个结果相当低。

```py
Mean AUC: 0.626
```

# 使用SMOTE()进行过采样

我们现在将应用过采样方法SMOTE来平衡数据集。我们将使用[imbalanced-learn](https://pypi.org/project/imbalanced-learn/)的SMOTE函数，并提供特征(X)和标签(y)。

```py
over = SMOTE()

X, y = over.fit_resample(X, y)

labels = Counter(y)
print("y labels after oversampling")
print(labels)
```

现在0和1标签的样本数量已经平衡，每个标签都有970个样本。

```py
y labels after oversampling
Counter({0: 970, 1: 970})
```

让我们可视化合成平衡的数据集。我们可以清楚地看到黄色和紫色的点数相等。

```py
plt.scatter(X[:, 0], X[:, 1], marker="o", c=y, s=50, edgecolor="k");
```

![SMOTE简介](../Images/14e3502b11ed60dc043e62ace29e3f20.png)

## 模型训练和评估

我们现在将在合成数据集上训练模型并评估结果。我们保持一切不变，以便将其与基准结果进行比较。

```py
model = DecisionTreeClassifier()

cv = RepeatedStratifiedKFold(n_splits=10, n_repeats=3, random_state=1)
result = cross_val_score(model, X, y, scoring="roc_auc", cv=cv, n_jobs=-1)

print("Mean AUC: %.3f" % np.mean(result))
```

经过几秒钟的训练后，我们得到了改进的结果或**ROC AUC**平均得分为**0.834**。这清楚地表明过采样确实能提高模型性能。

```py
Mean AUC: 0.834
```

# 结论

原始的SMOTE [论文](https://arxiv.org/abs/1106.1813)建议将过采样(SMOTE)与多数类的欠采样相结合，因为SMOTE在创建新样本时不考虑多数类。少数类的过采样(SMOTE)和多数类的欠采样的组合可以给我们更好的结果。

在本教程中，我们了解了为什么使用SMOTE及其工作原理。我们还学习了imbalanced-learn库及其如何用于提高模型性能和平衡类分布。

希望你喜欢我的工作，别忘了关注我在社交媒体上，以了解有关数据科学、机器学习、自然语言处理、MLOps、Python、Julia、R和Tableau的内容。

## 参考

1.  [使用Python进行不平衡分类的SMOTE - MachineLearningMastery.com](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan))是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个AI产品，帮助那些在精神疾病方面挣扎的学生。

### 更多相关主题

+   [7种SMOTE变体用于过采样](https://www.kdnuggets.com/2023/01/7-smote-variations-oversampling.html)

+   [使用 PyCaret 的二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 PyCaret 进行 Python 聚类的介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [数据科学的基础数学：奇异值分解的视觉介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [数据科学中的 Pandas 入门](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

+   [带代码的论文简要介绍](https://www.kdnuggets.com/2022/04/brief-introduction-papers-code.html)
