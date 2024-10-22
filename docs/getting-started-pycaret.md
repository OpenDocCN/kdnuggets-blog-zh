# 开始使用 PyCaret

> 原文：[`www.kdnuggets.com/2022/11/getting-started-pycaret.html`](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)

![开始使用 PyCaret](img/b403b58cba8bf7b381305dbaec25bc74.png)

图片由编辑提供

任何 AI 模型的训练和部署都经历漫长的数据过程。其中一些步骤是标准化的，可以自动化，从而实现快速的模型开发和部署。PyCaret 是为大规模生产机器学习模型而开发的软件包之一。让我们开始学习它吧。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

# 什么是 PyCaret？

PyCaret 是一个开源、低代码的数据科学 Python 包，通过自动化 ML 工作流来加速实验周期。它将数百行代码替换为仅几行，促进更快、更高效的实验。

它是围绕多个 ML 库和框架（如 scikit-learn、LightGBM/XGBoost/CatBoost 等提升库、spaCy、Hyperopt、Ray 等）构建的封装器，提供无缝且灵活的开发。

![开始使用 PyCaret](img/9ae4aad3f147edd95c9ae591d61bacce.png)

图片由 [Pycaret](https://pycaret.gitbook.io/docs/) 提供

PyCaret 简单易用，其操作按顺序存储在一个准备部署的管道中。PyCaret 自动处理预处理、特征工程以及超参数调优，全部开箱即用。

![开始使用 PyCaret](img/47c78bf95dd7850540cf6c3c735836b8.png)

图片由 [Moez Ali](https://moez-62905.medium.com/) 提供

它的模型库拥有超过 70 个未经训练的模型，适用于分类、回归、聚类等任务，模块覆盖了包括监督和无监督方法在内的广泛领域。

PyCaret 一应俱全——无论是与 SHAP 框架的集成以实现可解释性，还是与 MLFlow 的集成以跟踪实验。

# 安装

Pycaret 可以使用 pip 安装。默认安装仅安装硬依赖项，如下所示。

```py
pip install pycaret
```

要安装完整版本，请在终端/命令行中运行以下命令。

```py
pip install pycaret[full]
```

# 亲自动手！

Pycaret 提供一些标准的 [数据集](https://github.com/pycaret/pycaret/tree/master/datasets)。我们将使用“波士顿住房价格”数据集 ([boston.csv](https://github.com/pycaret/pycaret/blob/master/datasets/boston.csv)) 进行本教程。

首先，让我们导入诸如 PyCaret 中的“get_data”库，用于将数据加载到 Jupyter 环境中，以及“plotly express”库用于绘图/绘制图表。

```py
from pycaret.datasets import get_data
import plotly.express as px
from pycaret.regression import *
```

如果在导入库时遇到任何错误，可能是因为需要先解决的依赖性问题。请注意，PyCaret 目前不兼容 Python 3.9。本演示使用的是 [Google Colab](https://colab.research.google.com/) 中的 Python 3.7。

使用之前从 pycaret.datasets 导入的 get_data 函数将数据集加载到 Jupyter Notebook 中。get_data 函数返回一个数据框，可以按如下所示进行存储。

```py
data = get_data('boston')
```

PyCaret 默认显示数据框的前五行。因此，你不需要显式调用 dataframe.head() 函数。

![开始使用 PyCaret](img/08babd7779196ddbccc92c12e0a203ed.png)

数据集中的每一行代表波士顿地区的一个郊区或城镇。数据最初来自 1970 年的波士顿标准大都市统计区（SMSA），数据字典来源于 [这里](https://archive.ics.uci.edu/ml/machine-learning-databases/housing/housing.names)。

我们正在解决一个回归问题，考虑到目标变量 MEDV 的连续性。

下面的图表展示了具有以下特征的有趣散点图：

+   中位数自有住房价值（MEDV，单位为$1000）与人口低社会经济地位百分比（LSTAT）之间的双变量分布

+   将径向公路（RAD）的可达性指数作为一个分面列（相当于 Seaborn 中的“色调”）添加到方程中

+   使用 trendline 参数值为 “OLS” 的趋势线，表示普通最小二乘法

+   使用参数 trendline_color_override 将趋势线的颜色设置为红色以便于可见

```py
fig = px.scatter(x = data['lstat'], y = data['medv'], facet_col =
    data['rad'], opacity = 0.2, trendline = 'ols',
    trendline_color_override = 'red')
fig.show()
```

图表显示了中位数住房价格与人口低社会经济地位之间的强负相关关系。这意味着人口的经济状况越弱，可支配收入越低，因此住房价格也会越低。

![开始使用 PyCaret](img/288f1fd6211a27752247578bad4b05fa.png)

RAD 值为 24 的数据拟合效果良好，排除了一些异常值，并且在记录数量方面有不错的支持。

查看目标变量，即 MEDV 的分布，似乎在除了一些异常值外，分布相当接近正态分布。

```py
fig = px.histogram(data, x=["medv"])
fig.show()
```

![开始使用 PyCaret](img/20394f59f0207bef5c8af3a5be666bc6.png)

使用 setup 函数设置 PyCaret 实验非常简单。该函数使用以下参数作为输入——数据框、目标变量名称、一个用于记录实验结果的布尔值以及名称。

```py
s = setup(data, target = 'medv', log_experiment = True,
    experiment_name = 'housing_prices')
```

从 PyCaret 输出可以明显看出，它自动完成了许多任务，包括但不限于识别缺失值、连续和分类特征、变量的基数、划分训练集和测试集，并执行交叉验证，这些都需要相当的时间和资源。

![开始使用 PyCaret](img/998f4ad330e5a5284d1145995f28434b.png)

一旦实验设置完成，你需要运行 compare_models() 来对特定问题的多种算法进行实验。以下代码将最佳模型存储在一个变量中。

```py
best_model = compare_models()
```

上述代码在不同算法上训练和测试模型，并按错误的升序排列它们。它还显示了以秒为单位的训练时间，在最后一列中以 ‘TT (Sec)’ 表示。

值得注意的是，训练时间是基于使用所有 CPU 核心的，在不同机器上可能会有所不同。

![开始使用 PyCaret](img/3aa9118b971fb14bb1483ea5a18ef80d.png)

“get_params()” 方法用于检索最佳模型的超参数，在我们的例子中，这个最佳模型是 Extra Tree Regressor。

```py
best_model.get_params()
```

```py
{'bootstrap': False
'ccp_alpha': 0.0,
'criterion': 'mse',
'max_depth': None,
'Max_features': 'auto',
'max_leaf_nodes': None,
'max_samples': None,
'min_impurity_decrease': 0.0,
'min_impurity_split': None,
'min_samples_leaf': 1,
'min_samples_split': 2,
'min_weight_fraction_leaf': 0.0,
'n_estimators': 100,
'n_jobs': -1,
'oob_score': False,
'random _state': 8773,
'verbose': 0,
'warm_start': False}
```

特征重要性可以使用以下代码绘制。

```py
plot_model(best_model, plot = 'feature')
```

![开始使用 PyCaret](img/c74b2a7788393bba5583faa47506b922.png)

该可视化使得识别前 n 个特征变得更容易，最重要的特征位于顶部。

模型预测使用 predict_model() 函数生成。

```py
data_cpy = data.copy()
data_cpy.drop(‘medv’, axis = 1, inplace = True)
y_pred = predict_model(best_model, data = data_cpy)
```

save_model() 函数用于保存训练好的模型以便于部署。

```py
save_model(best_model, 'my_best_pipeline')
```

# 总结

在这篇文章中，你了解了 PyCaret 如何通过自动化大量重复任务来简化许多数据科学家和机器学习工程师的工作。文章还演示了如何使用 PyCaret 包自动化机器学习管道中的标准步骤。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位 AI 策略专家和数字化转型领袖，她在产品、科学和工程的交汇处工作，致力于构建可扩展的机器学习系统。她是获奖的创新领袖、作者和国际演讲者，致力于使机器学习普及并打破术语，使每个人都能参与这一转型。

### 更多主题

+   [PyCaret 二分类简介](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [使用 PyCaret 在 Python 中进行聚类简介](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [开始自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [开始清理数据](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)

+   [SQL 入门备忘单](https://www.kdnuggets.com/2022/08/getting-started-sql-cheatsheet.html)
