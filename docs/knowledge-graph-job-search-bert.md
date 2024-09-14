# 使用BERT构建职位搜索知识图谱

> 原文：[https://www.kdnuggets.com/2021/06/knowledge-graph-job-search-bert.html](https://www.kdnuggets.com/2021/06/knowledge-graph-job-search-bert.html)

[评论](#comments)

**由[Walid Amamou](https://www.linkedin.com/in/walid-amamou-b65105b9/)，UBIAI创始人**

![图示](../Images/b8badc45ee277fc3662f0bddcc1bae93.png)

知识图谱网络

### 介绍：

尽管NLP领域在过去两年中由于转移学习模型的发展而呈指数增长，但这些模型在职位搜索领域的应用范围仍然有限。LinkedIn作为职位搜索和招聘领域的领先公司就是一个很好的例子。尽管我拥有材料科学的博士学位和物理学的硕士学位，但我却收到如MongoDB的技术项目经理和Toptal的Go开发者职位的推荐，这些都是与我的背景不相关的网络开发公司。这种不相关感被许多用户所共有，并且是一个主要的挫折来源。

![LinkedIn职位推荐](../Images/2cd1dbe1c05318b0f79eaeb68dca6fe5.png)

LinkedIn职位推荐

求职者应有机会使用最佳工具，以帮助他们找到与自身背景最匹配的职位，而不浪费时间在不相关的推荐和手动搜索上……

然而，总体而言，传统的职位推荐系统基于简单的关键词和/或语义相似性，这些方法通常不适合提供良好的职位推荐，因为它们未考虑实体之间的相互联系。此外，随着申请者跟踪系统（ATS）的兴起，在简历中列出与领域相关的技能以及揭示哪些行业技能变得更加重要至关重要。例如，我可能在Python编程方面具有广泛的技能，但感兴趣的职位描述要求对Django框架有了解，而Django框架本质上基于Python；简单的关键词搜索将无法发现这种联系。

在本教程中，我们将构建一个职位推荐和技能发现脚本，该脚本将接受非结构化文本作为输入，然后根据技能、工作经验年限、学位和专业等实体输出职位推荐和技能建议。在[我之前的文章](https://towardsdatascience.com/how-to-train-a-joint-entities-and-relation-extraction-classifier-using-bert-transformer-with-spacy-49eb08d91b5c)的基础上，我们将使用BERT模型从职位描述中提取实体和关系，并尝试从技能和经验年限中构建知识图谱。

![职业分析流程](../Images/415726197a797205a6164805956bccea.png)

职业分析流程

为了训练NER和关系提取模型，我们使用了[UBIAI工具](https://ubiai.tools/)进行了数据注释，并在Google Colab上进行了模型训练，如我之前的文章所述。

### 数据提取：

在本教程中，我收集了来自5家主要公司的与软件工程、硬件工程和研究相关的职位描述：Facebook、谷歌、微软、IBM和英特尔。数据存储在一个csv文件中。

为了从职位描述中提取实体和关系，我创建了一个命名实体识别（NER）和关系抽取管道，使用了之前训练的变换器模型（有关更多信息，请查看[我的上一篇文章](https://towardsdatascience.com/how-to-train-a-joint-entities-and-relation-extraction-classifier-using-bert-transformer-with-spacy-49eb08d91b5c)）。我们将把提取出的实体存储在一个JSON文件中，以便进一步分析，使用以下代码。

```py
def analyze(text):   experience_year=[]
   experience_skills=[]
   diploma=[]
   diploma_major=[]   for doc in nlp.pipe(text, disable=[**"tagger"**]):
      skills = [e.text for e in doc.ents if e.label_ == **'SKILLS'**]
   for name, proc in nlp2.pipeline:
         doc = proc(doc)
   for value, rel_dict in doc._.rel.items():
      for e in doc.ents:
         for b in doc.ents:
            if e.start == value[0] and b.start == value[1]:
               if rel_dict[**'EXPERIENCE_IN'**] >= 0.9:
                  experience_skills.append(b.text)
                  experience_year.append(e.text)
               if rel_dict[**'DEGREE_IN'**] >= 0.9:
                  diploma_major.append(b.text)
                  diploma.append(e.text)
   return skills, experience_skills, experience_year, diploma, diploma_majordef analyze_jobs(item):
      with open(**'./path_to_job_descriptions'**, **'w'**, encoding=**'utf-8'**) as file:
         file.write(**'['**)
         for i,row in enumerate(item[**'Description'**]):
               try:
                  skill, experience_skills, experience_year, diploma, diploma_major=analyze([row])
                  data=json.dumps({**'Job ID'**:item[**'JOBID'**[i],**'Title'**:item[**'Title'**[i],**'Location'**:item[**'Location'**][i],**'Link'**:item[**'Link'**][i],**'Category'**:item[**'Category'**[i],**'document'**:row, **'skills'**:skill, **'experience skills'**:experience_skills, **'experience years'**: experience_year, **'diploma'**:diploma, **'diploma_major'**:diploma_major}, ensure_ascii=False)
                  file.write(data)
                  file.write(**','**)

               except:
                  continue
         file.write(**']'**)analyze_jobs(path)
```

### 数据探索：

提取职位描述中的实体后，我们现在可以开始探索数据。首先，我有兴趣了解在多个领域中所需学位的分布。在下方的箱形图中，我们注意到一些事情：在软件工程领域，最受欢迎的学位是学士学位，其次是硕士学位和博士学位。而在研究领域，博士学位和硕士学位的需求则更多，这是我们预期的结果。对于硬件工程，分布则较为均匀。这可能看起来很直观，但值得注意的是，我们只用几行代码就从完全非结构化的文本中自动获得了这些结构化的数据！

![各领域的学位分布](../Images/c233c77e90d522d1524a9e866f52fe4c.png)

各领域的学位分布

我有兴趣了解哪些公司在寻找物理学和材料科学的博士，因为我的背景正好涵盖这两个专业。我们看到谷歌和英特尔在寻找这类博士。Facebook则更关注计算机科学和电气工程领域的博士。请注意，由于数据集的样本量较小，这种分布可能并不代表真实的分布。更大的样本量肯定会得到更好的结果，但这超出了本教程的范围。

![学位专业分布](../Images/30eccf5299671263c48a58deee60a24c.png)

学位专业分布

由于这是关于NLP的教程，让我们看看当提到“NLP”或“自然语言处理”时需要哪些学位和专业：

```py
#Diploma
('Master', 54), ('PHD', 49),('Bachelor', 19)#Diploma major:
('Computer Science', 36),('engineering', 12), ('Machine Learning', 9),('Statistics', 8),('AI', 6)
```

提到NLP的公司在寻找具有计算机科学、工程、机器学习或统计学硕士或博士学位的候选人。另一方面，对学士学位的需求较少。

### 知识图谱

利用提取的技能和经验年限，我们现在可以构建一个知识图谱，其中源节点是职位描述ID，目标节点是技能，连接的强度是经验年限。我们使用python库pyvis和networkx来构建我们的图谱；我们将职位描述与提取的技能链接，使用经验年限作为权重。

```py
job_net = Network(height=**'1000px'**, width=**'100%'**, bgcolor=**'#222222'**, font_color=**'white'**)

job_net.barnes_hut()
sources = data_graph[**'Job ID'**]
targets = data_graph[**'skills'**]
values=data_graph[**'years skills'**]
sources_resume = data_graph_resume[**'document'**]
targets_resume = data_graph_resume[**'skills'**]

edge_data = zip(sources, targets, values )
resume_edge=zip(sources_resume, targets_resume)
for j,e in enumerate(edge_data):
    src = e[0]
    dst = e[1]
    w = e[2]
    job_net.add_node(src, src, color=**'#dd4b39'**, title=src)
    job_net.add_node(dst, dst, title=dst)
    if str(w).isdigit():
        if w is None:

            job_net.add_edge(src, dst, value=w, color=**'#00ff1e'**, label=w)
        if 1<w<=5:
            job_net.add_edge(src, dst, value=w, color=**'#FFFF00'**, label=w)
        if w>5:
            job_net.add_edge(src, dst, value=w, color=**'#dd4b39'**, label=w)

    else:
        job_net.add_edge(src, dst, value=0.1, dashes=True)for j,e in enumerate(resume_edge):
    src = **'resume'** dst = e[1]

    job_net.add_node(src, src, color=**'#dd4b39'**, title=src)
    job_net.add_node(dst, dst, title=dst)
    job_net.add_edge(src, dst, color=**'#00ff1e'**)neighbor_map = job_net.get_adj_list()for node in job_net.nodes:
    node[**'title'**] += **' Neighbors:<br>'** + **'<br>'**.join(neighbor_map[node[**'id'**]])
    node[**'value'**] = len(neighbor_map[node[**'id'**]])# add neighbor data to node hover data
job_net.show_buttons(filter_=[**'physics'**])
job_net.show(**'job_knolwedge_graph.html'**)
```

让我们可视化我们的知识图谱！为了清晰起见，我仅显示了知识图谱中的一些职位。在这个测试中，我使用了一个机器学习领域的样本简历。

红色节点是来源，可以是职位描述或简历。蓝色节点是技能。连接的颜色和标签表示所需的经验年限（黄色 = 1–5年；红色 = > 5年；虚线 = 无经验）。在下面的例子中，Python 将简历与4个职位相连接，这些职位都需要2年的经验。对于机器学习连接，无需经验。我们现在可以开始从我们的非结构化文本中获得有价值的见解！

![知识图谱](../Images/646fdb09c0a785af19f9f10a6ee20433.png)

知识图谱

让我们找出哪些职位与简历的联系最多：

```py
# JOB ID            #Connections
GO4919194241794048      7
GO5957370192396288      7
GO5859529717907456      7
GO5266284713148416      7
FB189313482022978       7
FB386661248778231       7
```

现在让我们看看包含少量最高匹配的知识图谱网络：

![最高职位匹配的知识图谱](../Images/b5d038c12ee5ce9e84a29185daeda012.png)

最高职位匹配的知识图谱

注意到在这个案例中（该教程中未进行），共指解析的重要性。技能“机器学习”、“机器学习模型”和“机器学习”被计算为不同的技能，但它们显然是相同的技能，应当计为一项。这会导致我们的匹配算法不准确，并突显了在进行NER提取时共指解析的重要性。

话虽如此，通过知识图谱我们可以直接看到 GO5957370192396288 和 GO5859529717907456 都是很好的匹配，因为它们不需要广泛的经验，而 FB189313482022978 需要2–4年的各种技能经验。瞧！

### 技能增强

现在我们已经识别出简历与职位描述之间的联系，目标是发现那些可能未出现在简历中但对我们分析的领域重要的相关技能。为此，我们按领域过滤职位描述——即软件工程、硬件工程和研究。接下来，我们查询所有与简历技能相关的邻近职位，并为每个找到的职位提取相关技能。为了清晰的可视化，我将词频绘制成了词云。让我们来看一下软件工程领域：

![软件工程技能词云](../Images/9859251b68539b8ee48889c9505ed923.png)

软件工程中的技能词云

注意到 Spark、SOLR 和 PLSQL 在与简历相关的职位中被频繁提及，可能对该领域很重要。

另一方面，对于硬件工程：

![硬件工程技能词云](../Images/54de32cd7b2816072358fcba61a0c33e.png)

硬件工程中的技能词云

设计、RF 和 RFIC 是这里的确切需求。

对于研究领域：

![研究中的技能词云](../Images/4caba12ad0c177ee94149a7ef22f92bc.png)

研究中技能的词云

热门技能包括机器学习、信号处理、TensorFlow、PyTorch、模型分析等…

仅需几行代码，我们便将非结构化数据转化为结构化信息，并提取了有价值的见解！

### 结论：

随着 NLP 领域的最新突破——无论是命名实体识别、关系分类、问答还是文本分类——对于公司而言，应用 NLP 以保持竞争力已成为一种必要。

在本教程中，我们使用 NER 和关系提取模型（使用 BERT transformer）构建了一个职位推荐和技能发现应用。我们通过构建一个连接职位和技能的知识图谱实现了这一目标。

知识图谱与 NLP 相结合，为数据挖掘和发现提供了强大的工具。欢迎分享您展示 NLP 如何应用于不同领域的用例。如果您有任何问题或想为您的具体情况创建自定义模型，请在下方留言或发送邮件至 admin@ubiai.tools。

**个人简介：[Walid Amamou](https://www.linkedin.com/in/walid-amamou-b65105b9/)** 是 UBIAI 的创始人，该公司开发了用于 NLP 应用的注释工具，并拥有物理学博士学位。

[原文](https://medium.com/mlearning-ai/building-a-knowledge-graph-for-job-search-using-bert-transformer-8677c8b3a2e7)。经授权转载。

**相关内容：**

+   [如何使用 spaCy 3 微调 BERT Transformer](/2021/06/fine-tune-bert-transformer-spacy.html)

+   [如何通过 API 创建和部署一个简单的情感分析应用](/2021/06/create-deploy-sentiment-analysis-app-api.html)

+   [如何将 Transformer 应用于任意长度的文本](/2021/04/apply-transformers-any-length-text.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 了解更多相关话题

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [8 篇创新的 BERT 知识蒸馏论文，它们改变了…](https://www.kdnuggets.com/2022/09/eight-innovative-bert-knowledge-distillation-papers-changed-nlp-landscape.html)

+   [使用 Python 进行网格搜索和随机搜索的超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过 Uplimit 的 ML 搜索课程提升您的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [使用 BERT 对长文本进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [利用 BERT 的 LLM 提取式摘要](https://www.kdnuggets.com/extractive-summarization-with-llm-using-bert)
