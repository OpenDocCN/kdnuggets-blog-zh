# 机器学习初创企业的机会在哪里？

> 原文：[https://www.kdnuggets.com/2016/06/opportunites-machine-learning-startups.html](https://www.kdnuggets.com/2016/06/opportunites-machine-learning-startups.html)

**作者：Libby Kinsey，风险投资人**。

机器学习和人工智能在数据驱动的业务中迅速普及，也就是说，很多企业都在使用。这里我选择了几个可能还未被大公司占领的领域。这并非全新领域——如果我能想到下一个杀手级应用，我会尝试去实现它！

### ‘工具和镐’玩法

这种说法源于加州淘金热，当时售卖工具的商人赚得盆满钵满（而淘金者的结果则各异），机器智能的“工具”则是硬件、数据流，以及（可以说是）算法本身。

1.  令人惊讶的是，机器智能的算法开发绝大多数都是开源的。当然也有例外——去年，牛津大学申请了一项高效的替代反向传播的专利，即[反馈对齐算法](http://isis-innovation.com/wp-content/uploads/2014/05/insights75.pdf)（第14页）——我想知道他们打算如何将其商业化？那些使人们能够轻松使用学习算法的高质量SaaS服务将会找到热切的客户，而[MetaMind](https://www.metamind.io/)将尖端深度学习带到你的数据集中，就是一个拥有所有正确资质的例子。我还喜欢另一个项目，[自动统计学家](http://www.automaticstatistician.com/)，它通过贝叶斯推断搜索模型，以发现最适合你数据的解释。[好奇AI公司](http://www.thecuriousaicompany.com/)，一家通用人工智能公司，其首个项目是废物分类（乍一看虽不吸引人但利润丰厚），[据报道](http://techcrunch.com/2015/10/02/do-androids-dream-of-curious/)计划将其AI软件作为工具包销售。

1.  大公司可以访问大量的数据集，并可以获得更多（如，[IBM最近收购《天气频道》数据资产，价值15亿美元](http://www.infoworld.com/article/2999630/big-data/ibm-acquires-the-weather-channel-when-technology-vendors-become-data-vendors.html)）。但迄今为止的重点还是在低挂的果实上，比如社交或电子商务数据，因此在获取和/或标注数据更困难的地方仍有机会。[Affectiva](http://www.affectiva.com/)的面部情感反应数据库、[Pallas Ludens](http://pallas-ludens.com/)（端到端数据注释服务）和[opensensors.io](http://www.opensensors.io/)（为公共传感器数据来源增加价值）都属于这一类别。基因组和医学图像数据——由于一些棘手的隐私问题——将促进个性化治疗和护理，并改善诊断。顺便提一句，看看[Genomics England如何推进其行业合作](http://www.genomicsengland.co.uk/working-with-industry/)将会很有趣。

1.  在硬件方面，GPU实现了一些非凡的进展。（也有非常普通的进展——NVIDIA GeForce GTX Titan使我能够在音频中检测到蝙蝠叫声。）

![频谱图](../Images/e7e20c95a95f4d80941c1002fce35541.png)

*蝙蝠叫声的频谱图（下图）和卷积神经网络预测（上图）。*

但这些处理器是为图形设计的。高效学习和推理的下一个飞跃将来自专门为机器学习设计的处理器。[Graphcore](http://www.graphcore.ai/)称它们为智能处理器单元。与此同时，[Nervana Systems](http://www.nervanasys.com/)、[Teradeep](http://www.teradeep.com/)（Yann LeCun是顾问）和[Thinci](http://thinci.com/)正在打造自己的定制硬件。

将[Udacity](https://www.udacity.com/)、[Coursera](https://www.coursera.org/)和[Kaggle](https://www.kaggle.com/)等公司，包括在这个类别中是合理的，它们帮助教育或管理代码库和项目（例如，[Atlassian，准备上市](http://www.smh.com.au/business/markets/atlassian-ipo-the-line-in-the-prospectus-everyone-missed-20151115-gkzp0w.html)）。

### 利用情感

这是一个初创公司数量较少的应用领域。引用[MIT情感计算小组](http://affect.media.mit.edu/)的话：

> “情感是人类体验的基础，影响认知、感知以及日常任务，如学习、交流，甚至理性决策。然而，技术人员在很大程度上忽视了情感，给人们创造了一个常常令人沮丧的体验……”

![微表情检测器](../Images/be2352dacdbbc887995c3cee7617d91b.png)

*用于训练微表情检测器的图像（感谢JB！）*

第一个任务是训练模型识别人的情感。[Emotient](http://www.emotient.com/)、[RealEyes](http://www.realeyes.co/)和[Affectiva](http://www.affectiva.com/)都使用面部表情来推测情感，目前主要（看起来）用于市场营销。[Cogito Corp](http://www.cogitocorp.com/)和[Beyond Verbal](http://www.beyondverbal.com/)则集中于理解声音中的情感线索，以进行市场研究并提供更好的客户体验。

然后，建模情感行为，例如为了与人类自然互动。[Jibo](https://www.jibo.com/)，这个‘友好’的机器人，是一个有趣的例子，仅用‘眼睛’来表达自己。肯定会有便宜的玩具，既适应又响应（像[Paro](http://www.parorobots.com/)治疗用的海豹机器人，但用于玩耍），虽然我还没找到这些玩具。这些玩具有避免像Toy Talk和Mattel的‘[Hello BarbieTM](https://www.toytalk.com/product/hello-barbie/)’这样的对话玩具隐私问题的优势，至少在语音可以在本地处理而不是在云端之前。

其他应用包括个性化护理和教育、冲突解决与谈判培训，以及自适应游戏。这些任务似乎非常适合机器学习，因为情感体验是主观且变化多端的。

### 渗透专业领域

我将把是否机器智能会使我们所有人变得多余的猜测留给其他人，而是指出它当然有潜力在许多专业任务中协助人类（从而为客户提供更多选择和价值）。

这些技术可能会做什么呢？以法律行业为例。[Ravn Systems](http://www.ravn.co.uk/)自动化处理法律工作中的（重复、乏味的）文件审查；[Bitproof’s Peter](https://hirepeter.com/)是一个AI法律助手，可以请求签名、生成合同和公证文件；而[Premonition.ai](http://premonition.ai/)利用数据搜索司法中的无意识偏见（对于看过《傲骨贤妻》第一季第十集的观众，这并不新鲜！）。

类似的招聘、保险、财务管理等工具将使专业人员能更多地关注工作中更令人满意的方面，如行使判断、决策以及……客户娱乐。

### 革新医疗

![Pharm](../Images/f68df9a84f6f2ace35f722279a14be2d.png)

药物发现被认为是昂贵且风险大的，但如果可以利用数据减少风险，找到更好的开发目标呢？这正是[Stratified Medical](http://www.stratifiedmedical.com/)的假设，使用深度学习进行制药研发。

其他方面，[Enlitic](http://www.enlitic.com/) 和 [Zebra Medical](https://www.zebra-med.com/) 试图利用深度学习进行准确的诊断/决策支持工具，而 [Your.MD](http://www.your.md/) 与英国国家健康服务合作，通过应用程序提供个性化健康援助。

### 更好的搜索

“那部电影叫什么来着，那个我姐姐喜欢的德国演员出演的…跟外星人有关的…非常朋克的原声带？”

搜索需要能够处理不精确、主观和个人化的内容，就像人类一样。它需要帮助我们从所有噪音中发现和策划对我们相关的内容。这涉及到学习上下文和内容属性。实际上，这本身就是一个博客文章，但这里有几个例子：

![地图](../Images/19195c696c729936095345d513723e8b.png)

+   [Clarify](http://clarify.io/) 通过其API使音频和视频可搜索。它相当于扫描文本中的关键词以确定相关性，是一个节省时间的绝佳应用。

+   [Lumi](https://lumi.do/) 从你的浏览历史中学习你的喜好，以提供相关的、最新的内容。一个为无尽好奇心者服务的服务。

+   [Yossarian Lives](https://yossarianlives.com/) 是一个可以进行横向联系的搜索引擎，类似于人类，以助力创造力。

+   [EyeEm](http://www.eyeem.com/) 已将机器学习融入其摄影市场，允许无标签地搜索诸如‘愉快的’和‘雨天伦敦’等属性，而 [Cortexica](https://www.cortexica.com/) 和 [Sentient Technologies / Shoes.com](http://www.sentient.ai/news/shoes-com-sentient-technologies-unveil-visual-filter-the-worlds-first-ai-shopping-experience/) 利用相似性来优化产品搜索。

搜索相关性的一个关键方面无疑是‘可信度’，以便能够验证或评分社交媒体和新闻网站中的内容和声明。有人在做这个吗？

### 网络安全

机器学习在网络安全领域已经吸引了大量的风险投资（例如，[Lookout](https://www.lookout.com/) 收到2.82亿美元，[Vectra Networks](http://www.vectranetworks.com/) 收到7800万美元，[Darktrace](http://www.darktrace.com/) 收到4000万美元，[Cybereason](http://www.cybereason.com/) 收到8900万美元），但不断传来的坏消息（如最近的 [黑客事件](http://www.bbc.co.uk/news/uk-34611857)）显示市场还远未稳定。

就像任何‘热门’领域一样，很难区分多个表面上类似的初创公司。我确实需要在这个领域做更多的工作，并将密切关注 [Cyber London](https://cylonlab.com/)，这是一个网络安全初创公司的加速器。

应用领域如此之多，很难只关注几个。我错过了一些我最喜欢的公司，因为它们不符合这些类别中的任何一个，而且我在这篇文章发布后不可避免地会改变对类别的看法。尤其是因为我下周将参加[NIPS](https://nips.cc/)，我期待这次经历会让我大开眼界。 (*编者注：NIPS 并不是下周*）。

发展速度以及对新数据集的应用是机器智能如此令人兴奋的原因。尤其是在伦敦，目前确实有一种势头感，因为它靠近一系列世界级学术机构（帝国理工、UCL、牛津和剑桥），有一个成熟的初创生态系统（例如，像[Entrepreneur First](http://www.joinef.com/)这样的加速器积极吸引机器学习人才），以及目标客户中心的所在地——金融、法律和政府。

那么，请告诉我，我漏掉了什么？

**简介：[Libby Kinsey](https://uk.linkedin.com/in/libbykinsey)** 与英国的机器学习初创公司和风险投资公司合作。她在去年完成了 UCL 的机器学习硕士学位，此前花了约十年时间投资于高科技初创公司。

[原文](https://medium.com/@libbykinsey/where-are-the-opportunities-for-machine-learning-startups-cd5025f14d3c)。转载已获许可。

**相关内容：**

+   [微软正变成 M(ai)crosoft](/2016/04/microsoft-becoming-m-ai-crosoft.html)

+   [英特尔在认知技术上的投资：影响与新机遇](/2016/05/intel-investment-cognitive-tech-impact-new-opportunities.html)

+   [数据科学与认知计算：HPE Haven OnDemand 的简单推理与洞察之道](/2016/05/hpe-haven-ondemand-data-science-cognitive-computing.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 需求

* * *

### 更多相关话题

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [2022年值得关注的6家数据科学初创公司](https://www.kdnuggets.com/2022/02/6-data-science-startups-work-2022.html)

+   [印度十大人工智能初创公司](https://www.kdnuggets.com/top-10-ai-startups-to-work-for-in-india)

+   [6家通过 OpenUSD 和生成式 AI 重新定义 3D 工作流的初创公司](https://www.kdnuggets.com/6-startups-redefining-3d-workflows-with-openusd-and-generative-ai)

+   [每个机器学习工程师应该掌握的5项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [KDnuggets 新闻，12月14日：3门免费的机器学习课程](https://www.kdnuggets.com/2022/n48.html)
