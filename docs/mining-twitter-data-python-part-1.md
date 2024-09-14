# 《使用 Python 矿工 Twitter 数据 第 1 部分：数据收集》

> 原文：[https://www.kdnuggets.com/2016/06/mining-twitter-data-python-part-1.html](https://www.kdnuggets.com/2016/06/mining-twitter-data-python-part-1.html)

**作者：Marco Bonzanini，独立数据科学顾问**。

[Twitter](https://www.twitter.com/) 是一个流行的社交网络，用户可以分享短消息，称为 *推文*。用户在 Twitter 上分享思想、链接和图片，记者评论实时事件，公司推广产品并与客户互动。使用 Twitter 的方式可以非常多样，每天有 5 亿条推文，有大量数据供分析和使用。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

这是一个系列文章的第一篇，专注于使用 Python 挖掘 Twitter 数据。在这一部分，我们将探讨从 Twitter 收集数据的不同选项。一旦我们建立了数据集，在接下来的文章中我们将讨论一些有趣的数据应用。

![Twitter 横幅](../Images/3da5b4dea824ea453ca3ae25f3548634.png)

### 注册你的应用程序

为了以编程方式访问 Twitter 数据，我们需要创建一个与 Twitter API 互动的应用程序。

第一步是注册你的应用程序。具体来说，你需要将浏览器指向 [http://apps.twitter.com](http://apps.twitter.com/)，登录 Twitter（如果尚未登录）并注册一个新应用程序。你现在可以为你的应用程序选择一个名称和描述（例如“矿工演示”或类似名称）。你将收到一个 *consumer key* 和一个 *consumer secret*：这些是应用程序设置，应该始终保密。你还可以从应用程序的配置页面申请一个访问令牌和一个访问令牌秘密。与 consumer keys 类似，这些字符串也必须保密：它们提供应用程序代表你的账户访问 Twitter。默认权限是只读，这正是我们所需的，但如果你决定更改权限以提供写入功能，你必须协商一个新的访问令牌。

重要说明：使用 Twitter API 有速率限制，并且如果你想提供可下载的数据集也有限制，请参见：

+   [https://dev.twitter.com/overview/terms/agreement-and-policy](https://dev.twitter.com/overview/terms/agreement-and-policy)

+   [https://dev.twitter.com/rest/public/rate-limiting](https://dev.twitter.com/rest/public/rate-limiting)

### 访问数据

Twitter 提供了 [REST APIs](https://dev.twitter.com/rest/public) 供我们与他们的服务交互。也有 [一堆基于 Python 的客户端](https://dev.twitter.com/overview/api/twitter-libraries#python) 可供使用，我们无需重新发明轮子。特别是，[Tweepy](http://tweepy.readthedocs.org/) 是一个最有趣且易于使用的库，所以我们来安装它：

```py
pip install tweepy==3.3.0

```

*更新*：Tweepy 的 3.4.0 版本引入了 Python 3 的问题，目前在 [github](https://github.com/tweepy/tweepy) 上已修复，但通过 `pip` 仍不可用，因此我们使用版本 3.3.0 直到新版本发布。

*更多更新*：Tweepy 的 3.5.0 版本，已通过 pip 提供，似乎解决了上述提到的 Python 3 问题。

为了授权我们的应用程序代表我们访问 Twitter，我们需要使用 OAuth 接口：

```py
import tweepy
from tweepy import OAuthHandler

consumer_key = 'YOUR-CONSUMER-KEY'
consumer_secret = 'YOUR-CONSUMER-SECRET'
access_token = 'YOUR-ACCESS-TOKEN'
access_secret = 'YOUR-ACCESS-SECRET'

auth = OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_secret)

api = tweepy.API(auth)

```

`api` 变量现在是我们可以执行的大多数 Twitter 操作的入口点。

例如，我们可以通过以下方式读取自己的时间线（即我们的 Twitter 首页）：

```py
for status in tweepy.Cursor(api.home_timeline).items(10):
    # Process a single status
    print(status.text) 

```

Tweepy 提供了方便的 Cursor 接口，用于迭代不同类型的对象。在上面的示例中，我们使用 *10* 来限制我们读取的推文数量，但我们当然可以访问更多。`status` 变量是 `Status()` 类的一个实例，这是一个很好的封装，用于访问数据。来自 Twitter API 的 JSON 响应在属性 `_json` 中（以一个下划线开头），这不是原始的 JSON 字符串，而是一个字典。

所以上面的代码可以重写为处理/存储 JSON：

```py
for status in tweepy.Cursor(api.home_timeline).items(10):
    # Process a single status
    process_or_store(status._json) 

```

如果我们想要一个包含所有关注者的列表？这就来了：

```py
for friend in tweepy.Cursor(api.friends).items():
    process_or_store(friend._json)

```

那么，所有推文的列表呢？很简单：

```py
for tweet in tweepy.Cursor(api.user_timeline).items():
    process_or_store(tweet._json)

```

通过这种方式，我们可以轻松收集推文（以及更多）并以原始 JSON 格式存储，转换成不同的数据模型相当容易，具体取决于我们的存储（许多 NoSQL 技术提供一些批量导入功能）。

函数 `process_or_store()` 是你自定义实现的占位符。在最简单的形式下，你可以将 JSON 打印出来，每条推文占一行：

```py
def process_or_store(tweet):
    print(json.dumps(tweet))

```

### 流式

如果我们想要“保持连接打开”，并收集关于特定事件的所有即将到来的推文，流式 API 是我们需要的。我们需要扩展 `StreamListener()` 以自定义处理传入数据的方式。一个工作示例，收集带有 #python 标签的所有新推文：

```py
from tweepy import Stream
from tweepy.streaming import StreamListener

class MyListener(StreamListener):

    def on_data(self, data):
        try:
            with open('python.json', 'a') as f:
                f.write(data)
                return True
        except BaseException as e:
            print("Error on_data: %s" % str(e))
        return True

    def on_error(self, status):
        print(status)
        return True

twitter_stream = Stream(auth, MyListener())
twitter_stream.filter(track=['#python'])

```

根据搜索词，我们可以在几分钟内收集大量推文。尤其是对于具有全球覆盖范围的实时事件（世界杯、超级碗、奥斯卡奖等），请关注 JSON 文件以了解其增长速度，并考虑你可能需要多少推文用于测试。上述脚本将每条推文保存在新的一行中，因此你可以使用命令`wc -l python.json`从 Unix shell 中知道你收集了多少条推文。

你可以在以下 Gist 中看到 Twitter Stream API 的一个最小工作示例：

[twitter_stream_downloader.py](https://gist.github.com/bonzanini/af0463b927433c73784d)

### 摘要

我们介绍了`tweepy`作为一种用 Python 轻松访问 Twitter 数据的工具。我们可以收集不同类型的数据，显然重点是“推文”对象。

一旦我们收集了一些数据，在分析应用方面的可能性是无穷无尽的。在接下来的章节中，我们将讨论一些选项。

**简介： [Marco Bonzanini](https://twitter.com/marcobonzanini)** 是一位驻伦敦的数据科学家。活跃于 PyData 社区，他喜欢从事文本分析和数据挖掘应用。他是《"[用 Python 掌握社交媒体挖掘](https://www.amazon.com/Mastering-Social-Media-Mining-Python-ebook/dp/B01BFD2Z2Q)"》(Packt Publishing, 2016年7月)的作者。

[原文](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/)。经授权转载。

**相关**：

+   [数据觉醒：星球大战情感分析](/2016/01/data-awakens-star-wars-sentiment-analysis.html)

+   [教程：构建 Twitter 情感分析过程](/2015/11/tutorial-twitter-sentiment-analysis.html)

+   [通过大数据视角剖析大数据 Twitter 社区](/2015/09/dissecting-big-data-twitter-community.html)

### 更多相关话题

+   [KDnuggets 新闻，4月6日：8 门免费 MIT 数据科学课程…](https://www.kdnuggets.com/2022/n14.html)

+   [数据科学备忘单完整集合 - 第 1 部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-1.html)

+   [构建视觉搜索引擎 - 第 1 部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [数据科学备忘单完整集合 - 第 2 部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-2.html)

+   [数据存储库完整集合 - 第 1 部分](https://www.kdnuggets.com/2022/04/complete-collection-data-repositories-part-1.html)

+   [数据存储库完整集合 - 第 2 部分](https://www.kdnuggets.com/2022/04/complete-collection-data-repositories-part-2.html)
