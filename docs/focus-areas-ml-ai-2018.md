# 2018 年机器学习/人工智能的关注重点应该是什么？

> 原文：[`www.kdnuggets.com/2018/04/focus-areas-ml-ai-2018.html`](https://www.kdnuggets.com/2018/04/focus-areas-ml-ai-2018.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：[Vijay Srinivas Agneeswaran](https://www.linkedin.com/in/vijaysrinivasagneeswaran/)，SapientRazorfish**。

![](img/ae585158cf38d64f007dd91b116e45b6.png)

### 机器学习的生产化

这是 2018 年最重要的关注领域。大多数企业已经完成了机器学习的概念验证，并寻求通过全面的生产实施算法来实现数据的全部价值。这个领域的关键技术可能是 [Clipper](https://rise.cs.berkeley.edu/blog/low-latency-model-serving-clipper/)。Clipper 是来自伯克利大学 Rise 实验室的最先进的机器学习服务系统，利用分布式计算概念来扩展模型，使用容器化模型部署来处理任何平台创建的模型，并执行跨框架缓存和批处理，以利用像 GPU 这样的并行架构。最后，Clipper 还可以通过像集成和多臂强盗这样的机器学习技术执行跨框架模型组合。

另一个有趣的技术与模型选择有关，称为 autoML——类似于 [Uber 的 Michelangelo](https://eng.uber.com/michelangelo/)。许多开源框架如 TPot 和 AutoSKLearn 已出现，帮助自动搜索多个模型的空间，使用适当的超参数并选择最适合当前任务的模型。模型管理是指跟踪生产中数百个模型的能力，包括模型/分析的传承——例如，每个模型在何种数据集上评估了哪些指标及结果，实质上是模型开发、再训练和更新的整个周期等。ModelDB 是该领域的一个可能选择。

另一个有趣的工作来自 MapR——名为机器学习物流（Machine Learning Logistics）的工作，其核心理念是 Rendezvous 架构。Rendezvous 架构的关键元素包括基于流的微服务和容器化，以及促进警报和诱饵的 DataOps 风格设计。诱饵是一个对输入数据不进行任何操作的模型——它用于保存输入流的副本。警报是一个提供参考或基准的模型，用于与其他模型进行比较。Rendezvous 架构允许模型被大程度地监控，并且可以测量模型漂移，使得新的模型能够逐步、无缝地接管。

### 深度学习将继续存在

最近有一些论文揭示了深度学习的局限性，包括著名的文章作者[加里·马库斯](https://arxiv.org/pdf/1801.00631.pdf)和[KDNuggets](https://www.kdnuggets.com/2015/01/deep-learning-flaws-universal-machine-learning.html)上的这篇文章。加里认为，深度学习的局限性主要来源于仅有有限的数据进行训练，而实际中可能需要无限的数据才能实现完美的泛化。KDNuggets 文章展示了深度学习网络在分类任务中如何容易受到输入图像扰动以及随机（无意义）图像的欺骗，这些扰动和图像会导致高置信度的错误分类。

然而，正如谷歌深度思维团队在使用深度学习结合强化学习进行 Atari 游戏系统或 AlphaGo 的研究中所展示的，深度学习是非常有用的，并且可以与其他已知的机器学习技术结合，产生出色的结果。

使用深度学习的一个常见问题是超参数调优。[一种最近的方法](https://research.google.com/pubs/pub45826.html)展示了如何通过强化学习的方法组合某些形式的递归网络，这些网络可以在显著程度上超越现有系统。

[另一篇最近的论文](https://arxiv.org/pdf/1704.01212.pdf)提出了如何使用深度学习来预测小分子的量子力学性质——它展示了深度学习（特别是消息传递神经网络这种特定形式的深度学习）可以应用于图结构数据，并且对图同构是不变的。

### 隐式信号与显式信号

人们会撒谎，尤其是在调查中。因此，使用调查来理解用户行为的传统方法似乎不再奏效。这在 Netflix 中尤为明显，当时他们发现经典影片的评分非常高，但实际上并未被观看。这在 Google 搜索中也很明显，并且是最近 Strata Data 主题演讲的内容。Google 研究人员指出，在对马里兰州毕业生的调查中，只有 2%的人表示他们的 CGPA 低于 2.5，而实际情况是 11%的人 CGPA 低于 2.5。类似地，在另一个调查中，40%的工程师表示他们是公司中前 5%的工程师。Google 搜索是数字真相血清，人们在上面比在任何调查或其他平台上更为诚实。这也告诉我们，我们应该关注隐性信号（如人们在 Netflix 上实际观看的内容），而不是依赖于调查的显性反馈来理解消费者行为。这在 Pinterest 开发的一些推荐系统中也有所体现，如在这场[keynote speech](https://conferences.oreilly.com/strata/strata-ca/public/schedule/detail/66316)中记录的那样；他们使用了用户如何与钉子互动的隐性信号（保存钉子、重新钉其他用户的钉子、搜索钉子以及用户不喜欢或忽略的推荐），并能够根据基于图的推荐引擎为用户推荐适当的个性化内容。

### 模型解释性

模型解释性或可解释性是机器学习算法解释其为何以某种方式做出预测的能力。由于训练数据中看到的多个欺诈案例，系统可能将其推断为欺诈交易。随着越来越多的机器学习模型在金融、保险、零售甚至医疗等多个领域投入生产，模型解释性变得越来越重要。这个领域的一个新兴技术包括[Skater](https://github.com/datascienceinc/Skater)和[Lime](https://github.com/marcotcr/lime)等。

### 超越深度学习的人工智能

[Libratus](http://science.sciencemag.org/content/early/2017/12/15/science.aao1733.full)，另一个最近的游戏系统，将纳什均衡和博弈论结合起来，解决了像扑克这样的不完全信息游戏，并在动态竞争定价和产品组合优化中得到应用。Libratus 的三个主要组件包括：

1.  游戏抽象器——计算游戏的一个更小且易于解决的抽象，并为该抽象计算博弈论策略。

1.  第二个模块构建了子游戏的精细抽象（几轮后的游戏状态），并使用一种称为嵌套子游戏求解的技术来解决它。

1.  第三个模块是自我改进器，它为游戏创建蓝图策略，填充蓝图抽象的部分，并计算这些分支的博弈论方法。

强化学习是数据科学家工具包中的另一个重要工具。它现在与深度学习结合，开发出深度强化学习网络，例如来自[谷歌](https://storage.googleapis.com/deepmind-data/assets/papers/DeepMindNature14236Paper.pdf)的网络。

### 机器学习时代的隐私。

数据隐私及其相关的数据保护法律，包括《通用数据保护条例》（GDPR），在这方面非常重要。例如，GDPR 规定必须获得用户的同意才能收集个人数据。用户还拥有询问所收集数据的权利，以及修改、删除数据或反对使用个人数据进行定向的权利（如果他们愿意的话）。数据处理者根据 GDPR 也有一定的义务。GDPR 将在 2018 年影响所有数据产品的设计。

确保数据管道和基础设施的安全不仅重要，确保商业智能（BI）和分析的安全也同样重要。这时，隐私保护分析变得相关，并在 2018 年将变得极为重要。保障 BI 安全的相关技术是 Uber 与伯克利大学 Rise Labs 最近联合开发的，基于所谓的弹性敏感度，它结合了局部敏感度机制与一般等值连接，确保 SQL 查询的差分隐私。

对于深度学习模型，隐私保护分析是通过一种名为联邦学习的技术实现的，该技术由谷歌推广。它基于一个集中式参数服务器，托管所有学习所需的参数。每部手机可以下载这些参数，利用本地数据来改进模型，并生成一个小的更新消息，然后发送回参数服务器。服务器从多部手机中聚合更新，并在中心更新模型参数。联邦学习的有趣之处在于数据都是本地的，而学习是全球性的。每部手机通过使用自己的本地数据并在本地进行计算来更新模型——将机器学习与将数据存储在云端解耦。这一技术在 Android 的 Gboard（谷歌键盘）中得到了应用。

上述通过联邦学习进行的单一模型训练确实存在一些挑战，如高通信开销、滞后者和容错性问题，以及将模型与数据适配的统计挑战。这些问题通过 CMU 的[近期研究](http://www.sysml.cc/doc/30.pdf)得到解决，该研究使用联邦核化多任务学习来应对统计挑战，通过使用多个模型并同时更新它们，而系统挑战则由 MOCHA——分布式优化模型来解决。

[原文](https://medium.com/@a.vijaysrinivas/what-should-be-focus-areas-for-ml-ai-in-2018-dfc2da2d5f8a)。经授权转载。

**相关：**

+   [2018 年四大大数据趋势](https://www.kdnuggets.com/2018/01/four-big-data-trends-2018.html)

+   [2018 年的四大破损系统与四大技术趋势](https://www.kdnuggets.com/2018/03/four-broken-systems-four-tech-trends-2018.html)

+   [不要在 24 小时内学习机器学习](https://www.kdnuggets.com/2018/04/dont-learn-machine-learning-24-hours.html)

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [适用于科技行业各领域的热门谷歌认证](https://www.kdnuggets.com/popular-google-certification-for-all-areas-in-the-tech-industry)

+   [每个机器学习工程师应具备的 5 项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [每个数据科学家应了解的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/05/6-python-machine-learning-tools-every-data-scientist-know.html)

+   [每位工程师都应该并且可以学习机器学习](https://www.kdnuggets.com/2022/06/corise-every-engineer-learn-machine-learning.html)

+   [2023 年你应该了解的 10 种惊人机器学习可视化](https://www.kdnuggets.com/2022/11/10-amazing-machine-learning-visualizations-know-2023.html)

+   [KDnuggets 新闻，5 月 25 日：每个数据科学家应了解的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/n21.html)
