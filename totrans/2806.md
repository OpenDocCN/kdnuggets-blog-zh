# Phishytics – 检测网络钓鱼网站的机器学习

> 原文：[https://www.kdnuggets.com/2020/03/phishytics-machine-learning-detecting-phishing-websites.html](https://www.kdnuggets.com/2020/03/phishytics-machine-learning-detecting-phishing-websites.html)

[评论](#comments)

**作者 [Faizan Ahmad](http://faizanahmad.tech/)，弗吉尼亚大学**

[![机器学习检测网络钓鱼](../Images/25eb8aef7508a3c43436ef8774c205f8.png)](https://faizanahmad.tech/blog/2020/02/phishytics-machine-learning-for-phishing-websites-detection/)

几乎每周你都会看到关于网络钓鱼的新闻。在过去的一周里，黑客们正在[向 Disney+ 用户发送网络钓鱼邮件](https://www.cordcuttersnews.com/disney-hackers-are-sending-phishing-emails-heres-how-to-stay-safe/)，[‘Shark Tank’ 明星巴巴拉·科克伦在网络钓鱼骗局中损失了近 40 万美元](https://pagesix.com/2020/02/26/shark-tank-star-barbara-corcoran-loses-almost-400k-in-phishing-scam/)，[某银行发布了网络钓鱼警告](https://www.kcci.com/article/bank-issues-warning-over-phishing-attempts-and-phone-scams/31112737#)，而且[现在几乎四分之三的网络钓鱼网站都使用 SSL](https://www.helpnetsecurity.com/2020/02/26/phishing-ssl/)。由于网络钓鱼在网络安全领域是一个如此广泛的问题，让我们来深入探讨一下***机器学习在网络钓鱼网站检测中的应用***。尽管已有许多关于此主题的文章和研究论文[[恶意 URL 检测](https://arxiv.org/pdf/1701.07179.pdf)] [[通过视觉白名单检测网络钓鱼网站](https://arxiv.org/abs/1909.00300)] [[检测网络钓鱼的新技术](https://arxiv.org/pdf/1510.06501.pdf)]，它们并不总是提供开源代码并深入分析。本文旨在填补这些空白。我们将使用一个大规模的网络钓鱼网站语料库，并应用一些简单的机器学习方法来获得高精度的结果。

### 数据

解决这个问题的最佳部分是有良好收集的网络钓鱼网站数据集，其中[一个](http://www.fcsit.unimas.my/research/legit-phish-set)由马来西亚沙捞越大学的研究人员收集。这个***[‘钓鱼数据集 – 钓鱼与合法数据集用于快速基准测试’](http://www.fcsit.unimas.my/research/legit-phish-set)***数据集包含30,000个网站，其中15,000个是钓鱼网站，15,000个是合法网站。数据集中的每个网站都包含HTML代码、whois信息、URL以及网页中嵌入的所有文件。这对那些希望应用机器学习进行钓鱼检测的人来说是一个宝贵的资源。这个数据集有几种使用方式。我们可以通过查看URL和whois信息来尝试检测钓鱼网站，并像之前的一些研究那样手动提取特征[[1](https://arxiv.org/pdf/1701.07179.pdf)]。然而，我们将使用网页的原始HTML代码来看看是否可以通过构建机器学习系统有效对抗钓鱼网站。在URL、whois信息和HTML代码中，HTML代码是最难被混淆或更改的，如果攻击者试图阻止系统检测其钓鱼网站，因此我们在系统中使用HTML代码。另一种方法是将三种来源结合起来，这应该能提供更好、更稳健的结果，但为了简化起见，我们将仅使用HTML代码，并证明仅靠它即可获得有效的钓鱼网站检测结果。关于数据集的最后一点：由于计算限制，我们将仅使用20,000个样本。我们还将仅考虑用*英语*编写的网站，因为其他语言的数据稀缺。

### HTML代码的字节对编码

对于一个初学者来说，HTML 代码看起来并不像一种简单的语言。此外，开发者在编写代码时常常不遵循所有良好的实践。这使得解析 HTML 代码和提取词汇/标记变得困难。另一个挑战是 HTML 代码中许多词汇和标记的稀缺性。例如，如果一个网页使用了一个复杂名称的特殊库，我们可能在其他网站上找不到这个名称。最后，由于我们希望将系统部署到实际环境中，可能会有新的网页使用我们模型之前未见过的完全不同的库和代码实践。这使得使用简单的语言标记器并根据空格或其他标签或字符将代码拆分为标记变得更加困难。幸运的是，我们有一个叫做**[字节对编码](https://en.wikipedia.org/wiki/Byte_pair_encoding)**（BPE）的算法，它*根据频率将文本拆分为子词标记*，并解决了未知词汇的问题。在 BPE 中，我们首先将每个字符视为一个标记，并根据最高频率迭代合并标记。例如，如果出现了一个新词“*googlefacebook*”，BPE 会将其拆分为*“google”*和*“facebook”*，因为这些词在语料库中可能会频繁出现。BPE 已被广泛应用于最近的深度学习模型 [[2](https://arxiv.org/abs/1508.07909)]。

训练 BPE 的库有很多。我们将使用一个很棒的库，叫做**[tokenizer](https://github.com/huggingface/tokenizers)**，由[Huggingface](https://huggingface.co/)提供。按照这个库的[github 仓库](https://github.com/huggingface/tokenizers)中的说明非常简单。我们在原始 HTML 数据上用**10,000 tokens**的词汇量训练 BPE。BPE 的优点是它可以自动将 HTML 关键词如“tag”、“script”、“div”分离成单独的标记，即使这些标签在 HTML 文件中大多是用括号书写的，例如 <tag>，<script>。训练后，我们得到一个保存的标记器实例，我们可以用它将任何 HTML 文件标记化为单独的标记。这些标记用于机器学习模型中。

![图](../Images/53aded7e5f0ccf06b2922c1d7ec60c43.png)

图：HTML 代码中 BPE 标记的数量直方图

### TFIDF 与字节对编码

一旦我们从 HTML 文件中获取了令牌，就可以应用任何模型。然而，与现在大多数人所做的不同，我们不会使用像卷积神经网络 (CNN) 或递归神经网络 (RNN) 这样的深度学习模型。这主要是因为深度学习模型的计算复杂性和数据集相对较小。上图展示了 1000 个 HTML 文件中 BPE 令牌的直方图。我们可以看到，这些文件包含数千个令牌，其处理在像 CNN 和 RNN 这样的复杂模型中将会产生较高的计算成本。此外，对于网络钓鱼检测，令牌顺序并不是必需的。这将在查看结果时显而易见。因此，我们将简单地在 BPE 的每个令牌上应用 TFIDF 权重。

正如在之前关于 [作者归属](https://faizanahmad.tech/blog/2020/02/large-scale-authorship-attribution-machine-learning/) 的帖子中解释的那样，TFIDF 代表词频（term frequency）和逆文档频率（inverse document frequency），可以通过下面给出的公式计算。词频（tf）是文档 ***j*** 中词 ***i*** 的计数，而逆文档频率（idf）表示每个词在语料库中的稀有性和重要性。文档频率是通过统计词 ***i*** 在所有文档中出现的次数来计算的。 TF-IDF 给我们每个文档中每个词的权重，即 tf 和 idf 的乘积。

(1) ![\begin{equation*} w_{i,j} = tf_{i,j} * df_i \end{equation*}](../Images/35939d2378d3032bd22800d8f9e61f66.png)

### 机器学习分类器

为了保持简洁，我们将使用来自 scikit-learn 的 [随机森林分类器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) (RF)。为了训练分类器，我们将数据分成 90% 的训练集和 10% 的测试集。由于我们不打算对任何超参数进行广泛调优，因此没有进行交叉验证。我们将使用 scikit-learn 实现中的随机森林的默认超参数。与需要长时间训练的深度学习模型相比，RF 在 CPU 上训练时间不到 2 分钟，并且可以展示有效的结果，下一步将展示这些结果。为了展示性能的鲁棒性，我们在数据的不同划分上训练模型 5 次，并报告平均测试结果。

### 结果

| 准确率 | 精确率 | 召回率 | F1 分数 | AUC |
| --- | --- | --- | --- | --- |
| 98.55 | 98.29 | 98.82 | 98.55 | 99.68 |

网络钓鱼网站检测结果

上表显示了在 5 次实验中对测试数据的平均结果。乍一看，这些结果似乎非常好，尤其是在没有任何超参数调整和使用简单模型的情况下。然而，这些结果并没有那么出色。该模型对两个类别的精度都达到了 98%，这意味着在检测网络钓鱼网站时，它会产生大约 2% 的假正例。这在安全上下文中是一个巨大的数字。假正例是指机器学习模型认为是网络钓鱼的网站，但实际上是合法的网站。如果用户经常遇到假正例，他们的用户体验会很差，可能不愿意再使用这个模型。此外，安全人员在处理假正例时会[遇到威胁警报疲劳](https://www.csoonline.com/article/3191379/false-positives-still-cause-alert-fatigue.html)。假正例在下方的混淆矩阵中得到了进一步量化，其中 x 轴显示实际类别，y 轴显示预测类别。尽管模型实现了很高的准确率，但仍有 11 次实例中，模型预测“网络钓鱼”的网站实际上是一个安全的网站。

| 16（假负例） | 912（真负例） | **合法** |
| --- | --- | --- |
| 920（真正例） | 11（假正例） | **网络钓鱼** |
| **网络钓鱼** | **合法** | 预测类别 |
| 实际类别 |

模型的混淆矩阵

既然我们知道模型仍然存在问题，不能直接部署它，让我们看看一个潜在的解决方案。我们将使用[接收器操作特征曲线 (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) 来查看假正例和真正例率。在下图中，可以很容易地看到，对于高达 80% 的真正例率，我们的假正例率为 0%，这可以用于决策。

![图](../Images/0ac09f5d79947d25d35c34f52f3aa575.png)

图：ROC 曲线

ROC 曲线显示，对于特定的置信度阈值（**红点**），真正例率大约为 80-90%，而假正例率接近零。为了证明这一点，让我们查看不同的置信度阈值并绘制与之相关的指标。为了应用 ***x%*** 的置信度阈值，我们将仅保留模型对网站是合法网站还是网络钓鱼网站有超过 ***x%*** 置信度的网站。当我们这样做时，我们可以识别的网络钓鱼网站的总数（真正例率）会减少，但我们的准确率会显著提高，精度也接近 100%。

![图](../Images/ac107008e1b165f321164654f1051663.png)

图：置信度阈值对准确性、TPR 和 FP 的影响

上图演示了置信度阈值对**测试准确率**、**假阳性数量**和**真正阳性率**的影响。我们可以看到，当我们使用默认阈值0.5时，我们有11个假阳性。随着我们开始提高置信度分数，真正阳性率下降，但假阳性数量变得非常低。最后，在图表的最后一点，我们的精度为零假阳性。这意味着每当我们的模型表示一个网站正在进行钓鱼时，它是***总是***准确的。然而，由于我们的真正阳性率已下降到82%，模型现在只能检测大约82%的钓鱼网站。这就是机器学习如何在网络安全中使用，通过观察假阳性和真正阳性之间的权衡。大多数时候，我们希望假阳性率极低。在这种情况下，可以采用上述方法从模型中获得有效的结果。

### 限制

在结束这篇文章之前，让我们讨论一下我们上面看到的方法的一些局限性。首先，我们的数据集相当大，但对于所有类型的钓鱼网站来说，它并不全面。在过去的几年里，可能有数百万个钓鱼网站，但数据集中仅包含15,000个。随着黑客技术的进步，新出现的钓鱼网站可能不会犯旧有的错误，这可能使得使用上述模型来检测它们变得困难。其次，由于TFIDF特征表示并未考虑代码编写的顺序，我们可能会丢失信息。这个问题在深度学习方法中不会出现，因为它们可以顺序处理序列并考虑代码的顺序。此外，由于我们使用的是原始HTML代码，攻击者可以观察模型的预测结果，并花时间尝试在代码中设计混淆，使模型失效。最后，有人可以使用现成的代码混淆工具来混淆HTML代码，这将使模型再次失效，因为它只见过普通的HTML代码文件。然而，尽管有这些局限性，机器学习仍然可以非常有效地补充钓鱼黑名单，如[Google Safe Browsing](https://safebrowsing.google.com/)使用的那种。将黑名单与机器学习系统结合，可以提供比单独依赖黑名单更好的结果。

### 开源代码

正如我在这个博客的第一篇文章中讨论的那样，我会始终开源我在博客中讨论的项目的代码。延续这一传统，这里是复制所有实验、训练自己钓鱼检测模型以及使用我预训练模型测试新网站的链接。

**Github 仓库:** [https://github.com/faizann24/phishytics-machine-learning-for-phishing](https://github.com/faizann24/phishytics-machine-learning-for-phishing)

**简介: [Faizan Ahmad](http://faizanahmad.tech/)** 目前是弗吉尼亚大学（UVA）的硕士生，并在UVA的麦金泰尔商学院担任研究助理。他将于2020年6月加入Facebook担任安全工程师。他的兴趣在于网络安全、机器学习和商业分析的交叉点，他在这些领域做了大量的研究和工业项目。

[原文](https://faizanahmad.tech/blog/2020/02/phishytics-machine-learning-for-phishing-websites-detection/) 。已获得许可转载。

**相关内容：**

+   [将数据科学应用于网络安全网络攻击与事件](/2019/09/applying-data-science-cybersecurity-network-attacks-events.html)

+   [信任与安全中的7大数据科学应用案例](/2019/12/top-7-data-science-use-cases-trust-security.html)

+   [深度伪造技术的安全风险](/2020/01/deepfakes-security-risks.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的IT工作

* * *

### 更多相关内容

+   [无需编码即可轻松从网站抓取图像](https://www.kdnuggets.com/2022/06/octoparse-scrape-images-easily-websites-nocoding-way.html)

+   [获取数据科学项目中惊人数据的10个网站](https://www.kdnuggets.com/2023/04/10-websites-get-amazing-data-data-science-projects.html)

+   [使用Eurybia检测数据漂移以确保生产ML模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [检测ChatGPT、GPT3和GPT2的5个免费工具](https://www.kdnuggets.com/2023/02/5-free-tools-detecting-chatgpt-gpt3-gpt2.html)

+   [检测ChatGPT、GPT-4、Bard和Claude的10个最佳工具](https://www.kdnuggets.com/2023/05/top-10-tools-detecting-chatgpt-gpt4-bard-llms.html)

+   [每个机器学习工程师都应该掌握的5项机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)
