# 数据科学命令行：探索数据

> 原文：[`www.kdnuggets.com/2018/02/data-science-command-line-book-exploring-data.html`](https://www.kdnuggets.com/2018/02/data-science-command-line-book-exploring-data.html)

评论

![数据科学命令行 - 书籍](img/ff0b28a9c33f280385a335b5c7bafd58.png) 数据科学的工具多种多样，涵盖了各种生态系统。Python 和 R 是一些比较受欢迎的编程环境，但还有许多其他选项，包括大量的编程和脚本语言、GUI 和基于 web 的工具。

一种较少考虑的攻击方式是严格的命令行方法。确实，你可能使用命令行来执行 Python 脚本，运行 C 程序，或调用 R 环境……或进行其他操作。但从终端运行整个过程呢？

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

回答这个问题的是[**Jeroen Janssens**](https://twitter.com/jeroenhjanssens)，他是现在免费提供的书籍《**数据科学命令行**》的作者。书籍的网站上：

> 本指南展示了命令行的灵活性如何帮助你成为更高效、更有生产力的数据科学家。你将学会如何将小而强大的命令行工具结合起来，以快速获取、清理、探索和建模你的数据。
> 
> ...
> 
> 发现为什么命令行是一种灵活、可扩展且可延展的技术。即使你已经熟练使用 Python 或 R 处理数据，通过利用命令行的强大功能，你的数据显示工作流将得到显著提升。

除了编写关于数据科学命令行工具的详尽调查外，Jeroen 还准备了一个[Docker 镜像](https://hub.docker.com/r/datascienceworkshops/data-science-at-the-command-line)，其中包含 80 多种相关工具，这些工具在书中都有涉及。

[**第七章**](https://www.datascienceatthecommandline.com/chapter-7-exploring-data.html)《数据科学命令行》一书的标题为“探索数据”，重点介绍在[OSEMN 模型](http://www.dataists.com/2010/09/a-taxonomy-of-data-science/)的第三步中使用命令行工具。

![`www.slideshare.net/snungd/big-data-in-public-safety`](img/a2587ced5391ce71c623fa891fcc9da8.png)

OSEMN 模型（来源）。

> 你可以从三个视角探索数据。第一个视角是检查数据及其属性。在这里，我们想知道，例如，原始数据是什么样的，数据集有多少数据点，以及数据集具有哪些特征。
> 
> 探索数据的第二种视角是计算描述性统计。这种视角对于了解单个特征非常有用。一个优势是输出通常简洁且为文本格式，因此可以在命令行上打印出来。
> 
> 第三个视角是创建数据的可视化。从这个视角我们可以深入了解多个特征如何相互作用。我们将讨论一种在命令行上打印的可视化创建方式。然而，可视化最适合在图形用户界面上显示。可视化相对于描述性统计的一个优势是它们更灵活，并且可以传达更多的信息。

这里快速展示了你在命令行中探索数据时可以完成的任务。

首先，你需要安装`python3-csvkit`：

```py

$ sudo apt install python3-csvkit

```

然后，下载一个 CSV 文件进行操作：

```py

$ wget https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv

```

我们可以使用`head`打印`iris.csv`的前几行：

```py

$ head iris.csv

sepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,setosa
4.9,3,1.4,0.2,setosa
4.7,3.2,1.3,0.2,setosa
4.6,3.1,1.5,0.2,setosa
5,3.6,1.4,0.2,setosa
5.4,3.9,1.7,0.4,setosa
4.6,3.4,1.4,0.3,setosa
5,3.4,1.5,0.2,setosa
4.4,2.9,1.4,0.2,setosa

```

很方便。但使用`csvlook`（`python3-csvkit`的一部分），你会得到一个更表格化的视图：

```py
$ head iris.csv | csvlook

|---------------+-------------+--------------+-------------+----------|
|  sepal_length | sepal_width | petal_length | petal_width | species  |
|---------------+-------------+--------------+-------------+----------|
|  5.1          | 3.5         | 1.4          | 0.2         | setosa   |
|  4.9          | 3           | 1.4          | 0.2         | setosa   |
|  4.7          | 3.2         | 1.3          | 0.2         | setosa   |
|  4.6          | 3.1         | 1.5          | 0.2         | setosa   |
|  5            | 3.6         | 1.4          | 0.2         | setosa   |
|  5.4          | 3.9         | 1.7          | 0.4         | setosa   |
|  4.6          | 3.4         | 1.4          | 0.3         | setosa   |
|  5            | 3.4         | 1.5          | 0.2         | setosa   |
|  4.4          | 2.9         | 1.4          | 0.2         | setosa   |
|---------------+-------------+--------------+-------------+----------|

```

更好。想查看整个文件吗？虽然我一直将`cat`的输出传输到`more`中：

```py
$ cat iris.csv | more
```

... Jeroen 提倡使用`less`，这是一个更通用的命令行工具，操作风格类似于`vi`文本编辑器。它允许使用一系列单键命令在文本文件中移动。试试看（'q'表示退出）：

```py
$ iris.csv csvlook | less -S
```

想知道数据集的属性名称吗？很简单，使用特殊的编辑器`sed`：

```py
$ < iris.csv sed -e 's/,/\n/g;q'

sepal_length
sepal_width
petal_length
petal_width
species

```

很好。那么更多的属性元数据呢？

```py
$ csvstat iris.csv

  1\. sepal_length
	 Nulls: False
	Min: 4.3
	Max: 7.9
	Sum: 876.5000000000002
	Mean: 5.843333333333335
	Median: 5.8
	Standard Deviation: 0.8253012917851412
	Unique values: 35
	5 most frequent values:
		5.0:	10
		6.3:	9
		5.1:	9
		6.7:	8
		5.7:	8
... 
```

数据集中的唯一属性值计数如何？

```py
$ csvstat iris.csv --unique

  1\. sepal_length: 35
  2\. sepal_width: 23
  3\. petal_length: 43
  4\. petal_width: 22
  5\. species: 3

```

根据你对命令行的熟悉程度或依赖程度（或愿意成为的程度），你可以深入了解更高级的概念，如创建包含多个顺序执行命令的 bash 脚本。然而，这将从“命令行”数据科学转向更熟悉的脚本编写领域，但使用的是 bash 而不是例如 Python。你也可以做一些介于两者之间的事情，比如创建一个包含 bash 片段的库作为函数，并将它们添加到你的`.bashrc`配置文件中，以便在命令行上重复调用它们处理不同的数据。可能性仅受限于你的想象力和技能。

这只是《[命令行中的数据科学](https://www.datascienceatthecommandline.com/chapter-7-exploring-data.html)》第七章中的一部分，而这本书只是触及了书中蕴含的丰富信息。

无论你怎么看，拥有命令行技能都是无价的。在这种情况下，你应该给这本书一个机会。

**相关：**

+   使用 Python 掌握数据准备的 7 个步骤

+   文本数据预处理的一般方法

+   Python 中的探索性数据分析

### 进一步了解这个话题

+   [停止学习数据科学以寻找目的，并以目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个 90 亿美元的 AI 失败，深度剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
