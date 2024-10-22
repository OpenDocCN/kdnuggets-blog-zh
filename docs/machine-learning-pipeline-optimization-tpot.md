# 机器学习管道优化与 TPOT

> 原文：[`www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html`](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

![图示](img/768d8e0940ca454156bd5deead1beb7d.png)

图片来源：[Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)于[Unsplash](https://unsplash.com/s/photos/pipeline?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# AutoML 与 TPOT

我已经有一段时间没查看[TPOT](https://github.com/EpistasisLab/tpot/)了，这是一种基于树的管道优化工具。TPOT 是一个 Python 自动化机器学习（AutoML）工具，通过遗传编程来优化机器学习管道。作者告诉我们要将其视为我们的“数据科学助手”。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

AutoML 的理由来源于这个想法：如果需要构建多个机器学习模型，使用各种算法和不同的超参数配置，那么可以自动化这一模型构建过程，模型性能和准确性的比较也可以自动化。

我希望重新审视 TPOT，看看我们是否能完善一个真正完全自动化的数据科学助手。假如我们可以扩展 TPOT 的功能，构建一个端到端的预测管道，将其指向数据集并得到预测结果，中间没有干预，那会怎样？当然，其他类似的工具也存在，但没有比自己动手构建机器学习管道和决策更好的方式来理解这一过程及其结果单一构建管道。

目标不一定是完全排除数据科学家，但提供一个基准或多个可能的解决方案，以便与手工制作的机器学习管道进行比较。在助手在后台默默工作时，专家可以提出更聪明的尝试方法。至少，结果预测管道可以作为数据科学家手动调整和干预的良好起点，大部分繁琐工作已经由其代劳。

一个 AutoML“解决方案”可能包括数据预处理、特征工程、算法选择、算法架构搜索和超参数调优的任务，或者这些不同任务的某个子集或变体。因此，自动化机器学习现在可以被认为是从仅执行单一任务（如自动特征工程）到完全自动化的管道（从数据预处理到特征工程，再到算法选择等等）的一切。那么，为什么不构建一个能完成所有这些的东西呢？

不管怎样，这个计划的第一步是重新熟悉 TPOT，这个项目最终将成为我们完全自动化预测管道优化器的核心。TPOT 是一个 Python 工具，它“通过遗传编程自动创建和优化机器学习管道。”TPOT 与 Scikit-learn 配合使用，自称为 Scikit-learn 的包装器。TPOT 是开源的，使用 Python 编写，旨在通过基于遗传编程的 AutoML 方法简化机器学习过程。最终结果是自动化超参数选择、使用多种算法建模，以及探索各种特征表示，所有这些都导致了迭代模型构建和模型评估。

![图示](img/49a6192aa0b8ec1192da08cbbdf0e83e.png)

TPOT 自动化的机器学习管道的各个方面 ([source](https://github.com/EpistasisLab/tpot/))

# 管道优化

我们将查看一个比[TPOT 仓库](https://github.com/EpistasisLab/tpot/)中的简单但非常有用的示例脚本更复杂的内容。代码应该比较直接且相对容易跟随，因此我不会对其进行详细审查。

这里是运行我们优化脚本后的一个示例输出：

```py
Optimizing prediction pipeline for the iris dataset with stratified k-fold cross-validation using the accuracy scoring metric

Pipeline optimization iteration: 0
>>> elapsed time: 135.48434898200503 seconds
>>> pipeline score on test data: 1.0

Pipeline optimization iteration: 1
>>> elapsed time: 132.3554882509925 seconds
>>> pipeline score on test data: 1.0

Pipeline optimization iteration: 2
>>> elapsed time: 133.29390010499628 seconds
>>> pipeline score on test data: 1.0

All best pipelines were the same:

Pipeline(memory=None,
         steps=[('stackingestimator',
                 StackingEstimator(estimator=KNeighborsClassifier(algorithm='auto',
                                                                  leaf_size=30,
                                                                  metric='minkowski',
                                                                  metric_params=None,
                                                                  n_jobs=None,
                                                                  n_neighbors=11,
                                                                  p=1,
                                                                  weights='uniform'))),
                ('extratreesclassifier',
                 ExtraTreesClassifier(bootstrap=True, ccp_alpha=0.0,
                                      class_weight=None, criterion='gini',
                                      max_depth=None,
                                      max_features=0.9000000000000001,
                                      max_leaf_nodes=None, max_samples=None,
                                      min_impurity_decrease=0.0,
                                      min_impurity_split=None,
                                      min_samples_leaf=18, min_samples_split=14,
                                      min_weight_fraction_leaf=0.0,
                                      n_estimators=100, n_jobs=None,
                                      oob_score=False, random_state=42,
                                      verbose=0, warm_start=False))],
         verbose=False)
```

输出提供了一些关于管道迭代的基本信息。如果你从脚本和输出的组合中无法看出，我们已经总共运行了 3 次优化过程；每次都使用了分层 10 折交叉验证；每次迭代的遗传优化过程都运行了 5 代，每代的种群规模为 50。你能计算出在这个过程中测试了多少个管道吗？这是我们在未来需要考虑的一个问题，尤其是与计算时间相关的实际原因。

正如你可能记得的，TPOT 会将最佳管道（或经过多次迭代后的管道）输出到文件中，然后可以用来重现相同的实验，或在新数据上使用相同的管道。我们将在创建我们完全自动化的端到端预测管道时利用这一点。

在我们的情况下，我们的脚本注意到每个生成的管道都是相同的，因此只输出了其中一个。在如此小的数据集上这是一个合理的结果，但由于遗传优化的性质，最佳管道在较大、复杂的数据集上可能会有所不同。

# 摘要

我们这次在脚本中尝试的一些方法，是我们过去没有做过的：

+   用于模型评估的交叉验证

+   对建模进行多次迭代——在如此小的数据集上可能没有用，但随着进展可能会有所帮助

+   比较这些多次迭代中产生的管道——它们都相同吗？

+   你知道 TPOT 现在在后台使用 PyTorch 来构建预测神经网络吗？

也许你已经看到了我们可以在上述内容上改进的一些方法。以下是我们未来实现中可能不希望做的一些具体事项：

+   我们希望考虑数据集拆分比例，以便获得理想的训练、验证和测试数据量

+   由于我们使用交叉验证进行训练和验证（与上述要点相关），我们希望保留测试数据，仅用于我们表现最佳的模型，而不是每一个模型上

+   由于特征选择/工程/构建是通过 TPOT 处理的，我们将希望在输入之前自动将类别变量转换为数值形式

+   我们希望能够处理更广泛的数据集 :)

+   还有更多内容！

这些要点虽然对实际建模很重要，但目前并不是问题，因为我们的重点只是建立结构，以便迭代地构建和评估机器学习管道。我们可以在未来进展时解决这些合理的担忧。

我鼓励你查看一下[TPOT 文档](http://epistasislab.github.io/tpot/)，看看它能为我们提供什么，因为我们利用它来帮助构建端到端的预测管道。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家和 KDnuggets 的主编，KDnuggets 是开创性的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计和优化、无监督学习、神经网络以及自动化机器学习方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。你可以通过 editor1 at kdnuggets[dot]com 联系他。

### 更多相关主题

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [SQL 查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [数据库优化：探索 SQL 中的索引](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)

+   [梯度下降：山地行者的优化数学指南](https://www.kdnuggets.com/gradient-descent-the-mountain-trekker-guide-to-optimization-with-mathematics)

+   [3 种基于研究的高级提示技术，提升 LLM 效率](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [超参数优化：10 个顶尖的 Python 库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)
