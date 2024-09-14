# 深度学习研究综述：强化学习

> 原文：[https://www.kdnuggets.com/2016/11/deep-learning-research-review-reinforcement-learning.html/2](https://www.kdnuggets.com/2016/11/deep-learning-research-review-reinforcement-learning.html/2)

### [DQN 强化学习](http://www.nature.com/nature/journal/v518/n7540/pdf/nature14236.pdf)** （使用 Atari 游戏的 RL）**

![](../Images/43d0762f632865f822943babc68ce0c8.png)

**介绍**

这篇论文由 Google Deepmind 于 2015 年 2 月发表，并登上了世界著名科学周刊《自然》的封面。这是将深度神经网络与强化学习结合起来的首次成功尝试之一（[这篇](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf) 是 Deepmind 的原始论文）。论文表明，他们的系统能够在 49 款游戏中以与专业游戏测试员相当的水平玩 Atari 游戏。让我们深入了解他们是如何做到的。

**方法**

好了，记住我们在帖子开头的介绍教程中停留在哪里了吗？我们刚刚描述了优化我们的动作价值函数 Q 的主要目标。Deepmind 的团队通过一个深度 Q 网络，或称 DQN，来处理这个任务。这个网络能够根据来自游戏屏幕的像素和当前分数的输入，制定出优化 Q 的成功策略。

**网络架构**

让我们更仔细地看看这个 DQN 的输入是什么。考虑 Breakout 游戏，并取当前游戏中的 4 帧最近的图像。每一帧最初是一个 210 x 160 x 3 的图像（因为宽度和高度分别为 210 和 160 像素，是彩色图像）。然后，进行一些预处理，将图像缩放到 84 x 84（了解具体过程并不极其重要，但请查看第 6 页了解详细信息）。所以，现在我们有了一个 84 x 84 x 4 的输入体积。这个体积将被输入到一个卷积神经网络中（[教程](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/)），经过一系列卷积和 ReLU 层处理。网络的输出是一个 18 维的向量，每个数字是用户可以采取的每个可能动作的 Q 值（向上移动、向下移动、向左移动等）。

![](../Images/2de0ce70346dc78245e2793eaebba9dd.png)

好的，让我们稍微退一步，弄清楚如何训练这个网络，使其能够预测准确的 Q 值。首先，让我们回顾一下我们要优化的内容。

![](../Images/60957a586ed7a59690253b4bf8219fe7.png)

这与我们之前看到的 Q 函数形式相同，只不过这个表示的是所有 Q 的最大值 Q*。让我们来研究一下如何获得这个 Q*。现在，记住我们只需要 Q* 的一个近似值，这就是我们的函数逼近器（我们的 Qhats）会派上用场的地方。请在我们稍作转变时保持这个想法。

为了找到最佳策略，我们想要将问题框定为某种监督学习问题，其中预测的Q函数与期望的Q函数进行比较，然后朝正确的方向调整。为了做到这一点，我们需要一组训练样本。在我们的案例中，我们将需要创建一组经验，这些经验存储了每个时间步的代理状态、动作、奖励和下一个状态。让我们更正式地说明一下。我们有一个**回放记忆**D，其中包含了(st, at, rt, st+1)的一组不同时间步数据。随着代理与环境的互动增多，这个数据集会逐渐建立。现在，我们将随机抽取这一数据的一个批次（比如64个时间步的数据），计算它们的损失函数，然后按照梯度来改进我们的Q函数近似。

![](../Images/42084c56cdcdd68cdcb38d031f39ca7f.png)

因此，正如你所见，损失函数旨在优化Q网络函数近似（Q(s,a,theta))与**Q学习目标**之间的均方误差（MSE）。让我快速解释一下这些。这个Q学习目标是奖励r加上你可以从某个动作a’中获得的最大Q值（在下一个时间步）。

一旦计算了损失函数，就会对theta值（或w向量）进行求导。然后更新这些值以最小化损失函数。

**结论**

这篇论文中我最喜欢的部分之一是它在游戏的某些时刻提供的值函数可视化。

![](../Images/db5d9b941dfe804bd1671e0a33aeec49.png)

正如你所记得的，值函数基本上是衡量“处于特定情况有多好”的指标。如果你查看#4，你可以看到，根据球的轨迹和砖块的位置，我们可以获得大量的分数，高值函数很好地代表了这一点。

所有49个Atari游戏使用了相同的网络架构、算法和超参数，这对强化学习方法的稳健性是一个令人印象深刻的见证。深度网络与传统强化学习策略（如Q学习）的结合，在为...

### [掌握AlphaGo与RL](http://www.nature.com/nature/journal/v529/n7587/pdf/nature16961.pdf)

![](../Images/6a21d464905371282791c48249f984ed.png)

**介绍**

