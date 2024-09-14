# 使用 Google MusicLM 从文本生成音乐

> 原文：[https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

![使用 Google MusicLM 从文本生成音乐](../Images/2b81acd0d8e64abebbd99827c62ad992.png)

图片来源于 [Freepik](https://www.freepik.com/free-photo/hearing-issues-collage-design_33535939.htm#page=2&query=artificial%20intelligence%20music&position=4&from_view=search&track=ais)

人工智能的发展比以往任何时候都更加迅猛，尤其是在生成 AI 领域。从生成类似于与人对话的文本到从文本生成图像，现在这一切都变得可能了。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 方面

* * *

这种进步也进入了音乐生成领域，谷歌推出了一种名为 MusicLM 的音乐生成模型。该模型于 2023 年 1 月发布，自那时以来，人们一直在尝试其功能。那么，MusicLM 的详细情况是什么？您如何尝试它？让我们深入讨论。

# Google MusicLM

MusicLM 首次在[Agostinelli 等人 (2023)](https://arxiv.org/pdf/2301.11325.pdf)的论文中介绍，研究小组将 MusicLM 解释为一个从文本描述中生成高保真音乐的模型。该模型通常建立在[AudioLM](https://arxiv.org/pdf/2209.03143.pdf)的基础上，实验表明，该模型可以生成多分钟高质量的音乐，采样率为 24 kHz，同时仍能遵循文本描述。

此外，研究还生成了公开的文本到音乐数据集 [musicaps](https://www.kaggle.com/datasets/googleai/musiccaps)，供任何希望开发类似模型或扩展研究的人使用。数据由专业音乐家手动策划和挑选。

此外，MusicLM 的开发遵循了负责任的模型开发实践，以防对创意内容可能的不当使用感到担忧。通过扩展 [Carlini 等人 (2022)](https://arxiv.org/abs/2202.07646) 的工作，MusicLM 生成的 token 与训练数据有显著不同。

# 尝试 MusicLM

如果你想深入了解 MusicLM 的结果样本，Google 研究小组提供了一个简单的[网站](https://google-research.github.io/seanet/musiclm/examples/)来展示 MusicLM 的能力。例如，你可以在网站上探索从文本标题生成的音频样本。

![使用 Google MusicLM 从文本生成音乐](../Images/6fc6a122a08f95a529aa6eeea9a991fa.png)

图片由作者提供（改编自 [google-research.github.io](https://google-research.github.io/seanet/musiclm/examples/)）

另一个例子是我最喜欢的样本，故事模式音乐生成，通过使用多个文本提示将不同风格的音乐整合成一个整体。

![使用 Google MusicLM 从文本生成音乐](../Images/95140ddb3fc180684542f8172da5b9c1.png)

图片由作者提供（改编自 [google-research.github.io](https://google-research.github.io/seanet/musiclm/examples/)）

也可以根据绘画标题生成音乐，可能会捕捉到图像的情绪。

![使用 Google MusicLM 从文本生成音乐](../Images/65d28e977fec776b86934f4fb6ffa075.png)

图片由作者提供（改编自 [google-research.github.io](https://google-research.github.io/seanet/musiclm/examples/)）

结果听起来很棒，但我们如何尝试这个模型？幸运的是，自 2023 年 5 月以来，Google 已经接受了在 [AI Test Kitchen](https://g.co/aitestkitchen) 测试 MusicLM 的注册。前往网站，使用你的 Google 帐号进行注册。

![使用 Google MusicLM 从文本生成音乐](../Images/64efc580849c6d5cfb07c4f0e65e5644.png)

图片由作者提供（改编自 [aitestkitchen](https://aitestkitchen.withgoogle.com/experiments/music-lm)）

注册后，我们需要等待轮到我们试用 MusicLM 的机会。因此，请留意你的电子邮件。

![使用 Google MusicLM 从文本生成音乐](../Images/f7426baa88b40b0a05e8e822299caf3d.png)

图片由作者提供（改编自 [aitestkitchen](https://aitestkitchen.withgoogle.com/experiments/music-lm)）

现在就这些；希望你能尽快轮到你试用令人兴奋的 MusicLM。

# 结论

MusicLM 是 Google 研究小组开发的一种模型，用于从文本生成音乐。该模型可以根据文本指令提供几分钟的高质量音乐。我们可以通过注册 [AI Test Kitchen](https://g.co/aitestkitchen) 试用 MusicLM。如果仅对样本结果感兴趣，我们可以访问 Google 研究[网站](https://google-research.github.io/seanet/musiclm/examples/)。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。

### 更多相关信息

+   [使用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用稳定扩散生成超逼真的人脸的3种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [结合数据管理与数据讲述生成价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)

+   [使用BERT对长文本文档进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)
