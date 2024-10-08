# Safetensors 介绍

> 原文：[`www.kdnuggets.com/2023/07/introduction-safetensors.html`](https://www.kdnuggets.com/2023/07/introduction-safetensors.html)

![Safetensors 介绍](img/9cf91bd787856ceb22649f830d86e344.png)

作者提供的图片

# 什么是 Safetensors？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

Hugging Face 开发了一种新的序列化格式，称为 [Safetensors](https://huggingface.co/docs/safetensors/index)，旨在简化和优化大规模复杂张量的存储和加载。张量是深度学习中使用的主要数据结构，其大小可能在效率上带来挑战。

Safetensors 使用有效的序列化和压缩算法组合来减少大张量的大小，使其比其他序列化格式如 pickle 更快、更高效。这意味着，Safetensors 在 CPU 上比传统的 PyTorch 序列化格式 `pytorch_model.bin` 快 76.6 倍，在 GPU 上快 2 倍。查看 [速度比较](https://huggingface.co/docs/safetensors/speed)。

# 使用 Safetensors 的好处

## 易用性

Safetensors 提供了一个简单直观的 API，用于在 Python 中序列化和反序列化张量。这意味着开发者可以专注于构建他们的深度学习模型，而不必花时间在序列化和反序列化上。

## 跨平台兼容性

你可以在 Python 中进行序列化，并方便地在各种编程语言和平台上加载生成的文件，例如 C++、Java 和 JavaScript。这使得在不同编程环境之间无缝共享模型成为可能。

## 速度

Safetensors 针对速度进行了优化，能够高效地处理大张量的序列化和反序列化。因此，它是使用大规模语言模型应用的优秀选择。

## 大小优化

它使用有效的序列化和压缩算法的结合来减小大张量的大小，从而比其他序列化格式如 pickle 提供更快、更高效的性能。

## 安全

为了防止在存储或传输序列化张量过程中发生任何损坏，Safetensors 使用了校验和机制。这提供了一层额外的安全保障，确保所有存储在 Safetensors 中的数据都是准确和可靠的。此外，它还能防止 [DOS 攻击](https://github.com/huggingface/safetensors#benefits)。

## 延迟加载

在分布式设置中，当有多个节点或 GPU 时，逐个加载每个模型的部分张量是非常有用的。BLOOM 利用这种格式在 45 秒内在 8 个 GPU 上加载模型，而普通 PyTorch 权重则需要 10 分钟。

# 入门 Safetensors

在本节中，我们将深入了解 `safetensors` API 以及如何保存和加载文件张量文件。

我们可以通过 pip 管理器简单地安装 safetensors：

```py
pip install safetensors
```

我们将使用 [Torch shared tensors](https://huggingface.co/docs/safetensors/torch_shared_tensors) 中的示例来构建一个简单的神经网络，并使用 `safetensors.torch` API 为 PyTorch 保存模型。

```py
from torch import nn

class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.a = nn.Linear(100, 100)
        self.b = self.a

    def forward(self, x):
        return self.b(self.a(x))

model = Model()
print(model.state_dict())
```

如我们所见，我们已成功创建了模型。

```py
OrderedDict([('a.weight', tensor([[-0.0913, 0.0470, -0.0209, ..., -0.0540, -0.0575, -0.0679], [ 0.0268, 0.0765, 0.0952, ..., -0.0616, 0.0146, -0.0343], [ 0.0216, 0.0444, -0.0347, ..., -0.0546, 0.0036, -0.0454], ...,
```

现在，我们将通过提供 `model` 对象和文件名来保存模型。之后，我们将把保存的文件加载到使用 `nn.Module` 创建的 `model` 对象中。

```py
from safetensors.torch import load_model, save_model

save_model(model, "model.safetensors")

load_model(model, "model.safetensors")
print(model.state_dict())
```

```py
OrderedDict([('a.weight', tensor([[-0.0913, 0.0470, -0.0209, ..., -0.0540, -0.0575, -0.0679], [ 0.0268, 0.0765, 0.0952, ..., -0.0616, 0.0146, -0.0343], [ 0.0216, 0.0444, -0.0347, ..., -0.0546, 0.0036, -0.0454], ...,
```

在第二个示例中，我们将尝试保存使用 `torch.zeros` 创建的张量。为此，我们将使用 `save_file` 函数。

```py
import torch
from safetensors.torch import save_file, load_file

tensors = {
   "weight1": torch.zeros((1024, 1024)),
   "weight2": torch.zeros((1024, 1024))
}
save_file(tensors, "new_model.safetensors")
```

为了加载张量，我们将使用 `load_file` 函数。

```py
load_file("new_model.safetensors")
```

```py
{'weight1': tensor([[0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         ...,
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.]]),
 'weight2': tensor([[0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         ...,
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.]])}
```

safetensors API 适用于 Pytorch、Tensorflow、PaddlePaddle、Flax 和 Numpy。您可以通过阅读 [Safetensors](https://huggingface.co/docs/safetensors/main/en/index) 文档来了解它。

![Safetensors 介绍](img/529ed7e796025ef8b83d187331adee90.png)

图像来自 [Torch API](https://huggingface.co/docs/safetensors/main/en/api/torch)

# 结论

简而言之，safetensors 是一种用于深度学习应用的大型张量存储新方式。与其他技术相比，它提供了更快、更高效且用户友好的功能。此外，它确保了数据的机密性和安全性，同时支持多种编程语言和平台。通过利用 Safetensors，机器学习工程师可以优化他们的时间，专注于开发更优质的模型。

我强烈推荐在您的项目中使用 Safetensors。许多顶尖的 AI 公司，如 Hugging Face、EleutherAI 和 StabilityAI，都在他们的项目中利用 Safetensors。

## 参考

+   **文档：** [Safetensors (huggingface.co)](https://huggingface.co/docs/safetensors/index)

+   **博客：** [什么是 Safetensors 以及如何将 .ckpt 模型转换为 .safetensors | hengtao tantai | Medium](https://medium.com/@zergtant/what-is-safetensors-and-how-to-convert-ckpt-model-to-safetensors-13d36eb94d57)

+   **GitHub：** [huggingface/safetensors: 简单、安全的张量存储和分发方式 (github.com)](https://github.com/huggingface/safetensors)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络开发一个 AI 产品，帮助那些面临心理健康问题的学生。

### 更多相关主题

+   [PyCaret 中的二分类简介](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [PyCaret 中的 Python 聚类简介](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [数据科学的基本数学：奇异值分解的可视化介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [数据科学中 Pandas 简介](https://www.kdnuggets.com/2020/06/introduction-pandas-data-science.html)

+   [论文与代码简要介绍](https://www.kdnuggets.com/2022/04/brief-introduction-papers-code.html)

+   [KDnuggets 新闻，4 月 27 日：论文与代码简要介绍；…](https://www.kdnuggets.com/2022/n17.html)
