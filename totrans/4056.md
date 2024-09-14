# 利用 GeoPandas 在 Python 中处理地理空间数据

> 原文：[https://www.kdnuggets.com/leveraging-geospatial-data-in-python-with-geopandas](https://www.kdnuggets.com/leveraging-geospatial-data-in-python-with-geopandas)

空间数据由与位置相关的记录组成。这些数据可以来自 GPS 跟踪、地球观测影像和地图。每个空间数据点可以使用坐标参考系统（如经度/纬度对）在地图上精确定位，这使我们能够调查它们之间的关系。

空间数据的真正潜力在于其将数据点及其对应位置连接起来的能力，创造了无限的高级分析可能性。地理空间数据科学是数据科学中的一个新兴领域，旨在利用地理空间信息，通过空间算法和高级技术（如机器学习或深度学习）提取有价值的见解，以得出有关事件发生及其原因的有意义结论。地理空间数据科学让我们洞察事件发生的地点以及发生的原因。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供 IT 支持

* * *

GeoPandas 是一个开源的 Python 包，专门用于处理信息。它在 pandas 提供的各种数据类型的基础上扩展，提供了对几何对象的空间操作——这使得使用 pandas 数据处理工具 pandas 在 Python 中进行空间分析变得更加便利。由于 GeoPandas 是建立在 Pandas 之上，因此对于熟悉 Python 语法的专业人士来说，可以迅速掌握 GeoPandas 语法。

![利用 GeoPandas 在 Python 中处理地理空间数据](../Images/57a482a9238b63706df83bbc2859b7b9.png)

# 1\. 安装 GeoPandas

我们必须安装 GeoPandas 包才能使用它。然而，需要特别注意的是，GeoPandas 依赖于其他必须安装的库才能正常使用。这些依赖项包括 [**shapely**](https://shapely.readthedocs.io/)，[**Fiona**](https://fiona.readthedocs.io/)，[**pyproj**](https://github.com/pyproj4/pyproj) 和 [**rtree**](https://github.com/Toblerity/rtree)**。 **

你可以通过两种方式下载 GeoPandas 包。首先，你可以使用 conda 安装 GeoPandas conda 包。这种方法推荐使用，因为它将提供 GeoPandas 的依赖项，而无需你自己安装。你可以运行以下命令来安装 GeoPandas：

```py
conda install geopandas
```

第二种方法是使用 pip，这是 Python 中的标准包安装程序。然而，使用此方法将需要安装其余提到的依赖项。

```py
pip install geopandas
```

一旦安装了 GeoPandas 包，你可以使用以下命令将其导入到你的 Python 代码中：

```py
import geopandas as gpd
```

# 2\. 读取与写入空间数据

GeoPandas 用于读取空间数据并将其转换为 **GeoDataFrame**。然而，重要的是要注意有两种主要的空间数据类型：

+   **矢量数据：** 矢量数据使用离散几何（点、线和多边形）描述地球位置的地理特征。

+   **栅格数据：** 栅格数据将世界编码为由网格表示的表面。网格的每个像素由一个连续值或分类类表示。

GeoPandas 主要处理矢量数据。然而，它可以与其他 Python 包结合使用来处理栅格数据，例如 [**rasterio**](https://geopandas-org.translate.goog/en/stable/gallery/geopandas_rasterio_sample.html?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=sc)**.** 你可以使用强大的 **geopandas.read_file()** 函数来读取大多数基于矢量的空间数据。矢量数据主要有两种类型：

+   **Shapefile：** Shapefile 是最常见的数据格式，被认为是行业级的数据类型。它由三个压缩文件组成，通常作为 zip 文件提供：

    **.shp** 文件：此文件包含形状几何体。

    **.dbf** 文件：此文件保存每个几何体的属性，

    **.shx** 文件：这是形状索引文件，帮助将属性链接到形状。

+   **GeoJSON：** 这是一种新的地理空间数据文件格式，于 2016 年发布。由于它只包含一个文件，相比于 **Shapefile**，它更易于使用。

在本文中，我们将使用 **geopandas.read_file()** 函数读取托管在 [**GitHub**](https://github.com/martgnz/bcn-geodata/blob/master/barris/barris.geojson) 上的包含关于巴塞罗那市区不同区域地理空间数据的 GeoJSON 文件。

首先通过加载数据并使用下面的代码打印前五列来开始：

```py
url = 'https://raw.githubusercontent.com/jcanalesluna/bcn-geodata/master/districtes/districtes.geojson'
districts = gpd.read_file(url)
districts.head()
```

![在 Python 中利用 GeoPandas 处理地理空间数据](../Images/93700d532c04edf9c9b30b5a7ba35eb6.png)

接下来，为了将数据写入文件，我们可以使用 **GeoDataFrame.to_file()** 函数将数据写入 **Shapefile**，但你可以通过 **driver** 参数将其转换为 **GeoJSON**。

```py
districts.to_file("districts.geojson", driver="GeoJSON")
```

# 3\. GeoDataFrames 属性

由于**GeoDataFrames**是 pandas DataFrame 的子类，它继承了很多属性。然而，也存在一些区别，主要的区别是它可以存储几何列（也称为 GeoSeries）并执行空间操作。GeoDataFrame中的几何列可以包含各种类型的矢量数据，包括点、线和多边形。然而，只有一列被认为是活动几何列，所有空间操作都将基于该列。

另一个关键特性是每列都有其相关的 CRS 信息，告诉我们候选者在地球上的位置。这个特性之所以关键，是因为如果需要合并两个空间数据集，需要确保它们表达在相同的 CRS 中，否则会得到错误的结果。CRS 信息存储在 GeoPandas 的 crs 属性中：

```py
districts.crs
```

![利用 GeoPandas 在 Python 中的地理空间数据](../Images/31f4b918a845cfe2fc153dadc09783da.png)

现在我们已设置正确的投影 CRS，我们准备好探索 GeoDataFrames 的属性了。

# 4\. 探索 GeoDataFrames

GeoPandas 具有四个有用的方法和属性，可用于探索数据。我们将探讨这四个方法：

+   面积

+   质心

+   边界

+   距离

## 4.1\. 面积

area 属性返回几何体的计算面积。在下面的示例中，我们将计算每个区的面积（单位为 km2）。

```py
districts['area'] = districts.area / 1000000
districts['area']
```

![利用 GeoPandas 在 Python 中的地理空间数据](../Images/a8b24dcfbd134d7190d2d04c3b7d5971.png)

## 4.2\. 质心

第二个属性是质心，它返回几何体的中心点。在下面的代码片段中，我们将添加一个新列并保存每个区的质心：

```py
districts['centroid']=districts.centroid
districts['centroid']
```

![利用 GeoPandas 在 Python 中的地理空间数据](../Images/6d2b8590b1a6b19fad391c35978a61ed.png)

## 4.3\. 边界

第三个方法是边界属性，它计算每个区的多边形边界。下面的代码返回它并将其保存到一个单独的列中：

```py
districts['boundary']=districts.boundary
```

![利用 GeoPandas 在 Python 中的地理空间数据](../Images/b63de49bc7136f86a7b70e0127a4037e.png)

## 4.4\. 距离

distance 方法计算某一几何体到特定位置的最小距离。例如，在下面的代码中，我们将计算从圣家族教堂到巴塞罗那每个区质心的距离。之后，我们将添加距离（单位为 km2）并保存到新列中。

```py
from shapely.geometry import Point

sagrada_fam = Point(2.1743680500855005, 41.403656946781304)
sagrada_fam = gpd.GeoSeries(sagrada_fam, crs=4326)
sagrada_fam= sagrada_fam.to_crs(epsg=2062)
districts['sagrada_fam_dist'] = [float(sagrada_fam.distance(centroid)) / 1000 for centroid in districts.centroid]
```

![利用 GeoPandas 在 Python 中的地理空间数据](../Images/dd7292d0c8b66796fbb93c818f0e6fc4.png)

# 5\. 使用 GeoPandas 绘制数据

绘制和可视化数据是更好地理解数据的关键步骤。使用 GeoPandas 绘图与使用 Pandas 绘图一样简单且直观。这是通过建立在 matplotlib Python 包上的 GeoDataFrame.plot() 函数完成的。

我们先通过绘制巴塞罗那区的基础图来开始探索：

```py
ax= districts.plot(figsize=(10,6))
```

![利用GeoPandas在Python中利用地理空间数据](../Images/e5fd076ccf9a7edcc906b960ab6345f0.png)

这是一个非常基础的图表，未能提供太多信息。然而，我们可以通过为每个区域着色来使其更具信息性。

```py
ax= districts.plot(column='DISTRICTE', figsize=(10,6), edgecolor='black', legend=True)
```

![利用GeoPandas在Python中利用地理空间数据](../Images/f7aeb7ee69677ada997d71de90cefe29.png)

最后，我们可以通过添加区域的质心来向我们的图表中添加更多信息。

```py
import contextily
import matplotlib.pyplot as plt

ax= districts.plot(column='DISTRICTE', figsize=(12,6), alpha=0.5, legend=True)
districts["centroid"].plot(ax=ax, color="green")
contextily.add_basemap(ax, crs=districts.crs.to_string())
plt.title('A Colored Map with the centroid of Barcelona')
plt.axis('off')
plt.show()
```

![利用GeoPandas在Python中利用地理空间数据](../Images/4afa93df50e4a1c7ae79e3811d27a31a.png)

接下来，我们将探索GeoPandas的一个非常重要的特性，即空间关系及其如何相互关联。

# 6. 定义空间关系

地理空间数据在空间上相互关联。GeoPandas使用pandas和shapely包来处理空间关系。本节涵盖常见操作。有两种主要方式来合并GeoPandas DataFrames，即属性连接和空间连接。在本节中，我们将探讨这两者。

## 6.1. 属性连接

属性连接允许你使用非几何变量连接两个GeoPandas DataFrame，这使得它类似于Pandas中的常规连接操作。连接操作使用pandas.merge()方法，如下面的示例所示。在这个例子中，我们将把[**巴塞罗那人口数据**](https://opendata-ajuntament.barcelona.cat/data/es/dataset/est-padro-sexe/resource/cd3125e4-f7d3-4217-8f9d-6d7abdad6ab0)连接到我们的地理空间数据中，以添加更多信息。

```py
import pandas as pd
pop =pd.read_csv('2022_padro_sexe.csv', usecols=['Nom_Districte','Nombre'])
pop = pd.DataFrame(pop.groupby('Nom_Districte')['Nombre'].sum()).reset_index()
pop.columns=['NOM','population_22']
districts = districts.merge(pop)
districts
```

![利用GeoPandas在Python中利用地理空间数据](../Images/3be56fdedc3a281dcc0ec5374743eff4.png)

## 6.2. 空间连接

另一方面，空间连接是基于空间关系来合并数据帧的。在下面的示例中，我们将识别具有[**自行车车道**](https://opendata-ajuntament.barcelona.cat/data/en/dataset/carril-bici/resource/4608cf0c-2f11-4a25-891f-c5afc3af82c5)**的区域。** 我们将首先加载数据，如下面的代码所示：

```py
url = 'https://opendata-ajuntament.barcelona.cat/resources/bcn/CarrilsBici/CARRIL_BICI.geojson'
bike_lane = gpd.read_file(url)
bike_lane = bike_lane.loc[:,['ID','geometry']]
bike_lane.to_crs(epsg=2062, inplace=True)
```

![利用GeoPandas在Python中利用地理空间数据](../Images/706a8dba115cbce5d1eaccb75e93aeda.png)

要在空间上连接两个数据帧，我们可以使用` sjoin()`函数。` sjoin()`函数有四个主要参数：第一个是GeoDataFrame，第二个参数是我们将添加到第一个GeoDataFrame的GeoDataFrame，第三个参数是连接类型，最后一个参数是**谓词**，它定义了我们希望用来匹配两个GeoDataFrame的空间关系。最常见的部分关系有**交叉**、**包含**和**在内**。在这个例子中，我们将使用**交叉**参数。

```py
lanes_districts = gpd.sjoin(districts, bike_lane, how='inner', predicate='intersects')
lanes_districts
```

![利用GeoPandas在Python中利用地理空间数据](../Images/db3714a05546bb3c038c019083c7fd79.png)

在这篇文章中，我向你介绍了如何使用开源的 GeoPandas 库进行地理空间数据分析。我们从下载 GeoPandas 包开始，然后讨论了不同类型的地理空间数据以及如何加载它们。最后，我们将探索一些基本操作，以便你能亲自接触到地理空间数据集。尽管地理空间数据分析还有很多需要探索的内容，但这篇博客作为你学习之旅的起点。

**[Youssef Rafaat](https://www.linkedin.com/in/youssef-hosni-b2960b135)** 是一名计算机视觉研究员和数据科学家。他的研究重点是开发用于医疗保健应用的实时计算机视觉算法。他还在营销、金融和医疗保健领域担任数据科学家超过 3 年。

### 更多相关内容

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [在 Python 中使用 Google Earth 构建地理空间应用程序](https://www.kdnuggets.com/2022/03/building-geospatial-application-python-google-earth-engine-greppo.html)

+   [使用 Geemap 进行地理空间数据分析](https://www.kdnuggets.com/geospatial-data-analysis-with-geemap)

+   [在 Python 中利用 CuPy 发挥 GPU 的强大功能](https://www.kdnuggets.com/leveraging-the-power-of-gpus-with-cupy-in-python)

+   [数据科学中的 SQL：理解和利用连接](https://www.kdnuggets.com/2023/08/sql-data-science-understanding-leveraging-joins.html)

+   [数据库内分析：利用 SQL 的分析功能](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)
