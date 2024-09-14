# 阅读论文如何帮助你成为更有效的数据科学家

> 原文：[https://www.kdnuggets.com/2021/02/reading-papers-effective-data-scientist.html](https://www.kdnuggets.com/2021/02/reading-papers-effective-data-scientist.html)

[评论](#comments)

**由 [Eugene Yan](https://eugeneyan.com/about/)，亚马逊的应用科学家**

“与其手动检查我们的数据，为什么不尝试LinkedIn的方法呢？这帮助他们实现了95%的精确度和80%的召回率。”

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

我的队友分享了 [LinkedIn](https://engineering.linkedin.com/research/2011/high-precision-phrase-based-document-classification-on-a-modern-scale) 如何使用*k*-最近邻算法来识别不一致的标签（在职位名称中）。然后，LinkedIn在一致的标签上训练了一个支持向量机（SVM），并使用这个SVM来更新不一致的标签。这帮助他们在职位标题分类器上达到了95%的精确度。

这个建议在我们的讨论中最为有用。跟进这个建议让我们的产品分类器最终达到了95%的准确率。我问她是如何提供这样关键的见解的。她回答道：“哦，我只是偶尔读读论文”，她具体说她每周尝试阅读1-2篇论文，通常是与团队正在研究的主题相关的。

通过阅读论文，我们能够了解其他人（例如，LinkedIn）发现哪些方法有效（以及哪些无效）。然后，我们可以调整他们的方法，而不必重新发明火箭。这帮助我们**用更少的时间和精力交付有效的解决方案**。

> *如果我比其他人看得更远，那是因为我站在巨人的肩膀上。*
> 
> — 艾萨克·牛顿*

阅读论文也**拓宽了我们的视野**。虽然我们可能只关注数据科学的某些方面，但相关研究的进展往往是有帮助的。例如，[词嵌入和图](https://eugeneyan.com/writing/recommender-systems-graph-and-nlp-pytorch/)的理念在推荐系统中非常有用。同样，[计算机视觉的思想](https://pakodas.substack.com/p/nlp-keeps-stealing-from-cv-) — 如迁移学习和数据增强 — 对自然语言处理（NLP）也很有帮助。

阅读论文也**让我们保持最新**。在过去十年间，NLP领域取得了巨大的进展。然而，通过阅读最关键的10篇论文，我们可以迅速跟上最新进展。保持最新状态使我们在工作中更高效，从而减少时间和精力的投入。这样，我们将有更多时间来阅读和学习，从而形成良性循环。

### 如何选择要阅读的论文？

如果我们刚开始养成这个习惯，可以**阅读任何感兴趣的内容**——大多数论文都会教会我们一些东西。阅读我们感兴趣的话题也更容易养成习惯。

我们也可以根据**实用性**选择论文。例如，我们可能需要快速了解某个领域以完成一个项目。在开始一个项目之前，我几乎总是[留出时间进行文献综述](https://eugeneyan.com/writing/what-i-do-during-a-data-science-project-to-ensure-success/#research-what-others-have-done-and-what-worked)。花几天时间深入研究论文可以节省几周，甚至几个月的时间，避免走弯路和不必要的重复劳动。

**推荐**也是识别有用论文的便捷方法。一种窍门是关注我们欣赏的人在社交媒体上的动态，或订阅策划的通讯——我发现这些来源的信息噪声比率很高。

我阅读哪些论文？出于实际考虑，我主要阅读与工作相关的论文。这使我能立即应用所读内容，从而巩固学习。在工作之外，我对[序列](https://github.com/eugeneyan/applied-ml#sequence-modelling)感兴趣，倾向于阅读关于[NLP](https://github.com/eugeneyan/applied-ml#natural-language-processing)和[强化学习](https://github.com/eugeneyan/applied-ml#reinforcement-learning)的论文。我特别喜欢那些分享有效和无效方法的论文，比如通过消融研究。这包括关于[Word2vec](https://arxiv.org/abs/1301.3781)、[BERT](https://arxiv.org/abs/1810.04805)和[T5](https://arxiv.org/abs/1910.10683)的论文。

### 如何阅读论文？

谷歌搜索“如何阅读论文”会返回大量有用的结果。但如果你觉得这些信息太多，以下是一些我觉得有帮助的：

+   *经典的*[三遍阅读法](https://web.stanford.edu/class/ee384m/Handouts/HowtoReadPaper.pdf)（以及一个[三分钟视频](https://www.youtube.com/watch?v=SKxm2HF_-k0)版本）

+   [OMSCS 6460 如何阅读学术论文](http://omscs6460.gatech.edu/research-guide/how-to-read-an-academic-paper/): 一位我喜欢的教授的建议

+   [采访](https://www.sciencemag.org/careers/2016/03/how-seriously-read-scientific-paper)其他科学家分享他们的阅读方法

+   关于工程研究论文的一个[方法](https://cseweb.ucsd.edu/~wgg/CSE210/howtoread.html)

+   [安慰](https://www.sciencemag.org/careers/2016/01/how-read-scientific-paper)我们并非唯一觉得困难的人

我的方法类似于三遍法。在下面的例子中，我将分享如何阅读几篇推荐系统论文，以了解新颖性、多样性、偶然性等指标。

**在第一遍**，我扫描摘要以了解论文是否包含我所需的内容。如果包含，我会浏览标题以识别问题陈述、方法和结果。在这个例子中，我特别寻找如何计算各种指标的公式。我会对列表上的所有论文进行第一次阅读（并且在完成列表之前避免开始第二遍）。在这个例子中，大约**一半的论文**进入了第二遍。

![图示](../Images/2966cdfbdd5881010251f197c5a3a582.png)

*第一次阅读后，30多篇论文减少到14篇——节省了不少精力。*

**在第二遍**，我再次阅读每篇论文并**突出相关部分**。这有助于我在之后查阅论文时快速找到重要部分。然后，我为每篇论文做笔记。在这个例子中，笔记主要集中在指标（即方法、公式）上。如果是关于某个应用的文献综述（如推荐系统、产品分类、欺诈检测），笔记将重点关注方法、系统设计和结果。

![图示](../Images/ee9b4f8e781279768ac7530f55afd502.png)

*来自三篇论文的示例笔记；与指标相关的笔记用红色框出。*

对于大多数论文，第二遍已经足够。我已经捕捉了关键信息，并可以在未来需要时参考。尽管如此，如果我在进行文献综述时阅读论文，或者想要巩固我的知识，我有时会进行第三遍。

> *阅读只为大脑提供知识材料；思考才使我们所读的内容成为我们自己的。*
> 
> — 约翰·洛克*

**在第三遍**，我将论文中的共通概念整合到自己的笔记中。不同的论文有各自测量新颖性、多样性、偶然性等的方式。我将它们汇总到一个笔记中，并比较其优缺点。在这个过程中，我经常发现笔记和知识的 gaps，必须重新查阅原论文。

![图示](../Images/aa6dc751e8a898f1c24351a1804eade3.png)

*关于偶然性和意外性指标的示例笔记。*

最后，如果我认为对其他人有用，我会写下我所学到的，并在线发布。与从头开始相比，拥有笔记作为参考使得写作变得容易得多。这导致了如下作品：

+   [用图谱和NLP在Pytorch中击败基准推荐系统](https://eugeneyan.com/writing/recommender-systems-graph-and-nlp-pytorch/)

+   [偶然性：推荐系统中不受欢迎的最佳伙伴](https://eugeneyan.com/writing/serendipity-and-accuracy-in-recommender-systems/)

+   [NLP在监督学习中的应用——简要调查](https://eugeneyan.com/writing/nlp-supervised-learning-survey/)

### 亲自尝试一下

在深入下一个项目之前，花一两天时间浏览几篇相关论文。我相信这将为你节省中长期的时间和精力。不知道从哪里开始？以下是一些有用的资源供你参考：

+   [带代码的论文](https://paperswithcode.com/): 机器学习研究及其实现代码

+   `[applied-ml](https://github.com/eugeneyan/applied-ml)`: 组织如何构建和部署机器学习系统的论文

+   `[ml-surveys](https://github.com/eugeneyan/ml-surveys)`: 总结近期机器学习进展的调研论文

+   [Google Scholar 提醒](https://scholar.google.com/intl/en/scholar/help.html#alerts): 当有新出版物符合你的查询时会收到更新

+   [42篇论文](https://42papers.com/): AI 和计算机科学领域的热门论文

**个人简介: [Eugene Yan](https://eugeneyan.com/about/)** 在机器学习与产品的交叉领域工作，致力于构建实用的面向客户的机器学习系统。他目前是亚马逊的应用科学家。此前，他曾领导 Lazada 和 uCare.ai 的数据科学团队。他在 [eugeneyan.com](https://eugeneyan.com/) 上撰写和演讲关于数据科学、数据/机器学习系统和职业发展方面的内容，并在 [@eugeneyan](https://twitter.com/eugeneyan) 上发推文。

[原文](https://eugeneyan.com/writing/why-read-papers/)。经允许转载。

**相关资源:**

+   [5篇必读的数据科学论文（及其使用方法）](/2020/10/5-must-read-data-science-papers.html)

+   [2020年十大计算机视觉论文](/2021/01/top-10-computer-vision-papers-2020.html)

+   [深度学习先驱 Geoff Hinton 论最新研究及 AI 未来](/2021/01/deep-learning-pioneer-geoff-hinton-research-future-ai.html)

### 更多相关主题

+   [每个数据科学家都应该了解的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [停止学习数据科学以寻找目标，并寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一桩90亿美元的 AI 失败，深入剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
