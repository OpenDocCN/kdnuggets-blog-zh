# 使用 K 均值聚类进行客户细分

> 原文：[https://www.kdnuggets.com/2019/11/customer-segmentation-using-k-means-clustering.html](https://www.kdnuggets.com/2019/11/customer-segmentation-using-k-means-clustering.html)

[评论](#comments)

客户细分是将市场细分为具有相似特征的离散客户群体。客户细分可以成为识别未满足客户需求的有力手段。利用上述数据，公司可以通过开发独特的吸引人产品和服务来超越竞争对手。

企业最常见的客户群体细分方式包括：

1.  **人口统计信息**，如性别、年龄、家庭和婚姻状况、收入、教育和职业。

1.  **地理信息**，这取决于公司的范围。对于本地化企业，这些信息可能涉及特定的城镇或县。对于较大的公司，这可能意味着客户所在的城市、州或甚至国家。

1.  **心理图谱数据**，如社会阶层、生活方式和个性特征。

1.  **行为数据**，如消费和消费习惯、产品/服务使用情况以及期望的收益。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 客户细分的优势

1.  确定合适的产品定价。

1.  制定定制化的营销活动。

1.  设计一个最佳分销策略。

1.  选择用于部署的具体产品特性。

1.  优先考虑新产品开发工作。

### K均值聚类算法

1.  指定簇的数量 *K*。

1.  通过首先打乱数据集，然后随机选择 *K* 个数据点作为质心进行初始化。

1.  不断迭代，直到质心没有变化，即数据点分配到簇的情况不再变化。

![图示](../Images/956f9bea9c5d8118b2e82b416a03bbb7.png)

K均值聚类，其中 K=3

### 挑战

您拥有一家超市购物中心，通过会员卡，您获得了一些基本的客户数据，如客户 ID、年龄、性别、年收入和消费评分。您希望了解客户，比如哪些是目标客户，以便可以传达给营销团队并相应地规划策略。

### 数据

该项目是 [购物中心客户细分数据](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python) 竞赛的一部分，该竞赛在 Kaggle 上举行。

数据集可以从kaggle网站下载，网址在[这里](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python)。

### 环境和工具

1.  scikit-learn

1.  seaborn

1.  numpy

1.  pandas

1.  matplotlib

### 代码在哪里？

不再多言，让我们开始编码。完整项目可以在github上找到，网址在[这里](https://github.com/abhinavsagar/Kaggle-Solutions/tree/master/customer%20segmentation)。

我开始加载所有库和依赖项。数据集中的列包括客户id、性别、年龄、收入和消费评分。

![](../Images/745a10e653cb1d1acc8f57d75577e694.png)

我删除了id列，因为它似乎与上下文无关。还绘制了客户的年龄频率。

![](../Images/848090cd2e4c89516a328c7586c5533b.png)

接下来，我制作了一个箱线图，以更好地可视化消费评分和年收入的分布范围。消费评分的范围明显大于年收入的范围。

![](../Images/b3fc65accb73fa9758005301d37c7dbd.png)

我制作了一个条形图，以检查数据集中男性和女性人口的分布情况。女性人口明显超过男性。

![](../Images/aa12e9a25d4de7c212d5b7b604c40c81.png)

接下来，我制作了一个条形图，以检查每个年龄组中客户的分布情况。显然，26-35岁年龄组的客户数量超过了其他任何年龄组。

![](../Images/6c7062fa11c8f7922b74a5cb7629abe5.png)

我继续制作了一个条形图，以可视化根据消费评分的客户数量。大多数客户的消费评分在41-60之间。

![](../Images/6f2a854e15e163190bf04ff972c185bd.png)

我还制作了一个条形图，以可视化根据年收入的客户数量。大多数客户的年收入在60000到90000之间。

![](../Images/8e4d636c2bb803dcbf98768a7e459bc7.png)

接下来，我将簇内平方和（WCSS）与簇的数量（K值）进行绘图，以找出最佳簇数量。WCSS衡量观察值与其簇质心的距离总和，其公式如下。

![](../Images/a2968373c583a3f2e4a684d77f4b462f.png)

其中*Yi*是观察值*Xi*的质心。主要目标是最大化簇的数量，在极限情况下，每个数据点都成为自己的簇质心。

![](../Images/3c0c4d748e4082479e05f7c4cf5b0912.png)

### 肘部法则

计算不同k值的簇内平方误差（WSS），并选择WSS首次开始减少的k值。在WSS与k的图中，这表现为一个肘部。

最优的K值通过肘部法则找到为5。

最后，我制作了一个3D图，以可视化客户的消费评分和年收入。数据点被分成5个类别，并用不同颜色表示，如3D图所示。

### 结果

![](../Images/093a0f8eb27af3a170048f2a9c9d322b.png)

### 结论

K均值聚类是最受欢迎的聚类算法之一，通常是从事聚类任务的实践者首先使用的工具，用以了解数据集的结构。K均值的目标是将数据点分组为独特的、互不重叠的子组。K均值聚类的一个主要应用是客户细分，以更好地理解客户，从而增加公司的收入。

### 参考文献/进一步阅读

[**客户细分的聚类算法**背景 在今天竞争激烈的世界中，了解客户行为并根据...](https://towardsdatascience.com/clustering-algorithms-for-customer-segmentation-af637c6830ac?source=post_page-----d33964f238c3----------------------)

[**您所需的K均值聚类最全面指南**

概述 K均值聚类是数据科学中的一个简单而强大的算法。在现实世界中有大量的应用...](https://www.analyticsvidhya.com/blog/2019/08/comprehensive-guide-k-means-clustering/?source=post_page-----d33964f238c3----------------------)

[**机器学习方法：K均值聚类算法**

2015年7月21日 作者：EduPristine k均值聚类（又称为细分）是最常见的机器学习...](https://www.edupristine.com/blog/beyond-k-means?source=post_page-----d33964f238c3----------------------)

### 离开之前

对应的源代码可以在这里找到。

[**abhinavsagar/Kaggle-Solutions**

Kaggle竞赛的示例笔记本。显微镜图像的自动分割是医学...](https://github.com/abhinavsagar/Kaggle-Solutions?source=post_page-----d33964f238c3----------------------)

### 联系方式

如果你想跟踪我最新的文章和项目，[关注我的Medium](https://medium.com/@abhinav.sagar)。以下是我的一些联系方式：

+   [个人网站](https://abhinavsagar.github.io/)

+   [Linkedin](https://in.linkedin.com/in/abhinavsagar4)

+   [Medium 个人主页](https://medium.com/@abhinav.sagar)

+   [GitHub](https://github.com/abhinavsagar)

+   [Kaggle](https://www.kaggle.com/abhinavsagar)

祝阅读愉快、学习愉快、编码愉快。

**个人简介：[Abhinav Sagar](https://www.linkedin.com/in/abhinavsagar4)** 是VIT Vellore的高年级本科生。他对数据科学、机器学习及其在实际问题中的应用感兴趣。

[原文](https://towardsdatascience.com/customer-segmentation-using-k-means-clustering-d33964f238c3)。经授权转载。

**相关：**

+   [R 用户的客户细分](/2019/09/customer-segmentation-r-users.html)

+   [如何使用Flask轻松部署机器学习模型](/2019/10/easily-deploy-machine-learning-models-using-flask.html)

+   [如何在Python中构建自己的逻辑回归模型](/2019/10/build-logistic-regression-model-python.html)

### 更多相关话题

+   [聚类解放：理解K-均值聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [k-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [什么是K-均值聚类及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [动手实践无监督学习：K-均值聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [Python 中的客户细分：一种实用方法](https://www.kdnuggets.com/customer-segmentation-in-python-a-practical-approach)

+   [使用 PyCaret 在 Python 中的聚类介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)
