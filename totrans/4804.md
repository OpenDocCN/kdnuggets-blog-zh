# 仅使用 Numpy：使用 Numpy 实现 GAN 和 Adam 优化器

> 原文：[https://www.kdnuggets.com/2018/08/only-numpy-implementing-gans-adam-optimizer.html](https://www.kdnuggets.com/2018/08/only-numpy-implementing-gans-adam-optimizer.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Jae Duk Seo](https://jaedukseo.me/)，瑞尔森大学**

今天我受到这篇博客 “[生成对抗网络在 TensorFlow 中的实现](https://wiseodd.github.io/techblog/2016/09/17/gan-tensorflow/)” 的启发，想要用 Numpy 自行实现 GAN。以下是 [**@**goodfellow_ian](https://twitter.com/goodfellow_ian) 的 [原始 GAN 论文](https://arxiv.org/abs/1406.2661)。下面是从 Simple GAN 生成的所有图像的 gif。

![](../Images/53ba5fda91dd8892b617e9c6df1f6ef4.png)

在继续阅读之前，请注意我不会涉及太多数学内容。我会侧重于代码的实现和结果，数学部分可能会在以后讨论。我正在使用 Adam 优化器，但在这篇文章中我不会解释 Adam 的实现。

### **GAN 中判别器的前向传播 / 部分反向传播**

![](../Images/92e29f62d3b24fb4a204a0ef4016facb.png)

再次说明，我不会深入详细讲解，但请注意红色框中的数据区域。在 GAN 的判别网络中，这些数据可以是真实图像或由生成网络生成的虚假图像。我们的图像是 (1,784) 的 MNIST 数据集向量。

还有一点需要注意的是 **红色 (L2A) 和蓝色 (L2A)**。红色 (L2A) 是我们判别网络在真实图像输入下的最终输出。而蓝色 (L2A) 是我们判别网络在虚假图像输入下的最终输出。

![](../Images/621c48ea875a48847e0c6708185cdf6e.png)

我们的实现方式是先获取真实图像和虚假数据，然后将它们输入网络中。

+   第 128 行——获取真实图像数据

+   第 147 行——获取虚假图像数据（由生成网络生成）

+   第 162 行——我们判别网络的成本函数。

同时，请注意蓝色框区域，即我们的成本函数。让我们比较一下原始论文中的成本函数，如下所示。

![](../Images/357d14a736bed4c408f787a3a28772b0.png)

图片 [来自原始 论文](https://arxiv.org/pdf/1406.2661.pdf)

区别在于我们在第一个术语 log(L2A) 前面加了一个 (-) 负号。

![](../Images/7e31bc8abaeb8cf6f9311a0630fd5b4d.png)

图片 [来自 Agustinus Kristiadi](https://wiseodd.github.io/techblog/2016/12/24/conditional-gan-tensorflow/)

如上所述，在 TensorFlow 实现中，我们如果要最大化某个值时会翻转符号，因为 TF 的自动微分只能最小化。

我考虑了这一点，决定以类似的方式实现。因为我想最大化我们判别器对真实图像正确猜测的机会，同时最小化判别器对假图像错误猜测的机会，并且希望这些值的总和能够平衡。然而，我对此部分尚未百分之百确定，会尽快重新审视这个问题。

### **前向传播/生成器在GAN中的部分反向传播**

![](../Images/271b5c892299581325f4b6e3aeaf0aea.png)

GAN中生成器网络的反向传播过程有点复杂。

+   蓝色框——来自生成器网络的生成假数据

+   绿色框（左下角）——判别器接收生成的（蓝色框）输入并执行前向传播过程

+   橙色框——生成器网络的成本函数（我们再次希望最大化生成真实数据的机会）

+   绿色框（右上角）——生成器网络的反向传播过程，但我们必须将梯度传递整个判别器网络。

下面是已实现代码的截图。

![](../Images/96b04dff34b3a7adb53d5c755e74d033.png)

标准的反向传播，没什么特别的。

### **训练结果：失败的尝试**

我很快意识到训练GAN是非常困难的，即使使用Adam优化器，网络似乎也没有很好地收敛。因此，我将首先向你展示所有失败的尝试及其网络架构。

*1\. 生成器，2层：200，560个隐藏神经元，输入向量大小100*

![](../Images/db544005fa036d235b647301bdccc234.png)

*2\. 生成器，tanh() 激活函数，2层：245，960个隐藏神经元，输入向量大小100*

![](../Images/05c0c34086bb1bc162a0b272c34575e8.png)

*3\. 生成器，3层：326，356，412个隐藏神经元，输入向量大小326*

![](../Images/fca293dfd8d27c1253a9645494278ece.png)

*4\. 生成器，2层：420，640个隐藏神经元，输入向量大小350*

![](../Images/9ed74e5863193c43fbbbbf3183f3a7a3.png)

*5\. 生成器，2层：660，780个隐藏神经元，输入向量大小600*

![](../Images/116ef5a027168b5c5a72867a841bc824.png)

*6\. 生成器，2层：320，480个隐藏神经元，输入向量大小200*

![](../Images/7db05c1d1e38126e17de30b621ba359b.png)

如上所示，它们似乎都学到了一些东西，但实际上并没有哈哈。然而，我能够使用一个很棒的技巧生成一种看起来像数字的图像。

### **极端步进梯度衰减**

![](../Images/1e679f74769b9f026b2939639a473771.png)

上面是一个gif，我知道区别很细微，但相信我 [我不是在整蛊你](https://www.youtube.com/watch?v=dQw4w9WgXcQ)。这个技巧非常简单且易于实现。我们首先设定一个较高的学习率进行第一次训练，然后在第一次训练后将学习率衰减因子设置为0.01。由于某些未知原因（我想进一步调查），这似乎有效。

但代价非常高，我认为我们正趋向于一个“地方”，在这里网络只能生成某种类型的数据。也就是说，从-1到1之间的均匀分布的数字来看，生成器只会生成仅仅看起来像3或2等的图像……但关键点在于，网络无法生成不同的一组数字。这一点从图像中所有的数字都看起来像3这一事实中可以得到证明。

然而，这些图像在某种程度上看起来像数字。所以，让我们再看看一些结果。

![](../Images/41b171e99a5b6f1692fa0017c4de70fe.png)

如上所示，随着时间的推移，数字变得更加清晰。一个好的例子是生成的3或9的图像。

### **互动代码**

![](../Images/871598a423ec4616a213c205f4edb3a3.png)

*更新：我已经迁移到 Google Colab 进行交互式代码演示！因此，您需要一个 Google 账号来查看代码，并且您不能在 Google Colab 中运行只读脚本，所以请在您的工作区复制一份。最后，我绝不会请求访问您 Google Drive 上的文件，仅供参考。祝编程愉快！*

请点击 [这里访问在线互动代码](https://colab.research.google.com/notebook#fileId=1D2kF1uBbJlVuglpuBSrw7A_3ws-Nvxk-)

![](../Images/b8e73637d1e0084468998e3abe489d41.png)

运行代码时，请确保您在“main.py”标签页上，如上图绿色框中所示。程序会要求您输入一个随机数作为种子，如蓝色框中所示。之后，它将生成一张图像，查看该图像请点击上方的“点击我”标签，红色框所示。

### **结语**

训练 GAN 即使是部分有效也是一项巨大的工作，我想研究更有效的 GAN 训练方法。最后，感谢 [**@**replit](https://twitter.com/replit)，这些家伙太棒了！

如果发现任何错误，请通过电子邮件联系我：jae.duk.seo@gmail.com。

同时，请在我的 Twitter 上关注我 [这里](https://twitter.com/JaeDukSeo)，访问 [我的网站](https://jaedukseo.me/)，或我的 [YouTube 频道](https://www.youtube.com/c/JaeDukSeo) 获取更多内容。如果您感兴趣，我还对解耦神经网络进行了比较 [这里](https://becominghuman.ai/only-numpy-implementing-and-comparing-combination-of-google-brains-decoupled-neural-interfaces-6712e758c1af)。

**参考文献**

1.  Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., … & Bengio, Y. (2014). 生成对抗网络。见 *神经信息处理系统的进展* (第2672–2680页)。

1.  免费在线 GIF 制作工具——轻松制作 GIF 图像。（无日期）。于 2018 年 1 月 31 日检索自 [http://gifmaker.me/](http://gifmaker.me/)

1.  TensorFlow 中的生成对抗网络。（无日期）。于 2018 年 1 月 31 日检索自 [https://wiseodd.github.io/techblog/2016/09/17/gan-tensorflow/](https://wiseodd.github.io/techblog/2016/09/17/gan-tensorflow/)

1.  J. (无日期). Jrios6/Adam-vs-SGD-Numpy。检索于2018年1月31日，来自 [https://github.com/jrios6/Adam-vs-SGD-Numpy/blob/master/Adam%20vs%20SGD%20-%20On%20Kaggles%20Titanic%20Dataset.ipynb](https://github.com/jrios6/Adam-vs-SGD-Numpy/blob/master/Adam%20vs%20SGD%20-%20On%20Kaggles%20Titanic%20Dataset.ipynb)

1.  Ruder, S. (2018年1月19日). 梯度下降优化算法概述。检索于2018年1月31日，来自 [http://ruder.io/optimizing-gradient-descent/index.html#adam](http://ruder.io/optimizing-gradient-descent/index.html#adam)

1.  E. (1970年1月1日). Eric Jang. 检索于2018年1月31日，来自 [https://blog.evjang.com/2016/06/generative-adversarial-nets-in.html](https://blog.evjang.com/2016/06/generative-adversarial-nets-in.html)

**个人简介: [Jae Duk Seo](https://jaedukseo.me/)** 是赖尔森大学的四年级计算机科学学生。

[原文](https://towardsdatascience.com/only-numpy-implementing-gan-general-adversarial-networks-and-adam-optimizer-using-numpy-with-2a7e4e032021). 经许可转载。

**相关内容:**

+   [通过Tensorflow中的交互式代码洞悉神经网络的思维](/2018/06/inside-mind-neural-network-interactive-code-tensorflow.html)

+   [使用NumPy从零开始构建卷积神经网络](/2018/04/building-convolutional-neural-network-numpy-scratch.html)

+   [我如何使用CNN和Tensorflow，并在Kaggle挑战赛中失去了银牌](/2018/05/lost-silver-medal-kaggle-mercari-price-suggestion-challenge-cnns-tensorflow.html)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供IT支持

* * *

### 更多相关话题

+   [在PyTorch中调整Adam优化器参数](https://www.kdnuggets.com/2022/12/tuning-adam-optimizer-parameters-pytorch.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [90亿美元AI失败的分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
