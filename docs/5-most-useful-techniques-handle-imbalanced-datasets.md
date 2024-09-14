# 处理不平衡数据集的5种最有用的技术

> 原文：[https://www.kdnuggets.com/2020/01/5-most-useful-techniques-handle-imbalanced-datasets.html](https://www.kdnuggets.com/2020/01/5-most-useful-techniques-handle-imbalanced-datasets.html)

[评论](#comments)

你是否曾遇到过这样的问题，即在数据集中正类样本非常少，模型无法学习？

***在这种情况下，你只需预测多数类就能获得相当高的准确率，但你未能捕捉到少数类，而这往往是创建模型的初衷。***

这种数据集是一种相当常见的现象，称为不平衡数据集。

> 不平衡数据集是分类问题中的一个特殊情况，其中类别分布在各个类别之间不均匀。通常，它们由两个类别组成：多数（负类）和少数（正类）。

不平衡数据集可以在各种领域的不同用例中找到：

+   **金融**：欺诈检测数据集中通常欺诈率为 ~1–2%

+   **广告投放**：点击预测数据集的点击率也不高。

+   **交通**/**航空**：飞机故障是否会发生？

+   **医学**：患者是否患有癌症？

+   **内容审查**：帖子是否包含不适内容？

那么我们如何解决这些问题呢？

***这篇文章讲解了你可以用来处理不平衡数据集的各种技术。***

### 1\. 随机下采样和上采样

![图](../Images/dc5e318bc4926bf091e11d5b4591bf5b.png)

[来源](https://www.kaggle.com/rafjaa/resampling-strategies-for-imbalanced-datasets#t1)

一种广泛采用且可能最直接的方法来处理高度不平衡的数据集称为重采样。它包括从多数类中删除样本（下采样）和/或从少数类中添加更多样本（上采样）。

首先，我们创建一些示例不平衡数据。

```py
from sklearn.datasets import make_classificationX, y = make_classification(
    n_classes=2, class_sep=1.5, weights=[0.9, 0.1],
    n_informative=3, n_redundant=1, flip_y=0,
    n_features=20, n_clusters_per_class=1,
    n_samples=100, random_state=10
)X = pd.DataFrame(X)
X['target'] = y
```

我们现在可以使用以下方法进行随机上采样和下采样：

```py
num_0 = len(X[X['target']==0])
num_1 = len(X[X['target']==1])
print(num_0,num_1)# random undersampleundersampled_data = pd.concat([ X[X['target']==0].sample(num_1) , X[X['target']==1] ])
print(len(undersampled_data))# random oversampleoversampled_data = pd.concat([ X[X['target']==0] , X[X['target']==1].sample(num_0, replace=True) ])
print(len(oversampled_data))------------------------------------------------------------
OUTPUT:
90 10
20
180
```

### 2\. 使用 imbalanced-learn 进行下采样和上采样

imbalanced-learn(`imblearn`) 是一个用于解决不平衡数据集问题的Python包。

它提供了多种下采样和上采样的方法。

### a. 使用Tomek链接进行下采样：

其中一种方法称为Tomek链接。Tomek链接是相邻的相反类别样本对。

在此算法中，我们最终会移除Tomek链接中的多数类元素，这为分类器提供了更好的决策边界。

![图](../Images/b438cc09ae1ce6d6be99d948a836939b.png)

[来源](https://www.kaggle.com/rafjaa/resampling-strategies-for-imbalanced-datasets#t1)

```py
from imblearn.under_sampling import TomekLinkstl = TomekLinks(return_indices=True, ratio='majority')X_tl, y_tl, id_tl = tl.fit_sample(X, y)
```

### b. 使用SMOTE进行上采样：

在SMOTE（合成少数类过采样技术）中，我们在已有样本的附近合成少数类元素。

![图](../Images/d989e541f778e055e91e7a284125f32f.png)

[来源](https://www.kaggle.com/rafjaa/resampling-strategies-for-imbalanced-datasets#t1)

```py
from imblearn.over_sampling import SMOTEsmote = SMOTE(ratio='minority')X_sm, y_sm = smote.fit_sample(X, y)
```

在`[imblearn](https://github.com/scikit-learn-contrib/imbalanced-learn#id3)`包中，有多种其他方法用于欠采样（Cluster Centroids, NearMiss等）和过采样（ADASYN和bSMOTE），你可以查看。

### 3\. 模型中的类别权重

![](../Images/4b2022f2f043a02a1b1a24858f55459d.png)

大多数机器学习模型提供了一个名为`class_weights`的参数。例如，在随机森林分类器中使用`class_weights`，我们可以通过字典为少数类指定更高的权重。

```py
from sklearn.linear_model import LogisticRegression*clf =* LogisticRegression(*class_weight={0:1,1:10}*)
```

***但后台究竟发生了什么？***

在逻辑回归中，我们使用二元交叉熵计算每个样本的损失：

```py
Loss = −*y*log(*p)* − (1−*y*)log(1−*p*)
```

在这种特定形式下，我们对正类和负类赋予了相等的权重。当我们将class_weight设置为`class_weight = {0:1,1:20}`时，后台的分类器尝试最小化：

```py
NewLoss = −20**y*log(*p)* − *1**(1−*y*)log(1−*p*)
```

***那么这里究竟发生了什么？***

+   如果我们的模型给出的概率是0.3，而我们错误分类了一个正例，那么NewLoss的值是-20log(0.3) = 10.45。

+   如果我们的模型给出的概率是0.7，而我们错误分类了一个负例，那么NewLoss的值是-log(0.3) = 0.52。

这意味着当我们的模型错误分类一个正例少数样本时，我们会惩罚模型约二十倍。

***我们如何计算类别权重？***

*没有一种方法可以做到这一点，这应该被构建为针对你特定问题的超参数搜索问题。*

但如果你想使用y变量的分布来获取类别权重，你可以使用`sklearn`中的以下实用工具。

```py
from sklearn.utils.class_weight import compute_class_weightclass_weights = compute_class_weight('balanced', np.unique(y), y)
```

### 4\. 更改你的评估指标

![](../Images/4b467aa75f4bcb6e3e4cbe7e352cb9a0.png)

选择正确的评估指标在处理不平衡数据集时非常重要。通常情况下，在这种情况下，我希望使用F1分数作为我的[***评估指标***](https://towardsdatascience.com/the-5-classification-evaluation-metrics-you-must-know-aa97784ff226)。

F1分数是介于0和1之间的一个数字，是精度和召回率的调和均值。

![](../Images/bd4bdbc8f8d459b73ed3c03de0723404.png)

***那么这有什么帮助呢？***

让我们从一个二分类预测问题开始。***我们正在预测小行星是否会撞击地球。***

因此，我们创建了一个对整个训练集预测“否”的模型。

***准确率（通常是最常用的评估指标）是多少？***

它超过99%，所以根据准确率，这个模型相当好，但实际上毫无价值。

***那么，F1分数是多少？***

我们在这里的精度是0。我们正类的召回率是多少？它是零。因此，F1分数也是0。

因此，我们得知一个准确率为99%的分类器在我们的案例中毫无价值。这样它解决了我们的问题。

![图示](../Images/f7ba297265c5c7338be06b63dc805cc4.png)

精度-召回权衡

简单来说，***F1分数在分类器的精准度和召回率之间保持了一种平衡***。如果你的精准度低，F1分数也低；如果召回率低，F1分数也低。

> 如果你是一个警察检查员，你想确保你抓到的人是罪犯（精准度），同时你还希望抓到尽可能多的罪犯（召回率）。F1分数管理了这种权衡。

### 如何使用？

你可以使用以下公式计算二元预测问题的F1分数：

```py
from sklearn.metrics import f1_score
y_true = [0, 1, 1, 0, 1, 1]
y_pred = [0, 0, 1, 0, 0, 1]
*f1_score(y_true, y_pred)*
```

这是我用来获得最佳阈值以最大化二元预测F1分数的一个函数。下面的函数会遍历可能的阈值以找到最佳F1分数。

```py
# y_pred is an array of predictions
def bestThresshold(y_true,y_pred):
    best_thresh = None
    best_score = 0
    for thresh in np.arange(0.1, 0.501, 0.01):
        score = f1_score(y_true, np.array(y_pred)>thresh)
        if score > best_score:
            best_thresh = thresh
            best_score = score
    return best_score , best_thresh
```

### 5\. 杂项

![图示](../Images/34b289f50ee5da5e6ff32c72026a4019.png)

尝试新的事物和探索新的领域

根据你的使用案例和你要解决的问题，可能会有其他各种方法有效：

### a) 收集更多数据

如果可能的话，这是一件你应该尝试的事情。获取更多数据和更多的正例将帮助你的模型获得对多数类和少数类的更广泛的视角。

### b) 将问题视为异常检测

你可能想把你的分类问题当作异常检测问题来处理。

**异常检测** 是识别那些与大多数数据显著不同的稀有项、事件或观察的过程，这些差异会引起怀疑。

你可以使用隔离森林或自编码器进行异常检测。

### c) 基于模型的方法

一些模型特别适合处理不平衡数据集。

例如，在提升模型中，我们在每次树迭代中对被错误分类的案例给予更多的权重。

### 结论

在处理不平衡数据集时，没有一种通用的解决方案。你需要根据你的问题尝试多种方法。

在这篇文章中，我谈到了每当遇到这种问题时，我脑海中浮现的常见问题。

建议是尝试上述所有方法，并看看哪种方法最适合你的使用案例。

如果你想要了解更多关于不平衡数据集及其带来的问题，可以查看这个由 Andrew Ng 提供的[***优秀课程***](https://www.coursera.org/learn/machine-learning?ranMID=40328&ranEAID=lVarvwc5BD0&ranSiteID=lVarvwc5BD0-btd7XBdF681VKxRe2H_Oyg&siteID=lVarvwc5BD0-btd7XBdF681VKxRe2H_Oyg&utm_content=2&utm_medium=partners&utm_source=linkshare&utm_campaign=lVarvwc5BD0&source=post_page---------------------------)。这是让我入门的课程。请务必查看一下。

感谢阅读。我未来也会写更多适合初学者的文章。关注我的 [**Medium**](https://medium.com/@rahul_agarwal?source=post_page---------------------------) 或订阅我的 [**博客**](http://eepurl.com/dbQnuX?source=post_page---------------------------)以获取相关信息。如往常一样，我欢迎反馈和建设性的批评，可以通过 Twitter [@mlwhiz](https://twitter.com/MLWhiz?source=post_page---------------------------) 联系我。

另外，一个小声明——这篇文章中可能包含一些关联链接，分享知识从来不是坏主意。

**简介: [Rahul Agarwal](https://www.linkedin.com/in/rahulagwl/)** 是 WalmartLabs 的高级统计分析师。关注他的 Twitter [@mlwhiz](https://twitter.com/MLWhiz)。

[原文](https://towardsdatascience.com/the-5-most-useful-techniques-to-handle-imbalanced-datasets-6cdba096d55a)。已获得许可转载。

**相关:**

+   [使用5种机器学习算法分类稀有事件](/2020/01/classify-rare-event-machine-learning-algorithms.html)

+   [每个数据科学家必须知道的5种分类评估指标](/2019/10/5-classification-evaluation-metrics-every-data-scientist-must-know.html)

+   [每个数据科学家需要知道的5种采样算法](/2019/09/5-sampling-algorithms.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关内容

+   [处理不平衡数据的7种技术](https://www.kdnuggets.com/2017/06/7-techniques-handle-imbalanced-data.html)

+   [KDnuggets 新闻，8 月 31 日: 完整的数据科学学习路线图…](https://www.kdnuggets.com/2022/n35.html)

+   [无监督解缠表示学习在类别不平衡数据集中的应用](https://www.kdnuggets.com/2023/01/unsupervised-disentangled-representation-learning-class-imbalanced-dataset-elastic-infogan.html)

+   [克服实际场景中的数据不平衡挑战](https://www.kdnuggets.com/2023/07/overcoming-imbalanced-data-challenges-realworld-scenarios.html)

+   [数据科学中4个有用的中级 SQL 查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)
