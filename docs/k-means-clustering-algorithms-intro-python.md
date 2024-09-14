# K-Means 和其他聚类算法：使用 Python 的快速介绍

> 原文：[https://www.kdnuggets.com/2017/03/k-means-clustering-algorithms-intro-python.html](https://www.kdnuggets.com/2017/03/k-means-clustering-algorithms-intro-python.html)

**Nikos Koufos，[LearnDataSci](http://www.learndatasci.com/) 作者。**

聚类是将对象分组，使得同一组（簇）中的对象彼此之间的相似度高于其他组（簇）中的对象。在本次介绍性聚类分析教程中，我们将查看一些 Python 中的算法，以便你对实际数据集上的聚类基础知识有一个基本了解。

### 数据集

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

对于聚类问题，我们将使用著名的 *Zachary’s Karate Club* 数据集。数据集背后的故事很简单：曾经有一个空手道俱乐部，俱乐部中有一位管理员“John A”和一位教练“Mr. Hi”（均为化名）。然后他们之间发生了冲突，导致学生（节点）分成了两组。一组跟随 John，另一组跟随 Mr. Hi。

![空手道俱乐部聚类的可视化](../Images/f34f92e5fccc0a8debafe667480bffb6.png)

来源：[维基百科](https://en.wikipedia.org/wiki/Zachary's_karate_club)

### 使用 Python 开始进行聚类

不过，讲解到此为止，我们直接进入你来到这里的主要原因——代码本身。首先，你需要安装 [scikit-learn](http://scikit-learn.org/) 和 [networkx](https://networkx.github.io/) 库来完成本教程。如果你不知道怎么做，上面的链接应该能帮到你。另外，你可以通过 [Github](https://github.com/LearnDataSci/blog-post-resources/blob/master/Karate%20Club%20Clustering/Classifiers.py) 获取本教程的源代码并跟随操作。

通常，我们想要检查的数据集以文本形式（JSON、Excel、简单的 txt 文件等）提供，但在我们的案例中，[networkx](https://networkx.github.io/) 为我们提供了它。此外，为了比较我们的算法，我们希望知道成员之间的真实关系（谁跟随谁），但遗憾的是没有提供。不过，通过这两行代码，你将能够加载数据并存储真实情况（从现在起我们称之为真实数据）：

```py
# Load and Store both data and groundtruth of Zachary's Karate Club
G = nx.karate_club_graph()
groundTruth = [0,0,0,0,0,0,0,0,1,1,0,0,0,0,1,1,0,0,1,0,1,0,1,1,1,1,1,1,1,1,1,1,1,1]

```

数据预处理的最后一步是将图形转换为矩阵（这是我们算法所需的输入）。这也非常简单：

```py
def graphToEdgeMatrix(G):

    # Initialize Edge Matrix
    edgeMat = [[0 for x in range(len(G))] for y in range(len(G))]

    # For loop to set 0 or 1 ( diagonal elements are set to 1)
    for node in G:
        tempNeighList = G.neighbors(node)
        for neighbor in tempNeighList:
            edgeMat[node][neighbor] = 1
        edgeMat[node][node] = 1

    return edgeMat

```

在我们深入聚类技术之前，我希望你能对我们的数据有一个可视化。所以，让我们编写一个简单的函数来实现这一点：

```py
def drawCommunities(G, partition, pos):
    # G is graph in networkx form
    # Partition is a dict containing info on clusters
    # Pos is base on networkx spring layout (nx.spring_layout(G))

    # For separating communities colors
    dictList = defaultdict(list)
    nodelist = []
    for node, com in partition.items():
        dictList[com].append(node)

    # Get size of Communities
    size = len(set(partition.values()))

    # For loop to assign communities colors
    for i in range(size):

        amplifier = i % 3
        multi = (i / 3) * 0.3

        red = green = blue = 0

        if amplifier == 0:
            red = 0.1 + multi
        elif amplifier == 1:
            green = 0.1 + multi
        else:
            blue = 0.1 + multi

        # Draw Nodes
        nx.draw_networkx_nodes(G, pos,
                               nodelist=dictList[i],
                               node_color=[0.0 + red, 0.0 + green, 0.0 + blue],
                               node_size=500,
                               alpha=0.8)

    # Draw edges and final plot
    plt.title("Zachary's Karate Club")
    nx.draw_networkx_edges(G, pos, alpha=0.5)

```

这个函数的作用是简单地提取结果中的聚类数量，然后为每个聚类分配不同的颜色（对于给定时间最多10个颜色即可），然后进行绘图。

![zacharys karate club cluster nodes](../Images/3f4fd1bd77a0f8eccd8114e9294a882b.png)

### 聚类算法

一些聚类算法会很好地对你的数据进行聚类，而另一些可能会失败。这是聚类问题如此困难的主要原因之一。但不用担心，我们不会让你在选择的海洋中迷失。我们将讨论几种已知表现非常好的算法。

**K-均值聚类**

![k-means clustering](../Images/2320bd58c7dced4d4b53fa875a2d2e61.png)

来源：[github.com/nitoyon/tech.nitoyon.com](https://github.com/nitoyon/tech.nitoyon.com)

K-均值被许多人认为是聚类的黄金标准，因为它的简单性和性能，它是我们将尝试的第一个算法。当你完全不知道使用哪个算法时，K-均值通常是首选。请记住，K-均值有时可能表现不佳，因为它的概念是：球形聚类可分离，以便均值收敛到聚类中心。要简单地构建和训练一个 K-均值模型，请使用以下几行：

```py
# K-means Clustering Model
kmeans = cluster.KMeans(n_clusters=kClusters, n_init=200)
kmeans.fit(edgeMat)

# Transform our data to list form and store them in results list
results.append(list(kmeans.labels_))

```

**聚合聚类**

聚合聚类的主要思想是每个节点从自己的聚类开始，然后递归地与增加最小链接距离的聚类对合并。聚合聚类（以及一般的层次聚类）的主要优点是你不需要指定聚类的数量。当然，这也有代价：性能。不过，在 scikit 的实现中，你可以指定聚类的数量以帮助算法的性能。要创建和训练一个聚合模型，请使用以下代码：

```py
# Agglomerative Clustering Model
agglomerative = cluster.AgglomerativeClustering(n_clusters=kClusters, linkage="ward")
agglomerative.fit(edgeMat)

# Transform our data to list form and store them in results list
results.append(list(agglomerative.labels_))

```

**光谱**

光谱聚类技术将聚类应用于归一化拉普拉斯的投影。在图像聚类方面，光谱聚类效果非常好。请查看以下几行 Python 代码，了解所有的魔法：

```py
# Spectral Clustering Model
spectral = cluster.SpectralClustering(n_clusters=kClusters, affinity="precomputed", n_init= 200)
spectral.fit(edgeMat)

# Transform our data to list form and store them in results list
results.append(list(spectral.labels_))

```

**亲和传播**

这个算法有些不同。与之前的算法不同，你会发现 AF 在运行算法之前不需要确定聚类的数量。AF 在处理许多计算机视觉和生物学问题上表现非常好，比如人脸图像聚类和识别调控转录本：

```py
# Affinity Propagation Clustering Model
affinity = cluster.affinity_propagation(S=edgeMat, max_iter=200, damping=0.6)

# Transform our data to list form and store them in results list
results.append(list(affinity[1]))

```

### 指标与绘图

现在，是时候选择哪个算法更适合我们的数据了。对于小数据集，简单的可视化结果可能效果不错，但试想一下有一千个甚至一万个节点的图形。这对于人眼来说会有点混乱。所以，让我展示如何计算调整兰德指数（ARS）和归一化互信息（NMI）：

```py
# Append the results into lists
for x in results:
    nmiResults.append(normalized_mutual_info_score(groundTruth, x))
    arsResults.append(adjusted_rand_score(groundTruth, x))

```

如果你对这些指标不太熟悉，这里有一个简短的解释：

**归一化互信息（NMI）**

两个随机变量的互信息是衡量这两个变量之间相互依赖的度量。归一化互信息是对互信息（MI）得分进行归一化，将结果缩放到0（无互信息）到1（完美相关）之间。换句话说，0表示不相似，1表示完全匹配。

**调整兰德指数（ARS）**

调整兰德指数则通过考虑所有样本对，并统计在预测和真实聚类中分配到相同或不同聚类的样本对，来计算两个聚类之间的相似度。如果这有点难以理解，请记住，目前的0表示最低相似度，而1表示最高相似度。

因此，为了得到这些指标（NMI和ARS）的组合，我们只需计算它们总和的平均值。记住，数值越高，结果越好。

下面，我绘制了评分评估图，以便我们能更好地理解我们的结果。我们可以用很多方式绘制它们，比如点图、折线图，但我认为条形图在我们的情况下更好。为此，只需使用以下代码：

```py
# Code for plotting results

# Average of NMI and ARS
y = [sum(x) / 2 for x in zip(nmiResults, arsResults)]

xlabels = ['Spectral', 'Agglomerative', 'Kmeans', 'Affinity Propagation']

fig = plt.figure()
ax = fig.add_subplot(111)

# Set parameters for plotting
ind = np.arange(len(y))
width = 0.35

# Create barchart and set the axis limits and titles
ax.bar(ind, y, width,color='blue', error_kw=dict(elinewidth=2, ecolor='red'))
ax.set_xlim(-width, len(ind)+width)
ax.set_ylim(0,2)
ax.set_ylabel('Average Score (NMI, ARS)')
ax.set_title('Score Evaluation')

# Add the xlabels to the chart
ax.set_xticks(ind + width / 2)
xtickNames = ax.set_xticklabels(xlabels)
plt.setp(xtickNames, fontsize=12)

# Add the actual value on top of each chart
for i, v in enumerate(y):
    ax.text( i, v, str(round(v, 2)), color='blue', fontweight='bold')

# Show the final plot
plt.show()

```

正如你在下方图表中看到的，K-means和层次聚类在我们的数据集中表现最佳（最佳可能结果）。当然，这并不意味着谱聚类和AF算法表现较差，只是它们不适合我们的数据。

![聚类评分评估](../Images/bc767fb2612fb0eac38cb7a8b335e5bb.png)

好了，这就是全部内容！

感谢你参与这个聚类入门。我希望你在观察如何轻松操作公共数据集并在Python中应用几种不同的聚类算法时，能够发现一些价值。如果你有任何问题，请在下方评论区告诉我，也可以随意附上你尝试过的聚类项目！

**个人简介：尼科斯·库福斯** 是 **[LearnDataSci](http://www.learndatasci.com/)** 的作者，希腊伊奥尼纳大学计算机科学与工程研究生，并担任计算机科学本科教学助理。

[原文](http://www.learndatasci.com/k-means-clustering-algorithms-python-intro/)。转载已获许可。

**相关：**

+   [比较聚类技术：简明技术概述](/2016/09/comparing-clustering-techniques-concise-technical-overview.html)

+   [使用聚类自动分割数据](/2017/02/automatically-segmenting-data-clustering.html)

+   [聚类关键术语解析](/2016/10/clustering-key-terms-explained.html)

### 更多相关主题

+   [聚类释放：理解k-means 聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [什么是k-means 聚类及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [动手实践无监督学习：k-means 聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [快速指南：如何找到适合注释的头脑](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)

+   [快速数据科学技巧和窍门以学习SAS](https://www.kdnuggets.com/2022/05/sas-quick-data-science-tips-tricks-learn.html)
