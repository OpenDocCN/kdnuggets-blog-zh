# Phi-2: 小型语言模型正在做大事

> 原文：[https://www.kdnuggets.com/phi-2-small-lms-that-are-doing-big-things](https://www.kdnuggets.com/phi-2-small-lms-that-are-doing-big-things)

![Phi-2: 小型语言模型正在做大事](../Images/13e6e7354ab345f92ec5f0c6bdeee03d.png)

作者提供的图片

在我们深入了解Phi-2的惊人之处之前。如果你还没有了解phi-1.5，我建议你快速浏览一下微软几个月前的[有效的小型语言模型：微软的13亿参数phi-1.5](/effective-small-language-models-microsoft-phi-15)。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

现在你已经了解了基础，我们可以继续学习更多关于Phi-2的内容。微软一直在努力发布一系列名为“Phi”的小型语言模型（SLMs）。这系列模型已被证明能够取得显著的性能，就像大型语言模型一样。

微软的第一个模型是[Phi-1](https://huggingface.co/microsoft/phi-1)，具有13亿参数，然后是[Phi-1.5](https://huggingface.co/microsoft/phi-1_5)。

我们已经看到了Phi-1、Phi-1.5，现在我们有了[Phi-2](https://www.microsoft.com/en-us/research/blog/phi-2-the-surprising-power-of-small-language-models/)。

# 什么是Phi-2？

Phi-2变得更大、更好。更大，更好。它是一个27亿参数的语言模型，已被证明在推理和语言理解能力上表现出色。

对于如此小的语言模型，这真是惊人，不是吗？

Phi-2已被证明在性能上超越了大25倍的模型。这都归功于模型的扩展和训练数据的策划。小巧、紧凑且性能卓越。由于其规模，Phi-2适用于研究人员探索解释能力、微调实验以及深入安全改进。它可以在Azure AI Studio模型目录中获得。

## Phi-2的创建

微软的训练数据是合成数据集的混合，这些数据集用于教授模型常识，例如一般知识、科学、心智理论和日常活动。

训练数据经过精心挑选，以确保其经过优质内容的筛选，并具有教育价值。凭借这种可扩展性，他们将1.3亿参数的Phi-1.5模型提升到了2.7亿参数的Phi-2。

![Phi-2: 小型语言模型正在做大事](../Images/0c662b6c043f0225c74375a313f5d649.png)

图片来源：[微软Phi-2](https://www.microsoft.com/en-us/research/uploads/prod/2023/12/figure2_phi_comp-2048x474.png)

微软对Phi-2进行了测试，因为他们意识到当前模型评估的挑战。他们在测试用例中将Phi-2与Mistral和Llama-2进行了比较。结果显示，Phi-2在某些情况下超越了Mistral-7B，而70亿参数的Llama-2模型在某些情况下超越了Phi-2，如下所示：

![Phi-2: 小型语言模型的巨大潜力](../Images/b3de725465ff1a06288ca9256c9c0094.png)

图片来源：[微软Phi-2](https://www.microsoft.com/en-us/research/uploads/prod/2023/12/figure2_phi_comp-2048x474.png)

# Phi-2的局限性

不过，尽管如此，Phi-2仍然有其局限性。例如：

+   不准确性：该模型在生成错误代码和事实方面存在一些局限，用户应对此保持谨慎，将这些输出视为起点。

+   限制的代码知识：Phi-2的训练数据基于Python及常见包，因此生成其他语言和脚本的结果需要进行验证。

+   指令：该模型尚未经过指令微调，因此可能难以真正理解用户提供的指令。

Phi-2还有其他局限性，例如语言限制、社会偏见、毒性和冗长。

尽管如此，每个新产品或服务都有其局限性，而Phi-2仅发布了一周左右。因此，微软需要将Phi-2推广到公众手中，以帮助改进服务并克服当前的局限性。

# 总结

微软以一个小型语言模型结束了这一年，这个模型可能会成为2024年最受关注的模型。既然如此，我们应该对2024年的语言模型世界有什么期待呢？

[](https://www.linkedin.com/in/nisha-arya-ahmed/)****[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)**** 是一位数据科学家、自由职业技术作家，以及KDnuggets的编辑和社区经理。她特别关注提供数据科学职业建议或教程，以及围绕数据科学的理论知识。Nisha涵盖了广泛的话题，并希望探索人工智能如何有利于人类生命的长寿。作为一个热衷学习者，Nisha寻求扩展她的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [在使用神经网络之前尝试的10件简单事](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [将数据科学家与其他职业区分开的5件事](https://www.kdnuggets.com/2021/11/5-things-set-data-scientist-apart-other-professions.html)

+   [数据管理的6件事及其重要性](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [你可以用 R 做的 5 件令人惊讶的事情](https://www.kdnuggets.com/2022/08/5-surprising-things-r.html)

+   [你不知道的 7 件低代码工具的用法](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [事物并非总是正常的：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)
