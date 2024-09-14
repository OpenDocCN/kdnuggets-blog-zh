# 提示工程：一场集成的梦想

> 原文：[https://www.kdnuggets.com/prompt-engineering-an-integrated-dream](https://www.kdnuggets.com/prompt-engineering-an-integrated-dream)

![提示工程：一场集成的梦想](../Images/88c229b1317415dfe2078ac1d864d7fa.png)

图片由我使用 Microsoft Image Creator 创建

自从 OpenAI 向公众发布 ChatGPT 以来，关于一种新兴梦幻职业——提示工程师的讨论如潮水般涌现。这被誉为“[AI 最火的工作](https://www.bloomberg.com/news/videos/2023-07-05/ai-s-hottest-job-prompt-engineer)”，承诺提供六位数薪资，[无需编程经验](https://www.washingtonpost.com/technology/2023/02/25/prompt-engineers-techs-next-big-job/)。爱好者们将其描述为[未来的工作](https://www.weforum.org/agenda/2023/03/new-emerging-jobs-work-skills/)，在这里，[任何人都可以赚取](https://mashable.com/article/what-are-prompt-engineer-jobs-ai) 高达[$335K](https://time.com/6272103/ai-prompt-engineer-job/)的薪水，只需通过巧妙的交流让机器人给出正确的答案。毫不奇怪，Instagram上的[赚钱达人](https://www.instagram.com/reel/Co45VwXqkaV/?igsh=MTB4a3p0dXczbGt6eQ==)、YouTube上的职业[布道者](https://www.youtube.com/watch?v=lVIPPIyCmkM)和自称的TikTok预言者对此十分热衷。虽然这听起来像是一份梦幻工作，但它真的可实现吗？让我们深入了解一下背后的实际情况吧。

分析招聘广告数据可以提供关于劳动需求趋势、职责、资格要求和薪资预期的有价值见解。因此，我决定查看所谓的“[AI 最火的工作](https://www.bloomberg.com/news/videos/2023-07-05/ai-s-hottest-job-prompt-engineer)”的广告数据，没有任何推测或假设。我从流行的在线招聘平台收集了73份最近发布的独特职位广告数据。阅读我的数据收集方法并访问数据集[这里](https://github.com/ahmadi-analyst/prompt-eng-job-ads)。虽然73个样本可能不是理想的样本量，但它为我们的分析提供了全面的起点。初步的发现令人清醒：雇主对“提示工程师”的需求稀缺。

现在，让我们看看数据。最常提到的职位名称是“提示工程师”。然而，“IT 创新分析师”、“自由职业 ML/AI 工程师”、“数据科学家”和“AI 工程师”等其他职位名称也出现了。我为职位描述中提到的资格要求和职责创建了词云。我认为词云并不意味着揭示非凡的见解，但它们可以代表文本中重要亮点的简明版本。如您所见，在职位广告中，雇主谈论计算机科学、模型开发、python、提示设计、机器学习、大型语言模型、自然语言处理和人工智能的频率高于其他内容。

![提示工程：一个综合梦想](../Images/3af3321c739cdfcde82c88d620c58aa7.png)

1.如果将其与许多早期的轶事文章相比，这个样本量大得多，那些文章仅根据一个职位广告构建了六位数薪水的整个论点。

![提示工程：一个综合梦想](../Images/4ee495214c5783f1b6c0b2f2145c4784.png)

接下来，我使用ChatGPT和Claude总结了收集到的广告文本语料库，以识别顶级提示工程资格。我进行了多轮不同方法的提示，然后手动检查数据，以确保获得稳定和有效的输出。

提示工程师职位所需的基本资格：

1.  **Python编程能力（2-5年的经验）**，包括对TensorFlow、PyTorch、Keras等AI/机器学习框架的经验。

1.  **NLP和LLM的工作知识（2-5年的经验）**，如BERT、GPT-3/4、T5等。了解这些模型的工作原理以及如何对其进行微调。

1.  **强大的分析和解决问题的能力**。具备批判性思维、设计有效提示、分析模型性能和解决问题的能力是至关重要的。

1.  **精通提示工程原理和技术**，如思维链、上下文学习、思维树等。这有助于引导模型实现期望结果。

1.  **出色的沟通技能**，包括口头和书面沟通。这对于跨团队协作、解释技术概念和文档记录是必要的。

提示工程职位的核心职责包括：

1.  **提示设计与优化**：设计、开发、测试和改进AI生成的文本提示，以最大限度地提高在各种应用中的有效性。这包括利用迁移学习技术和语言学专业知识来打造高质量和多样化的提示。

1.  **集成与部署**：确保优化后的提示与整体产品或系统的无缝集成。与工程师协作，将提示和模型实施到生产环境中。

1.  **性能评估与改进**：通过使用指标和用户反馈对提示性能进行严格评估。进行持续的测试和分析，以识别优化领域并迭代提示。

1.  **协作与需求收集**：与数据科学家、内容创作者和产品经理等跨职能团队紧密合作，以理解需求并确保提示与业务目标和用户需求一致。

1.  **知识共享**：记录提示工程的过程和结果。教育团队关于提示的最佳实践。保持对最新人工智能进展的关注，以带来创新。

可以公平地说，所谓的“AI最热门工作”中的“无编程经验”前提与现实相差甚远，因为提示工程市场上最受需求的技能是编程熟练度和NLP及LLMs经验。而且这些并不是简单的编程技能，他们寻找的是熟悉ML和AI框架的专家。雇主不仅要求对LLMs和编码有“了解”，而且平均来看，他们寻求具有2-5年处理结构化和非结构化数据、编码、NLP、ML和AI经验的专家。

阅读主要职责可以更清楚地了解为什么这个职位要求如此高的编程和LLMs技能。作为一种专业工作，提示工程并不是坐在电脑前玩生成AI模型来提供正确答案。它是关于构建业务信息系统，优化输入，与其他信息系统和产品无缝集成，并向用户和客户提供价值。换句话说，企业并不是寻找能够与ChatGPT聊天的人，而是希望聘请能够优化类似GPT的模型并将其与自己产品集成的专家。

职位广告数据分析显示，技术背景在计算机科学、数学、分析、工程、物理或语言学方面更受青睐。通常要求计算机科学或相关领域的学士学位，更高级的职位通常要求或更倾向于高级学位。薪资根据责任和资历差异很大，最低可达30k美元，每年最高可达50万美元。平均而言，包含薪资信息的职位广告年薪在90k到195k美元之间。

尽管最初充满热情，但关于提示工程作为梦想工作的可行性产生了怀疑。正如沃顿商学院教授**伊桑·莫利克**去年在一条[twitter post](https://twitter.com/emollick/status/1627804798224580608)中写道，“提示工程师不是未来的工作”，因为“AI变得更容易”，在解读基本提示方面更为智能。一个月前，Coursera发布了一份经过深思熟虑的[prompt工程职业指南](https://www.coursera.org/articles/prompt-engineering-jobs)（另见[此处](https://www.techtarget.com/whatis/feature/Skills-needed-to-become-a-prompt-engineer)）。似乎最初的生成AI[热潮正在缓慢消退](/why-prompt-engineering-is-a-fad)，我们现在有更好的机会了解AI的现状和未来趋势。不要误解我的意思。生成AI输出的质量在很大程度上依赖于输入。学习如何使用和与这些复杂模型互动正成为几乎所有人都需要的重要技能。越来越多的科学研究表明，系统化的提示方法可以显著改善这些模型的结果（参见[1](https://arxiv.org/abs/2201.11903)，[2](https://arxiv.org/abs/2304.14670)，[3](https://arxiv.org/abs/2303.08769)，[4](https://arxiv.org/abs/2301.08721)，[5](https://dl.acm.org/doi/abs/10.1145/3491101.3519729?casa_token=DZRbqRwuGfoAAAAA:FDvB1bUOqHo0VXei8bOjYAW9YdwDg6Zx-EO3jW_hF80GEo2G3kcJvRjmfeYD4FYIIGxS8zwYrzbh)，[6](https://arxiv.org/abs/2308.10379)，[7](https://arxiv.org/abs/2401.14043)）。然而，“提示工程”并不是（也从未是）一些人想象中的梦想工作。如果没有丰富的编程、自然语言处理、机器学习、产品开发和软件集成经验，没有人会为你仅仅通过顺利引导ChatGPT得到正确答案而支付六位数的薪水。

提示工程和生成AI应用的现状和未来似乎受到两个重要趋势的影响：首先，正如伊桑·莫利克提到的，生成AI模型在从简单的提示中生成良好输出方面变得越来越熟练，可能类似于互联网搜索引擎在从简单的搜索查询中返回更相关结果的能力。其次，生成AI模型正越来越多地集成到商业产品、服务和平台中。这种适应对AI经济的成功至关重要。因此，了解如何优化、微调、定制和将生成AI模型与当前信息系统和产品集成是并将继续是一项宝贵的技能。这就是为什么当前的提示工作广告中对程序员、系统设计师以及能够与其他产品开发团队成员协作的人的需求如此巨大。

**[Mahdi Ahmadi](https://www.linkedin.com/in/ahmadi-analyst/)** 是北德克萨斯大学信息技术与决策科学系的临床助理教授，我教授数据挖掘、商业智能和数据分析。我的主要研究领域是机器学习和数据挖掘技术在商业中的应用。我还为企业、高等教育机构和非营利组织提供数据分析问题的咨询。

### 更多相关内容

+   [从虚构到现实：ChatGPT 和科幻对真正人工智能的梦想…](https://www.kdnuggets.com/from-fiction-to-reality-chatgpt-and-the-sci-fi-dream-of-true-ai-conversation)

+   [免费的数据科学面试书籍，助你获得梦想工作](https://www.kdnuggets.com/free-data-science-interview-book-to-land-your-dream-job)

+   [提示工程的艺术：解码 ChatGPT](https://www.kdnuggets.com/2023/06/art-prompt-engineering-decoding-chatgpt.html)

+   [一些顶尖的提示工程技术以提升我们的 LLM 模型](https://www.kdnuggets.com/some-kick-ass-prompt-engineering-techniques-to-boost-our-llm-models)

+   [为什么提示工程是一个潮流](https://www.kdnuggets.com/why-prompt-engineering-is-a-fad)

+   [提示工程的兴起与衰退：潮流还是未来？](https://www.kdnuggets.com/the-rise-and-fall-of-prompt-engineering-fad-or-future)
