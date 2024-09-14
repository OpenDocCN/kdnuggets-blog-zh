# 使用机器学习检测恶意 URL

> 原文：[https://www.kdnuggets.com/2016/10/machine-learning-detect-malicious-urls.html](https://www.kdnuggets.com/2016/10/machine-learning-detect-malicious-urls.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Faizan Ahmad，Fsecurify CEO**。

![Header](../Images/cd0f8aa5892b3c56ece07f70ea6880e8.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

随着过去几年机器学习的增长，许多任务都借助**机器学习**算法来完成。不幸或幸运的是，关于机器学习算法在安全方面的工作很少。因此，我想在[**Fsecurify**](http://fsecurify.com/using-machine-learning-detect-malicious-urls/)上展示一些内容。

几天前，我有了一个想法：如果我们可以使用某种机器学习算法从非恶意 URL 中检测恶意 URL 会怎样。对此主题已经有一些研究，所以我认为应该尝试一下，从零开始实现一些东西。让我们开始吧。

**数据收集**

第一个任务是收集数据。我做了一些搜索，发现了一些提供恶意链接的网站。我设置了一个小爬虫，爬取了许多来自不同网站的恶意链接。接下来的任务是寻找清晰的 URL。幸运的是，我不需要爬取任何数据。已经有一个数据集可用。如果我没有提及数据来源，请不要担心。你将在本文末尾获得数据。

所以，我收集了大约 400,000 个 URL，其中大约 80,000 个是恶意的，其他的是干净的。这就是我们的数据集。接下来我们继续。

**分析**

我们将使用**逻辑回归**，因为它速度很快。第一步是对 URL 进行标记化。我为此编写了自己的标记化函数，因为 URL 与其他文档文本有所不同。

![](../Images/b135c873cc912a9e67d1f47ab1ef5933.png)

下一步是加载数据并将其存储到列表中。

![](../Images/3665cea6de0ac3f99fe7a10adb4756cc.png)

既然我们已经将数据放入列表中，我们需要对 URL 进行向量化。我使用了**tf-idf** 分数，而不是使用词袋分类，因为 URL 中的某些词比其他词更重要，例如 *‘virus’, ‘.exe’, ‘.dat’* 等。让我们将 URL 转换为向量形式。

![](../Images/f23c14315a9184249d32aea98f46a722.png)

我们有了向量。现在让我们将其转换为测试和训练数据，并进行逻辑回归分析。

![](../Images/03275d8ba40e4c9ba4b99e9f91593803.png)

就是这样。看，它简单却有效。**我们获得了98%的准确率**。这是一个非常高的值，能让机器有效检测恶意网址。想测试一些链接看看模型的预测是否准确吗？当然。我们来试试。

![](../Images/1990f5237d5f3272d6e1375bc5d176e8.png)

结果非常惊人。

+   wikipedia.com **（好网址）**

+   google.com/search=faizanahmad **（好网址）**

+   pakistanifacebookforever.com/getpassword.php/ **（坏网址）**

+   www.radsport-voggel.de/wp-admin/includes/log.exe **（坏网址）**

+   ahrenhei.without-transfer.ru/nethost.exe **（坏网址）**

+   www.itidea.it/centroesteticosothys/img/_notes/gum.exe **（坏网址）**

这正是一个人类可能预测的结果。不是吗？

数据和代码可在[**Github**](https://github.com/faizann24/Using-machine-learning-to-detect-malicious-URLs)获取。

**简历： [Faizan Ahmad](http://www.fsecurify.com/)** 是一名富布赖特计算机科学本科生，同时也是Fsecurify的首席执行官。

**相关：**

+   [大数据安全的5个最佳实践](/2016/06/5-best-practices-big-data-security.html)

+   [数据科学家如何缓解敏感数据暴露的漏洞？](/2016/08/data-scientists-mitigate-sensitive-data-exposure-vulnerability.html)

+   [大数据与数据科学在安全和欺诈检测中的应用](/2015/12/big-data-science-security-fraud-detection.html)

### 更多相关内容

+   [识别虚假数据科学家：20个问题（附答案）ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别虚假数据科学家：20个问题（附答案）ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [为什么越来越多的开发者选择Python进行机器学习项目？](https://www.kdnuggets.com/2022/01/developers-python-machine-learning-projects.html)

+   [使用SHAP值提升机器学习模型的可解释性](https://www.kdnuggets.com/2023/08/shap-values-model-interpretability-machine-learning.html)

+   [用基础和现代算法解决计算机科学问题](https://www.kdnuggets.com/2023/11/packt-tackle-computer-science-problems-fundamental-modern-algorithms-machine-learning)

+   [构建GPU机器与使用GPU云服务](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)
