# 用数据科学拯救莎拉·康纳

> 原文：[https://www.kdnuggets.com/2021/10/save-sarah-connor-data-science.html](https://www.kdnuggets.com/2021/10/save-sarah-connor-data-science.html)

[评论](#comments)

**作者：[彼得·科兹洛夫](https://www.linkedin.com/in/peter-kozlov/)，数据分析硕士，拥有超过30年的不同行业经验**。

***下面的图片发生了什么？***

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行IT服务。

* * *

***我们在这里看到数据窃贼了吗？***

![](../Images/6a6b2fb376640c02b458c823acce22dd.png)

*图片 1\. 来自1984年上映的[《终结者》](https://en.wikipedia.org/wiki/The_Terminator)电影的镜头。*

这是1984年的加州。我们可能会认出未来的加州州长阿诺德·施瓦辛格，他在电影《终结者》中扮演了直接针对加州人的角色。他正在使用在电话亭里找到的电话簿。

终结者是数据窃贼吗？实际上，他并不是，因为在1984年，这类个人数据是公开可用的。

近年来个人信息处理的认知发生了显著变化，并且有一些迹象：

+   (欧洲) 一般数据保护条例 ([GDPR](https://gdpr-info.eu/)) 于2018年5月25日实施。

+   (澳大利亚) 消费者数据权利 ([CDR](https://www.cdr.gov.au/)) 从2019年9月1日起生效。

+   (美国) 健康保险流通与问责法案 ([HIPAA](https://www.cdc.gov/phlp/publications/topic/hipaa.html)) 预计在2021年会有变更。

今天，加州人对数据隐私非常重视，根据2019年10月11日签署的加州消费者隐私法案 ([CCPA](https://oag.ca.gov/privacy/ccpa))。加州人走在前面，为每一条不合规的客户记录附上了价格标签。价格范围在每条记录100到750美元之间。

数据隐私的最佳实践是什么？美国监管机构HIPAA一直是健康行业以及其他行业的数据隐私模范。它传播知识，并为隐私数据处理方法提供明确的定义。HIPAA区分了安全港和专家判定方法。我们将比较这些数据模糊化的方法。

![](../Images/70eb2f07777e939ee94827b293607ad6.png)

*图片 2\. HIPAA推荐的去标识化方法。来自[美国卫生与公共服务部](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html#standard)的插图。*

专家鉴定由合格的统计学家/数据科学家提供作为客观风险评估。相比之下，Safe Harbour是一个让企业自己完成这项工作的配方。

“Safe Harbour”是一个非常引人注目的概念，因为它基于一套简单的规则来移除明确的标识符。例如，前两个标识符是个人的姓名和小于州的地理细分。完整的标识符列表可以在[这里](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html#standard)找到。除了移除密钥外，我们还需要确保没有留下识别个人的信息。这可能是由于微密钥（如非常罕见的疾病或独特的治疗参数）而成为一个挑战。Safe Harbour是基于规则的，因此在考虑数据的实际内容和微密钥时过于僵化。技术科学期刊中的文章[A Re-identification of Patients in Maine and Vermont Statewide Hospital Data](https://techscience.org/a/2018100901/)演示了3.2%的Safe Harbour数据通过地方出版物重新识别为个人。

专家鉴定方法可以考虑数据研究的目的，并在研究背景中容忍各种[混淆方法](https://www.researchgate.net/publication/339242645_GDPR_COMPLIANT_METHODS_OF_DATA_PROTECTION)：

+   扰乱

+   加密

+   目录替换

+   屏蔽

+   标记化

+   数据模糊

+   噪声添加

+   聚合

专家鉴定使我们能够在产生的信息丧失与个人信息泄露的概率之间取得平衡，并确保信息处理的意义得以保留。

**让我们讨论一个例子。**

我们的任务是进行客户研究并减少数据风险。我们想要将城市与农村客户通过Safe Harbour和专家鉴定进行比较。我们将使用两个记录和四个字段的数据示例。我们假设为研究记录收集的顺序在内部数据集中是随机的，并且收集的“有价值客户数据”不会逐行公开。我们还假设以下数据集在一个诊所中可用，作为本练习的外部数据源：

![](../Images/897d0c98299e973acb4d4ce55c1ca91d.png)

*表 1\. 外部数据源的数据示例。*

我们的Safe Harbour混淆要求我们从出生日期（DOB）中移除姓名、日期和月份，并将位置仅替换为州：

![](../Images/3bb2d1147efdd1792cdd467016dd5321.png)

*表 2\. Safe Harbour数据混淆的示例。*

对于数据泄漏，我们有两个关注点：

+   泄露的数据与外部源结合以利用出生日期。

+   从我们的内部数据集中可能推测出出生日期。

*P(DOB 公开|Ext|Int)* 是当内部和外部数据泄露时出生日期公开的概率。由于我们在内部数据集中有两个具有相同出生年份（YOB）和地点的记录，因此有一半的机会可以将“1963，加利福尼亚”匹配到外部来源的出生日期。

*P(DOB 公开|Int)* 是当攻击者尝试从我们的内部数据集中恢复出生日期时的出生日期公开概率。1963 年有 365 天，内部数据集中没有其他信息来猜测这样一个大日期范围中的具体日期。

专家判断方法分为两个步骤。首先，我们尽量保留最大的信息，其次，我们通过调整保留的数据来审查这种“数据储存”，以符合研究目的。

![](../Images/a13cf64d60c3419c12eb05fd7671c337.png)

*表 3\. 数据示例的专家判断数据混淆的第一次尝试。*

*P(DOB 公开|Ext|Int)* 与 Safe Harbor 数据集中的值相同。

*P(DOB 公开|Int)* 是 30 分之一的几率，比之前的情况更高。根据我们的任意决策，这个 *P* 值可以被接受为低风险或中等风险。

在第一次专家判断混淆尝试中，我们设法保留了姓名、地点和出生日期到月份级别。问题在于这些混淆结果是否与研究目的对齐。更仔细地对齐城乡客户研究，可以排除姓名，仅标记客户年龄组为成人和儿童。

![](../Images/a5b23ac5b39183a17c19c406bccb0b6e.png)

*表 4\. 数据示例的专家判断数据混淆的第二次尝试。*

*P(DOB 公开|Ext|Int)* 的值没有变化。

*P(DOB 公开|Int)* 是基于最大成人年龄 100（你也可以使用更保守的数字，例如平均寿命）的估算值。其他两个参数是最小成人年龄 18 和每年的平均天数 365.25。第二次专家判断混淆的 *P(DOB 公开|Int)* 明显低于 Safe Harbor 实施的结果。

对这两种 HIPAA 混淆方法的数据示例的最终比较：

![](../Images/a0bd87d2f58753b8cb127e2aeb4d6b7d.png)

*表 5\. 比较 HIPAA 方法对数据示例和研究目的的影响。*

所选的数据示例和研究目的旨在展示专家判断方法相较于 Safe Harbour 的灵活性。专家判断的主要优势在于，该方法不仅处理了披露的概率，同时还允许我们控制相关的信息丢失。使用专家判断数据示例混淆的数据样本允许我们进行城乡对比研究，而 Safe Harbour 则不允许。

或许这个例子会证明，问题的最简单解决方案并不总是合适的，应用最佳而非最快的解决方案可以拯救莎拉·康纳免于被终结者发现。

**相关：**

+   [在接触数据集之前不要忘记问这 10 个问题](https://www.kdnuggets.com/2021/09/dataset-asking-10-questions.html)

+   [当机器学习对你了解得过多时](https://www.kdnuggets.com/2020/11/machine-learning-knows-too-much-about-you.html)

+   [数据保护技术以保证隐私](https://www.kdnuggets.com/2020/10/data-protection-techniques-guarantee-privacy.html)

### 更多主题

+   [解锁你的下一个动作：节省高达 67% 的热门数据技能提升课程](https://www.kdnuggets.com/2023/03/datacamp-unlock-next-move-save-67-indemand-data-upskilling.html)

+   [停止学习数据科学来寻找目的，并找到目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学入门：你需要知道的 10 个基础技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [数据科学定义幽默：奇特名言合集…](https://www.kdnuggets.com/2022/02/data-science-definition-humor.html)

+   [5 个数据科学项目来学习 5 个关键的数据科学技能](https://www.kdnuggets.com/2022/03/5-data-science-projects-learn-5-critical-data-science-skills.html)

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理及其应用…](https://www.kdnuggets.com/2022/n46.html)
