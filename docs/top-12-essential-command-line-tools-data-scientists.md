# 数据科学家的前 12 个必备命令行工具

> 原文：[`www.kdnuggets.com/2018/03/top-12-essential-command-line-tools-data-scientists.html`](https://www.kdnuggets.com/2018/03/top-12-essential-command-line-tools-data-scientists.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

![Header image](img/f33aa745a691adba06e0d95f1c1542f6.png)

本文简要概述了十几个类 Unix 操作系统命令行工具，这些工具对数据科学任务可能有用。列表中不包括任何一般的文件管理命令（`pwd`、`ls`、`mkdir`、`rm` 等）或远程会话管理工具（`rsh`、`ssh` 等），而是包括了一些从数据科学角度来看有用的工具，通常与各种程度的数据检查和处理相关。这些工具也都包含在典型的类 Unix 操作系统中。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

这确实很基础，但我鼓励你在适当的时候寻找更多命令示例。工具名称链接到维基百科条目，而不是 [man 页面](https://linux.die.net/man/)，因为我认为前者对新手更友好。

### 1\. `wget`

[`wget`](https://en.wikipedia.org/wiki/Wget) 是一个文件检索工具，用于从远程位置下载文件。

`wget` 的最基本用法如下所示，用于下载远程文件：

```py

~$ wget https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv

--2018-03-20 18:27:21--  https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.20.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.20.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3716 (3.6K) [text/plain]
Saving to: ‘iris.csv’

iris.csv
100 [=======================================================================================================>]   3.63K  --.-KB/s    in 0s      

2018-03-20 18:27:21 (19.9 MB/s) - ‘iris.csv’ saved [3716/3716]

```

### 2\. `cat`

[`cat`](https://en.wikipedia.org/wiki/Cat_(Unix)) 是一个将文件内容输出到标准输出的工具。名称来源于 *concatenate*。

更复杂的用例包括将文件合并在一起（实际的连接）、将文件附加到另一个文件、对文件行进行编号等等。

```py

~$ cat iris.csv

sepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,setosa
4.9,3,1.4,0.2,setosa
4.7,3.2,1.3,0.2,setosa
4.6,3.1,1.5,0.2,setosa
5,3.6,1.4,0.2,setosa
...
6.7,3,5.2,2.3,virginica
6.3,2.5,5,1.9,virginica
6.5,3,5.2,2,virginica
6.2,3.4,5.4,2.3,virginica
5.9,3,5.1,1.8,virginica

```

### 3\. `wc`

[`wc`](https://en.wikipedia.org/wiki/Wc_(Unix)) 命令用于从文本文件中生成字数统计、行数统计、字节数统计及相关信息。`wc` 的默认输出，在没有选项时，是一行包含行数、字数（注意每行没有换行的单个字符串被算作一个单词）、字符数和文件名。

```py

~$ wc iris.cs

151  151 3716 iris.csv

```

### 4\. `head`

[`head`](https://en.wikipedia.org/wiki/Head_(Unix)) 将文件的前 n 行（默认 10 行）输出到标准输出。显示的行数可以通过 `-n` 选项设置。

```py

~$ head -n 5 iris.csv

sepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,setosa
4.9,3,1.4,0.2,setosa
4.7,3.2,1.3,0.2,setosa
4.6,3.1,1.5,0.2,setosa

```

### 5\. `tail`

有人猜到 [`tail`](https://en.wikipedia.org/wiki/Tail_(Unix)) 是做什么的吗？

```py
~$ tail -n 5 iris.csv

6.7,3,5.2,2.3,virginica
6.3,2.5,5,1.9,virginica
6.5,3,5.2,2,virginica
6.2,3.4,5.4,2.3,virginica
5.9,3,5.1,1.8,virginica

```

![Typing crazy](img/34e52931353bd75ff0d26c15821b1320.png)

操作命令行魔法。

### 6\. `find`

[`find`](https://en.wikipedia.org/wiki/Find_(Unix)) 是一个用于在文件系统中搜索特定文件的实用程序。

以下命令在当前目录（"."）中搜索以“iris”开头并以任意字符（"-name 'iris*'"）结尾的文件，且文件类型为常规文件（"-type f"）：

```py
~$ find . -name 'iris*' -type f

./iris.csv
./notebooks/kmeans-sharding-init/sharding/tests/results/iris_time_results.csv
./notebooks/ml-workflows-python-scratch/iris_raw.csv
./notebooks/ml-workflows-python-scratch/iris_clean.csv
...

```

### 7\. `cut`

[`cut`](https://en.wikipedia.org/wiki/Cut_(Unix)) 用于从文件中切割出文本行的部分。虽然这些切片可以使用多种标准进行，但`cut`在从 CSV 文件中提取列数据时非常有用。

这会输出使用逗号作为字段分隔符（"-d ','"）的 iris.csv 文件的第五列（"-f 5"）：

```py
~$ cut -d ',' -f 5 iris.csv

species
setosa
setosa
setosa
...

```

### 8\. `uniq`

[`uniq`](https://en.wikipedia.org/wiki/Uniq) 通过将相同的连续行合并为单一副本，来修改文本文件的输出。单独使用时这可能看起来并不特别有趣，但在构建命令行管道（将一个命令的输出传递到另一个命令的输入）时，这会变得非常有用。

以下内容为我们提供了在第五列中持有的鸢尾花数据集类名称及其计数的唯一统计：

```py
~$ tail -n 150 iris.csv | cut -d "," -f 5 | uniq -c

50 setosa
50 versicolor
50 virginica

```

![Cowsay whut?!?](img/6df61c61b757a4c270b8969d61750952.png)

牛说什么。

### 9\. `awk`

[`awk`](https://en.wikipedia.org/wiki/AWK) 实际上不是一个“命令”，而是一个完整的编程语言。它用于处理和提取文本，可以在命令行中以单行命令形式调用。

掌握`awk`需要一些时间，但在此之前，下面是它能完成的一个示例。考虑到我们的示例文件——iris.csv——相当有限（尤其是在文本多样性方面），这行代码将调用`awk`，在指定文件（"iris.csv"）中搜索“setosa”字符串，并逐一打印遇到的项目（保存在$0 变量中）：

```py
~$ awk '/setosa/ { print $0 }' iris.csv

5.1,3.5,1.4,0.2,setosa
4.9,3,1.4,0.2,setosa
4.7,3.2,1.3,0.2,setosa
4.6,3.1,1.5,0.2,setosa
5,3.6,1.4,0.2,setosa

```

### 10\. `grep`

[`grep`](https://en.wikipedia.org/wiki/Grep) 是另一个文本处理工具，用于字符串和正则表达式匹配。

```py
~$ grep -i "vir" iris.csv

6.3,3.3,6,2.5,virginica
5.8,2.7,5.1,1.9,virginica
7.1,3,5.9,2.1,virginica
...

```

如果你在命令行中花费了大量时间进行文本处理，`grep`无疑是一个你会非常熟悉的工具。[点击这里查看更多有用的细节](https://www.thegeekstuff.com/2009/03/15-practical-unix-grep-command-examples)。

### 11\. `sed`

[`sed`](https://en.wikipedia.org/wiki/Sed) 是一个流编辑器，另一种文本处理和转换工具，类似于`awk`。我们将使用下面这行代码将 iris.csv 文件中“setosa”的出现替换为“iris-setosa”：

```py
~$ sed 's/setosa/iris-setosa/g' iris.csv > output.csv
~$ head output.csv

sepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,iris-setosa
4.9,3,1.4,0.2,iris-setosa
4.7,3.2,1.3,0.2,iris-setosa
...

```

### 12\. `history`

[`history`](https://en.wikipedia.org/wiki/History_(Unix)) 非常简单，但也非常有用，特别是当你依赖于在命令行中完成的数据准备时。

```py
~$ history

547  tail iris.csv
548  tail -n 150 iris.csv
549  tail -n 150 iris.csv | cut -d "," -f 5 | uniq -c
550  clear
551  history

```

-   这里有一个简单的介绍，讲述了 12 个实用的命令行工具。这只是数据科学（或其他任何目标）在命令行上可能实现的一小部分。摆脱鼠标的束缚，看看你的生产力如何提高。

**相关**：

+   命令行中的数据科学：探索数据

+   Docker 如何帮助你成为更高效的数据科学家

+   在 Pandas 中使用 Excel

### 更多相关话题

+   [数据科学的 5 个额外命令行工具](https://www.kdnuggets.com/2023/03/5-command-line-tools-data-science.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学以寻找目标，并通过找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成功的数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [90 亿美元的人工智能失败，经过检讨](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)
