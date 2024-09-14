# 集成 Python 和 R，第 2 部分：从 Python 执行 R 及反之亦然

> 原文：[https://www.kdnuggets.com/2015/10/integrating-python-r-executing-part2.html](https://www.kdnuggets.com/2015/10/integrating-python-r-executing-part2.html)

**作者：Chris Musselle ([Mango Solutions](http://www.mango-solutions.com))**。

在[上一篇文章](http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-into-a-data-analysis-pipeline-part-1/)中，我们讨论了为什么你可能想将 R 和 Python 集成到一个管道中，以及如何通过使用平面文件隔离来实现这一点。在此过程中，我们介绍了如何从命令行运行 Python 或 R 脚本，以及如何访问传入的任何附加参数。在这篇文章中，我们通过展示如何将两个脚本链接在一起，完成集成过程，方法是让 R 调用 Python，反之亦然。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

![](../Images/0eb09d69ebb8a6b50ef17b89287ac9a4.png)

### 命令行执行和执行子进程

为了更好地理解子进程执行时发生的情况，值得详细回顾在命令行上执行 Python 或 R 进程时发生的事情。当运行以下命令时，会启动一个新的 Python 进程来执行脚本。

`python path/to/myscript.py arg1 arg2 arg3`

在执行过程中，任何输出打印到[标准输出和标准错误流](https://en.wikipedia.org/wiki/Standard_streams)的内容都会显示在控制台上。实现这一点最常见的方法是通过内置函数（Python 中的`print()`和 R 中的`cat()`或`print()`），这些函数将给定的字符串写入`stdout`流。Python 进程将在脚本执行完成后关闭。

以这种方式运行命令行脚本是有用的，但如果有多个顺序但独立的脚本需要以这种方式执行，则可能变得繁琐且容易出错。然而，Python 或 R 进程可以以类似于上述命令行方法的方式直接执行另一个进程。这是有益的，因为它允许，例如，父 Python 进程启动子 R 进程来运行特定的分析脚本。一旦 R 脚本完成，这个子 R 进程的输出可以传递回父 Python 进程，而不是打印到控制台。使用这种方法可以消除在命令行上手动逐步执行的需要。

### 示例

为了说明一个进程如何执行另一个进程，我们将使用两个简单的示例：一个是 Python 调用 R，另一个是 R 调用 Python。每个案例中执行的分析故意简化，以便集中于实现这种调用机制的过程。

#### 示例 R 脚本

我们的简单示例 R 脚本将从命令行接收一系列数字并返回最大值。

`# max.R`

`# 获取命令行参数 myArgs <- commandArgs(trailingOnly = TRUE)`

`# 转换为数字 nums = as.numeric(myArgs)`

`# cat 将结果写入 stdout 流 cat(max(nums))`

#### 从 Python 执行 R 脚本

要从 Python 执行此操作，我们使用 [subprocess](https://docs.python.org/3/library/subprocess.html) 模块，这是标准库的一部分。我们将使用 `check_output` 函数来调用 R 脚本，该函数执行一个命令并存储 stdout 的输出。

要从 Python 执行 `max.R` 脚本，你首先需要构建要执行的命令。这的格式类似于我们在 [第一部分](http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-into-a-data-analysis-pipeline-part-1/) 的博文系列中看到的命令行语句，在 Python 中表示为一个字符串列表，其中的元素对应于以下内容：

`['<command_to_run>', '<path_to_script>', 'arg1' , 'arg2', 'arg3', 'arg4']`

从 Python 执行 R 脚本的一个示例见以下代码。

`# run_max.py 导入 subprocess`

`# 定义命令和参数 command ='Rscript' path2script ='path/to your script/max.R'`

`# 列表中的变量数量 args = ['11','3','9','42']`

`# 构建 subprocess 命令 cmd = [command, path2script] + args`

# check_output 将运行命令并将结果存储到变量中

x = subprocess.check_output(cmd, universal_newlines=True)

`print('这些数字的最大值是:', x)`

参数 `universal_newlines=True` 告诉 Python 将返回的输出解释为文本字符串，并处理 Windows 和 Linux 的换行符。如果省略该参数，输出将以字节字符串形式返回，并且必须通过调用 `x.decode()` 将其解码为文本，才能进行进一步的字符串操作。

#### 示例 Python 脚本

对于我们的简单 Python 脚本，我们将根据提供的子字符串模式（第二个参数）将给定字符串（第一个参数）分割成多个子字符串。结果将逐行打印到控制台。

`# splitstr.py import sys`

`# 获取传递的参数 string = sys.argv[1] pattern = sys.argv[2]`

`# 执行分割 ans = string.split(pattern)`

`# 将结果元素列表连接成一个以换行符分隔的字符串并打印 print('\n'.join(ans))`

#### 从 R 执行 Python 脚本

执行 R 的子进程时，建议使用 R 的[system2](https://stat.ethz.ch/R-manual/R-devel/library/base/html/system2.html)函数来执行和捕获输出。这是因为内置的[system](https://stat.ethz.ch/R-manual/R-devel/library/base/html/system.html)函数使用起来更复杂，且不具备跨平台兼容性。

构建要执行的命令类似于上述 Python 示例，但`system2`期望命令与其参数分开解析。此外，这些参数的第一个必须始终是要执行的脚本的路径。

处理 R 脚本路径中的空格可能会带来一个最终的复杂问题。解决这个问题的最简单方法是将整个路径名用双引号括起来，然后用单引号将这个字符串包裹起来，以便 R 保留参数中的双引号。

执行 Python 脚本的 R 示例见以下代码。

`# run_splitstr.R`

`command ="python`

`# 注意字符串中的单引号 + 双引号（如果路径中有空格需要） path2script='"path/to your script/splitstr.py"'`

`# 在向量中构建参数 string ="3523462---12413415---4577678---7967956---5456439" pattern ="---" args = c(string, pattern)`

`# 将脚本路径作为第一个参数添加 allArgs = c(path2script, args)`

`output = system2(command, args=allArgs, stdout=TRUE)`

`print(paste("子字符串为：\n", output))`

要将标准输出捕获到字符向量中（每个元素一行），必须在`system2`中指定`stdout=TRUE`，否则仅返回退出状态。当`stdout=TRUE`时，退出状态会存储在一个名为“status”的属性中。

#### 总结

可以通过使用子进程调用将 Python 和 R 集成到一个应用程序中。这些调用允许一个父进程调用另一个子进程，并捕获打印到 stdout 的任何输出。在这篇文章中，我们展示了如何使用这种方法让 R 脚本调用 Python 及反之亦然。

在未来的文章中将基于这篇文章的内容以及[第一部分](http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-into-a-data-analysis-pipeline-part-1/)，展示一个实际的示例，说明如何在应用程序中将 Python 和 R 结合使用。

[原文](http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-part-ii-executing-r-from-python-and-vice-versa/ )。

**相关：**

+   [R 与 Python：正面交锋的数据分析](/2015/10/r-vs-python-data-analysis.html)

+   [数据科学编程：Python 与 R](/2015/10/data-science-programming-python-vs-r.html)

+   [数据科学中的 R 与 Python：赢家是…](/2015/05/r-vs-python-data-science.html)

+   [R 和 Python 用户表现出惊人的稳定性，但地区差异显著](/2015/07/poll-primary-analytics-language-r-python.html)

### 更多相关话题

+   [通过整合 Jupyter 和 KNIME 减少实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [将 ChatGPT 融入数据科学工作流程：提示和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [优化数据分析：在 Databricks 中整合 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

+   [在内容创作中整合生成式人工智能](https://www.kdnuggets.com/integrating-generative-ai-in-content-creation)

+   [机器学习不像你的大脑 第 6 部分：…的重](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [数据科学备忘单的完整合集 - 第 2 部分](https://www.kdnuggets.com/2022/02/complete-collection-data-science-cheat-sheets-part-2.html)
