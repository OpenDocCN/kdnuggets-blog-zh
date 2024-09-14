# 深度学习库简介：PyTorch 和 Lightning AI

> 原文：[https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)

![深度学习库简介：PyTorch 和 Lightning AI](../Images/291f913d791d2fd9fc8513dccfb9ebc0.png)

照片由谷歌 [DeepMind](https://www.pexels.com/photo/an-artist-s-illustration-of-artificial-intelligence-ai-this-image-depicts-the-potential-of-ai-for-society-through-3d-visualisations-it-was-created-by-novoto-studio-as-part-of-the-visua-18069496/)

深度学习是基于 [神经网络](https://en.wikipedia.org/wiki/Neural_network) 的机器学习模型的一部分。在其他机器模型中，数据处理以发现有意义的特征通常是手动完成的或依赖领域专业知识；然而，深度学习可以模拟人脑来发现基本特征，从而提高模型性能。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

深度学习模型有很多应用，包括面部识别、欺诈检测、语音转文本、文本生成等。深度学习已经成为许多先进机器学习应用中的标准方法，我们学习它没有什么损失。

为了开发这个深度学习模型，我们可以依靠各种库框架，而不是从头开始工作。在本文中，我们将讨论两个不同的库：PyTorch 和 Lightning AI。让我们深入了解一下。

# PyTorch

PyTorch 是一个开源库框架，用于训练深度学习神经网络。PyTorch 由 Meta 团队于 2016 年开发，并逐渐获得了人气。流行的增长归功于 PyTorch 结合了来自 [Torch](http://torch.ch/) 的 GPU 后端库和 Python 语言。这种结合使得该软件包对用户易于使用，但在开发深度学习模型时仍然强大。

有一些突出的 [PyTorch 特性](https://pytorch.org/features/)，这些特性由库提供，包括良好的前端、分布式训练以及快速且灵活的实验过程。由于有很多 PyTorch 用户，社区开发和投资也非常庞大。这就是为什么学习 PyTorch 从长远来看是有益的原因。

PyTorch 的构建块是一个 [tensor](https://en.wikipedia.org/wiki/Tensor)，这是一个多维数组，用于编码所有输入、输出和模型参数。你可以将 tensor 想象成类似于 NumPy 数组，但具有在 GPU 上运行的能力。

让我们尝试一下 PyTorch 库。如果你没有访问 GPU 系统的权限，建议在云端执行本教程，例如 Google Colab（虽然在 CPU 上也可以运行）。但如果你想在本地开始，我们需要通过[此页面](https://pytorch.org/get-started/locally/)安装库。选择你拥有的合适系统和规格。

例如，下面的代码用于在具有 CUDA 功能的系统上进行 pip 安装。

```py
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

安装完成后，让我们尝试一些 PyTorch 功能以开发深度学习模型。我们将在本教程中基于他们的网络教程使用 PyTorch 创建一个简单的图像分类模型。我们将逐步讲解代码，并解释代码中的每一步。

首先，我们将使用 PyTorch 下载数据集。在这个示例中，我们将使用 MNIST 数据集，它是手写数字分类数据集。

```py
from torchvision import datasets

train = datasets.MNIST(
    root="image_data",
    train=True,
    download=True
)

test = datasets.MNIST(
    root="image_data",
    train=False,
    download=True,
)
```

我们将 MNIST 训练和测试数据集下载到根文件夹。让我们看看我们的数据集是什么样的。

```py
import matplotlib.pyplot as plt

for i, (img, label) in enumerate(list(train)[:10]):
    plt.subplot(2, 5, i+1)
    plt.imshow(img, cmap="gray")
    plt.title(f'Label: {label}')
    plt.axis('off')

plt.show()
```

![介绍深度学习库：PyTorch 和 Lightning AI](../Images/5beec8ab85fb6221d4f0f8c808e9fe79.png)

每个图像是一个介于零到九之间的单数字，意味着我们有十个标签。接下来，让我们基于这个数据集开发一个图像分类器。

我们需要将图像数据集转换为 tensor，以便使用 PyTorch 开发深度学习模型。由于我们的图像是 PIL 对象，我们可以使用 PyTorch 的 ToTensor 函数来执行转换。此外，我们还可以通过 datasets 函数自动转换图像。

```py
from torchvision.transforms import ToTensor
train = datasets.MNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor()
)

test = datasets.MNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor()
)
```

通过将变换函数传递给 transform 参数，我们可以控制数据的样子。接下来，我们将数据包装成 DataLoader 对象，以便 PyTorch 模型可以访问我们的图像数据。

```py
from torch.utils.data import DataLoader
size = 64

train_dl = DataLoader(train, batch_size=size)
test_dl = DataLoader(test, batch_size=size)

for X, y in test_dl:
    print(f"Shape of X [N, C, H, W]: {X.shape}")
    print(f"Shape of y: {y.shape} {y.dtype}")
    break
```

```py
Shape of X [N, C, H, W]: torch.Size([64, 1, 28, 28])
Shape of y: torch.Size([64]) torch.int64 
```

在上述代码中，我们为训练和测试数据创建了一个 DataLoader 对象。每次数据批次迭代将返回 64 个特征和标签。除此之外，我们的图像的形状是 28 * 28（高度 * 宽度）。

接下来，我们将开发神经网络模型对象。

```py
from torch import nn

#Change to 'cuda' if you have access to GPU
device = 'cpu'

class NNModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.lr_stack = nn.Sequential(
            nn.Linear(28*28, 128),
            nn.ReLU(),
            nn.Linear(128, 128),
            nn.ReLU(),
            nn.Linear(128, 10)
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.lr_stack(x)
        return logits

model = NNModel().to(device)
print(model)
```

```py
NNModel(
  (flatten): Flatten(start_dim=1, end_dim=-1)
  (lr_stack): Sequential(
    (0): Linear(in_features=784, out_features=128, bias=True)
    (1): ReLU()
    (2): Linear(in_features=128, out_features=128, bias=True)
    (3): ReLU()
    (4): Linear(in_features=128, out_features=10, bias=True)
  )
) 
```

在上述对象中，我们创建了一个具有少量层结构的神经模型。为了开发神经模型对象，我们使用 nn.module 函数的子类方法，并在__init__中创建神经网络层。

我们首先使用 flatten 函数将 2D 图像数据转换为层内的像素值。然后，我们使用 sequential 函数将我们的层包裹成一系列层。在 sequential 函数内部，我们有我们的模型层：

```py
nn.Linear(28*28, 128),
nn.ReLU(),
nn.Linear(128, 128),
nn.ReLU(),
nn.Linear(128, 10)
```

上述过程按顺序发生的情况是：

1.  首先，数据输入（28*28 特征）通过线性层中的线性函数转换，并输出 128 个特征。

1.  ReLU是一个非线性激活函数，位于模型的输入和输出之间，以引入非线性。

1.  128个特征输入到线性层，输出128个特征。

1.  另一个ReLU激活函数

1.  线性层的输入为128个特征，输出为10个特征（我们的数据集标签只有10个标签）。

最后，前向函数用于模型的实际输入过程。接下来，模型还需要一个损失函数和优化函数。

```py
from torch.optim import SGD

loss_fn = nn.CrossEntropyLoss()
optimizer = SGD(model.parameters(), lr=1e-3)
```

对于下一个代码，我们只是准备训练和测试的准备工作，然后再进行建模活动。

```py
import torch
def train(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    model.train()
    for batch, (X, y) in enumerate(dataloader):
        X, y = X.to(device), y.to(device)
        pred = model(X)
        loss = loss_fn(pred, y)

        loss.backward()
        optimizer.step()
        optimizer.zero_grad()

        if batch % 100 == 0:
            loss, current = loss.item(), (batch + 1) * len(X)
            print(f"loss: {loss:>2f}  [{current:>5d}/{size:>5d}]")

def test(dataloader, model, loss_fn):
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    model.eval()
    test_loss, correct = 0, 0
    with torch.no_grad():
        for X, y in dataloader:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>2f} \n")
```

现在我们准备好运行模型训练了。我们需要决定模型要运行多少个epoch（迭代次数）。在这个示例中，我们设定为运行五次。

```py
epoch = 5
for i in range(epoch):
    print(f"Epoch {i+1}\n-------------------------------")
    train(train_dl, model, loss_fn, optimizer)
    test(test_dl, model, loss_fn)
print("Done!")
```

![深度学习库简介：PyTorch和Lighting AI](../Images/a2376135f7b6ed629ed26dea0ac08e4f.png)

现在模型已经完成训练，可以用于任何图像预测活动。结果可能会有所不同，因此请期待与上述图片不同的结果。

这只是PyTorch可以做的一些事情，但你可以看到，使用PyTorch构建模型很简单。如果你对预训练模型感兴趣，PyTorch有一个你可以访问的[hub](https://pytorch.org/hub/)。

# Lighting AI

[Lighting AI](https://lightning.ai/)是一家提供各种产品的公司，旨在缩短训练PyTorch深度学习模型的时间并简化过程。他们的一个开源产品是[PyTorch Lighting](https://lightning.ai/pytorch-lightning)，这是一个提供训练和部署PyTorch模型框架的库。

Lighting提供了一些功能，包括代码灵活性、无需样板代码、最小化API以及改进的团队协作。Lighting还提供了多GPU利用和快速、低精度训练等功能。这使得Lighting成为开发我们PyTorch模型的一个很好的替代选择。

让我们试试用Lighting进行模型开发。首先，我们需要安装这个包。

```py
pip install lightning
```

安装了Lighting后，我们还将安装另一个Lighting AI产品，名为[TorchMetrics](https://lightning.ai/torchmetrics)，以简化指标选择。

```py
pip install torchmetrics
```

所有库都安装完成后，我们将尝试使用Lighting封装器开发与之前示例相同的模型。下面是开发模型的完整代码。

```py
import torch
import torchmetrics
import pytorch_lightning as pl
from torch import nn
from torch.optim import SGD

# Change to 'cuda' if you have access to GPU
device = 'cpu'

class NNModel(pl.LightningModule):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.lr_stack = nn.Sequential(
            nn.Linear(28 * 28, 128),
            nn.ReLU(),
            nn.Linear(128, 128),
            nn.ReLU(),
            nn.Linear(128, 10)
        )
        self.train_acc = torchmetrics.Accuracy(task="multiclass", num_classes=10)
        self.valid_acc = torchmetrics.Accuracy(task="multiclass", num_classes=10)

    def forward(self, x):
        x = self.flatten(x)
        logits = self.lr_stack(x)
        return logits

    def training_step(self, batch, batch_idx):
        x, y = batch
        x, y = x.to(device), y.to(device)
        pred = self(x)
        loss = nn.CrossEntropyLoss()(pred, y)
        self.log('train_loss', loss)

        # Compute training accuracy
        acc = self.train_acc(pred.softmax(dim=-1), y)
        self.log('train_acc', acc, on_step=True, on_epoch=True, prog_bar=True)
        return loss

    def configure_optimizers(self):
        return SGD(self.parameters(), lr=1e-3)

    def test_step(self, batch, batch_idx):
        x, y = batch
        x, y = x.to(device), y.to(device)
        pred = self(x)
        loss = nn.CrossEntropyLoss()(pred, y)
        self.log('test_loss', loss)

        # Compute test accuracy
        acc = self.valid_acc(pred.softmax(dim=-1), y)
        self.log('test_acc', acc, on_step=True, on_epoch=True, prog_bar=True)
        return loss
```

让我们分析一下上面的代码。与我们之前开发的PyTorch模型的不同之处在于，NNModel类现在使用了LightingModule的子类。此外，我们使用TorchMetrics分配了准确性指标进行评估。然后，我们在类中添加了训练和测试步骤，并设置了优化函数。

在所有模型设置完成后，我们将使用转换后的DataLoader对象运行模型训练。

```py
# Create a PyTorch Lightning trainer
trainer = pl.Trainer(max_epochs=5)

# Create the model
model = NNModel()

# Fit the model
trainer.fit(model, train_dl)

# Test the model
trainer.test(model, test_dl)

print("Training Finish")
```

![深度学习库简介：PyTorch和Lighting AI](../Images/87749cd3f95ff12d202df318f44ab167.png)

使用Lighting库，我们可以轻松调整所需的结构。有关进一步阅读，你可以查看他们的[文档](https://lightning.ai/docs/pytorch/latest/)。

# 结论

PyTorch是一个用于开发深度学习模型的库，它为我们提供了一个简单的框架，以便访问许多高级API。Lightning AI 也支持这个库，提供了一个简化模型开发并增强开发灵活性的框架。本文介绍了这个库的功能以及简单的代码实现。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**是一名数据科学助理经理和数据撰稿人。在全职工作于Allianz Indonesia期间，他喜欢通过社交媒体和写作媒体分享Python和数据技巧。

### 更多相关话题

+   [入门PyTorch Lightning](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [免费使用Lightning AI Studio](https://www.kdnuggets.com/using-lightning-ai-studio-for-free)

+   [Python数据清洗库简介](https://www.kdnuggets.com/2023/03/introduction-python-libraries-data-cleaning.html)

+   [完全免费的PyTorch深度学习课程](https://www.kdnuggets.com/2022/10/complete-free-pytorch-course-deep-learning.html)

+   [使用PyTorch进行迁移学习的实用指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [PyTorch还是TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)
