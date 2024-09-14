# 使用 Matplotlib 和 Seaborn 创建可视化

> 原文：[https://www.kdnuggets.com/creating-visuals-with-matplotlib-and-seaborn](https://www.kdnuggets.com/creating-visuals-with-matplotlib-and-seaborn)

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/f0c896249a7b05ed54bf272c9ddb2a55.png)

图片来自 [storyset](https://www.freepik.com/free-vector/site-stats-concept-illustration_7140744.htm#query=visualization&position=31&from_view=search&track=sph) 由 [Freepik](https://www.freepik.com/) 提供

数据可视化在数据工作中至关重要，因为它帮助人们理解数据的变化。直接处理原始数据很困难，但可视化可以激发人们的兴趣和参与感。这就是为什么学习数据可视化对在数据领域取得成功非常重要。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

Matplotlib 是 Python 中最受欢迎的数据可视化库之一，因为它非常多功能，你可以从零开始几乎可视化任何东西。使用这个包，你可以控制可视化的许多方面。

另一方面，[Seaborn](https://seaborn.pydata.org/) 是一个建立在 Matplotlib 之上的 Python 数据可视化包。它提供了更简单的高级代码，并且包内含有多种内置主题。如果你想快速进行美观的数据可视化，这个包非常适合你。

在这篇文章中，我们将探索这两个包，并学习如何使用这些包来可视化你的数据。让我们开始吧。

# 使用 Matplotlib 进行可视化

如上所述，Matplotlib 是一个多功能的 Python 包，我们可以控制可视化的各种方面。这个包基于 [Matlab](http://mathworks.com/products/matlab.html) 编程语言，但我们在 Python 中使用了它。

Matplotlib 库通常已经在你的环境中可用，特别是如果你使用 Anaconda。如果没有，你可以使用以下代码进行安装。

```py
pip install matplotlib
```

安装后，我们将使用以下代码导入 Matplotlib 包进行可视化。

```py
import matplotlib.pyplot as plt
```

让我们从使用 Matplotlib 进行基本绘图开始。首先，我会创建一些示例数据。

```py
import numpy as np

x = np.linspace(0,5,21)
y = x**2
```

利用这些数据，我们将使用 Matplotlib 包创建一个折线图。

```py
plt.plot(x, y, 'b')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')
plt.title('Sample Plot')
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/ff53230a30b21fa4f127cdf83e507186.png)

在上面的代码中，我们将数据传递给matplotlib函数（x和y），以创建一个简单的蓝色折线图。此外，我们使用上述代码控制轴标签和标题。

让我们尝试使用`subplot`函数创建多个matplotlib图表。

```py
plt.subplot(1,2,1)
plt.plot(x, y, 'b--')
plt.title('Subplot 1')
plt.subplot(1,2,2)
plt.plot(x, y, 'r')
plt.title('Subplot 2')
```

![使用Matplotlib和Seaborn创建视觉效果](../Images/2d5c04a01628c7dac4d74a8cfe60dad7.png)

在上面的代码中，我们并排创建了两个图表。`subplot`函数控制图表的位置；例如，`plt.subplot(1,2,1)`意味着我们将有两个图表在一行（第一个参数）和两列（第二个参数）。第三个参数用于控制我们现在引用的是哪个图表。所以`plt.subplot(1,2,1)`意味着单行双列图表中的第一个图表。

这就是Matplotlib函数的基础，但如果我们想对Matplotlib可视化进行更多控制，我们需要使用面向对象方法（OOM）。使用OOM，我们可以直接从图形对象生成可视化，并调用指定对象的任何属性。

让我给你一个使用Matplotlib OOM的可视化示例。

```py
#create figure instance (Canvas)
fig = plt.figure()

#add the axes to the canvas
ax = fig.add_axes([0.1, 0.1, 0.7, 0.7]) #left, bottom, width, height (range from 0 to 1)

#add the plot to the axes within the canvas
ax.plot(x, y, 'b')
ax.set_xlabel('X label')
ax.set_ylabel('Y label')
ax.set_title('Plot with OOM')
```

![使用Matplotlib和Seaborn创建视觉效果](../Images/8c7458618dec4593d0cdacef53cccb25.png)

结果类似于我们创建的图表，但代码更复杂。一开始，这似乎是适得其反的，但使用OOM让我们几乎可以完全控制我们的可视化。例如，在上面的图表中，我们可以控制坐标轴在画布上的位置。

为了查看使用OOM与普通绘图函数的区别，我们将两个图表及其各自的坐标轴重叠在一起。

```py
#create figure instance (Canvas)
fig = plt.figure()

#add two axes to the canvas
ax1 = fig.add_axes([0.1, 0.1, 0.7, 0.7]) 
ax2 = fig.add_axes([0.2, 0.35, 0.2, 0.4]) 

#add the plot to the respective axes within the canvas
ax1.plot(x, y, 'b')
ax1.set_xlabel('X label Ax 1')
ax1.set_ylabel('Y label Ax 1')
ax1.set_title('Plot with OOM Ax 1')

ax2.plot(x, y, 'r--')
ax2.set_xlabel('X label Ax 2')
ax2.set_ylabel('Y label Ax 2')
ax2.set_title('Plot with OOM Ax 2')
```

![使用Matplotlib和Seaborn创建视觉效果](../Images/0e22c0956b8ec2689e95af70ca2d1644.png)

在上面的代码中，我们使用`plt.figure`函数指定了一个画布对象，并从该画布对象生成了所有这些图表。我们可以在一个画布中生成尽可能多的坐标轴，并在其中放置可视化图表。

也可以通过`subplot`函数自动创建图形和坐标轴对象。

```py
fig, ax = plt.subplots(nrows = 1, ncols =2)

ax[0].plot(x, y, 'b--')
ax[0].set_xlabel('X label')
ax[0].set_ylabel('Y label')
ax[0].set_title('Plot with OOM subplot 1')
```

![使用Matplotlib和Seaborn创建视觉效果](../Images/a282c6ee4cfcce188adda921fb7c9f8b.png)

使用`subplots`函数，我们创建了图形以及坐标轴对象的列表。在上述函数中，我们指定了图表的数量和一行两列图表的位置。

对于坐标轴对象，它是一个包含所有可访问图表坐标轴的列表。在上面的代码中，我们访问了列表中的第一个对象来创建图表。结果是两个图表，一个填充了折线图，而另一个只有坐标轴。

因为`subplots`生成了一个坐标轴对象的列表，你可以像下面的代码一样对它们进行迭代。

```py
fig, axes = plt.subplots(nrows = 1, ncols =2)

for ax in axes:

    ax.plot(x, y, 'b--')
    ax.set_xlabel('X label')
    ax.set_ylabel('Y label')
    ax.set_title('Plot with OOM')

plt.tight_layout()
```

![使用Matplotlib和Seaborn创建视觉效果](../Images/718427096d3f34d87dfe1594d9752524.png)

你可以修改代码以生成所需的图表。此外，我们使用`tight_layout`函数，因为图表可能会重叠。

让我们尝试一些可以用来控制 Matplotlib 图的基本参数。首先，尝试更改画布和像素大小。

```py
fig = plt.figure(figsize = (8,4), dpi =100)
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/80ca2e2b66776773bd26579fb14d0618.png)

参数 figsize 接受一个由两个数字（宽度，高度）组成的元组，结果与上述图类似。

接下来，让我们尝试为图添加图例。

```py
fig = plt.figure(figsize = (8,4), dpi =100)

ax = fig.add_axes([0.1, 0.1, 0.7, 0.7])

ax.plot(x, y, 'b', label = 'First Line')
ax.plot(x, y/2, 'r', label = 'Second Line')
ax.set_xlabel('X label')
ax.set_ylabel('Y label')
ax.set_title('Plot with OOM and Legend')
plt.legend()
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/4661c2b21ccbe9bf5c15f76a01c4ceed.png)

通过将标签参数分配给图并使用图例函数，我们可以将标签显示为图例。

最后，我们可以使用以下代码来保存我们的图。

```py
fig.savefig('visualization.jpg')
```

直方图可视化了以箱体形式表示的数据分布。

还有许多线图之外的特殊图。我们可以使用这些函数访问这些图。让我们尝试几种可能对你的工作有帮助的图。

我们可以创建一个散点图来可视化特征关系，使用以下代码。

```py
plt.scatter(x,y)
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/448f49532d809a70ebf21df2fe0cf194.png)

**直方图**

**散点图**

```py
plt.hist(y, bins = 5)
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/a282c6ee4cfcce188adda921fb7c9f8b.png)

**箱线图**

箱线图是一种将数据分布表示为四分位数的可视化技术。

```py
plt.boxplot(x)
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/0628557a97b35967115c2d143d190ade.png)

