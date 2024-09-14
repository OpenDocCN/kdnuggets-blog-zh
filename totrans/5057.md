# 有用的数据科学：特征哈希

> 原文：[https://www.kdnuggets.com/2016/01/useful-data-science-feature-hashing.html](https://www.kdnuggets.com/2016/01/useful-data-science-feature-hashing.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**威尔·麦金尼斯**。

在之前关于 [分类编码](http://www.willmcginnis.com/2015/11/29/beyond-one-hot-an-exploration-of-categorical-variables/) 的文章中，我们探讨了将分类变量转换为数值特征的不同方法。 在这篇文章中，我们将探讨另一种方法：特征哈希。

特征哈希，或称哈希技巧，是将任意特征转化为稀疏二进制向量的方法。 它可以非常高效，因为只需一个独立的哈希函数，而无需预先构建可能类别的字典。

一个简单的实现方法是让用户选择所需的输出维度，只需将输入值哈希为一个数字，然后将其除以所需的输出维度并取余数R。 这样，你可以将特征编码为一个零向量，其中在索引R的位置为一。

在伪代码中，来自维基百科的文章：

`function hashing_vectorizer(features : array of string, N : integer): x := new vector[N] for f in features: h := hash(f) x[h mod N] += 1 return x`

或在Python中，从 [分类编码](https://github.com/wdm0006/categorical_encoding):

`def hash_fn(x): tmp = [0for_inrange(N)] for val in x.values: tmp[hash(val)% N] += 1 return pd.Series(tmp, index=cols)`

`cols = ['col_%d'% d for d in range(N)] X = X.apply(hash_fn, axis=1)`

这效果如何呢？好吧，上次我们用相同的模型运行了所有编码器并比较了得分。 一些编码每次略有不同（取决于初始序数变换如何进行），所以我们运行模型100次，并生成其得分的箱线图。 使用蘑菇数据集：

[![分类编码得分](../Images/a9a36cbf5da8885b83438dc34c5d2a8f.png)](http://i2.wp.com/www.willmcginnis.com/wp-content/uploads/2016/01/mushroom_boxplot.png)

在这个图中，你可以看到一些有趣的东西。 首先，虽然在上篇文章中介绍的二进制编码在平均水平上表现良好，但运行到运行的方差相当高，这并不理想。 此外，你可以看到随着用于哈希编码器的组件数量增加，性能逐渐上升，但在16时饱和，因为这个数据集的类别数量不足以需要超过16个组件。

在下一篇文章中，我将介绍这些兼容scikit-learn的转换器，提供一些使用示例，并分享一些关于如何在高维分类数据上构建优质模型的经验法则，以避免内存溢出。

查看更新的存储库，尝试你自己的编码器，使用我的编码器，或在你自己的数据上尝试：

[https://github.com/wdm0006/categorical_encoding](https://github.com/wdm0006/categorical_encoding)

[原文](http://www.willmcginnis.com/2016/01/16/even-further-beyond-one-hot-hashing/).

**相关：**

+   [数据科学职位市场 – 现状如何](/2015/10/data-science-job-market.html "数据科学职位市场 – 现状如何")

+   [Michael Li，Data Incubator谈数据驱动的招聘策略](/2015/04/interview-michael-li-data-incubator-hiring.html)

+   [数据科学的艺术：你需要的技能及如何获得它们](/2015/12/art-data-science-skills.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关内容

+   [Feature Store Summit 2022：一场关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [KDnuggets 新闻，12月7日：揭示数据科学的十大误区 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [4 个有用的中级 SQL 查询用于数据科学](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [5 个真正有用的 Bash 脚本用于数据科学](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [Kaggle 竞赛对现实世界问题有用吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)
