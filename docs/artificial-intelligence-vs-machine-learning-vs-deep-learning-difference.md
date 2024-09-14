# 人工智能 vs. 机器学习 vs. 深度学习：有什么区别？

> 原文：[https://www.kdnuggets.com/2019/08/artificial-intelligence-vs-machine-learning-vs-deep-learning-difference.html](https://www.kdnuggets.com/2019/08/artificial-intelligence-vs-machine-learning-vs-deep-learning-difference.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [ActiveWizards](https://activewizards.com/) 提供**

> 实际上，未来 10,000 个初创公司的商业计划很容易预测：拿到 X 并加入 AI。找到那些通过添加在线智能可以改进的东西。
> 
> — 凯文·凯利，《不可避免：理解将塑造我们未来的 12 大技术力量》

过去几年，人工智能继续成为最热门的话题之一。最顶尖的人才参与 AI 研究，最大的公司为这一领域的发展分配了巨额资金，而 AI 初创公司每年都获得数十亿美元的投资。

如果你从事业务流程改进或寻找新的商业创意，那么你很可能会接触到 AI。为了有效地与其合作，你需要了解其组成部分。

![图示](../Images/47b1ca51e464177a3dd91e6960b74671.png)

### 人工智能

让我们了解一下人工智能的本质。弗朗索瓦·肖莱在他的书《[Python 深度学习](https://www.manning.com/books/deep-learning-with-python)》中做了简要描述：“这是将通常由人类执行的智力任务自动化的努力。因此，AI 是一个包含机器学习和深度学习的通用领域，但也包括许多不涉及任何学习的方法。”

例如，今天聊天机器人的前身，[ELIZA](http://elizagen.org/index.html)，这是在 MIT 人工智能实验室创建的。这个程序能够与人进行长时间对话，但不能在对话过程中学习新词汇或修正其行为。ELIZA 的行为是通过一种特殊的编程语言明确指定的。

人工智能在现代意义上的历史始于 1950 年代，当时艾伦·图灵的工作和达特茅斯研讨会汇集了这一领域的首批爱好者，并制定了 AI 科学的基本原则。此后，该领域经历了几次兴趣激增和随后的萧条（所谓的“AI 冬天”），以至于今天成为世界科学的关键领域之一。

值得提及的是强人工智能和弱人工智能的假设。强人工智能能够思考并将自己意识到作为一个独立的个体。弱人工智能则没有这些能力，只能执行某些范围的任务（例如下棋、识别图像中的猫或[以$432,500绘制一幅画](https://www.forbes.com/sites/williamfalcon/2018/10/25/what-happens-now-that-an-ai-generated-painting-sold-for-432500/#5df73f2ea41c)）。所有现有的人工智能都是弱的，不必担心。

如今，很难想象任何一种活动没有人工智能的使用。无论是开车、自拍、在网上商店挑选运动鞋，还是规划假期，你几乎在每个地方都得到一个小巧、弱小但已经非常有用的人工智能的帮助。

### 机器学习

智能（无论是人工的还是非人工的）的一个关键特性是学习的能力。对于人工智能来说，这种能力由一系列机器学习模型负责。它们的本质很简单：与传统算法不同，传统算法是将输入数据转化为结果的明确指令集，而基于数据示例和相应结果的机器学习则在数据中找到模式，生成一个将任意数据转化为期望结果的算法。

机器学习主要分为三类：

+   ****监督学习**** - 系统基于具有先前已知结果的数据示例进行训练。机器学习中有两种最受欢迎的任务：回归和分类任务。回归是对连续结果的预测，例如房价或制造业排放水平。分类是对类别（类）的预测，例如电子邮件是否为垃圾邮件，或者书籍是侦探小说还是百科全书。

+   ****无监督学习**** - 系统在数据中发现内部关系和模式。在这种情况下，每个示例的结果是未知的。

+   ****强化学习**** 是一种方法，在这种方法中，系统因正确的行动而获得奖励，对错误的行动则给予惩罚。因此，系统学习制定一个算法，以获得最高奖励和最低惩罚。

一个理想的机器学习模型可以分析任何数据，发现所有模式，并创建一个实现任何期望结果的算法。但这个理想模型尚未创建。你可以在Pedro Domingos的[《大师算法》](https://www.basicbooks.com/titles/pedro-domingos/the-master-algorithm/9780465061921/)中了解创建它的过程。

![图像](../Images/10771446fa150382dfb833702a3e811f.png)

目前的机器学习模型专注于特定任务，它们都有各自的优点和缺点。这些模型包括以下几种：

+   ****线性回归**** 是一种经典的统计模型。顾名思义，它用于回归任务，即对连续值的预测。例如，根据天气预测会售出多少柠檬水。

+   ****逻辑回归**** 用于分类任务。它预测给定样本属于某个特定类别的概率。

+   ****决策树**** 是一种常用于分类任务的方法。在这种方法中，给定对象的类别被定义为一系列问题，每个问题通常涉及“是”或“否”的回答。

+   ****K-Nearest Neighbors**** 是一种简单且快速的方法，主要用于分类。在这种方法中，数据点的类别由与数据点示例最相似的 k 个（k 可以是任何数字）确定。

+   ****朴素贝叶斯**** 是一种流行的分类方法，它利用概率论和贝叶斯定理来确定在给定条件下（例如邮件中出现“免费贷款”这一短语 20 次）某个事件（如邮件是否为垃圾邮件）的可能性。

+   ****SVM**** 是一种监督机器学习算法，常用于分类任务。它能够有效地分离不同类别的对象，即使每个对象具有许多相关的特征。

+   ****集成学习**** 结合了许多机器学习模型，并通过对每个模型的响应进行投票或平均来确定对象的类别。

+   ****神经网络**** 基于人脑的原理。神经网络由许多神经元及其之间的连接组成。一个神经元可以表示为一个具有多个输入和一个输出的函数。每个神经元从输入中获取参数（每个输入可能具有不同的权重，这决定了其重要性），对其进行特定的操作，并将结果提供给输出。一个神经元的输出可以作为另一个神经元的输入。因此，形成了多层神经网络，这就是深度学习的主题。我们将更详细地讨论这一点。

神经元结构图：

![图](../Images/9b98a994d5b1ea8ffd4b5119df02d236.png)

具有两个隐藏层的人工神经网络：

![图](../Images/27b0ab0b6985e30c083ecb55699113b6.png)

通过学习给定示例，神经网络调整神经元之间的权重，以便给予对获得期望结果影响最大的神经元最大的权重。例如，如果一个动物是有条纹的、蓬松的并且在喵喵叫，那么它可能是只猫。与此同时，我们将最大权重分配给喵喵叫这个参数。所以即使动物没有条纹也不蓬松，但如果它喵喵叫 - 它仍然很可能是只猫。

### 深度学习

深度学习涉及深度神经网络。关于深度的意见可能有所不同。一些专家认为，如果网络具有多个隐藏层，它就可以被视为深度的，而其他专家则认为只有当网络具有许多隐藏层时，才算深度网络。

现在有几种类型的神经网络正在被广泛使用。其中最受欢迎的包括：

+   ****长短期记忆网络（LSTM）**** - 用于文本分类和生成、语音识别、音乐创作生成和时间序列预测。

+   ****卷积神经网络（CNN）**** - 用于图像识别、视频分析和自然语言处理任务。

### 结论

那么人工智能、机器学习和深度学习之间有什么区别呢？我们希望，阅读本文后，你已经知道这个问题的答案。人工智能是自动化智力任务（如阅读、[下围棋](https://techcrunch.com/2017/05/24/alphago-beats-planets-best-human-go-player-ke-jie/)、图像识别和创造自动驾驶汽车）的一个广泛领域。机器学习是一组负责AI学习能力的人工智能方法。深度学习是机器学习方法的一个子类，研究多层神经网络。

**[ActiveWizards](https://activewizards.com/)** 是一个数据科学家和工程师团队，专注于数据项目（大数据、数据科学、机器学习、数据可视化）。核心专长领域包括数据科学（研究、机器学习算法、可视化和工程）、数据可视化（d3.js、Tableau 等）、大数据工程（Hadoop、Spark、Kafka、Cassandra、HBase、MongoDB 等）以及数据密集型 Web 应用开发（RESTful API、Flask、Django、Meteor）。

[原文](https://activewizards.com/blog/artificial-intelligence-vs-machine-learning-vs-deep-learning-what-is-the-difference/)。已获许可转载。

**相关：**

+   [2018年数据科学领域前20大Python库](/2018/06/top-20-python-libraries-data-science-2018.html)

+   [金融领域数据科学的7大应用案例](/2018/05/top-7-data-science-use-cases-finance.html)

+   [前6大Python自然语言处理库比较](/2018/07/comparison-top-6-python-nlp-libraries.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 更多相关主题

+   [效率决定了生物神经元与人工神经元的差异](https://www.kdnuggets.com/2022/11/efficiency-spells-difference-biological-neurons-artificial-counterparts.html)

+   [免费人工智能与深度学习速成课程](https://www.kdnuggets.com/2022/07/free-artificial-intelligence-deep-learning-crash-course.html)

+   [从人工智能到机器学习再到……的演变](https://www.kdnuggets.com/2022/08/evolution-artificial-intelligence-machine-learning-data-science.html)

+   [人工智能系统中的不确定性量化](https://www.kdnuggets.com/2022/04/uncertainty-quantification-artificial-intelligencebased-systems.html)

+   [人工智能如何变革数据集成](https://www.kdnuggets.com/2022/04/artificial-intelligence-transform-data-integration.html)

+   [2022年最受欢迎的人工智能技能](https://www.kdnuggets.com/2022/08/indemand-artificial-intelligence-skills-learn-2022.html)
