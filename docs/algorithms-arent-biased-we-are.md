# 算法并不偏见，我们才是

> 原文：[`www.kdnuggets.com/2019/01/algorithms-arent-biased-we-are.html`](https://www.kdnuggets.com/2019/01/algorithms-arent-biased-we-are.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：[Rahul Bhargava](https://www.media.mit.edu/people/rahulb/overview/)，麻省理工学院**。

对使用 AI 改善你组织的运营感到兴奋？对计算机模型带来的见解和预测充满好奇？我想提醒你关于偏见及其如何在这些类型的项目中出现，分享一些说明性的例子，并翻译最新的关于“算法偏见”的学术研究。

首先——语言很重要。我们称呼事物的方式影响我们的理解。这就是为什么**我尽量避免使用“人工智能”这一受炒作驱动的术语**。大多数被称为人工智能的项目更恰当地描述为“机器学习”。机器学习可以描述为训练计算机做出你希望它帮助做出的决策的过程。本文描述了为什么你需要关注你机器学习问题中的数据。

这在很多方面都很重要。现在“算法偏见”这个词在新闻中频繁出现。这个术语是什么意思？算法[给法官提供带有歧视性的判决建议](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing)。算法正在[将性别刻板印象融入翻译服务](http://mashable.com/2017/11/30/google-translate-sexism/)。算法在[推动观众观看极端主义视频](https://twitter.com/zeynep/status/917370470579822598)。我认识的大多数人都认为这不是我们想要的世界。让我们深入探讨一下为什么会发生这种情况，并将责任归咎于应有的地方。

### 你的机器在学习，但谁在教它？

物理学对我来说很困难。更糟糕的是——我认为我*永远*不会擅长物理。我把这一点归咎于一个糟糕的高中物理老师，他对我和其他学生都很傲慢。另一方面，虽然我对复杂的数学不太擅长，但我喜欢尝试更好地学习它。我将这种持续的热情归因于我的初中数学老师，他以兴奋和玩耍的方式向我们介绍了这个主题（包括解决额外问题的甜甜圈奖励！）。

我分享这个故事的观点是什么？教师很重要。在机器学习中尤为如此——机器没有先前的经验、背景信念和所有其他使人类学习者得到重视的因素。机器只能从你展示给它的内容中学习。

**所以在机器学习中，重要的问题是“教材是什么”和“老师是谁。”** 机器学习中的教材是你展示给软件的“训练数据”，以教它如何做出决策。这通常是你已经检查并标记了你想要的答案的一些数据。通常这些数据是你从很多其他来源收集的，这些来源已经完成了这项工作（我们通常称之为“语料库”）。如果你试图预测一个接受微贷款的人还款的可能性，那么你可能会选择包括当前贷款接受者的历史支付记录的训练数据。

第二部分讲的是老师是谁。老师决定提问的问题，并告诉学习者什么是重要的。在机器学习中，老师负责“特征选择”——决定机器可以使用数据中的哪些部分来做出决策。有时，这种特征选择是通过你拥有的训练集的内容来完成的。更多时候，你使用一些统计数据来让计算机选择最有可能有用的特征。回到我们的微贷款例子：一些候选特征可能是贷款期限、总金额、接受者是否有手机、婚姻状态或种族。

这两个问题——训练数据和训练特征——是任何机器学习项目的核心。

### 算法是镜子

带着这个意识回到语言的问题上，也许“机器教学”会是一个更有用的术语。这将把责任放在老师身上。如果你在做“机器学习”，你最关心的是它学会做什么。**而“机器教学”中，你最关心的是你在教机器做什么**。这是语言上的微妙差异，但理解上有很大不同。

将责任放在老师身上，帮助我们意识到这个过程的复杂性。记得我开始时列出的那些偏见例子吗？那个判刑算法存在歧视，因为它是用美国法院系统的判刑数据训练的，这些数据表明对除了黑人男性以外的每个人都非常宽容。那个带有性别刻板印象的翻译算法可能是用新闻或文学中的数据训练的，而这些数据包含了过时的性别角色和规范（即医生是“他”，而护士是“她”）。那个在你的信息流中出现虚假故事的算法是被训练去分享其他人分享的内容，不管其准确性如何。

所有这些数据都是关于我们的。

**那些算法没有偏见，是我们有！算法是镜子。**

![](img/07623c5d75a367f0c5d8fc2bcaa5bea5.png)

**算法镜子无法完全反射我们周围的世界，也无法反射我们想要的世界**

它们反映了我们问题和数据中的偏见。这些偏见会在特征选择和训练数据中被融入机器学习项目中。这是我们的责任，而不是计算机的。

### 矫正镜片

那么我们如何检测和纠正这些问题呢？教师对学生的学习感到责任和自豪。机器学习模型的开发者也应该感受到类似的责任，或许也应该有类似的自豪感。

我对像[微软努力消除公共可用语言模型中的性别偏见](https://www.technologyreview.com/s/602025/how-vector-space-mathematics-reveals-the-hidden-sexism-in-language/)（尝试解决“医生是男性”问题）这样的例子感到振奋。我喜欢我的同事 Joy Buolamwini 将其重新框架为她称之为“[算法正义联盟](https://www.ajlunited.org/)”的社会和技术干预中的“正义”问题的努力（[视频](https://www.ted.com/talks/joy_buolamwini_how_i_m_fighting_bias_in_algorithms)）。[ProPublica](https://www.propublica.org/)的调查报道让公司对其歧视性量刑预测负责。令人惊叹的[Zeynep Tufekci](https://www.ted.com/talks/zeynep_tufekci_we_re_building_a_dystopia_just_to_make_people_click_on_ads)在谈论和写作社会面临的危险方面走在前沿。Cathy O’Neil 的[数学毁灭武器](https://weaponsofmathdestructionbook.com/)记录了这一切的诸多影响，对社会发出了警告。像[法律领域正在讨论的影响](https://scholarship.law.duke.edu/cgi/viewcontent.cgi?referer=&httpsredir=1&article=1315&context=dltr)一样，算法驱动的决策在公共政策设置中的影响也在讨论中。[城市条例](https://www.propublica.org/article/new-york-city-moves-to-create-accountability-for-algorithms)也开始处理如何立法应对我描述的一些影响的问题。

这些努力希望能作为这些算法镜子的“校正镜片”——解决我们在自我反射中看到的令人担忧的方面。关键在于记住，我们必须采取行动解决这个问题。**通过算法做出的决策并不会自动使其可靠和可信；就像用数据量化某事并不会自动使其成为事实一样**。我们需要审视这些算法镜中的自我反射，确保看到我们想要的未来。

**个人简介**：[Rahul Bhargava](https://www.media.mit.edu/people/rahulb/overview/)是专注于公民技术和数据素养的研究员和技术专家。

[原文](https://medium.com/mit-media-lab/the-algorithms-arent-biased-we-are-a691f5f6f6f2)。经许可转载。

**资源：**

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [数据科学与伦理 - 为什么公司需要新的首席伦理官 (Chief Ethics Officer)](https://www.kdnuggets.com/2019/01/data-science-ethics.html)

+   [2018 年 NLP 的 10 个令人兴奋的想法](https://www.kdnuggets.com/2019/01/10-exciting-ideas-2018-nlp.html)

+   [可解释的 AI 和机器学习的案例](https://www.kdnuggets.com/2018/12/explainable-ai-machine-learning.html)

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 需求

* * *

### 更多相关主题

+   [事物并非总是正态分布：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

+   [基础机器学习算法：初学者指南](https://www.kdnuggets.com/2021/05/essential-machine-learning-algorithms-beginners.html)

+   [分类的机器学习算法](https://www.kdnuggets.com/2022/03/machine-learning-algorithms-classification.html)

+   [流行的机器学习算法](https://www.kdnuggets.com/2022/05/popular-machine-learning-algorithms.html)

+   [机器学习中主要的监督学习算法](https://www.kdnuggets.com/2022/06/primary-supervised-learning-algorithms-used-machine-learning.html)

+   [免费的 Python 算法课程](https://www.kdnuggets.com/2022/09/free-algorithms-python-course.html)
