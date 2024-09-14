# 使用 Python 的欠采样技术

> 原文：[https://www.kdnuggets.com/undersampling-techniques-using-python](https://www.kdnuggets.com/undersampling-techniques-using-python)

随着数字化环境的不断发展，来自多种来源的大量数据正在生成和捕获。虽然这些信息极具价值，但这个庞大的信息宇宙通常反映了现实世界现象的不平衡分布。不平衡数据的问题不仅仅是一个统计挑战，它对数据驱动模型的准确性和可靠性有深远的影响。

以金融行业中日益严重的欺诈检测为例。尽管我们希望避免欺诈，因为它具有极大的破坏性，但机器（甚至人类）不可避免地需要从欺诈交易的实例中学习（尽管很少见），以将其与日常合法交易区分开来。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

欺诈与非欺诈交易之间的数据分布不平衡对旨在检测这些异常活动的机器学习模型提出了重大挑战。如果不适当地处理数据不平衡，这些模型可能会倾向于将交易预测为合法，可能忽略稀有的欺诈实例。

医疗保健是另一个利用机器学习模型预测不平衡结果的领域，例如癌症或罕见的遗传疾病。这些结果发生的频率远低于其良性对手。因此，训练在这些不平衡数据上的模型更容易产生错误的预测和诊断。这样的健康预警失效违背了模型的最终目的，即早期检测疾病。

这些只是突显数据不平衡深远影响的一些例子，即一个类别显著多于另一个类别。过采样和欠采样是平衡数据集的两种标准数据预处理技术，我们将在本文中重点讨论欠采样。

让我们讨论一些流行的欠采样方法。

# 理解不平衡的缺点

让我们从一个说明性示例开始，以更好地理解欠采样技术的重要性。以下可视化展示了每类点的相对数量的影响，执行方式是由一个具有线性核的[支持向量机](https://en.wikipedia.org/wiki/Support_vector_machine)。以下代码和图表参考了[Kaggle notebook](https://www.kaggle.com/code/residentmario/undersampling-and-oversampling-imbalanced-data)。

```py
import matplotlib.pyplot as plt
from sklearn.svm import LinearSVC
import numpy as np
from collections import Counter
from sklearn.datasets import make_classification

def create_dataset(
    n_samples=1000, weights=(0.01, 0.01, 0.98), n_classes=3, class_sep=0.8, n_clusters=1
):
    return make_classification(
        n_samples=n_samples,
        n_features=2,
        n_informative=2,
        n_redundant=0,
        n_repeated=0,
        n_classes=n_classes,
        n_clusters_per_class=n_clusters,
        weights=list(weights),
        class_sep=class_sep,
        random_state=0,
    )

def plot_decision_function(X, y, clf, ax):
    plot_step = 0.02
    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
    xx, yy = np.meshgrid(
        np.arange(x_min, x_max, plot_step), np.arange(y_min, y_max, plot_step)
    )

    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    ax.contourf(xx, yy, Z, alpha=0.4)
    ax.scatter(X[:, 0], X[:, 1], alpha=0.8, c=y, edgecolor="k")

fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(15, 12))

ax_arr = (ax1, ax2, ax3, ax4)
weights_arr = (
    (0.01, 0.01, 0.98),
    (0.01, 0.05, 0.94),
    (0.2, 0.1, 0.7),
    (0.33, 0.33, 0.33),
)
for ax, weights in zip(ax_arr, weights_arr):
    X, y = create_dataset(n_samples=1000, weights=weights)
    clf = LinearSVC().fit(X, y)
    plot_decision_function(X, y, clf, ax)
    ax.set_title("Linear SVC with y={}".format(Counter(y))) 
```

上面的代码生成了四种不同分布的图，从一个高度不平衡的数据集开始，其中一个类主导了97%的实例。第二个和第三个图分别有93%和69%的实例来自单个类，而最后一个图具有完全平衡的分布，即所有三个类各占三分之一的实例。以下是从最不平衡到最平衡的数据集的图。将SVM拟合到这些数据上时，第一幅图（高度不平衡）的超平面被推到图表的一侧，主要是因为算法对每个实例一视同仁，无论其类别如何，并尝试以最大边界分隔类。因此，靠近中心的多数黄色群体将超平面推到角落，使算法误分类少数类。

![使用Python的欠采样技术](../Images/bec1e1e84da0b88c706b1ca6945c2ec3.png)

随着我们向更均衡的分布移动，算法成功地对所有感兴趣的类进行了分类。

总之，当数据集被一个或几个类主导时，结果解决方案通常会导致一个具有更高误分类率的模型。然而，随着每个类的观察分布接近均匀分布，分类器的偏差逐渐减小。

在这种情况下，对黄色点进行欠采样是解决由稀有类问题引起的模型错误的最简单方法。值得注意的是，并非所有数据集都遇到这个问题，但对于遇到这个问题的数据集来说，纠正这种不平衡是建模数据的关键初步步骤。

# Imbalanced-Learn库

我们将使用Imbalanced-Learn Python库（imbalanced-learn或imblearn）。我们可以通过pip安装它：

```py
pip install -U imbalanced-learn
```

# 动手实践！

让我们讨论并实验一些最受欢迎的欠采样技术。假设你有一个二分类数据集，其中类'0'的数量远多于类'1'。

## NearMiss欠采样

NearMiss是一种欠采样技术，通过减少接近少数类的多数类样本的数量来实现。这将通过任何使用空间分离或在两个类之间分割维度空间的算法来促进干净的分类。NearMiss有三个版本：

**NearMiss-1**：与三个最近的少数类样本之间的最小平均距离的多数类样本。

**NearMiss-2**：与三个最远少数类样本之间的最小平均距离的多数类样本。

**NearMiss-3**：与每个少数类样本距离最小的多数类样本。

让我们通过一个代码示例来演示 NearMiss-1 欠采样算法：

```py
# Import necessary libraries and modules
import numpy as np
import matplotlib.pyplot as plt
from collections import Counter
from sklearn.datasets import make_classification
from imblearn.under_sampling import NearMiss

# Generate the dataset with different class weights
features, labels = make_classification(
    n_samples=1000,
    n_features=2,
    n_redundant=0,
    n_clusters_per_class=1,
    weights=[0.95, 0.05],
    flip_y=0,
    random_state=0,
)

# Print the distribution of classes
dist_classes = Counter(labels)
print("Before Undersampling:")
print(dist_classes)

# Generate a scatter plot of instances, labeled by class
for class_label, _ in dist_classes.items():
    instances = np.where(labels == class_label)[0]
    plt.scatter(features[instances, 0], features[instances, 1], label=str(class_label))
plt.legend()
plt.show()

# Set up the undersampling method
undersampler = NearMiss(version=1, n_neighbors=3)

# Apply the transformation to the dataset
features, labels = undersampler.fit_resample(features, labels)

# Print the new distribution of classes
dist_classes = Counter(labels)
print("After Undersampling:")
print(dist_classes)

# Generate a scatter plot of instances, labeled by class
for class_label, _ in dist_classes.items():
    instances = np.where(labels == class_label)[0]
    plt.scatter(features[instances, 0], features[instances, 1], label=str(class_label))
plt.legend()
plt.show()
```

![使用 Python 的欠采样技术](../Images/9a4af1e161350ba9bc196e2540919101.png)

将 NearMiss() 类中的 version=1 更改为 version=2 或 version=3，以使用 NearMiss-2 或 NearMiss-3 欠采样算法。

![使用 Python 的欠采样技术](../Images/f8c8917c39a65cace83b0d697f67d9b2.png)![使用 Python 的欠采样技术](../Images/b90b8b6b5c296ec65b4a1c384e6bae09.png)

NearMiss-2 选择位于两个类别之间重叠区域核心的实例。使用 NeverMiss-3 算法时，我们观察到每个与多数类区域重叠的少数类实例最多有三个来自多数类的邻居。代码示例中的属性 n_neighbors 定义了这一点。

# **压缩最近邻（CNN）规则**

该方法首先将多数类的一个子集视为噪声。然后，使用 1-最近邻算法来分类实例。如果多数类的实例被错误分类，则将其包含在子集中。这个过程持续进行，直到没有更多实例被包含在子集中。

```py
from imblearn.under_sampling import CondensedNearestNeighbour

cnn = CondensedNearestNeighbour(random_state=42)
X_res, y_res = cnn.fit_resample(X, y)
```

# Tomek 链欠采样

Tomek 链是紧密相邻的相反类别实例对。移除每对中的多数类实例可以增加两个类别之间的空间，从而促进分类过程。

```py
from imblearn.under_sampling import TomekLinks

tl = TomekLinks()
X_res, y_res = tl.fit_resample(X, y)

print('Original dataset shape:', Counter(y))
print('Resample dataset shape:', Counter(y_res))
```

通过这些内容，我们已深入探讨了 Python 中欠采样技术的基本方面，涵盖了三种突出的方法：Near Miss 欠采样、压缩最近邻和 Tomek 链欠采样。

欠采样是处理机器学习中类别不平衡问题的关键数据处理步骤，并且有助于提高模型的性能和公平性。这些技术各有独特的优势，可以根据特定数据集和机器学习项目的目标进行调整。

本文提供了对欠采样方法及其在 Python 中应用的全面理解。希望这能帮助你在机器学习项目中做出明智的决策，以应对类别不平衡问题。

**[](https://vidhi-chugh.medium.com/)**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位 AI 策略师和数字化转型领袖，致力于在产品、科学和工程的交汇点上构建可扩展的机器学习系统。她是一位屡获殊荣的创新领袖、作者和国际演讲者。她的使命是将机器学习民主化，并打破术语，使每个人都能参与这一转型。**

### 更多相关内容

+   [如何使用 Pandas 中的插值技术处理缺失数据](https://www.kdnuggets.com/how-to-deal-with-missing-data-using-interpolation-techniques-in-pandas)

+   [使用 Python 探索数据清洗技术](https://www.kdnuggets.com/2023/04/exploring-data-cleaning-techniques-python.html)

+   [集成学习技术：使用 Python 中的随机森林进行演示](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [处理不平衡数据的 7 种技术](https://www.kdnuggets.com/2017/06/7-techniques-handle-imbalanced-data.html)

+   [终极指南：不同的词嵌入技术在 NLP 中的应用](https://www.kdnuggets.com/2021/11/guide-word-embedding-techniques-nlp.html)

+   [学习现代预测技术，帮助预测未来的商业结果…](https://www.kdnuggets.com/2022/12/sphere-learn-modern-forecasting-techniques-help-predict-future-business-outcomes.html)
