# 向前特征选择：Python 中的实际示例

> 原文：[`www.kdnuggets.com/2018/06/step-forward-feature-selection-python.html`](https://www.kdnuggets.com/2018/06/step-forward-feature-selection-python.html)

评论

存在许多[特征选择](https://en.wikipedia.org/wiki/Feature_selection)的方法，其中一些将过程视为艺术形式，另一些视为科学，而实际上，一些领域知识结合严格的方法可能是你最好的选择。

在特征选择的严格方法中，[包装方法](https://en.wikipedia.org/wiki/Feature_selection#Wrapper_method)是将特征选择过程与正在构建的模型类型相结合，评估特征子集以检测特征之间的模型表现，从而选择表现最佳的子集。换句话说，包装方法不是作为独立的过程在模型构建*之前*进行，而是试图*与*给定的机器学习算法一起优化特征选择过程。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

特征选择的两个显著包装方法是向前特征选择和向后特征选择。

![图片](img/17075aa097f90c967aa0ef0730e1a2ba.png)

[图片来源](https://en.wikipedia.org/wiki/Feature_selection)

向前特征选择从评估每个单独特征开始，选择那些能产生最佳表现的算法模型的特征。什么是“最佳”？这完全取决于定义的评估标准（AUC、预测准确率、RMSE 等）。接下来，评估所有可能的选定特征与随后的特征的组合，选择第二个特征，依此类推，直到选择出所需的预定义数量的特征。

向后特征选择密切相关，正如你可能已经猜到的那样，它从整个特征集开始，然后向后操作，从中去除特征，以找到预定义大小的最佳子集。

这两种方法都可能非常耗费计算资源。你有一个大型的、多维的数据集吗？这些方法可能会花费过多时间，变得毫无用处，或者完全不可行。不过，考虑到数据集的规模和维度，这种方法可能是你最佳的选择。

为了了解它们是如何工作的，我们具体看一下逐步前进特征选择。请注意，如前所述，机器学习算法必须在开始我们的共生特征选择过程之前定义。

请记住，使用给定算法选择的优化特征集可能在不同算法下表现不同。例如，如果我们使用逻辑回归选择特征，并不能保证这些相同的特征在使用 K 近邻算法或支持向量机时也能表现最佳。

### 实现特征选择和构建模型

那么，我们如何在 Python 中执行逐步前进特征选择呢？[Sebastian Raschka 的 mlxtend 库](https://github.com/rasbt/mlxtend)包含了一个实现（[Sequential Feature Selector](https://rasbt.github.io/mlxtend/user_guide/feature_selection/SequentialFeatureSelector/)），因此我们将使用它来演示。显然，在继续之前你应该已经安装了 mlxtend（查看 Github 仓库）。

![Image](img/d13fd31db64ac19a44c703d8cdc720ec.png)

我们将使用随机森林分类器进行特征选择和模型构建（再次强调，在逐步前进特征选择的情况下，这两者是紧密相关的）。

我们需要数据来进行演示，所以我们使用[葡萄酒质量数据集](https://archive.ics.uci.edu/ml/datasets/wine+quality)。具体来说，我在下面的代码中使用了未处理的`winequality-white.csv`文件作为输入。

随意地，我们将所需的特征数量设置为 5（数据集中有 12 个特征）。我们能够做的是比较特征选择过程每次迭代的评估分数，因此请记住，如果我们发现特征数量较少的情况下得分更好，我们可以选择该表现最佳的子集来继续在我们的“实时”模型中使用。同时，请注意，设置所需的特征数量过低可能会导致决定一个次优的特征数量和组合（例如，如果我们发现某些 11 个特征的组合比我们在选择过程中找到的<=10 个特征的最佳组合更好）。

由于我们更关注演示如何实现逐步前进特征选择，而不是对这个特定数据集的实际结果，我们不会过于关注模型的实际性能，但我们会进行比较，以展示如何在一个有意义的项目中进行。

首先，我们将进行导入操作，加载数据集，并将其拆分为训练集和测试集。

```py
import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score as acc
from mlxtend.feature_selection import SequentialFeatureSelector as sfs

# Read data
df = pd.read_csv('winequality-white.csv', sep=';')

# Train/test split
X_train, X_test, y_train, y_test = train_test_split(
    df.values[:,:-1],
    df.values[:,-1:],
    test_size=0.25,
    random_state=42)

y_train = y_train.ravel()
y_test = y_test.ravel()

print('Training dataset shape:', X_train.shape, y_train.shape)
print('Testing dataset shape:', X_test.shape, y_test.shape)

```

```py

Training dataset shape: (3673, 11) (3673,)
Testing dataset shape: (1225, 11) (1225,)
```

接下来，我们将定义一个分类器，以及一个逐步前进特征选择器，然后执行特征选择。mlxtend 中的特征选择器有一些我们可以定义的参数，以下是我们的处理方式：

+   首先，我们将我们的分类器传递给定义的特征选择器

+   接下来，我们定义了我们希望选择的特征子集（k_features=5）。

+   然后我们将浮动设置为 False；有关浮动的更多信息，请参阅文档：

> 浮动算法有一个额外的排除或包含步骤，以便在特征被包含（或排除）后将其移除，从而可以采样更多的特征子集组合。

+   我们设置了 mlxtend 报告的期望详细程度。

+   重要的是，我们将评分设置为准确率；这只是一个用于对基于所选特征构建的模型进行评分的指标。

+   mlxtend 特征选择器内部使用交叉验证，我们将演示中的折数设置为 5。

我们选择的数据集不是很大，因此以下代码不应该花费很长时间执行。

```py
# Build RF classifier to use in feature selection
clf = RandomForestClassifier(n_estimators=100, n_jobs=-1)

# Build step forward feature selection
sfs1 = sfs(clf,
           k_features=5,
           forward=True,
           floating=False,
           verbose=2,
           scoring='accuracy',
           cv=5)

# Perform SFFS
sfs1 = sfs1.fit(X_train, y_train)

```

```py

[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    2.2s remaining:    0.0s
[Parallel(n_jobs=1)]: Done  11 out of  11 | elapsed:   24.3s finished

[2018-06-12 14:47:47] Features: 1/5 -- score: 0.49768148939247264[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    2.2s remaining:    0.0s
[Parallel(n_jobs=1)]: Done  10 out of  10 | elapsed:   22.7s finished

[2018-06-12 14:48:09] Features: 2/5 -- score: 0.5442629071398873[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    2.7s remaining:    0.0s
[Parallel(n_jobs=1)]: Done   9 out of   9 | elapsed:   21.2s finished

[2018-06-12 14:48:31] Features: 3/5 -- score: 0.6052194438136681[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    2.5s remaining:    0.0s
[Parallel(n_jobs=1)]: Done   8 out of   8 | elapsed:   20.3s finished

[2018-06-12 14:48:51] Features: 4/5 -- score: 0.6261526236769334[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    2.4s remaining:    0.0s
[Parallel(n_jobs=1)]: Done   7 out of   7 | elapsed:   17.3s finished

[2018-06-12 14:49:08] Features: 5/5 -- score: 0.6444222989869156
```

根据我们的评分指标，我们表现最佳的模型是某个包含 5 个特征的子集，得分为 0.644（记住这是使用交叉验证，因此与下面使用训练和测试集的完整模型的报告结果不同）。但是，选择了哪一组 5 个特征？

```py
# Which features?
feat_cols = list(sfs1.k_feature_idx_)
print(feat_cols)

```

```py

[1, 2, 3, 7, 10]

```

这些索引处的列是那些被选择的列。太好了！那么接下来做什么呢...？

我们现在可以使用这些特征在训练和测试集上构建一个完整的模型。如果我们有一个更大的数据集（即更多的实例而不是更多的特征），这将特别有利，因为我们可以在较小的实例子集上使用上述特征选择器，确定我们表现最佳的特征子集，然后将它们应用于完整的数据集进行分类。

以下代码在**仅**所选特征的子集上构建了一个分类器。

```py
# Build full model with selected features
clf = RandomForestClassifier(n_estimators=1000, random_state=42, max_depth=4)
clf.fit(X_train[:, feat_cols], y_train)

y_train_pred = clf.predict(X_train[:, feat_cols])
print('Training accuracy on selected features: %.3f' % acc(y_train, y_train_pred))

y_test_pred = clf.predict(X_test[:, feat_cols])
print('Testing accuracy on selected features: %.3f' % acc(y_test, y_test_pred))

```

```py

Training accuracy on selected features: 0.558
Testing accuracy on selected features: 0.512

```

不用担心实际的准确率；我们在这里关注的是过程，而不是最终结果。

但如果我们*关心*最终结果，并想知道我们的特征选择是否值得？好吧，我们可以比较使用所选特征（见上文）构建的完整模型的结果准确率与使用**所有**特征的另一个完整模型的结果准确率，就像下面这样：

```py
# Build full model on ALL features, for comparison
clf = RandomForestClassifier(n_estimators=1000, random_state=42, max_depth=4)
clf.fit(X_train, y_train)

y_train_pred = clf.predict(X_train)
print('Training accuracy on all features: %.3f' % acc(y_train, y_train_pred))

y_test_pred = clf.predict(X_test)
print('Testing accuracy on all features: %.3f' % acc(y_test, y_test_pred))

```

```py

Training accuracy on all features: 0.566
Testing accuracy on all features: 0.509

```

这里是它们的比较结果。它们都表现不佳，并且与我们使用所选特征构建的模型相当（虽然我保证这并非总是如此！）。通过非常少的工作，您可以查看这些选定特征在不同算法下的表现，以帮助解答这些特征是否在另一种算法下表现同样优秀。

这种特征选择方法可以是一个有力的、有纪律的机器学习流程中的组成部分。请记住，前向（或后向）方法，尤其是在处理特别大或高维数据集时，可能会带来问题。可以通过从数据中采样以找到最适合的特征子集，然后在完整数据集上使用这些特征进行建模的方式来绕过（或**尝试**绕过）这些难点。当然，这些也不是唯一的特征选择的有纪律的方法，因此在处理这些较大的数据集时，检查其他方法也是值得的。

**相关**:

+   [使用 fast.ai 快速进行日期特征工程](https://www.kdnuggets.com/2018/03/feature-engineering-dates-fastai.html)

+   [用 4 行代码生成文本的 RNN](https://www.kdnuggets.com/2018/06/generating-text-rnn-4-lines-code.html)

+   [特征选择的多目标优化](https://www.kdnuggets.com/2017/12/rapidminer-multi-objective-optimization-feature-selection.html)

### 更多相关话题

+   [揭开 Midjourney 5.2 的面纱：AI 图像生成的飞跃](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)

+   [揭开 Meta 的 Llama 2 的力量：生成 AI 的飞跃？](https://www.kdnuggets.com/2023/07/unveiling-power-metas-llama-2-leap-forward-generative-ai.html)

+   [特征选择：科学与艺术的交汇点](https://www.kdnuggets.com/2021/12/feature-selection-science-meets-art.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [机器学习中特征工程的实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)
