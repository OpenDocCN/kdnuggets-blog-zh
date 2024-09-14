# 使用 Python 进行的电子商务数据分析以制定销售策略

> 原文：[https://www.kdnuggets.com/2021/04/e-commerce-data-analysis-sales-strategy-python.html](https://www.kdnuggets.com/2021/04/e-commerce-data-analysis-sales-strategy-python.html)

[评论](#comments)

**由 [Juhi Sharma](https://www.linkedin.com/in/juhi-sharma-ds/<code>)，产品分析师**

![图](../Images/bf7dd9db63df30276ccbe6a200ef0e48.png)

来源 — [https://www.wvgazettemail.com/](https://www.wvgazettemail.com/)

### 介绍

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

Kmart 是美国领先的在线零售商，作为其年度销售回顾会议的一部分，他们需要根据2019年的销售数据洞察来决定2020年的销售策略。

数据与2019年每个月的销售有关，任务是生成关键洞察，以帮助 Kmart 的**销售团队**做出有关优化销售策略的重要业务决策。

### 数据理解

1.  数据属于 Kmart - 美国领先的在线零售商。

1.  时间范围 — 2019年1月 — 2019年12月

1.  唯一产品 — 19

1.  总订单数 — 178437

1.  城市 — 9

1.  KPI’s — 总销售额，总销售产品数

![](../Images/fc97de29a2918a55e5979a4c211b07d1.png)

来源 — 作者

### 商业问题陈述

1.  销售最佳的月份是哪一个？那个时候赚了多少钱？

1.  哪个城市的销售数量最高？

1.  推荐展示广告的最佳时间，以最大化客户购买产品的可能性？

1.  什么产品卖得最多？你认为为什么卖得最好？

### 使用 Python 进行数据分析

1.  加载了每个月的数据，并使用 pandas 制作了数据框

1.  合并数据集以制作一个2019年销售数据的数据集。

1.  处理空值和垃圾数据。

1.  数据预处理后制作了一个筛选的数据集

1.  分析和解决商业问题。（使用 matplotlib 和 seaborn 库进行可视化）

### 1\. 导入库

```py
import pandas as pd
```

### 2\. 加载数据集并制作数据框

```py
df1=pd.read_csv("Sales_January_2019.csv")
df1["month"]="Jan"
df2=pd.read_csv("Sales_February_2019.csv")
df2["month"]="feb"
df3=pd.read_csv("Sales_March_2019.csv")
df3["month"]="mar"
df4=pd.read_csv("Sales_April_2019.csv")
df4["month"]="apr"
df5=pd.read_csv("Sales_May_2019.csv")
df5["month"]="may"
df6=pd.read_csv("Sales_June_2019.csv")
df6["month"]="june"
df7=pd.read_csv("Sales_July_2019.csv")
df7["month"]="july"
df8=pd.read_csv("Sales_August_2019.csv")
df8["month"]="aug"
df9=pd.read_csv("Sales_September_2019.csv")
df9["month"]="sep"
df10=pd.read_csv("Sales_October_2019.csv")
df10["month"]="oct"
df11=pd.read_csv("Sales_November_2019.csv")
df11["month"]="nov"
df12=pd.read_csv("Sales_December_2019.csv")
df12["month"]="dec"list=[df1,df2,df3,df4,df5,df6,df7,df8,df9,df10,df11,df12]
```

### 3\. 每个月的数据集的形状

```py
for i in list:
    print(i.shape)
```

![](../Images/e593956194ad706c433425d6f21d21b0.png)

来源 — 作者

### 4\. 合并数据集

```py
frame=pd.concat(list)
```

![](../Images/c6906550d4a1c87636dde07e8be87d14.png)

来源 — 作者

### 5\. 最终数据集的列

```py
frame.columns
```

![](../Images/f9318bf578a2311a67f37443c0eaf3f8.png)

来源 — 作者

### 6\. 数据框信息

```py
frame.info()
```

![](../Images/033d03f8b338ad6e93c10eb2cc7bb108.png)

来源 — 作者

### 7\. 数据集中的空值

```py
frame.isnull().sum() # there are 545 null values in each column except month
```

![](../Images/4d315872e6e2a94f5d20d4737352f10e.png)

来源-作者

```py
(frame.isnull().sum().sum())/len(frame)*100  # we have 1.75 percent null values , so we can drop them
```

![](../Images/5de95e180d7ec9acfbf22ecd9867e9a1.png)

来源-作者

### 8\. 删除空值

```py
frame=frame.dropna()
frame.isnull().sum()
```

![](../Images/18e2c353cb15e087a1821e2c6e52b886.png)

来源-作者

### 9\. 删除垃圾数据

我们观察到有355列中的行值与标题相同，因此创建一个新的数据框，将这些值排除在外。

```py
frame[frame['Quantity Ordered'] == "Quantity Ordered"]
```

![](../Images/228929931269f8362444b6a4834b9334.png)

```py
df_filtered = frame[frame['Quantity Ordered'] != "Quantity Ordered"] 
df_filtered.head(15) 
df_filtered.shape
```

![](../Images/7da3808b46bae01f931117ed513eb33e.png)

来源-作者

### 解决商业问题

**Q 1\. 哪个月的销售最好？那个时候赚了多少钱？**

```py
df_filtered["Quantity Ordered"]=df_filtered["Quantity Ordered"].astype("float")
df_filtered["Price Each"]=df_filtered["Price Each"].astype("float")# Creating Sales Column By multiplying Quantity Ordered and Price of Each Productdf_filtered["sales"]=df_filtered["Quantity Ordered"]*df_filtered["Price Each"]
```

![](../Images/ec137ea3ef442afd506b66b0670c7049.png)

来源-作者

```py
month=["dec","oct","apr","nov","may","mar","july","june","aug",'feb',"sep","jan"] 
df["month"]=monthfrom matplotlib import pyplot as plt
a4_dims = (11.7, 8.27)
fig, ax = pyplot.subplots(figsize=a4_dims)
import seaborn as sns
sns.barplot(x = "sales",
            y = "month",
            data = df)
plt.title("Month wise Sale")
plt.show()
```

![](../Images/1bcf676c7f980fdb4a880486c2e094b3.png)

来源-作者

销售最佳的月份是12月。

12月的总销售额为 $ 4619297。

**Q 2\. 哪个城市的销售额最高？**

```py
dftemp = df_filtered
list_city = []
for i in dftemp['Purchase Address']:
    list_city.append(i.split(",")[1])
dftemp['City'] = list_city
dftemp.head()
```

![](../Images/7ff44c98c75e2ee64708515d4e5c09ab.png)

来源-作者

```py
df_city=df_filtered.groupby(["City"])['sales'].sum().sort_values(ascending=False)
df_city=df_city.to_frame()
df_city
```

![](../Images/da07bec9025e8b5ba27158194000439f.png)

来源-作者

```py
city=["San Francisco","Los Angeles","New York City","Boston","Atlanta","Dallas","Seattle","Portland","Austin"]
df_city["city"]=cityfrom matplotlib import pyplot
a4_dims = (11.7, 8.27)
fig, ax = pyplot.subplots(figsize=a4_dims)
sns.barplot(x = "sales",
            y = "city",
            data = df_city)
plt.title("City wise Sales")
plt.show()
```

![](../Images/3a96717d8e5cc881c14304f949689d60.png)

来源-作者

旧金山的销售额最高，大约为 $8262204。

**Q 3 哪些产品卖得最多？**

```py
print(df_filtered["Product"].unique())
print(df_filtered["Product"].nunique())
```

![](../Images/c231220300186229151ac418984d1423.png)

来源-作者

```py
df_p=df_filtered.groupby(['Product'])['Quantity Ordered'].sum().sort_values(ascending=False).head()
df_p=df_p.to_frame()
df_p
```

![](../Images/4c1ad0466d355bc86f233ca28bc848de.png)

来源-作者

```py
product=["AAA Batteries (4-pack)","AA Batteries (4-pack)","USB-C Charging Cable","Lightning Charging Cable","Wired Headphones"]
df_p["Product"]=productfrom matplotlib import pyplot
a4_dims = (11.7, 8.27)
fig, ax = pyplot.subplots(figsize=a4_dims)
sns.barplot(x = "Quantity Ordered",
            y = "Product",
            data = df_p)
plt.title("Prouct and Quantity Ordered")
plt.show()
```

![](../Images/8ed745c019b7bb0ecd24417dd66b02da.png)

来源-作者

一年内销售了31017.0数量的AAA电池（4包）。它销量最大，因为它是最便宜的产品。

**Q 4 什么时候展示广告最合适，以最大化客户购买产品的可能性？**

```py
dftime = df_filtered
list_time = []
for i in dftime['Order Date']:
    list_time.append(i.split(" ")[1])
dftime['Time'] = list_time
dftime.head()
```

![](../Images/fac960006954ebce29b86097355cc91a.png)

来源-作者

```py
df_t=df_filtered.groupby(['Time'])['sales'].sum().sort_values(ascending=False).head()
df_t=df_t.to_frame()
df_t
```

![](../Images/dff4673b462152af0d0e07c02c24a917.png)

来源-作者

```py
df_t.columns
```

![](../Images/089001f2000239da1697df648ea01488.png)

来源-作者

### 在你离开之前

*感谢阅读！如果你想联系我，请随时通过 jsc1534@gmail.com 或我的*[*LinkedIn 个人资料*](http://www.linkedin.com/in/juhi-sharma-ds)*与我联系。你还可以在我的*[*GitHub*](https://github.com/jsc1535/K-Mart-Data-Analysis)*账户上找到这篇文章的代码以及一些非常有用的数据科学项目。*

**简介: [Juhi Sharma](https://www.linkedin.com/in/juhi-sharma-ds/)** ([Medium](https://juhi95.medium.com/), [GitHub](https://github.com/jsc1535)) 具有2年以上分析师工作经验，涉及项目管理、业务分析和客户处理。目前，Juhi在一家产品公司担任数据分析师。Juhi拥有分析数据集、创建机器学习和深度学习模型的实际经验。Juhi热衷于通过数据驱动的方法解决商业问题。

[原文](https://pub.towardsai.net/ecommerce-data-analysis-for-sales-strategy-using-python-5b026fd36a6e). 经许可转载。

**相关内容：**

+   [Pandas Profiling: 一行代码的魔法数据分析](/2021/02/pandas-profiling-one-line-magical-code-eda.html)

+   [让你的数据项目更有价值的问题](/2021/03/one-question-makes-data-project-more-valuable.html)

+   [如何构建正确的问题以通过数据回答](/2021/03/right-questions-answered-using-data.html)

### 更多相关话题

+   [掌握数据战略的15本顶级书籍](https://www.kdnuggets.com/2022/06/top-15-books-master-data-strategy.html)

+   [KDnuggets新闻，6月22日：主要的监督学习算法…](https://www.kdnuggets.com/2022/n25.html)

+   [GenAI时代的人工智能转型战略](https://www.kdnuggets.com/the-ai-transformation-strategy-in-the-genai-era)

+   [如何创建有效的人工智能战略](https://www.kdnuggets.com/2022/11/create-effective-ai-strategy.html)

+   [终极人工智能战略手册](https://www.kdnuggets.com/the-ultimate-ai-strategy-playbook)

+   [使用聚类分析对数据进行分段](https://www.kdnuggets.com/using-cluster-analysis-to-segment-your-data)
