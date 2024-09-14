# 数据科学对我们心理发展的作用

> 原文：[https://www.kdnuggets.com/2019/02/data-science-mental-development.html](https://www.kdnuggets.com/2019/02/data-science-mental-development.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Syed Sadat Nazrul](https://www.linkedin.com/in/snazrul1/)，分析科学家**

![Header image](../Images/df9d3d5ee06c36c58db2b4c41c46ef1e.png)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT 需求。

* * *

情感是人类社会的基本元素。如果你仔细考虑，任何值得分析的事物都受到人类行为的影响。网络攻击受到不满员工的高度影响，他们可能会忽视应有的谨慎，或从事 [内部滥用](https://www.ca.com/content/dam/ca/us/files/ebook/insider-threat-report.pdf)。股市依赖于经济气候的影响，而经济气候本身取决于大众的整体行为。在沟通领域，众所周知 [我们所说的只占信息的7%，其余93%则编码在面部表情和其他非言语线索中](http://www.nonverbalgroup.com/2011/08/how-much-of-communication-is-really-nonverbal)。整个心理学和行为经济学领域都致力于这一领域。尽管如此，有效测量和分析情感的能力将使我们以显著的方式改善社会。例如，加州大学旧金山分校的心理学教授 [保罗·艾克曼](https://psychology.berkeley.edu/people/paul-ekman) 在他的书中，[*撒谎：市场、政治和婚姻中的欺骗线索*](https://www.amazon.com/Telling-Lies-Marketplace-Politics-Marriage-ebook/dp/B00ECXIF6C) 描述了如何通过面部表情来帮助心理学家发现潜在的自杀意图，即使患者在隐瞒这种意图。这听起来像是面部识别模型的工作？神经映射呢？我们能否有效地从神经冲动中映射情感状态？改善认知能力又如何？甚至是情感智力和有效沟通？利用我们拥有的大量非结构化数据来解决世界上许多问题是大有作为的。

尽管如此，像所有数据科学问题一样，我们需要深入探讨建模情感的核心挑战：

+   如何框定问题？我们的模型类别应是什么？我们优化的目标是什么？

+   我们应该收集哪些数据？我们在寻找哪些相关性？哪些方面需要更深入地探讨？

+   获取此类数据是否存在任何问题？社会和文化对获取情感数据的看法是什么？需要遵守哪些隐私法规？数据安全又如何保障？

想了解如何有效设计AI产品，请阅读我的[*数据科学家和AI产品UX设计指南*](https://towardsdatascience.com/ux-design-guide-for-data-scientists-and-ai-products-465d32d939b0)。在这篇博客中，我旨在概述AI如何帮助我们未来的心理发展，并讨论一些当前的解决方案。

### 医疗保健

患者对医生撒谎并不少见。男性和女性的信任障碍源于尴尬和与医生接触时间太少。一项数字健康平台的研究， [ZocDoc](https://www.zocdoc.com/about/news/new-zocdoc-study-reveals-women-are-more-likely-than-men-to-lie-to-doctors/)，揭示了近一半（46%）的美国人因为尴尬或害怕被评判而避免告诉医生健康问题。约三分之一的人表示，他们因无法找到合适的机会或在预约期间时间不足（27%），或因医生没有提问或特别询问是否有困扰他们的事（32%）而隐瞒了细节。这对自杀领域产生了重大影响。根据[世界卫生组织](http://www.who.int/news-room/fact-sheets/detail/depression)（WHO）的数据，每年多达80万人死于自杀，60%的人面临严重的[抑郁症](https://www.verywellmind.com/suicide-rates-overstated-in-people-with-depression-2330503)。尽管抑郁症使患者更容易进行自杀行为，但区分自杀性抑郁症和普通抑郁症并不容易。

[Deena Zaidi](https://hackernoon.com/@deenazaidi) 在她的博客中，[*机器学习利用面部表情区分抑郁症和自杀行为*](https://hackernoon.com/machine-learning-uses-facial-expressions-to-distinguish-between-depression-and-suicidal-behavior-c9e1cd095026)*, *描述了一位自杀专家如何通过对风险因素的深入评估，准确预测患者未来的自杀想法和行为，与不了解患者的人的准确性相当。这与根据抛硬币做决定没有区别。虽然使用监督学习模型读取面部表情的技术仍在开发中，但这一领域已显示出很大的潜力。

![](../Images/80f5b8fa390ab4d8790e4ff5975e43bc.png)

从SVM结果分析的杜氏（上）与非杜氏（下）笑容有助于检测自杀风险（来源： [Laksana等人研究自杀意念的面部行为指标](https://www.extremetech.com/wp-content/uploads/2017/06/fg2017-submitted.pdf)）。

[一份报告](https://www.extremetech.com/wp-content/uploads/2017/06/fg2017-submitted.pdf)，与南加州大学、卡内基梅隆大学和辛辛那提儿童医院医疗中心的科学家合作撰写，研究了非语言面部行为以检测自杀风险，并声称发现了一种区分抑郁症和自杀患者的模式。使用SVM，他们发现面部行为描述符如涉及眼轮匝肌收缩的微笑百分比（杜氏笑容）在自杀和非自杀群体之间具有统计学意义。

### 认知能力

认知能力是我们进行从最简单到最复杂任务所需的脑基技能。它们与我们如何学习、记忆、解决问题和集中注意力的机制关系更大，而不是与任何实际知识有关。人们都希望提高认知能力。谁不希望更好地记住名字和面孔，更快地掌握困难的抽象概念，并且能更好地“看到联系”呢？

![](../Images/fd78d7103a7b8503d948bad53a2f1926.png)

[Elevate](https://itunes.apple.com/us/app/elevate-brain-training/id875063456?mt=8) 应用程序在 Apple Store

目前，有一些应用程序可以帮助我们训练认知能力。其中一个例子是 [Elevate](https://itunes.apple.com/us/app/elevate-brain-training/id875063456?mt=8)，它由脑力游戏组成，用户可以在合适的难度级别上玩，以提高心理数学、阅读和批判性思维能力。 [最佳认知功能](http://www.nickbostrom.com/posthuman.pdf) 的价值是如此显而易见，以至于详细说明可能是多余的。我们不断推陈出新，以超越我们的五感来更深刻地理解周围的世界。例如，在图像识别领域，AI已经能够 [“看”得比我们更清楚](https://www.entrepreneur.com/article/283990)，通过观察远超RGB光谱的变量，从而帮助我们突破自身的视觉限制。然而，当我们可以进入虚拟世界时，为什么要局限于二维屏幕来可视化三维物体呢？

![](../Images/189abc95009d1c70dd81f1b03638cc2d.png)

[Nanome.AI](https://nanome.ai/) 开发了用于分析抽象分子结构的增强现实技术

增强现实让我们感觉仿佛已经传送到了另一个世界。当我想到这个问题时，计算材料科学和生物学是我立即想到的领域。作为过去的一名计算材料科学家，我知道可视化复杂的分子结构对于许多研究人员来说是一项挑战。[Nanome.AI](https://nanome.ai/)帮助在增强现实中可视化这些复杂的结构。更进一步，已经有很多初创公司在[解剖学领域使用增强现实培训外科医生](https://www.nanalyze.com/2018/09/startups-augmented-reality-surgery/)。

![](../Images/b0698efd84b17d581597eacedc281310.png)

[平行坐标](https://datavizcatalogue.com/methods/parallel_coordinates.html)图可视化7维空间

数据可视化和降维算法的新术语不断出现，帮助我们更好地体验周围的世界。例如，我们有了[平行坐标](https://datavizcatalogue.com/methods/parallel_coordinates.html)，它允许我们在高维空间中进行可视化和过滤，而[t-SNE](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding)则因能够将复杂空间降维到二维或三维空间而广受欢迎。

### 情商

情商是意识到、控制和表达自身情绪的能力，以及审慎和富有同理心地处理人际关系的能力。每个人都会经历情绪，但只有少数人能够准确识别它们的发生。这可能是因为缺乏自我意识，或者只是我们有限的情感词汇。很多时候，我们甚至不知道自己想要什么。我们努力以特定的方式与周围的人建立联系，或消费某种产品，只是为了体验一种非常独特的情感，而我们无法描述它。我们的感受远不止于幸福、悲伤、愤怒、焦虑或恐惧。我们的情绪是上述所有情绪的复杂组合。理解自身情绪以及他人情绪的能力对情感健康和维持积极关系至关重要。

![](../Images/705c2e32bad27b660a4a227b393d9b45.png)

大脑活动的分布模式预测了使用fMRI扫描检测到的离散情绪的体验，顶部显示活动模式，底部显示灵敏度范围（来源：[解码人脑中的自发情绪状态，Kragal等](https://www.researchgate.net/publication/308122235_Decoding_Spontaneous_Emotional_States_in_the_Human_Brain)）

随着[神经映射的创新](https://www.researchgate.net/publication/308122235_Decoding_Spontaneous_Emotional_States_in_the_Human_Brain)，我们将更好地理解作为人类的自我以及我们能够达到的各种情感状态。监督学习已经提供了一些常见情感的识别。通过对脑波进行无监督学习，我们可能会更好地理解复杂的情感模式。例如，一个简单的异常检测算法或许可以揭示新的情感模式或需要关注的显著情感压力点。这些研究有可能揭示改进我们情感智力的创新方法。

![](../Images/c73e83c028e928d576b1e98315a60b47.png)

准确解读微表情如何有助于在商业交易中进行谈判的演示（来源：[TED演讲：如何通过身体语言和微表情预测成功 — Patryk & Kasia Wezowski](https://www.youtube.com/watch?v=CWry8xRTwpo)）

即使是帮助预防自杀的监督图像识别模型也可以让个人读取与他们交谈者的情感。例如，[一个关于微表情的TED演讲](https://www.youtube.com/watch?v=CWry8xRTwpo)展示了如何通过注意某些面部表情来估算商业谈判中的理想价格点。该TED演讲还提到，能够更好地解读微表情的销售人员比无法做到这一点的销售人员多销售20%。因此，这20%的优势可能通过投资于能够揭示谈话者情感的眼镜来实现。

情感智力包括理解我们自己的情感以及对周围人的情感更为敏感。通过对我们情感状态的深入研究，我们或许可以发现一些我们从未体验过的新情感。在正确的手中，AI可以作为我们的延伸，帮助我们与生活中珍视的人建立有意义的联系。

### 体验与想象力

想象力是形成新想法、图像或对外部对象的概念的能力或行为，这些对象当前不在感官范围内。AI对我们体验和想象力的影响将源于更好的认知能力和情感智力的综合。简单来说，接触到更丰富的认知能力和情感智力将使我们能够体验到今天普通思维无法轻易构思的想法。

牛津大学的哲学教授，[尼克·博斯特罗姆](https://nickbostrom.com/)，在他的论文《[*当我长大后为什么想成为超人类*](https://nickbostrom.com/posthuman.pdf)》中描述了如何通过新体验模式来提升我们的体验和想象力。假设我们今天的体验模式被表示在空间X中。10年后，假设这些体验模式被表示在空间Y中。空间Y将比空间X大得多。这个未来的空间Y可能拥有我们传统的快乐、悲伤和愤怒之外的新情感。这种新空间Y甚至可以让我们更准确地理解反映我们想表达的抽象思想。我们每个人都有可能以[文森特·梵高](https://en.wikipedia.org/wiki/Vincent_van_Gogh)在最狂野的想象中也无法想象的方式来看待世界！

Y的新领域实际上可以解锁超越我们当前想象的新世界。未来的人们将以比我们今天更丰富的方式思考、感受和体验世界。

### 沟通

当你缺乏情感智力时，很难理解自己给他人的印象。你会感到被误解，因为你传达信息的方式别人无法理解。即使经过练习，情感智能高的人也知道他们并不能完美地沟通每一个想法。人工智能有可能通过增强自我表达来改变这一点。

![](../Images/ca5bbaf1056286f2e159b95e48b896fe.png)

谷歌眼镜可以将德语翻译成英语（来源：[谷歌收购Word Lens制造商以提升翻译功能](https://www.cnet.com/news/google-buys-word-lens-maker-to-boost-translate/)）

10年前，我们的大多数沟通仅限于电话和电子邮件。今天，我们可以使用视频会议、增强现实以及社交媒体上的各种应用程序。随着我们认知能力和情感智力的提升，我们可以通过更高分辨率和更低抽象级别的成语来表达自己。[谷歌眼镜可以即时翻译外语文本](https://www.cnet.com/news/google-buys-word-lens-maker-to-boost-translate/)。我已经在早些时候提到过谷歌眼镜可能用于阅读微表情。然而，为什么要将我们的沟通仅限于我们可以“看到”的内容呢？

![](../Images/50af65fe6816e59dc842da615ca01abc.png)

通过向头戴设备传送电脉冲来控制无人机（来源：[脑控无人机比赛：佛罗里达大学举办独特的无人机比赛](https://www.rt.com/usa/340654-drone-brain-race-mind/)）

佛罗里达大学的学生实现了[仅用意念控制无人机](https://www.eng.ufl.edu/newengineer/news/mind-controlled-drones-race-to-the-future/)。我们甚至可以使用振动游戏机来利用触觉，使得《马里奥卡丁车》游戏更加逼真。现今的增强现实仅限于我们的视觉和听觉。在未来，增强现实可能会使我们能够嗅觉、味觉和触摸我们的虚拟环境。除了接触我们的五感外，我们对某些情境的情感反应可能还会通过AI的力量进行微调和优化。这可能意味着在*《灵异鬼照片》*中分享主角的恐惧，感受*《艾玛》*中的心碎，或对*《宝可梦》*的冒险感到兴奋。

### 一些潜在的关注点

虽然我已经列举了利用AI读取情感、帮助我们理解自己和周围人的所有积极面，但我们不能忽视潜在的挑战：

+   **数据安全**：根据[世界隐私论坛](https://www.csoonline.com/article/2882052/data-breach/health-data-breaches-could-be-expensive-and-deadly.html)的报告，被盗医疗凭证的街头价值大约为50美元，而被盗信用卡信息或社会保障号码的街头价值仅为1美元。同样，心理健康信息也是敏感的个性化数据，可能被黑客利用。正如黑客们试图窃取我们的信用卡和健康保险信息一样，访问情感数据在黑市上也可能有很高的回报。

+   **政府数据法规**：对于任何高度敏感的个性化数据，不同国家有不同的法规需要遵守。在美国，医疗相关数据需要遵守[HIPAA](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html)法规，而财务应用相关的数据则需要遵守[PCI](https://www.pcisecuritystandards.org/)标准。如果我们放眼全球，欧盟有[GDPR](https://www.insideprivacy.com/international/china/china-issues-new-personal-information-protection-standard/)，中国则有[SAC](https://www.insideprivacy.com/international/china/china-issues-new-personal-information-protection-standard/)。

+   **伦理边界**：与任何新技术一样，社会可能对其情感数据的访问感到不快。让我们面对现实吧。我们可能对医生检查我们的情感数据以改善健康状况表示接受，但对保险公司试图收取更高保费则不太能接受。同样，我们曾经对[美国通过操控大众心理进行选举舞弊的怀疑](https://www.theguardian.com/uk-news/2018/mar/26/cambridge-analytica-trump-campaign-us-election-laws)。然而，伦理规范在很大程度上取决于给定社会的“正常”标准。某些目前不可接受的东西在未来可能会被接受。尽管数据科学在某些领域的应用可能让公众感到不适，但如防止信用欺诈的欺诈分析、反洗钱以及像亚马逊和Netflix这样的推荐系统的市场分析等其他应用则完全可以接受。在引入新想法时，社会接受度将高度依赖于所收集数据用于解决的问题类型。有关AI产品开发的更多细节，请查看我关于[*数据科学家和AI产品的UX设计指南*](https://towardsdatascience.com/ux-design-guide-for-data-scientists-and-ai-products-465d32d939b0)*的博客*。

**简介： [Syed Sadat Nazrul](https://www.linkedin.com/in/snazrul1/)** 白天使用机器学习追捕网络和金融犯罪分子，夜晚则撰写有趣的博客。

[原文](https://towardsdatascience.com/data-science-for-our-mental-development-3fccbd29eff2)。经授权转载。

**相关：**

+   [接收者操作特性曲线解析（Python版）](/2018/07/receiver-operating-characteristic-curves-demystified-python.html)

+   [数据科学面试指南](/2018/04/data-science-interview-guide.html)

+   [数据科学家的DevOps：驯服独角兽](/2018/07/devops-data-scientists-taming-unicorn.html)

### 更多相关话题

+   [AIoT革命：AI和物联网如何改变我们的世界](https://www.kdnuggets.com/2022/07/aiot-revolution-ai-iot-transforming-world.html)

+   [一些强力的提示工程技巧，以提升我们的LLM模型](https://www.kdnuggets.com/some-kick-ass-prompt-engineering-techniques-to-boost-our-llm-models)

+   [KDnuggets新闻，7月27日：AIoT革命：AI和物联网如何……](https://www.kdnuggets.com/2022/n30.html)

+   [数据科学如何改变移动应用开发？](https://www.kdnuggets.com/2023/03/data-science-transform-mobile-app-development.html)

+   [文本摘要开发：使用GPT-3.5的Python教程](https://www.kdnuggets.com/2023/04/text-summarization-development-python-tutorial-gpt35.html)

+   [开放助手：探索开放和协作的可能性……](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)
