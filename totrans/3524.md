# 深度学习能帮助找到完美的约会对象吗？

> 原文：[https://www.kdnuggets.com/2015/07/can-deep-learning-help-find-perfect-girl.html](https://www.kdnuggets.com/2015/07/can-deep-learning-help-find-perfect-girl.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)**由 Harm de Vries 提供**

![在线约会](../Images/cbc2001f70f00d64c7689314473cc48c.png)我搬到蒙特利尔后做的第一件事就是安装[Tinder](http://www.tinder.com/)。对于那些不熟悉在线约会市场的人，Tinder 是一个展示附近用户的约会应用，你可以通过他们的个人资料图片来喜欢或不喜欢他们。在使用这个应用一段时间后，我发现尽管我始终不喜欢有很多穿孔和纹身的女孩（无意冒犯，只是我的类型），但这个应用一直在向我展示这些个人资料。Tinder 没有利用我的滑动历史来了解我喜欢哪种类型的女孩，这让我感到惊讶。

这个观察让我思考：计算机能否学会我对哪类女孩有吸引力？我通过对 Tinder 上超过 9000 张个人资料图片进行标记，并利用[深度学习](http://www.wired.com/2015/05/remaking-google-facebook-deep-learning-tackles-robotics/)，即人工智能领域的最新革命，来进行尝试。在这篇博客文章中，我将提供一个高层次的视角，介绍我如何使用这些技术来预测我的 Tinder 滑动。你可以在[这篇论文](http://arxiv.org/abs/1505.00359)中找到技术细节，该论文已被接受用于[ICML 深度学习研讨会](https://sites.google.com/site/deeplearning2015/)。

![](../Images/456267832e9db367141a6b18f9d43ed0.png)

### 机器学习

计算机已经存在了几十年，但仍然存在许多最强大的计算机难以解决的问题，而我们（作为人类）可以轻松完成。你可以想到检测图片中的面孔、识别语音和翻译文本。问题是，尽管我们能轻松完成这些任务，但我们无法解释我们是如何做到的。这意味着，我们不能提出一套可以转换为计算机程序的规则。

从个人资料图片中预测吸引力是这样一个任务吗？是的，确实很难指定一套规则来定义你被吸引的对象。对你们中的一些人来说，这可能听起来有悖直觉，因为你可以指定一个明确的特征列表，比如蓝眼睛、金发等。然而，即使你可以这样做，你也得相信，构造一个能在图片中检测这些特征的程序是非常复杂的。

另一方面，如果我给你一张女孩/男孩的照片，你可以在一瞬间决定你是否喜欢她/他。这提示了一种更好的策略：我们让计算机通过展示你喜欢或不喜欢的女孩的个人资料图片来自我学习。我们称之为*机器学习*，因为程序从这些个人资料图片中学习一套吸引力规则。

![](../Images/1a367f14313bf3f7f699572667d32b0a.png)

### 深度学习

[深度学习](http://www.economist.com/news/briefing/21650526-artificial-intelligence-scares-peopleexcessively-so-rise-machines) 是一种特定类别的学习算法，最近在[图像识别](http://www-etud.iro.umontreal.ca/~devries/perfect-girl.html)（以及许多其他人工智能问题）方面取得了突破。如果你想亲自看看这些技术现在的效果，我鼓励你上传一张照片到

+   [how-old.net](http://how-old.net/) 用于估算图片中人物的年龄

+   [imageidentify.com](http://imageidentify.com/) 用于检测图片中的内容

在深度学习中，程序的结构是一个人工神经网络，其灵感来源于生物神经网络，即大脑。当神经网络由多个层的连接神经元组成时，我们称其为*深度*。每个神经元与上一层的一组神经元相连。这些连接的强度（如红色所示）决定了神经元对哪些图像模式做出反应。你可以将其视为一个可调节的滤波器，仅在看到特定模式时才会做出反应。

网络的连接强度是随机初始化的，这意味着所有神经元只是随机滤波器。因此，当我们将图像输入网络（通过输入其像素的强度）时，第一层中的只有几个神经元会做出反应，这反过来会使下一层中的一些随机神经元变得活跃（以此类推直到输出）。因此，网络的输出将是随机的喜欢或不喜欢。

因此，为了做出准确的预测，我们需要对网络进行*训练*。我们通过输入一张个人资料照片并将其传递通过网络以获得输出。如果网络的预测与期望的输出（喜欢或不喜欢）不同，我们会调整其滤波器（即其连接强度），以使预测更准确。如何具体调整滤波器是相当困难的，超出了这篇博客的范围，你只需要知道我们可以做到这一点。训练阶段就是重复这个过程许多次，然后希望网络对它尚未见过的个人资料照片进行准确预测。

训练后，我们通常观察到第一层的神经元对边缘和颜色斑点做出反应，而下一层利用这些观察来检测更复杂的模式，例如眼睛和耳朵。最后一层可能会检测到完整的面孔或穿孔和纹身（这对于预测我的偏好非常重要）。

参见后续内容: [**深度学习能帮助你找到完美女孩吗？ – 第二部分。**](/2015/07/can-deep-learning-help-find-perfect-girl-2.html)

[![harm](../Images/a2f74a8aafd2b288dfce247857ef42e7.png)Harm de Vries](http://www-etud.iro.umontreal.ca/~devries/) 是一名博士生，由 Aaron Courville 和 Roland Memisevic 监督，并且是由 Yoshua Bengio 领导的蒙特利尔学习算法研究所的成员。他的主要兴趣在于深度学习的理论和应用。

[原始资料](http://www-etud.iro.umontreal.ca/~devries/perfect-girl.html) 作者 Harm de Vries。

**相关：**

+   [爱、性与预测分析](/2015/06/love-sex-predictive-analytics.html)

+   [Thomas Levi，PlentyOfFish 讲述大数据对浪漫的启示](/2014/07/interview-thomas-levi-pof-big-data-romance.html)

+   [漫画：人类在深度学习上仍然领先](/2015/07/cartoon-humans-ahead-deep-learning.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

### 更多相关话题

+   [停止学习数据科学，寻找目的并发现目的……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [揭开选择完美机器学习算法的秘密！](https://www.kdnuggets.com/2023/07/ml-algorithm-choose.html)

+   [等级系统如何帮助预测 AI 成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [数据科学项目，助你解决现实世界问题](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [数据访问在大多数公司中严重不足，71% 认为……](https://www.kdnuggets.com/2023/07/mostly-data-access-severely-lacking-synthetic-data-help.html)

+   [SAS 如何帮助推动从业者的职业发展](https://www.kdnuggets.com/2023/07/sas-help-catapult-practitioners-careers.html)
