# PyTorch初学者指南

> 原文：[https://www.kdnuggets.com/a-beginners-guide-to-pytorch](https://www.kdnuggets.com/a-beginners-guide-to-pytorch)

![PyTorch初学者指南](../Images/de78123b8b7e42a4ca8e1e0143a58f88.png)

图片来源：编辑 | Midjourney & Canva

深度学习在人工智能研究的许多领域被广泛使用，并且促进了技术进步。例如，文本生成、面部识别和语音合成应用都基于深度学习研究。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

最常用的深度学习包之一是[PyTorch](https://pytorch.org/)。它是由Meta AI于2016年创建的开源包，并且被许多人使用。

PyTorch有很多优点，包括：

+   灵活的模型架构

+   对CUDA的原生支持（可以使用GPU）

+   基于Python

+   提供低级别的控制，这对于研究和许多用例非常有用

+   开发者和社区的积极开发

让我们通过这篇文章探索PyTorch，帮助你入门。

## 准备工作

你应该访问他们的安装网页，选择适合你环境要求的版本。下面的代码是安装示例。

```py
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

PyTorch准备好了，让我们进入核心部分。

## PyTorch张量

张量是PyTorch中的基本构件。它类似于NumPy数组，但可以使用GPU。我们可以使用以下代码尝试创建一个PyTorch张量：

```py
a = torch.tensor([2, 4, 5])
print(a)
```

```py
Output>> 
tensor([2, 4, 5])
```

像NumPy数组张量一样，它允许矩阵运算。

```py
e = torch.tensor([[1, 2, 3],
                [4, 5, 6]])
f = torch.tensor([7, 8, 9])

print(e * f)
```

```py
Output>>
tensor([[ 7, 16, 27],
        [28, 40, 54]])
```

也可以执行矩阵乘法。

```py
g = torch.randn(2, 3)
h = torch.randn(3, 2)
print( g @ h)
```

```py
Output>> 
tensor([[-0.8357,  0.0583],
        [-2.7121,  2.1980]])
```

我们可以使用下面的代码访问张量信息。

```py
x = torch.rand(3,4)

print("Shape:", x.shape)
print("Data type:", x.dtype)
print("Device:", x.device)
```

```py
Output>>
Shape: torch.Size([3, 4])
Data type: torch.float32
Device: cpu
```

## PyTorch的神经网络训练

通过使用nn.Module类定义神经网络，我们可以开发一个简单的模型。让我们试试下面的代码。

```py
import torch

class SimpleNet(nn.Module):
    def __init__(self, input, hidden, output):
        super(SimpleNet, self).__init__()
        self.fc1 = torch.nn.Linear(input, hidden)
        self.fc2 = torch.nn.Linear(hidden, output)

    def forward(self, x):
        x = torch.nn.functional.relu(self.fc1(x))
        x = self.fc2(x)
        return x

inp = 10
hid = 10
outp = 2
model = SimpleNet(inp, hid, out)

print(model)
```

```py
Output>>
SimpleNet(
  (fc1): Linear(in_features=10, out_features=10, bias=True)
  (fc2): Linear(in_features=10, out_features=2, bias=True)
)
```

上面的代码定义了一个`SimpleNet`类，继承自`nn.Module`，用于设置层。我们使用`nn.Linear`作为层，`relu`作为激活函数。

我们可以添加更多层或使用不同的层，如Conv2D或CNN。但我们不会使用这些。

接下来，我们将用样本张量数据训练我们开发的`SimpleNet`。

```py
import torch

inp = torch.randn(100, 10) 
tar = torch.randint(0, 2, (100,)) 
criterion = torch.nn.CrossEntropyLoss()
optimizr = torch.optim.SGD(model.parameters(), lr=0.01)

epochs = 100
batchsize = 10

for epoch in range(numepochs):
    model.train()

    for i in range(0, inp.size(0), batchsize):
        batch_inp = inputs[i:i+batch_size]
        batch_tar = targets[i:i+batch_size]

        out = model(batch_inp)
        loss = criterion(out, batch_tar)

        optimizer.zero_grad()
        loss.backward()
        optimizr.step()

    if (epoch + 1) % 10 == 0:
        print(f'Epoch [{epoch+1}/{num_epochs}], Loss: {round(loss.item(),4})')
```

在上述训练过程中，我们使用随机张量数据，并初始化一个称为`CrossEntropyLoss`的损失函数。同时，我们初始化SGD优化器来管理模型参数以最小化损失。

训练过程根据周期次数多次运行，然后进行优化过程。这是通常的深度学习流程。

我们可以添加几个步骤以提高训练的复杂性，例如早停、学习率和其他技术。

最后，我们可以使用未见过的数据评估我们训练的模型。以下代码允许我们做到这一点。

```py
from sklearn.metrics import classification_report

model.eval()
test_inputs = torch.randn(20, 10)
test_targets = torch.randint(0, 2, (20,))

with torch.no_grad():
    test_outputs = model(test_inputs)
    _, predicted = torch.max(test_outputs, 1)

print(classification_report(test_targets, predicted))
```

上述情况发生的原因是我们将模型切换到评估模式，这会关闭 dropout 和批量归一化更新。此外，我们还禁用梯度计算过程以加快处理速度。

你可以访问 [PyTorch 文档](https://pytorch.org/docs/stable/index.html) 以进一步了解你可以做什么。

## 结论

在本文中，我们将深入探讨 PyTorch 的基础知识。从张量创建到张量操作，再到开发一个简单的神经网络模型。本文为初学者级别，所有新手应能迅速跟上。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一位数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉及多种 AI 和机器学习主题的写作。

### 更多相关主题

+   [使用 PyTorch 的实践指南](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

+   [初学者的端到端机器学习指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [基本机器学习算法：初学者指南](https://www.kdnuggets.com/2021/05/essential-machine-learning-algorithms-beginners.html)

+   [初学者的 Q 学习指南](https://www.kdnuggets.com/2022/06/beginner-guide-q-learning.html)

+   [初学者的 Python 网络抓取指南](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)

+   [初学者的云计算指南](https://www.kdnuggets.com/2023/01/beginner-guide-cloud-computing.html)
