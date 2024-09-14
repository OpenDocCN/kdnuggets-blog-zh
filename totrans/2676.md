# 使用机器学习预测心脏病？不要！

> 原文：[https://www.kdnuggets.com/2020/11/predicting-heart-disease-machine-learning.html](https://www.kdnuggets.com/2020/11/predicting-heart-disease-machine-learning.html)

[评论](#comments)

**[Venkat Raman](https://www.linkedin.com/in/venkat-raman-analytics/)，True Influence 的数据科学家**

![图片](../Images/8e831252b1e1682d387cc0b9eb5f9de3.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 工作

* * *

最近，我被邀请担任数据科学竞赛的评委。学生们被提供了“心脏病预测”数据集，可能是 Kaggle 上可用数据集的改进版本。我之前见过这个数据集，并且经常看到各种自称数据科学大师的人教导天真的人如何通过机器学习预测心脏病。

我相信“使用机器学习预测心脏病”是一个经典的例子，展示了如何不将机器学习应用于一个问题，特别是在需要大量领域经验的情况下。

让我拆解一下在这个数据集上应用机器学习的各种问题。

**直接投入问题的综合症** – 这通常是许多人犯的第一个错误。直接投入到问题中，想着应用哪种机器学习算法。将 EDA 等作为这一过程的一部分，并不是在*思考*问题。相反，这表明你已经接受了问题需要数据科学解决方案的观点。相反，在开始任何分析之前需要提出的一个相关问题是，“这个问题通过应用机器学习真的可以预测吗？”。

**对数据的盲目信任** – 这是第一个要点的扩展。直接投入问题意味着你对数据有盲目的信任。人们假设数据是正确的，而不去仔细审查数据。例如，数据集只提供了收缩压。如果你和任何医生或甚至急救员谈话，他们会告诉你，单靠收缩压不能给出完整的情况。舒张压的报告也很重要。很多人甚至不会问“特征是否足够预测结果，还是需要更多特征”这个问题。

![图表展示了数据不足。](../Images/8186f7265cf7ccd784c5c1f9c5f8b088.png)

**每位患者的数据不足：**我们来看看上面的数据集。如果你注意到，每个特征下只有一个数据点。根本性的问题在于血压、胆固醇、心跳等特征不是静态的。**它们是有范围的**。一个人的血压每小时和每天都可能变化，心跳也是如此。因此，当涉及到预测问题时，无法确定135 mm hg的血压是否是导致心脏病的因素，还是140 mm hg，而数据集可能报告的是130 mm hg。理想情况下，每个特征应对每位患者进行多次测量。

现在让我们来探讨问题的核心。

**在没有领域经验的情况下应用算法** - 数据科学应用在医疗保健中失败率高的原因之一是，应用算法的数据科学家缺乏足够的医学知识。

其次，在医疗保健中，因果关系被非常重视。进行许多严格的临床和统计测试以推断因果关系。

在案例研究中，任何机器学习算法只是试图将输入映射到输出，同时减少一些误差度量。此外，机器学习算法本身不是分类器，我们通过设置一些切割点或阈值将其作为分类器。**再次强调，这些切割点不是为了推导因果关系**，而只是为了获得“有利的指标”。

这个问题被低代码库的使用所加剧。这个案例研究恰恰展示了低代码库可能危险的原因。低代码库适合十几种或更多的算法。大多数人甚至不知道这些算法中的一些是如何工作的！他们只是根据像F1、精准率、召回率和准确率这样的指标选择“最佳”算法。

专注于准确性指标的低代码库导致了‘古德哈特法则’——“当一个衡量标准成为目标时，它就不再是一个好的衡量标准。”

![图](../Images/6a7c0ccd1815d6a00ee546a23c4153b4.png)

图片来源：https://sketchplanations.com/goodharts-law

如果你在进行预测，你就暗示了因果关系。在医疗保健中，单纯的预测是不够的，需要证明因果关系。机器学习分类算法无法回答‘因果关系’的问题。

**认为他们解决了一个真正的医疗保健问题 –** 最后但同样重要的是，许多人认为，通过将机器学习算法应用于*医疗保健*数据集并获得一些准确性指标，他们已经解决了一个真正的医疗保健问题。尤其是在涉及医疗保健领域时，没有什么比这更远离真相的了。

**总结：**

也许有成千上万的商业问题真正需要数据科学/机器学习解决方案。但与此同时，人们不应陷入“对一个拿着锤子的人来说，所有的东西都像钉子”的陷阱。把一切都看作钉子（数据科学问题），把机器学习算法看作（锤子），可能会非常适得其反。数据科学在商业问题中的80%失败率很大程度上可以归因于此。

优秀的数据科学家就像优秀的医生。优秀的医生会在开重药或手术前首先建议保守的治疗。同样，优秀的数据科学家在盲目应用多个机器学习算法之前，应首先提出一些相关的问题。

医生：手术 :: 数据科学家 : 机器学习

欢迎您的评论和意见。

**个人简介：[Venkat Raman](https://www.linkedin.com/in/venkat-raman-analytics/)** 是一位具有商业头脑的数据科学家。他通过数据科学帮助企业蓬勃发展。他在创新和将数据科学技术应用于商业问题方面有着良好的业绩记录。他是一个永恒的知识追求者，并相信对任何任务都应尽最大努力。

[原文](https://www.linkedin.com/pulse/predicting-heart-disease-using-machine-learning-dont-venkat-raman/)。已获得许可转载。

**相关内容：**

+   [人工智能将如何变革医疗保健（它能解决美国医疗系统的问题吗？）](/2019/09/ai-transform-healthcare.html)

+   [用于精准医学和更好医疗保健的人工智能](/2020/09/artificial-intelligence-precision-medicine-better-healthcare.html)

+   [医疗保健中的人工智能：创新初创公司的回顾](/2020/09/ai-healthcare-review-innovative-startups.html)

### 相关主题

+   [使用回归模型预测加密货币价格](https://www.kdnuggets.com/2022/05/predicting-cryptocurrency-prices-regression-models.html)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [深度神经网络不会引导我们走向AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [如果没有合适的学位，如何进入数据分析领域](https://www.kdnuggets.com/2021/12/how-to-get-into-data-analytics.html)

+   [不要成为商品化的数据科学家](https://www.kdnuggets.com/2022/10/commoditized-data-scientist.html)

+   [不要错过！在2023年结束前注册免费课程](https://www.kdnuggets.com/dont-miss-out-enroll-in-free-courses-before-2023-ends)
