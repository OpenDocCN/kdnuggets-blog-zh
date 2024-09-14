# 超越 Pandas 性能的 Rising Library

> 原文：[https://www.kdnuggets.com/2020/12/rising-library-beating-pandas-performance.html](https://www.kdnuggets.com/2020/12/rising-library-beating-pandas-performance.html)

[评论](#comments)

**由 [Ezz El Din Abdullah](https://www.linkedin.com/in/ezzeddinabdullah/)，数据平台工程师**

![Header image](../Images/69442ed2c7287d82e6fe40287b3f23b9.png)

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

**pandas** 最初在 2008 年发布，使用 [Python, Cython 和 C](https://en.wikipedia.org/wiki/Pandas_%28software%29) 编写。今天，我们将比较这个著名库与 [**pypolars**](https://github.com/ritchie46/polars) 的性能，后者是一个使用 Rust 编写的新兴 DataFrame 库。我们在排序和连接 2500 万记录的数据以及连接两个 CSV 文件时进行了比较。

### 下载 Reddit 用户名数据

首先从 Kaggle 下载一个包含约 2600 万个 Reddit 用户名的 CSV 文件：[https://www.kaggle.com/colinmorris/reddit-usernames](https://www.kaggle.com/colinmorris/reddit-usernames)

接下来，让我们创建另一个 CSV 文件，你可以使用你喜欢的文本编辑器或通过命令行创建它：

```py
$ cat >> fake_users.csv
author,n
x,5
z,7
y,6 
```

### 排序

现在，让我们比较这两种排序算法：

**pandas**

```py
$ python sort_with_pandas.py 
Time:  34.35690135599998 
```

**pypolars**

```py
$  python sort_with_pypolars.py 
Time:  23.965840922999973 
```

**约 24 秒，意味着 pypolars 在这里比 pandas 快 1.4 倍**

### 级联

现在我们将查看在连接两个数据框并将它们堆叠成一个数据框时每种工具的表现

**pandas**

pandas 用时 18 秒

```py
$ python concat_with_pandas.py 
Time:  18.12720185799992 
```

**pypolars**

‍

**在这里 pypolars 快 1.2 倍**

```py
$ python concat_with_pypolars.py 
Time: 15.001723207000055 
```

### 连接

**下载 COVID 跟踪数据**

从 [COVID Tracking Project](https://covidtracking.com/data/download) 下载数据，使用这个命令：

```py
$ curl -LO https://covidtracking.com/data/download/all-states-history.csv 
```

将获得你所需的最新冠状病毒传播数据，这些数据存储在 **all-states-history.csv** 文件中

**下载州数据**

这是一个 CSV 文件，指示了每个州的缩写，因为我们需要将其与之前只列出了*州*列缩写的 CSV 进行连接。我们通过这个命令获取数据：

```py
$ curl -LO https://gist.githubusercontent.com/afomi/8824ddb02a68cf15151a804d4d0dc3b7/raw/5f1cfabf2e65c5661a9ed12af27953ae4032b136/states.csv 
```

这将输出 **states.csv** 文件，该文件包含两列：*State* 和 *Abbreviation*

**pandas**

让我们使用 **csvcut** 来筛选出这个结果文件 **joined_pd.csv**：

```py
$ csvcut -c date,state,State joined_pd.csv | head | csvlook 
|       date | state | State       |
| ---------- | ----- | ----------- |
| 2020-11-16 | AK    | ALASKA      |
| 2020-11-16 | AL    | ALABAMA     |
| 2020-11-16 | AR    | ARKANSAS    |
| 2020-11-16 | AS    |             |
| 2020-11-16 | AZ    | ARIZONA     |
| 2020-11-16 | CA    | CALIFORNIA  |
| 2020-11-16 | CO    | COLORADO    |
| 2020-11-16 | CT    | CONNECTICUT |
| 2020-11-16 | DC    |             | 
```

看起来连接正常，且是左连接。如果你对 AS 和 DC 的相关*State* 值为何为空感到好奇，那是因为在**states.csv**文件中不存在这些缩写。如果你查看*Abbreviation*列，你会发现 AS 和 DC 的值都为空。

这里没有 AS 缩写：

```py
$ grep AS states.csv 
ALASKA,AK
ARKANSAS,AR
KANSAS,KS
MASSACHUSETTS,MA
NEBRASKA,NE
TEXAS,TX
WASHINGTON,WA 
```

这里 DC 的值为空：

```py
$ grep DC states.csv 
```

附言：如果你不清楚**csvcut**的用途，我们有一些关于 csvkit 的教程（csvkit 是一个命令行工具，包含 csvcut 及其他用于清理、处理和分析 CSV 的实用命令行工具）。

+   [清理 CSV 数据的第 1 部分](https://www.ezzeddinabdullah.com/posts/how-to-clean-csv-data-at-the-command-line)

+   [清理 CSV 数据的第 2 部分](https://www.ezzeddinabdullah.com/posts/how-to-clean-csv-data-at-the-command-line-part-2) [‍](https://www.ezzeddinabdullah.com/posts/how-to-clean-text-data-at-the-command-line)

**pypolars**

```py
$ python join_with_pypolars.py 
Time:  0.41163978699978543 
```

现在让我们看看合并的数据框是怎样的：

```py
$ csvcut -c date,state,State joined_pl.csv | head | csvlook 
|       date | state | State       |
| ---------- | ----- | ----------- |
| 2020-11-16 | AK    | ALASKA      |
| 2020-11-16 | AL    | ALABAMA     |
| 2020-11-16 | AR    | ARKANSAS    |
| 2020-11-16 | AZ    | ARIZONA     |
| 2020-11-16 | CA    | CALIFORNIA  |
| 2020-11-16 | CO    | COLORADO    |
| 2020-11-16 | CT    | CONNECTICUT |
| 2020-11-16 | DE    | DELAWARE    |
| 2020-11-16 | FL    | FLORIDA     | 
```

所以看起来**pypolars**在连接的列中遗漏了空值，但这是因为默认的连接方式是内连接，而不像 pandas 默认的连接方式是左连接。要获得与 pandas 相同的结果，你需要将第 8 行更改为：

```py
df_all_states_history.join(df_states, left_on=”state”, right_on=”Abbreviation”, how=”left”).to_csv(“joined_pl.csv”) 
```

在我的机器上花费了大约 317 毫秒，这意味着：

**pypolars 在左连接中快 3 倍**

### 最后的思考

最终，我们发现了**pypolars**与**pandas**的性能对比。当然，**pandas**更成熟，因为它已经有 12 年了，社区仍在继续投入，但如果对**pypolars**进行更多的合作，这个新兴库会很出色！

你可能会发现这些教程很有用：

+   [如何在命令行清理文本数据](https://www.ezzeddinabdullah.com/posts/how-to-clean-text-data-at-the-command-line)‍

+   ‍[我们为何使用 Docker](https://www.ezzeddinabdullah.com/posts/penguins-in-docker-a-tutorial-on-why-we-use-docker)‍

保重，下次教程见 :)

和平！

**[**点击这里**](https://upbeat-crafter-1565.ck.page/0f7fd6d5d6)** 获取最新内容到你的邮箱****

**资源**

+   [pandas 维基百科](https://en.wikipedia.org/wiki/Pandas_%28software%29)

+   [pandas 文档](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html)

+   [由 Ritchie Vink 编写的 pypolars 仓库](https://github.com/ritchie46/polars)

+   [pypolars 文档](https://ritchie46.github.io/polars/pypolars/frame.html)

+   [Ritchie Vink 在 Reddit 的评论](https://www.reddit.com/r/rust/comments/jqiwpj/xsv_a_commandline_toolkit_for_csv_data_written_in/gbrfexy?utm_source=share&utm_medium=web2x&context=3)

+   [COVID 跟踪数据](https://covidtracking.com/data/download)

+   [州缩写数据](https://worldpopulationreview.com/states/state-abbreviations)

+   [Reddit 用户名数据集 | Kaggle](https://www.kaggle.com/colinmorris/reddit-usernames)

**个人简介: [Ezz El Din Abdullah](https://www.linkedin.com/in/ezzeddinabdullah/)** ([ezzeddinabdullah.com](http://ezzeddinabdullah.com)) 是 Affectiva 的数据平台工程师。

[原文](https://www.ezzeddinabdullah.com/posts/a-rising-library-beating-pandas-in-performance)。经许可转载。

**相关：**

+   [在Python中合并Pandas数据框](/2020/12/merging-pandas-dataframes-python.html)

+   [Pandas的强化版：使用Dask进行端到端的数据科学](/2020/11/pandas-steroids-dask-python-data-science.html)

+   [每种复杂数据框操作的直观解释与可视化](/2020/11/dataframe-manipulation-explained-visualized.html)

### 更多相关话题

+   [成为伟大数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学，找到目的，再找到目的...] (https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个90亿美元的AI失败，深入探讨](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [建立一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
