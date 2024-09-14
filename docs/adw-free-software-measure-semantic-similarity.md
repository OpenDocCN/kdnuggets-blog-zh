# ADW，免费测量语义相似性的软件下载工具

> 原文：[`www.kdnuggets.com/2014/10/adw-free-software-measure-semantic-similarity.html`](https://www.kdnuggets.com/2014/10/adw-free-software-measure-semantic-similarity.html)

**作者：Taher Pilehvar（罗马大学），2014 年 10 月。**

![对齐、消歧和行走 (ADW)，用于测量语义相似性的软件下载工具

**对齐、消歧和行走 (ADW)**](https://github.com/pilehvar/ADW)

任意词汇对的语义相似性，从单词意义到文本！

罗马萨比恩扎大学的语言计算实验室很高兴地宣布 ADW 的首次发布。

ADW 是一个用于测量任意词汇对的语义相似性的软件下载工具，从单词意义到文本。该软件基于“对齐、消歧和行走” [1]，这是一种基于 WordNet 的先进语义相似性方法，在 ACL 2013 会议上提出。

ADW 的特点：

+   在多个词汇层次上的先进表现 [1]（数据集如 RG-65 和 TOEFL 上的词汇相似性，句子相似性 - STS 任务，以及 WordNet 意义聚类）。

+   跨级别语义相似性，即不同类型的词汇项之间（例如，一个词义和一个短语）。

+   通过易于使用的 Java API 提供。

+   即插即用：无需任何训练或参数调优。

+   适用于即将到来的 SemEval-2015 任务 1、2 和 3

要获取该软件，请访问 ADW 的 GitHub 仓库：

[`github.com/pilehvar/ADW`](https://github.com/pilehvar/ADW)

你还可以尝试 ADW 的在线演示：

[lcl.uniroma1.it/adw/](http://lcl.uniroma1.it/adw/)

参考资料

[1] M. T. Pilehvar, D. Jurgens 和 R. Navigli. 对齐、消歧和行走：一种测量语义相似性的统一方法。第 51 屆计算语言学协会年会论文集（ACL 2013），保加利亚索非亚，2013 年 8 月 4-9 日，第 1341-1351 页。

*Gregory Piatetsky:* 我尝试了在线演示，语义相似度为

+   “apple”（苹果）和“orange”（橙子）的相似度是 0.47，

+   “tangerine”（橘子）和“orange”（橙子）的相似度是 0.78，

+   “shoe”（鞋子）和“orange”（橙子）的相似度是 0.33

因此计算值必须用于相对比较，而不是绝对意义上的比较。

**相关：**

+   **文本分析入门**

+   **数据科学显示调查可能评估语言而非态度**

+   **Ontotext 文本挖掘、语义搜索和图数据库**

+   **情感分析创新峰会 2014：第一天亮点**

+   **BabelNet 2.5：非常大的多语言百科全书字典和语义网络**

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

### 更多相关话题

+   [评估文档相似性计算方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [了解如何设计、衡量和实施可靠的 A/B 测试…](https://www.kdnuggets.com/2023/01/sphere-design-measure-implement-trustworthy-ab-tests-ronny-kohavi.html)

+   [软件开发者与软件工程师](https://www.kdnuggets.com/2022/05/software-developer-software-engineer.html)

+   [关于语义分割注释的误解](https://www.kdnuggets.com/2022/01/misconceptions-semantic-segmentation-annotation.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [使用向量数据库的语义搜索](https://www.kdnuggets.com/semantic-search-with-vector-databases)
