# Python 中的日期处理和特征工程

> 原文：[`www.kdnuggets.com/2021/07/date-pre-processing-feature-engineering-python.html`](https://www.kdnuggets.com/2021/07/date-pre-processing-feature-engineering-python.html)

![图](img/02425d80b59385202558ada22d460b29.png)

由[Sonja Langford](https://unsplash.com/@sonjalangford?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

也许像我一样，你在用 Python 处理数据时经常处理日期。也许像我一样，你在处理 Python 中的日期时感到沮丧，并发现你为了重复做相同的事情而过于频繁地查阅文档。

* * *

## 我们的前 3 门课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

就像任何编程的人一样，当发现自己重复做同样的事情时，我想通过自动化一些常见的日期处理任务以及一些简单而频繁的特征工程来简化生活，从而使得对给定日期的常见日期解析和处理任务可以通过一次函数调用完成。然后我可以在之后选择我感兴趣的特征进行提取。

这个日期处理是通过一个单一的 Python 函数实现的，该函数仅接受一个格式为 '`YYYY-MM-DD`' 的日期字符串（因为这是日期的格式），并返回一个包含（当前）18 对键/值特征的字典。其中一些键是非常直接的（例如解析出的四位数年份），而其他的是经过工程处理的（例如日期是否为公共假期）。如果你觉得这段代码有用，你应该能够弄清楚如何修改或扩展它以适应你的需求。有关你可能想要编写的其他日期/时间相关特征的一些想法，[请查看这篇文章](https://www.kdnuggets.com/2021/04/feature-engineering-datetime-variables-data-science-machine-learning.html)。

大部分功能是通过 Python [`datetime`](https://docs.python.org/3/library/datetime.html) 模块实现的，许多功能依赖于 [`strftime()`](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior) 方法。然而，真正的好处是有一种标准的、自动化的方法来处理相同的重复查询。

唯一使用的非标准库是 [`holidays`](https://pypi.org/project/holidays/)，它是一个“快速、高效的 Python 库，用于即时生成特定国家、省份和州的假期集合。”虽然该库可以处理各种国家和次国家的假期，但我在这个例子中使用了美国的全国假期。只需快速浏览项目文档和以下代码，你就能很容易确定如何在需要时进行更改。

那么，让我们首先查看 `process_date()` 函数。评论应该能提供有关发生了什么的见解，如果你需要的话。

```py
import datetime, re, sys, holidays

def process_date(input_str: str) -> {}:
    """Processes and engineers simple features for date strings

    Parameters:
      input_str (str): Date string of format '2021-07-14'

    Returns:
      dict: Dictionary of processed date features
    """

    # Validate date string input
    regex = re.compile(r'\d{4}-\d{2}-\d{2}')
    if not re.match(regex, input_str):
        print("Invalid date format")
        sys.exit(1)

    # Process date features
    my_date = datetime.datetime.strptime(input_str, '%Y-%m-%d').date()
    now = datetime.datetime.now().date()
    date_feats = {}

    date_feats['date'] = input_str
    date_feats['year'] = my_date.strftime('%Y')
    date_feats['year_s'] = my_date.strftime('%y')
    date_feats['month_num'] = my_date.strftime('%m')
    date_feats['month_text_l'] = my_date.strftime('%B')
    date_feats['month_text_s'] = my_date.strftime('%b')
    date_feats['dom'] = my_date.strftime('%d')
    date_feats['doy'] = my_date.strftime('%j')
    date_feats['woy'] = my_date.strftime('%W')

    # Fixing day of week to start on Mon (1), end on Sun (7)
    dow = my_date.strftime('%w')
    if dow == '0': dow = 7
    date_feats['dow_num'] = dow

    if dow == '1':
        date_feats['dow_text_l'] = 'Monday'
        date_feats['dow_text_s'] = 'Mon'
    if dow == '2':
        date_feats['dow_text_l'] = 'Tuesday'
        date_feats['dow_text_s'] = 'Tue'
    if dow == '3':
        date_feats['dow_text_l'] = 'Wednesday'
        date_feats['dow_text_s'] = 'Wed'
    if dow == '4':
        date_feats['dow_text_l'] = 'Thursday'
        date_feats['dow_text_s'] = 'Thu'
    if dow == '5':
        date_feats['dow_text_l'] = 'Friday'
        date_feats['dow_text_s'] = 'Fri'
    if dow == '6':
        date_feats['dow_text_l'] = 'Saturday'
        date_feats['dow_text_s'] = 'Sat'
    if dow == '7':
        date_feats['dow_text_l'] = 'Sunday'
        date_feats['dow_text_s'] = 'Sun'

    if int(dow) > 5:
        date_feats['is_weekday'] = False
        date_feats['is_weekend'] = True
    else:
        date_feats['is_weekday'] = True
        date_feats['is_weekend'] = False

    # Check date in relation to holidays
    us_holidays = holidays.UnitedStates()
    date_feats['is_holiday'] = input_str in us_holidays
    date_feats['is_day_before_holiday'] = my_date + datetime.timedelta(days=1) in us_holidays
    date_feats['is_day_after_holiday'] = my_date - datetime.timedelta(days=1) in us_holidays

    # Days from today
    date_feats['days_from_today'] = (my_date - now).days

    return date_feats
```

有几点需要注意：

+   默认情况下，Python 将一周的天数视为从星期天（0）开始，到星期六（6）结束；对我而言，我的处理方式是一周从星期一开始，到星期天结束——我不需要第 0 天（与将一周从第 1 天开始相对）——所以这需要进行更改。

+   创建一个工作日/周末特征很简单。

+   使用 `holidays` 库创建假期相关特征很简单，并且可以执行简单的日期加减运算；同样，替换其他国家或次国家假期（或添加到现有假期中）也是很容易做到的。

+   一个 `days_from_today` 功能是通过一两行简单的日期数学运算创建的；负数表示给定日期在今天*之前*的天数，而正数则表示从今天*到*给定日期的天数。

我个人不需要，例如，`is_end_of_month` 特征，但你应该能看到如何在此时相对轻松地将其添加到上述代码中。自己尝试一些自定义设置吧。

现在让我们测试一下。我们将处理一个日期，并打印返回的内容，即键值特征对的完整字典。

```py
import pprint
my_date = process_date('2021-07-20')
pprint.pprint(my_date)
```

```py
{'date': '2021-07-20',
 'days_from_today': 6,
 'dom': '20',
 'dow_num': '2',
 'dow_text_l': 'Tuesday',
 'dow_text_s': 'Tue',
 'doy': '201',
 'is_day_after_holiday': False,
 'is_day_before_holiday': False,
 'is_holiday': False,
 'is_weekday': True,
 'is_weekend': False,
 'month_num': '07',
 'month_text_l': 'July',
 'month_text_s': 'Jul',
 'woy': '29',
 'year': '2021',
 'year_s': '21'}
```

在这里你可以看到特征键的完整列表以及相应的值。通常情况下，我不需要打印整个字典，而是获取特定键或键集合的值。

我们可以通过下面的代码演示这如何实际工作。我们将创建一个日期列表，然后逐个处理这些日期，最终创建一个 Pandas 数据框，其中包含处理过的日期特征的选择，并将其打印到屏幕上。

```py
import pandas as pd

dates = ['2021-01-01', '2020-04-04', '1993-05-11', '2002-07-19', '2024-11-03', '2050-12-25']
df = pd.DataFrame()

for d in dates:
    my_date = process_date(d)
    features = [my_date['date'],
                my_date['year'],
                my_date['month_num'],
                my_date['month_text_s'],
                my_date['dom'],
                my_date['doy'],
                my_date['woy'],
                my_date['is_weekend'],
                my_date['is_holiday'],
                my_date['days_from_today']]
    ds = pd.Series(features)
    df = df.append(ds, ignore_index=True)

df.rename(columns={0: 'date',
                   1: 'year',
                   2: 'month_num',
                   3: 'month',
                   4: 'day_of_month',
                   5: 'day_of_year',
                   6: 'week_of_year',
                   7: 'is_weekend',
                   8: 'is_holiday',
                   9: 'days_from_today'}, inplace=True)

df.set_index('date', inplace=True)
print(df)
```

```py
            year month_num month day_of_month day_of_year week_of_year is_weekend  is_holiday  days_from_today
date                                                                                                          
2021-01-01  2021        01   Jan           01         001          00         0.0         1.0           -194.0
2020-04-04  2020        04   Apr           04         095          13         1.0         0.0           -466.0
1993-05-11  1993        05   May           11         131          19         0.0         0.0         -10291.0
2002-07-19  2002        07   Jul           19         200          28         0.0         0.0          -6935.0
2024-11-03  2024        11   Nov           03         308          44         1.0         0.0           1208.0
2050-12-25  2050        12   Dec           25         359          51         1.0         1.0          10756.0

```

希望这个数据框能让你更好地了解这种功能在实际应用中的有用性。

祝好运，数据处理愉快。

**相关**：

+   5 个 Python 数据处理技巧与代码片段

+   数据科学与机器学习中的日期时间变量特征工程

+   使用 SQL 处理时间序列

评论

### 更多相关话题

+   [特征存储峰会 2022：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [滴答：使用 Pendulum 实现 Python 中的轻松日期和时间管理](https://www.kdnuggets.com/tick-tock-using-pendulum-for-easy-date-and-time-management-in-python)

+   [如何在 Python 中工程化日期特征](https://www.kdnuggets.com/2021/08/engineer-date-features-python.html)

+   [使用 NumPy 进行日期和时间计算](https://www.kdnuggets.com/using-numpy-to-perform-date-and-time-calculations)

+   [构建可处理的、多变量特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [利用 RAPIDS cuDF 在特征工程中发挥 GPU 的作用](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)
