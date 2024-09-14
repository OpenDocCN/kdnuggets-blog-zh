# 强化学习：商业用例，第1部分

> 原文：[https://www.kdnuggets.com/2018/08/reinforcement-learning-business-use-case-part-1.html](https://www.kdnuggets.com/2018/08/reinforcement-learning-business-use-case-part-1.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Aishwarya Srinivasan](https://www.linkedin.com/in/aishwarya-srinivasan/)，深度学习研究员**

![](../Images/4d3186ef6bb45edf185fd49ae8531fc0.png)

强化学习的热潮始于DeepMind推出的AlphaGo，这是一款专为围棋设计的人工智能系统。从那时起，各家公司投入了大量的时间、精力和研究，今天强化学习已经成为深度学习中的热门话题。尽管如此，大多数企业在寻找强化学习的应用场景或将其纳入业务逻辑方面仍面临困难。这并不令人惊讶。到目前为止，它仅在无风险、可观察且易于模拟的环境中进行研究，这意味着像金融、健康、保险和技术咨询等行业不愿意冒着自己资金的风险来探索其应用。此外，强化学习中的“风险因素”对系统施加了很大的压力。Coursera的联合主席兼创始人Andrew Ng曾表示：“强化学习是一种对数据需求甚至超过监督学习的机器学习类型。获得足够的数据用于强化学习算法非常困难。还有更多工作需要完成，以将其转化为业务和实践。”

以这种略显悲观的观点为出发点，让我们利用本博客的第1部分深入探讨强化学习的技术方面。在第2部分中，我们将探讨一些可能的商业应用。从根本上说，RL是一个复杂的算法，用于将观察到的实体和度量映射到一组动作，同时优化长期或短期奖励。RL代理与环境互动，尝试学习策略，这些策略是获取奖励的决策或动作序列。实际上，RL在驱动与代理的互动时，会考虑即时奖励和延迟奖励。

强化学习模型由一个代理组成，该代理推断出一个动作，然后对环境进行操作以实现改变，动作的意义通过奖励函数来反映。这个奖励是为代理优化的，反馈会传递给代理，以便其评估下一步最佳行动。系统通过回顾在类似情况下采取的最佳行动，从之前的动作中学习。

![](../Images/dc56fb7c6dd7211a6e8198322d49ba7b.png)

图1：强化学习模型

从数学角度来看，我们可以将强化学习视为一个状态模型，特别是一个完全可观察的 [马尔可夫决策过程](https://en.wikipedia.org/wiki/Markov_decision_process)(MDP)。要理解 MDP 背后的概率理论，我们需要了解马尔可夫属性：

> *“在给定当前状态的情况下，未来与过去无关”*

马尔可夫属性用于不同结果的概率不依赖于过去状态的情况，因此只需要当前状态。一些人用“无记忆”来描述这种属性。在需要以前状态来决定结果的情况下，马尔可夫决策过程将不起作用。

模型的环境是一个随机有限状态机，代理发出的行动作为输入，环境向代理发出的奖励/反馈作为输出。整体奖励函数包括即时奖励和延迟奖励。即时奖励是行动对状态环境的定量影响。延迟奖励是行动对未来环境状态的影响。延迟奖励通过*‘折扣因子 (*γ*)’* 参数进行计算，0 < γ *<1。*折扣因子的较高值使系统倾向于远见奖励，而较低值使系统倾向于即时奖励。*X(t)* 是时间 ‘*t*’ 的环境状态表示。*A(t)* 是代理在时间 *‘t’* 采取的行动。

**状态转移函数**：在环境中，由代理发出的行动导致从一个状态转移到另一个状态。

![](../Images/24a69e2d466cc46f4c8b0aec565d4f33.png)

代理也被建模为一个随机有限状态机，其中环境发出的奖励是输入，而环境中为下一时间步发出的行动是输出。*S(t)* 是代理在时间 *‘t’* 收到环境反馈后的当前状态，反馈来自时间 *‘t-1’* 的环境应用，*A(t)* 表示使用模型学习通过整体奖励优化建立的策略。

![](../Images/b0a1910a2d6e85f55d95bc0ef5e14f7e.png)

**状态转移函数**：在代理中，由环境发出的奖励导致从一个状态转移到另一个状态。

![](../Images/95215638d0b466de150decf7b7738cc4.png)

**策略函数**：从代理到环境的策略/输出函数，根据奖励函数的优化给出行动。

![](../Images/1c220cddc57d1846a5dc09b1a9df5e05.png)

代理的目标是找到策略 P(pi)，以最大化带有折扣因子的整体期望奖励。

![](../Images/9a7f2bcea68aad3db6ff48c2f2f61a10.png)

使用 MDP 训练的代理尝试从当前状态获得最多的预期奖励总和。因此，需要获得最优值函数。贝尔曼方程用于值函数，分解为当前奖励和下一状态值的折扣值。

![](../Images/13c2d168962f7b7e9056ae04783b9efb.png)

我希望你现在已经了解了强化学习的技术方面！！

在本系列文章的下一部分，我们将查看作为金融行业业务案例的实际应用，即股票交易。

继续深入学习！

**简历：[Aishwarya Srinivasan](https://www.linkedin.com/in/aishwarya-srinivasan/)**: 数据科学硕士 - 哥伦比亚大学 || IBM - 数据科学精英 || 数据科学领域的独角兽 || Scikit-Learn 贡献者 || 深度学习研究员

[原文](https://medium.com/inside-machine-learning/reinforcement-learning-the-business-use-case-part-1-65976c745319)。经许可转载。

**相关内容：**

+   [你需要了解的5件强化学习的事情](/2018/03/5-things-reinforcement-learning.html)

+   [解释强化学习：主动 vs 被动](/2018/06/explaining-reinforcement-learning-active-passive.html)

+   [什么时候不应该使用强化学习？](/2017/12/when-reinforcement-learning-not-used.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

### 更多相关话题

+   [实操强化学习课程 第3部分：SARSA](https://www.kdnuggets.com/2022/01/handson-reinforcement-learning-course-part-3-sarsa.html)

+   [实操强化学习课程，第1部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-course-part-1.html)

+   [实操强化学习课程，第2部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-part-2.html)

+   [使用 Python 的自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [Chip Huyen 分享了实施 ML 系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [本土大型语言模型的案例](https://www.kdnuggets.com/the-case-of-homegrown-large-language-models)
