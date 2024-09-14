# OpenML：分享、发现和进行机器学习

> 原文：[`www.kdnuggets.com/2014/08/openml-share-discover-do-machine-learning.html`](https://www.kdnuggets.com/2014/08/openml-share-discover-do-machine-learning.html)

![openml](img/openml.jpg)

最近，[一篇有趣的论文](http://arxiv.org/abs/1407.7722)介绍了[OpenML](http://www.openml.org)，这可能提供了一种挖掘数据的替代方法。不要混淆。这里介绍的 OpenML 是一个开放科学平台，正如其名称所示，机器学习研究人员可以在这里分享他们的所有数据集、算法和实验。它的标志由四种颜色组成，每种颜色代表 OpenML 的一个重要部分。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织中的 IT 工作

* * *

**以下是 OpenML 的四个关键数字。**（截至 2014 年 8 月 11 日）

***[230 个数据集](http://www.openml.org/d?from=20)。***这些是机器学习的输入数据——如蘑菇数据库、垃圾邮件、字母图像识别等。每个数据集提供了属性的简要描述，如默认准确度、类别数、特征数等。

***[1172 个任务](http://www.openml.org/t)。***如果研究人员想要操作数据，则会创建任务。这些任务分为四种类型：监督分类、学习曲线、监督数据流分类和监督回归，具体取决于期望分享的结果类型。所有用户都可以下载并解决这些任务。

***[364 个流程](http://www.openml.org/f)。***流程是解决 OpenML 任务的算法、工作流或脚本的实现，通常通过插件完成。科学家们还可以上传实际代码或通过 URL 引用它，如果代码托管在 GitHub 或其他开源平台上。在每个流程页面上，都会比较该流程运行过的所有任务的结果。

***[24990 次运行](http://www.openml.org/r)。***尝试解决任务并获得所需输出称为一次运行。以运行 24980 为例。它在*任务 36*上执行*Flow weka.Bagging_SMO_PolyKernel(1)*，这是对数据集*segment*的*监督分类*。它还提供了评估结果，如 AUC、混淆矩阵、预测准确性等。人们可以轻松比较同一任务上的所有运行结果。

![openml_weka](img/openml_weka.jpg)

值得一提的是，OpenML 可以与其他机器学习工具集成，如 Weka、R，以便人们可以自动上传数据和代码。例如，在 Weka 中，我们可以添加多个任务和 Weka 算法进行运行。插件将下载所有数据，在每个任务上运行每个算法，然后自动将结果上传到 OpenML。手动运行上传目前正在开发中。人们只能通过插件或 API（Java/R）上传运行结果。

**OpenML 还是 Kaggle？**

开源平台的好处随着用户的增加而增长。当人们开始熟悉 OpenML 时，上述数字肯定会增加，或者当你阅读这篇文章时可能已经在增长。OpenML 的一个明显好处是研究人员可以定义自己的任务，并构建算法来解决其他任务。所有共享的结果都在线存储和组织，方便访问、重用和讨论。

你可能会想到 Kaggle，人们也可以在上面下载数据集并评估不同的算法。然而，OpenML 的设计目的是共享和比较研究结果。它侧重于合作，而不是竞争。在 Kaggle 竞赛中展示机器学习技能是个好主意。但只要进行足够的运行，OpenML 将是一个发现新知识的地方。

**Ran Bi** 是纽约大学数据科学项目的硕士生。在 NYU 学习期间，她完成了多个机器学习、深度学习及大数据分析的项目。她的本科背景是金融工程，因此也对商业分析感兴趣。

**相关：**

+   Prediction.io 开源机器学习服务器

+   MLlib：Apache Spark 的机器学习组件

+   当 Watson 遇到机器学习

### 更多相关话题

+   [发现计算机视觉的世界：介绍 MLM 的最新……](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [学习数据科学、机器学习和深度学习的完善计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [每个机器学习工程师都应该掌握的 5 项机器学习技能……](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [KDnuggets 新闻，12 月 14 日：3 门免费的机器学习课程……](https://www.kdnuggets.com/2022/n48.html)

+   [AI、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [打破数据壁垒：如何通过零样本、单样本和少样本学习来变革机器学习](https://www.kdnuggets.com/2023/08/breaking-data-barrier-zeroshot-oneshot-fewshot-learning-transforming-machine-learning.html)
