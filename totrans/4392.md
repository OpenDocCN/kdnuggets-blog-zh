# 使用 CatBoost 进行快速梯度提升

> 原文：[https://www.kdnuggets.com/2020/10/fast-gradient-boosting-catboost.html](https://www.kdnuggets.com/2020/10/fast-gradient-boosting-catboost.html)

[评论](#comments)

在梯度提升中，预测是通过一组弱学习器进行的。与为每个样本创建决策树的随机森林不同，梯度提升中树是一个接一个地创建的。模型中的前一棵树不会被更改。前一棵树的结果用于改进下一棵树。在本文中，我们将更详细地了解一种称为 CatBoost 的梯度提升库。

![图示](../Images/94e889b5133e25e115ddeb1ea1e6afd7.png)

[来源](https://catboost.ai/news/catboost-enables-fast-gradient-boosting-on-decision-trees-using-gpus)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

[CatBoost](https://github.com/catboost) 是由 [Yandex](https://yandex.com/company/) 开发的深度梯度提升库。它使用无感知决策树来生成平衡树。在树的每一层中，使用相同的特征来进行左右分裂。

![图示](../Images/22cec580000a5a651f8a7b907b81bca5.png)

[来源](https://catboost.ai/news/catboost-enables-fast-gradient-boosting-on-decision-trees-using-gpus)

与经典树相比，无感知树在 CPU 上的实现更高效，且更容易拟合。

### 处理分类特征

机器学习中处理分类变量的常见方法是独热编码和标签编码。CatBoost 允许你使用分类特征而无需预处理。

使用 CatBoost 时，我们不应使用独热编码，因为这会影响训练速度以及预测质量。相反，我们只需使用 `cat_features` 参数来指定分类特征。

### 使用 CatBoost 的优点

下面是考虑使用 CatBoost 的几个理由：

+   CatBoost 允许在多个 GPU 上进行数据训练。

+   使用默认参数可以获得很好的结果，从而减少了参数调整所需的时间。

+   由于过拟合减少，精度得到提高。

+   使用 CatBoost 的模型应用程序进行快速预测。

+   训练好的 CatBoost 模型可以导出到 Core ML 以进行设备端推断（iOS）。

+   可以内部处理缺失值。

+   可以用于回归和分类问题。

### 训练参数

让我们来看看 CatBoost 中的常见参数：

+   `loss_function` 别名 `objective` — 用于训练的指标。这些是回归指标，例如回归的均方根误差和分类的对数损失。

+   `eval_metric` — 用于检测过拟合的指标。

+   `iterations` — 要构建的最大树数量，默认为1000。别名有 `num_boost_round`、`n_estimators` 和 `num_trees`。

+   `learning_rate` 别名 `eta` — 确定模型学习速度的学习率。默认值通常为0.03。

+   `random_seed` 别名 `random_state` — 用于训练的随机种子。

+   `l2_leaf_reg` 别名 `reg_lambda` — 成本函数L2正则化项的系数。默认值为3.0。

+   `bootstrap_type` — 确定对象权重的抽样方法，例如贝叶斯（Bayesian）、伯努利（Bernoulli）、MVS和泊松（Poisson）。

+   `depth` — 树的深度。

+   `grow_policy` — 决定贪婪搜索算法的应用方式。可以是 `SymmetricTree`、`Depthwise` 或 `Lossguide`。`SymmetricTree` 是默认值。在 `SymmetricTree` 中，树按层构建，直到达到指定深度。在每一步中，上一棵树的叶子按照相同条件进行分裂。选择 `Depthwise` 时，树一步步构建，直到达到指定深度。在每一步中，最后一棵树层的所有非终端叶子都进行分裂。叶子按照最优损失改进条件进行分裂。在 `Lossguide` 中，树逐步构建叶子，直到达到指定数量。在每一步中，最佳损失改进的非终端叶子被分裂。

+   `min_data_in_leaf` 别名 `min_child_samples` — 叶子中最小的训练样本数。该参数仅与 `Lossguide` 和 `Depthwise` 增长策略一起使用。

+   `max_leaves` 别名 `num_leaves` — 该参数仅与 `Lossguide` 策略一起使用，决定树中的叶子数量。

+   `ignored_features` — 指示在训练过程中应忽略的特征。

+   `nan_mode` — 处理缺失值的方法。选项有 `Forbidden`、`Min` 和 `Max`。默认值为 `Min`。使用 `Forbidden` 时，缺失值的存在会导致错误。使用 `Min` 时，缺失值被视为该特征的最小值。在 `Max` 中，缺失值被视为该特征的最大值。

+   `leaf_estimation_method` — 用于计算叶子值的方法。在分类中，使用10次 `Newton` 迭代。回归问题使用分位数或MAE损失则使用一次 `Exact` 迭代。多分类使用一次 `Newton` 迭代。

+   `leaf_estimation_backtracking` — 在梯度下降过程中使用的回溯类型。默认值是 `AnyImprovement`。`AnyImprovement` 减少下降步长，直到损失函数值小于上次迭代中的值。`Armijo` 减少下降步长，直到满足 [Armijo 条件](https://en.wikipedia.org/wiki/Wolfe_conditions#Armijo_rule_and_curvature)。

+   `boosting_type` — 提升方案。可以是 `plain`，用于经典的梯度提升方案，或者 `ordered`，它在较小的数据集上提供更好的质量。

+   `score_function` — 用于选择树构建过程中下一个分裂的 [评分类型](https://catboost.ai/docs/concepts/algorithm-score-functions.html)。`Cosine` 是默认选项。其他可用选项包括 `L2`、`NewtonL2` 和 `NewtonCosine`。

+   `early_stopping_rounds` — 当设置为 `True` 时，将过拟合检测器类型设置为 `Iter` 并在达到最佳指标时停止训练。

+   `classes_count` — 多分类问题中的类别数。

+   `task_type` — 指示你是使用 CPU 还是 GPU。CPU 是默认选项。

+   `devices` — 用于训练的 GPU 设备的 ID。

+   `cat_features` — 包含分类列的数组。

+   `text_features` — 用于声明分类问题中的文本列。

### 回归示例

CatBoost 在实现中使用了 scikit-learn 标准。让我们看看如何使用它进行回归分析。

第一步——像往常一样——是导入回归器并实例化它。

```py
from catboost import CatBoostRegressor
cat = CatBoostRegressor()
```

在拟合模型时，CatBoost 还允许我们通过设置 `plot=true` 进行可视化：

```py
cat.fit(X_train,y_train,verbose=False, plot=True)
```

![发布图像](../Images/b7a07bcc879003b93fae2c1c40b964b5.png)

它还允许你执行交叉验证并可视化过程：

![发布图像](../Images/cc4a7d964a02c88e7ad3997232ab4d69.png)

同样，你也可以执行网格搜索并进行可视化：

![发布图像](../Images/2abad5ea87a4baf9e06c9f175d29840a.png)

我们还可以使用 CatBoost 绘制树。这里的图是第一棵树的绘图。正如你从树中看到的，每一层的叶子都在相同条件下进行分裂——例如 297，值 >0.5。

```py
cat.plot_tree(tree_idx=0)
```

![发布图像](../Images/1bf072864f605a5db714f0a388c18dd4.png)

CatBoost 还为我们提供了一个包含所有模型参数的字典。我们可以通过遍历字典来打印它们。

```py
for key,value in cat.get_all_params().items():
 print(‘{}, {}’.format(key,value))
```

![发布图像](../Images/33f889fb3e9caa39f98841f2938006c5.png)

### 最终想法

在这篇文章中，我们探讨了 CatBoost 的优缺点及其主要训练参数。然后，我们通过 scikit-learn 完成了一个简单的回归实现。希望这能为你提供足够的信息，以便你可以进一步探索该库。

[**CatBoost - 最先进的开源梯度提升库，支持类别特征**](https://catboost.ai/)

CatBoost 是一种决策树上的梯度提升算法。它由 Yandex 的研究人员和工程师开发...

[**Python 数据科学训练营**](https://www.udemy.com/course/data-science-bootcamp-in-python/?referralCode=9F6DFBC3F92C44E8C7F4)

学习 Python 数据科学、NumPy、Pandas、Matplotlib、Seaborn、Scikit-learn、Dask、LightGBM、XGBoost、CatBoost 等等…

**简介：[Derrick Mwiti](https://derrickmwiti.com/)** 是数据分析师、作家和导师。他致力于在每个任务中提供出色的成果，并且是 Lapid Leaders Africa 的导师。

[原文](https://heartbeat.fritz.ai/fast-gradient-boosting-with-catboost-38779b0d5d9a)。经许可转载。

**相关：**

+   [LightGBM：高效的梯度提升决策树](/2020/06/lightgbm-gradient-boosting-decision-tree.html)

+   [理解梯度提升机](/2019/02/understanding-gradient-boosting-machines.html)

+   [在 Google Colaboratory 上使用免费 GPU 掌握快速梯度提升](/2019/03/mastering-fast-gradient-boosting-google-colaboratory-free-gpu.html)

### 相关主题

+   [CatBoost ML 为您的数据带来的 5 大优势，让数据更“喵”](https://www.kdnuggets.com/2023/02/top-5-advantages-catboost-ml-brings-data-make-purr.html)

+   [提升机器学习算法：概述](https://www.kdnuggets.com/2022/07/boosting-machine-learning-algorithms-overview.html)

+   [**掌握季节性和提升商业结果的终极指南**](https://www.kdnuggets.com/2023/08/media-mix-modeling-ultimate-guide-mastering-seasonality-boosting-business-results.html)

+   [BERT 在稀疏性下能有多快？](https://www.kdnuggets.com/2022/04/fast-bert-go-sparsity.html)

+   [通过快速克里金（FKR）加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)

+   [如何让 Python 代码运行得非常快](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)
