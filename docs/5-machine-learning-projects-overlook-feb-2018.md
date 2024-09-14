# 5 个你不应忽视的机器学习项目

> 原文：[https://www.kdnuggets.com/2018/02/5-machine-learning-projects-overlook-feb-2018.html](https://www.kdnuggets.com/2018/02/5-machine-learning-projects-overlook-feb-2018.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

在暂别之后，本月的“[忽视...](https://www.kdnuggets.com/tag/overlook)”帖子将卷土重来，继续温和地引入一些强大的鲜为人知的 [机器学习项目](https://www.kdnuggets.com/2020/03/20-machine-learning-datasets-project-ideas.html)。

查看下面的 5 个项目，获得一些潜在的新机器学习点子。

**1\. [skift: scikit-learn 的 Python fastText 封装器](https://github.com/shaypal5/skift)**

什么是 skift？

> skift 包含几个与 fastText Python 包兼容的 scikit-learn 封装器，以适应这些用例。

什么是 [fastText](https://github.com/facebookresearch/fastText)？

> fastText 是一个高效学习词表示和句子分类的库。
> 
> fastText 仅适用于文本数据，这意味着它将只使用数据集中的一列，而这个数据集可能包含许多不同类型的特征列。因此，一个常见的用例是让 fastText 分类器使用单列作为输入，忽略其他列。当 fastText 被用作堆叠分类器中的多个分类器之一时，其他分类器使用非文本特征时，这一点尤其如此。

理解 fastText 是拼图中的重要一环，但一旦掌握了这一理解，skift 可以帮助你轻松实现 fastText，并将其与其他 Scikit-learn 功能集成。

```py
>>> from skift import FirstColFtClassifier
>>> df = pandas.DataFrame([['woof', 0], ['meow', 1]], columns=['txt', 'lbl'])
>>> sk_clf = FirstColFtClassifier(lr=0.3, epoch=10)
>>> sk_clf.fit(df[['txt']], df['lbl'])
>>> sk_clf.predict([['woof']])
[0]
```

![PHP-ML](../Images/f7fb43d3e37db911485b376f2c04c89f.png)

**2\. [PHP-ML: PHP 的机器学习库](https://github.com/php-ai/php-ml)**

对于 PHP 没有体面的机器学习替代方案感到厌倦了吗？你是一个受虐狂（如果你在用 PHP，这个问题答案已经显而易见）？那么，这个项目可能正适合你！

> PHP 中机器学习的新方法。一库包含算法、交叉验证、神经网络、预处理、特征提取等功能。

虽然我是在开玩笑，但我已经远离 PHP 世界，不知道这是否能满足某些特定的紧迫需求；5K+ 的星标表明它可能确实有这个作用！除此之外，我总是对不同编程语言环境中的机器学习生态系统如何展开感兴趣。也许你也一样，或者更重要的是，你可能真的需要一个初看起来对 PHP 用户来说是一个坚实的库。

![Keras](../Images/2c0ef14f22fd4f68dc8fec252e13820a.png)

**3\. [Keras Scikit-Learn API 封装器](https://keras.io/scikit-learn-api/)**

虽然这从技术上讲不是一个独立项目，但我认为它足够重要，值得在这里突出显示。

> 你可以通过在 keras.wrappers.scikit_learn.py 找到的包装器，将 Sequential Keras 模型（仅单输入）作为 Scikit-Learn 工作流的一部分使用。

类似于了解 skift（如上文）的底层项目最重要，理解使用 Keras 实现神经网络的部分是这个难题的关键，Keras 本身是一个高级 API。能够将 Keras 与额外的 Scikit-learn 功能集成，并使用熟悉的 API 和方法，就是这些包装器所实现的功能。可以在官方 [Keras Github repository](https://github.com/keras-team/keras) 上找到 API。

如果你已经在使用 Keras，这对你来说可能并不陌生。如果你还没有，知道这个集成是可能的可能足以让你看看。

![CatBoost](../Images/d8d86bb0d0785a1faef7410b7a8fd49a.png)

**4\. [CatBoost: 基于决策树的梯度提升机器学习方法](https://github.com/catboost/catboost)**

梯度提升依然非常流行。或者至少某种程度上流行。最近进入梯度提升树领域的是 CatBoost。

> CatBoost 的主要优势：
> 
> +   相较于其他 GBDT 库，质量更优。
> +   
> +   同类最佳的推断速度。
> +   
> +   支持数值和分类特征。
> +   
> +   对训练的快速 GPU 和多 GPU（在一个节点上）支持。
> +   
> +   包含数据可视化工具。

CatBoost 提供 Python、R 和命令行界面版本。查看 [这里的教程](https://github.com/catboost/catboost/tree/master/catboost/tutorials)，以及完整的 [文档](https://tech.yandex.com/catboost/doc/dg/concepts/about-docpage/)。

![PyMC3](../Images/284f5cf62db5858af8a93bafc5868596.png)

**5\. [PyMC3: Python 中的概率编程](https://github.com/pymc-devs/pymc3)**

> PyMC3 是一个用于贝叶斯统计建模和概率机器学习的 Python 包，专注于高级 Markov 链蒙特卡罗和变分拟合算法。它的灵活性和可扩展性使其适用于大量问题。

PyMC3 基于 Theano 提供：

> +   计算优化和动态 C 编译
> +   
> +   Numpy 广播和高级索引
> +   
> +   线性代数运算符
> +   
> +   简单的可扩展性

……还有更多。你可以查看 [入门指南](http://docs.pymc.io/notebooks/getting_started) 和 [API 快速入门指南](http://docs.pymc.io/notebooks/api_quickstart)。

**相关：**

+   [5 个你无法再忽视的机器学习项目](/2016/05/five-machine-learning-projects-cant-overlook.html)

+   [5 个你无法再忽视的机器学习项目](/2016/06/five-more-machine-learning-projects-cant-overlook.html)

+   [5 个你无法再忽视的机器学习项目 – 第六集](/2017/09/five-machine-learning-projects-cant-overlook-episode-vi.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

### 更多相关话题

+   [每个数据科学家都应知道的三大 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学的顶级统计资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
