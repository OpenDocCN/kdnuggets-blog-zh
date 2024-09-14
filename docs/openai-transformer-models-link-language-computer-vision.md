# OpenAI 发布了两种神奇地将语言与计算机视觉联系起来的变换器模型

> 原文：[https://www.kdnuggets.com/2021/01/openai-transformer-models-link-language-computer-vision.html](https://www.kdnuggets.com/2021/01/openai-transformer-models-link-language-computer-vision.html)

[评论](#comments) ![图示](../Images/96196e8b2230d9c485205752ca1ef5d3.png)

来源: [https://www.rev.com/blog/what-is-gpt-3-the-new-openai-language-model](https://www.rev.com/blog/what-is-gpt-3-the-new-openai-language-model)

> * * *
> 
> ## 我们的三大课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT
> 
> * * *
> 
> 我最近开始了一份新的专注于 AI 教育的通讯，**已经拥有超过 50,000 名订阅者**。TheSequence 是一份无废话（即无炒作、无新闻等）的 AI 专注通讯，阅读时间为 5 分钟。其目标是让你了解机器学习项目、研究论文和概念。请通过下面的订阅链接试试：

[![图片](../Images/f2aed90f956dea213be7c9bbf9cd7072.png)](https://thesequence.substack.com/)

变换器被广泛认为是过去十年机器学习的最大突破之一，而OpenAI一直处于这一领域的中心。OpenAI 的 GPT-3 可以说是最著名且争议最大的机器学习模型之一。GPT-3 训练有数十亿个参数，被数百家公司积极使用，以自动化各种语言任务，如问答、文本生成等。如此成功，自然 OpenAI 继续探索 GPT-3 和变换器模型的不同变种。几天前，我们看到了一种变种，当时 OpenAI 发布了两种新的变换器架构，这些架构以有趣且几乎神奇的方式将图像和语言任务结合在一起。

等等，我刚才说变换器被用在计算机视觉任务中？没错！虽然自然语言理解（NLU）仍然是变换器模型的最大战场，但将这些架构适应于计算机视觉领域已经取得了惊人的进展。OpenAI在这一领域的首次亮相有两个模型：

1.  **CLIP：** 使用变换器从语言监督中有效地学习视觉概念。

1.  **DALL·E：** 使用变换器从文本说明生成图像。

### CLIP

通过 CLIP，OpenAI 试图解决一些计算机视觉模型的显著挑战。首先，为计算机视觉构建训练数据集非常具有挑战性且昂贵。虽然语言模型可以在像维基百科这样的广泛可用的数据集上进行训练，但计算机视觉领域却没有类似的资源。其次，大多数计算机视觉模型高度专业化于单一任务，且很少能适应新任务。

CLIP 是一种基于图像数据集并使用语言监督的变换器架构。CLIP 使用图像编码器和文本解码器来预测哪些图像与数据集中给定的文本配对。这一行为随后用于训练一个零样本分类器，可以适应多个图像分类任务。

![图](../Images/34f4971f6252af8ebbcdc916865cb70f.png)

来源: [https://openai.com/blog/clip/](https://openai.com/blog/clip/)

这是一个能够学习复杂视觉概念同时保持高效性能的模型。零样本方法使得 CLIP 能够在不进行重大调整的情况下适应不同的数据集。

![图](../Images/b5a61f8e32459f622f8b5d646a1cc6c2.png)

来源: [https://openai.com/blog/clip/](https://openai.com/blog/clip/)

### **DALL·E**

OpenAI 的 DALL·E 是一个基于 GPT-3 的模型，能够从文本描述生成图像。其概念是结合变换器和生成模型，以适应复杂的图像生成场景。

DALL·E 接收文本和图像作为输入数据集，其中包含约 1280 个标记（256 个文本标记和 1024 个图像标记）。该模型基于简单的解码器架构，训练时使用最大似然生成所有标记，逐个生成。DALL·E 还包括一个注意力掩码，用于关联文本和图像。

变换器架构的使用产生了一个能够从高度复杂的句子生成图像的生成模型。请查看下面的一些示例。

![图](../Images/c8670d47e9a8b99abcdbee3b85d0d99f.png)

来源: [https://openai.com/blog/dall-e/#fn1](https://openai.com/blog/dall-e/#fn1)

DALL·E 和 CLIP 都代表了多任务变换器模型的重要进展，毫无疑问是计算机视觉领域的重要里程碑。我们很可能很快会看到这些模型的重大应用。

[原文](https://jrodthoughts.medium.com/openai-releases-two-transformer-models-that-magically-link-language-and-computer-vision-d755a83843a3). 经许可转载。

**相关:**

+   [如何将表格数据与 HuggingFace Transformers 结合](/2020/11/tabular-data-huggingface-transformers.html)

+   [计算走向极致: 回顾 Sutton 对 AI 的痛苦教训](/2020/11/revisiting-sutton-bitter-lesson-ai.html)

+   [数据科学家必读的NLP和深度学习文章](/2020/08/must-read-nlp-deep-learning-articles.html)

### 更多相关话题

+   [DINOv2: Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [用噪声标记的数据微调OpenAI语言模型](https://www.kdnuggets.com/2023/04/finetuning-openai-language-models-noisily-labeled-data.html)

+   [你需要了解的6个数据管理要点及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [TensorFlow在计算机视觉中的应用 - 迁移学习简化版](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍MLM的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的5个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)
