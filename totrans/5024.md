# 你会在泰坦尼克号上生存吗？Python机器学习指南第二部分

> 原文：[https://www.kdnuggets.com/2016/07/titanic-machine-learning-guide-part-2.html/2](https://www.kdnuggets.com/2016/07/titanic-machine-learning-guide-part-2.html/2)

**分类 - 有趣的部分**

我们将从一个简单的决策树分类器开始。决策树一次检查一个变量，并根据该值的结果分裂成两个分支之一，然后对下一个变量执行相同的操作。关于决策树如何工作的精彩可视化解释可以在[这里](http://www.r2d3.us/visual-intro-to-machine-learning-part-1/)找到。

如果我们将最大层数设置为3，这就是经过训练的泰坦尼克号数据集的决策树的样子：

[![决策树](../Images/ba8a5a296a46ba87dcc053bdef4ff8e7.png)](/wp-content/uploads/socialcops-tree.jpg)

树首先按性别分裂，然后按类别分裂，因为它在训练阶段了解到这些是决定生存的两个最重要的特征。深蓝色框表示可能生存的乘客，而深橙色框则代表几乎肯定会遇难的乘客。有趣的是，在按类别分裂后，决定女性生存的主要因素是她们支付的票价，而男性的决定因素是他们的年龄（儿童更有可能生存）。

要创建这个树，首先我们初始化一个未训练的决策树分类器（这里我们将树的最大深度设置为10）。接下来，我们将这个分类器“拟合”到我们的训练集上，使其能够学习不同因素如何影响乘客的生存率。现在决策树已经准备好了，我们可以使用测试数据对其进行“评分”，以确定它的准确性。

```py
clf_dt = tree.DecisionTreeClassifier(max_depth=10)

clf_dt.fit (X_train, y_train)
clf_dt.score (X_test, y_test)

# 0.78947368421052633

```

*[点击这里查看要点。](https://gist.github.com/triestpa/5858dc07caab1e33af10178fd1f236d5)*

结果为0.7703，意味着模型正确预测了77%的测试集生存情况。对于我们的第一个模型来说，表现不错！

如果你是一个细心、怀疑的读者（你应该这样），你可能会想到模型的准确性可能会根据选择的训练和测试集的行而有所不同。我们将通过使用洗牌验证器来解决这个问题。

```py
shuffle_validator = cross_validation.ShuffleSplit(len(X), n_iter=20, test_size=0.2, random_state=0)
def test_classifier(clf):
    scores = cross_validation.cross_val_score(clf, X, y, cv=shuffle_validator)
    print("Accuracy: %0.4f (+/- %0.2f)" % (scores.mean(), scores.std()))

test_classifier(clf_dt)

# Accuracy: 0.7742 (+/- 0.02)

```

*[点击这里查看要点。](https://gist.github.com/triestpa/e326db921a5400428aeb33130fb3152b)*

这个洗牌验证器应用了与之前相同的随机20:80拆分，但这次生成了20个独特的拆分排列。通过将此洗牌验证器作为参数传递给“cross_val_score”函数，我们可以针对每个不同的拆分对分类器进行评分，并计算结果的平均准确性和标准差。

结果显示我们的决策树分类器的总体准确率为77.34%，虽然根据训练/测试拆分，准确率可能会高达80%或降到75%。使用scikit-learn，我们可以轻松地用完全相同的语法测试其他机器学习算法。

```py
clf_rf = ske.RandomForestClassifier(n_estimators=50)
test_classifier(clf_rf)

# Accuracy: 0.7837 (+/- 0.02)

clf_gb = ske.GradientBoostingClassifier(n_estimators=50)
test_classifier(clf_gb)

# Accuracy: 0.8201 (+/- 0.02)

eclf = ske.VotingClassifier([('dt', clf_dt), ('rf', clf_rf), ('gb', clf_gb)])
test_classifier(eclf)

# Accuracy: 0.8036 (+/- 0.02)

```

*[点击这里查看概要。](https://gist.github.com/triestpa/b6b3db3ac3424b664b59fbbf48d19859)*

“随机森林”分类算法将创建大量（通常质量较差的）树，使用输入变量的不同随机子集，并返回最多树所返回的预测。这有助于避免“过拟合”，这是一种当模型过于紧密地拟合训练数据中的任意相关性，以至于在测试数据上表现不佳的问题。

“梯度提升”分类器将生成许多弱小、浅层的预测树，并将它们组合或“提升”成一个强大的模型。该模型在我们的数据集上表现非常好，但缺点是相对较慢且难以优化，因为模型构建是顺序进行的，因此不能并行化。

“投票”分类器可以用来将多个概念上不同的分类模型应用于同一数据集，并返回所有分类器中的多数票。例如，如果梯度提升分类器预测乘客将不会生存，而决策树和随机森林分类器预测他们将生存，则投票分类器将选择后者。

这是对每种技术的非常简要和非技术性概述，因此我鼓励你深入了解所有这些算法的数学实现，以获得对它们相对优缺点的更深入理解。更多分类算法可以在scikit-learn中“开箱即用”，可以在[这里](http://scikit-learn.org/stable/modules/ensemble.html)探索。

![Patrick Triest](../Images/58fde3736bd01dbcfdf3fd2657ea5996.png)**简介：[Patrick Triest](https://www.linkedin.com/in/triestpa)** 是一位23岁的Android开发者/物联网工程师/数据科学家/有志成为先驱者，来自波士顿，现在在SocialCops工作。他热衷于学习，有时在发现一些特别酷的东西后会非常兴奋并写下自己的见解。

[原文](http://blog.socialcops.com/engineering/machine-learning-python)。经许可转载。

**相关：**

+   [掌握Python机器学习的7个步骤](/2015/11/seven-steps-machine-learning-python.html)

+   [数据科学和机器学习的前10大IPython笔记本教程](/2016/04/top-10-ipython-nb-tutorials.html)

+   [R学习路径：从R初学者到专家的7个步骤](/2016/03/datacamp-r-learning-path-7-steps.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

### 更多相关主题

+   [数据科学家需要专门化以应对技术冬季](https://www.kdnuggets.com/2023/08/data-scientists-need-specialize-survive-tech-winter.html)

+   [如果我重新开始学习数据科学，我会怎么做？](https://www.kdnuggets.com/2020/08/start-learning-data-science-again.html)

+   [什么时候集成技术是一个好选择？](https://www.kdnuggets.com/2022/07/would-ensemble-techniques-good-choice.html)

+   [数据科学面试指南 - 第2部分：面试资源](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-2-interview-resources.html)

+   [数据科学面试指南 - 第1部分：结构](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-1-structure.html)

+   [想成为数据科学家？第1部分：你需要的10项硬技能](https://www.kdnuggets.com/want-to-become-a-data-scientist-part-1-10-hard-skills-you-need)
