# 在 Python 中整理数据

> 原文：[https://www.kdnuggets.com/2017/01/tidying-data-python.html](https://www.kdnuggets.com/2017/01/tidying-data-python.html)

**由 Jean-Nicholas Hould, JeanNicholasHould.com 提供。**

我最近遇到了一篇名为[Tidy Data](http://vita.had.co.nz/papers/tidy-data.pdf)的论文，作者是 Hadley Wickham。该论文发表于2014年，重点讲述了数据清理的一个方面——整理数据：结构化数据集以便于分析。通过这篇论文，Wickham 展示了如何在分析之前将任何数据集结构化为标准化的方式。他详细介绍了不同类型的数据集以及如何将它们整理成标准格式。

作为数据科学家，我认为你应该非常熟悉这种数据集的标准化结构。数据清理是数据科学中最常见的任务之一。无论你处理何种数据或进行何种分析，你最终都必须清理数据。将数据整理成标准格式可以简化后续工作。你可以在不同的分析中重用一套标准工具。

在这篇文章中，我将总结一些 Wickham 在论文中使用的整理示例，并演示如何使用 Python 的[pandas](http://pandas.pydata.org/)库进行整理。

### 定义整洁数据

Wickham 定义的整洁结构具有以下属性：

+   每个*变量*形成一列，并包含*值*

+   每个*观察*形成一行

+   每种*观察单元*形成一个表格

一些定义：

+   变量：一种测量或属性。*身高, 体重, 性别, 等等*

+   值：实际的测量或属性。*152 cm, 80 kg, female, etc.*

+   观察：所有的值都是在相同的单元上测量的。*每个人。*

一个*混乱数据集*的例子：

|  | 处理 A | 处理 B |
| --- | --- | --- |
| 约翰·史密斯 | - | 2 |
| 简·多 | 16 | 11 |
| 玛丽·约翰逊 | 3 | 1 |

一个*整洁数据集*的例子：

| 姓名 | 处理 | 结果 |
| --- | --- | --- |
| 约翰·史密斯 | a | - |
| 简·多 | a | 16 |
| 玛丽·约翰逊 | a | 3 |
| 约翰·史密斯 | b | 2 |
| 简·多 | b | 11 |
| 玛丽·约翰逊 | b | 1 |

### 整理混乱的数据集

通过以下从 Wickham 的论文中提取的示例，我们将把混乱的数据集整理成整洁的格式。这里的目标不是分析数据集，而是将它们以标准化的方式准备好以便进行分析。这是我们将处理的五种类型的混乱数据集：

+   列标题是值，而不是变量名称。

+   多个变量被存储在一列中。

+   变量存储在行和列中。

+   多种类型的观察单元存储在同一个表格中。

+   单一的观察单元被存储在多个表格中。

*注意：本文中呈现的所有代码都可以在[Github](https://github.com/nickhould/tidy-data-python)上找到。*

### 列标题是值，而不是变量名称

**皮尤研究中心数据集**

这个数据集探讨了收入与宗教之间的关系。

问题：列标题由可能的收入值组成。

```py
import pandas as pd
import datetime
from os import listdir
from os.path import isfile, join
import glob
import re

df = pd.read_csv("./data/pew-raw.csv")
df
```

| 宗教 | <$10k | $10-20k | $20-30k | $30-40k | $40-50k | $50-75k |
| --- | --- | --- | --- | --- | --- | --- |
| 无神论者 | 27 | 34 | 60 | 81 | 76 | 137 |
| 无神论者 | 12 | 27 | 37 | 52 | 35 | 70 |
| 佛教徒 | 27 | 21 | 30 | 34 | 33 | 58 |
| 天主教 | 418 | 617 | 732 | 670 | 638 | 1116 |
| 不知道/拒绝 | 15 | 14 | 15 | 11 | 10 | 35 |
| 福音派新教 | 575 | 869 | 1064 | 982 | 881 | 1486 |
| 印度教 | 1 | 9 | 7 | 9 | 11 | 34 |
| 历史性黑人新教 | 228 | 244 | 236 | 238 | 197 | 223 |
| 耶和华见证人 | 20 | 27 | 24 | 24 | 21 | 30 |
| 犹太教 | 19 | 19 | 25 | 25 | 30 | 95 |

数据集的整洁版本是收入值不会成为列标题，而是作为`income`列中的值。为了整理这个数据集，我们需要[转化](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.melt.html)它。*pandas*库有一个内置函数可以做到这一点，它将数据框从宽格式“反转”成长格式。我们将在本文中多次重用这个函数。

```py
formatted_df = pd.melt(df,
                       ["religion"],
                       var_name="income",
                       value_name="freq")
formatted_df = formatted_df.sort_values(by=["religion"])
formatted_df.head(10)
```

这将输出数据集的整洁版本：

| 宗教 | 收入 | 频率 |
| --- | --- | --- |
| 无神论者 | <$10k | 27 |
| 无神论者 | $30-40k | 81 |
| 无神论者 | $40-50k | 76 |
| 无神论者 | $50-75k | 137 |
| 无神论者 | $10-20k | 34 |
| 无神论者 | $20-30k | 60 |
| 无神论者 | $40-50k | 35 |
| 无神论者 | $20-30k | 37 |
| 无神论者 | $10-20k | 27 |
| 无神论者 | $30-40k | 52 |

**公告牌前100名数据集**

这个数据集表示从歌曲进入《公告牌》前100名时开始到接下来的75周的每周排名。

问题：

+   列标题由值组成：周数（`x1st.week`，…）

+   如果一首歌在前100名中停留少于75周，剩余的列将填充缺失值。

```py
df = pd.read_csv("./data/billboard.csv", encoding="mac_latin2")
df.head(10)
```

| 年份 | 艺术家.倒序 | 曲目 | 时长 | 风格 | 进入日期 | 高峰日期 | 第一周 | 第二周 | ... |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2000 | 命运之子 | 独立女性第一部分 | 3:38 | 摇滚 | 2000-09-23 | 2000-11-18 | 78 | 63.0 | ... |
| 2000 | 桑坦娜 | 玛利亚，玛利亚 | 4:18 | 摇滚 | 2000-02-12 | 2000-04-08 | 15 | 8.0 | ... |
| 2000 | 野人花园 | 我知道我爱你 | 4:07 | 摇滚 | 1999-10-23 | 2000-01-29 | 71 | 48.0 | ... |
| 2000 | 麦当娜 | 音乐 | 3:45 | 摇滚 | 2000-08-12 | 2000-09-16 | 41 | 23.0 | ... |
| 2000 | 克里斯蒂娜·阿奎莱拉 | 来吧宝贝（我只要你） | 3:38 | 摇滚 | 2000-08-05 | 2000-10-14 | 57 | 47.0 | ... |
| 2000 | 贾妮特 | 无关紧要 | 4:17 | 摇滚 | 2000-06-17 | 2000-08-26 | 59 | 52.0 | ... |
| 2000 | 命运之子 | 说我的名字 | 4:31 | 摇滚 | 1999-12-25 | 2000-03-18 | 83 | 83.0 | ... |
| 2000 | 恩里克·伊格莱西亚斯 | 和你在一起 | 3:36 | 拉丁 | 2000-04-01 | 2000-06-24 | 63 | 45.0 | ... |
| 2000 | 西斯科 | 不完整 | 3:52 | 摇滚 | 2000-06-24 | 2000-08-12 | 77 | 66.0 | ... |
| 2000 | Lonestar | Amazed | 4:25 | Country | 1999-06-05 | 2000-03-04 | 81 | 54.0 | ... |

这个数据集的整洁版本是没有周数作为列的，而是作为单一列的值。为了做到这一点，我们将把周数列“*融化*”成一个`date`列。我们将为每条记录创建一行每周。如果给定周没有数据，我们将不会创建行。

```py
# Melting
id_vars = ["year",
           "artist.inverted",
           "track",
           "time",
           "genre",
           "date.entered",
           "date.peaked"]

df = pd.melt(frame=df,id_vars=id_vars, var_name="week", value_name="rank")

# Formatting 
df["week"] = df['week'].str.extract('(\d+)', expand=False).astype(int)
df["rank"] = df["rank"].astype(int)

# Cleaning out unnecessary rows
df = df.dropna()

# Create "date" columns
df['date'] = pd.to_datetime(df['date.entered']) + pd.to_timedelta(df['week'], unit='w') - pd.DateOffset(weeks=1)

df = df[["year", 
         "artist.inverted",
         "track",
         "time",
         "genre",
         "week",
         "rank",
         "date"]]
df = df.sort_values(ascending=True, by=["year","artist.inverted","track","week","rank"])

# Assigning the tidy dataset to a variable for future usage
billboard = df

df.head(10)
```

数据集的更整洁版本如下所示。仍然有很多歌曲详细信息的重复：曲目名称、时间和类型。由于这个原因，这个数据集仍然不完全符合Wickham的定义。我们将在下一个示例中解决这个问题。

| year | artist.inverted | track | time | genre | week | rank | date |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 1 | 87 | 2000-02-26 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 2 | 82 | 2000-03-04 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 3 | 72 | 2000-03-11 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 4 | 77 | 2000-03-18 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 5 | 87 | 2000-03-25 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 6 | 94 | 2000-04-01 |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 7 | 99 | 2000-04-08 |
| 2000 | 2Ge+her | The Hardest Part Of Breaking Up (Is Getting Ba... | 3:15 | R&B | 1 | 91 | 2000-09-02 |
| 2000 | 2Ge+her | The Hardest Part Of Breaking Up (Is Getting Ba... | 3:15 | R&B | 2 | 87 | 2000-09-09 |
| 2000 | 2Ge+her | The Hardest Part Of Breaking Up (Is Getting Ba... | 3:15 | R&B | 3 | 92 | 2000-09-16 |

### 一个表中包含多种类型

继Billboard数据集之后，我们现在将解决前一个表的重复问题。

问题：

+   多个观察单位（`song`和它的`rank`）在一个表中。

我们将首先创建一个包含每首歌详细信息的`songs`表：

```py
songs_cols = ["year", "artist.inverted", "track", "time", "genre"]
songs = billboard[songs_cols].drop_duplicates()
songs = songs.reset_index(drop=True)
songs["song_id"] = songs.index
songs.head(10)
```

| year | artist.inverted | track | time | genre | song_id |
| --- | --- | --- | --- | --- | --- |
| 2000 | 2 Pac | Baby Don't Cry (Keep Ya Head Up II) | 4:22 | Rap | 0 |
| 2000 | 2Ge+her | The Hardest Part Of Breaking Up (Is Getting Ba... | 3:15 | R&B | 1 |
| 2000 | 3 Doors Down | Kryptonite | 3:53 | Rock | 2 |
| 2000 | 3 Doors Down | Loser | 4:24 | Rock | 3 |
| 2000 | 504 Boyz | Wobble Wobble | 3:35 | Rap | 4 |
| 2000 | 98° | Give Me Just One Night (Una Noche) | 3:24 | Rock | 5 |
| 2000 | A*Teens | Dancing Queen | 3:44 | Pop | 6 |
| 2000 | Aaliyah | I Don't Wanna | 4:15 | Rock | 7 |
| 2000 | Aaliyah | Try Again | 4:03 | Rock | 8 |
| 2000 | Adams, Yolanda | Open My Heart | 5:30 | Gospel | 9 |

然后我们将创建一个只包含`song_id`、`date`和`rank`的`ranks`表。

```py
ranks = pd.merge(billboard, songs, on=["year","artist.inverted", "track", "time", "genre"])
ranks = ranks[["song_id", "date","rank"]]
ranks.head(10)
```

| song_id | date | rank |
| --- | --- | --- |
| 0 | 2000-02-26 | 87 |
| 0 | 2000-03-04 | 82 |
| 0 | 2000-03-11 | 72 |
| 0 | 2000-03-18 | 77 |
| 0 | 2000-03-25 | 87 |
| 0 | 2000-04-01 | 94 |
| 0 | 2000-04-08 | 99 |
| 1 | 2000-09-02 | 91 |
| 1 | 2000-09-09 | 87 |
| 1 | 2000-09-16 | 92 |

### 一列中存储多个变量

**世界卫生组织的结核病记录**

这个数据集记录了按国家、年份、年龄和性别确认的结核病病例数量。

问题：

+   一些列包含多个值：性别和年龄。

+   零值和缺失值`NaN`的混合。这是由于数据收集过程中的原因，区分这些值对于这个数据集很重要。

```py
df = pd.read_csv("./data/tb-raw.csv")
df
```

| country | year | m014 | m1524 | m2534 | m3544 | m4554 | m5564 | m65 | mu | f014 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AD | 2000 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | NaN | NaN |
| AE | 2000 | 2 | 4 | 4 | 6 | 5 | 12 | 10 | NaN | 3 |
| AF | 2000 | 52 | 228 | 183 | 149 | 129 | 94 | 80 | NaN | 93 |
| AG | 2000 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | NaN | 1 |
| AL | 2000 | 2 | 19 | 21 | 14 | 24 | 19 | 16 | NaN | 3 |
| AM | 2000 | 2 | 152 | 130 | 131 | 63 | 26 | 21 | NaN | 1 |
| AN | 2000 | 0 | 0 | 1 | 2 | 0 | 0 | 0 | NaN | 0 |
| AO | 2000 | 186 | 999 | 1003 | 912 | 482 | 312 | 194 | NaN | 247 |
| AR | 2000 | 97 | 278 | 594 | 402 | 419 | 368 | 330 | NaN | 121 |
| AS | 2000 | NaN | NaN | NaN | NaN | 1 | 1 | NaN | NaN | NaN |

为了整理这个数据集，我们需要从标题中删除不同的值，并将其展开成行。我们首先需要将`sex + age group`列融化成一个单一列。一旦我们有了那个单一列，我们将从中派生出三列：`sex`、`age_lower`和`age_upper`。有了这些，我们将能够正确地构建一个整洁的数据集。

```py
df = pd.melt(df, id_vars=["country","year"], value_name="cases", var_name="sex_and_age")

# Extract Sex, Age lower bound and Age upper bound group
tmp_df = df["sex_and_age"].str.extract("(\D)(\d+)(\d{2})")    

# Name columns
tmp_df.columns = ["sex", "age_lower", "age_upper"]

# Create `age`column based on `age_lower` and `age_upper`
tmp_df["age"] = tmp_df["age_lower"] + "-" + tmp_df["age_upper"]

# Merge 
df = pd.concat([df, tmp_df], axis=1)

# Drop unnecessary columns and rows
df = df.drop(['sex_and_age',"age_lower","age_upper"], axis=1)
df = df.dropna()
df = df.sort(ascending=True,columns=["country", "year", "sex", "age"])
df.head(10)
```

这导致了一个*整洁的数据集*。

| country | year | cases | sex | age |
| --- | --- | --- | --- | --- |
| AD | 2000 | 0 | m | 0-14 |
| AD | 2000 | 0 | m | 15-24 |
| AD | 2000 | 1 | m | 25-34 |
| AD | 2000 | 0 | m | 35-44 |
| AD | 2000 | 0 | m | 45-54 |
| AD | 2000 | 0 | m | 55-64 |
| AE | 2000 | 3 | f | 0-14 |
| AE | 2000 | 2 | m | 0-14 |
| AE | 2000 | 4 | m | 15-24 |
| AE | 2000 | 4 | m | 25-34 |

### 变量既存储在行中，也存储在列中

**全球历史气候网络数据集**

这个数据集记录了2010年墨西哥一个气象站（MX17004）五个月的每日天气记录。

问题：

+   变量既存储在行（`tmin`、`tmax`）中，也存储在列（`days`）中。

```py
df = pd.read_csv("./data/weather-raw.csv")
df
```

| id | year | month | element | d1 | d2 | d3 | d4 | d5 | d6 | d7 | d8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MX17004 | 2010 | 1 | tmax | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 1 | tmin | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 2 | tmax | NaN | 27.3 | 24.1 | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 2 | tmin | NaN | 14.4 | 14.4 | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 3 | tmax | NaN | NaN | NaN | NaN | 32.1 | NaN | NaN | NaN |
| MX17004 | 2010 | 3 | tmin | NaN | NaN | NaN | NaN | 14.2 | NaN | NaN | NaN |
| MX17004 | 2010 | 4 | tmax | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 4 | tmin | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 5 | tmax | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |
| MX17004 | 2010 | 5 | tmin | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |

为了使数据集整洁，我们希望将三个错误放置的变量（`tmin`、`tmax`和`days`）作为三个独立的列：`tmin`、`tmax`和`date`。

```py
# Extracting day
df["day"] = df["day_raw"].str.extract("d(\d+)", expand=False)  
df["id"] = "MX17004"

# To numeric values
df[["year","month","day"]] = df[["year","month","day"]].apply(lambda x: pd.to_numeric(x, errors='ignore'))

# Creating a date from the different columns
def create_date_from_year_month_day(row):
    return datetime.datetime(year=row["year"], month=int(row["month"]), day=row["day"])

df["date"] = df.apply(lambda row: create_date_from_year_month_day(row), axis=1)
df = df.drop(['year',"month","day", "day_raw"], axis=1)
df = df.dropna()

# Unmelting column "element"
df = df.pivot_table(index=["id","date"], columns="element", values="value")
df.reset_index(drop=False, inplace=True)
df
```

| id | date | tmax | tmin |
| --- | --- | --- | --- |
| MX17004 | 2010-02-02 | 27.3 | 14.4 |
| MX17004 | 2010-02-03 | 24.1 | 14.4 |
| MX17004 | 2010-03-05 | 32.1 | 14.2 |

### 一个类型在多个表中

数据集：2014/2015 年伊利诺伊州男性婴儿名字。

问题：

+   数据分布在多个表/文件中。

+   “Year” 变量存在于文件名中。

为了将这些不同的文件加载到一个 DataFrame 中，我们可以运行一个自定义脚本，将文件合并在一起。此外，我们还需要从文件名中提取“Year”变量。

```py
def extract_year(string):
    match = re.match(".+(\d{4})", string) 
    if match != None: return match.group(1)

path = './data'
allFiles = glob.glob(path + "/201*-baby-names-illinois.csv")
frame = pd.DataFrame()
df_list= []
for file_ in allFiles:
    df = pd.read_csv(file_,index_col=None, header=0)
    df.columns = map(str.lower, df.columns)
    df["year"] = extract_year(file_)
    df_list.append(df)

df = pd.concat(df_list)
df.head(5)
```

| rank | name | frequency | sex | year |
| --- | --- | --- | --- | --- |
| 1 | Noah | 837 | Male | 2014 |
| 2 | Alexander | 747 | Male | 2014 |
| 3 | William | 687 | Male | 2014 |
| 4 | Michael | 680 | Male | 2014 |
| 5 | Liam | 670 | Male | 2014 |

### 终极想法

在这篇文章中，我专注于 Wickham 论文中的一个方面，即数据处理部分。我的主要目标是演示如何在 Python 中进行数据操作。值得提到的是，他的论文中有一个重要部分涵盖了工具和可视化，您可以通过整理数据集受益。我在这篇文章中没有涵盖这些内容。

总体而言，我很喜欢准备这篇文章并将数据集整理成流畅的格式。定义的格式使得查询和筛选数据变得更加容易。这种方法使得在分析中重用库和代码变得更加简便。它也使得与其他数据分析师共享数据集变得更简单。

**简历: Jean-Nicholas Hould** 是来自 [加拿大蒙特利尔的数据科学家](http://jeannicholashould.com/?utm_source=kdnugget)。JeanNicholasHould.com 的作者。

[原文](http://www.jeannicholashould.com/tidy-data-in-python.html)。经许可转载。

**相关:**

+   [数据科学统计学 101](/2016/07/data-science-statistics-101.html)

+   [数据科学中的中心极限定理](/2016/08/central-limit-theorem-data-science.html)

+   [用 SQL 做统计](/2016/08/doing-statistics-sql.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 了解更多相关主题

+   [通过《数据科学的快速 Python》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化 Python 代码性能：深入了解 Python 性能分析工具](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python 枚举：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [掌握 SQL、Python、数据清洗、数据处理与探索性数据分析的指南集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据科学、数据可视化及其他的 38 个顶级 Python 库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [KDnuggets 新闻 22:n16, 4月 20日：学习的顶级 YouTube 频道](https://www.kdnuggets.com/2022/n16.html)
