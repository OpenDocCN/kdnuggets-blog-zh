# 在 60 分钟内解锁 LLMs 的秘密，与 Andrej Karpathy

> 原文：[`www.kdnuggets.com/unlock-the-secrets-of-llms-in-a-60-minute-with-andrej-karpathy`](https://www.kdnuggets.com/unlock-the-secrets-of-llms-in-a-60-minute-with-andrej-karpathy)

![在 60 分钟内解锁 LLMs 的秘密，与 Andrej Karpathy](img/0115dbe580574df16e470366c3365eec.png)

图片来源：编辑

你听说过[Andrej Karpathy](https://karpathy.ai/)吗？他是一位著名的计算机科学家和人工智能研究员，以其在深度学习和神经网络方面的工作而闻名。他在 OpenAI 的 ChatGPT 开发中发挥了关键作用，曾担任特斯拉 AI 高级总监。在此之前，他设计并主讲了第一门深度学习课程[斯坦福 - CS 231n：卷积神经网络用于视觉识别](http://cs231n.stanford.edu/2016/)。该课程成为斯坦福大学最大的课程之一，从 2015 年的 150 名学生增长到 2017 年的 750 名学生。我强烈推荐对深度学习感兴趣的人观看这个 YouTube 视频。我将不再详细介绍他，我们将转向他在 YouTube 上最受欢迎的讲座之一，观看量超过**140 万**的“介绍大型语言模型”。这次讲座是一个忙碌人士了解 LLMs 的入门必看内容。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

我提供了这次演讲的简要总结。如果这引起了你的兴趣，我强烈建议你查看文章末尾提供的幻灯片和 YouTube 链接。

# 演讲概述

这次讲座提供了 LLMs 的全面介绍，包括它们的能力以及使用它们可能带来的潜在风险。它被分为以下三个主要部分：

## 第一部分：LLMs

![在 60 分钟内解锁 LLMs 的秘密，与 Andrej Karpathy](img/2e869eee5c552f500a823fcc845cfe5e.png)

幻灯片由 Andrej Karpathy 提供

LLMs 在大量文本语料库上进行训练，以生成类似人类的回应。在这一部分中，Andrej 具体讨论了 Llama 2-70b 模型。它是最大的 LLM 之一，具有 700 亿个参数。该模型由两个主要组件组成：参数文件和运行文件。参数文件是一个大型二进制文件，包含模型的权重和偏差。这些权重和偏差本质上是模型在训练过程中学习到的“知识”。运行文件是一段用于加载参数文件并运行模型的代码。模型的训练过程可以分为以下两个阶段：

### 1\. 预训练

这涉及从互联网收集大量文本，约 10TB，然后使用 GPU 集群对这些数据进行模型训练。训练过程的结果是一个基本模型，它是互联网的有损压缩。它能够生成连贯和相关的文本，但不能直接回答问题。

### 2\. 微调

预训练模型会在高质量的数据集上进一步训练，以使其更有用。这将产生一个助手模型。Andrej 还提到微调的第三阶段，即使用对比标签。模型不是从头生成答案，而是给出多个候选答案，并要求选择最佳答案。这比生成答案可能更简单、更高效，并且能进一步提高模型性能。这个过程称为从人类反馈中进行的强化学习（RLHF）。

## 第二部分：LLMs 的未来

![在与 Andrej Karpathy 的 60 分钟中揭开 LLM 的秘密](img/5182a5aad95602cce677ea2a91f42166.png)

幻灯片由 Andrej Karpathy 提供

在讨论大型语言模型及其能力的未来时，讨论了以下关键点：

### 1\. 规模定律

模型的性能与两个变量相关——参数数量和训练文本量。训练在更多数据上的较大模型往往能够取得更好的性能。

### 2\. 工具的使用

像 ChatGPT 这样的语言模型（LLMs）可以利用浏览器、计算器和 Python 库等工具，执行对模型单独来说具有挑战性或不可能完成的任务。

### 3\. LLM 中的系统一和系统二思维

当前，LLMs 主要采用系统一思维——快速、本能和基于模式的。然而，人们对开发能够进行系统二思维——较慢、理性并且需要有意识努力的 LLMs 感兴趣。

### 4\. LLM 操作系统

LLMs 可以被视为新兴操作系统的内核进程。它们可以读取和生成文本，拥有广泛的各种主题知识，浏览互联网或引用本地文件，使用现有的软件基础设施，生成图像和视频，听到和说话，并使用系统 2 进行长时间的思考。LLM 的上下文窗口类似于计算机中的 RAM，内核进程尝试将相关信息分页进出其上下文窗口以执行任务。

## 第三部分：LLMs 安全性

![与 Andrej Karpathy 一起在 60 分钟内解锁 LLM 的秘密](img/7cb3ab39debee876028b8a4ea75e8256.png)

幻灯片由 Andrej Karpathy 制作

Andrej 强调了在解决 LLMs 相关安全挑战方面的持续研究工作。讨论了以下攻击：

### 1\. 越狱

尝试绕过 LLM 中的安全措施以提取有害或不适当的信息。示例包括通过角色扮演欺骗模型以及使用优化的单词或图像序列操控响应。

### 2\. 提示注入

涉及向 LLM 中注入新的指令或提示，以操控其响应。攻击者可以将指令隐藏在图像或网页中，从而导致模型的回答中包含无关或有害的内容。

### 3\. 数据中毒 / 后门攻击 / 睡眠代理攻击

涉及在恶意或操控的数据上训练大型语言模型，这些数据包含触发短语。当模型遇到这些触发短语时，它可能被操控执行不良操作或提供错误预测。

你可以通过点击下面的链接在 YouTube 上观看全面的视频：

**幻灯片：** [点击这里](https://drive.google.com/file/d/1pxx_ZI7O-Nwl7ZLNk5hI3WzAsTLwvNU7/view)

# 结论

如果你对 LLMs 是新手，并且正在寻找启动学习之旅的资源，那么这个全面的列表是一个很好的起点！它包含了基础课程和特定于 LLM 的课程，将帮助你建立扎实的基础。此外，如果你对更结构化的学习体验感兴趣，[Maxime Labonne](https://github.com/mlabonne)最近推出了他的 LLM 课程，有三个不同的课程轨道可以根据你的需求和经验水平进行选择。这里是两个资源的链接，供你参考：

1.  掌握大型语言模型的全面资源列表（作者：Kanwal Mehreen

1.  [Maxime Labonne 的语言模型课程](https://github.com/mlabonne/llm-course)

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学以及人工智能与医学的交集充满了深厚的热情。她共同撰写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年亚太地区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认定为 Teradata 技术多样性学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的坚定倡导者，她创立了 FEMCodes，旨在赋能女性进入 STEM 领域。

### 更多相关话题

+   [解锁选择完美机器学习算法的秘密！](https://www.kdnuggets.com/2023/07/ml-algorithm-choose.html)

+   [解锁人工智能的力量 - KDnuggets 和 Machine…的特别发布](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [通过这个免费的 DevOps 速成课程激发你的潜力](https://www.kdnuggets.com/2023/03/corise-unlock-potential-with-this-free-devops-crash-course.html)

+   [ChatGPT 驱动的数据探索：解锁数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)

+   [解锁你的下一步行动：节省高达 67%的数据技能提升费用](https://www.kdnuggets.com/2023/03/datacamp-unlock-next-move-save-67-indemand-data-upskilling.html)

+   [通过 DataOps.live 解锁 DataOps 成功 - 被 Gartner 推荐…](https://www.kdnuggets.com/2023/07/dataopslive-unlock-dataops-success-featured-gartner-market-guide.html)
