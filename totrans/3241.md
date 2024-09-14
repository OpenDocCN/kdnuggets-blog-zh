# 新时代的机器学习算法在零售贷款中的应用

> 原文：[https://www.kdnuggets.com/2017/09/machine-learning-algorithms-lending.html](https://www.kdnuggets.com/2017/09/machine-learning-algorithms-lending.html)

**由Jayesh Ametha撰写**

十多年前，当我加入一家大型美国信用卡公司时，发现预测分析仅限于多元回归和逻辑回归模型，这让我感到惊讶。这与之前在NASA/NIST资助的初创公司工作的经历形成了对比，在那些公司中，经常应用包括SVM、神经网络、随机森林或梯度提升树等更广泛的机器学习（ML）方法。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

![Credit](../Images/ce13f0629bb52dc2db0c33f2125c6de1.png)

在零售贷款中使用更简单模型的原因有很多。首先，决策框架已经到位，使得输入特征选择相对简单。例如，对于信用决策，可以从5Cs of Credit（信用的5个要素：Character、Capacity、Capital、Collateral、Conditions）的角度考虑，并寻找满足这些要素的数据变量。这不像使用深度学习从原始图像中创建特征那么困难。其次，目标变量与输入的关系并不复杂，例如，信用风险与收入之间有一个平滑的反向关系。实际上并不需要径向基函数将收入转换到更高维空间，就像SVM基于图像分类所需的那样。第三，与今天不同的是，训练和部署平台当时并不适合复杂方法。最后，一个常被提到的原因是模型可解释性（尽管经验丰富的高级机器学习模型用户可能会对此提出质疑）。

随着时间的推移，前述的机器学习方法开始被探索，因为开源软件包变得普遍，数据也以不同的形式出现。然而，业务的主要价值来自于识别新的强大数据，这些数据可以显著改善客户级别的决策。虽然替代数据源始终是一个重点领域，但有些特定的业务问题可以通过近年来普及的新机器学习算法更好地解决。在这里，我们讨论三种这样的算法，重点介绍它们在零售贷款中的应用。

**RNNs / LSTMs：**

来自银行存款、贷款或信用卡交易的序列数据可以用来生成强大的洞察和行动。一些示例用例：

+   **信用风险**：理解消费者的信用历史和交易量/档案随时间的变化可以帮助做出更好的信用决策，无论是对于新申请者还是现有客户。

+   **欺诈检测**：识别特定的交易序列可能会提示欺诈或洗钱行为，并可作为触发器来阻止信用访问并进行调查。

+   **流失预测**：理解交易量和客户档案随时间的变化可以帮助识别可能流失的客户，并采取保留措施。

+   **产品交叉销售/上销售**：通过查看交易序列来评估客户是否经历了生活事件或“升级”以潜在改变产品或条款。

+   **客户服务**：通过记住和学习过去的互动，模型可以改善客户互动（无论是人工还是机器人驱动的）。

常见的统计和机器学习算法并不适合处理这类数据。虽然统计学家传统上创建了诸如不同时间窗口的平均值等特征来捕捉一些趋势信息，但[LSTM（长短期记忆）网络](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)是一类专门构建来学习序列数据的递归神经网络。

**矩阵分解：**

推荐系统因其在零售（亚马逊）、网络流媒体（Netflix）和知识共享（Quora）中的应用而变得流行。许多实现使用了[矩阵分解](https://blog.insightdatascience.com/explicit-matrix-factorization-als-sgd-and-all-that-jazz-b00e4d9b21ea)，这是一种通过更快的计算能力实现的传统线性代数公式。以下是一些适用于零售贷款的用例：

+   **下一步最佳推荐**：这是为了确定对现有客户的下一步推荐。传统上，这通过为每个客户细分构建大量响应倾向模型来解决。“矩阵分解”可以帮助用一个优雅的模型替代，特别是当需要选择的产品众多时。

+   **缺失数据**：建模前的一个关键预处理步骤是处理数据集中缺失值（尽管基于树的算法本身也会处理）。矩阵分解可以用于从整体数据集中学习“相似”模式，并在应用非树型机器学习算法之前填补缺失值。

**深度学习：**

不言而喻，[深度学习](https://www.toptal.com/machine-learning/an-introduction-to-deep-learning-from-perceptrons-to-deep-networks) 是过去 5 年里最为显著的新兴机器学习算法，其在从大规模非结构化数据集中生成洞察方面取得了显著成功。一些零售贷款的使用案例包括：

+   **语音转文本:** 现成的深度学习软件可以将客户的音频转换为文本，然后可以与其他机器学习方法结合用于自动智能客服。

+   **社交倾听:** 将深度学习应用于来自社交媒体和客户日志的非结构化文本数据，可以帮助理解公司的 4C（品牌评估、产品反馈）、竞争对手（基准测试、战略变化）、客户（情感分析）和气候（市场趋势）。

+   **客户细分:** 无监督聚类通常用于细分和描述客户基础，以更好地理解并制定针对每个细分市场的战略。其训练过程受到“维度灾难”的困扰。虽然像 PCA 这样的方法被应用于解决这一问题，但深度学习可以作为创建更先进的低维特征的替代方案。

现代的端到端大数据平台提供了训练新型机器学习算法并简化其部署的计算能力。像部分依赖和决策边界距离这样的变量重要性度量可以帮助模型的可解释性。对于特定的分析问题，使用合适的技术并避免复杂性仍然很重要。模型的稳健性、增量业务价值、客户体验、实施和治理应被视为至关重要。机器学习和大量的替代数据源无疑为零售贷款领域带来了更令人兴奋的“建模”时代。

**简介: [Jayesh Ametha](https://www.linkedin.com/in/jayeshametha/)** 是一位拥有超过 15 年业务战略、信用风险和高级分析经验的零售银行专业人士。

**相关:**

+   [什么是优化及其如何使业务受益？](/2017/08/optimization-benefit-business.html)

+   [深度学习如何改变金融和零售行业](/2017/05/rework-deep-learning-finance-retail.html)

+   [数据科学如何推动金融科技革命的 7 种方式](/2016/09/7-ways-how-data-science-fuels-fintech-revolution.html)

### 更多相关内容

+   [停止学习数据科学以寻找目标，寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
