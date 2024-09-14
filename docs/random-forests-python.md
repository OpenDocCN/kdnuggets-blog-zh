# 随机森林® 在 Python 中

> 原文：[https://www.kdnuggets.com/2016/12/random-forests-python.html](https://www.kdnuggets.com/2016/12/random-forests-python.html)

这篇文章最初发表于 [Yhat 博客](http://blog.yhat.com/)。[**Yhat**](https://www.yhat.com/) 是一家位于布鲁克林的公司，旨在使数据科学适用于开发人员、数据科学家和企业。Yhat 提供一个软件平台，用于将预测算法部署和管理为 REST API，同时消除与生产环境相关的痛苦工程障碍，如测试、版本控制、扩展和安全。

* * *

[随机森林](https://en.wikipedia.org/wiki/Random_forest "random forest - wikipedia") 是一种高度多用途的机器学习方法，应用广泛，从市场营销到医疗保健和保险。它可以用于[建模市场营销的影响](http://epubl.ltu.se/1653-0187/2008/014/LTU-PB-EX-08014-SE.pdf "Response Rate Modeling - a Data Mining Based Approach for Target Selection")，如客户获取、留存和流失，或用于[预测疾病风险和易感性](http://www.biomedcentral.com/1472-6947/11/51 "Predicting disease risks from highly imbalanced data using random forest")。

随机森林可以进行回归和分类。它能处理大量特征，并且有助于估计在建模的基础数据中哪些变量是重要的。

这是一个关于使用 Python 的随机森林的帖子。

### 什么是随机森林？

随机森林是几乎任何预测问题的可靠选择（即使是非线性问题）。这是一种相对较新的机器学习策略（它起源于 90 年代的贝尔实验室），可以用于几乎任何用途。它属于一种称为集成方法的更大类的机器学习算法。

**集成学习**

[集成学习](https://en.wikipedia.org/wiki/Ensemble_learning)涉及将多个模型组合在一起解决单一预测问题。它通过生成多个分类器/模型来工作，这些模型独立学习并进行预测。然后将这些预测组合成一个单一的（超级）预测，这个预测应该与任何一个分类器的预测一样好或更好。

随机森林是集成学习的一种，它依赖于决策树的集成。有关 Python 中集成学习的更多信息，请参见：[Scikit-Learn 文档](http://scikit-learn.org/dev/modules/ensemble.html)。

**随机化决策树**

所以我们知道随机森林是其他模型的聚合，但它聚合了哪些类型的模型呢？正如你可能从名字中猜到的那样，随机森林聚合了[分类（或回归）树](https://en.wikipedia.org/wiki/Decision_tree_learning "decision tree learning - wikipedia")。决策树由一系列决策组成，这些决策可以用来对数据集中一个观察值进行分类。

***随机* 森林**

用于生成随机森林的算法会自动创建一堆随机决策树。由于这些树是随机生成的，大多数树对你的分类/回归问题并没有什么有意义的帮助（可能有99.9%的树）。

![](../Images/8b2ffa2eb8e26a731d7ca9f9fbd27da6.png)

如果一个观测值的长度为45，蓝色眼睛，2 条腿，它将被分类为 **红色**。

**树木投票**

那么，10000 个（可能）糟糕的模型有什么用呢？结果证明，它们真的不是很有帮助。但 *有帮助的* 是那些你同时生成的少数几个非常好的决策树。

当你做出预测时，新的观测值会被推入每棵决策树，并被分配一个预测值/标签。一旦森林中的每棵树都报告了其预测值/标签，预测结果将被汇总，并以所有树的模式投票作为最终预测。

简单来说，那99.9% 的无关树做出的预测到处都是，并相互抵消。而少数几棵好的树的预测超越了这些噪音，得出了好的预测结果。

![](../Images/1089e8b31317d5481c2c9f906914a3d4.png)

### 为什么你应该使用它？

**很简单**

随机森林是 [Leatherman](http://www.leatherman.com/product/Super_Tool_300 "leatherman super tool") 的学习方法。你几乎可以对它进行任何操作，它都会做得很好。它特别擅长估计推断转换，因此不像 SVM 那样需要大量调优（即适合于时间紧迫的人）。

**一个示例转换**

随机森林能够在没有精心设计的数据转换的情况下进行学习。例如，考虑 `f(x) = log(x)` 函数。

好的，我们来写一些代码。我们将在 Yhat 自家的交互式环境 Rodeo 中编写 Python 代码。你可以在 [这里](https://www.yhat.com/products/rodeo) 下载适用于 Mac、Windows 或 Linux 的 Rodeo。

首先，创建一些虚假数据并添加一点噪声。

```py
import numpy as np
import pylab as pl

x = np.random.uniform(1, 100, 1000)
y = np.log(x) + np.random.normal(0, .3, 1000)

pl.scatter(x, y, s=1, label="log(x) with noise")
pl.plot(np.arange(1, 100), np.log(np.arange(1, 100)), c="b", label="log(x) true function")
pl.xlabel("x")
pl.ylabel("f(x) = log(x)")
pl.legend(loc="best")
pl.title("A Basic Log Function")
pl.show()
```

[在这里查看要点](https://gist.github.com/glamp/5716253)

我们将在 Rodeo 中进行 Python 分析和可视化，Yhat 是一个开源的数据科学环境。想要跟随吗？你可以在 [这里](https://www.yhat.com/products/rodeo) 下载适用于 Mac、Windows 或 Linux 的 Yhat Python IDE。

在 Rodeo 中跟随？这是你应该看到的内容。

[![](../Images/60c88f42a3be91a24868ac8a456551d6.png)](http://blog.yhat.com/static/img/random-forest-1.png)

让我们更详细地查看一下那个图表。

![](../Images/229d178dff3a906a4900b8e77bfff9a3.png)

如果我们尝试建立一个基本的线性模型来预测`y`，使用`x`的话，我们最终会得到一条直线，这条直线大致上是将`log(x)`函数分开。相比之下，如果我们使用随机森林，它在近似`log(x)`曲线方面表现得更好，我们得到的结果看起来更像真实函数。![](../Images/ba71b34f9c50bf4ec9f6ab94eef89633.png)![](../Images/e0f2fd97697a9bdf57bfc60630e0d7aa.png)

你可以说随机森林对`log(x)`函数进行了些许过拟合。无论如何，我认为这很好地说明了随机森林不受线性约束的限制。

### 用途

**变量选择**

随机森林最好的使用案例之一是特征选择。尝试许多决策树变体的副产品之一是你可以检查哪些变量在每棵树中表现最好/最差。

当某棵树使用一个变量而另一棵树不使用时，你可以比较因包括/排除该变量而丢失或获得的价值。好的随机森林实现会为你完成这项工作，所以你只需知道查看哪种方法或变量即可。

在接下来的例子中，我们试图找出哪些变量对于将酒分类为红酒或白酒最为重要。

![](../Images/35593bee262ea31d7ff57b2424fa7bfd.png)

![](../Images/1dc88ad49fcdbdee484b7eeba6868983.png)

**分类**

随机森林在分类方面也表现出色。它可以用于预测具有多个可能值的类别，并且可以校准以输出概率。然而，你需要注意的是[过拟合](https://en.wikipedia.org/wiki/Overfitting)。随机森林容易出现过拟合，尤其是在处理相对较小的数据集时。如果你的模型在测试集上的预测“过于准确”，你应该保持怀疑态度。

避免过拟合的一种方法是只在模型中使用真正相关的特征。虽然这并不总是简单明了，但使用特征选择技术（如前面提到的那种）可以使这变得容易得多。

![](../Images/51000c974461f8fb2e93691cf81fc5f4.png)

**回归**

是的，它也做回归。

我发现与其他算法不同，随机森林在学习分类变量或分类变量与实际变量混合时表现非常好。具有高基数（可能值数量）的分类变量可能会很棘手，因此拥有类似的工具在你手头会非常有用。

### 一个简短的 Python 示例

Scikit-Learn 是入门随机森林的绝佳选择。scikit-learn API 在各种算法中非常一致，因此你可以非常容易地进行模型比较和切换。很多时候我从简单的模型开始，然后转向随机森林。

scikit-learn中随机森林实现的最佳特性之一是`n_jobs`参数。这将根据你希望使用的核心数量自动并行化随机森林的拟合。[这是一个很棒的演示](https://vimeo.com/63269736 "Vimeo - Scaling Machine Learning in Python")，由scikit-learn贡献者Olivier Grisel介绍，他讲述了如何在20节点EC2集群上训练随机森林。

```py
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier
import pandas as pd
import numpy as np

iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df['is_train'] = np.random.uniform(0, 1, len(df)) <= .75
df['species'] = pd.Categorical.from_codes(iris.target, iris.target_names)
df.head()

train, test = df[df['is_train']==True], df[df['is_train']==False]

features = df.columns[:4]
clf = RandomForestClassifier(n_jobs=2)
y, _ = pd.factorize(train['species'])
clf.fit(train[features], y)

preds = iris.target_names[clf.predict(test[features])]
pd.crosstab(test['species'], preds, rownames=['actual'], colnames=['preds'])
```

跟上进度了吗？这是你应该看到的（大致）。我们使用的是*随机*选择的数据，因此你的确切值每次可能会有所不同。

| preds | sertosa | versicolor | virginica |
| --- | --- | --- | --- |
| actual |  |  |  |
| sertosa | 6 | 0 | 0 |
| versicolor | 0 | 16 | 1 |
| virginica | 0 | 0 | 12 |

[![](../Images/2a2215f7c85231cf599969e6d2a4400d.png)](http://blog.yhat.com/static/img/random-forest-3.png)

### 最终思考

随机森林尽管非常先进，但使用起来却异常简单。与任何建模一样，要小心过拟合。如果你对在`R`中使用随机森林感兴趣，可以查看[`randomForest`](http://cran.r-project.org/web/packages/randomForest/randomForest.pdf)包。

+   [伯克利资源](http://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm "random forests - UC Berkley Resources")

+   [Kaggle博客文章](https://www.kaggle.com/wiki/RandomForests "Random Forests Blog Post from Kaggle")

+   [Andy Mueller的博客（scikit-learn贡献者）](http://peekaboo-vision.blogspot.de/ "Andy Mueller's blog")

+   [随机森林指南](http://www.stanford.edu/~stephsus/R-randomforest-guide.pdf "Random Forests for Classification - Stephanie Shih")

+   [Olivier Grisel的网站](http://ogrisel.com/ ">Olivier Grisel")

[原文](http://blog.yhat.com/posts/python-random-forest.html)。经许可转载。

RANDOM FORESTS和RANDOMFORESTS是Minitab, LLC的注册商标。

**相关：**

+   [随机森林：犯罪教程](/2016/09/reandom-forest-criminal-tutorial.html)

+   [深度学习何时优于SVM或随机森林？](/2016/04/deep-learning-vs-svm-random-forest.html)

+   [伟大的算法教程汇总](/2016/09/great-algorithm-tutorial-roundup.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

### 更多相关内容

+   [集成学习技术：Python中的随机森林实战](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [决策树与随机森林，解析](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [使用网格搜索和随机搜索进行超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [随机森林与决策树：主要区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [随机森林算法是否需要标准化？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [使用 NumPy 生成随机数据](https://www.kdnuggets.com/generating-random-data-with-numpy)
