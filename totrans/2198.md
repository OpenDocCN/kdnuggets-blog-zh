# 超越猜测：利用贝叶斯统计进行有效文章标题选择

> 原文：[https://www.kdnuggets.com/beyond-guesswork-leveraging-bayesian-statistics-for-effective-article-title-selection](https://www.kdnuggets.com/beyond-guesswork-leveraging-bayesian-statistics-for-effective-article-title-selection)

![超越猜测：利用贝叶斯统计进行有效文章标题选择](../Images/5209ec4975d252aa4bbfee61879367e3.png)

图片由作者提供

# 介绍

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

一个好的标题对于文章的成功至关重要。人们通常只用一秒钟的时间（如果我们相信 Ryan Holiday 的书籍《[相信我，我在撒谎](https://en.wikipedia.org/wiki/Trust_Me,_I%27m_Lying)》）来决定是否点击标题打开整篇文章。媒体对优化 [点击率 (CTR)](https://support.google.com/google-ads/answer/2615875) 情有独钟，点击率是标题被点击的次数与标题展示的次数之比。具有吸引点击的标题会增加点击率。媒体通常会选择点击率更高的标题，因为这样可以产生更多的收入。

我并不特别关注挤压广告收入。更重要的是传播我的知识和专业技能。然而，观众的时间和注意力有限，而互联网上的内容几乎是无限的。因此，我必须与其他内容创作者竞争，以获得观众的注意力。

我该如何为下一篇文章选择一个合适的标题？当然，我需要一组选项供选择。希望我可以自己生成这些选项或请 ChatGPT 帮忙。但接下来我该怎么办？作为数据科学家，我建议进行 A/B/N 测试，以数据驱动的方式理解哪个选项最好。但也有问题。首先，我需要迅速决定，因为内容会很快过时。其次，可能没有足够的观察数据来发现点击率的统计显著差异，因为这些值相对较低。因此，除了等待几周来决定，还有其他选择。

希望有解决方案！我可以使用一种“多臂老虎机”机器学习算法，该算法会根据我们观察到的观众行为数据进行调整。选择集中的某一选项被点击的次数越多，我们可以分配给该选项的流量就越多。在本文中，我将简要解释什么是“贝叶斯多臂老虎机”，并演示如何使用 Python 在实践中应用它。

# 什么是贝叶斯多臂赌博机？

[多臂赌博机](https://en.wikipedia.org/wiki/Multi-armed_bandit)是机器学习算法。贝叶斯类型利用[汤普森采样](https://en.wikipedia.org/wiki/Thompson_sampling)来选择选项，这基于我们对点击率概率分布的先验信念，这些信念会根据新数据进行更新。所有这些概率论和数学统计学的术语可能听起来复杂且令人生畏。让我尽可能少用公式来解释整个概念。

假设只有两个标题可以选择。我们对它们的点击率（CTR）毫无头绪。但我们希望找到表现最好的标题。我们有多个选择。第一个是选择我们更相信的标题。这是多年来行业中的运作方式。第二个是将50%的流量分配给第一个标题，将50%分配给第二个标题。这种方法随着数字媒体的兴起变得可能，你可以精确决定在观众请求阅读文章列表时显示的文本。通过这种方法，你可以确保50%的流量分配给了表现最好的选项。这是极限吗？当然不是！

有些人在文章发布后的几分钟内就会阅读这篇文章。有些人则需要几个小时或几天。这意味着我们可以观察到“早期”读者对不同标题的反应，并将流量分配从50/50调整为更多地分配给表现更好的选项。过一段时间后，我们可以再次计算点击率（CTR），并调整分配比例。在极限情况下，我们希望在每个新观众点击或跳过标题后调整流量分配。我们需要一个科学且自动的框架来适应流量分配。

这就是贝叶斯定理、贝塔分布和汤普森采样的用武之地。

![超越猜测：利用贝叶斯统计有效选择文章标题](../Images/becfca1c57745153a65818de5ab6398b.png)

假设一篇文章的点击率是一个随机变量“theta”。按设计，它的值在0到1之间。如果我们没有先验信念，它可以是0到1之间的任何一个数，且概率相等。在观察到一些数据“x”后，我们可以调整我们的信念，并通过贝叶斯定理得到一个新的“theta”分布，该分布会偏向于0或1。

![超越猜测：利用贝叶斯统计有效选择文章标题](../Images/ca44832427e3e5e6949fdc28e49093e5.png)

点击标题的人数可以建模为一个[二项分布](https://en.wikipedia.org/wiki/Binomial_distribution)，其中"n"是看到标题的访问者数量，"p"是标题的CTR。这是我们的似然性！如果我们将先验（我们对CTR分布的信念）建模为一个[贝塔分布](https://en.wikipedia.org/wiki/Beta_distribution)并采用二项似然性，则后验分布也将是一个具有不同参数的贝塔分布！在这种情况下，贝塔分布被称为对似然性的[共轭先验](https://en.wikipedia.org/wiki/Conjugate_prior)。

证明这一事实并不难，但需要一些数学练习，这在本文中不相关。请参考[这里](https://en.wikipedia.org/wiki/Conjugate_prior#Example)的精美证明：

![超越猜测：利用贝叶斯统计有效选择文章标题](../Images/a7d8bb1a7aff967294356d869202cbd9.png)

贝塔分布的范围在0到1之间，这使其成为建模CTR（点击率）分布的理想选择。我们可以从"**a = 1**"和"**b = 1**"作为建模CTR的贝塔分布参数开始。在这种情况下，我们对分布没有任何信念，使得任何CTR都是同等可能的。然后，我们可以开始添加观察到的数据。如你所见，每一次"**成功**"或"**点击**"都将"a"增加1，每一次"**失败**"或"**跳过**"都将"b"增加1。这会使CTR的分布偏斜，但不会改变分布的家族，它仍然是贝塔分布！

我们假设CTR可以建模为贝塔分布。然后，有两个标题选项和两个分布。我们如何选择向观众展示什么？因此，算法被称为"多臂老虎机"。当观众请求标题时，你"拉动两个臂"并采样CTR。之后，你比较这些值并展示具有最高采样CTR的标题。然后，观众要么点击，要么跳过。如果标题被点击，你将调整此选项的贝塔分布参数"a"，代表"成功"。否则，你将增加此选项的贝塔分布参数"b"，意味着"失败"。这会使分布偏斜，对于下一个观众，选择此选项（或"臂"）的概率与其他选项相比会有所不同。

经过几次迭代，算法将对CTR分布进行估计。从该分布中采样将主要触发最高CTR的选项，但仍允许新用户探索其他选项并重新调整分配。

好吧，这一切在理论上都是可行的。它真的比我们之前讨论过的50/50拆分更好吗？

# 用Python构建模拟

创建模拟和构建图表的所有代码可以在我的[GitHub Repo](https://github.com/IgorKhomyanin/blog/blob/main/multiarmed-bandits-for-media/multiarmed-bandits-for-media.ipynb)中找到。

如前所述，我们只有两个标题可供选择。我们对这些标题的点击率没有先验信念。因此，我们从a=1和b=1开始，为两个Beta分布设定初始值。我将模拟一个简单的流量假设为观众队列。我们确切知道在向新观众展示标题之前，前一个观众是“点击”还是“跳过”。为了模拟“点击”和“跳过”动作，我需要定义一些真实的点击率。设定它们为5%和7%。需要强调的是，算法对这些值一无所知。我需要它们来模拟点击；你会在现实世界中有实际的点击。我将为每个标题抛掷一个超级偏向的硬币，其正面出现的概率为5%或7%。如果抛出正面，则为点击。

然后，算法是直接的：

1.  根据观察到的数据，为每个标题获取Beta分布。

1.  从两个分布中采样点击率。

1.  了解哪个点击率（CTR）更高，并抛掷相关的硬币。

1.  了解是否有点击发生。

1.  如果有点击，则将参数“a”增加1；如果有跳过，则将参数“b”增加1。

1.  重复直到队列中有用户。

为了了解算法的质量，我们还将保存一个值，表示暴露于第二个选项的观众份额，因为它具有更高的“真实”点击率。我们将使用50/50分配策略作为基线质量的对比。

作者提供的代码

在队列中有1000名用户之后，我们的“多臂老虎机”已经对点击率有了很好的了解。

![超越猜测：利用贝叶斯统计进行有效的文章标题选择](../Images/58a1f90577ef9e9f09d7ae27e226e523.png)

这里有一个图表显示了这种策略能带来更好的结果。在100名观众之后，“多臂老虎机”超过了50%观众的第二个选项份额。由于越来越多的证据支持第二个标题，算法将更多流量分配给了第二个标题。几乎80%的观众都看到了表现最好的选项！而在50/50的分配中，只有50%的观众看到了表现最好的选项。

![超越猜测：利用贝叶斯统计进行有效的文章标题选择](../Images/d80b82a28a658edc8f8aa969ff85e031.png)

贝叶斯多臂老虎机展示了额外的25%观众看到表现更好的选项！随着数据的增加，这两种策略之间的差异只会加大。

# 结论

当然，“多臂老虎机”并不完美。实时采样和服务选项的成本很高。最好拥有良好的基础设施以在期望的延迟下实现整个过程。此外，你可能不希望通过更改标题来让观众感到困惑。如果你的流量足够进行快速A/B测试，那就做吧！然后，手动更改一次标题。然而，这个算法可以应用于媒体以外的许多其他领域。

我希望你现在理解了什么是“多臂赌博机”以及它如何用于在适应新数据的情况下在两个选项之间进行选择。我特别没有关注数学和公式，因为教科书会更好地解释这些内容。我打算介绍一种新技术，并激发对它的兴趣！

如果你有任何问题，请随时通过 [LinkedIn](https://www.linkedin.com/in/igorkhomyanin/) 联系我。

所有代码的笔记本可以在我的 [GitHub 仓库](https://github.com/IgorKhomyanin/blog/blob/main/multiarmed-bandits-for-media/multiarmed-bandits-for-media.ipynb) 中找到。

**[](https://www.linkedin.com/in/igorkhomyanin/)**[Igor Khomyanin](https://www.linkedin.com/in/igorkhomyanin/)**** 是 Salmon 的数据科学家，曾在 Yandex 和 McKinsey 担任数据相关职位。我专注于使用统计学和数据可视化从数据中提取价值。

### 更多相关话题

+   [数据科学中的贝叶斯统计与频率统计](https://www.kdnuggets.com/2023/05/bayesian-frequentist-statistics-data-science.html)

+   [数据库内分析：利用 SQL 的分析函数](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)

+   [数据科学中的 SQL：理解和利用连接](https://www.kdnuggets.com/2023/08/sql-data-science-understanding-leveraging-joins.html)

+   [利用 XGBoost 进行时间序列预测](https://www.kdnuggets.com/2023/08/leveraging-xgboost-timeseries-forecasting.html)

+   [使用 GeoPandas 在 Python 中利用地理空间数据](https://www.kdnuggets.com/leveraging-geospatial-data-in-python-with-geopandas)

+   [利用 GPT 模型将自然语言转化为 SQL 查询](https://www.kdnuggets.com/leveraging-gpt-models-to-transform-natural-language-to-sql-queries)
