# 数据驱动的人工智能：你需要了解的最新研究

> 原文：[`www.kdnuggets.com/2022/02/datacentric-ai-latest-research-need-know.html`](https://www.kdnuggets.com/2022/02/datacentric-ai-latest-research-need-know.html)

![数据驱动的人工智能：你需要了解的最新研究](img/fed9eb27fdbd33a0cc676a227318abea.png)

[技术照片由 rawpixel.com 创建 - www.freepik.com](https://www.freepik.com/photos/technology)

尽管目前大多数研究工作专注于机器学习模型和算法，但数据本身往往被忽视，并且被视为固定不变。这种观点可能是有害的——因为模型与真实情况脱节，理论优于实践的风险很大。需要通过激励和信息来应对这一趋势，促使研究人员和从业者更多地关注数据。

## **让我们深入了解一下**

两个值得研究的有趣观点：

+   面对模型改进的问题时，应通过提升数据质量来解决，而不是重新设计算法。

+   在处理数据时，应将过程视为一个整体（包括对标签者的精心选择），而不仅仅是技术层面（例如，聚合）。

一些有用的资源和比赛：

+   [数据驱动的人工智能竞赛](https://https-deeplearning-ai.github.io/data-centric-comp/)由 Andrew Ng 及其团队组织，邀请来自全球的参与者在不接触算法的情况下改进竞赛数据，从而绕过常见的模型驱动方法。数据集包含罗马数字，需要通过应用各种数据驱动技术来调整——修正错误标签、添加自有数据、数据增强等。

+   [Koch 等人的论文](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/3b8a614226a953a8cd9526fca6fe9ba5-Abstract-round2.html)以及[Koch 的简短讲座](https://neurips.cc/virtual/2021/poster/29818)聚焦于不同机器学习子社区的数据集使用模式的差异。这项研究的主要结论是，除了自然语言处理（NLP），研究人员往往将相同的数据集用于不同的任务。因此，基准测试代表真实世界数据科学问题的情况有所下降。

+   [MLCommons 基金会](https://mlcommons.org/en/)致力于加速机器学习创新，并在其页面上列出了各种实用的材料和技术。

+   [DataPerf](https://dataperf.org/)是由几所大学、公司和知名研究人员发起的，旨在为训练集、测试集和各种数据驱动算法创建基准测试。

所有的文章都可以在会议记录页面[这里](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021)找到，包括[数据集和基准](https://neurips.cc/virtual/2021/events/Datasets%20and%20Benchmarks)以及[数据驱动的人工智能研讨会](https://neurips.cc/virtual/2021/workshop/21860)。

## **数据质量**

测量数据集的质量极为重要；然而，目前尚无普遍认可的方法来实现这一点。虽然可以通过不同的指标来评估模型的准确性，但可靠的数据评估方法尚未找到。数据相关的问题可能导致模型在涉及噪声数据集时表现不佳。同时，不正确标注的数据也可能导致看似健康的机器学习模型学习到错误甚至潜在危险的模式。

一些研究探讨了各种数据集错误——提升数据质量的第一步：

+   Northcutt、Athalye 和 Mueller [研究了](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/f2217062e9a397a1dca429e7d70bc6ca-Abstract-round1.html) [十个最常用的计算机视觉、自然语言处理和音频数据集中的错误](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/f2217062e9a397a1dca429e7d70bc6ca-Abstract-round1.html)，以确定这些错误如何影响基准测试。他们的努力成果是[Cleanlab](https://neurips.cc/virtual/2021/poster/22763)，这是一个开源解决方案，帮助识别标签错误。附上[Github 链接](https://github.com/cleanlab/cleanlab)。

+   Kang 等人提出的[论文](https://datacentricai.org/papers/57_CameraReady_main.pdf)为[闪电演讲](https://neurips.cc/virtual/2021/workshop/21860)奠定了基础。该团队提出了*学习观察断言*，这是一种用于测试机器学习管道的概率技术。

数据收集程序也值得讨论。主要的问题仍然是如何保持高质量，并理想地朝着主流软件工程的方法前进，寻找通用的标准和方法。两个有趣的资源：

+   [Google Brain 的《人工智能中的技术债务》](https://neurips.cc/virtual/2021/workshop/21860)（从 5:53 开始）。

+   [与 REAIS](https://eval.how/reais-2020/)（人工智能系统的严格评估）同时进行的[数据卓越研讨会](https://eval.how/dew2020/index.html)。这里有一个[总结](https://arxiv.org/ftp/arxiv/papers/2111/2111.10391.pdf)主要内容。

### **数据收集和标注**

关于数据收集的另一个问题是如何拥有足够的数据变异性，以准确代表较少见的现实世界事件。一些提供的解决方案集中在手动设置数据收集参数（例如，AV 的天气模式和行人数量）、利用学习迭代不断将不寻常的案例添加到数据集中以及利用合成数据。以下是这些研究努力中的一些：

+   Raquel Urtasun（多伦多大学教授和 Waabi 创始人）关于[AV 数据收集的全面讲座](https://neurips.cc/virtual/2021/datasets-and-benchmarks/47107#wse-detail-47186)，从第 57 分钟开始。

+   一篇名为[意图分类的数据增强](https://datacentricai.org/papers/138_CameraReady_Data_Aug_v5.pdf)的研究论文，由 Chen 和 Yin 撰写，并附有相应的[闪电讲座](https://neurips.cc/virtual/2021/workshop/21860)。研究人员使用混合增强技术，从航空和电信行业的种子数据中生成伪标签训练样本。

+   Pavlichenko 等人发表的关于[CrowdSpeech](https://openreview.net/forum?id=3_hgF1NAXU7)的论文贡献了一个大型数据集，以解决众包中最常见的问题之一：音频转录聚合。该数据集包含了一个知名语音识别数据集的转录录音，[LibriSpeech](https://www.openslr.org/12)。有趣的是，最佳聚合模型最初是为文本摘要设计的，但其表现超越了其他模型，包括特定任务的模型。此外，为了处理其他语言，作者创建了一个名为 Vox DIY 的管道，可以为几乎任何自然语言复制类似的数据集。

有关监督数据标注、说明不足和标注员培训的问题也相当有趣：

+   斯坦福大学的 Michael Bernstein 讲述的[人机交互和数据中心人工智能的众包](https://neurips.cc/virtual/2021/workshop/21860)（从 1:12 开始） – 研究人员讨论了十年来众包研究中的“好”数据和“坏”数据。

+   **数据中心人工智能与程序化监督**：[Alex Ratner 的讲座](https://neurips.cc/virtual/2021/workshop/21860)，讲述了如何使用 Snorkel 进行数据标注（从 5:38 开始）。

数据管理是人工智能社区面临的另一个挑战。当需要在长时间内进行多个操作和编辑时，如何管理嵌入复杂管道的数据集？是否需要版本控制和维护变更日志？这两场讲座特别关注数据文档化：

+   [数据卡片](https://datacentricai.org/papers/112_CameraReady_Data_Cards.pdf)是 Google Research 的 Pushkarna 和 Zaldivar 建议的模板。该方法用于总结数据集的关键信息。可以参考[闪电讲座](https://neurips.cc/virtual/2021/workshop/21860)。

+   Tagliabue 等人提出的 [DAG 卡片](https://datacentricai.org/papers/43_CameraReady_neurips_data_centric_2021_DAG_CARDS_camera_ready.pdf) 是最新的灵活管道文档化方案之一。这个 [闪电演讲](https://neurips.cc/virtual/2021/workshop/21860) 总结了其背后的逻辑及其主要优点。

## **伦理**

伦理问题是一个高度争议的话题。需要牢记的一些关键方面包括：

+   隐私，包括但不限于 GDPR 指南。

+   偏见 – 避免数据集中的刻板印象，这些刻板印象常常对训练模型产生负面影响。

+   少数群体 – 数据集缺乏多样化，许多社会群体往往被低估或完全排除在外。

+   仅通过修改算法很难解决这些问题；因此，需要改进数据收集和数据标注过程。

解决这些问题的一个良好起点是识别这些问题的原因及其发生频率。查看我们专门讨论这一工作三场演讲：

+   Bao 等人的 [演讲](https://neurips.cc/virtual/2021/poster/22773) 及其支持的研究论文 [《这真是复杂的 COMPAS》](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/92cc227532d17e56e07902b254dfad10-Abstract-round1.html)。作者批评了由执法部门使用的风险评估工具（RAI）数据集——即 COMPAS——这些数据集本质上存在偏见和种族歧视。

+   Peng、Mathur 和 Narayanan 的一篇 [研究论文](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/file/077e29b11be80ab57e1a2ecabb7da330-Paper-round2.pdf) 和 [演讲](https://neurips.cc/virtual/2021/poster/29847) 关注伦理问题明显的面部识别数据集及其使用的影响，通过分析引用这些数据集的 1000 篇论文进行探讨。

+   一篇关于数据驱动的人工智能工作坊的论文提出了一种新的大规模数据集用于学习对人们年龄的主观意见，[IMDB-WIKI-SbS](https://datacentricai.org/papers/115_CameraReady_NeurIPS_2021_Data_Centric_AI_IMDB_WIKI_SbS-2.pdf)，它有助于设计更好的推荐和排名算法。值得承认的是，该数据集在年龄和性别组方面是平衡的，目前机器学习模型很难在所有组别中平等处理照片。

尽管伦理问题显而易见，但解决方案却不那么明显。Bartl 和 Leavy 的 [《数据驱动的人工智能的女性主义策展》](https://datacentricai.org/papers/79_CameraReady_DCAI_Workshop_NeurIPS_2021_final.pdf) 是为现有数据集做出补救的少数研究之一。Bartl 的 [闪电演讲](https://neurips.cc/virtual/2021/workshop/21860) 概述了研究人员如何利用女性主义语言学来评估和缓解文本数据集及语言模型中的性别偏见。

最后，一篇值得阅读的有用论文是题为[不一致性解卷积](http://www.kayur.org/papers/chi2021.pdf)。这篇由伯恩斯坦共同撰写的论文重点讨论了如何通过 ROC-AUC 指标展示聚合中的多数投票有时会导致不良结果。

**[Alexey Umnov](https://www.linkedin.com/in/alexey-umnov-710b72222/)** 是 Toloka 的机器学习顾问和博士。

### 相关主题

+   [KDnuggets 新闻，4 月 13 日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [朴素贝叶斯算法：你需要了解的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [数据科学基础：你需要了解的 10 项关键技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [想用你的数据技能解决全球问题？这里是…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [关于张量你需要知道的一切](https://www.kdnuggets.com/2022/05/everything-need-know-tensors.html)

+   [关于数据管理你需要了解的 6 件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)
