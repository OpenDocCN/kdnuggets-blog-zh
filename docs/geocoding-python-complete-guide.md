# Python中的地理编码：完整指南

> 原文：[https://www.kdnuggets.com/2022/11/geocoding-python-complete-guide.html](https://www.kdnuggets.com/2022/11/geocoding-python-complete-guide.html)

![Python中的地理编码：完整指南](../Images/a8f230f39f95766be24eca94bc96ee99.png)

照片由[Andrew Stutesman](https://unsplash.com/@drewmark?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上提供

# 介绍

在处理大数据集进行机器学习时，你是否遇到过看起来像这样的地址列？

![Python中的地理编码：完整指南](../Images/6318cb0b25a530d5683bbb65e57515ca.png)

图片由作者提供

位置数据可能非常混乱且难以处理。

编码地址很困难，因为它们的基数非常高。如果你尝试使用像独热编码这样的技术对这样的列进行编码，会导致高维度，并且你的机器学习模型可能表现不佳。

克服这个问题的最简单方法是**地理编码**这些列。

# 什么是地理编码？

地理编码是将地址转换为地理坐标的过程。这意味着你将把原始地址转换为纬度/经度对。

# Python中的地理编码

有许多不同的库可以帮助你用Python完成这项工作。最快的是**Google Maps API**，如果你需要在短时间内转换超过1000个地址，我推荐使用它。

然而，Google Maps API并不是免费的。你需要为每1000个请求支付约$5。

Google Maps API的一个免费替代方案是OpenStreetMap API。然而，OpenStreetMap API的速度较慢，且准确性稍差。

在这篇文章中，我将带你通过这两个API的地理编码过程。

# 方法1：Google Maps API

首先使用Google Maps API将地址转换为纬度/经度对。你需要首先创建一个Google Cloud账户，并输入你的信用卡信息。

尽管这是一个付费服务，但Google在你第一次创建Google Cloud账户时会提供$200的免费信用。这意味着在收费之前，你可以使用他们的地理编码API大约40,000次。只要你没有达到这个限制，你的账户将不会被收费。

首先，[设置一个免费的账户](https://cloud.google.com/gcp/getting-started)在Google Cloud上。然后，一旦你设置了账户，你可以跟随[这个](https://www.youtube.com/watch?v=OGTG1l7yin4)教程获取你的Google Maps API密钥。

一旦你获得了API密钥，就可以开始编码了！

## 前提条件

我们将使用[Zomato餐馆Kaggle](https://www.kaggle.com/shrutimehta/zomato-restaurants-data)数据集进行本教程。确保将数据集安装在你的路径中。然后，使用以下命令安装googlemaps API包：

```py
pip install -U googlemaps
```

## 导入

运行以下代码行来导入你需要开始的库：

```py
pip install -U googlemaps
```

## 读取数据集

现在，让我们读取数据集并检查数据框的前几行：

```py
data = pd.read_csv('zomato.csv',encoding="ISO-8859-1")
df = data.copy()
df.head()
```

![Geocoding in Python: A Complete Guide](../Images/e1b79a68ffe95d0cecb702963c56f7de.png)

图片作者提供

这个数据框有21列和9551行。

我们只需要*address*列进行地理编码，所以我将删除所有其他列。然后，我将删除重复项，以便只保留唯一的地址：

```py
df = df[['Address']]
df = df.drop_duplicates()
```

再次查看数据框的前几行，我们只能看到*address*列：

![Geocoding in Python: A Complete Guide](../Images/ee9199fd5796fd53ca08a9493e25cbc2.png)

图片作者提供

很好！我们现在可以开始地理编码了。

## 地理编码

首先，我们需要用Python访问我们的API密钥。运行以下代码行来完成此操作：

```py
gmaps_key = googlemaps.Client(key="your_API_key")
```

现在，让我们尝试首先进行一个地址的地理编码，并查看输出结果。

```py
add_1 = df['Address'][0]
g = gmaps_key.geocode(add_1)
lat = g[0]["geometry"]["location"]["lat"]
long = g[0]["geometry"]["location"]["lng"]
print('Latitude: '+str(lat)+', Longitude: '+str(long))
```

上述代码的输出如下所示：

![Geocoding in Python: A Complete Guide](../Images/d312aa8edf8c0b0b987705c0356fadba.png)

图片作者提供

如果你得到了上述输出，太好了！一切正常。

我们现在可以对整个数据框重复这个过程：

```py
# geocode the entire dataframe:

def geocode(add):
    g = gmaps_key.geocode(add)
    lat = g[0]["geometry"]["location"]["lat"]
    lng = g[0]["geometry"]["location"]["lng"]
    return (lat, lng)

df['geocoded'] = df['Address'].apply(geocode)
```

让我们再次查看数据框的前几行，看看这是否有效：

```py
df.head()
```

![Geocoding in Python: A Complete Guide](../Images/4bd8b23bd238c16246a789a5a5fe0fcf.png)

如果你的输出与上面的截图类似，恭喜你！你已经成功对整个数据框进行了地址地理编码。

# 方法2：OpenStreetMap API

OpenStreetMap API完全免费，但比Google地图API更慢且准确度较低。

这个API无法找到数据集中的许多地址，因此这次我们将使用*locality*列。

在我们开始教程之前，让我们看看*address*列和*locality*列之间的区别。运行以下代码行来完成此操作：

```py
print('Address: '+data['Address'][0]+'\n\nLocality: '+data['Locality'][0])
```

你的输出将如下所示：

![Geocoding in Python: A Complete Guide](../Images/09756fc883050be5cc34decca0a16b64.png)

图片作者提供

*address*列比*locality*列更详细，它提供了餐厅的确切位置，包括楼层号。这可能是地址未被OpenStreetMap API识别的原因，但*locality*却被识别了。

让我们对第一个*locality*进行地理编码，并查看输出结果。

## 地理编码

运行以下代码行：

```py
import url
import requests

data = data[['Locality']]

url = 'https://nominatim.openstreetmap.org/search/' + urllib.parse.quote(df['Locality'][0]) +'?format=json'
response = requests.get(url).json()
print('Latitude: '+response[0]['lat']+', Longitude: '+response[0]['lon'])
```

上述代码的输出与Google Maps API生成的结果非常相似：

![Geocoding in Python: A Complete Guide](../Images/48e35458c0ec81e3387fb6831a5a871d.png)

图片作者提供

现在，让我们创建一个函数来查找整个数据框的坐标：

```py
def geocode2(locality):
    url = 'https://nominatim.openstreetmap.org/search/' + urllib.parse.quote(locality) +'?format=json'
    response = requests.get(url).json()
    if(len(response)!=0):
        return(response[0]['lat'], response[0]['lon'])
    else:
        return('-1')

data['geocoded'] = data['Locality'].apply(geocode2)
```

很好！现在，让我们查看数据框的前几行：

```py
data.head(15)
```

请注意，这个API无法为数据框中的许多地点生成坐标。

尽管这是Google Maps API的一个很好的免费替代品，但如果使用OpenStreetMap进行地理编码，你可能会丢失很多数据。

本教程就到这里！希望你能从中学到一些新知识，对处理地理空间数据有更好的理解。

祝你在数据科学之旅中好运，感谢你的阅读！

**[Natassha Selvaraj](https://www.natasshaselvaraj.com/)** 是一位自学成才的数据科学家，热衷于写作。你可以通过[LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/)与她联系。

[原文](https://www.natasshaselvaraj.com/a-step-by-step-guide-on-geocoding-in-python/)。转载已获许可。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 支持

* * *

### 更多相关话题

+   [数据科学家的地理编码](https://www.kdnuggets.com/2023/06/geocoding-data-scientists.html)

+   [决策树软件完全指南](https://www.kdnuggets.com/2022/08/complete-guide-decision-tree-software.html)

+   [如何建立数据科学赋能团队：完全指南](https://www.kdnuggets.com/2022/10/build-data-science-enablement-team-complete-guide.html)

+   [KDnuggets 新闻, 5月 25: 每个…都应该知道的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/n21.html)

+   [KDnuggets 新闻, 8月 17: 如何使用…进行运动检测](https://www.kdnuggets.com/2022/n33.html)

+   [KDnuggets™ 新闻 22:n06, 2月 9: 数据科学编程…](https://www.kdnuggets.com/2022/n06.html)
