# 机器学习的学习曲线

> 原文：[https://www.kdnuggets.com/2018/01/learning-curves-machine-learning.html/2](https://www.kdnuggets.com/2018/01/learning-curves-machine-learning.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/01/learning-curves-machine-learning.html?page=2#comments)

### `scikit-learn`中的`learning_curve()`函数

我们将使用`learning_curve()` [函数](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.learning_curve.html) 从`scikit-learn`库生成回归模型的学习曲线。我们不需要单独划分验证集，因为`learning_curve()`会处理这一点。

在下面的代码单元格中，我们：

+   从`sklearn`中进行必要的导入。

+   声明特征和目标。

+   使用`learning_curve()`生成绘制学习曲线所需的数据。该函数返回一个包含三个元素的元组：训练集大小，以及验证集和训练集上的错误评分。在函数内部，我们使用以下参数：

    +   `estimator`— 指示我们用来估计真实模型的学习算法；

    +   `X`— 包含特征的数据；

    +   `y`— 包含目标的数据；

    +   `train_sizes`— 指定要使用的训练集大小；

    +   `cv`— 确定交叉验证拆分策略（我们将立即讨论这一点）；

    +   `scoring`— 指定使用的错误度量；目标是使用均方误差（MSE）度量，但`scoring`中没有这个参数；我们将使用最接近的代理，即负MSE，稍后需要翻转符号。

```py
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import learning_curve

features = ['AT', 'V', 'AP', 'RH']
target = 'PE'

train_sizes, train_scores, validation_scores = learning_curve(
                                                   estimator = LinearRegression(), X = electricity[features],
                                                   y = electricity[target], train_sizes = train_sizes, cv = 5,
                                                   scoring = 'neg_mean_squared_error')
```

我们已经知道了`train_sizes`中的内容。现在让我们检查另外两个变量，看看`learning_curve()`返回了什么：

```py
print('Training scores:\n\n', train_scores)
print('\n', '-' * 70) # separator to make the output easy to read
print('\nValidation scores:\n\n', validation_scores)
```

```py
Training scores:

 [[ -0\.          -0\.          -0\.          -0\.          -0\.        ]
 [-19.71230701 -18.31492642 -18.31492642 -18.31492642 -18.31492642]
 [-18.14420459 -19.63885072 -19.63885072 -19.63885072 -19.63885072]
 [-21.53603444 -20.18568787 -19.98317419 -19.98317419 -19.98317419]
 [-20.47708899 -19.93364211 -20.56091569 -20.4150839  -20.4150839 ]
 [-20.98565335 -20.63006094 -21.04384703 -20.63526811 -20.52955609]]

 ----------------------------------------------------------------------

Validation scores:

 [[-619.30514723 -379.81090366 -374.4107861  -370.03037109 -373.30597982]
 [ -21.80224219  -23.01103419  -20.81350389  -22.88459236  -23.44955492]
 [ -19.96005238  -21.2771561   -19.75136596  -21.4325615   -21.89067652]
 [ -19.92863783  -21.35440062  -19.62974239  -21.38631648  -21.811031  ]
 [ -19.88806264  -21.3183303   -19.68228562  -21.35019525  -21.75949097]
 [ -19.9046791   -21.33448781  -19.67831137  -21.31935146  -21.73778949]]
```

由于我们指定了六个训练集大小，你可能期望每种评分都有六个值。相反，我们得到了六行数据，每行有五个错误评分。

之所以会发生这种情况，是因为`learning_curve()`在后台执行了`k`-折交叉验证，其中`k`的值由我们为`cv`参数指定的值决定。

在我们的例子中，`cv = 5`，所以会有五次切分。对于每个切分，都会为每个指定的训练集大小训练一个估计器。上面两个数组中的每列表示一个切分，每行对应一个测试大小。下面是一个用于训练误差评分的表格，以帮助你更好地理解过程：

| 训练集大小（索引） | 切分1 | 切分2 | 切分3 | 切分4 | 切分5 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 100 | -19.71230701 | -18.31492642 | -18.31492642 | -18.31492642 | -18.31492642 |
| 500 | -18.14420459 | -19.63885072 | -19.63885072 | -19.63885072 | -19.63885072 |
| 2000 | -21.53603444 | -20.18568787 | -19.98317419 | -19.98317419 | -19.98317419 |
| 5000 | -20.47708899 | -19.93364211 | -20.56091569 | -20.4150839 | -20.4150839 |
| 7654 | -20.98565335 | -20.63006094 | -21.04384703 | -20.63526811 | -20.52955609 |

要绘制学习曲线，我们只需要每个训练集大小的一个误差评分，而不是5个。因此，在下一个代码单元格中，我们取每一行的平均值，并翻转误差评分的符号（如上所述）。

```py
train_scores_mean = -train_scores.mean(axis = 1)
validation_scores_mean = -validation_scores.mean(axis = 1)

print('Mean training scores\n\n', pd.Series(train_scores_mean, index = train_sizes))
print('\n', '-' * 20) # separator
print('\nMean validation scores\n\n',pd.Series(validation_scores_mean, index = train_sizes))
```

```py
Mean training scores

 1       -0.000000
100     18.594403
500     19.339921
2000    20.334249
5000    20.360363
7654    20.764877
dtype: float64

 --------------------

Mean validation scores

 1       423.372638
100      22.392186
500      20.862362
2000     20.822026
5000     20.799673
7654     20.794924
dtype: float64
```

现在我们拥有绘制学习曲线所需的所有数据。

然而，在进行绘图之前，我们需要暂停并做一个重要的观察。你可能注意到一些*训练*集上的误差评分是相同的。对于训练集大小为1的行，这是预期的，但其他行呢？除了最后一行，我们有很多相同的值。例如，取第二行，从第二次拆分开始的值是相同的。为什么会这样？

这是由于没有为每次拆分随机化*训练*数据。我们通过下面的图示来逐步了解一个示例。当训练集的大小为500时，前500个实例被选中。对于第一次拆分，这500个实例将从第二个数据块中选取。从第二次拆分开始，这500个实例将从第一个数据块中选取。因为我们没有随机化训练集，所以第二次拆分之后用于训练的500个实例是相同的。这解释了500个训练实例的情况下，从第二次拆分开始相同的值。

相同的推理适用于100个实例的情况，类似的推理适用于其他情况。

![splits](../Images/edfe52b4e23dc00451643d1c2c6ac0a8.png)

要停止这种行为，我们需要将`shuffle`参数设置为`True`，以便在`learning_curve()`函数中对*训练*数据的每次拆分进行随机化。我们之前没有进行随机化有两个原因：

+   数据已经预先洗牌了五次（如[文档](https://archive.ics.uci.edu/ml/datasets/Combined+Cycle+Power+Plant)中提到的），所以不需要再随机化了。

+   我想让你知道这个怪癖，以防你在实践中遇到它。

最后，让我们进行绘图。

### 学习曲线 - 高偏差和低方差

我们使用常规的matplotlib工作流绘制学习曲线：

```py
import matplotlib.pyplot as plt
%matplotlib inline

plt.style.use('seaborn')

plt.plot(train_sizes, train_scores_mean, label = 'Training error')
plt.plot(train_sizes, validation_scores_mean, label = 'Validation error')

plt.ylabel('MSE', fontsize = 14)
plt.xlabel('Training set size', fontsize = 14)
plt.title('Learning curves for a linear regression model', fontsize = 18, y = 1.03)
plt.legend()
plt.ylim(0,40)
```

```py
(0, 40)
```

![Learning_curves_12_1](../Images/d08412056c4965911fcf9e9506f74f34.png)

从这个图中我们可以提取很多信息。让我们详细地进行分析。

当训练集大小为1时，我们可以看到训练集的MSE为0。这是正常行为，因为模型对一个数据点的拟合没有问题。因此，当在同一个数据点上进行测试时，预测是完美的。

但在验证集（包含1914个实例）上测试时，均方误差（MSE）飙升至大约423.4。这一相对较高的值是我们将y轴范围限制在0到40之间的原因。这使我们能够更精确地读取大多数MSE值。如此高的值是可以预期的，因为模型在单一数据点上训练，不太可能对1914个在训练中未见过的新实例做出准确的泛化。

当训练集大小增加到100时，训练MSE急剧上升，而验证MSE也同样下降。线性回归模型没有完美预测所有100个训练点，因此训练MSE大于0。然而，模型在验证集上的表现现在好多了，因为它使用了更多的数据进行估计。

从500个训练数据点开始，验证MSE大致保持不变。这告诉我们一个极其重要的事情：添加更多的训练数据点不会显著改善模型。因此，我们需要尝试其他方法，比如切换到可以构建更复杂模型的算法，而不是浪费时间（可能还会浪费金钱）去收集更多数据。

![add_data](../Images/a32f9b2090fa9861adde627057f852a2.png)

为了避免误解，重要的是注意到真正没有帮助的是在训练数据中添加更多的*实例*（行）。然而，添加更多的特征是另一回事，很可能会有所帮助，因为这会增加我们当前模型的复杂性。

现在让我们来诊断偏差和方差。偏差问题的主要指标是高验证错误。在我们的案例中，验证MSE停滞在大约20的值。但这有多好呢？我们需要一些领域知识（在这个情况下可能是物理学或工程学）来回答这个问题，但我们还是尝试一下。

从技术上讲，20的值单位是MW²（兆瓦平方）（单位也会平方[当我们计算MSE时](https://en.wikipedia.org/wiki/Mean_squared_error#Predictor)）。但我们目标列中的值是以MW为单位（根据[文档](https://archive.ics.uci.edu/ml/datasets/Combined+Cycle+Power+Plant)）。取20 MW²的平方根约为4.5 MW。每个目标值代表净*小时*电力输出。因此，每小时我们的模型平均偏差为4.5 MW。根据[这个Quora回答](https://www.quora.com/How-can-I-get-an-intuitive-understanding-of-what-a-Kw-Mw-Gw-of-electricity-equates-to-in-real-life-terms)，4.5 MW相当于4500个手持吹风机所产生的热功率。如果我们尝试预测一天或更长时间的总能量输出，这个值将会累积。

我们可以得出结论，20 MW²的MSE相当大。因此，我们的模型存在偏差问题。但是这是一个*低*偏差问题还是一个*高*偏差问题？

要找到答案，我们需要查看训练误差。如果训练误差非常低，这意味着估计模型对训练数据的拟合非常好。如果模型对训练数据的拟合非常好，这意味着它对该数据集的偏差很*低*。

如果训练误差很高，这意味着估计模型对训练数据的拟合不够好。如果模型无法很好地拟合训练数据，这意味着它对该数据集的偏差很*高*。

![low_high_bias](../Images/2b7be8d34b94b5b41f0bd0156ff9d9a1.png)

在我们特定的案例中，训练 MSE 平稳在大约 20 MW22 的值。正如我们已经确定的，这是一个高误差分数。由于验证 MSE 很高，而训练 MSE 也很高，我们的模型存在高偏差问题。

现在让我们继续诊断最终的方差问题。估计方差至少有两种方法：

+   通过检查验证学习曲线和训练学习曲线之间的差距。

+   通过检查训练误差：其值及随着训练集大小增加的演变。

![lc_regression](../Images/6a3d18a5b15c8d34fd28790435933b1a.png)

窄的差距表示低方差。一般来说，差距越窄，方差越低。反之亦然：差距越宽，方差越大。现在让我们解释为什么会这样。

正如我们之前讨论的，如果方差很高，则模型对训练数据拟合得过好。当训练数据拟合得过好时，模型将难以对未在训练中见过的数据进行泛化。当这样的模型在其训练集上进行测试，然后在验证集上进行测试时，训练误差将很低，而验证误差通常会很高。随着训练集大小的变化，这种模式会持续下去，训练误差和验证误差之间的差异将决定两条学习曲线之间的差距。

训练误差和验证误差之间的关系可以总结如下：

gap=验证误差−训练误差gap=验证误差−训练误差

因此，两种误差之间的差异越大，差距也越大。差距越大，方差也越大。

在我们的案例中，差距非常小，因此我们可以放心地得出方差很低的结论。

高*训练* MSE 分数也是检测低方差的快捷方式。如果学习算法的方差很低，那么随着训练集的变化，算法会产生简化且类似的模型。由于模型过于简化，它们甚至无法很好地拟合训练数据（它们*欠拟合*了数据）。因此，我们应该预期高训练 MSE。因此，高训练 MSE 可以用作低方差的指示器。

![low_high_var](../Images/31c509512a7d66c68d59887ac5c7830a.png)

在我们的案例中，训练的均方误差（MSE）在大约20处趋于平稳，我们已经得出结论这是一个较高的值。因此，除了狭窄的差距，我们现在还有另一个确认表明我们有一个低方差问题。

到目前为止，我们可以得出结论：

+   我们的学习算法存在高偏差和低方差的问题，对训练数据进行欠拟合。

+   在当前学习算法下，向训练数据中添加更多实例（行）极不可能导致更好的模型。

此时的一个解决方案是更改为更复杂的学习算法。这应该能减少偏差并增加方差。错误的做法是尝试增加训练实例的数量。

通常，这另外两种修复方法也适用于处理高偏差和低方差问题：

+   在更多特征上训练当前学习算法（为了避免*收集*新数据，你可以轻松生成[多项式特征](http://scikit-learn.org/stable/modules/preprocessing.html)）。这应该通过增加模型的复杂性来降低偏差。

+   减少当前学习算法的[正则化](https://www.quora.com/What-is-regularization-in-machine-learning)，如果是这种情况。简而言之，正则化防止算法对训练数据过于拟合。如果我们减少正则化，模型将更好地拟合训练数据，因此方差将增加，偏差将减少。

### 学习曲线 - 低偏差和高方差

让我们看看一个未正则化的随机森林回归器在这里的表现。我们将使用与上述相同的工作流程生成学习曲线。这次我们将所有内容打包成一个函数，以便以后使用。为了进行比较，我们还将展示线性回归模型的学习曲线。

```py
### Bundling our previous work into a function ###

def learning_curves(estimator, data, features, target, train_sizes, cv):
    train_sizes, train_scores, validation_scores = learning_curve(
                                                 estimator, data[features], data[target], train_sizes = train_sizes,
                                                 cv = cv, scoring = 'neg_mean_squared_error')
    train_scores_mean = -train_scores.mean(axis = 1)
    validation_scores_mean = -validation_scores.mean(axis = 1)

    plt.plot(train_sizes, train_scores_mean, label = 'Training error')
    plt.plot(train_sizes, validation_scores_mean, label = 'Validation error')

    plt.ylabel('MSE', fontsize = 14)
    plt.xlabel('Training set size', fontsize = 14)
    title = 'Learning curves for a ' + str(estimator).split('(')[0] + ' model'
    plt.title(title, fontsize = 18, y = 1.03)
    plt.legend()
    plt.ylim(0,40)

### Plotting the two learning curves ###

from sklearn.ensemble import RandomForestRegressor

plt.figure(figsize = (16,5))

for model, i in [(RandomForestRegressor(), 1), (LinearRegression(),2)]:
    plt.subplot(1,2,i)
    learning_curves(model, electricity, features, target, train_sizes, 5)
```

![学习曲线_15_0](../Images/543485dfb23baba95c68632d5c624a0a.png)

现在让我们尝试应用我们刚刚学到的知识。此时暂停阅读并尝试自己解释新的学习曲线会是个好主意。

通过查看验证曲线，我们可以看到我们已经设法减少了偏差。虽然仍然存在一些显著的偏差，但比之前少了很多。通过查看训练曲线，我们可以推测这次存在一个*低*偏差问题。

两条学习曲线之间的新差距表明方差显著增加。低的训练均方误差（MSE）证实了这一高方差的诊断。

大的差距和低的训练误差也表明存在过拟合问题。过拟合发生在模型在训练集上表现良好，但在测试（或验证）集上表现较差时。

我们可以在这里做出一个更重要的观察，即*添加新的训练实例*很可能会导致更好的模型。验证曲线没有在使用的最大训练集大小处趋于平稳。它仍然有潜力降低并趋近于训练曲线，类似于我们在线性回归情况下看到的收敛情况。

到目前为止，我们可以得出结论：

+   我们的学习算法（随机森林）在训练数据上过拟合，表现出较高的方差和较低的偏差。

+   在当前学习算法下，增加更多的训练实例很可能会导致更好的模型。

到目前为止，我们可以做几件事情来改进我们的模型：

+   增加更多的训练实例。

+   增加我们当前学习算法的正则化。这应该会减少方差并增加偏差。

+   减少我们当前使用的训练数据中的特征数量。算法仍然会很好地拟合训练数据，但由于特征数量的减少，它会构建较简单的模型。这应该会增加偏差并减少方差。

在我们的案例中，我们没有其他现成的数据。我们可以去电厂进行一些测量，但我们将留到另一个帖子中（开个玩笑）。

不如尝试对我们的随机森林算法进行正则化。实现这一点的一种方法是调整每棵决策树中的最大叶节点数。可以通过使用 `RandomForestRegressor()` 的 `max_leaf_nodes` 参数来完成。你不必理解这种正则化技术。对于我们目前的目的，你需要关注的是这种正则化对学习曲线的影响。

```py
learning_curves(RandomForestRegressor(max_leaf_nodes = 350), electricity, features, target, train_sizes, 5)
```

![Learning_curves_17_0](../Images/e2668882d10fe739e9a86d9f5d803409.png)

不错！现在差距缩小了，因此方差减少了。偏差似乎稍微增加了一点，这正是我们想要的。

但我们的工作还远未结束！验证 MSE 仍显示出很大的下降潜力。你可以采取的一些步骤包括：

+   增加更多的训练实例。

+   增加更多的特征。

+   特征选择。

+   超参数优化。

### 理想的学习曲线和不可减少的误差

学习曲线是一个很好的工具，可以在机器学习工作流中的每一个点上快速检查我们的模型。但是我们如何知道何时停止？我们如何识别完美的学习曲线？

对于我们之前的回归案例，你可能会认为完美的情境是当两条曲线都收敛到 MSE 为 0\. 这确实是一个完美的情境，但不幸的是，这在实践中和理论上都不可能。原因是所谓的*不可减少的误差*。

当我们构建一个模型来映射特征***X***和目标***Y***之间的关系时，我们首先假设存在这样的关系。如果假设是正确的，那么确实存在一个完美描述***X***和***Y***之间关系的真实模型 ![Equation](../Images/0124e78268c96059dbff35809fb67ffe.png)，如下面所示：

![Equation](../Images/0617d3c146a95ad60f2d2561bd93af9b.png)     (1)

那么为什么会有错误呢？！我们不是刚刚说过 ![Equation](../Images/0124e78268c96059dbff35809fb67ffe.png) 完美地描述了 X 和 Y 之间的关系吗？！

这里有一个错误，因为***Y***不仅是我们有限特征***X***的函数。可能还有许多其他特征影响***Y***的值。我们没有这些特征。也可能***X***包含测量误差。因此，除了***X***之外，***Y***也是*不可约误差*的函数。

现在让我们解释为什么这种误差是*不可约的*。当我们用模型![Equation](../Images/e95a915c8e775f7ecfcf46075ea77dec.png)估计***f(X)***时，我们引入了另一种误差，称为*可约误差*：

![Equation](../Images/d87907e57e1aca8903ee05fe75b060c0.png)     (2)

在（1）中替换***f(X)***，我们得到：

![Equation](../Images/f4958e321c98d383fbf90c05a31e2410.png)     (3)

可约误差可以通过构建更好的模型来减少。从方程（2）中我们可以看到，如果可约误差为0，我们估计的模型![Equation](../Images/e95a915c8e775f7ecfcf46075ea77dec.png)等于真实模型***f(X)***。然而，从（3）中我们可以看到，即使*可约误差*为0，*不可约误差*仍然存在于方程中。因此，我们推断无论我们的模型估计有多好，通常仍然存在一些我们无法减少的误差。这就是为什么这种误差被认为是*不可约*的原因。

这告诉我们，在实际操作中，我们看到的最佳学习曲线是那些收敛到某种不可约误差值的曲线，而不是收敛到某种理想误差值（对于均方误差，理想的误差值是0；我们会立即看到其他误差度量有不同的理想误差值）。

![irr_error](../Images/511282f6512b6f0ce0e5e0d5af5d4a23.png)

在实践中，无法准确知道不可约误差的值。我们还假设不可约误差与***X***无关。这意味着我们不能使用***X***来找到不可约误差的真实值。用更精确的数学语言表达，就是没有函数***g***可以将***X***映射到不可约误差的真实值：

*不可约误差 ≠ g(X)*

因此，根据我们拥有的数据无法知道不可约误差的真实值。在实践中，一个好的方法是尽可能降低误差分数，同时牢记限制由某种不可约误差决定。

### 那么分类呢？

到目前为止，我们了解了回归设置中的学习曲线。对于分类任务，工作流程几乎相同。主要的区别在于我们需要选择另一种误差度量——一种适合评估分类器性能的度量。我们来看一个例子：

![classification](../Images/f0f1d038900ad67bb1b9b5c16f88d42a.png)

来源：[scikit-learn documentation](http://scikit-learn.org/stable/auto_examples/model_selection/plot_learning_curve.html)

与我们迄今为止看到的不同，注意训练误差的学习曲线高于验证误差的曲线。这是因为使用的分数，*准确性*，描述了模型的好坏。准确性越高，模型越好。而均方误差（MSE）则描述了模型的差劲程度。均方误差越低，模型越好。

这也对不可减少的错误有影响。对于描述模型有多差的误差度量，不可减少的错误提供了一个下界：你不能低于这个值。对于描述模型有多好的误差度量，不可减少的错误提供了一个上界：你不能高于这个值。

这里补充一点，在更技术性的写作中，通常使用[*贝叶斯误差率*](https://en.wikipedia.org/wiki/Bayes_error_rate)来指代分类器的最佳可能错误分数。这个概念类似于不可减少的错误。

### 下一步

学习曲线是诊断任何监督学习算法中的偏差和方差的绝佳工具。我们已经学习了如何使用scikit-learn和matplotlib生成它们，并如何利用它们来诊断我们模型中的偏差和方差。

为了巩固你所学到的内容，以下是一些值得考虑的下一步：

+   使用不同的数据集生成回归任务的学习曲线。

+   生成分类任务的学习曲线。

+   通过从头开始编写代码（不要使用`learning_curve()`来自scikit-learn），生成监督学习任务的学习曲线。使用交叉验证是可选的。

+   比较没有进行交叉验证和使用交叉验证获得的学习曲线。这两种曲线应针对相同的学习算法。

**简介: [亚历克斯·奥尔特亚努](https://www.dataquest.io/blog/author/alex-olteanu/)** 是Dataquest.io的学生成功专家。他喜欢学习和分享知识，并为新的AI革命做好准备。

[原文](https://www.dataquest.io/blog/learning-curves-machine-learning/?utm_source=kdnuggets&utm_medium=blog)。经授权转载。

**相关内容:**

+   [机器学习中的正则化](/2018/01/regularization-machine-learning.html)

+   [如何在Python中生成FiveThirtyEight图表](/2017/12/generate-fivethirtyeight-graphs-python.html)

+   [训练集、测试集和10折交叉验证](/2018/01/training-test-sets-cross-validation.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

### 更多相关话题

+   [KDnuggets 新闻，12月14日：3 个免费机器学习课程](https://www.kdnuggets.com/2022/n48.html)

+   [每个机器学习工程师都应该掌握的5个机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [学习数据科学、机器学习和深度学习的稳固计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习…](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [突破数据障碍：如何通过零样本、单样本和少样本…](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [联邦学习：带教程的协作机器学习入门…](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)
