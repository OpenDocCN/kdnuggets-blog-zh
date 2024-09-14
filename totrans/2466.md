# 决策树算法解析

> 原文：[https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)

![](../Images/3a3912c533fa3be38427762c11a17e10.png)

### 介绍

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT需求

* * *

分类是机器学习中的一个两步过程，包括学习步骤和预测步骤。在学习步骤中，模型根据给定的训练数据进行开发。在预测步骤中，模型用于预测给定数据的响应。决策树是最容易理解和解释的分类算法之一。

### 决策树算法

决策树算法属于监督学习算法的范畴。与其他监督学习算法不同，决策树算法也可以用来解决**回归和分类问题**。

使用决策树的目标是创建一个训练模型，通过**学习从先前数据（训练数据）推断出的简单决策规则**来预测目标变量的类别或值。

在决策树中，为了预测记录的类别标签，我们从**根**开始。我们比较根属性的值与记录的属性。根据比较结果，我们跟随与该值对应的分支，并跳到下一个节点。

### 决策树的类型

决策树的类型基于目标变量的类型。可以分为两种类型：

1.  **分类变量决策树：** 如果决策树具有一个分类目标变量，那么它被称为**分类变量决策树**。

1.  **连续变量决策树：** 如果决策树具有一个连续的目标变量，那么它被称为**连续变量决策树**。

**示例：** 假设我们有一个问题，要预测客户是否会向保险公司支付续保费用（是/否）。在这里，我们知道客户的收入是一个重要变量，但保险公司没有所有客户的收入细节。既然这是一个重要变量，我们可以建立一个决策树，根据职业、产品和各种其他变量来预测客户的收入。在这种情况下，我们预测的是连续变量的值。

### 与决策树相关的重要术语

1.  **根节点：** 它代表整个群体或样本，随后被分为两个或更多同质的集合。

1.  **分割：** 这是将节点分成两个或更多子节点的过程。

1.  **决策节点：** 当一个子节点进一步分裂成子节点时，它被称为决策节点。

1.  **叶节点/终端节点：** 不再分割的节点称为叶节点或终端节点。

1.  **剪枝：** 当我们移除决策节点的子节点时，这个过程称为剪枝。你可以认为这是分割的反过程。

1.  **分支/子树：** 整棵树的一个子部分称为分支或子树。

1.  **父节点和子节点：** 一个被分割成子节点的节点称为子节点的父节点，而子节点是父节点的子节点。

![](../Images/b8fac40aba9c2297b93f56d82a72a308.png)

决策树通过从根节点将示例向下排序到某个叶节点/终端节点来进行分类，叶节点/终端节点提供了示例的分类。

树中的每个节点都充当某些属性的测试用例，节点下的每条边对应于测试用例的可能答案。这个过程是递归的，并且对每个以新节点为根的子树重复。

### 创建决策树时的假设

以下是我们使用决策树时的一些假设：

+   在开始时，整个训练集被视为**根节点**。

+   特征值最好是分类的。如果值是连续的，则在构建模型之前进行离散化。

+   记录**递归分布**在属性值的基础上。

+   属性作为树的根节点或内部节点的排序是通过使用一些统计方法完成的。

决策树遵循**乘积和（SOP）**表示法。乘积和（SOP）也称为**析取范式**。对于一个类别，从树的根到具有相同类别的叶节点的每条分支都是值的合取（乘积），不同的分支以该类别结束，形成析取（和）。

决策树实现中的主要挑战是确定哪些属性需要作为根节点和每一层。处理这个问题的过程称为属性选择。我们有不同的属性选择度量来识别可以在每一层作为根节点的属性。

### 决策树是如何工作的？

制定战略性分割的决策会对树的准确性产生重大影响。分类树和回归树的决策标准不同。

决策树使用多种算法来决定是否将节点分割成两个或更多子节点。子节点的创建增加了结果子节点的同质性。换句话说，我们可以说节点的纯度随着目标变量的增加而增加。决策树在所有可用变量上分割节点，然后选择导致最同质子节点的分割。

算法选择也基于目标变量的类型。让我们看一下决策树中使用的一些算法：

**ID3** →（D3 的扩展）

**C4.5** →（ID3 的继任者）

**CART** →（分类和回归树）

