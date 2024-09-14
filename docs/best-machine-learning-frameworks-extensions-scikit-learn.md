# Scikit-learn 的最佳机器学习框架和扩展

> 原文：[https://www.kdnuggets.com/2021/03/best-machine-learning-frameworks-extensions-scikit-learn.html](https://www.kdnuggets.com/2021/03/best-machine-learning-frameworks-extensions-scikit-learn.html)

[评论](#comments)

很多包实现了 [Scikit-learn](https://scikit-learn.org/) 估算器 API。

如果你已经熟悉 Scikit-learn，你会发现这些库的集成非常简单。

使用这些包，我们可以扩展 Scikit-learn 估算器的功能，我将在这篇文章中展示如何使用其中一些。

### 数据格式

在这一部分，我们将探索可以用于处理和转换数据的库。

### [Sklearn-pandas](https://github.com/scikit-learn-contrib/sklearn-pandas)

你可以使用这个包将 ‘DataFrame’ 列映射到 Scikit-learn 转换中。然后你可以将这些列组合成特征。

要开始使用这个包，通过 pip 安装 ‘sklearn-pandas’。‘DataFrameMapper’ 可用于将 pandas 数据帧列映射到 Scikit-learn 转换中。我们来看看怎么做。

首先，创建一个虚拟 DataFrame：

```py
data =pd.DataFrame({ 
    'Name':['Ken','Jeff','John','Mike','Andrew','Ann','Sylvia','Dorothy','Emily','Loyford'], 
    'Age':[31,52,56,12,45,50,78,85,46,135], 
    'Phone':[52,79,80,75,43,125,74,44,85,45], 
    'Uni':['One','Two','Three','One','Two','Three','One','Two','Three','One'] 
})
```

`DataFrameMapper` 接受一个元组列表——第一个项的名称是 [Pandas](https://pandas.pydata.org/) DataFrame 中的列名。

第二个传递的项是将应用于该列的转换类型。

例如，‘[LabelBinarizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelBinarizer.html)’ 可以应用于 ‘Uni’ 列，而 ‘Age’ 列则使用 ‘[StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)’ 进行缩放。

```py
from sklearn_pandas import DataFrameMapper mapper = DataFrameMapper([ 
    ('Uni', sklearn.preprocessing.LabelBinarizer()), 
    (['Age'], sklearn.preprocessing.StandardScaler()) 
])
```

在定义映射器之后，接下来我们使用它来拟合和转换数据。

```py
mapper.fit_transform(data)
```

映射器的 `transformed_names_` 属性可用于显示转换后的结果名称。

```py
mapper.transformed_names_
```

![scikit-learn 扩展](../Images/e20ec6b1d2a971d30cf79b13060e9d58.png)

传递`df_out=True`给映射器将把结果返回为 Pandas DataFrame。

```py
mapper = DataFrameMapper([ 
    ('Uni', sklearn.preprocessing.LabelBinarizer()), 
    (['Age'], sklearn.preprocessing.StandardScaler()) 
],df_out=True)
```

![scikit-learn 扩展](../Images/3af27754b889c66f1b59217310b36c41.png)

### [Sklearn-xarray](https://phausamann.github.io/sklearn-xarray/)

这个包将来自 [xarray](http://xarray.pydata.org/en/stable/) 的 n 维标记数组与 [Scikit-learn](http://scikit-learn.org/stable/) 工具结合起来。

你可以将 Scikit-learn 估算器应用于 ‘xarrays’，而不会丢失它们的标签。你还可以：

+   确保 Sklearn 估算器与 xarray DataArrays 和 Datasets 之间的兼容性，

+   使估算器能够更改样本数量，

+   具有预处理转换器。

Sklearn-xarray 基本上是 xarray 和 Scikit-learn 之间的桥梁。为了使用其功能，通过 pip 或 ‘conda’ 安装 ‘sklearn-xarray’。

该包具有包装器，可以让你在 xarray DataArrays 和 Datasets 上使用 sklearn 估算器。为了说明这一点，首先我们创建一个 ‘DataArray’。

```py
import numpy as np 
import xarray as xr 
data = np.random.rand(16, 4) 
my_xarray = xr.DataArray(data)
```

![scikit-learn 扩展](../Images/72abea9b124f6c109fed33700306eb55.png)

从 Sklearn 中选择一种转换应用于此 ‘DataArray’。在这种情况下，[我们来应用](https://phausamann.github.io/sklearn-xarray/content/wrappers.html) ‘StandardScaler’。

```py
from sklearn.preprocessing import StandardScaler 
Xt = wrap(StandardScaler()).fit_transform(X)
```

![scikit-learn 扩展](../Images/0608abb02873fc4fb3286eac01d25fda.png)

包装的估算器可以无缝地在 Sklearn 管道中使用。

```py
pipeline = Pipeline([ 
    ('pca', wrap(PCA(n_components=50), reshapes='feature')), 
    ('cls', wrap(LogisticRegression(), reshapes='feature')) 
])
```

在拟合此管道时，您只需传入 DataArray。

类似地，DataArrays 可以在交叉验证网格搜索中使用。

为此，您需要从 ‘sklearn-xarray’ 创建一个 ‘CrossValidatorWrapper’ 实例。

```py
from sklearn_xarray.model_selection 
import CrossValidatorWrapper from sklearn.model_selection 
import GridSearchCV, KFold 
cv = CrossValidatorWrapper(KFold()) 
pipeline = Pipeline([ 
    ('pca', wrap(PCA(), reshapes='feature')), 
    ('cls', wrap(LogisticRegression(), reshapes='feature')) 
]) 
gridsearch = GridSearchCV( 
    pipeline, cv=cv, param_grid={'pca__n_components': [20, 40, 60]} 
)
```

之后，您将把 ‘gridsearch’ 拟合到 ‘DataArray’ 数据类型中的 X 和 y。

### 自动化机器学习

是否有工具和库可以将 Sklearn 集成以更好地进行自动化机器学习？是的，有一些例子。

### [Auto-sklearn](https://automl.github.io/auto-sklearn/master/)

通过这个，您可以使用 Scikit-learn 进行自动化机器学习。对于设置，您需要手动[安装](https://automl.github.io/auto-sklearn/master/installation.html)一些依赖项。

```py
$ curl https://raw.githubusercontent.com/automl/auto-sklearn/master/requirements.txt | xargs -n 1 -L 1 pip install
```

接下来，通过 pip 安装 ‘auto-sklearn’。

使用此工具时，您无需担心算法选择和超参数调整。Auto-sklearn 会为您处理这些。

这是得益于[最新进展](https://proceedings.neurips.cc/paper/2015/file/11d0e6287202fced83f79975ec59a3a6-Paper.pdf)在贝叶斯优化、元学习和集成构建领域。

要使用它，您需要选择一个分类器或回归器，并将其拟合到训练集上。

```py
from autosklearn.classification 
import AutoSklearnClassifier 
cls = AutoSklearnClassifier() 
cls.fit(X_train, y_train) 
predictions = cls.predict(X_test)
```

### [Auto_ViML](https://github.com/AutoViML) – 自动化变体可解释机器学习（发音为 “Auto_Vimal”）

给定某个数据集，Auto_ViML 尝试不同的模型和特征，最终选定表现最佳的模型。

该软件包还会在构建模型时选择最少数量的特征。这会为您提供一个更简单且易于解释的模型。该软件包还：

+   通过建议更改缺失值、格式和添加变量来帮助您清理数据；

+   自动分类变量，无论是文本、数据还是数字；

+   当详细信息设置为 1 或 2 时，会自动生成模型性能图；

+   让您使用 ‘featuretools’ 进行特征工程；

+   当 ‘Imbalanced_Flag’ 设置为 ‘True’ 时，处理不平衡数据

要查看实际效果，请通过 pip 安装 ‘autoviml’。

```py
from sklearn.model_selection import train_test_split, cross_validate 

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1, random_state=42) 
X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, test_size=0.25, random_state=54) 

train, test = X_train.join(y_train), X_val.join(y_val) 
model, features, train, test = Auto_ViML(train,"target",test,verbose=2)
```

### [TPOT –](http://proceedings.mlr.press/v64/olson_tpot_2016.pdf) 基于树的管道优化工具

这是一个基于 Python 的自动化机器学习工具。它使用遗传编程来优化机器学习管道。

它探索多个管道以便选出最适合您的数据集的管道。

通过 pip 安装 ‘tpot’ 开始进行尝试。运行 ‘tpot’ 后，您可以将生成的管道保存到文件中。文件将在探索过程完成后或当您终止过程时导出。

下面的代码片段展示了如何在数字数据集上创建分类管道。

```py
from tpot import TPOTClassifier 
from sklearn.datasets import load_digits 
from sklearn.model_selection import train_test_split 

digits = load_digits() 
X_train, X_test, y_train, y_test = train_test_split(digits.data, digits.target, train_size=0.75, test_size=0.25, random_state=42) 
tpot = TPOTClassifier(generations=5, population_size=50, verbosity=2, random_state=42) 
tpot.fit(X_train, y_train) 
print(tpot.score(X_test, y_test)) 
tpot.export('tpot_digits_pipeline.py')
```

### [Feature Tools](https://github.com/alteryx/featuretools)

这是一个自动特征工程的工具。它通过将时间序列和关系数据集转换为特征矩阵来工作。

通过pip安装‘featuretools[complete]’以开始使用它。

深度特征合成（DFS）可以用于自动特征工程。

首先，你需要定义一个包含数据集中所有实体的字典。在‘featuretools’中，实体是一个单独的表。然后，定义不同实体之间的关系。

下一步是将实体、关系列表和目标实体传递给DFS。这将为你提供特征矩阵及其对应的特征定义列表。

```py
import featuretools as ft

entities = {
    "customers" : (customers_df, "customer_id"),
    "sessions" : (sessions_df, "session_id", "session_start"),
    "transactions" : (transactions_df, "transaction_id", "transaction_time")
 }

relationships = [("sessions", "session_id", "transactions", "session_id"),
                 ("customers", "customer_id", "sessions", "customer_id")]

feature_matrix, features_defs = ft.dfs(entities=entities,
                                                 relationships = relationships,
                                                 target_entity = "customers")
```

### [Neuraxle](https://www.neuraxle.org/)

你可以使用Neuraxle进行超参数调优和AutoML。通过pip安装‘neuraxle’以开始使用它。

除了Scikit-learn，Neuraxle还兼容Keras、TensorFlow和PyTorch。它还具有：

+   并行计算和序列化，

+   通过提供对这类项目至关重要的抽象来处理时间序列。

要使用Neuraxle进行自动化机器学习，你需要：

+   一个定义好的管道

+   一个验证分割器

+   通过‘ScoringCallback’定义评分指标

+   选择的‘hyperparams’仓库

+   选择的‘hyperparams’优化器

+   一个‘AutoML’循环

完整的[示例请查看这里](https://www.neuraxle.org/stable/hyperparameter_tuning.html)。

### 实验框架

现在是时候介绍几个你可以用于机器学习实验的SciKit工具了。

### [SciKit-Learn实验室](https://scikit-learn-laboratory.readthedocs.io/en/latest/)

SciKit-Learn实验室是一个命令行工具，你可以用来运行机器学习实验。要开始使用它，请通过pip安装`skll`。

在那之后，你需要获取一个`SKLL`格式的数据集。

接下来，为实验创建一个[配置文件](https://skll.readthedocs.io/en/latest/run_experiment.html#create-config)，然后在终端中运行实验。

```py
$ run_experimen experiment.cfg
```

当实验完成时，多个文件将存储在[结果](https://skll.readthedocs.io/en/latest/run_experiment.html#results)文件夹中。你可以使用这些文件来检查实验。

### Neptune

[Scikit-learn与Neptune的集成](https://docs.neptune.ai/integrations/sklearn.html)让你可以使用Neptune记录你的实验。例如，你可以记录你的Scikit-learn回归器的总结。

```py
from neptunecontrib.monitoring.sklearn import log_regressor_summary

log_regressor_summary(rfr, X_train, X_test, y_train, y_test)
```

查看这个[笔记本](https://colab.research.google.com/github/neptune-ai/neptune-examples/blob/master/integrations/sklearn/docs/Neptune-Scikit-learn.ipynb#scrollTo=GvDSBSrOx-R4)以获取完整示例。

### 模型选择

现在让我们转移话题，看看专注于模型选择和优化的SciKit库。

### [Scikit-optimize](https://scikit-optimize.github.io/stable/)

这个库实现了基于序列模型的优化方法。通过pip安装‘scikit-optimize’以开始使用这些功能。

Scikit-optimize可以通过基于贝叶斯定理的贝叶斯优化来进行超参数调优。

你可以使用‘BayesSearchCV’根据这一理论获得最佳参数。一个Scikit-learn模型作为第一个参数传递给它。

```py
from skopt.space import Real, Categorical, Integer
from skopt import BayesSearchCV
regressor = BayesSearchCV(
    GradientBoostingRegressor(),
      {
         'learning_rate': Real(0.1,0.3),
         'loss': Categorical(['lad','ls','huber','quantile']),
         'max_depth': Integer(3,6),
      },
    n_iter=32,
    random_state=0,
    verbose=1,
    cv=5, n_jobs=-1,
  )
regressor.fit(X_train,y_train)
```

适配后，你可以通过‘best_params_’属性获得模型的最佳参数。

### [Sklearn-deap](https://github.com/rsteca/sklearn-deap)

Sklearn-deap是一个用于实现[进化算法](https://en.wikipedia.org/wiki/Evolutionary_algorithm)的包。它减少了你找到最佳模型参数所需的时间。

它并不会尝试每一种可能的组合，而是只演化出结果最佳的组合。通过pip安装‘sklearn-deap’。

```py
from evolutionary_search import EvolutionaryAlgorithmSearchCV
cv = EvolutionaryAlgorithmSearchCV(estimator=SVC(),
                                   params=paramgrid,
                                   scoring="accuracy",
                                   cv=StratifiedKFold(n_splits=4),
                                   verbose=1,
                                   population_size=50,
                                   gene_mutation_prob=0.10,
                                   gene_crossover_prob=0.5,
                                   tournament_size=3,
                                   generations_number=5,
                                   n_jobs=4)
cv.fit(X, y)
```

### 生产用模型导出

继续前进，现在我们来看看可以用来导出模型以供生产的Scikit工具。

### [sklearn-onnx](https://github.com/onnx/sklearn-onnx/)

sklearn-onnx实现了将Scikit-learn模型转换为[ONNX](https://onnx.ai/)。

要使用它，你需要通过pip获取‘skl2onnx’。一旦你的管道准备好，你可以使用‘to_onnx’函数来[转换](http://onnx.ai/sklearn-onnx/auto_tutorial/plot_abegin_convert_pipeline.html)模型为ONNX格式。

```py
from skl2onnx import to_onnx 
onx = to_onnx(pipeline, X_train[:1].astype(numpy.float32))
```

### [Treelite](https://treelite.readthedocs.io/en/latest/)

这是一个用于决策树集合的模型编译器。

它处理各种基于树的模型，如随机森林和梯度提升树。

你可以用它导入Scikit-learn模型。这里的‘model’是一个scikit-learn模型对象。

```py
import treelite.sklearn 
model = treelite.sklearn.import_model(model)
```

### 模型检查和可视化

在这一部分，我们将探讨可以用于模型可视化和检查的库。

### [Dtreeviz](https://github.com/parrt/dtreeviz/)

dtreeviz用于决策树的可视化和模型解释。

```py
from dtreeviz.trees import dtreeviz 
viz = dtreeviz( 
   model, X_train, y_train, 
   feature_names=boston.feature_names, 
   fontname="Arial", title_fontsize=16, 
   colors = {"title":"red"} 
)
```

![](../Images/d12bd6f8518cbf2053f0378db032cb43.png)

### [Eli5](https://github.com/TeamHG-Memex/eli5/)

eli5是一个可以用于调试和检查机器学习分类器的包。你还可以用它来解释这些分类器的预测。

例如，Scikit-learn估算器权重的解释可以如下所示：

```py
import eli5 
eli5.show_weights(model)
```

![scikit-learn扩展](../Images/2f9114ee1b717a317a3ddf58fa13bfc1.png)

### 其他SciKit工具

### [dabl](https://github.com/amueller/dabl)– 数据分析基础库

dabl提供了常见机器学习任务的样板代码。它仍在积极开发中，因此不推荐用于生产系统。

```py
import dabl
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_digits
X, y = load_digits(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1)
sc = dabl.SimpleClassifier().fit(X_train, y_train)
print("Accuracy score", sc.score(X_test, y_test))
```

### [skorch](https://github.com/skorch-dev/skorch)

Skorch是PyTorch的Scikit-learn封装器。

它允许你在Scikit-learn中使用PyTorch。它支持多种数据类型，如PyTorch Tensors、NumPy数组和Python字典。

```py
from skorch import NeuralNetClassifier
net = NeuralNetClassifier(
    MyModule,
    max_epochs=10,
    lr=0.1,
    iterator_train__shuffle=True,
)
net.fit(X, y)
```

### 最后的思考

在本文中，我们探讨了一些扩展Scikit-learn生态系统的流行工具和库。

如你所见，这些工具可以用于：

+   处理和转换数据，

+   实现自动化机器学习，

+   执行自动特征选择，

+   进行机器学习实验，

+   选择适合你问题的最佳模型和管道，

+   导出模型以供生产…

…以及更多！

尝试在您的 Scikit-learn 工作流程中使用这些包，您可能会惊讶于它们的便利性。

**个人简介：德里克·穆伊提**是一位数据科学家，对分享知识充满热情。他通过 Heartbeat、Towards Data Science、Datacamp、Neptune AI、KDnuggets 等博客积极贡献于数据科学社区。他的内容在互联网上的观看次数已超过百万。德里克还是一名作者和在线讲师。他还与各个机构合作，实施数据科学解决方案以及提升员工技能。您可能想查看他的[Python 完整数据科学与机器学习训练营课程](https://www.udemy.com/course/data-science-bootcamp-in-python/?referralCode=9F6DFBC3F92C44E8C7F4)。

[原文](https://neptune.ai/blog/the-best-ml-framework-extensions-for-scikit-learn)。经许可转载。

**相关：**

+   [数据科学、数据可视化和机器学习的顶级 Python 库](/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [深度学习、自然语言处理和计算机视觉的顶级 Python 库](/2020/11/top-python-libraries-deep-learning-natural-language-processing-computer-vision.html)

+   [在 TensorFlow 中修剪机器学习模型](/2020/12/pruning-machine-learning-models-tensorflow.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 管理

* * *

### 更多相关主题

+   [停止学习数据科学以寻找目标，并寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [分析一个90亿美元的 AI 失败案例](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每位数据科学家都应了解的三种 R 库（即使您使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
