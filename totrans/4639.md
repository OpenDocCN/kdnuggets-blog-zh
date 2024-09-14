# 如何使用 Python 的 datetime

> 原文：[https://www.kdnuggets.com/2019/06/how-use-datetime.html](https://www.kdnuggets.com/2019/06/how-use-datetime.html)

[评论](#comments)

有关视频讲解和幻灯片，以及其他数据处理教程，请访问 [数据处理技巧与窍门](https://end-to-end-machine-learning.teachable.com/p/data-munging-tips-and-tricks/) 课程页面。该课程免费提供。

### 在 Python 中处理日期和时间

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 方面的组织

* * *

[Python 的 datetime 包](https://docs.python.org/3/library/datetime.html) 是一个处理日期和时间的便捷工具集。通过我即将展示的五个技巧，你可以处理大多数的 datetime 处理需求。

在深入了解之前，了解 datetime 是如何组合在一起的会很有帮助。基本的构建块是一个 datetime 对象。不出所料，它是日期对象和时间对象的组合。日期对象仅是一组年、月、日的值，以及一组处理这些值的函数。时间对象的结构类似。它有小时、分钟、秒、微秒和时区的值。任何时间都可以通过适当地选择这些值来表示。

![](../Images/5350fa6b7fc2d13b7fbf4e4129e7bf56.png)

### 1. **combine()**

```py
import datetime

# (hours, minutes)
start_time = datetime.time(7, 0)
# (year, month, day)
start_date = datetime.date(2015, 5, 1)
# Create a datetime object
start_datetime = datetime.datetime.combine(
    start_date, start_time)
```

处理 datetimes 的第一个技巧是通过组合日期和时间对象来创建它们。我们首先创建一个时间，传递小时 7 和分钟 0。这表示 7 点。因为我们没有提供秒或微秒，这些默认为零。然后我们通过传递年、月和日来创建一个日期。

创建一个 datetime 非常简单。我们使用 **combine()** 函数，并传递我们想要构建 datetime 的日期对象和时间对象。

由于命名约定，调用 datetime 可能会让人困惑。Datetime 是包名、包内的模块以及对象的名称。因此，当我们组合日期和时间时，我们用看似冗余的 **datetime.datetime** 前缀来调用它。第一个 **datetime** 代表包，第二个 **datetime** 代表模块，**combine()** 是该模块中的一个函数。

### 2\. timedelta

```py
# Differences between datetimes are timedelta objects. 
timedelta_total = end_datetime - start_datetime
# timedeltas have days, seconds, and microseconds
# They can be used to increment dates and times,
# accounting for quirks of dates and timezones.
end_datetime = start_datetime + timedelta_total
```

使用日期时间的第二个技巧是称为timedelta的类型。这表示两个日期时间之间的差异。一个timedelta只有三个值：天、秒和微秒。两个日期时间之间的差异可以以这种方式唯一表示。

timedelta非常有用，因为它们允许我们对日期时间进行简单的加减法运算。它们免去了考虑一个月有多少天、一天有多少秒和闰年的需要。

### 3\. 时间戳

```py
# Number of seconds from 12:00 am, January 1, 1970, UTC
# is a computer-friendly way to handle time.
unix_epoch = timestamp(start_datetime)
start_datetime = fromtimestamp(1457453760)
```

获取日期时间最大效用的第三个技巧是使用时间戳。以天、小时、分钟和秒为单位对计算机来说很笨重。需要检查规则和边角情况。为了使日期和时间更易于处理，创建了[UNIX纪元](https://en.wikipedia.org/wiki/Unix_time)这一概念。这是自1970年1月1日12:00 AM以来经过的秒数，使用[协调世界时](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)（UTC +0时区）。这允许任何日期和时间被表示为一个单一的、常见可解释的浮点数。唯一的缺点是对人类读者而言并不直观。**timestamp()** 和 **fromtimestamp()** 函数允许将我们人类可解释的日期时间对象转换为UNIX纪元，以便于计算。

### 4\. weekday()

```py
# Gets the day of the week for a given date.
# Monday is 0, Sunday is 6
weekday_number = start_datetime.date().weekday()
```

我们包中的第四个技巧是**weekday()**函数。对于任何给定的日期，它计算星期几。要使用它，请在你的日期时间上调用**date()**函数。这将隔离日期对象，并忽略时间部分。然后调用它的**weekday()**函数。它返回一个从0到6的数字，其中0是星期一，1是星期二，以此类推，6是星期日。它处理所有跟踪星期几的特殊情况，使你无需操心。

### 5\. 日期字符串

```py
# Pass a date string and a code for interpreting it.
new_datetime = datetime.datetime.strptime(
    '2018-06-21', '%Y-%m-%d')
# Turn a datetime into a date string.
datestr = new_datetime.strftime('%Y-%m-%d')
print(datestr)

>> "2018-06-21"
```

最后，我们来到第五个技巧，即将日期转换为字符串或从字符串转换。这在我们从文本文件中获取数据，并想将文本日期转换为日期时间对象时特别有用。它在我们想向用户展示日期时间对象或将其导出到文本文件时也很有用。

为此，我们使用**strptime()**和**strftime()**函数。在进行任一方向的转换时，我们必须提供一个指定格式的字符串。在这个代码片段中，'%Y'表示年份，'%m'表示两位数的月份，'%d'表示两位数的日期。

另外，实际上表示年份、月份和日期的正确方式是：'YYYY-MM-DD'。（这是1988年制定的国际标准，[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)）。例如，2018年7月31日将表示为'2018-07-31'。我强烈建议你在有选择时以这种格式来格式化日期，以便于解释和兼容性。然而，要注意在实际使用中存在多种日期格式。准备好进行一些复杂的转换，以便将你获取的数据转换为这种格式。

现在你已经掌握了五个最有用的日期时间技巧。

1.  **combine()**

1.  timedelta，

1.  时间戳之间的转换，

1.  **weekday()**，以及

1.  字符串格式化。

有了这些工具，你已经完成了解决下一个Python项目中90%日期和时间挑战的工作。祝好运，希望它对你有所帮助。

[原始版本](https://brohrer.github.io/datetime_tricks.html)。经授权转载。

**相关：**

+   [Pandas数据框索引](/2019/04/pandas-dataframe-indexing.html)

+   [PyCharm为数据科学家量身定制](/2019/05/pycharm-data-scientists.html)

+   [如何（不）使用机器学习进行时间序列预测：避免陷阱](/2019/05/machine-learning-time-series-forecasting.html)

### 更多相关内容

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [如何使用Python和机器学习预测足球比赛胜者](https://www.kdnuggets.com/2023/01/python-machine-learning-predict-football-match-winners.html)

+   [学习如何使用ChatGPT学习Python（或其他任何东西）](https://www.kdnuggets.com/2023/02/learn-python-chatgpt.html)

+   [如何（不）使用Python的海象运算符](https://www.kdnuggets.com/how-not-to-use-pythons-walrus-operator)

+   [停止在数据科学项目中硬编码 - 使用配置文件代替](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

+   [这里是我使用的AI工具和技能来赚取$10,000的方式…](https://www.kdnuggets.com/2023/07/ai-tools-along-skills-make-10000-monthly-bs.html)
