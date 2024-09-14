# 自动微分：你没在用的最佳机器学习库？

> 原文：[https://www.kdnuggets.com/2020/09/autograd-best-machine-learning-library-not-using.html](https://www.kdnuggets.com/2020/09/autograd-best-machine-learning-library-not-using.html)

[评论](#comments)

### **自动微分：你错过的机器学习库**

### 等等，人们真的使用 TensorFlow 和 PyTorch 以外的库吗？

询问一组深度学习从业者他们选择的编程语言，你肯定会听到很多关于 Python 的讨论。另一方面，问到他们常用的机器学习库，你可能会得到一个混合了 TensorFlow 和 PyTorch 的双库系统的画面。虽然有很多人可能对这两者都很熟悉，但在机器学习（ML）的商业应用中，TensorFlow 通常占据主导地位，而人工智能/ML 研究项目则[主要使用 PyTorch](https://thegradient.pub/state-of-ml-frameworks-2019-pytorch-dominates-research-tensorflow-dominates-industry/)。尽管在[TensorFlow 2.0](https://blog.exxactcorp.com/tensorflow-2-0-dynamic-readable-and-highly-extended/)中默认引入了急切执行，以及通过[Torchscript](https://pytorch.org/docs/master/jit.html)构建静态可执行模型的功能，两者之间仍有显著的融合，但大多数人似乎仍主要坚持使用其中之一。

虽然普遍共识似乎是，如果你想进入公司，应该选择 TensorFlow，因为它在部署和边缘支持方面更好，而如果你想从事学术研究，则选择 PyTorch，因为它在灵活性和可读性方面更强，但 AI/ML 库的世界远不止 PyTorch 和 TensorFlow。就像 AI/ML 不仅仅是深度学习一样。事实上，驱动深度学习的梯度和张量计算承诺在从物理学到生物学的领域产生广泛的影响。虽然我们敢打赌，所谓的 ML/AI 研究人员短缺是夸大的（谁愿意把自己最具创造力的岁月投入到[maximizing ad engagement](https://www.fastcompany.com/3008436/why-data-god-jeffrey-hammerbacher-left-facebook-found-cloudera) 和推荐更多令人上瘾的新闻推送中？），我们预计可微分编程的工具在可预见的未来将对各种专业人士越来越有价值。

### 可微计算比深度学习更为广泛

深度学习，即使用基于哺乳动物大脑计算思想的多层人工神经网络，以其对计算机视觉和自然语言处理等领域的影响而闻名。我们还看到，在过去十年中，与深度学习一起发展的硬件和软件的许多教训（梯度下降、函数逼近和加速张量计算）在没有神经网络的情况下找到了有趣的应用。

自动微分和 [量子电路参数上的梯度下降](https://pennylane.ai/qml/demos/tutorial_qubit_rotation.html) 为噪声中等规模量子（NISQ）计算设备时代的量子计算提供了有意义的实用性（*即* 现在可用的量子计算设备）。在 [DeepMind在CASP13](https://blog.exxactcorp.com/deepminds-protein-folding-upset/) 蛋白质折叠预测大会和竞赛中的令人印象深刻的逆袭的倒数第二步使用了直接应用于预测氨基酸位置的梯度下降，而不是Google Alphabet子公司所知名的深度神经网络。这些只是可微编程不受人工神经元范式限制的力量的几个例子。

![深度学习可以被归类为更广泛的可微编程的一个子空间](../Images/ea7f9a42f05755997fec3fe8344e6faa.png)

*深度学习可以被归类为更广泛的可微编程的一个子空间。深度神经进化指的是通过选择来优化神经网络，而不需要显式的微分或梯度下降。*

可微编程是一个更广泛的编程范式，涵盖了大部分深度学习，除了如神经进化/进化算法等不使用梯度优化的方法。Facebook首席AI科学家Yann LeCun在一个 [Facebook帖子](https://www.facebook.com/yann.lecun/posts/10155003011462143?_fb_noscript=1) 中吹捧了可微编程的可能性（内容 [在Github gist中镜像](https://gist.github.com/halhenke/872708ccea42ee8cafd950c6c2069814)）。据LeCun所述，可微编程不过是现代深度学习的重新品牌，结合了具有循环和条件的神经网络的动态定义。

我认为，可微分编程的广泛应用的后果更接近于Andrej Karpathy所描述的[“软件 2.0”](https://medium.com/@karpathy/software-2-0-a64152b37c35)，尽管他也主要将讨论限制在神经网络上。合理地说，软件 2.0/可微分编程作为一个整体比LeCun或Karpathy所描述的更广泛。可微分编程代表了一种超越神经网络作为函数近似器的约束，以便为各种系统提供基于梯度的优化算法的概括。如果有一个Python库能够体现可微分编程的简单性、灵活性和实用性，那一定是Autograd。

### 结合深度学习与可微分编程

关于任意物理模拟和数学原语的微分为解决深度神经网络低效或无效的问题提供了机会。这并不是说你应该抛弃所有深度学习的直觉和经验。相反，最令人印象深刻的解决方案将结合深度学习元素与可微分编程的更广泛能力，例如[Degrave et al. 2018](https://arxiv.org/abs/1611.01652)的工作，其作者将一个可微分物理引擎与神经网络控制器结合在一起，解决了机器人控制任务。

本质上，他们将环境中可微分的部分扩展到了神经网络之外，包含了模拟的机器人运动学。然后，他们可以通过机器人环境的参数反向传播到神经网络策略中，从而在样本效率方面将优化过程加速约6到8倍。他们选择使用[Theano](http://deeplearning.net/software/theano/)作为自动微分库，这限制了他们通过条件语句进行微分的能力，从而限制了他们可以实现的接触约束类型。使用Autograd构建的可微分物理模拟器，甚至是支持动态分支微分的最新版本的PyTorch或TensorFlow 2.0，将提供更多优化神经网络机器人控制器的可能性，例如提供更现实的碰撞检测。

深度神经网络的[通用逼近能力](https://en.wikipedia.org/wiki/Universal_approximation_theorem)使它们成为解决科学、控制和数据科学问题的绝佳工具，但有时这种灵活性更像是负担而非实用，任何曾经挣扎于过拟合的人都可以证明这一点。正如约翰·冯·诺依曼的名言所说：“用四个参数我可以拟合一头大象，用五个参数我可以让它摆动鼻子。”（这个概念的实际演示可以在“Mayer *et al.* 的‘用 4 个复杂参数画大象’”中找到[[pdf](https://publications.mpi-cbg.de/getDocument.html?id=ff8080812daff75c012dc1b7bc10000c)]。） 

在现代机器学习实践中，这意味着要小心不要将你的模型与数据集不匹配，对于小数据集，这种情况很容易出现。换句话说，对于仅有几百到几千个样本的定制数据集，大型卷积网络很可能是过度的。例如，在许多物理问题中，最好将你的问题数学化，并对自由参数进行梯度下降。Autograd 是一个非常适合这种方法的 Python 包，特别是对于倾向于 Pythonic 的数学家、物理学家以及那些擅长用 Python 矩阵和数组计算包 NumPy 描述问题的其他人。

### Autograd：你能用 NumPy 做的任何事，都可以用它进行微分

这是一个简单的 Autograd 示例：

```py
import autograd.numpy as np
from autograd import elementwise_grad as egrad

import matplotlib.pyplot as plt

x = np.linspace(-31.4,31.4, 256)

sinc = lambda x: np.sin(x) / x

plt.figure(figsize=(12,7))

plt.title(“sinc function and derivatives”, fontsize=24)

my_fn = sinc

for ii in range(9):

    plt.plot(x, my_fn(x), lw=3, label=”d{} sinc(x)/dx{}”.format(ii,ii))

    plt.legend(fontsize=18)

    plt.axis([-32, 32, -0.50, 1.2])

    plt.savefig(“./sinc_grad{}.png”.format(ii))

    my_fn = egrad(my_fn) 
```

![使用 Autograd 进行微分](../Images/e2c8eff7d2033e84302f17d109e6c79c.png)

*使用 Autograd 进行微分。在这种情况下，Autograd 能够微分到第七阶，但在 x=0 附近遇到了一些数值稳定性问题（注意图中央的尖锐橄榄绿色峰值）。*

Autograd 是一个强大的自动微分库，使得可以对原生 Python 和 NumPy 代码进行微分。可以计算任意阶的导数（你可以计算导数的导数的导数，依此类推），并将其分配给多个参数数组，只要最终输出是一个标量（例如损失函数）。生成的代码是[Pythonic](https://stackoverflow.com/questions/25011078/what-does-pythonic-mean)，即它是可读和可维护的，并且不需要学习新的语法或风格。这意味着我们不必担心记住复杂的 API，如 torch.nn 或 tf.keras.layers 的内容，我们可以专注于问题的细节，*例如* 将数学转化为代码。Autograd+NumPy 是一个成熟的库，虽然维护但不再开发，因此没有真正的风险会因未来的更新破坏你的项目。

你 *可以* 使用 Autograd 轻松实现神经网络，因为密集神经层（矩阵乘法）和卷积的数学基本操作（你可以轻松地使用傅里叶变换，或者使用 `convolve2d` 从 scipy）在 NumPy 中有相对快速的实现。要尝试在 scikit-learn 的微型数字数据集上进行简单 MLP 演示，请下载这个 [Github gist](https://gist.github.com/riveSunder/1223824a4fb7e6831f20fde3b4871354)，（你也可能对研究 [官方示例](https://github.com/HIPS/autograd/blob/master/examples/neural_net.py) 感兴趣，它在 autograd 仓库中）。

如果你复制了这个 gist 并在本地虚拟环境中运行，你需要用 `pip install` 安装 `autograd` 和 `scikit-learn`，后者用于其数字数据集。设置完成后，运行代码应该会得到如下进度报告：

```py
epoch 10, training loss 2.89e+02, train acc: 5.64e-01, val loss 3.94e+02, val accuracy 4.75e-01
total time: 4.26, epoch time 0.38

epoch 20, training loss 8.79e+01, train acc: 8.09e-01, val loss 9.96e+01, val accuracy 7.99e-01

total time: 7.73, epoch time 0.33

epoch 30, training loss 4.54e+01, train acc: 9.20e-01, val loss 4.55e+01, val accuracy 9.39e-01

total time: 11.49, epoch time 0.35

…

epoch 280, training loss 1.77e+01, train acc: 9.99e-01, val loss 1.39e+01, val accuracy 9.83e-01

total time: 110.70, epoch time 0.49

epoch 290, training loss 1.76e+01, train acc: 9.99e-01, val loss 1.39e+01, val accuracy 9.83e-01

total time: 115.41, epoch time 0.43
```

这是一个相当不错的结果，在不到两分钟的训练后达到了 98.3% 的验证准确率。通过稍微调整超参数，你可能会将性能提升到 100% 准确率或非常接近。Autograd 处理这个小数据集非常轻松和高效（虽然 Autograd 和 NumPy 操作不会在 GPU 上运行，但像矩阵乘法这样的基本操作确实可以利用多个核心）。但如果你只是想构建一个浅层的 MLP，那么用更主流和现代的机器学习库可以在开发和计算时间上更快地完成。

在这种低级别构建简单模型的确有一些实用性，无论是为了优先控制还是作为学习练习，但如果小型密集神经网络是最终目标，我们建议你使用 PyTorch 或 TensorFlow，以便简洁并兼容如 GPU 这样的硬件加速器。相反，让我们深入一些更有趣的内容：模拟光学神经网络。以下教程确实涉及一些物理知识和相当多的代码：如果这不是你的兴趣，可以跳过到下一部分，我们将讨论一些 Autograd 的局限性。

### 使用 Autograd 模拟光学神经网络

光学神经网络（ONNs）是一个古老的概念，科学期刊《应用光学》曾在 [1987年](https://www.osapublishing.org/ao/issue.cfm?volume=26&issue=23) 和 [1993年](https://www.osapublishing.org/ao/issue.cfm?volume=32&issue=8) 上刊登过相关专题。近期，学术界（*例如*[ Zuo *et al*. 2019](https://www.osapublishing.org/optica/abstract.cfm?uri=optica-6-9-1132)）和一些初创公司如 [Optalysys](https://www.optalysys.com/)、  [Fathom Computing](https://www.wired.com/story/this-computer-uses-lightnot-electricityto-train-ai-algorithms/) 和 [Lightmatter](https://lightmatter.co/) 以及 [Lightelligence](https://www.lightelligence.ai/technology) 也重新审视了这一概念，其中最后两个公司是由同一MIT实验室的合著者在 [《自然》](https://www.nature.com/articles/nphoton.2017.93)上发表的高影响力论文中衍生出来的。

光是实现神经网络的一个有吸引力的物理现象，因为描述神经网络和光学传播所用的数学有相似之处。由于[透镜的傅里叶变换性质](https://en.wikipedia.org/wiki/Fourier_optics#Applications_of_Fourier_optics_principles)和[傅里叶变换的卷积性质](http://www.thefouriertransform.com/transform/properties.php)，卷积层可以通过在输入平面后放置一个扰动元素（这被称为[4f相关器](https://en.wikipedia.org/wiki/Optical_correlator)）来实现，同时，矩阵乘法可以通过将元素放置在2个焦距和1个透镜的位置来实现。但这不是一堂光学课，而是一个编码教程，所以我们来看看代码吧！

要安装必要的依赖项，请使用你选择的环境管理器激活所需的虚拟环境，并使用 pip 安装Autograd和scikit-image（如果你还没有安装的话）。

`pip install autograd`

pip install scikit-image

我们将模拟一个光学系统，该系统基本上作为一个单输出生成器，通过一系列均匀间隔的相位图像处理一个平坦的输入波前。为了保持教程的相对简单性和减少代码行数，我们将尝试仅匹配一个目标图像，见下文（如果你想跟随教程，可以下载该图像到你的工作目录）。完成这个简单的教程后，你可能会想尝试构建一个光学分类器、自编码器或其他图像变换。

![Autograd图像变换示例](../Images/a156bf0c62efa17b762b1a91f8b2bc59.png)

现在开始一些Python代码，首先导入我们需要的包。

```py
import autograd.numpy as np
from autograd import grad

import matplotlib.pyplot as plt 

import time

import skimage

import skimage.io as sio 
```

我们将使用角谱方法来模拟光学传播。这是一种适用于近场条件的方法，在这种条件下，你的镜头或光束的孔径大小与传播距离相似。以下函数执行角谱方法传播，给定一个初始波前及其尺寸、光的波长和传播距离。

```py
def asm_prop(wavefront, length=32.e-3, \
wavelength=550.e-9, distance=10.e-3):

    if len(wavefront.shape) == 2:

        dim_x, dim_y = wavefront.shape

    elif len(wavefront.shape) == 3:

        number_samples, dim_x, dim_y = wavefront.shape

    else:

        print(“only 2D wavefronts or array of 2D wavefronts supported”)

    assert dim_x == dim_y, “wavefront should be square”

    px = length / dim_x

    l2 = (1/wavelength)**2

    fx = np.linspace(-1/(2*px), 1/(2*px) – 1/(dim_x*px), dim_x)

    fxx, fyy = np.meshgrid(fx,fx)

    q = l2 – fxx**2 – fyy**2

    q[q<0] = 0.0

    h = np.fft.fftshift(np.exp(1.j * 2 * np.pi * distance * np.sqrt(q)))

    fd_wavefront = np.fft.fft2(np.fft.fftshift(wavefront))

    if len(wavefront.shape) == 3:

        fd_new_wavefront = h[np.newaxis,:,:] * fd_wavefront

        New_wavefront = np.fft.ifftshift(np.fft.ifft2(\

                fd_new_wavefront))[:,:dim_x,:dim_x]

    else:

        fd_new_wavefront = h * fd_wavefront

        new_wavefront = np.fft.ifftshift(np.fft.ifft2(\

                fd_new_wavefront))[:dim_x,:dim_x]

    return new_wavefront
```

我们不会将我们的光学神经网络限制在卷积或矩阵乘法操作上，而是通过一系列均匀间隔的相位物体图像来传播光束。从物理上讲，这类似于将相干光束通过一系列薄的波浪玻璃板，只不过在这种情况下，我们将使用 Autograd 进行反向传播，以设计这些板使其将光从输入波前引导到最终匹配给定目标图案的位置。通过相位元件后，我们将在图像传感器的等效位置收集光。这给我们提供了一个从复杂场到实值强度的有趣非线性，我们可以利用这种非线性通过堆叠多个这样的组件来构建一个更复杂的光学神经网络。

每一层由通过一系列相隔较短距离的相位图像定义。计算上这描述为在短距离内传播，然后是一个薄相位板（实现为乘法）：

```py
def onn_layer(wavefront, phase_objects, d=100.e-3):
    for ii in range(len(phase_objects)):

        wavefront = asm_prop(wavefront * phase_objects[ii], distance=d)

    return wavefront
```

在 Autograd 中训练模型的关键在于定义一个返回标量损失的函数。然后可以将此损失函数包装在 Autograd 的 `grad` 函数中以计算梯度。你可以通过 `grad` 的 `argnum` 参数指定哪个参数包含要计算梯度的参数，并且要记住损失函数必须返回一个标量值，而不是数组。

```py
def get_loss(wavefront, y_tgt, phase_objects, d=100.e-3):
    img = np.abs(onn_layer(wavefront, phase_objects, d=d))**2

    mse_loss = np.mean( (img – y_tgt)**2 + np.abs(img-y_tgt) )

    return mse_loss

get_grad = grad(get_loss, argnum=2)
```

首先，让我们读取目标图像并设置输入波前。可以使用你选择的 64 x 64 图像，或者下载文章早期提到的灰度笑脸图像。

```py
# target image
tgt_img = sio.imread(“./smiley.png”)[:, :, 0]

y_tgt = 1.0 * tgt_img / np.max(tgt_img)

# set up input wavefront (a flat plane wave with an 16mm aperture)

dim = 128

side_length = 32.e-3

aperture = 8.e-3

wavelength = 550.e-9

k0 = 2*np.pi / wavelength

px = side_length / dim

x = np.linspace(-side_length/2, side_length/2-px, dim)

xx, yy = np.meshgrid(x,x)

rr = np.sqrt(xx**2 + yy**2)

wavefront = np.zeros((dim,dim)) * np.exp(1.j*k0*0.0)

wavefront[rr <= aperture] = 1.0 
```

接下来，定义学习率、传播距离和模型参数。

```py
lr = 1e-3
dist = 50.e-3

phase_objects = [np.exp(1.j * np.zeros((128,128))) \

        for aa in range(32)]

losses = []
```

如果你熟悉用 PyTorch 或类似库训练神经网络，训练循环应该很熟悉。我们调用之前定义的梯度函数（这是我们编写的计算损失函数的函数转换），并将得到的梯度应用到模型的参数上。我发现通过仅用梯度的相位更新参数（phase_objects），而不是原始的复数梯度，可以得到更好的结果。梯度的实值相位分量通过使用 NumPy 的 `np.angle` 访问，并通过 `np.exp(1.j * value)` 转换回复数值。

```py
for step in range(128):
    my_grad = get_grad(wavefront, y_tgt, phase_objects, d=dist)

    for params, grads in zip(phase_objects, my_grad):

        params -= lr * np.exp( -1.j * np.angle(grads))

     loss = get_loss(wavefront, y_tgt, phase_objects,d=dist)

     losses.append(loss)

     img = np.abs(onn_layer(wavefront, phase_objects))**2

     print(“loss at step {} = {:.2e}, lr={:.3e}”.format(step, loss, lr))

     fig = plt.figure(figsize=(12,7))

     plt.imshow(img / 2.0, cmap=”jet”)

     plt.savefig(“./smiley_img{}.png”.format(step))

     plt.close(fig)

fig = plt.figure(figsize=(7,4))

plt.plot(losses, lw=3)

plt.savefig(“./smiley_losses.png”)
```

如果一切顺利，你应该会看到均方误差损失单调减少，代码会保存一系列图像，描绘光学网络的输出如何越来越接近目标图像。

![光学系统优化以匹配目标图像](../Images/18839ccfb209efdfa4bc4bc317ce4ab4.png)

*光学系统优化以匹配目标图像。每个带有蓝色背景的编号图像是模型在不同训练步骤下的输出。对单样本训练，损失在训练过程中平稳下降，这并不令人惊讶。*

就这些！我们模拟了一个作为单输出生成器的光学系统。如果在运行代码时遇到问题，尝试一次性复制代码来自 [this Github gist](https://gist.github.com/riveSunder/96267f5a52a1ebe8f567505516d4e068)，以防引入错误。

### Autograd 的使用和局限性

Autograd 是一个灵活的自动微分包，在许多方面影响了主流机器学习库。在像机器学习这样快速发展的领域中，很难确定不同思想如何相互影响的谱系。然而，命令式、运行时定义的方法在 Chainer、PyTorch 和一些 2.0 之后具有即时执行的 TensorFlow 版本中占据了重要地位。根据 [libraries.io](https://libraries.io/pypi/autograd/dependents)，另外十个 Python 包依赖于 Autograd，包括用于 [求解逆运动学](https://github.com/lanius/tinyik)、 [敏感性分析](https://github.com/rgiordan/vittles)和 [高斯过程](https://github.com/Gattocrucco/lsqfitgp)的包。我个人最喜欢的是量子机器学习包 [PennyLane](https://github.com/xanaduai/pennylane)。

Autograd 可能不如 PyTorch 或 TensorFlow 强大，它也没有实现所有最新的深度学习技巧，但在某些开发阶段，这可能会成为一种优势。没有很多专业的 API 需要记忆，对熟悉 Python 和/或 NumPy 的人来说，学习曲线特别平缓。它没有用于部署或扩展的各种附加功能，但对于控制和自定义很重要的项目，它使用起来简单而高效。它特别适合需要将抽象数学概念转化为代码以构建任意机器学习或优化解决方案的数学家和物理学家。

在我们看来，使用 Autograd 的最大缺点是缺乏硬件加速支持。或许没有比 [这个 Github 问题](https://github.com/HIPS/autograd/issues/46) 上长达 4 年的讨论更能描述这一缺陷的了，该讨论探讨了引入 GPU 支持的各种方式。如果你按照本文中的光学神经网络教程进行操作，你可能已经注意到，即使是一个适中的模型，进行实验也可能需要极高的计算时间。使用 Autograd 的计算速度足以成为一个缺点，以至于我们实际上不推荐将其用于比上述 MLP 或 ONN 生成器演示更大的项目。

相反，可以考虑 JAX，这是一个由 Google Brain 研究人员（包括 Autograd 开发人员）开发的 Apache 2.0 许可库。JAX 结合了硬件加速和即时编译，在速度上比本地 NumPy 代码有显著提升。此外，JAX 提供了一套函数转换，用于自动并行化代码。JAX 可能比直接用 Autograd 替代 NumPy 要稍微复杂，但其强大的功能完全可以弥补这一点。我们将在未来的文章中比较 JAX、Autograd 以及流行的 PyTorch 和 TensorFlow。

[原文](https://blog.exxactcorp.com/autograd-the-best-machine-learning-library-youre-not-using/)。经许可转载。

**相关：**

+   [深度学习的 PyTorch：免费电子书](/2020/07/pytorch-deep-learning-free-ebook.html)

+   [深度神经网络中的批量归一化](/2020/08/batch-normalization-deep-neural-networks.html)

+   [信号处理中的深度学习：你需要知道的]( /2020/07/deep-learning-signal-processing.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

### 更多相关内容

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [成为优秀数据科学家所需的 5 种关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [停止学习数据科学以寻找目标，并通过找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
