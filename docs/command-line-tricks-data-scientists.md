# 数据科学家的命令行技巧

> 原文：[`www.kdnuggets.com/2018/06/command-line-tricks-data-scientists.html`](https://www.kdnuggets.com/2018/06/command-line-tricks-data-scientists.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者 [Kade Killary](http://kadekillary.work/)，数据科学家与工程师**

![图片](img/41bfa054c7f477d9be3f2d3a5bcb3cee.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

对于许多数据科学家来说，数据处理开始和结束于 Pandas 或 Tidyverse。理论上，这种观念没有错。毕竟，这些工具存在的初衷就是如此。然而，这些选项对于像分隔符转换这样简单的任务来说，往往会显得过于复杂。

每个开发者，尤其是数据科学家，都应该把掌握命令行作为目标。学习你终端的来龙去脉无疑会让你更高效。除此之外，命令行还为计算机科学提供了极好的历史课程。例如，awk——一种基于数据的脚本语言。Awk 最早出现在 1977 年，得到了 [布赖恩·柯宁汉](https://en.wikipedia.org/wiki/Brian_Kernighan) 的帮助，他是传奇的 [K&R 书籍](https://en.wikipedia.org/wiki/The_C_Programming_Language) 中的 *K*。直到今天，近 50 年后，awk 依然相关，每年都有 [新书](https://www.amazon.com/Learning-AWK-Programming-cutting-edge-text-processing-ebook/dp/B07BT98HDS) 继续出版！因此，可以放心地认为，对命令行技巧的投资不会很快贬值。

### 我们将涵盖的内容

+   ICONV

+   HEAD

+   TR

+   WC

+   SPLIT

+   SORT & UNIQ

+   CUT

+   PASTE

+   JOIN

+   GREP

+   SED

+   AWK

### ICONV

文件编码可能很棘手。现在大多数文件都是 UTF-8 编码的。要了解 UTF-8 背后的部分魔力，可以查看这个 [精彩视频](https://www.youtube.com/watch?v=MijmeoH9LT4)。尽管如此，有时我们会收到不是这种格式的文件。这可能会导致一些奇怪的尝试去更换编码方案。在这里，`iconv` 是救星。Iconv 是一个简单的程序，可以将一种编码的文本转换为另一种编码的文本。

```py
# Converting -f (from) latin1 (ISO-8859-1)
# -t (to) standard UTF_8
iconv -f ISO-8859-1 -t UTF-8 < input.txt > output.txt
```

有用的选项：

+   `iconv -l` 列出所有已知编码

+   `iconv -c` 静默丢弃无法转换的字符

### HEAD

如果你是频繁使用 Pandas 的用户，那么`head`将是熟悉的。通常在处理新数据时，我们首先想要了解数据的内容。这导致我们启动 Pandas，读取数据，然后调用`df.head()`——可以说是相当费劲。`head`在没有任何标志的情况下，会打印出文件的前 10 行。`head`的真正力量在于测试清理操作。例如，如果我们想将文件的分隔符从逗号更改为管道字符。一个快速的测试方法是：`head mydata.csv | sed 's/,/|/g'`。

```py
# Prints out first 10 lines

head filename.csv

# Print first 3 lines

head -n 3 filename.csv
```

有用的选项：

+   `head -n` 打印特定行数

+   `head -c` 打印特定字节数

### TR

`tr`类似于翻译。这个强大的工具在基本文件清理中是一个主力军。一个理想的用例是用于在文件中交换分隔符。

```py
# Converting a tab delimited file into commas

cat tab_delimited.txt | tr "\\t" "," comma_delimited.csv
```

`tr`的另一个特性是所有内置的`[:class:]`变量。这些包括：

```py
[:alnum:] all letters and digits
[:alpha:] all letters
[:blank:] all horizontal whitespace
[:cntrl:] all control characters
[:digit:] all digits
[:graph:] all printable characters, not including space
[:lower:] all lower case letters
[:print:] all printable characters, including space
[:punct:] all punctuation characters
[:space:] all horizontal or vertical whitespace
[:upper:] all upper case letters
[:xdigit:] all hexadecimal digits
```

你可以将各种这些选项链在一起，组成强大的程序。以下是一个基本的单词计数程序，你可以用来检查你的 README 文件是否过度使用。

```py
cat README.md | tr "[:punct:][:space:]" "\n" | tr "[:upper:]" "[:lower:]" | grep . | sort | uniq -c | sort -nr
```

另一个使用基本正则表达式的示例：

```py
# Converting all upper case letters to lower case

cat filename.csv | tr '[A-Z]' '[a-z]'
```

有用的选项：

+   `tr -d` 删除字符

+   `tr -s` 压缩字符

+   `\b` 退格键

+   `\f` 换页符

+   `\v` 垂直制表符

+   `\NNN` 具有八进制值 NNN 的字符

### WC

单词计数。其值主要由`-l`标志派生，该标志会给出行数。

```py
# Will return number of lines in CSV

wc -l gigantic_comma.csv
```

这个工具很方便用来确认各种命令的输出。因此，如果我们将文件中的分隔符转换，然后运行`wc -l`，我们可以期望总行数保持不变。如果不一致，那么我们知道出现了问题。

有用的选项：

+   `wc -c` 打印字节计数

+   `wc -m` 打印字符计数

+   `wc -L` 打印最长行的长度

+   `wc -w` 打印单词计数

### SPLIT

文件大小可以有很大差异。根据任务的不同，拆分文件可能会有好处——因此使用`split`。`split`的基本语法是：

```py
# We will split our CSV into new_filename every 500 line

split -l 500 filename.csv new_filename_

# filename.csv
# ls output
# new_filename_aaa
# new_filename_aab
# new_filename_aac
```

两个怪癖是命名约定和缺乏文件扩展名。后缀约定可以通过`-d`标志设置为数字。要添加文件扩展名，你需要运行以下`find`命令。它会通过追加`.csv`来更改当前目录下*所有*文件的名称，因此要小心。

```py
find . -type f -exec mv '{}' '{}'.csv \;

# ls output
# filename.csv.csv
# new_filename_aaa.csv
# new_filename_aab.csv
# new_filename_aac.csv
```

有用的选项：

+   `split -b` 按特定字节大小拆分

+   `split -a` 生成长度为 N 的后缀

+   `split -x` 使用十六进制后缀进行拆分

### SORT & UNIQ

前面的命令很直观：它们执行的正是它们所描述的功能。这两个命令配合使用效果最佳（即唯一词汇计数）。这是因为`uniq`只作用于相邻的重复行。因此，原因在于在将输出通过管道传输之前需要`sort`。一个有趣的说明是，`sort -u`将实现与典型的`sort file.txt | uniq`模式相同的结果。

`sort`对数据科学家有一个非常有用的功能：根据特定列对整个 CSV 文件进行排序。

```py
# Sorting a CSV file by the second column alphabetically

sort -t, -k2 filename.csv

# Numerically

sort -t, -k2n filename.csv

# Reverse order

sort -t, -k2nr filename.csv
```

这里的 `-t` 选项用于指定逗号作为分隔符。更常见的是假定使用空格或制表符。此外，`-k` 标志用于指定键。

有用的选项：

+   `sort -f` 忽略大小写。

+   `sort -r` 反向排序。

+   `sort -R` 打乱顺序。

+   `uniq -c` 计算出现次数。

+   `uniq -d` 仅打印重复的行。

### CUT

Cut 用于删除列。例如，如果我们只想要第一列和第三列。

```py
cut -d, -f 1,3 filename.csv
```

选择除第一列之外的所有列。

```py
cut -d, -f 2- filename.csv
```

结合其他命令，`cut` 作为一个过滤器。

```py
# Print first 10 lines of column 1 and 3, where "some_string_value" is present

head filename.csv | grep "some_string_value" | cut -d, -f 1,3
```

找出第二列中的唯一值数量。

```py
cat filename.csv | cut -d, -f 2 | sort | uniq | wc -l

# Count occurences of unique values, limiting to first 10 results

cat filename.csv | cut -d, -f 2 | sort | uniq -c | head
```

### PASTE

Paste 是一个具有有趣功能的小众命令。如果你有两个需要合并的文件，并且它们已经排序，`paste` 能满足你的需求。

```py
# names.txt
adam
john
zach

# jobs.txt
lawyer
youtuber
developer

# Join the two into a CSV

paste -d ',' names.txt jobs.txt > person_data.txt

# Output
adam,lawyer
john,youtuber
zach,developer
```

对于更类似 SQL 的变体，请参见下面的内容。

### JOIN

Join 是一种简单的、类 SQL 的操作。最大区别在于 `join` 会返回所有列，匹配只能在一个字段上进行。默认情况下，`join` 会尝试使用第一列作为匹配键。要获得不同的结果，需要使用以下语法：

```py
# Join the first file (-1) by the second column
# and the second file (-2) by the first

join -t, -1 2 -2 1 first_file.txt second_file.txt
```

标准的 join 是内连接。然而，通过 `-a` 标志也可以进行外连接。另一个值得注意的特点是 `-e` 标志，可以在找到缺失字段时替换一个值。

```py
# Outer join, replace blanks with NULL in columns 1 and 2
# -o which fields to substitute - 0 is key, 1.1 is first column, etc...

join -t, -1 2 -a 1 -a2 -e ' NULL' -o '0,1.1,2.2' first_file.txt second_file.txt
```

不是最用户友好的命令，但在紧急情况下，采取紧急措施。

有用的选项：

+   `join -a` 打印无法配对的行。

+   `join -e` 替换缺失的输入字段。

+   `join -j` 等同于 `-1 FIELD -2 FIELD`。

### GREP

全局搜索正则表达式并打印，或 `grep`；可能是最知名的命令，也是有充分理由的。Grep 功能强大，特别是在大型代码库中查找内容时。在数据科学领域，它作为其他命令的精炼机制。虽然它的标准用法也很有价值。

```py
# Recursively search and list all files in directory containing 'word'

grep -lr 'word' .

# List number of files containing word

grep -lr 'word' . | wc -l
```

计算包含词语/模式的总行数。

```py
grep -c 'some_value' filename.csv

# Same thing, but in all files in current directory by file name

grep -c 'some_value' *
```

使用或操作符 `\|` 为多个值执行 grep。

```py
grep "first_value\|second_value" filename.csv
```

有用的选项：

+   `alias grep="grep --color=auto"` 使 grep 显示彩色。

+   `grep -E` 使用扩展正则表达式。

+   `grep -w` 仅匹配完整单词。

+   `grep -l` 打印包含匹配项的文件名。

+   `grep -v` 反向匹配。

### 大招

Sed 和 Awk 是本文中最强大的两个命令。为了简洁，我不会详细讨论它们。相反，我将介绍各种证明其强大能力的命令。如果你想了解更多，[有一本书](https://www.amazon.com/sed-awk-Dale-Dougherty/dp/1565922255/ref=sr_1_1?ie=UTF8&qid=1524381457&sr=8-1&keywords=sed+and+awk)专门讲解这些内容。

### SED

从本质上讲，`sed` 是一个逐行操作的流编辑器。它擅长于替换，但也可以用于全面的重构。

最基本的 `sed` 命令是 `s/old/new/g`。这表示搜索旧值，将所有出现的旧值替换为新值。如果没有 `/g`，我们的命令将在行中第一次出现后终止。

要快速体验这种强大功能，我们来看看一个例子。在这个场景中，你会得到以下文件：

```py
balance,name
$1,000,john
$2,000,jack
```

我们首先可能要做的是移除美元符号。`-i`标志表示就地修改。`''`表示零长度的文件扩展名，从而覆盖我们的初始文件。理想情况下，你应当逐一测试这些命令，然后输出到新文件。

```py
sed -i '' 's/\$//g' data.txt

# balance,name
# 1,000,john
# 2,000,jack
```

接下来是我们`balance`列值中的逗号。

```py
sed -i '' 's/\([0-9]\),\([0-9]\)/\1\2/g' data.txt

# balance,name
# 1000,john
# 2000,jack
```

最后，Jack 决定有一天辞职了。所以，再见，我的朋友。

```py
sed -i '' '/jack/d' data.txt

# balance,name
# 1000,john
```

如你所见，`sed`确实非常强大，但乐趣并不止于此。

### AWK

最后的最好。Awk 不仅仅是一个简单的命令：它是一种全面的语言。在本文涵盖的一切中，`awk`无疑是最酷的。如果你感到印象深刻，还有很多很棒的资源 - 见[这里](https://www.amazon.com/AWK-Programming-Language-Alfred-Aho/dp/020107981X/ref=sr_1_1?ie=UTF8&qid=1524388936&sr=8-1&keywords=awk)、[这里](http://www.grymoire.com/Unix/Awk.html)和[这里](https://www.tutorialspoint.com/awk/index.htm)。

`awk`的常见用例包括：

+   文本处理

+   格式化文本报告

+   执行算术操作

+   执行字符串操作

Awk 可以在最初形态上与`grep`并驾齐驱。

```py
awk '/word/' filename.csv
```

或者通过一些魔法结合`grep`和`cut`。这里，`awk`打印第三和第四列，以制表符分隔，针对所有包含我们*word*的行。`-F,`只是将分隔符改为逗号。

```py
awk -F, '/word/ { print $3 "\t" $4 }' filename.csv
```

Awk 内置了许多有用的变量。例如，`NF` - 字段数 - 和`NR` - 记录数。要获取文件中的第 53 条记录：

```py
awk -F, 'NR == 53' filename.csv
```

一个额外的功能是基于一个或多个值进行筛选。下面的第一个例子，将打印记录的行号和列，对于第一列等于*string*的记录。

```py
awk -F, ' $1 == "string" { print NR, $0 } ' filename.csv

# Filter based off of numerical value in second column

awk -F, ' $2 == 1000 { print NR, $0 } ' filename.csv
```

多个数值表达式：

```py
# Print line number and columns where column three greater
# than 2005 and column five less than one thousand

awk -F, ' $3 >= 2005 && $5 <= 1000 { print NR, $0 } ' filename.csv
```

求第三列的和：

```py
awk -F, '{ x+=$3 } END { print x }' filename.csv
```

对于第一列等于“something”的值，第三列的总和。

```py
awk -F, '$1 == "something" { x+=$3 } END { print x }' filename.csv
```

获取文件的尺寸：

```py
awk -F, 'END { print NF, NR }' filename.csv

# Prettier version

awk -F, 'BEGIN { print "COLUMNS", "ROWS" }; END { print NF, NR }' filename.csv
```

打印出现两次的行：

```py
awk -F, '++seen[$0] == 2' filename.csv
```

移除重复行：

```py
# Consecutive lines
awk 'a !~ $0; {a=$0}']

# Nonconsecutive lines
awk '! a[$0]++' filename.csv

# More efficient
awk '!($0 in a) {a[$0];print}
```

使用内置函数`gsub()`替换多个值。

```py
awk '{gsub(/scarlet|ruby|puce/, "red"); print}'
```

这个`awk`命令会合并多个 CSV 文件，忽略头部，然后在末尾追加。

```py
awk 'FNR==1 && NR!=1{next;}{print}' *.csv > final_file.csv
```

需要缩小一个庞大的文件？嗯，`awk`可以在`sed`的帮助下处理。具体来说，这条命令会根据行数将一个大文件拆分成多个小文件。这条单行命令也会添加一个扩展名。

```py
sed '1d;$d' filename.csv | awk 'NR%NUMBER_OF_LINES==1{x="filename-"++i".csv";}{print > x}'

# Example: splitting big_data.csv into data_(n).csv every 100,000 lines

sed '1d;$d' big_data.csv | awk 'NR%100000==1{x="data_"++i".csv";}{print > x}'
```

### 结束

命令行具有无尽的力量。本文所涵盖的命令足以让你迅速从新手成长为高手。除了这些，还有许多实用工具可供考虑，以应对日常数据操作。[Csvkit](http://csvkit.readthedocs.io/en/1.0.3/)、[xsv](https://github.com/BurntSushi/xsv) 和 [q](https://github.com/harelba/q) 是值得注意的三个。如果你希望进一步深入探讨命令行数据科学，那么不妨看看 [这本书](https://www.amazon.com/Data-Science-Command-Line-Time-Tested/dp/1491947853/ref=sr_1_1?ie=UTF8&qid=1524390894&sr=8-1&keywords=data+science+at+the+command+line)。这本书也可以在线 [免费阅读](https://www.datascienceatthecommandline.com/)！

### [更多内容请见我的博客！](http://kadekillary.work/post/statusline/)

### 链接

+   [Sed 一行命令](http://sed.sourceforge.net/sed1line.txt)

+   [Awk 一行命令](http://www.catonmat.net/blog/wp-content/uploads/2008/09/awk1line.txt)

+   [命令行中的 CSV 处理](http://bconnelly.net/working-with-csvs-on-the-command-line/)

+   [附加文件扩展名](https://stackoverflow.com/questions/1108527/recursively-add-file-extension-to-all-files)

+   [我最好的 AWK 技巧](http://blog.jpalardy.com/posts/my-best-awk-tricks/)

+   [生物信息学一行命令](https://github.com/stephenturner/oneliners)

+   [UNIX 学校](http://www.theunixschool.com/)

+   [Sed — 介绍与教程](http://www.grymoire.com/Unix/Sed.html)

**简介：[Kade Killary](http://kadekillary.work/)** 是 XMedia 的数据科学家和工程师。

[原文](https://medium.com/@kadek/command-line-tricks-for-data-scientists-c98e0abe5da)。经许可转载。

**相关：**

+   数据科学家的 12 大必备命令行工具

+   命令行中的数据科学：探索数据

+   使用 Python 掌握数据准备的 7 个步骤

### 更多相关主题

+   [命令行中的数据科学：免费电子书](https://www.kdnuggets.com/2022/03/data-science-command-line-free-ebook.html)

+   [数据科学的 5 个命令行工具](https://www.kdnuggets.com/2023/03/5-command-line-tools-data-science.html)

+   [ChatGPT CLI：将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)

+   [通过这个 GitHub 仓库掌握命令行的艺术](https://www.kdnuggets.com/master-the-art-of-command-line-with-this-github-repository)

+   [用 Python 在 7 个简单步骤中构建命令行应用](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

+   [数据科学家的 10 个 Jupyter Notebook 技巧与窍门](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)
