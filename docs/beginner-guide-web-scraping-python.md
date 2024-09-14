# Python 网页抓取初学者指南

> 原文：[`www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html`](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)

![Python 网页抓取初学者指南](img/637ee3ccbed5ab43272fcd76953ae22d.png)

[图片来自 jemastock](https://www.freepik.com/free-vector/cleaners-with-cleaning-products-housekeeping-service_4887688.htm#query=cleaning&position=3&from_view=search&track=sph) 来自 Freepik

有时需要从网站上收集大量信息，以便用于各种目的。这就是网页抓取，可以通过多种方式实现。一个有效的网页抓取方法是使用一种名为 Python 的编程语言。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT 需求

* * *

本文作为使用 Python 进行网页抓取的初学者指南，介绍了可以使用的不同框架和方法，内容简单明了。

# 什么是网页抓取？

网页抓取是从网站提取数据（主要是非结构化数据），通常是大量的数据。收集后，这些信息被导出为可用的结构化格式，如电子表格或应用程序编程接口（API）。

对于小型数据集，可以手动完成；然而，处理大量数据最好使用自动化系统，因为这更快且成本更低。

对于[网页抓取](https://www.parsehub.com/blog/what-is-web-scraping/)没有一刀切的方法，因为所有网站的规模和形式各不相同。每个网站可能会提供各种需要克服的障碍，如验证码挑战测试，这就是为什么网页抓取工具需要非常多才多艺的原因。

# 网页抓取的目的是什么？

网页抓取器可以用于多种用途。以下是一些最受欢迎的用途：

+   比价购物网站

+   房地产列表

+   潜在客户生成

+   显示行业特定的统计数据和见解

+   当前股票价格、加密货币价格和其他财务数据

+   来自 eBay 和 Amazon 等网站的产品数据

+   赌博网站和幻想联盟的体育统计数据

与任何网络项目一样，遵守法律法规非常重要。这不仅可以避免任何法律行动，还可以确保你的系统更好地防范黑客和网络犯罪。务必确保[你遵循良好的数字公民实践](https://gabbwireless.com/family-resources/what-is-digital-citizenship/)，如保护隐私、定期更换密码以及报告你在网上遇到的任何非法活动。

# 什么是 Python，为什么它用于网络爬虫？

Python 是一种通用计算机编程语言，可以用于各种任务，从构建网站和软件到自动化特定任务甚至机器学习。它与几乎任何类型的程序兼容，并不是为了单一目标而开发的。

## 为什么 Python 是网络爬虫的良好选择？

你应该选择 Python 进行网络爬虫项目的原因有五个。

### 1\. Python 拥有广泛的库选择

Python 拥有大量可以用于你项目的库（库是任何人都可以用来包含在自己程序中的代码段）。Python 库[包括 pandas](https://pandas.pydata.org/)、Matplotlib、Numpy 等等。

这些库可以用于许多不同的功能，非常适合数据处理和网络爬虫项目。

### 2\. Python 相对简单

Python 是最简单的编程语言之一，因为它不使用分号和大括号等符号，使代码不那么复杂。

### 3\. Python 是动态的

Python 可以是动态类型的，这意味着你不需要为 Python 中的变量定义任何数据类型。相反，你可以在需要时插入它们，使过程更加快捷。

### 4\. Python 只需少量代码即可完成复杂任务

网络爬虫的目标是节省时间并快速收集数据，但如果编写代码的过程很长，这个目标也没多大意义。然而，Python 是精简的，只需少量代码即可实现用户的目标。

### 5\. Python 语法可以快速学习

与其他编程语言相比，Python 语法（决定代码如何编写的规则）非常简单易学。每个范围或块在代码中都很容易区分，这使得即使是初学者也能轻松跟随。

# 初学者 Python 网络爬虫指南

在本节中，我们将讨论一些可以帮助构建网络爬虫程序的框架。接下来，我们将展示如何抓取一个电子商务网站的示例方法。

# Python 网络爬虫框架

当使用 Python 进行网络爬虫时，程序可以使用三个框架。这些框架是 Beautiful Soup、Scrapy 和 Selenium。

+   **Beautiful Soup** - Beautiful Soup 框架用于解析 XML 和 HTML 文件以提取数据。在抓取网站时，必须使用 requests 库发送网站请求并接收响应。然后提取 HTML 并传递给 Beautiful Soup 对象进行解析。

+   **Scrapy** - Scrapy 是 Python 使用的顶级 网络爬虫和抓取框架之一，能够有效地爬取网站并提取结构化数据。它可用于许多任务，包括数据挖掘、网站监控和自动化测试。Scrapy 专注于 HTML，通过简单地指定一个 URL 来工作。

+   **Selenium** - Selenium 框架抓取加载动态内容的网站（例如 Facebook、Twitter 和其他社交媒体网站）。它也可以抓取需要登录或注册的网站。

注意！除了上述框架外，你还应该了解数据分析和处理库 Pandas。这个库用于提取数据并将其保存为用户首选的格式。

# 抓取在线购物网站 - 一个示例

对于这种方法，你需要：

+   Ubuntu 操作系统

+   Google Chrome 浏览器

+   安装了 Selenium、Pandas 和 [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 库的 Python 2+ 或 3+。

## 第一步

第一步是找到你想抓取的页面/页面的 URL。在这个例子中，我们将抓取一个最大的电子商务网站，以提取智能手机的价格、名称和评级。

## 第二步

接下来，你应该检查你选择的页面并查看其页面源代码。你寻找的数据通常会在标签内，因此你必须首先确定你想抓取的信息在页面代码中的位置。

在 Google Chrome 中，右键点击网页中的任何元素并点击检查。然后你可以查看页面元素。要查找你的 数据位置，通过右键点击图像或价格然后选择‘查看页面源代码’。

## 第三步

搜索页面源代码以提取你想要的数据。在这种情况下，评级、名称和价格信息将嵌套在“div”标签中。

## 第四步

现在，是时候使用 Python 开发代码了。为此，首先打开 Ubuntu 终端并输入：gedit your file name> 的 .py 扩展名。我们将文件命名为‘web scrape’；因此，命令是：

1\. 现在，是时候使用 Python 开发代码了。为此，首先打开 Ubuntu 终端并输入：gedit your file name> 的 .py 扩展名。我们将文件命名为‘web scrape’；因此，命令是：

```py
gedit web-scrape.py
```

2\. 使用以下命令提取所需的库：

```py
from selenium import webdriver
from BeautifulSoup import BeautifulSoup
import pandas as pd
```

3\. 确保你安装了 Python 3+ 和 Beautiful Soup

4\. 设置 Chrome 驱动程序的路径以使用 Chrome 浏览器：

```py
driver = webdriver.Chrome("/usr/lib/chromium-browser/chromedriver")
```

5\. 接下来，我们需要打开网页并将收集的信息存储为列表：

```py
products = []# store name of the product
prices = []# store price of the product
ratings = []# store rating of the product
driver.get(insert URL)
```

6\. 现在，您已准备好提取数据。输入数据嵌套的 div 标签：

```py
content = driver.page_source
soup = BeautifulSoup(content)
for a in soup.findAll('a', href = True, attrs = {
        'class': '_31qSD5'
    }):
    name = a.find('div', attrs = {
        'class': '_3wU53n'
    })
price = a.find('div', attrs = {
    'class': '_1vC4OE _2rQ-NK'
})
rating = a.find('div', attrs = {
    'class': 'hGSR34 _2beYZw'
})
products.append(name.text)
prices.append(price.text)
ratings.append(rating.text)
```

7\. 运行代码：

```py
python web-scrape.py
```

8\. 将收集的信息以您喜欢的格式保存；在本例中，我们将其保存为 CSV 文件。

```py
df = pd.DataFrame({
    'Product Name': products,
    'Price': prices,
    'Rating': ratings
})
df.to_csv('products.csv', index = False, encoding = 'utf-8')
```

9\. 然后，再次运行程序以完成过程。

正如您所见，通过安装正确的工具并了解简单的命令，网站可以轻松地使用 Python 进行抓取。我们希望您觉得这份指南有用，并能够将上述一些技巧应用到您的下一个网页抓取项目中。

**[Nahla Davies](http://nahlawrites.com/)** 是一名软件开发人员和技术作家。在全职从事技术写作之前，她在一家 Inc. 5,000 的体验品牌组织中担任首席程序员，该组织的客户包括三星、时代华纳、Netflix 和索尼。

### 了解更多相关主题

+   [Python 和 Beautiful Soup 的网页抓取逐步指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [使用 BeautifulSoup 掌握网页抓取](https://www.kdnuggets.com/mastering-web-scraping-with-beautifulsoup)

+   [Octoparse 8.5: 强大的本地抓取及更多](https://www.kdnuggets.com/2022/02/octoparse-85-empowering-local-scraping.html)

+   [预测入门：Python 中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [掌握 GPU：Python 中 GPU 加速数据框的初学者指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)

+   [Python 机器学习初学者指南](https://www.kdnuggets.com/beginners-guide-to-machine-learning-with-python)
