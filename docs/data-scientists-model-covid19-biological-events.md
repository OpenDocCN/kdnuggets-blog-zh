# 数据科学家是否应该对 COVID19 和其他生物事件进行建模

> 原文：[https://www.kdnuggets.com/2020/04/data-scientists-model-covid19-biological-events.html](https://www.kdnuggets.com/2020/04/data-scientists-model-covid19-biological-events.html)

[评论](#comments)![新冠数据科学](../Images/9b312a2dbb976fd452818e5bf037641a.png)

数据科学家的角色多年来一直在扩展。从用统计数据处理数字，到构建可扩展的数据库，再到构建生产级的机器学习或深度学习模型。生物统计学和流行病学是统计学的高度专业化领域，大学为其提供不同的学位课程。

生物统计学家使用的统计技术可能是你目前的日常数据科学家从未听说过的。这是一个很好的例子，说明缺乏领域知识会暴露你作为一个不知所措、只是跟随趋势的人。虽然社区中已知建立预测模型以查看谁更可能在泰坦尼克号上幸存或对鸢尾花进行分类作为数据科学之旅的一部分，但对于如全球大流行这样正在杀害数十万人甚至数百万人的更严重问题，或许应该给予更多的谨慎。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

### 流行病学家和生物统计学家

![流行病学-生物统计学家](../Images/7b55dc3e3f0a8d71c109d8dc1633d3b1.png)[来源](https://publichealth.gsu.edu/academics-student-life/graduate-divisions/graduate-studies-epidemiology-biostatistics/)

流行病学是研究疾病在特定人群和环境中的频率和分布的学科。流行病学是公共健康的重要方面，因为它涉及对人群中疾病的理解和风险评估。流行病学家通常具有生物学、医学和病毒学等领域的科学背景。这是流行病学家建立领域知识的方式，从而能够理解他们所建模的内容。

生物统计学是将统计技术应用于健康相关领域的科学研究，包括医学、流行病学和公共卫生。拥有统计学学位的人可能会成为零售、人口统计、房地产、经济学、金融等领域的数据分析师。生物科学则是一个完全不同的领域，需要单独的资格认证。

现在，数据科学家可能来自非统计/数学背景，突然开始建模疾病数据以展示他们的技能。这并不是展示你知识的正确数据类型。每个人都需要知道自己是否有能力正确处理这些数据。已经发布了大量虚假和误导性内容，这进一步玷污了数据科学家的职业，因为这表明仍然有人对数据一无所知，只关心使用 Python 中的随机森林或 xgboost 模型，而不是 R（因为 R 显然已经不再像以前那样酷）并在 LinkedIn 上推广，希望能让招聘人员或高级数据科学家留下深刻印象。

[**COVID-19 预测、达克效应和数据科学家的希波克拉底誓言**](https://blog.datasciencedojo.com/covid19-dunning-kruger-effect-hippocratic-oath-of-a-data-scientist/?utm_content=124596689&utm_medium=social&utm_source=linkedin&hss_channel=lcp-3740012)由 Raj Iqbal 完美总结了这一点。达克效应简单来说就是当一个人高估自己的能力时，实际上他们完成任务的能力非常低。

### 预测 COVID-19

以下内容摘自[这里](https://robjhyndman.com/hyndsight/forecasting-covid19/)。

预测和时间序列专家 Rob J Hyndman 表示，为了使预测相对准确，有三个主要因素：

1.  我们对影响其因素的理解程度；

1.  可用数据的多少；

1.  预测是否会影响我们试图预测的内容。

例如，明天的股票价格预测准确性较低，因为上述因素 1 和 3 并未得到满足。首先，影响股票价格变化的因素并没有得到特别好的理解，并且至少部分依赖于人类心理学。其次，广泛宣传的股票市场预测可以直接影响许多投资者的行为。

上述三个因素并非都适用于疾病，但我们可以看到第二点因实际病例数量被低估而成为问题。第二个问题是，COVID-19 的预测可能会影响我们试图预测的内容，因为各国政府正在做出反应，有些比其他国家更好。除非能够考虑到减缓传播的各种措施，否则使用现有数据的简单模型将会产生误导。

他和其他科学家使用分 compartments 流行病学模型来模拟感染过程。最简单的模型是基于将人口中的活跃个体分类为易感、传染或康复——因此它们被称为SIR模型。

### 结论

尽管数据科学家的任务是分析数据以提供见解，但我们有责任认识到我们的才能应如何使用，以及何时退一步让真正的专家带领前进。传染病建模是一个过于专业和敏感的领域，不应盲目发表意见。我们需要意识到我们何时被需要，何时不被需要。

**相关内容：**

+   [用AI对抗冠状病毒：利用深度学习和计算机视觉改进检测](/2020/04/fighting-coronavirus-ai-improving-testing-deep-learning-computer-vision.html)

+   [数据科学家应避免的5个统计陷阱](/2019/10/statistical-traps-data-scientists-avoid.html)

+   [数据科学家应对COVID-19的5种方式及5个应避免的行动](/2020/04/5-ways-data-scientists-can-help-covid-19.html)

### 更多相关主题

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并找到目标去……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [一个90亿美元的AI失败案例，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么使Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
