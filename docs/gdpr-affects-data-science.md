# GDPR 如何影响数据科学

> 原文：[`www.kdnuggets.com/2017/07/gdpr-affects-data-science.html`](https://www.kdnuggets.com/2017/07/gdpr-affects-data-science.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Thomas W. Dinsmore](https://thomaswdinsmore.com/author/thomaswdinsmore/)撰写。**

*改编自原文[发布](https://vision.cloudera.com/general-data-protection-regulation-gdpr-and-data-science/)于 Cloudera VISION 博客。*

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

如果你的组织收集有关欧盟（EU）公民的数据，你可能已经了解了通用数据保护条例（GDPR）。GDPR 定义并加强了对消费者的数据保护，并在欧盟内统一了数据安全规则。欧洲议会[批准](http://www.europarl.europa.eu/news/en/news-room/20160407IPR21776/data-protection-reform-parliament-approves-new-rules-fit-for-the-digital-era)了这一措施，并于 2016 年 4 月 27 日通过。该条例将在不到一年的时间内生效，即 2018 年 5 月 25 日。

![](img/a65c2f5273c11593cf004f99d939cc3c.png)

关于 GDPR 的大部分[评论](http://www.williamfry.com/newsandinsights/news-article/2016/06/08/key-impacts-of-the-eu-general-data-protection-regulation)集中在新规则如何影响消费者个人信息（PII）的收集和管理。然而，GDPR 也将改变组织进行数据科学的方式。这正是本文的主题。

在开始之前有一个警告。GDPR 非常复杂。在某些领域，GDPR 定义了高层次的结果，但将详细的合规规则委托给了一个新的实体——欧洲数据保护委员会。GDPR 法规与许多国家的法律和规定交织在一起；在英国开展业务的组织还必须评估脱欧带来的未知影响。受 GDPR 约束的组织应寻求专家管理和法律顾问的帮助，以制定合规计划。

#### GDPR 与数据科学

GDPR 在三个领域影响数据科学实践。首先，GDPR 对数据处理和消费者画像施加了限制。其次，对于使用自动化决策的组织，GDPR 为消费者创造了“解释权”。第三，GDPR 要求公司对自动化决策中的偏见和歧视负责。

**数据处理和分析**。GDPR 对数据处理和消费者分析施加控制；这些规则补充了数据收集和管理的要求。GDPR 将分析定义为：

*任何形式的个人数据自动化处理，包括使用个人数据评估与自然人相关的某些个人方面，特别是分析或预测有关自然人在工作中的表现、经济状况、健康、个人偏好、兴趣、可靠性、行为、位置或移动的方面。*

一般而言，组织在能够证明合法商业目的（如客户或雇佣关系）且不与消费者的权利和自由相冲突时，可以处理个人数据。组织必须告知消费者分析及其后果，并提供选择退出的机会。

**解释权**。GDPR 赋予消费者“不得仅基于自动化处理作出决策，并对当事人产生法律效力”的权利。专家[将](https://arxiv.org/abs/1606.08813)这一规则称为“解释权”。GDPR 并未精确定义该部分涵盖的决策范围。英国信息专员办公室（ICO）[表示](https://ico.org.uk/media/for-organisations/documents/2013559/big-data-ai-ml-and-data-protection.pdf)该权利“很可能”适用于信用申请、招聘和保险决策。其他机构、法院或欧洲数据保护委员会可能会对这一范围作出不同的定义。

**偏见和歧视**。当组织使用自动化决策时，他们必须防止基于种族或民族出身、政治观点、宗教或信仰、工会成员身份、基因或健康状况或性取向的歧视性效果，或导致产生这种效果的措施。此外，他们不得在自动化决策中使用特定类别的个人数据，除非在规定的情况下。

#### GDPR 对数据科学实践的影响

新规则将如何影响数据科学团队的工作方式？让我们在三个关键领域探讨其影响。

**数据处理和分析**。新规则允许组织出于特定商业目的处理个人数据，履行合同义务，并遵守国家法律。信用卡发行机构可以处理个人数据以确定持卡人的可用信用额度；银行可以根据监管机构的要求筛查交易以防止洗钱。消费者不能选择退出在这些“安全港”下进行的处理和分析。

然而，组织不得在未经消费者额外许可的情况下将个人数据用于原意之外的目的。这一要求可能限制了可用于探索性数据科学的数据量。

GDPR 对数据处理和数据分析的限制仅适用于能够识别个人消费者的数据。

*因此，数据保护原则不应适用于……以使数据主体无法识别或不再能够识别的方式匿名化的个人数据。因此，本法规不涉及此类匿名信息的处理，包括用于统计或研究目的。*

明确的含义是，受 GDPR 约束的组织必须在数据工程和数据科学流程中建立稳健的[匿名化](https://en.wikipedia.org/wiki/Data_anonymization)机制。

**可解释的决策。** 对这一条款的影响存在一些争议。一些人[欢呼](https://www.techdirt.com/articles/20160708/11040034922/activists-cheer-eus-right-to-explanation-algorithmic-decisions-how-will-it-work-when-theres-nothing-to-explain.shtml)它；另一些人[不赞成](http://www.techzone360.com/topics/techzone/articles/2017/01/25/429101-eus-right-explanation-harmful-restriction-artificial-intelligence.htm)；还有一些人[否认](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2903469)GDPR 创造了这样的权利。一位欧盟法律专家[认为](http://privacylawblog.fieldfisher.com/2015/getting-to-know-the-gdpr-part-5-your-big-data-analytics-and-profiling-activities-may-be-seriously-curtailed/)，该要求可能迫使数据科学家停止使用不透明的技术（如深度学习），这些技术可能难以解释和理解。

毫无疑问，GDPR 将影响组织处理某些决策的方式。然而，对数据科学家的影响可能被夸大了：

“解释权”在范围上是有限的。如上所述，某些监管机构将法律解释为涵盖信用申请、招聘和保险决定。其他监管机构或法院可能会有不同的解释，但很明显，这项权利适用于特定场景，而不适用于所有自动化决策。

在许多司法管辖区，“解释权”已经存在且存在多年。例如，英国的信用决定监管规定与美国类似，美国的发行商必须提供对基于信用局信息的负面信用决定的解释。GDPR 扩大了这些规则的适用范围，但今天已经有商业化的合规工具。

大多数拒绝一些客户请求的企业理解，负面决定应该向客户解释。这在贷款和保险行业已经是普遍做法。一些企业将负面决定视为推广替代产品的机会。

— 需要提供解释会影响*决策引擎*，但不必影响*模型训练*的方法选择。现有技术使得即使数据科学家使用不透明的方法来训练模型，也可以“逆向工程”出可解释的模型评分解释。

尽管如此，[数据科学家](http://www.csail.mit.edu/making_computers_explain_themselves)确实有充分的理由考虑使用可解释的技术。金融服务巨头 Capital One [认为](https://www.technologyreview.com/s/604122/the-financial-world-wants-to-open-ais-black-boxes/)这些技术是对抗隐性偏见（如下文所述）的有力武器。但不应因此得出结论，GDPR 将迫使数据科学家限制他们用于训练预测模型的技术。

**偏见与歧视。** GDPR 要求组织必须避免在自动化决策中出现歧视性效果。这一规则对建立预测模型的数据科学家以及组织批准预测模型用于生产的程序施加了额外的尽职调查负担。

使用自动化决策的组织必须：

+   确保公平和透明的处理

+   使用适当的数学和统计程序

+   建立措施以确保在决策中使用的主题数据的准确性

GDPR 明确禁止在自动化决策中使用个人特征（如年龄、种族、民族和其他列举的类别）。然而，仅仅避免使用这些数据是不够的。禁止歧视性结果的规定意味着数据科学家还必须采取措施防止由代理变量、多重共线性或其他原因引起的间接偏倚。例如，使用看似中立的特征（如消费者的居住社区）的自动决策可能会无意中对少数民族群体造成歧视。

数据科学家还必须采取积极措施确认他们在开发预测模型时使用的数据是准确的；“垃圾进/垃圾出”或 GIGO，不能成为借口。他们还必须考虑以往结果的偏倚训练数据是否会影响模型。结果是，数据科学家需要关注[数据血统](https://en.wikipedia.org/wiki/Data_lineage)，追踪数据从源头到目标的所有处理步骤。GDPR 还将推动对[可重复性](https://en.wikipedia.org/wiki/Reproducibility)的更大关注，即准确复制预测建模项目的能力。

#### 下一步

如果你在欧盟开展业务，现在是开始规划 GDPR 的时机。需要做的事情很多：评估你收集的数据，实施合规程序，评估你的处理操作等。如果你目前使用机器学习进行分析和自动决策，你现在需要做四件事。

**限制对消费者个人身份信息（PII）的访问。**

实施稳健的匿名化，以便默认情况下分析用户无法访问 PII。定义一个允许在特殊情况下在适当安全下访问 PII 的例外过程。

**识别当前使用 PII 的预测模型。**

在每种情况下，询问：

+   这些数据在分析上是否必要？

+   这些 PII 是否提供独特且不可替代的信息价值？

+   预测模型是否支持被允许的使用案例？

**盘点面向消费者的自动化决策。**

+   识别需要解释的决策。

+   实施程序以处理消费者的问题和担忧。

**建立一个能够最小化错误和偏见风险的数据科学流程。**

+   实施一个确保模型开发和测试正确的工作流程。

+   考虑训练数据中可能存在的“内建”偏见。

+   严格测试和验证预测模型。

+   实施同行评审，以独立评估每个模型。

即使你的组织不受 GDPR 约束，也可以考虑实施这些实践。这是正确的商业方式。

[原文](https://thomaswdinsmore.com/2017/07/17/how-gdpr-affects-data-science/)。经允许转载。

**简介：[托马斯·W·丁斯莫尔](https://thomaswdinsmore.com/)** 最近加入 Cloudera，担任数据科学产品营销总监。此前，作为独立顾问，他为寻求机器学习市场情报的私人客户提供了市场见解。

**相关：**

+   **数据科学治理 - 为什么重要？为什么现在？**

+   **数据匿名化与数据科学的未来**

+   **算法偏见的基础**

### 更多相关主题

+   [停止学习数据科学以寻找目标，寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [成为优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)
