# 每个数据科学家都知道的肮脏小秘密（但不愿意承认）

> 原文：[https://www.kdnuggets.com/2018/04/dirty-little-secret-data-scientist.html](https://www.kdnuggets.com/2018/04/dirty-little-secret-data-scientist.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Scott W. Strong](https://www.cloudzero.com/blog/author/scott-w-strong)，Cloudzero**。

[原文](https://www.cloudzero.com/blog/the-dirty-little-secret-every-data-scientist-knows-but-wont-admit)。经许可转载。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

![](../Images/aa72c84a65097bc1090ca2fb77616f90.png)

去年五月，我需要接受左膝手术，希望能够恢复跑步——这是我最喜欢的活动之一。现在，你可能会想“这和数据科学有什么关系？”，但在我恢复的过程中，我发现了比你想象的更多的相似之处。我在康复几个月后开始建立这种联系，通过所有的深蹲、冰敷、平衡训练和按摩。恢复跑步状态和我每天需要做的任务非常相似，这些任务是为了建立一个强大的数据科学文化。更重要的是，我逐渐意识到，今天的数据科学很像准备和跑马拉松（这是少数人做的事情，而且做得好的人更少）。

许多人称机器学习和 [人工智能为下一个工业革命](https://www.wired.co.uk/article/ai-revolution-alexander-graubner-muller-kreditech)，这是一种 [推动创新](https://sloanreview.mit.edu/article/how-big-data-and-ai-are-driving-business-innovation-in-2018/) 的技术，也是我们时代的 [最重要的通用技术](https://hbr.org/cover-story/2017/07/the-business-of-artificial-intelligence)。

在我八年的行业经验中，我亲身观察了机器学习的好处，并不否认机器学习的影响可以是革命性的。

尽管获得了这些荣誉，多年来对机器学习是否真的能带来商业价值一直存在健康的怀疑。而这种批评并非没有道理。在机器学习/人工智能被广泛知晓的这十年中，只有在过去几年中，[C级高管表示他们正在从中获得收益](https://sloanreview.mit.edu/article/how-big-data-and-ai-are-driving-business-innovation-in-2018/)。机器学习的概念是否只是领先于十年前可用的计算能力，而现在才赶上？还是有其他因素在发挥作用？

让我们探讨一下如何为这场“数据科学马拉松”做准备，并揭示这个备受谈论领域背后的现实。

### 什么是数据科学？

数据科学，令人惊讶的是，并不是关于设计最先进的机器学习算法并用所有数据进行训练（然后拥有天网）。它是关于找到正确的数据，成为你尝试建模的过程、系统或事件的准专家，并创建能够帮助古怪且有时脆弱的统计算法做出准确预测的特征。实际上，算法本身所花的时间非常少。当然，也有例外，但即使在这些领域，大多数研究今天都是由像 Google、Facebook 等少数几个人（和/或公司）进行的，他们在推进该领域的技术方面。每个人通常是在新算法被整齐地打包成开源软件后才开始使用它们。一项对行业数据科学家的最新研究表明，79% 的时间花在收集、整理和清理数据上（60% 用于清理和整理，19% 用于收集数据），这在经验上[确认了有大量的马拉松准备工作](http://visit.crowdflower.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport_2016.pdf)。但实际的比赛何时开始？这展示了幕后发生的情况的非常不同的视角，并指出了早前提到的价值延迟的可能原因。

为了更好地理解为什么会发生这种情况，我们来看看一个典型的数据科学家在工作中遇到的情况。

### 一天的生活…

当你走进一家从未考虑过使用机器学习的公司时，一些挑战立即显现出来。经过几个问题的询问，你会很快发现，要将正确的信息放在正确的位置以供机器学习算法使用，这将是一项巨大的努力。这在十年前尤其如此，但即使今天也非常普遍。

在由传统组织孤岛管理的分散数据库中，且无法跨组织链接数据点时，你开始听到类似“哦……你需要去找Jeff谈谈销售管道数据”或“Beth，在DevOps部门……她是唯一一个拥有这些信息的人”之类的话。一旦你开始了解业务，你不可避免地会发现一些关键的信息没有被追踪或存储。甚至更糟的是，你需要开发一种收集之前不可用的数据的方法。这听起来可能有些戏剧化，但这是许多数据科学家在开始于刚刚涉足机器学习的公司时所经历的现实。早前提到的同一研究显示，[78%的数据科学家表示，他们最不喜欢的工作部分是收集、组织和清理数据](http://visit.crowdflower.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport_2016.pdf)（其中57%是清理和组织数据，21%是收集数据）。

对许多人来说，这意味着开始制定数据收集（和保留）政策，或者获得组织内的批准，以存储进行工作所需的客户数据。

### 数据科学家作为变革推动者

通过了解大多数数据科学家所面临的环境和限制，越来越明显的是，机器学习并不是驱动那些成功使用机器学习的公司的创新。当然，有些行业确实能从机器学习中获得巨大的好处（例如自动驾驶汽车、自然语言处理、语音识别等）。然而，对于绝大多数公司来说，机器学习就像一把吸引眼球的锤子，每个人都认为自己有一个闪亮的钉子。这些公司可能会将他们的成功归因于机器学习的魔力，但成功可能源自其他地方。

那么，为什么人们会声称他们在机器学习方面取得了成功，这种成功的驱动力是什么呢？我不认为他们是在虚假声称成功，我确实相信机器学习正在产生影响，但对大多数公司而言，我相信成功是由其他因素推动的。

公司未意识到的是，通过拥有内部的数据科学家和机器学习专家，他们从根本上改变了公司的运营方式。这些专家迫使公司组织其数据！

仅仅因为机器学习在公司所有（或大部分）数据的“鸭子”排好队之前是无效的，这些人就引导公司采取本来不会采取的行动。此外，数据科学家开始批判性地思考如何以数据驱动的方式为产品和业务决策提供信息。这是大多数公司运作方式的剧变，我们甚至还没有提到机器学习！我相信这种转变（组织公司的数据、批判性思考他们拥有或可能拥有的数据，并使这些数据可操作和可用）才是机器学习所催生的真正创新。

大多数人没有意识到，真正“花哨”的机器学习算法就像马拉松的最后一公里。在你到达那里之前，还有很多事情要做！所有的训练和准备，然后你还得跑完前25.2英里。没什么大不了的……对吧？

实际上，能到达马拉松的起跑线本身就是一件大事，更不用说跑完比赛了。

### 马拉松准备的挑战

在组织的“数据科学马拉松”起跑线前开始并体现这些数据驱动的原则在实践中是相当困难的。让我们看看CloudZero团队在构建数据驱动产品和公司过程中遇到的一些障碍。

在CloudZero，团队付出了大量努力，以理解来自Amazon Web Services（AWS）的不同数据源，从而提供对我们客户网站部署的全面视图。能够可视化和预测网站可靠性，我们的核心价值，并不是一项简单的任务，需要大量准备。我们仍处于马拉松的训练阶段，但幸运的是，我们在将数据放在正确位置上投入了大量资金，这被认为是[机器学习成功的关键](https://hbr.org/2018/04/if-your-data-is-bad-your-machine-learning-tools-are-useless)。

采纳这种心态意味着在云环境中开发事件和资源的标准表示，并使其对机器**和人类**可搜索。这还意味着确定在客户端环境中发生事件的真正参与者，通过“AssumeRole”事件链追踪活动（如果你对AWS有所了解，你会知道这有多痛苦）。最后，这意味着能够连接资源之间的点，了解它们如何相互通信，展示依赖关系和潜在风险领域。

我们认识到这些关键信息对于能够上下文化任何云部署的可靠性是必需的，并且必须与我们的机器学习和数据科学工作协调。虽然这些任务并不容易，但我们正迎头面对，知道以数据为先的方法将为我们提供许多在行业中创新的机会。

### 准备好比赛了吗？

机器学习确实是一个革命性的概念，它正渗透到我们的社会中，从我们与设备互动的方式、相互沟通的方式，到全球范围内的商业活动。但当你更深入地剖析它时，你会开始看到所有促成其成功的因素。虽然说出来可能有些离经叛道，但公司需要认识到，只要有一些基本的统计学和有意义的数据组织，他们实际上已经走了95%的路。机器学习无疑为最终产品增添了一些额外的特色（和准确性），但这个肮脏的小秘密是，它并不是成功的必要条件。

这绝不会使数据科学家的工作显得微不足道，也不会削弱机器学习研究员的努力。事实上，我相信了解这些现实会增加他们作为组织变革推动者的重要性。没有他们对干净和有意义数据的不断渴求，以及用这些数据做出突破性成果的愿景，公司可能永远不会推动数据组织上的进步。

我希望这篇文章能帮助打破一些阻碍人们（或公司）利用数据驱动决策的障碍，并使我们所有人（即使只是一点点）更接近一个更美好的未来的起点！

这在你们公司是否也如此？数据驱动决策对你们有何影响？

**个人简介：[Scott Strong](https://www.cloudzero.com/blog/author/scott-w-strong)** 是 CloudZero 的研究总监，他在 CloudZero 的产品中将机器学习、建模与仿真以及黑客技术完美结合。在加入 CloudZero 之前，Scott 曾在位于亚特兰大的初创公司 Pindrop 担任机器学习研究员和数据科学家，致力于防止电话诈骗。

[原文](https://www.cloudzero.com/blog/the-dirty-little-secret-every-data-scientist-knows-but-wont-admit)。经许可转载。

**相关内容：**

+   [数据科学家的日常生活：第 4 部分](https://www.kdnuggets.com/2018/04/day-life-data-scientist-part-4.html)

+   [有志数据科学家的关键算法和统计模型](https://www.kdnuggets.com/2018/04/key-algorithms-statistical-models-aspiring-data-scientists.html)

+   [在 Python 中整理数据](https://www.kdnuggets.com/2017/01/tidying-data-python.html)

### 更多相关内容

+   [GPT-4: 一种模型，八种用途；秘密被揭晓](https://www.kdnuggets.com/2023/08/gpt4-8-models-one-secret.html)

+   [HuggingGPT：解决复杂 AI 任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)

+   [开始 LLMOps：无缝交互的秘密秘诀](https://www.kdnuggets.com/getting-started-with-llmops-the-secret-sauce-behind-seamless-interactions)

+   [MLOps 是一团糟，但这在意料之中](https://www.kdnuggets.com/2022/03/mlops-mess-expected.html)

+   [Baize：一个开源聊天模型（但有所不同？）](https://www.kdnuggets.com/2023/04/baize-opensource-chat-model-different.html)

+   [如果你想精通生成式 AI，忽略所有（但有两个）工具](https://www.kdnuggets.com/if-you-want-to-master-generative-ai-ignore-all-but-two-tools)
