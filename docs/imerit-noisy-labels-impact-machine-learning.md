# 噪声标签对机器学习模型的影响

> 原文：[https://www.kdnuggets.com/2021/04/imerit-noisy-labels-impact-machine-learning.html](https://www.kdnuggets.com/2021/04/imerit-noisy-labels-impact-machine-learning.html)

赞助文章。

[![Imerit Kickstart](../Images/96634a8676754809b55bcf6130b527eb.png)](https://go.imerit.net/data-annotation?latest_sfdc_campaign=7011Y000002WJrU&latest_sfdc_campaign_status=Responded&utm_campaign=noise_ml&utm_medium=blogpost&utm_source=kdnuggets&utm_content=06_april_2021_image)

监督式机器学习需要标记的训练数据，而大型机器学习系统需要大量的训练数据。标记训练数据是资源密集型的，虽然众包和网络抓取等技术可以提供帮助，但它们可能容易出错，给训练集添加‘标签噪声’。

位于[ iMerit](https://go.imerit.net/data-annotation?latest_sfdc_campaign=7011Y000002WJrj&latest_sfdc_campaign_status=Responded&utm_campaign=noise_ml&utm_medium=blogpost&utm_source=kdnuggets&utm_content=06_april_2021_middletextbrand)的团队是提供高质量数据的领导者，他们已经审查了关于如何使训练有噪声标签的机器学习系统有效运行的现有研究。如果你希望了解更多关于创建成功机器学习应用所需的训练数据的信息，请联系[我们与专家对话](https://go.imerit.net/data-annotation?latest_sfdc_campaign=7011Y000002WJre&latest_sfdc_campaign_status=Responded&utm_campaign=noise_ml&utm_medium=blogpost&utm_source=kdnuggets&utm_content=06_april_2021_middletextcta)。

在某些条件下，使用标签错误的数据训练的机器学习系统可以运行良好。例如，一项2018年由MIT/康奈尔大学进行的[研究](https://arxiv.org/abs/1705.10694)测试了不同标签噪声水平下的机器学习图像分类系统的准确性。他们发现，在以下条件下，机器学习系统能够在高水平的标签噪声下保持良好的性能：

+   机器学习系统必须具有足够大的参数集来管理图像分类任务的复杂性。例如，四层卷积神经网络（CNN）对手写字符基准任务足够，但对一般对象识别基准任务则需要18层残差网络。

+   训练数据集必须非常大——足够大以包含许多正确标记的样本，即使大部分训练数据被标记错误。通过足够好的训练样本，机器学习系统可以通过在标签噪声中找到‘信号’来学习准确的分类。

另一个对研究结果重要的因素是标签噪声的性质。错误标记的样本以足够随机的方式添加到训练集中，以至于不会创建强烈的模式来覆盖由正确标记的样本表示的‘信号’。

尽管这项研究表明机器学习系统有基本的能力处理误标，但许多实际应用中的机器学习面临的复杂情况使得标签噪声成为更大问题。这些复杂情况包括：

+   无法创建非常大的训练集，并且

+   系统性标签错误会混淆机器学习。

其中一个例子[是研究](https://digitalcommons.mtu.edu/michigantech-p/924/)，它使用远程感测和机器学习来评估地震损害。

在2017年，研究人员[分析了](https://digitalcommons.mtu.edu/michigantech-p/924/)误标训练数据对用于分类2011年新西兰地震瓦砾的机器学习系统的影响。他们注意到，在这种远程感测应用中，标签噪声并不像MIT/康奈尔研究中那样遵循随机模式。观察到的标签错误主要是由于地理空间划分不准确，这可能是由于缺乏培训（例如，误解了什么应被包含为瓦砾）或工具不充分（例如，一个粗略绘制的多边形将未受损的步道包括为瓦砾）。

研究人员模拟了他们观察到的地理空间标签噪声类型的训练数据集，以及随机标签噪声。他们比较了机器学习分类在这两种数据集上的表现，发现地理空间误标比随机误标使分类性能下降约五倍。

我们可以从这些研究中获得什么启示？

+   并非所有训练数据标签错误对机器学习系统性能的影响都相同。如果你的标签错误大多是随机的，它们对你的机器学习系统的伤害会较小。错误不会产生足够大的‘信号’来使训练偏离正确方向。

+   如果你的标签错误是结构化的，例如由于标签规则的反复错误应用，它们可能对你的机器学习系统非常有害。系统会将这些错误数据创建的模式当作正确标注来学习。

+   为了减少标签错误的影响：

    +   确保你的训练数据向机器学习系统提供强学习‘信号’，并具有足够量的准确标注样本。

    +   明确界定标签要求。这是绝对关键的——使用未能充分反映你在应用中所寻找的内容的标签进行训练将会破坏你的机器学习系统。

    +   选择一个技术高超的标注合作伙伴。提供满足要求的数据的专业知识与要求本身一样重要。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关话题

+   [处理文本数据中的噪声标签](https://www.kdnuggets.com/2023/04/dealing-noisy-labels-text-data.html)

+   [如何利用数据可视化为工作报告增添影响力…](https://www.kdnuggets.com/2022/08/data-visualization-add-impact-work-reports-presentations.html)

+   [IMPACT 2022: 数据可观察性峰会，10月25-26日](https://www.kdnuggets.com/2022/09/monte-carlo-impact-2022-data-observability-summit.html)

+   [从数据分析师到数据战略家：为创造影响力的职业路径](https://www.kdnuggets.com/2023/05/data-analyst-data-strategist-career-path-making-impact.html)

+   [IMPACT: 数据可观察性峰会将于11月8日回归，还有…](https://www.kdnuggets.com/2023/10/monte-carlo-impact-the-data-observability-summit-is-back)

+   [Monte Carlo 启动 IMPACT 2023，来自数据与AI先锋的主旨演讲](https://www.kdnuggets.com/2023/11/monte-carlo-is-kicking-off-impact-2023-keynotes-from-data-ai-pioneers)
