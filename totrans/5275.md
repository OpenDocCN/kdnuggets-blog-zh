# 使用 SQL 进行统计分析

> 原文：[https://www.kdnuggets.com/2016/08/doing-statistics-sql.html](https://www.kdnuggets.com/2016/08/doing-statistics-sql.html)

**作者：Jean-Nicholas Hould，JeanNicholasHould.com**

我最近写了一篇关于为什么你应该 [学习 SQL 进行数据分析](http://www.jeannicholashould.com/sql-for-data-analysis.html) 的文章。我收到了很多反馈，人们希望看到一些使用 SQL 进行数据分析的具体例子。我决定应用一些我自己的建议，加载其中一个 [超赞的数据集](https://github.com/caesar0301/awesome-public-datasets) 进行一些基本的数据探索查询。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

![SQL](../Images/7b90faf67d8c5a9b93c5838add6004a0.png)

对于这篇文章，我使用了一个开放数据集。百万歌曲数据集是一个*“自由获取的现代流行音乐曲目音频特征和元数据的集合”*。

如果你想跟随，请按照以下步骤操作：

1.  下载百万歌曲数据集。[我使用了 10K 歌曲子集](http://labrosa.ee.columbia.edu/millionsong/pages/getting-dataset#subset)。

1.  下载一个 [SQLite 客户端](http://sqlitebrowser.org/)

1.  使用 SQLite 打开 `subset_track_metadata.db` 文件并开始探索

### 为什么使用描述性统计？

当我开始分析新的数据集时，我会运行一些基本查询以了解数据的组织方式和数值的分布情况。我试图理解我所处理的内容。对于数值数据，我通常会运行一些描述性统计：我会测量中心趋势（**均值**、**中位数**、**众数**）并测量数据的变异水平。这些测量通常是数据探索的良好起点，常常会引发新的问题，推动我的分析。

### 中心趋势

**均值**

均值是将一组数字相加后除以该组总数得到的数字。均值对异常值非常敏感。它可能会被远高于或低于其余数据集的值严重影响。

```py
SELECT CAST(AVG(songs.year) as int) as avg_year FROM songs

-- | avg_year |
-- |----------|
-- | 934      |
```

+   `CAST`：*在兼容数据类型之间进行运行时数据类型转换。在这种情况下，我将浮点数转换为整数以进行四舍五入。*

+   `AVG`：*聚合函数，返回输入表达式值的均值。*

+   `as avg_year`：*临时重命名列标题 - 仅用于提高可读性，使代码更具人性化。我将在整个帖子中使用这种别名。*

**中位数**

中位数是将有序数据集的上半部分与下半部分分开的数字。中位数有时是衡量中点的更好指标，因为每个数据点的权重相等。

```py
SELECT songs.year as median_year
FROM songs 
ORDER BY songs.year 
LIMIT 1 
OFFSET (SELECT COUNT(*) FROM songs) / 2

-- | median_year |
-- |-------------|
-- | 0           |
```

+   `ORDER BY`：*按一列或多列对结果数据集进行排序。排序可以是升序`ASC`（默认）或降序`DESC`。*

+   `COUNT`：*聚合函数，返回符合条件的行数。*

+   `LIMIT`：*指定从结果数据集中返回的最大行数。*

+   `OFFSET`：*在开始返回行之前跳过X行。在这个特定示例中，我们跳过了5000行（相当于总行数`COUNT(*)`除以2）。*

**众数**

众数是数据集中出现频率最高的值。

```py
SELECT 
    songs.year,
    COUNT(*) as count
FROM songs
GROUP BY songs.year
ORDER BY COUNT(*) DESC
LIMIT 1

-- | year | count |
-- |------|-------|
-- | 0    | 5320  |
```

+   `GROUP BY`：*与聚合函数如`COUNT`、`AVG`等一起使用。按一列或多列对结果数据集进行分组。*

### 变异性

**最小/最大值**

数据集中的最小/最大值。

```py
SELECT 
    MIN(songs.year) as min_year,
    MAX(songs.year) as max_year
FROM
    songs

-- | min_year | max_year |
-- |----------|----------|
-- | 0        | 2010     |
```

+   `MIN`：*聚合函数，返回数据集中最小的值。*

+   `MAX`：*聚合函数，返回数据集中最大的值。*

**每年的歌曲分布**

每年发行歌曲的数量

```py
SELECT 
    songs.year,
    COUNT(*) songs_count
FROM songs
GROUP BY songs.year
ORDER BY songs.year ASC

-- | year | song_count |
-- |------|------------|
-- | 0    | 5320       |
-- | 1926 | 2          |
-- | 1927 | 3          |
-- | ...  | ...        |
-- | 2009 | 250        |
-- | 2010 | 64         |
```

### 下一步

这篇文章中的SQL查询相对简单。它们不是技术性技巧的结果。它们只是帮助我们理解数据集的简单度量，这正是它们的优点。

在这个特定的数据集中，我们注意到超过一半的数据集中，歌曲的年份为`0`。这意味着我们要么在查看非常旧的歌曲数据集，要么在处理缺失值。后者更为现实。如果我们过滤掉年份为`0`的歌曲，我们的数据就更有意义了。歌曲的年份范围从`1926`到`2010`，中位数是`2001`年。

数据部分清理后，我们可以开始探索数据集中的其他列，并提出更多问题：每年有多少独特的艺术家创作歌曲？这种情况随时间的变化如何？现在的歌曲是否比以前短？这些简单的度量指标的优点在于，它们可以在查询和过滤器变得越来越复杂时重复使用。通过应用简单的描述性统计度量，我们可以很好地掌握数据，并不断深入探索。

**简历：Jean-Nicholas Hould** 是来自 [加拿大蒙特利尔的数据科学家](http://jeannicholashould.com/?utm_source=kdnugget)。作者，见 JeanNicholasHould.com。

[原文](http://www.jeannicholashould.com/descriptive-statistics-in-sql.html)。经许可转载。

**相关：**

+   [掌握数据科学SQL的7个步骤](/2016/06/seven-steps-mastering-sql-data-science.html)

+   [数据科学统计学101](/2016/07/data-science-statistics-101.html)

+   [构建与购买 – 分析仪表盘](/2016/07/build-buy-analytics-dashboards.html)

### 更多相关主题

+   [停止在 ChatGPT 上做这些事情，超越 99% 的用户](https://www.kdnuggets.com/2023/05/stop-chatgpt-get-ahead-99-users.html)

+   [数据科学基础：你需要掌握的 10 个必备技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [ChatGPT 是如何工作的，为什么它有效？](https://www.kdnuggets.com/2023/04/chatgpt-work.html)

+   [Phi-2：正在做大事的小型语言模型](https://www.kdnuggets.com/phi-2-small-lms-that-are-doing-big-things)

+   [描述性统计关键术语解释](https://www.kdnuggets.com/2017/05/descriptive-statistics-key-terms-explained.html)

+   [数据科学的 8 个基本统计概念](https://www.kdnuggets.com/2020/06/8-basic-statistics-concepts.html)
