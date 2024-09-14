# 你可以使用 ChatGPT 视觉进行数据分析的 5 种方法

> 原文：[https://www.kdnuggets.com/5-ways-you-can-use-chatgpt-vision-for-data-analysis](https://www.kdnuggets.com/5-ways-you-can-use-chatgpt-vision-for-data-analysis)

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/1005a3fa050ef56fd90c8581e3bb4aa0.png)

图片来源：作者

数据分析是做出数据驱动决策的关键部分，无论是在商业、研究还是日常生活中。它涉及从数据中提取见解和模式，以便深入理解潜在的信息。随着 ChatGPT 新视觉功能的引入，数据分析取得了重大进展。ChatGPT 视觉允许用户解读图像、方程式、图表和曲线图，为从视觉数据中提取见解开辟了广泛的可能性。

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

在本文中，我们将探讨 ChatGPT 视觉可以用于数据分析任务的 5 种关键方法。

# 1\. SQL 表

现在，你可以简单地截图数据集，并要求 ChatGPT 为你编写 SQL 查询。

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/b34d1308122a24f19f0ce5a0d1deaec0.png)

演示数据库来自 [programiz.com](https://www.programiz.com/sql/online-compiler/)

**提示：**

```py
I have uploaded three tables. Please write an SQL query to determine whether John has received his keyboard.
```

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/7335f4716255b61ee1aadd157c8218a8.png)

正如我们所见，SQL 查询运行得非常完美，我也得到了我的答案（待处理）。

```py
SELECT s.status
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
JOIN Shippings s ON o.order_id = s.shipping_id
WHERE c.first_name = 'John' AND o.item = 'Keyboard';
```

**结果：**

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/d6415a0ea6822b073f28ae644cf09866.png)

ChatGPT 视觉功能使非技术经理能够对多个关系表和复杂问题进行查询。

# 2\. 图表分析

使用 ChatGPT 视觉进行图表分析是理解每个图表所传达的信息的最佳方式。

在我们的案例中，我们提供了来自 [COVID19 期间数字学习演变](https://www.kaggle.com/code/kingabzpro/evolution-of-digital-learning-during-covid19) 笔记本的多张数据分析图像，并要求 ChatGPT 为我们编写详细报告。

**提示：**

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/941ab0ae11cd222482370a9b1bc0b010.png)

![你可以使用 ChatGPT 视觉进行数据分析的 5 种方法](../Images/9bb591d39c483b99c19bb80fedaf2177.png)

作为数据科学家，制作一个合适的数据分析报告通常需要整整一天的时间。然而，借助ChatGPT，我们能够在一分钟内完成报告。它能够识别出我在初步分析中遗漏的隐藏模式。再次证明，ChatGPT Vision是一个宝贵而可靠的助手。

# 3\. 仪表板

接下来，我们将提供[超级样本超级商店](https://www.tableau.com/data-insights/dashboard-showcase/superstore)仪表板的更复杂图像，以帮助理解每个组件及其含义。

**提示：**

```py
Can you please explain each dashboard section in detail? 
```

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/30f873436dd30587a3eef8e3f08b9fc3.png)

ChatGPT通过一个简单的提示提供了详细的仪表板解释。此外，它解释了仪表板上的数字和图表，如KPI、趋势和区域比较。

# 4\. 评估

我经常在评估结果和理解结果时遇到困难。例如，当我尝试确定KMeans算法的最佳聚类数时，使用[俄罗斯酒类促销](https://deepnote.com/@abid/Alcoholic-Drinks-Promotion-in-Russia-4ecfda78-7305-42b1-921a-f6c710801051)数据集。所以，我会提供一个肘部图给ChatGPT Vision，并让它为我选择一个数字，而不是检查多个聚类。

**提示：**

```py
I have uploaded the elbow plot to figure out the optimal number of clusters for the KMeans algorithm. Please pick the number for me. 
```

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/8928e90077494bb4d637423f3b5b8385.png)

你还可以利用这个新功能来更好地理解机器学习结果。例如，理解[移动价格分类](https://deepnote.com/@abid/Getting-Started-with-SHAP-Values-3e9de750-8212-4ff3-979c-e14a916ac919)模型的分类报告。

**提示：**

```py
I have uploaded the classification report of Mobile Price Classification. Can you please explain the result? 
```

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/8e02bfcb22a9ce9bdc5078bb32e2716c.png)

利用这些结果，我可以轻松向我的非技术经理和利益相关者解释我们的初步结果。这使我的生活变得简单。

# 5\. 方程式

ChatGPT Vision的最佳用法是用来理解研究论文、网站、视频和博客上的各种数学方程式。你可以直接截图方程式，并要求ChatGPT用简单的词汇为你解释。例如，我们要求它解释[奇异值分解](/2023/05/principal-component-analysis-pca-scikitlearn.html)的方程式。

**提示：**

```py
Can you explain the singular value decomposition using the uploaded image of equations
```

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/cc4a463c548f493caf7fcac83c56e539.png)

另一个例子是，我们将要求ChatGPT将[基于人类偏好的微调语言模型](https://arxiv.org/pdf/1909.08593.pdf)研究论文中的奖励函数转换为Latex。

**提示：**

```py
Can you convert the reward function equation into latex?
```

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/a747e505ab50da3daea17b1f54a1d767.png)

如你所见，生成的Latex代码效果完美。

![你可以利用ChatGPT Vision进行数据分析的5种方法](../Images/463795cea13b127d0037bcae70cb060b.png)

来自 [Codecogs](https://editor.codecogs.com/) 的截图

# 最后的想法

凭借其视觉解释技能，ChatGPT 已成为数据科学家、分析师、研究人员甚至非技术专业人员在数据处理中的宝贵助手。它消除了手动分析的需求，并加快了从数据到洞察的过程。随着能力的不断提升，ChatGPT Vision 承诺将彻底改变我们处理和理解数据的方式。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证数据科学专业人员，热衷于构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络构建一个帮助面临心理健康问题的学生的 AI 产品。

### 更多相关主题

+   [你可以使用 ChatGPT 代码解释器进行数据科学的 5 种方法](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)

+   [7 种方法让 ChatGPT 帮助你编写更好更快的代码](https://www.kdnuggets.com/2023/06/7-ways-chatgpt-makes-code-better-faster.html)

+   [企业如何从机器学习中受益的 6 种方式](https://www.kdnuggets.com/2022/08/6-ways-businesses-benefit-machine-learning.html)

+   [如何利用机器学习自动标注数据](https://www.kdnuggets.com/2022/02/machine-learning-automatically-label-data.html)

+   [你需要了解的 6 件数据管理事项及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [利用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)
