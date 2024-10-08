# 用 100 行代码编写随机森林®*

> 原文：[`www.kdnuggets.com/2019/08/coding-random-forests.html`](https://www.kdnuggets.com/2019/08/coding-random-forests.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [STATWORX](https://www.statworx.com/) 提供**

### 动机

现有的机器学习算法种类繁多。要掌握所有算法的细节几乎是不可能的；然而，许多算法源自最成熟的算法，例如普通最小二乘法、梯度提升、支持向量机、基于树的算法和神经网络。在 [STATWORX](https://www.statworx.com/) 我们每天讨论算法，以评估它们在特定项目中的好处。无论如何，理解这些核心算法是文献中大多数机器学习算法的关键。

### 为什么要从头开始编写？

虽然我喜欢阅读机器学习研究论文，但数学有时难以理解。这就是为什么我喜欢自己用 R 实现算法。当然，这意味着深入挖掘数学和算法。然而，你可以直接挑战你对算法的理解。

在我上一篇博客中，我用 150 行 R 代码介绍了两种机器学习算法。你可以在我们的 [博客](https://www.statworx.com/blog/coding-gradient-boosted-machines-in-100-lines-of-code) 上找到关于用代码实现 [梯度提升机](https://www.statworx.com/blog/coding-gradient-boosted-machines-in-100-lines-of-code) 和 [回归树](https://www.statworx.com/blog/coding-regression-trees-in-150-lines-of-code) 的其他博客帖子，或者在我的 [GitHub](https://www.github.com/andrebleier/cheapml) 上的 readme 中找到。这篇博客文章介绍了随机森林，这可能是最著名的机器学习算法。你可能注意到了标题中的星号（*）。这些通常暗示着有些问题。就像在电视广告中看到手机计划的价格，而在阅读小字时你发现它仅适用于你成功攀登了珠穆朗玛峰，并且你的游艇上有三只长颈鹿。此外，是的，你的怀疑是合理的；不幸的是，100 行代码仅适用于如果我们不添加回归树算法的代码，而回归树算法对随机森林来说是至关重要的。因此，如果你不熟悉回归树算法，我强烈建议阅读关于 [回归树](https://www.statworx.com/blog/coding-regression-trees-in-150-lines-of-code) 的博客。

### 用简单和可访问的代码理解机器学习

在这个系列中，我们尝试生成非常通用的代码，即它不会产生最先进的性能。它的设计旨在非常通用且易于阅读。

毋庸置疑，有大量出色的文章理论上解释了随机森林，并配有实际操作示例。但这不是这篇博客文章的目的。如果你对包含所有必要理论的实际教程感兴趣，我强烈推荐这个 [教程](https://datascienceplus.com/random-forests-in-r/)。这篇博客文章的目的是通过编写简单的 R 代码来建立算法的理论。除了回归树的基础知识外，你需要知道的唯一事情是我们的目标：我们希望用一组实值特征（`X`）来估计我们的实值目标（`y`）。

### 使用特征重要性减少维度

幸运的是，我们在这个教程中不需要覆盖太多的数学，因为那部分已经在回归树 [教程](https://www.statworx.com/blog/coding-regression-trees-in-150-lines-of-code)中涵盖了。然而，有一个部分，我在代码中加入了，因为自上一个 [博客文章](https://www.statworx.com/blog/coding-regression-trees-in-150-lines-of-code)以来发生了变化。回归树，因此随机森林，相对于标准的 OLS 回归，可以忽略不重要的特征。这是基于树的算法的一个重要优势，也是我们基本算法中应该涵盖的内容。你可以在新的 `reg_tree_imp.R` 脚本中找到这一改进，位于 [GitHub](https://www.github.com/andrebleier/cheapml)。我们使用这个函数来在森林中生长树木。

在我们跳入随机森林代码之前，我想简要介绍一下如何计算回归树中的特征重要性。当然，有许多方法可以计算特征重要性，但以下方法相当直观和直接。

### 评估分割的好坏

回归树通过选择最小化某个标准的特征来分割数据，例如我们预测的平方误差。当然，有些特征可能永远不会被选择用于分割，这使得计算它们的重要性变得非常简单。然而，如何计算已选择特征的重要性呢？一个初步的方法可能是计算每个特征的分割次数，并以所有分割的总数进行相对化。这个度量简单而直观，但它不能量化分割的影响力，这可以通过一个简单但更复杂的度量来实现。这个度量是加权拟合优度。我们从定义每个节点的拟合优度开始。例如，均方误差，定义为：

![MSE_{node} = \frac{1}{n_{node}} \sum_{i = 1}^{n_{node}} (y_i - \bar{y}_{node}))²](img/06b98047f3df3521f7fcf81a2166027b.png)

该指标描述了我们在用预测器（当前节点的平均值 \bar(y)_{node}）估计目标 y_i 时所犯的平均平方误差。现在我们可以通过选择特征来分裂数据来测量改进，并比较父节点的拟合优度与其子节点的表现。本质上，这与我们为当前节点评估最佳分裂特征时所执行的步骤基本相同。

### 权重分裂的影响

树顶的分裂具有更大的影响，因为在树的这一阶段，更多的数据到达了这些节点。这就是为什么在考虑到到达该节点的观测数量时，更早的分裂显得更为重要。

![w_{node} = \frac{n_{node}}{n}](img/ea7138c77f3291744cc275fc151db827.png)

这个权重描述了当前节点中的观测数量，相对于总观测数量。结合以上结果，我们可以推导出单个节点中特征 p 的加权改进：

### 通过在回归树中分裂数据来量化改进

![Improvement^{[p]}_{node} = w_{node}MSE_{node} - (w_{left}MSE_{left} + w_{right}MSE_{right})](../Images/2018c97a3834cc3069d6c190f0194edb.png)

这种加权改进在每个被分裂的节点中计算，针对相应的分裂特征 p。为了更好地解释这种改进，我们将树中每个特征的改进总和，并通过树中的整体改进进行归一化。

### 量化回归树中特征的重要性

![Importance^{[p]} = \frac{\sum_{k=1}^{p} Improvement^{[k]}}{\sum_{i=1}^{n_{node}} Improvement^{[i]}}](../Images/e7110ee9243f520f45e655f6c4072946.png)

这是回归树算法中使用的最终特征重要性度量。同样，您可以在[回归树的代码](https://github.com/andrebleier/cheapml/blob/master/algorithms/reg_tree_imp.R)中按照这些步骤进行。我已经将所有参与特征重要性计算的变量、函数和列名标记为`imp_*`或`IMP_*`，这应该会使其更易于跟踪。

### 随机森林算法

好了，回归树和重要性讲得差不多了——在这篇博客文章中，我们关心的是随机森林。随机森林的目标是结合多个回归树或决策树。这种单一结果的组合被称为集成技术。这个技术的理念非常简单但却非常强大。

### 构建回归树乐团

在一个交响乐团中，不同组的乐器组合成一个合奏，这创造了更强大且多样的和声。本质上，机器学习中的情况也相同，因为我们在随机森林中生成的每一个回归树都有机会从不同的角度探索数据。我们的回归树乐团因此对数据有不同的视角，这使得组合比单一回归树更强大和多样。

### 随机森林的简单性

如果你不熟悉这个算法的机制，你可能会觉得代码变得非常复杂且难以跟随。对我来说，这个算法令人惊讶的地方在于它的简单性和有效性。编码部分并不像你想象的那么具有挑战性。就像在其他博客文章中，我们首先查看整个代码，然后逐步分析。

### 一窥算法

```py
#' reg_rf
#' Fits a random forest with a continuous scaled features and target 
#' variable (regression)
#'
#' @param formula an object of class formula
#' @param n_trees an integer specifying the number of trees to sprout
#' @param feature_frac an numeric value defined between [0,1]
#'                     specifies the percentage of total features to be used in
#'                     each regression tree
#' @param data a data.frame or matrix
#'
#' @importFrom plyr raply
#' @return
#' @export
#'
#' @examples # Complete runthrough see: www.github.com/andrebleier/cheapml
reg_rf <- function(formula, n_trees, feature_frac, data) {

  # source the regression tree function
  source("algorithms/reg_tree_imp.R")

  # load plyr
  require(plyr)

  # define function to sprout a single tree
  sprout_tree <- function(formula, feature_frac, data) {
    # extract features
    features <- all.vars(formula)[-1]

    # extract target
    target <- all.vars(formula)[1]

    # bag the data
    # - randomly sample the data with replacement (duplicate are possible)
    train <-
      data[sample(1:nrow(data), size = nrow(data), replace = TRUE)]

    # randomly sample features
    # - only fit the regression tree with feature_frac * 100 % of the features
    features_sample <- sample(features,
                              size = ceiling(length(features) * feature_frac),
                              replace = FALSE)

    # create new formula
    formula_new <-
      as.formula(paste0(target, " ~ ", paste0(features_sample,
                                              collapse =  " + ")))

    # fit the regression tree
    tree <- reg_tree_imp(formula = formula_new,
                         data = train,
                         minsize = ceiling(nrow(train) * 0.1))

    # save the fit and the importance
    return(list(tree$fit, tree$importance))
  }

  # apply the rf_tree function n_trees times with plyr::raply
  # - track the progress with a progress bar
  trees <- plyr::raply(
    n_trees,
    sprout_tree(
      formula = formula,
      feature_frac = feature_frac,
      data = data
    ),
    .progress = "text"
  )

  # extract fit
  fits <- do.call("cbind", trees[, 1])

  # calculate the final fit as a mean of all regression trees
  rf_fit <- apply(fits, MARGIN = 1, mean)

  # extract the feature importance
  imp_full <- do.call("rbind", trees[, 2])

  # build the mean feature importance between all trees
  imp <- aggregate(IMPORTANCE ~ FEATURES, FUN = mean, imp_full)

  # build the ratio for interpretation purposes
  imp$IMPORTANCE <- imp$IMPORTANCE / sum(imp$IMPORTANCE)

  # export
  return(list(fit = rf_fit,
              importance = imp[order(imp$IMPORTANCE, decreasing = TRUE), ]))
}
```

正如你可能已经注意到的，我们的算法大致可以分为两个部分。首先是一个函数 `sprout_tree()`，然后是一些调用这个函数并处理其输出的代码行。现在让我们逐块分析所有的代码。

```py
# source the regression tree function
  source("algorithms/reg_tree_imp.R")

  # load plyr
  require(plyr)

  # define function to sprout a single tree
  sprout_tree <- function(formula, feature_frac, data) {
    # extract features
    features <- all.vars(formula)[-1]

    # extract target
    target <- all.vars(formula)[1]

    # bag the data
    # - randomly sample the data with replacement (duplicate are possible)
    train <-
      data[sample(1:nrow(data), size = nrow(data), replace = TRUE)]

    # randomly sample features
    # - only fit the regression tree with feature_frac * 100 % of the features
    features_sample <- sample(features,
                              size = ceiling(length(features) * feature_frac),
                              replace = FALSE)

    # create new formula
    formula_new <-
      as.formula(paste0(target, " ~ ", paste0(features_sample,
                                              collapse =  " + ")))

    # fit the regression tree
    tree <- reg_tree_imp(formula = formula_new,
                         data = train,
                         minsize = ceiling(nrow(train) * 0.1))

    # save the fit and the importance
    return(list(tree$fit, tree$importance))
  }
```

### 在森林中生成一个单一回归树

代码的第一部分是 `sprout_tree()` 函数，它只是回归树函数 `reg_tree_imp()` 的一个封装，我们将其作为代码的第一个动作来调用。然后我们从公式对象中提取我们的目标和特征。

### 通过数据的袋装创建不同的角度

之后，我们对数据进行袋装，这意味着我们随机抽取数据，并且有可能重复。还记得我说过每棵树将从不同的角度查看我们的数据吗？好吧，这就是我们创建这些角度的部分。随机抽样与替代仅仅是对我们观察结果生成权重的另一种说法。这意味着在某棵树的数据集中，一个特定的观察值可能会被重复 10 次。然而，下一棵树可能完全丢失这个观察值。此外，还有另一种在我们的树中创建不同角度的方法：特征抽样。

### 解决森林中树之间的共线性问题

从我们的完整特征集 `X` 中，我们随机抽取 `feature_frac * 100` 百分比来减少维度。通过特征抽样，我们可以 a) 更快地计算和 b) 从假设上较弱的特征捕捉数据的角度，因为我们降低了 `feature_frac` 的值。假设，我们的特征之间存在一定程度的多重共线性。如果我们在每棵树中使用所有特征，回归树可能只会选择某个特定的特征。

然而，特征的改进较少可能会带来对模型有价值的新信息，但却未获得机会。这可以通过将参数`feature_frac`降低来实现。如果分析的目标是特征选择，例如特征重要性，你可能希望将该参数设置为 80%-100%，因为这样你会得到更明确的选择。好吧，函数的其余部分是拟合回归树，并导出拟合值以及重要性。

### 培育森林

在接下来的代码块中，我们开始使用`plyr::raply()`函数`n_trees`次来应用`sprout_tree()`函数。该函数重复应用具有相同参数的函数调用，并将结果合并为列表。请记住，我们无需更改`sprout_tree()`函数中的任何内容，因为每次调用函数时角度都是随机生成的。

```py
# apply the rf_tree function n_trees times with plyr::raply
  # - track the progress with a progress bar
  trees <- plyr::raply(
    n_trees,
    sprout_tree(
      formula = formula,
      feature_frac = feature_frac,
      data = data
    ),
    .progress = "text"
  )

  # extract fit
  fits <- do.call("cbind", trees[, 1])

  # calculate the final fit as a mean of all regression trees
  rf_fit <- apply(fits, MARGIN = 1, mean)

  # extract the feature importance
  imp_full <- do.call("rbind", trees[, 2])

  # build the mean feature importance between all trees
  imp <- aggregate(IMPORTANCE ~ FEATURES, FUN = mean, imp_full)

  # build the ratio for interpretation purposes
  imp$IMPORTANCE <- imp$IMPORTANCE / sum(imp$IMPORTANCE)

  # export
  return(list(fit = rf_fit,
              importance = imp[order(imp$IMPORTANCE, decreasing = TRUE), ]))
```

之后，我们将单个回归树的拟合结果合并到一个数据框中。通过计算行均值，我们得到了森林中每棵回归树的平均拟合值。我们的最后一步是计算集成模型的特征重要性。这是所有树中某一特征的平均特征重要性，经过所有变量总体均值的归一化。

### 应用算法

让我们应用这个函数，看看拟合是否确实比单一回归树更好。此外，我们还可以检查特征的重要性。我在[GitHub](https://www.github.com/andrebleier/cheapml)上创建了一个小例子，你可以查看。首先，我们用[Xy](https://www.github.com/andrebleier/Xy)包模拟数据。在这个模拟中，使用了五个线性变量来创建我们的目标`y`。为了增加一点难度，我们还添加了五个在模拟中创建的不相关变量。现在的挑战是，算法是否会使用任何不相关特征，或者算法能否完美识别重要特征。我们的公式是：

```py
eq <- y ~ LIN_1 + LIN_2 + LIN_3 + LIN_4 + LIN_5 + NOISE_1 + NOISE_2 + NOISE_3 + NOISE_4 + NOISE_5
```

### 森林的力量

随机森林和回归树都没有选择任何不必要的特征。然而，回归树仅由两个最重要的变量进行分裂。相反，随机森林选择了所有五个相关特征。

![figure-name](img/59136161b342dd98f09c5aa53c1e854a.png)

这并不意味着回归树无法找到正确的答案。这取决于树的最小观测量(`minsize`)。如果我们降低最小尺寸，回归树肯定会最终找到所有重要特征。然而，随机森林在相同的最小尺寸下找到了所有五个关键特征。

### 学习优势与劣势

当然，这只是一个例子。我强烈建议你自己动手操作函数和示例，因为只有这样你才能真正感受算法的优劣。随意克隆 [GitHub](https://www.github.com/andrebleier/cheapml) 仓库并尝试示例。模拟总是不错的，因为你会得到明确的答案；然而，将算法应用于熟悉的现实世界数据也可能对你有益。

**简介: [STATWORX](https://www.statworx.com/)** 是一家位于法兰克福、苏黎世和维也纳的数据科学、统计学、机器学习和人工智能咨询公司。

[原文](https://www.statworx.com/de/blog/coding-random-forests-in-100-lines-of-code/)。经许可转载。

RANDOM FORESTS 和 RANDOMFORESTS 是 Minitab, LLC 的注册商标。

**相关:**

+   机器学习和数据科学的决策树指南

+   解释随机森林（带 Python 实现）

+   随机森林直观解释

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关话题

+   [决策树与随机森林的比较说明](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [集成学习技术：在 Python 中使用随机森林的演示](https://www.kdnuggets.com/ensemble-learning-techniques-a-walkthrough-with-random-forests-in-python)

+   [少于 15 行代码的多模态深度学习](https://www.kdnuggets.com/2023/01/predibase-multi-modal-deep-learning-less-15-lines-code.html)

+   [随机森林与决策树：主要区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [随机森林算法是否需要归一化？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [使用网格搜索和随机搜索进行超参数调整（Python）](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)
