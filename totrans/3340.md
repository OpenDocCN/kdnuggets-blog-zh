# 机器学习驱动的防火墙

> 原文：[https://www.kdnuggets.com/2017/02/machine-learning-driven-firewall.html](https://www.kdnuggets.com/2017/02/machine-learning-driven-firewall.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**作者：Faizan Ahmad，Fsecurify CEO**。

最近，我一直在思考将机器学习应用于我可以做的安全项目并与大家分享的方法。几天前，我偶然发现了一个叫做[ZENEDGE](https://www.zenedge.com/)的网站，提供**AI驱动的网络应用防火墙**。我喜欢这个概念，想做一些类似的东西并与社区分享。所以，让我们做一个吧。

### fWaf – 机器学习驱动的网络应用防火墙

### 数据集：

首先需要做的是找到标记的数据，但我找到的数据相当旧（2010年）。有一个网站叫做[**SecRepo**](http://secrepo.com/)，提供了很多安全相关的数据集。其中之一是包含数百万个查询的http日志。这是我想要的数据集，但它没有标记。我使用了一些启发式方法和我以前的安全知识，通过编写几个脚本来标记数据集。

在修剪数据之后，我想收集更多的恶意查询。因此，我去找了[**payloads**](https://github.com/foospidy/payloads)，并找到了一些包含Xss、SQL及其他攻击负载的著名GitHub仓库，并将它们全部用于我的恶意查询数据集。

现在，我们有两个文件；一个包含干净的网络查询（1000000），另一个包含恶意的网络查询（50000）。这就是我们训练分类器所需的所有数据。

### 训练：

对于训练，我使用了**逻辑回归**，因为它速度快，我需要快速的东西。我们可以使用SVM或神经网络，但它们比逻辑回归稍慢。我们的问题是**二分类问题**，因为我们需要预测一个查询是否恶意。我们将使用**ngrams**作为我们的标记。我阅读了一些研究论文，使用ngrams对于这种项目来说是一个好主意。对于这个项目，我使用了**n=3**。

让我们直接进入代码部分。

让我们定义我们的分词器函数，它将提供3个grams。

```py
def getNGrams(query):
	tempQuery = str(query)
	ngrams = []
	for i in range(0,len(tempQuery)-3):
		ngrams.append(tempQuery[i:i+3])
	return ngrams
```

让我们加载查询数据集。

```py
filename = 'badqueries.txt'
directory = str(os.getcwd())
filepath = directory + "/" + filename
data = open(filepath,'r').readlines()
data = list(set(data))
badQueries = []
validQueries = []
count = 0
for d in data:
	d = str(urllib.unquote(d).decode('utf8')) 
	badQueries.append(d)

filename = 'goodqueries.txt'
directory = str(os.getcwd())
filepath = directory + "/" + filename
data = open(filepath,'r').readlines()
data = list(set(data))
for d in data:
	d = str(urllib.unquote(d).decode('utf8'))
	validQueries.append(d)

```

现在我们已经将数据集加载到好的查询和坏的查询中。让我们尝试可视化它们。我使用了[**主成分分析**](https://en.wikipedia.org/wiki/Principal_component_analysis)来可视化数据集。红色的是坏查询的ngrams，蓝色的是好查询的ngrams。

![](../Images/8ee8424c78c60945e537f716ece59318.png)

我们可以看到，坏点和好点确实在不同的位置出现。让我们继续深入。

```py
badQueries = list(set(badQueries))
tempvalidQueries = list(set(validQueries]))
tempAllQueries = badQueries + tempvalidQueries
bady = [1 for i in range(0,len(tempXssQueries))]
goody = [0 for i in range(0,len(tempvalidQueries))]
y = bady+goody
queries = tempAllQueries
```

现在让我们使用 **Tfidvectorizer** 将数据转换为 **tfidf 值**，然后使用我们的分类器。我们使用 tfidf 值是因为我们想给我们的 ngrams 赋予权重，例如 ngram ‘<img’ 应该有较大的权重，因为包含此 ngram 的查询很可能是恶意的。你可以在这个 [链接](http://www.tfidf.com/) 中了解更多关于 tfidf 的信息。

```py
vectorizer = TfidfVectorizer(tokenizer=getNGrams)
X = vectorizer.fit_transform(queries)
X_train, X_test, y_train, y_test = train_test_split(X, y, 
test_size=0.2, random_state=42)
```

现在我们一切准备就绪，让我们应用逻辑回归。

```py
lgs = LogisticRegression()
lgs.fit(X_train, y_train)
print(lgs.score(X_test, y_test))
```

就这些了。

**这是大家都期待的部分**，你一定想看到准确度，对吧？准确度达到 **99%**。这非常惊人，对吧？但你不会相信，直到你看到一些证据。所以，让我们检查一些查询，看看模型是否将其检测为恶意的。这是结果。

```py
wp-content/wp-plugins (CLEAN)
<script>alert(1)</script> (MALICIOUS)
SELECT password from admin (MALICIOUS)
"> (MALICIOUS)
/example/test.php (CLEAN)
google/images (CLEAN)
q=../etc/passwd (MALICIOUS)
javascript:confirm(1) (MALICIOUS)
"> (MALICIOUS)
foo/bar (CLEAN)
foooooooooooooooooooooo (CLEAN)
example/test/q=<script>alert(1)</script> (MALICIOUS)
example/test/q= (MALICIOUS)
fsecurify/q= (MALICIOUS)
example/test/q= (MALICIOUS)
```

看起来不错，不是吗？它能很好地检测恶意查询。

**接下来呢？** 这是一个周末项目，还有很多可以做或添加的东西。我们可以进行多类分类，以检测恶意查询是 SQL 注入还是跨站脚本攻击或其他任何注入。我们可以拥有一个更大的数据集，包含所有类型的恶意查询，并在其上训练模型，从而扩展其检测的恶意查询类型。还可以将此模型保存并与 Web 服务器一起使用。如果你做了上述任何操作，请告诉我。

**数据和脚本：** [https://github.com/faizann24/F<wbr>waf-Machine-Learning-driven-We<wbr>b-Application-Firewall](https://github.com/faizann24/Fwaf-Machine-Learning-driven-Web-Application-Firewall)

希望你喜欢这篇文章。我们致力于向社区免费提供安全资源。欢迎提出你的意见和批评。

Fsecurify

**简介：[**Faizan Ahmad**](https://www.linkedin.com/in/faizan-ahmad-015964118)** 是现任 Fulbright 本科生，正在拉合尔的 NUCES FAST 学习。

**相关：**

+   [机器学习与网络安全资源](/2017/01/machine-learning-cyber-security.html)

+   [使用机器学习检测恶意 URL](/2016/10/machine-learning-detect-malicious-urls.html)

+   [通过 Databricks 实现 Apache Spark 的端到端安全](/2016/06/achieving-security-apache-spark-databricks.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [KDnuggets 新闻，12 月 14 日：3 个免费的机器学习课程……](https://www.kdnuggets.com/2022/n48.html)

+   [每个机器学习工程师都应掌握的5项机器学习技能](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [数据科学、数据可视化及…的38个顶级Python库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [使用TPOT优化机器学习流程](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [如何处理机器学习中的分类数据](https://www.kdnuggets.com/2021/05/deal-with-categorical-data-machine-learning.html)

+   [谷歌推荐你在参加机器学习课程之前做的事情…](https://www.kdnuggets.com/2021/10/google-recommends-before-machine-learning-data-science-course.html)
