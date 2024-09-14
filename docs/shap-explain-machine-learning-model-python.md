# SHAP: 用 Python 解释任何机器学习模型

> 原文：[https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

# 动机

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

假设你正在训练一个机器学习模型，预测某个人是否会点击广告。在接收到有关某个人的一些信息后，模型预测该人不会点击广告。

![SHAP: 用 Python 解释任何机器学习模型](../Images/3b30d440cb417081f0bb238bf94a000c.png)

图片由作者提供

但为什么模型会有这样的预测？每个特征对预测的贡献有多大？如果能看到一个图示，展示每个特征对预测的贡献情况，会不会很不错呢？

![SHAP: 用 Python 解释任何机器学习模型](../Images/4b1a6818bd977e96f5dc0c6b454656c1.png)

图片由作者提供

这时 Shapley 值就派上用场了。

# 什么是 Shapley 值？

Shapley 值是一种游戏理论方法，涉及在联盟中公平地分配收益和成本。

由于每个参与者对联盟的贡献不同，Shapley 值确保每个参与者**根据他们的贡献获得公平的份额**。

![SHAP: 用 Python 解释任何机器学习模型](../Images/3e9bf83ef146048b60cbf1886bafde1e.png)

图片由作者提供

## 一个简单的例子

Shapley 值用于解决各种问题，涉及对每个工作者/特征在一个小组中的贡献进行质疑。为了理解 Shapley 值如何工作，假设你的公司刚刚进行了 A/B 测试，测试了不同的广告策略组合。

每种策略在特定月份的收入如下：

+   无广告：$150

+   社交媒体：$300

+   谷歌广告：$200

+   邮件营销：$350

+   社交媒体和谷歌广告 $320

+   社交媒体和邮件营销：$400

+   谷歌广告和邮件营销：$350

+   邮件营销、谷歌广告和社交媒体：$450

![SHAP: 用 Python 解释任何机器学习模型](../Images/449d824635515d27a596cf0b37eb620e.png)

图片由作者提供

使用三种广告与不使用广告之间的收入差为 $300。**每种广告对这一差异的贡献有多少？**

![SHAP: 用 Python 解释任何机器学习模型](../Images/6b4b9968cef63b06a607e3a4c92eacd7.png)

图片由作者提供

我们可以通过计算每种广告类型的 Shapley 值来找出结果。[这篇文章](https://towardsdatascience.com/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30)提供了一种出色的计算 Shap 值的方法。我将在这里总结。

我们首先计算 Google 广告对公司收入的总贡献。Google 广告的总贡献可以通过以下公式计算：

![SHAP: 用 Python 解释任何机器学习模型](../Images/cc602f3fd670a9e28005d002e8543cc8.png)

图片由作者提供

让我们找出 Google 广告的边际贡献及其权重。

## 查找 Google 广告的边际贡献

首先，我们将找到 Google 广告对以下组合的边际贡献：

+   无广告

+   Google 广告 + 社交媒体

+   Google 广告 + 邮件营销

+   Google 广告 + 邮件营销 + 社交媒体

![SHAP: 用 Python 解释任何机器学习模型](../Images/400acda745fee657d1d12d91e0a7223d.png)

图片由作者提供

Google 广告对无广告的边际贡献是：

![SHAP: 用 Python 解释任何机器学习模型](../Images/4cdc3cdd36e65e573ef6c2aa9d40cafd.png)

图片由作者提供

Google 广告对 Google 广告和社交媒体组合的边际贡献是：

![SHAP: 用 Python 解释任何机器学习模型](../Images/a7a5a6a05f437b8228620bf44b6cf4ce.png)

图片由作者提供

Google 广告对 Google 广告和邮件营销组合的边际贡献是：

![SHAP: 用 Python 解释任何机器学习模型](../Images/00d32e8e7b834e04bc0d42b778eb1208.png)

图片由作者提供

Google 广告对 Google 广告、邮件营销和社交媒体组合的边际贡献是：

![SHAP: 用 Python 解释任何机器学习模型](../Images/09edb469c85e8a6d4c8cdf7b13b096bf.png)

图片由作者提供

## 查找权重

为了找到权重，我们将不同广告策略的组合组织成如下多个层级。每个层级对应每个组合中的广告策略数量。

然后我们将根据每个层级中的边数分配权重。我们看到：

+   第一层包含**3条边**，所以每条边的权重将是**1/3**

+   第二层包含**6条边**，所以每条边的权重将是**1/6**

+   第三层包含**3条边**，所以每条边的权重将是**1/3**

![SHAP: 用 Python 解释任何机器学习模型](../Images/9c86a68002fb59152d2014f2a6979ef3.png)

图片由作者提供

## 查找 Google 广告的总贡献

现在我们准备好根据之前找到的权重和边际贡献来找出 Google 广告的总贡献！

![SHAP: 用 Python 解释任何机器学习模型](../Images/a5a128331e8948d95ede48cda6815deb.png)

图片由作者提供

![SHAP: 解释任何机器学习模型的 Python 库](../Images/c865e74919223dd7a960cd4fbd8d8d87.png)

图片由作者提供

太棒了！所以 Google 广告对使用三种广告策略和不使用广告之间的总收入差异贡献了 $36.67。36.67 是 Google 广告的 Shapley 值。

![SHAP: 解释任何机器学习模型的 Python 库](../Images/4cde13a07089c1fa9093fec319eda42e.png)

图片由作者提供

对另外两种广告策略重复上述步骤，我们可以看到：

+   电子邮件营销贡献了 $151.67

+   社交媒体贡献了 $111.67

+   Google 广告贡献了 $36.67

![SHAP: 解释任何机器学习模型的 Python 库](../Images/27526b46d60f4e9395c71581c8cceb97.png)

图片由作者提供

它们总共贡献了 $300，来说明使用三种不同广告类型和不使用广告之间的差异！很酷，对吧？

现在我们了解了 Shapley 值，让我们看看如何使用它来解释机器学习模型。

# SHAP — 在 Python 中解释任何机器学习模型

[SHAP](https://github.com/slundberg/shap) 是一个使用 Shapley 值来解释任何机器学习模型输出的 Python 库。

要安装 SHAP，请输入：

```py
pip install shap
```

## 训练模型

为了理解 SHAP 的工作原理，我们将使用一个 [广告数据集](https://drive.google.com/file/d/1oMUOmXf67DxPe5CV6YJTiEm9TeozKK5H/view?usp=sharing) 进行实验：

我们将构建一个机器学习模型来预测用户是否点击了广告，基于关于该用户的一些信息。

我们将使用 [Patsy](https://towardsdatascience.com/patsy-build-powerful-features-with-arbitrary-python-code-bb4bb98db67a#3be4-4bcff97738cd) 将 DataFrame 转换为特征数组和目标值数组：

将数据分成训练集和测试集：

接下来，我们将使用 XGBoost 构建一个模型并进行预测：

为了查看模型的表现，我们将使用 F1 分数：

```py
0.9619047619047619
```

非常好！

## 解释模型

模型在预测用户是否点击了广告方面表现良好。但它是如何得出这些预测的？ **每个特征对最终预测和平均预测之间的差异贡献了多少？**

请注意，这个问题与我们在文章开头讨论的问题非常相似。

这就是为什么找出每个特征的 Shapley 值可以帮助我们确定它们的贡献。获得特征 i 重要性的步骤，与之前类似，其中 i 是特征的索引。

+   获取所有不包含特征 i 的子集

+   找出特征 i 对每个子集的边际贡献

+   聚合所有边际贡献以计算特征 i 的贡献

要使用 SHAP 找到 Shapley 值，只需将训练好的模型插入到 `shap.Explainer` 中：

# SHAP 瀑布图

可视化第一次预测的解释：

![SHAP: 解释任何机器学习模型的 Python 库](../Images/bd7178c7376b4a180d961773567aceef.png)

图片由作者提供

哦！现在我们知道了每个特征对第一次预测的贡献。以上图表的解释如下：

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/238b3e7f3c8a917a6b2a6ba6ef6b4e0b.png)

图片由作者提供

+   蓝色条形图显示了某个特征降低预测值的程度。

+   红色条形图显示了某一特征对预测值的影响程度。

+   负值表示点击广告的概率小于 0.5

对于这些子集，SHAP 不会移除某个特征再重新训练模型，而是将该特征替换为该特征的平均值，然后生成预测。

我们应当期望总贡献等于预测与均值预测之间的差值。让我们来检查一下：

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/643dc5338b0e7fe8c2ea0652ea9c61bb.png)

图片由作者提供

太棒了！它们是相等的。

可视化第二次预测的解释：

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/1aa86245f98e416efa7c029b8b04611b.png)

图片由作者提供

# SHAP 总结图

我们可以使用 SHAP 总结图可视化这些特征在多个实例中的整体影响，而不是查看每个个体实例。

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/22ca7fd9b6cebe3fc38f23a61b9d117a.png)

图片由作者提供

SHAP 总结图告诉我们数据集中最重要的特征及其影响范围。

从上图中，我们可以获得一些关于模型预测的有趣见解：

+   用户的每日互联网使用对是否点击广告有最强的影响。

+   随着**每日互联网使用**的**增加，**用户**不太可能**点击广告**。

+   随着**每日网站使用时间的增加，**用户**不太可能**点击广告**。**

+   随着**区域收入的增加，**用户**不太可能**点击广告**。**

+   随着年龄**增加，**用户**更可能**点击广告**。**

+   如果用户**是男性**，那么该用户**不太可能**点击广告**。**

# SHAP 条形图

我们还可以使用 SHAP 条形图获取全局特征重要性图。

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/a9d0566b64a4029dcf31f0ea63934b37.png)

