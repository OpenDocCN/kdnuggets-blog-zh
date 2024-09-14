# 必知：为什么在机器学习模型中使用较少的预测变量可能更好？

> 原文：[https://www.kdnuggets.com/2017/04/must-know-fewer-predictors-machine-learning-models.html](https://www.kdnuggets.com/2017/04/must-know-fewer-predictors-machine-learning-models.html)
> 
> **编辑注：** 本文最初作为对我们[17 个必知的数据科学面试问题与答案](/2017/02/17-data-science-interview-questions-answers.html)系列中一个问题的回答而发布。由于回答内容足够详尽，因此决定将其独立成文。

以下是为什么使用较少的预测变量可能比使用更多预测变量更好的几个原因：

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

**冗余/无关性：**

如果你处理的预测变量很多，那么很可能它们之间存在隐藏的关系，从而导致冗余。除非你在数据分析的早期阶段识别并处理这些冗余（通过仅选择非冗余的预测变量），否则这可能会对你后续的步骤造成很大阻碍。

同样，也有可能并不是所有的预测变量对因变量有显著影响。你应该确保你选择的预测变量集合中没有任何无关的变量——即使你知道数据模型会通过赋予它们较低的重要性来处理它们。

*注意：冗余和无关性是两个不同的概念——一个相关的特征可能由于存在其他相关特征而变得冗余。*

**过拟合**：

即使你有大量的预测变量，且它们之间没有任何关系，通常还是建议使用较少的预测变量。具有大量预测变量的数据模型（也称为复杂模型）常常会遇到过拟合的问题，在这种情况下，数据模型在训练数据上表现很好，但在测试数据上表现较差。

**生产力**：

假设你有一个项目，其中有大量的预测变量，并且所有这些变量都是相关的（即对因变量有可测量的影响）。因此，你显然希望使用所有变量，以获得一个成功率非常高的数据模型。尽管这种方法听起来非常诱人，但实际考虑因素（如可用数据量、存储和计算资源、完成所需时间等）使得这种方法几乎不可能实现。

因此，即使你有大量相关的预测变量，使用较少的预测变量（通过特征选择筛选或通过特征提取开发）也是一个好主意。这本质上类似于帕累托原则，即许多事件中，大约80%的效果来自20%的原因。

专注于这20%最重要的预测变量，将大大有助于在合理时间内建立成功率可观的数据模型，而不需要大量不切实际的数据或其他资源。

![训练与复杂度](../Images/b30a107ef5009d61aa270f2047eb9950.png)

训练误差与测试误差对模型复杂度的关系（来源：[Quora](https://www.quora.com/Why-might-it-be-preferable-to-include-fewer-predictors-over-many/answer/Sergül-Aydöre)由[Sergul Aydore](https://www.quora.com/profile/Sergül-Aydöre)发布）

**可理解性**：

具有较少预测变量的模型更容易理解和解释。由于数据科学步骤将由人类执行，结果也将由人类展示（并希望被使用），因此考虑到人脑的全面能力非常重要。这实际上是一种权衡——你放弃了一些可能带来数据模型成功率的潜在好处，同时使你的数据模型更易于理解和优化。

如果在项目结束时你需要向某人展示结果，而他们不仅对高成功率感兴趣，还希望了解“幕后”的情况，这个因素就尤为重要。

**相关：**

+   [17个必须了解的数据科学面试问题及答案](/2017/02/17-data-science-interview-questions-answers.html)

+   [接近（几乎）任何机器学习问题](/2016/08/approaching-almost-any-machine-learning-problem.html)

+   [识别可能更好预测的变量](/2017/02/schmarzo-variables-better-predictors.html)

### 更多相关内容

+   [为什么机器学习模型会默默“死去”？](https://www.kdnuggets.com/2022/01/machine-learning-models-die-silence.html)

+   [ETL与机器学习有什么关系？](https://www.kdnuggets.com/2022/08/etl-machine-learning.html)

+   [评估你的机器学习模型的（更好）方法](https://www.kdnuggets.com/2022/01/much-better-approach-evaluate-machine-learning-model.html)

+   [为什么你应该使用线性回归模型而不是……](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [每个数据科学家应该具备的13项技能](https://www.kdnuggets.com/2022/03/top-13-skills-every-data-scientist.html)

+   [21份数据科学面试必备的备忘单：解锁…](https://www.kdnuggets.com/2022/06/21-cheat-sheets-data-science-interviews.html)
