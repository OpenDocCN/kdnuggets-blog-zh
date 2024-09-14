# 每个数据科学家都应该了解的三种 R 库（即使你使用 Python）

> 原文：[https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

**作者 [Terence Shin](https://www.linkedin.com/in/terenceshin/)，数据科学家 | 硕士分析与 MBA 学生**

![每个数据科学家都应该了解的三种 R 库（即使你使用 Python）](../Images/8f8144be8580f75d2f8c1f8b92692607.png)

图片由 [Denis Pavlovic](https://unsplash.com/@itsdenispavlovic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/coder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

## 介绍

长时间以来，我对使用 R 持反对态度，仅仅因为它不是 Python。

但在过去几个月玩弄 R 后，我意识到 R 在多个使用案例中优于 Python，特别是在统计分析方面。此外，R 拥有一些由全球最大的科技公司开发的强大包，而这些**不在** Python 中！

因此，在这篇文章中，我想探讨三个我**强烈****推荐**你花时间学习并装备在你的工具库中的 R 包，因为它们确实是非常强大的工具。

不再赘述，这里是每个数据科学家都应该了解的三种 R 包，即使你只使用 Python：

1.  因果影响与谷歌

1.  Robyn 与 Facebook

1.  使用 Twitter 进行异常检测

## [1\. 因果影响（谷歌）](https://google.github.io/CausalImpact/CausalImpact.html)

假设你的公司为超级碗推出了一个新的电视广告，并想查看它对转化率的影响。因果影响分析尝试预测如果活动从未发生，会发生什么——这被称为反事实。

![每个数据科学家都应该了解的三种 R 库（即使你使用 Python）](../Images/37dd5158977818000228dc4aed4a1403.png)

图片由作者创建

举个实际的例子，因果影响分析尝试预测反事实，即顶部图表中的蓝色虚线，然后将实际值与反事实进行比较，以估算差异。

因果影响对市场营销活动、扩展到新区域、测试新产品功能等非常有用！

## [2\. Robyn (Facebook)](https://facebookexperimental.github.io/Robyn/)

营销组合建模是一种现代技术，用于估计多个营销渠道或活动对目标变量（如转化率或销售）的影响。

营销组合模型 (MMMs) 非常受欢迎，超过了归因模型，因为它们允许您衡量电视、广告牌和广播等无法直接量化的渠道的影响。

通常，营销组合模型从零开始构建需要几个月的时间。但 Facebook 创建了一个名为 Robyn 的新 R 包，可以在几分钟内创建一个强大的 MMM。

![每个数据科学家都应该知道的三个 R 库（即使您使用 Python）](../Images/1dc83c3c9c33cc6eb0924695dbf9b900.png)

作者创作的图像

使用 Robyn，您不仅可以评估每个营销渠道的效果，还可以优化您的营销预算！

> **请确保**[**在这里订阅**](https://terenceshin.medium.com/membership)**，以及订阅我的**[**个人通讯**](https://terenceshin.substack.com/embed)**，以确保不会错过关于数据科学指南、技巧和窍门、生活经验等更多文章！**

## [3\. 异常检测 (Twitter)](https://github.com/twitter/AnomalyDetection)

异常检测，也称为离群点分析，是一种识别与其他数据点显著不同的数据点的方法。

一般异常检测的一个子集是**时间序列数据中的异常检测**，这是一种独特的问题，因为您还需要考虑数据的趋势和季节性。

![每个数据科学家都应该知道的三个 R 库（即使您使用 Python）](../Images/47a047199aeadec7f9488bbd488869b4.png)

作者创作的图像

Twitter 通过创建一个异常检测包来解决这个问题，包内的算法能够自动完成所有工作。它是一种复杂的算法，能够识别全球和局部的异常情况。除了时间序列外，它还可以用于检测值向量中的异常。

**感谢阅读！**

> 如果您喜欢这篇文章，请务必[在这里订阅](https://terenceshin.medium.com/membership)以及订阅我的[独家通讯](https://terenceshin.substack.com/embed)，以确保不会错过关于数据科学指南、技巧和窍门、生活经验等更多文章！

不确定接下来读什么？我为您挑选了另一篇文章：[**2021 年十大最佳数据可视化**](https://towardsdatascience.com/the-10-best-data-visualizations-of-2021-fec4c5cf6cdb)

以及另一篇：[**2022 年你应该知道的所有机器学习算法**](https://towardsdatascience.com/all-machine-learning-algorithms-you-should-know-in-2022-db5b4ccdf32f)

**Terence Shin**

+   ***如果您喜欢这篇文章，***[***请订阅我的 Medium***](https://terenceshin.medium.com/membership)***以获取独家内容！***

+   ***同样，您还可以***[***在 Medium 上关注我***](https://medium.com/@terenceshin)

+   [***注册我的个人通讯***](https://terenceshin.substack.com/embed)

+   ***关注我在***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)***上的其他内容***

**个人简介: [Terence Shin](https://www.linkedin.com/in/terenceshin/)** 是一位数据爱好者，拥有3年以上的SQL经验和2年以上的Python经验，同时也是《Towards Data Science》和《KDnuggets》的博客作者。

[原文](https://towardsdatascience.com/three-r-libraries-every-data-scientist-should-know-even-if-you-use-python-7e9d95e4a415)。经授权转载。

### 更多相关话题

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并寻找目标以...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个90亿美元的AI失败，分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
