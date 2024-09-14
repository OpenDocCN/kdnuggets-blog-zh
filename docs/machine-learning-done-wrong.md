# 进行机器学习时的 7 个常见错误

> [`www.kdnuggets.com/2015/03/machine-learning-data-science-common-mistakes.html/2`](https://www.kdnuggets.com/2015/03/machine-learning-data-science-common-mistakes.html/2)

**4\. 当 n<<p 时使用高方差模型**

支持向量机（SVM）是最流行的现成建模算法之一，其最强大的特性之一是能够使用不同的核函数来拟合模型。SVM 的核函数可以看作是自动组合现有特征以形成更丰富特征空间的一种方式。由于这种强大的功能几乎是免费的，因此大多数从业者在训练 SVM 模型时默认使用核函数。然而，当数据的 n<<p（样本数量 << 特征数量）——在医疗数据等行业中很常见——时，更丰富的特征空间意味着更高的过拟合风险。实际上，当 n<<p 时，高方差模型应该完全避免。

**5\. 没有标准化的 L1/L2/... 正则化**

对线性或逻辑回归施加 L1 或 L2 惩罚大系数是一种常见的正则化方法。然而，许多从业者没有意识到在应用这些正则化之前标准化特征的重要性。

回到欺诈检测的例子，假设有一个具有交易金额特征的线性回归模型。如果没有正则化，当交易金额的单位是美元时，拟合的系数将比单位为美分时的拟合系数大约大 100 倍。使用正则化时，由于 L1 / L2 对较大系数的惩罚更多，因此如果单位是美元，交易金额会受到更多的惩罚。因此，正则化存在偏倚，倾向于惩罚较小规模的特征。为了解决这个问题，作为预处理步骤，标准化所有特征并使其具有相同的基础。

**6\. 使用线性模型而不考虑多重共线性预测变量**

想象构建一个包含两个变量 X1 和 X2 的线性模型，假设真实模型是 Y=X1+X2。理想情况下，如果数据观察时噪声很小，线性回归解决方案将恢复真实模型。然而，如果 X1 和 X2 存在共线性，则对大多数优化算法而言，Y=2*X1、Y=3*X1-X2 或 Y=100*X1-99*X2 都是同样有效的。这个问题可能不会对估计造成偏差，但确实使问题变得条件不良，并使系数权重无法解释。

**7\. 从线性或逻辑回归中解读系数的绝对值作为特征重要性**

由于许多现成的线性回归模型返回每个系数的 p 值，许多从业者认为，对于线性模型来说，系数的绝对值越大，对应的特征就越重要。这种情况很少发生，因为（a）改变变量的尺度会改变系数的绝对值，（b）如果特征之间存在多重共线性，系数可能会从一个特征转移到其他特征。此外，数据集中的特征越多，特征之间存在多重共线性的可能性就越大，通过系数解释特征重要性也就越不可靠。

那么，这里是：实际应用机器学习时的 7 个常见错误。这个列表并不是详尽无遗的，而只是为了促使读者考虑可能不适用于当前数据的建模假设。为了获得最佳的模型性能，选择最适合假设的建模算法非常重要——而不仅仅是你最熟悉的算法。

原文：[机器学习中的常见错误](http://ml.posthaven.com/machine-learning-done-wrong)

![Cheng-Tao Chu Cheng-Tao Chu](https://www.linkedin.com/pub/cheng-tao-chu/4/80/783) 是 Codecademy 的分析总监。专长：数据工程和机器学习。曾任职于：Google、LinkedIn 和 Square。

**相关：**

+   11 种聪明的过拟合方法及如何避免它们

+   大数据是否加速了医学研究？还是没有？

+   统计学教会我们关于大数据分析的 10 件事

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关话题

+   [避免这些每个 AI 新手都会犯的 5 个常见错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)

+   [5 个常见的数据科学错误及如何避免](https://www.kdnuggets.com/5-common-data-science-mistakes-and-how-to-avoid-them)

+   [如何应对 3 个常见的机器学习挑战](https://www.kdnuggets.com/2022/09/comet-tackle-3-common-machine-learning-challenges.html)

+   [新手数据科学家应避免的错误](https://www.kdnuggets.com/2022/06/mistakes-newbie-data-scientists-avoid.html)

+   [可能影响你数据分析准确性的 3 个错误](https://www.kdnuggets.com/2023/03/3-mistakes-could-affecting-accuracy-data-analytics.html)

+   [软件错误与权衡：Tomasz Lelek 等人的新书](https://www.kdnuggets.com/2021/12/manning-software-mistakes-tradeoffs-book.html)
