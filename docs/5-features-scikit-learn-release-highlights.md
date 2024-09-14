# 最新 Scikit-learn 发布版本中的 5 个重要新特性

> 原文：[https://www.kdnuggets.com/2019/12/5-features-scikit-learn-release-highlights.html](https://www.kdnuggets.com/2019/12/5-features-scikit-learn-release-highlights.html)

[评论](#comments)![图示](../Images/af7c40c6dbf55897ff31b808c010e204.png)

Python 的主要机器学习库的最新版本包含了许多新特性和错误修复。您可以在官方的 Scikit-learn 0.22 [发布亮点](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_0_22_0.html) 中找到这些更改的完整说明，并可以在 [变更日志](https://scikit-learn.org/stable/whats_new/v0.22.html#changes-0-22) 中查看详细内容。

更新您的安装通过 pip 完成：

`   pip install --upgrade scikit-learn`

或 conda：

`   conda install scikit-learn`

以下是 Scikit-learn 最新发布版本中值得关注的 5 个新特性。

### 1. 新的绘图 API

现在提供了新的绘图 API，无需任何重新计算即可使用。支持的绘图包括局部依赖图、混淆矩阵和 ROC 曲线等。以下是 API 的演示，使用了来自 Scikit-learn 用户指南的 [示例](https://scikit-learn.org/stable/visualizations.html#visualizations)：

```py
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.metrics import plot_roc_curve
from sklearn.datasets import load_wine

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)
svc = SVC(random_state=42)
svc.fit(X_train, y_train)

svc_disp = plot_roc_curve(svc, X_test, y_test)
```

![图示](../Images/78f780b3ffe836fc2e9b8333cc9f20d7.png)

请注意，绘图是通过代码的最后一行完成的。

### 2. 堆叠泛化

堆叠估计器以减少偏差的集成学习技术现已加入 Scikit-learn。`StackingClassifier` 和 `StackingRegressor` 是实现估计器堆叠的模块，`final_estimator` 使用这些堆叠的估计器预测作为输入。参见 [用户指南](https://scikit-learn.org/stable/modules/ensemble.html#stacked-generalization) 中的示例，使用下面定义的回归估计器作为 `estimators`，并配有一个梯度提升回归器最终估计器：

```py
from sklearn.linear_model import RidgeCV, LassoCV
from sklearn.svm import SVR
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.ensemble import StackingRegressor
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split

estimators = [('ridge', RidgeCV()),
              ('lasso', LassoCV(random_state=42)),
              ('svr', SVR(C=1, gamma=1e-6))]

reg = StackingRegressor(
        estimators=estimators,
        final_estimator=GradientBoostingRegressor(random_state=42))

X, y = load_boston(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)

reg.fit(X_train, y_train)
```

`   StackingRegressor(...)`

### 3. 任何估计器的特征重要性

[基于置换的特征重要性](https://scikit-learn.org/stable/modules/permutation_importance.html) 现在适用于任何拟合的 Scikit-learn 估计器。关于如何计算特征的置换重要性的描述，请参见 [用户指南](https://scikit-learn.org/stable/modules/generated/sklearn.inspection.permutation_importance.html#sklearn.inspection.permutation_importance)：

> 特征的置换重要性计算方法如下。首先，在由 X 定义的（可能不同的）数据集上评估由评分定义的基准指标。接下来，将验证集中的特征列进行置换，然后重新评估指标。置换重要性定义为基准指标与置换特征列后的指标之间的差异。

一个完整的[示例](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_0_22_0.html#permutation-based-feature-importance)来自发布说明：

```py
from sklearn.ensemble import RandomForestClassifier
from sklearn.inspection import permutation_importance

X, y = make_classification(random_state=0, n_features=5, n_informative=3)

rf = RandomForestClassifier(random_state=0).fit(X, y)
result = permutation_importance(rf, X, y, n_repeats=10, random_state=0, n_jobs=-1)

fig, ax = plt.subplots()
sorted_idx = result.importances_mean.argsort()
ax.boxplot(result.importances[sorted_idx].T, vert=False, labels=range(X.shape[1]))
ax.set_title("Permutation Importance of each feature")
ax.set_ylabel("Features")
fig.tight_layout()
plt.show()
```

![图示](../Images/a763ee46ff64e0c595bfcc16be9aeb6e.png)

### 4\. 梯度提升缺失值支持

现在梯度提升分类器和回归器都原生支持处理缺失值，从而不再需要手动填补。以下是如何做出缺失值决策的：

> 在训练过程中，树生长器会在每个分裂点判断缺失值样本是应该分到左子树还是右子树，这取决于潜在的增益。在预测时，缺失值样本会被相应地分配到左子树或右子树。如果在训练期间某个特征没有遇到缺失值，那么缺失值样本将被映射到具有最多样本的子树。

以下[示例](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_0_22_0.html#native-support-for-missing-values-for-gradient-boosting)演示了：

```py
from sklearn.experimental import enable_hist_gradient_boosting  # noqa
from sklearn.ensemble import HistGradientBoostingClassifier
import numpy as np

X = np.array([0, 1, 2, np.nan]).reshape(-1, 1)
y = [0, 0, 1, 1]

gbdt = HistGradientBoostingClassifier(min_samples_leaf=1).fit(X, y)
print(gbdt.predict(X))
```

`   [0 0 1 1]`

### 5\. 基于 KNN 的缺失值填补

尽管梯度提升现在原生支持缺失值填补，但可以使用 K 近邻填补器对任何数据集进行显式填补。每个缺失值都根据训练集中的 *n* 个最近邻的均值进行填补，只要那些样本在某些特征上不是缺失的邻近即可。默认的距离度量是欧氏距离。

一个[示例](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_0_22_0.html#knn-based-imputation)：

```py
import numpy as np
from sklearn.impute import KNNImputer

X = [[1, 2, np.nan], [3, 4, 3], [np.nan, 6, 5], [8, 8, 7]]
imputer = KNNImputer(n_neighbors=2)
print(imputer.fit_transform(X))
```

```py
[[1\.  2\.  4\. ]
 [3\.  4\.  3\. ]
 [5.5 6\.  5\. ]
 [8\.  8\.  7\. ]]
```

最新发布的 Scikit-learn 中还有更多特性未在此覆盖。你可以查看[完整发布亮点](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_0_22_0.html)和[变更日志](https://scikit-learn.org/stable/whats_new/v0.22.html#changes-0-22)以获取更多信息。

祝机器学习愉快！

**相关**：

+   [训练 sklearn 快速 100 倍](/2019/09/train-sklearn-100x-faster.html)

+   [如何扩展 Scikit-learn 并为你的机器学习工作流带来理智](/2019/10/extend-scikit-learn-bring-sanity-machine-learning-workflow.html)

+   [Scikit-Learn & 更多用于机器学习的合成数据集生成](/2019/09/scikit-learn-synthetic-dataset.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关话题

+   [解锁AI的力量 - KDnuggets与Machine…的特别发布](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [数据驱动的AI：你需要了解的最新研究](https://www.kdnuggets.com/2022/02/datacentric-ai-latest-research-need-know.html)

+   [探索AI/DL的最新趋势：从元宇宙到量子计算](https://www.kdnuggets.com/2023/07/exploring-latest-trends-aidl-metaverse-quantum-computing.html)

+   [探索Zephyr 7B：最新大型语言模型的全面指南](https://www.kdnuggets.com/exploring-the-zephyr-7b-a-comprehensive-guide-to-the-latest-large-language-model)

+   [生成式AI游乐场：带有Camel-5b和Open LLaMA 3B的LLM](https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-llms-with-camel-5b-and-open-llama-3b)

+   [发现计算机视觉的世界：介绍MLM的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)
