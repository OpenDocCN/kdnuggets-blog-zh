# 使用 Python 可视化决策树（Scikit-learn，Graphviz，Matplotlib）

> 原文：[https://www.kdnuggets.com/2020/04/visualizing-decision-trees-python.html](https://www.kdnuggets.com/2020/04/visualizing-decision-trees-python.html)

[评论](#comments)

**作者 [Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/)，数据科学家**

![图像](../Images/fb382bbd316bf7f8b976fa63f307ca8c.png)

图片来源于我的 [理解分类的决策树（Python）教程](https://towardsdatascience.com/understanding-decision-trees-for-classification-python-9663d683c952)。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

决策树是一种流行的监督学习方法，原因有很多。决策树的优点包括它们可以用于回归和分类，不需要特征缩放，并且相对容易解释，因为你可以可视化决策树。这不仅是理解模型的强大方式，也有助于传达模型的工作原理。因此，了解如何基于模型进行可视化是有帮助的。

本教程包括：

+   如何使用 Scikit-Learn 拟合决策树模型

+   如何使用 Matplotlib 可视化决策树

+   如何使用 Graphviz 可视化决策树（什么是 Graphviz，如何在 Mac 和 Windows 上安装它，以及如何使用它来可视化决策树）

+   如何可视化袋装树或随机森林中的单个决策树

一如既往，本教程中使用的代码可以在我的 [GitHub](https://github.com/mGalarnyk/Python_Tutorials/blob/master/Sklearn/CART/Visualization/DecisionTreesVisualization.ipynb) 上找到。让我们开始吧！

### 如何使用 Scikit-Learn 拟合决策树模型

为了可视化决策树，我们首先需要使用 scikit-learn 拟合一个决策树模型。如果这一部分不清楚，我鼓励你阅读我的 [理解分类的决策树（Python）教程](https://towardsdatascience.com/understanding-decision-trees-for-classification-python-9663d683c952)，因为我详细讲解了决策树的工作原理以及如何使用它们。

### 导入库

以下导入语句是我们在本教程这一部分中将使用的。

```py
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
import pandas as pd
import numpy as np
from sklearn import tree
```

### 加载数据集

Iris 数据集是 scikit-learn 附带的一个数据集，无需从外部网站下载任何文件。下面的代码加载了 iris 数据集。

```py
import pandas as pd
from sklearn.datasets import load_irisdata = load_iris()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target
```

![图示](../Images/b0dad4c5d86a7accf90b27aa29f1cf59.png)

原始 Pandas df（特征 + 目标）

### 将数据拆分为训练集和测试集

下面的代码将 75% 的数据放入训练集中，将 25% 的数据放入测试集中。

```py
X_train, X_test, Y_train, Y_test = train_test_split(df[data.feature_names], df['target'], random_state=0)
```

![图示](../Images/9d5993aa8f78a54086dec8b135c7ee7b.png)

图像中的颜色表示数据来自数据框 df 的哪个变量（X_train、X_test、Y_train、Y_test），以进行特定的训练测试分割。图片由 [Michael Galarnyk](https://twitter.com/GalarnykMichael) 提供。

### Scikit-learn 4 步建模模式

```py
# **Step 1:** Import the model you want to use
# This was already imported earlier in the notebook so commenting out
#from sklearn.tree import DecisionTreeClassifier**# Step 2:** Make an instance of the Model
clf = DecisionTreeClassifier(max_depth = 2, 
                             random_state = 0)**# Step 3:** Train the model on the data
clf.fit(X_train, Y_train)**# Step 4:** Predict labels of unseen (test) data
# Not doing this step in the tutorial
# clf.predict(X_test)
```

### 如何使用 Matplotlib 可视化决策树

从 scikit-learn 版本 21.0（大约 2019 年 5 月）开始，决策树现在可以使用 scikit-learn 的 `[**tree.plot_tree**](https://scikit-learn.org/stable/modules/generated/sklearn.tree.plot_tree.html#sklearn.tree.plot_tree)` 绘制，而无需依赖 `dot` 库，这是一个难以安装的依赖项，我们将在博客文章中后面部分讨论。

下面的代码使用 scikit-learn 绘制决策树。

```py
tree.plot_tree(clf);
```

![图示](../Images/b7e93c00e7ff79f78413d8a485d00751.png)

这还不是最具可解释性的树。

除了添加代码以允许你保存图像外，下面的代码尝试通过添加特征和类名称（以及设置 `filled = True`）使决策树更具可解释性。

```py
fn=['sepal length (cm)','sepal width (cm)','petal length (cm)','petal width (cm)']
cn=['setosa', 'versicolor', 'virginica']fig, axes = plt.subplots(nrows = 1,ncols = 1,figsize = (4,4), dpi=300)tree.plot_tree(clf,
               feature_names = fn, 
               class_names=cn,
               filled = True);fig.savefig('imagename.png')
```

![](../Images/0ce9a81306bca7e1509d042fd8999dc1.png)

### 如何使用 Graphviz 可视化决策树

![图示](../Images/31cfb0690b8025e8ab0056954eed2681.png)

通过 Graphviz 生成的决策树。请注意，我编辑了文件，使文本颜色与叶子/终端节点或决策节点相对应，使用了文本编辑器。

`Graphviz` 是开源的图形可视化软件。图形可视化是一种将结构信息表示为抽象图和网络图的方式。在数据科学中，`Graphviz` 的一个用途是可视化决策树。我需要说明的是，我在讲解 Matplotlib 之后介绍 Graphviz 的原因是因为使其正常工作可能会很困难。这一过程的第一部分涉及创建 dot 文件。dot 文件是决策树的 Graphviz 表示。问题在于使用 Graphviz 将 dot 文件转换为图像文件（png、jpg 等）可能会很困难。有几种方法可以做到这一点，包括：通过 Anaconda 安装 `python-graphviz`，通过 Homebrew（Mac）安装 Graphviz，从官方站点（Windows）安装 Graphviz 可执行文件，或使用在线转换器将 dot 文件的内容转换为图像。

![图示](../Images/ba1305b0404993219b0b3a4d38075251.png)

创建 dot 文件通常不是问题。将 dot 文件转换为 png 文件可能会很困难。

### 将模型导出为 dot 文件

下面的代码将在任何操作系统上工作，因为 python 生成 dot 文件并将其导出为名为 `tree.dot` 的文件。

```py
tree.export_graphviz(clf,
                     out_file="tree.dot",
                     feature_names = fn, 
                     class_names=cn,
                     filled = True)
```

### 安装和使用 Graphviz

将 dot 文件转换为图像文件（png、jpg 等）通常需要安装 Graphviz，这取决于你的操作系统和其他一些因素。本节的目的是帮助人们尝试解决常见的错误，即`dot: command not found`。

![Figure](../Images/9989054c402acde650472d475716dbeb.png)

`dot: command not found`

**如何通过 Anaconda 在 Mac 上安装和使用**

要通过这种方法在 Mac 上安装 Graphviz，你首先需要安装 Anaconda（如果你没有安装 Anaconda，你可以在[这里](https://www.datacamp.com/community/tutorials/installing-anaconda-mac-os-x)学习如何安装）。

打开终端。你可以通过点击屏幕右上角的 Spotlight 放大镜，输入 terminal 然后点击终端图标来完成。

![](../Images/74de505821de993e49ce79a3f4a3dbbd.png)

输入下面的命令以安装 Graphviz。

```py
conda install python-graphviz
```

之后，你应该能够使用下面的`dot`命令将 dot 文件转换为 png 文件。

```py
dot -Tpng tree.dot -o tree.png
```

**如何通过 Homebrew 在 Mac 上安装和使用**

如果你没有 Anaconda 或者只是想要另一种在 Mac 上安装 Graphviz 的方式，你可以使用[Homebrew](https://brew.sh/)。我之前写了一篇关于如何安装 Homebrew 并使用它将 dot 文件转换为图像文件的文章，[请点击这里](https://medium.com/@GalarnykMichael/how-to-install-and-use-homebrew-80eeb55f73e9)（请参阅教程中的 Homebrew 帮助可视化决策树部分）。

**如何通过 Anaconda 在 Windows 上安装和使用**

这是我在 Windows 上首选的方法。要通过这种方法在 Windows 上安装 Graphviz，你首先需要安装 Anaconda（如果你没有安装 Anaconda，你可以在[这里](https://medium.com/@GalarnykMichael/install-python-anaconda-on-windows-2020-f8e188f9a63d)学习如何安装）。

打开终端/命令提示符，输入下面的命令以安装 Graphviz。

```py
conda install python-graphviz
```

之后，你应该能够使用下面的`dot`命令将 dot 文件转换为 png 文件。

```py
dot -Tpng tree.dot -o tree.png
```

![Figure](../Images/18649899806d0dd0b1e8f157dbcdcf48.png)

通过 conda 在 Windows 上安装 Graphviz。这应该可以解决‘dot’无法识别为内部或外部命令、可操作程序或批处理文件的问题。

**如何通过 Graphviz 可执行文件在 Windows 上安装和使用**

如果你没有 Anaconda 或者只是想要另一种在 Windows 上安装 Graphviz 的方式，你可以使用以下链接[下载并安装](https://graphviz.gitlab.io/_pages/Download/Download_windows.html)。

![Figure](../Images/c2a1388c18dbd5a6823bc0aca9fda749.png)

如果你不熟悉更改 PATH 变量，并且希望在命令行中使用 dot，我建议尝试其他方法。[有关此特定问题的许多 Stackoverflow 问题](https://datascience.stackexchange.com/questions/37428/graphviz-not-working-when-imported-inside-pydotplus-graphvizs-executables-not)。

### 如何使用在线转换器可视化你的决策树

如果一切都失败了或者你根本不想安装任何东西，你可以使用[在线转换器](https://dreampuf.github.io/GraphvizOnline)。

在下图中，我用 Sublime Text 打开了文件（虽然有很多不同的程序可以打开/读取 dot 文件），并复制了文件的内容。

![图示](../Images/1a95f1cbd651e9efc3575ea2b430b338.png)

复制 dot 文件的内容

在下图中，我将 dot 文件的内容粘贴到在线转换器的左侧。然后你可以选择你想要的格式，并在屏幕右侧保存图像。

![图示](../Images/b72e7e6e43ab7a013592d3e78bd70ebe.png)

将可视化保存到计算机

请记住，还有其他的[在线转换器](http://www.webgraphviz.com/)可以帮助完成相同的任务。

### 如何从 Bagged Trees 或随机森林®中可视化单独的决策树

![图示](../Images/15fab36e1c278586ce160b8d42ba9952.png)

本节教程的灵感来源于[Will Koehrsen](https://twitter.com/koehrsen_will)的[如何使用 Scikit-Learn 在 Python 中可视化随机森林算法中的决策树](https://towardsdatascience.com/how-to-visualize-a-decision-tree-from-a-random-forest-in-python-using-scikit-learn-38ad2d75f21c)。图片由[Michael Galarnyk](https://twitter.com/GalarnykMichael)提供。

决策树的一个弱点是它们通常没有最佳的预测准确度。这部分是因为高方差，这意味着训练数据中的不同分裂可能导致非常不同的树。

上图可能是 Bagged Trees 或随机森林算法模型的示意图，这些都是集成方法。这意味着使用多个学习算法来获得比任何单一学习算法更好的预测性能。在这种情况下，许多树保护彼此免受各自错误的影响。Bagged Trees 和随机森林算法模型的具体工作原理是另一个博客的主题，但需要注意的是，对于这两个模型，我们都生长 N 棵树，其中 N 是用户指定的决策树数量。因此，在拟合模型后，查看构成模型的单独决策树是很有意义的。

### 使用 Scikit-Learn 拟合随机森林®模型

为了可视化单独的决策树，我们首先需要使用 scikit-learn 拟合 Bagged Trees 或随机森林®模型（下面的代码拟合了随机森林算法模型）。

```py
# Load the Breast Cancer (Diagnostic) Dataset
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target# Arrange Data into Features Matrix and Target Vector
X = df.loc[:, df.columns != 'target']
y = df.loc[:, 'target'].values# Split the data into training and testing sets
X_train, X_test, Y_train, Y_test = train_test_split(X, y, random_state=0)# Random Forests in `scikit-learn` (with N = 100)
rf = RandomForestClassifier(n_estimators=100,
                            random_state=0)
rf.fit(X_train, Y_train)
```

### 可视化你的估计器

现在你可以查看所有来自拟合模型的单独树。在这一部分，我将使用 matplotlib 可视化所有决策树。

```py
rf.estimators_
```

![图示](../Images/480ebbb631ba1bc333c4e8100f218dc9.png)

在这个例子中，注意我们有 100 个估计器。

现在你可以可视化单独的树。下面的代码可视化了第一棵决策树。

```py
fn=data.feature_names
cn=data.target_names
fig, axes = plt.subplots(nrows = 1,ncols = 1,figsize = (4,4), dpi=800)
tree.plot_tree(rf.estimators_[0],
               feature_names = fn, 
               class_names=cn,
               filled = True);
fig.savefig('rf_individualtree.png')
```

![图示](../Images/1acd00d63266403a70edb44da7223347.png)

请注意，随机森林算法和袋装树中的个别树会生长得很深

你可以尝试使用 matplotlib 子图来可视化你喜欢的任意多的树。下面的代码可视化了前 5 棵决策树。我个人不太喜欢这种方法，因为它更难阅读。

```py
# This may not the best way to view each estimator as it is smallfn=data.feature_names
cn=data.target_names
fig, axes = plt.subplots(nrows = 1,ncols = 5,figsize = (10,2), dpi=3000)for index in range(0, 5):
    tree.plot_tree(rf.estimators_[index],
                   feature_names = fn, 
                   class_names=cn,
                   filled = True,
                   ax = axes[index]);

    axes[index].set_title('Estimator: ' + str(index), fontsize = 11)fig.savefig('rf_5trees.png')
```

![](../Images/f41063731c52ec4a0528728376223cf0.png)

### 为每个决策树（估计器）创建图像

请记住，如果由于某种原因你需要所有估计器（决策树）的图像，你可以使用我在 [GitHub](https://github.com/mGalarnyk/Python_Tutorials/blob/master/Sklearn/CART/Visualization/DecisionTreesVisualization.ipynb) 上的代码。如果你只是想查看本教程中随机森林算法模型拟合的 100 个估计器的每一个，而不运行代码，你可以查看下面的视频。

### 结论

本教程介绍了如何使用 Graphviz 和 Matplotlib 可视化决策树。请注意，使用 Matplotlib 可视化决策树是一种较新的方法，因此未来可能会有所更改或改进。Graphviz 目前更为灵活，因为你总是可以修改 dot 文件，使其更具视觉吸引力，如我所用的 [dot 语言](https://www.graphviz.org/pdf/dotguide.pdf)，甚至可以改变决策树的方向。我们没有涉及的是如何使用 [dtreeviz](https://github.com/parrt/dtreeviz)，这是另一种可视化决策树的库。有关该库的优秀文章可以在 [这里](https://explained.ai/decision-tree-viz/index.html) 查阅。

![图示](../Images/41748ebd82ebd398ef25881f5d8f88b1.png)

图像由 [dtreeviz 库](https://github.com/parrt/dtreeviz) 生成。

如果你对教程有任何问题或想法，请随时在下方评论或通过 [Twitter](https://twitter.com/GalarnykMichael) 联系我。如果你想了解更多关于如何利用 Pandas、Matplotlib 或 Seaborn 库的信息，请考虑参加我的 [数据可视化 Python 课程 LinkedIn Learning](https://www.linkedin.com/learning/python-for-data-visualization/value-of-data-visualization)。

RANDOM FORESTS 和 RANDOMFORESTS 是 Minitab, LLC 的注册商标。

**简介: [迈克尔·加拉尔尼克](https://www.linkedin.com/in/michaelgalarnyk/)** 是一位数据科学家和企业培训师。他目前在Scripps转化研究所工作。你可以在Twitter (https://twitter.com/GalarnykMichael)、Medium (https://medium.com/@GalarnykMichael) 和 GitHub (https://github.com/mGalarnyk) 找到他。

[原文](https://towardsdatascience.com/visualizing-decision-trees-with-python-scikit-learn-graphviz-matplotlib-1c50b4aa68dc)。已获许可转载。

**相关:**

+   [理解 Python 中的分类决策树](/2019/08/understanding-decision-trees-classification-python.html)

+   [决策树算法解析](/2020/01/decision-tree-algorithm-explained.html)

+   [决策树直观理解：从概念到应用](/2020/02/decision-tree-intuition.html)

### 更多相关主题

+   [从零开始的机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)

+   [决策树与随机森林，详解](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [广义和可扩展的最优稀疏决策树（GOSDT）](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [揭开现实世界中的决策树面纱](https://www.kdnuggets.com/demystifying-decision-trees-for-the-real-world)

+   [Python Matplotlib 备忘单](https://www.kdnuggets.com/2023/01/python-matplotlib-cheat-sheets.html)

+   [在 Scikit-learn 中可视化你的混淆矩阵](https://www.kdnuggets.com/2022/09/visualizing-confusion-matrix-scikitlearn.html)
