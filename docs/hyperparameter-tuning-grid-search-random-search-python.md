# 使用网格搜索和随机搜索进行超参数调优

> 原文：[`www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html`](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

![使用网格搜索和随机搜索进行超参数调优](img/0209199f770a3f9c2a8349dc5ca5f3c8.png)

图片由编辑提供

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 方面的工作

* * *

所有机器学习模型都有一组超参数或参数，这些参数必须由从业者指定。

例如，一个逻辑回归模型有不同的求解器用于找到可以给出最佳输出的系数。每个求解器使用不同的算法来找到最佳结果，并且这些算法没有哪个严格优于其他算法。除非你尝试所有的求解器，否则很难判断哪个求解器在你的数据集上表现最好。

最佳超参数是主观的，并且对于每个数据集都不同。Python 中的 [Scikit-Learn](https://scikit-learn.org/stable/) 库提供了一组默认的超参数，这些超参数在所有模型上表现都还算不错，但它们不一定适用于每个问题。

找到数据集的最佳超参数的唯一方法是通过试验和错误，这也是 **超参数优化** 的主要概念。

简单来说，超参数优化是一种技术，涉及在一系列值中搜索，以找到一组能够在给定数据集上实现最佳性能的结果。

有两种流行的技术用于进行超参数优化——网格搜索和随机搜索。

## 网格搜索

在进行超参数优化时，我们首先需要定义一个 **参数空间** 或 **参数网格**，其中包含一组可以用于构建模型的可能超参数值。

网格搜索技术将这些超参数放置在类似矩阵的结构中，然后在每个超参数值的组合上训练模型。

然后选择表现最佳的模型。

## 随机搜索

虽然网格搜索会查看所有可能的超参数组合以找到最佳模型，但随机搜索只会选择并测试一个随机的超参数组合。

该技术从超参数网格中随机抽样，而不是进行详尽的搜索。

我们可以指定随机搜索应该尝试的总运行次数，然后再返回最佳模型。

现在你对随机搜索和网格搜索的基本原理有了了解，我将向你展示如何使用 Scikit-Learn 库实现这些技术。

# 使用网格搜索和随机搜索优化随机森林分类器

## 第 1 步：加载数据集

在 Kaggle 上下载[酒质数据集](https://www.kaggle.com/datasets/rajyellow46/wine-quality)，然后输入以下代码行，使用[Pandas](https://pandas.pydata.org/)库读取数据：

```py
import pandas as pd

df = pd.read_csv('winequality-red.csv')
df.head()
```

数据框的头部如下所示：

![使用网格搜索和随机搜索进行超参数调优](img/cb924c16c04680333424a19f249a9ab4.png)

## 第 2 步：数据预处理

目标变量“quality”包含范围在 1 到 10 之间的值。

我们将通过将所有质量值小于或等于 5 的数据点分配为 0，将其余观察值分配为 1，将其转化为二分类任务：

```py
import numpy as np

df['target'] = np.where(df['quality']>5, 1, 0)

```

让我们将数据框中的因变量和自变量分开：

```py
df2 = df.drop(['quality'],axis=1)
X = df2.drop(['target'],axis=1)
y = df2[['target']]

```

## 第 3 步：构建模型

现在，让我们实例化一个随机森林分类器。我们将调整该模型的超参数，以创建适合我们数据集的最佳算法：

```py
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier()

```

## 第 4 步：使用 Scikit-Learn 实现网格搜索

### 定义超参数空间

我们现在将尝试调整该模型的以下超参数集合：

1.  **“Max_depth”**：该超参数表示随机森林模型中每棵树的最大层级。较深的树可以很好地捕捉训练数据中的大量信息，但对测试数据的泛化能力较差。默认情况下，Scikit-Learn 库中的该值设置为“None”，这意味着树会完全扩展。

1.  **“Max_features”**：随机森林模型在每次分裂时允许尝试的最大特征数量。Scikit-Learn 中的默认值设置为数据集中变量总数的平方根。

1.  **“N_estimators”**：森林中决策树的数量。Scikit-Learn 中的默认估计器数量为 10。

1.  “Min_samples_leaf”：每棵树的叶节点所需的最小样本数。Scikit-Learn 中的默认值为 1。

1.  **“Min_samples_split”**：每棵树内部节点分裂所需的最小样本数。Scikit-Learn 中的默认值为 2。

我们现在将创建一个包含所有上述超参数的多个可能值的字典。这也叫做**超参数空间**，将会通过它来寻找最佳的参数组合：

```py
grid_space={'max_depth':[3,5,10,None],
              'n_estimators':[10,100,200],
              'max_features':[1,3,5,7],
              'min_samples_leaf':[1,2,3],
              'min_samples_split':[1,2,3]
           }

```

### 运行网格搜索

现在，我们需要进行搜索以找到模型的最佳超参数组合：

```py
from sklearn.model_selection import GridSearchCV

grid = GridSearchCV(rf,param_grid=grid_space,cv=3,scoring='accuracy')
model_grid = grid.fit(X,y)

```

### 评估模型结果

最后，让我们输出最佳模型的准确性，并附上产生该分数的超参数集合：

```py
print('Best hyperparameters are: '+str(model_grid.best_params_))
print('Best score is: '+str(model_grid.best_score_))

```

最佳模型的准确率约为 0.74，其超参数如下：

![使用网格搜索和随机搜索进行超参数调优](img/6d3aac2ac36e11b8123c41c55a83a71c.png)

现在，让我们在相同的数据集上使用随机搜索，看看是否能得到类似的结果。

## 步骤 5：使用 Scikit-Learn 实现随机搜索

### 定义超参数空间

现在，让我们定义超参数空间以实现随机搜索。这个参数空间可以比我们为网格搜索构建的范围更大，因为随机搜索不会尝试每一种超参数组合。

它随机抽样超参数以找到最佳参数，这意味着与网格搜索不同，随机搜索可以快速浏览大量值。

```py
from scipy.stats import randint

rs_space={'max_depth':list(np.arange(10, 100, step=10)) + [None],
              'n_estimators':np.arange(10, 500, step=50),
              'max_features':randint(1,7),
              'criterion':['gini','entropy'],
              'min_samples_leaf':randint(1,4),
              'min_samples_split':np.arange(2, 10, step=2)
         }

```

### 运行随机搜索

运行以下代码行以在模型上执行随机搜索：（注意，我们已指定 *n_iter=500*，这意味着随机搜索将在选择最佳模型之前运行 500 次。你可以尝试不同的迭代次数以查看哪种给你最优的结果。请记住，较大的迭代次数将带来更好的性能，但也更耗时。）

```py
from sklearn.model_selection import RandomizedSearchCV

rf = RandomForestClassifier()
rf_random = RandomizedSearchCV(rf, space, n_iter=500, scoring='accuracy', n_jobs=-1, cv=3)
model_random = rf_random.fit(X,y)

```

### 评估模型结果

现在，运行以下代码行以打印随机搜索找到的最佳超参数及最佳模型的最高准确率：

```py
print('Best hyperparameters are: '+str(model_random.best_params_))
print('Best score is: '+str(model_random.best_score_))

```

随机搜索找到的最佳超参数如下：

![使用网格搜索和随机搜索进行超参数调优](img/734136ba652922a356c39a64f55a9c84.png)

所有构建的模型的最高准确率也大约为 0.74。

观察到网格搜索和随机搜索在数据集上表现都相当不错。请记住，如果你在相同的代码上运行随机搜索，你的结果可能会与我上面展示的结果大相径庭。

这是因为它通过随机初始化搜索非常大的参数网格，这可能导致每次使用该技术时结果变化剧烈。

## 完整代码

这里是教程中使用的完整代码：

```py
# imports

import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import GridSearchCV
from scipy.stats import randint
from sklearn.model_selection import RandomizedSearchCV

# reading the dataset 

df = pd.read_csv('winequality-red.csv')

# preprocessing 
df['target'] = np.where(df['quality']>5, 1, 0)
df2 = df.drop(['quality'],axis=1)
X = df2.drop(['target'],axis=1)
y = df2[['target']]

# initializing random forest
rf = RandomForestClassifier()

# grid search cv
grid_space={'max_depth':[3,5,10,None],
              'n_estimators':[10,100,200],
              'max_features':[1,3,5,7],
              'min_samples_leaf':[1,2,3],
              'min_samples_split':[1,2,3]
           }

grid = GridSearchCV(rf,param_grid=grid_space,cv=3,scoring='accuracy')
model_grid = grid.fit(X,y)

# grid search results
print('Best grid search hyperparameters are: '+str(model_grid.best_params_))
print('Best grid search score is: '+str(model_grid.best_score_))

# random search cv
rs_space={'max_depth':list(np.arange(10, 100, step=10)) + [None],
              'n_estimators':np.arange(10, 500, step=50),
              'max_features':randint(1,7),
              'criterion':['gini','entropy'],
              'min_samples_leaf':randint(1,4),
              'min_samples_split':np.arange(2, 10, step=2)
          }

rf = RandomForestClassifier()

rf_random = RandomizedSearchCV(rf, rs_space, n_iter=500, scoring='accuracy', n_jobs=-1, cv=3)
model_random = rf_random.fit(X,y)

# random random search results
print('Best random search hyperparameters are: '+str(model_random.best_params_))
print('Best random search score is: '+str(model_random.best_score_))

```

# 网格搜索 vs 随机搜索 - 该使用哪一个？

如果你在选择网格搜索和随机搜索之间犹豫，以下是一些帮助你决定使用哪一个的提示：

1.  如果你已经有一个大致的已知超参数值范围，并且这些值表现良好，就使用网格搜索。确保保持参数空间小，因为网格搜索可能非常耗时。

1.  如果你还不清楚哪些参数在你的模型上表现良好，可以在广泛的值范围上使用随机搜索。随机搜索比网格搜索更快，并且在参数空间很大时应始终使用。

1.  同时使用随机搜索和网格搜索也是一个好主意，以获得最佳结果。

你可以先使用随机搜索，特别是在参数空间很大的情况下，因为它更快。然后，使用随机搜索找到的最佳超参数来缩小参数网格，并将更小范围的值提供给网格搜索。

**[Natassha Selvaraj](https://www.natasshaselvaraj.com/)** 是一位自学成才的数据科学家，对写作充满热情。你可以通过[LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/)与她联系。

### 相关主题

+   [超参数调优：GridSearchCV 和 RandomizedSearchCV 解释](https://www.kdnuggets.com/hyperparameter-tuning-gridsearchcv-and-randomizedsearchcv-explained)

+   [调整随机森林的超参数](https://www.kdnuggets.com/2022/08/tuning-random-forest-hyperparameters.html)

+   [集成学习技术：使用 Python 中的随机森林进行详细讲解](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [超参数优化：10 个顶级 Python 库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)

+   [通过 Uplimit 的“机器学习搜索”课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第二部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)
