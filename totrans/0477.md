# 如何克服机器学习工程师的冒名顶替综合症

> 原文：[https://www.kdnuggets.com/2021/10/defeat-machine-learning-engineer-impostor-syndrome.html](https://www.kdnuggets.com/2021/10/defeat-machine-learning-engineer-impostor-syndrome.html)

[评论](#comments)

**由[帕乌·拉巴尔塔·巴霍](https://www.linkedin.com/in/pau-labarta-bajo-4432074b/)，数学家和数据科学家**。

![](../Images/2904b3192b694f6d0d3e95d44a91d2d1.png)

## 背景

当我第一次申请[Toptal](https://www.toptal.com/Bxdpg6/worlds-top-talent)时，我想同时成为一个自由职业者和“真正的机器学习工程师”。

在此之前，我在Nordeus公司担任机器学习工程师，这是一家著名的手机游戏公司，以其旗舰游戏TopEleven上穆里尼奥的面孔而闻名。我在Nordeus的机器学习经历包括设计和实现一个智能系统，以帮助客服团队更快地解决玩家问题。其核心是从大量历史玩家票据和代理解决方案中构建一个文本分类器。

我心里有整个系统的构思，数据（至少我认为是这样），以及对GPU的访问。从纸面上看，一切似乎都很适合我大显身手，交付一个优秀的模型和更出色的解决方案。

但这从未发生。令我绝望的是，我花了超过一个月的时间才意识到我试图用来训练我监督模型的数据集糟糕透了。在意识到这一点之前，我花费了无数小时和Jupyter notebooks试图让这一切运作。我忙得没时间看数据。可以说，我的经验不足没有帮助。

在这个失败的项目后三个月，我决定辞职并开始我的自由职业生涯，加入了[Toptal](https://www.toptal.com/Bxdpg6/worlds-top-talent)。经过几轮面试和技术筛选，我进入了最后一轮。你猜怎么着？我需要解决一个机器学习任务。几乎和我之前失败的任务一模一样。我有一周的时间来完成它。

描述那一周我不得不与大量负面自我对话作斗争的情况很难。冒名顶替综合症的长长阴影遮蔽了我的思维。

这一章有一个快乐的结局。我解决了问题，成功加入了Toptal。三年和10个项目后，我可以说我更好地应对了冒名顶替综合症。

### 提示1：勇敢尝试自由职业

勇敢是最能帮助你的东西。而自由职业确实是勇敢的。如果你想了解更多，请查看我之前的文章[如何成为自由职业数据科学家](https://www.kdnuggets.com/2021/08/how-become-freelance-data-scientist.html)。

当你作为自由职业者/合同工工作时，反馈不会在季度或年度评估中出现。它每天都会到来。没有捷径可走。客户期望你提供质量和速度。顺便说一下，这也是你比当前工作获得更高薪资的主要原因。

一旦你觉得自己掌握了机器学习的基础，就去试试自己。挑战自己。你聪明，你可以做到。多上在线课程不会消除冒名顶替综合症。相信我。

我认为排名前2的自由职业平台是

+   [Toptal](https://www.toptal.com/Bxdpg6/worlds-top-talent)。全球排名第一的顶尖自由职业平台。他们有一个相当严格的申请流程，但绝对值得经历。如今，大型企业和新兴初创公司都在使用Toptal来实施机器学习解决方案。进入那里会给你提供大量闪耀的机会。

+   [Braintrust](https://app.usebraintrust.com/r/pau1/)。一个新兴的人才网络，受到一种全新经济模型的启发（听说过以太坊吗？）。他们正在快速成长，我预计他们很快会赶上Toptal。

![](../Images/92eba4e7b72db1d4cefee155afd29b1d.png)

*照片由[Johannes Plenio](https://www.pexels.com/@jplenio?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)提供，来自[Pexels](https://www.pexels.com/photo/two-person-on-boat-in-body-of-water-during-golden-hour-2850287/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。*

### 提示2：永远不要忘记数据。绝对不要。

机器学习工程比传统软件工程更难，因为数据（大写字母，是的）。

很少有机会能得到一套干净且完整的特征和标签来构建你的机器学习模型。相反，你通常需要自己生成训练数据。我遇到的最常见问题是：

+   **训练数据有缺陷**。通常，你通过SQL查询和一些Python脚本自动提取原始特征和标签来生成这些数据。编写SQL查询很简单，但调试它们可能会非常困难。提高你的SQL技能是成为更好机器学习工程师的最佳途径之一。

+   **训练数据不完整**。在生成第一版训练数据后，你进入机器学习开发的下一阶段，建立一个快速基线模型。这个基线模型通常不足以解决业务问题，因此你需要迭代。缺乏经验的机器学习开发人员往往过于专注于改进模型，而忽视了他们创建的数据集。这是一个典型的错误，会引发挫折感和冒名顶替综合症。回到数据中。扩展你的SQL查询以添加更多相关特征。与环境中的领域专家（数据工程师、商业智能人员等）交流，他们可以帮助你获取能推动进展的数据，解除你的阻碍。

数据（DATA）是推动所有模型的神奇成分，从简单的线性回归到庞大的Transformer模型。如果燃料不好，驾驶哪辆车都无所谓。你是不会前进的。

这听起来如此琐碎和愚蠢，以至于我们（包括我在内）机器学习工程师有惊人的倾向去忘记。随着你在构建机器学习解决方案方面经验的增加，你会更好地记住这一点，并在遇到障碍时回到数据（DATA）上。

你不能使用Stackoverflow来调试你的数据集。你在那里是孤单的。你需要改变你的思维方式。你必须表现得像一个问题解决者。你需要了解数据集，最佳方式是可视化。我个人喜欢Tableau Desktop，但还有其他选项，比如Power BI、Apache Superset等。如果你愿意，还可以使用Python库，例如 [sweetviz](https://github.com/fbdesignpro/sweetviz)。

无论你偏好什么工具，每次遇到困难时都要回到数据（DATA）上。

![](../Images/4f31219550088c9683ed1cc9a3f77071.png)

*照片由 [若昂·杰苏斯](https://www.pexels.com/@joaojesusdesign?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来自 [Pexels](https://www.pexels.com/photo/orange-brick-wall-921319/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。*

### 提示3：不要期望知道一切（尤其是在开始时）。

机器学习是一个涵盖广泛技术复杂性的领域：软件开发、运维（MLOps）、经典ML、前沿深度学习研究、硬件优化……

如果你试图涵盖所有内容，你会失去焦点，过于浮于表面。了解机器学习（ML）中的某些东西意味着你亲自实现了它。至此为止。

跟上深度学习（DL）的最新进展是很棒的，例如。但要以原则性的方式进行。设定明确的目标（例如，我想成为Transformer模型的专家），并为自己制定实现该目标的路径，选择相关的论文、库、网络研讨会，甚至会议。

从一个话题跳到另一个话题虽然让你忙碌，但不会让你集中注意力。保持谦虚。从小而专注的地方开始。一旦到达那里，迈出下一步，征服另一个领域。

## 结论

征服你的恐惧是一项每日（全职）的工作。不仅在机器学习中，而是在你生活的每一个希望成长并在明天变得更好的方面。

[原文](http://datamachines.xyz/2021/09/22/how-to-defeat-the-ml-engineer-impostor-syndrome/)。经许可转载。

**简介：** [保·拉巴塔·巴霍](http://datamachines.xyz/)（[@paulabartabajo_](https://twitter.com/paulabartabajo_)) 是一位数学家和AI/ML自由职业者及演讲者，拥有超过10年的经验，为各种问题，包括金融交易、移动游戏、在线购物和医疗保健，处理数字和模型。

**相关内容：**

+   [忙碌时数据专业人士如何打动他人](https://www.kdnuggets.com/2021/10/data-professionals-impress-busy.html)

+   [2022 年最实用的数据科学技能](https://www.kdnuggets.com/2021/10/11-most-practical-data-science-skills-2022.html)

+   [避免这五种让你看起来像数据小白的行为](https://www.kdnuggets.com/2021/10/avoid-five-behaviors-data-novice.html)

### 更多相关话题

+   [通过声明式机器学习从工程师转型为机器学习工程师](https://www.kdnuggets.com/2023/05/predibase-go-engineer-ml-engineer-declarative-ml.html)

+   [每位机器学习工程师应该掌握的五项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [如何成为一名机器学习工程师](https://www.kdnuggets.com/2022/05/become-machine-learning-engineer.html)

+   [每个工程师都应该且能够学习机器学习](https://www.kdnuggets.com/2022/06/corise-every-engineer-learn-machine-learning.html)

+   [为什么 Emily Ekdahl 选择 co:rise 来提升她作为…的工作表现](https://www.kdnuggets.com/2022/08/corise-emily-ekdahl-chose-corise-level-job-performance-machine-learning-engineer.html)

+   [机器学习工程师的一天](https://www.kdnuggets.com/2022/10/day-life-machine-learning-engineer.html)
