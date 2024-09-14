# 使用 Python 进行 Twitter 数据挖掘 第7部分：地理位置和交互式地图

> 原文：[https://www.kdnuggets.com/2016/07/mining-twitter-data-python-part-7.html](https://www.kdnuggets.com/2016/07/mining-twitter-data-python-part-7.html)

**作者：Marco Bonzanini，独立数据科学顾问。**

[地理定位](https://en.wikipedia.org/wiki/Geolocation)是识别对象地理位置的过程，例如移动电话或计算机。Twitter 允许用户在发布推文时提供他们的位置，形式为纬度和经度坐标。有了这些信息，我们准备好为我们的数据创建一些不错的交互式地图可视化。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

本文简要介绍了[GeoJSON](http://geojson.org/)格式和[Leaflet.js](http://leafletjs.com/)，这是一个用于交互式地图的优秀 Javascript 库，并讨论了它与我们在本教程前面部分收集的 Twitter 数据的集成（详细信息请参见[第4部分](https://marcobonzanini.com/2015/03/23/mining-twitter-data-with-python-part-4-rugby-and-term-co-occurrences/)）。

### GeoJSON

GeoJSON 是一种用于编码地理数据结构的格式。该格式支持各种几何类型，这些类型可以用于将所需的形状可视化到地图上。对于我们的示例，我们只需要最简单的结构，即`Point`。点通过其坐标（纬度和经度）来识别。

在 GeoJSON 中，我们还可以表示如`Feature`或`FeatureCollection`这样的对象。第一个基本上是一个带有附加属性的几何体，而第二个是特征列表。

我们的 Twitter 数据集可以表示为 GeoJSON 中的一个`FeatureCollection`，其中每条推文将是一个包含单个几何体（即前面提到的`Point`）的独立`Feature`。

这就是 JSON 结构的样子：

```py
{
    "type": "FeatureCollection",
    "features": [
        { 
            "type": "Feature",
            "geometry": {
                "type": "Point", 
                "coordinates": [some_latitude, some_longitude]
            },
            "properties": {
                "text": "This is sample a tweet",
                "created_at": "Sat Mar 21 12:30:00 +0000 2015"
            }
        },
        /* more tweets ... */
    ]
}

```

### 从推文到 GeoJSON

假设数据存储在单个文件中，如本教程第一章所述，我们只需遍历所有推文，查找可能存在的*coordinates*字段。请记住，你需要使用*coordinates*，因为*geo*字段已被弃用（请参见[API](https://dev.twitter.com/overview/api/tweets)）。

这段代码将读取数据集，查找坐标明确给出的推文。一旦创建了 GeoJSON 数据结构（以 Python 字典形式），数据将被转储到名为 `geo_data.json` 的文件中：

```py
# Tweets are stored in "fname"
with open(fname, 'r') as f:
    geo_data = {
        "type": "FeatureCollection",
        "features": []
    }
    for line in f:
        tweet = json.loads(line)
        if tweet['coordinates']:
            geo_json_feature = {
                "type": "Feature",
                "geometry": tweet['coordinates'],
                "properties": {
                    "text": tweet['text'],
                    "created_at": tweet['created_at']
                }
            }
            geo_data['features'].append(geo_json_feature)

# Save geo data
with open('geo_data.json', 'w') as fout:
    fout.write(json.dumps(geo_data, indent=4))

```

### 使用 Leaflet.js 的互动地图

[Leaflet.js](http://leafletjs.com/) 是一个开源的 Javascript 库，用于创建互动地图。你可以创建包含你选择的瓦片（例如来自 OpenStreetMap 或 MapBox）的地图，并叠加互动组件。

为了准备一个可以托管地图的网页，你只需在文档的头部部分包含库及其 CSS，添加以下几行代码：

```py
<link rel="stylesheet" href="http://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.css" />
<script src="http://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.js"></script>

```

此外，我们将所有的 GeoJSON 数据放在一个单独的文件中，因此我们希望动态加载数据，而不是手动将所有点放在地图上。为此，我们可以轻松地使用[jQuery](https://jquery.com/)，这也是我们需要包括的：

```py
<script src="http://code.jquery.com/jquery-2.1.0.min.js"></script>

```

地图本身将放置在一个 `div` 元素中：

```py
<script src="http://code.jquery.com/jquery-2.1.0.min.js"></script><!-- this goes in the <head> -->
<style>
#map {
    height: 600px;
}
</style>
<!-- this goes in the <body> -->
<div id="map"></div>

```

我们现在准备使用 Leaflet 创建地图：

```py
// Load the tile images from OpenStreetMap
var mytiles = L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
});
// Initialise an empty map
var map = L.map('map');
// Read the GeoJSON data with jQuery, and create a circleMarker element for each tweet
// Each tweet will be represented by a nice red dot
$.getJSON("./geo_data.json", function(data) {
    var myStyle = {
        radius: 2,
        fillColor: "red",
        color: "red",
        weight: 1,
        opacity: 1,
        fillOpacity: 1
    };

    var geojson = L.geoJson(data, {
        pointToLayer: function (feature, latlng) {
            return L.circleMarker(latlng, myStyle);
        }
    });
    geojson.addTo(map)
});
// Add the tiles to the map, and initialise the view in the middle of Europe
map.addLayer(mytiles).setView([50.5, 5.0], 5);

```

结果的截图：

[![rugby-map-osm](../Images/74995a7417291a318ded650623b26b71.png)](https://marcobonzanini.files.wordpress.com/2015/06/rugby-map-osm.png)

上面的例子使用了 OpenStreetMap 的瓦片图像，但 Leaflet 允许你选择其他服务。例如，在下面的截图中，瓦片图像来自 MapBox。

[![rugby-map-mapbox](../Images/320461a68e73c99fbde1fe78eb3e0d5b.png)](https://marcobonzanini.files.wordpress.com/2015/06/rugby-map-mapbox.png)

你可以在这里看到互动地图的实际效果：

+   [OpenStreetMap 的瓦片](http://bonzanini.github.io/mining-twitter/rugby-osm.html)

+   [MapBox 的瓦片](http://bonzanini.github.io/mining-twitter/rugby-mapbox.html)

### 总结

一般来说，Python 有许多数据可视化的选项，但在基于浏览器的交互方面，Javascript 也是一个有趣的选择，两种语言可以很好地配合使用。本文展示了构建简单互动地图是一个相当直接的过程。

几行 Python 代码就能将数据转换为可以传递给 Javascript 进行可视化的通用格式（GeoJSON）。Leaflet.js 是一个很棒的 Javascript 库，几乎开箱即用，让我们创建一些不错的互动地图。

**个人简介：[Marco Bonzanini](https://twitter.com/marcobonzanini)** 是一位常驻伦敦的 数据科学家。他活跃于 PyData 社区，喜欢从事文本分析和数据挖掘应用。他是《[用 Python 掌握社交媒体挖掘](https://www.amazon.com/Mastering-Social-Media-Mining-Python-ebook/dp/B01BFD2Z2Q)》（Packt Publishing，2016年7月）的作者。

[原文](https://marcobonzanini.com/2015/06/16/mining-twitter-data-with-python-and-js-part-7-geolocation-and-interactive-maps/)。转载已获许可。

**相关内容**：

+   [用 Python 挖掘 Twitter 数据 第4部分：橄榄球和术语共现](/2016/06/mining-twitter-data-python-part-4.html)

+   [用 Python 矿工 Twitter 数据 第 5 部分：数据可视化基础](/2016/06/mining-twitter-data-python-part-5.html)

+   [用 Python 矿工 Twitter 数据 第 6 部分：情感分析基础](/2016/07/mining-twitter-data-python-part-6.html)

### 更多相关主题

+   [使用 Pandas 创建美丽互动可视化的最简单方法](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

+   [机器学习不像你的大脑 第 6 部分：……的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [KDnuggets 新闻，4 月 6 日：8 门免费的 MIT 数据科学课程](https://www.kdnuggets.com/2022/n14.html)

+   [数据科学书籍完全合集 - 第 1 部分](https://www.kdnuggets.com/2022/05/complete-collection-data-science-books-part-1.html)

+   [数据科学书籍完全合集 - 第 2 部分](https://www.kdnuggets.com/2022/05/complete-collection-data-science-books-part-2.html)

+   [数据科学面试完全合集 – 第 1 部分](https://www.kdnuggets.com/2022/06/complete-collection-data-science-interviews-part-1.html)
