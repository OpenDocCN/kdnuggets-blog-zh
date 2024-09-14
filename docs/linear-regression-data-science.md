# 数据科学的线性回归

> 原文：[https://www.kdnuggets.com/2022/07/linear-regression-data-science.html](https://www.kdnuggets.com/2022/07/linear-regression-data-science.html)

![数据科学的线性回归](../Images/62fce92048c2a2ef4da383ecd1217d6e.png)

作者提供的图片

## 关键要点

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的IT需求

* * *

+   大多数有兴趣进入数据科学领域的初学者总是担心数学要求。

+   数据科学是一个非常量化的领域，需要高级数学知识。

+   但要开始，您只需掌握几个数学主题。

+   在这篇文章中，我们讨论了线性回归在数据科学和机器学习中的重要性。

# 线性回归（连续目标变量）

回归模型是最受欢迎的机器学习模型。回归模型用于预测连续尺度上的目标变量。回归模型几乎在每个学科领域都有应用，因此，它是最广泛使用的机器学习模型之一。本文将讨论线性回归的基础知识，并面向数据科学领域的初学者。

## 1\. 简单线性回归

在简单线性回归中，只有一个预测变量。由于我们的目标是预测船员变量，我们从**图 1**中看到，舱位变量与船员变量的相关性最高。因此，我们的简单回归模型可以表示为：

![简单线性回归](../Images/618633767d020925ac04805709d07647.png)

其中 m 是斜率或回归系数，c 是截距

## 2\. 多重线性回归

假设目标变量现在依赖于几个预测变量（例如四个预测变量），那么可以使用多重回归分析来建模系统：

![多重线性回归](../Images/fb66f84ffe338318ae7822bb3e69ac41.png)

其中 X 是特征矩阵，w_0 是截距，w_1、w_2、w_3 和 w_4 是回归系数。

# 评估线性回归模型

用于评估线性回归模型性能的最受欢迎的指标是 R2 分数指标，其计算方法如下：

![评估线性回归模型](../Images/4c4ccfc18a244153f71a4a160defdda7.png)

R2 评分的取值范围在 0 和 1 之间。当 R2 接近 1 时，表示预测值与实际值非常接近。如果 R2 接近零，则表示模型的预测能力非常差。

还可以使用以下其他指标来评估线性回归模型：

**MSE（均方误差）**：使用欧几里得距离计算误差。MSE 仅给出误差的大小。

![均方误差](../Images/4be79045813bcd13f92c1774fe7712ec.png)

**MAE（均值绝对误差）**：使用曼哈顿距离计算误差。MAE（如同 MSE）仅给出误差的大小。

![均值绝对误差](../Images/ad0fdc15abd2ae9d7dfb087a07de8897.png)

**ME（均值误差）**：跟踪误差的符号，模型是过度预测（*ME > 0*）还是低估预测（*ME < 0*）？

![均值误差](../Images/c128a3db4cb708261dc023bbbe09f07e.png)

+   *R2 评分* 是评估线性回归模型性能的非常流行的指标。

+   在比较两个或更多模型时，使用 *MSE* 或 *MAE*，*MSE* 或 *MAE* 的值越低，模型越好。

+   当你想了解你的模型是否在平均上过度预测（*ME > 0*）或低估预测（*ME < 0*）时，请使用 *ME*。也可以使用 *R2 评分* 来比较不同的模型。

# 案例研究：预测船员数量的机器学习模型

在本案例研究中，我们使用 cruise_ship_info.csv 数据集构建了一个多元线性回归模型来预测船员数量。可以从这个 GitHub 仓库下载数据集和代码：[https://github.com/bot13956/ML_Model_for_Predicting_Ships_Crew_Size](blank)

# 总结

+   线性回归（用于连续目标变量预测）是最流行的机器学习模型。回归模型几乎在每个学科领域都有应用，因此它是最广泛使用的机器学习模型之一。

+   线性回归模型可以分为简单回归（单一特征）和多重回归（多个目标变量）模型。

+   线性回归模型可以使用如 Pylab、Numpy 或 scikit-learn 等软件库来实现。

+   评估回归模型时可以使用多种指标，如 *MSE*、*ME*、*MAE* 和 *R2* 评分。*R2* 评分仍然是最受欢迎的指标。

+   其他回归模型包括 KNR（[k-近邻回归](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html)）和 SVR（[支持向量回归](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVR.html)）。

**[本杰明·O·泰约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，也是 DataScienceHub 的所有者。此前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程和物理学。

### 更多内容

+   [KDnuggets 新闻 22:n12，2023年3月：最佳数据科学书籍](https://www.kdnuggets.com/2022/n12.html)

+   [线性回归与逻辑回归的比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [3个理由说明你应该使用线性回归模型而不是…](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [预测制作：Python中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [线性回归模型选择：平衡简洁性与复杂性](https://www.kdnuggets.com/2023/02/linear-regression-model-selection-balancing-simplicity-complexity.html)
