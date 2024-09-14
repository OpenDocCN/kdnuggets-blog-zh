# 数据科学家应该使用 LightGBM 的 3 个理由

> 原文：[`www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html`](https://www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html)

## **介绍**

有许多优秀的 Python 提升库供数据科学家利用。其中包括 XGBoost 和新推出的 CatBoost 算法。然而，有一种算法结合了这些算法的某些特征，使其成为数据科学家的必备工具。虽然这些好处在学习和教育方面很棒，但更重要的是，它在需要快速的专业环境中表现尤为出色。接下来，我将讨论 [LightGBM](https://lightgbm.readthedocs.io/en/latest/FAQ.html) [1] 的好处以及它们如何与您的数据科学工作密切相关。

## **分类编码**

![3 Reasons to Use LightGBM](img/6d31ac99849ebac049210036fe773f8b.png)

图片由 [米哈伊尔·瓦西里耶夫](https://unsplash.com/@miklevasilyev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [2]

这个库的最佳特性之一是对分类特征的支持。虽然很多数据科学家可能使用独热编码为一个分类特征创建大量新列，但这个库允许你使用 categorical_feature 参数来指定分类特征。

虽然独热编码很有用，但在学术界，例如在 Jupyter Notebook 中，它在专业环境中可能不那么有用。假设你有 10 个分类特征，每个特征有 100 个独特的 bin，这将扩展到 1,000 个新列。这不仅使你的数据框变得稀疏，而且还使你的模型变得非常缓慢。这种稀疏性的另一个令人焦虑的结果是，当你需要将特征转换为生产代码供软件工程师在你的预测服务和部署中使用时（*如果你有这种设置的话*），这对双方来说可能会令人困惑和难以处理。

**以下是使用 LightGBM 进行分类编码的一些好处：**

+   更容易对分类特征进行编码

+   更容易使用

+   更容易与其他数据科学家、软件工程师、后台工程师和产品经理合作

+   可以保留原始列名

+   可以利用分类特征的好处，而不是使用独热编码进行传统的数值转换

+   这些好处可以*最终*使你的模型更快、更准确

## **快速**

![3 Reasons to Use LightGBM](img/8b675b876edb6aa5a92d298261365cc2.png)

图片由 [安迪·比尔斯](https://unsplash.com/@andybeales?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/sprint?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [3]。

不仅仅是对类别特征进行编码使你的模型更快，LightGBM 还具备一些其他技巧来提高训练和预测速度。LightGBM 使用了 GOSS 和 EFB，或称为基于梯度的单侧采样（Gradient-based One-Side Sampling）和排他特征绑定（Exclusive Feature Binding），以及基于直方图的分裂方法。

**以下是为什么快速的 LightGBM 模型对专业人士有用的原因：**

+   并不是每个工作都允许你花费几周或几个月来制定模型，有些甚至可能希望在同一周内得到一个——或者至少是一个概念验证模型

+   这种更快的建模可以让你更快地测试特征和参数，最终使你在更快的环境中工作得更好

+   可以测试更多特征，而不会像其他算法那样显著减慢模型速度

它简单、快速，当有很多人依赖你的模型时，速度快可以让你更高效地帮助业务。

## **准确**

![使用 LightGBM 的 3 个理由](img/bf775900501c92daa0f0ccaded4b89a4.png)

图片由 [Silvan Arnet](https://unsplash.com/@silvanarnet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/s/photos/bullseye?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [4]。

XGBoost、CatBoost 和 LightGBM 都是准确的模型。是的，最终这取决于你的问题、特征和数据，但总体而言，这些算法在你执行了必要步骤后会产生准确的结果。

因为你可以使用类别特征，所以你更有可能得到一个准确的模型，比起只能执行独热编码的算法要更好。LightGBM 的分裂方式也可以导致更准确的模型。不过，重要的是要注意你需要防止过拟合。

**以下是 LightGBM 更准确的一些原因，以及它如何在职业上帮助你的：**

+   分裂方法

+   类别特征支持

+   当然，每个人都希望有一个更准确的模型，特别是在业务中（*只需要确保你没有过拟合*）

## **总结**

虽然这些好处很简单，但它们非常重要，并且使你的工作变得更加轻松。因此，你的公司——包括利益相关者和工程师，会对你使用 LightGBM 感到满意。

总结一下，这里是一些在职业上使用 LightGBM 的主要好处：

+   类别编码

+   快速

+   准确

我希望你觉得我的文章既有趣又有用。如果你同意或不同意这些好处，请随时在下面评论。为什么或为什么不？你认为 LightGBM 还有哪些其他重要的好处需要指出？这些内容当然可以进一步澄清，但我希望我能够对 LightGBM 提供一些启示。

请随时**[查看我的 Medium 个人资料](https://datascience2.medium.com/subscribe)**。

## **参考文献**

[1] 微软公司，[LightGBM 文档](https://lightgbm.readthedocs.io/en/latest/FAQ.html)，(2022)

[2] 图片由 [Mikhail Vasilyev](https://unsplash.com/@miklevasilyev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，（2017 年）

[3] 图片由 [Andy Beales](https://unsplash.com/@andybeales?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/sprint?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，（2015 年）

[4] 图片由 [Silvan Arnet](https://unsplash.com/@silvanarnet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/bullseye?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，（2020 年）

**[Matthew Przybyla](https://www.linkedin.com/in/matthew-przybyla-0a095b31/)** ([Medium](https://datascience2.medium.com/)) 是位于德克萨斯州 Favor Delivery 的高级数据科学家。他拥有南美 Methodist University 的数据科学硕士学位。他喜欢撰写关于数据科学领域的趋势话题和教程，从新算法到数据科学家日常工作经验的建议。Matt 喜欢突出数据科学的商业方面，而不仅仅是技术方面。欢迎通过 [LinkedIn](https://www.linkedin.com/in/matthew-przybyla-0a095b31) 联系 Matt。

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关主题

+   [3 个你应该使用线性回归模型而不是…的理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [避免数据科学职业的 5 个理由](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)

+   [你应该获得认证的 5 个理由](https://www.kdnuggets.com/2023/05/sas-5-reasons-get-certified.html)

+   [4 个你不应该使用机器学习的理由](https://www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html)

+   [5 个你需要合成数据的理由](https://www.kdnuggets.com/2023/02/5-reasons-need-synthetic-data.html)

+   [7 个你不应该成为数据科学家的理由](https://www.kdnuggets.com/7-reasons-why-you-shouldnt-become-a-data-scientist)
