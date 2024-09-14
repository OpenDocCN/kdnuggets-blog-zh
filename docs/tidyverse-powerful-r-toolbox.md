# 一个主观的 R 数据科学工具箱，来自 Hadley Wickham 的 tidyverse

> 原文：[https://www.kdnuggets.com/2017/10/tidyverse-powerful-r-toolbox.html](https://www.kdnuggets.com/2017/10/tidyverse-powerful-r-toolbox.html)

![](../Images/c25a61e728673f95213e84868a59ef3e.png)

在这篇文章中，我们将总结一下 **tidyverse**，这是一套 R 包，它们共享一种常见的数据表示和 API 设计，以实现数据科学工作流程的协调和流畅性，旨在提高一致性，易于安装。它可以被视为 R 中的数据科学和数据管理工具箱，因为它是为指导用户进行提高可重现性和沟通能力的工作流程而构建的。

* * *

## 我们的前 3 门课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速跟踪网络安全职业发展

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 部门支持你的组织

* * *

只要将这个包加载到你的项目中，你就可以轻松地执行基本的数据科学任务，如导入、绘图、整理和建模数据，以及进行新开发的函数编程。这个超级包的核心部分是由一系列强大的 R 包组成，如 ggplot2、dplyr、tidyr、readr、purr 和 tibble 等。我们将在后面的部分详细介绍这些包的细节，并展示一些基本的示例，帮助你开始使用这个令人惊奇的工具箱。这个软件包旨在成为一个和谐和兼容的工具和命令集，为有效的数据科学工作流程提供符合规范的定义。

首先，介绍 tidyverse 的历史，以及它是如何形成的和背后的幕后推手是谁。这个“包里的包”是由 Hadley Wickham 开发的，他是 RStudio 的首席科学家，也是令人惊叹的 O'Reilly 系列书籍“R 数据科学”的合著者；他也负责将其维护到最佳状态。你可能已经熟悉 Hadley，因为他开发了之前的 R 包，如 reshape、reshape2 和 plyr；这些又是 tidyverse 的构建块，是几次实验和版本的产物。它旨在为统计学家和数据科学家提供帮助，提高他们的生产力，并试图将规范化的数据科学工作流程（图）抽象为一个实际的产品。它是一个非常多功能和一致性的软件包，因为其强大的功能涵盖从提高生产力和工作流程改进到新的数据科学软件开发和数据科学教育。

![](../Images/0e8a6ee7ad2203a2bbc1496486941786.png)

重要的是要更详细地介绍一下这个包的一般特性，这样我们就可以在我们的实际示例中看到它的运作方式。它的一致性深深根植于变量、函数和运算符遵循正则模式和语法的事实。例如，每个函数的第一个参数将是一个整洁的数据框（每个观察一行，每个变量一列，每个单元格一个入口）。为了执行操作，可以直观地连接一系列命令、基本函数和运算符来创建一个整洁的管道。核心包组织的方式、编码风格和测试程序构成了第二个较低级别的一致性因素，因此当我们说“一致”这个词时，不应轻易忽视。最后，由于分析工作流程过程与不同的 tidyverse 子包之间存在一对一的关系，因此能够轻松建立有效的端到端工作流程，以响应特定的分析目的并利用多种类型的数据。

因此，有经验的用户和初学者之间对其的使用越来越受欢迎并不奇怪。事实上，那些希望学习数据科学中的 R 的人应该从 tidyverse 开始，因为它具有友好且学习曲线不陡峭的特点，使早期职业人员能够在短时间内处理和解决复杂的数据集。

现在，让我们深入了解每个核心包，这样我们就可以对我们所提到的流畅性和一致性有一个概览。您可以查看下面链接的文档以获取有关在使用 tidyverse 时加载的其他包的更多信息。

+   导入你的数据：这里的主要包是 readr。它是一种友好且快速的读取矩形数据的方式，其灵活性允许解析不同的数据类型。除了 readr，还将加载其他次要包，例如 readxl 和 haven 等。您可以在[这里](https://readr.Tidyverse.org)找到更多信息。

+   清洁你的数据：首选的包是 tibble 和 tidyr。

    +   tibble 是一种优化的数据存储方式。将 tibble 看作是 R 数据框的重新想象版本，尽管它相当倔强和缓慢。然而，这不是一个限制，而是一件好事，因为它需要在管道的早期解决问题，从而带来更优雅的代码。有关更多信息，请访问[此链接](https://tibble.Tidyverse.org)。

    +   tidyr 允许你创建整洁的数据，即每个变量一列；每个观察一行，每个单元格一个值。通过其新的函数 gather() 和 spread()，你可以执行来自以前 R 包的经典的“cast”和“melt”，使得宽表和长表之间的转换变得轻松高效。tidyr 的更多信息[在这里](https://tidyr.Tidyverse.org/)。

+   转换你的数据：核心包是 dplyr，它是 R 语言中最受欢迎的包之一。它是一组语法一致的数据操作动词，非常直观而强大。它的主要函数是 mutate()、filter()、select()、summarise() 和 arrange()。这些可以通过使用 group_by() 命令按组执行操作。在[这里](https://dplyr.Tidyverse.org/)了解更多关于 tidyr 的信息。

+   数据可视化：对于这一任务，使用 ggplot2，这是一个基于图形语法创建数据可视化的强大而优雅的方法。你只需输入你的数据，声明你的变量美学和选择几何图形的类型，ggplot2 将会处理剩下的事情。更多关于 tidyr 的信息在[这里](https://ggplot2.Tidyverse.org)。

+   编程分析：purrr。如果你想要使用 R 的函数式编程，你可以使用这个包的 map() 函数系列。它允许你以清晰简洁的方式迭代你的数据。访问这个 [链接](https://purr.Tidyverse.org) 了解更多信息。

现在，让我们开始吧。在这个例子中，我们将使用来自 Kaggle 竞赛的泰坦尼克号数据集，你可以在[这里](https://www.kaggle.com/c/titanic/data) 轻松下载。我们的例子目的是向你展示一些 tidyverse 中常见的操作，并展示一下这个超级 R 包的语法和一致性的强大。希望你能自己复现这段代码。我鼓励你阅读文档，这样你就可以为数据分析想出自己的 tidyverse 配方。

注意：你会经常看到这个符号（‘%>%’）在这个例子中。这被称为管道操作符，在脚本中非常有用，因为它允许你跟踪分析逻辑。可以这样想：函数 f(x) 现在表示为 x %>% f。

欲了解更多 tidyverse 文档，请访问[官方网站](https://www.tidyverse.org) 并查看额外的例子。

如果你是初学 R 语言，并且想了解更多有关 tidyverse 的用法，Hadley Wickham 的书籍 [R for Data Science](https://r4ds.had.co.nz) 是最好的资源。

另外，关于 tidyverse 及其特性的另一个很好的总结，来自 [R Views](https://rviews.rstudio.com/2017/06/08/what-is-the-tidyverse/) 的这篇文章是一个很棒的参考。

**相关**:

+   [R 和 dplyr 的下一代数据处理](/2017/08/next-generation-data-manipulation-dplyr.html)

+   [缺失数据的解决方案：使用 R 进行插补](/2017/09/missing-data-imputation-using-r.html)

+   [Python vs R – 数据科学、机器学习中谁更领先？](/2017/09/python-vs-r-data-science-machine-learning.html)

### 更多相关主题

+   [2024 年每位数据科学家工具箱中必备的 5 种工具](https://www.kdnuggets.com/5-tools-every-data-scientist-needs-in-their-toolbox-in-2024)

+   [停止学习数据科学，找到目标，找到目标...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最低要求：你需要知道的10个基本技能才能开始...](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [数据科学定义幽默：一系列古怪的引语...](https://www.kdnuggets.com/2022/02/data-science-definition-humor.html)

+   [5个数据科学项目，学习5个关键的数据科学技能](https://www.kdnuggets.com/2022/03/5-data-science-projects-learn-5-critical-data-science-skills.html)

+   [KDnuggets新闻，11月30日：Chebychev的定理是什么，以及如何...](https://www.kdnuggets.com/2022/n46.html)