**饼图**

饼图是一种圆形图，用于表示分类图的数值比例，例如数据中分类值的频率。

```py
freq = [2,4,1,3]
fruit = ['Apple', 'Banana', 'Grape', 'Pear']
plt.pie(freq, labels = fruit)
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/3f69aeeb1f8423712c3f108f3c126993.png)

你还可以在 [这里](https://matplotlib.org/stable/plot_types/index.html) 查看 Matplotlib 库中的许多特殊图。

# 使用 Seaborn 进行可视化

[Seaborn](https://seaborn.pydata.org/) 是一个构建在 Matplotlib 之上的 Python 统计可视化包。Seaborn 的突出之处在于它通过优秀的样式简化了可视化的创建。这个包也与 Matplotlib 配合使用，因为许多 Seaborn API 依赖于 Matplotlib。

让我们尝试一下 Seaborn 包。如果你还没有安装这个包，可以使用以下代码进行安装。

```py
pip install seaborn
```

Seaborn 内置了一个 API，可以获取用于测试包的样本数据集。我们将使用这个数据集来创建各种 Seaborn 可视化效果。

```py
import seaborn as sns

tips = sns.load_dataset('tips')
tips.head()
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/48050471847e87be6104aa2c979c87cf.png)

使用上述数据，我们将探索 Seaborn 绘图，包括分布图、分类图、关系图和矩阵图。

