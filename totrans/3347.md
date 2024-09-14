# 6个需要密切关注的AI和机器学习领域

> 原文：[https://www.kdnuggets.com/2017/01/6-areas-ai-machine-learning.html](https://www.kdnuggets.com/2017/01/6-areas-ai-machine-learning.html)

[评论](#comments)

**由 [Nathan Benaich](http://www.nathanbenaich.com/) 提供，投资于未来技术。**

![](../Images/71ab4e2fdf178256a081d486d31fbd0c.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

提炼出一个普遍接受的人工智能（AI）定义已成为近期重新讨论的话题。有些人将AI重新品牌化为“认知计算”或“机器智能”，而另一些人错误地将AI与“机器学习”混为一谈。这部分是因为AI不是*一种*技术。事实上，它是一个广泛的领域，由*许多*学科组成，从机器人技术到机器学习。大多数人认为，AI的最终目标是构建能够执行任务和认知功能的机器，而这些任务和功能通常仅限于人类智能的范围。为了实现这一目标，机器必须能够自动学习这些能力，而不是将每项能力逐步编程。

在过去10年中，AI领域取得了令人惊叹的进展，从自动驾驶汽车到语音识别和合成。背景下，AI已成为越来越多公司和家庭讨论的话题，他们开始将AI视为不再是20年后的技术，而是正在影响他们生活的事物。的确，主流媒体几乎每天都报道AI，科技巨头们也一个个阐明了他们的重要长期AI战略。尽管一些投资者和现有者渴望了解如何在这个新世界中获取价值，大多数人仍在摸索这意味着什么。与此同时，政府正在处理自动化对社会的影响（参见奥巴马的 [告别演讲](https://www.nytimes.com/2017/01/12/upshot/in-obamas-farewell-a-warning-on-automations-perils.html?_r=0)）。

鉴于AI将影响整个经济体，参与这些对话的各方代表了意图、理解水平和构建或使用AI系统的经验程度的全面分布。因此，关于AI的讨论——包括从中得出的问题、结论和建议——必须以数据和现实为基础，而非推测。从已发布的研究结果或技术新闻公告、投机性评论和思想实验中对结果进行大胆的外推是极其容易（有时也令人兴奋）的。

下面是六个在影响数字产品和服务未来方面特别值得关注的AI领域。我将描述它们是什么，为什么重要，它们的应用现状，并列出（绝非详尽）正在研究这些技术的公司和研究人员。

#### **![人工智能](../Images/fe5182fe4b645ad53ad3c5ab25b92b04.png)1\. 强化学习 (RL)**

RL 是一种通过试错学习新任务的范式，灵感来自于人类学习新任务的方式。在典型的RL设置中，代理被任务化为观察其在数字环境中的当前状态，并采取最大化长期奖励的行动。代理从环境中收到每个行动的反馈，以便知道该行动是促进还是阻碍了它的进展。因此，RL代理必须平衡对环境的探索，以找到获得奖励的最佳策略，同时利用其已发现的最佳策略来实现预期目标。这种方法在Google DeepMind关于[Atari游戏和围棋](https://www.youtube.com/watch?v=Ih8EfvOzBOY)的研究中变得流行。在现实世界中，RL的一个例子是优化Google数据中心的能效。在这里，RL系统实现了[40%的减少](https://deepmind.com/blog/deepmind-ai-reduces-google-data-centre-cooling-bill-40%)。在可以模拟的环境（例如视频游戏）中使用RL代理的一个重要固有优势是可以以极低的成本生成大量训练数据。这与通常需要昂贵且难以从现实世界中获取的训练数据的监督深度学习任务形成了鲜明对比。

+   **应用**：多个代理在[共享模型的环境实例中学习](https://arxiv.org/abs/1602.01783)或通过[在同一环境中互动和学习](https://arxiv.org/abs/1605.06676)，学习[在像迷宫这样的3D环境中导航](https://arxiv.org/abs/1606.04671)或城市街道以实现[自主驾驶](https://arxiv.org/abs/1610.03295)，逆向强化学习通过学习任务目标（例如[学习驾驶](https://arxiv.org/abs/1612.03653)）来重现观察到的行为，或者赋予非玩家视频游戏角色类似人类的行为。

+   **主要研究人员**：Pieter Abbeel（OpenAI），David Silver，Nando de Freitas，Raia Hadsell，Marc Bellemare（Google DeepMind），Carl Rasmussen（剑桥），Rich Sutton（阿尔伯塔），John Shawe-Taylor（UCL）及[其他](https://www.quora.com/Who-are-the-best-researchers-in-Deep-Learning-and-or-Reinforcement-Learning-who-mainly-work-in-a-university-not-in-the-industry)。

+   **公司**：Google DeepMind，Prowler.io，Osaro，MicroPSI，Maluuba/Microsoft，NVIDIA，Mobileye。

#### [2\. 生成模型](https://openai.com/blog/generative-models/)

与用于分类或回归任务的判别模型相比，生成模型学习训练示例的概率分布。通过从这个高维分布中采样，生成模型输出的新示例与训练数据相似。例如，一个在真实面孔图像上训练的生成模型可以输出类似面孔的新合成图像。有关这些模型如何工作的更多细节，请参见Ian Goodfellow的[精彩NIPS 2016教程](https://arxiv.org/abs/1701.00160)。他介绍的架构，生成对抗网络（GANs），在研究界特别受关注，因为它们[提供了一条通向无监督学习的路径](https://code.facebook.com/posts/1587249151575490/a-path-to-unsupervised-learning-through-adversarial-networks/)。使用GANs时，有两个神经网络：一个*生成器*，以随机噪声作为输入，负责合成内容（例如图像），以及一个*判别器*，已学会真实图像的样子，负责识别生成器创建的图像是真实的还是伪造的。对抗训练可以被视为一种游戏，生成器必须迭代地学习如何从噪声中创建图像，以便判别器无法再区分生成的图像和真实图像。该框架正被扩展到许多数据模式和任务中。

+   **应用**：模拟时间序列的可能未来（例如，用于强化学习中的任务规划）；[图像超分辨率](https://arxiv.org/abs/1609.04802)；从[2D图像恢复3D结构](https://arxiv.org/abs/1607.00662)；[从小型标记数据集中泛化](https://arxiv.org/abs/1406.5298)；一个输入可以产生多个正确输出的任务（例如，[预测视频中的下一帧](https://arxiv.org/abs/1511.06380)）；在对话接口中创建自然语言（例如，聊天机器人）；[密码学](https://arxiv.org/abs/1610.06918)；在标签不全时的半监督学习；[艺术风格迁移](http://dmlc.ml/mxnet/2016/06/20/end-to-end-neural-style.html)；[合成音乐和声音](https://deepmind.com/blog/wavenet-generative-model-raw-audio/)；图像修复。

+   **公司**：Twitter Cortex，Adobe，Apple，Prisma，Jukedeck*，Creative.ai，Gluru*，Mapillary*，Unbabel。

+   **主要研究人员**：Ian Goodfellow（OpenAI），Yann LeCun 和 [Soumith Chintala](https://research.facebook.com/soumith-chintala)（Facebook AI Research），[Shakir Mohamed](http://shakirm.com/) 和 [Aäron van den Oord](https://twitter.com/avdnoord)（Google DeepMind），Alyosha Efros（伯克利）及其他[研究人员](https://sites.google.com/site/nips2016adversarial/)。

#### ![machine_learning](../Images/9d5f8d7a623362b0bd87dc4210b66be2.png)3\. 具有记忆的网络

为了使 AI 系统能够像我们一样在多样化的现实环境中进行泛化，它们必须能够持续学习新任务并记住如何将所有任务执行到未来。然而，传统的神经网络通常无法在不遗忘的情况下进行这种顺序任务学习。这种缺陷被称为*灾难性遗忘*。它发生的原因是，当网络在之后被训练以解决任务 B 时，解决任务 A 所需的重要权重发生了变化。

然而，确实有几种强大的架构可以赋予神经网络不同程度的记忆。这些架构包括 [长短期记忆](https://en.wikipedia.org/wiki/Long_short-term_memory) 网络（一种递归神经网络变体），它能够处理和预测时间序列；DeepMind 的 [可微神经计算机](https://deepmind.com/blog/differentiable-neural-computers/)，它结合了神经网络和记忆系统，以便从复杂的数据结构中自主学习和导航；[*弹性权重巩固*](https://arxiv.org/abs/1612.00796) 算法，根据权重对先前任务的重要性来减缓特定权重的学习；以及[*渐进式神经网络*](https://arxiv.org/abs/1606.04671)，它学习任务特定模型之间的横向连接，从之前学习的网络中提取有用的特征以用于新任务。

+   **应用**：能够泛化到新环境的学习代理；机器人臂控制任务；自动驾驶车辆；时间序列预测（例如金融市场、视频、物联网）；自然语言理解和下一个词预测。

+   **公司**：Google DeepMind，NNaisense（？），SwiftKey/Microsoft Research，Facebook AI Research。

+   **主要研究人员**：Alex Graves，Raia Hadsell，Koray Kavukcuoglu（Google DeepMind），Jürgen Schmidhuber（IDSIA），Geoffrey Hinton（Google Brain/Toronto），James Weston，Sumit Chopra，Antoine Bordes（FAIR）。

#### 4\. 从较少的数据中学习并构建更小的模型

深度学习模型以需要大量训练数据才能达到最先进的性能而著称。例如，[ImageNet大规模视觉识别挑战赛](http://image-net.org/challenges/LSVRC/2016/)中，团队挑战其图像识别模型，包含120万张手动标记的训练图像，分为1000个物体类别。如果没有大规模的训练数据，深度学习模型将无法收敛到其最佳设置，也无法在语音识别或机器翻译等复杂任务中表现良好。这种数据需求在使用单一神经网络端到端解决问题时尤为突出；即，接受原始音频录音作为输入并输出语音的文本转录。这与使用多个网络每个提供中间表示（例如，原始语音音频输入→音素→单词→文本转录输出；或[从相机直接映射到转向命令的原始像素](https://arxiv.org/abs/1604.07316)）形成对比。如果我们希望人工智能系统解决那些训练数据特别具有挑战性、成本高昂、敏感或耗时的任务，那么开发能够从较少示例（即一-shot或零-shot学习）中学习最佳解决方案的模型是重要的。在小数据集上训练时，面临的挑战包括过拟合、处理异常值的困难、训练和测试数据分布之间的差异。另一种方法是通过转移知识来改善新任务的学习，这些知识是机器学习模型从先前任务中获得的，这些过程统称为[*迁移学习*](http://ftp.cs.wisc.edu/machine-learning/shavlik-group/torrey.handbook09.pdf)。

一个相关的问题是使用类似数量或显著更少的参数构建具有最先进性能的更小深度学习架构。[优势包括](https://arxiv.org/abs/1602.07360)更高效的分布式训练，因为数据需要在服务器之间传输，减少将新模型从云端导出到边缘设备的带宽，并提高在内存有限的硬件上部署的可行性。

+   **应用**：通过学习[模仿深度网络的表现](https://arxiv.org/abs/1312.6184)来训练浅层网络，这些深度网络最初是在大量标记数据上训练的；具有更少参数但性能等同于深度模型的架构（例如，[SqueezeNet](https://arxiv.org/abs/1602.07360)）；[机器翻译](https://research.googleblog.com/2016/11/zero-shot-translation-with-googles.html)。

+   **公司**：Geometric Intelligence/Uber、DeepScale.ai、微软研究院、Curious AI Company、谷歌、Bloomsbury AI。

+   **主要研究人员**：Zoubin Ghahramani（剑桥）、Yoshua Bengio（蒙特利尔）、Josh Tenenbaum（麻省理工学院）、Brendan Lake（纽约大学）、Oriol Vinyals（谷歌DeepMind）、Sebastian Riedel（伦敦大学学院）。

#### 5\. 用于训练和推理的硬件

AI 进步的一个主要催化剂是将图形处理单元（GPU）重新用于训练大型神经网络模型。与以顺序方式计算的中央处理单元（CPU）不同，GPU 提供了一个[大规模并行架构](http://www.nvidia.com/object/what-is-gpu-computing.html)，可以同时处理多个任务。鉴于神经网络必须处理大量（通常是高维）数据，在 GPU 上训练比在 CPU 上要快得多。这就是为什么自从 [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) 2012 年发布以来，GPU 实质上成为了淘金热中的铲子——第一个在 GPU 上实现的神经网络。NVIDIA 继续领先于 Intel、Qualcomm、AMD 和最近的 Google。

然而，GPU 并不是专门为训练或推断而设计的；它们是为了渲染视频游戏图形而创建的。GPU 拥有高计算精度，但这并不总是必要，并且存在内存带宽和数据吞吐量问题。这为新兴初创企业和大型公司如 Google 内部的新项目提供了机会，以专门为高维机器学习应用设计和生产硅片。[新芯片设计承诺的改进](https://www.graphcore.ai/blog/5-reasons-why-we-need-new-machine-learning-hardware)包括更大的内存带宽、在图形而非向量（GPU）或标量（CPU）上进行计算、更高的计算密度、每瓦特效率和性能。这令人兴奋，因为 AI 系统为其所有者和用户提供了明显的加速回报：更快、更高效的模型训练 → 更好的用户体验 → 用户更多地与产品互动 → 生成更大的数据集 → 通过优化提高模型性能。因此，那些能够更快训练和部署计算及能源高效 AI 模型的人将占据显著优势。

+   **应用**: 更快的模型训练（尤其是在图形上）；预测时的能源和数据效率；边缘（IoT 设备）上运行 AI 系统；始终监听的 IoT 设备；云基础设施即服务；自动驾驶汽车、无人机和机器人。

+   **公司**: Graphcore，[Cerebras](http://cerebras.net/)、Isocline Engineering、Google（TPU）、NVIDIA（DGX-1）、Nervana Systems（Intel）、Movidius（Intel）、Scortex

+   **主要研究人员**: ？

#### 6\. 仿真环境

如前所述，为AI系统生成训练数据通常具有挑战性。此外，AI必须能够在多种情况下进行泛化，才能在现实世界中对我们有用。因此，开发能够模拟现实世界物理和行为的数字环境将为我们提供测试平台，以衡量和训练AI的通用智能。这些环境向AI展示原始像素，然后AI采取行动以实现设定（或学习）的目标。在[这些模拟环境](https://en.wikipedia.org/wiki/List_of_computer_simulation_software)中进行训练可以帮助我们[理解AI系统如何学习](http://www.ee.ic.ac.uk/gelenbe/index_files/Intellisim.pdf)，如何改进它们，还可以为我们提供可以潜在转移到现实世界应用的模型。

+   **应用**：[学习驾驶](https://openai.com/blog/GTA-V-plus-Universe/)；制造业；工业设计；游戏开发；智能城市。

+   **公司**：Improbable、Unity 3D、微软（Minecraft）、谷歌DeepMind/暴雪、OpenAI、Comma.ai、虚幻引擎、亚马逊Lumberyard

+   **研究员**：[安德烈亚·维达尔迪](http://www.robots.ox.ac.uk/~vgg/research/researchdoom/)（牛津）

[原创帖子](https://medium.com/@NathanBenaich/6-areas-of-artificial-intelligence-to-watch-closely-673d590aa8aa)。经许可转载。

**简介：[内森·贝奈奇](https://medium.com/@NathanBenaich)** 投资于科技公司[@PlayfairCapital](https://twitter.com/PlayfairCapital)。涉及数据、机器智能、用户体验。前癌症研究员、摄影师、永恒的美食爱好者。

**相关：**

+   [通过AI/持续学习实现物联网的持续改进](/2016/11/continuous-improvement-iot-ai-learning.html)

+   [强化学习与物联网](/2016/08/reinforcement-learning-internet-things.html)

+   [增强版聊天机器人：推动聊天机器人的10项关键机器学习能力](/2017/01/chatbots-steroids-10-key-machine-learning-capabilities.html)

### 了解更多

+   [成为出色数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
