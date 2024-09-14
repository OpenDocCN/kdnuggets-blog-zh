# 使用R进行过程挖掘：介绍

> 原文：[https://www.kdnuggets.com/2017/11/process-mining-r-introduction.html/2](https://www.kdnuggets.com/2017/11/process-mining-r-introduction.html/2)

然后，我们可以从这些信息创建一个igraph对象，并导出它以使用Graphviz渲染，从而得到以下过程图（片段）：

![](../Images/9c345ba368aa3995a51390fa9649ced0.png)

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 进军网络安全事业快速通道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT方面支持您的组织

* * *

现在，我们可以进一步扩展这个基本框架，并将其与预测性和描述性分析模型合并。一些实验的想法包括：

+   筛选事件日志和流程图，只显示高频行为

+   尝试不同的着色方式，例如不基于频率或性能，而是其他有趣的数据属性

+   扩展框架以包括基本的符合性检查，例如通过突出显示偏差流程

+   将决策挖掘（或概率度量）应用于分流，以预测将遵循哪些路径

现在，我们来应用一个简单的扩展：假设我们对“分析报价请求”活动感兴趣，并希望带入一些与该活动相关的更多信息，例如资源属性，即执行该活动的人员。

因此，我们使用频率计数构建了两个新的数据框，将频率计数作为我们感兴趣的指标：

```py
col.box.blue <- colorRampPalette(c('#DBD8E0', '#014477'))(20)

col.arc.blue <- colorRampPalette(c('#938D8D', '#292929'))(20)

activities.counts <- activities.basic %>%

select(act) %>%

group_by_all %>%

summarize(metric=n()) %>%

ungroup %>%

mutate(fillcolor=col.box.blue[floor(linMap(metric, 1,20))])

edges.counts <- edges.basic %>%

select(a.act, b.act) %>%

group_by_all %>%

summarize(metric=n()) %>%

ungroup %>%

mutate(color=col.arc.blue[floor(linMap(metric, 1,20))],

penwidth=floor(linMap(metric, 1, 5)),

metric.char=as.character(metric))

```

接下来，我们筛选我们感兴趣的活动的事件日志，计算持续时间，然后为每个执行此活动的资源-活动对计算频率和性能指标。

```py
acts.res <- eventlog %>%

filter(Activity == "Analyze Request for Quotation") %>%

mutate(Duration=Complete-Start) %>%

select(Duration, Resource, Activity) %>%

group_by(Resource, Activity) %>%

summarize(metric.freq=n(), metric.perf=median(Duration)) %>%

ungroup %>%

mutate(color='#75B779',

penwidth=1,

metric.char=formatSeconds(as.numeric(metric.perf)))

```

在这里使用的一个有用的igraph函数是“graph_from_data_frame”。此函数期望一个顶点的数据框（除了第一列以外的所有列自动充当属性），并期望一个边的数据框（除了前两列以外的所有列自动充当属性）。因此，我们构造了最终的顶点（活动）数据框如下所示。填充颜色基于节点是否表示一个活动（我们使用上面设置的颜色）或一个资源（我们使用绿色）：

```py
a <- bind_rows(activities.counts %>% select(name=act, metric=metric) %>%
mutate(type='Activity'),

acts.res %>% select(name=Resource, metric=metric.freq) %>% 
mutate(type='Resource')) %>%

distinct %>%

rowwise %>%

mutate(fontsize=8,

fontname='Arial',

label=paste(name, metric, sep='\n'),

shape=ifelse(type == 'Activity', 'box', 'ellipse'),

style=ifelse(type == 'Activity', 'rounded,filled', 'solid,filled'),

fillcolor=ifelse(type == 'Activity',
activities.counts[activities.counts$act==name,]$fillcolor, '#75B779'))

And construct a data frame for the edges as well:

e <- bind_rows(edges.counts,

acts.res %>% select(a.act=Resource, b.act=Activity, metric.char, color, penwidth)) %>%

rename(label=metric.char) %>%

mutate(fontsize=8, fontname='Arial')

Finally, we can construct an igraph object as follows:

gh <- graph_from_data_frame(e, vertices=a, directed=T)

```

导出和绘制此对象后，我们看到所有三个资源都对我们的活动的执行做出了相同的贡献，它们的中位执行时间（显示在绿色弧上）也相似：

![](../Images/f5c4bac2dc25a04937358e1b8198c1aa.png)

当然，我们现在已经有一个强大的框架，可以从中开始创建更丰富的过程图，并嵌入更多注释和信息。

在这篇文章中，我们提供了一个快速的旋风之旅，介绍了如何使用R创建丰富的流程图。正如所见，这在典型的流程发现工具不可用的情况下可能非常有用，同时还提供了关于如何将典型的流程挖掘任务与其他与数据挖掘相关的任务进行增强或更容易组合的提示。希望您喜欢阅读这篇文章，并迫不及待地想看看您会如何处理您的事件日志。

**更多阅读：**

+   vanden Broucke S., Vanthienen J., Baesens B. (2013). Volvo IT比利时 VINST. 第3届业务流程智能挑战赛论文集合，与第9届国际业务流程智能研讨会（BPI 2013）共同举办。北京（中国），2013年8月26日（编号3）。亚琛（德国）：亚琛工业大学。[http://ceur-ws.org/Vol-1052/paper3.pdf](http://ceur-ws.org/Vol-1052/paper3.pdf)

+   [http://www.dataminingapps.com/dma_research/process-analytics/](http://www.dataminingapps.com/dma_research/process-analytics/)

**个人简介：[Seppe vanden Broucke](https://www.seppe.net)** 是比利时鲁汶大学经济与商业学院的助理教授。他的研究兴趣包括商业数据挖掘和分析、机器学习、流程管理和流程挖掘。他的工作已发表在知名国际期刊上，并在顶级会议上做过报告。Seppe的教学包括高级分析、大数据和信息管理课程。他还经常为行业和企业的听众进行教学。

**相关文章：**

+   [流程挖掘中的隐私、安全性和伦理](/2016/12/privacy-security-ethics-process-mining.html)

+   [流程挖掘：数据科学与流程科学的交叉点](/2016/11/process-mining-coursera-course.html)

+   [通过统计模型改进您的流程](/2016/05/jmp-improve-processes-statistical-models.html)

### 关于这个话题的更多内容

+   [如何在几秒钟内处理包含数百万行的DataFrame](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

+   [处理机器学习过程的框架方法](https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html)

+   [Python中处理CSV文件的3种方法](https://www.kdnuggets.com/2022/10/3-ways-process-csv-files-python.html)

+   [自然语言处理简介](https://www.kdnuggets.com/2022/06/gentle-introduction-natural-language-processing.html)

+   [AI中的爬山算法简介](https://www.kdnuggets.com/2022/07/introduction-hill-climbing-algorithm-ai.html)

+   [SMOTE简介](https://www.kdnuggets.com/2022/11/introduction-smote.html)
