# 如何使用自动自助法计算机器学习中性能指标的置信区间

> 原文：[https://www.kdnuggets.com/2021/10/calculate-confidence-intervals-performance-metrics-machine-learning.html](https://www.kdnuggets.com/2021/10/calculate-confidence-intervals-performance-metrics-machine-learning.html)

[评论](#comments)

**[David B Rosen (PhD)](http://linkedin.com/in/rosen1)，IBM全球融资自动化信用审批的首席数据科学家**

![](../Images/c6c5f7248330c5ca16fac26349bbd195.png)

橙色线显示了平衡准确率置信区间的下限89.7%，绿色表示原始观测的平衡准确率=92.4%（点估计），红色表示上限94.7%。（除非另有说明，否则所有图像均由作者提供。）

## 介绍

如果你报告你的分类器在测试集上的表现为准确率=94.8%和F1=92.3%，在不了解测试集的大小和组成情况的情况下，这并不意味着什么。那些性能测量的误差范围会因测试集的大小而变化，或者对于不平衡的数据集，主要取决于其中包含多少个独立的*少数类*实例（重复抽样相同实例对这一目的没有帮助）。

如果你能够收集另一个独立来源的相似测试集，你模型在这个数据集上的准确率和F1分数可能不会相同，但它们可能会有多大的差异呢？类似的问题在统计学中被称为**置信区间**。

如果我们从基础总体中抽取许多独立的样本数据集，那么对于95%的这些数据集，该指标的真实基础总体值将位于我们为该特定样本数据集计算的95%置信区间内。

在本文中，我们将向你展示如何一次计算任意数量的机器学习性能指标的置信区间，使用一种*自动*确定默认生成多少自助样本数据集的方法。

如果你只是想查看如何调用这些代码来计算置信区间，请跳到下面的部分“**计算结果！**”。

## 自助法的方法论

如果我们能够从数据的真实分布中抽取额外的测试数据集，我们将能够看到这些数据集上感兴趣的性能指标的分布。（在抽取这些数据集时，我们不会采取任何措施来防止多次抽取相同或相似的实例，尽管这种情况可能只会偶尔发生。）

由于我们不能这样做，下一步最佳的做法是从这个测试数据集的*经验分布*中抽取额外的数据集，这意味着从其实例中进行带替换的抽样，以生成新的引导样本数据集。带替换的抽样意味着一旦我们抽取了某个特定实例，我们将其放回，以便可能再次抽取相同的数据集。因此，每个这样的数据集通常包含一些实例的多个副本，并且不包括基准测试集中所有的实例。

如果我们*不带*替换地抽样，那么每次都会得到一个与原始数据集完全相同的副本，只是顺序被打乱，这样的数据没有实际用途。

用于估计置信区间的*百分位*引导方法如下：

1.  生成`*nboots*`个“引导样本”数据集，每个数据集的大小与原始测试集相同。每个样本数据集通过从测试集中随机抽取实例（带替换）来获得。

1.  在每个样本数据集上，计算度量并保存结果。

1.  95%的置信区间由`*nboots*`计算的度量值中的2.5*百分位*到97.5*百分位*给出。如果`*nboots*`=1001，并且你对长度为1001的系列/数组/列表*X*中的值进行了排序，则0*百分位*为*X*[0]，100*百分位*为*X*[1000]，因此置信区间由*X*[25]到*X*[975]给出。

当然，你可以在第2步中计算每个样本数据集的任意数量的度量，但在第3步中，你需要分别查找每个度量的百分位数。

## 示例数据集和置信区间结果

我们将使用来自之前文章的结果作为示例：[**如何处理不平衡分类，而无需重新平衡数据**：*在考虑过采样你的偏斜数据之前，尝试调整你的分类决策阈值*](/2021/09/imbalanced-classification-without-re-balancing-data.html)*。

在那篇文章中，我们使用了*高度*不平衡的两类Kaggle [信用卡欺诈识别数据集](https://www.kaggle.com/mlg-ulb/creditcardfraud)。我们选择使用一个与predict()方法中隐含的默认0.5阈值相差甚远的分类阈值，这样就不需要平衡数据。这种方法有时称为*阈值移动*，在这种方法中，我们的分类器通过将所选阈值应用于predict**_proba**()方法提供的预测类概率来分配类别。

我们将把本文（和代码）的范围限制为二元分类：类别0和1，其中类别1按惯例为“正类”，特别是在不平衡数据中为少数类，尽管代码也应该适用于回归（单个连续目标）。

## 生成一个引导样本数据集

尽管我们的置信区间代码可以处理传递给度量函数的各种数据参数数量，我们将专注于sklearn风格的度量，这些度量始终接受两个数据参数，`y_true`和`y_pred`，其中`y_pred`可以是二进制类别预测（0或1），或者是连续类别概率或决策函数预测，甚至是连续回归预测（如果`y_true`也是连续的）。以下函数生成一个单一的引导样本数据集。它接受任何`data_args`，但在我们的案例中，这些参数将是`ytest`（我们实际的/真实的测试集目标值，见[前一篇文章](/2021/09/imbalanced-classification-without-re-balancing-data.html)）和`hardpredtst_tuned_thresh`（预测的类别）。这两个参数都包含零和一，用于指示每个实例的真实类别或预测类别。

## 自定义度量`specificity_score()`和实用函数

我们将定义一个针对特异性的自定义度量函数，这只是*负类*（类0）的召回率的另一种名称。同时，还定义一个`calc_metrics`函数，它将一系列感兴趣的度量应用于我们的数据，以及几个实用函数：

在这里我们列出了度量列表并将它们应用于数据。我们没有考虑准确率作为一个相关度量，因为假阴性（将真实的欺诈行为误判为合法）对业务的成本远高于假阳性（将真实的合法行为误判为欺诈），而准确率将这两种误分类视为同样糟糕，因此更倾向于正确分类那些真实类别是多数类的样本，因为这些样本出现得更频繁，从而对总体准确率的贡献更大。

```py
met=[ metrics.recall_score, specificity_score, 
      metrics.balanced_accuracy_score
    ]
calc_metrics(met, ytest, hardpredtst_tuned_thresh)
```

![](../Images/f3ca98e5c535788006b5ccfba07c388b.png)

## 制作每个引导样本数据集并计算其度量

在`raw_metric_samples()`中，我们将实际逐个生成多个样本数据集，并保存每个数据集的度量：

你给`raw_metric_samples()`提供一个度量列表（或仅一个度量）以及真实和预测类别数据，它会获取nboots个样本数据集，并返回一个仅包含从每个数据集中计算出的度量值的数据框。通过`_boot_generator()`，它一次调用一个`one_boot()`，而不是一次性存储所有数据集作为一个可能*巨大的*列表。

## 查看7个引导样本数据集上的度量

我们列出度量函数的列表，并调用`raw_metric_samples()`来获取仅针对7个样本数据集的结果。我们在这里调用`raw_metric_samples()`是为了理解——在使用下面的`ci_auto()`获取置信区间时并不是必须的，不过为`ci_auto()`指定一个度量列表（或仅一个度量）*是*必要的。

```py
np.random.seed(13)
raw_metric_samples(met, ytest, hardpredtst_tuned_thresh, 
          nboots=7).style.format('{:.2%}')  #optional #style
```

![](../Images/c846c20dfd1b1fbd5427e5982c455172.png)

上面的每一列包含从一个引导样本数据集（编号0到6）计算出的度量，因此计算出的度量值因随机抽样而异。

## 引导数据集的数量，默认为计算值

在我们的实现中，默认情况下，`nboots` 的数量将根据所需的置信水平（例如 95%）自动计算，以符合[North, Curtis, and Sham](https://www.google.com/search?q=Am+J+Hum+Genet.+2002+Aug%3B+71%282%29%3A+439%E2%80%93441.+doi%3A+10.1086%2F341527+A+Note+on+the+Calculation+of+Empirical+P+Values+from+Monte+Carlo+Procedures+B.+V.+North+D.+Curtis+P.+C.+Sham&btnI)的建议，即在分布的每个尾部具有最小数量的引导结果。（实际上，这项建议适用于*p*-值和假设检验的*接受区域*，但*置信区间*与这些相似，因此可以作为经验法则。）尽管这些作者建议尾部至少有 10 个引导结果，[*Davidson & MacKinnon*](http://hdl.handle.net/10419/67820) 建议 95% 置信度下至少使用 399 次引导，这需要尾部有 11 次引导，因此我们采用这一更保守的建议。

我们指定 alpha，即 1 减去置信水平。例如，95% 置信度变为 0.95，而 alpha=0.05。如果你指定了一个明确的引导次数（可能因为你想要更快的结果而选择较小的 `nboots`），但这对于你的请求的 alpha 不够，则会自动选择更高的 alpha，以便为该次数的引导获得准确的置信区间。将使用至少 51 次引导，因为任何更少的次数只能准确计算出异常小的置信水平（例如 40% 的置信度，其区间从第 30*百分位数* 到第 70*百分位数*，该区间包含 40% 的内容，但 60% 在区间外），并且不清楚最小引导次数的建议是否考虑了这种情况。

函数 get_alpha_nboots() 设置默认 nboots 或根据上述内容修改请求的 alpha 和 nboots：

让我们展示不同 alpha 值下的默认 nboots：

```py
g = get_alpha_nboots 
pd.DataFrame( [ g(0.40), g(0.20, None), g(0.10), g(), g(alpha=0.02), 
                g(alpha=0.01, nboots=None), g(0.005, nboots=None)
              ], columns=['alpha', 'default nboots']
            ).set_index('alpha')
```

![](../Images/7cbe0c18e8b8170e1285b23a43dfeb0a.png)

如果我们请求一个明确的 nboots，会发生什么情况：

```py
req=[(0.01,3000), (0.01,401), (0.01,2)]
out=[get_alpha_nboots(*args) for args in req]
mydf = lambda x: pd.DataFrame(x, columns=['alpha', 'nboots'])
pd.concat([mydf(req),mydf(out)],axis=1, keys=('Requested','Using'))
```

![](../Images/7a2070ccbc3e6f4df16f4b2ea3248dc6.png)

较小的 nboots 值将 alpha 提高到 0.05 和 0.40，而 nboots=2 将更改为最小值 51。

## 引导样本数据集的直方图显示仅针对平衡准确率的置信区间

再次说明，我们不需要通过调用 ci_auto() 来获取下文的置信区间。

```py
np.random.seed(13)
metric_boot_histogram\
  (metrics.balanced_accuracy_score, ytest, hardpredtst_tuned_thresh)
```

![](../Images/28956ddfbee463dff63b60b602fb2671.png)

橙色线条显示 89.7% 作为平衡准确率置信区间的下界，绿色表示原始观察到的平衡准确率=92.4%（点估计），红色表示上界为 94.7%。（相同的图像出现在本文的顶部。）

## 如何计算所有度量指标的置信区间

这是调用上述内容并从度量结果的百分位数计算置信区间的主要函数，并将点估计插入其输出结果数据框的第一列。

## 计算结果！

这就是我们真正需要做的：调用 ci_auto()，如下所示，带上一个指标列表（`met` 上述已分配）以获取它们的置信区间。百分比格式是可选的：

```py
np.random.seed(13)
ci_auto( met, ytest, hardpredtst_tuned_thresh
       ).style.format('{:.2%}')
```

![](../Images/c939ced030c244587387abaa1149ead5.png)

## 结果置信区间的讨论

这是来自[原始文章](/2021/09/imbalanced-classification-without-re-balancing-data.html)的混淆矩阵。类别0是负类（多数类），类别1是正类（非常稀有的类别）。

![](../Images/0e24bf1a73e647c875c0149f33d4e377.png)

134/(134+14) 的召回率（真正率）具有最宽的置信区间，因为这是涉及小样本的二项分布比例。

特异性（真负率）为80,388/(80,388+4,907)，涉及*much*更大的样本，因此它具有极其狭窄的置信区间，仅为[94.11%到94.40%]。

由于平衡准确率仅计算为召回率和特异性的平均值，因此它的置信区间宽度介于两者之间。

## 测量指标的不精确性由于测试数据的变化，与训练数据的变化

在这里，我们没有考虑基于我们*训练*数据随机性的*模型*的变异性（尽管这对于某些目的也可能有兴趣，例如如果你有自动重复训练并希望了解未来模型表现可能的变化），而只是考虑了由于我们*测试*数据的随机性造成的这个*特定*模型（从某些特定训练数据创建）的表现测量的变异性。

如果我们有足够的独立测试数据，我们可以非常精确地测量该特定模型在基础总体上的表现，我们将知道如果部署该模型，它的表现如何，不管我们是如何构建模型的，也不管我们是否可以通过不同的训练样本数据集获得更好或更差的模型。

## 个体实例的独立性

自助法假设每个实例（案件，观察值）是从基础总体中独立抽取的。如果你的测试集有不独立的行组，例如相同实体的重复观察可能彼此相关，或测试集中实例被过度抽样/复制/从其他实例生成的结果可能无效。你可能需要使用*分组*抽样，在随机抽取整个组而不是单独的行时，避免拆分任何组或仅使用部分组。

你还要确保没有将分组数据分割到训练集和测试集中，因为这样测试集可能不独立，你可能会得到未被检测到的过拟合。例如，如果使用过采样，通常应**在**从测试集中分离之后进行，而不是之前。通常你会对训练集进行过采样，但不对测试集，因为测试集必须保持对模型未来部署时会看到的实例具有代表性。对于交叉验证，你可以使用 scikit-learn 的 `model_selection.GroupKFold()`。

## 结论

你可以随时计算评估指标的置信区间，以查看测试数据如何准确地衡量模型的表现。我计划撰写另一篇文章，展示用于评估概率预测（或置信度评分——与统计置信度无关）的指标的置信区间，即软分类，如 Log Loss 或 ROC AUC，而不是我们这里使用的评估模型离散类别选择（硬分类）的指标。相同的代码适用于这两种情况，也适用于回归（预测连续目标变量）——你只需传入不同类型的预测（以及回归情况下不同类型的真实目标）。

*此 Jupyter notebook 可在 GitHub 上获取：*[*bootConfIntAutoV1o_standalone.ipynb*](https://github.com/DavidRosen/conf-intervals-auto/blob/main/bootConfIntAutoV1o_standalone.ipynb)

**这篇文章对你有帮助吗？如果你对这篇文章或关于置信区间、自助法、引导数、实现方式、数据集、模型、阈值移动或结果有任何评论或问题，请在下面留言。**

除了上述的 [先前文章](https://towardsdatascience.com/how-to-deal-with-imbalanced-classification-without-re-balancing-the-data-8a3c02353fe3)，你可能还对我的 [**如何在 Pandas 中自动检测日期/时间列并在读取 CSV 文件时设置其数据类型**](https://towardsdatascience.com/auto-detect-and-set-the-date-datetime-datatypes-when-reading-csv-into-pandas-261746095361) 感兴趣，尽管它与当前文章没有直接关系。

[保留一些权利](https://creativecommons.org/licenses/by-sa/4.0/)

**简介：[David B Rosen (博士)](http://linkedin.com/in/rosen1)** 是 IBM Global Financing 自动化信用审批的首席数据科学家。更多 David 的文章可以在 [dabruro.medium.com](https://dabruro.medium.com/) 找到。

[原文](https://towardsdatascience.com/get-confidence-intervals-for-any-model-performance-metrics-in-machine-learning-f9e72a3becb2?source=user_profile---------0----------------------------)。经许可转载。

**相关：**

+   [如何在 Python 中进行“无限”数学运算](/2021/10/limitless-math-python.html)

+   [数据科学中的高级统计概念](/2021/09/advanced-statistical-concepts-data-science.html)

+   [如何使用 Python 确定最佳拟合数据分布](/2021/09/determine-best-fitting-data-distribution-python.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求

* * *

### 更多相关话题

+   [处理置信区间](https://www.kdnuggets.com/2023/04/working-confidence-intervals.html)

+   [如何通过使用自动 EDA 工具来应对数据科学评估测试](https://www.kdnuggets.com/2022/04/ace-data-science-assessment-test-automatic-eda-tools.html)

+   [如何使用 Grounding DINO 自动标记图像](https://www.kdnuggets.com/2023/05/automatic-image-labeling-grounding-dino.html)

+   [使用 Pandas Dataframes 的 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [模型信心的探索：你能信任黑箱吗？](https://www.kdnuggets.com/the-quest-for-model-confidence-can-you-trust-a-black-box)

+   [更多分类问题的性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)
