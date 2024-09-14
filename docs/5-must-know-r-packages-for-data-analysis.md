# 5 个必须了解的 R 数据分析包

> 原文：[`www.kdnuggets.com/5-must-know-r-packages-for-data-analysis`](https://www.kdnuggets.com/5-must-know-r-packages-for-data-analysis)

![5 个必须了解的 R 数据分析包](img/4065694acdeedbecb12700c703a4f572.png)

图片来源 | Ideogram

R 是一种功能强大且多用途的编程语言，广泛用于数据分析和统计。该编程语言的一个主要优势是其强大的包生态系统。这些包增强了基础 R 的功能，使得执行各种分析任务变得更简单。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

以下是五个必须了解的 R 数据分析包。

## 1\. dplyr

[Dplyr](https://dplyr.tidyverse.org/) 是 [tidyverse](https://www.tidyverse.org/) 套件中的一部分，旨在有效的数据处理。它提供了直观的功能，使数据处理变得更简单。这些功能包括 filter 用于从数据集中选择满足特定条件的行，mutate 用于创建新列，arrange 用于排序数据，以及 summarize 用于生成汇总统计。

dplyr 的一个关键特点是管道操作符 (%>%)。这允许你将数据分析中的多个步骤链接成一个连续的流程。例如，你可以先过滤数据集，然后将结果通过管道传递给 mutate 函数以创建新列，再通过管道传递给 arrange 以排序最终结果，这一切都可以在一行代码中完成。dplyr 也与下文讨论的其他包如 ggplot2 和 tidyr 兼容良好，使其成为任何数据分析项目中一个关键的包。

## 2\. ggplot2

[ggplot2 包](https://ggplot2.tidyverse.org/) 也是 tidyverse 套件的一部分。它允许你使用最少的代码生成各种图表，包括散点图、条形图和折线图。每个图表都是分层开发的，你首先设置坐标轴，然后叠加你想要包含的点或线，再添加标签、图例和其他元素。这允许创建非常复杂但高度自定义的可视化。

ggplot2 的一个好处是这种分层方法可以创建独特的可视化。例如，你可以从箱线图开始，然后在其上叠加散点图。ggplot2 包还与其他 tidyverse 包集成得非常好，使其成为任何工作流程中的可靠组成部分。

## 3\. tidyr

[tidyr 包](https://tidyr.tidyverse.org/) 再次是 tidyverse 集合的一部分。它的目的是用于数据清理和组织，将数据从混乱的原始格式转换为整洁的版本。主要功能包括 gather 将宽数据转换为长格式，spread 将长数据转换为宽格式，以及 unite 将多个列合并为一个。虽然它是一个简单的包，但在处理复杂数据集或合并多个数据源时是不可或缺的。

## 4\. lubridate

处理日期和时间变量可能非常棘手，因为格式多样且时区复杂。[lubridate 包](https://lubridate.tidyverse.org/) 通过提供一组专门为处理日期和时间变量构建的函数来简化这些复杂性。其核心特征是日期和时间格式函数，如 ymd 和 mdy_hms，这些函数可以自动识别常见的日期时间格式并将其转换为 R 能够直接操作的日期对象。

函数如 year 或 minute 也允许你提取日期时间对象的特定组件。你可以使用这些函数快速创建一个只包含变量年份的新列，或者计算两个日期之间的年份差。这些包对于处理事件、时间序列数据或包含出生日期的人口统计数据的人至关重要。

## 5\. shiny

[shiny 包](https://www.rstudio.com/products/shiny/) 是一个强大的框架，用于在 R 中直接构建交互式可视化和网页应用程序。你可以创建动态数据仪表板和可视化，而无需了解网页开发语言（如 HTML 或 JavaScript）。用 shiny 构建的应用程序将包括用户界面部分（定义视觉布局和外观）和服务器部分（定义如何处理和更新数据以响应用户交互）。

Shiny 由反应式编程驱动，因此当用户与之交互时，视觉化内容会自动更新。例如，你可以创建一个散点图，根据用户输入过滤数据。Shiny 与其他 R 包（如 ggplot2 和 dplyr）集成良好，以增强分析和可视化效果。

## 摘要

这些 R 包是任何数据分析师的必备工具，提供了高效的数据操作、可视化和分析方法。掌握这些包将显著提升你的分析能力和生产力。

[](https://www.linkedin.com/in/mehrnazsiavoshi/)**[Mehrnaz Siavoshi](https://www.linkedin.com/in/mehrnazsiavoshi/)** 拥有数据分析硕士学位，是一名全职生物统计学家，专注于医疗保健中的复杂机器学习开发和统计分析。她拥有人工智能方面的经验，并在人民大学教授生物统计学和机器学习课程。

### 更多相关信息

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [2023 年值得了解的顶级数据 Python 包](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

+   [3 个用于数据可视化的 Julia 包](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [掌握 SQL、Python、数据清洗、数据整理和探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [数据科学家探索性数据分析的必备指南](https://www.kdnuggets.com/2023/06/data-scientist-essential-guide-exploratory-data-analysis.html)

+   [SQL 数据清洗：如何准备杂乱的数据进行分析](https://www.kdnuggets.com/data-cleaning-in-sql-how-to-prepare-messy-data-for-analysis)
