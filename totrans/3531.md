# 机器学习大战：Amazon 对抗 Google 对抗 BigML 对抗 PredicSis

> 原文：[https://www.kdnuggets.com/2015/05/machine-learning-wars-amazon-google-bigml-predicsis.html/2](https://www.kdnuggets.com/2015/05/machine-learning-wars-amazon-google-bigml-predicsis.html/2)

Kaggle 竞赛中的大致排名

+   Amazon 排名 #60

+   PredicSis 排名 #570

+   BigML 排名 #770

+   Google 排名 #810

重要的是要注意，根据你的应用需求，这三种性能指标中的某些可能比其他指标更为关键。这个 Kaggle 挑战的排行榜并没有考虑时间，但在某些需要高频预测的应用中（例如当你需要预测用户是否会点击广告时，对于每个访问高流量网站的用户），预测时间将变得极其重要。

**摘要**

***免责声明**：此比较使用了真实世界的数据集，但使用其他数据集可能会得到不同的结果。你应该用自己的数据试用这些 API 以找出最适合你的！*

+   PredicSis 提供了精度和速度之间的最佳折衷，成为第二快和第二准确的。

+   BigML 在训练和预测中都最快，但精度较低。

+   Amazon 最准确，但代价是训练速度最慢，预测也非常慢。

+   Google 在精度和预测时间上都排在最后。

**关于实际基准测试**

这次比较非常简单，但仍然距离实际基准测试有些差距。我希望改善的一件事是让其他人更容易复制（和验证）这些结果。我使用了这些服务的网页接口来获取 AUC 值，最好能有能在本地计算 AUC 的代码。目前，你可以查看这个 [评估 ML/预测 API 的库](https://github.com/louisdorard/papiseval)。欢迎提交拉取请求！（例如新的 API、新的评估指标等。）

在未来的基准测试中，尝试回归问题以及各种类型的数据集：小型、大型、不平衡等，将会很有趣。

**了解更多**

+   如果你想了解更多关于 PredicSis 和 BigML 的信息，它们将在 5月21日于巴黎的 [PAPIs Connect](http://www.papis.io/connect) 会议上出现——欢迎来加入我们！

+   BigML 也将在 [APIdays Mediterranea](http://mediterranea.apidays.io/) 参加会议，日期是5月7日，地点是巴塞罗那，他们的首席技术官将就 ML API 的未来进行精彩演讲。

+   我正在赠送两个会议的免费票！ [在此注册 PAPIs Connect](https://docs.google.com/forms/d/1Nrn6u0Z7BrRxV1MFFjuGolg36s2Olwfdk7PtdflgBBU/viewform?c=0&w=1) 和 [在此注册 APIdays Mediterranea](https://docs.google.com/forms/d/1l7xXHK6OxdmM2jMky9kwzrqsl0LuSm3jBs9Y4ZUvjPg/viewform?c=0&w=1)。

+   通过这些新的机器学习/预测 API，我正在考虑更新我的书籍，[Bootstrapping Machine Learning](http://www.louisdorard.com/machine-learning-book)，书中已经涵盖了 Google 预测和 BigML…但在那之前，你可能会对在我的机器学习入门套件中查看当前版本的摘录感兴趣！

在此获取[机器学习入门套件](http://www.louisdorard.com/blog/machine-learning-apis-comparison)

个人简介：[路易斯·多拉德](http://www.louisdorard.com/#home)，[@louisdorard](https://twitter.com/louisdorard) 是一名独立顾问及[PAPIs.io](http://www.papis.io/)国际会议的总主席，专注于预测 API 和应用。他还是《Bootstrapping Machine Learning》一书的作者。路易斯拥有伦敦大学学院的机器学习博士学位。

[原文](http://www.louisdorard.com/blog/machine-learning-apis-comparison)。

**相关内容：**

+   [云机器学习大战：亚马逊 vs IBM Watson vs 微软 Azure](/2015/04/cloud-machine-learning-amazon-ibm-watson-microsoft-azure.html)

+   [BigML 机器学习平台 2015 年冬季发布，2 月 11 日](/2015/02/bigml-machine-learning-platform-winter-release.html)

+   [Google BigQuery 公共数据集](/2015/02/google-bigquery-public-datasets.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [使用 Python 为亚马逊产品构建推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [了解如何设计、测量和实施值得信赖的 A/B 测试…](https://www.kdnuggets.com/2023/01/sphere-design-measure-implement-trustworthy-ab-tests-ronny-kohavi.html)

+   [学习生成 AI 的免费亚马逊课程：适合所有水平](https://www.kdnuggets.com/free-amazon-courses-to-learn-generative-ai-for-all-levels)

+   [Google 推荐你在参加他们的机器学习…](https://www.kdnuggets.com/2021/10/google-recommends-before-machine-learning-data-science-course.html)

+   [7 门免费 Google 课程，助你成为机器学习工程师](https://www.kdnuggets.com/7-free-google-courses-to-become-a-machine-learning-engineer)

+   [Google 免费的生成 AI 学习路径](https://www.kdnuggets.com/2023/07/free-google-generative-ai-learning-path.html)
