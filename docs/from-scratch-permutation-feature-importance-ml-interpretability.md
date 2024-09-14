# 从头开始：用于机器学习解释性的置换特征重要性

> 原文：[https://www.kdnuggets.com/2021/06/from-scratch-permutation-feature-importance-ml-interpretability.html](https://www.kdnuggets.com/2021/06/from-scratch-permutation-feature-importance-ml-interpretability.html)

**作者：[Seth Billiau](https://www.linkedin.com/in/seth-billiau-b4b062136/)，数据科学家和统计学家**

![](../Images/b22185d12db44de4fd434cebf30c3851.png)

图片由[Arno Senoner](https://unsplash.com/@arnosenoner)提供，来源于[Unsplash](https://unsplash.com/photos/ZxA9vV0phGI)

### 介绍

高级机器学习主题通常被黑箱模型主导。顾名思义，黑箱模型是复杂模型，很难理解模型输入如何组合以进行预测。深度学习模型如[人工神经网络](https://link.springer.com/article/10.1007/s12518-021-00360-9)和集成模型如[随机森林](https://towardsdatascience.com/understanding-random-forest-58381e0602d2)、梯度提升学习者和[模型堆叠](https://machinelearningmastery.com/stacking-ensemble-machine-learning-with-python/)是黑箱模型的例子，它们在从[城市规划](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6567884/)到[计算机视觉](https://medium.com/swlh/computer-vision-with-convolutional-neural-networks-22f06360cac9)等多个领域提供了极其准确的预测。

![](../Images/8975079b777cc8ef90ea8076241d93cc.png)

黑箱模型的示意图

然而，使用这些黑箱模型的一个缺点是，很难解释预测因子如何影响预测结果——尤其是使用传统统计方法时。本文将解释一种解释黑箱模型的替代方法，称为置换特征重要性。置换特征重要性是一个强大的工具，允许我们检测数据集中哪些特征具有预测能力，无论我们使用什么模型。

我们将首先讨论传统统计推断与特征重要性之间的差异，以激发对置换特征重要性的需求。然后，我们将解释置换特征重要性并从头开始实现它，以发现哪些预测因子对预测Blotchville的房价很重要。最后，我们将讨论这种方法的一些缺点，并介绍一些未来可以帮助我们进行置换特征重要性的包。

### 统计推断与特征重要性

在使用传统的参数统计模型时，我们可以依靠统计推断来精确说明我们的输入与输出之间的关系。例如，当我们使用线性回归时，我们知道预测因子的单位变化对应于输出的*线性*变化。这个变化的幅度在模型拟合过程中被估计出来，我们可以使用概率理论为这些估计提供不确定性度量。

![](../Images/9eb6adf2cc2ec36a05fd68bede54b580.png)

由 [Javier Allegue Barros](https://unsplash.com/@soymeraki) 在 [Upsplash](https://unsplash.com/photos/0nOP5iHVaZ8) 上拍摄的照片

不幸的是，当使用黑箱模型时，我们通常无法做出这些声明。深度神经网络可能具有数百、数千甚至[百万](https://towardsdatascience.com/understanding-and-coding-a-resnet-in-keras-446d7ff84d33)个可训练权重，这些权重将输入预测器连接到输出预测（ResNet-50 具有超过 2300 万个可训练参数），以及多个非线性激活函数。处理如此复杂的模型时，极其困难的是从分析角度绘制出预测器与预测之间的关系。

特征重要性技术的发展旨在帮助缓解解释性危机。特征重要性技术根据每个预测器改善预测的能力为其分配分数。这使我们能够根据预测器的相对预测能力对模型中的预测器进行排序。

生成这些特征重要性分数的一种方法是利用随机排列的强大功能。下一部分将解释如何使用 Python 执行排列特征重要性。

### 排列特征重要性

特征重要性的理念很简单。对预测有用的输入包含有价值的信息。如果你通过随机打乱特征值来破坏这些信息，那么预测质量应该会下降。如果质量下降很小，那么原始预测器中的信息在确定预测时并没有很大影响——你的模型即使没有这些信息也仍然很好。此外，如果质量下降很大，那么原始预测器中的信息对预测的影响很大。

这个理念可以通过三个简单的步骤来实现。假设你已经训练了一个 ML 模型，并记录了预测的某些质量度量（例如 MSE、对数损失等）。对于数据集中的每个预测器：

1.  在保持其他预测器值不变的情况下，随机打乱预测器中的数据。

1.  基于打乱的值生成新的预测，并评估新预测的质量。

1.  通过计算新预测相对于原始预测质量的下降来计算特征重要性分数。

一旦你计算了所有特征的特征重要性分数，你可以根据预测的有用性对它们进行排名。为了更具体地解释排列特征重要性，请考虑以下合成案例研究。

### 案例研究：预测房价

*注意：代码包括在内时最具指导性。请参考本指南的完整代码*[*这里*](https://github.com/sethbilliau/FeatureImportance)*。*

假设在[Blotchville](http://www.hcs.harvard.edu/cs50-probability/hw0705.php)的10,000栋房子的价格由四个因素决定：房屋颜色、社区密度分数、社区犯罪率分数和社区教育分数。Blotchville的房屋要么是红色，要么是蓝色，因此颜色被编码为二进制指示符。三个定量分数已标准化并近似呈正态分布。房屋价格 *i c*可以根据以下数据生成方程从这些因素中确定：

![](../Images/2b8a74c54d0e62acac1872934d2494dc.png)

数据生成方程

数据集中还包含五个与房价无关且没有预测能力的预测变量。下面是数据集前五行的快照，`df`。

![](../Images/d80fb79133017e0625f51e15d8f20f41.png)

数据集快照

假设我们想训练一个模型，根据其他九个预测变量来预测房价。我们可以使用任何黑箱模型，但为了这个例子，我们训练一个随机森林回归模型。为此，我们将数据分为训练集和测试集。然后，我们使用sklearn拟合一个简单的随机森林模型。

```py
from sklearn.model_selection import train_test_split 
from sklearn.ensemble import RandomForestRegressorX = df.drop(columns = 'price')
# One-hot encode color for sklearn
X['color'] = (X['color'] == 'red')
y = df.price# Train Test Split
X_train, X_test, y_train, y_test = train_test_split(X, y,
                                                    test_size=0.33, 
                                                    random_state=42)# Instantiate a Random Forest Regressor
regr = RandomForestRegressor(max_depth=100, random_state=0)# Fit a random forest regressor
regr.fit(X_train, y_train)
```

此时，您可以随意花些时间调整随机森林回归模型的超参数。但由于这不是关于[超参数调整](https://towardsdatascience.com/hyperparameter-tuning-the-random-forest-in-python-using-scikit-learn-28d2aa77dd74)的指南，我将继续使用这个简单的随机森林模型——它对于说明置换特征重要性的用途足够了。

一个常用的度量回归预测质量的指标是 [均方根误差 (RMSE)](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)，它是在测试集上评估的。让我们计算模型预测的RMSE并将其存储为 `rmse_full_mod`。

```py
from sklearn.metrics import mean_squared_errorrmse_full_mod = mean_squared_error(regr.predict(X_test), y_test, squared = False)
```

现在，我们可以通过打乱每个预测变量并记录RMSE的增加来实现置换特征重要性。这将帮助我们评估哪些预测变量对于做出预测是有用的。下面是从头开始实现这一点的代码。看看你是否能将这段代码的注释与我们之前的算法对上。

```py
# Initialize a list of results
results = []# Iterate through each predictor
for predictor in X_test:

    # Create a copy of X_test
    X_test_copy = X_test.copy()

    # Scramble the values of the given predictor
    X_test_copy[predictor] = X_test[predictor].sample(frac=1).values

    # Calculate the new RMSE
    new_rmse = mean_squared_error(regr.predict(X_test_copy), y_test,
                                  squared = False)

    # Append the increase in MSE to the list of results 
    results.append({'pred': predictor,
                    'score': new_rmse - rmse_full_mod })# Convert to a pandas dataframe and rank the predictors by score
resultsdf = pd.DataFrame(results).sort_values(by = 'score',
                                              ascending = False)
```

结果数据框包含置换特征重要性分数。较大的分数对应于RMSE的显著增加——这是预测变量被打乱时模型性能变差的证据。检查表格时，我们看到四个数据生成的预测变量（教育、颜色、密度和犯罪）具有相对较大的值，这意味着它们在我们的模型中具有预测能力。另一方面，五个虚拟预测变量的值相对较小，这意味着它们在预测中不那么有用。

![](../Images/ee79363356fd5e28bdbdfb7a808183f2.png)

置换数据框结果

我们还可以使用matplotlib绘制置换特征重要性分数图，以便于比较。

![](../Images/8ef8103386aea8ad2e72be9f404ee06c.png)

排列特征重要性图

从这项分析中，我们获得了关于模型如何进行预测的宝贵见解。我们看到教育得分是预测房价时提供最有价值信息的预测变量。房屋颜色、密度得分和犯罪得分似乎也是重要的预测变量。最后，似乎这五个虚拟预测变量的预测能力并不强。事实上，由于删除虚拟预测变量3实际上导致了RMSE的下降，我们可能考虑在未来的分析中进行特征选择，移除这些不重要的预测变量。

### 排列特征重要性的缺点

![](../Images/176fcc49ddc9f174be73b548a949f6a7.png)

照片由[马丁·埃斯特维](https://unsplash.com/@tme18)拍摄，发布在[Upsplash](https://unsplash.com/photos/4y8A6Ve-3GE)

尽管我们已经看到排列特征重要性的诸多好处，但同样重要的是承认其缺点（不是双关）。以下是使用排列特征重要性的一些缺点：

1.  **计算时间：** 这个过程可能计算开销较大，因为它需要你对每个预测变量进行迭代并进行预测。如果预测成本不低或预测变量非常多，这可能会非常昂贵。

1.  **在多重共线性存在时表现不佳：** 如果数据集中有相关特征，排列特征重要性可能表现不佳。如果一个预测变量的信息也存在于一个相关的预测变量中，那么当这些预测变量之一被打乱时，模型仍然可能表现良好。

1.  **得分是相对的，而非绝对的：** 排列重要性得分显示了模型中特征的*相对*预测能力。然而，这些得分在没有上下文的情况下实际上没有任何有意义的价值——任何得分都可能因其他得分的不同而显得好或坏。

1.  **特征重要性仍不是统计推断：** 特征重要性技术只能告诉你一个预测变量有多有用——它们不能提供关于关系的性质（例如线性、二次等）或预测变量效应的大小的任何见解。排列特征重要性不是统计推断的替代方案，而是当无法进行传统推断时的替代解决方案。

### 结论

排列特征重要性是分析黑箱模型和提供机器学习可解释性的有价值工具。通过这些工具，我们可以更好地理解预测变量与预测结果之间的关系，甚至可以进行更有原则的特征选择。

尽管我们是从零开始实现了排列特征重要性，但也有多个包提供了复杂的排列特征重要性实现及其他与模型无关的方法。Python 用户应查看 `eli5`、`alibi`、`scikit-learn`、`LIME` 和 `rfpimp` 包，而 R 用户则应转向 `iml`、`DALEX` 和 `vip`。

祝你在排列中愉快！如果你有任何问题，随时留言，我会尽力提供答案。

*鸣谢：非常感谢出色的 Claire Hoffman 校对和编辑这篇文章，并忍受我对牛津逗号的忽视。我也感谢 Leo Saenger 阅读这篇文章并提供建议。*

**简介： [Seth Billiau](https://www.linkedin.com/in/seth-billiau-b4b062136/)** 是数据科学家和统计学家，哈佛大学统计学 A.B. 学位，前哈佛大学开放数据项目副总裁。电子邮件：sethbilliau [at] college.harvard.edu

[原文](https://towardsdatascience.com/from-scratch-permutation-feature-importance-for-ml-interpretability-b60f7d5d1fe9)。转载已获许可。

**相关：**

+   [这种数据可视化是有效特征选择的第一步](/2021/06/data-visualization-feature-selection.html)

+   [特征选择 – 你想知道的一切](/2021/06/feature-selection-overview.html)

+   [用于解释和比较机器学习模型的仪表盘](/2021/06/dashboards-interpreting-comparing-machine-learning-models.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关内容

+   [排列在神经网络预测中的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)

+   [使用 Python 和 Scikit-learn 简化决策树可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)

+   [使用 SHAP 值进行机器学习模型可解释性分析](https://www.kdnuggets.com/2023/08/shap-values-model-interpretability-machine-learning.html)

+   [特征存储峰会 2022：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [庆祝对数据隐私重要性的认知](https://www.kdnuggets.com/2022/01/celebrating-awareness-importance-data-privacy.html)

+   [数据科学中实验设计的重要性](https://www.kdnuggets.com/2022/08/importance-experiment-design-data-science.html)
