# 训练 sklearn 快速 100 倍

> 原文：[https://www.kdnuggets.com/2019/09/train-sklearn-100x-faster.html](https://www.kdnuggets.com/2019/09/train-sklearn-100x-faster.html)

[评论](#comments)

**作者：[Evan Harris](https://www.linkedin.com/in/evan-harris-387375b2/)，Ibotta, Inc. 机器学习与数据科学经理**

在Ibotta，我们训练了大量的机器学习模型。这些模型驱动了我们的推荐系统、搜索引擎、定价优化引擎、数据质量等。它们为数百万用户提供预测，用户在使用我们的移动应用时会与这些模型互动。

虽然我们用[Spark](https://spark.apache.org/)处理了很多数据，但我们首选的机器学习框架是[scikit-learn](https://scikit-learn.org/stable/)。随着计算成本的降低以及机器学习解决方案的市场时间变得越来越关键，我们探索了加速模型训练的选项。解决方案之一是将Spark和scikit-learn的元素结合成我们自己的混合解决方案。

### 介绍 sk-dist

我们很高兴地宣布我们的开源项目[sk-dist](https://github.com/Ibotta/sk-dist)的上线。该项目的目标是提供一个通用框架，以便在Spark中分发scikit-learn元估计器。元估计器的例子包括决策树集成（[随机森林](https://en.wikipedia.org/wiki/Random_forest) 和 [额外随机树](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.ExtraTreesClassifier.html)）、[超参数调优器](https://scikit-learn.org/stable/modules/grid_search.html)（[网格搜索](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html#sklearn.model_selection.GridSearchCV) 和 [随机搜索](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html#sklearn.model_selection.RandomizedSearchCV)）以及多类技术（[一对其余](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsRestClassifier.html#sklearn.multiclass.OneVsRestClassifier) 和 [一对一](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsOneClassifier.html#sklearn.multiclass.OneVsOneClassifier)）。

![](../Images/4df608e27f8783fb241b1762eed6f489.png)

我们的主要动机是填补传统机器学习模型分发选项中的空白。在神经网络和深度学习的领域之外，我们发现，训练模型的大部分计算时间并不是用来在单一数据集上训练单一模型。大部分时间用于在多个数据集的多个迭代上训练多个模型，使用像网格搜索或集成这样的元估计器。

### 示例

考虑一下 [手写数字数据集](https://scikit-learn.org/stable/auto_examples/classification/plot_digits_classification.html)。在这里，我们已经对手写数字图像进行了编码，以便进行适当分类。我们可以在单台机器上非常快速地训练一个 [支持向量机](https://en.wikipedia.org/wiki/Support-vector_machine) 处理 1797 条记录的数据集。这只需不到一秒钟。但超参数调整需要在不同的训练数据子集上进行多个训练作业。

如下所示，我们构建了一个总共需要 1050 个训练作业的参数网格。在具有超过一百个核心的 Spark 集群上使用 sk-dist 只需 3.4 秒。此作业的总任务时间为 7.2 分钟，这意味着在没有并行化的情况下，单台机器上训练将需要这么长时间。

```py
import timefrom sklearn import datasets, svm
from skdist.distribute.search import DistGridSearchCV
from pyspark.sql import SparkSession # instantiate spark session
spark = (   
    SparkSession    
    .builder    
    .getOrCreate()    
    )
sc = spark.sparkContext # the digits dataset
digits = datasets.load_digits()
X = digits["data"]
y = digits["target"] # create a classifier: a support vector classifier
classifier = svm.SVC()
param_grid = {
    "C": [0.01, 0.01, 0.1, 1.0, 10.0, 20.0, 50.0], 
    "gamma": ["scale", "auto", 0.001, 0.01, 0.1], 
    "kernel": ["rbf", "poly", "sigmoid"]
    }
scoring = "f1_weighted"
cv = 10# hyperparameter optimization
start = time.time()
model = DistGridSearchCV(    
    classifier, param_grid,     
    sc=sc, cv=cv, scoring=scoring,
    verbose=True    
    )
model.fit(X,y)
print("Train time: {0}".format(time.time() - start))
print("Best score: {0}".format(model.best_score_))------------------------------
Spark context found; running with spark
Fitting 10 folds for each of 105 candidates, totalling 1050 fits
Train time: 3.380601406097412
Best score: 0.981450024203508
```

这个例子说明了一个常见的场景，其中将数据适配到内存并训练单个分类器是微不足道的，但超参数调整所需的拟合次数很快就会增加。以下是使用 sk-dist 运行如上例的网格搜索问题的幕后分析：

![图](../Images/282b2a6db0d1e7459055893272e39ba7.png)

使用 sk-dist 的网格搜索

在 Ibotta 的传统机器学习实际应用中，我们经常发现自己处于类似的情况：小到中等规模的数据（10 万到 100 万条记录），需要多次迭代简单的分类器以进行超参数调整、集成和多类解决方案。

### 现有解决方案

目前有一些解决方案用于分布式传统机器学习元估计器训练。第一个是最简单的：scikit-learn 内置的使用 [joblib](https://joblib.readthedocs.io/en/latest/) 的元估计器并行化。这与 sk-dist 的操作非常相似，除了一个主要限制：性能受限于任何一台机器的资源。即使与理论上拥有数百个核心的单台机器相比，Spark 仍具有诸如 [针对执行器的精细调优内存规范](https://spark.apache.org/docs/latest/tuning.html)、[容错](https://techvidvan.com/tutorials/fault-tolerance-in-spark/) 以及像使用 [抢占实例](https://aws.amazon.com/ec2/spot/) 作为工作节点的成本控制选项等优势。

另一种现有解决方案是 [Spark ML](https://spark.apache.org/docs/latest/ml-guide.html)。这是 Spark 的原生机器学习库，支持许多与 scikit-learn 相同的分类和回归算法。它还具有像树集成和网格搜索这样的元估计器，并支持多类问题。虽然这听起来可能是一个足以分布式处理 scikit-learn 风格机器学习负载的解决方案，但它的分布式训练方式并未解决我们关心的并行性问题。

![图](../Images/3fcf7a970a8832f2998bd8ca037f6870.png)

在不同维度上的分布式

如上所示，Spark ML 会在分布在多个执行器上的数据上训练一个单一模型。当数据量很大且无法适应单台机器的内存时，这种方法效果很好。然而，当数据量较小时，这可能会导致 scikit-learn 在单台机器上表现*不如预期*。此外，例如，在训练随机森林时，Spark ML 会按顺序训练每棵决策树。无论为此任务分配了多少资源，其壁垒时间将随着决策树数量的增加而线性增长。

对于[网格搜索，Spark ML](http://spark.apache.org/docs/latest/api/python/pyspark.ml.html#pyspark.ml.tuning.CrossValidator)确实实现了一个[并行](http://spark.apache.org/docs/latest/api/python/pyspark.ml.html#pyspark.ml.tuning.CrossValidator.parallelism)参数，该参数可以[并行训练单独模型](https://issues.apache.org/jira/browse/SPARK-19357)。然而，每个单独的模型仍然是在分布在执行器上的数据上进行训练的。如果纯粹沿模型维度而非数据维度进行分布，该任务的总并行度是[可以有的一个小数](https://github.com/Ibotta/sk-dist/blob/master/examples/search/spark_ml.py)。

最终，我们希望在不同于 Spark ML 的维度上分布我们的训练。当使用小型或中型数据时，将数据适配到内存中并不是问题。以随机森林为例，我们希望将完整的训练数据广播到每个执行器，分别在每个执行器上拟合一个独立的决策树，然后将这些拟合的决策树带回驱动程序以组装随机森林。沿此维度分布可以比将数据分布和串行训练决策树快[数量级](https://github.com/Ibotta/sk-dist/blob/master/examples/search/spark_ml.py)得多。其他元估计技术，如网格搜索和多类别，也有类似的行为。

### 特性

鉴于这些现有解决方案在我们问题空间中的局限性，我们决定内部开发 sk-dist。底线是我们想要**分布模型，而不是数据**。

虽然主要关注的是元估计器的分布式训练，但 sk-dist 还包括用于与 Spark 进行 scikit-learn 模型分布式预测的模块、用于不使用 Spark 的多个前处理/后处理 scikit-learn 转换器以及一个灵活的特征编码器，可在有或没有 Spark 的情况下使用。

+   **分布式训练** — 使用 Spark 分发元估计器训练。支持以下算法：[超参数优化](https://github.com/Ibotta/sk-dist/blob/master/skdist/distribute/search.py)（包括网格搜索和随机搜索）、[树集成](https://github.com/Ibotta/sk-dist/blob/master/skdist/distribute/ensemble.py)（包括随机森林、额外树和随机树嵌入）以及[多类别策略](https://github.com/Ibotta/sk-dist/blob/master/skdist/distribute/multiclass.py)（包括一对多和一对一）。所有这些元估计器在拟合后都与其 scikit-learn 对应物一致。

+   **分布式预测** — 使用[Spark DataFrames](https://spark.apache.org/docs/latest/sql-programming-guide.html)分发拟合的 scikit-learn 估计器的预测方法。这样可以实现大规模的分布式预测，使用便携的 scikit-learn 估计器，可以选择是否使用 Spark。

+   **特征编码** — 使用一个名为[Encoderizer](https://github.com/Ibotta/sk-dist/blob/master/skdist/distribute/encoder.py)的灵活特征变换器来分发特征编码。它可以在有无 Spark 并行化的情况下运行。它会推断数据类型和形状，自动应用默认的特征变换器，以最佳猜测实现标准特征编码技术。它还可以作为一个完全可定制的特征联合编码器使用，具有 Spark 分布式变换器拟合的附加优势。

### 用例

这里是一些指导方针，用于决定 sk-dist 是否适合你的机器学习问题领域：

1.  **传统机器学习** — 像[广义线性模型](https://scikit-learn.org/stable/modules/linear_model.html)、[随机梯度下降](https://scikit-learn.org/stable/modules/sgd.html)、[最近邻](https://scikit-learn.org/stable/modules/neighbors.html)、[决策树](https://scikit-learn.org/stable/modules/tree.html)和[朴素贝叶斯](https://scikit-learn.org/stable/modules/naive_bayes.html)等方法在 sk-dist 中表现良好。这些方法在 scikit-learn 中都有实现，可以直接与 sk-dist 元估计器配合使用。

1.  **小到中等规模数据** — 大数据与 sk-dist 不兼容。请记住，训练分布的维度沿着模型的轴，而不是数据。数据不仅需要能够适应每个执行器的内存，还需要足够小以便进行广播。根据 Spark 配置，最大广播大小可能会成为限制因素。

1.  **Spark 定向和访问** — sk-dist 的核心功能需要运行 Spark。这对于个人或小型数据科学团队来说并不总是可行的。此外，还需要一些 Spark 调优和配置，以成本效益的方式充分利用 sk-dist，这需要对 Spark 基础知识有一定的培训。

这里需要注意的是，尽管神经网络和深度学习技术理论上可以与 sk-dist 一起使用，但这些技术需要大量的训练数据，有时还需要专门的基础设施才能有效。深度学习并不是 sk-dist 的预期使用场景，因为它违背了上述 (1) 和 (2) 条。作为替代方案，我们在 Ibotta [使用了](https://medium.com/building-ibotta/heating-up-word2vec-blazingtext-for-real-time-search-c2121bd1396) [Amazon SageMaker](https://aws.amazon.com/sagemaker/) 进行这些技术，我们发现其在这些工作负载中的计算效率优于 Spark。

### 入门指南

要开始使用 sk-dist，请查看 [安装指南](https://github.com/Ibotta/sk-dist#installation)。代码库还包含一个 [示例](https://github.com/Ibotta/sk-dist/tree/master/examples) 库，以展示 sk-dist 的一些使用案例。 [欢迎所有人](https://github.com/Ibotta/sk-dist/blob/master/CODE_OF_CONDUCT.md) 提交问题并参与项目贡献。

如果这些项目和挑战对您感兴趣，Ibotta 正在招聘！请查看我们的 [招聘页面](https://ibotta.com/careers) 了解更多信息。

感谢 charley frazier、Chad Foley 和 Sam Weiss。

**简介: [Evan Harris](https://www.linkedin.com/in/evan-harris-387375b2/)** 是一位经验丰富的技术专家，专注于数据科学和机器学习。他目前领导一个机器学习团队，致力于构建搜索相关性、内容推荐、定价优化和欺诈检测服务，支持一个服务于数百万用户的移动应用程序。

[原文](https://medium.com/building-ibotta/train-sklearn-100x-faster-bec530fc1f45)。经许可转载。

**相关:**

+   [理解 Python 中的决策树分类](/2019/08/understanding-decision-trees-classification-python.html)

+   [高级 Keras — 准确恢复训练过程](/2019/03/advanced-keras-accurately-resuming-training-process.html)

+   [分布式人工智能：多智能体系统、基于代理的建模和群体智能简介](/2019/04/distributed-artificial-intelligence-multi-agent-systems-agent-based-modeling-swarm-intelligence.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 服务

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并通过寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学的统计学顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功的数据科学家的五个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
