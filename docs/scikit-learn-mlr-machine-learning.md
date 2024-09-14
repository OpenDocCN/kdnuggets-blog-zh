# Scikit-Learn与MLR在机器学习中的对比

> 原文：[https://www.kdnuggets.com/2019/09/scikit-learn-mlr-machine-learning.html](https://www.kdnuggets.com/2019/09/scikit-learn-mlr-machine-learning.html)

[评论](#comments)

![](../Images/4b0bc0e253bafaade27861da7aedba31.png)

Scikit-Learn以其易于理解的Python用户API而闻名，而MLR成为了流行的Caret包的替代品，提供了更多的算法库和简单的超参数调整方式。这两个包因许多人在分析工作中倾向于使用Python进行机器学习而R用于统计分析而处于竞争状态。

使用Python的一个原因可能是当前用于机器学习的R包通过其他包含算法的包提供。这些包通过MLR调用，但仍需额外安装。甚至外部特征选择库也需要，且它们还有其他外部依赖需要满足。

Scikit-Learn被称为一个统一的API，提供多个机器学习算法，无需用户调用更多库。

**这绝不会贬低R。** R在数据科学领域仍然是一个主要组成部分，不论在线调查如何说。任何有统计学或数学背景的人都会知道为什么你应该使用R（即使他们自己不使用，它们也认识到它的吸引力）。

现在我们将查看用户如何通过典型的机器学习工作流。我们将使用Scikit-Learn中的Logistic Regression和MLR中的决策树。

**创建你的训练和测试数据**

+   Scikit-Learn

    +   `x_train, x_test, y_train, y_test = train_test_split(x,y,test_size)`

        这是在Scikit-Learn中分割数据集的最简单方法。test_size用于确定数据的百分比进入测试集。train_test_split将自动在一行代码中创建训练集和测试集。x是特征集，y是目标变量。

+   MLR

    +   `train <- sample(1:nrow(data), 0.8 * nrow(data))`

    +   `test <- setdiff(1:nrow(train), train)`

    +   MLR没有内置的函数来子集数据集，因此用户需要依赖其他R函数。这是创建80/20训练测试集的一个例子。

**选择算法**

+   Scikit-Learn

    +   `LogisticRegression()`

        分类器可以通过调用一个明显命名的函数来选择和初始化，这样便于识别。

+   MLR

    +   `makeLearner('classif.rpart')` 算法被称为学习器，这个函数被调用以初始化它。

    +   `makeClassifTask(data=, target=)` 如果我们进行分类，我们需要调用此函数来初始化分类任务。此函数将接受两个参数：训练数据和目标变量的名称。

**超参数调整**

在任何一个软件包中，调整超参数都有一个流程。你首先需要指定要改变哪些参数以及这些参数的范围。然后进行网格搜索或随机搜索，以找到最佳的参数组合，从而获得最佳结果（即，最小化错误或最大化准确度）。

+   Scikit-Learn

    +   `penalty = ['l2']`

    +   `C = np.logspace(0, 4, 10)`

    +   `dual= [False]`

    +   `max_iter= [100,110,120,130,140]`

    +   `hyperparameters = dict(C=C, penalty=penalty, dual=dual, max_iter=max_iter)`

    +   `GridSearchCV(logreg, hyperparameters, cv=5, verbose=0)`

    +   `clf.fit(x_train, y_train)`

+   MLR

    +   `makeParamSet( makeDiscreteParam("minsplit", values=seq(5,10,1)), makeDiscreteParam("minbucket", values=seq(round(5/3,0), round(10/3,0), 1)), makeNumericParam("cp", lower = 0.01, upper = 0.05), makeDiscreteParam("maxcompete", values=6), makeDiscreteParam("usesurrogate", values=0), makeDiscreteParam("maxdepth", values=10) )`

    +   `ctrl = makeTuneControlGrid()`

    +   `rdesc = makeResampleDesc("CV", iters = 3L, stratify=TRUE)`

    +   `tuneParams(learner=dt_prob, resampling=rdesc, measures=list(tpr,auc, fnr, mmce, tnr, setAggregation(tpr, test.sd)), par.set=dt_param, control=ctrl, task=dt_task, show.info = TRUE) )`

    +   `setHyperPars(learner, par.vals = tuneParams$x)`

**训练**

两个软件包都提供了一行代码来训练模型。

+   Scikit-Learn

    +   `LogisticRegression().fit(x_train50, y_train50)`

+   MLR

    +   `train(learner, task)`

这可以说是过程中的较简单步骤。最艰巨的步骤是调整超参数和特征选择。

**预测**

就像训练模型一样，预测也可以通过一行代码完成。

+   Scikit-Learn

    +   `LogisticRegression().predict(x_test)`

+   MLR

    +   `predict(trained model, newdata)`

Scikit-learn 将返回一个预测标签的数组，而 MLR 将返回一个预测标签的数据框。

**模型评估**

评估监督分类器最流行的方法是混淆矩阵，从中可以获得准确度、错误率、精确度、召回率等。

+   Scikit-Learn

    +   `confusion_matrix(y_test, prediction)` OR

    +   `classification_report(y_test,prediction)`

+   MLR

    +   `performance(prediction, measures = list(tpr,auc,mmce, acc,tnr))` OR

    +   `calculateROCMeasures(prediction)`

两个软件包都提供了多种获取混淆矩阵的方法。**然而，为了以最简单的方式获得信息性视图，Python 可能没有 R 那么直观。第一个 Python 代码只会返回一个没有标签的矩阵。用户必须返回文档中解读哪些列和行对应于哪些类别。** 第二种方法提供了更好且更具信息性的输出，但它仅会生成精确度、召回率、F1 分数和支持度。这些也是在不平衡分类问题中更重要的性能衡量标准。

**决策阈值调整（即，改变分类阈值）**

分类问题中的阈值是将每个实例分类到预测类别的给定概率。默认阈值通常为 0.5（即 50%）。这是在 Python 和 R 中进行机器学习时的一个主要差异。R 提供了一行代码解决方案来调整阈值以应对类别不平衡。而 Python 没有内置函数来实现这一点，用户需要通过定义自定义脚本/函数来编程操作阈值。

![](../Images/48225c67de6b8769d9300f4c0ff33966.png)

一对显示决策阈值的图表。

+   Scikit-Learn

    +   在 Scikit-Learn 中没有一种标准的阈值设置方式。查看这篇文章了解你可以自己实现的一种方法：**在 Scikit-Learn 中微调分类器**

+   MLR

    +   `setThreshold(prediction, threshold)`

        这行`mlr`中的代码将自动更改你的阈值，并可以作为参数传递，以计算你的新性能指标（即混淆矩阵）。

### 结论

最终，MLR 和 Scikit-Learn 在处理机器学习时各有优缺点。我们的比较集中在使用其中一种进行机器学习，并不意味着使用其中一种而不是另一种。了解这两者能为该领域的人提供真正的竞争优势。对过程的概念性理解将使得使用这两种工具更为容易。

[原文](https://blog.exxactcorp.com/scikitlearn-vs-mlr-for-machine-learning/)。经许可转载。

**相关：**

+   [使用 Scikit-Learn 进行线性回归的初学者指南](https://www.kdnuggets.com/2019/03/beginners-guide-linear-regression-python-scikit-learn.html)

+   [在 R 中使用线性回归进行预测建模](https://www.kdnuggets.com/2018/06/linear-regression-predictive-modeling-r.html)

+   [你需要知道的：现代开源数据科学/机器学习生态系统](https://www.kdnuggets.com/2019/06/top-data-science-machine-learning-tools.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关主题

+   [KDnuggets 新闻，12月14日：3 个免费机器学习课程……](https://www.kdnuggets.com/2022/n48.html)

+   [每位机器学习工程师应具备的 5 项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [学习数据科学、机器学习和深度学习的稳固计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习…](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [突破数据壁垒：如何实现零样本、单样本和少样本学习](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)

+   [联邦学习：协作机器学习教程…](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)
