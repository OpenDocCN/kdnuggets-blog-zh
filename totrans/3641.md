# 使用 BERT 的抽取式摘要

> 原文：[https://www.kdnuggets.com/extractive-summarization-with-llm-using-bert](https://www.kdnuggets.com/extractive-summarization-with-llm-using-bert)

![使用 BERT 的抽取式摘要](../Images/c1bd380b5cea926954e185cf7890779f.png)

图片由编辑提供

# 引言

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

在当今快节奏的世界中，我们被大量信息轰炸。我们越来越习惯于在更短的时间内接收更多信息，当需要阅读大量文档或书籍时，这会导致沮丧。此时，抽取式摘要就发挥了作用。这个过程从文章、片段或页面中提取关键句子，为我们提供其最重要点的快照。

对于任何需要在不逐字阅读每一个单词的情况下理解大型文档的人来说，这是一项颠覆性的改变。

在这篇文章中，我们将深入探讨抽取式摘要的基础和应用。我们将审视大型语言模型，特别是 [BERT](https://www.exxactcorp.com/blog/Deep-Learning/how-do-bert-transformers-work) （双向编码器表示模型）在提升这一过程中的作用。文章还将包括一个关于使用 BERT 进行抽取式摘要的实践教程，展示其在将大量文本浓缩为信息摘要中的实用性。

# 理解抽取式摘要

抽取式摘要是自然语言处理（NLP）和文本分析领域的一个重要技术。通过这种方法，从原始文本中精心选择关键句子或短语，并将其组合成简洁且信息丰富的摘要。这涉及仔细筛选文本，以识别所选内容中最关键的元素和中心思想或论点。

抽取式摘要涉及从原始文本中提取句子，保持原文的措辞和结构，而不改变或改写。这样，摘要保持了源材料的语气和内容。这种方法在准确性和保留作者原意的情况下极为有用。

它有许多不同的用途，比如总结新闻文章、学术论文或冗长的报告。该过程有效地传达了原始内容的信息，而不会出现可能的偏见或重新解释，这些情况可能在改述中发生。

# 提取性总结如何使用 LLM？

## 1\. 文本解析

这一步骤涉及将文本分解为其基本元素，主要是句子和短语。目标是识别算法随后将评估以纳入摘要的基本单位（在此上下文中为句子），类似于剖析文本以理解其结构和各个组成部分。

例如，该模型将通过将四句段落拆分为以下四句组件来分析它。

1.  吉萨金字塔，建于古埃及，巍峨屹立了千年。

1.  它们被建作为法老的墓。

1.  大金字塔是最著名的。

1.  这些结构象征着建筑智慧。

## 2\. 特征提取

在这一阶段，算法分析每个句子，以识别可能指示其对整体文本重要性的特征或“特征”。常见的特征包括关键词和短语的频率和重复使用、句子的长度、它们在文本中的位置及其含义，以及文本主要主题的特定关键词或短语的存在。

以下是 LLM 如何对第一个句子“吉萨金字塔，建于古埃及，巍峨屹立了千年”进行特征提取的示例。

| 属性 | 文本 |
| --- | --- |
| 频率 | “吉萨金字塔”，“古埃及”，“千年” |
| 句子长度 | 中等 |
| 位置在文本中 | 引言，设置主题 |
| 特定关键词 | “吉萨金字塔”，“古埃及” |

## 3\. 句子评分

每个句子根据其内容分配一个得分。这个得分反映了句子在整个文本中的重要性。得分较高的句子被认为在整体文本中更有分量或相关性。

简而言之，该过程对每个句子的潜在重要性进行评分，以总结整个文本。

| 句子得分 | 句子 | 解释 |
| --- | --- | --- |
| 9 | 吉萨金字塔，建于古埃及，巍峨屹立了千年。 | 这是引言句，设置了主题和背景，包含了诸如“吉萨金字塔”和“古埃及”这样的关键词。 |
| 8 | 它们被建作为法老的墓。 | 这句话提供了关于金字塔的关键历史信息，可能表明它们在理解其目的和重要性方面的意义。 |
| 3 | 大金字塔是最著名的。 | 尽管这句话提供了关于大金字塔的具体信息，但在总结金字塔总体重要性的广泛背景下，它被认为不那么关键。 |
| 7 | 这些结构象征着建筑智慧。 | 这个总结陈述捕捉了金字塔的整体重要性。 |

## 4\. 选择和聚合

最后阶段涉及选择得分最高的句子并将其编入总结。经过精心处理，这确保了总结保持连贯，并且全面代表了原文的主要思想和主题。

要创建有效的总结，算法必须平衡包括简洁的重要句子，避免冗余，并确保所选的句子提供对整个原文的清晰和全面的概述。

+   建于古埃及的吉萨金字塔雄伟地矗立了数千年。它们是作为法老的陵墓而建造的。这些结构象征着建筑才华。

这个例子非常基础，从总共4个句子中提取了3个句子以获得最佳的整体总结。多读一个句子也无妨，但当文本更长时会怎样呢？假设是3段文字呢？

# 如何使用BERT LLMs运行抽取式总结

## 步骤1：安装和导入必要的包

我们将利用预训练的BERT模型。然而，我们不会使用任何BERT模型，而是专注于BERT Extractive Summarizer。这个特定的模型已经为抽取式总结的专业任务进行了精细调优。

```py
!pip install bert-extractive-summarizer
from summarizer import Summarizer
```

## 步骤2：引入总结函数

从Python的summarizer中导入的Summarizer()函数是一个抽取式文本总结工具。它使用BERT模型来分析和提取较大文本中的关键句子。该函数旨在保留最重要的信息，提供原始内容的精简版本。它通常用于高效地总结冗长的文档。

```py
model = Summarizer()
```

## 步骤3：导入我们的文本

在这里，我们将导入任何我们想要测试模型的文本。为了测试我们的抽取式总结模型，我们使用ChatGPT 3.5生成了以下文本，提示是：“提供一个关于GPU历史及其今日应用的3段总结。”

```py
text = "The history of Graphics Processing Units (GPUs) dates back to the early 1980s when companies like IBM and Texas Instruments developed specialized graphics accelerators for rendering images and improving overall graphical performance. However, it was not until the late 1990s and early 2000s that GPUs gained prominence with the advent of 3D gaming and multimedia applications. NVIDIA's GeForce 256, released in 1999, is often considered the first GPU, as it integrated both 2D and 3D acceleration on a single chip. ATI (later acquired by AMD) also played a significant role in the development of GPUs during this period. The parallel architecture of GPUs, with thousands of cores, allows them to handle multiple computations simultaneously, making them well-suited for tasks that require massive parallelism. Today, GPUs have evolved far beyond their original graphics-centric purpose, now widely used for parallel processing tasks in various fields, such as scientific simulations, artificial intelligence, and machine learning.  Industries like finance, healthcare, and automotive engineering leverage GPUs for complex data analysis, medical imaging, and autonomous vehicle development, showcasing their versatility beyond traditional graphical applications. With advancements in technology, modern GPUs continue to push the boundaries of computational power, enabling breakthroughs in diverse fields through parallel computing. GPUs also remain integral to the gaming industry, providing immersive and realistic graphics for video games where high-performance GPUs enhance visual experiences and support demanding game graphics. As technology progresses, GPUs are expected to play an even more critical role in shaping the future of computing." 
```

## 步骤4：执行抽取式总结

最后，我们将执行我们的总结函数。该函数需要两个输入：要总结的文本和所需的总结句子数量。处理后，它将生成一个抽取式总结，我们将显示出来。

```py
# Specifying the number of sentences in the summary
summary = model(text, num_sentences=4) 
print(summary)
```

**抽取式总结输出：**

*图形处理单元（GPU）的历史可以追溯到1980年代初期，当时像IBM和德州仪器这样的公司开发了专用的图形加速器，用于渲染图像和提高整体图形性能。NVIDIA的GeForce 256，发布于1999年，常被认为是第一款GPU，因为它将2D和3D加速集成在一个芯片上。如今，GPU已经远远超出了其最初的图形中心用途，现在广泛用于各种领域的并行处理任务，如科学模拟、人工智能和机器学习。随着技术的发展，GPU预计将在塑造计算未来中发挥更为关键的作用。*

我们的模型从大量文本中提取了4个最重要的句子来生成这个摘要！

# 使用LLM进行提取式总结的挑战

1.  上下文理解的局限性

    1.  虽然LLM在处理和生成语言方面表现出色，但它们对上下文的理解，尤其是在较长文本中的理解，仍然有限。LLM可能会遗漏细微的差别或未能识别文本中的关键方面，导致总结不够准确或相关。语言模型越先进，总结的质量就越好。

1.  训练数据中的偏差

    1.  大型语言模型（LLM）从包括互联网在内的各种来源编译的大量数据集中学习。这些数据集可能包含偏差，模型可能无意中学习并在总结中复制这些偏差，导致扭曲或不公平的表现。

1.  处理专业或技术语言

    1.  虽然LLM通常在各种普通文本上进行训练，但它们可能无法准确捕捉到法律、医学或其他高度专业领域的专业或技术语言。这可以通过提供更多专业和技术文本来缓解。缺乏对专业术语的训练可能会影响这些领域中总结的质量。

# 结论

显然，提取式总结不仅仅是一个方便的工具；在我们每天被大量文本淹没的信息时代，它变得越来越必要。通过利用像BERT这样的技术，我们可以看到复杂的文本如何被提炼成易于理解的摘要，从而节省时间，并帮助我们更好地理解被总结的文本。

无论是用于学术研究、商业洞察，还是仅仅为了在技术先进的世界中保持信息更新，提取式总结都是在我们被信息海洋包围的世界中导航的实用方式。随着自然语言处理技术的不断发展，像提取式总结这样的工具将变得更加重要，帮助我们快速找到并理解在每分钟都很关键的世界中最重要的信息。

[原文](https://www.exxactcorp.com/blog/deep-learning/extractive-summarization-with-llm-using-bert)。经许可转载。

**[Kevin Vu](https://blog.exxactcorp.com/)** 负责管理 [Exxact Corp 博客](https://blog.exxactcorp.com/)，并与许多才华横溢的作者合作，这些作者撰写关于深度学习不同方面的内容。

### 了解更多主题

+   [使用 BERT 对长文本文档进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [文本摘要方法概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [开始使用自动化文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [使用 GPT-3 进行摘要](https://www.kdnuggets.com/2022/04/packt-summarization-gpt3.html)

+   [文本摘要开发：使用 GPT-3.5 的 Python 教程](https://www.kdnuggets.com/2023/04/text-summarization-development-python-tutorial-gpt35.html)

+   [通过密度链提示解锁 GPT-4 摘要](https://www.kdnuggets.com/unlocking-gpt-4-summarization-with-chain-of-density-prompting)
