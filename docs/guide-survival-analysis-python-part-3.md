# 《Python生存分析完整指南》第3部分

> 原文：[https://www.kdnuggets.com/2020/07/guide-survival-analysis-python-part-3.html](https://www.kdnuggets.com/2020/07/guide-survival-analysis-python-part-3.html)

[评论](#comments)

**由[Pratik Shukla](https://medium.com/@shuklapratik22)，有志于成为机器学习工程师**。

### 系列索引

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

**[第1部分](https://www.kdnuggets.com/2020/07/complete-guide-survival-analysis-python-part1.html)：**

(1) 生存分析基础。

**[第2部分](https://www.kdnuggets.com/2020/07/guide-survival-analysis-python-part-2.html)：**

(2) Kaplan-Meier拟合器理论与示例。

(3) Nelson-Aalen拟合器理论与示例。

**第3部分：**

(4) 基于不同组的Kaplan-Meier拟合器。

(5) 带示例的Log-Rank检验。

(6) 带示例的Cox回归。

在上一篇文章中，我们讨论了如何分析患者的生存概率。但我们还需要知道哪个因素对生存的影响最大。因此，在这篇文章中，我们将讨论基于不同组的Kaplan-Meier估计器。

### 示例3：带组的Kaplan-Meier估计器

让我们将数据分为2组：男性和女性。我们的目标是检查如果我们按性别划分数据集，生存率是否存在显著差异。

**(1) 导入所需的库：**

![](../Images/a98e1d29859ed38f7aca99118ddd0ac4.png)

**(2) 读取数据集：**

![](../Images/0fd6f50d8f037d0116e8eaf2dc20a47e.png)

**(3) 组织我们的数据：**

![](../Images/508afac7e0a10ae0aed4e341cf3981bb.png)

**(4) 创建两个KaplanMeierFitter()对象：**

*kmf_m* 用于男性数据集。

*kmf_f* 用于女性数据集。

![](../Images/cb44ce5fa7c8ac486dc98f5fca31120f.png)

**(5) 将数据划分为组：**

![](../Images/a6678c79a22511f5485f9f7ae34cc0c5.png)

**(6) 将数据拟合到我们的对象中：**

![](../Images/3e57916c122d0ee976c60dd7f1d54bcc.png)

**(7) 生成事件表：**

![](../Images/4c09df0ddb93ef610fdbc3aa5afe1497.png)

*男性事件表*。

![](../Images/eaa7fd49bc96c6ee20538fde573a8d9c.png)

*女性事件表*。

**(7) 预测生存概率：**

现在我们可以预测两个组的生存概率。

![](../Images/e489ead25115d300d791611fd310862f.png)

**(8) 获取生存概率的完整列表：**

![](../Images/91c898c05a1581e4a61408408250e081.png)

![](../Images/20eb9f7edafe2c36f6c518f99d80053d.png)

**(9) 绘制图表：**

![](../Images/f7ad28ee270cedcf1f1b7af23c845377.png)

注意到女性存活于肺癌的概率高于男性。因此，从这些数据中，我们可以说医学研究人员应更多关注导致男性患者存活率较低的因素。

**（10）累积分布：**

它给出了一个人在某一时间点死亡的概率。

![](../Images/681efbf20c16afe80f6e5d7651eb1ac1.png)

![](../Images/c13339fe127e3d68ee919da091e95096.png)

**（11）绘制数据：**

![](../Images/9a6b1bdef159ba263ac27ad872620a2a.png)

**（12）风险函数：**

![](../Images/9f79b46a4c10175250897a0ee52cb7f8.png)

**（13）数据拟合：**

![](../Images/be0f4e9ce6162a53f6ac37ff13789d52.png)

**（14）累积风险：**

![](../Images/7911d97d14dcec5d8ae7009bf9ad8738.png)

![](../Images/65feb99dcce21e424e78fd193aa4a386.png)

**（15）绘制数据：**

![](../Images/6fa7fb18f9002851970a8921a24f4f9f.png)

### 对数秩检验

目标：在这里，我们的目标是查看被比较组之间是否存在显著差异。

**原假设**：原假设表明所研究的组之间没有显著差异。如果这些组之间存在显著差异，则必须拒绝我们的原假设。

**我们如何说存在显著差异？**

统计显著性由一个介于 0 和 1 之间的 p 值表示。p 值越小，所研究组之间的统计差异越大。请注意，我们的目标是找出我们比较的组之间是否存在差异。如果存在，我们可以基于各种信息如饮食、生活方式等进一步研究为何某个组的存活机会较低。

**小于（5% = 0.05）P**-**值**表示我们比较的组之间存在显著差异。我们可以根据性别、年龄、种族、治疗方法等对组进行划分。

**这是一种找出 P 值的方法。**

![](../Images/b421214d381f53a72f37a8a2d1ff604a.png)

![](../Images/d3c7c092c02af7b018b73416d73f9c4a.png)

在这里，我们将通过著名的对数秩检验方法比较两个不同组的生存分布。请注意，对于我们的组，检验统计量为 10.33，P 值显示为（<0.005），这是统计上显著的，表示我们必须拒绝原假设，并承认两个组的生存函数存在显著差异。P 值为“性别”与生存天数相关提供了有力证据。简而言之，我们可以说在我们的示例中，“性别”对生存天数有主要贡献。

汇总起来：

**[你可以从这里下载 Jupyter 笔记本。](https://drive.google.com/open?id=1Vj9UNMfcsqP3S7D3-oQRjEuc2Gnw43dF)**

### 示例 4：Cox 比例风险模型

Cox比例风险模型基本上是一个回归模型，通常由医学研究人员用来找出受试者的生存时间与一个或多个预测变量之间的关系。简而言之，我们想了解年龄、性别、体重、身高等不同参数如何影响受试者的生存时间。

在前一节中，我们了解了Kaplan-Meier、Nelson-Aalen和Log-Rank检验。但在那些方法中，我们只能逐个考虑变量。而且需要注意的是，我们只对性别、状态等分类变量进行操作，这些变量通常不用于像年龄、体重等非分类数据。作为解决方案，我们使用Cox比例风险回归分析，**它适用于定量预测（非分类）变量和分类变量。**

**为什么我们需要它？**

在医学研究中，通常我们考虑多个因素来诊断一个人的健康状况或生存时间，即我们通常使用性别、年龄、血压和血糖等因素，以找出不同组之间是否有显著差异。例如，如果我们根据一个人的年龄对数据进行分组，那么我们的目标是找出哪个年龄组的生存机会更高。是儿童组、成人组，还是老年组？现在我们需要找出我们如何分组。为此，我们使用Cox回归分析，找出不同参数的系数。让我们看看它是如何工作的！

**Cox比例风险方法的基础：**

Cox比例风险方法的**最终目的**是观察数据集中不同因素对感兴趣事件的影响。

**风险函数：**

![](../Images/f5ccb83ec979341166f78e8ba57fe5ef.png)

**exp(bi)的值称为风险比（HR）**。HR大于1表示，当第i个协变量的值增加时，事件风险增加，从而生存期减少。

总结一下，

![](../Images/714aec5c8f27b924fbe7cdfa46ba2001.png)

**让我们编写代码：**

**(1) 导入所需库：**

![](../Images/a98e1d29859ed38f7aca99118ddd0ac4.png)

**(2) 读取CSV文件：**

![](../Images/d02b904ec8a401506c29026ba1a4b71a.png)

**(3) 删除包含空值的行：**

在这里，我们需要删除包含空值的行。我们的模型无法处理含有空值的行。如果不对数据进行预处理，我们可能会遇到错误。

![](../Images/9a5b72f1ea4163588cddeb3fb031d9df.png)

**(4) 创建KapanMeierFitter对象：**

![](../Images/a2f8f82d9f5136fa91f968a2578c4d9e.png)

**(5) 组织数据：**

![](../Images/cbcc1bde9cfa0ee11eba7666b9a9a118.png)

**(6) 拟合值：**

![](../Images/4ca2a13c145dc5fd55db4061376a4537.png)

**(7) 事件表：**

![](../Images/8695f88461fbf9a294a12a6d18364657.png)

**(8) 导入Cox回归库：**

![](../Images/1a68dade41fffb969bf73052d30fce2c.png)

**（9）在拟合我们的模型时需要考虑的参数：**

![](../Images/2a1e8a8c38f083dd0216d2f2fe99f4cb.png)

**（10）拟合数据并打印摘要：**

我们的模型将考虑所有参数，以找到相应的系数值。

![](../Images/3d6452bad3e1f1d48867c0f0d5b57cc9.png)

![](../Images/23ddc30db01973db3a506386be10b8da.png)

注意不同参数的 p 值，我们知道 p 值（<0.05）被认为是显著的。你可以看到 sex 和 ph.ecog 的 p 值都 <0.05。因此，我们可以根据这些参数对数据进行分组。

**HR（风险比） = exp(bi)**

性别的 p 值为 0.01，HR（风险比）为 0.57，表明患者性别与死亡风险降低之间存在很强的关系。例如，在其他协变量保持不变的情况下，**女性（性别=2）将风险降低了 0.58 倍，即 42%。** 这意味着女性的生存几率更高。注意，我们使用前一部分的图表得出了这个结论。

ph.ecog 的 p 值为 <0.005，HR 为 2.09，表明 ph.ecog 值与死亡风险增加之间有很强的关系。在其他协变量保持不变的情况下，较高的 ph.ecog 值与较差的生存有关。**这里 ph.ecog 值较高的人有 109% 更高的死亡风险。** 所以，简而言之，我们可以说医生试图通过提供相关药物来降低 ph.ecog 的值。

现在注意到年龄的 HR 是 1.01，这表明年龄较大组的风险仅增加 1%。所以我们可以说不同年龄组之间没有显著差异。

简而言之，

![](../Images/714aec5c8f27b924fbe7cdfa46ba2001.png)

**（11）检查图表中哪个因素影响最大：**

你可以清楚地看到 ph.ecog 和性别变量有显著差异。

![](../Images/ca785dd72a220b6628e7c982ea0c9c41.png)

**（12）绘制图表：**

在这里，我绘制了我们数据集中不同个体的生存概率图。注意，person-1 的生存几率最高，而 person-3 的生存几率最低。如果你查看主要数据，你会发现 person-3 的 ph.ecog 值较高。

注意，即使 person-5 仍然活着，由于其较高的 ph.ecog 值，他/她的生存概率较低。

![](../Images/af0a4200fb92a0718e7dbd4f20520e36.png)

**（13）找出时间线的中位事件时间：**

注意，随着时间的推移，中位生存时间在减少。

![](../Images/e2ff499174e128e650fd0cd1f645d487.png)

**将所有内容整合在一起：**

**[你可以从这里下载 Jupyter 笔记本。](https://drive.google.com/open?id=1Vj9UNMfcsqP3S7D3-oQRjEuc2Gnw43dF)**

[原文](https://medium.com/@shuklapratik22/a-complete-guide-to-survival-analysis-in-python-956588e4dd17)。已获得许可转载。

**个人简介：**[Pratik Shukla](https://medium.com/@shuklapratik22)是一名有抱负的机器学习工程师，喜欢将复杂的理论简化表达。Pratik完成了计算机科学的本科学业，并计划在南加州大学攻读计算机科学硕士学位。“瞄准月球。即使错过了，你也会落在星星之间。-- 莱斯·布朗”

**相关内容：**

+   [Python生存分析完整指南，第1部分](https://www.kdnuggets.com/2020/07/complete-guide-survival-analysis-python-part1.html)

+   [Python生存分析完整指南，第2部分](https://www.kdnuggets.com/2020/07/guide-survival-analysis-python-part-2.html)

+   [业务分析中的生存分析](https://www.kdnuggets.com/2017/11/survival-analysis-business-analytics.html)

### 更多主题

+   [数据科学备忘单完整合集 - 第1部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-1.html)

+   [数据科学备忘单完整合集 - 第2部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-2.html)

+   [数据仓库完整合集 - 第1部分](https://www.kdnuggets.com/2022/04/complete-collection-data-repositories-part-1.html)

+   [数据仓库完整合集 - 第2部分](https://www.kdnuggets.com/2022/04/complete-collection-data-repositories-part-2.html)

+   [数据科学书籍完整合集 - 第1部分](https://www.kdnuggets.com/2022/05/complete-collection-data-science-books-part-1.html)

+   [数据科学书籍完整合集 - 第2部分](https://www.kdnuggets.com/2022/05/complete-collection-data-science-books-part-2.html)