**CHAID** →（卡方自动交互检测，在计算分类树时执行多级拆分）

**MARS** →（多变量自适应回归样条）

ID3 算法通过自顶向下的[贪婪搜索](https://www.hackerearth.com/practice/algorithms/greedy/basics-of-greedy-algorithms/tutorial/)方法构建决策树，不进行回溯。正如其名，贪婪算法总是做出在当下看起来最好的选择。

**ID3 算法中的步骤：**

1.  它从原始集合 S 作为根节点开始。

1.  在算法的每次迭代中，它遍历集合 S 中未使用的属性，并计算该属性的**熵（H）**和**信息增益（IG）**。

1.  然后选择具有最小熵或最大信息增益的属性。

1.  然后通过所选属性将集合 S 划分为数据子集。

1.  算法继续在每个子集上递归，仅考虑从未选择过的属性。

### 属性选择度量

如果数据集包含**N**个属性，则决定将哪个属性放在根节点或树的不同层级作为内部节点是一个复杂的步骤。仅仅随机选择一个节点作为根节点无法解决问题。如果我们遵循随机方法，可能会得到低准确率的不良结果。

为了解决这个属性选择问题，研究人员工作并提出了一些解决方案。他们建议使用一些*标准*，如：

**熵**，

**信息增益**，

**基尼指数**，

**增益比**，

**方差减少**

**卡方**

这些标准将为每个属性计算值。这些值会被排序，属性按照顺序放置在树中，即，具有高值（在信息增益情况下）的属性放置在根节点。

在使用信息增益作为标准时，我们假设属性是分类的，而对于基尼指数，则假设属性是连续的。

### **熵**

熵是处理信息中随机性的度量。熵越高，从信息中得出结论就越困难。掷硬币就是提供随机信息的一个例子。

![](../Images/e47ddfca8b8db435c48ef441aa56a83f.png)

从上面的图表中可以明显看出，当概率为 0 或 1 时，熵 H(X) 为零。当概率为 0.5 时，熵最大，因为它在数据中投射了完美的随机性，并且无法准确确定结果。

> **ID3 遵循规则 — 熵为零的分支是叶节点，熵大于零的分支需要进一步拆分。**

数学上，1个属性的熵表示为：

![](../Images/57204386d55659ed9b46775794a49db7.png)

其中 **S → 当前状态，Pi → 状态 S 或类 *i* 的事件 *i* 的概率，或状态 S 节点中的类 *i* 的百分比。**

数学上，多属性的熵表示为：

![](../Images/a1944b5e9919dde88ddd097cdd6b44d6.png)

其中 **T → 当前状态和 X → 选择的属性**

### **信息增益**

信息增益或 **IG** 是一种统计属性，用于测量给定属性根据其目标分类分离训练样本的效果。构建决策树的核心是找到一个能返回最高信息增益和最小熵的属性。

![图示](../Images/d88b86d888a94376d44650a42cbea64e.png)[信息增益](https://becominghuman.ai/decision-trees-in-machine-learning-f362b296594a?gi=a8ffb5170258)

信息增益是熵的减少。它计算了基于给定属性值的分裂前后的熵差。ID3（迭代二分器）决策树算法使用信息增益。

数学上，IG 表示为：

![](../Images/4c38c19ad3a40772df5891ecee13348e.png)

更简单地说，我们可以得出结论：

![](../Images/4be9c5fa0c7275f79eee4c7096462fb3.png)

[信息增益](https://towardsdatascience.com/from-a-single-decision-tree-to-a-random-forest-b9523be65147)

其中“before”是分裂前的数据集，K是分裂生成的子集数量，(j, after)是分裂后的子集 j。

### 基尼指数

你可以将基尼指数理解为用来评估数据集分裂的成本函数。它通过从1中减去每个类别的平方概率总和来计算。它偏向于较大的分区且易于实现，而信息增益则偏向于较小的具有不同值的分区。

![图示](../Images/6669e4c680091234497fb509d5101542.png)基尼指数

基尼指数适用于类别目标变量“成功”或“失败”。它仅执行二分裂。

> 较高的基尼指数意味着更高的不平等性，更高的异质性。

**计算分裂基尼指数的步骤**

1.  计算子节点的基尼，使用上述公式计算成功（p）和失败（q）的值（p²+q²）。

1.  计算分裂的基尼指数，使用该分裂每个节点的加权基尼得分。

CART（分类与回归树）使用基尼指数方法来创建分裂点。

### 增益比

信息增益偏向于选择具有大量值的属性作为根节点。这意味着它更倾向于选择具有大量不同值的属性。

C4.5，作为 ID3 的改进，使用增益比，这是一种信息增益的修改，减少了其偏倚，通常是最佳选择。增益比通过考虑拆分前产生的分支数量来克服信息增益的问题。它通过考虑拆分的固有信息来修正信息增益。

> 假设我们有一个数据集，其中包含用户和他们的电影类型偏好，基于性别、年龄组、评分等变量。借助信息增益，你可以在‘性别’上进行拆分（假设它具有最高的信息增益），现在‘年龄组’和‘评分’变量可能同样重要，通过增益比的帮助，它会惩罚具有更多不同值的变量，这将帮助我们在下一个层次决定拆分。

![图](../Images/a732854b47d32f5f3cdf5fed4b9fa605.png)[增益比](https://towardsdatascience.com/from-a-single-decision-tree-to-a-random-forest-b9523be65147)

其中“before”是拆分前的数据集，K 是拆分生成的子集的数量，(j, after) 是拆分后子集 j。

### **方差减少**

**方差减少** 是一种用于连续目标变量（回归问题）的算法。该算法使用方差的标准公式来选择最佳拆分。选择方差较低的拆分作为拆分总体的标准：

![](../Images/bb8247ef89486499b2dbcc281977b898.png)

上面的 X-bar 是值的均值，X 是实际值，n 是值的数量。

**计算方差的步骤：**

1.  计算每个节点的方差。

1.  计算每个拆分的方差，作为每个节点方差的加权平均值。

### **卡方**

缩写 CHAID 代表 *卡方* 自动交互检测器。它是最古老的树分类方法之一。它找出子节点和父节点之间差异的统计显著性。我们通过目标变量的观察频率和期望频率之间的标准化差异的平方和来衡量它。

它适用于分类目标变量“成功”或“失败”。它可以执行两个或更多的拆分。卡方值越高，子节点和父节点之间差异的统计显著性越高。

它生成一个称为 CHAID（卡方自动交互检测器）的树。

从数学上讲，卡方被表示为：

![](../Images/c6e29fe0d58d993aae89fa9762398c2f.png)

**计算拆分卡方的步骤：**

1.  通过计算成功和失败的偏差来计算单个节点的卡方。

1.  通过计算拆分后每个节点的成功和失败的所有卡方值的总和来计算拆分的卡方值。

### **如何避免/对抗决策树中的过拟合？**

决策树的常见问题，特别是当表中列很多时，它们的拟合程度很高。有时，看起来树似乎记住了训练数据集。如果决策树没有设置限制，它会在训练数据集上提供100%的准确率，因为在最坏的情况下，它会为每个观察值生成一个叶子节点。因此，这会影响对训练集之外样本的预测准确性。

这里有两种去除过拟合的方法：

1.  剪枝决策树。

1.  随机森林

**剪枝决策树**

分裂过程会生成完全生长的树，直到达到停止标准。但完全生长的树很可能会过拟合数据，从而在未见数据上的准确性较差。

![图示](../Images/1c59703d8f644408230aa4c6ea5e4684.png)[剪枝的实际应用](https://gfycat.com/enchantedyellowishbarasinga)

在**剪枝**中，你会修剪树的分支，即从叶子节点开始移除决策节点，以确保总体准确性不受影响。这是通过将实际训练集分成两个集合来完成的：训练数据集D和验证数据集V。使用分隔后的训练数据集D来准备决策树。然后继续修剪树，以优化验证数据集V的准确性。

![图示](../Images/7bb957cdb54435ba9af5e032a34af199.png)[剪枝](https://www.cs.cmu.edu/~bhiksha/courses/10-601/decisiontrees/)

在上述图示中，树的左侧的‘年龄’属性已被剪枝，因为它在树的右侧更重要，因此去除了过拟合。

**随机森林**

随机森林是集成学习的一个例子，在这种学习中，我们结合多个机器学习算法以获得更好的预测性能。

**为什么叫“随机”？**

赋予其“随机”名称的两个关键概念：

1.  在构建树时，随机抽样训练数据集。

1.  分裂节点时考虑的特征随机子集。

一种称为“自助法”（bagging）的技术用于创建一个决策树集成模型，其中多个训练集通过有放回的抽样生成。

在自助法技术中，数据集被划分为**N**个样本，使用随机抽样。然后，使用单一学习算法在所有样本上建立模型。之后，通过投票或平均的方式将结果预测合并在一起。

![图示](../Images/9a79dc5b0e44ede04284ddcb44888a0a.png)[随机森林的实际应用](https://towardsdatascience.com/why-random-forests-outperform-decision-trees-1b0f175a0b5)

### 线性模型还是基于树的模型哪个更好？

这要看你正在解决的是什么问题。

1.  如果因变量与自变量之间的关系可以通过线性模型很好地近似，线性回归将优于基于树的模型。

1.  如果因变量与自变量之间有高度非线性和复杂关系，树模型将优于经典的回归方法。

1.  如果你需要建立一个容易向人们解释的模型，决策树模型总比线性模型更好。决策树模型甚至比线性回归更易于解释！

### 使用Scikit-learn构建决策树分类器

我们拥有的数据集是超市数据，可以从[这里](https://drive.google.com/open?id=1x1KglkvJxNn8C8kzeV96YePFnCUzXhBS)下载。

加载所有基本库。

```py
import numpy as np
import matplotlib.pyplot as plt 
import pandas as pd
```

加载数据集。它包括5个特征：`UserID`、`Gender`、`Age`、`EstimatedSalary` 和 `Purchased`。

```py
data = pd.read_csv('/Users/ML/DecisionTree/Social.csv')
data.head()
```

![图](../Images/3b51f398bfcc049794918ba1ef132e40.png) 数据集

我们将仅使用 `Age` 和 `EstimatedSalary` 作为我们的自变量 `X`，因为像 `Gender` 和 `User ID` 这样的其他特征与个人的购买能力无关且没有影响。Purchased 是我们的因变量 `y`。

```py
feature_cols = ['Age','EstimatedSalary' ]X = data.iloc[:,[2,3]].values
y = data.iloc[:,4].values
```

下一步是将数据集拆分为训练集和测试集。

```py
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test =  train_test_split(X,y,test_size = 0.25, random_state= 0)
```

执行特征缩放

```py
#feature scaling
from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
X_train = sc_X.fit_transform(X_train)
X_test = sc_X.transform(X_test)
```

在决策树分类器中拟合模型。

```py
from sklearn.tree import DecisionTreeClassifier
classifier = DecisionTreeClassifier()
classifier = classifier.fit(X_train,y_train)
```

做出预测并检查准确性。

```py
#prediction
y_pred = classifier.predict(X_test)#Accuracy
from sklearn import metricsprint('Accuracy Score:', metrics.accuracy_score(y_test,y_pred))
```

决策树分类器的准确率为91%。

混淆矩阵

```py
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, y_pred)Output:
array([[64,  4],
       [ 2, 30]])
```

这意味着6个观察结果被归类为虚假。

**让我们首先可视化模型预测结果。**

```py
from matplotlib.colors import ListedColormap
X_set, y_set = X_test, y_test
X1, X2 = np.meshgrid(np.arange(start = X_set[:,0].min()-1, stop= X_set[:,0].max()+1, step = 0.01),np.arange(start = X_set[:,1].min()-1, stop= X_set[:,1].max()+1, step = 0.01))
plt.contourf(X1,X2, classifier.predict(np.array([X1.ravel(), X2.ravel()]).T).reshape(X1.shape), alpha=0.75, cmap = ListedColormap(("red","green")))plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())for i,j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set==j,0],X_set[y_set==j,1], c = ListedColormap(("red","green"))(i),label = j)
plt.title("Decision Tree(Test set)")
plt.xlabel("Age")
plt.ylabel("Estimated Salary")
plt.legend()
plt.show()
```

![](../Images/ddd705134cdcc0f99c1814fe4b72041a.png)

**让我们也可视化一下这棵树：**

你可以使用Scikit-learn的 *export_graphviz* 函数在Jupyter notebook中显示树。为了绘制树，你还需要安装Graphviz和pydotplus。

`conda install python-graphviz`

`pip install pydotplus`

*export_graphviz* 函数将决策树分类器转换为dot文件，pydotplus 将此dot文件转换为png或在Jupyter中可显示的形式。

```py
from sklearn.tree import export_graphviz
from sklearn.externals.six import StringIO  
from IPython.display import Image  
import pydotplusdot_data = StringIO()
export_graphviz(classifier, out_file=dot_data,  
                filled=True, rounded=True,
                special_characters=True,feature_names = feature_cols,class_names=['0','1'])
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())  
Image(graph.create_png())
```

![图](../Images/7f1f7d8aa6d9c4a09aa5e79ec82e76a1.png) 决策树。

在决策树图表中，每个内部节点都有一个决策规则来分割数据。基尼指数（Gini）被称为基尼比率，用来衡量节点的不纯度。可以说，当所有记录都属于同一类别时，节点是纯的，这种节点称为叶节点。

在这里，结果树是未经修剪的。这个未经修剪的树是不可解释的，且不容易理解。在下一节中，我们将通过修剪来优化它。

**优化决策树分类器**

**criterion**：可选（默认值为“gini”）或 选择属性选择度量：此参数允许我们使用不同的属性选择度量。支持的标准有“gini”用于基尼指数和“entropy”用于信息增益。

**splitter**：字符串， 可选（默认值为“best”）或 分割策略：这个参数允许我们选择分割策略。支持的策略有“best”以选择最佳分割和“random”以选择最佳随机分割。

**max_depth**：int 或 None，可选（默认=None）或树的最大深度：树的最大深度。如果为 None，则节点扩展直到所有叶子包含的样本数少于 min_samples_split。较高的最大深度值会导致过拟合，而较低的值会导致欠拟合（来源）。

在 Scikit-learn 中，决策树分类器的优化仅通过预剪枝进行。树的最大深度可以作为预剪枝的控制变量。

```py
# Create Decision Tree classifer object
classifier = DecisionTreeClassifier(criterion="entropy", max_depth=3)# Train Decision Tree Classifer
classifier = classifier.fit(X_train,y_train)#Predict the response for test dataset
y_pred = classifier.predict(X_test)# Model Accuracy, how often is the classifier correct?
print("Accuracy:",metrics.accuracy_score(y_test, y_pred))
```

好的，分类率提高到了94%，比之前的模型准确度更高。

现在，让我们在优化后再次可视化剪枝后的决策树。

```py
dot_data = StringIO()
export_graphviz(classifier, out_file=dot_data,  
                filled=True, rounded=True,
                special_characters=True, feature_names = feature_cols,class_names=['0','1'])
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())  
Image(graph.create_png())
```

![图像](../Images/c76148e38059f19cd1326993c049ed16.png)剪枝后的决策树

这个剪枝模型比之前的决策树模型图更简单、可解释且易于理解。

### 结论

在这篇文章中，我们涵盖了关于决策树的许多细节；它的工作原理、属性选择度量如信息增益、增益比和基尼指数，决策树模型的构建、可视化和在超市数据集上使用 Python Scikit-learn 包的评估，以及通过参数调整优化决策树性能。

好了，这篇文章就到此为止，希望大家喜欢阅读，欢迎在评论区分享你的评论/想法/反馈。

![图像](../Images/fe29fca4d34cf017cf70bdb1fde78042.png)[来源](https://www.ilikesticker.com/LineStickerAnimation/W433402-Movie-End-Credits-Style-[English-Ver]/en)

感谢阅读!!!

**简介：[Nagesh Singh Chauhan](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是数据科学爱好者。对大数据、Python 和机器学习感兴趣。

[原文](https://towardsdatascience.com/decision-tree-algorithm-explained-83beb6e78ef4)。已获转载许可。

### 更多相关主题

+   [用 Python 和 Scikit-learn 简化决策树可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [通过实现理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [讲述精彩数据故事：可视化决策树](https://www.kdnuggets.com/2021/02/telling-great-data-story-visualization-decision-tree.html)

+   [随机森林与决策树：关键差异](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [KDnuggets™ 新闻 22:n09，3 月 2 日：讲述精彩数据故事：A…](https://www.kdnuggets.com/2022/n09.html)

+   [决策树软件完整指南](https://www.kdnuggets.com/2022/08/complete-guide-decision-tree-software.html)
