# 5 种将伦理应用于人工智能的方法

> 原文：[`www.kdnuggets.com/2019/12/5-ways-apply-ethics-ai.html`](https://www.kdnuggets.com/2019/12/5-ways-apply-ethics-ai.html)

评论

**作者 [Marek Rogala](https://twitter.com/marekrog)，Appsilon 首席技术官**

*在 [上一篇文章](https://appsilon.com/dusting-under-the-bed-machine-learners-responsibility-for-the-future-of-our-society/) 中，我表达了我有机会在 *[*ML in PL*](https://conference.mlinpl.org/)* 会议上发表演讲的快乐。我有机会退后一步，反思一下我们作为数据科学从业者和机器学习模型构建者所做工作的伦理。这是一个重要的话题，却没有得到应有的关注。*

*我们构建的算法会影响生活。*

*我对这个话题进行了大量研究，在此过程中，我发现了一些对我印象深刻的故事。这里有六个基于真实生活的教训，我认为作为从事机器学习的人员，无论你是研究员、工程师还是决策者，我们都应该记住。*

### 该是展示你卡片的时候了

![](img/68b436bd5645c3e0ec7d568a1326d583.png)

现在是时候举一个更积极的例子了，这是我们在日常工作中可以遵循的做法。OpenAI 最终发布了完整的 [GPT-2 模型](https://openai.com/blog/better-language-models/) 用于文本生成。OpenAI 注意到该模型功能强大，可能会被用于非常不良的方式（从个人测试来看，我可以确认它通常非常逼真）。因此，他们在 2 月份发布了一个限制版本，并启动了一个过程。他们邀请研究人员对模型进行实验，要求人们构建检测系统，以查看检测某些内容是否由机器人生成的方法的准确性。他们还聘请了社会科学家，因为作为工程师我们应该知道自己的局限性，无法理解我们发布的模型的所有影响。但我们可以与那些理解这些的人合作。

他们使用的工具之一是我们在日常工作中都可以使用的——**模型卡片**。这是 [谷歌的几位员工建议的](https://ai.google/research/pubs/pub48120/)。模型卡片以标准化的方式展示了预期用途和误用情况。它显示了数据是如何收集的，以便研究人员可以进行实验并发现过程中的一些错误。卡片还可以包含警告和建议。无论你是向公众发布还是仅限内部使用，我认为完成一张“M-card”是很有用的。我认为 OpenAI 做得很好。这就引出了第 6 课。

> ### **第 6 课：评估风险。沟通预期用途。**

前进。我上周在推特上看到了这个消息。一些研究人员展示了一种模型，这种模型将使用面部识别来支付伦敦地铁的入口费用。

> 面部识别可以让你的通勤变得轻松很多 [pic.twitter.com/B3SISYq0Zb](https://t.co/B3SISYq0Zb)
> 
> — Mashable (@mashable) [2019 年 11 月 9 日](https://twitter.com/mashable/status/1193088083492630528?ref_src=twsrc%5Etfw)

-   我震惊于他们根本没有提到任何风险，例如执法滥用、隐私问题、监控、移民权利、偏见以及威权国家的滥用。影响巨大。所以，第 7 课：获得媒体关注对一个酷炫的模型很容易，但我们不应该像布里斯托尔的那些研究人员那样。如果一个视频以这种方式被展示，我们应该确保风险被指出。

> ### **第 7 课：媒体报道很容易获得。确保风险被传达。**

![](img/a4335775911823e04d9df26e65e79f4a.png)这是我想给你展示的另一个正面例子——这是[Evan Estola 的讲座](https://www.youtube.com/watch?v=MqoRzNhrTnQ)，他是[Meetup](https://www.meetup.com/)的首席机器学习工程师。他做了一个有用的讲座，名为“当推荐系统变坏时”，讨论了他们做出的一些决策。他提醒我们 Goodhart 定律：

> “当一个衡量标准变成目标时，它就不再是一个好的衡量标准。”

“我们有道德义务不要教会我们的机器有偏见，”他补充道。例如，在美国，科技岗位中男性多于女性。那么 Meetup 推荐模型是否应该因为技术聚会主要由男性参加而劝阻女性参加？当然不应该。但是如果模型没有特别设计，模型可能会从数据中轻易推断出女性对技术活动不感兴趣，然后反过来维持性别刻板印象。所以第 8 课……

> ### **第 8 课：记住，一个指标始终是我们关心的事物的代理。**

那么政府监管的问题呢？以下是对我来说最震惊的例子。也许你们中的一些人知道去年在缅甸发生了种族灭绝。数千名罗兴亚人在军队、警察和其他多数族群成员的手中丧生。Facebook 今年终于承认，他们没有做足够的工作——这个平台成为了人们传播暴力和暴力内容的途径。所以，基本上多数族群的人传播了对少数族群罗兴亚人的仇恨。他们信仰不同的宗教，这只会加剧暴力。

![](img/3dc9bea082dc25213b68d96faf12025b.png)

情况最糟糕的部分是，Facebook 高管早在 2013 年就已被警告。五年后，暴力爆发。在 2015 年，第一次警告后，Facebook 只有四名讲缅甸语的承包商审查内容——显然不够。他们只是没有足够重视。

[瑞秋·托马斯](https://www.youtube.com/watch?v=WC1kPtG8Iz8) 比较了 Facebook 在两个国家的反应。一个是针对缅甸，Facebook 宣称他们为缅甸语添加了“数十个”内容审查员。在同一年，他们在德国雇用了 1,500 名内容审查员。为什么会这样？因为德国威胁 Facebook（及其他公司）如果不遵守仇恨言论法，将处以 5000 万美元的罚款。这是一个法规如何发挥作用的例子，因为它使那些主要关注利润的管理者认真对待风险。

这是一个关于法规的个人例子。我有两个小孩，所以我对儿童座椅变得非常熟悉。过去，很多人声称汽车不能受到监管。安全问题被归咎于驾驶员。稍微快进一下，现在的计算结果是，相比于面向前方的座椅，儿童在面向后方的座椅中安全性提高了五倍。各国的法规不同。在瑞典，他们的法规基本上支持使用面向后方的座椅。因此，从 1992 年到 2013 年，仅有 15 名儿童在交通事故中死亡。相比之下，在没有这样的法规的波兰，每年有 70 到 150 名儿童在交通事故中死亡**。

监管最终会到来 AI。问题是它是否会明智还是愚蠢。技术人员通常反对监管，因为监管往往设计和实施不佳。但我认为这是因为我们需要使其明智。我们最终会有关于 AI 的监管，但尚不确定其质量和何时发生。

> ### **第 9 课：法规是我们的盟友，而不是敌人。倡导明智的法规。**

最后的例子。在 [Appsilon](https://appsilon.com/) 我们投入了相当多的时间进行“AI for Good”倡议。因此，我们与非政府组织合作，利用 AI 模型研究气候变化、保护野生动物等，这非常好，我很高兴看到其他公司也在这样做。但我们应该意识到一种叫做**技术主义**的现象。

有一本书，作者是 Kentaro Toyama，书名为 “[极客异端](https://geekheresy.org/)”。Toyama 先生是微软的工程师，他被派往印度通过技术帮助社会变革和改善人们的生活。他发现，人们在尝试用技术解决问题时，往往把西方视角应用于一切，结果犯了很多错误。他展示了许多通过技术解决问题的高期望是如何失败的例子。

我们应该与领域专家密切合作，首先解决简单问题**，以合适的深度**，以便在领域专家和工程师之间建立共同的理解。工程师需要了解问题的根源，而领域专家需要了解技术能够做什么。只有这样，真正有用的想法才能出现。

> ### **第 10 课：在 AI for Good 中，与领域专家紧密合作，并警惕技术主义。**

![](img/1988efbe7edd6da14c91ceed457a6a45.png)

我们所构建的算法影响着生活。通过互联网和社交媒体，它们可以字面上塑造你的思维。它们影响医疗、就业和法庭案件。考虑到不到百分之一的人口会编码，想象一下其中有多小的一部分真正理解人工智能。所以我们肩负着塑造社会未来的**重大且令人兴奋的责任**，以确保它光明。

![你了解其他人不知道的问题。你对我们社会的形态负有责任。](img/9b17bae0c05cc89deb22acdd4956cd5c.png)

你了解其他人不知道的问题。你对我们社会的形态负有责任。

你有自己的“经验教训”吗？请在下面的评论中添加。

感谢阅读！在 Twitter 上关注我 [@marekog](https://twitter.com/marekrog)。

**关注 Appsilon 数据科学的社交媒体**

+   在 Twitter 上关注[ @Appsilon](https://twitter.com/appsilon)

+   在[ LinkedIn](https://www.linkedin.com/company/appsilon) 上关注我们

+   注册我们的[ 新闻通讯](https://appsilon.com/blog/)

+   尝试我们的 R Shiny[ 开源](https://appsilon.com/opensource/) 包

**个人简介：[Marek Rogala](https://twitter.com/marekrog)** 是 Appsilon 的首席技术官。

[原文](https://appsilon.com/5-ways-to-apply-ethics-to-ai/?nabe=4634331497365504:0)。经许可转载。

**相关：**

+   床下的灰尘：机器学习者对我们社会未来的责任

+   5 个数据科学家应该避免的统计陷阱

+   设计伦理算法

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [5 种将人工智能应用于小数据集的方法](https://www.kdnuggets.com/2022/02/5-ways-apply-ai-small-data-sets.html)

+   [人工智能伦理：导航智能机器的未来](https://www.kdnuggets.com/2023/04/ethics-ai-navigating-future-intelligent-machines.html)

+   [使用 Pandas DataFrames 的 apply() 方法](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [MLOps：最佳实践及如何应用](https://www.kdnuggets.com/2022/04/mlops-best-practices-apply.html)

+   [什么是切比雪夫定理，它如何应用于数据科学？](https://www.kdnuggets.com/2022/11/chebychev-theorem-apply-data-science.html)

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理及其如何…](https://www.kdnuggets.com/2022/n46.html)
