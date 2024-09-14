# 管理机器学习工作流程与Scikit-learn管道 第3部分：多个模型、管道和网格搜索

> 原文：[https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-3.html](https://www.kdnuggets.com/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-3.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

首先，我知道我在上篇文章中承诺我们会结束玩具数据集，但为了比较目的，我们将继续使用鸢尾花数据集。我认为我们应该在整个过程中保持一致，进行“苹果对苹果”的比较。

![头图](../Images/d7c978b124395772dd8df1743b133d2b.png)

到目前为止，在前两篇文章中，我们已经：

+   介绍了Scikit-learn管道

+   通过创建和比较一些管道，展示了它们的基本用法。

+   介绍了网格搜索

+   通过使用网格搜索来寻找优化的超参数，并将其应用到嵌入式管道中，展示了管道和网格搜索如何协同工作。

下面是我们计划继续的内容：

+   在这篇文章中，我们将使用网格搜索来优化由多种不同类型的估算器构建的模型，然后对这些模型进行比较。

+   在后续文章中，我们将转向使用自动化机器学习技术来协助优化模型超参数，最终结果将是一个自动生成的、优化的Scikit-learn管道脚本文件，由[TPOT](https://github.com/EpistasisLab/tpot)提供。

这次不会有太多需要重新解释的内容；我建议你阅读[本系列的第一篇文章](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)以获得Scikit-learn管道的入门介绍，并阅读[本系列的第二篇文章](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html)以了解如何将网格搜索集成到管道中。我们现在要做的是构建一系列不同估算器的管道，使用网格搜索进行超参数优化，然后比较这些不同的“苹果与橙子”以确定最准确的（“最佳”）模型。

下面的代码注释良好，如果你已经阅读了前两篇文章，应该容易跟随。

请注意，这里有很多重构的机会。例如，每个管道都被显式地定义，而可以使用一个简单的函数作为生成器；网格搜索对象也是如此。长形式的代码希望在下一篇文章中提供更好的“苹果对苹果”比较。

让我们试试看。

```py
  $ python3 pipelines-3.py
```

下面是输出结果：

```py

Performing model optimizations...

Estimator: Logistic Regression
Best params: {'clf__penalty': 'l1', 'clf__C': 1.0, 'clf__solver': 'liblinear'}
Best training accuracy: 0.917
Test set accuracy score for best params: 0.967 

Estimator: Logistic Regression w/PCA
Best params: {'clf__penalty': 'l1', 'clf__C': 0.5, 'clf__solver': 'liblinear'}
Best training accuracy: 0.858
Test set accuracy score for best params: 0.933 

Estimator: Random Forest
Best params: {'clf__criterion': 'gini', 'clf__min_samples_split': 2, 'clf__max_depth': 3, 'clf__min_samples_leaf': 2}
Best training accuracy: 0.942
Test set accuracy score for best params: 1.000 

Estimator: Random Forest w/PCA
Best params: {'clf__criterion': 'entropy', 'clf__min_samples_split': 3, 'clf__max_depth': 5, 'clf__min_samples_leaf': 1}
Best training accuracy: 0.917
Test set accuracy score for best params: 0.900 

Estimator: Support Vector Machine
Best params: {'clf__kernel': 'linear', 'clf__C': 3}
Best training accuracy: 0.967
Test set accuracy score for best params: 0.967 

Estimator: Support Vector Machine w/PCA
Best params: {'clf__kernel': 'rbf', 'clf__C': 4}
Best training accuracy: 0.925
Test set accuracy score for best params: 0.900 

Classifier with best test set accuracy: Random Forest

Saved Random Forest grid search pipeline to file: best_gs_pipeline.pkl

```

重要的是，在我们拟合估计器后，我们测试了每个网格搜索的最佳参数在测试数据集上的效果。这是我们在上一篇文章中没有做的，尽管我们在比较不同模型，但鉴于引入了其他概念，之前关键的步骤——在之前未见的测试数据上比较不同模型——直到现在才被忽略。我们的例子证明了这一步骤的必要性。

如上所示，**在我们的训练数据中表现“最佳”的模型**是支持向量机（不使用 PCA），使用线性核和 C 值为 3（[控制正则化量](http://scikit-learn.org/stable/auto_examples/svm/plot_svm_scale_c.html)），它能够准确分类 96.7% 的训练实例。然而，**在测试数据中表现最佳的模型**（我们数据集中20%在训练后对所有模型都是未见过的数据）是随机森林（不使用 PCA），使用基尼标准，最小样本分裂数为 2，最大深度为 3，叶节点的最小样本数为 2，能够准确分类 100% 的未见数据实例。请注意，该模型的训练准确率较低，为 94.2%。

因此，除了观察我们如何混合和匹配各种不同的估计器类型、网格搜索参数组合和数据转换，以及如何准确比较训练后的模型外，显然评估不同模型时应始终包括在之前未见的保留数据上进行测试。

对管道感到厌倦了吗？下次我们将探讨一种替代的方法来自动化它们的构建。

**相关：**

+   [使用 Scikit-learn 管理机器学习工作流第 1 部分：简要介绍](/2017/12/managing-machine-learning-workflows-scikit-learn-pipelines-part-1.html)

+   [使用 Scikit-learn 管理机器学习工作流第 2 部分：整合网格搜索](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-2.html)

+   [使用遗传算法优化递归神经网络](/2018/01/genetic-algorithm-optimizing-recurrent-neural-network.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 相关主题

+   [使用网格搜索和随机搜索进行超参数调优的 Python 实践](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [如何在Python中管理多重继承](https://www.kdnuggets.com/2022/03/manage-multiple-inheritance-python.html)

+   [管理深度学习数据集的新方法](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)

+   [作为数据科学家管理可重用的Python代码](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

+   [管理数据科学项目的4个步骤](https://www.kdnuggets.com/2022/05/4-steps-managing-data-science-project.html)

+   [使用MLOps管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)
