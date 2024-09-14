# 如何在 Bash 中管理文件和目录

> 原文：[https://www.kdnuggets.com/how-to-manage-files-and-directories-in-bash](https://www.kdnuggets.com/how-to-manage-files-and-directories-in-bash)

![如何在 Bash 中管理文件和目录](../Images/f590d131e1aa939d9aa76bdb9e94d636.png)

图片由作者提供 | Midjourney & Canva

## 了解 Bash Shell

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

Bash，即Bourne-Again Shell，是一种命令行解释器，允许用户通过输入命令与操作系统交互。它通常用于Linux和macOS等基于Unix的系统，并提供了管理文件和目录的各种工具。

要开始使用bash，你需要打开终端：

+   在**Linux**上，在应用程序菜单中查找终端应用程序。

+   在**macOS**上，使用Spotlight搜索（Cmd + Space）并输入“Terminal”。

+   在**Windows**上，你可以使用Git Bash或Windows子系统（WSL）。

一旦你打开终端并可以使用，我们就准备好学习如何使用bash管理文件和目录。我们从一些基本的导航命令开始，然后转到管理目录和文件。

## `pwd` - 打印工作目录

`pwd`命令显示你当前所在的目录。这对于确认你在文件系统中的位置非常有用。

```py
pwd
```

## `ls` - 列出目录内容

`ls`命令列出当前目录中的文件和目录。你可以添加选项，如`-l`以获取详细信息，或`-a`以包括隐藏文件。

```py
ls
ls -l
ls -a
```

## `mkdir` - 创建目录

语法：`mkdir <directory_name>`

示例：创建一个名为`data`的目录

```py
mkdir data
```

你可以一次创建多个目录：

```py
mkdir dir1 dir2 dir3
```

要创建嵌套目录，请使用`-p`选项：

```py
mkdir -p parent/child/grandchild
```

## `rmdir` - 删除目录

语法：`rmdir <directory_name>`

示例：删除一个名为`data`的空目录：

```py
rmdir data
```

请注意，`rmdir`仅对空目录有效。要删除非空目录，请使用`rm -r`。

## `cp` - 复制文件和目录

语法：`cp <source> <destination>`

示例：将名为`file.txt`的文件复制到`backup`目录中：

```py
cp file.txt backup/
```

要复制多个文件：

```py
cp file1.txt file2.txt backup/
```

要复制目录，请使用`-r`（递归）选项：

```py
cp -r dir1 backup/
```

## `mv` - 移动/重命名文件和目录

语法：`mv <source> <destination>`

示例：将名为`file.txt`的文件移动到`backup`目录中：

```py
mv file.txt backup/
```

将`file.txt`重命名为`file_backup.txt`：

```py
mv file.txt file_backup.txt
```

`mv`命令可以移动文件/目录并重命名它们。

## `rm` - 删除文件和目录

语法：`rm <file_name>`

示例：删除名为 `file.txt` 的文件：

```py
rm file.txt
```

要删除目录及其内容，请使用 `-r`（递归）选项：

```py
rm -r dir1
```

若要强制删除而不提示，请添加 `-f`（强制）选项：

```py
rm -rf dir1
```

## 数据科学家的实用示例

**创建项目目录结构**

示例：为数据科学项目创建目录

```py
mkdir -p project/{data,scripts,results}
```

**组织数据文件**

示例：将所有 `.csv` 文件移动到 `data` 目录

```py
mv *.csv data/
```

**清理不必要的文件**

示例：删除所有 `.tmp` 文件

```py
rm *.tmp
```

## 组合命令

**使用 `&&` 链接命令**

示例：在一个命令中创建目录并移动文件

```py
mkdir backup && mv *.csv backup/
```

**使用分号顺序执行**

示例：列出内容然后删除文件

```py
ls; rm file.txt
```

## 提示和最佳实践

**使用 `rm` 的安全性**

使用 `rm` 前务必双重检查路径，以避免意外删除。

**使用通配符**

通配符如 `*` 可以匹配多个文件，使命令更高效。例如，`*.csv` 匹配所有 CSV 文件。

**备份重要文件**

在执行大规模操作之前，创建备份以防数据丢失。

## 快速参考

这里是一个快速参考摘要表，总结了 `cp`、`mv`、`rm` 和 `mkdir` 的语法和用法。

| 命令 | 语法 | 描述 |
| --- | --- | --- |
| pwd | pwd | 打印工作目录 |
| ls | ls | 列出目录内容 |
| mkdir | mkdir <directory_name> | 创建新目录 |
| rmdir | rmdir <directory_name> | 删除空目录 |
| cp | cp <source> <destination> | 复制文件或目录 |
| mv | mv <source> <destination> | 移动或重命名文件或目录 |
| rm | rm <file_name> | 删除文件或目录 |

[](https://www.linkedin.com/in/mattmayo13/)****[马修·梅奥](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/)和[Statology](https://www.statology.org/)的主编，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的贡献编辑，马修旨在使复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他的使命是使数据科学社区的知识民主化。马修从6岁开始编程。

### 更多相关内容

+   [5 个真正有用的数据科学 Bash 脚本](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [如何使用 Bash 导航文件系统](https://www.kdnuggets.com/how-navigate-filesystem-bash)

+   [用 Bash 构建你的第一个 ETL 管道](https://www.kdnuggets.com/building-your-first-etl-pipeline-with-bash)

+   [准备好利用网络安全硕士学位管理威胁…](https://www.kdnuggets.com/2022/07/baypath-prepared-manage-threat-ms-cybersecurity.html)

+   [准备好通过…的网络安全硕士学位来应对威胁](https://www.kdnuggets.com/2022/12/baypath-prepared-manage-threat-ms-cybersecurity.html)

+   [管理太多的 Python 版本？Pyenv 来救援](https://www.kdnuggets.com/too-many-python-versions-to-manage-pyenv-to-the-rescue)
