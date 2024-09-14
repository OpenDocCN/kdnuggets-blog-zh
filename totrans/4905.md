# 使用 Python 进行数据科学的网络爬虫

> 原文：[https://www.kdnuggets.com/2017/12/baesens-web-scraping-data-science-python.html](https://www.kdnuggets.com/2017/12/baesens-web-scraping-data-science-python.html)

**由 [Seppe vanden Broucke 和 Bart Baesens](http://www.webscrapingfordatascience.com/)** 赞助的帖子。

> 对于那些不熟悉编程或网络深层工作原理的人来说，网络爬虫常常看起来像是一种黑魔法：能够编写一个独立探索互联网并收集数据的程序被视为一种神奇而令人兴奋的能力。在 [![Web Scraping for Data Science with Python](../Images/a0d81c168534c1bf8d2466965719e467.png) **使用 Python 进行数据科学的网络爬虫**](https://www.amazon.com/Web-Scraping-Data-Science-Python/dp/1979343780/ref=sr_1_6?ie=UTF8&qid=1512404325&sr=8-6) 中，我们提供了一个简洁但全面且现代的网络爬虫指南，以 Python 作为编程语言。此外，这本书还考虑了数据科学受众的需求。
> 
> 我们自己也是数据科学家，发现网络爬虫常常是你工具箱中的一项强大工具，因为许多数据科学项目的第一步是获得合适的数据集，为什么不利用网络提供的信息宝藏呢？这本书采用了“优先编码”的方法，让你迅速上手，没有太多的样板文本，展示了如何处理现代网络，包括 JavaScript、cookies 和常见的网络爬虫对策，并包括了关于网络爬虫的详细管理和法律讨论。我们还提供了大量的进一步阅读和学习的指引，并包含了十四个现实生活中的完整示例。[有关更多细节，请点击这里](https://www.amazon.com/Web-Scraping-Data-Science-Python/dp/1979343780/ref=sr_1_6?ie=UTF8&qid=1512404325&sr=8-6)。

在这篇文章中，我们简要探讨了网络爬虫在数据科学项目中的实用性。网络“爬虫”（也称为“网络采集”、“网络数据提取”或甚至“网络数据挖掘”）可以定义为“构建一个代理程序以自动方式从网络下载、解析和组织数据”。换句话说：与其让人类最终用户在浏览器中点击并将感兴趣的部分复制粘贴到电子表格中，不如将这项任务交给计算机程序，后者可以比人类更快、更准确地执行这项任务。

从互联网自动收集数据的做法可能和互联网本身一样久远，甚至“抓取”（"scraping"）这个术语早在网络出现之前就已经存在。在“网页抓取”（"web scraping"）这一术语流行之前，一种被称为“屏幕抓取”（"screen scraping"）的做法已经作为一种从视觉表现中提取数据的方式被广泛采用——在计算机的早期（想象一下 1960 年代至 1980 年代），这通常涉及到简单的文本型“终端”。正如今天一样，那时的人们已经对从这些终端中抓取大量文本以备后用感兴趣。当你使用普通的网页浏览器浏览网页时，可能会遇到多个你考虑过收集、存储和分析页面上数据的网站。对于数据科学家来说，数据是他们的“原材料”，网络展示了很多有趣的机会。在这种情况下，使用网页抓取可能会派上用场。如果你能在网页浏览器中查看某些数据，你就能够通过程序访问和提取这些数据。如果你能通过程序访问这些数据，那么数据可以被存储、清理，并以任何方式使用。无论你的兴趣领域是什么，总是有几乎总能利用数据改进或丰富你的实践的应用场景。俗话说“数据是新石油”，网络上有大量的数据。

在这篇文章中，我们的目标是构建一个 S&P 500 公司及其通过董事会成员互联的社交图谱。我们将从 [路透社的 S&P 500 页面](https://www.reuters.com/finance/markets/index/.SPX) 开始，获取一个符号列表：

```py

from bs4 import BeautifulSoup
import requests
import re

session = requests.Session()

sp500 = 'https://www.reuters.com/finance/markets/index/.SPX'

page = 1
regex = re.compile(r'/finance/stocks/overview/.*')
symbols = []

while True:
  print('Scraping page:', page)
  params = params={'sortBy': '', 'sortDir' :'', 'pn': page}
  html = session.get(sp500, params=params).text
  soup = BeautifulSoup(html, "html.parser")
  pagenav = soup.find(class_='pageNavigation')
  if not pagenav:
    break
  companies = pagenav.find_next('table', class_='dataTable')
  for link in companies.find_all('a', href=regex):
    symbols.append(link.get('href').split('/')[-1])
  page += 1

print(symbols)

```

一旦获得了符号列表，我们可以访问每个符号的董事会成员页面（例如 [https://www.reuters.com/finance/stocks/company-officers/MMM.N](https://www.reuters.com/finance/stocks/company-officers/MMM.N)），提取出董事会成员的表格，并将其存储为 pandas 数据框：

```py

from bs4 import BeautifulSoup
import requests
import pandas as pd

session = requests.Session()

officers = 'https://www.reuters.com/finance/stocks/company-officers/{symbol}'

symbols = ['MMM.N', [...], 'ZTS.N']
dfs = []

for symbol in symbols:
  print('Scraping symbol:', symbol)
  html = session.get(officers.format(symbol=symbol)).text
  soup = BeautifulSoup(html, "html.parser")
  officer_table = soup.find('table', {"class" : "dataTable"})
  df = pd.read_html(str(officer_table), header=0)[0]
  df.insert(0, 'symbol', symbol)
  dfs.append(df)

df = pd.concat(dfs)
df.to_pickle('data.pkl')

```

这种信息可以引发许多有趣的应用场景，尤其是在图形和社交网络分析领域。例如，我们可以利用收集到的信息导出一个图形，并使用 Gephi 这款流行的图形可视化工具进行可视化：

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 方面的工作

* * *

```py

import pandas as pd
import networkx as nx
from networkx.readwrite.gexf import write_gexf

df = pd.read_pickle('data.pkl')

G = nx.Graph()

for row in df.itertuples():
  G.add_node(row.symbol, type='company')
  G.add_node(row.Name,type='officer')
  G.add_edge(row.symbol, row.Name)

write_gexf(G, 'graph.gexf')

```

输出文件可以在Gephi中打开、过滤和修改。下图展示了苹果、谷歌和亚马逊的3阶自网络的快照，显示它们确实是相互连接的：

![Sp500 亚马逊 苹果 谷歌 网络](../Images/237f9e2706af8cf93501923c07c4a2f1.png)

**进一步阅读**

我们的书《["Python数据科学中的网页抓取"](https://www.amazon.com/Web-Scraping-Data-Science-Python/dp/1979343780/ref=sr_1_6?ie=UTF8&qid=1512404325&sr=8-6)》即将发布，旨在帮助希望在工作流程中采用网页抓取技术的数据科学家。请关注[www.webscrapingfordatascience.com/](http://www.webscrapingfordatascience.com/)以获取更多信息。

图表也可以在预测设置中以多种方式使用。有关此主题的更多阅读资料，请参见：

+   [www.dataminingapps.com/dma_research/fraud-analytics/](http://www.dataminingapps.com/dma_research/fraud-analytics/)

+   Node2Vec是一种强大的特征化技术，将图中的节点转换为特征向量：[https://snap.stanford.edu/node2vec/](https://snap.stanford.edu/node2vec/)

+   个性化Pagerank在例如流失和欺诈分析的背景中非常常用作为特征化方法：[https://www.r-bloggers.com/from-random-walks-to-personalized-pagerank/](https://www.r-bloggers.com/from-random-walks-to-personalized-pagerank/)

+   Van Vlasselaer, V., Akoglu, L., Eliassi-Rad, T., Snoeck, M., Baesens, B. (2015). Guilt-by-constellation: fraud detection by suspicious clique memberships. 48届夏威夷国际系统科学会议论文集：第接受卷。HICSS-48。考艾岛（夏威夷），2015年1月5-8日

+   Van Vlasselaer, V., Akoglu, L., Eliassi-Rad, T., Snoeck, M., Baesens, B. (2014). 在大型欺诈网络中寻找团体：理论与见解。国际运筹学学会会议（IFORS 2014）。巴塞罗那（西班牙），2014年7月13-18日。

+   Van Vlasselaer, V., Akoglu, L., Eliassi-Rad, T., Snoeck, M., Baesens, B. (2014). Gotch'all! 先进的网络分析用于检测欺诈团体。PAW（预测分析世界）。伦敦（英国），2014年10月29-30日

+   Van Vlasselaer, V., Van Dromme, D., Baesens, B. (2013). 社会网络分析用于检测社会保障欺诈中的蜘蛛构造：新见解和挑战：第接受卷。欧洲运筹学会议。罗马（意大利），2013年7月1-4日

+   Van Vlasselaer, V., Meskens, J., Van Dromme, D., Baesens, B. (2013). 使用社会网络知识检测社会保障欺诈中的蜘蛛构造。2013 IEEE/ACM国际社交网络分析与挖掘会议论文集。ASONAM。尼亚加拉大瀑布（加拿大），2013年8月25-28日（第813-820页）。445 Hoes Lane, PO Box 1331, Piscataway, NJ 08855-1331, USA: IEEE计算机协会

**个人简介：塞佩·范登·布鲁克**是比利时鲁汶大学经济与商学院的助理教授。他的研究兴趣包括商业数据挖掘与分析、机器学习、流程管理和流程挖掘。他的研究成果已发表在知名国际期刊上，并在顶级会议上展示。塞佩的教学内容包括高级分析、大数据和信息管理课程。他也经常为行业和商业观众授课。

**巴特·贝森斯**是比利时鲁汶大学（KU Leuven）的大数据与分析教授，同时也是英国南安普顿大学的讲师。他在大数据与分析、信用风险建模、欺诈检测和市场分析方面进行了广泛的研究。他著有8本书和200多篇科学论文，其中一些发表在知名国际期刊上，并在顶级国际会议上展示。他曾获得各种最佳论文和最佳演讲奖项。他的研究总结可以在 [www.dataminingapps.com](http://www.dataminingapps.com) 找到。

### 相关话题

+   [使用 Python 的初学者网页抓取指南](https://www.kdnuggets.com/2022/10/beginner-guide-web-scraping-python.html)

+   [使用 Python 和 Beautiful Soup 的逐步网页抓取指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [精通 BeautifulSoup 的网页抓取](https://www.kdnuggets.com/mastering-web-scraping-with-beautifulsoup)

+   [Octoparse 8.5: 探索本地数据抓取及更多功能](https://www.kdnuggets.com/2022/02/octoparse-85-empowering-local-scraping.html)

+   [创建一个用 Python 从音频中提取主题的网页应用](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [5 分钟用 Python 构建一个网页抓取器](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)
