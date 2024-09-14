# 打开黑箱：如何利用可解释的机器学习

> 原文：[`www.kdnuggets.com/2019/08/open-black-boxes-explainable-machine-learning.html`](https://www.kdnuggets.com/2019/08/open-black-boxes-explainable-machine-learning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Maarten Grootendorst](https://www.linkedin.com/in/mgrootendorst/)，Emset**。

随着机器学习和人工智能越来越受欢迎，越来越多的组织正在采用这一新技术。预测建模帮助过程变得更加高效，同时也让用户获得利益。人们可以根据专业技能和经验预测你可能赚多少钱。输出可能只是一个数字，但用户通常想知道为什么给出这个值！

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在本文中，我将演示一些创建可解释预测的方法，并指导你打开这些黑箱模型。

![](img/313ed8b941464d1b36522ff8b99f30db.png)

本文使用的数据集是美国成人收入数据集，通常用于预测某人是否赚取少于 50K 或多于 50K，这是一个简单的二分类任务。你可以在[这里](https://www.kaggle.com/johnolafenwa/us-census-data)获取数据，或者可以跟随[这里](https://github.com/MaartenGr/InterpretableML)的笔记本进行操作。

数据相对简单，包含个人的关系、职业、种族、性别等信息。

![](img/683718f5d319cd68e46d7d1116649495.png)

### 建模

类别变量被进行了一次性编码，目标设置为 0（≤50K）或 1（>50K）。现在，假设我们想使用一个在分类任务中表现优秀但非常复杂且输出难以解释的模型，这个模型是 LightGBM，它与 CatBoost 和 XGBoost 一起，常用于分类和回归任务。

我们从简单地拟合模型开始：

```py
from lightgbm import LGBMClassifier
X = df[df.columns[1:]]
y = df[df.columns[0]]
clf = LGBMClassifier(random_state=0, n_estimators=100)
fitted_clf = clf.fit(X, y)

```

我应该注意到，最好有一个训练/测试分割，并且有额外的保留数据来防止任何过拟合。

接下来，我快速检查模型的性能，使用 10 折交叉验证：

```py
from sklearn.model_selection import cross_val_score
scores_accuracy = cross_val_score(clf, X, y, cv=10)
scores_balanced = cross_val_score(clf, X, y, cv=10,scoring="balanced_accuracy")

```

有趣的是，scores_accuracy 在 10 折交叉验证中的平均准确率为 87%，而 scores_balanced 为 80%。事实证明，目标变量是不平衡的，25%的目标属于 1，75%属于 0。因此，选择正确的验证指标非常重要，因为它可能会虚假地表示一个好的模型。

现在我们已经创建了模型，接下来就是解释它具体做了什么。由于 LightGBM 使用高效的梯度提升决策树，输出的解释可能会很困难。

### 部分依赖图（PDP）

部分依赖图（PDP）显示了某个特征对预测模型结果的影响。它在特征分布上对模型输出进行边际化，以提取感兴趣特征的重要性。我使用的包，PDPbox，可以在[这里](https://github.com/SauceCat/PDPbox)找到。

**假设**

这个重要性计算基于一个重要的假设，即感兴趣的特征与所有其他特征（目标除外）不相关。原因在于，这样会显示出可能不现实的数据点。例如，体重和身高是相关的，但 PDP 可能会显示大体重和非常小的身高对目标的影响，而这种组合的可能性很小。通过在 PDP 底部显示一个小条，可以部分解决这个问题。

**相关性**

因此，我们检查特征之间的相关性，以确保没有问题：

```py
import seaborn as sns
corr = raw_df.corr()
sns.heatmap(corr, 
 xticklabels=corr.columns,
 yticklabels=corr.columns)

```

![](img/60bb33e90dd4bc99b3dae6fb4f12957b.png)

我们可以看到特征之间没有明显的相关性。然而，我稍后会进行一些独热编码，以准备数据进行建模，这可能会导致相关特征的产生。

**连续变量**

PDP 图可以用来可视化一个变量在所有数据点上的输出影响。让我们从一个明显的例子开始，连续变量*capital_gain*对目标的影响：

```py
from pdpbox import pdp
pdp_fare = pdp.pdp_isolate(
    model=clf, dataset=df[df.columns[1:]], model_features=df.columns[1:], feature='Capital_gain'
)
fig, axes = pdp.pdp_plot(pdp_fare, 'Capital_gain', plot_pts_dist=True)

```

![](img/3a4b24499ee4089c1915fe3aad26d54a.png)

x 轴显示了 capital_gain 可能的值，y 轴则表示它对二分类概率的影响。显然，随着*capital_gain*的增加，其赚取<50K 的机会也会增加。注意，底部的数据点条有助于识别不常出现的数据点。

**独热编码变量**

现在，如果你有一个已经进行独热编码的分类变量呢？你可能想查看各类别的单独影响，而不必单独绘制它们。使用 PDPbox，你可以同时显示多个二分类变量的效果：

```py
pdp_race = pdp.pdp_isolate(
    model=clf, dataset=df[df.columns[1:]],    
    model_features=df.columns[1:], 
    feature=[i for i in df.columns if 'O_' in i if i not in 
                          ['O_ Adm-clerical', 
                           'O_ Armed-Forces', 
                           'O_ Armed-Forces',
                           'O_ Protective-serv', 
                           'O_ Sales', 
                           'O_ Handlers-cleaners']])
fig, axes = pdp.pdp_plot(pdp_race, 'Occupation', center=True, 
                         plot_lines=True, frac_to_plot=100,  
                         plot_pts_dist=True)

```

![](img/1f2a8c0afee9ce8416afcc2c1c66e290.png)

在这里，你可以清楚地看到，无论是处于管理职位还是技术职位，都对获得更多机会有正面影响。如果你在渔业工作，机会会减少。

**变量之间的交互**

最后，变量之间的交互可能是有问题的，因为 PDP 会为组合创建值，如果变量高度相关，这些值可能不太可能存在。

```py
inter1 = pdp.pdp_interact(
    model=clf, dataset=df[df.columns[1:]], 
    model_features=df.columns[1:], features=['Age', 'Hours/Week'])
fig, axes = pdp.pdp_interact_plot(
    pdp_interact_out=inter1, feature_names=['Age', 'Hours/Week'], 
    plot_type='grid', x_quantile=True, plot_pdp=True)

```

![](img/5d5167ae19832b3753cbd36e65aa82d1.png)

该矩阵告诉你，如果一个人约 49 岁并且每周工作大约 50 小时，他们可能赚得更多。我应该指出，重要的是要记住数据集中所有交互的实际分布。这个图可能会显示出一些有趣的交互，但这些交互很少或永远不会发生。

### 局部可解释模型无关解释（LIME）

LIME 基本上试图远离推导全局特征的重要性，而是近似局部预测中特征的重要性。它通过从中预测的行（或数据点集）生成假数据，然后计算假数据与真实数据之间的相似性，并根据假数据和真实数据之间的相似性来近似变化的影响。我使用的包可以在[这里](https://github.com/marcotcr/lime)找到。

```py
from lime.lime_tabular import LimeTabularExplainer
explainer = LimeTabularExplainer(X.values, feature_names=X.columns, 
                                 class_names=["<=50K", ">50K"], 
                                 discretize_continuous=True,
                                 kernel_width=5)
i = 304
exp = explainer.explain_instance(X.values[i], clf.predict_proba, 
                                 num_features=5)
exp.show_in_notebook(show_table=True, show_all=False)

```

![](img/e929e6ebcb6cbdb81d8ddeadfb30f189.png)

*LIME 对单行数据的输出。*

输出显示了前 5 个变量对预测概率的影响。这有助于确定模型为何做出特定预测，同时也为用户提供了解释。

**缺点**

注意 LIME 尝试为初始行寻找不同值的邻域（核宽度）在一定程度上是一个可以优化的超参数。有时你需要根据数据选择更大的邻域。找到合适的核宽度有点像试错过程，因为它可能会影响解释的可解释性。

### Shapley 加法解释（SHAP）

一个（相当）新的发展是将 Shapley 值应用于机器学习。其本质上，SHAP 使用博弈论来跟踪每个变量的*边际贡献*。对于每个变量，它随机从数据集中抽样其他值并计算模型评分的变化。这些变化然后被平均以创建总结评分，同时也提供了有关特定数据点的某些变量的重要性的信息。

点击[这里](https://github.com/slundberg/shap)查看我在分析中使用的包。有关 SHAP 理论背景的更深入解释，请点击[这里](https://towardsdatascience.com/one-feature-attribution-method-to-supposedly-rule-them-all-shapley-values-f3e04534983d)。

**可解释性的三个公理**

SHAP 因其满足可解释性的三个公理而受到好评：

+   任何对预测值没有影响的特征，其 Shapley 值应为 0（虚拟特征）

+   如果两个特征对预测增加相同的值，则它们的 Shapley 值应相同（可替代性）

+   如果你有两个或更多预测需要合并，你应该能够简单地将对个别预测计算的 Shapley 值相加（加性原理）

**二分类**

让我们看看如果计算单行的 Shapley 值结果会如何：

```py
import shap
explainer = shap.TreeExplainer(clf)
shap_values = explainer.shap_values(X)
shap.initjs()
shap.force_plot(explainer.expected_value, shap_values[1,:],  
                X.iloc[1,:])

```

![](img/f20722d81563bf308ce241839154c08c.png)

*单个数据点的 Shapley 值。*

该图显示了一个基准值，用于指示预测的方向。由于大多数目标是 0，因此基准值为负数并不奇怪。

红色条形图显示当*教育年数*为 13 时，目标为 1（>50K）的概率增加了多少。更高的教育通常会带来更多的收入。

蓝色条形图显示这些变量降低了概率，其中*年龄*的影响最大。这是合理的，因为年轻人通常赚得更少。

**回归**

Shapley 在处理回归（连续变量）时直观上表现得更好，而不是二分类。为了展示一个例子，让我们训练一个模型，从同一个数据集中预测年龄：

```py
# Fit model with target Age
X = df[df.columns[2:]]
y = df[df.columns[1]]
clf = LGBMRegressor(random_state=0, n_estimators=100)
fitted_clf = clf.fit(X, y)
# Create Shapley plot for one row
explainer = shap.TreeExplainer(clf)
shap_values = explainer.shap_values(X)
shap.initjs()
i=200
shap.force_plot(explainer.expected_value, shap_values[i,:], X.iloc[i,:])

```

![](img/284c3c397552aa096e96fe0e57c541e3.png)

*单个数据点的 Shapley 值。*

在这里，你可以快速观察到，如果你从未结婚，预测的年龄大约降低 8 岁。这比分类任务更有助于解释预测，因为你直接谈论的是目标的值，而不是其概率。

**一热编码特征**

加性公理允许对所有数据点的每个特征的 Shapley 值进行求和，以创建均值绝对 Shapley 值。换句话说，它提供了全局特征重要性：

```py
shap.summary_plot(shap_values, X, plot_type="bar", show=False)

```

![](img/e9b93c3774dbe2de1b9d998f466553da.png)

然而，你可以立即看到使用 Shapley 值处理一热编码特征的问题，它们是针对每个一热编码特征显示的，而不是它们最初所代表的。

幸运的是，加性公理允许将每个一热编码生成特征的 Shapley 值进行求和，以表示整个特征的 Shapley 值。

首先，我们需要对所有一热编码特征的 Shapley 值进行求和：

```py
summary_df = pd.DataFrame([X.columns, 
                           abs(shap_values).mean(axis=0)]).T
summary_df.columns = ['Feature', 'mean_SHAP']
mapping = {}
for feature in summary_df.Feature.values:
    mapping[feature] = feature
for prefix, alternative in zip(['Workclass', 'Education',  
                                    'Marital_status', 'O_',  
                                    'Relationship', 'Gender',   
                                    'Native_country', 'Capital',  
                                    'Race'],
                                   ['Workclass', 'Education', 
                                    'Marital_status', 'Occupation', 
                                    'Relationship', 'Gender', 
                                    'Country', 'Capital', 'Race']):
        if feature.startswith(prefix):
            mapping[feature] = alternative
            break

summary_df['Feature'] = summary_df.Feature.map(mapping)
shap_df = (summary_df.groupby('Feature')
                    .sum()
                    .sort_values("mean_SHAP", ascending=False)
                    .reset_index())

```

现在，所有 Shapley 值已在所有特征中取平均并对一热编码特征进行求和，我们可以绘制结果特征重要性：

```py
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(style="whitegrid")
f, ax = plt.subplots(figsize=(6, 15))
sns.barplot(x="mean_SHAP", y="Feature", data=shap_df[:5],
            label="Total", color="b")

```

![](img/381d21c7fd64b721b0e565944e50af1e.png)

现在我们可以看到，*职业*比最初的 Shapley 总结图表所显示的要重要得多。因此，在向用户解释特征的重要性时，确保利用加性原理。用户可能更感兴趣于*职业*的重要性，而不是具体的*职业*。

### 结论

尽管这不是第一篇讨论可解释和解释性机器学习的文章，但我希望这能帮助你了解在开发模型时如何使用这些技术。

对 SHAP 的关注日益增加，我希望通过使用独热编码特征展示加法公理，能够深入理解如何使用这种方法。

带有代码的笔记本可以在[此处](https://github.com/MaartenGr/InterpretableML)找到。

**简介：** [Maarten Grootendorst](https://www.linkedin.com/in/mgrootendorst/) 是数据科学家 | I/O 心理学家 | 临床心理学家 | Emset 的联合创始人。

[原文](https://towardsdatascience.com/opening-black-boxes-how-to-leverage-explainable-machine-learning-dd4ab439998e)。经许可转载。

**相关：**

+   [解释性机器学习/xAI 的数据科学指南](https://www.kdnuggets.com/2019/07/domino-xai-data-science-explainable.html)

+   [“请解释。” 机器学习模型的可解释性](https://www.kdnuggets.com/2019/05/interpretability-machine-learning-models.html)

+   [可解释 AI 介绍及其必要性](https://www.kdnuggets.com/2019/04/introduction-explainable-ai.html)

### 更多相关内容

+   [黑色星期五优惠 - 使用 DataCamp 以更低价格掌握机器学习](https://www.kdnuggets.com/2022/11/datacamp-black-friday-deal-master-machine-learning-less-datacamp.html)

+   [如何更好地利用数据科学推动业务增长](https://www.kdnuggets.com/2022/08/better-leverage-data-science-business-growth.html)

+   [利用 RAPIDS cuDF 在特征工程中发挥 GPU 作用](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [如何利用 Docker 缓存优化构建速度](https://www.kdnuggets.com/how-to-leverage-docker-cache-for-optimizing-build-speeds)

+   [对模型信心的探索：你能信任一个黑箱吗？](https://www.kdnuggets.com/the-quest-for-model-confidence-can-you-trust-a-black-box)

+   [缩小人类理解与机器学习之间的差距：…](https://www.kdnuggets.com/2023/06/closing-gap-human-understanding-machine-learning-explainable-ai-solution.html)
