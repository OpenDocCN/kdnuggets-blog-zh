# 强大的 CSV 处理与 kdb+

> 原文：[https://www.kdnuggets.com/2020/07/powerful-csv-processing-kdb.html](https://www.kdnuggets.com/2020/07/powerful-csv-processing-kdb.html)

[评论](#comments)

**由 [Ferenc Bodon 博士](https://www.linkedin.com/in/ferencbodon/)，数据工程师，Kx 的云解决方案架构师**

![Image](../Images/5312dad5957ac7d5fc264099ee0f10d3.png)

* * *

## 我们的前三课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

逗号分隔文本文件（CSV）是数据处理的最基本格式。所有支持处理关系数据的编程语言和软件，都提供一定程度的 CSV 处理功能。你可以在不安装数据库管理系统的情况下持久化和处理数据。通常，你不需要具有所有功能的完整 DBMS，如处理事务和并发/远程访问、索引等……轻量级的 CSV 格式允许轻松处理和共享捕获的信息。

CSV 格式早于个人电脑，并且已成为近 50 年来最常见的数据交换格式之一。CSV 文件将在未来继续存在。有效地处理这种格式是高效开发人员、数据工程师/科学家、DevOps 人员等的核心要求……你可能需要过滤行、按列排序、选择现有列或派生新列。也许你需要进行复杂的分析，这需要聚合和分组。

本文提供了处理 CSV 文件的可用工具的概述，并描述了 kdb+ 及其查询语言 q 如何将 CSV 处理提升到新的性能和简洁水平。

### 常见的 CSV 工具

### Linux 命令行工具

许多 CSV 处理需要在具有强大终端控制台和某种 shell 的 Linux 或 Mac 环境中完成。大多数 shell，如 Bash，支持数组。你可以逐行读取 CSV 并将所有字段存储在数组变量中。你可以使用内置的字符串操作和整数计算（甚至用 `bc -l` 进行浮点计算）对单元格值进行操作。代码将会很长且难以维护。

一般的文本处理工具如[`awk`](https://en.wikipedia.org/wiki/AWK)和[`sed`](https://en.wikipedia.org/wiki/Sed)脚本可能会导致更短更简单的代码。像[`cut`](https://en.wikipedia.org/wiki/Cut_(Unix))、[`sort`](https://en.wikipedia.org/wiki/Sort_(Unix))、[`uniq`](https://en.wikipedia.org/wiki/Uniq)和[`paste`](https://en.wikipedia.org/wiki/Paste_(Unix))这样的命令进一步简化了 CSV 处理。你可以指定分隔符并**按位置引用字段**。

世界在不断变化，CSV 文件也是如此。如果在引用的列前添加了新列，或者列被重新排列（例如，将相关列移到一起），基于位置的引用会被打破。这个问题表现得很隐蔽：你的脚本可能运行顺利，但你只是用到了不同的列进行计算！如果你没有回归测试框架来保护你的代码库，最终用户（或你的竞争对手）可能会发现这个问题。这可能会很尴尬。

基于位置的引用会创建脆弱的代码。使用这些 Linux 命令处理 CSV 对于原型设计和快速分析很有效，但一旦你的代码库开始增加或你与其他同事共享脚本，就会遇到限制。难怪在 SQL 中，基于位置的列引用是有限制且不鼓励使用的。

Linux 命令行工具的巨大优势在于无需安装。你的 shell 脚本很可能可以在其他人的 Linux 系统上运行。熟悉 Linux 中随手可用的工具是有用的，但在处理复杂且长期的任务时，通常应避免使用这些工具。

### CSVKit

许多开源库提供 CSV 支持。Python 库[CSVKit](https://csvkit.readthedocs.io/en/latest/#)是最受欢迎的库之一。它提供了比本地 Linux 命令更强大的解决方案，例如允许**按名称引用列**。列名存储在 CSV 的第一行。按名称引用对列重命名很敏感，但这可能发生的频率低于添加或移动列。

此外，CSVKit 对第一行的处理比通用文本工具更好。Linux 命令`sort`将第一行视为其他任何一行，并可能将其放置在输出的中间。类似地，`cat`在连接多个 CSV 文件时包括了第一行。命令`csvsort`和`csvstack`正确处理第一行。

最后，CSVKit 开发者特别注意提供一致的命令行参数，例如分隔符由`-d`定义。相比之下，你需要记住`sort`的分隔符由`-t`指定，而其他 Linux 命令，如[`cut`](https://en.wikipedia.org/wiki/Cut_(Unix))、[`paste`](https://en.wikipedia.org/wiki/Paste_(Unix))，则使用`-d`。

CSVKit 包含一些简单命名的工具，如 [`csvcut`](https://csvkit.readthedocs.io/en/latest/scripts/csvcut.html)、[`csvgrep`](https://csvkit.readthedocs.io/en/latest/scripts/csvgrep.html) 和 [`csvsort`](https://csvkit.readthedocs.io/en/latest/scripts/csvsort.html)，它们替代了传统的 Linux 命令 `cut`、`grep` 和 `sort`。尽管如此，Linux 命令的优点在于它们的速度。

你可能使用 Linux 命令 [`head`](https://en.wikipedia.org/wiki/Head_(Unix))、[`tail`](https://en.wikipedia.org/wiki/Tail_(Unix))、[`less`](https://en.wikipedia.org/wiki/Less_(Unix))/[`more`](https://en.wikipedia.org/wiki/More_(command)) 和 [`cat`](https://en.wikipedia.org/wiki/Cat_(Unix)) 来快速查看文本文件的内容。不幸的是，这些工具的输出对于 CSV 文件并不理想。列对不齐，你会花费大量时间眯着眼睛在单色屏幕上判断给定单元格属于哪个列。你可能会放弃并将数据导入 Excel 或 Google 表格。但是，如果文件在远程机器上，你首先需要将其通过 SCP 传输到桌面。你可以通过使用 [`csvlook`](https://csvkit.readthedocs.io/en/latest/scripts/csvlook.html) 节省时间并在控制台中工作。命令 `csvlook` 能很好地对齐列名下的列。要执行下面的命令，请下载武器销售数据并将其转换为 `data.csv`，如 [CSVKit 教程](https://csvkit.readthedocs.io/en/latest/tutorial/1_getting_started.html#getting-the-data) 所述。

```py
$ csvlook --max-rows 20 data.csv
```

如果你的控制台很窄，不用担心：将输出管道到 `less -S` 并使用箭头键左右移动。

CSVKit 中另一个有用的扩展是命令 [`csvstat`](https://csvkit.readthedocs.io/en/latest/scripts/csvstat.html)。它分析文件内容并显示统计信息，比如所有列的不同值的数量。同时，它会尝试推断数据类型。如果列类型是数字，它还会返回值的最大值、最小值、均值、中位数和标准差。

要执行聚合、过滤和分组操作，可以使用 CSVKit 命令 [`csvsql`](https://csvkit.readthedocs.io/en/latest/scripts/csvsql.html)，该命令允许你对 CSV 文件运行 ANSI SQL 命令。

### xsv

一些 CSVKit 命令比较慢，因为它们将整个文件加载到内存中并创建内存数据库。Rust 开发者重新实现了几个传统工具，如 `cat`、`ls`、`grep` 和 `find`，并且像 [`bat`](https://github.com/sharkdp/bat)、[`exa`](https://github.com/ogham/exa)、[`ripgrep`](https://github.com/BurntSushi/ripgrep) 和 [`fd`](https://github.com/sharkdp/fd) 这样的工具应运而生。难怪他们还创建了一个高性能的 CSV 处理工具，即库 [`xsv`](https://github.com/BurntSushi/xsv)。

Rust 库还支持选择列、过滤、排序和连接 CSV 文件。可以为经常处理的 CSV 文件添加索引，以加快操作速度。索引是迈向 DBMS 的优雅而轻量化的一步。

### 类型推断

CSV是一种文本格式，不包含列的类型信息。一个字符串可以根据其值转换为数据类型。如果列的所有值都匹配模式`YYYY.MM.DD`，我们可以得出列包含日期的结论。但我们如何处理字面量100000？它是整数，还是时间10:00:00？也许源过程只支持数字并省略了时间分隔符？在现实生活中，关于来源的信息并不总是可用，你需要逆向工程数据。如果列的所有值都匹配字符串`HHMMSS`，那么我们**可以**高置信度地得出列包含时间值的结论。以下是我们可以采取的两种方法来做出决定。

首先，我们可以严格一些：我们预定义任何类型需要匹配的模式。模式不重叠。如果时间被定义为`HH:MM:SS`而整数定义为`[1-9][0-9]*`，那么100000是一个整数。

其次，我们可以让模式重叠，如果发生冲突，我们选择域较小的类型或基于某些规则。这种方法偏好时间而非整数，如果时间模式还包含`HHMMSS`。

CSVKit库实现了第一种方法。

### q/kdb+

Kdb+是[全球最快的时间序列数据库](https://kx.com/media/2018/10/Kx-FAQ.pdf)，经过优化，用于摄取、分析和存储大量结构化数据。其查询语言叫做[Q](https://code.kx.com/q4m3/)，是一种通用编程语言。表格在q中是第一类对象。Q表语义上类似于Pandas/R数据框。你可以将表格持久化到磁盘，因此这个解决方案可以被认为是一个数据库，称为[kdb+](https://code.kx.com/q4m3/14_Introduction_to_Kdb+/)。

导出和导入CSV文件是核心语言的一部分。表`t`可以通过命令保存在目录`dir`中。

```py
q) save `:dir/t.csv
```

这里`q)`表示启动q解释器后的默认提示符。

要选择不同的名称，例如`output.csv`，然后使用文件文本操作符`0:`来保存文本，并使用实用工具[`.h.cd`](https://code.kx.com/q/ref/doth/#hcd-csv-from-data)将kdb+表转换为字符串列表。

```py
q) `output.csv 0: .h.cd t
```

你也可以使用[其他分隔符](https://code.kx.com/q/ref/file-text/#prepare-text)而不是逗号。

要导入CSV`data.csv`，请指定列类型和分隔符。以下命令假设列名在第一行。

```py
q) ("SSE**F"; enlist csv) 0: `data.csv
```

类型编码可以在[kdb+参考卡](https://code.kx.com/q/ref/#datatypes)上找到，`E`代表实数，`I`代表整数，`S`代表枚举（kdb+术语中的符号），`D`代表日期，等等……使用空格来忽略列。字符`*`代表字符串。

手动指定类型的维护成本高，对于宽CSV文件来说繁琐且容易出错。插入一个新列可能会破坏代码。

幸运的是，Kx 开源库[csvutil](https://github.com/KxSystems/kdb/blob/master/utils/csvutil.q)和[csvguess](https://github.com/KxSystems/kdb/blob/master/utils/csvguess.q)提供了一个方便且可靠的解决方案。

脚本`csvutil.q`包含一个函数，用于加载 CSV 文件，分析其值，推断类型并返回一个 kdb+ 表。

```py
q) \l utils/csvutil.q
q) .csv.read `data.csv
```

[![通过 .csv.read 显示 CSV 内容](../Images/fdebb8a6b54bd78bf986c1e29bd54f73.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/csvread.png)

脚本`csvguess.q`让你能够将关于列的元数据保存到文本文件中。开发人员可以查看和调整类型列，并在生产环境中使用这些元数据以正确的类型加载 CSV。这两个脚本有不同的用户。数据科学家更倾向于使用`csvutil.q`进行临时分析。当 IT 人员配置 kdb+ CSV 数据源时，他们会使用`csvguess.q`的辅助元数据导出和导入功能。类型提示提供了一个比手动输入所有列的类型更少出错的解决方案。

### 类型转换

库`csvutil.q`支持两种类型转换 - 严格模式由`.csv.basicread`实现，而`.csv.read`函数检查更多模式以推断类型。

函数`.csv.read`只是一个通用函数`.csv.data`的包装器，它接受一个文件名和一个元数据表。元数据可以通过`.csv.basicinfo`和`.csv.info`生成，具体取决于我们希望采用的推断规则。你可以在 q 解释器中通过输入函数名称并按 Enter 键来查看函数定义。

```py
q) .csv.read
{[file] data[file; info[file]]}
```

列元数据表有点类似于`csvstat`的输出。

[![通过 csvinfo 显示 CSV 元信息](../Images/97a66dfa6a6970a44e94db09b7821fd1.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/csvinfo.png)

每一行属于一列，每个字段存储一些关于列的有用信息，如名称（`c`）、推断的类型（`t`）、最大宽度（`mw`）等。字段`gr`，即*粒度*，特别有趣，因为它表示列值的压缩效果如何，以及它们是否应作为枚举存储而不是字符串。有关列的更多详细信息，请参见[文档](https://github.com/KxSystems/kdb/blob/master/utils/csv.md)。

你可以通过变量`READLINES`控制要检查的行数以进行类型推断。默认值为 5555。这个数字越小，推断规则的偶然性越大。例如，在样本表（也用于 CSVKit 教程）中，列`fips`在前 916 行匹配模式`HMMSS`，所以我们可以推断时间为类型。从第 917 行开始，模式匹配中断，出现了像`31067`这样的值。要禁用基于文件的部分类型推断，只需更改`READLINES`。

```py
q) .csv.READLINES: count read0 `data.csv
```

### 基于 kdb+ 的单行命令

让我们将 q 解释器和`csvutil.q`的加载包装成一个简单的 shell 函数，以创建一个强大的命令行 CSV 处理工具。

```py
$ function qcsv { q -c 25 320 -s $(nproc --all) <<< 'system "l utils/csvutil.q";'"$1"; }
```

`-c 25 320`命令行参数修改了默认的25×80控制台大小，以便更好地显示宽表。`-s`开关为并行处理分配多个线程。我们将此值设置为计算机上的核心数量。如果你在 Mac 上工作，可以使用`$(sysctl -n hw.ncpu)`。你可以通过执行以下命令来验证设置：

```py
$ qcsv 'system "s"'
```

显示为`qcsv`分配的工作线程数量。

这个简单的包装器可以轻松实现`csvlook`、`csvcut`、`csvgrep`和`csvsort`的功能……甚至更多。例如，

```py
$ qcsv '.csv.read10 `data.csv'
```

模拟`csvlook`并整齐对齐`data.csv`的前10行。你可以将输出管道传输到`less -S`来处理宽表。

[![qcsv 模拟 csvlook](../Images/c6c9d702eb1d053d896faaedc4e2f83e.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/qmockscsvlook.png)

每当你想查看表的前几行或后几行时，使用[sublist](https://code.kx.com/q/ref/sublist/)函数。

```py
$ qcsv '20 sublist .csv.read `data.csv'
```

### 选择列、过滤、排序

一旦我们拥有 kdb+ 表格，我们可以使用 qSQL 的全部功能进行任何数据操作。要选择列

```py
$ qcsv 'select nsn,item_name from .csv.read `data.csv'
```

也许，你不喜欢将资源用于分析列，将其加载到内存中，然后丢弃它们的想法。好消息！我们有一个捷径。

在 kdb+ 中，函数`.csv.infoonly`接受列列表以限制列分析。我们可以将输出插入到通用的`.csv.read`中。

```py
$ qcsv '.csv.data[`data.csv; .csv.infoonly[`data.csv; `nsn`item_name]]'
```

再次使用qSQL，我们可以进一步过滤结果以选择匹配的行。

```py
$ qcsv 'select from .csv.read `data.csv where item_name like "RIFLE*", fips > 31100'
```

我们可以使用关键字`xasc`和`xdesc`来模拟`csvsort`。

```py
$ qcsv '`fips xdesc .csv.read `data.csv'
```

### 管道

Unix 系统的一个显著特性是管道：将一个命令的输出作为另一个命令的输入。CSVKit 也遵循这一原则。一旦 CSV 的内容转换为 kdb+ 表格，你可能希望停留在这个地方，因为 q 的强大功能提供了一个方便而强大的数据处理环境。

然而，在某些情况下，可能需要执行一个通过 STDIN 接受输入的黑箱脚本。我们的`qcsv`命令可以将 kdb+ 表格转换为所需的输出。例如，在下面的命令中，我们通过一个匿名函数处理输入 CSV，然后将输出发送到`blackboxcommand`。

```py
$ qcsv '-1 .h.cd { // do something here } .csv.read `data.csv;' | blackboxcommand
```

如果你想处理标准输入，记得在末尾加上分号。

如果你想在一个无法完全放入内存的表上运行 `csvsql` 查询，而查询包含一个可以用来减少表大小的 WHERE 子句，使用管道是最优雅的解决方案。例如，代替执行

```py
$ csvsql --query "select ... FROM data WHERE item_name like "RIFLE*" data.csv
```

构建一个大型内存数据库，你可以做

```py
$ csvgrep -c item_name -r "RIFLE*" data.csv | csvsql --query "select ... FROM STDIN"
```

不幸的是，`csvgrep`的过滤功能有限。你可以指定列和单个值或正则表达式。即使 ANSI SQL 也能表达更复杂的过滤。

库 `csvutil.q` 和 `csvguess.q` 提供了 q 语言在预过滤输入表时的全部功能。在批量加载函数 `POSTLOADEACH` 中，输入的 CSV 段将被处理，其输出将被附加到最终结果中。为了避免创建庞大的单行代码，我使用了 q 解释器并逐行执行所有语句。

```py
$ q
q) \l utils/csvutil.q
q) POSTLOADEACH: {x where x[`item_name] like "RIFLE*"};
q) DATA: ();
q) bulkload[`data.csv; .csv.info[`data.csv]];
q) select ... from DATA
```

在 q 函数中，你可以做任何事情：使用业务或实用函数，进行数学运算，调用外部服务器等……

在批量加载完成后执行函数 `POSTLOADALL`。这种后处理在 `csvguess.q` 生成的加载脚本中特别有用。例如，在 [纽约街道树数据](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35) 中，多个列（例如 `root_stone`）包含 `Yes/No` 值，你可以轻松地将其转换为布尔值。

```py
POSTLOADALL: {update `Yes=root_stone from x}
```

### 索引

xsv 的索引功能通过在 CSV 旁边创建扩展名为 `idx` 的二进制文件来加速查询。

```py
$ xsv index data.csv
```

CSVKit 推荐使用索引来加速 SQL 查询的方法是将 CSV 内容转移到 [SQLite](https://sqlite.org/) 数据库中，并使用其 CLI 创建索引和查询数据。这些操作都假设你有权限在计算机上创建文件。

```py
$ csvsql --db sqlite:///data.db --insert data.csv
$ sqlite3 data.db 'CREATE INDEX idx ON data(county)'
```

要获取格式良好的查询输出，请使用命令行选项 `-header` 和 `-csv`。

```py
$ sqlite3 -header -csv data.db 'SELECT county, COUNT(*) AS NR FROM data GROUP BY county;'
```

同样地，如果你将 CSV 转换为专有的 kdb+ 格式，你可以获得巨大的性能提升。如果你对经常查询的列添加索引，性能会进一步提升。你还可以在保存之前对表进行排序。 [排序属性](https://code.kx.com/q4m3/8_Tables/#881-sorted-s) 被附加到列上，多个操作的速度加快，例如，通过用二分查找替换线性查找。

在下面的命令中，我创建了 `data.csv` 的 kdb+ 等效表 `t`，位于目录 `db` 中，并通过属性 [应用索引](https://code.kx.com/q4m3/8_Tables/#88-attributes) 于 `county` 列，通过属性 [`g#`](https://github.com/BodonFerenc/blog/blob/master/csv) 实现。

```py
$ mkdir db
$ qcsv '`:db/t/ set .Q.en[hsym `db] update `g#county from .csv.read `data.csv'
```

你不需要在 q 解释器周围使用 `qcsv` 包装器来运行查询。

```py
$ q db <<< 'select nr: count i by county from t'
```

如果我们有有限的硬件资源且 CSV 无法载入内存，则可以使用支持批量加载和保存的 `csvguess.q`。

```py
$ q csvguess.q data.csv -savescript -exit
# overwrite POSTSAVEALL definition in data.load.q to
# POSTSAVEALL: {@[SAVEPATH[]; `county; `g#]}
$ q data.load.q -bulksave -savedb kdb -savename t -exit
```

注意，kdb+ 是一个列式数据库，每一列都有其自己的文件表示。当你运行查询 `select count i by county from t` 时，仅读取 `county` 列并需要资源。这使得你可以对无法完全载入内存的表执行查询，而 `csvsql` 将退出并显示错误消息。

比较 SQLite 和 kdb+ 超出了本文的范围，但在一个有 310 万行的表上进行的快速比较表明，它们在性能上存在巨大差异。

| 操作 | SQLite | kdb+ |
| --- | --- | --- |
| **导入 CSV 的执行时间** | 223 秒 | 6 秒 |
| **导入 CSV 的内存需求** | 5841 千字节 | 178 千字节 |
| **查询的执行时间** | 6693 毫秒 | 16 毫秒 |

### 特殊函数

**qSQL 是 ANSI SQL 的超集**。通过我们的单行命令 `qcsv`，我们可以表达 ANSI SQL 无法处理的复杂逻辑。此外，qSQL 只是 q 编程语言的一部分。q 的所有特性、库和函数都可以进一步处理 CSV 文件。这些功能包括向量操作、函数式编程、高级迭代器、日期/时间和字符串操作等。

Kdb+ 还有更多的技巧。我们可以加载我们在生产中使用的业务逻辑。**这就像使用我们数据库管理系统的存储过程来分析本地 CSV 文件一样。Kdb+ 提供了一个用于流处理、内存中的历史数据处理的单一解决方案，你还可以在你的临时数据分析中利用它。**

可能性并不止于此。除了加载现有脚本外，你还可以轻松连接到现有的 kdb+ 服务。例如，要在远程 kdb+ 服务器（如 `72.7.9.248:5001`）上调用 q 函数 `fn` 并传递 CSV 内容作为参数，你可以使用 [one-shot](https://code.kx.com/q/basics/ipc/#sync-request-get) TCP 请求。

```py
qcsv '`:72.7.9.248:5001 (`fn; .csv.read `data.csv)'
```

使用 kdb+，你可以完全灵活地将脚本和服务嵌入到单行命令中以处理 CSV 文件。简单而强大，对吧？

让我们考察三个领域，以了解在分析 CSV 文件时可以达到多远的深度。

### Pivot

数据透视表是数据分析中常用的功能。它通过将列值转换为新列来提供更紧凑的数据视图。新的视图允许并排检查相关值。这种技术常用于可视化多个分组的聚合结果。

我在最 [广泛使用的数据透视实现](https://code.kx.com/q/kb/pivoting-tables/#a-very-general-pivot-function-and-an-example) 周围添加了一个 [封装函数](https://github.com/BodonFerenc/kdbutils/blob/master/doc/md/pvt.md)，放在 `utils/pivot.q` 中。我们只需加载库 `pivot.q` 并添加 `.pvt.pivot` 前缀。它需要一个 [键控表](https://code.kx.com/q4m3/8_Tables/#84-primary-keys-and-keyed-tables)，通常是通过多个分组聚合的结果。数据透视列是最后一个键列。看看数据透视如何将一个狭窄、难以消化的表格转换为一个适合控制台的方形格式。

[![pivot](../Images/b3bfab149c1349f3c1f069a115abfa54.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/pivot.png)

q 语言允许你通过总计列和总计行轻松扩展数据透视表，前提是你愿意离开方便的选择语句世界并使用函数式形式。探索任何选择语句的函数等效形式超出了本文的范围。这里，我仅展示解决方案以演示其可行性。

[![pivot](../Images/08965b0cc529a99ef6e41f5316a84fff.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/pivotWithTotal.png)

注意，总计的中位数不能直接从原始数据透视表中得出。后台执行了三个额外的选择语句。

### 数组列

没有什么阻止你将值列表放入CSV的一个单元格中。你只需要使用除逗号之外的分隔符，例如空白或分号。与ANSI SQL不同，kdb+可以处理数组列。

我们之前已经使用函数[.h.cd](https://code.kx.com/q/ref/doth/#hcd-csv-from-data)将kdb+表转换为字符串列表，然后保存到CSV中。.h.cd按预期处理数组列。你可以通过[.h.d](https://code.kx.com/q/ref/doth/#hd-delimiter)设置次级分隔符。尽管q中的字符串是字符列表，`.h.cd`不会在字符之间插入次级分隔符。这是大多数用户期望的行为。

在读取CSV的数组列时，它将被存储为字符串列。kdb+函数[vs](https://code.kx.com/q/ref/vs/)（缩写为**v**ector来自**s**tring）通过分隔符字符串分割一个字符串。

```py
q)" " vs "10 31 -42"
"10"
"31"
"-42"
```

你可以通过整数转换，将字符串列表转换为整数列表，用`"I"$`表示。

与kdb+中的许多函数一样，`vs`可以与中缀或括号符号一起使用。

```py
q) vs[" "; "10 31 -42"]
```

Kdb+支持函数式编程。你可以使用`each`轻松地将单目函数应用于列表。这类似于Python的`map`函数。此外，你可以通过绑定参数从二元函数推导出单目函数。这称为[投影](https://code.kx.com/q4m3/6_Functions/#64-projection)。将这些结合起来，我们可以按如下方式按空格分割字符串列表

```py
q) vs[" "] each ("10 31 -42"; "104 105 107")
"10"  "31"  "-42"
"104" "105" "107"
```

或者更优雅地使用Each迭代器（用`'`表示），如果分隔符是单个字符的话。

```py
q) " " vs' ("10 31 -42"; "104 105 107")
```

一个表列可以是一个字符串列表。假设列`A`包含用空白分隔的整数。函数`.csv.read`返回一个字符串列，我们可以轻松将其转换为数组列。

```py
q) update "I"$" " vs' A from .csv.read `data.csv
```

为了展示q语言的强大功能，假设`data.csv`中有另一列数组称为`IDX`，其中包含索引。对于每一行，我们需要计算数组列`A`在`IDX`指定的索引处的和。让我稍微探讨一下索引操作。

在q中，你可以像在其他编程语言中一样索引一个列表。

```py
q) list: 4 1 6 3    // this is an integer list
q) list[2]
6
```

Q是一个向量语言。许多运算符不仅接受标量，还接受列表。索引就是这样的一个操作。

```py
q) list[2 1]
6 1
```

方括号只是语法糖。你可以改为使用`@`运算符和中缀符号。

```py
q) list @ 2 1
6 1
```

当将列表的列表作为`@`运算符的两个参数传递时，我们需要再次使用Each迭代器。将所有内容汇总起来，我们通过

```py
$ qcsv $'
t:update "I"$" " vs\' A, "I"$" " vs\' IDX from .csv.read `data.csv;
update sum_A_of: sum each A@\'IDX from t'
```

[![数组列上的操作](../Images/2a9e1b662e317eb6a1c5f55261e9c172.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/array.png)

我将表达式拆分成多行以提高可读性，但从shell的角度来看，这仍然是一个单一的命令。

Each迭代器（`'`）需要转义，我们需要使用ANSI-C引用，因此在开头的引号前面有`$`。

### 连接

连接两个CSV文件已经被Linux命令`join`支持。命令`csvjoin`进一步支持所有类型的SQL连接：内连接、左连接、右连接和外连接。Q也支持这些经典的连接操作。

对于时间序列，另一个常用的连接类型是[asof](https://code.kx.com/q/ref/asof/)及其泛化形式[window join](https://code.kx.com/q/ref/wj/)。如果你有两个数据流且时间不同，则asof连接可以合并这两个数据流。

让我展示一下在实际场景中如何使用window join来分析分布式进程。我们的主进程向worker进程发送请求。每个请求会产生多个任务。我们存储请求的`start`和`end`时间以及任务的`start`时间和`duration`。我们希望看到worker在每个请求上花费的时间比例。由于网络延迟，任务的开始时间在请求的开始时间之后。以下是主进程数据的一个示例。

| requestID | workerID | start | end |
| --- | --- | --- | --- |
| RQ1 | SL1 | 12:31 | 12:52 |
| RQ2 | SL2 | 12:31 | 12:50 |
| RQ3 | SL1 | 12:54 | 12:59 |
| RQ4 | SL3 | 12:51 | 13:00 |
| RQ5 | SL1 | 13:10 | 13:13 |

合并后的worker数据是

| workerID | taskID | start | duration |
| --- | --- | --- | --- |
| SL1 | 1 | 12:32 | 1 |
| SL1 | 2 | 12:35 | 2 |
| SL1 | 3 | 12:37 | 10 |
| SL2 | 1 | 12:31 | 17 |
| SL1 | 4 | 12:55 | 1 |
| SL1 | 5 | 12:56 | 3 |
| SL3 | 1 | 12:52 | 3 |
| SL3 | 2 | 12:58 | 1 |
| SL1 | 6 | 13:10 | 2 |

函数`wj1`帮助找到具有给定workerID值且落在主表的`start`和`end`列指定的时间窗口内的任务。

```py
q) m: .csv.read `main.csv
q) s: .csv.read `worker.csv
q) wj1[(m.start;m.end); `workerID`start; m; (`workerID`start xasc s; (::; `taskID))]
requestID workerID start end   taskID
------------------------------------
1         sl1     12:31 12:52 1 2 3h
2         sl2     12:31 12:50 ,1h
3         sl1     12:54 12:59 4 5h
4         sl3     12:51 13:00 1 2h
5         sl1     13:10 13:13 ,6h 
```

列`taskID`中整数列表末尾的字符`h`表示*短整型*，即一个整数占用两个字节。函数`.csv.info`尝试节省内存，并使用占用最少空间的整数和浮点数表示，同时保留所有信息。

为了获取比例，我们需要处理经过的时间。

```py
q) select requestID, rate: duration % end-start from
     wj1[(m.start;m.end); `workerID`start; m;
       (`workerID`start xasc s; (sum; `duration))] 
```

[![高级连接操作](../Images/ff61bd59b9bb359068bee0590460c79a.png)](https://github.com/BodonFerenc/blog/blob/master/csv/pic/windowjoin.png)

### 性能

让我们比较在公开的CSV上执行简单聚合的性能，该CSV用于基准测试`xsv`包。该145 MByte的文件包含近3200万行。

```py
$ curl -LO https://burntsushi.net/stuff/worldcitiespop.csv
```

我们希望查看按人口排序的前10个区域。我们需要汇总每个区域城市的人口。包xsv不支持聚合，因此不适用。

q查询非常简单。

```py
$ qcsv '10 sublist `Population xdesc select sum Population by Region from .csv.basicread `worldcitiespop.csv'
```

聚合操作只需要`Population`和`Region`列，因此我们可以通过省略未使用列的类型转换来加快查询速度。

```py
$ qcsv '10 sublist `Population xdesc select sum Population by Region from .csv.data[`worldcitiespop.csv; .csv.infoonly[`worldcitiespop.csv; `Population`Region]]
```

`csvsql`非常相似

```py
$ csvsql --query "select Region, SUM(Population) AS Population FROM worldcitiespop GROUP BY Region ORDER BY Population DESC LIMIT 10" worldcitiespop.csv
```

我们还可以通过命令行选项`--no-inference --snifflimit 0`禁用类型推断和方言嗅探。这将使命令执行速度提高一倍。

还有其他几个开源工具可以在CSV文件上运行SQL语句。我还评估了两个用Go编写的包。

```py
$ textql -header -sql "select Region, SUM(Population) AS Population FROM worldcitiespop GROUP BY Region ORDER BY Population DESC LIMIT 10" worldcitiespop.csv
```

下面显示了以秒为单位的运行时间。第二行对应于对同一 CSV 文件进行五次重复的实验。

| CSVKit | textql | csvq | kdb+ |
| --- | --- | --- | --- |
| 116 | 23 | 13 | 2 |
| OUT OF MEM | 102 | OUT OF MEM | 6 |

以下表格包含了以千字节为单位的内存需求

| CSVKit | textql | csvq | kdb+ |
| --- | --- | --- | --- |
| 5224 | 216 | 4343 | 172 |

kdb+ 以其惊人的速度而著称。基准测试通常关注数据是否驻留在内存中或使用其专有格式存储在磁盘上。CSV 支持是该语言的一个有用功能。然而，极致的优化、对向量操作的支持以及固有的并行化都得到了回报：kdb+ 在 CSV 分析工具中表现显著优越。执行速度直接转化为生产力。如果查询返回时间从2秒变成近两分钟，这会让你付出多少代价？如果开发人员在等待几分钟的过程中频繁中断，他们会怎么做？

选择工具时，支持也是一个关键因素。维护成本不断增加的个人项目和其他非营利解决方案很容易被抛弃。q/kdb+ 的供应商，[Kx Systems](https://kx.com/)，成立于1993年，提供了稳定的背景、专业的支持渠道和[高质量文档](https://code.kx.com/q/)供 kdb+ 开发人员使用。

测试在 `n1-standard-4` GCP 虚拟机上运行。kdb+ 解决方案的运行时间在更多核心的机器上会进一步下降，因为 kdb+ 4.0 利用 [多线程原语](https://code.kx.com/q/kb/mt-primitives/)。

### 结论

处理 CSV 文件的库有很多，每个库都有其存在的理由。kdb+ 拥有一个出色的开源库 `csvutil.q`/`csvguess.q`，具有复杂的类型推断引擎。一旦将 CSV 转换为 kdb+ 内存表，你可以轻松处理其他工具仅能困难应对的问题 - 如果它们能处理的话。你可以以可读的方式表达复杂的逻辑，这种方式易于维护，只需将加载库的 q 解释器包装到一个 shell 函数中即可。该解决方案比替代方法[更环保](https://kx.com/blog/kdb-enables-green-computing/)，因为它执行速度更快，占用的内存更少。

**致谢**

我想对[Stephen Taylor](https://www.linkedin.com/in/stephen-taylor-b5ba78/)、[Tim Thornton](https://www.linkedin.com/in/tthornton6/)、[Rebecca Kelly](https://www.linkedin.com/in/kellyr13/) 和 [Rian O'Cuinneagain](https://github.com/rianoc) 表达我的感激，他们提供了宝贵的反馈，并大大改善了内容。

**个人简介：[Ferenc Bodon 博士](https://www.linkedin.com/in/ferencbodon/)** 是一位经验丰富的数据工程师、软件开发人员、多语言程序员、具有数据挖掘和统计学学术背景的软件架构师。他具有长远的思维，并始终致力于寻找顶级质量、稳健的解决方案，这些方案具有可扩展性并允许快速开发。他相信软件质量，直到找到“荣耀解决方案”才能放松。

[原文](https://github.com/BodonFerenc/blog/blob/master/csv/csv.md)。经许可转载。

**相关内容：**

+   [Python用于数据分析……真的这么简单吗？！?](/2020/04/python-data-analysis-really-that-simple.html)

+   [10个Python字符串处理技巧与窍门](/2020/01/python-string-processing-primer.html)

+   [R中的文本处理](/2018/03/text-processing-r.html)

### 更多相关主题

+   [3种处理CSV文件的Python方法](https://www.kdnuggets.com/2022/10/3-ways-process-csv-files-python.html)

+   [从CSV到完整的分析报告，使用ChatGPT的5个简单步骤](https://www.kdnuggets.com/from-csv-to-complete-analytical-report-with-chatgpt-in-5-simple-steps)

+   [LLaMA 3：Meta最强大的开源模型](https://www.kdnuggets.com/llama-3-metas-most-powerful-open-source-model-yet)

+   [自然语言处理关键术语解释](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)

+   [Python 字符串处理备忘单](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)