**分布图**

我们将尝试的第一个图是分布图，用于可视化数值特征的分布。我们可以使用以下代码完成这项任务。

```py
sns.displot(data = tips, x = 'tip')
```

![使用 Matplotlib 和 Seaborn 创建视觉效果](../Images/e13b19abaca3afadb943cae3acbef03b.png)

默认情况下，displot 函数会生成一个直方图。如果我们想要平滑图形，可以使用 KDE 参数。

```py
sns.displot(data = tips, x = 'tip', kind = 'kde')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/f591a0460b704f826797ffc622b032bc.png)

分布图也可以根据 DataFrame 中的分类值进行拆分，使用 hue 参数。

```py
sns.displot(data = tips, x = 'tip', kind = 'kde', hue = 'smoker')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/f4b19c6a5983381bf3ded64da25cf37b.png)

我们甚至可以通过 row 或 col 参数进一步拆分图形。通过这些参数，我们可以生成多个图形，这些图形按照分类值的组合进行划分。

```py
sns.displot(data = tips, x = 'tip', kind = 'kde', hue = 'smoker', row = 'time', col = 'sex')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/7a23d4602d8492a55777ced2b70f3dea.png)

另一种展示数据分布的方式是使用箱形图。Seaborn 可以通过以下代码轻松实现可视化。

```py
sns.boxplot(data = tips, x = 'time', y = 'tip')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/276c54bf19d87fbe3b5acad1b749d479.png)

使用小提琴图，我们可以展示结合了箱形图和核密度估计（KDE）的数据分布。

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/4be3161d7ffe2b82b24c40d4d743b756.png)

最后，我们可以通过结合小提琴图和散点图将数据点展示到图中。

```py
sns.violinplot(data = tips, x = 'time', y = 'tip')
sns.swarmplot(data = tips, x = 'time', y = 'tip', palette = 'Set1')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/3d0660a41d594096bff54199e0d2f980.png)

**分类图**

分类图是一种多种 Seaborn API 的可视化方法，适用于处理分类数据。让我们探索一些可用的图形。

首先，我们会尝试创建一个计数图。

```py
sns.countplot(data = tips, x = 'time')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/387eba29a61798c066b7953f465a778c.png)

计数图会显示分类值的频率条形图。如果我们想要在图中显示计数数字，我们需要将 Matplotlib 函数结合到 Seaborn API 中。

