# 数据分析领域的职业趋势：NLP用于职位趋势分析

> 原文：[https://www.kdnuggets.com/job-trends-in-data-analytics-nlp-for-job-trend-analysis](https://www.kdnuggets.com/job-trends-in-data-analytics-nlp-for-job-trend-analysis)

**作者：Mahantesh Pattadkal & Andrea De Mauro**

数据分析在[近年来经历了显著增长](https://growthtribe.io/blog/data-analytics-career#:~:text=The%20US%20Bureau%20of%20Labor,million%20data%20professionals%20last%20year)，这得益于数据在关键决策过程中的应用进展。数据的收集、存储和分析也因这些进展而显著提升。此外，数据分析领域对人才的需求暴涨，使得具备必要技能和经验的个人在求职市场上面临高度竞争。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

数据驱动技术的快速扩展相应地导致了对专业角色的需求增加，例如“数据工程师”。这种需求激增不仅限于数据工程，还包括数据科学家和数据分析师等相关职位。

认识到这些职业的重要性，我们的博客系列旨在从在线职位发布中收集实际数据，并分析这些数据以了解这些职位的需求性质，以及每个类别中所需的各种技能。

在这篇博客中，我们介绍了一个基于浏览器的“[数据分析职位趋势](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2tfbG93XFBhdGgiOiI1ZjFkMmNiYjE5YTgyYjExZDNhZmNlMjE4MmZjYmI2IiwidG9rZW5UeXBlIjoiZGF0YV9hcHBfZGF0YSIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNDNiYjliNjAtZTIxMC00ZTYzLWJiOTUtM2JkODkwOTk5ZTc0In0.wJ4g1zSHs3TqQQm4ToC4Ae2rj1I0gKwBPOOW6hD1Kg”)” 应用程序，用于可视化和分析数据分析市场的职位趋势。在从在线职位代理处抓取数据后，它使用 NLP 技术来识别职位发布中所需的关键技能。图 1 显示了数据应用程序的快照，探索了数据分析职位市场的趋势。

![数据分析领域的职业趋势：NLP用于职位趋势分析](../Images/338d4d1df2628ed5a4df030478131199.png)

图 1： [KNIME 数据应用程序“数据分析职位趋势”](https://docs.knime.com/latest/data_apps_beginners_guide/#introduction)的快照

对于实施，我们采用了低代码数据科学平台：[KNIME Analytics Platform](https://www.knime.com/downloads)。该开源且免费的端到端数据科学平台基于可视化编程，提供广泛的功能，从纯 ETL 操作和多种数据源连接器进行数据混合，到机器学习算法，包括深度学习。

支持该应用程序的一系列工作流可以从[KNIME 社区中心](https://hub.knime.com/)免费下载，链接为[“数据分析职位趋势”](https://hub.knime.com/-/spaces/-/latest/~wX_j15juFbQ_-vWC/)。可以通过[“数据分析职位趋势”](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2Zsb3dQYXRoIjoiL1VzZXJzL3Bhb2xvdGFtYWcvam9iX2xpc3RpbmdzX2RhdGFfYXBwIiwidG9rZW5UeXBlIjoid29ya2Zsb3dUb2tlbiIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNGIwZTUwY2QtMTFjNC00NmNlLTg3Y2UtZWU5OWFjOTAxZWVkIn0.5Vo10tEa1KpX0SJ7jHJ92Z77jiBfRR0zs6fUhl0s7ew)在浏览器中进行评估。

# “数据分析职位趋势”应用程序

该应用程序由图 2 中显示的四个工作流生成，这些工作流按顺序执行以下步骤：

1.  网络爬虫用于数据收集

1.  NLP 解析和数据清理

1.  主题建模

1.  职位角色技能归因分析

工作流可以在[KNIME 社区中心](https://hub.knime.com/)的公共空间 - [数据分析职位趋势](https://hub.knime.com/-/spaces/-/latest/~wX_j15juFbQ_-vWC/)中找到。

![数据分析中的职位趋势：职位趋势分析的 NLP](../Images/93258a01194d511ca70106854f833743.png)

图 2： KNIME 社区中心空间 - [数据分析职位趋势](https://hub.knime.com/-/spaces/-/latest/~wX_j15juFbQ_-vWC/)包含一组用于构建应用程序“数据分析职位趋势”的四个工作流

+   “01_网络爬虫用于数据收集”工作流爬取在线职位发布信息，并将文本信息提取为结构化格式

+   “02_NLP 解析和清理”工作流执行必要的清理步骤，然后将长文本解析成更小的句子

+   “03_主题建模和探索数据应用”使用清理后的数据构建主题模型，然后在数据应用中可视化其结果

+   “04_职位技能归因”工作流评估技能在职位角色（如数据科学家、数据工程师和数据分析师）之间的关联，基于 LDA 结果。

## 网络爬虫用于数据收集

为了及时了解就业市场上所需的技能，我们选择了分析来自在线职位代理机构的网页抓取职位。鉴于区域差异和语言多样性，我们专注于美国的职位发布。这确保了相当大比例的职位发布以英语呈现。我们还专注于2023年2月到2023年4月的职位发布。

KNIME工作流“[01_Web Scraping for Data Collection](https://hub.knime.com/-/spaces/-/latest/~IiS2j_XjCwqBLTCy/)”在图3中通过职位代理网站上的一系列搜索URL进行爬取。

为了提取与数据分析相关的职位信息，我们使用了六个关键词进行搜索，这些关键词涵盖了数据分析领域，分别是：“大数据”、“数据科学”、“商业智能”、“数据挖掘”、“机器学习”和“数据分析”。搜索关键词存储在一个Excel文件中，并通过[Excel Reader](https://hub.knime.com/knime/extensions/org.knime.features.ext.poi/latest/org.knime.ext.poi3.node.io.filehandling.excel.reader.ExcelTableReaderNodeFactory/)节点读取。

![数据分析中的职位趋势：用于职位趋势分析的自然语言处理](../Images/4d8cb85678793480cb4a1676ed60d69d.png)

图3：KNIME工作流“[01_Web Scraping for Data Collection](https://hub.knime.com/-/spaces/-/latest/~IiS2j_XjCwqBLTCy/)”根据多个搜索URL抓取职位发布信息

该工作流的核心节点是[Webpage Retriever](https://hub.knime.com/knime/extensions/org.knime.features.rest/latest/org.knime.rest.nodes.webpageretriever.WebpageRetrieverNodeFactory/)节点。它被使用了两次。第一次（外部循环），该节点根据提供的关键词爬取网站，并生成在过去24小时内发布的美国职位的相关URL列表。第二次（内部循环），该节点从每个职位发布的URL中提取文本内容。紧随[Webpage Retriever](https://hub.knime.com/knime/extensions/org.knime.features.rest/latest/org.knime.rest.nodes.webpageretriever.WebpageRetrieverNodeFactory/)节点之后的Xpath节点解析提取的文本，以获取所需的信息，例如职位标题、要求的资格、职位描述、薪资和公司评级。最后，结果被写入本地文件以供进一步分析。图4展示了2023年2月抓取的职位样本。

![数据分析中的职位趋势：用于职位趋势分析的自然语言处理](../Images/22e17a8c9e83c774c463922e7022ca47.png)

图4：2023年2月的网页抓取结果样本

## 自然语言处理解析和数据清理

![数据分析中的职位趋势：用于职位趋势分析的自然语言处理](../Images/5128d310979cb89ab721b4965d0c69d4.png)

图5：[02_NLP Parsing and cleaning](https://hub.knime.com/-/spaces/-/latest/~bQ5O_rNnYC8Ap29p/) 工作流用于文本提取和数据清理

与所有新收集的数据一样，我们的网络抓取结果也需要一些清理。我们进行了NLP解析和数据清理，并使用图5所示的工作流[02_NLP解析与清理](https://hub.knime.com/-/spaces/-/latest/~bQ5O_rNnYC8Ap29p/)编写了相应的数据文件。

从抓取的数据中保存了多个字段，形式为字符串值的串联。在这里，我们使用一系列[字符串操作](https://hub.knime.com/knime/extensions/org.knime.features.javasnippet/latest/org.knime.base.node.preproc.stringmanipulation.StringManipulationNodeFactory/)节点在“标题-位置-公司名称提取”元节点中提取了各个部分，然后去除了不必要的列并删除了重复的行。

我们将每个职位发布文本分配了一个唯一的ID，并通过[单元分割器](https://hub.knime.com/knime/extensions/org.knime.features.base/latest/org.knime.base.node.preproc.cellsplit2.CellSplitter2NodeFactory/)节点将整个文档分割成句子。每个职位的元信息——标题、地点和公司——也被提取并与职位ID一起保存。

从所有文档中提取了最常见的1000个词，以生成一个停用词列表，包括“申请人”、“合作”、“就业”等词。这些词出现在每个职位发布中，因此对下一步的NLP任务没有信息增益。

这个清理阶段的结果是一组三个文件：

- 包含文档句子的表格；

- 包含职位描述元数据的表格；

- 包含停用词列表的表格。

## 主题建模及结果探索

![数据分析中的职位趋势：职位趋势分析的NLP](../Images/f10a064acc003707abc1a032538b355d.png)

图6: [03_主题建模与探索数据应用](https://hub.knime.com/-/spaces/-/latest/~RbOL9g4dAuB6GK4e/)工作流构建了一个主题模型，并允许用户通过[主题探索视图](https://hub.knime.com/-/spaces/-/latest/~2YaazzKTz9EdIKTE/)组件以可视化方式探索结果。

工作流[03_主题建模与探索数据应用](https://hub.knime.com/-/spaces/-/latest/~RbOL9g4dAuB6GK4e/)（图6）使用了前一个工作流中的清理数据文件。在这个阶段，我们的目标是：

+   检测并移除出现在许多职位发布中的常见句子（停用词组）

+   执行标准文本处理步骤，为主题建模准备数据

+   建立主题模型并可视化结果。

我们将在以下小节中详细讨论上述任务。

### 3.1 使用N-gram移除停用词组

许多职位发布中包含公司政策或一般协议中常见的句子，例如“非歧视政策”或“保密协议”。图7提供了一个例子，其中职位发布1和2提到了“非歧视”政策。这些句子与我们的分析无关，因此需要从我们的文本语料库中删除。我们将这些句子称为“停用短语”，并采用两种方法来识别和过滤它们。

第一个方法很简单：我们计算语料库中每个句子的频率，并删除频率大于10的句子。

第二种方法涉及N-gram方法，其中N的值范围可以从20到40。我们选择一个N值，并通过计算分类为停用短语的N-gram的数量来评估从语料库中获得的N-gram的相关性。我们对范围内的每个N值重复这一过程。我们选择了N=35作为识别最多停用短语的最佳N值。

![数据分析中的职位趋势：职位趋势分析的自然语言处理](../Images/b4942d03537ea49e0b2052e9e1195e49.png)

图7：职位发布中常见句子的示例，这些句子可以被视为“停用短语”

我们使用了两种方法来移除“停用短语”，如图7所示的工作流程所示。首先，我们删除了最常见的句子，然后我们创建了N=35的N-gram，并在每个文档中使用[字典标记器](https://hub.knime.com/knime/extensions/org.knime.features.ext.textprocessing/latest/org.knime.ext.textprocessing.nodes.tagging.dict.inport.DictionaryTaggerNodeFactory2/)节点对它们进行标记，最后，我们使用[字典替换器](https://hub.knime.com/knime/extensions/org.knime.features.ext.textprocessing/latest/org.knime.ext.textprocessing.nodes.preprocessing.dictreplacer.twoinports.DictionaryReplacer2InPortsNodeFactory2/)节点移除了这些N-gram。

### 3.2 使用文本预处理技术准备主题建模的数据

删除停用短语后，我们进行标准的文本预处理，以准备数据进行主题建模。

首先，我们从语料库中去除数字和字母数字值。然后，我们移除标点符号和常见的英语停用词。此外，我们使用之前创建的自定义停用词列表来过滤出职位领域特定的停用词。最后，我们将所有字符转换为小写字母。

我们决定关注具有重要意义的词汇，因此我们过滤了文档，只保留名词和动词。这可以通过为文档中的每个词分配词性标记（POS）来完成。我们使用[POS标记器](https://hub.knime.com/knime/extensions/org.knime.features.ext.textprocessing/latest/org.knime.ext.textprocessing.nodes.tagging.pos.PosTaggerNodeFactory2/)节点来分配这些标记，并根据其值进行过滤，特别是保留POS = 名词和POS = 动词的词汇。

最后，我们应用了斯坦福词形还原，确保语料库准备好进行主题建模。所有这些预处理步骤都由图6所示的“预处理”组件完成。

### 3.3 构建主题模型并进行可视化

在实现的最终阶段，我们应用了Latent Dirichlet Allocation (LDA)算法，通过图6所示的[Topic Extractor (Parallel LDA)](https://hub.knime.com/knime/extensions/org.knime.features.ext.textprocessing/latest/org.knime.ext.textprocessing.nodes.mining.topic.ParallelTopicExtractorNodeFactory/)节点构建主题模型。LDA算法生成多个主题（k），每个主题由(m)个关键词描述。参数(k,m)必须定义。

另外，k和m不能过大，因为我们希望通过查看关键词（技能）及其相应的权重来可视化和解释主题（技能集）。我们探索了k的范围[1, 10]，并将m的值固定为15。经过仔细分析，我们发现k=7能够产生最具多样性和明显区分的主题，并且关键词的重叠最小。因此，我们确定k=7是我们分析的**最佳**值。

## 使用互动数据应用探索主题建模结果

为了让每个人都能访问主题建模结果并自己尝试，我们将工作流程（如图6所示）作为一个[数据应用](https://docs.knime.com/latest/data_apps_beginners_guide/#introduction)部署在[KNIME Business Hub](https://www.knime.com/knime-business-hub)上，并公开了它，供所有人访问。你可以在以下链接查看：[数据分析职位趋势](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2tmb3dQYXRoIjoiL1VzZXJzL3Bhb2xvdGFtYWcvam9iX2xpc3RpbmdzX2RhdGFfYXBwIiwidG9rZW5UeXBlIjoid29ya2Zsb3dUb2tlbiIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNGIwZTUwY2QtMTFjNC00NmNlLTg3Y2UtZWU5OWFjOTAxZWVkIn0.5Vo10tEa1KpX0SJ7jHJ92Z77jiBfRR0zs6fUhl0s7ew)。

该数据应用的可视化部分来自[Topic Explorer View](https://hub.knime.com/-/spaces/-/latest/~2YaazzKTz9EdIKTE/)组件，由[Francesco Tuscolano](https://hub.knime.com/francescots/about)和[Paolo Tamagnini](https://hub.knime.com/paolotamag/about)提供，并可以从[KNIME Community Hub](https://hub.knime.com/)免费下载，提供了按主题和文档的多个交互式可视化。

![数据分析中的职位趋势：职位趋势分析的NLP](../Images/bc995289121f58974baca199b55a1f1d.png)

图8: [数据分析职位趋势](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2Zsb3dQYXRoIjoiL1VzZXJzL3Bhb2xvdGFtYWcvam9iX2xpc3RpbmdzX2RhdGFfYXBwIiwidG9rZW5UeXBlIjoid29ya2Zsb3dUb2tlbiIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNGIwZTUwY2QtMTFjNC00NmNlLTg3Y2UtZWU5OWFjOTAxZWVkIn0.5Vo10tEa1KpX0SJ7jHJ92Z77jiBfRR0zs6fUhl0s7ew) 用于探索主题建模结果

图8中展示的这个数据应用程序提供了两个不同的视图选择：“主题”和“文档”视图。

“主题”视图使用多维尺度算法将主题绘制在二维图上，有效地展示了它们之间的语义关系。在左侧面板中，你可以方便地选择感兴趣的主题，系统会显示其相应的顶级关键词。

要深入探讨个别职位发布，请选择“文档”视图。 “文档”视图以两个维度对所有文档进行简化呈现。利用框选方法定位重要文档，底部将显示你所选择文档的概述。

# 使用NLP探索数据分析职位市场

我们在这里提供了“数据分析职位趋势”应用程序的总结，该程序已实施并用于探索数据科学职位市场中最新的技能要求和职位角色。在这篇博客中，我们将行动范围限制在2023年2月至4月期间以英语书写的美国职位描述。

要了解工作趋势并提供审查， “[数据分析职位趋势](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2Zsb3dQYXRoIjoiL1VzZXJzL3Bhb2xvdGFtYWcvam9iX2xpc3RpbmdzX2RhdGFfYXBwIiwidG9rZW5UeXBlIjoid29ya2Zsb3dUb2tlbiIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNGIwZTUwY2QtMTFjNC00NmNlLTg3Y2UtZWU5OWFjOTAxZWVkIn0.5Vo10tEa1KpX0SJ7jHJ92Z77jiBfRR0zs6fUhl0s7ew)” 爬取招聘网站，提取在线职位发布的文本，通过执行一系列自然语言处理（NLP）任务提取主题和关键词，最后通过主题和文档对结果进行可视化，以识别数据中的模式。

该应用程序包含一组四个KNIME工作流，按顺序运行进行网页抓取、数据处理、主题建模，然后是交互式可视化，以帮助用户发现职位趋势。

我们在[KNIME Business Hub](https://www.knime.com/knime-business-hub)上部署了该工作流并使其公开，所有人都可以访问。你可以在以下链接查看：[数据分析职位趋势](https://communityserver.knime.com/knime/webportal/space/Users/paolotamag/job_listings_data_app?exec&embed&knime:access_token=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJwdWJsaWNfZGF0YV9hcHBfYWRtaW4iLCJyb2xlcyI6WyJhZG1pbiIsImtuaW1lIl0sInNhbHQiOiI5ODBmNTQxODJjMjhlNzljIiwid29ya2tmbG93ZFBhdGgiOiIvVXNlcnMvcGFvbG90YW1hZy9qb2JfX2xpc3RpbmdzX2RhdGFfYXBwIiwidG9rZW5UeXBlIjoid29ya2Zsb3dUb2tlbiIsInVzYWdlVHlwZSI6ImxpbmsiLCJ0b2tlbklkIjoiNGIwZTUwY2QtMTFjNC00NmNlLTg3Y2UtZWU5OWFjOTAxZWVkIn0.5Vo10tEa1KpX0SJ7jHJ92Z77jiBfRR0zs6fUhl0s7ew)。

完整的工作流集可以从KNIME Community Hub免费下载，链接为[数据分析职位趋势](https://hub.knime.com/-/spaces/-/latest/~wX_j15juFbQ_-vWC/)。这些工作流可以轻松修改和适应，以发现其他领域的市场趋势。只需更改Excel文件中的搜索关键词、网站和搜索时间范围即可。

结果如何？在今天的数据科学就业市场中，哪些技能和职业角色最受追捧？在我们下一篇博客文章中，我们将引导你探索这一主题建模的结果。我们将深入研究职位角色和技能之间的有趣互动，同时获得关于数据科学就业市场的宝贵见解。敬请关注我们引人入胜的探索！

## 资源

1.  [数据分析职位要求和在线课程的系统性综述](https://www.tandfonline.com/doi/abs/10.1080/08874417.2021.1971579) 由A. Mauro等人撰写。

**[马亨特什·帕塔德卡尔](https://www.linkedin.com/in/mahantesh-pattadkal-7a2607101/)** 拥有超过6年的数据科学项目和产品咨询经验。凭借数据科学硕士学位，他在深度学习、自然语言处理和可解释机器学习方面展现了出色的专长。此外，他积极参与KNIME社区，以便在基于数据科学的项目中进行合作。

[**安德烈亚·德·毛罗**](https://www.linkedin.com/in/andread/)在宝洁和沃达丰等跨国公司拥有超过15年的业务分析和数据科学团队建设经验。除了企业角色外，他还在意大利和瑞士的几所大学教授市场营销分析和应用机器学习。通过他的研究和写作，他探讨了数据和人工智能的商业及社会影响，坚信更广泛的分析素养将使世界变得更好。他的最新著作《数据分析轻松入门》由Packt出版。他还登上了《CDO》杂志2022年全球“40位40岁以下精英”名单。

### 相关主题

+   [5个关键数据科学趋势及分析趋势](https://www.kdnuggets.com/2022/08/5-key-data-science-trends-analytics-trends.html)

+   [数据分析中的职位趋势：第二部分](https://www.kdnuggets.com/job-trends-in-data-analytics-part-2)

+   [掌握NLP职位面试](https://www.kdnuggets.com/2022/10/nlp-interview-questions.html)

+   [机器学习的最佳点：NLP和文档分析中的纯方法](https://www.kdnuggets.com/2022/05/machine-learning-sweet-spot-pure-approaches-nlp-document-analysis.html)

+   [多标签NLP：类别不平衡与损失函数的分析…](https://www.kdnuggets.com/2023/03/multilabel-nlp-analysis-class-imbalance-loss-function-approaches.html)

+   [AI、分析、机器学习、数据科学、深度学习…](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)
