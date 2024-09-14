# NLP 初学者的研究论文

> 原文：[`www.kdnuggets.com/2022/11/research-papers-nlp-beginners.html`](https://www.kdnuggets.com/2022/11/research-papers-nlp-beginners.html)

![NLP 初学者的研究论文](img/06c3080b57d4b24c0cd05a468f8c5022.png)

[Sincerely Media](https://unsplash.com/@sincerelymedia) 通过 Unsplash

如果你是数据世界的新手，并对 NLP（自然语言处理）有特别的兴趣，你可能在寻找资源以帮助你更好地理解。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

你可能已经遇到过许多不同的研究论文，并对选择哪一篇感到困惑。因为说实话，它们并不短，而且确实需要大量的脑力。因此，选择一篇对你掌握 NLP 有帮助的论文将是明智的。

我做了一些研究，并收集了一些在 NLP 领域高度推荐的研究论文，以供初学者和全面的 NLP 知识参考。

我会将其拆分为几个部分，以便你可以找到你想要的内容。

# 机器学习与 NLP

**[利用 EM 从标记和未标记文档中进行文本分类](https://www.ri.cmu.edu/pub_files/pub1/nigam_k_1999_1/nigam_k_1999_1.pdf) 作者：Kamal Nigam，1999 年**

本论文讲述了如何通过将少量标记的训练文档与大量未标记的文档结合来提高文本分类器的准确性。

**[超越准确性：使用 CheckList 对 NLP 模型进行行为测试](https://aclanthology.org/2020.acl-main.442.pdf) 作者：Marco Tulio Ribeiro 等，2020 年**

在这篇论文中，你将更多地了解 CheckList，一种测试 NLP 模型的任务无关方法，因为不幸的是，一些目前最常用的方法高估了 NLP 模型的性能。

# 神经模型

**[自然语言处理（几乎）从零开始](https://www.jmlr.org/papers/volume12/collobert11a/collobert11a.pdf) 作者：Ronan Collobert，2011 年**

在这篇论文中，你将学习 NLP 的基础知识——正如标题所示，它几乎是从零开始的。主题包括命名实体识别、语义角色标注、网络、训练等。

**[理解 LSTM 网络](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) 作者：Christopher Olah，2015 年**

神经网络是 NLP 的重要组成部分，因此对其有良好的理解将对你长期受益。这篇论文重点关注广泛使用的 LSTM 网络。

# 单词/句子表示和嵌入

**[词汇和短语的分布式表示及其组合性](https://proceedings.neurips.cc/paper/2013/file/9aa42b31882ec039965f3c4923ce901b-Paper.pdf)** 由 Tomas Mikolov 发表，2013 年

由 Mikolov 撰写，他引入了 Skip-gram 模型，用于从大量非结构化文本数据中学习高质量的单词向量表示——这篇论文将介绍原始 Skip-gram 模型的几个扩展。

**[句子和文档的分布式表示](https://cs.stanford.edu/~quocle/paragraph_vector.pdf)** 由 Quoc Le 和 Tomas Mikolov 发表，2014 年

深入探讨了袋词模型的两个主要弱点，作者介绍了段落向量——这是一种无监督算法，用于从可变长度的文本片段（如句子）中学习固定长度的特征表示。

# 语言建模

**[语言模型是无监督的多任务学习者](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)** 由 Alec Radford 发表，2018 年

自然语言处理任务通常通过在特定任务的数据集上进行监督学习来解决。然而，多任务学习被测试作为一种有前途的框架，以提高 NLP 的整体性能。

**[递归神经网络的非凡有效性](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)** 由 Andrej Karpathy 发表，2015 年

这篇论文回溯了递归神经网络的起点，并探讨了它们为何如此有效和稳健，并附有代码示例以帮助你更好地理解。

# 注意力与 Transformer

**[BERT：深度双向 Transformer 的语言理解预训练](https://arxiv.org/pdf/1810.04805.pdf)** 由 Jacob Devlin 等人 发表，2019 年

在学习机器学习时，你可能听说过 BERT——来自 Transformer 的双向编码表示。它被广泛使用，并以能够从未标记的文本中进行深度双向表示预训练而著称。在这篇论文中，你将进一步理解并学习如何基于 BERT 改进你的微调。

**[注意力机制才是关键](https://arxiv.org/pdf/1706.03762.pdf)** 由 Ashish Vaswani 等人 发表，2017 年

这篇论文专注于 Transformer，完全关注注意力机制，这与通常基于复杂递归或卷积神经网络的模型不同。你将学习 Transformer 如何很好地泛化到其他任务，并可能是更好的选择。

**[HuggingFace 的 Transformers：最先进的自然语言处理](https://arxiv.org/pdf/1910.03771.pdf)** 由 Thomas Wolf 等人 发表，2020 年

想了解更多关于已经成为自然语言处理主流架构的 Transformer 吗？在这篇论文中，你将深入了解它的架构以及它如何促进预训练模型的分发。

# 总结

正如我上面所说，我不想用这么多不同的研究论文让你感到困扰——因此我保持了最小化的水平。

如果你知道任何适合初学者的资源，请在评论中分享，以便他们可以看到。谢谢！

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由技术作家。她特别关注提供数据科学职业建议或教程以及数据科学的理论知识。她还希望探索人工智能如何/能如何有助于人类寿命的延续。作为一个热衷于学习的人，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关话题

+   [过去 12 个月的必读 NLP 论文](https://www.kdnuggets.com/2023/03/must-read-nlp-papers-last-12-months.html)

+   [8 篇创新的 BERT 知识蒸馏论文，这些论文改变了…](https://www.kdnuggets.com/2022/09/eight-innovative-bert-knowledge-distillation-papers-changed-nlp-landscape.html)

+   [你应该阅读的生成代理研究论文](https://www.kdnuggets.com/generative-agent-research-papers-you-should-read)

+   [初学者到专业人士的前 5 名 NLP 备忘单](https://www.kdnuggets.com/2022/12/top-5-nlp-cheat-sheets-beginners-professional.html)

+   [带代码的论文简要介绍](https://www.kdnuggets.com/2022/04/brief-introduction-papers-code.html)

+   [KDnuggets 新闻，4 月 27 日：带代码的论文简要介绍；…](https://www.kdnuggets.com/2022/n17.html)
