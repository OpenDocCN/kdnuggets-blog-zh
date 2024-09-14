# Python 数据分析的 Instagram 使用指南

> 原文：[https://www.kdnuggets.com/2017/08/instagram-python-data-analysis.html/2](https://www.kdnuggets.com/2017/08/instagram-python-data-analysis.html/2)

**保存/加载数据到磁盘**

因为这个请求可能需要很长时间，我们不希望在不必要的时候运行它，所以保存结果并在继续工作时加载它是一个好习惯。为此，我们将使用 Pickle。Pickle 可以序列化任何变量，保存到文件，然后加载。以下是它的工作示例。

保存：

```py
import pickle
filename=username+"_posts"
pickle.dump(myposts,open(filename,"wb"))
```

加载：

```py
import pickle
filename="nourgalaby_posts"
myposts=pickle.load(file=open(filename))
```

**按点赞数量排序**

现在我们有一个名为‘myposts’的字典列表。由于我们想按字典中的某个键进行排序，我们可以这样使用 lambda 表达式：

```py
myposts_sorted = sorted(myposts, key=lambda k:
k['like_count'],reverse=True) 
top_posts=myposts_sorted[:10]
bottom_posts=myposts_sorted[-10:]
```

然后我们可以像上面一样展示它们：

```py
image_urls=get_images_from_list(top_posts)
display_images_from_url(image_urls)
```

**过滤照片**

我们可能希望对帖子列表应用一些过滤器。例如，如果帖子中有视频但我只想要图片，我可以这样过滤：

```py
myposts_photos= filter(lambda k: k['media_type']==1, myposts)
myposts_vids= filter(lambda k: k['media_type']==2, myposts)
print len(myposts)
print len(myposts_photos)
print len(myposts_vids)
```

当然，你可以对结果中的任何变量应用过滤器，所以发挥你的创造力吧 ;)

**通知**

```py
InstagramAPI.getRecentActivity()
get_recent_activity_response= InstagramAPI.LastJson 
for notifcation in get_recent_activity_response['old_stories']:
    print notifcation['args']['text']
```

你应该看到如下内容：

```py
userohamed3 liked your post.
userhacker32 liked your post.
user22 liked your post.
userz77 liked your post.
userwww77 started following you.
user2222 liked your post.
user23553 liked your post.
```

**仅来自一个用户的通知**

此时，我们可以随意操作和玩弄通知。例如，我可以仅获取特定用户的通知列表：

```py
username="diana"
for notifcation in get_recent_activity_response['old_stories']:
    text = notifcation['args']['text']
    if username  in text:
        print text
```

让我们尝试一些更有趣的事情：查看你什么时候获得最多的点赞，即一天中的哪个时间段人们最常点赞。为此，我们将绘制一天中的时间与收到的点赞数量之间的关系。

绘制通知的日期时间：

```py
import pandas as pd
df = pd.DataFrame({"date":dates})
df.groupby(df["date"].dt.hour).count().plot(kind="bar",title="Hour" )
```

![Instagram](../Images/573488697377868a9508a7d15e74bf57.png)

正如我们在我的案例中所见，我在下午6:00到10:00之间获得最多的点赞。如果你对社交媒体感兴趣，你会知道这是高峰使用时间，也是大多数公司选择发布以获得最多互动的时间。

**获取粉丝和关注者列表**

在这里我将获取粉丝和关注者列表，并对其进行一些操作。

为了使用两个函数`getUserFollowings`和`getUserFollowers`，你需要先获取`user_id`。你可以通过以下方式获取`user_id`：

![](../Images/349b256a812148b872d80d90100a012e.png)

现在你可以简单地按如下方式使用这些函数。请注意，如果粉丝数量很多，你需要进行多次请求（稍后会详细说明）。在这里，我们进行了一个请求以获取粉丝/关注者。JSON结果包含一个“用户”列表，其中包含每个粉丝/关注者的所有信息。

```py
InstagramAPI.getUserFollowings(user_id)
print len(InstagramAPI.LastJson['users'])
following_list=InstagramAPI.LastJson['users']

InstagramAPI.getUserFollowers(user_id)
print len(InstagramAPI.LastJson['users'])
followers_list=InstagramAPI.LastJson['users']
```

如果数量很大，这个结果可能不包含完整的数据集。

**获取所有粉丝**

获取粉丝列表类似于获取所有帖子。我们将进行一次请求，然后使用`next_max_id`键进行迭代：

**感谢Francesc Garcia的支持**

```py
import time

followers   = []
next_max_id = True
while next_max_id:
    print next_max_id
    #first iteration hack
    if next_max_id == True: next_max_id=''
    _ = InstagramAPI.getUserFollowers(user_id,maxid=next_max_id)
    followers.extend ( InstagramAPI.LastJson.get('users',[]))
    next_max_id = InstagramAPI.LastJson.get('next_max_id','')
    time.sleep(1) 

followers_list=followers
```

你应该对关注者做同样的操作，但在这种情况下，我不会这样做，因为一个请求就足以获取到我的所有关注者。

现在我们有了所有关注者和被关注者数据的JSON格式列表，我将它们转换为更友好的数据类型——一个集合——以便对其执行一些集合操作。

我将只取'username'并将其转换为一个`set()`。

```py
user_list = map(lambda x: x['username'] , following_list)
following_set= set(user_list)
print len(following_set)

user_list = map(lambda x: x['username'] , followers_list)
followers_set= set(user_list)
print len(followers_set)
```

在这里，我选择将每个用户的用户名集合在一起。'full_name'也可以用——而且更友好——但它不是唯一的，有些用户可能没有'full_name'的值。

现在我们有了两个集合，我们可以进行以下操作：

![Instagram](../Images/ef186da22d5aa8788a9670210c1c67d8.png)

这里我们有一些关于粉丝的统计数据。你可以从这一点做很多事情，例如保存粉丝列表，然后在稍后的时间比较，以获得取消关注者的列表。

这些是你可以用Instagram数据做的一些事情。我希望你了解到如何使用Instagram API，并对它的基本用途有了一个初步了解。请关注原文，因为它仍在开发中，未来将会有更多你可以做的事情。如有任何问题或建议，请随时联系我。

**简介: [Nour Galaby](https://www.linkedin.com/in/nourgalaby/)** 是一位对数据科学和机器学习充满热情的数据科学爱好者。

**相关内容：**

+   [6件你可以用Python处理Facebook数据的有趣事情](/2017/06/6-interesting-things-facebook-python.html)

+   [情感和抑郁的分析](/2017/04/analytics-emotion-depression.html)

+   [Python为何突然变得如此受欢迎的6个原因](/2017/07/6-reasons-python-suddenly-super-popular.html)

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业之路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

### 更多相关话题

+   [每个数据科学家都应该了解的三大R语言库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让Python成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)
