# LSTMs 再次崛起：扩展LSTM模型挑战变换器的优势

> 原文：[https://www.kdnuggets.com/lstms-rise-again-extended-lstm-models-challenge-the-transformer-superiority](https://www.kdnuggets.com/lstms-rise-again-extended-lstm-models-challenge-the-transformer-superiority)

![LSTMs 再次崛起](../Images/159eea8cbfb38f1606c4fa351a3de0ad.png)

作者提供的图片

LSTM最初由[Sepp Hochreiter](https://scholar.google.at/citations?user=tvUH3WMAAAAJ&hl=en)和[Jurgen Schmidhuber](https://scholar.google.com/citations?user=gLnCTgIAAAAJ&hl=en)于1990年代初期引入。最初的模型计算成本极高，直到2010年代中期，RNN和LSTM才受到关注。随着数据量的增加和更好的GPU的出现，LSTM网络成为语言建模的标准方法，并成为第一个大型语言模型的基础。这种情况一直持续到2017年[基于注意力的变换器架构](https://arxiv.org/abs/1706.03762)的发布。LSTM逐渐被变换器架构取代，后者现在是所有最近的大型语言模型，包括ChatGPT、Mistral和Llama的标准。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT工作。

* * *

然而，由原始LSTM作者**[Sepp Hochreiter](https://arxiv.org/abs/2405.04517)** 最近发布的xLSTM论文在研究界引起了巨大轰动。结果显示其预训练结果与最新的LLMs进行了比较，并且提出了一个问题：LSTM是否可以再次在自然语言处理领域取得突破。

## 高级架构概述

原始的LSTM网络有一些主要的局限性，这些局限性限制了它在更大上下文和更深层模型中的可用性。具体来说：

+   LSTM是顺序模型，这使得训练和推理难以并行化。

+   他们的存储能力有限，所有信息必须压缩到一个单独的单元状态中。

最近发布的xLSTM网络引入了新的sLSTM和mLSTM模块，以解决这些不足之处。让我们从整体上了解一下模型架构，并看看作者采用了什么方法。

### 原始LSTM的简短评述

LSTM网络使用了隐藏状态和单元状态来解决普通RNN网络中的梯度消失问题。他们还添加了忘记、输入和输出的sigmoid门来控制信息流动。方程如下：

![LSTM方程](../Images/9401b41e35bec3728ce22f18db903377.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

细胞状态（ct）经过LSTM单元时经过了微小的线性变换，帮助在较长的输入序列中保持梯度。

xLSTM模型在新模块中修改了这些方程，以补救模型已知的局限性。

### sLSTM模块

该模块修改了sigmoid门，并对输入门和遗忘门使用了指数函数。正如作者所引述的，这可以改善LSTM中的存储问题，并且仍然允许多个记忆单元在每个头内混合记忆，但不跨头。修改后的sLSTM模块方程如下：

![sLSTM方程](../Images/d2abcf38ff255621e3659e5d35a26b36.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

此外，由于指数函数可能导致大值，门值使用对数函数进行归一化和稳定。

### mLSTM模块

为了解决LSTM网络中的并行性和存储问题，xLSTM将细胞状态从1维向量修改为2维方阵。他们存储了作为键和值向量的分解版本，并使用与sLSTM模块相同的指数门控。方程如下：

![mLSTM方程](../Images/8fff002f2873a8f54c668208a7d318fa.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

### 架构图

![xLSTM架构图](../Images/eb03b32b1d213056a0f3ece07c260e13.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

整体xLSTM架构是mLSTM和sLSTM模块按不同比例顺序组合而成。如图所示，xLSTM模块可以具有任意记忆单元。不同的模块通过层归一化堆叠在一起，形成深度残差网络。

## 评估结果与比较

作者在语言模型任务上训练xLSTM网络，并将训练模型的困惑度 *(越低越好)* 与当前的基于Transformer的LLMs进行比较。

作者首先在SlimPajama的15B令牌上训练模型。结果表明，xLSTM在验证集上的困惑度得分最低，优于所有其他模型。

![xLSTM评估与比较](../Images/601c3fa61e44dba4d5701227f48d4eb5.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

### 序列长度外推

作者还分析了测试时序列长度超过模型训练时上下文长度的表现。他们在2048的序列长度上训练了所有模型，下面的图展示了随着令牌位置变化的验证困惑度：

![xLSTM序列长度外推](../Images/19523ecbdaaaac64f805fbf8f96065ac.png)

图片来源于 [论文](https://arxiv.org/abs/2405.04517)

图表显示，即使对于更长的序列，xLSTM网络仍能保持稳定的困惑度得分，并在更长的上下文长度中表现优于其他任何模型。

### 扩展xLSTM到更大模型尺寸

作者进一步在300B个来自SlimPajama数据集的标记上训练模型。结果表明，即使对于更大的模型，xLSTM的扩展性也优于当前的Transformer和Mamba架构。

![Scaling xLSMT](../Images/48ffcf08af07141efd683e72144e3295.png)

图片来源于[论文](https://arxiv.org/abs/2405.04517)

## 总结

这可能很难理解，不过没关系！尽管如此，你现在应该明白为什么这篇研究论文最近受到了如此关注。它的表现至少与近期的大型语言模型一样好，甚至更优。已证明它可以扩展到更大的模型，并且可以成为所有近期基于Transformers构建的LLMs的严肃竞争者。只有时间会证明LSTMs是否会重新获得辉煌，但现在我们知道xLSTM架构在挑战著名的Transformers架构的优势。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)** Kanwal是一位机器学习工程师和技术作家，对数据科学以及人工智能与医学的交叉领域充满深厚的热情。她共同撰写了电子书《利用ChatGPT最大化生产力》。作为2022年亚太地区Google一代学者，她倡导多样性和学术卓越。她还被誉为Teradata技术多样性学者、Mitacs全球研究学者和哈佛WeCode学者。Kanwal是变革的热心倡导者，创办了FEMCodes以赋能STEM领域的女性。

### 更多相关话题

+   [如果我必须重新开始学习数据科学，我会怎么做？](https://www.kdnuggets.com/2020/08/start-learning-data-science-again.html)

+   [R与Python（再次）：一个人因视角](https://www.kdnuggets.com/2022/01/r-python-human-factor-perspective.html)

+   [从那里和回来……一个RAPIDS故事](https://www.kdnuggets.com/2023/06/back-again-rapids-tale.html)

+   [为什么Emily Ekdahl选择co:rise来提升她作为…的工作表现](https://www.kdnuggets.com/2022/08/corise-emily-ekdahl-chose-corise-level-job-performance-machine-learning-engineer.html)

+   [拖拽、分析：无代码数据科学的崛起](https://www.kdnuggets.com/drag-drop-analyze-the-rise-of-nocode-data-science)

+   [提示工程的兴起与衰落：时尚还是未来？](https://www.kdnuggets.com/the-rise-and-fall-of-prompt-engineering-fad-or-future)
