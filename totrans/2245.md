# Keras 3.0：你需要知道的一切

> 原文：[https://www.kdnuggets.com/2023/07/keras-30-everything-need-know.html](https://www.kdnuggets.com/2023/07/keras-30-everything-need-know.html)

![Keras 3.0：你需要知道的一切](../Images/7b824424c35c725d8c30d2656e25f025.png)

作者通过 Playground AI 创建的图像

在我们深入了解这一令人兴奋的发展之前，让我们探讨一个场景以更好地理解它。设想你是一位资深数据科学家，负责一个复杂的图像分类项目。你的基于 TensorFlow 的模型表现非常出色。然而，随着你添加更多功能，你发现有些团队成员倾向于使用 JAX 以实现可扩展性，而其他人则偏爱 PyTorch 因为它更易于使用。作为团队领导，你如何确保在维护模型在各种深度学习框架中的效率的同时，实现无缝协作？

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速迈向网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

认识到这一挑战，Keras 团队推出了 Keras Core——一种创新的多后端实现 Keras API 的库，支持 TensorFlow、JAX 和 PyTorch。该库将于 2023 年秋季发展成 Keras 3.0。 在我们直接进入 Keras 3.0 之前，先简要回顾一下 Keras 的历史。

# Keras 的简史与 3.0 之路

2015 年，François Chollet 推出了 Keras，一个用 Python 编写的开源深度学习库。这个简单而强大的 API 很快在研究人员、学生和专业人士中获得了广泛欢迎，因为它简化了复杂的神经网络构建。随着时间的推移，Keras 得到了显著的增强，使其对深度学习社区更具吸引力。最终，Keras 成为 TensorFlow 的一个重要组成部分——Google 的前沿深度学习框架。与此同时，Facebook 的 AI 研究实验室开发了 PyTorch，以其直观和灵活的模型构建而闻名。与此同时，JAX 作为另一个强大的高性能机器学习研究框架出现了。随着这些框架的蓬勃发展，开发者开始面临选择框架的困境。这导致了深度学习社区的进一步碎片化。

由于面临碎片化框架带来的挑战，Keras 的开发者决定再次彻底改造该库，诞生了 Keras 3.0。

# Keras 3.0 的显著特点

Keras 3.0使你能够与团队有效协作。你可以通过结合TensorFlow、JAX和PyTorch的优点来开发复杂的模型。以下是Keras 3.0被认为是绝对改变游戏规则的一些特性：

## 1\. 多后端支持

Keras 3.0充当超级连接器，实现了TensorFlow、JAX和PyTorch的无缝使用。开发者可以自由地混合和匹配最适合特定任务的工具，而无需更改代码。

## 2\. 性能优化

性能优化是Keras 3.0的关键特性。默认情况下，Keras 3.0利用XLA（加速线性代数）编译。XLA编译优化你的数学计算，使它们在GPU和TPU等硬件上运行得更快。它还允许你动态选择最佳的后端，以确保最佳效率。这些性能优化功能大大加快了训练速度，使你能够训练更多模型，进行更多实验，并更快地获得结果。

## 3\. 扩展的生态系统表面

你的Keras模型可以作为PyTorch模块、TensorFlow SavedModels或JAX大规模TPU训练基础设施的一部分使用。这意味着你可以利用每个框架的优势。因此，凭借Keras 3.0扩展的生态系统，你不会被锁定在单一的生态系统中。这就像一个通用适配器，让你可以将喜欢的设备连接到任何机器上。

## 4\. 跨框架底层语言

**keras_core.ops**命名空间的引入是一个突破性特征，它允许你一次性编写自定义操作，并在不同的深度学习框架中轻松使用。**keras_core.ops**提供了一组工具和函数，类似于流行的NumPy API，NumPy是Python中广泛使用的数值计算库。这种跨框架的兼容性促进了代码的重用，并鼓励了协作。

## 5\. 复杂性的渐进揭示

Keras 3.0的设计方法使其与其他深度学习框架不同。假设你是一个初学者，想使用Keras 3.0构建一个简单的神经网络。它在开始时为你提供了最简单的工作流程。一旦你熟悉了基础知识，你可以访问所有高级功能和底层功能。它不限制你只使用预定义的工作流程。这种方法的魅力在于其适应性，既欢迎初学者，也适合经验丰富的深度学习从业者。

## 6\. 层、模型、指标和优化器的无状态API

在深度学习的背景下，状态指的是在训练过程中变化的内部变量和参数。然而，JAX 的工作原理是无状态的，这意味着函数没有可变变量或内部状态。Keras 3.0 通过**无状态 API**拥抱 JAX 的无状态性。它允许深度学习的核心组件，即层、模型、度量标准和优化器以无状态的方式进行设计。这种独特的兼容性使 Keras 3.0 成为现代 AI 开发中不可或缺的工具。

# 开始使用 Keras 3.0

Keras Core 兼容 Linux 和 MacOS 系统。设置 Keras 3.0 是一个简单的过程。以下是逐步指南供你参考：

## 1\. 克隆并导航到仓库

使用以下命令将仓库克隆到本地系统中

```py
git clone https://github.com/keras-team/keras-core.git
```

使用以下命令将根目录更改为克隆的 keras-core：

```py
cd keras-core
```

## 2\. 安装依赖项

打开你的终端并运行以下命令以安装所需的依赖项。

```py
pip install -r requirements.txt
```

## 4\. 运行安装命令

运行以下脚本以处理安装过程：

```py
python pip_build.py --install
```

## 5\. 配置后端

默认情况下，Keras Core 严格要求 TensorFlow 作为后端框架，但你可以使用以下两种方法进行配置：

**选项 01：** 你可以将 KERAS_BACKEND 环境变量设置为你首选的后端选项。

```py
export KERAS_BACKEND="jax"
```

**选项 02：** 你可以编辑位于 ~/.keras/keras.json 的本地 Keras 配置文件。在文本编辑器中打开文件，并将“backend”选项更改为你首选的后端。

```py
{
    "backend": "jax",
    "floatx": "float32",
    "epsilon": 1e-7,
    "image_data_format": "channels_last"
}
```

## 6\. 验证安装

为确保 Keras Core 与你选择的后端正确安装，你可以通过导入库进行测试。打开 Python 解释器或 Jupyter Notebook 并运行以下命令：

```py
import keras_core as keras
```

# 结束说明

虽然 Keras 3.0 有一些限制，比如当前的 TensorFlow 依赖和与其他后端的有限 tf.data 支持，但这个框架的未来潜力是值得期待的。Keras 目前已发布了测试版，并鼓励开发者提供宝贵的反馈。如果你有兴趣了解更多信息，可以在[Keras Core (Keras 3.0) 文档](https://keras.io/api/)中找到相关资料。不要害怕尝试新想法。Keras 3.0 是一款强大的工具，现在是参与进化的激动人心的时刻。

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1)** 是一位有志的软件开发者，对数据科学和 AI 在医学中的应用有浓厚的兴趣。Kanwal 被选为 2022 年 APAC 区域的 Google Generation Scholar。Kanwal 喜欢通过撰写有关趋势话题的文章来分享技术知识，并对改善女性在科技行业中的代表性感到热情。

### 更多相关话题

+   [KDnuggets 新闻，4 月 13 日：数据科学家应关注的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [朴素贝叶斯算法：你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [关于张量的所有信息](https://www.kdnuggets.com/2022/05/everything-need-know-tensors.html)

+   [关于数据湖屋的所有信息](https://www.kdnuggets.com/2022/09/everything-need-know-data-lakehouses.html)

+   [关于MLOps的所有信息：KDnuggets技术简报](https://www.kdnuggets.com/tech-brief-everything-you-need-to-know-about-mlops)

+   [ChatGPT：你需要知道的一切](https://www.kdnuggets.com/2023/01/chatgpt-everything-need-know.html)
