# 变压器的内存复杂度

> 原文：[https://www.kdnuggets.com/2022/12/memory-complexity-transformers.html](https://www.kdnuggets.com/2022/12/memory-complexity-transformers.html)

变压器模型的关键创新是引入了自注意力机制，它为输入序列中的所有位置对计算相似度分数，并且可以并行评估每个标记，避免了递归神经网络的顺序依赖，从而使变压器能够大大超越以前的序列模型如LSTM。

其他地方有很多深度解释，因此在这里我想分享一些**面试设置**中的示例问题。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

> 在一个有100万标记的书籍上运行变压器模型的问题是什么？解决这个问题的办法是什么？

![变压器的内存复杂度](../Images/80ca231462808dc45630f195ba2547af.png)

![变压器的内存复杂度](../Images/2d61db24c250ff1ddf07881a6494581f.png)

**以下是供读者参考的一些提示：**

简而言之，

> 如果你尝试在长序列上运行大型变压器模型，你会发现内存不够用。

根据谷歌研究博客（2021年）：

> 现有变压器模型及其衍生模型的一个限制是，完整的[self-attention mechanism](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html)的计算和内存需求与输入序列长度的平方成正比。使用当前常见的硬件和模型尺寸，这通常将输入序列限制在大约512个标记左右，并且阻止变压器直接应用于需要更大上下文的任务，如[问答](https://huggingface.co/tasks/question-answering)、[文档摘要](https://arxiv.org/pdf/1804.05685)或[基因组片段分类](https://www.biorxiv.org/content/10.1101/353474v3.full)。

*查看***Dr.Younes Bensouda Mourri***来自*[*Deeplearning.ai*](https://www.deeplearning.ai/courses/natural-language-processing-specialization/)*的解释：*

[查看解释！](https://www.coursera.org/lecture/attention-models-in-nlp/transformer-complexity-oGXK3)

解决变压器模型的内存复杂度问题。

> 对变换器进行的两个“改革”使其在内存和计算方面更高效：可逆层减少内存，局部敏感哈希（LSH）减少大输入大小下点积注意力的成本。

当然，还有其他解决方案，如 [扩展变换器构建](https://arxiv.org/abs/2004.08483) （ETC）等。我们将在后续文章中深入探讨更多细节！

> *快乐练习！*

**注意：**回答面试问题有不同的角度。此新闻通讯的作者并未尝试找到一个详尽回答问题的参考，而是希望分享一些快速见解，帮助读者思考、练习并在必要时进行进一步研究。

图片来源/好读物： [论文](https://arxiv.org/pdf/2112.04426.pdf)。通过 Deepmind 改进语言模型，通过数万亿个标记进行检索（2022） [博客](https://ai.googleblog.com/2021/03/constructing-transformers-for-longer.html)。通过 Google（2021）使用稀疏注意力方法构建更长序列的变换器

视频/答案来源： [自然语言处理中的注意力模型](https://www.coursera.org/learn/attention-models-in-nlp)由**Dr.Younes Bensouda Mourri** *提供，来自*[*Deeplearning.ai*](https://www.deeplearning.ai/courses/natural-language-processing-specialization/)

**[Angelina Yang](https://www.linkedin.com/in/yangyy/)** 是一位数据和机器学习高级执行官，拥有超过15年的经验，致力于提供先进的机器学习解决方案和能力，以提高金融服务和金融科技行业的业务价值。专长包括在客户体验、监控、对话式 AI、风险与合规、营销、运营、定价和数据服务领域的 AI/ML/NLP/DL 模型开发和部署。

[原文](https://medium.com/@angelina.yang/memory-complexity-with-transformers-c4e1517670b1)。经授权转载。

### 更多相关主题

+   [线性回归模型选择：平衡简洁性与复杂性](https://www.kdnuggets.com/2023/02/linear-regression-model-selection-balancing-simplicity-complexity.html)

+   [Python 中的内存分析简介](https://www.kdnuggets.com/introduction-to-memory-profiling-in-python)

+   [如何使用 Pandas 对大数据集执行内存高效操作](https://www.kdnuggets.com/how-to-perform-memory-efficient-operations-on-large-datasets-with-pandas)

+   [使用 HuggingFace Transformers 构建简单的 NLP 流水线](https://www.kdnuggets.com/2023/02/simple-nlp-pipelines-huggingface-transformers.html)

+   [比较自然语言处理技术：RNNs、变换器、BERT](https://www.kdnuggets.com/comparing-natural-language-processing-techniques-rnns-transformers-bert)

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)
