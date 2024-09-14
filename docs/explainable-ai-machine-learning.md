# 《解释性人工智能与机器学习的案例》

> 原文：[https://www.kdnuggets.com/2018/12/explainable-ai-machine-learning.html](https://www.kdnuggets.com/2018/12/explainable-ai-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Katarina Athens-Miller、Anna Olecka 和 Jason Otte**

在最近的 KDnuggets 文章中 ***企业人工智能的圣杯******—******解释性人工智能*** [1]***，*** Genpact 的人工智能产品工程负责人 Saurabh Kaushik 写道：

> *可信赖的人工智能系统生产部署。是的，这确实是人工智能的圣杯，原因也很正当；无论是因为错误的流失预测导致失去高价值客户，还是因为金融交易分类错误而损失金钱。在现实中，客户较少关心人工智能模型的准确性，而更关注数据科学家无法解释的“我如何相信其决策？”*

在消费者领域构建信用风险模型的数据科学家面临着透明度的要求，这种要求可能存在于该领域的整个发展过程中，因为这受到了管理消费者风险的法规的约束。市场营销人员也受到某些规则的约束，这些规则不允许诸如性别或种族等受保护的类别进入模型。这些法规是在美国制定的，以保护消费者。在欧洲，这些要求甚至更严格。然而，对透明度的需求远远超出了消费者保护。

随着人工智能模型在工业物联网领域（如预测性维护）中的应用，使用这些模型结果的操作员要求提供警报的原因。如果他们需要对这些警报作出反应，尤其是在某些情况下需要快速反应，他们需要知道应该具体集中在哪里。

![机器学习业务](../Images/ae585158cf38d64f007dd91b116e45b6.png)

长期以来，人工智能社区内部存在一种不成文的共识，即最复杂的模型（如图像识别）应当免于解释性要求。然而，随着这些模型在广泛的社会环境中部署，如建筑安全或刑事司法系统，越来越清楚的是，这些模型往往建立在偏见数据上，因此会产生偏见结果。例如，几项研究表明，如果使用显著较少的深色皮肤个体的数据样本来训练面部识别算法，这些算法对肤色较深的个体的准确性较低。根据麻省理工学院媒体实验室的 Joy Buolamwini [2] & [3] 的说法，高达 35% 的深色皮肤女性可能会被人工智能误识别。而白人男性的错误率为 1%。

2016年10月，乔治敦大学法学院的隐私与技术中心发布了一项广泛的研究 *《永恒的排队 - 美国无监管的警察面部识别》* [4]。研究人员发现，执法中使用的面部识别软件存在系统性偏差。

> *警察面部识别将不成比例地影响非洲裔美国人。许多警察部门并未意识到这一点。在一份常见问题解答文件中，西雅图警察局表示其面部识别系统“看不到种族”。然而，一项由 FBI 共同撰写的研究表明，面部识别在黑人员工身上可能不够准确。此外，由于逮捕率不成比例地高，依赖于拘留照片数据库的系统可能包含了不成比例的非洲裔美国人。尽管有这些发现，但没有独立的测试机制来检测种族偏见的错误率。在采访中，两家主要的面部识别公司承认，他们没有在内部进行这些测试。*

![](../Images/ca3291cad6f745cb545cf39b0afbb0c3.png)

**[来源](https://medium.com/@BonsaiAI/what-do-we-want-from-explainable-ai-5ed12cb36c07)**

一起最近的事件中，亚马逊构建了一个 AI 工具来自动化招聘决策，几年后才发现该模型由于基础数据倾斜而产生了偏见的结果，这一事件在媒体上广泛报道。路透社 [5] 评论说“*该公司的实验，路透社首次报道，为机器学习的局限性提供了一个案例研究。它还给包括希尔顿全球控股公司（HLT.N）和高盛集团（GS.N）在内的越来越多的大型公司提供了一个教训，这些公司正在寻求自动化招聘过程的部分内容*。”

仍然有一些情况，透明度在操作上或监管上并不至关重要。例如，推荐引擎的接收者不需要知道 Netflix 如何选择下一个推荐的电影。但即使在这些情况下，消费者常常会问“为什么”，满足这种好奇心将大大增加用户对推荐的信心。

关于 AI 偏见的信息传播速度几乎与 AI 应用本身一样快。最近的一些例子包括主要的尊敬平台，如 Ted Talk [3]，以及主要出版物，如哈佛商业评论 [6]、Quartz [7] 或纽约时报 [8]。早期认识到这个问题的 ACT（计算语言学协会）自 2014 年以来一直在组织机器学习中的公平性、问责制和透明度会议，受邀的演讲者包括顶级的 ML 研究人员和从业者[9] ([https://fatconference.org/2019/cfp.html](https://fatconference.org/2019/cfp.html))。

尽管如此，AI 社区中的一些人仍然怀疑可解释性 AI 的必要性，并更愿意专注于进一步提高模型的准确性。

随着 AI 和 ML 模型越来越受欢迎，并应用于越来越广泛的用例，我们必须牢记风险。我们不要陷入准确性胜过解释性的陷阱。如果我们，数据科学家，陷入了这个陷阱，我们将失去公众和那些部署我们模型的人的信任。

为了支持可解释AI的倡议，DataWisen及我们的合作伙伴展示了一些突出的使用案例。这些案例仅仅是对可解释ML/AI需求的一个小样本。鼓励读者在评论中分享自己的例子。

我们将这一系列使用案例分为三个部分：

1.  操作需求

1.  合规性和

1.  公众信任和社会接受度。

### A. 操作需求

#### 用例 #1: 油炼厂资产可靠性：炉子洪水预测

+   **情况**

    维持稳定的燃烧对油炼厂炉子的安全操作至关重要。如果燃烧过程变得不稳定且未能及时识别和处理，炉子可能会发生洪水，最终可能引发爆炸。在油炼厂炉子中，洪水每年发生12到20次。在每个案例中，炉子需要停机以避免爆炸。生产中断的成本估计每年高达数百万美元。

+   **ML/AI解决方案**

    可靠的在线预测炉子洪水，以便及时响应操作员。该ML项目的数据将包括来自炉子的所有传感器信号以及外部数据，如天气、湿度等。解决方案应在事件发生前20分钟预测炉子洪水的可能性。操作员要求警报包括最可能的警报原因（即哪些传感器指向洪水风险）。

+   **为什么需要可解释的AI**

    造成洪水风险增加的原因有很多：例如燃烧器可能被扑灭，空气分配器可能出现故障等。操作员需要迅速对ML模型发出的警报做出反应。了解哪些传感器出现故障，使得操作员能够采取预防措施，而无需浪费时间进行额外调查。

#### 用例 #2: 公用事业收入保护：智能电网中的能源盗窃检测

+   **情况**

    能源盗窃造成的商业损失严重困扰全球公用事业公司，估计每年损失达890亿美元，并推动客户的能源价格上涨。能源盗贼使用多种手段盗取能源：他们可以接入变压器和房屋之间的线路，接入邻居的计量器，倒转自己的计量器等。

+   **ML/AI解决方案**

    为了最小化盗窃，收入保护经理需要一个优先级列表，列出最可能需要调查的盗窃案件。这种列表可以通过训练于智能计量器数据和外部因素（如天气、地区地理风险等）的ML模型生成。该工具必须足够灵活，以适应不断演变的盗窃方法。工具需要能够识别提升盗窃风险的因素，并输出犯罪的地理位置。

+   **为什么需要可解释的AI**

    不同类型的盗窃案件需要调查人员采取不同的行动。对于被逆向改装的电表，需要断开电表；对于盗贼接入邻居电源的家庭，需要发出警报并更换被篡改的电表。对于变压器线路的盗用，需要派遣卡车到适当的位置等。

#### 用例 #3: 电力公用事业 – 线路维护

+   **情况概述**

    维护电力基础设施对电力输送系统的可靠性至关重要。如果电力线路或变压器因天气条件而老化或退化，并且这一状况未被识别和处理，设备故障可能会导致电力中断和停电。

+   **机器学习/人工智能解决方案**

    传统的解决方案依赖于定期维护，但随着设备中内置传感器的增加，维护设备的趋势正在转向“及时”方式，当线路或变压器传感器发出即将需要维护的信号时。一个可靠的在线预测潜在设备故障的系统应包括所有传感器的信号以及天气、湿度等外部数据。该解决方案应实时预测设备故障的可能性。

+   **为何需要可解释的人工智能**

    电力输送基础设施覆盖广泛的地理区域。了解哪些传感器发出信号，使维护人员能够被派遣到正确的位置。

### B. 监管合规与法律影响

#### 用例 #4: 信用风险评分

+   **情况概述**

    所有金融机构以及许多其他企业都根据消费者的风险评分做出消费信用决策。信用评分模型是一种工具，通常用于接受或拒绝贷款的决策过程中。

+   **机器学习/人工智能解决方案**生成风险评分的机器学习消费者模型基于借款人的财务行为、过往支付历史、信用利用情况等信息，使信用提供者和其他相关人员能够基于未来违约的估计风险做出合理决策。

+   **为何需要可解释的人工智能**《公平信用报告法案》(FCRA)是一部联邦法律，规范信用报告机构，并强制要求它们确保所收集和分发的信息是消费者信用历史的公平和准确总结。... 这项法律旨在保护消费者不受误导信息的侵害。如果消费者因信用报告而被拒绝信用、保险或就业，他/她有法律权利要求具体的拒绝理由。因此，任何在信用决策中使用的风险模型都需要提供评分的关键理由。此外，目前美国银行业的所有风险计算模型都在受到银行合规团队和外部监管机构的审查。这些机构要求对输入模型的数据和各个特征的影响进行完全透明，以防止对受保护群体的潜在歧视。

#### 用例 #5: 视频行为检测

+   **情况**

    设施需要安全措施来保护人身和物理资产。典型的解决方案是安保人员和视频监控的结合。人员无法时刻监控每个入口点和每个视频数据流，并且由于错误可能会遗漏一些威胁。

+   **机器学习/人工智能解决方案**

    AI和深度学习模型评估视频数据以检测威胁，然后将这些威胁标记给安保人员。例如，AI图像识别模型被训练来标记接近入口的高风险个人（例如，可能携带武器或已知犯罪分子），而忽略合法员工。

+   **为什么需要可解释的AI**

    将个人标记为威胁可能会产生重大法律影响。如果AI使用的数据在某些类别（例如，特定少数群体）中训练样本比例不均，就会引入对这些类别的偏见。

    被拦截、阻挡或被安保人员搜查而感到羞辱的个人可能会质疑事件的合法性。公司可能被迫在法庭上为其行为辩护，并披露挑选该个人的原因。如果不能解释模型为何标记该个人，将无法保护公司免受歧视指控。

### C. 公众信任和社会接受度

#### 用例 #6：马术运动行业的定价模型

+   **情况**

    在马术运动行业中，马匹的价格和估值缺乏标准和透明度。该行业还面临代理人隐性获利的广泛做法，这些代理人促进了马匹的买卖。

    驱动马匹价值和价格的最重要因素包括血统、身体构造、在特定项目（如跳跃、盛装舞步、三项赛等）的能力、教育/训练水平和表现记录、年龄及健康状况。马匹的视觉特征也会影响其价值。

    了解驱动运动马匹价值的数据对公平和合理的马匹定价至关重要。这对于使该行业可负担且可行作为一种运动至关重要。关于估值和定价的建模和透明度将有助于防止回扣、过高佣金和价格操控。

+   **机器学习/人工智能解决方案**

    机器学习模型可以处理大量历史销售数据，以建立可靠的定价模型。AI视觉识别模型可以识别马匹的特征，并使定价模型得以应用。这些工具结合在一起可以为运动马匹的定价提供宝贵且客观的工具。

+   **为什么需要可解释的AI**

    AI模型所显示的价格本身并不能透露马匹的特征和能力。潜在买家必须看到潜在因素和马匹的特征，以便做出符合其需求的正确决定。一个解释驱动价格的因素的模型是满足市场需求并增强用户对价格信心所必需的。

### 参考文献

[1] 企业AI的圣杯——可解释的AI，Saurabh Kaushik；KDnuggets，2018年10月；[https://www.kdnuggets.com/2018/10/enterprise-explainable-ai.html](https://www.kdnuggets.com/2018/10/enterprise-explainable-ai.html)

[2] 性别阴影：商业性别分类中的交叉准确性差异；Joy Buolamwini，麻省理工学院媒体实验室 & Timnit Gebru，微软研究院；《机器学习研究公平、问责和透明度会议论文集》，2018年1月

[3] 我如何在算法中对抗偏见；Joy Buolamwini Ted Talk，2016年11月 [https://www.ted.com/talks/joy_buolamwini_how_i_m_fighting_bias_in_algorithms?utm_campaign=tedspread&utm_medium=referral&utm_source=tedcomshare](https://www.ted.com/talks/joy_buolamwini_how_i_m_fighting_bias_in_algorithms?utm_campaign=tedspread&utm_medium=referral&utm_source=tedcomshare)

[4] 永续排队——美国不受监管的警用面部识别；乔治城大学隐私与技术中心；2016年10月

[5] 亚马逊取消了一个显示对女性有偏见的秘密AI招聘工具，Jeffrey Dastin；路透社，2018年10月；[https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)

[6] 算法无法解决社会问题 [...], Dave Gershgorn；Quartz，2018年10月；[https://qz.com/1427159/algorithms-cant-fix-societal-problems-and-often-amplify-them/](https://qz.com/1427159/algorithms-cant-fix-societal-problems-and-often-amplify-them/)

[7] 算法偏见审计；Rumman Chowdhury & Narendra Mulani，哈佛商业评论，2018年10月；[https://hbr.org/2018/10/auditing-algorithms-for-bias](https://hbr.org/2018/10/auditing-algorithms-for-bias)

[8] 面部识别准确度高，但前提是你是白人；Steve Lohr，《纽约时报》，2018年2月 [https://nyti.ms/2BNurVq](https://nyti.ms/2BNurVq)

[9] 今年4月，欧洲委员会通过了关于人工智能在司法系统中使用的欧洲伦理章程 [https://www.coe.int/en/web/human-rights-rule-of-law/-/council-of-europe-adopts-first-european-ethical-charter-on-the-use-of-artificial-intelligence-in-judicial-systems](https://www.coe.int/en/web/human-rights-rule-of-law/-/council-of-europe-adopts-first-european-ethical-charter-on-the-use-of-artificial-intelligence-in-judicial-systems)

**资源：**

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [用于分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**相关：**

+   [机器学习可解释性与解释性：两个可能帮助恢复对AI信任的概念](https://www.kdnuggets.com/2018/12/machine-learning-explainability-interpretability-ai.html)

+   [解释AI和机器学习的四种方法](https://www.kdnuggets.com/2018/12/four-approaches-ai-machine-learning.html)

+   [解释性对信任AI和机器学习至关重要](https://www.kdnuggets.com/2018/11/interpretability-trust-ai-machine-learning.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

### 相关话题更多

+   [使用Python进行自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [Chip Huyen分享实施机器学习系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [自制大型语言模型的案例](https://www.kdnuggets.com/the-case-of-homegrown-large-language-models)

+   [弥合人类理解与机器学习之间的差距：…](https://www.kdnuggets.com/2023/06/closing-gap-human-understanding-machine-learning-explainable-ai-solution.html)

+   [为对话解释可解释的AI](https://www.kdnuggets.com/2022/10/explaining-explainable-ai-conversations.html)

+   [最先进的深度学习技术的可解释预测和即时预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)
