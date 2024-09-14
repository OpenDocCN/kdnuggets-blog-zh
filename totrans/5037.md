# 使用 Python 挖掘 Twitter 数据 第五部分：数据可视化基础

> 原文：[https://www.kdnuggets.com/2016/06/mining-twitter-data-python-part-5.html](https://www.kdnuggets.com/2016/06/mining-twitter-data-python-part-5.html)

**作者：Marco Bonzanini，自由数据科学顾问**。

一图胜千言：设计一个好的[可视化表现](https://en.wikipedia.org/wiki/Data_visualization)可以帮助我们理解数据并突出有趣的见解。收集和分析 Twitter 数据后，本教程将继续介绍使用 Python 进行数据可视化的一些概念。

![Twitter](../Images/3da5b4dea824ea453ca3ae25f3548634.png)

### 从 Python 到 JavaScript 使用 Vincent

尽管使用如 matplotlib 或 ggplot 等库在 Python 中创建图表有一些选项，但可能最酷的数据可视化库是[D3.js](http://d3js.org/)，顾名思义，它是基于 JavaScript 的。D3 与 CSS 和 SVG 等 Web 标准兼容，并允许创建一些精彩的互动可视化效果。

[Vincent](https://github.com/wrobstory/vincent)弥合了 Python 后端和支持 D3.js 可视化的前端之间的差距，让我们能够受益于两者。Vincent 的口号实际上是“Python 的数据能力。JavaScript 的可视化能力”。Vincent 作为一个 Python 库，将我们的数据转换成[Vega](https://github.com/trifacta/vega)，一种基于 JSON 的可视化语法，该语法将在 D3 上使用。听起来可能很复杂，但实际上很简单且*符合 Python 风格*。如果你不想的话，你不需要写一行 JavaScript/D3 代码。

首先，让我们安装 Vincent：

```py
sudo pip install vincent

```

其次，让我们创建我们的第一个图表。使用我们的[rugby data set](https://marcobonzanini.com/2015/03/23/mining-twitter-data-with-python-part-4-rugby-and-term-co-occurrences/)中的[最常见术语列表](https://marcobonzanini.com/2015/03/17/mining-twitter-data-with-python-part-3-term-frequencies/)（不包括标签），我们想要绘制它们的频率：

```py
import vincent

word_freq = count_terms_only.most_common(20)
labels, freq = zip(*word_freq)
data = {'data': freq, 'x': labels}
bar = vincent.Bar(data, iter_idx='x')
bar.to_json('term_freq.json')

```

此时，`term_freq.json` 文件将包含一个描述图表的内容，可以交给 D3.js 和 Vega。一个简单的模板（取自 Vincent 资源）用于可视化图表：

```py
<html>  
<head>    
    <title>Vega Scaffold</title>
    <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
    <script src="http://d3js.org/topojson.v1.min.js"></script>
    <script src="http://d3js.org/d3.geo.projection.v0.min.js" charset="utf-8"></script>
    <script src="http://trifacta.github.com/vega/vega.js"></script>
</head>
<body>
    <div id="vis"></div>
</body>
<script type="text/javascript">
// parse a spec and create a visualization view
function parse(spec) {
  vg.parse.spec(spec, function(chart) { chart({el:"#vis"}).update(); });
}
parse("term_freq.json");
</script>
</html>

```

将上述 HTML 页面保存为`chart.html`，然后运行简单的 Python Web 服务器：

```py
python -m http.server 8888 # Python 3
python -m SimpleHTTPServer 8888 # Python 2

```

现在你可以打开浏览器访问**http://localhost:8888/chart.html**，观察结果：

[![术语频率](../Images/b1f3356aba782317f239a457ff539d1d.png)](https://marcobonzanini.files.wordpress.com/2015/04/term_freq.png)

*点击放大。*

注意：你可以直接从 Python 保存 HTML 模板，使用：

```py
bar.to_json('term_freq.json', html_out=True, html_path='chart.html')

```

但至少在 Python 3 中，输出的 HTML 并不完整，你需要手动去除一些字符。

使用这个过程，我们可以用 Vincent 绘制多种类型的图表。让我们花点时间[browse the docs](http://vincent.readthedocs.org/en/latest/)查看其功能。

### 时间序列可视化

分析 Twitter 数据的另一个有趣方面是观察推文随时间的分布。换句话说，如果我们将频率组织成时间段，我们可以观察 Twitter 用户对实时事件的反应。

我最喜欢的 Python 数据分析工具之一是[Pandas](http://pandas.pydata.org/)，它对时间序列也有相当不错的支持。作为示例，让我们跟踪标签`#ITAvWAL`以观察第一场比赛期间发生了什么。

首先，如果我们还没有这样做，我们需要安装 Pandas：

```py
sudo pip install pandas

```

在读取所有推文的主循环中，我们只需跟踪标签的出现次数，即我们可以将代码重构为类似于[之前的几集](https://marcobonzanini.com/2015/03/23/mining-twitter-data-with-python-part-4-rugby-and-term-co-occurrences/)的形式：

```py
import pandas
import json

dates_ITAvWAL = []
# f is the file pointer to the JSON data set
for line in f:
    tweet = json.loads(line)
    # let's focus on hashtags only at the moment
    terms_hash = [term for term in preprocess(tweet['text']) if term.startswith('#')]
    # track when the hashtag is mentioned
    if '#itavwal' in terms_hash:
        dates_ITAvWAL.append(tweet['created_at'])

# a list of "1" to count the hashtags
ones = [1]*len(dates_ITAvWAL)
# the index of the series
idx = pandas.DatetimeIndex(dates_ITAvWAL)
# the actual series (at series of 1s for the moment)
ITAvWAL = pandas.Series(ones, index=idx)

# Resampling / bucketing
per_minute = ITAvWAL.resample('1Min', how='sum').fillna(0)

```

最后一行使我们能够跟踪频率的变化。序列以1分钟的间隔重新采样。这意味着所有在特定一分钟内的推文将被汇总，更准确地说，它们将被求和，给定`how='sum'`。时间索引将不再跟踪秒数。如果在特定的一分钟内没有推文，`fillna()`函数将用零填充空白。

要将时间序列绘制到图表中，使用 Vincent：

```py
time_chart = vincent.Line(ITAvWAL)
time_chart.axis_titles(x='Time', y='Freq')
time_chart.to_json('time_chart.json')

```

一旦将`time_chart.json`文件嵌入到上面讨论的 HTML 模板中，你将看到以下输出：

[![Time Series](../Images/c07ddda13c75deb1b47a79f071bb8cb6.png)](https://marcobonzanini.files.wordpress.com/2015/04/time1.png)

*点击放大。*

比赛的有趣时刻可以通过序列中的峰值观察到。第一次峰值发生在下午 1 点前，对应于第一次意大利尝试。1:30 和 2:30 之间的其他所有峰值对应于威尔士的尝试，显示了威尔士在下半场的主导地位。比赛在 2:30 结束，所以之后 Twitter 变得安静。

与其一次观察一个序列，不如比较不同的序列以观察匹配情况如何演变。因此，让我们重构时间序列的代码，将三个不同的标签`#ITAvWAL`、`#SCOvIRE`和`#ENGvFRA`跟踪到相应的`pandas.Series`中。

```py
# all the data together
match_data = dict(ITAvWAL=per_minute_i, SCOvIRE=per_minute_s, ENGvFRA=per_minute_e)
# we need a DataFrame, to accommodate multiple series
all_matches = pandas.DataFrame(data=match_data,
                               index=per_minute_i.index)
# Resampling as above
all_matches = all_matches.resample('1Min', how='sum').fillna(0)

# and now the plotting
time_chart = vincent.Line(all_matches[['ITAvWAL', 'SCOvIRE', 'ENGvFRA']])
time_chart.axis_titles(x='Time', y='Freq')
time_chart.legend(title='Matches')
time_chart.to_json('time_chart.json')

```

以及输出：

[![time2](../Images/68b37061e2635500f804fbd18f407310.png)](https://marcobonzanini.files.wordpress.com/2015/04/time2.png)

*点击放大。*

我们可以立即观察到不同比赛的发生时间（大约 12:30-2:30、2:30-4:30 和 5-7），并且可以看到最后一场比赛吸引了所有的关注，特别是在赢家揭晓时。

### 概述

数据可视化是数据分析中一个重要的学科。通过支持数据的可视化表示，我们可以提供有趣的见解。我们讨论了一个相对简单的选项，使用 Python 和 Vincent 支持数据可视化。特别是，我们看到如何轻松地弥合 Python 与 Javascript 这样一个提供了 D3.js 这样重要工具的语言之间的差距。总体而言，我们仅仅触及了数据可视化的表面，但作为一个起点，这应该足以激发一些好的想法。Twitter 作为一种媒介的性质也促使我们快速了解时间序列分析，使我们提到`pandas`作为一个出色的 Python 工具。

**简历：[Marco Bonzanini](https://twitter.com/marcobonzanini)** 是一位总部位于英国伦敦的数据科学家。他活跃于 PyData 社区，喜欢从事文本分析和数据挖掘应用。他是《[用 Python 掌握社交媒体挖掘](https://www.amazon.com/Mastering-Social-Media-Mining-Python-ebook/dp/B01BFD2Z2Q)》（Packt Publishing，2016 年 7 月）的作者。

[原文](https://marcobonzanini.com/2015/04/01/mining-twitter-data-with-python-part-5-data-visualisation-basics/)。经许可转载。

**相关**：

+   [用 Python 挖掘 Twitter 数据 第 2 部分：文本预处理](/2016/06/mining-twitter-data-python-part-2.html)

+   [用 Python 挖掘 Twitter 数据 第 3 部分：术语频率](/2016/06/mining-twitter-data-python-part-3.html)

+   [用 Python 挖掘 Twitter 数据 第 4 部分：橄榄球和术语共现](/2016/06/mining-twitter-data-python-part-4.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关主题

+   [回到基础，第二部分：梯度下降](https://www.kdnuggets.com/2023/03/back-basics-part-dos-gradient-descent.html)

+   [Python 基础：语法、数据类型和控制结构](https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures)

+   [回到基础 第 1 周：Python 编程与数据科学基础](https://www.kdnuggets.com/back-to-basics-week-1-python-programming-data-science-foundations)

+   [掌握数据科学中的 Python：超越基础](https://www.kdnuggets.com/mastering-python-for-data-science-beyond-the-basics)

+   [如何通过 ChatGPT 学习 Python 基础](https://www.kdnuggets.com/how-to-learn-python-basics-with-chatgpt)

+   [基础回顾第2周：数据库、SQL、数据管理与统计概念](https://www.kdnuggets.com/back-to-basics-week-2-database-sql-data-management-and-statistical-concepts)