图片由作者提供

# SHAP 依赖散点图

我们可以使用 SHAP 依赖散点图观察单个特征对所有模型预测的影响。

## 每日互联网使用时间

每日互联网使用特征的散点图：

![SHAP: 解释任何机器学习模型的 Python 实现](../Images/fbc15be0826789197d67b3d116e2c1d1.png)

图片由作者提供

从上图中，我们可以看到，随着每日互联网使用时间的增加，SHAP 值下降。这验证了我们在之前图中看到的情况。

我们还可以通过在同一图中添加 `color=shap_values` 来观察每日互联网使用特征与其他特征之间的交互。

散点图将尝试挑选出与每日互联网使用最强交互的特征列，即每日网站使用时间。

![SHAP: 用 Python 解释任何机器学习模型](../Images/520b6d842131e2570085e3a9298cc8b6.png)

作者提供的图片

酷！从上面的图表中，我们可以看到，使用互联网每天 150 分钟且每天在网站上花费时间较少的人更有可能点击广告。

让我们看看其他特征的散点图：

## 每日网站使用时间

![SHAP: 用 Python 解释任何机器学习模型](../Images/b57a8a8fb5e97ab2050fcd8b726c0c3f.png)

作者提供的图片

## 区域收入

![SHAP: 用 Python 解释任何机器学习模型](../Images/7b03af5d34caf7a20f509044dd2c22ee.png)

