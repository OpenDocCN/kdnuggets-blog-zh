# 数据科学技能的4个象限和创建病毒式数据可视化的7个原则

> 原文：[https://www.kdnuggets.com/2019/10/4-quadrants-data-science-skills-data-visualization.html](https://www.kdnuggets.com/2019/10/4-quadrants-data-science-skills-data-visualization.html)

[评论](#comments)

**作者：[何塞·贝伦格雷斯](https://www.linkedin.com/in/berengueres/)，教授及天使投资人**。

（关于 [哪些数据科学技能是核心的，哪些是热门/新兴的？](https://www.kdnuggets.com/2019/09/core-hot-data-science-skills.html) 的后续文章）

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

我教授计算机科学和设计思维。但今天，我的任务是展示如何制作优秀的图表，因为我不喜欢令人困惑的图表。你可以把我看作是图表界的玛丽·孔多，或者是迪拜的科尔·克纳弗里克，你可能不会完全错。在这篇文章中，我将分享**7个原则**，帮助你从*Aha-图表*转变为*Wow-图表*。让我们用最近的一项 KDnuggets 调查数据来动手实践吧。

调查只有两个问题：

1.  你目前具备哪些技能/知识领域？以及

1.  你想要添加或提高哪些技能？

KDnuggets 收到了 1,500 个回答，我们将使用按技能汇总的数据 [[1]](#_ftn1)。

*表 1\. 调查汇总。*

| **技能** | **%拥有** | **%希望拥有** | **比例** |
| --- | --- | --- | --- |
| Python | 71.2% | 37.1% | 0.52 |
| 数据可视化 | 69.0% | 25.3% | 0.37 |
| 批判性思维 | 66.7% | 15.5% | 0.23 |
| Excel | 66.5% | 4.6% | 0.07 |

| 30 行数据未显示 …

…

… |  |  |  |

假设你被要求可视化这项调查。你会怎么做？第一个机器学习直觉是做一个散点图来识别有趣的聚类。X轴可以是拥有某项技能的受访者的百分比，Y轴则是希望拥有该技能的受访者的百分比。然而，数据点太多，人类很难理解。这是信息超载的经典案例。

![](../Images/1706370118beaf280a4c92aa7c1bf34e.png)

*图 1\. 信息超载的受害者？*

### 原则 1: 制作以人为本的图表

如何制作更具人性化的图表？我们可以借鉴 [Gartner 魔力象限](https://en.wikipedia.org/wiki/Magic_Quadrant)，他们通过使用象限层级来降低复杂性到人类水平 [[2]](#_ftn2)。

![](../Images/716fa891cd89699d1e7308d2d4946932.png)

*图 2\. Gartner 使用象限来减少复杂性。*

### 原则 2：不要与重力或惯例作斗争

关于图表有一些不成文的规则。其中一个是关于重力的。Y轴与重力隐喻对齐（高度想要，高Y）。另一个不成文的规则（这是Guy Kawasaki提出的）是“你想要（期望目标）在图表的右上角[[3]](#_ftn3)”。在这种情况下，最受欢迎的技能（深度学习）在错误的一侧——我们需要翻转X轴。

![](../Images/171ff32f9299d8233a34789f6b0db435.png)

*图3\. 目标应该是“高且偏右”——Guy Kawasaki*

### 原则 3：让你的图表讲述一个令人难忘的故事

如果你制作了一个图表而没有人记得它，那它还发生了吗？早期我们用类别赋予意义，但如果没有人记得这些类别，它们有什么用？帮助你的观众记住图表的一种方法是使用人物角色（在Z世代的说法中是梗）。让我们应用一些人物角色和聚类原则。四个象限可能意味着：

+   **不需要的技能**（拥有但不想要 = Excel）

+   **不需要的技能**（不想要且没有 = JAVA）

+   **热门技能**（想要但没有 = TF）

+   **喜爱的技能**（想要且拥有 = Python）

![](../Images/feb1a3098ed88ff1cabfa1d2aa405a55.png)

*图4\. 流行文化：用来让你的图表更具吸引力。*

### 原则 4：像玛丽·孔多一样整理

象限创造意义并减少复杂性，但玛丽·孔多的工作还没有完成。让我们通过突出每个象限中的一种技能来进一步整理。在这里我们使用红色框，但如果你觉得时尚的话，我们也可以使用更大的字体或粗体字母。

![](../Images/53f0b2d55c9af38adb8c099fda4b1865.png)

*图5\. 使用层级结构来创造意义。*

### 原则 5：从“为什么”开始

最后，图表应该有一个目的——这就是Simon Sinek[[4]](#_ftn4)所称的**为什么**。

我的观点是“我希望我的课堂上看到更多的Python，少一点Java”。

### 原则 6：标题是你图表的最佳朋友

![](../Images/f8f23e56cd7bd3ad834c646fded9cb7d.png)

*图6\. 清晰的沟通胜过风格。*

不要害怕在画布上做标记！标题是给你的故事增添意义和冲击力的机会。注意我们用“被忽视”的标签打破了对称性，这对图表而言就像风水一样。

### 原则 7：如有疑问：预测未来

![](../Images/5b75fe1590e4fab89e76f23a28daf0b1.png)

*图7\. 如果我知道现在知道的事……*

假设你的前一个图表获得了病毒式传播。我们还能可视化什么呢？如果你没有更多的想法，那么这里有一个：预测未来[[5]](#_ftn5)。我们可以将**拥有**的普及率视为当前的快照，将（想要）[[6]](#_ftn6)视为如果所有受访者能够实现他们的学习目标时的未来预测。现在我们可以思考当前和未来之间的差距。这个图表是在ggplot2中完成的。幸运的是，ggplot2在绘制负堆叠条形图和正堆叠条形图时的方法在这里非常有用。还要注意，我们避免了按升序排列条形图的诱惑，因为这会在图表的波峰处产生虚假的模式。

### 综合考虑

现在让我们将角色、等级制度、意义构建策略和商业徽标结合在一个所谓的**战略图**中。

![](../Images/9dd9ad058dcf53904df9ea9f1194fc54.png)

*图 8. 为什么有些公司有时会支持免费的产品？*

### 结论

数据科学家比其他专业人士更有可能传播知识。数据可视化就是其中的一种方式。不幸的是，有很多优秀的工作被忽视了，比如[这个](https://usafacts.org/)、[这个](http://letsfreecongress.org/)或[这个](https://www.climate-lab-book.ac.uk/2016/spiralling-global-temperatures/)。我希望这些新的超能力能激励你用引人入胜的图表开始改变世界。

*脚注：*

[[1]](#_ftnref1) 查看 csv 数据 [https://www.kaggle.com/harriken/kdnuggets-magic-skills](https://www.kaggle.com/harriken/kdnuggets-magic-skills)。

[[2]](#_ftnref2) [https://en.wikipedia.org/wiki/Magic_Quadrant](https://en.wikipedia.org/wiki/Magic_Quadrant) 根据乔丹·彼得森的说法，人类深刻理解并接受等级制度。这也是组织和整理图表的一个好方法。

[[3]](#_ftnref3) 盖·川崎，《创业的艺术》，TIECON 2006

[[4]](#_ftnref4) 西蒙·西内克，《从为什么开始》。

[[5]](#_ftnref5) 如果你是对的，那么至少你会出名。

[[6]](#_ftnref6) 未来（预测）流行度 = 现在的水平 + 需求，如果假设没有遗忘、人口静态，并且所有想学的人都会最终成功，那么这就100%正确。可能真相在两者之间，但排名而不是精确度是这张图表的重点。

**简介：**[Jose Berengueres](https://www.linkedin.com/in/berengueres/) 在东京工业大学获得了生物启发机器人学的硕士和博士学位。他目前在阿联酋的计算机科学系工作，教授敏捷、精益 UX 和设计思维。他还在迪拜辅导企业家，教授大学孵化器项目，并且是 PyData Dubai 的创始人。他曾在 Apple、比勒费尔德大学、墨西哥 CEDIM 和迪拜 Hult 商学院教授设计思维、大数据分析和商业模型。他为各种公司如 Awok、Etihad 和新加坡的 Healint 提供 UX 设计和数据科学咨询。2007-2008 年，他开发了两个初创公司（一个视觉 Twitter 和一个照片分享网站）。他是《数据可视化与讲故事导论》（2019）、《设计思维棕皮书》和《草图思维》的作者。他的研究重点是创造力、人机交互、用户体验和数据科学。Jose 是 Kaggle 大师。

**相关：**

+   [哪些数据科学技能是核心技能，哪些是热门/新兴技能？](https://www.kdnuggets.com/2019/09/core-hot-data-science-skills.html)

+   [为什么数据可视化是数据分析师武器库中最重要的技能](https://www.kdnuggets.com/2019/08/simpliv-data-visualization-data-analyst.html)

+   [数据科学家进行高级数据可视化的简便方法](https://www.kdnuggets.com/2019/08/advanced-data-visualisation-data-scientists.html)

### 更多相关话题

+   [停止学习数据科学来寻找目的，并找到目的…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个90亿美元的AI失败，进行了审视](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
