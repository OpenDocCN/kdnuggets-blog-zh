# 在 Google Colab 中使用 PyTorch 构建神经网络

> 原文：[https://www.kdnuggets.com/2020/10/building-neural-networks-pytorch-google-colab.html](https://www.kdnuggets.com/2020/10/building-neural-networks-pytorch-google-colab.html)

[评论](#comments)

### 在 Google Colab 中使用 PyTorch 进行深度学习

[PyTorch](https://blog.exxactcorp.com/the-most-important-fundamentals-of-pytorch-you-should-know/) 和 Google Colab 已成为[深度学习](https://blog.exxactcorp.com/category/deep-learning/)的代名词，因为它们为人们提供了一种简便且经济实惠的方式，迅速开始构建自己的神经网络和训练模型。GPU 并不便宜，这使得构建自己的定制工作站对许多人来说是一个挑战。虽然深度学习工作站的成本可能是许多人的障碍，但由于[NVIDIA 新款 RTX 30 系列](https://www.exxactcorp.com/Deep-Learning-NVIDIA-GPU-Workstations)的价格下降，这些系统最近变得更为实惠。

即使拥有更实惠的深度学习系统选项，许多人仍然趋向于使用 PyTorch 和 Google Colab，因为他们逐渐适应了深度学习项目的工作。

![PyTorch 和 Google Colab 标志](../Images/50078f8211f21460eba3628c9ee9bde6.png)

[来源](https://medium.com/@nrezaeis/pytorch-in-google-colab-640e5d166f13)

### **PyTorch 和 Google Colab 在开发神经网络方面非常强大**

PyTorch 是由 Facebook 开发的，并在深度学习研究社区中声名显赫。它允许并行处理，并且具有易于阅读的语法，这导致了其采纳率的上升。与 TensorFlow 相比，PyTorch 通常更易于学习且更轻便，适合快速项目和快速原型的构建。许多人将 PyTorch 用于计算机视觉和自然语言处理（NLP）应用。

Google Colab 是由 Google 开发的，旨在帮助大众访问强大的 GPU 资源以运行深度学习实验。它提供 GPU 和 TPU 支持，并与 Google Drive 集成用于存储。这些原因使其成为构建简单神经网络的绝佳选择，特别是与类似于[随机森林](https://blog.exxactcorp.com/3-reasons-to-use-random-forest-over-a-neural-network-comparing-machine-learning-versus-deep-learning/)的选项相比。

### **使用 Google Colab**

![与 Google Colab 的关系](../Images/b7f138ecd96f19da537f64d45ea13155.png)

[来源](https://zerowithdot.com/colab-github-workflow/)

Google Colab 提供了一个环境设置选项的组合，具有类似 Jupyter 的界面、GPU 支持（包括免费和付费选项）、存储以及代码文档记录功能，全部集成在一个应用程序中。数据科学家可以在不花费大量资金购买 GPU 支持的情况下，享受全方位的深度学习体验。

记录代码对于在不同人之间共享代码很重要，并且有一个中立的地方来存储数据科学项目也很重要。结合了 GPU 实例的[Jupyter notebook](https://blog.exxactcorp.com/the-4-best-jupyter-notebook-environments-for-deep-learning/)界面提供了一个良好的可重复环境。你也可以从 GitHub 导入笔记本或上传你自己的。

*重要说明：由于 Python 2 已经过时，因此在 Colab 上不再提供。然而，仍有遗留代码在运行 Python 2。幸运的是，Colab 提供了一个[修复方案](https://colab.research.google.com/drive/1fNru_ht5fNmtf7W4TkB2cFbxW3fMxS6f)，你可以使用它继续运行 Python 2。如果你尝试一下，你会看到 Python 2 在 Google Colab 中已正式弃用的警告。*

### **使用 PyTorch**

![PyTorch 标志](../Images/c029a48084032c05aa34ef79f416f05d.png)

[来源](https://engineering.fb.com/ai-research/facebook-accelerates-ai-development-with-new-partners-and-production-capabilities-for-pytorch-1-0/)

PyTorch 在功能上类似于其他深度学习库，提供了一整套构建深度学习模型的模块。不同之处在于 PyTorch 的 Tensor 类，它类似于 Numpy 的*ndarray*。

Tensor 的一个主要优点是它本身支持 GPU。Tensor 可以在 CPU 或 GPU 上运行。[要在 GPU 上运行，我们只需使用 PyTorch 内置的 CUDA 模块将环境切换为 GPU。](https://pytorch.org/docs/master/cuda.html)这使得在 GPU 和 CPU 之间切换变得容易。

提供给神经网络的数据必须是数值格式的。使用 PyTorch，我们通过将数据表示为 Tensor 来实现。Tensor 是一种数据结构，可以存储*N*维数据；向量是 1 维 Tensor，矩阵是 2 维 Tensor。**通俗来说，Tensor 可以存储比向量或矩阵更高维的数据。**

### **为什么 GPU 更受青睐？**

**![PyTorch 技术编译](../Images/929382124362cfa66fea0cb5796bccf0.png)**

[来源](https://hackernoon.com/how-to-run-pytorch-with-gpu-and-cuda-9-2-support-on-google-colab-64d58ba3083a)

Tensor 处理库可以用于计算大量计算，但在使用单核 GPU 时，计算编译需要很长时间。

这就是 Google Colab 的作用。它在技术上是免费的，但可能不适合大规模工业深度学习。它更适合初学者到中级从业者。它确实提供了一个付费服务，用于更大规模的项目，比如在免费版本中连接时间最长为 24 小时，而不是 12 小时，并且可以根据需要提供直接访问更强大的资源。

### 如何编写基本的神经网络

要开始构建一个基本的神经网络，我们需要在 Google Colab 环境中安装 PyTorch。这可以通过运行以下 pip 命令以及使用下面的其余代码来完成：

`!pip3 install torch torchvision`

```py
# Import libraries
import torch
import torchvision
from torchvision import transforms, datasets
Import torch.nn as nn
Import torch.nn.functional as F
import torch.optim as optim

# Create test and training sets
train = datasets.MNIST('', train=True, download=True,
                       transform=transforms.Compose([
                           transforms.ToTensor()
                       ]))

test = datasets.MNIST('', train=False, download=True,
                       transform=transforms.Compose([
                           transforms.ToTensor()
                       ]))

# This section will shuffle our input/training data so that we have a randomized shuffle of our data and do not risk feeding data with a pattern. Anorther objective here is to send the data in batches. This is a good step to practice in order to make sure the neural network does not overfit our data. NN’s are too prone to overfitting just because of the exorbitant amount of data that is required. For each batch size, the neural network will run a back propagation for new updated weights to try and decrease loss each time.
trainset = torch.utils.data.DataLoader(train, batch_size=10, shuffle=True)
testset = torch.utils.data.DataLoader(test, batch_size=10, shuffle=False)

# Initialize our neural net
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(28*28, 64)
        self.fc2 = nn.Linear(64, 64)
        self.fc3 = nn.Linear(64, 64)
        self.fc4 = nn.Linear(64, 10)

    def forward(self, x):
        x = self.fc1(x)
        x = self.fc2(x)
        x = self.fc3(x)
        x = self.fc4(x)
        return F.log_softmax(x, dim=1)

net = Net()

print(net)

### Output:
### Net(
###  (fc1): Linear(in_features=784, out_features=64, bias=True)
###  (fc2): Linear(in_features=64, out_features=64, bias=True)
###  (fc3): Linear(in_features=64, out_features=64, bias=True)
###  (fc4): Linear(in_features=64, out_features=10, bias=True)
###)

# Calculate our loss 
loss_function = nn.CrossEntropyLoss()
optimizer = optim.Adam(net.parameters(), lr=0.001)

for epoch in range(5): # we use 5 epochs
    for data in trainset:  # `data` is a batch of data
        X, y = data  # X is the batch of features, y is the batch of targets.

        net.zero_grad()  # sets gradients to 0 before calculating loss.

        output = net(X.view(-1,784))  # pass in the reshaped batch (recall they are 28x28 atm, -1 is needed to show that output can be n-dimensions. This is PyTorch exclusive syntax)

        loss = F.nll_loss(output, y)  # calc and grab the loss value

        loss.backward()  # apply this loss backwards thru the network's parameters

        optimizer.step()  # attempt to optimize weights to account for loss/gradients
    print(loss)  

### Output:
### tensor(0.6039, grad_fn=)
### tensor(0.1082, grad_fn=<nlllossbackward>)
### tensor(0.0194, grad_fn=<nlllossbackward>)
### tensor(0.4282, grad_fn=<nlllossbackward>)
### tensor(0.0063, grad_fn=<nlllossbackward>)

# Get the Accuracy
correct = 0
total = 0

with torch.no_grad():
    for data in testset:
        X, y = data
        output = net(X.view(-1,784))
        #print(output)
        for idx, i in enumerate(output):
            #print(torch.argmax(i), y[idx])
            if torch.argmax(i) == y[idx]:
                correct += 1
            total += 1

print("Accuracy: ", round(correct/total, 3))

### Output: 
### Accuracy:  0.915</nlllossbackward></nlllossbackward></nlllossbackward></nlllossbackward>
```

### **PyTorch和Google Colab在数据科学中是绝佳选择**

PyTorch和Google Colab都是有用、强大且简单的选择，尽管PyTorch于2017年（3年前）发布，而Google Colab于2018年（2年前）发布，但它们在数据科学社区中已被广泛采纳。

这些环境在深度学习中已被证明是绝佳的选择，并且随着新开发的发布，它们可能成为最好的工具。两者都由科技界两大巨头Facebook和Google支持。PyTorch提供了全面的工具和模块，以尽可能简化深度学习过程，而Google Colab则提供了一个环境来管理你的编码和项目的可重现性。

如果你已经在使用这些工具，你发现哪些对你的工作最有价值？

[原文](https://blog.exxactcorp.com/building-neural-networks-with-pytorch-in-google-colab/)。经许可转载。

**相关内容：**

+   [深度学习的4个最佳Jupyter Notebook环境](/2020/03/4-best-jupyter-notebook-environments-deep-learning.html)

+   [5个Google Colaboratory技巧](/2020/03/5-google-colaboratory-tips.html)

+   [Google Colab在深度学习中的完整指南](/2020/06/google-colab-deep-learning.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并以目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [90亿美元的AI失败，深度剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
