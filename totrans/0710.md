# 学习 Julia 数据分析

> 原文：[https://www.kdnuggets.com/learn-data-analysis-with-julia](https://www.kdnuggets.com/learn-data-analysis-with-julia)

![学习 Julia 数据分析](../Images/42a5f176adcdb595dfdd19337c9afeda.png)

图片作者

Julia 是另一种编程语言，类似于 Python 和 R。它结合了 C 语言的速度和 Python 的简洁性。Julia 在数据科学领域越来越受欢迎，所以如果你想扩展你的技能并学习一种新语言，你来对地方了。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

在本教程中，我们将学习如何为数据科学设置 Julia，加载数据，进行数据分析，然后进行可视化。这个教程非常简单，以至于任何人，包括学生，都可以在 5 分钟内开始使用 Julia 进行数据分析。

## 1\. 设置你的环境

1.  下载 Julia 并通过访问 [(julialang.org)](https://julialang.org/downloads/) 安装包。

1.  现在我们需要为 Jupyter Notebook 设置 Julia。启动一个终端（PowerShell），输入 `julia` 启动 Julia REPL，然后输入以下命令。

```py
using Pkg
Pkg.add("IJulia")
```

1.  启动 Jupyter Notebook，并以 Julia 作为内核开始新的笔记本。

1.  创建新的代码单元，并输入以下命令以安装必要的数据科学包。

```py
using Pkg
Pkg.add("DataFrames")
Pkg.add("CSV")
Pkg.add("Plots")
Pkg.add("Chain")
```

## 2\. 加载数据

对于这个示例，我们使用来自 Kaggle 的 [在线销售数据集](https://www.kaggle.com/datasets/shreyanshverma27/online-sales-dataset-popular-marketplace-data)。它包含了不同产品类别的在线销售交易数据。

我们将加载 CSV 文件并将其转换为 DataFrames，这类似于 Pandas DataFrames。

```py
using CSV
using DataFrames

# Load the CSV file into a DataFrame
data = CSV.read("Online Sales Data.csv", DataFrame)
```

## 3\. 探索数据

我们将使用 'first' 函数，而不是 `head` 来查看 DataFrame 的前 5 行。

```py
first(data, 5)
```

![学习 Julia 数据分析](../Images/6f315580cdadde360102b6b46ee752f4.png)

要生成数据摘要，我们将使用 `describe` 函数。

```py
describe(data)
```

![学习 Julia 数据分析](../Images/1647a3e64e92ca1a4e3cf4a6248624e7.png)

类似于 Pandas DataFrame，我们可以通过提供行号和列名来查看特定值。

```py
data[3,"Product Name"]
```

输出：

```py
"Levi's 501 Jeans"
```

## 4\. 数据操作

我们将使用 `filter` 函数根据特定值过滤数据。它需要列名、条件、值和 DataFrame。

```py
filtered_data = filter(row -> row[:"Unit Price"] > 230, data)
last(filtered_data, 5)
```

![学习 Julia 数据分析](../Images/a7a82ec5ce6ac10c35c9fddf3628f236.png)

我们也可以创建一个类似于Pandas的新列。这么简单。

```py
data[!, :"Total Revenue After Tax"] = data[!, :"Total Revenue"] .* 0.9  
last(data, 5)
```

![使用Julia学习数据分析](../Images/ce2c1d654e9235d734ab3ce5545b1b36.png)

现在，我们将基于不同的“产品类别”计算“税后总收入”的均值。

```py
using Statistics

grouped_data = groupby(data, :"Product Category")
aggregated_data = combine(grouped_data, :"Total Revenue After Tax" .=> mean)
last(aggregated_data, 5)
```

![使用Julia学习数据分析](../Images/d9d688e04d689e7791c3d0885c2eac2f.png)

## 5\. 可视化

可视化类似于Seaborn。在我们的案例中，我们正在可视化最近创建的汇总数据的条形图。我们将提供X和Y列，然后是标题和标签。

```py
using Plots

# Basic plot
bar(aggregated_data[!, :"Product Category"], aggregated_data[!, :"Total Revenue After Tax_mean"], title="Product Analysis", xlabel="Product Category", ylabel="Total Revenue After Tax Mean")
```

总均收入的大部分是通过电子产品产生的。可视化效果完美且清晰。

![使用Julia学习数据分析](../Images/a3fe1b959290c9ef8f13d250188081ff.png)

要生成直方图，我们只需提供X列和标签数据。我们希望可视化销售商品的频率。

```py
histogram(data[!, :"Units Sold"], title="Units Sold Analysis", xlabel="Units Sold", ylabel="Frequency")
```

![使用Julia学习数据分析](../Images/4ece5f1191f2e61c81ce95a8dad6b2bb.png)

看起来大多数人购买了一到两件商品。

为了保存可视化效果，我们将使用`savefig`函数。

```py
savefig("hist.png")
```

## 6\. 创建数据处理管道

创建一个合适的数据管道是自动化数据处理工作流程、确保数据一致性，以及实现可扩展和高效的数据分析的必要条件。

我们将使用`Chain`库来创建之前用来基于不同产品类别计算总均收入的各种函数链。

```py
using Chain
# Example of a simple data processing pipeline
processed_data = @chain data begin
       filter(row -> row[:"Unit Price"] > 230, _)
       groupby(_, :"Product Category")
       combine(_, :"Total Revenue" => mean)
end
first(processed_data, 5)
```

![使用Julia学习数据分析](../Images/19c05c40de279d4f30cc19769864bf33.png)

为了将处理后的DataFrame保存为CSV文件，我们将使用`CSV.write`函数。

```py
CSV.write("output.csv", processed_data)
```

## 结论

在我看来，Julia比Python更简单、更快。我习惯的许多语法和函数在Julia中也可用，如Pandas、Seaborn和Scikit-Learn。那么，为什么不学习一门新语言，并开始做得比你的同事更好呢？此外，这也将帮助你获得与研究相关的工作，因为大多数临床研究人员更倾向于使用Julia而不是Python。

在本教程中，我们学习了如何设置Julia环境、加载数据集、进行强大的数据分析和可视化，并构建用于可重复性和可靠性的数据管道。如果你有兴趣了解更多关于Julia的数据科学知识，请告诉我，这样我可以为你们编写更多简单的教程。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan))是一位认证的数据科学专业人士，他喜欢构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络构建一款AI产品，帮助那些遭遇心理健康问题的学生。

### 更多信息

+   [我应该学习 Julia 吗？](https://www.kdnuggets.com/2022/11/learn-julia.html)

+   [3 个用于数据可视化的 Julia 包](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [5 本免费 Julia 数据科学书籍](https://www.kdnuggets.com/2023/06/5-free-julia-books-data-science.html)

+   [如何在 Jupyter Notebook 上设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [学习数据分析和数据科学的最佳免费资源](https://www.kdnuggets.com/2024/03/365datascience-best-free-resources-learn-data-analysis-data-science)

+   [使用 Scikit-Learn 的主成分分析 (PCA)](https://www.kdnuggets.com/2023/05/principal-component-analysis-pca-scikitlearn.html)
