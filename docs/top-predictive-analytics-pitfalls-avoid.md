# 避免顶级预测分析陷阱

> 原文：[https://www.kdnuggets.com/2017/01/top-predictive-analytics-pitfalls-avoid.html](https://www.kdnuggets.com/2017/01/top-predictive-analytics-pitfalls-avoid.html)

**由 Robin Davies， [Principa.](http://www.principa.co.za)**

![](../Images/c1e9232e97c11fb89eecfad3fd888e9a.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

预测分析可以产生惊人的结果。基于历史事件中的观察模式进行未来决策所获得的提升，可能远远超过依赖直觉或凭借轶事事件来进行决策的效果。虽然有许多示例展示了在各行各业中可能获得的提升，但我们最近在零售行业做的测试显示，应用稳定的预测模型使我们在产品接受度上提高了五倍，相较于随机样本。说实话，如果预测分析，特别是机器学习没有取得令人印象深刻的结果，就不会如此受到关注。

[阅读我们在使用机器学习预测2015年橄榄球世界杯结果时学到的经验教训](http://insights.principa.co.za/machine-learning-out-predicts-humans-during-the-rugby-world-cup)

但预测模型并不是万无一失的。它们有点像赛马：对变化非常敏感，容易让骑手感到困惑。

机器学习的商品化使得数据科学比以往任何时候都更容易被非数据科学家所接受。考虑到这一点，我和我的同事坐下来思考，我们制定了以下避免的顶级预测分析陷阱清单，以确保你的模型按预期运行：

+   **对基础训练数据做出不正确的假设。**

    匆忙进行过多的假设往往会带来相应的尴尬。花时间理解数据和分布中的趋势、缺失值、异常值等。

+   **处理低数据量。**

    低数据量是数据科学家的痛点——它们可能导致统计上薄弱、不稳定和不可靠的模型。

+   **过拟合的问题。**

    换句话说，创建一个有很多分支的模型，从而似乎提供了更好的目标变量区分，但在现实世界中失败，因为它在模型中引入了太多噪音。

+   **训练数据中的偏差。**

    例如，你只向千禧一代提供了某种产品。那么，猜猜看？千禧一代将在模型中表现得很强。

+   **将测试数据包含在训练数据中。**

    曾经有一些巨大的失败，其中测试数据被包括在训练数据中——这给人一种模型表现会非常好的印象，但实际上导致了一个破损的模型。在预测分析的世界里，如果结果好得令人难以置信，值得花更多时间进行验证，甚至获得第二意见来检查你的工作。

+   **未能对提供的数据进行创意处理。**

    通过创建一些聪明的特征或属性来更好地解释数据中的趋势，可以显著提升预测模型。数据科学家往往只会使用提供的数据，而不会花足够的时间考虑来自底层数据的更具创意的特征，这些特征可以以改进算法无法实现的方式强化模型。

+   **期望机器理解业务。**

    机器还无法搞清楚业务问题是什么，以及如何最佳解决问题。这并不总是简单明了，可能需要一些细致的思考，包括与业务相关方的全面讨论。

+   **使用错误的度量标准来衡量模型的表现。**

    例如，在10,000个案例中只有两个案例是欺诈的，9,998个案例不是欺诈。如果在模型训练中使用的性能度量只是简单的准确率，那么模型将试图最大化准确率。因此，如果它预测所有10,000个案例都不是欺诈，那么模型的准确率将达到99.98%，这看起来非常惊人，但在识别欺诈方面并没有任何实际用途。它仅仅正确地识别了99.98%的非欺诈实例。因此，对于稀有事件建模（例如欺诈），需要应用其他方法。

+   **在非线性交互上使用简单线性模型。**

    当构建二元分类器时，选择逻辑回归作为首选方法，而实际上特征之间的关系并不是线性的，这种情况很常见。在这种情况下，基于树的模型或支持向量机表现更好。不知道哪些方法适用于哪些问题会导致模型和后续预测的质量差。

+   **忽略异常值。** 异常值通常需要特别关注，或者应完全忽略，有些建模方法对异常值非常敏感，忘记去除或处理这些异常值会导致模型表现差。

+   **进行正则化而没有标准化。**

    许多实践者没有意识到，在数据标准化之前对模型特征应用正则化的冗余。因为正则化会对尺度较小的特征施加更多惩罚。例如，如果有一个尺度在3,000到10,000之间的特征，另一个尺度在0到1之间，另一个在-9,999到9,999之间。

+   **未考虑实时评分环境。**

    实践者有时会被构建最完美模型的目标所分心，但在部署时，模型复杂到无法集成进操作系统。

+   **由于操作原因，使用未来将无法获得的特征。**

    可能会识别出一个非常具有预测性的特征（如性别），但由于法规，该字段不能用于建模，或者该字段的捕获已被暂停，未来可以用于模型。

+   **未考虑应用有效预测分析的现实世界影响及可能的后果。**

    四年前，美国零售商Target成为头条新闻，当时《纽约时报》记者查尔斯·杜希格（Charles Duhigg）揭示了[Target的分析模型预测了一名青少年的怀孕](https://www.example.org/2014/05/target-predict-teen-pregnancy-inside-story.html)，而她的父亲还不知道。正如一些人所指出的，虽然你可以做到，并不意味着你应该这样做。

多年来，我们通过预测分析共同学到了一些宝贵的经验。为什么不利用这些经验呢？如果你想在预测分析领域获得进一步的指导，[请联系我们！](http://www.principa.co.za/contact) 我们很高兴与你面谈，分享知识，并了解你的预测分析项目和计划。

[原文](http://insights.principa.co.za/the-top-predictive-analytics-pitfalls-to-avoid)。经许可转载。

**简介：** [罗宾·戴维斯](http://insights.principa.co.za/author/robin-davies)是[Principa](http://www.principa.co.za/)的决策分析主管。罗宾的团队在金融服务、营销和忠诚度领域，使用描述性、预测性和规范性分析技术取得了优异的成绩。

**相关：**

+   [可靠的数据科学：避免最具害的预测陷阱](/2017/01/siegel-data-science-avoiding-prediction-pitfall.html)

+   [通过AI进行物联网的持续改进/持续学习](/2016/11/continuous-improvement-iot-ai-learning.html)

+   [4个原因你的机器学习模型可能出错（以及如何修复它）](/2016/12/4-reasons-machine-learning-model-wrong.html)

+   [Target真的预测了青少年的怀孕吗？内幕故事](/2014/05/target-predict-teen-pregnancy-inside-story.html)

### 更多相关话题

+   [你为什么应该避免从事数据科学职业的5个主要原因](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)

+   [新手数据科学家应该避免的错误](https://www.kdnuggets.com/2022/06/mistakes-newbie-data-scientists-avoid.html)

+   [如何避免过拟合](https://www.kdnuggets.com/2022/08/avoid-overfitting.html)

+   [对话式AI开发中的3个关键挑战及如何避免](https://www.kdnuggets.com/3-crucial-challenges-in-conversational-ai-development-and-how-to-avoid-them)

+   [5个常见的Python陷阱（及如何避免）](https://www.kdnuggets.com/5-common-python-gotchas-and-how-to-avoid-them)

+   [避免这5个AI新手常犯的错误](https://www.kdnuggets.com/avoid-these-5-common-mistakes-every-novice-in-ai-makes)
