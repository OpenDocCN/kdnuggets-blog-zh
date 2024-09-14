# 学习自然语言处理的免费资源

> 原文：[`www.kdnuggets.com/2018/09/free-resources-natural-language-processing.html`](https://www.kdnuggets.com/2018/09/free-resources-natural-language-processing.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Muktabh Mayank](https://twitter.com/muktabh), [ParallelDots](https://www.paralleldots.com/)** 提供。

![](img/fb0dc19ad05061fdc63ed485a5b17f2a.png)

自然语言处理（NLP）是计算机系统理解人类语言的能力。自然语言处理是人工智能（AI）的一个子集。在线有许多资源可以帮助你在自然语言处理方面发展专长。

在这篇博客文章中，我们列出了供初学者和中级学习者使用的资源。

### **初学者的自然语言资源**

![](img/d873ca29f1691333fbfa3d861d733b30.png)

初学者可以选择两种方法，即传统机器学习和深度学习，以开始自然语言处理。这两种方法有很大的不同。对于好奇者，[这里](https://towardsdatascience.com/why-deep-learning-is-needed-over-traditional-machine-learning-1b6a99177063)是这两者之间的差异。

### **传统机器学习**

传统机器学习算法复杂，且常常不易理解。以下是一些资源，将帮助你开始学习使用机器学习进行自然语言处理：

+   由 Jurafsky 和 Martin 编写的《语音与语言处理》是传统自然语言处理的经典之作。你可以通过[这里](https://web.stanford.edu/~jurafsky/slp3/)访问它。

+   对于更实际的方法，你可以尝试[Natural Language Toolkit](http://www.nltk.org/book/)。

### **深度学习**

深度学习是机器学习的一个子领域，由于引入了人工神经网络，它比传统的机器学习方法要好得多。初学者可以从以下资源开始：

+   CS 224n：这是使用深度学习进行自然语言处理的最佳课程。该课程由斯坦福大学提供，访问[这里](http://web.stanford.edu/class/cs224n/)。

+   Yoav Golberg 的免费和付费书籍是开始进行自然语言处理深度学习的优秀资源。免费版本可以通过[这里](https://u.cs.biu.ac.il/~yogo/nnlp.pdf)访问，完整版可通过[这里](https://www.amazon.com/Language-Processing-Synthesis-Lectures-Technologies/dp/1627052984)获取。

+   Jacob Einsenstein 在 GATECH 的 NLP 课程中的笔记对所有算法进行了非常全面的覆盖，涉及几乎所有的 NLP 方法。你可以在 GitHub 上通过[这里](https://github.com/jacobeisenstein/gt-nlp-class/blob/master/notes/eisenstein-nlp-notes.pdf)访问这些笔记。

### **自然语言资源供从业者使用**

![](img/0509064d98e73366d835bb6b6c84c51e.png)

如果你是一名数据科学家，你将需要三种类型的资源：

1.  快速入门指南 / 了解最新热点

1.  针对特定问题的方法综述

1.  定期关注的博客

### 快速入门指南 / 了解最新热点

+   可以从 Otter 等人的《自然语言处理的深度学习》综述开始。你可以在 [这里](https://arxiv.org/abs/1807.10854) 访问这篇综述。

+   Young 等人的综述论文尝试总结深度学习自然语言处理中的所有前沿技术，推荐给实践者作为自然语言处理的入门读物。你可以在 [这里](https://arxiv.org/abs/1708.02709) 访问这篇论文。

+   你可以参考 [这篇](https://arxiv.org/abs/1808.03314)文章，以理解 LSTM 和 RNN 的基础知识，这些技术在自然语言处理（NLP）中被广泛使用。另一篇更常被引用（且声誉极高）的 LSTM 综述可以在 [这里](https://arxiv.org/abs/1503.04069)找到。这是一篇有趣的论文，可以帮助理解 RNN 的隐藏状态如何工作。阅读起来很愉快，可以在 [这里](https://github.com/locuslab/TCN) 访问。我总是推荐以下两个博客文章给任何还没有读过的人：

1.  http://colah.github.io/posts/2015-08-Understanding-LSTMs

1.  https://distill.pub/2016/augmented-rnns/

+   卷积神经网络（Convnets）可以用于理解自然语言。你可以通过阅读这篇论文 [这里](https://arxiv.org/abs/1801.06287) 来可视化 Convnets 在 NLP 中的工作原理。

+   Convnets 和 RNN 的比较在 [这篇](https://arxiv.org/abs/1803.01271) 由 Bai 等人撰写的论文中得到了强调。所有的 pytorch 代码（我已经停止或大幅减少阅读非 pytorch 编写的深度学习代码）在 [这里](https://github.com/locuslab/TCN) 开源，并且让你感受到哥斯拉对战金刚或福特野马对比雪佛兰 Camaro（如果你喜欢那种东西）。谁将赢得胜利！

### 针对特定问题的方法综述

实践者需要的另一类资源是对以下类型问题的解答：“我需要训练一个算法来做 X，我可以应用什么最酷（且易于访问）的东西？”。

你需要以下内容：

#### **文本分类**

人们首先解决的问题是什么？主要是文本分类。文本分类可以是将文本分类到不同的类别中，或检测文本中的情感/情绪。

我想强调一个关于不同情感分析综述的简单阅读，描述在这个 ParallelDots [博客](https://blog.paralleldots.com/data-science/breakthrough-research-papers-and-models-for-sentiment-analysis/) 中。尽管这篇综述是关于情感分析技术的，但它可以扩展到大多数文本分类问题。

我们（ParallelDots）的调查问卷技术性稍弱，旨在引导您到有趣的资源以理解一个概念。我推荐给您的 Arxiv 调查论文将非常技术性，需要您阅读其他重要论文以深入理解一个主题。我们建议的方式是通过我们的链接来熟悉并乐在其中，但要确保阅读我们推荐的详细指南。（奥克利博士的[课程](https://www.coursera.org/learn/learning-how-to-learn)讲解了块化学习，即您首先尝试从各处获取小块信息，然后再深入学习）。记住，虽然有趣很重要，但除非详细理解技术，否则在新情况中应用概念将会很困难。

另一个情感分析算法的调查（由 Linked University 和 UIUC 的人员编写）请见[这里](https://arxiv.org/abs/1801.07883)。

迁移学习革命已经影响了深度学习。就像在图像中，一个在 ImageNet 分类上训练的模型可以针对任何分类任务进行微调一样，针对维基百科的语言建模训练的 NLP 模型现在可以在相对较少的数据上进行文本分类。这里有两篇来自 OpenAI 和 Ruder 及 Howard 的论文，处理了这些技术。

1.  [`arxiv.org/abs/1801.06146`](https://arxiv.org/abs/1801.06146)

1.  [`s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf`](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)

Fast.ai 提供了更友好的文档来应用这些方法，详见[这里](http://nlp.fast.ai/classification/2018/05/15/introducting-ulmfit.html)。

如果您正在进行两个不同任务的迁移学习（不是从维基百科语言建模任务迁移），有关使用 Convnets 的技巧请参见[这里](https://arxiv.org/abs/1801.06480)。

恕我直言，这种方法将会慢慢取代所有其他分类方法（这是从视觉领域发生的情况进行的简单推测）。我们还发布了关于[零样本文本分类](https://arxiv.org/abs/1712.05972)的研究，能够在没有数据集训练的情况下获得良好的准确性，并且正在开发其下一代。我们建立了一个自定义文本分类 API，通常称为 Custom Classifier，您可以定义自己的类别。您可以在[这里](https://www.paralleldots.com/custom-classifier)查看免费版。

#### SEQUENCE LABELING

序列标注是一个为单词标注不同属性的任务。这些属性包括词性标注、命名实体识别、关键词标注等。

我们对类似任务的方法进行了有趣的回顾，详见[这里](https://blog.paralleldots.com/data-science/named-entity-recognition-milestone-models-papers-and-technologies/)。

针对这些问题的一个优秀资源是今年 COLING 的研究论文，它提供了训练序列标注算法的最佳指南。你可以在[这里](https://arxiv.org/abs/1806.04470)获取。

#### 机器翻译

+   最近自然语言处理领域最大的进展之一是发现了能够将文本从一种语言翻译到另一种语言的算法。Google 的系统是一个令人惊叹的 16 层 LSTM（因为他们有大量的数据进行训练，所以不需要丢弃）并且提供了最先进的翻译结果。

媒体专家夸大了这一炒作，发布了虚假报道声称“Facebook 不得不关闭发明了自己语言的 AI”。以下是一些相关报道。

1.  [`gadgets.ndtv.com/social-networking/news/facebook-shuts-ai-system-after-bots-create-own-language-1731309`](https://gadgets.ndtv.com/social-networking/news/facebook-shuts-ai-system-after-bots-create-own-language-1731309)

1.  [`www.forbes.com/sites/tonybradley/2017/07/31/facebook-ai-creates-its-own-language-in-creepy-preview-of-our-potential-future/#1d1ca041292c`](https://www.forbes.com/sites/tonybradley/2017/07/31/facebook-ai-creates-its-own-language-in-creepy-preview-of-our-potential-future/#1d1ca041292c)

+   关于机器翻译的详细教程，请参考 Philip Koehn 的研究论文[在这里](https://arxiv.org/abs/1709.07809)。使用深度学习进行机器翻译（我们称之为 NMT 或神经机器翻译）的具体综述在[这里](https://arxiv.org/abs/1707.07631)。

我的一些最喜欢的论文在这里—

+   这篇[Google 的论文](https://arxiv.org/abs/1609.08144)告诉你当你有大量的钱和数据时，如何解决一个问题。

+   Facebook 的[卷积神经机器翻译系统](https://arxiv.org/abs/1705.03122)（仅仅因为其酷炫的卷积方法）及其代码作为库被发布[在这里](https://github.com/facebookresearch/fairseq)。

+   [`marian-nmt.github.io/`](https://marian-nmt.github.io/)是一个用于快速翻译的 C++框架[`www.aclweb.org/anthology/P18-4020`](http://www.aclweb.org/anthology/P18-4020)

+   [`opennmt.net/`](http://opennmt.net/)使每个人都能训练他们的 NMT 系统。

#### 问答系统

在我看来，这将是下一个“机器翻译”。问答任务有很多不同的类型。从选项中选择，基于段落或知识图谱选择答案，或基于图像回答问题（也称为视觉问答），有不同的数据集可以了解最前沿的方法。

+   [SQuAD 数据集](https://rajpurkar.github.io/SQuAD-explorer/)是一个测试算法阅读理解和回答问题能力的问答数据集。微软今年早些时候发布了一篇论文，声称他们已达到该[任务](https://blogs.microsoft.com/ai/microsoft-creates-ai-can-read-document-answer-questions-well-person/)的人类水平准确性。论文可以在[这里](https://www.ailab.microsoft.com/experiments/ef90706b-e822-4686-bbc4-94fd0bca5fc5)找到。另一个重要的算法（我觉得非常酷）是 Allen AI 的[BIDAF](https://allenai.github.io/bi-att-flow/)及其改进。

+   另一个重要的算法集是视觉问答，它回答关于图像的问题。Teney 等人的[论文](https://allenai.github.io/bi-att-flow/)来自 VQA 2017 挑战赛，是一个很好的入门资源。你还可以在 Github 上找到它的实现[这里](https://github.com/markdtw/vqa-winner-cvprw-2017)。

+   对大文档进行抽取式问答（类似于谷歌如何在前几条结果中突出显示对查询的回答）在现实生活中可以使用迁移学习（因此只需少量标注），如这篇 ETH 论文中所示[这里](https://arxiv.org/abs/1804.07097)。一篇非常好的论文批评了问答算法的“理解”，请参阅[这里](https://arxiv.org/abs/1808.04926)。如果你在这个领域工作，必须阅读。

#### 同义句生成、句子相似性或推理

句子比较任务。NLP 有三个不同的任务：句子相似性、同义句检测和自然语言推理（NLI），每个任务对语义理解的要求逐渐提高。[MultiNLI](https://www.nyu.edu/projects/bowman/multinli/)及其子集斯坦福 NLI 是最知名的 NLI 基准数据集，最近成为了研究的重点。还有 MS 同义句语料库和 Quora 语料库用于同义句检测，以及 SemEval 数据集用于 STS（语义文本相似性）。有关该领域高级模型的良好调查可以在[这里](https://arxiv.org/abs/1806.04330)找到。在临床领域应用 NLI 非常重要。（查找正确的医疗程序、药物的副作用及交叉效应等）。如果你希望将技术应用于特定领域，[这篇教程](https://arxiv.org/abs/1808.06752)是一个不错的阅读选择。

这里是我在这个领域中最喜欢的论文列表

+   自然语言推理在交互空间中的应用 – 它展示了一种非常巧妙的方法，将 DenseNet（用于句子表示的卷积神经网络）应用于此。这个成果还是一个实习项目的结果，这让它更加酷炫！你可以在[这里](https://arxiv.org/pdf/1709.04348.pdf)阅读这篇论文。

+   Omar Levy 小组的这篇研究[论文](https://homes.cs.washington.edu/~roysch/papers/artifacts/artifacts_poster.pdf)表明，即使是简单的算法也能完成任务。这是因为算法仍然没有学习“推理”。

+   BiMPM 是一个很酷的模型，用于预测同义句，可以在[这里](https://arxiv.org/abs/1702.03814)访问。

+   我们也有一项关于同义句检测的新工作，它将关系网络应用于句子表示，并且已被今年的 AINL 会议接受。你可以在[这里](https://peerj.com/preprints/26847)阅读。

#### 其他领域

这里有一些更详细的调查论文，可以获取有关你可能在构建 NLP 系统时遇到的其他任务的研究信息。

+   **语言建模（LM） –** 语言建模的任务是学习语言的无监督表示。通过预测给定前 N 个单词后第 (n+1) 个单词来完成。这些模型有两个重要的实际应用，自动补全和作为文本分类转移学习的基础模型，如前所述。详细调查可以在[这里](https://arxiv.org/abs/1708.07252)找到。如果你有兴趣了解基于搜索历史的 LSTM 如何在手机/搜索引擎中进行自动补全，[这里](https://arxiv.org/abs/1804.09661)有一篇很酷的论文值得阅读。

+   **关系抽取 –** 关系抽取的任务是提取句子中存在的实体之间的关系。给定句子“A 与 B 的关系为 r”，给出三元组 (A, r, B)。该领域的研究工作调查可以在[这里](https://arxiv.org/abs/1712.05191)找到。[这里](https://arxiv.org/abs/1706.04115)是我发现非常有趣的一篇研究论文。它使用 BIDAFs 进行零样本关系抽取（即，它可以识别它未曾接受训练的关系）。

+   **对话系统 –** 随着聊天机器人革命的兴起，对话系统现在非常流行。许多人（包括我们）将对话系统构建为意图检测、关键词检测、问答等模型的组合，而其他人则尝试端到端建模。JD.com 团队对对话系统模型的详细调查可以在[这里](https://arxiv.org/abs/1711.01731)找到。我还想提到 Facebook AI 提供的框架 Parl.ai。

+   **文本摘要 –** 文本摘要用于从文档（段落/新闻文章等）中获取简洁的文本。有两种方法：提取式摘要和抽象式摘要。提取式摘要提供了信息含量最高的句子（这种方法已经存在几十年），而抽象式摘要则旨在像人类一样撰写摘要。这款[演示](https://www.salesforce.com/products/einstein/ai-research/tl-dr-reinforced-model-abstractive-summarization/)来自 Einstein AI，将抽象式摘要引入主流研究。有关技术的详细调查报告[这里](https://arxiv.org/abs/1804.04589)。

+   **自然语言生成 (NLG) –** 自然语言生成是计算机旨在像人类一样写作的研究。这可能包括故事、诗歌、图像描述等。在这些方面，当前研究在图像描述上表现很好，LSTM 和注意力机制的结合已经提供了可以实际使用的输出。有关技术的调查报告[这里](https://arxiv.org/abs/1703.09902)。

### 推荐关注的博客

这里有一份博客推荐列表，适合任何对跟踪 NLP 研究新动态感兴趣的人。

Einstein AI – [ https://einstein.ai/research](https://einstein.ai/research)

Google AI 博客 – [`ai.googleblog.com/`](https://ai.googleblog.com/)

WildML – [`www.wildml.com/`](http://www.wildml.com/)

DistillPub – [`distill.pub/`](https://distill.pub/) (distillpub 是独特的，既是博客也是出版物)

Sebastian Ruder – [`ruder.io/`](http://ruder.io/)

如果你喜欢这篇文章，必须关注我们的博客。我们经常在这里提供资源列表。

就这些了！享受让神经网络理解语言的乐趣。你还可以尝试基于自然语言处理的文本分析 API，[这里](https://www.paralleldots.com/text-analysis-apis)。

你还可以阅读有关机器学习算法的文章，了解成为数据科学家应掌握的算法[这里](https://blog.paralleldots.com/data-science/machine-learning/ten-machine-learning-algorithms-know-become-data-scientist/)。

你还可以[这里](https://www.paralleldots.com/text-analysis-apis)查看 ParallelDots AI API 的免费演示。

[原创](https://blog.paralleldots.com/data-science/nlp/free-natural-language-processing-resources/)。已获授权转载。

**简介**: [Muktabh Mayank](https://twitter.com/muktabh) 是 [ParallelDots](https://www.paralleldots.com/) 的联合创始人，该公司致力于“简化每个人的机器学习”。Muktabh 是数据科学家、企业家、社会学家，不是传统的书呆子。

**相关链接：**

+   [“Someone Like You” 的数据科学或阿黛尔歌曲的情感分析](https://www.kdnuggets.com/2018/09/sentiment-analysis-adele-songs.html)

+   [使用 Python 中的 SpaCy 进行文本分类的机器学习](https://www.kdnuggets.com/2018/09/machine-learning-text-classification-using-spacy-python.html)

+   [自然语言处理的深度学习：近期趋势概述](https://www.kdnuggets.com/2018/09/deep-learning-nlp-overview-recent-trends.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 相关主题

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [2023 年值得阅读的 5 本自然语言处理免费书籍](https://www.kdnuggets.com/2023/06/5-free-books-natural-language-processing-read-2023.html)

+   [掌握 SQL、Python、数据科学、机器学习和自然语言处理的 25 本免费书籍](https://www.kdnuggets.com/25-free-books-to-master-sql-python-data-science-machine-learning-and-natural-language-processing)

+   [掌握自然语言处理的 5 门免费课程](https://www.kdnuggets.com/5-free-courses-to-master-natural-language-processing)

+   [图像识别和自然语言处理的迁移学习](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html)

+   [如何使用 PyTorch 开始自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)