作者提供的图片

## 年龄

![SHAP: 用 Python 解释任何机器学习模型](../Images/e15a46c55b39439f040ff894113590c0.png)

作者提供的图片

## 性别

![SHAP: 用 Python 解释任何机器学习模型](../Images/332c51197f0fe98e3c9c3224de04cb12.png)

作者提供的图片

# SHAP 交互图

你还可以通过 SHAP 交互值汇总图观察**特征之间的交互矩阵**。在这个图中，主要效应位于对角线上，而交互效应则位于对角线之外。

![SHAP: 用 Python 解释任何机器学习模型](../Images/eb70796205fff65c237132a5dfb3c8e3.png)

作者提供的图片

非常酷！

# 结论

恭喜！你刚刚学习了 Shapey 值以及如何使用它来解释机器学习模型。希望这篇文章能为你提供必要的知识，以便用 Python 解释你自己的机器学习模型。

我建议查看[SHAP 的文档](https://shap.readthedocs.io/en/latest/overviews.html)，以了解 SHAP 的其他应用。

随意查看[这个交互式笔记本](https://deepnote.com/project/Data-science-hxlyJpi-QrKFJziQgoMSmQ/%2FData-science%2Fdata_science_tools%2Fshapey_values%2Fshapey_values.ipynb)中的源代码或克隆我的[代码库](https://github.com/khuyentran1401/Data-science/blob/master/data_science_tools/shapey_values/shapey_values.ipynb)。

## 参考

Mazzanti, S. (2021年4月21日)。*SHAP 以我希望有人向我解释的方式进行了说明*。Medium。于2021年9月23日获取，来源：[https://towardsdatascience.com/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30.](https://towardsdatascience.com/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30.)

**[Khuyen Tran](https://www.linkedin.com/in/khuyen-tran-1401/)** 是一位高产的数据科学作家，撰写了[一系列令人印象深刻的有用数据科学主题以及代码和文章](https://github.com/khuyentran1401/Data-science)。Khuyen 目前正在寻找 Bay Area 的机器学习工程师角色、数据科学家角色或开发者推广者角色，预计从 2022 年 5 月开始，如果你在寻找具备她技能的人才，请联系她。

[原文](https://towardsdatascience.com/shap-explain-any-machine-learning-model-in-python-24207127cad7)。经许可转载。

### 更多相关主题

+   [NExT-GPT介绍：全方位多模态大型语言模型](https://www.kdnuggets.com/introduction-to-nextgpt-anytoany-multimodal-large-language-model)

+   [使用SHAP值进行机器学习模型解释](https://www.kdnuggets.com/2023/08/shap-values-model-interpretability-machine-learning.html)

+   [使用LIME解释NLP模型](https://www.kdnuggets.com/2022/01/explain-nlp-models-lime.html)

+   [如何在没有工作经验的情况下找到你的第一份数据科学工作](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)

+   [在你参加任何免费的数据科学课程之前，请阅读此文](https://www.kdnuggets.com/read-this-before-you-take-any-free-data-science-course)

+   [Segment Anything模型：图像分割的基础模型](https://www.kdnuggets.com/2023/07/segment-anything-model-foundation-model-image-segmentation.html)
