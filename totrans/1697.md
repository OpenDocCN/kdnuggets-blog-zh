# 数据科学中的治理

> 原文：[https://www.kdnuggets.com/2018/01/governance-data-science.html](https://www.kdnuggets.com/2018/01/governance-data-science.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Ben Weber](https://www.linkedin.com/in/ben-weber-3b87482/)，Windfall Data 的首席数据科学家**

![Header image](../Images/7e2de33000086ef26f82718a18458e4f.png)

美国主要大城市的通货膨胀率。来源：美国劳工统计局数据

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

为了构建预测模型，数据科学家需要准确的数据进行训练和验证。虽然通常会花费大量精力来清理用于建模的数据源，例如处理缺失的属性，但通常底层数据集存在更大的问题，这些问题需要正确处理，以便训练出的模型能够真正具有代表性。数据治理的一个目标是数据完整性，它涉及验证你对数据集的基本假设是否与现实相符。一个展示数据科学中这一方面重要性的例子是最近的 [FiveThirtyEight 文章](https://fivethirtyeight.com/features/we-used-broadband-data-we-shouldnt-have-heres-what-went-wrong/)，该文指出，由于使用了一个有缺陷的数据集，之前关于宽带接入的结论无效。

数据科学和分析团队的治理角色变得越来越普遍，因为公司正在使用来自各种内部和外部来源的大型复杂数据集。这个角色的一个关键职能是对数据集进行分析和验证，以建立对底层数据集的信心。在将数据集作为输入到我们的模型中之前，我们希望建立对这些数据集的信任，这些模型的输出是对客户可见的。在 [Windfall](https://angel.co/windfall-data) ，我们使用多种公共和专有的数据源作为我们净资产模型的输入。我们正在招聘一个 [数据治理科学家](https://angel.co/windfall-data/jobs/295441-data-scientist-governance) 的角色，专注于数据完整性等方面，以确保我们在建模过程中使用经过验证的数据集。

由于这是一个较新的角色，我想要明确数据科学家在这个角色中应执行的关键职能：

1.  质疑数据的基本假设

1.  识别如何解决数据源中的不一致问题

1.  评估新数据源的价值

**质疑假设**

使用数据集时的一个主要挑战是确定数据的有效性。数据常常是过时的，或以不代表整体人口的方式进行采样。如果你使用的是几年前的数据，从数据中得出的许多结论可能已经不再成立。例如，使用2010年的宽带连接数据来确定取消网络中立性对今天美国家庭的影响会有问题。在《FiveThirtyEight》文章的例子中，使用了一个样本数据集，其中宽带订阅者的分布与其他分析的数据源有显著差异。

为了质疑数据的基本假设，通常需要将数据与不同的来源进行审计。例如，FEC提供的关于政治捐款的交易级数据可以与来自竞选活动的汇总金额进行比较，住房价值的估算可以与Zillow和Redfin的估算进行比较。治理角色将优先考虑哪些数据点需要手动检查，以便建立对数据集的更大信心，并确保从样本数据集中得出的结论可以应用于更广泛的人群。

**解决差异**

这个角色的另一个方面是确定如何解决数据集发现的问题。在发布错误的发现时，应该发布一份事后分析，解释基于新发现的信息这些发现如何发生变化，《FiveThirtyEight》文章就是一个很好的例子。但如果输入数据用于建模，则该角色应与工程团队合作，解决数据管道中的问题。

在Windfall遇到的一个非平凡的情况是处理多物业交易，其中多个地址的物业作为同一交易的一部分进行购买。处理这些类型的交易需要在我们的自动化估值模型（AVM）计算中添加新规则。就像产品化一个模型一样，治理数据科学家应该能够将数据质量修复投入生产。这可能涉及交付脚本或提交包含代码更改的PR。

**评估新来源**

我们为治理角色定义的一个额外职能是评估新的数据源是否值得用于建模目的。在Windfall，这意味着确定添加新数据源是否会提高我们净资产模型的准确性。这个角色中的数据科学家应该能够处理各种数据格式和来源的第三方数据，并对数据进行探索性分析。探索新数据集的目标通常是测试不同数据集之间属性的相关性，数据科学家需要能够有效地处理不同的数据来源。

**治理角色概况**

公司在治理角色中寻找什么？在Windfall，我们正在寻找具备以下技能的数据科学家：

1.  **EDA：**在大型且混乱的数据集上展示的探索性数据分析（EDA）经验。例如，使用第三方API并测试数据的核心假设。

1.  **脚本编写：**如上所述，数据科学家应能够将他们的发现产品化。R和Python是设置可重复研究的良好起点，但我们还希望脚本项目中的发现可以转化到我们的数据管道中。

1.  **写作：**书面和口头沟通对这个角色至关重要，因为治理角色需要能够与技术团队、商业领导者和第三方数据供应商分享发现。这包括撰写长篇报告，创建引人注目的可视化，以及记录新的数据来源。

这个角色不同于机器学习角色，因为重点不在于预测建模，而是集中在提高数据质量和完整性。它也不同于产品分析角色，因为目标是识别基础数据中的差异，而不是商业指标。尽管存在这些差异，该角色仍然需要统计知识、领域专业知识和数据科学常见的黑客技能。

**个人简介：[本·韦伯](https://www.linkedin.com/in/ben-weber-3b87482/)** 是[Windfall Data](https://angel.co/windfall-data)的首席数据科学家，我们的使命是识别全球每个家庭的净资产。

[原文](https://medium.com/@bgweber/governance-in-data-science-710ff7e6ed94)。经许可转载。

**相关内容：**

+   [数据科学治理 - 它为什么重要？为什么是现在？](/2017/07/data-science-governance.html)

+   [数据科学学习的艺术](/2018/01/art-learning-data-science.html)

+   [2018年数据科学中的五个重要因素](/2018/01/packt-5-things-important-data-science.html)

### 更多相关主题

+   [数据治理和可观测性解释](https://www.kdnuggets.com/2022/08/data-governance-observability-explained.html)

+   [数据治理能解决AI疲劳吗？](https://www.kdnuggets.com/can-data-governance-address-ai-fatigue)

+   [什么是AI模型治理？](https://www.kdnuggets.com/2021/12/ai-model-governance.html)

+   [停止学习数据科学以找到目的，并找到目的…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最小要求：你需要了解的10项基本技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [KDnuggets™新闻 22:n06，2月9日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)
