# 实践强化学习课程第3部分：SARSA

> 原文：[https://www.kdnuggets.com/2022/01/handson-reinforcement-learning-course-part-3-sarsa.html](https://www.kdnuggets.com/2022/01/handson-reinforcement-learning-course-part-3-sarsa.html)

![实践强化学习课程第3部分：SARSA](../Images/30748c058f5a536beb5706b08cffa316.png)

**欢迎来到我的强化学习课程❤️**

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

这是我实践强化学习课程的第3部分，该课程将你从零带到HERO ????‍♂️。今天我们将学习SARSA，这是一种强大的RL算法。

我们仍然处于旅程的起点，解决相对简单的问题。

在 [**第2部分**](http://datamachines.xyz/2021/12/06/hands-on-reinforcement-learning-course-part-2/) 中，我们实现了离散Q学习来训练`Taxi-v3`环境中的一个代理。

今天，我们将更进一步，使用SARSA算法解决`MountainCar`环境????。

让我们帮助这辆可怜的车赢得与重力的战斗！

本课的所有代码都在 [**这个Github仓库**](https://github.com/Paulescu/hands-on-rl)**。** 克隆它以跟随今天的问题。

[![实践强化学习课程第3部分：SARSA](../Images/5a7273317f4e2bc8225fd4e3f167146b.png)](https://github.com/Paulescu/hands-on-rl)

### 1\. Mountain Car问题????

Mountain Car问题是一个存在重力的环境（多么惊人），目标是帮助一辆可怜的车赢得这场与重力的战斗。

这辆车需要逃离被困的山谷。车的引擎没有足够的动力一次性爬上山，因此唯一的方法就是来回驾驶，积累足够的动量。

让我们看看实际效果：

Sarsa Agent 实际效果！

你刚才看到的视频对应于我们今天将要构建的`SarsaAgent`。

有趣，不是吗？

你可能在想。

*这看起来很酷，但你为什么最初选择这个问题呢？*

**为什么是这个问题？**

这个课程的哲学是逐步增加复杂性。一步一步来。

今天的环境相比第2部分的`Taxi-v3`环境代表了一个小但相关的复杂性增加。

但是，*这里到底难在哪里呢？*

正如我们在**[第2部分](http://datamachines.xyz/2021/12/06/hands-on-reinforcement-learning-course-part-2/)**中看到的，强化学习问题的难度与

+   动作空间：*智能体在每一步可以选择多少种动作？*

+   状态空间：*智能体可以在多少种不同的环境配置中找到自己？*

对于*小环境*，其动作和状态数量有限且较少，我们有很强的保证，像 Q-learning 这样的算法会表现良好。这些环境被称为**表格或离散环境**。

Q 函数本质上是一个矩阵，行数等于状态数，列数等于动作数。在这些*小*世界中，我们的智能体可以轻松探索状态并构建有效的策略。随着状态空间和（特别是）动作空间的增大，RL 问题变得更难解决。

今天的环境**不是**表格型的。然而，我们将使用离散化“技巧”将其转换为表格型环境，然后解决它。

让我们首先熟悉环境！

### 2. 环境、动作、状态、奖励

**[???????? notebooks/00_environment.ipynb](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/notebooks/00_environment.ipynb)**

让我们加载环境：

![动手强化学习课程 第3部分：SARSA](../Images/b83d04fba09a39066cf90d2b622b248b.png)

并绘制一个帧：

![动手强化学习课程 第3部分：SARSA](../Images/e13755e2f902e1032dea4b03b8415391.png)![动手强化学习课程 第3部分：SARSA](../Images/07c85dba5cf0d6bd13e8798117845c00.png)

两个数字决定了汽车的**状态**：

+   它的位置范围是**-1.2** 到 **0.6**

+   它的速度范围是**-0.07** 到 **0.07**。

![动手强化学习课程 第3部分：SARSA](../Images/22fe4c7604415a9d20c8db6f4196a93a.png)

状态由 2 个连续数字给出。这与 [第2部分](https://towardsdatascience.com/hands-on-reinforcement-learning-course-part-2-1b0828a1046b) 的 `Taxi-v3` 环境有显著不同。我们稍后将看到如何处理这个问题。

什么是**动作**？

有 3 种可能的动作：

+   `0` 向左加速

+   `1` 什么都不做

+   `2` 向右加速

![动手强化学习课程 第3部分：SARSA](../Images/c2f912ce54c18b93315418e9a0f8594b.png)

那**奖励**呢？

+   如果汽车的位置小于 0.5，则奖励为 -1。

+   一旦汽车的位置高于 0.5 或达到最大步骤数时，剧集结束：`n_steps >= env._max_episode_steps`

默认负奖励 -1 鼓励汽车尽快脱离山谷。

一般来说，我建议你直接在 [Open AI Gym environments’](https://github.com/openai/gym/tree/master/gym/envs) GitHub 上查看实现，以了解状态、动作和奖励。

代码文档齐全，可以帮助你快速理解开始使用 RL 代理所需的一切。例如，`MountainCar` 的实现可以在 [这里](https://github.com/openai/gym/blob/master/gym/envs/classic_control/mountain_car.py) 找到。

好的，我们对环境有了了解。

让我们为这个问题构建一个基线代理！

### 3. 随机代理基线 ????????

**[???????? notebooks/01_random_agent_baseline.ipynb](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/notebooks/01_random_agent_baseline.ipynb)**

强化学习问题可能很容易变得复杂。结构良好的代码是你保持复杂性受控的最佳盟友。

今天我们将提升我们的 Python 技能，并为所有代理使用 `BaseAgent` 类。从这个 `BaseAgent` 类，我们将派生出 `RandomAgent` 和 `SarsaAgent` 类。

![实践强化学习课程 第3部分: SARSA](../Images/97f7073609a002ece390eef9d20f5aff.png)

`BaseAgent` 是我们在 `[src/base_agent.py](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/src/base_agent.py)` 中定义的**抽象类**。

它有 4 个方法。

它的两个方法是抽象的，这意味着我们在从 `BaseAgent` 派生 `RandomAgent` 和 `SarsaAgent` 时被迫实现它们：

+   `get_action(self, state)` → 根据状态返回要执行的动作。

+   `update_parameters(self, state, action, reward, next_state)` → 使用经验来调整代理参数。在这里，我们将实现 SARSA 公式。

另外两个方法让我们能够将训练好的代理保存到磁盘或从磁盘加载。

+   `save_to_disk(self, path)`

+   `load_from_disk(cls, path)`

随着我们开始实现更复杂的模型和训练时间的增加，在训练过程中保存检查点将是一个很好的主意。

这是我们 `BaseAgent` 类的完整代码：

![实践强化学习课程 第3部分: SARSA](../Images/121260f614782f3b0541ffec3920a03b.png)

从这个 `BaseAgent` 类，我们可以定义 `RandomAgent` 如下：

![实践强化学习课程 第3部分: SARSA](../Images/724e1d2baf07028b161c42a4c63ce38e.png)

让我们对这个 `RandomAgent` 进行 `n_episodes = 100` 次评估，看看它的表现如何：

![实践强化学习课程 第3部分: SARSA](../Images/46caf646c57a884bd3d0aa4a8c3a3939.png)![实践强化学习课程 第3部分: SARSA](../Images/f5e247010acc481fa782c1b72fdc6bc5.png)

而我们 `RandomAgent` 的成功率是…

![实践强化学习课程 第3部分: SARSA](../Images/c5c2451995329a41fb26bb883b16244d.png)

0% ????…

我们可以通过以下直方图查看代理在每个回合中的表现：

![实践强化学习课程 第3部分: SARSA](../Images/ff47391459ccea26e6f14f03e335fb59.png)![实践强化学习课程 第3部分: SARSA](../Images/35f6ba7e5565b8feaf6567872decc31b.png)

在这 `100` 次运行中，我们的 `RandomAgent` 没有突破**0.5**的标记。一时间都没有。

*当你在本地机器上运行这段代码时，你会得到略微不同的结果，但完成率超过 0.5 的百分比在任何情况下都远未达到 100%。*

你可以使用漂亮的 `show_video` 函数观看我们痛苦的 `RandomAgent` 的操作，函数位于 `[**src/viz.py**](https://github.com/Paulescu/hands-on-rl/blob/37fbac23d580a44d46d4187525191b324afa5722/02_mountain_car/src/viz.py#L52-L61)`

一个随机代理不足以解决这个环境。

让我们尝试一些更聪明的东西 ????…

### 4\. SARSA 代理 ????????

**[???????? notebooks/02_sarsa_agent.ipynb](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/notebooks/02_sarsa_agent.ipynb)**

[**SARSA**](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.17.2539&rep=rep1&type=pdf)（由 Rummery 和 Niranjan 提出）是一种通过学习最优 q 值函数来训练强化学习代理的算法。

它在 1994 年发布，比 [**Q-learning**](https://www.gatsby.ucl.ac.uk/~dayan/papers/cjch.pdf)（由 Chris Watkins 和 Peter Dayan 提出）晚两年。

SARSA 代表 **S**tate **A**ction **R**eward **S**tate **A**ction。

SARSA 和 Q-learning 都利用贝尔曼方程来迭代地寻找更好的最优 q 值函数 **Q*(s, a)**

![动手强化学习课程第3部分：SARSA](../Images/2eab3dbcb1f4e5f188efee35f0a488a2.png)

如果你记得第2部分，Q-learning 的更新公式是

![动手强化学习课程第3部分：SARSA](../Images/8ea0dd53fcb0b32c31128e0c1319bbf8.png)

这个公式是一种计算新的 q 值估计的方法，它更接近于

![动手强化学习课程第3部分：SARSA](../Images/10c9f1d1a6ca78e20fc96351401cf53c.png)

这个量是一个 *目标* ???? 我们想要将旧估计值校正到这个目标。这是我们应该瞄准的最优 q 值的 *估计*，它会随着我们训练代理和 q 值矩阵的更新而变化。

> 强化学习问题通常看起来像带有 ***移动目标*** 的监督学习问题 ???? ????

SARSA 有一个类似的更新公式，但目标不同

![动手强化学习课程第3部分：SARSA](../Images/4b749da798bc061e5706df3750558c37.png)

SARSA 的目标

![动手强化学习课程第3部分：SARSA](../Images/3610db26defe49e792b600720dcf46de.png)

还取决于代理在下一个状态 **s’** 中将采取的动作 **a’**。这就是 SARS**A’** 名称中的最终 **A**。

如果你足够探索状态空间并使用 SARSA 更新你的 q 矩阵，你将得到一个最优策略。太棒了！

你可能在想……

***Q-learning 和 SARSA 看起来几乎一样。有什么不同？* ????**

在线策略与离线策略算法

SARSA 和 Q-learning 之间有一个关键的区别：

???? SARSA 的更新依赖于下一个动作 **a’**，因此依赖于当前策略。随着训练的进行，q 值（及相关策略）得到更新，新策略可能会对同一状态 **s’** 产生不同的下一个动作 **a’’**。

你不能使用过去的经验 **(s, a, r, s’, a’)** 来改进你的估计。相反，你需要利用每个经验来更新 q 值，然后将其丢弃。

正因为如此，SARSA 被称为**在线策略**方法。

???? 在 Q 学习中，更新公式不依赖于下一个动作**a’**，而仅依赖于**(s, a, r, s’)**。你可以重用以前的经验**(s, a, r, s’)**，这些经验是用旧版本策略收集的，以改进当前策略的 q 值。Q 学习是一种**离策略**方法。

离策略方法比在线策略方法需要更少的经验进行学习，因为你可以多次重用过去的经验来改进你的估计。它们更**样本高效**。

然而，离策略方法在状态和动作空间扩展时，收敛到最优 q 值函数 Q*(s, a) 可能会出现问题。它们可能很棘手且**不稳定**。

我们将在课程后面遇到这些权衡问题，当我们进入深度强化学习领域 ???? 时。

回到我们的问题……

在 `MountainCar` 环境中，状态不是离散的，而是一对连续值（位置 `s1`，速度 `s2`）。

*连续*在这个背景下基本上意味着*无限可能的值*。如果有无限可能的状态，就不可能访问所有状态来保证 SARSA 会收敛。

为了解决这个问题，我们可以使用一个技巧。

让我们将状态向量离散化为有限的值集合。本质上，我们没有改变环境，而是改变了代理用来选择动作的状态表示。

我们的 `SarsaAgent` 将状态 `(s1, s2)` 从连续转换为离散，通过将位置 `[-1.2 … 0.6]` 四舍五入到最接近的 `0.1` 标记，将速度 `[-0.07 … 0.07]` 四舍五入到最接近的 `0.01` 标记。

这个函数正是这样做的，将连续状态转换为离散状态：

![动手强化学习课程第3部分：SARSA](../Images/0fda14d9637f91b534e2d1ad2493d6b6.png)

一旦代理使用离散化的状态，我们可以使用上述的 SARSA 更新公式，随着迭代的进行，我们会越来越接近最优 q 值。

这是 `SarsaAgent` 的完整实现。

![动手强化学习课程第3部分：SARSA](../Images/5115d60823b807644f2be72a5a485eaf.png)

注意 ???? q 值函数是一个三维矩阵：2 个用于状态（位置、速度），1 个用于动作。

让我们选择合理的超参数，并将这个 `SarsaAgent` 训练 `n_episodes = 10,000` 次。

![动手强化学习课程第3部分：SARSA](../Images/30d4886cfc816b5449c7ce86ce34f678.png)![动手强化学习课程第3部分：SARSA](../Images/1be80c6b85e074e60a1f949506d3ada1.png)

让我们绘制 `rewards` 和 `max_positions`（蓝色线条）以及它们的 50 次迭代移动平均线（橙色线条）。

![动手强化学习课程第3部分：SARSA](../Images/d2701bf7bf100ca95a5276ef8d013642.png)![动手强化学习课程第3部分：SARSA](../Images/05874c4c72e14310237e160a9eceb2a9.png)

太棒了！看起来我们的 `SarsaAgent` 正在学习。

你可以看到它的实际应用：

如果你观察上面的`max_position`图表，你会发现汽车偶尔会失败爬山。

这发生的频率是多少？让我们在`1,000`个随机回合中评估一下智能体：

![实践强化学习课程 第三部分：SARSA](../Images/5c690f4e9a6f9e4d1f5d7287c1313493.png)

并计算成功率：

![实践强化学习课程 第三部分：SARSA](../Images/51dbbd94b417b19993f904d9a00795c6.png)

**95.2%**的表现相当不错，但仍然不完美。记住这一点，我们将在课程后面再回来讨论。

**注意：** 当你在本地运行此代码时，你将得到略微不同的结果，但我敢打赌你不会得到100%的表现。

做得好！我们实现了一个`SarsaAgent`，它能够学习????

现在是一个很好的时机来暂停一下……

### 5\. 暂停一下，深呼吸 ⏸????

**[???????? notebooks/03_momentum_agent_baseline.ipynb](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/notebooks/03_momentum_agent_baseline.ipynb)**

如果我告诉你`MountainCar`环境有一个更简单的解决方案呢……

这辆车100%的时间都能正常工作？ ????

最好的策略是简单的。

*只需跟随动量*：

+   当汽车向右移动时，`velocity > 0`加速向右

+   当汽车向左移动时，`velocity <= 0`加速向左

从视觉上看，这种策略如下：

![实践强化学习课程 第三部分：SARSA](../Images/26889061f1c4db0ed1802e70bcfd8797.png)

这是如何在Python中编写`MomentumAgent`：

![实践强化学习课程 第三部分：SARSA](../Images/593d3214eeec9f93aa99bb76f46d4c7f.png)

你可以再次检查它是否完成了每一集。100%的成功率。

![实践强化学习课程 第三部分：SARSA](../Images/f31f46328e106eceb1c181f8e963f3a0.png)![实践强化学习课程 第三部分：SARSA](../Images/6ecc1050677bbc69346f4dd8e3b48152.png)

如果你绘制训练后的`SarsaAgent`的策略，你会看到如下图：

![实践强化学习课程 第三部分：SARSA](../Images/f99251db39d0582f83b48560483d751f.png)

这与完美的`MomentumAgent`策略有50%的重叠

![实践强化学习课程 第三部分：SARSA](../Images/673958d367ba2427a0d8c059a3acf000.png)

这意味着我们的`SarsaAgent`的正确率*仅为*50%。

这很有趣……

**为什么`SarsaAgent`错误这么频繁但仍能取得良好表现？**

这是因为`MountainCar`仍然是一个较小的环境，因此在50%的时间里做出错误决定并不那么关键。对于更大的问题，频繁的错误不足以建立智能体。

> *你会买一辆95%时间正确的自动驾驶汽车吗？ ????*

另外，你还记得我们用来应用SARSA的*离散化技巧*吗？那是一个对我们帮助很大的技巧，但也给我们的解决方案带来了误差/偏差。

**为什么我们不提高状态和速度的离散化分辨率，以获得更好的解决方案？**

做到这一点的问题在于状态数量的指数增长，也称为**维度诅咒**。随着每个状态组件分辨率的提高，总状态数量呈指数增长。状态空间增长速度太快，SARSA 智能体无法在合理时间内收敛到最优策略。

**好的，但是否有其他强化学习算法可以完美解决这个问题？**

是的，有的。我们将在接下来的讲座中介绍这些。在强化学习算法方面，没有一种适合所有问题的通用方案，因此你需要尝试几种算法来找到最适合你问题的方案。

在 `MountainCar` 环境中，完美的策略看起来非常简单，我们可以尝试直接学习它，而无需计算复杂的 q 值矩阵。**策略优化** 方法可能会效果最佳。

但我们今天不会做这个。如果你想用强化学习完美解决这个环境，请跟随课程学习。

享受你今天所取得的成就。

![动手强化学习课程第 3 部分：SARSA](../Images/210f53a985969df08d72258294b59fb3.png)谁在玩得开心？

### 6\. 回顾 ✨

哇！我们今天覆盖了很多内容。

这些是 5 个要点：

+   SARSA 是一种你可以在表格环境中使用的策略算法。

+   小型连续环境可以视为表格形式，通过离散化状态，然后使用表格 SARSA 或表格 Q 学习解决。

+   较大的环境由于维度诅咒无法被离散化和解决。

+   对于比 `MountainCar` 更复杂的环境，我们将需要更先进的强化学习解决方案。

+   **有时候强化学习不是最好的解决方案**。当你尝试解决你关心的问题时，请记住这一点。不要过于依赖你的工具（在这种情况下是强化学习），而是专注于找到一个好的解决方案。不要因为树木而看不到森林 ????????????。

### 7\. 作业 ????

[**???????? notebooks/04_homework.ipynb**](https://github.com/Paulescu/hands-on-rl/blob/main/02_mountain_car/notebooks/04_homework.ipynb)

这是我希望你做的：

1.  [**Git 克隆**](https://github.com/Paulescu/hands-on-rl) 该库到本地计算机。

1.  [**设置**](https://github.com/Paulescu/hands-on-rl/tree/main/02_mountain_car#quick-setup) 本课程的环境 `02_mountain_car`

1.  打开 `[02_mountain_car/notebooks/04_homework.ipynb](http://02_mountain_car/notebooks/04_homework.ipynb)` 并尝试完成两个挑战。

在第一个挑战中，我要求你调整 SARSA 超参数 `alpha`（学习率）和 `gamma`（折扣因子）以加快训练速度。你可以从 [part 2](https://towardsdatascience.com/hands-on-reinforcement-learning-course-part-2-1b0828a1046b) 中获得灵感。

在第二个挑战中，尝试提高离散化的分辨率，并使用表格 SARSA 学习 q 值函数。就像我们今天做的那样。

如果你构建了一个达到 99% 性能的智能体，请告诉我。

### 8\. 下一步是什么？ ❤️

在下一课中，我们将进入一个强化学习与监督机器学习交汇的领域 ????。

这将会非常酷，我保证。

在此之前，

享受在这个神奇的地球上的一天 ????

爱 ❤️

继续学习 ????

如果你喜欢这个课程，请与朋友和同事分享。

你可以通过`plabartabajo@gmail.com`联系我。我很乐意与您交流。

很快见！

**个人简介：[Pau Labarta Bajo](https://www.linkedin.com/in/pau-labarta-bajo-4432074b/)** ([**@paulabartabajo_**](https://twitter.com/paulabartabajo_)) 是一名数学家和AI/ML自由职业者及演讲者，拥有超过10年的经验，处理各种问题的数字和模型，包括金融交易、移动游戏、在线购物和医疗保健。

[原文](http://datamachines.xyz/2021/12/17/hands-on-reinforcement-learning-course-part-3-sarsa/)。经授权转载。

### 更多相关话题

+   [动手强化学习课程，第1部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-course-part-1.html)

+   [动手强化学习课程，第2部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-part-2.html)

+   [HuggingFace推出了免费的深度强化学习课程](https://www.kdnuggets.com/2022/05/huggingface-launched-free-deep-reinforcement-learning-course.html)

+   [动手学习监督学习：线性回归](https://www.kdnuggets.com/handson-with-supervised-learning-linear-regression)

+   [动手学习无监督学习：K均值聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [使用大型语言模型的生成性AI：动手培训](https://www.kdnuggets.com/2023/07/generative-ai-large-language-models-handson-training.html)
