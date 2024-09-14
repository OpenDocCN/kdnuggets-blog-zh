# Python 中的数据可视化：Matplotlib 与 Seaborn

> 原文：[https://www.kdnuggets.com/2019/04/data-visualization-python-matplotlib-seaborn.html](https://www.kdnuggets.com/2019/04/data-visualization-python-matplotlib-seaborn.html)

[评论](#comments)![matplotlib-vs-seaborn](../Images/46cbf075d0af69589d9b5b1f6db550f1.png)

Python 提供了多种数据绘图包。本教程将使用以下包来演示 Python 的绘图功能：

+   [Matplotlib](https://matplotlib.org/)

+   [Seaborn](https://seaborn.pydata.org/)

### Matplotlib

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

```py
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np
```

在上述代码块中，我们使用 `PyPlot` 模块以 `plt` 作为别名导入了 Matplotlib 库。这是为了方便执行命令，正如我们稍后在教程中会看到的。`PyPlot` 包含了创建和编辑图表所需的一系列命令。运行 `%matplotlib inline` 是为了确保在代码块执行时，图表会自动显示在代码块下方。否则，用户需要每次创建新图表时输入 `plt.show()`。该功能仅适用于 Jupyter Notebook/IPython。Matplotlib 的高度可定制的代码结构使其成为其他绘图库的优秀指南。让我们看看如何从 matplotlib 生成散点图。

***一个实用的小贴士是，每当执行 matplotlib 时，输出总会包括一个文本输出，这可能在视觉上不太吸引人。为了解决这个问题，在执行代码块生成图形时，在最后一行代码的末尾添加一个分号 - ';'。***

使用的数据集是来自 UCI 机器学习库的 [共享单车数据集](https://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset)。

### Matplotlib: 散点图

散点图是你工具库中最具影响力、最具信息量和多功能的图表之一。它可以在不费太多力气的情况下向用户传达一系列信息（如下所示）。

+   `plt.scatter()` 将为我们提供一个包含传递的初始参数的数据的散点图。`temp` 是 x 轴，`cnt` 是 y 轴。

+   `c` 确定数据点的颜色。由于我们传递了一个字符串 - 'season'，它是数据框架 day 的一列，因此颜色对应于不同的季节。这是一种快速简便的视觉格式数据分组方法。

```py
plt.scatter('temp', 'cnt', data=day, c='season')
plt.xlabel('Normalized Temperature', fontsize='large')
plt.ylabel('Count of Total Bike Rentals', fontsize='large');
```

![matplotlib-scatterplot](../Images/a237310524eecb5772508a747c5001e2.png)

让我们看看它显示了哪些信息：

**曾经有超过 8000 次自行车租赁。**

+   标准化的温度已上升至 0.8 以上。

+   自行车租赁量与温度或季节的差异不大。

+   自行车租赁与标准化温度之间存在正线性关系。

**这张图确实给了我们很多信息。然而，图表没有生成图例，这使得解读季节组别变得困难。这是因为在这种方式下绘制的图表，Matplotlib 无法生成图例。在下一部分中，我们将看到上述图如何隐藏甚至误导观众。**

*让我们看看经过彻底编辑的相同图。这里的目标是生成图例以解读组之间的差异。*

```py
plt.rcParams['figure.figsize'] = [15, 10]

fontdict={'fontsize': 18,
          'weight' : 'bold',
         'horizontalalignment': 'center'}

fontdictx={'fontsize': 18,
          'weight' : 'bold',
          'horizontalalignment': 'center'}

fontdicty={'fontsize': 16,
          'weight' : 'bold',
          'verticalalignment': 'baseline',
          'horizontalalignment': 'center'}

spring = plt.scatter('temp', 'cnt', data=day[day['season']==1], marker='o', color='green')
summer = plt.scatter('temp', 'cnt', data=day[day['season']==2], marker='o', color='orange')
autumn = plt.scatter('temp', 'cnt', data=day[day['season']==3], marker='o', color='brown')
winter = plt.scatter('temp', 'cnt', data=day[day['season']==4], marker='o', color='blue')
plt.legend(handles=(spring,summer,autumn,winter),
           labels=('Spring', 'Summer', 'Fall/Autumn', 'Winter'),
           title="Season", title_fontsize=16,
           scatterpoints=1,
           bbox_to_anchor=(1, 0.7), loc=2, borderaxespad=1.,
           ncol=1,
           fontsize=14)
plt.title('Bike Rentals at Different Temperatures\nBy Season', fontdict=fontdict, color="black")
plt.xlabel("Normalized temperature", fontdict=fontdictx)
plt.ylabel("Count of Total Rental Bikes", fontdict=fontdicty);
```

![matplotlib-scatterplot-2](../Images/ead72af98b85416fe378d8513bddccf2.png)

+   `plt.rcParams['figure.figsize'] = [15, 10]` 允许控制整个图的大小。这对应于一个*15∗10（长度∗宽度）*的图。

+   `fontdict` 是一个可以作为参数传递的字典，用于标记坐标轴。标题的 `fontdict`，x 轴的 `fontdictx` 和 y 轴的 `fontdicty`。

+   现在有 4 个 `plt.scatter()` 函数调用，对应于四个季节之一。这在数据参数中再次出现，数据已经被子集化以对应单个季节。 marker 和 color 参数对应于使用 `'o'` 视觉上表示数据点及该标记的相应颜色。

+   [plt.legend()](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.legend.html) 是我们可以传递参数以创建图例的地方。前两个参数是 handles：实际的图形在图例中表示，以及 labels：与每个图形对应的名称，这些名称将在图例中显示。 scatterpoints 是散点图中每个标记的大小。 `bbox_to_anchor=(1, 0.7), loc=2, borderaxespad=1`。这三个参数一起使用，表示图例的位置；点击句首的链接以了解这些参数的性质。

现在我们可以区分季节以检查更多潜在的信息。然而，即使在添加这些额外层次后，图表仍然可能隐藏信息并容易被误解。

这个图：

**数据相互重叠。

+   图表杂乱。**

+   没有揭示出自行车租赁的季节性差异。

+   隐藏了例如春季和夏季自行车租赁随温度上升的模式。

+   显示了总自行车租赁与温度之间的整体正趋势。

+   没有清晰地显示哪个季节的温度最低。

### 子图

创建子图可能是行业中最吸引人和最专业的图表技术之一。当单个图表信息过于拥挤时，子图是必要的。那些信息在这种状态下无法评估。

***分面*** 是创建多个共享相同坐标轴的图表的过程。分面是数据可视化中最具多样性的技术之一。分面图可以在多个维度上传达信息，并能揭示以前隐藏的信息。

+   `plt.figure()`将用于创建一个空的绘图画布，如前所述。它被保存为fig。

+   `fig.add_subplot()`将重复4次，以对应各自的季节。参数对应于`nrows`、`ncols`和索引。例如在`ax1`中，它对应于图形的第一个绘图（索引从左上角开始为1，向右增加）。

+   其余的函数调用要么是自解释的，要么已在前面讲解过。

```py

fig = plt.figure()

plt.rcParams['figure.figsize'] = [15,10]
plt.rcParams["font.weight"] = "bold"

fontdict={'fontsize': 25,
          'weight' : 'bold'}

fontdicty={'fontsize': 18,
          'weight' : 'bold',
          'verticalalignment': 'baseline',
          'horizontalalignment': 'center'}

fontdictx={'fontsize': 18,
          'weight' : 'bold',
          'horizontalalignment': 'center'}

plt.subplots_adjust(wspace=0.2, hspace=0.2)

fig.suptitle('Bike Rentals at Different Temperatures\nBy Season', fontsize=25,fontweight="bold", color="black", 
             position=(0.5,1.01))

ax1 = fig.add_subplot(221)
ax1.scatter('temp', 'cnt', data=day[day['season']==1], c="green")
ax1.set_title('Spring', fontdict=fontdict, color="green")
ax1.set_ylabel("Count of Total Rental Bikes", fontdict=fontdicty, position=(0,-0.1))

ax2 = fig.add_subplot(222)
ax2.scatter('temp', 'cnt', data=day[day['season']==2], c="orange")
ax2.set_title('Summer', fontdict=fontdict, color="orange")

ax3 = fig.add_subplot(223)
ax3.scatter('temp', 'cnt', data=day[day['season']==3], c="brown")
ax3.set_title('Fall or Autumn', fontdict=fontdict, color="brown")

ax4 = fig.add_subplot(224)
ax4.scatter('temp', 'cnt', data=day[day['season']==4], c="blue")
ax4.set_title("Winter", fontdict=fontdict, color="blue")
ax4.set_xlabel("Normalized temperature", fontdict=fontdictx, position=(-0.1,0));

```

![scatterplot-facet-matplotlib-subplot](../Images/49ac0e0f067f2c92418c8e2560bd6d23.png)

现在我们可以更有效地独立分析每组数据。首先，我们应该注意的是温度与自行车租赁量的关系在不同季节间有所不同：

**   春季的正线性关系。**

+   冬季和夏季的二次非线性关系。

+   秋季的正相关性较弱，甚至没有明显关系。

然而，再次可能误导观众，这并非显而易见的原因。所有四个图表的坐标轴都不同。大多数人不会意识到这一点可能导致误导性见解。如果不加以注意，可能会造成这种情况。见下文以了解如何解决这个问题：

```py

fig = plt.figure()

plt.rcParams['figure.figsize'] = [12,12]
plt.rcParams["font.weight"] = "bold"

plt.subplots_adjust(hspace=0.60)

fontdicty={'fontsize': 20,
          'weight' : 'bold',
          'verticalalignment': 'baseline',
          'horizontalalignment': 'center'}

fontdictx={'fontsize': 20,
          'weight' : 'bold',
          'horizontalalignment': 'center'}

fig.suptitle('Bike Rentals at Different Temperatures\nBy Season', fontsize=25,fontweight="bold", color="black", 
             position=(0.5,1.0))

#ax2 is defined first because the other plots are sharing its x-axis
ax2 = fig.add_subplot(412, sharex=ax2)
ax2.scatter('temp', 'cnt', data=day.loc[day['season']==2], c="orange")
ax2.set_title('Summer', fontdict=fontdict, color="orange")
ax2.set_ylabel("Count of Total Rental Bikes", fontdict=fontdicty, position=(-0.3,-0.2))

ax1 = fig.add_subplot(411, sharex=ax2)
ax1.scatter('temp', 'cnt', data=day.loc[day['season']==1], c="green")
ax1.set_title('Spring', fontdict=fontdict, color="green")

ax3 = fig.add_subplot(413, sharex=ax2)
ax3.scatter('temp', 'cnt', data=day.loc[day['season']==3], c="brown")
ax3.set_title('Fall or Autumn', fontdict=fontdict, color="brown")

ax4 = fig.add_subplot(414, sharex=ax2)
ax4.scatter('temp', 'cnt', data=day.loc[day['season']==4], c="blue")
ax4.set_title('Winter', fontdict=fontdict, color="blue")
ax4.set_xlabel("Normalized temperature", fontdict=fontdictx);

```

![scatterplot-facet-subplots-sharex](../Images/504f53d184b6c9740a5a9194f8b63ad6.png)

现在这个图表网格已调整为与夏季共享相同的x轴，因为它的温度范围更宽。现在有趣的是，这些数据给我们提供了一些新的见解：

**   春季的温度最低。

+   秋季的温度最高。

+   自行车租赁总量与温度在夏季和秋季似乎存在二次关系。

+   无论季节如何，低温下的自行车租赁量较少。

+   春季的温度与总自行车租赁量之间存在明显的正线性关系。

+   秋季的温度与自行车租赁量之间似乎存在轻微的负线性关系。

```py

fig = plt.figure()

plt.rcParams['figure.figsize'] = [10,10]
plt.rcParams["font.weight"] = "bold"

plt.subplots_adjust(wspace=0.5)
fontdicty1={'fontsize': 18,
          'weight' : 'bold'}

fontdictx1={'fontsize': 18,
          'weight' : 'bold',
          'horizontalalignment': 'center'}

fig.suptitle('Bike Rentals at Different Temperatures\nBy Season', fontsize=25,fontweight="bold", color="black", 
             position=(0.5,1.0))

ax3 = fig.add_subplot(143, sharey=ax3)
ax3.scatter('temp', 'cnt', data=day.loc[day['season']==3], c="brown")
ax3.set_title('Fall or Autumn', fontdict=fontdict,color="brown")

ax1 = fig.add_subplot(141, sharey=ax3)
ax1.scatter('temp', 'cnt', data=day.loc[day['season']==1], c="green")
ax1.set_title('Spring', fontdict=fontdict, color="green")
ax1.set_ylabel("Count of Total Rental Bikes", fontdict=fontdicty1, position=(0.5,0.5))

ax2 = fig.add_subplot(142, sharey=ax3)
ax2.scatter('temp', 'cnt', data=day.loc[day['season']==2], c="orange")
ax2.set_title('Summer', fontdict=fontdict, color="orange")

ax4 = fig.add_subplot(144, sharey=ax3)
ax4.scatter('temp', 'cnt', data=day.loc[day['season']==4], c="blue")
ax4.set_title('Winter', fontdict=fontdict, color="blue")
ax4.set_xlabel("Normalized temperature", fontdict=fontdictx, position=(-1.5,0));

```

![scatterplot-facet-subplots-sharey](../Images/8e883df36b98acc495ecffa2249c253d.png)

重新调整/对比这些图表现在显示了另一个视角：

**   所有季节在某个时间点的自行车租赁量都超过了8000。

+   与其他季节相比，秋季和春季的聚类现象明显。

+   冬季和夏季的自行车租赁量变化最大。

***不要试图从这个角度解读变量之间的关系。这可能会再次误导你，因为现在看起来春季和夏季的自行车租赁量与温度之间存在负线性关系，而我们之前看到的并非如此。***

这里是一个由[Real Python关于使用Matplotlib的直观教程](https://realpython.com/python-matplotlib-guide/)的链接。

### Seaborn

seaborn 包是基于 Matplotlib 库开发的。它用于创建更具吸引力和信息性的统计图形。尽管 seaborn 是一个不同的包，但它也可以用来提升 matplotlib 图形的吸引力。

虽然 matplotlib 很棒，但我们总是希望做得更好。运行下面的代码块以导入 seaborn 库并创建之前的图表，看看会发生什么。

首先我们用 `import seaborn as sns` 导入库。下一行 `sns.set()` 将加载 seaborn 的默认主题和颜色调色板到会话中。运行下面的代码，观察图表区域和文本的变化。

```py
import seaborn as sns
sns.set()
```

一旦我们将 seaborn 加载到会话中，每次执行 matplotlib 图表时，seaborn 的默认自定义设置都会添加，如上所示。然而，一个困扰许多用户的大问题是标题可能会重叠。再加上 matplotlib 对标题的混乱命名规则，这变得令人烦恼。尽管如此，吸引人的视觉效果仍使其对数据科学家工作有用。

为了获得我们想要的标题格式并增加自定义性，我们需要使用下面的结构。*注意，如果我们在图表中使用副标题，这一点是必要的。有时候它们是必要的，因此最好备着。*

```py
fig = plt.figure()
fig.suptitle('Seaborn with Python', fontsize='x-large', fontweight='bold')
fig.subplots_adjust(top=0.87)
#This is used for the main title. 'figure()' is a class that provides all the plotting elements of a diagram. 
#This must be used first or else the title will not show.fig.subplots_adjust(top=0.85) solves our overlapping title problem.

ax = fig.add_subplot(111)

fontdict={'fontsize': 14,
        'fontweight' : 'book',
        'verticalalignment': 'baseline',
        'horizontalalignment': 'center'}

ax.set_title('Plotting Tutorial', fontdict=fontdict)
#This specifies which plot to add the customizations. fig.add_sublpot(111) corresponds to top left plot no.1 
#(there is only one plot). 

plt.plot(x, y, 'go-', linewidth=1) #linewidth=1 to make it narrower
plt.xlabel('x-axis', fontsize=14)
plt.ylabel('yaxis', fontsize=14);
```

![seaborn-python-titlefix](../Images/79df64d4b5230d57c1b58a7402c24595.png)

***深入了解 seaborn，我们可以用更少的代码行和类似的语法重现来自 Bike Rentals 数据集的上述可视化。Seaborn 仍然使用 Matplotlib 语法来执行 seaborn 图表，具有相对较小但明显的语法差异。***

为了简化和提高可视性，我将重命名和重新标记 Bike Rentals 数据集中的 'season' 列。

```py

day.rename(columns={'season':'Season'}, inplace=True)
day['Season']=day.Season.map({1:'Spring', 2:'Summer', 3:'Fall/Autumn', 4:'Winter'})
```

现在 'Season' 列已编辑为我们喜欢的样子，我们将继续创建前面图表的 seaborn 样式可视化。

第一个明显的区别是 seaborn 在加载其默认美学时呈现的默认主题。正如你直接看到的默认主题是因为在调用 `sns.set()` 时后台应用了 `sns.set_style('whitegrid')`。正如我们所见，这可以根据我们的喜好轻松覆盖，使用下方单元格中列出的现成主题：

+   `sns.set_style()` 必须是 'white', 'dark', 'whitegrid', 'darkgrid', 'ticks' 中的一个。这控制图表区域，如颜色、网格和刻度线的存在。

+   `sns.set_context()` 必须是 'paper', 'notebook', 'talk', 'poster' 中的一个。这控制图表的布局方式，如在 'poster' 上看到的放大图像和文字。'Talk' 将创建一个字体更粗的图表。

```py

plt.figure(figsize=(7,6))

fontdict={'fontsize': 18,
          'weight' : 'bold',
         'horizontalalignment': 'center'}

sns.set_context('talk', font_scale=0.9)
sns.set_style('ticks')

sns.scatterplot(x='temp', y='cnt', hue='Season', data=day, style='Season', 
                    palette=['green','orange','brown','blue'], legend='full')

plt.legend(scatterpoints=1,
           bbox_to_anchor=(1, 0.7), loc=2, borderaxespad=1.,
           ncol=1,
           fontsize=14)
plt.xlabel('Normalized Temperature', fontsize=16, fontweight='bold')
plt.ylabel('Count of Total Bike Rentals', fontsize=16, fontweight='bold')
plt.title('Bike Rentals at Different Temperatures\nBy Season', fontdict=fontdict, color="black",
         position=(0.5,1));
```

![seaborn-scatterplot-talk-ticks](../Images/3963b42dd222399f624bd4eeb0ffac9a.png)

现在让我们来看一下相同的图表，但使用 `sns.set_context('paper', font_scale=2)` 和 `sns.set_style('white')`

```py

plt.figure(figsize=(7,6))

fontdict={'fontsize': 18,
          'weight' : 'bold',
         'horizontalalignment': 'center'}

sns.set_context('paper', font_scale=2) #this makes the font and scatterpoints much smaller, hence the need for size adjustemnts
sns.set_style('white')

sns.scatterplot(x='temp', y='cnt', hue='Season', data=day, style='Season', 
                    palette=['green','orange','brown','blue'], legend='full', size='Season', sizes=[100,100,100,100])

plt.legend(scatterpoints=1,
           bbox_to_anchor=(1, 0.7), loc=2, borderaxespad=1.,
           ncol=1,
           fontsize=14)

plt.xlabel('Normalized Temperature', fontsize=16, fontweight='bold')
plt.ylabel('Count of Total Bike Rentals', fontsize=16, fontweight='bold')
plt.title('Bike Rentals at Different Temperatures\nBy Season', fontdict=fontdict, color="black",
         position=(0.5,1));
```

![seaborn-scatterplot-paper-white](../Images/d39f0d78ac0047837a939bd78cbc7cf3.png)

现在我们终于用**更少的代码行**和**更好的分辨率**使用Seaborn重新创建了之前的matplotlib样式图。让我们再进一步，对图进行分面处理以完成：

```py

sns.set(rc={'figure.figsize':(20,20)}) 
sns.set_context('talk', font_scale=2) 
sns.set_style('ticks')
g = sns.relplot(x='temp', y='cnt', hue='Season', data=day,palette=['green','orange','brown','blue'],
                col='Season', col_wrap=4, legend=False
                height=6, aspect=0.5, style='Season', sizes=(800,1000))

g.fig.suptitle('Bike Rentals at Different Temperatures\nBy Season' ,position=(0.5,1.05), fontweight='bold', size=18)
g.set_xlabels("Normalized Temperature",fontweight='bold', size=15)
g.set_ylabels("Count of Total Bike Rentals",fontweight='bold', size=20);
```

![seaborn-scatterplot-talk-ticks-facet](../Images/8fc662673c2eae7da534dae2f56e8af0.png)

要更改图形的形状，需要调整`aspect`参数。增加此处的aspect值将创建一个更方形的图形。它与`height`配合使用，所以可以通过这两个参数实验不同的大小。

要更改行和列的数量，请使用`col_wrap`参数。它与`col`参数配合使用。它检测类别的数量并相应分配。

```py

sns.set(rc={'figure.figsize':(20,20)}) 
sns.set_context('talk', font_scale=2) 
sns.set_style('ticks')
g = sns.relplot(x='temp', y='cnt', hue='Season', data=day,palette=['green','orange','brown','blue'],
                col='Season', col_wrap=2, legend=False
                height=4, aspect=1.6, style='Season', sizes=(800,1000))

g.fig.suptitle('Bike Rentals at Different Temperatures\nBy Season' ,position=(0.5,1.05), fontweight='bold', size=18)
g.set_xlabels("Normalized Temperature",fontweight='bold', size=15)
g.set_ylabels("Count of\nTotal Bike Rentals",fontweight='bold', size=20);
```

![figure-name](../Images/570390be3c0eace90dcebfda7cd49703.png)

***注意：本教程的部分内容曾用于我为[维多利亚理工学院](https://www.vit.edu.au/)准备的教程中***

**相关：**

+   [6个数据可视化灾难 – 如何避免](/2019/02/data-visualization-disasters-avoid.html)

+   [5个用Python和代码快速简便的数据可视化](/2018/07/5-quick-easy-data-visualizations-python-code.html)

+   [10个适用于任何学科的有用Python数据可视化库](/2016/06/python-data-visualization-libraries.html)

### 更多相关话题

+   [使用Matplotlib和Seaborn创建可视化]（https://www.kdnuggets.com/creating-visuals-with-matplotlib-and-seaborn）

+   [KDnuggets新闻 22:n16，4月20日：顶级YouTube学习频道…](https://www.kdnuggets.com/2022/n16.html)

+   [使用Seaborn进行Python数据可视化](https://www.kdnuggets.com/2022/04/data-visualization-python-seaborn.html)

+   [每个数据科学家都应该知道的三种R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么使Python成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [Matplotlib数据可视化简介](https://www.kdnuggets.com/2022/12/introduction-data-visualization-matplotlib.html)
