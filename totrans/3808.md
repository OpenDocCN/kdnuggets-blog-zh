# 3 Julia 数据可视化包

> 原文：[https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

![3 Julia 数据可视化包](../Images/5f07d11cdde1cad0eb8414926ed6ee0e.png)

作者提供的图片

Julia 编程语言在数据可视化工具方面取得了新进展，这些工具类似于 Python 的 matplotlib 和 R 的 ggplot。这些包提供了类似 C++ 的速度和开箱即用的并行处理。因此，当你需要可视化大型数据集时，这非常有用。

在本博客中，我们将学习 [Plots.jl](https://docs.juliaplots.org/latest/)、[Gadfly.jl](http://gadflyjl.org/stable/) 和 [VegaLite.jl](https://github.com/queryverse/VegaLite.jl) 的代码示例。所以，让我们从安装所需的包开始。

```py
import Pkg; Pkg.add(["RDatasets","Plots","Gadfly","VegaLite"])
```

# Plots.jl

[Plots.jl](https://docs.juliaplots.org/latest/) 是 Julia 中一个强大的可视化工具。它是一个元包，使用 GR、PythonPlot、PGFPlotsX 或 Plotly 作为后端。如果某个后端不支持你需要的功能，你可以随时切换到另一个而无需更改代码。

Plots.jl 是一个轻量级、直观、简洁、灵活且一致的绘图包。

## 示例 1

要显示正弦波，我们必须导入包，然后创建 x 和 y1 变量。

+   x: 从 0 到 10 的范围。

+   y: sin(x)

要显示折线图，我们只需将 x 和 y 参数提供给`Plots.plot`函数。

```py
using Plots
x = range(0, 10, length=100)
y1 = sin.(x)
Plots.plot(x, y1)
```

![3 Julia 数据可视化包](../Images/38ed1aff86e70f760f5ebb1a0e26ae5d.png)

你可以使用`Plots.plot!`函数重叠图表。这将显示相同轴的两个图表。

```py
y2 = @. cos(x)^2 - 1/2 
Plots.plot!(x, y2)
```

![3 Julia 数据可视化包](../Images/5a64e7fee073c540ddbe106a022c723f.png)

## 示例 2

让我们绘制一个复杂的条形图，为此我们将首先从`RDatasets`包中导入`cars`数据集。

```py
using RDatasets
cars = dataset("datasets", "mtcars")
first(cars,5)
```

![3 Julia 数据可视化包](../Images/090df84efbdc1db5271baed41314a5de.png)

然后，我们将使用`Plots.bar`函数来表示每个模型的“每加仑的英里数”和 QSec。

我们根据需求定制了函数：

+   重命名了标签。

+   添加标题。

+   将 x 刻度旋转 45 度。

+   限制图表的大小。

+   更改图例的位置。

+   显示所有车型。

+   限制 y 刻度。

```py
Plots.bar(cars.Model,
    [cars.MPG,cars.QSec],
    label = ["Miles per Gallon" "QSec"],
    title = "Models-Miles per Gallon and Qsec",
    xrotation = 45,
    size = [600, 600],
    legend =:topleft,
    xticks =:all,
    yticks =0:25)
```

![3 Julia 数据可视化包](../Images/d0384303f7e47b15c412b56c877bf861.png)

## 示例 3

对于绘制饼图，我们只需将标签和值添加到`Plots.pie`函数中。我们还添加了标题和线条宽度。

```py
x = ["A","B","C","D"]
y = [0.1,0.2,0.4,0.3]
Plots.pie(x,y,title ="KDnuggets Readers" ,l = 0.5)
```

![3 Julia 数据可视化包](../Images/690b76658f14ea28f5a5693ae2f01f95.png)

# Gadfly.jl

[Gadfly.jl](http://gadflyjl.org/stable/) 是一个流行的统计绘图和数据可视化工具。它深受 R 的 ggplot 库的影响。

**关键特性：**

+   它与 Ijulia 和 Jupyter Notebook 兼容。

+   渲染高质量的图表为 SVG、PNG、Postscript 和 PDF 格式。

+   它与 DataFrames.jl 的集成非常强大。

+   它还提供了如平移、缩放和切换等交互功能。

+   支持大量常见的绘图类型。

## 示例 1

为了绘制男性和女性的历史数据，我们将导入伦敦出生率数据。之后，我们将使用 `stack` 函数将宽格式数据转换为长格式。

这将给我们年、变量和数值列。变量将是男性或女性，数值将是出生率。

```py
births = RDatasets.dataset("HistData", "Arbuthnot")[:,[:Year, :Males, :Females]]
stacked_birth = stack(births, [:Males, :Females])
first(stacked_birth,5)
```

![3 Julia Packages for Data Visualization](../Images/50d374a45b58471e82c4891427bb1112.png)

我们正在堆叠这些列，以便可以显示两个不同颜色的图表。

`Gadfly.plot` 函数需要数据集、x 和 y 变量、颜色以及绘图类型。在我们的案例中，我们显示的是线图。

```py
using Gadfly
Gadfly.plot(stacked_birth, x=:Year, y=:value, color=:variable,
     Geom.line)
```

![3 Julia Packages for Data Visualization](../Images/ab4281db7c3e7b42e00d320a248de6c7.png)

## 示例 2

在示例中，我们将设置默认大小，并根据变量和数值显示箱型图。我们使用相同的函数，但有不同的绘图类型和主题。

**注意：** 你可以通过查看文档 [Themes · Gadfly.jl](http://gadflyjl.org/stable/man/themes/) 来了解更多关于主题的信息。

```py
set_default_plot_size(8cm, 12cm)

Gadfly.plot(
            stacked_birth, 
            x=:variable, 
            y=:value, 
            Geom.boxplot, 
            Theme(default_color="red")
            )
```

![3 Julia Packages for Data Visualization](../Images/970e567902741e81e47a37e188b74ce7.png)

# VegaLite.jl

[VegaLite.jl](https://github.com/queryverse/VegaLite.jl) 是一个用于 Julia 编程语言的交互式绘图包。它基于 [Vega-Lite](https://vega.github.io/vega-lite/)，并具有与 [Altair](https://altair-viz.github.io/getting_started/overview.html) 相似的功能，Altair 是一个交互式、简单且快速的 Python 库。

## 示例 1

在示例中，我们导入 VegaLite 并将汽车数据集传递给 `@vlplot` 函数以显示点图。

在我们的案例中，我们提供了：

+   绘图类型。

+   X 和 y 变量。

+   将 ‘Cyl’ 列添加到颜色参数中。

+   设置图形的宽度和高度。

**注意：** 我们通过在列名前添加 `:n` 将整数转换为分类值。在我们的案例中，它是“Cyl:n”。

```py
using VegaLite

cars |> @vlplot(
    :point,
    x=:HP,
    y=:MPG,
    color="Cyl:n",
    width=400,
    height=300
)
```

![3 Julia Packages for Data Visualization](../Images/7aec8be30290c9c613222b642a236188.png)

## 示例 2

在第二个示例中，我们将绘制气缸类型的条形图。对于 x 参数，我们将使用“Cyl”作为类别，对于 y，我们使用“count()”，它将计算“Cyl”列中类别的数量。

```py
@vlplot(
    data=cars,
    height=350,
    width=300,
    :bar,
    x="Cyl:n",
    y="count()",
)
```

![3 Julia Packages for Data Visualization](../Images/4c2c1539eff4b34eb7263adf785e7444.png)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专家，喜欢构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为那些在心理健康方面挣扎的学生构建 AI 产品。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

### 了解更多相关信息

+   [5 本免费的 Julia 数据科学书籍](https://www.kdnuggets.com/2023/06/5-free-julia-books-data-science.html)

+   [使用 Julia 学习数据分析](https://www.kdnuggets.com/learn-data-analysis-with-julia)

+   [我应该学习 Julia 吗？](https://www.kdnuggets.com/2022/11/learn-julia.html)

+   [如何在 Jupyter Notebook 上设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [2023 年需要了解的顶级数据 Python 包](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)
