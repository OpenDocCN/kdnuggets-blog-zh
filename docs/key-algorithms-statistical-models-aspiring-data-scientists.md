# 有志数据科学家的关键算法和统计模型

> 原文：[`www.kdnuggets.com/2018/04/key-algorithms-statistical-models-aspiring-data-scientists.html`](https://www.kdnuggets.com/2018/04/key-algorithms-statistical-models-aspiring-data-scientists.html)

![评论](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

作为一名已经从业多年的数据科学家，我经常在 LinkedIn 和 Quora 上收到学生和职业转行者的职业建议或课程选择指导。一些问题围绕教育路径和课程选择，但许多问题集中在当前数据科学中常见的算法或模型上。

面对大量的算法选择，很难知道从哪里开始。课程可能包括当前行业中不常用的算法，也可能排除一些当前并不流行但非常有用的方法。基于软件的程序可能会忽略重要的统计概念，而基于数学的程序可能会略过一些关键的算法设计主题。

![算法设计主题](img/d44c47adc5657cd240475fb395aaf2df.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

我为有志的数据科学家准备了一份简短的指南，特别关注统计模型和机器学习模型（监督和无监督）；许多这些主题在教科书、研究生级统计课程、数据科学训练营和其他培训资源中都有涵盖（其中一些在文章的参考部分包含）。由于机器学习是统计学的一个分支，机器学习算法技术上属于统计知识，同时也涉及数据挖掘和更多基于计算机科学的方法。然而，由于一些算法与计算机科学课程内容重叠，并且许多人将传统统计方法与新方法分开，我将在列表中将这两个分支分开。

![机器学习方法](img/f7261b772d9e5959e3437d6918d965fa.png)

统计方法包括一些在训练营和证书课程中常见的方法概述，以及一些在研究生统计课程中通常教授但在实践中非常有用的不太常见的方法。我建议的所有工具都是我经常使用的：

1) 广义线性模型，它是大多数监督学习方法的基础（包括逻辑回归和 Tweedie 回归，它对行业中遇到的大多数计数或连续结果进行广义化……）

2) 时间序列方法（ARIMA、SSA、基于机器学习的方法）

3) 结构方程建模（用于建模和测试中介路径）

4) 因子分析（用于调查设计和验证的探索性和确认性）

5) 功效分析/试验设计（特别是基于模拟的试验设计，以避免过度强大的分析）

6) 非参数检验（从头开始推导测试，特别是通过模拟）/MCMC

7) K 均值聚类

8) 贝叶斯方法（朴素贝叶斯、贝叶斯模型平均、贝叶斯自适应试验……）

9) 惩罚回归模型（弹性网、LASSO、LARS……）以及一般模型中添加的惩罚（SVM、XGBoost……），这些对预测变量多于观测值的数据集非常有用（在基因组学和社会科学研究中很常见）

10) 基于样条的模型（MARS……）用于灵活建模过程

11) 马尔可夫链和随机过程（时间序列建模和预测建模的替代方法）

12) 缺失数据插补方案及其假设（missForest，MICE……）

13) 生存分析（对建模流失和退化过程非常有帮助）

14) 混合建模

15) 统计推断和分组测试（A/B 测试及许多市场营销活动中实现的更复杂的设计）

机器学习扩展了许多这些框架，特别是 K 均值聚类和广义线性建模。一些在许多行业中通用的有用技术（以及一些更为晦涩但意外有用的算法，但很少在训练营或证书课程中教授）包括：

1) 回归/分类树（广义线性模型的早期扩展，具有高准确性、良好的可解释性和低计算开销）

2) 降维（PCA 和流形学习方法如 MDS 和 tSNE）

3) 经典前馈神经网络

4) 装袋集成（作为随机森林和 KNN 回归集成算法的基础）

7) 提升集成（作为梯度提升和 XGBoost 算法的基础）

8) 参数调优或设计项目的优化算法（遗传算法、量子启发的进化算法、模拟退火、粒子群优化）

9) 拓扑数据分析工具，这些工具特别适合用于小样本量的无监督学习（持久同源性、Morse-Smale 聚类、Mapper……）

10) 深度学习架构（一般深度架构）

11) KNN 方法用于局部建模（回归、分类）

12) 基于梯度的优化方法

13) 网络指标和算法（中心性测量、介数、多样性、熵、拉普拉斯、流行病传播、谱聚类）

14) 深度架构中的卷积和池化层（在计算机视觉和图像分类模型中尤为有用）

15) 层次聚类（与 k 均值聚类和拓扑数据分析工具相关）

16) 贝叶斯网络（路径挖掘）

17) 复杂性和动态系统（与微分方程相关，但通常用于建模没有已知驱动因素的系统）

根据所选择的行业，可能需要与自然语言处理（NLP）或计算机视觉相关的额外算法。然而，这些是数据科学和机器学习的专业领域，进入这些领域的人通常已经是该领域的专家。

一些在学术项目之外学习这些方法的资源包括：

Christopher, M. B. (2016). 《模式识别与机器学习》。Springer-Verlag New York.

Friedman, J., Hastie, T., & Tibshirani, R. (2001). 《统计学习的元素》（第 1 卷，第 337-387 页）。纽约：Springer 统计系列。

[`www.coursera.org/learn/machine-learning`](https://www.coursera.org/learn/machine-learning)

[`professional.mit.edu/programs/short-programs/machine-learning-big-data`](http://professional.mit.edu/programs/short-programs/machine-learning-big-data)

[`www.slideshare.net/ColleenFarrelly/machine-learning-by-analogy-59094152`](https://www.slideshare.net/ColleenFarrelly/machine-learning-by-analogy-59094152)

**相关：**

+   [你需要知道的十种机器学习算法，成为数据科学家必备](https://www.kdnuggets.com/2018/04/10-machine-learning-algorithms-data-scientist.html)

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)

+   [Netflix 的数据科学与娱乐制作艺术](https://www.kdnuggets.com/2018/04/data-science-entertainment-netflix.html)

### 更多相关主题

+   [成为出色数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学，寻找目标，再寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [构建强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)
