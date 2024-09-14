# Python中的大文件并行处理

> 原文：[https://www.kdnuggets.com/2022/07/parallel-processing-large-file-python.html](https://www.kdnuggets.com/2022/07/parallel-processing-large-file-python.html)

![Python中的大文件并行处理](../Images/644d2d1ea41ea4a38f2569081bef10d5.png)

图片由作者提供

对于并行处理，我们将任务划分为子单元。这增加了程序处理的作业数量，并减少了总体处理时间。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的IT组织

* * *

例如，如果你正在处理一个大型CSV文件，并且想要修改单列。我们会将数据以数组的形式传递给函数，函数会根据可用的**工作者**的数量同时并行处理多个值。这些工作者的数量基于你处理器中的核心数。

> **注意：** 在较小的数据集上使用并行处理不会提高处理速度。

在这篇博客中，我们将学习如何使用**multiprocessing**、**joblib**和**tqdm** Python包来减少大型文件的处理时间。这是一个简单的教程，适用于任何文件、数据库、图像、视频和音频。

> **注意：** 我们使用的是Kaggle笔记本进行实验。处理时间可能因机器而异。

# 入门

我们将使用来自Kaggle的[美国事故数据集（2016 - 2021）](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)，该数据集包含280万条记录和47列。

我们将导入`multiprocessing`、`joblib`和`tqdm`用于**并行处理**，`pandas`用于**数据引入**，以及`re`、`nltk`和`string`用于**文本处理**。

```py
*# Parallel Computing*
import multiprocessing as mp
from joblib import Parallel, delayed
from tqdm.notebook import tqdm

*# Data Ingestion* 
import pandas as pd

*# Text Processing* 
import re 
from nltk.corpus import stopwords
import string
```

在我们开始之前，让我们将`n_workers`设置为`cpu_count()`的两倍。如你所见，我们有8个工作者。

```py
n_workers = 2 * mp.cpu_count()

print(f"{n_workers} workers are available")

>>> 8 workers are available
```

在下一步中，我们将使用**pandas**的`read_csv`函数引入大型CSV文件。然后，打印出数据框的形状、列名和处理时间。

> **注意：** Jupyter的魔法函数`%%time`可以在处理结束时显示**CPU时间**和**墙钟时间**。

```py
%%time
file_name="../input/us-accidents/US_Accidents_Dec21_updated.csv"
df = pd.read_csv(file_name)

print(f"Shape:{df.shape}\n\nColumn Names:\n{df.columns}\n")
```

**输出**

```py
Shape:(2845342, 47)

Column Names:

Index(['ID', 'Severity', 'Start_Time', 'End_Time', 'Start_Lat', 'Start_Lng',
'End_Lat', 'End_Lng', 'Distance(mi)', 'Description', 'Number', 'Street',
'Side', 'City', 'County', 'State', 'Zipcode', 'Country', 'Timezone',
'Airport_Code', 'Weather_Timestamp', 'Temperature(F)', 'Wind_Chill(F)',
'Humidity(%)', 'Pressure(in)', 'Visibility(mi)', 'Wind_Direction',
'Wind_Speed(mph)', 'Precipitation(in)', 'Weather_Condition', 'Amenity',
'Bump', 'Crossing', 'Give_Way', 'Junction', 'No_Exit', 'Railway',
'Roundabout', 'Station', 'Stop', 'Traffic_Calming', 'Traffic_Signal',
'Turning_Loop', 'Sunrise_Sunset', 'Civil_Twilight', 'Nautical_Twilight',
'Astronomical_Twilight'],
dtype='object')

CPU times: user 33.9 s, sys: 3.93 s, total: 37.9 s
Wall time: 46.9 s
```

# 清理文本

`clean_text`是一个简单的文本处理和清理函数。我们将使用`nltk.corpus`获取英语**stopwords**，并用它来过滤文本中的停用词。之后，我们将从句子中移除特殊字符和多余的空格。这将成为确定**serial**、**parallel**和**batch**处理时间的基准函数。

```py
def clean_text(text): 
  *# Remove stop words*
  stops = stopwords.words("english")
  text = " ".join([word for word in text.split() if word not in stops])
  *# Remove Special Characters*
  text = text.translate(str.maketrans('', '', string.punctuation))
  *# removing the extra spaces*
  text = re.sub(' +',' ', text)
  return text
```

# 串行处理

对于串行处理，我们可以使用pandas的`.apply()`函数，但如果你想查看进度条，你需要激活**tqdm**以支持**pandas**，然后使用`.progress_apply()`函数。

我们将处理280万条记录，并将结果保存回“Description”列。

```py
%%time
tqdm.pandas()

df['Description'] = df['Description'].progress_apply(clean_text)
```

**输出**

高端处理器处理280万行数据的串行处理时间为9分钟5秒。

```py
100%  2845342/2845342 [09:05<00:00, 5724.25it/s]

CPU times: user 8min 14s, sys: 53.6 s, total: 9min 7s
Wall time: 9min 5s
```

# 多进程

有多种方法可以并行处理文件，我们将学习所有这些方法。`multiprocessing`是一个内置的Python包，通常用于并行处理大文件。

我们将创建一个包含**8个工作线程**的多进程**Pool**，并使用**map**函数来启动处理。为了显示进度条，我们使用**tqdm**。

map函数由两个部分组成。第一部分需要函数，第二部分需要一个参数或参数列表。

了解更多信息，请阅读[文档](https://docs.python.org/3/library/multiprocessing.html)。

```py
%%time
p = mp.Pool(n_workers) 

df['Description'] = p.map(clean_text,tqdm(df['Description']))
```

**输出**

我们将处理时间提高了近**3倍**。处理时间从**9分钟5秒**减少到**3分钟51秒**。

```py
100%  2845342/2845342 [02:58<00:00, 135646.12it/s]

CPU times: user 5.68 s, sys: 1.56 s, total: 7.23 s
Wall time: 3min 51s
```

# 并行

我们现在将学习另一个Python包来执行并行处理。在这一部分，我们将使用joblib的**Parallel**和**delayed**来模拟**map**函数。

+   Parallel需要两个参数：n_jobs = 8和backend = multiprocessing。

+   然后，我们将把**clean_text**添加到**delayed**函数中。

+   创建一个循环一次处理一个值。

下面的过程相当通用，你可以根据需要修改你的函数和数组。我已经用它处理了成千上万的音频和视频文件，没有遇到任何问题。

**推荐：** 使用`try:`和`except:`添加异常处理

```py
def text_parallel_clean(array):
  result = Parallel(n_jobs=n_workers,backend="multiprocessing")(
  delayed(clean_text)
  (text) 
  for text in tqdm(array)
  )
  return result
```

将“Description”列添加到`text_parallel_clean()`中。

```py
%%time
df['Description'] = text_parallel_clean(df['Description'])
```

**输出**

我们的函数比使用**Pool**的多进程慢了13秒。即便如此，**Parallel**比**serial**处理快了4分钟59秒。

```py
100%  2845342/2845342 [04:03<00:00, 10514.98it/s]

CPU times: user 44.2 s, sys: 2.92 s, total: 47.1 s
Wall time: 4min 4s
```

# 并行批处理

有一种更好的方法来处理大文件，即将其拆分为批次并进行并行处理。让我们从创建一个批处理函数开始，该函数将在一批值上运行`clean_function`。

## 批处理函数

```py
def proc_batch(batch):
  return [
  clean_text(text)
  for text in batch
  ]
```

## 将文件拆分为批次

下面的函数将根据工作线程的数量将文件拆分为多个批次。在我们的案例中，我们得到8个批次。

```py
def batch_file(array,n_workers):
  file_len = len(array)
  batch_size = round(file_len / n_workers)
  batches = [
  array[ix:ix+batch_size]
  for ix in tqdm(range(0, file_len, batch_size))
  ]
  return batches

batches = batch_file(df['Description'],n_workers)

>>> 100%  8/8 [00:00<00:00, 280.01it/s]
```

## 运行并行批处理

最后，我们将使用**Parallel**和**delayed**来处理批次。

> **注意：** 要获取单个值数组，我们必须运行列表推导式，如下所示。

```py
%%time
batch_output = Parallel(n_jobs=n_workers,backend="multiprocessing")(
  delayed(proc_batch)
  (batch) 
  for batch in tqdm(batches)
  )

df['Description'] = [j for i in batch_output for j in i]
```

**输出**

我们已经改进了处理时间。这种技术因处理复杂数据和训练深度学习模型而闻名。

```py
100%  8/8 [00:00<00:00, 2.19it/s]

CPU times: user 3.39 s, sys: 1.42 s, total: 4.81 s
Wall time: 3min 56s
```

# tqdm并发

tqdm将多进程处理提升到了一个新的水平。它简单且强大。我会向每位数据科学家推荐它。

查看[文档](https://tqdm.github.io/)以了解更多关于多进程的信息。

`process_map` 需要：

1.  函数名称

1.  数据框列

1.  max_workers

1.  chucksize类似于批大小。我们将使用工作数量来计算批大小，或者你可以根据个人喜好添加数量。

```py
%%time
from tqdm.contrib.concurrent import process_map
batch = round(len(df)/n_workers)

df["Description"] = process_map(
    clean_text, df["Description"], max_workers=n_workers, chunksize=batch
)
```

**输出**

只需一行代码，我们就能获得最佳结果。

```py
100%  2845342/2845342 [03:48<00:00, 1426320.93it/s]

CPU times: user 7.32 s, sys: 1.97 s, total: 9.29 s
Wall time: 3min 51s
```

# 结论

你需要找到一个平衡点，选择最适合你情况的技术。可以是串行处理、并行处理或批处理。如果你处理的是较小且复杂性较低的数据集，并行处理可能会适得其反。

在这个迷你教程中，我们学习了各种Python包和技术，这些技术允许我们并行处理数据函数。

如果你仅处理表格数据集并希望提升处理性能，我建议你尝试[Dask](https://www.dask.org/)、[datatable](https://github.com/h2oai/datatable)和[RAPIDS](https://rapids.ai/)

## 参考

+   [Python中的并行批处理 | Dennis Bakhuis | Towards Data Science](https://towardsdatascience.com/parallel-batch-processing-in-python-8dcce607d226)

+   [Python中大文件的并行处理 · Nurda Bolatov](https://nurdabolatov.com/parallel-processing-large-file-in-python)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为有心理疾病困扰的学生开发AI产品。

### 主题更多

+   [KDnuggets新闻，7月20日：机器学习算法解释](https://www.kdnuggets.com/2022/n29.html)

+   [提示工程中的并行处理：思维骨架技术](https://www.kdnuggets.com/parallel-processing-in-prompt-engineering-the-skeleton-of-thought-technique)

+   [用Python的Watchdog监控你的文件系统](https://www.kdnuggets.com/monitor-your-file-system-with-pythons-watchdog)

+   [Python字符串处理备忘单](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)

+   [掌握SQL、Python、数据科学、机器学习等的25本免费书籍](https://www.kdnuggets.com/25-free-books-to-master-sql-python-data-science-machine-learning-and-natural-language-processing)

+   [顶级开源大型语言模型](https://www.kdnuggets.com/2022/09/john-snow-top-open-source-large-language-models.html)

![](../Images/eb105b2b614c4a6165d83fbc2b9711a1.png)

[](/news/subscribe.html)

[获取免费的电子书《伟大的自然语言处理入门》和《数据科学备忘单的完整合集》，以及有关数据科学、机器学习、人工智能和分析的领先新闻通讯，直接送到你的收件箱。](/news/subscribe.html)

订阅即表示你接受 KDnuggets 的 [隐私政策](https://www.kdnuggets.com/news/privacy-policy.html)

* * *

[<= 上一篇文章](https://www.kdnuggets.com/2023/02/data-cleaning-python-cheat-sheet.html)[下一篇文章 =>](https://www.kdnuggets.com/2023/02/essential-ab-testing-course-data-science.html)

### [最新文章](/news/index.html)

+   [7 个你可能错过的免费的数据科学云 IDE](https://www.kdnuggets.com/7-free-cloud-ide-for-data-science-that-you-are-missing-out)

+   [启动你的数据科学职业生涯：自学路径指南](https://www.kdnuggets.com/bootstrapping-your-data-science-career-a-guide-to-self-learning-pathways)

+   [开发端到端的数据科学管道，包括数据摄取、处理和可视化](https://www.kdnuggets.com/developing-end-to-end-data-science-pipelines-with-data-ingestion-processing-and-visualization)

+   [如何跟踪 Python 中的内存分配](https://www.kdnuggets.com/how-to-trace-memory-allocation-in-python)

+   [如何在 R 中导入数据](https://www.kdnuggets.com/how-to-import-data-in-r)

+   [值得关注的 5 个机器学习 API](https://www.kdnuggets.com/top-5-machine-learning-apis-practitioners-should-know)

|

## 热门文章

|

+   [5 个数据科学中隐藏的宝石 Python 库](https://www.kdnuggets.com/5-hidden-gem-python-libraries-for-data-science)

+   [掌握数据科学所需的 7 个数学步骤](https://www.kdnuggets.com/7-steps-to-mastering-math-for-data-science)

+   [微软的 4 个入门级证书帮助你找到热门工作](https://www.kdnuggets.com/4-entry-level-certificates-from-microsoft-to-land-in-demand-jobs)

+   [如何使用 Pandas 有效管理分类数据](https://www.kdnuggets.com/how-to-manage-categorical-data-effectively-with-pandas)

+   [每个数据工程师都应该知道的 10 个内置 Python 模块](https://www.kdnuggets.com/10-built-in-python-modules-every-data-engineer-should-know)

+   [值得关注的 5 个机器学习 API](https://www.kdnuggets.com/top-5-machine-learning-apis-practitioners-should-know)

+   [使用 FastAPI 构建 ML 驱动的 Web 应用](https://www.kdnuggets.com/using-fastapi-for-building-ml-powered-web-apps)

+   [我参加了 Udacity 的 Google 免费 A/B 测试课程：我学到了什么](https://www.kdnuggets.com/i-took-udacitys-free-a-b-testing-course-by-google-heres-what-i-learned)

+   [掌握数据工程的项目想法](https://www.kdnuggets.com/project-ideas-to-master-data-engineering)

+   [在本地使用 FLUX.1](https://www.kdnuggets.com/using-flux-1-locally)

* * *

© 2024 [Guiding Tech Media](https://www.guidingtechmedia.com/)   |   [关于](/about/index.html)   |   [联系](/contact.html)   |   [广告合作](https://mailchi.mp/kdnuggets/media-kit) |   [隐私政策](/news/privacy-policy.html)   |   [服务条款](/terms-of-service.html)

发表时间：2023年2月21日，作者：Abid Ali Awan
