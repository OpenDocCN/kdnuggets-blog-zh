# 学习人工智能的路径

> 原文：[https://www.kdnuggets.com/2017/05/path-learning-artificial-intelligence.html](https://www.kdnuggets.com/2017/05/path-learning-artificial-intelligence.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由 Kirill Eremenko 和 Hadelin de Ponteves 提供，Super Data Science。**

学习人工智能的路径通常被复杂的数学和技术话题所压倒。但不必如此……我们想通过创建一个直观而令人兴奋的课程来打破这种趋势，该课程将引导你进入蓬勃发展的人工智能世界，同时享受乐趣：

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

**点击这里加入人工智能在线课程** [链接: [http://bit.ly/2BuildAI](http://bit.ly/2BuildAI) ]

就在此刻，我们正在进行一个 Kickstarter 项目，以创建一个革命性的人工智能培训项目。在这篇博客中，我们将描述课程结构背后的秘密，以便即使你还没有准备好加入培训——你也可以在自己的学习计划中复制这些步骤。

![](../Images/2d2d6160970f9995ca497248743429da.png)

**图. 人工智能、机器学习与深度学习**

在这次人工智能之旅中，我们将实现四个层级的人工智能，从简单到高级：

1.  **人工智能等级 1: Q 学习**

最简单的 AI 算法之一是 Q 学习。简单但强大，我们将使用它来训练像 R2D2 这样的机器人找到迷宫的出口。这将是我们在课程中制作的第一个 AI，只是为了热身，同时享受乐趣。

1.  **人工智能等级 2 – 深度 Q 学习**

然后我们将通过研究深度 Q 学习（DQN）将事情提升到下一个层级。通过 DQN，我们将能够解决一个更具挑战性的问题：征服流行的游戏《打砖块》！为了完成这个挑战，我们的输入将是编码值的向量，所有这些都描述了环境的一个状态，即：球的位置坐标、球的运动方向向量坐标，以及每个砖块是否存在的二进制值，其中1表示砖块仍在，0表示砖块已不存在。

![](../Images/8ecd060cdff02b9b28f8a2e4fe1b464a.png)

**图. 人工智能玩《打砖块》**

深度 Q 学习的理念是将 Q 学习算法与神经网络结合起来。我们的输入是编码向量。它们进入一个神经网络，输出将是要执行的动作。

这已经是一个更高级的人工智能，但我们可以更进一步：如果输入不是一些编码向量，而是游戏中我们看到的实际图像呢？这就引出了 AI 级别

1.  **AI 第 3 级 – 深度卷积 Q 学习**

开始吧！通过这个，我们将构建一个非常接近人类玩游戏的人工智能。由于编码向量无法保留图像的空间结构，这不是描述状态的最佳形式。空间结构确实很重要，因为它为我们提供了更多的信息来预测下一个状态，而预测下一个状态对于人工智能了解正确的下一步至关重要。因此，我们需要保留空间结构，为此，我们的输入必须是 3D 图像（2D 的像素数组加上一个额外的颜色维度）。在这种情况下，输入就是屏幕上的图像，与人类玩游戏时看到的完全相同。按照这个类比，人工智能的行为像人类：它观察游戏时屏幕的输入图像，输入图像进入一个卷积神经网络（相当于人类的大脑），该网络将检测每张图像中的状态，然后通过 Q 学习预测下一个状态，人工智能/人类将预测最佳动作。而这个动作再次是神经网络的输出。

我们将构建这个高度先进的人工智能，挑战在非常受欢迎的游戏《毁灭战士》中通过一个关卡。

1.  **AI 第 4 级 – 异步演员-评论员代理（A3C）**

如果环境中有多个代理需要训练呢？在这种情况下，最佳的人工智能是 A3C，这是人工智能领域的热门话题，由 Google DeepMind 去年推出。我们希望在同一地图上实现多个自动驾驶汽车的 AI。我们将训练汽车避免相互碰撞并避开障碍物。这将是结束这段 AI 之旅的一个非常激动人心的挑战！

![](../Images/372895dc6ee93817807740dc5093da88.png)

**加入我们**

这是我们在全新人工智能课程中将采用的方法。如果你想为自己的学习计划构建类似的内容，可以随意复制……但与他人一起学习会更加令人兴奋。

我们的 Kickstarter 项目已经得到了超过 1,500 名学生的支持。距离截止日期仅剩几天——快来加入我们，获取课程及大量早鸟奖励！

**了解如何构建人工智能** [链接: [http://bit.ly/2BuildAI](http://bit.ly/2BuildAI) ]

人工智能是一项每个人都应该可以掌握的技能，这不仅是学习 AI 的机会，也是站在下一个工业革命前沿的机会。

此致敬礼，

Hadelin de Ponteves & Kirill Eremenko

##### 注：图片 #1 和 #3 的版权归 SuperDataScience Pty Ltd 所有。KDNuggets 在本博客及任何相关宣传媒体中的使用已获批准。

**作者简介：** **[Kirill Eremenko](https://www.linkedin.com/in/keremenko/?ppe=1)** 是一位多语言企业家，在教育领域有3年的经验，在数据科学领域有7年的经验。而**[Hadelin de Ponteves](https://www.linkedin.com/in/hadelin-de-ponteves-1425ba5b/)** 是Google的数据工程师。

**相关：**

+   [什么是人工智能？智能的组成成分](/2017/04/grakn-artificial-intelligence-ingredients-intelligence.html)

+   [人工智能与深度学习的进展：DeepMind、Facebook 和 OpenAI](/2017/05/advances-ai-deep-learning-deepmind-facebook-openai.html)

+   [人工智能简史](/2017/04/brief-history-artificial-intelligence.html)

### 更多相关主题

+   [免费人工智能与深度学习速成课程](https://www.kdnuggets.com/2022/07/free-artificial-intelligence-deep-learning-crash-course.html)

+   [从人工智能到机器学习的演变](https://www.kdnuggets.com/2022/08/evolution-artificial-intelligence-machine-learning-data-science.html)

+   [基于人工智能的系统中的不确定性量化](https://www.kdnuggets.com/2022/04/uncertainty-quantification-artificial-intelligencebased-systems.html)

+   [人工智能如何变革数据整合](https://www.kdnuggets.com/2022/04/artificial-intelligence-transform-data-integration.html)

+   [2022年最受欢迎的人工智能技能](https://www.kdnuggets.com/2022/08/indemand-artificial-intelligence-skills-learn-2022.html)

+   [AI指数报告概述：衡量人工智能的趋势](https://www.kdnuggets.com/2023/04/overview-ai-index-report-measuring-trends-artificial-intelligence.html)
