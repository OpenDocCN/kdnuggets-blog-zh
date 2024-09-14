# 优秀的数据科学家不仅仅是跳出框框思考，他们重新定义了框框

> 原文：[https://www.kdnuggets.com/2018/03/great-data-scientists-think-outside-redefine-box.html](https://www.kdnuggets.com/2018/03/great-data-scientists-think-outside-redefine-box.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

*特别感谢Michael Shepherd，AI研究策略师，Dell EMC服务，感谢他的共同创作。了解更多关于Michael的信息请见本文底部。*

想象一下你想确定在某个房子上添加太阳能电池板可以产生多少太阳能。这就是Google的[Project Sunroof](https://www.google.com/get/sunroof#p=0)使用深度学习来做的。输入一个地址，Google使用深度学习框架来估计你在20年内使用太阳能电池板可以节省多少能源费用（见图1）。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

![](../Images/b851cc48a08423b6e5605eeeff57eb5c.png)

图1：Google Project Sunroof 项目

这是深度学习的一个非常酷的应用。但是，让我们假设“可能”有一种更好的方法来估计太阳能节省。例如，你想用深度学习来估计在金门大桥上安装太阳能电池板可以产生多少太阳能（这在旧金山可能不会是一个非常受欢迎的决定）。显而易见的应用是分析金门大桥的几张照片，并基于云层覆盖估计晴天情况。

但是，如果我们不基于“云层覆盖”来估计潜在的太阳能发电量，而是想用“阳光反射”来生成太阳能估计（见图2），会怎样呢？

![](../Images/6a987ebbdccf88593bd27a15cae6a4dc.png)

图2：确定金门大桥的最佳预测变量

或者你可能想基于大桥生成的“阴影的清晰度”测试另一种指标？或者基于照片中有多少人戴着太阳镜测试另一种指标？又或者基于…

你如何知道这些变量——云层、反射、阴影、太阳镜或其他任何东西——哪个是更好的太阳能发电预测指标？你可以尝试所有的！

这种思维过程突显了顶级数据科学家的一个重要行为特征；顶级数据科学家不仅具有“跳出框框”的强大想象力——而且实际上在尝试寻找***可能***更好地预测性能的变量和指标时重新定义框框。

“可能”这个词是一个强大的推动者。“可能”用来表示或指示某事是可能的。它是数据科学家最重要的概念，因为“可能”赋予数据科学家探索、犯错、学习和再次尝试的自由。

### “不可能完成”不是数据科学家的术语

人工智能的远见者和我们许多人心目中的无畏领袖安德鲁·吴，最近撰写了一篇题为“[人工智能现在能做什么和不能做什么](https://hbr.org/2016/11/what-artificial-intelligence-can-and-cant-do-right-now)”的文章。在文章中，安德鲁表示：

*“令人惊讶的是，尽管人工智能的影响范围广泛，但它应用的类型仍然非常有限。人工智能的几乎所有最新进展都是通过一种类型，其中一些输入数据（A）被用来快速生成一些简单的响应（B）。例如：”*

![](../Images/3e0004e7ec261eb2fb56722cc1abda17.png)

图3：机器学习能做什么

虽然当前的应用案例有限**今天**，但数据科学家在利用大数据和现有的机器学习和深度学习技术方面的创造力令人震惊。让我举一个例子，说明我们的 Dell EMC 服务团队中的数据科学家如何跳出框框，发现帮助我们的客户避免 IT 环境问题和创造更轻松支持体验的新方法。

### 预测硬盘故障

假设你在设备的整个生命周期内，每分钟捕获超过260个不同的遥测数据点。大多数这些260多个变量的数据不完整或稀疏，数据收集的时间不总是整齐一致，并且在设备之间保持时间连续性是一个重大挑战。

如果你使用传统的机器学习算法，数据科学团队将不得不花费大量时间 1) 基于领域知识进行特征工程，创建新的变量，以及 2) 通过试错法来确定哪些变量组合应该包含在机器学习模型中。

相反，我们的 Dell EMC 服务数据科学家采用了专利待批的深度学习方法来“像素化”数据。他们将260多个变量转化为设备性能“图像”。然后，一旦创建了这些“图像”，团队利用递归神经网络从随机像素中寻找“形状”和可重复的模式（见图3）。

![](../Images/4a2a341a1e3a919d767796f25a18158b.png)

图4：像素化遥测数据

循环神经网络（RNN）是一类人工神经网络，其中单元之间的连接形成一个有向循环。RNN 可以利用其内部记忆处理任意序列的输入，这通常使得 RNN 适合用于手写或语音识别。除了在这种情况下，数据科学团队使用 RNN 来***解码***看似随机的像素，以预测设备状态（见图4）。

![](../Images/a453a47a504e2ef87bb0504a4f05980c.png)

图5：使用 RNN 识别埋藏在遥测数据中的形状和模式

我喜欢这个例子，因为团队没有觉得必须把方形 peg 适配到圆形的“机器学习”孔里。相反，他们在不同的背景下使用深度学习，将看似随机的像素解码为设备健康的预测。数据科学家没有等到有人开发出更好的机器学习算法。相反，他们查看了广泛的机器学习和深度学习工具及算法，并将它们应用于一个不同但相关的使用案例。如果我们可以预测设备的健康状况和可能出现的问题，那么我们也可以帮助客户预防这些问题，显著提升他们的支持体验，并对他们的环境产生积极影响。

### 摘要

![](../Images/966b26cac6d10031893b97d57112e415.png) 数据科学家最重要的特征之一是他们拒绝接受“无法完成”的回答。他们愿意尝试不同的变量和指标，以及各种高级分析算法，以查看是否有其他方式来预测性能。

顺便提一下，我包含这张图片是因为我觉得它很酷。这个图形测量了不同IT系统之间的活动。就像数据科学一样，这张图显示了在构建机器学习和深度学习模型时，没有缺乏需要考虑的变量！

想了解更多关于 [戴尔EMC服务](http://www.dell.com/en-us/work/learn/it-support-lifecycle) 如何使用数据科学的信息吗？

查看 Doug Schmitt（戴尔EMC全球服务总裁）撰写的 “[解码客户DNA与数据科学](https://infocus.emc.com/doug_schmitt/decoding-customer-dna-with-data-science/)” 博客，并关注即将推出的播客 “与两位数据极客的对话”，直接听取我们变革性技术背后的数据科学家的见解。

![合著者](../Images/6848e79b83bdbce1d04c351f9c466007.png) 我想感谢我的合著者迈克尔·谢泼德，戴尔EMC服务的人工智能研究战略家。迈克尔拥有硬件和软件方面的美国专利，并且是一位技术布道者，通过变革性的人工智能数据科学提供远见。凭借在供应链、制造和服务方面的经验，他喜欢展示实际场景，使用SupportAssist智能引擎展示如何在“思维速度”下运行的预测性和主动性人工智能平台在各行各业中都是可行的。

[原文](https://infocus.dellemc.com/william_schmarzo/great-data-scientists-dont-just-think-outside-the-box-they-redefine-the-box/)。经授权转载。

**相关：**

+   [为什么使用数据分析来预防，而不仅仅是报告](/2017/12/data-analytics-prevent-report.html)

+   [让人工智能、深度学习、机器学习民主化，使用戴尔EMC Ready解决方案](/2018/01/democratizing-ai-deep-learning-machine-learning-dell-emc.html)

+   [数据货币化的关键](/2017/07/key-data-monetization.html)

### 更多相关话题

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [什么是大型语言模型，它们是如何工作的？](https://www.kdnuggets.com/2023/05/large-language-models-work.html)

+   [什么是基础模型，它们是如何工作的？](https://www.kdnuggets.com/2023/05/foundation-models-work.html)

+   [什么是向量数据库，它们为何对LLMs至关重要？](https://www.kdnuggets.com/2023/06/vector-databases-important-llms.html)

+   [你的特征重要吗？这并不意味着它们是好的](https://www.kdnuggets.com/your-features-are-important-it-doesnt-mean-they-are-good)

+   [边界框深度学习：视频标注的未来](https://www.kdnuggets.com/2022/07/bounding-box-deep-learning-future-video-annotation.html)
