# 使用Dask进行K-means聚类：猫咪图片的图像滤镜

> 原文：[https://www.kdnuggets.com/2019/06/k-means-clustering-dask-image-filters.html](https://www.kdnuggets.com/2019/06/k-means-clustering-dask-image-filters.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由[Luciano Strika](http://www.datastuff.tech)提供，MercadoLibre**

![一只小猫的图片。它将通过K-means聚类进行压缩。](../Images/d0f4d993a98270043bb07a9e804b179d.png)

对图像应用滤镜对任何人来说都不是新概念。我们拍一张照片，对其进行一些更改，然后它看起来更酷。但是人工智能的作用在哪里？让我们尝试一下用K Means 聚类进行**无监督机器学习**的有趣用例。

我之前写过**K Means 聚类**，所以我会假设你对这个算法已经**熟悉**。如果你不熟悉，可以查看我写的[深入介绍](http://www.datastuff.tech/machine-learning/k-means-clustering-unsupervised-learning-for-recommender-systems/)。

此外，我还尝试了用[自编码器](http://www.datastuff.tech/machine-learning/autoencoder-deep-learning-tensorflow-eager-api-keras/)进行**图像压缩**（好吧，是重建），效果有好有坏。

然而，这一次，我的目标是**不重建**最好的图像，而只是查看用最少颜色**重建图片**的效果。

与使图片尽可能接近原始图片不同，我只是希望我们看完之后能说“真棒！”。

那我们该如何做呢？很高兴你问了。

### 如何使用K-means聚类进行图像滤镜

首先，永远记住**图像**只是**像素向量**。每个像素是三个介于0到255之间的整数值的元组（无符号字节），代表该像素颜色的RGB值。

我们想要使用K-means聚类来找到最能**表征图像**的*k*个**颜色**。这意味着我们可以将每个**像素**视为**单个数据点**（在3维空间中），并进行聚类。

首先，我们需要在Python中将图像转换为**像素向量**。以下是实现方法。

顺便说一句，我认为*vector_of_pixels*函数不需要使用Python列表。我确信一定有办法**展平numpy数组**，只是我没找到（至少没有找到按照我想要的顺序进行展平的方法）。

如果你能想到任何方法，请在评论中告诉我！

下一步是**拟合**模型到图像，以便它将像素**聚类**为*k*种颜色。然后，只需将相应的聚类颜色分配给图像中的每个位置即可。

例如，也许我们的图片只有三种颜色：两种红色和一种绿色。如果我们将其拟合到2个聚类，那么所有红色像素都会变成不同的红色阴影（被聚在一起），其他的则会变成绿色。

但解释够了，让我们看看程序的实际效果！

和往常一样，你可以用任何你想要的图片自行运行，以下是带有代码的[GitHub 仓库](https://github.com/StrikingLoo/K-means-image-compression)。

### 结果

我们将把滤镜应用于小猫的图片，这些图片取自令人惊叹的“Cats vs Dogs”[kaggle 数据集](https://www.kaggle.com/c/dogs-vs-cats)。

我们将从一张猫的图片开始，并使用不同的*k*值应用滤镜。以下是原始图片：

![一张小猫的图片。它将使用K均值聚类进行压缩。](../Images/02437fb057943adcc789ae8c047b6b57.png)

首先，我们来检查一下这张图片最初有多少种颜色。

仅用一行*Numpy*代码，我们计算了这张图片上像素的唯一值。这张图片特别有**243种不同颜色**，尽管它总共有**166167个像素**。

现在，让我们看看将其聚类为2、5和10种不同颜色后的结果。

![猫的图片](../Images/6abccd2057d9dfedb5424988395cf7d6.png)

只有两种颜色，它所做的只是标记最暗和最亮的区域。不过，如果你是艺术家，正在用黑白（例如墨水）绘制某些东西，并希望看到参考图像的轮廓，这可能会很有用。

![图像](../Images/55971350b7a0f1861ef6c9fc9935b1d4.png)只有5种不同的颜色，这只猫已经可以辨认出来了！！[这只小猫的图片是使用K均值聚类压缩的。](../Images/3ec93a8e54a7b5bb0554d1b33de1e723.png)10种颜色的图片可能看起来有点迷幻，但它比较清晰地展示了原图的内容。

你注意到什么趋势了吗？我们添加的每一种颜色的回报递减。拥有2种颜色和5种颜色之间的差别远大于5种和10种之间的差别。然而，10种颜色时，平坦区域更小，细节更丰富。接下来看看15种和24种颜色！

![小猫的压缩图片。](../Images/a048d65e65c7bdbbe0a10aeffb618639.png)![小猫的压缩图片。](../Images/3054055891015af45daf4acd7511ce6c.png)

尽管很明显，上面的图片使用了24种颜色的滤镜（原始数量的10%），但我们足够好地展示了猫，并达到了一定的细节水平。

继续看另一张图片：这是原图（256种不同颜色），这是压缩后的图片（24种颜色）。

![两个相同的白色小猫并排显示。其中一个是用K均值聚类压缩的。](../Images/c424ebe8bd42fd56e98c70ef317bcb56.png)256种颜色与24种颜色。注意到有什么区别吗？

有趣的是，“压缩”图像的大小为18KB，而未压缩的图像为16KB。我不太清楚为什么会这样，因为压缩器是相当复杂的，但希望看到你们在评论中的理论。

### 结论

我们能够仅用原图 10% 的颜色生成新图像，且看起来非常相似。得益于 K-means 聚类，我们还获得了一些酷炫的滤镜。你能想到其他有趣的聚类应用吗？你认为其他聚类技术会产生更有趣的结果吗？

如果你想回答这些问题，可以通过 [Twitter](https://twitter.com/strikingloo)、[Medium](https://medium.com/@strikingloo) 或 [Dev.to](http://www.dev.to/strikingloo) 与我联系。

**简介： [Luciano Strika](http://www.datastuff.tech)** 是布宜诺斯艾利斯大学的计算机科学学生，兼任 MercadoLibre 的数据科学家。他还在[**www.datastuff.tech**](http://www.datastuff.tech)撰写有关机器学习和数据的文章。

[原文](http://www.datastuff.tech/machine-learning/k-means-clustering-with-dask-editing-pictures-of-kittens)。经许可转载。

**相关：**

+   [K-Means 聚类：推荐系统的无监督学习](/2019/04/k-means-clustering-unsupervised-learning-recommender-systems.html)

+   [提升你的图像分类模型](/2019/05/boost-your-image-classification-model.html)

+   [介绍 Dask-SearchCV：使用 Scikit-Learn 进行分布式超参数优化](/2018/12/solve-image-classification-problem-quickly-easily.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [聚类释放：理解 K-Means 聚类](https://www.kdnuggets.com/2023/07/clustering-unleashed-understanding-kmeans-clustering.html)

+   [K-means 聚类的质心初始化方法](https://www.kdnuggets.com/2020/06/centroid-initialization-k-means-clustering.html)

+   [什么是 K-Means 聚类及其算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [动手实践无监督学习：K-Means 聚类](https://www.kdnuggets.com/handson-with-unsupervised-learning-kmeans-clustering)

+   [卡尔曼滤波器简要介绍](https://www.kdnuggets.com/2022/12/brief-introduction-kalman-filters.html)

+   [使用 PyCaret 在 Python 中进行聚类介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)
