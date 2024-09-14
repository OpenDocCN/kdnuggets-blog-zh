# 基于神经网络的初创公司名称生成器

> 原文：[https://www.kdnuggets.com/2018/04/neural-network-startup-name-generator.html](https://www.kdnuggets.com/2018/04/neural-network-startup-name-generator.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Alexander Engelhardt](http://www.alpha-epsilon.de/)，自由数据科学家**

### 摘要

在这篇文章中，我展示了一个自动生成初创公司名称建议的 Python 脚本。你给它一个具有特定主题的文本语料库，例如凯尔特文本，然后它会输出类似发音的建议。一个示例调用如下：

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

./generate.py -n 10 -t 1.2 -m models/gallic_500epochs.h5 wordlists/gallic.txt --suffix 软件

输出：

=======

Ercos 软件

Riuri 软件

Palia 软件

Critim 软件

Arios 软件

Veduos 软件

Eigla 软件

Isbanos 软件

Edorio 软件

Emmos 软件

我将这个脚本应用于英语、德语和法语的“普通”文本，然后尝试了凯尔特歌曲、宝可梦名字和 J.R.R. 托尔金的黑语（魔多之语）的语料库。

我已经将一些更长的样本提案列表发布在[这里](https://github.com/AlexEngelhardt/startup-name-generator/tree/master/sample-outputs/)。

你可以在我的 GitHub 仓库中找到代码、我使用的所有文本语料库以及一些预计算模型：

[https://github.com/AlexEngelhardt/startup-name-generator](https://github.com/AlexEngelhardt/startup-name-generator)

### 目标

最近，我和一位同事开始创办一家软件公司，但我们想到的大多数名字都已经在用。我们想要一个具有凯尔特风格的名字，并且需要大量的候选名称来找到一个仍然可用的。

所以我开始创建一个生成新型人工词汇的神经网络。你需要输入一个你喜欢的特定风格的样本文本，例如凯尔特歌曲，它就能捕捉文本的特点（“语言”）并生成新的类似发音的词汇。著名的[Andrej Karpathy 的博客文章](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)为我提供了必要的知识和信心，这个想法是现实的。

### 数据预处理

我首先建立了一个原始文本的语料库。对于预处理，我剥离了所有非字母字符。然后，我将文本分割成单词并只保留唯一的单词。我认为这个步骤是合理的，因为我不希望模型学习最常见的单词，而是理解整个语料库的结构。

之后，大多数文本语料库最终成为 1000 到 2000 个单词的列表。

### RNN 架构

循环神经网络可以特别好地建模语言，并且是这个词生成任务的合适类型。然而，找到‘完美’的 RNN 架构仍然有些像黑魔法。像使用多少层或多少单元这样的问题没有确定的答案，而是依赖于经验和直觉。

为了节省训练时间，我希望模型既足够复杂又尽可能简单。我选择了一个每层有 50 个单元的字符级双层 LSTM，并训练了 500 个周期。这个模型输出的单词已经听起来非常不错了。

### 采样温度

RNN 逐个字符生成新名称。它不仅输出下一个字符，还输出下一个字符的*分布*。这允许我们要么选择概率最高的字母，要么从提供的分布中采样。

我发现一个很好的方法是改变采样过程的[温度](https://cs.stackexchange.com/questions/79241/what-is-temperature-in-lstm-and-neural-networks-generally)。温度是一个调整采样权重的参数。“标准”温度 1 不改变权重。对于较低的温度，采样变得不那么随机，即更保守，几乎总是选择最大权重对应的字母。另一个极端，大温度，会调整权重接近均匀分布，代表完全的随机性。对于实际文本采样，温度低于 1 可能是合适的，但由于我想要新词，高温度似乎更好。

![变化温度的效果](../Images/aa677edde1f381f8c762b7caaefe6fde.png)

在上图中，假设我们想从 A、B、…、J 中采样一个字母。你 RNN 输出的原始权重可能是红色条。如果你降低温度，权重会变成黄色条（温度 = 0.1），如果你提高温度，它们会变成绿色条（温度 = 3）。

### 示例调用和输出

使用-h参数调用脚本以打印所有可能的参数概述。以下命令在wordlists/english.txt语料库上训练一个LSTM模型500轮（-e 500），将模型保存（-s）到models/english_500epochs.h5，然后用1.2的温度（-t 1.2）采样10个公司名称（-n 10），最后将“Software”添加到名称后（--suffix）（我在[这里](https://www.reddit.com/r/Entrepreneur/comments/4jfrgl/is_there_a_list_of_generic_company_name_endings/)找到了一长串可能的后缀）。在训练过程中，我喜欢传递-v参数以运行详细模式。然后，模型每10轮打印一些额外信息以及几个生成的示例单词：

./generate.py -v -e 500 -n 10 -t 1.2 -s models/english_500epochs.h5 wordlists/english.txt --suffix Software

我的呼叫返回了这些建议：

Officers Software

Ahips Software

Appearing Software

Introduce Software

Using Software

Alarmed Software

Interettint Software

Entwrite Software

Understood Software

Aspemardan Software

我遇到的一些其他优质名字建议，太好以至于不分享不行：

+   确实（看，它有效！）

+   Unifart（我敢你！）

+   Lyston

+   Alton

+   Rocking

+   Moor

+   Purrs

+   Ture

+   Exace

+   Overheader

在你存储模型后（使用-s选项），加载模型比重新计算模型要快（使用-m而不是-s参数）。

### 更具异国情调的语料库

我收集了一些德语、英语和法语的文本，只为拥有一些听起来真实的单词，并评估模型对语料结构的学习效果。

然而，我的大部分时间随后花在了更有趣的语料库上。下面，我将简要描述它们，并展示一些生成单词的随机样本。

#### Celtic

该语料库由一个高卢词典和[Eluveitie](http://www.darklyrics.com/e/eluveitie.html)的高卢语歌曲歌词组成：

Lucia

Reuoriosi

Iacca

Helvetia

Eburo

Ectros

Uxopeilos

Etacos

Neuniamins

Nhellos

#### Pokemon

如果你提供一个[所有宝可梦的列表](https://github.com/AlexEngelhardt/startup-name-generator/blob/master/wordlists/pokemon.txt)，你会得到宝可梦主题的名字：

Grubbin

Agsharon

Oricorina

Erskeur

Electrode

Ervivare

Unfeon

Whinx

Onterdas

Cagbanitl

#### 托尔金的黑语

托尔金的[黑语](http://www.angelfire.com/ia/orcishnations/englishorcish.html)，魔多的语言，是一个纯粹的娱乐实验：

Aratani

Arau

Ushtarak

Ishi

Kakok

Ulig

Ruga

Arau

Lakan

Udaneg

这个工具已经证明对我们很有用，提出了一些非常悦耳的名字。它也可能对其他人有帮助，我

**Bio: [亚历山大·恩格尔哈特](http://www.alpha-epsilon.de/)**，最近获得了慕尼黑大学的统计学硕士和博士学位，随后成为了专注于机器学习的自由数据科学家。亚历山大最近对开源产生了兴趣，开始贡献于R包‘mlr’。这篇文章介绍了我第一个个人侧项目。

[原文](http://alpha-epsilon.de/python/2018/04/04/an-lstm-based-startup-name-generator/)。转载经许可。

**相关：**

+   [成为数据科学家应该了解的十种机器学习算法](https://www.kdnuggets.com/2018/04/10-machine-learning-algorithms-data-scientist.html)

+   [PyTorch入门第1部分：理解自动微分如何工作](https://www.kdnuggets.com/2018/04/getting-started-pytorch-understanding-automatic-differentiation.html)

+   [实用数据科学：构建最小可行模型](https://www.kdnuggets.com/2016/11/practical-data-science-building-minimum-viable-models.html)

### 更多相关主题

+   [12个建议：从数据分析师到创业公司联合创始人](https://www.kdnuggets.com/2021/12/12-tips-data-analyst-to-co-founder.html)

+   [在ChatGPT时代构建深科技创业公司的10大障碍](https://www.kdnuggets.com/2023/04/10-hurdles-building-deep-tech-startup-age-chatgpt.html)

+   [2022年通过构建15个神经网络项目学习深度学习](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用AIMET进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [使用TensorFlow和Keras构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [使用PyTorch构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)
