# 使用 torchlayers 轻松构建 PyTorch 模型

> 原文：[https://www.kdnuggets.com/2020/04/pytorch-models-torchlayers.html](https://www.kdnuggets.com/2020/04/pytorch-models-torchlayers.html)

[评论](#comments)

根据 [在线搜索](https://trends.google.com/trends/explore?date=today%205-y&geo=US&q=%2Fg%2F11gd3905v1) 和更重要的 [PyTorch 采用率的持续增长](/2020/01/openai-pytorch-adoption.html) 来看，PyTorch 继续受到广泛关注。PyTorch 被认为是强大而灵活的，这对研究人员来说是受欢迎的。然而，过去 PyTorch 因缺乏类似 TensorFlow 的 Keras 的简化高级 API 而受到批评。这个情况最近发生了变化。

![图示](../Images/920a252480639f8131d0b2e4b22d5974.png)

**[torchlayers](https://github.com/szymonmaszke/torchlayers)** 旨在为 PyTorch 做到 Keras 对 TensorFlow 所做的事情。简洁地由项目开发者定义：

> torchlayers 是一个基于 PyTorch 的库，提供 **`torch.nn` 层的自动形状和维度推断** + 当前 SOTA 架构中的附加构建块（例如 Efficient-Net）。
> 
> 上述操作无需用户干预（除了对 torchlayers.build 的单次调用），类似于 Keras 中的操作。

除了上述的形状和维度推断，torchlayers 还包括类似 Keras 的附加层，例如 `[torchlayers.Reshape](https://szymonmaszke.github.io/torchlayers/packages/torchlayers.html?highlight=reshape#torchlayers.Reshape)`（在保持批次维度的同时重塑输入张量），包含之前在 ImageNet 竞赛中看到的 SOTA 层（例如 `[PolyNet](https://szymonmaszke.github.io/torchlayers/packages/torchlayers.convolution.html?highlight=polynet#torchlayers.convolution.Poly)`），并提供一些有用的默认设置，例如卷积核大小（torchlayers 的默认值为 3）。

使用 pip 安装很简单：

```py
pip install --user torchlayers
```

额外的安装信息（如 Docker 镜像和 GPU）[可以在这里找到](https://szymonmaszke.github.io/torchlayers/#installation)。完整的 torchlayers 文档 [可以在这里找到](https://szymonmaszke.github.io/torchlayers/)。

torchlayers 的 GitHub 页面提供了一些示例来展示其功能。我喜欢这个 [简单的图像和文本分类器！](https://github.com/szymonmaszke/torchlayers#simple-image-and-text-classifier-in-one) 示例，下面我复制了它的代码。这个示例展示了：

+   `torch.nn` 和 `torchlayers` 层的混合使用

+   形状和维度推断（`Conv` 和 `Linear` 输入及 `BatchNorm`）

+   默认卷积核大小

+   `Conv` 填充默认设置为 "same"

+   使用 torchlayers 池化层（`GlobalMaxPool`，类似于 Keras）

```py
import torch
import torchlayers as tl

# torch.nn and torchlayers can be mixed easily
model = torch.nn.Sequential(
    tl.Conv(64),                   # specify ONLY out_channels
    torch.nn.ReLU(),               # use torch.nn wherever you wish
    tl.BatchNorm(),                # BatchNormNd inferred from input
    tl.Conv(128),                  # Default kernel_size equal to 3
    tl.ReLU(),
    tl.Conv(256, kernel_size=11),  # "same" padding as default
    tl.GlobalMaxPool(),            # Known from Keras
    tl.Linear(10),                 # Output for 10 classes
)
```

然后可以使用 `[torchlayers.build](https://szymonmaszke.github.io/torchlayers/packages/torchlayers.html?highlight=build#torchlayers.build)` 来构建一个定义的网络，同时指定输入形状（下面展示了图像和文本分类的输入形状，适用于上面定义的模型）：

```py
# Image...
mnist_model = tl.build(model, torch.randn(1, 3, 28, 28))

# ...or text
# [batch, embedding, timesteps], first dimension > 1 for BatchNorm1d to work
text_model = tl.build(model, torch.randn(2, 300, 1))

```

`build` 显然像在 Keras 中一样工作，将模型编译为 PyTorch 基本操作；它通过 `post_build` 函数（例如下面显示的权重初始化）提供一些附加功能，你可以在[这里查看更多](https://szymonmaszke.github.io/torchlayers/packages/torchlayers.html?highlight=build#torchlayers.build)。

```py
class _MyModuleImpl(torch.nn.Linear):
    def post_build(self):
        # You can do anything here really
        torch.nn.init.eye_(self.weights)
```

[torchlayers](https://github.com/szymonmaszke/torchlayers) 提供了一些有助于使用 PyTorch 进行类似 Keras 的模型构建的功能，并填补了明显的空白。时间将证明该项目如何发展和长期受欢迎，但它无疑有一个有前途的开始。

**相关**：

+   [OpenAI 正在采用 PyTorch... 他们并不孤单](/2020/01/openai-pytorch-adoption.html)

+   [PyTorch 1.2 的温和介绍](/2019/09/gentle-introduction-pytorch-12.html)

+   [使用 TensorFlow 和 Keras 进行标记化和文本数据准备](/2020/03/tensorflow-keras-tokenization-text-data-prep.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [轻松从网站抓取图像，无需编码](https://www.kdnuggets.com/2022/06/octoparse-scrape-images-easily-websites-nocoding-way.html)

+   [在你的笔记本电脑上轻松探索 LLMs 使用 openplayground](https://www.kdnuggets.com/2023/04/explore-llms-easily-laptop-openplayground.html)

+   [使用 Scikit-LLM 将 LLMs 轻松集成到你的 Scikit-learn 工作流中](https://www.kdnuggets.com/easily-integrate-llms-into-your-scikit-learn-workflow-with-scikit-llm)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)

+   [使用 PyTorch 进行迁移学习的实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [如何使用图形数据库构建实时推荐引擎](https://www.kdnuggets.com/2023/08/build-realtime-recommendation-engine-graph-databases.html)
