# 使用 Python API 进行数据科学项目

> 原文：[https://www.kdnuggets.com/2021/09/python-apis-data-science-project.html](https://www.kdnuggets.com/2021/09/python-apis-data-science-project.html)

[评论](#comments)<picture>![使用 Python API 进行数据科学项目](../Images/70890df2bdfc855812b2da3ffefaaaf6.png)</picture> * * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

使用 API 进行数据科学是所有数据科学家必备的[技能](https://www.stratascratch.com/blog/most-in-demand-data-science-technical-skills/)，应纳入你的数据科学项目中。在我们之前的博客 - [数据分析项目创意](https://www.stratascratch.com/blog/data-analytics-project-ideas-that-will-get-you-the-job/)，我们概述了你所需的唯一数据科学项目，并讨论了使用 API 收集数据的重要性。因此，在这篇文章中，我们想展示如何[使用 Python](https://www.stratascratch.com/blog/how-much-python-is-required-for-data-science/) 从 YouTube API 中提取数据，并使用 Python 中的请求库。

所以让我们深入了解如何使用 API 进行数据科学。我们将提取数据并查看 JSON 响应，然后将所有这些数据保存到 Pandas DataFrame 中。

<picture>![如何使用 API 进行数据科学](../Images/84584c537b252fc875798a0a958762df.png)</picture> 我们将使用良好的软件工程技能以编程方式完成所有工作，以使你的代码看起来简洁清晰，而不是像某个 10 岁的小孩写的那样。本文并非关于如何使用 YouTube API，而是如何一般性地使用 API，因此我们会确保使用可适用于任何 API 服务的库和技术。

## 为什么要从 API 收集数据？

你可能会想，为什么我们不直接使用 CSV 或从数据库中提取数据？你应该学习 API 的原因有两个：

**1.** API 是一种非常常见的行业、专业数据收集方式，因此如果你将来从事数据科学工作，你需要学习如何使用它。

**2.** 与从数据库提取数据相比，这是一种更复杂和高级的数据收集方式。因此，这也是学习 API 的另一个理由，可以给你的同事和招聘经理留下深刻印象。

### 工具和平台

**平台**

我们将使用 Google Colabs，它基本上是 Jupyter notebooks。如果你愿意，可以使用 Jupyter notebooks，但我们将使用 Colabs，因为它容易启动并将工作保存到 Google 云端硬盘中。

**导入**

现在第一步是导入库。

```py
#import libraries
import requests
import pandas as pd
import time
```

<picture>![导入 APIs](../Images/5d35319068d540d42e3a2e39ed8dc9f4.png)</picture> requests 库是一个允许我们进行 API 调用的库。你可以使用这个库向任何 API 发出请求，因此根据你想从哪个 API 获取数据，所覆盖的技术将是相同的。如果你想了解更多关于 requests 库的信息，下面是一个链接 - https://realpython.com/python-requests/。然后我们有 Pandas 库，因为我们将把数据保存到 Pandas DataFrame 中，还有一个时间库。

**API 密钥**

下一步是获取 API 密钥。我们将从 Youtube API 获取数据，特别是从我们的频道中获取数据。为了通过 API 访问我们的频道信息，我们需要申请一个 API 密钥。你可以通过访问这个链接 - https://www.slickremix.com/docs/get-api-key-for-youtube/ 来完成。我们不想在这篇文章中详细讲解如何使用 Youtube API，所以我们将留给你自己去获取 API 密钥。但一般来说，每当你使用 API 时，你总是需要一个密钥。获取密钥的方法因每个 API 服务而异。所以假设你完成了这个过程并获得了你的 API 密钥。

现在我们将获取我们频道中的所有视频，然后从每个视频中获取指标：

**频道中的视频列表**

+   视频 ID

+   视频标题

+   发布日期

**视频指标**

+   观看次数

+   链接数量

+   不喜欢次数

+   评论数量

现在我们需要的是我们的频道 ID。这两个参数将用于进行 API 调用。

```py
#keys
API_KEY = "AIXXXXXXXX"
CHANNEL_ID = "UCW8Ews7tdKKkBT6GdtQaXvQ"
```

### 使用 requests 库进行测试

让我们快速测试一下 API 调用。使用 requests 库，你可以通过将 API 的 URL 放入 get() 方法中来进行调用。

```py
#make API call
response = requests.get('https://api.github.com').json()
```

<picture>![使用 requests 库进行测试](../Images/da5cc43a29478a7fa00970e07ad9ec96.png)</picture> 要获取一些数据，我们使用 get() 方法。数据位于 api.github.com。我们将 URL 传递给 get() 方法，并添加 json() 方法，它将在响应中返回一个 JSON 对象。

**什么是 JSON 文件？**

它是一种常见的数据文件，以 JS 对象形式发送，并通常以属性-值对或数组的形式包含你的数据。并将数据保存在一个叫做 response 的变量中。

现在让我们查看数据：

```py
response
```

<picture>![使用 requests 库进行测试](../Images/5f42c9380dea80a3243c3e192fa6fc83.png)</picture> 正如你在输出中看到的，整个结果被括在大括号中，每一行都有一个属性或键，每个键都有一个值。这是一个你可以访问的特定信息的 URL 列表，来自 Github。

例如，如果你想查找用户的电子邮件，你可以在 get() 方法中使用 email_url。

```py
emails_url': 'https://api.github.com/user/emails'
```

我们刚刚测试了请求库，并快速测试了其功能。现在让我们调用 Youtube API 并抓取一些数据。

### 与 YouTube API 的协作

所以进行 API 调用最困难的部分是弄清楚如何构建 URL，主要是要在 URL 中添加哪些参数。我们目前有一个根 URL。

```py
url = "https://www.googleapis.com/youtube/v3/
```

这是我们数据的位置。我们只需要定义要收集的数据类型。为此，我们现在需要将参数添加到 URL 中，以从特定频道获取特定的视频信息。

最困难的部分是弄清楚要在 URL 中添加哪些参数和属性？你如何弄清楚这一点？最佳的方法是阅读官方文档。[YouTube API 官方文档](https://developers.google.com/youtube/v3/docs/search)

我们将进行一次“搜索”，并包含多个参数，如“part”、“channelID”和我的 API 密钥。在“parts”参数中，我们将添加 id 和 snippet 属性，以抓取包含 videoID 和有关视频本身的信息的 ID 数据，如你在列表中看到的。

<picture>![与 YouTube API 的协作](../Images/ca0756b2d7890b238e54ad0319065ca8.png)</picture> 现在我们将编写包含参数的整个 URL，这将给我们提供我们想要收集的所有数据。再说一次，这篇文章并非专门关于 YouTube，所以我们不会深入讨论我们如何弄清楚使用哪些参数等等。很多都是试错的过程。但让我们指导你如何构建这个 URL。

```py
url = "https://www.googleapis.com/youtube/v3/search?key="+API_KEY+"&
channelId="+CHANNEL_ID+"&part=snippet,id&order=date&maxResults=10000"
+pageToken
```

我们通过 YouTube API 执行“搜索”。所有位于“？”右侧的部分是我们添加的参数，用于请求特定的信息。

+   首先，我们在这个 key 参数中添加存储在 API_KEY 变量中的 API 密钥。

+   我们指定要收集信息的频道 ID。

+   接下来是 part 参数，我们指定要获取 snippet 和 ID 数据。从文档中可以看到，当我们请求 snippet 和 ID 数据时，可以期待获得哪些数据。

+   按日期排序数据，然后我们希望 API 调用中的 maxResults 为 10000 个视频。

+   最后，pageToken 是一个令牌，即一个代码，它用于获取搜索结果的下一页。我们将在稍后尝试提取所有数据时处理它。

构建这个可能很困难，而且需要很多试错。但是一旦你玩弄它并获取到所需的数据，你就不必再担心了。所以整个 URL 被保存在我们的 URL 变量中。

### API 调用的响应

我们以与 GitHub 示例相同的方式进行 API 调用。

```py
pageToken = ""
response = requests.get(url).json()
```

这是 API 调用的输出。

<picture>![API 调用的响应](../Images/1ded876e6225f683eb6564a7c923c1ee.png)</picture> 如你所见，我们在响应变量中保存了相同的 JSON 对象。你会看到所有 id 和 snippet 的属性。

### 数据解析

你如何理解这些数据？首先，让我们确定我们感兴趣的内容是什么？我们看到响应顶部的 `etag` 键，然后是响应中的第二个键 `items` 键。`items` 键以方括号开始，然后基本上列出了我们频道中的所有视频。如果我们查看响应的末尾，我们最终会看到频道中的最后一个视频以及 `items` 键的结束方括号。然后我们会看到其他的键，如 `kind`、`nextPageToken` 等等。

你还可以看到我们有 95 个结果但只检索了 50 个。`nextPageToken` 键将帮助我们获取搜索结果的下一页视频。但我们稍后会讨论这一点。

所以我们的 API 调用给我们提供了所有视频的搜索结果以及一些关于视频的信息。所有这些信息都存储在 `items` 键中。所以让我们只抓取这些数据并过滤掉其他的。

你可以通过指定 `items` 键来轻松完成这一操作。

```py
response['items']
```

<picture>![解析数据](../Images/533342403610eb0fb029f7312a1c7e93.png)</picture> 你会看到输出以方括号开始，并列出了我们频道上的所有视频。为了孤立一个视频，我们可以指定位置。

```py
response['items'][0]
```

<picture>![使用 Python API 进行数据科学的孤立视频](../Images/16f99992a89b0e2af2fb1ff5d8bd2e61.png)</picture> 所以，这是我们最新的视频。

### 解析输出并将其保存到变量中

很明显，我们需要做的是遍历 `items` 键中的所有视频并保存特定的信息。让我们先将信息保存到变量中，然后最后构建循环。

让我们保存视频 ID。为此，我们需要调用 `response` 变量、`item` 键和第一个位置。然后一旦我们得到这个，我们选择 `id` 键和 `videoID` 键。我们可以将该值保存在 `video_id` 变量中。这基本上是你如何遍历数组并保存你想要的数据。

```py
video_id = response['items'][0]['id']['videoId']
```

对视频标题做相同的操作。这里我们还将任何 `&` 符号替换为空白

```py
video_title = response['items'][0]['snippet']['title']
video_title = str(video_title).replace("&","")
```

然后让我们抓取上传日期

```py
upload_date = response['items'][0]['snippet']['publishedAt']
```

我们只想抓取日期，省略时间戳。

<picture>![解析输出并将其保存到变量中](../Images/524aaef64e8fc910d9e5f5813a1f0056.png)</picture> 为了做到这一点，我们可以在 `T` 上拆分，并抓取输出的左侧部分。

```py
upload_date = str(upload_date).split("T")[0]
```

所以这就是你如何保存所有信息。

### 创建循环

让我们创建一个循环，遍历在 API 调用中收集的所有视频并保存信息。

我们将遍历 `response['items']` 数组，所以我们的 `for loop` 从

```py
for video in response['items']:
```

接下来，我们需要添加一些逻辑，确保我们只收集视频信息。因此，为了确保这一点，我们需要确保只关注视频。如果你查看响应，你会看到

```py
'kind': 'youtube#video',
```

所以我们将添加一个 `if` 语句来确保我们保存的是视频信息。正如你所看到的，我们不是使用 `response['items']` 而是使用 `video` 变量，因为我们在 `for loop` 中。

```py
if video['id']['kind'] == "youtube#video":
```

最后，我们构建的所有用于收集响应信息的变量，我们将再次使用这些变量，但只需将变量名更改为 video。你的最终结果将如下所示。

```py
for video in response['items']:
   if video['id']['kind'] == "youtube#video":
       video_id = video['id']['videoId']
       video_title = video['snippet']['title']
       video_title = str(video_title).replace("&amp;","")
       upload_date = video['snippet']['publishedAt']
       upload_date = str(upload_date).split("T")[0]
```

### 进行第二次 API 调用

收集这些信息是很好的，但这还不够有趣。我们还希望收集每个视频的观看次数、点赞和点踩次数。这些不在第一次 API 调用中。我们需要做的是进行第二次 API 调用来收集这些信息，因为我们需要使用从第一次 API 调用中收集到的 video_id，然后进行第二次 API 调用以获取观看次数、点赞、点踩和评论数。

既然我们已经向你展示了如何进行 API 调用，我们建议你尝试自己完成第二次 API 调用。如果你成功完成了第二次 API 调用，它应该类似于下面的样子：

```py
url_video_stats = "https://www.googleapis.com/youtube/v3/videos?id="
                  +video_id+"&part=statistics&key="+API_KEY
response_video_stats = requests.get(url_video_stats).json()

view_count = response_video_stats['items'][0]['statistics']['viewCount']
like_count = response_video_stats['items'][0]['statistics']['likeCount']
dislike_count = response_video_stats['items'][0]['statistics']
                ['dislikeCount']
comment_count = response_video_stats['items'][0]['statistics']
                ['commentCount']
```

现在，我们需要将其添加到 'for loop' 中。我们会得到类似这样的结果：

```py
for video in response['items']:
   if video['id']['kind'] == "youtube#video":
       video_id = video['id']['videoId']
       video_title = video['snippet']['title']
       video_title = str(video_title).replace("&amp;","")
       upload_date = video['snippet']['publishedAt']
       upload_date = str(upload_date).split("T")[0]

       #colleccting view, like, dislike, comment counts
       url_video_stats = "https://www.googleapis.com/youtube/v3/videos?id="
                          +video_id+"&part=statistics&key="+API_KEY
       response_video_stats = requests.get(url_video_stats).json()

       view_count = response_video_stats['items'][0]['statistics']
                    ['viewCount']
       like_count = response_video_stats['items'][0]['statistics']
                    ['likeCount']
       dislike_count = response_video_stats['items'][0]['statistics']
                    ['dislikeCount']
       comment_count = response_video_stats['items'][0]['statistics']
                    ['commentCount']
```

### 保存到 Pandas DataFrame

现在让我们构建一个 Pandas DataFrame，以便保存所有这些信息。由于我们已经知道了所有要保存的信息，我们将创建一个空白的 Pandas DataFrame，并在 'for loop' 之前添加列标题。

```py
df = pd.DataFrame(columns=["video_id","video_title","upload_date",
"view_count","like_count","dislike_count","comment_count"])
```

接下来，我们将把在 'for loop' 中保存到变量的数据追加到这个 Pandas DataFrame 中。我们将在 'for loop' 内部使用 append() 方法来实现这一点。

```py
df = df.append({'video_id':video_id,'video_title':video_title,
                'upload_date':upload_date,'view_count':view_count,
                'like_count':like_count,'dislike_count':dislike_count,
                'comment_count':comment_count},ignore_index=True)
```

整个 'for loop' 看起来是这样的：

```py
for video in response['items']:
   if video['id']['kind'] == "youtube#video":
       video_id = video['id']['videoId']
       video_title = video['snippet']['title']
       video_title = str(video_title).replace("&amp;","")
       upload_date = video['snippet']['publishedAt']
       upload_date = str(upload_date).split("T")[0]

       #collecting view, like, dislike, comment counts
       url_video_stats = "https://www.googleapis.com/youtube/v3/videos?id="
                          +video_id+"&part=statistics&key="+API_KEY
       response_video_stats = requests.get(url_video_stats).json()

       view_count = response_video_stats['items'][0]['statistics']
                    ['viewCount']
       like_count = response_video_stats['items'][0]['statistics']
                    ['likeCount']
       dislike_count = response_video_stats['items'][0]['statistics']
                    ['dislikeCount']
       comment_count = response_video_stats['items'][0]['statistics']
                    ['commentCount']

       df = df.append({'video_id':video_id,'video_title':video_title,
                        'upload_date':upload_date,'view_count':view_count,
'like_count':like_count,'dislike_count':dislike_count,
                         'comment_count':comment_count},ignore_index=True)
```

<picture>![Python API for data science output of a pandas dataframe](../Images/f9b80b8229a1758b4ae90efb2a56d39c.png)</picture> 现在我们在这个 Pandas DataFrame 中拥有了所有数据，输出应该是一个包含所有视频统计信息的 Pandas DataFrame。

<picture>![Saving APIs to a Pandas DataFrame](../Images/aa980dab7b66f759a42ca8122dd6ba55.png)</picture> ### 创建函数

我们目前编写的代码运行得非常好，但仍有一些可以改进的地方。主要需要改进的是将抓取视频信息的 API 调用与主部分分开，因为第二次 API 调用的逻辑不需要与保存数据的逻辑混合。

因此，我们可以将第二次 API 调用分离到自己的函数中，只需传递我们从第一次 API 调用中收集到的 video_id。

```py
def get_video_details(video_id):
   url = "https://www.googleapis.com/youtube/v3/videos?id="+video_id+"&
          part=statistics&key="+API_KEY
   response = requests.get(url).json()

   return response['items'][0]['statistics']['viewCount'],\
          response['items'][0]['statistics']['likeCount'],\
          response['items'][0]['statistics']['dislikeCount'],\
          response['items'][0]['statistics']['commentCount']
```

然后我们可以将主要工作封装到一个函数中。

```py
def get_videos(df):
   pageToken = ""
   while 1:
       url = "https://www.googleapis.com/youtube/v3/search?key="+API_KEY+"
&channelId="+CHANNEL_ID+"&part=snippet,id&order=date&maxResults=10000&"
+pageToken

       response = requests.get(url).json()
       time.sleep(1) #give it a second before starting the for loop
       for video in response['items']:
           if video['id']['kind'] == "youtube#video":
               video_id = video['id']['videoId']
               video_title = video['snippet']['title']
               video_title = str(video_title).replace("&amp;","")
               upload_date = video['snippet']['publishedAt']
               upload_date = str(upload_date).split("T")[0]
               print(upload_date)
               view_count, like_count, dislike_count, comment_count = get_
               video_details(video_id)

               df = df.append({'Video_id':video_id,'Video_title':
                    video_title,
                               "Upload_date":upload_date,"View_count":
                                view_count,
                               "Like_count":like_count,"Dislike_count":
                                dislike_count,
                               "Comment_count":comment_count},ignore_index=
                                True)
       try:
           if response['nextPageToken'] != None: #if none, it means it 
           reached the last page and break out of it
               pageToken = "pageToken=" + response['nextPageToken']

       except:
           break

   return df
```

最后，我们可以以这种方式调用函数。

```py
df = pd.DataFrame(columns=["Video_id","Video_title","Upload_date",
"View_count","Like_count","Dislike_count","Comment_count"]) #build our 
dataframe

df = get_videos(df)

print(df)
```

让我们看看最终的输出，我们得到一个 Pandas DataFrame，其中包括 video_id、video_title、upload_date、view_count、like_count、dislike_count 和 comment_count。

<picture>![Youtube Python APIs for Data Science final output](../Images/0052acefeb78e9123f9c90ce0cc6fc02.png)</picture> ### **结论**

这就是你可以在数据科学项目中使用 Python API 的方法，从 API 中获取数据并将其保存到 Pandas DataFrame 中。作为数据科学家，你应该知道如何从 API 中抓取数据。让我们来分解一下我们所执行的步骤：

+   学会了使用 request 库进行 API 调用

+   向 YouTube API 发出了 API 调用：我们向 API 传递了一个 URL，指定了我们想要的数据。我们必须仔细阅读文档来构建 URL。

+   将数据收集为 JSON：我们将数据作为 JSON 对象收集，并首先将其解析为变量。

+   将数据保存为 Pandas DataFrame

+   最后，我们添加了一些错误处理逻辑并清理了代码

*找到一些令人兴奋的 [Python 数据科学家职位面试问题](https://www.stratascratch.com/blog/python-interview-questions-for-data-scientist-position/)，这些问题适合初学者或寻找更具挑战性的任务的人。*

**个人简介：Nathan Rosidi** (**[@StrataScratch](https://twitter.com/StrataScratch)**) 是一名数据科学家和产品策略专家。他还是一名兼职教授，教授分析课程，并且是 [StrataScratch](https://www.stratascratch.com/) 的创始人，该平台帮助数据科学家准备顶级公司的真实面试问题。

**相关内容：**

+   [生产就绪的机器学习 NLP API 使用 FastAPI 和 spaCy](/2021/04/production-ready-machine-learning-nlp-api-fastapi-spacy.html)

+   [使用 Flask 构建 RESTful API](/2021/05/building-restful-apis-flask.html)

+   [构建你的第一个数据科学应用程序](/2021/02/build-first-data-science-application.html)

### 更多相关话题

+   [在 Python 中使用 SQLite 数据库的指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)

+   [FastAPI 教程：几分钟内用 Python 构建 API](https://www.kdnuggets.com/fastapi-tutorial-build-apis-with-python-in-minutes)

+   [在实际应用中实现深度学习：以数据为中心的课程](https://www.kdnuggets.com/2022/04/corise-deep-learning-wild-data-centric-course.html)

+   [远程工作的数据科学家必备的 6 项软技能](https://www.kdnuggets.com/2022/05/6-soft-skills-data-scientists-working-remotely.html)

+   [处理大数据：工具和技术](https://www.kdnuggets.com/working-with-big-data-tools-and-techniques)

+   [在实际应用中实现深度学习：以数据为中心的课程](https://www.kdnuggets.com/2022/11/corise-deep-learning-wild-data-centric-course.html)
