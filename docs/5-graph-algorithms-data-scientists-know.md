# 数据科学家应该知道的 5 种图算法

> 原文：[`www.kdnuggets.com/2019/09/5-graph-algorithms-data-scientists-know.html`](https://www.kdnuggets.com/2019/09/5-graph-algorithms-data-scientists-know.html)

评论![图](img/4b410eb2ecdf09cf1fc254d5fdae2a0d.png)

作为数据科学家，我们对 Pandas、SQL 或任何其他关系型数据库都已经相当熟悉。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

我们习惯于将用户按行显示，属性作为列。但现实世界真的这样吗？

在一个连通的世界中，用户不能被视为独立的实体。他们之间存在某些关系，我们有时希望在构建机器学习模型时包括这些关系。

现在，在关系型数据库中，我们无法在不同的行（用户）之间使用这样的关系，而在图数据库中，这相当简单。

***在这篇文章中，我将讨论一些你应该了解的重要图算法以及如何使用 Python 实现它们。***

此外，这里有一个 [由加州大学圣地亚哥分校提供的 Coursera 上的图分析课程](https://www.coursera.org/learn/big-data-graph-analytics?ranMID=40328&ranEAID=lVarvwc5BD0&ranSiteID=lVarvwc5BD0-uD3tAFL0mCUdzcfwDd6FTQ&siteID=lVarvwc5BD0-uD3tAFL0mCUdzcfwDd6FTQ&utm_content=2&utm_medium=partners&utm_source=linkshare&utm_campaign=lVarvwc5BD0)，我强烈推荐学习图论基础。

### 1\. 连通分量

![图](img/f227f7e9ec2a5381e321bfe86830c792.png)

一个包含 3 个连通分量的图

我们都知道聚类是如何工作的？

*你可以把连通分量看作是一种硬聚类算法，它在相关/连接的数据中找到簇/岛屿。*

*举个具体的例子：****假设你有关于全球各个城市间道路的数据。你需要找出所有大洲以及这些大洲包含哪些城市。***

你将如何实现这一点？来想一想吧。

我们用来实现这一点的连通分量算法基于 **BFS/DFS** 的特殊情况。我在这里不会详细讨论它的工作原理，但我们将看到如何使用 `Networkx` 运行代码。

### 应用

从**零售角度**看：假设我们有很多客户使用很多账户。我们可以使用连通组件算法的一种方式是找出数据集中不同的家庭。

我们可以根据相同的信用卡使用、相同的地址或相同的手机号码等，假设在 CustomerID 之间有边（道路）。一旦有了这些连接，我们可以在其上运行连通组件算法，以创建独立的簇，然后可以为每个簇分配一个家庭 ID。

我们可以利用这些家庭 ID 提供基于家庭需求的个性化推荐。我们还可以使用这些家庭 ID 通过基于家庭创建分组特征来推动分类算法。

从**金融角度**看：另一个用例是使用这些家庭 ID 捕捉欺诈行为。如果一个账户过去曾发生过欺诈，那么与之连接的账户也很可能会受到欺诈的影响。

可能性只受限于你的想象力。

### 代码

我们将使用`Networkx`模块在 Python 中创建和分析我们的图。

让我们从一个示例图开始，该图用于我们的目的。包含城市及其之间的距离信息。

![图](img/0f9a5ab2c75434fd56b2f1054431e5dc.png)

图带有一些随机距离

我们首先开始创建一个包含边及其距离的列表，这些距离将作为边的权重添加：

```py
edgelist = [['Mannheim', 'Frankfurt', 85], ['Mannheim', 'Karlsruhe', 80], ['Erfurt', 'Wurzburg', 186], ['Munchen', 'Numberg', 167], ['Munchen', 'Augsburg', 84], ['Munchen', 'Kassel', 502], ['Numberg', 'Stuttgart', 183], ['Numberg', 'Wurzburg', 103], ['Numberg', 'Munchen', 167], ['Stuttgart', 'Numberg', 183], ['Augsburg', 'Munchen', 84], ['Augsburg', 'Karlsruhe', 250], ['Kassel', 'Munchen', 502], ['Kassel', 'Frankfurt', 173], ['Frankfurt', 'Mannheim', 85], ['Frankfurt', 'Wurzburg', 217], ['Frankfurt', 'Kassel', 173], ['Wurzburg', 'Numberg', 103], ['Wurzburg', 'Erfurt', 186], ['Wurzburg', 'Frankfurt', 217], ['Karlsruhe', 'Mannheim', 80], ['Karlsruhe', 'Augsburg', 250],["Mumbai", "Delhi",400],["Delhi", "Kolkata",500],["Kolkata", "Bangalore",600],["TX", "NY",1200],["ALB", "NY",800]]
```

让我们使用`Networkx`创建一个图：

```py
g = nx.Graph()
for edge in edgelist:
    g.add_edge(edge[0],edge[1], weight = edge[2])
```

现在***我们想要找出图中的不同大陆及其城市。***

我们现在可以使用连通组件算法来实现：

```py
for i, x in enumerate(nx.connected_components(g)):
    print("cc"+str(i)+":",x)
------------------------------------------------------------
cc0: {'Frankfurt', 'Kassel', 'Munchen', 'Numberg', 'Erfurt', 'Stuttgart', 'Karlsruhe', 'Wurzburg', 'Mannheim', 'Augsburg'}
cc1: {'Kolkata', 'Bangalore', 'Mumbai', 'Delhi'}
cc2: {'ALB', 'NY', 'TX'}
```

如你所见，我们能够在数据中找到不同的组件。仅使用边和顶点就能实现。这种算法可以在不同的数据上运行，以满足我之前提出的任何用例。

### 2. 最短路径

![](img/fb28cd8bcd9912a10eeca6e986a6a1a7.png)

继续以上示例，我们得到一个包含德国城市及其间距离的图。

**你想找出如何从法兰克福（起始节点）到慕尼黑，以覆盖最短距离**。

我们用来解决这个问题的算法叫做**Dijkstra**。用 Dijkstra 的话来说：

> 从[鹿特丹](https://en.wikipedia.org/wiki/Rotterdam)到[格罗宁根](https://en.wikipedia.org/wiki/Groningen)的最短路径是什么？一般来说，从给定城市到给定城市的最短路径。[这是最短路径算法](https://en.wikipedia.org/wiki/Shortest_path_problem)，我在大约二十分钟内设计了这个算法。一天早晨，我和我的年轻未婚妻在[阿姆斯特丹](https://en.wikipedia.org/wiki/Amsterdam)购物，累了，我们坐在咖啡馆的露台上喝咖啡，我在想是否能做到这一点，然后我设计了最短路径算法。正如我所说，这是一个二十分钟的发明。实际上，它是在 1959 年发布的，比设计时晚了三年。这个出版物仍然可以阅读，实际上，相当不错。它之所以这么好，其中一个原因是我没有使用铅笔和纸进行设计。我后来了解到，无铅笔和纸的设计方式的一个优点是，你几乎被迫避免所有可避免的复杂性。最终，这个算法成为了我声誉的一个基石，令我感到非常惊讶。
> 
> — 埃德斯格·迪克斯特拉，在与菲利普·L·弗拉纳的访谈中，《ACM 通讯》，2001[[3]](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm#cite_note-Dijkstra_Interview-3)

### 应用

+   Dijkstra 算法的变体在 Google Maps 中被广泛使用，以寻找最短路线。

+   你在沃尔玛商店里。你有不同的过道以及所有过道之间的距离。你想为顾客提供从过道 A 到过道 D 的最短路径。

![](img/0c5ad8c169957881384dc7569124e8ea.png)

+   你已经看到 LinkedIn 如何显示一级连接、二级连接。幕后发生了什么？

![](img/091b0f441ab50491d95c8dbb8a60c49d.png)

### 代码

```py
print(nx.shortest_path(g, 'Stuttgart','Frankfurt',weight='weight'))
print(nx.shortest_path_length(g, 'Stuttgart','Frankfurt',weight='weight'))
--------------------------------------------------------
['Stuttgart', 'Numberg', 'Wurzburg', 'Frankfurt']
503
```

你也可以使用以下方法找到所有对之间的最短路径：

```py
for x in nx.all_pairs_dijkstra_path(g,weight='weight'):
    print(x)
--------------------------------------------------------
('Mannheim', {'Mannheim': ['Mannheim'], 'Frankfurt': ['Mannheim', 'Frankfurt'], 'Karlsruhe': ['Mannheim', 'Karlsruhe'], 'Augsburg': ['Mannheim', 'Karlsruhe', 'Augsburg'], 'Kassel': ['Mannheim', 'Frankfurt', 'Kassel'], 'Wurzburg': ['Mannheim', 'Frankfurt', 'Wurzburg'], 'Munchen': ['Mannheim', 'Karlsruhe', 'Augsburg', 'Munchen'], 'Erfurt': ['Mannheim', 'Frankfurt', 'Wurzburg', 'Erfurt'], 'Numberg': ['Mannheim', 'Frankfurt', 'Wurzburg', 'Numberg'], 'Stuttgart': ['Mannheim', 'Frankfurt', 'Wurzburg', 'Numberg', 'Stuttgart']})('Frankfurt', {'Frankfurt': ['Frankfurt'], 'Mannheim': ['Frankfurt', 'Mannheim'], 'Kassel': ['Frankfurt', 'Kassel'], 'Wurzburg': ['Frankfurt', 'Wurzburg'], 'Karlsruhe': ['Frankfurt', 'Mannheim', 'Karlsruhe'], 'Augsburg': ['Frankfurt', 'Mannheim', 'Karlsruhe', 'Augsburg'], 'Munchen': ['Frankfurt', 'Wurzburg', 'Numberg', 'Munchen'], 'Erfurt': ['Frankfurt', 'Wurzburg', 'Erfurt'], 'Numberg': ['Frankfurt', 'Wurzburg', 'Numberg'], 'Stuttgart': ['Frankfurt', 'Wurzburg', 'Numberg', 'Stuttgart']})....
```

### 3. 最小生成树

![](img/b1c0422c0bda13b46391bdfefab29c8b.png)

现在我们遇到了另一个问题。我们为一家铺设水管或互联网光纤的公司工作。***我们需要使用最少量的电线/管道连接图中的所有城市。***我们该如何做到这一点？

![图](img/677a92f263dd0a398551eac6d666666a.png)

一个无向图及其右侧的最小生成树。

### 应用

+   最小生成树在网络设计中有直接应用，包括计算机网络、通信网络、交通网络、供水网络和电力网（这些网络最初就是为了这些目的发明的）。

+   MST 用于近似旅行商问题

+   聚类——首先构建最小生成树，然后使用簇间距离和簇内距离确定一个阈值，以断开最小生成树中的一些边。

+   图像分割——它用于图像分割，其中我们首先在一个图上构建最小生成树（MST），图中的像素是节点，像素之间的距离基于某些相似度度量（如颜色、强度等）。

### 代码

```py
# nx.minimum_spanning_tree(g) returns a instance of type graph
nx.draw_networkx(nx.minimum_spanning_tree(g))
```

![图示](img/63f765d13c6868568be507c058a2a57f.png)

我们图的最小生成树。

如你所见，上面是我们要铺设的线路。

### 4. PageRank

![](img/59eaabe2b166f762b0b574d3739a17c3.png)

这是长期以来驱动谷歌的页面排序算法。它根据传入和传出链接的数量和质量为页面分配分数。

### 应用

PageRank 可以用于任何需要估计网络中节点重要性的地方。

+   它已经被用来通过引用找到最具影响力的论文。

+   已被谷歌用于排名页面

+   它可以用于排名推文——用户和推文作为节点。如果用户 A 关注用户 B，则在用户之间创建连接；如果用户发布/转发一条推文，则在用户和推文之间创建连接。

+   推荐引擎

### 代码

对于这个练习，我们将使用 Facebook 数据。我们有一个 Facebook 用户之间的边/链接文件。我们首先使用以下代码创建 FB 图：

```py
# reading the datasetfb = nx.read_edgelist('../input/facebook-combined.txt', create_using = nx.Graph(), nodetype = int)
```

它的外观如下：

```py
pos = nx.spring_layout(fb)import warnings
warnings.filterwarnings('ignore')plt.style.use('fivethirtyeight')
plt.rcParams['figure.figsize'] = (20, 15)
plt.axis('off')
nx.draw_networkx(fb, pos, with_labels = False, node_size = 35)
plt.show()
```

![图示](img/a66ef34f848ca9e7a05d55f67fde7989.png)

FB 用户图

现在我们想要找出具有高影响力的用户。

直观地，PageRank 算法会给那些拥有大量朋友且这些朋友又有很多 FB 朋友的用户更高的分数。

```py
pageranks = nx.pagerank(fb)
print(pageranks)
------------------------------------------------------
{0: 0.006289602618466542,
 1: 0.00023590202311540972,
 2: 0.00020310565091694562,
 3: 0.00022552359869430617,
 4: 0.00023849264701222462,
........}
```

我们可以使用排序后的 PageRank 或最具影响力的用户：

```py
import operator
sorted_pagerank = sorted(pagerank.items(), key=operator.itemgetter(1),reverse = True)
print(sorted_pagerank)
------------------------------------------------------
[(3437, 0.007614586844749603), (107, 0.006936420955866114), (1684, 0.0063671621383068295), (0, 0.006289602618466542), (1912, 0.0038769716008844974), (348, 0.0023480969727805783), (686, 0.0022193592598000193), (3980, 0.002170323579009993), (414, 0.0018002990470702262), (698, 0.0013171153138368807), (483, 0.0012974283300616082), (3830, 0.0011844348977671688), (376, 0.0009014073664792464), (2047, 0.000841029154597401), (56, 0.0008039024292749443), (25, 0.000800412660519768), (828, 0.0007886905420662135), (322, 0.0007867992190291396),......]
```

上面的 ID 是最具影响力的用户。

我们可以看到最具影响力用户的子图：

```py
first_degree_connected_nodes = list(fb.neighbors(3437))
second_degree_connected_nodes = []
for x in first_degree_connected_nodes:
    second_degree_connected_nodes+=list(fb.neighbors(x))
second_degree_connected_nodes.remove(3437)
second_degree_connected_nodes = list(set(second_degree_connected_nodes))subgraph_3437 = nx.subgraph(fb,first_degree_connected_nodes+second_degree_connected_nodes)pos = nx.spring_layout(subgraph_3437)node_color = ['yellow' if v == 3437 else 'red' for v in subgraph_3437]
node_size =  [1000 if v == 3437 else 35 for v in subgraph_3437]
plt.style.use('fivethirtyeight')
plt.rcParams['figure.figsize'] = (20, 15)
plt.axis('off')nx.draw_networkx(subgraph_3437, pos, with_labels = False, node_color=node_color,node_size=node_size )
plt.show()
```

![图示](img/65588b2ba8f7f2fbfdf7deadaa850856.png)

我们最具影响力的用户（黄色）

### 5. **中心性度量**

有很多中心性度量可以作为机器学习模型的特征。我将讨论其中两个。你可以在[这里](https://networkx.github.io/documentation/networkx-1.10/reference/algorithms.centrality.html#current-flow-closeness)查看其他度量。

**中介中心性：** 不仅仅是拥有最多朋友的用户很重要，连接一个地理位置到另一个地理位置的用户也很重要，因为这让用户可以看到来自不同地理位置的内容。***中介中心性量化了一个特定节点在两个其他节点之间的最短路径中出现的次数。***

**度中心性：** 这是节点的连接数量。

### 应用

中心性度量可以作为任何机器学习模型的特征。

### 代码

这里是用于找到子图中介中心性的代码。

```py
pos = nx.spring_layout(subgraph_3437)
betweennessCentrality = nx.betweenness_centrality(subgraph_3437,normalized=True, endpoints=True)node_size =  [v * 10000 for v in betweennessCentrality.values()]
plt.figure(figsize=(20,20))
nx.draw_networkx(subgraph_3437, pos=pos, with_labels=False,
                 node_size=node_size )
plt.axis('off')
```

![](img/f099b9fa5e8842ff2778748973ef73a7.png)

你可以在这里看到按其中介中心性值大小显示的节点。它们可以被认为是信息传递者。打破任何一个具有高中介中心性的节点将会把图分割成许多部分。

### 结论

***在这篇文章中，我谈到了改变我们生活方式的一些最具影响力的图算法。***

随着大量社交数据的出现，网络分析可以在改善我们的模型和创造价值方面发挥很大作用。

***甚至对世界有更多的了解。***

有很多图算法，但这些是我最喜欢的。如果你喜欢的话，可以更详细地了解这些算法。在这篇文章中，我只是想介绍这一领域的基本知识。

如果你觉得我遗漏了你最喜欢的算法，请在评论中告诉我。

这里是包含完整代码的 [Kaggle Kernel](https://www.kaggle.com/mlwhiz/top-graph-algorithms)。

如果你想深入了解图算法，这里有一个由 UCSanDiego 提供的[大数据图分析课程](https://www.coursera.org/learn/big-data-graph-analytics?ranMID=40328&ranEAID=lVarvwc5BD0&ranSiteID=lVarvwc5BD0-uD3tAFL0mCUdzcfwDd6FTQ&siteID=lVarvwc5BD0-uD3tAFL0mCUdzcfwDd6FTQ&utm_content=2&utm_medium=partners&utm_source=linkshare&utm_campaign=lVarvwc5BD0)，我强烈推荐学习图论基础。

感谢阅读。我将来还会写更多适合初学者的文章。关注我在 [**Medium**](https://medium.com/@rahul_agarwal?source=post_page---------------------------) 或订阅我的 [**博客**](http://eepurl.com/dbQnuX?source=post_page---------------------------) 以获取相关信息。像往常一样，我欢迎反馈和建设性的批评，可以通过 Twitter 联系我 [@mlwhiz](https://twitter.com/MLWhiz?source=post_page---------------------------)。

**个人简介: [Rahul Agarwal](https://www.linkedin.com/in/rahulagwl/)** 是沃尔玛实验室的数据科学家。

[原文](https://towardsdatascience.com/data-scientists-the-five-graph-algorithms-that-you-should-know-30f454fa5513)。经许可转载。

**相关:**

+   10 个适合有抱负的数据科学家的 Python 资源

+   用 100 行代码编写随机森林*

+   用世界填充 GRAKN.AI 知识图谱

### 更多相关内容

+   [每个数据科学家都应该知道的三大 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [一个 90 亿美元的 AI 失败，经过审视](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
