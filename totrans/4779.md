# 使用Folium在Python中可视化地理空间数据

> 原文：[https://www.kdnuggets.com/2018/09/visualising-geospatial-data-python-folium.html](https://www.kdnuggets.com/2018/09/visualising-geospatial-data-python-folium.html)

[评论](#comments)

**由[Parul Pandey](https://www.linkedin.com/in/parul-pandey-a5498975/)提供**

![Folium地图](../Images/d6553c75e17345e93a10225e3cec47bf.png)

数据可视化是一个更广泛的术语，描述了通过将数据放置在视觉上下文中来帮助人们理解数据重要性的任何努力。模式、趋势和关联可以通过视觉方式轻松展示，否则可能在文本数据中被忽视。它是数据科学家工具包的基本组成部分。创建可视化很简单，但创建好的可视化则困难得多。它需要细致的眼光和大量的专业知识来创建既简单又有效的可视化。今天有强大的可视化工具和库，它们重新定义了可视化的意义。

使用Python的优点在于它提供了满足各种数据可视化需求的库。其中一个库是**Folium**，它对于可视化地理数据（**Geo data**）非常有用。地理数据（**Geo data**）科学是数据科学的一个子集，涉及基于位置的数据，即对象及其在空间中的关系的描述。

### **前提条件**

本教程假设你具备基本的Python和Jupyter notebook知识，并熟悉Pandas库。

### **Folium简介**

Folium是一个强大的Python数据可视化库，主要用于帮助人们可视化地理空间数据。使用Folium，可以创建世界上任何位置的地图，只要知道其经纬度值。此外，Folium创建的地图本质上是交互式的，所以在地图渲染后可以进行缩放，这是一项非常有用的功能。

Folium利用了Python生态系统在数据处理方面的优势以及Leaflet.js库在制图方面的优势。数据在Python中处理，然后通过folium在Leaflet地图中可视化。

### **安装**

在使用Folium之前，可能需要通过以下两种方法之一在系统上安装它：

```py

$ pip install folium

```

或

```py

$ conda install -c conda-forge folium

```

### **数据集**

**下载数据集**

我们将使用[世界发展指标](https://www.kaggle.com/worldbank/world-development-indicators)数据集，该数据集是Kaggle上的一个开放数据集。我们将使用数据集中的‘indicators.csv’文件。

由于我们处理的是地理空间地图，因此还需要国家坐标来进行绘制。从[这里](https://github.com/python-visualization/folium/blob/master/examples/data/world-countries.json)下载文件。

文件也可以从我的[github仓库](https://github.com/parulnith/Visualising-Geospatial-data-with-Python/blob/master/world-countries.json)下载。

### **探索数据集**

世界发展指标数据集只是从世界银行实际提供的数据集中稍作修改的版本。它包含了从1960年到2015年，来自约247个国家的超过一千个年度经济发展指标。一些指标包括：

```py
1\. Adolescent fertility rate (births per 1,000 women)
2\. CO2 emissions (metric tons per capita)
3\. Merchandise exports by the reporting economy
4\. Time required to build a warehouse (days)
5\. Total tax rate (% of commercial profits)
6\. Life expectancy at birth, female (years)

```

### **开始使用**

+   转到 Jupyter Notebooks 并导入所需的库。确保在与数据相同的文件夹中创建 Jupyter 笔记本，以方便操作。

+   设置国家坐标

+   读取数据库并探索数据库

### **代码：**

```py

import folium
import pandas as pd

country_geo = 'world-countries.json'

data = pd.read_csv('Indicators.csv')
data.shape

data.head()

```

我们得到如下结果。看来这些指标数据集中，不同国家有不同的指标，包含了指标的年份和数值。

![Folium 国家指标](../Images/36b25563ec5b72ec14eef16624591e03.png)

**出生时女性预期寿命（年）**似乎是一个很好的调查指标。因此，我们提取2013年所有国家的预期寿命数据。我们只是随机选择了这一年。

同时，我们来设置绘图的数据，仅保留国家代码和我们绘制的数值。我们还需要提取指标名称，以便在图例中使用。

### **代码：**

```py

# select Life expectancy for females for all countries in 2013
hist_indicator =  'Life expectancy at birth'
hist_year = 2013

mask1 = data['IndicatorName'].str.contains(hist_indicator) 
mask2 = data['Year'].isin([hist_year])

# apply our mask
stage = data[mask1 & mask2]
stage.head()

#Creating a data frame with just the country codes and the values we want plotted.
data_to_plot = stage[['CountryCode','Value']]
data_to_plot.head()

# labelling the legend
hist_indicator = stage.iloc[0]['IndicatorName']

```

![Folium 预期寿命](../Images/38b0e98192ab09df483e62ef05c40c53.png)

![Folium 国家代码](../Images/4528dff0089a9542db5e1bdcac9f6173.png)

### **创建 Folium 交互地图**

现在我们实际上要创建 Folium 交互地图。我们将创建一个相对较高缩放级别的地图。接下来，我们将使用内置方法 **choropleth** 来附加国家的地理 json 和绘图数据。

### **代码：**

```py

# Setup a folium map at a high-level zoom
map = folium.Map(location=[100, 0], zoom_start=1.5)

# choropleth maps bind Pandas Data Frames and json geometries.
#This allows us to quickly visualize data combinations
map.choropleth(geo_data=country_geo, data=plot_data,
             columns=['CountryCode', 'Value'],
             key_on='feature.id',
             fill_color='YlGnBu', fill_opacity=0.7, line_opacity=0.2,
             legend_name=hist_indicator)

```

我们还需要指定相关参数。‘key on’ 参数指的是 json 对象中的标签，该标签包含了附加在每个国家边界信息上的国家代码作为特征 ID。这是我们需要在数据中设置的连接。数据框中的国家代码应与 json 对象中的特征 ID 匹配。

接下来，我们指定一些美学特征，比如颜色方案、不透明度，然后标注图例。

这个图的输出将被保存为一个实际互动的 html 文件。因此，我们需要将其保存下来，并将其读回到笔记本中，以便在地图上进行交互。

### **代码：**

```py
map.save('plot_data.html')
# Import the Folium interactive html file

from IPython.display import HTML
HTML('<iframe src=plot_data.html width=700 height=450></iframe>')
```

我们将获得如下图所示的地图：

![Folium 地图2](../Images/fa449546d59ceddc76332d849cc9b67d.png)

现在我们有了我们的地图。首先注意，深色表示女性的预期寿命较高。显然，美国和大多数欧洲国家的女性预期寿命较高。

所以，这个示例展示了如何进行地理叠加。它也是如何使用额外的可视化库以及根据我们的可视化需求这些库如何变得强大的一个例子。

这是进入使用 Pandas 数据框和 Folium 进行 choropleth 地图的一个相当简单的第一步。你可以在官方文档页面上探索更多关于 folium 及其提供的交互功能的信息。

要查看地图的实际互动性，请访问[**Github repo**](https://github.com/parulnith/Visualising-Geospatial-data-with-Python)**。**

**相关：**

+   [数据可视化备忘单](https://www.kdnuggets.com/2018/08/data-visualization-cheatsheet.html)

+   [欧洲的大数据、机器学习和数据可视化的未来](https://www.kdnuggets.com/2018/08/ieg-future-big-data-machine-learning-data-visualization-europe.html)

+   [可视化地理空间数据的7种技巧](https://www.kdnuggets.com/2017/10/7-techniques-visualize-geospatial-data.html)

* * *

## 我们的3个最佳课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在IT领域的组织

* * *

### 更多相关主题

+   [5个用于地理空间数据分析的Python包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [在Python中利用GeoPandas进行地理空间数据分析](https://www.kdnuggets.com/leveraging-geospatial-data-in-python-with-geopandas)

+   [使用Google Earth Engine在Python中构建地理空间应用程序…](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [使用Geemap进行地理空间数据分析](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

+   [如何使用Python确定最佳拟合数据分布](https://www.kdnuggets.com/2021/09/determine-best-fitting-data-distribution-python.html)

+   [KDnuggets新闻，8月17日：如何进行运动检测…](https://www.kdnuggets.com/2022/n33.html)