**4-1**。这是 Deepmind 的强化学习代理在与世界顶级围棋选手李世石对局时的记录。万一你不知道的话，围棋是一种在棋盘上争夺领土的抽象策略游戏。由于游戏情境和移动方式的数量极为庞大，围棋被认为是世界上最难的 AI 游戏之一。论文开始时对围棋与象棋和跳棋等常见棋盘游戏进行了比较。虽然可以用变体的树搜索算法来攻击这些游戏，但围棋却完全不同，因为在一局游戏中大约有 250150 种不同的移动序列。显然需要强化学习，因此让我们看看 AlphaGo 是如何战胜困难的。

**方法**

AlphaGo 背后的基础是评估和选择的理念。在任何强化学习问题中（尤其是在棋盘游戏中），你需要一种评估环境或当前棋盘位置的方法。这将是我们的**价值网络**。然后，你需要一种通过**策略网络**选择行动的方法。我们确实对这两个术语——价值和策略——有一定的经验。

**网络架构**

让我们来看一下这两个网络的输入是什么。棋盘位置作为一个 19 x 19 的图像传入，并经过一系列卷积层，以构建当前状态的良好表示。首先，让我们看看我们的 SL（监督学习）策略网络。这个网络将图像作为输入，然后输出一个代理可以采取的所有合法动作的概率分布。这个网络在实际游戏之前已经在 3000 万个不同的围棋棋盘位置上进行了预训练。这些棋盘位置中的每一个都标记了在该情况下专家的最佳移动。团队还训练了一个较小但更快速的回合策略网络。

现在，卷积神经网络（CNN）在给定当前棋盘的表示时，能预测你应该采取的正确动作的能力是有限的。这时，强化学习发挥作用。我们将通过一个叫做**策略梯度**的过程来改进这个策略网络。还记得在上一篇论文中，我们希望优化我们的动作价值函数 Q 吗？现在，我们直接优化我们的策略（策略梯度的解释比较复杂，但 David Silver 在[第七讲](https://www.youtube.com/watch?v=KHZVXao4qXs)中讲得很好）。从高层次来看，策略通过模拟当前策略网络与网络的先前迭代之间的对局来改进。奖励信号为赢得比赛 +1，输掉比赛 -1，因此我们可以通过正常的梯度下降来改进网络。

![](../Images/a3679d9f59bc52dd92867ee3b9ed9802.png)

好的，现在我们有了一个相当不错的网络，它告诉我们最佳的行动步骤。下一步是拥有一个价值网络，预测在棋盘位置 S 上的游戏结果，其中两个玩家都使用策略 P。

![](../Images/70d0b6b99dd07a7490e63b634ae7bb58.png)

为了获得最佳的V*，我们将使用我们熟悉的带有权重W的函数逼近器。这些权重由价值网络训练，训练数据基于状态、结果对（类似于我们在上一篇论文中看到的情况）。

现在我们有了这两个主要网络，我们的最后一步是使用**蒙特卡洛树搜索**将一切整合起来。MCTS的基本思想是通过前瞻搜索选择最佳动作，其中树中的每条边存储一个动作值Q、一个访问计数和一个先验概率。根据这些信息，MCTS算法将从当前状态中选择最佳动作A。系统的这一部分略显传统AI而少于RL，因此如果你想了解更多细节，绝对可以查看论文，它会更好地总结这一部分。

**结论**

一台计算机系统刚刚击败了世界上最优秀的选手，赢得了最难的棋盘游戏之一。谁还需要结论呢？

[via GIPHY](http://giphy.com/gifs/obama-mic-drop-out-3o7qDSOvfaCO9b3MlO)

**特别感谢David Silver提供方程式和出色的强化学习讲座课程**

**简历：[Adit Deshpande](https://twitter.com/aditdeshpande3)** 目前是加州大学洛杉矶分校计算机科学专业的二年级本科生，同时辅修生物信息学。他热衷于将自己在机器学习和计算机视觉方面的知识应用到医疗领域，为医生和患者提供更好的解决方案。

[原文](https://adeshpande3.github.io/adeshpande3.github.io/Deep-Learning-Research-Review-Week-2-Reinforcement-Learning)。已获授权转载。

**相关：**

+   [深度学习研究综述：生成对抗网络](/2016/10/deep-learning-research-review-generative-adversarial-networks.html)

+   [神经网络中的深度学习：概述](/2016/04/deep-learning-neural-networks-overview.html)

+   [9篇关键深度学习论文，解释详解](/2016/09/9-key-deep-learning-papers-explained.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 相关主题

+   [HuggingFace推出了免费的深度强化学习课程](https://www.kdnuggets.com/2022/05/huggingface-launched-free-deep-reinforcement-learning-course.html)

+   [AI、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [动手强化学习课程第3部分：SARSA](https://www.kdnuggets.com/2022/01/handson-reinforcement-learning-course-part-3-sarsa.html)

+   [动手强化学习课程，第1部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-course-part-1.html)

+   [动手强化学习课程，第2部分](https://www.kdnuggets.com/2021/12/hands-on-reinforcement-learning-part-2.html)

+   [强化学习入门](https://www.kdnuggets.com/2022/05/reinforcement-learning-newbies.html)
