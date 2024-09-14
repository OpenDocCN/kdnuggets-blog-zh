# 使用 OpenAI Gym、RLlib 和 Google Colab 进行强化学习简介

> 原文：[https://www.kdnuggets.com/2021/09/intro-reinforcement-learning-openai-gym-rllib-colab.html](https://www.kdnuggets.com/2021/09/intro-reinforcement-learning-openai-gym-rllib-colab.html)

[评论](#comments)

**由 [Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/) 和 [Sven Mika](https://www.anyscale.com/blog?author=sven-mika)**

本教程将使用强化学习（RL）来帮助平衡一个虚拟的 CartPole。上面的 [视频](https://www.youtube.com/watch?v=XiigTGKZfks&t=106s) 来自 PilcoLearner，展示了在真实 CartPole 环境中使用 RL 的结果。

强化学习（RL）的一个可能定义是，一种计算方法，用于学习如何在与环境交互时最大化奖励的总和。虽然定义是有用的，但本教程旨在通过图像、代码和视频示例来展示强化学习是什么，并在此过程中介绍像代理和环境这样的强化学习术语。

特别是，本教程探讨：

+   什么是强化学习

+   OpenAI Gym CartPole 环境

+   代理在强化学习中的作用

+   如何使用 Python 库 RLlib 训练代理

+   如何使用 GPU 加速训练

+   使用 Ray Tune 进行超参数调优

## 什么是强化学习

正如 [之前的帖子提到的](https://www.anyscale.com/blog/reinforcement-learning-with-rllib-in-the-unity-game-engine)，机器学习（ML），作为 AI 的一个子领域，利用神经网络或其他类型的数学模型来学习如何解释复杂的模式。由于其成熟度高，最近非常流行的两个 ML 领域是监督学习（SL），在这个领域中，神经网络学习基于大量数据进行预测，以及强化学习（RL），在该领域中，网络通过模拟器以试错的方式学习做出良好的行动决策。

![intro-to-rl-1](../Images/f4070fd03af2cf9672e423792ec19925.png)

RL 是 DeepMind 的 AlphaGo Zero 和 StarCraft II AI（AlphaStar）或 OpenAI 的 DOTA 2 AI（“OpenAI Five”）等令人惊叹的成功背后的技术。请注意，[强化学习的许多令人印象深刻的应用](https://www.anyscale.com/blog/best-reinforcement-learning-talks-from-ray-summit-2021) 以及它在现实决策问题中如此强大和有前景的原因是，RL 能够不断学习——有时甚至在不断变化的环境中——从完全没有做出决策的知识（随机行为）开始。

## 代理和环境

![agents-and-environments](../Images/c4030151c211208821ed55d1d5fc4a13.png)

上图展示了代理与环境之间的交互和通信。在强化学习中，一个或多个代理在一个环境中进行交互，这个环境可能是像本教程中的 CartPole 这样的模拟，也可能是连接到现实世界传感器和执行器的环境。在每一步，代理接收一个观察（即环境的状态），采取一个动作，并通常接收一个奖励（代理接收奖励的频率取决于给定任务或问题）。代理通过重复试验进行学习，这些试验的序列称为一个 episode——从初始观察到“成功”或“失败”的一系列动作，使环境达到“完成”状态。强化学习框架的学习部分训练一个策略，以确定哪些动作（即顺序决策）可以使代理最大化其长期的累积奖励。

## OpenAI Gym Cartpole 环境

![Figure](../Images/fb66617e97423db91b7a05be0ca7dab1.png)

CartPole

我们要解决的问题是保持杆子直立。具体来说，杆子通过一个未驱动的关节连接到一个沿着无摩擦轨道移动的小车上。摆锤从直立位置开始，目标是通过增加或减少小车的速度来防止它倒下。

![policy](../Images/76939bcc741d03593c2fcc04fa4e1a03.png)

与其从头编写这个环境的代码，不如使用 [OpenAI Gym](http://gym.openai.com/)，这是一个提供各种模拟环境的工具包（例如 Atari 游戏、棋盘游戏、2D 和 3D 物理模拟等）。Gym 不对代理的结构做任何假设（在这个 cartpole 示例中是什么推动小车向左或向右），并且与任何数值计算库（如 numpy）兼容。

以下代码加载了 cartpole 环境。

```py
import gym
env = gym.make("CartPole-v0")
```

现在让我们通过查看动作空间来理解这个环境。

```py
env.action_space
```

![env.action_space](../Images/4f953445588d3239751a5922010d343a.png)

输出 Discrete(2) 表示有两个动作。在 cartpole 中，0 对应于“将小车推向左侧”，而 1 对应于“将小车推向右侧”。注意，在这个特定的示例中，静止不动不是一个选项。在强化学习中，代理产生一个动作输出，这个动作被发送到环境中，环境随后作出反应。环境生成一个观察（以及一个奖励信号，此处未显示），我们可以看到下面的内容：

```py
env.reset()
```

![env.reset ()](../Images/33891c8ea8faa80c232aaed5c512eed7.png)

观察是一个维度为 4 的向量，包括小车的 x 位置、小车的 x 速度、杆子的角度（以弧度表示，1 弧度 = 57.295 度）以及杆子的角速度。上面显示的数字是开始新一集后的初始观察（`env.reset()`）。每个时间步（和动作）后，观察值将会变化，具体取决于小车和杆子的状态。

## 训练代理

在强化学习中，代理的目标是随着时间的推移产生越来越聪明的动作。它通过策略来实现这一点。在深度强化学习中，这一策略通过神经网络来表示。我们首先与gym环境进行互动，不使用神经网络或任何机器学习算法。相反，我们从随机运动（左或右）开始。这只是为了理解机制。

![策略随机移动](../Images/8cffa24f22da5da6a05db5329f856048.png)

以下代码重置环境并执行20步（20个周期），每次都采取随机动作并打印结果。

```py
# returns an initial observation
env.reset()

for i in range(20):

  # env.action_space.sample() produces either 0 (left) or 1 (right).
  observation, reward, done, info = env.step(env.action_space.sample())

  print("step", i, observation, reward, done, info)

env.close()
```

![步骤](../Images/b12cc3182b86e800c621b97f36132b17.png)

示例输出。cartpole 中有多个回合终止条件。在图像中，回合因超过12度（0.20944弧度）而终止。其他回合终止条件包括小车位置超过2.4（即小车中心到达显示屏边缘）、回合长度超过200，或解决要求，即平均回报在100次连续试验中大于或等于195.0。

上面打印的输出显示了以下内容：

+   步骤（环境已经循环了多少次）。在每个时间步中，代理选择一个动作，环境返回一个观察结果和一个奖励。

+   环境的观察 [x 小车位置, x 小车速度, 杆角度（弧度），杆角速度]

+   奖励是通过先前的动作获得的。这个尺度在不同环境中有所不同，但目标始终是增加你的总奖励。对于cartpole，每一步的奖励是1，包括终止步骤。之后奖励为0（图中的第18和19步）。

+   done 是一个布尔值。它指示是否需要重新设置环境。大多数任务被划分为定义明确的回合，done 为 True 表示回合已经结束。在 cart pole 中，这可能是由于杆子倾斜得太远（超过12度/0.20944弧度）、位置超过2.4（即小车中心到达显示屏边缘）、回合长度超过200，或解决要求，即平均回报在100次连续试验中大于或等于195.0。

+   info 是有用的诊断信息，用于调试。对于这个 cartpole 环境，它是空的。

尽管这些数字是有用的输出，但视频可能更清晰。如果你在 Google Colab 中运行此代码，请注意，生成视频时没有可用的显示驱动程序。然而，可以安装虚拟显示驱动程序以使其正常工作。

```py
# install dependencies needed for recording videos
!apt-get install -y xvfb x11-utils
!pip install pyvirtualdisplay==0.2.*
```

下一步是启动虚拟显示实例。

```py
from pyvirtualdisplay import Display
display = Display(visible=False, size=(1400, 900))
_ = display.start()
```

OpenAI gym 有一个 VideoRecorder 包装器，可以将运行环境录制为 MP4 格式的视频。以下代码与之前相同，只是它录制了200步。

```py
from gym.wrappers.monitoring.video_recorder import VideoRecorder
before_training = "before_training.mp4"

video = VideoRecorder(env, before_training)
# returns an initial observation
env.reset()
for i in range(200):
  env.render()
  video.capture_frame()
  # env.action_space.sample() produces either 0 (left) or 1 (right).
  observation, reward, done, info = env.step(env.action_space.sample())
  # Not printing this time
  #print("step", i, observation, reward, done, info)

video.close()
env.close()
```

![用户警告](../Images/bacf6b8d7d383ca0f38b53d49dfa3ad2.png)

> 通常，当 done 为 1（True）时，你会结束仿真。上面的代码让环境在达到终止条件后继续运行。例如，在 CartPole 中，这可能是当杆子倾斜、杆子消失在屏幕外，或达到其他终止条件时。

上面的代码将视频文件保存到了 Colab 磁盘中。为了在笔记本中显示它，你需要一个辅助函数。

```py
from base64 import b64encode
def render_mp4(videopath: str) -> str:
  """
  Gets a string containing a b4-encoded version of the MP4 video
  at the specified path.
  """
  mp4 = open(videopath, 'rb').read()
  base64_encoded_mp4 = b64encode(mp4).decode()
  return f'<video width=400 controls><source src="data:video/mp4;' \
         f'base64,{base64_encoded_mp4}" type="video/mp4"></video>'
```

下面的代码渲染了结果。你应该能获得一个类似于下面的视频。

```py
from IPython.display import HTML
html = render_mp4(before_training)
HTML(html)
```

播放视频显示，随机选择动作并不是保持 CartPole 直立的好策略。

## 如何使用 Ray 的 RLlib 训练智能体

本教程的前一部分让我们的智能体随机执行动作，忽略了来自环境的观察和奖励。智能体的目标是随着时间的推移产生越来越智能的动作，而随机动作无法实现这一目标。为了让智能体随着时间的推移产生更智能的动作，它需要一个更好的策略。在深度强化学习中，策略是通过神经网络来表示的。

![policy deep-rl](../Images/bcf88c97a0e018a6555b2e832af00d3e.png)

本教程将使用 [RLlib 库](https://docs.ray.io/en/master/rllib.html) 来训练一个更智能的智能体。RLlib 具有许多优势，比如：

+   极大的灵活性。它允许你定制 RL 周期的每一个方面。例如，本教程的这一部分将使用 PyTorch 制作一个自定义神经网络策略（RLlib 也原生支持 TensorFlow）。

+   可扩展性。强化学习应用程序可能会非常消耗计算资源，并且通常需要扩展到集群以加速训练。RLlib 不仅对 GPU 提供一流的支持，还建立在 [Ray](https://ray.io/) 之上，这使得从笔记本电脑到集群的 Python 程序扩展变得简单。

+   统一的 API 以及对离线、基于模型、无模型、多智能体算法等的支持（这些算法在本教程中不会被探讨）。

+   作为 [Ray Project 生态系统](https://github.com/ray-project/ray)的一部分。这有一个好处，即 RLlib 可以与生态系统中的其他库一起运行，如 [Ray Tune](https://docs.ray.io/en/master/tune/index.html)，这是一个用于实验执行和超参数调整的库，支持任何规模（稍后会详细介绍）。

虽然这些功能中的一些在本帖中不会被完全利用，但它们对于当你想做一些更复杂的事情和解决实际问题时非常有用。你可以在 [这里](https://www.anyscale.com/blog/best-reinforcement-learning-talks-from-ray-summit-2021) 了解一些 RLlib 的令人印象深刻的用例。

要开始使用 RLlib，你需要首先安装它。

```py
!pip install 'ray[rllib]'==1.6
```

现在你可以使用 Proximal Policy Optimization (PPO) 算法训练一个 PyTorch 模型。这是一种非常全面的、适合所有场景的算法，你可以在[这里](https://docs.ray.io/en/master/rllib-algorithms.html#proximal-policy-optimization-ppo)了解更多。下面的代码使用了一个包含 32 个神经元和线性激活函数的单隐层神经网络。

```py
import ray
from ray.rllib.agents.ppo import PPOTrainer
config = {
    "env": "CartPole-v0",
    # Change the following line to `“framework”: “tf”` to use tensorflow
    "framework": "torch",
    "model": {
      "fcnet_hiddens": [32],
      "fcnet_activation": "linear",
    },
}
 stop = {"episode_reward_mean": 195}
 ray.shutdown()
ray.init(
  num_cpus=3,
  include_dashboard=False,
  ignore_reinit_error=True,
  log_to_driver=False,
)
# execute training 
analysis = ray.tune.run(
  "PPO",
  config=config,
  stop=stop,
  checkpoint_at_end=True,
)
```

这段代码应该会产生相当多的输出。最终条目应该类似于：

![状态](../Images/55c8f51dc6dba6aecf920e54657ea47f.png)

该条目显示解决环境需要 35 次迭代，运行了 258 秒。每次可能会有所不同，但每次迭代大约需要 7 秒（258 / 35 = 7.3）。如果你希望了解 Ray API 并查看类似 ray.shutdown 和 ray.init 的命令，你可以[查看这个教程](https://www.anyscale.com/blog/writing-your-first-distributed-python-application-with-ray)。

## 如何使用 GPU 加速训练

尽管教程的其余部分使用了 CPU，但值得注意的是，你可以通过在 Google Colab 中使用 GPU 来加速模型训练。可以通过选择**运行时 > 更改运行时类型**并将硬件加速器设置为**GPU**来完成。然后选择**运行时 > 重启并运行所有**。

![状态 2](../Images/35133d87395977e42ff8f7e55d01a004.png)

注意到，尽管训练迭代的次数可能差不多，但每次迭代的时间显著减少（从 7 秒减少到 5.5 秒）。

## 创建训练模型运行的视频

RLlib 提供了一个 Trainer 类，用于持有环境交互的策略。通过训练器接口，可以训练策略、计算动作和保存检查点。虽然之前从*ray.tune.run*返回的分析对象没有包含任何训练器实例，但它拥有从保存的检查点重建一个所需的所有信息，因为传递了*checkpoint_at_end=True*作为参数。下面的代码展示了这一点。

```py
# restore a trainer from the last checkpoint
trial = analysis.get_best_logdir("episode_reward_mean", "max")
checkpoint = analysis.get_best_checkpoint(
  trial,
  "training_iteration",
  "max",
)
trainer = PPOTrainer(config=config)
trainer.restore(checkpoint)
```

现在我们再创建一个视频，但这次选择训练模型推荐的动作，而不是随机行动。

```py
after_training = "after_training.mp4"
after_video = VideoRecorder(env, after_training)
observation = env.reset()
done = False
while not done:
  env.render()
  after_video.capture_frame()
  action = trainer.compute_action(observation)
  observation, reward, done, info = env.step(action)
after_video.close()
env.close()
# You should get a video similar to the one below. 
html = render_mp4(after_training)
HTML(html)
```

这次，杆子平衡得很好，这意味着代理已经解决了 cartpole 环境！

## 使用 Ray Tune 进行超参数调整

![图示](../Images/c8485cf99eafb673e010078db6660b81.png)

Ray 生态系统

RLlib 是一个强化学习库，是 Ray 生态系统的一部分。Ray 是一个高度可扩展的通用框架，用于并行和分布式 Python。它非常通用，这种通用性对于支持其库生态系统至关重要。生态系统涵盖了从[训练](https://docs.ray.io/en/latest/tune/index.html)、到[生产服务](https://docs.ray.io/en/master/serve/index.html)、到[数据处理](https://www.anyscale.com/blog/data-processing-support-in-ray)等各方面。你可以将多个库一起使用，构建执行这些操作的应用程序。

本部分教程使用了[Ray Tune](https://docs.ray.io/en/latest/tune/index.html)，这是 Ray 生态系统中的另一个库。它是一个用于实验执行和超参数调优的库，支持任何规模。虽然本教程只使用网格搜索，但请注意 Ray Tune 还提供了更高效的超参数调优算法，如基于种群的训练、BayesOptSearch 和 HyperBand/ASHA。

现在让我们尝试找出能够在最少时间步数内解决 CartPole 环境的超参数。

输入以下代码，准备好可能需要一段时间来运行：

```py
parameter_search_config = {
    "env": "CartPole-v0",
    "framework": "torch",

    # Hyperparameter tuning
    "model": {
      "fcnet_hiddens": ray.tune.grid_search([[32], [64]]),
      "fcnet_activation": ray.tune.grid_search(["linear", "relu"]),
    },
    "lr": ray.tune.uniform(1e-7, 1e-2)
}

# To explicitly stop or restart Ray, use the shutdown API.
ray.shutdown()

ray.init(
  num_cpus=12,
  include_dashboard=False,
  ignore_reinit_error=True,
  log_to_driver=False,
)

parameter_search_analysis = ray.tune.run(
  "PPO",
  config=parameter_search_config,
  stop=stop,
  num_samples=5,
  metric="timesteps_total",
  mode="min",
)

print(
  "Best hyperparameters found:",
  parameter_search_analysis.best_config,
)
```

通过传递 num_cpus=12 到 ray.init 请求 12 个 CPU 核心时，四个试验将在三个 CPU 上并行运行。如果这不起作用，可能是 Google 更改了 Colab 上可用的虚拟机。任何三个或更多的值都应该有效。如果 Colab 由于内存不足而出错，你可能需要执行**Runtime > Factory reset runtime**，然后**Runtime > Run all**。注意 Colab 笔记本右上角有显示 RAM 和磁盘使用情况的区域。

指定 num_samples=5 意味着你将获得五个随机样本用于学习率。对于每一个样本，有两个隐藏层大小的值和两个激活函数的值。因此，将会有 5 * 2 * 2 = 20 个试验，试验状态将在单元格的输出中显示。

注意 Ray 在运行过程中会打印当前的最佳配置。这包括所有已设置的默认值，这是找到可以调整的其他参数的好地方。

运行后，最终输出可能类似于以下输出：

INFO tune.py:549 -- 总运行时间：3658.24 秒（调优循环的时间为 3657.45 秒）。

找到的最佳超参数：{'env': 'CartPole-v0', 'framework': 'torch', 'model': {'fcnet_hiddens': [64], 'fcnet_activation': 'relu'}, 'lr': 0.006733929096170726};'''

因此，在二十组超参数中，具有 64 个神经元、ReLU 激活函数和大约 6.7e-3 学习率的组合表现最好。

## 结论

![图](../Images/96b0fd8599b56a82579732a4fa9ecfdd.png)

Neural MMO 是一个从大规模多人在线游戏建模的环境——这种游戏类型支持数百到数千名同时在线玩家。你可以通过点击[这里](https://www.anyscale.com/blog/best-reinforcement_learning_talks_from_ray_summit_2021)了解 Ray 和 RLlib 如何帮助实现这些和其他项目的关键功能。

本教程通过介绍强化学习术语、展示代理与环境的互动，并通过代码和视频示例演示这些概念，从而阐明了强化学习的内容。如果你想深入了解强化学习，建议查看 [Sven Mika 的 RLlib 教程](https://www.anyscale.com/events/2021/06/24/hands-on-reinforcement-learning-with-rays-rllib)。这是学习 RLlib 最佳实践、多代理算法及更多内容的绝佳方式。如果你希望随时了解 RLlib 和 Ray 的所有最新动态，考虑 [关注 @raydistributed 的 Twitter](https://twitter.com/raydistributed) 并 [注册 Ray 新闻通讯](https://anyscale.us5.list-manage.com/subscribe?u=524b25758d03ad7ec4f64105f&id=d94e960a03)。

[原文](https://www.anyscale.com/blog/an-introduction-to-reinforcement-learning-with-openai-gym-rllib-and-google) 已获授权转载。

**相关：**

+   [强化学习入门](/2021/04/getting-started-reinforcement-learning.html)

+   [Facebook 推出历史上最具挑战性的强化学习挑战之一](/2021/06/facebook-launches-toughest-reinforcement-learning-challenges.html)

+   [强化学习的 10 个实际应用](/2021/04/10-real-life-applications-reinforcement-learning.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关内容

+   [在 Google Colab 上运行 Redis](https://www.kdnuggets.com/2022/01/running-redis-google-colab.html)

+   [从 Google Colab 到 Ploomber 管道：使用 GPU 进行大规模机器学习](https://www.kdnuggets.com/2022/03/google-colab-ploomber-pipeline-ml-scale-gpus.html)

+   [在 Google Colab 上免费微调 LLAMAv2 与 QLora](https://www.kdnuggets.com/fine-tuning-llamav2-with-qlora-on-google-colab-for-free)

+   [在 Google Colab 上免费运行 Mixtral 8x7b](https://www.kdnuggets.com/running-mixtral-8x7b-on-google-colab-for-free)

+   [RAPIDS cuDF 在 Google Colab 上加速数据科学](https://www.kdnuggets.com/2023/01/rapids-cudf-accelerated-data-science-google-colab.html)

+   [OpenAI 的 Whisper API 用于转录和翻译](https://www.kdnuggets.com/2023/06/openai-whisper-api-transcription-translation.html)
