# 使用 Scikit-learn 管道管理机器学习工作流第 2 部分：集成网格搜索

> 原文：[https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html](https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![Pipeline](../Images/f6e4b8b675c6c9d128270e42ce22de20.png)

在我们[上一篇文章](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)中，我们讨论了 Scikit-learn 管道作为简化机器学习工作流的方法。管道设计为一种可管理的方式，先进行一系列数据转换，然后应用估计器，管道被认为是一个简单的工具，主要用于：

+   创建连贯且易于理解的工作流的便利性

+   强制执行工作流实施和所需步骤应用的顺序

+   可复现性

+   完整管道对象的持久性价值（涉及可复现性和便利性）

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

另一个可以与管道配对以提高性能的简单但强大的技术是**网格搜索**，它尝试优化模型超参数组合。详尽的网格搜索——与随机优化等替代超参数组合优化方案不同——测试和比较所有可能的期望超参数值组合，这是一个指数增长的过程。在可能最终导致高昂运行时间的权衡中，所期望的是尽可能优化的模型。

来自[官方文档](http://scikit-learn.org/stable/modules/grid_search.html)：

> GridSearchCV 提供的网格搜索会根据 param_grid 参数指定的参数值网格生成候选项。例如，以下 param_grid：
> 
> ```py
> param_grid = [
> 
>   {'C': [1, 10, 100, 1000], 'kernel': ['linear']},
> 
>   {'C': [1, 10, 100, 1000], 'gamma': [0.001, 0.0001], 'kernel': ['rbf']},
> 
>  ]
> ```
> 
> 指定应该探索两个网格：一个是线性核和 C 值在 [1, 10, 100, 1000] 之间，另一个是 RBF 核，C 值的交叉乘积范围在 [1, 10, 100, 1000] 之间，gamma 值在 [0.001, 0.0001] 之间。

首先回顾一下上一篇文章中的代码，并运行修改后的摘录。由于这次练习中我们将使用一个单独的管道，所以不需要像上一篇文章那样使用完整的集合。我们将再次使用鸢尾花数据集。

让我们将这个非常简单的管道实现起来。

```py
  $ python3 pipelines-2a.py
```

以及模型返回的准确性和超参数：

```py
Test accuracy: 0.867

Model hyperparameters:
 {'min_impurity_decrease': 0.0, 'min_weight_fraction_leaf': 0.0, 'max_leaf_nodes': None, 'max_depth': None,
  'min_impurity_split': None, 'random_state': 42, 'class_weight': None, 'min_samples_leaf': 1, 'splitter': 'best', 
  'max_features': None, 'presort': False, 'min_samples_split': 2, 'criterion': 'gini'}
```

再次注意，我们正在应用特征缩放、降维（使用 PCA 将数据投影到二维空间），最后应用我们的最终估算器。

现在让我们将网格搜索添加到我们的管道中，希望能够优化模型的超参数并提高其准确性。默认模型参数是否是最佳选择？让我们来找出答案。

由于我们的模型使用决策树估算器，我们将使用网格搜索来优化以下超参数：

+   **criterion** - 这是用于评估分割质量的函数；我们将使用 Scikit-learn 中的两个选项：基尼不纯度和信息增益（熵）

+   **min_samples_leaf** - 这是构成有效叶节点所需的最小样本数；我们将使用整数范围 1 到 5

+   **max_depth** - 这是树的最大深度；我们将使用整数范围 1 到 5

+   **min_samples_split** - 这是拆分非叶节点所需的最小样本数；我们将使用整数范围 1 到 5

+   **presort** - 这指示是否预排序数据以加速在拟合过程中找到最佳分割；这不会影响最终模型的准确性（仅影响训练时间），但为了在网格搜索模型中使用 True/False 超参数而被包含在内（有趣吧？！？）

这是在我们调整的管道示例中使用详尽的网格搜索的代码。

重要的是，请注意我们的管道是网格搜索对象中的估算器，且模型是在网格搜索对象的层级中拟合的。还要注意我们的网格参数空间在一个字典中定义，然后传递给我们的网格搜索对象。

在网格搜索对象创建过程中还发生了什么？为了对结果模型进行评分（潜在有 2 * 5 * 5 * 5 * 2 = 500 种），我们将引导网格搜索通过在测试集上的准确性来评估它们。我们还指定了 10 折的交叉验证拆分策略。请注意有关 GridSearchCV 的以下内容：

> 用于应用这些方法的估算器的参数通过对参数网格进行交叉验证网格搜索来优化。

最终，当然，我们的模型已拟合。

你可以查看官方的 [GridSearchCV 模块文档](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)，以获取所有其他有用配置的信息，包括但不限于并行处理。

让我们试试看。

```py
  $ python3 pipelines-2b.py
```

这是结果：

```py
Best accuracy: 0.925

Best params:
 {'clf__min_samples_split': 2, 'clf__criterion': 'gini', 'clf__max_depth': 2, 
  'clf__min_samples_leaf': 1, 'clf__presort': True}
```

脚本报告了最高的准确度（0.925），这显然比默认的0.867要好，并且额外的计算量并不多，至少在我们的玩具数据集上是这样。我们包括500个模型的详尽方法在强大的数据集上可能会有更严重的计算影响，正如你所想象的那样。

脚本还报告了具有最高准确度的模型的最佳超参数配置，如上所示。在我们的简单示例中，这种差异应该足以表明，不应盲目跟随 Scikit-learn 的默认设置。

这一切看起来过于简单。确实是。Scikit-learn 使用起来几乎过于简单，只要你知道有哪些选项。我们使用玩具数据集也没有让它看起来更复杂。

不妨这样看：管道和网格搜索就像巧克力和花生酱一样相互配合，现在我们已经了解了它们如何一起工作，接下来我们可以挑战一些更复杂的问题。更复杂的数据？当然可以。比较各种估算器和每个估算器的大量参数搜索空间，以找到“真正的”最优化模型？为什么不呢？将管道转换组合起来，既有趣又有利？是的！

下次加入我们，我们将超越配置和玩具数据集的基础知识。

**相关：**

+   [使用 Scikit-learn 管道管理机器学习工作流 第1部分：温和的介绍](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)

+   [掌握 Python 数据准备的 7 个步骤](/2017/06/7-steps-mastering-data-preparation-python.html)

+   [从零开始的 Python 机器学习工作流 第1部分：数据准备](/2017/05/machine-learning-workflows-python-scratch-part-1.html)

### 了解更多相关主题

+   [使用网格搜索和随机搜索进行超参数调整的 Python 方法](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [将 ChatGPT 集成到数据科学工作流中：技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [构建视觉搜索引擎 - 第2部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [通过 Uplimit 的机器学习课程提升您的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [通过集成 Jupyter 和 KNIME 来缩短实现时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [优化数据分析：在 Databricks 中集成 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)
