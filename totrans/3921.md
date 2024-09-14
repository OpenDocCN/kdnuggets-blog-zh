# 数据科学家常犯的 10 大编码错误

> 原文：[https://www.kdnuggets.com/2019/04/top-10-coding-mistakes-data-scientists.html](https://www.kdnuggets.com/2019/04/top-10-coding-mistakes-data-scientists.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [诺曼·尼默](https://www.linkedin.com/in/normanniemer/)，首席数据科学家**

数据科学家是一个“在统计学上比任何软件工程师都强，在软件工程上比任何统计学家都强”的人。许多数据科学家具有统计学背景，但在软件工程方面经验不足。我是一名高级数据科学家，在 Stackoverflow 上的 Python 编码排名前 1%，并与许多（初级）数据科学家一起工作。以下是我经常看到的 10 个常见错误。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

![标题图片](../Images/f1f471b811e4b07b98d166e825945e20.png)

### 1\. 不要分享代码中引用的数据

数据科学需要代码和数据。因此，为了让其他人能够复现你的结果，他们需要访问数据。这看起来很基本，但很多人忘记将数据与代码一起分享。

```py
import pandas as pd
df1 = pd.read_csv('file-i-dont-have.csv') # fails
do_stuff(df)

```

**解决方案**：使用 [d6tpipe](https://github.com/d6t/d6tpipe) 将数据文件与代码一起分享，或上传到 S3/web/google drive 等，或者保存到数据库中，以便接收者可以检索文件（但不要将其添加到 git 中，详见下文）。

### 2\. 硬编码不可访问的路径

类似于第 1 个错误，如果你硬编码了其他人无法访问的路径，他们无法运行你的代码，还得到处查找并手动更改路径。真糟糕！

```py
import pandas as pd
df = pd.read_csv('/path/i-dont/have/data.csv') # fails
do_stuff(df)

# or 
import os
os.chdir('c:\\Users\\yourname\\desktop\\python') # fails

```

**解决方案**：使用相对路径、全局路径配置变量或 [d6tpipe](https://github.com/d6t/d6tpipe) 使你的数据更易于访问。

### 3\. 将数据与代码混合

由于数据科学代码需要数据，为什么不把数据放在同一个目录下呢？同时，图片、报告和其他杂物也一起保存。哎呀，真是一团糟！

```py
├── data.csv
├── ingest.py
├── other-data.csv
├── output.png
├── report.html
└── run.py

```

**解决方案**：将你的目录组织成类别，例如数据、报告、代码等。查看 [Cookiecutter Data Science](https://drivendata.github.io/cookiecutter-data-science/#directory-structure) 或 [d6tflow 项目模板](https://github.com/d6t/d6tflow-template)（参见第 5 条），并使用第 1 条中提到的工具来存储和共享数据。

### 4\. 将数据与源代码一起提交到 Git

现在大多数人都对代码进行版本控制（如果你没有，那是另一个错误！！参见 [git](https://git-scm.com/)）。在尝试共享数据时，可能会诱使你将数据文件添加到版本控制中。这对于非常小的文件是可以的，但 git 对数据，特别是大型文件，优化得不好。

```py
git add data.csv
```

**解决方案**： 使用 #1 中提到的工具来存储和共享数据。如果你确实想对数据进行版本控制，可以查看 [d6tpipe](https://github.com/d6t/d6tpipe)、[DVC](https://dvc.org/) 和 [Git Large File Storage](https://git-lfs.github.com/)。

### 5. 编写函数而不是 DAGs

说够了数据，让我们来谈谈实际的代码！由于你学习编程时首先学会的是函数，数据科学代码大多组织成一系列线性运行的函数。这会引发几个问题，参见 [4 Reasons Why Your Machine Learning Code is Probably Bad](https://github.com/d6t/d6t-python/blob/master/blogs/reasons-why-bad-ml-code.rst)。

```py
def process_data(data, parameter):
    data = do_stuff(data)
    data.to_pickle('data.pkl')

data = pd.read_csv('data.csv')
process_data(data)
df_train = pd.read_pickle(df_train)
model = sklearn.svm.SVC()
model.fit(df_train.iloc[:,:-1], df_train['y'])

```

**解决方案**： 数据科学代码最好不要线性链式调用函数，而是将其组织为一系列具有依赖关系的任务。使用 [d6tflow](https://github.com/d6t/d6tflow) 或 [airflow](https://airflow.apache.org/)。

### 6. 编写 for 循环

就像函数一样，for 循环是你学习编程时学到的第一件事。易于理解，但它们速度慢且过于冗长，通常表明你不了解向量化的替代方法。

```py
x = range(10)
avg = sum(x)/len(x); std = math.sqrt(sum((i-avg)**2 for i in x)/len(x));
zscore = [(i-avg)/std for x]
# should be: scipy.stats.zscore(x)

# or
groupavg = []
for i in df['g'].unique():
	dfg = df[df[g']==i]
	groupavg.append(dfg['g'].mean())
# should be: df.groupby('g').mean()

```

**解决方案**： [Numpy](http://www.numpy.org/)、[scipy](https://www.scipy.org/) 和 [pandas](https://pandas.pydata.org/) 提供了大多数你认为可能需要循环的向量化函数。

### 7. 不写单元测试

随着数据、参数或用户输入的变化，你的代码可能会出错，有时你甚至没有注意到。这可能导致错误的输出，如果有人基于你的输出做决策，错误的数据会导致错误的决策！

**解决方案**： 使用 `assert` 语句检查数据质量。[pandas](https://pandas.pydata.org/pandas-docs/stable/reference/general_utility_functions.html#testing-functions) 提供了相等性测试，[d6tstack](https://github.com/d6t/d6tstack) 提供了数据摄取检查，[d6tjoin](https://github.com/d6t/d6tjoin/blob/master/examples-prejoin.ipynb) 提供了数据连接检查。示例数据检查代码：

```py
assert df['id'].unique().shape[0] == len(ids) # have data for all ids?
assert df.isna().sum()<0.9 # catch missing values
assert df.groupby(['g','date']).size().max() ==1 # no duplicate values/date?
assert d6tjoin.utils.PreJoin([df1,df2],['id','date']).is_all_matched() # all ids matched?

```

### 8. 不记录代码

我明白了，你急于完成一些分析。你快速组合代码以向客户或老板提供结果。然后一周后，他们回来并说“你能改动 xyz 吗”或“你能更新一下吗”。你看着自己的代码却记不起当初的做法。现在想象一下，别人还要运行这段代码。

```py
def some_complicated_function(data):
	data = data[data['column']!='wrong']
	data = data.groupby('date').apply(lambda x: complicated_stuff(x))
	data = data[data['value']<0.9]
	return data

```

**解决方案**： 即使在交付分析后，也要花额外的时间记录你所做的工作。你会感谢自己，别人会更加感激你！你将显得像个专业人士！

### 9. 将数据保存为 csv 或 pickle

备份数据，毕竟这是数据科学。就像函数和循环一样，CSV 和 pickle 文件是常用的，但实际上并不好。CSV 不包含模式，因此每个人都必须重新解析数字和日期。Pickles 解决了这个问题，但只在 python 中工作且没有压缩。两者都不是存储大型数据集的好格式。

```py
def process_data(data, parameter):
    data = do_stuff(data)
    data.to_pickle('data.pkl')

data = pd.read_csv('data.csv')
process_data(data)
df_train = pd.read_pickle(df_train)

```

**解决方案**: 使用 [parquet](https://github.com/dask/fastparquet) 或其他带有数据模式的二进制数据格式，理想情况下是那些能压缩数据的格式。[d6tflow](https://github.com/d6t/d6tflow) 自动将任务的数据输出保存为 parquet 文件，因此你不必处理它。

### 10\. 使用 jupyter notebooks

让我们以一个有争议的问题总结一下：jupyter notebooks 和 CSV 一样普遍。很多人使用它们，但这并不意味着它们好。Jupyter notebooks 促进了上述提到的许多糟糕的软件工程习惯，特别是：

1.  你会倾向于把所有文件放在一个目录中

1.  你编写的代码是从上到下执行的，而不是 DAGs

1.  你没有模块化你的代码

1.  难以调试

1.  代码和输出混合在一个文件中

1.  他们的版本控制做得不好

它看起来容易入门，但扩展性差。

**解决方案**: 使用 [pycharm](https://www.jetbrains.com/pycharm/) 和/或 [spyder](https://www.spyder-ide.org/)。

**简介: [诺曼·尼默](https://www.linkedin.com/in/normanniemer/)** 是一家大型资产管理公司的首席数据科学家，他提供基于数据的投资见解。他拥有哥伦比亚大学的金融工程硕士学位和伦敦Cass商学院的银行与金融学士学位。

[原文](https://github.com/d6t/d6t-python/blob/master/blogs/top10-mistakes-coding.md)。已获得许可转载。

**相关:**

+   [你机器学习代码可能糟糕的4个原因](/2019/02/4-reasons-machine-learning-code-probably-bad.html)

+   [机器学习项目清单](/2018/12/machine-learning-project-checklist.html)

+   [初创公司数据科学项目流程](/2019/01/data-science-project-flow-startups.html)

### 更多相关话题

+   [建立一个强大的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [我在转行数据科学时犯的5个错误](https://www.kdnuggets.com/2023/07/5-mistakes-made-switching-data-science-career.html)

+   [每位初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成为伟大数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)
