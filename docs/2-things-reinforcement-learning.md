# 你需要知道的关于强化学习的 2 件事——计算效率和样本效率

> 原文：[`www.kdnuggets.com/2020/04/2-things-reinforcement-learning.html`](https://www.kdnuggets.com/2020/04/2-things-reinforcement-learning.html)

评论

### 深度学习的高成本

你是否曾因为空调太冷而穿上毛衣？是否忘记在上床睡觉前关掉另一间房间的灯？你每天通勤超过 30 分钟只是为了“填补办公室的座位”，尽管你在工作中做的一切都可以通过家里的笔记本完成？

![](img/c9abbeb0f5af3bd45a17a2f8e048b315.png)

*在强化学习中样本效率和计算效率之间的反直觉权衡中，选择进化策略可能比看起来更明智。*

现代生活充满了大大小小的低效，但我们深度学习研究/热情/企业的能源成本并不那么明显。有了高性能的工作站塔，散热风扇吹出的热空气始终提醒着能源使用的持续性和规模。但是，如果你把作业发送到走廊里的共享[集群](https://www.exxactcorp.com/Deep-Learning-HPC-Clusters)，直观上感受到设计决策对电费的影响就更困难了。如果你为大型工作使用云计算，能源使用和随之而来的后果更是被抽象化了。但“眼不见，心不烦”并不是在极端情况下，可能代表比整车生命周期更大的能源开销的项目决策的可行政策。

### 训练深度学习模型的能源需求

[Strubell et al.](https://arxiv.org/abs/1906.02243)在 2019 年引起了对训练现代深度学习模型的巨大能源需求的关注。虽然他们主要关注了一种叫做变换器的大型自然语言处理模型，但他们论文中讨论的考虑和结论应该大致适用于在类似硬件上训练其他任务的深度模型，大致时间也相同。

他们估计，从头开始训练大型的[Vaswani et al.](https://arxiv.org/abs/1706.03762)变换器大约排放了相当于从纽约到旧金山的航班约 10%的二氧化碳排放，这基于主要云服务提供商发布的能源组合的一些假设，财务成本略低于 1000 美元（假设使用云计算）。对于一个决定业务方向的项目，这可能是值得的支出，但当考虑到调整和实验时，能源费用很容易被放大 10 倍或更多。

Strubell 及其同事估计，将[神经架构搜索（NAS）](https://arxiv.org/abs/1901.11117)添加到 NLP 模型开发中需要数百万美元的费用，并且具有相应的碳足迹。虽然许多最大的模型利用专用硬件，如 Google 的 TPU 进行训练，这可能将能源成本降低[30 到 80 倍](https://cloud.google.com/blog/products/gcp/an-in-depth-look-at-googles-first-tensor-processing-unit-tpu)和训练价格降低一个稍低的倍数，但这仍然是一个巨大的成本。在我们关心的强化学习领域中，低效训练的后果可能是一个失败的项目、产品或业务。

### DeepMind 使用《星际争霸 II》的训练示例

在 44 天的时间里，[Deepmind 训练](https://deepmind.com/blog/article/AlphaStar-Grandmaster-level-in-StarCraft-II-using-multi-agent-reinforcement-learning) 代理，以在多人实时战略游戏《星际争霸 II》中获得三种可玩种族的宗师地位，击败了 99.8%的所有玩家。在 Battle.net 排行榜中名列前茅。OpenAI 针对 Dota 2 的主要游戏掌握项目使用了 10 个实时月（大约 800 拍次/天）的训练时间来[击败世界冠军、人类玩家](https://openai.com/blog/openai-five-defeats-dota-2-world-champions/)。

由于使用了 TPU、虚拟工人等，准确估算这些引人注目的游戏玩家的能量成本并不容易，但估算的范围在[12](https://medium.com/swlh/deepmind-achieved-starcraft-ii-grandmaster-level-but-at-what-cost-32891dd990e4)到[1800 万美元](https://www.aitrends.com/business/openai-establishes-for-profit-company/)的云计算账单，分别用于训练冠军 Alphastar 和 OpenAI Five。显然，这对于典型的学术或行业机器学习团队来说是不可企及的。在强化学习领域中，另一个危险是低效学习的出现。一个试图学习中等复杂任务但探索不佳且样本效率低的代理可能永远找不到可行的解决方案。

### 使用强化学习预测蛋白质结构（NL）

作为一个例子，我们可以将中等复杂度的强化学习任务与[预测蛋白质结构](https://blog.exxactcorp.com/deepminds-protein-folding-upset/)的挑战进行比较。考虑一个由 100 个氨基酸链连接在一起的小型蛋白质，就像一个有 100 个链环的链条。每个链环是 21 种独特链环变体中的一种，结构将根据每个链环之间的角度形成。假设简化为 9 种可能的配置用于每个氨基酸/链环之间的键角，那么需要

> *999 = 29,512,665,430,652,752,148,753,480,226,197,736,314,359,272,517,043,832,886,063,884,637,676,943,433,478,020,332,709,411,004,889
> 
> （这大约是一个 3 后面跟着 94 个零）*

迭代以查看每个可能的结构。那个有 100 个链接的蛋白质大致相当于一个具有 100 个时间步和每个步骤 9 个可能动作的强化学习环境。*谈到组合爆炸。*

当然，强化学习代理并不是随机采样所有可能的游戏状态。相反，代理将通过某种组合的最佳猜测、定向探索和随机搜索来生成学习轨迹。但这种生成经验的方法，典型的“策略”学习算法，可能会有两面性。一个找到局部最优解的代理可能会一直停留在那儿，重复同样的错误，永远无法解决整体问题。

从历史上看，强化学习一直受益于尽可能将问题形式化为类似于监督学习的方式，例如让学生背上三台相机去远足。左侧相机生成一个标记为“向右转”的图像，而右侧相机标记为“向左走”，中间相机则标记为“直行”。

最终，你将拥有一个适合训练[四旋翼无人机导航森林小径](https://www.youtube.com/watch?v=umRdt3zGgpU)的标记数据集。在下面的章节中，我们将讨论类似的概念（例如模仿学习）和基于模型的强化学习如何大幅减少学习任务所需的训练样本数量，以及为什么这并不总是好事。

### 少量更多：强化学习代理从示例中学习最佳

深度强化学习是机器学习的一个分支，灵感来源于动物和人类认知、最优控制和深度学习等多种领域。显然，它与动物行为实验有类似之处，其中一个动物（代理）被置于一个必须学习解决问题以获得食物奖励的情境中。动物只需几个例子便开始将一系列动作与奖励关联起来，但深度强化学习算法可能需要考虑[每个周期 10 到 100 千个时间步](https://youtu.be/8EcdaCk9KaQ?t=736)以便对代理的参数进行稳定更新。

大多数强化学习环境是以步骤形式构建的。环境生成一个观察值，基于此，代理决定一个对环境施加的动作。环境根据其当前状态和代理选择的动作进行更新，在本文中我们称之为一个时间步。学习所需的时间步或“样本”越少，算法就越高效。

大多数现代强化学习算法的一个重要方面是，它们本质上是一种试错法。为了让代理在没有采用显式探索技术的情况下进行学习，[随机活动偶尔应该做对一些事情](https://youtu.be/8EcdaCk9KaQ?t=417)，否则代理可能会与环境互动（基本上）永远而得不到正向奖励。事实上，近端策略优化算法，在多个基准中表现优异，[在 OpenAI 的程序生成环境套件的困难探索模式中完全失败](https://arxiv.org/abs/1912.01588)。我们可以想象这对广泛相关任务是一个问题。

一个学习玩视频游戏的代理可能会通过随机动作（即按键乱按）获得正向奖励，这也是为什么 Atari 套件是流行的强化学习基准之一。对于像解决复杂打结问题这样的复杂任务，机器人偶然遇到解决方案的可能性极低。无论允许多少随机互动，实现期望结果都太不可能了。

正如我们在讨论[学习魔方操作](https://blog.exxactcorp.com/timeline-review-of-openais-robotic-hand-project/)时所见，已经有[定制的打结机器人](https://www.youtube.com/watch?v=erNi07dH5pw)被手动构建和编程以创建结，但从零开始学习打结的强化学习代理仍然无法实现。不言而喻，人类也不会从零开始学习打结。一个被独自留在充满未系鞋带的房间里的小孩，可能会很难自己发现标准的[“兔子结”](https://www.youtube.com/watch?v=erNi07dH5pw)。就像思维实验中的小孩一样，强化学习代理可以通过示例学得更好，即使是像打结这样的复杂任务。

### 使用强化学习及其他方法教机器打结的示例

通过将绳索物理的自主学习与人工示范相结合，伯克利的研究人员成功解决了[教机器打结](https://arxiv.org/pdf/1703.02018.pdf)的问题。首先，机器人在桌面上随机地与绳子互动，目的是学习一个合理的模型，了解它们对世界的有限视角是如何运作的。经过自主学习阶段后，机器人会被展示打简单结的期望动作。当一切顺利时，机器人能够模仿期望的动作。将自主学习的世界动态模型（如上例中的桌面和绳子）与人类示例的期望行为结合是一种强大的组合。模仿学习及相关的逆强化学习代表了目前一些最有效的强化学习方法。

绑结是一项略显冷门的任务（显然超出了许多学习算法的能力），但我们可以比较应用于更标准任务的不同学习算法的样本效率。来自 Sergey Levine 在伯克利的深度 RL 课程的讲座 21，第 39 张幻灯片 ([pdf](http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-21.pdf))，通过比较 HalfCheetah 任务的已发表结果，给我们提供了各种 RL 算法的相对样本效率的一个概念。HalfCheetah 是一个 2D 运动任务，模拟了一个大致类似于一半快的猫的机器人。HalfCheetah 的实现可以在 Mujoco（需要付费许可）和 [Bullet](https://github.com/bulletphysics/bullet3/releases)（开源，免费）物理模拟器中找到。

根据幻灯片，我们预计进化策略在样本效率方面最为低效，而基于模型/逆强化学习在总时间步数方面最为高效。下面重新列出了幻灯片中的估计值，并附上了文献中的具体示例。

![](img/e8e671c1f2d44a829211ecaf0c78129a.png)

*HalfCheetah 在 PyBullet 中模拟的机器人，其目标是尽可能快地向前移动。尽管以 3D 渲染，但运动受到限制在 2D 平面上。它像一只猎豹，但只有一半。*

| **方法** | *来自 [CS 285 lec 21](http://rail.eecs.berkeley.edu/deeprlcourse/)* 的*估计步骤* | *解决 HalfCheetah 的示例时间步数* |
| --- | --- | --- |
| **进化策略** | ~1,000,000,000 | ~3,000,000 [(ES, Salimans 等, 2016)](https://arxiv.org/abs/1703.03864) |
| **全在线演员-评论家方法** | ~100,000,000 | ~100,000,000[ (Trust-A3C, Wang 等, 2016)](https://arxiv.org/abs/1611.01224) |
| **策略梯度** | ~10,000,000 | ~1,000,000 [(PPO, Schulman 等, 2017)](https://arxiv.org/abs/1707.06347) |
| **值函数（离线）方法** | ~1,000,000 | ~1,500,000 [(Gu 等, 2018)](https://arxiv.org/abs/1603.00748) |
| (**基于模型**) | ~30,000 | ~400,000 [(PE-TS, Kurtland Chua 等, 2018)](https://arxiv.org/abs/1805.12114) |

*表 1: 各种 RL 算法家族的相对样本效率。*

样本效率在同一类算法的实现之间差异很大，我发现幻灯片中的估计值在文献中的具体示例中可能有些夸张。特别是，OpenAI 的 [进化策略](https://arxiv.org/abs/1703.03864) 论文实际上报告了比 TRPO 更好的样本效率，TRPO 是他们用作比较的策略梯度方法。他们报告的 HalfCheetah 解决方案需要 300 万时间步数，这与 Levine 估计的 10 亿相去甚远。

另一方面，由 Uber AI 实验室发布的[相关论文](https://arxiv.org/abs/1712.06567)应用遗传算法于 Atari 游戏中发现样本效率大约在 10 亿时间步数左右，约为他们比较的值函数算法的 5 倍，与 Levine 的课程估计一致。

### 在某些情况下，多即是少——从进化中学习

在上述讨论的例子中，进化策略是最样本效率低下的方法之一，通常需要比其他方法多出至少 10 倍的步骤来学习一个给定任务。在另一端，基于模型和模仿的方法需要最少的时间步数来学习相同的任务。

初看起来，这似乎是一个对进化方法实用性的明确否定，但当你优化计算而不是样本效率时，会发生有趣的事情。由于运行进化策略的开销较小，这些策略甚至不需要反向传播，它们实际上可能需要[更少的计算](https://youtu.be/tzieElmtAjs?list=PLkFD6_40KJIwhWJpGazJ9VSj9CFMkb79A&t=3949)。它们也天生具有并行性。因为一个种群中的每个回合或个体都不依赖于其他代理/回合的结果，所以学习算法变得[显然可以并行](https://en.wikipedia.org/wiki/Embarrassingly_parallel)。有了一个快速的模拟器，解决一个给定问题可能需要的时间（按墙上的时钟计算）会大大减少。

![](img/83f38ef1d2b6751c657e82a87a10375a.png)

*InvertedPendulumSwingup* 任务的起始和目标状态。绿色胶囊沿黄色杆滑动，目标是将未驱动的红色杆保持直立。

为了粗略比较计算效率，我在[PyBullet](https://pybullet.org/wordpress/)的一个倒立摆摆动任务 InvertedPendulumSwingupBulletEnv-v0 上运行了几种开源实现的各种学习算法。代表性的进化算法协方差矩阵自适应的代码[可以在 Github 上找到](http://bit.ly/exxact_cma)，而其他算法都属于 OpenAI 的[Spinning Up in Deep RL](https://spinningup.openai.com/)资源。

政策架构保持不变：一个前馈密集型神经网络，具有两个每层 16 个神经元的隐藏层。我在一台中等配置的 Intel i5 2.4 GHz CPU 的单核心上运行了所有算法，因此结果只能互相比较，训练时间的差异没有任何并行加速。值得注意的是，OpenAI 使用进化策略在[10 分钟内用 1440 个工人](https://openai.com/blog/evolution-strategies/)训练了 MujoCo 人形机器人，因此如果你有足够的 CPU 核心，平行加速是真实且有利可图的。

| *方法家族* | *算法* | *解决所需时间步数* | *解决时间（秒）* |
| --- | --- | --- | --- |
| **进化策略** | [协方差矩阵自适应](https://en.wikipedia.org/wiki/CMA-ES)[[代码](http://bit.ly/exxact_cma)] | ~4,100,000 | ~500 s |
| **策略梯度** | [近端策略优化](https://spinningup.openai.com/en/latest/algorithms/ppo.html)[[代码](https://github.com/openai/spinningup)] | ~1,800,000 | ~2000 s |
| **完全在线演员-评论家方法** | [软演员-评论家](https://spinningup.openai.com/en/latest/algorithms/sac.html)[[代码](https://github.com/openai/spinningup)] | ~1,500,000 | ~17,000 s |
| **值函数（离策略）方法** | [双延迟 DDPG](https://spinningup.openai.com/en/latest/algorithms/td3.html)[[代码](https://github.com/openai/spinningup)] | ~2,000,000 | ~17,500 s |

*表 2：比较不同学习方法的实际时间和样本效率。*

上表很好地展示了不同方法所需资源的相对关系。样本效率最高的算法（软演员评论家）所需步骤约为其他方法的一半，但消耗的 CPU 时间是其 34 倍。双延迟 DDPG（也称为 TD3）和软演员评论家比其他方法稳定性差。

由于缺乏开源工具，我们在运行表 2 实验时没有考虑基于模型的 RL 算法。基于模型的 RL 的研究进展受限于闭源开发和缺乏标准环境，但[Wang 等](https://arxiv.org/abs/1907.02057)做出了英雄般的努力，为许多基于模型的算法提供了基准。他们发现，大多数基于模型的方法能够在大约 25,000 步内解决一个他们称之为 Pendulum 的任务，该任务与上表中报告的任务非常相似。与策略梯度方法相比，他们调查的多数基于模型的方法的训练（实际）时间大约长 100 到 200 倍。

尽管深度学习仍然偏爱“华丽”的强化学习方法，但基于进化的方法正在复兴。OpenAI 的“[进化策略作为一种可扩展的强化学习替代方案](https://openai.com/blog/evolution-strategies/)”于 2016 年发布，2017 年则有大规模的遗传算法[学习玩 Atari 游戏](https://eng.uber.com/deep-neuroevolution)的展示。与所有复杂的数学装饰不同，进化算法在概念上很简单：生成一个种群并设置个体任务，保留表现最好的个体用于种群繁殖，并重复直到达到性能阈值。由于其相对简单的概念和固有的并行化潜力，你可能会发现进化算法能在你的最稀缺资源——开发者时间上带来显著节省。

### 有一些需要了解的计算和样本效率的警示

![](img/0247a876640cfeb39f5186b8fbf18dde.png)

在单一任务上直接比较不同 RL 算法的计算和样本效率并不完全公平，因为不同实现之间存在许多变量。一个算法可能更早收敛，但从未达到与其他较慢算法相同的分数，通常，你可以打赌，在任何发布的 RL 工作中，作者花费了比任何比较算法更多的精力来优化和实验他们的新方法。

然而，我们上面讨论的例子给了我们对强化学习代理的预期一个很好的概念。虽然基于模型的方法有前景，但与其他选项相比尚不成熟，对于大多数任务可能不值得额外的努力。也许令人惊讶的是，进化算法在各种任务上表现良好，不应因为被认为是“过时”或“过于简单”而被忽视。在考虑所有可用资源的价值及其被现有方法消耗的情况下，进化算法确实可能提供最佳回报。

[原文](https://blog.exxactcorp.com/2-things-you-need-to-know-about-reinforcement-learning/)。经许可转载。

**相关：**

+   [我们创建了一个懒惰的 AI](https://www.kdnuggets.com/2020/01/created-lazy-ai.html)

+   [使 AlphaStar 在《星际争霸 II》中超越几乎所有人类玩家的强化学习方法](https://www.kdnuggets.com/2019/11/reinforcement-learning-methods-alphastar-outcompete-human-players-starcraft.html)

+   [关于强化学习的三件事](https://www.kdnuggets.com/2019/10/mathworks-reinforcement-learning.html)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

### 更多相关内容

+   [计算深度学习模型的计算效率…](https://www.kdnuggets.com/2023/06/calculate-computational-efficiency-deep-learning-models-flops-macs.html)

+   [关于数据管理及其重要性的 6 件事](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [构建 LLM 应用程序时需要知道的 5 件事](https://www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html)

+   [你不知道的低代码工具的 7 个功能](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [KDnuggets 新闻，4 月 13 日：数据科学家应该了解的 Python 库](https://www.kdnuggets.com/2022/n15.html)

+   [扩展你的网络数据驱动产品时应该了解的事项](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)
