# 你可能不知道的 10 件关于 Scikit-Learn 的事

> 原文：[https://www.kdnuggets.com/2020/09/10-things-know-scikit-learn.html](https://www.kdnuggets.com/2020/09/10-things-know-scikit-learn.html)

[评论](#comments)

**作者：[Rebecca Vickery](https://www.linkedin.com/in/rebecca-vickery-20b94133/)，数据科学家**

![图示](../Images/cbdc641473704432ff7792a819e0fc6d.png)

图片由 [Sasha • Stories](https://unsplash.com/@sanfrancisco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/crystal-ball?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

Scikit-learn 是最广泛使用的 Python 机器学习库之一。它提供了一个标准化且简单的接口用于数据预处理、模型训练、优化和评估。

该项目起初是由 David [Cournapeau](https://en.wikipedia.org/wiki/David_Cournapeau) 开发的 Google 夏季编程项目，并于 2010 年首次公开发布。自创建以来，该库已发展成一个丰富的机器学习模型开发生态系统。

随着时间的推移，该项目开发了许多实用的功能和能力，提升了其易用性。在本文中，我将介绍你可能不知道的 10 个最有用的功能。

### 1\. Scikit-learn 内置了数据集

Scikit-learn API 内置了各种[**玩具数据集和真实数据集**](https://scikit-learn.org/stable/datasets/index.html)。这些数据集可以通过一行代码访问，非常适合学习或快速尝试新功能。

你还可以使用[**生成器**](https://scikit-learn.org/stable/datasets/index.html#generated-datasets)轻松生成合成数据集，包括回归的 `make_regression()`、聚类的 `make_blobs()` 和分类的 `make_classification()`。

所有加载工具都提供了将数据拆分成 X（特征）和 y（目标）的选项，以便可以直接用于训练模型。

### 2\. 第三方公共数据集也很容易获得

如果你希望通过 Scikit-learn 直接访问更多种类的公开数据集，有一个方便的函数可以让你直接从[openml.org](https://www.openml.org/home)网站导入数据。这个网站包含了超过 21,000 个用于机器学习项目的多样化数据集。

### 3\. 有现成的分类器用于训练基线模型

在为项目开发机器学习模型时，首先创建一个基线模型是明智的。这个模型本质上应该是一个“虚拟”模型，例如总是预测最常见的类别。这样可以为你的“智能”模型提供一个基准，以确保它的表现优于随机结果。

Scikit-learn 包含一个`[**DummyClassifier()**](https://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html)`用于分类任务和一个`**DummyRegressor()**`用于回归问题。

### 4\. Scikit-learn 具有自己的绘图 API

Scikit-learn 具有内置的[**绘图 API**](https://scikit-learn.org/stable/developers/plotting.html)，允许你在不导入其他库的情况下可视化模型性能。包括以下绘图工具；偏依赖图、混淆矩阵、精确度-召回曲线和 ROC 曲线。

```py

import matplotlib.pyplot as plt 
from sklearn import metrics, model_selection
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_breast_cancer

X,y = load_breast_cancer(return_X_y = True)

X_train, X_test, y_train, y_test = model_selection.train_test_split(X, y, random_state=0)
clf = RandomForestClassifier(random_state=0)
clf.fit(X_train, y_train)

metrics.plot_roc_curve(clf, X_test, y_test)
plt.show()

```

![Image for post](../Images/adae73e95dd662f04474adbc8c32218a.png)

### 5\. Scikit-learn 具有内置的特征选择方法

提高模型性能的一种技术是仅使用最佳特征集进行训练或去除冗余特征。这个过程被称为特征选择。

Scikit-learn 提供了许多函数来执行[**特征选择**](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.feature_selection)。一个例子是`[**SelectPercentile()**](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectPercentile.html#sklearn.feature_selection.SelectPercentile)`。该方法基于选定的统计方法为评分选择表现最好的 X 百分位特征。

### 6\. Pipelines 允许你将机器学习工作流中的所有步骤链式组合在一起

除了提供广泛的机器学习算法外，Scikit-learn 还提供了一系列用于数据预处理和转换的函数。为了促进机器学习工作流的可重复性和简便性，Scikit-learn 创建了[**pipelines**](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)，允许你将多个预处理步骤与模型训练阶段链式组合。

管道将工作流中的所有步骤作为一个单一实体存储，可以通过 fit 和 predict 方法调用。当你在管道对象上调用 fit 方法时，预处理步骤和模型训练会自动执行。

### 7\. 使用 ColumnTransformer，你可以对不同的特征应用不同的预处理

在许多数据集中，你会遇到需要不同预处理步骤的各种特征。例如，你可能有类别型数据和数值型数据的混合，可能需要通过独热编码将类别数据转换为数值，并对数值变量进行缩放。

Scikit-learn 管道中有一个叫 [**ColumnTransformer**](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html#sklearn.compose.ColumnTransformer) 的函数，它允许你通过索引或指定列名轻松指定应用最合适的预处理的列。

### 8\. 你可以轻松输出管道的 HTML 表示

管道往往会变得相当复杂，特别是在处理实际数据时。因此，Scikit-learn 提供了一种方法来输出 [**HTML 图示**](https://scikit-learn.org/stable/modules/compose.html#visualizing-composite-estimators) 以显示管道中的步骤非常方便。

![文章配图](../Images/5427a5b0ffaa48ee38798396281e2b25.png)

### 9\. 有一个绘图函数用于可视化树

` [**plot_tree()**](https://scikit-learn.org/stable/modules/generated/sklearn.tree.plot_tree.html)` 函数允许你创建决策树模型中步骤的图示。

![文章配图](../Images/e02b4283a42a67f179b6df6016b20cb4.png)

### 10\. 有许多第三方库可以扩展 Scikit-learn 的功能

许多第三方库可以与 Scikit-learn 配合使用，并扩展其功能。

两个示例包括 [**category-encoders**](http://contrib.scikit-learn.org/category_encoders/) 库，它提供了更多的类别特征预处理方法，以及 [**ELI5**](https://eli5.readthedocs.io/en/latest/) 包，用于更好的模型解释。

这两个包也可以直接在 Scikit-learn 管道中使用。

感谢阅读！

[**我每月发送一次通讯，如果你想加入，请通过这个链接注册。期待成为你学习旅程的一部分！**](https://mailchi.mp/ce8ccd91d6d5/datacademy-signup)

**简历： [Rebecca Vickery](https://www.linkedin.com/in/rebecca-vickery-20b94133/)** 正通过自学数据科学。现任 Holiday Extras 数据科学家。alGo 联合创始人。

[原始链接](https://towardsdatascience.com/10-things-you-didnt-know-about-scikit-learn-cccc94c50e4f)。经授权转载。

**相关：**

+   [可解释机器学习的 Python 库](/2019/09/python-libraries-interpretable-machine-learning.html)

+   [数据科学的五个命令行工具](/2019/07/five-command-line-tools-data-science.html)

+   [每位数据科学家都应该知道的命令行基础知识](/2019/08/command-line-basics-every-data-scientist.html)

### 更多相关话题

+   [你不知道的低代码工具的 7 件事](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [你不知道的SAS数据科学学院的3件事](https://www.kdnuggets.com/2022/07/sas-3-things-didnt-know-sas-academy-data-science.html)

+   [你可能不知道的4个Python Itertools过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [关于数据管理你需要知道的6件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [扩展你的Web数据驱动产品时你应该知道的事](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)

+   [构建LLM应用时需要知道的5件事](https://www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html)
