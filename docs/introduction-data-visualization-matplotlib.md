# 使用 Matplotlib 进行数据可视化简介

> 原文：[https://www.kdnuggets.com/2022/12/introduction-data-visualization-matplotlib.html](https://www.kdnuggets.com/2022/12/introduction-data-visualization-matplotlib.html)

![使用 Matplotlib 进行数据可视化简介](../Images/5f3bae23e6bb8870e327a022e594b0e1.png)

图片来源于作者

# 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

许多组织收集大量数据以进行业务决策。**数据可视化** 是将这些信息以各种图表和图形的形式呈现的过程。它简化了复杂数据，使得识别模式、分析趋势和发现可操作的见解变得更加容易。**Matplotlib** 是一个多平台的数据可视化库，使用 Python 编写。它最初是为了模拟 MATLAB 的绘图功能而创建的，但它强大且易于使用。Matplotlib 的一些优点如下：

+   更容易自定义

+   更适合入门

+   高质量输出

+   轻松获取

+   提供对图形各个元素的良好控制

# 入门

## 安装 Matplotlib

要安装 Matplotlib，请在 Windows、Mac OS 和 Linux 的终端中运行以下命令：

```py
pip install matplotlib
```

对于 Jupyter notebook：

```py
!pip install matplotlib
```

对于 Anaconda 环境：

```py
conda install matplotlib
```

## 导入库

```py
import numpy as np
import pandas as pd  #If you are reading data from CSV
import matplotlib.pyplot as plt
```

# Matplotlib 基础

## 创建图形

在 matplotlib 中创建图形有两种方法：

### 1) 函数式方法

它们使用简单，但不允许非常高的控制程度。它使用 **py.plot(x,y)** 函数。我们在本教程的其他地方不会使用它，但你应该了解它的工作原理，所以让我们看看其中一个示例。

```py
x = np.arange(0,8) 
y = x  

plt.plot(x, y) 
plt.xlabel('Hours of Study')
plt.ylabel('Class Performance')
plt.title('Student Performance Analysis')
plt.show() # For non-jupyter users
```

![使用 Matplotlib 进行数据可视化简介](../Images/3224d824cc68dde2f9b252b939ab1787.png)

### 2) OOP 方法

OOP 方法是创建图形的推荐方式。它通过创建图形对象，然后将坐标轴添加到其中来实现。图形对象在没有添加坐标轴之前是不可见的。

```py
fig = plt.figure()
```

![使用 Matplotlib 进行数据可视化简介](../Images/be077195dd3f8c14a8e01889b8424fd0.png)

在我们绘制坐标轴之前，让我们理解其语法。

```py
figureobject.add_axes([a,b,c,d])
```

这里的 a,b 指的是原点的位置。(0,0) 代表左下角，c,d 设置绘图的宽度和高度。两个值的范围从 0 到 1。

```py
fig = plt.figure() # blank canvas
axes = fig.add_axes([0, 0, 0.5, 0.5]) 
axes.plot(x, y)
plt.show()
```

![使用 Matplotlib 进行数据可视化简介](../Images/5ab228762f37982900d167d8f11dca16.png)

图形对象可以接受一些额外的参数，如dpi和图形大小。Dpi指的是每英寸的点数，如果图形模糊，可以增加分辨率。图形大小则控制图形的尺寸（以英寸为单位）。

```py
fig = plt.figure(figsize=(0.5,0.5),dpi=200) # blank canvas
axes = fig.add_axes([0, 0, 0.5, 0.5]) 
axes.plot(x, y)
plt.show()
```

![使用Matplotlib进行数据可视化介绍](../Images/1bb8204614cdd768c65f416a7d97b4e1.png)

你还可以按照如下方式将多个坐标轴添加到图形对象中：

```py
a = np.arange(0,50)
b = a**3
fig = plt.figure()
outer_axes = fig.add_axes([0,0,1,1])
inner_axes = fig.add_axes([0.25,0.5,0.25,0.25])
outer_axes.plot(a,b)
inner_axes.set_xlim(10,20)   #sets the range on x-axis
inner_axes.set_ylim(0,10000) #sets the range on y-axis
inner_axes.set_title("Zoomed Version")
inner_axes.plot(a,b)
plt.show()
```

![使用Matplotlib进行数据可视化介绍](../Images/b68a4a4d6621a4883eece7b778aa93d1.png)

我们可以使用subplots()函数来创建多个图形，而不是手动管理图形对象中的不同坐标轴。让我们查看其语法，

```py
fig, axes = plt.subplots(nrows=1, ncols=2)
```

它返回一个包含图形对象以及保存所有坐标轴对象的numpy数组的元组。我们需要指定实际坐标轴的行数和列数。每个坐标轴对象都会单独返回，可以独立访问。

```py
exercise_hrs = np.arange(0, 5)
male_cal = exercise_hrs
female_cal = 0.70 * exercise_hrs
fig, axes = plt.subplots(nrows=1, ncols=2)
axes[0].plot(exercise_hrs, male_cal)
axes[0].set_ylim(0, 5)  # Sets range of y
axes[0].set_title("Male")
axes[1].plot(exercise_hrs, female_cal)
axes[1].set_ylim(0, 5)
axes[1].set_title("Female")
fig.suptitle(
    "Calories Burnt vs Workout Hours Analysis", fontsize=16
)  # Displays the main title
fig.tight_layout()  # Prevents overlapping of subplots
```

![使用Matplotlib进行数据可视化介绍](../Images/f1be44504eb8baf10b4cf93d76fad54d.png)

子图间距可以通过以下方法手动调整：

```py
fig.subplots_adjust(left=None,top=None,right=None,top=None,wspace=None,    hspace=None)
```

+   left = 图形子图的左侧

+   right = 图形子图的右侧

+   bottom = 图形子图的底部

+   top = 图形子图的顶部

+   wspace = 保留的子图之间的宽度空间

+   hspace = 保留的子图之间的高度空间

```py
fig.subplots_adjust(left=0.2,top=0.8,wspace=0.9, hspace=0.1)
```

应用到上面的图形中：

![使用Matplotlib进行数据可视化介绍](../Images/1457edadc3c3504bfa5c9c54d480f437.png)

## 自定义图形

### 1) 图例

如果我们在一个图形对象中创建多个图形，可能会变得难以识别每个图形所表示的内容。因此，我们在**axes.plot()**函数中添加**label= “text”**属性，然后调用**axes.legend()**函数来显示图例。

```py
axes.legend(loc=0) or axes.legend() #Default - Matplotlib decides position
axes.legend(loc=1) # upper right
axes.legend(loc=2) # upper left
axes.legend(loc=3) # lower left
axes.legend(loc=4) # lower right 
axes.legend(loc=(x,y)) # At (x,y) position 
```

axes.legend() 还有一个参数loc，用于决定图例的位置。

```py
x = np.arange(0,11)
fig = plt.figure()
ax = fig.add_axes([0,0,0.75,0.75])
ax.plot(x, x**2, label="X^2")
ax.plot(x, x**3, label="X^3")
ax.legend(loc=0) #Let matplotlib decide
```

![使用Matplotlib进行数据可视化介绍](../Images/64703666988b9facc91623c110fc48cc.png)

### 2) 线条样式

Matplotlib提供了很多自定义选项。让我们深入分析一下更改线条颜色、宽度和样式的语法。

```py
axes.plot(x, y, color or c = 'red',alpha= ‘0.5’, linestyle or ls = ':', linewidth or lw= 5)
```

**颜色：** 我们可以通过名称或RGB值来定义颜色，或者使用Matlab类型的语法，其中r表示红色等。我们还可以使用alpha属性设置透明度。

**线型：** 自定义样式也可以创建，但由于我们主要关注可视化，因此简单的样式对我们来说就足够了。它们如下所示：

```py
linestyle = “-”  or linestyle = “solid”
linestyle = “:”  or linestyle = “dotted”
linestyle = “--” or linestyle = “dashed”
linestyle = “-.” or linestyle = “dashdot”
```

**线宽：** 默认值为1，但我们可以根据需要进行更改。

```py
fig, ax = plt.subplots()
ax.plot(x, x-2, color="#000000", linewidth=1 , linestyle='solid')
ax.plot(x, x-4, color="red", lw=2 ,ls=":")
ax.plot(x, x-6, color="blue",alpha=0.4,lw=4 , ls="-.")
```

![使用Matplotlib进行数据可视化介绍](../Images/ca11dcfaee3d1f0bef81746bb7897a0b.png)

### 3) 标记样式

在matplotlib中，所有绘制的点称为标记。默认情况下，我们只看到最终的线条，但我们可以根据自己的选择设置标记类型及其大小。

```py
axes.plot(x, y,marker =”+” , markersize or ms= 20)
```

标记有很多类型，详见[这里](https://matplotlib.org/stable/api/markers_api.html)，但我们只讨论主要的几种：

```py
marker='+' # plus
marker='o' # circle
marker='s' # square
marker='.' # point
```

示例：

```py
fig, ax = plt.subplots()
ax.plot(x, x+2,marker='+',markersize=20)
ax.plot(x, x+4,marker='o',ms=10) 
ax.plot(x, x+6,marker='s',ms=15,lw=0) 
ax.plot(x, x+8,marker='.',ms=10) 
```

![使用 Matplotlib 进行数据可视化简介](../Images/16d5a0eca0c2ef69a5a92dfb5081b48b.png)

# 图表类型

Matplotlib 提供了各种特殊图表，因为不同类型的数据需要不同的表示方式。选择图表取决于分析的问题。例如，如果你关注部分与整体的关系，可以使用饼图；如果你想比较值或组，可以使用条形图；如果你想观察不同变量之间的对应关系，可以使用散点图等。在本教程中，我们将逐一讲解并讨论5种最常用的图表。让我们开始吧：

## 1) 折线图

这是表示数据的最简单形式。它们主要用于分析与时间相关的数据，因此也称为时间序列图。向上的趋势表示变量之间的正相关，反之亦然。它广泛应用于天气预报、股市预测以及监测日常客户或销售等。

```py
# Data is collected from worldometer
years = ["1980", "1990", "2000", "2010", "2020"]
Asia = [2649578300, 3226098962, 3741263381, 4209593693, 4641054775]
Europe = [693566517, 720858450, 725558036, 736412989, 747636026]
fig, ax = plt.subplots()
ax.set_title("Population Analysis (1980 - 2020)")
ax.set_xlabel("Years")
ax.set_ylabel("Population in billions")
ax.plot(years, Asia, label="Asia")
ax.plot(years, Europe, label="Europe")
ax.legend()
```

![使用 Matplotlib 进行数据可视化简介](../Images/bf1029e8f33405f2a38e13deefac2392.png)

我们可以看到，自1980年以来，亚洲的人口呈指数增长。

## 2) 饼图

饼图将圆圈分成代表部分与整体关系的比例区域。每一部分加起来总和为100%。这些切片的面积也被称为楔形。

```py
matplotlib.pyplot.pie(data,explode=None,labels=None,colors=None,autopct=None, shadow=False) 
```

+   data = 你想绘制的值的数组

+   explode = 分离图表中的楔形

+   labels = 代表不同切片的字符串

+   colors = 用指定颜色填充楔形

+   autopct = 在楔形图上标注数值

+   shadow = 为楔形添加阴影

```py
labels = [
    "Rent",
    "Utility bills",
    "Transport",
    "University fees",
    "Grocery",
    "Savings",
]
expenses = [200, 100, 80, 500, 100, 60]
explode = [0.0, 0.0, 0.0, 0.0, 0.0, 0.4]
colors = [
    "lightblue",
    "orange",
    "lightgreen",
    "purple",
    "crimson",
    "red",
]
fig, ax = plt.subplots()
ax.set_title("University Student Expenses")
ax.pie(
    expenses,
    labels=labels,
    explode=explode,
    colors=colors,
    autopct="%.1f%%",
    shadow=True,
)
plt.show()
```

![使用 Matplotlib 进行数据可视化简介](../Images/08c8a5d0327f0ba793e49572605ee2d2.png)

## 3) 散点图

散点图，也称为XY图，用于观察因变量和自变量之间的关系。它绘制个别数据点以进行趋势分析。使用散点图可以轻松检测异常值和相关关系。

```py
matplotlib.pyplot.scatter(x, y, s=None, c=None, marker=None,alpha=None, linewidths=None, edgecolors=None)
```

+   (x,y) = 数据位置

+   s= 标记的大小

+   c= 标记颜色的序列

+   marker= 标记样式

+   alpha= 透明度

+   linewidth= 标记边缘的线宽

+   edgecolors= 标记边缘的颜色

```py
x = np.linspace(0, 11, 40)
y = np.cos(x)
fig, ax = plt.subplots()
ax.scatter(
    x,
    y,
    s=50,
    c="green",
    marker="o",
    alpha=0.4,
    linewidth=2,
    edgecolor="black",
) 
```

![使用 Matplotlib 进行数据可视化简介](../Images/552a7dd6feb888c87a6dbdf00e95c8be.png)

## 4) 条形图

条形图用于通过垂直或水平的矩形条来可视化分类数据。条形的长度或高度（取决于是柱状图还是水平条形图）表示其数值。当你想比较某些组时，条形图非常有用。

```py
matplotlib.pyplot.bar(x, height, width, bottom, align)
```

+   x= 类别变量

+   height= 对应的数值

+   width= 条形图的宽度（默认值为0.8）

+   bottom= 条形图基底的起始点（默认值为0）

+   align= 类别名称的对齐方式（默认值为居中）

**注意：** 颜色、边框颜色和线宽也可以自定义。

```py
fig,ax = plt.subplots()
courses = ['Maths', 'Science', 'History', 'Computer', 'English']
students = [24,12,15,31,22]
ax.bar(courses,students,width=0.5,color="red",alpha=0.6,edgecolor="red",linewidth=2)
```

![使用 Matplotlib 进行数据可视化介绍](../Images/a68c3b5474b2789a61e094eaaf3960d7.png)

我们还可以通过调整底部属性来堆叠类别。

```py
# For Horizontal Bar Chart
ax.barh(
    courses,
    students,
    height=0.7,
    color="red",
    alpha=0.6,
    edgecolor="red",
    linewidth=2,
) 
```

![使用 Matplotlib 进行数据可视化介绍](../Images/a73709d24be49e8876110d533d82b74b.png)

我们还可以通过调整底部属性来堆叠类别。

```py
fig,ax = plt.subplots()
courses = ['Maths', 'Science', 'History', 'Computer', 'English']
students = [[24,12,15,31,22],[19,14,19,26,18]] #Male array then female array
ax.bar(courses,students[0],width=0.5,label="male")
ax.bar(courses,students[1],width=0.5,bottom=students[0],label="female")
ax.set_ylabel("No of Students")
ax.legend()
```

![使用 Matplotlib 进行数据可视化介绍](../Images/dd0dd32576ee7f82d149d26caf0a63bb.png)

我们还可以通过调整条形的厚度和位置来绘制多个条形。

```py
fig,ax = plt.subplots()
courses = ['Maths', 'Science', 'History', 'Computer', 'English']
males = (24,12,15,31,22)
females = (19,14,19,26,18)
index=np.arange(5)
bar_width=0.4
ax.bar(index,males,bar_width,alpha=.9,label="Male")
# We will adjust the bar_width so it is placed side to side 
ax.bar(index + bar_width ,females,bar_width,alpha=.9,label="Female") 
ax.set_xticks(index + 0.2,courses) # Show labels
ax.legend()
```

![使用 Matplotlib 进行数据可视化介绍](../Images/f9ab079ea878665d6e54fd2a29c4d0a9.png)

## 5) 直方图

由于其相似性，许多人经常将其与条形图混淆，但它在表示信息的方式上有所不同。它将数据点组织成称为“箱”的范围，绘制在X轴上，而Y轴包含有关频率的信息。与条形图不同，它仅用于表示数值数据。

```py
matplotlib.pyplot.hist(x,bins=None,cumulative=False,range=None,bottom=None,histtype=’bar’,rwidth=None, color=None, label=None, stacked=False)
```

+   bins = 如果是 int，则为等宽的箱子，否则取决于序列

+   cumulative = 最后一个箱将给出总数据点（基于累积频率）

+   bottom = 箱的位置

+   range = 用于切割数据

+   histtype= bar, barstacked, step, stepfilled（默认= bar）

+   rwidth= 直方图条的相对宽度

+   stacked= 如果为 True，则多个数据将堆叠在一起

+   data = np.random.normal(140, 10,100) # 生成100人的身高数据

+   bins = 10

```py
data = np.random.normal(140, 10,100) # Generating height of 100 people
bins = 10
fig,ax = plt.subplots()
ax.set_xlabel("Height in cm")
ax.set_ylabel("No of people")
ax.hist(data,bins=bins, color="green",alpha=0.5,edgecolor="green")
```

![使用 Matplotlib 进行数据可视化介绍](../Images/6a58dd5813a19d10aee4f1da2133c099.png)

```py
male = np.random.normal(140, 10,100) # Generating height of 100 males
female = np.random.normal(125,10,100) # Generating height of 100 females
bins = 10
fig,ax = plt.subplots()
ax.set_xlabel("Height in cm")
ax.set_ylabel("No of people")
ax.hist([male,female],bins=bins,label=["Male","Female"])
ax.legend()
```

![使用 Matplotlib 进行数据可视化介绍](../Images/cfc90b1e8829bac9b968c6264efc103e.png)

# 结论

希望你喜欢阅读这篇文章，并且现在能够使用 Matplotlib 执行不同的可视化。如果你有任何想法或反馈，请随时在评论区分享。这里是 [Matplotlib 文档](https://matplotlib.org/stable/index.html)的链接，如果你有兴趣深入了解。

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1)** 是一名有志的软件开发人员，对数据科学和AI在医学中的应用充满热情。Kanwal 被选为2022年亚太地区的 Google Generation Scholar。Kanwal 喜欢通过撰写有关趋势话题的文章来分享技术知识，并热衷于改善女性在科技行业中的代表性。

### 更多相关话题

+   [Python Matplotlib 备忘单](https://www.kdnuggets.com/2023/01/python-matplotlib-cheat-sheets.html)

+   [使用 Matplotlib 和 Seaborn 创建可视化](https://www.kdnuggets.com/creating-visuals-with-matplotlib-and-seaborn)

+   [数据科学、数据可视化和…的前38个 Python 库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [KDnuggets 新闻 22:n16, 4月20日: 最佳 YouTube 学习频道…](https://www.kdnuggets.com/2022/n16.html)

+   [数据科学中的绘图和数据可视化](https://www.kdnuggets.com/2022/06/plotting-data-visualization-data-science.html)

+   [数据可视化的SQL：如何为图表和图形准备数据](https://www.kdnuggets.com/sql-for-data-visualization-how-to-prepare-data-for-charts-and-graphs)
