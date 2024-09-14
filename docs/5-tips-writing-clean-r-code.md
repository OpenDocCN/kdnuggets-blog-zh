# 编写干净 R 代码的 5 个技巧

> 原文：[`www.kdnuggets.com/2021/08/5-tips-writing-clean-r-code.html`](https://www.kdnuggets.com/2021/08/5-tips-writing-clean-r-code.html)

comments

**由 [Marcin Dubel](https://appsilon.com/author/marcin/)，Appsilon**

![Image](img/978a8b58e3a2eed28f54c8122f234d6e.png)

## 干净的 R 代码至关重要

多年的成功项目经验让我发现每个实施方案中都有一个共同的元素。一个干净、可读且简洁的代码库是有效协作的关键，并为客户提供了最高质量的价值。

代码审查是维护高质量代码流程的一个重要部分。它也是共享最佳实践和在团队成员之间分配知识的绝佳方式。在 Appsilon，我们将代码审查视为每个项目的必备环节。阅读更多关于我们如何组织工作的内容，查看 [Olga 的博客文章](https://appsilon.com/remote-data-science-team-best-practices-scrum-github-and-docker/)，了解推荐给所有数据科学团队的最佳实践。

拥有一个完善的代码审查流程并不改变开发者编写良好、干净代码的责任！指出代码中的所有基本错误是痛苦的、耗时的，并且会分散审查员深入代码逻辑或提高代码有效性的注意力。

编写不良代码还可能伤害团队士气——代码审查员会感到沮丧，而代码创建者可能会因为大量评论而感到受伤。因此，在发送代码进行审查之前，开发者需要确保代码尽可能干净。同时，请注意，并不是总会有代码审查员可以提供帮助。有时候你需要独自处理项目。即使你现在认为代码没问题，也考虑在几个月后重新阅读一遍——你希望它足够清晰，以免以后浪费自己的时间。

在这篇文章中，我总结了在编程中需要避免的最常见错误，并概述了应该遵循的最佳实践。遵循这些提示可以加快代码审查的迭代过程，并在审查者眼中成为一名超级开发者！

## 避免带有注释的注释

向代码添加注释是开发者的一个关键技能。然而，更为关键且难以掌握的技能是知道何时*不*添加注释。编写好的注释更像是一门艺术而非科学。这需要大量的经验，你可以为此写整本书（例如，[这里](https://books.google.pl/books/about/Clean_Code.html?id=hjEFCAAAQBAJ)）。

你应该遵循几个简单的规则，以避免关于你注释的注释：

+   注释应向读者添加外部知识：如果它们在解释代码本身发生了什么，这是一个警示信号，说明代码不够清晰，需要重构。如果使用了某种黑客技术，则可能需要注释来解释发生了什么。注释所需的业务逻辑或例外是故意添加的。尝试考虑可能会让未来的读者感到惊讶的内容，并预先解决他们的困惑。

+   只写必要的注释！你的注释不应成为易于搜索的信息字典。一般来说，注释会分散注意力，没有代码本身解释逻辑好。例如，最近我看到代码中的注释是：`trimws(.) # 这个函数修剪前导/尾随空格`——这是多余的。如果读者不知道`trimws`函数的作用，可以很容易查找。一个更稳健的注释可能会更有帮助，例如：`trimws(.) # TODO(Marcin Dubel): 修剪空格在这里至关重要，因为数据库条目不一致；数据需要清理。`

+   编写 R 语言函数时，即使你没有编写包，我也推荐[使用{roxygen2}注释](https://cran.r-project.org/web/packages/roxygen2/vignettes/roxygen2.html)。这是一个组织关于函数目标、参数和输出知识的优秀工具。

+   仅用英语编写注释（以及所有代码部分）。使其对所有读者可理解可能会节省因使用母语特殊字符而出现的编码问题。

+   如果将来需要重构/修改某些代码，请用`# TODO`注释标记。同时，添加一些信息以标识你作为此注释的作者（以便在需要详细信息时联系）以及简要解释为什么将以下代码标记为 TODO 而不是立即修改。

+   永远不要留下未注释的注释代码！保留一些部分供将来使用或暂时关闭是可以的，但始终标记此操作的原因。

请记住，注释将保留在代码中。如果有些事情你只想告诉审查者一次，请将注释添加到 Pull（Merge）请求中，而不是代码本身。

**示例**：我最近看到移除代码的一部分，附带注释如：“由于逻辑更改而移除。” 好的，这样知道了，但之后在代码中的这个注释显得奇怪且多余，因为读者不再看到被移除的代码。

## 字符串

与文本相关的常见问题是字符串连接的可读性。我遇到的一个问题是`paste`函数的过度使用。不要误解我的意思；当你的字符串很简单时，例如`paste("我的名字是", my_name)`，它是一个很棒的函数，但对于更复杂的形式，阅读起来很困难：

`paste("我的名字是", my_name, "，我住在", my_city, "，在", language, "中开发超过", years_of_coding)`

更好的解决方案是使用`sprintf`函数或`glue`，例如

`glue(“我的名字是{my_name}，我住在{my_city}，从事{language}开发已经有{years_of_coding}年了”)`

没有那些逗号和引号不是更清楚吗？

当处理许多代码块时，将它们提取到单独的位置，例如，**.yml 文件**，会非常好。这使得代码和文本块更易于阅读和维护。

关于文本的最后一个提示：调试技术之一，通常在 Shiny 应用程序中使用，是添加`print()`语句。仔细检查代码中是否没有遗留的打印语句——这在代码审查时可能非常尴尬！

## 循环

循环是编程的基本构件之一，是非常强大的工具。然而，它们可能计算开销很大，因此需要谨慎使用。你应该遵循的经验法则是：总是仔细检查循环是否是一个好的选择。在`data.frame`中循环通常不是必要的：应该有一个`{dplyr}`函数来更有效地解决问题。

另一个常见的问题来源是使用对象的长度来循环元素，例如`for(i in 1:length(x)) ...`。但如果 x 的长度为零会怎样！是的，循环会对迭代器值 1 和 0 采取不同的方式。这可能不是你的计划。使用`seq_along`或`seq_len`函数要安全得多。

同时，记得使用`apply`系列函数进行循环。它们很棒（更不用说`{purrr}`解决方案了）！请注意，使用`sapply`可能会被审查者评论为不稳定——因为这个函数会自动选择输出的类型！所以有时它会是一个列表，有时会是一个向量。使用`vapply`更安全，因为程序员定义了预期的输出类。

## 代码共享

即使你是一个人工作，你也可能希望你的程序能在其他机器上正常运行。当你与团队共享代码时，这一点尤其重要！为此，切勿在代码中使用绝对路径，例如“/home/marcin/my_files/old_projects/september/project_name/file.txt”。它对其他人将不可访问。请注意，任何文件夹结构的违规都会导致代码崩溃。

由于你应该已经有一个用于所有编码工作的项目，你需要使用与特定项目相关的路径——在这种情况下，将是“./file.txt”。此外，我建议将所有路径作为变量保存在一个地方——这样重命名文件只需在代码中更改一次，而不是，例如，在六个不同的文件中更改二十次。

有时你的软件需要使用一些凭证或令牌，例如，访问数据库或私有库。你绝不应该将这些秘密提交到代码库中！即使团队中的条目是相同的。通常，好的做法是将这些值保存在`.Renviron`文件中作为环境变量，在启动时加载，并且该文件本身在仓库中被忽略。你可以在[这里](http://www.dartistics.com/renviron.html)阅读更多关于它的内容。

## 良好的编程实践

最后，让我们关注如何改进你的代码。首先，你的代码应该易于理解且干净——即使你一个人工作，当你一段时间后回到代码时，它也会让你的生活更轻松！

使用具体的变量名，即使它们看起来较长——经验法则是你应该能仅通过读取名称来猜测内容，所以`table_cases_per_country`是可以的，但`tbl1`则不行。避免使用缩写。长度优于模糊。保持一致的对象命名风格（如 camelCase 或 snake_case），并在团队成员之间达成一致。

不要将逻辑值`T`缩写为`TRUE`和`F`缩写为`FALSE`——代码会工作，但`T`和`F`是常规对象，可以被覆盖，而`TRUE`和`FALSE`是特殊值。

不要使用等式来比较逻辑值，如`if(my_logical == TRUE)`。如果你可以与`TRUE`比较，这意味着你的值已经是逻辑值，因此`if(my_logical)`就足够了！如果你想双重检查该值是否确实是`TRUE`（而不是，例如，`NA`），可以使用`isTRUE()`函数。

确保你的逻辑语句是正确的。检查你是否理解 R 中[单一和双重逻辑运算符](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Logic.html)之间的区别！

良好的间距对于可读性至关重要。确保规则在团队中是一致且达成一致的。这将使彼此的代码更易于遵循。最简单的解决方案是站在巨人的肩膀上，遵循[tidyverse 风格指南](https://style.tidyverse.org/)。

然而，在审查过程中检查每一行的风格是相当低效的，因此确保在你的开发工作流程中引入**linter**和**styler**，正如[Olga 的博客文章](https://appsilon.com/remote-data-science-team-best-practices-scrum-github-and-docker/)中所展示的那样。这可能会拯救你的生命！最近我们发现了一些遗留代码中的错误，这些错误本可以被 linter 自动识别：

```py
sum_of_values <- first_element 
  + second_element
```

这并没有返回作者所期望的元素之和。

说到变量名——这是编程中已知的最难的事情之一。因此，在不必要时应避免使用变量名。请注意，R 函数默认返回最后创建的元素，因此你可以轻松地替换它：

```py
sum_elements <- function(first, second) {
  my_redundant_variable_name <- sum(first, second)
  return(my_redundant_variable_name)
}
```

用更简短（且简单的，你不需要考虑名称）的方式：

```py
sum_elements <- function(first, second) {
  sum(first, second)
}
```

另一方面，请在每次重复某些函数调用或计算时使用额外的变量！这将使计算更有效，并且未来更容易修改。记住保持代码[DRY – 不重复自己](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)。如果你复制粘贴一些代码，三思是否应该将其保存到变量中、在循环中完成，或移动到函数中。

## 结论

这样，你就有了五种编写清晰 R 代码并让你的代码审查者无话可说的策略。这五种方法将确保你编写出优质的代码，即使多年后也容易理解。祝编程愉快！

**简介：[Marcin Dubel](https://appsilon.com/author/marcin/)** 是 Appsilon 的工程师。

[原始](https://appsilon.com/write-clean-r-code/)。经许可转载。

**相关：**

+   用于手写字母识别的支持向量机（R 语言）

+   顶级编程语言及其用途

+   如何成为数据科学家的指南（逐步方法）

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [使用管道编写清晰的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [掌握 Python：7 种编写清晰、组织良好的代码策略](https://www.kdnuggets.com/mastering-python-7-strategies-for-writing-clear-organized-and-efficient-code)

+   [提升 Python 函数的 5 个技巧](https://www.kdnuggets.com/5-tips-for-writing-better-python-functions)

+   [利用数据科学使清洁能源更加公平](https://www.kdnuggets.com/2022/03/data-science-make-clean-energy-equitable.html)

+   [宣布博客写作比赛，获胜者将获得 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [生产可读的数据科学代码的 7 个技巧](https://www.kdnuggets.com/2022/11/7-tips-produce-readable-data-science-code.html)
