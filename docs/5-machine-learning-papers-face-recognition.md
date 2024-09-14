# 5 篇关于面部识别的机器学习论文

> 原文：[`www.kdnuggets.com/2020/05/5-machine-learning-papers-face-recognition.html`](https://www.kdnuggets.com/2020/05/5-machine-learning-papers-face-recognition.html)

评论

![5 篇关于面部识别的机器学习论文](img/3150c9e6bb0238dc10071f262fe0bfa6.png)

面部识别，或称面部识别，是计算机视觉领域最大的研究领域之一。我们现在可以使用面部识别来解锁手机、在安检门验证身份，以及在一些国家进行购买。由于其使众多过程更高效的能力，许多公司投资于面部识别技术的研究与开发。本文将重点介绍一些相关研究，并介绍五篇关于面部识别的机器学习论文。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 方面

* * *

### 面部识别领域的基础机器学习论文

### 1\. 一个大规模多模态面部反欺骗的数据集与基准

随着面部识别技术在现实世界中的应用越来越广泛，它变得越来越重要。从智能手机解锁到面部验证支付方式，面部识别可以在许多方面提高安全性和监控。然而，这项技术也带来了若干风险。众多面部欺骗方法可能被用来欺骗这些系统。因此，面部反欺骗技术对防止安全漏洞至关重要。

为了支持面部反欺骗研究，本文的作者引入了一个名为 CASIASURF 的多模态面部反欺骗数据集。截至本文撰写时，这是最大的公开面部反欺骗数据集。具体而言，该数据集包括 21,000 段视频，拍摄自 1,000 名受试者，涵盖 RGB、深度和红外模态。除了数据集之外，作者还提出了一个新颖的多模态融合模型，作为面部反欺骗的基准。

**发布 / 最后更新** – 2019 年 4 月 1 日

**作者与贡献者** – Shifeng Zhang（NLPR, CASIA, UCAS, 中国），Xiaobo Wang（JD AI Research），Ajian Liu（MUST, 澳门，中国），Chenxu Zhao（JD AI Research），Jun Wan（NLPR, CASIA, UCAS, 中国），Sergio Escalera（巴塞罗那大学），Hailin Shi（JD AI Research），Zezheng Wang（JD Finance），Stan Z. Li（NLPR, CASIA, UCAS, 中国）。

**[立即阅读](https://arxiv.org/pdf/1812.00408v3.pdf)**

### 2\. FaceNet：一个统一的面部识别与聚类嵌入

在本文中，作者介绍了一种名为 FaceNet 的人脸识别系统。该系统使用深度卷积神经网络来优化嵌入，而不是使用中间瓶颈层。作者指出，这种方法最重要的方面是系统的端到端学习。

团队在 CPU 集群上对卷积神经网络进行了 1,000 到 2,000 小时的训练。然后，他们在四个数据集上评估了他们的方法。值得注意的是，FaceNet 在著名的 Labeled Faces in the Wild (LFW) 数据集上达到了 99.63% 的准确率，在 Youtube Faces Database 上达到了 95.12% 的准确率。

**发布时间/最后更新** – 2015 年 6 月 17 日

**作者和贡献者** – Florian Schroff、Dmitry Kalenichenko 和 James Philbin，来自 Google Inc.

**[立即阅读](https://arxiv.org/pdf/1503.03832v3.pdf)**

### 3\. 概率性人脸嵌入

在撰写本文时，用于人脸识别的当前嵌入方法能够在受控环境中实现高性能。这些方法通过拍摄人脸图像并将关于该人脸的数据存储在潜在语义空间中来工作。然而，在完全不受控的环境中测试时，当前方法的表现不能那么好。这是由于图像中缺少或模糊的面部特征。例如，在监控视频中的人脸识别就是这样一个案例，视频质量可能较低。

为了帮助解决这个问题，本文的作者提出了概率性人脸嵌入（PFEs）。作者提出了一种将现有确定性嵌入转换为 PFEs 的方法。最重要的是，作者指出这种方法有效地提高了人脸识别模型的性能。

**发布时间/最后更新** – 2019 年 8 月 7 日

**作者和贡献者** – Yichun Shi 和 Anil K. Jain，来自密歇根州立大学。

**[立即阅读](https://arxiv.org/pdf/1904.09658.pdf)**

### 4\. 人脸识别的难题在于噪声

在这项研究中，来自 SenseTime Research、加利福尼亚大学圣地亚哥分校和南洋理工大学的研究人员研究了噪声在大规模人脸图像数据集中的影响。许多大数据集由于其规模和成本效益的性质，容易受到标签噪声的影响。通过这篇论文，作者旨在提供有关标签噪声来源及其对人脸识别模型的影响的知识。此外，他们还计划构建并发布一个名为 IMDb-Face 的干净人脸识别数据集。

研究的两个主要目标是发现噪声对最终性能的影响，并确定标注人脸身份的最佳策略。为此，团队手动清理了两个流行的开放人脸图像数据集 MegaFace 和 MS-Celeb-1M。他们的实验显示，使用其清理后的 MegaFace 数据集的 32% 和清理后的 MS-Celeb-1M 数据集的 20% 训练的模型，性能与在原始未清理数据集上训练的模型相似。

**发布日期 / 最后更新** – 2018 年 7 月 31 日

**作者和贡献者** – Fei Wang（SenseTime）、Liren Chen（加州大学圣地亚哥分校）、Cheng Li（SenseTime）、Shiyao Huang（SenseTime）、Yanjie Chen（SenseTime）、Chen Qian（SenseTime）和 Chen Change Loy（南洋理工大学）。

**[立即阅读](https://arxiv.org/pdf/1807.11649v1.pdf)**

### 5. VGGFace2: 一个用于识别姿势和年龄变化面孔的数据集

许多研究已经对面部识别的深度卷积神经网络进行了研究。反过来，也创建了许多大规模的面部图像数据集来训练这些模型。然而，本文的作者指出，之前发布的数据集在面部姿势和年龄变化方面的数据并不多。

在本文中，牛津大学的研究人员介绍了 VGGFace2 数据集。该数据集包含了具有广泛年龄、种族、光照和姿势变化的图像。总的来说，该数据集包含 331 万张图像和 9131 个对象。

**发布日期 / 最后更新** – 2018 年 5 月 13 日

**作者和贡献者** – 牛群曹、李申、魏迪·谢、Omkar M. Parkhi 和安德鲁·齐瑟曼，来自牛津大学视觉几何组。

**[立即阅读](https://arxiv.org/pdf/1710.08092v2.pdf)**

希望上述关于面部识别的机器学习论文能帮助你加深对该领域工作的理解。

每年都有新的面部识别研究。为了跟上最新的机器学习论文和其他人工智能新闻，请 [订阅我们的新闻通讯](https://lionbridge.ai/ai-newsletter-subscription/)。有关面部识别的更多阅读，请参阅下面的相关资源。

**个人简介：[Limarc Ambalina](https://www.linkedin.com/in/limarc-ambalina-11604371/)** 是一位驻东京的作家，专注于人工智能、技术和流行文化。他为包括 Hacker Noon、Japan Today 和 Towards Data Science 在内的多个出版物撰写过文章。

[原文](https://lionbridge.ai/articles/5-machine-learning-papers-on-face-recognition/)。经授权转载。

**相关：**

+   每个数据科学家都应阅读的 5 篇 CNN 论文

+   2020 年 3 月 10 篇必读的机器学习文章

+   2018 版 50+个有用的机器学习与预测 API

### 更多相关内容

+   [用于图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [语音识别指标的演变](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [5 个需求高但不够被认可的 IT 工作](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition)

+   [KDnuggets 新闻，4 月 27 日：论文与代码的简要介绍；…](https://www.kdnuggets.com/2022/n17.html)

+   [2023 年值得阅读的顶级机器学习论文](https://www.kdnuggets.com/2023/03/top-machine-learning-papers-read-2023.html)

+   [2024 年值得阅读的 5 篇机器学习论文](https://www.kdnuggets.com/5-machine-learning-papers-to-read-in-2024)
