# GDPR会使机器学习变得非法吗？

> 原文：[https://www.kdnuggets.com/2018/03/gdpr-machine-learning-illegal.html](https://www.kdnuggets.com/2018/03/gdpr-machine-learning-illegal.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)![GDPR](../Images/61fdf315671b1d092544e9d5e473e4ac.png)欧盟通用数据保护条例，即 [**GDPR**](https://www.eugdpr.org/)，是21世纪数据隐私法规中的重大变革，并将于2018年5月25日生效。

这将对数据收集和处理欧盟公民数据的许多方面产生重大影响，并将影响到不仅是欧盟公司，还包括在欧盟运营的跨国公司。

GDPR对机器学习的一个可能且重要的影响是“解释权”。

GDPR的一些条款可以解释为要求对机器学习算法做出的决策进行解释，尤其是当其应用于人类对象时。

UW的Pedro Domingos教授，一位领先的人工智能研究员，通过他的推文引发了轩然大波。

> 从5月25日起，欧盟将要求算法解释其输出，这将使深度学习成为非法行为。— Pedro Domingos (@pmddomingos) [2018年1月29日](https://twitter.com/pmddomingos/status/957825455666618368?ref_src=twsrc%5Etfw)

GDPR是否真的要求对机器学习算法进行解释？

我们应该区分

1.  全球解释：机器学习算法如何工作（这对于复杂的方法如深度学习可能非常困难）以及

1.  本地解释：哪些因素影响了特定个人的决策（较容易）。已有一些算法，如 [LIME: 本地可解释的模型无关解释](https://www.kdnuggets.com/2016/08/introduction-local-interpretable-model-agnostic-explanations-lime.html)，可以解释任何机器学习分类器的预测结果。

    比如，如果一个人被拒绝抵押贷款，她是否应该知道哪些因素影响了这一决定？一方面，如果算法拒绝了你，你会想知道原因并有机会上诉。另一方面，过多的解释可能会使决策边界被逆向工程，从而让潜在的不法分子操控系统。在许多情况下这是非常不希望出现的（例如，安全应用）。

我向 [![Sandra Wachter](../Images/79c296648194b32e7674bfe2e02bc544.png) **Sandra Wachter博士**](https://www.oii.ox.ac.uk/people/sandra-wachter/)，一位欧盟律师，牛津大学大数据、人工智能与机器人伦理法律研究员，（[@SandraWachter5](https://twitter.com/SandraWachter5) 在Twitter上）询问了这个问题。

她说，GDPR要求数据控制者采取适当措施保护数据主体的权利、自由和合法利益。这些措施应包括让数据主体获得人工干预、表达观点和争议决定的方法。

她的观点还包括第15条暗示了一种更一般形式的监督，而不是对特定决策的解释权。

**因此，《GDPR》中的解释权在法律上并不具约束力，但可以自愿提供。**

以下是Sandra博客的摘录，[走向欧洲负责任的AI？](https://www.turing.ac.uk/media/opinion/towards-accountable-ai-europe/)，经许可转载。

> **AI及其挑战**
> 
> 基于AI的系统常常是不透明的“黑箱”，难以审查。随着我们经济、社会和公民互动越来越多地由算法进行——从信用市场和健康保险申请，到招聘和刑事司法系统——对技术透明度不足的担忧也随之增加，这使得个人对决策如何作出了解甚少。我们需要适当的保障措施，确保对我们作出的决策确实公平准确。
> 
> **AI与欧盟通用数据保护条例**
> 
> 2016年，[欧盟通用数据保护条例](http://ec.europa.eu/justice/data-protection/reform/files/regulation_oj_en.pdf)（GDPR），欧洲的新数据保护框架，已获批准。新法规将于2018年在整个欧洲——包括英国——生效。人们广泛且反复声称，新法规将强制要求对自动化或人工智能算法系统作出的所有决策提供“解释权”。这种“解释权”被视为增强自动化算法决策透明度和问责制的理想机制。
> 
> 这种权利将使人们能够询问如何作出特定的决策（例如，保险被拒或晋升被拒）。
> 
> 解释可以通过各种方式提供。至少有两种可能的算法解释：对“系统功能”的解释和对个体决策“理由”的解释。解释用于评估信用价值或设定利率的算法方法（系统功能）与解释“如何”设定某个利率或“为什么”信用卡申请被拒绝的解释并不相同。
> 
> 我们与图灵研究人员Dr. Brent Mittelstadt和Prof. Luciano Floridi一起研究了这一声明。不幸的是，与预期相反，[我们的研究](https://academic.oup.com/idpl/article/7/2/76/3860948/Why-a-Right-to-Explanation-of-Automated-Decision)揭示GDPR可能只会赋予个人有关自动决策存在性和“系统功能”的信息，但没有对决策理由的解释。事实上，在整个GDPR中，“解释权”只在第71前言中提到过一次，这一前言没有法律效力来建立独立的权利。前言的目的是在框架的操作部分存在歧义时提供解释，但在我们的研究中，我认为对于需要进一步澄清的最低要求并不存在歧义。
> 
> 将“解释权”放在前言中，并且欧洲议会关于将这一权利具有法律约束力的建议未被采纳，这表明欧洲立法者并不想赋予这一概念与GDPR第22条中的其他保护措施相同的法律地位。当然，这并不意味着数据控制者不能自愿决定提供解释，或者未来的法律或以此前言为基础的法律可能会在未来创造这样的权利。

欲了解更多细节，请收听[与Sandra Wachter讨论算法、解释和GDPR的播客](https://philosophicaldisquisitions.blogspot.co.uk/2018/01/episode-36wachter-on-algorithms.html)。

一种可能的解释解决方案是*反事实*，例如

> 你被拒绝贷款是因为你的年收入为30,000英镑。如果你的收入是45,000英镑，你就会获得贷款。

由Sandra Wachter、Brent Mittelstadt和Chris Russell撰写的论文[《无需揭开黑箱的反事实解释：自动决策与GDPR》](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3063289)展示了即使使用高度复杂的系统，也能为人们提供有意义的解释，而无需理解算法的内部逻辑。使用反事实也不太可能侵犯商业秘密。另见[关于反事实的访谈](https://www.oii.ox.ac.uk/blog/could-counterfactuals-explain-algorithmic-decisions-without-opening-the-black-box/)。

然而，Sandra认为GDPR中没有解释权的观点并未被其他专家完全认同。

Andrew D. Selbst和Julia Powles在[《有意义的信息与解释权》](https://academic.oup.com/idpl/article/7/4/233/4762325)中写道，

> 在欧洲新的《通用数据保护条例》（GDPR）中，没有单一明确标示为“解释权”的法定条款。但这并不意味着这样的权利是虚幻的。
> 
> 第13-15条文章赋予了“关于自动化决策中涉及的逻辑的有意义信息”的权利。这是一种解释权，无论是否使用这一说法。

安德鲁·伯特在 [*GDPR中是否存在机器学习的“解释权”？*](https://iapp.org/news/a/is-there-a-right-to-explanation-for-machine-learning-in-the-gdpr/) 中写道

> 像其他领域一样，GDPR并不十分明确。因此，GDPR是否要求机器学习模型提供“解释权”——即那些受模型显著影响的人员是否有权知道模型如何做出特定决策——已成为一个有争议的话题。例如，一些学者强烈反对这种权利的存在可能性。另一些人，如英国信息专员办公室，似乎认为这一权利是显而易见的。
> 
> 最终，我有一些好消息要告诉律师和隐私专业人士……也有一些可能对数据科学家来说不太好的消息。

GDPR中似乎存在足够的模糊性，使得律师们忙得不可开交。

敬请关注！

**相关内容：**

+   [**解决AI困境的3条原则：优化与解释**](https://www.kdnuggets.com/2018/02/3-principles-ai-dilemma-optimization-explanation.html)

+   [**GDPR如何影响数据科学**](https://www.kdnuggets.com/2017/07/gdpr-affects-data-science.html)

+   [**数据匿名化与数据科学的未来**](https://www.kdnuggets.com/2017/04/anonymization-future-data-science.html)

+   [**局部可解释模型无关解释（LIME）简介**](https://www.kdnuggets.com/2016/08/introduction-local-interpretable-model-agnostic-explanations-lime.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

### 更多相关内容

+   [成为一名优秀数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [6种每个初学者数据科学家应该掌握的预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目的，并以找到目的来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学统计学习的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