```py
p = sns.countplot(data = tips, x = 'time')
p.bar_label(p.containers[0])
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/e8f9b9adac6bb3ad627b9b4ff86a041f.png)

我们可以通过使用 hue 参数进一步扩展图形，并用以下代码显示频率值。

```py
p = sns.countplot(data = tips, x = 'time', hue = 'sex')
for container in p.containers:
    ax.bar_label(container)
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/573220835749102df28b1a16ca92eb4a.png)

接下来，我们将尝试开发一个条形图。条形图是一种分类图，展示了数据的聚合以及误差条。

```py
sns.barplot(data = tips, x = 'time', y = 'tip')
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/1056127658caac3d58016e9465972ffb.png)

条形图使用分类特征和数值特征的组合来提供聚合统计数据。默认情况下，条形图使用平均聚合函数，并具有 95% 置信区间的误差条。

如果我们想要更改聚合函数，可以将函数传递给 estimator 参数。

```py
import numpy as np
sns.barplot(data = tips, x = 'time', y = 'tip', estimator = np.median)
```

![使用 Matplotlib 和 Seaborn 创建视觉图像](../Images/53802206efe9c210213bed5f839dcd3f.png)

**关系图**

关系图是一种可视化技术，用于展示特征之间的关系。它主要用于识别数据集中存在的任何模式。

首先，我们将使用散点图来展示某些数值特征之间的关系。

```py
sns.scatterplot(data = tips, x = 'tip', y = 'total_bill')
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/c9919188862d9d0b5975edda917985c0.png)

我们可以使用联合图将散点图与分布图结合起来。

```py
sns.jointplot(data = tips, x = 'tip', y = 'total_bill')
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/47fd2d5f59f8994ce79d7ead8a7e540a.png)

最后，我们可以使用 pairplot 自动绘制数据框中各特征之间的成对关系。

```py
sns.pairplot(data = tips)
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/6d6c11604260571653f0a9bbf33220e5.png)

**矩阵图**

矩阵图用于将数据可视化为颜色编码的矩阵。它用于查看特征之间的关系或帮助识别数据中的簇。

例如，我们有来自数据集的相关性数据矩阵。

```py
tips.corr()
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/4926467db3382f51ec936ceb7f5773cf.png)

如果我们将这些数据表示为颜色编码的图，我们可以更好地理解上述数据集。这就是我们使用热图的原因。

```py
sns.heatmap(tips.corr(), annot = True)
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/88542f674a16cc65d83ee1c8f9c59b75.png)

矩阵图还可以生成一个层次聚类图，推断数据集中的值，并根据现有相似性将它们分组。

```py
sns.clustermap(tips.pivot_table(values = 'tip', index = 'size', columns = 'day').fillna(0))
```

![使用 Matplotlib 和 Seaborn 创建可视化](../Images/c359443fa899ae9be08234fb48f15130.png)

# 结论

数据可视化是数据世界中的关键部分，它帮助观众快速理解我们的数据发生了什么。标准的 Python 数据可视化包是 Matplotlib 和 Seaborn。在本文中，我们学习了这些包的主要用途，除了 Matplotlib 和 Seaborn 外，还有哪些包可用于 Python 数据可视化，并介绍了几种可能帮助我们工作的可视化方式。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是数据科学助理经理和数据撰稿人。在 Allianz 印度尼西亚全职工作的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。

### 更多相关内容

+   [使用 Seaborn 创建美丽的直方图](https://www.kdnuggets.com/2023/01/creating-beautiful-histograms-seaborn.html)

+   [使用 Seaborn 进行 Python 数据可视化](https://www.kdnuggets.com/2022/04/data-visualization-python-seaborn.html)

+   [KDnuggets 新闻 22:n16, 4 月 20 日: 学习的最佳 YouTube 频道…](https://www.kdnuggets.com/2022/n16.html)

+   [使用 Matplotlib 进行数据可视化入门](https://www.kdnuggets.com/2022/12/introduction-data-visualization-matplotlib.html)

+   [Python Matplotlib 速查表](https://www.kdnuggets.com/2023/01/python-matplotlib-cheat-sheets.html)

+   [赢得房间: 创建和呈现有效的数据驱动…](https://www.kdnuggets.com/2022/04/franks-winning-room-creating-delivering-effective-data-driven-presentation.html)
