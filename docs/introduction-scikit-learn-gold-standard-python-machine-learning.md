# Scikit Learn 简介：Python 机器学习的黄金标准

> 原文：[https://www.kdnuggets.com/2019/02/introduction-scikit-learn-gold-standard-python-machine-learning.html](https://www.kdnuggets.com/2019/02/introduction-scikit-learn-gold-standard-python-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![图](../Images/b4c2ace28629c74c6bce82ecc2623ca6.png)

对 Scikit Learn 的众多机器学习模型的比较

### 机器学习 黄金标准

如果你打算在 Python 中进行机器学习，[Scikit Learn](https://scikit-learn.org/stable/index.html) 是黄金标准。Scikit-learn 提供了广泛的监督学习和非监督学习算法。最棒的是，它是迄今为止最简单和最干净的机器学习库。

Scikit Learn 的创建带有软件工程思维。它的核心 API 设计围绕易用性、强大功能和对研究工作的灵活性展开。这种稳健性使其非常适合用于任何端到端的机器学习项目，从研究阶段到生产部署。

### Scikit Learn 提供了什么

Scikit Learn 构建在几个常见的数据和数学 Python 库之上。这种设计使得它们之间的集成变得非常简单。你可以将 numpy 数组和 pandas 数据框直接传递给 Scikit 的机器学习算法！它使用以下库：

+   [**NumPy**](http://www.numpy.org/): 处理矩阵，尤其是数学运算

+   [**SciPy**](https://www.scipy.org/): 科学和技术计算

+   [**Matplotlib**](https://matplotlib.org/): 数据可视化

+   [**IPython**](https://ipython.org/): Python 的交互式控制台

+   [**Sympy**](https://www.sympy.org/en/index.html): 符号数学

+   [**Pandas**](https://pandas.pydata.org/): 数据处理、操作和分析

Scikit Learn 专注于机器学习，例如 *数据建模*。它 *不关心* 数据的加载、处理、操作和可视化。因此，使用上述库，尤其是 NumPy 来完成这些额外的步骤是自然和常见的做法；它们是天作之合！

Scikit 的强大算法集合包括：

+   **回归：** 拟合线性和非线性模型

+   **聚类：** 无监督分类

+   **决策树：** 用于分类和回归任务的树生成和修剪

+   **神经网络：** 端到端的训练，适用于分类和回归。层可以很容易地在元组中定义

+   **支持向量机：** 用于学习决策边界

+   **朴素贝叶斯：** 直接概率建模

更重要的是，它还有一些其他库不常提供的非常方便和高级的功能：

+   **集成方法：** 提升、装袋、随机森林、模型投票和平均

+   **特征操作：** 降维、特征选择、特征分析

+   **异常值检测：** 用于检测异常值和排除噪声

+   **模型选择和验证：** 交叉验证、超参数调整和指标

### 一次口味测试

为了让你感受到使用 Scikit Learn 训练和测试机器学习模型的简单，这里有一个如何为决策树分类器做这件事的例子！

决策树用于分类和回归在 Scikit Learn 中都非常容易使用，并且有一个内置的类。我们首先加载我们的数据集，该数据集实际上是库中自带的。然后，我们将初始化我们的决策树分类器，这也是一个内置的类。训练只需一行代码！`.fit(X, Y)` 函数训练模型，其中*X* 是输入的 numpy 数组，*Y* 是对应的输出 numpy 数组。

Scikit Learn 还允许我们使用 graphviz 库来可视化我们的树。它提供了一些选项，有助于可视化决策节点和模型学习到的分裂，这对理解其工作原理非常有用。下面我们将根据特征名称为节点着色，并显示每个节点的类别和特征信息。

![](../Images/ecaf20a53098ea1fa03240e432bd03e3.png)

此外，Scikit Learn 的文档非常精美！每个[算法参数](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html#sklearn.tree.DecisionTreeClassifier)都解释得很清楚，命名也很直观。此外，他们还提供了[带有示例代码的教程](https://scikit-learn.org/stable/modules/tree.html)，介绍如何训练和应用模型、其优缺点以及实际应用技巧！

### 喜欢学习？

关注我的[twitter](https://twitter.com/GeorgeSeif94)，我会发布最新的人工智能、技术和科学相关内容！

**简介： [George Seif](https://towardsdatascience.com/@george.seif94)** 是一名认证极客和人工智能/机器学习工程师。

[原文](https://towardsdatascience.com/an-introduction-to-scikit-learn-the-gold-standard-of-python-machine-learning-e2b9238a98ab)。转载需经许可。

**相关：**

+   [Python 中的 5 种快速且简单的数据可视化（带代码）](/2018/07/5-quick-easy-data-visualizations-python-code.html)

+   [数据科学家需要了解的 5 种聚类算法](/2018/06/5-clustering-algorithms-data-scientists-need-know.html)

+   [为你的回归问题选择最佳机器学习算法](/2018/08/selecting-best-machine-learning-algorithm-regression-problem.html)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关内容

+   [成为伟大的数据科学家需要的 5 个关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，然后再以目标来找回学习](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
