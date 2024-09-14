# 如何使用 Python 和机器学习来预测足球比赛的赢家

> 原文：[`www.kdnuggets.com/2023/01/python-machine-learning-predict-football-match-winners.html`](https://www.kdnuggets.com/2023/01/python-machine-learning-predict-football-match-winners.html)

![如何使用 Python 和机器学习来预测足球比赛的赢家](img/6615eaaadd9f0b259c4193da10950ebd.png)

图片来源：[Freepik](https://www.freepik.com/free-photo/crop-legs-shooting-ball_2233009.htm#query=football%20field&position=0&from_view=keyword)

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

Python 是最具多功能性的编程语言之一。多年来，Python 编程已发展成为构建各种机器学习应用程序的最受欢迎的编程语言。

这种应用程序的一个关键元素通常是根据可处理的数据进行某种预测。预测具有不确定性这一方面，使用 Python 编程可以很容易地解决。

在本文中，我们将尝试解决这样一个问题。借助 Python 编程，我们将尝试预测足球比赛的结果。

由于这个问题涉及一定程度的不确定性，Python 编程可能是研究和解决这个问题的最佳选择。这正是我们在这里尝试实现的目标。

# 概述

足球是一项运动，与其他运动一样，涉及到许多本质上真正不可预测的元素。

众所周知，足球比赛的结果往往与预期不同。

在这种情况下，预测足球比赛的赢家成为一个挑战。然而，即使我们无法预知特定比赛的事件，我们也可以了解过去比赛中发生的事件。

当需要时，这些数据成为成功预测的关键元素。这是数据科学问题的基础，即研究过去的数据统计以预测可能的未来。

因此，在这个问题中，我们将基于过去比赛的数据来得出结果。我们将基于过去的数据进行统计研究，并预测足球比赛中最可能的赢家。

为此，我们将使用监督学习来构建一个用于检测的算法，使用 Python 编程。

# 问题陈述

本文旨在执行：

1.  网络爬虫以收集过去足球比赛的数据

1.  使用检测模型的监督学习来预测足球比赛的结果，基于收集的数据

1.  检测模型的评估

# 涉及的步骤

## 1\. 网络爬虫

网络爬虫是一种从互联网上不同网站的大量数据中提取相关数据的方法。

要提取的数据大多是非结构化的，且以 HTML 格式存在。这些数据以将其转化为结构化的列表形式的方式进行爬取，便于后续处理应用。

为了成功进行网络爬虫，我们需要将搜索范围缩小到包含特定足球比赛数据的网站。

确定后，我们将使用该 URL 主要访问网页的 HTML 脚本。

使用此 HTML 代码，爬虫将其转换为所需的输出格式（可能是电子表格、列表或 CSV/JSON 文件等）。

为了解决这个问题，我们将对网站 [FBref.com](https://fbref.com/en/matches/e1867e7b/Sao-Paulo-Botafogo-RJ-April-27-2019-Serie-A) 上的数据进行网络爬虫。

涉及的步骤包括：

1.  导航到上述网站的“Competitions”部分。

1.  选择任何提到的比赛（如 2022-23 赛季英超），以提取结果用于预测。

1.  转到所选比赛部分下的“Scores & Fixtures”部分。

比分将用于进行预测，因此我们需要抓取这些信息。因此，复制页面的 URL。

在这种情况下（假设为英超联赛），链接将是：[`fbref.com/en/comps/9/schedule/Premier-League-Scores-and-Fixtures#sched_2022-2023_9_1`](https://fbref.com/en/comps/9/schedule/Premier-League-Scores-and-Fixtures#sched_2022-2023_9_1)

你还可以根据需要获取其他比赛的链接。

1.  然而，需要注意的是，我们也可以使用任何其他网站来进行检测。

    例如，我们可以通过提供比赛比分的链接，比如 [`en.wikipedia.org/wiki/2022_FIFA_World_Cup`](https://en.wikipedia.org/wiki/2022_FIFA_World_Cup)，直接从维基百科抓取比赛结果。

1.  要执行实际的网络爬虫，需要将复制的 URL 提供给网络爬虫脚本或代码，以提取相关的比赛数据。

1.  脚本将用于将一个赛季中的所有比赛合并成一个列表或.csv 文件。

1.  从上面复制的 URL 将作为输入提供，同时提供包含锦标赛信息的表格的 ID。

1.  汇总的包含所有比赛的列表将作为输出接收。

1.  不必要的信息会被省略，如球员统计数据。

1.  信息仅限于包含映射到球队数据的比赛数据，以便能够预测哪个球队会赢。

1.  结果被附加以包含有关比赛和球队的数据（省略了与球员相关的信息），并借助数据框完成。

这主要是网页抓取的方式，提取的数据是基于过去的数据，用于预测未来的赢家。

让我们通过以下代码片段来理解这一点：

首先，我们将导入必要的库。

```py
import pandas as pd
from bs4 import BeautifulSoup
import requests
```

接下来，我们将使用 Beautiful Soup 创建一个 soup 来提取网站的 HTML 代码。

```py
url = 'https://en.wikipedia.org/wiki/2022_FIFA_World_Cup'
res = requests.get(url)
content = res.text
soup = BeautifulSoup(content, 'lxml')
```

然后，我们将提取比赛的信息，以此为基础预测，例如 FIFA 世界杯比赛的数据。

```py
match_data = soup.find_all('div', class_='footballbox')
```

接下来，我们将提取主场和客场球队的数据/分数。

```py
for match in match_data:
    home_team.append(match.find('th', class_='fhome').get_text())
    score.append(match.find('th', class_='fscore').get_text())
    away_team.append(match.find('th', class_='faway').get_text())
```

最后，我们将数据存储在数据框中，以便导出到 .csv 文件。

```py
dict_football = {'home_team': home_team, 'score': score, 'away_team': away_team}
df_football = pd.DataFrame(dict_football)

df_football.to_csv("fifa_worldcup_data.csv", index=False)
```

## 2\. 数据预处理

在运行实际检测模型之前处理数据变得至关重要。因此，我们在这种情况下也将执行相同的操作。

这些步骤包括创建一个变量来存储在之前比赛中赢得的分数的平均值。

这是因为检测只能基于我们已有的数据进行，因为我们无法访问任何未来的数据。

我们将计算存储赛季比赛信息的不同变量的平均值。

此外，我们还将存储各种其他变量的移动平均值。

球队的分数通过每场胜利记为 3 分，平局记为 2 分，失败记为 1 分来汇总。这些值用于汇总球队在过去几场比赛中的所有分数。

接下来，为了确保主场和客场球队之间的区分，我们可以进行适当的计算。

然而，在这种情况下，我们可以假设需要为 FIFA 世界杯推导结果。

由于比赛在中立场地进行，我们可以在这种情况下忽略主场和客场球队的概念。

如果需要考虑这些，我们必须记住从客场球队的结果中减去主场球队的结果，以检查主场球队是否优于客场球队。

## 3\. 实现预测模型

为了进行实际检测，我们可以使用不同类型的预测模型。在这种情况下，我们将考虑 3-4 种模型来实施实际预测。这里考虑的预测模型如下：

### 泊松分布

泊松分布是一种预测算法，用于通过在固定区间内定义概率并具有恒定均值速率来检测事件的可能性。

泊松分布预测某个事件在特定时间间隔内发生的次数。这意味着它帮助提供事件发生的概率度量，而不是简单的可能或不可能的结果。

这就是为什么它通常适用于多分类问题，但对二分类问题也同样有效（将两个类别视为数据集中的多类别）。

实现的代码片段如下：

定义一个“predict”函数来计算主队和客队的积分。

```py
def predict(home_team, away_team):

    # Calculate the value of lambda (λ) for both Home Team and Away Team.
    if home_team in df_football.index and away_team in df_football.index:
        lambda_home_team = df_football.at[home_team,'GoalsScored'] * df_football.at[away_team,'GoalsConceded']
        lambda_away_team = df_football.at[away_team,'GoalsScored'] * df_football.at[home_team,'GoalsConceded']
```

接下来，使用泊松分布的公式来计算“p”的值，如下所示。

然后使用这个值来计算平局（pr_draw）、主队获胜（pr_home）和客队获胜（pr_away）的相应概率。

```py
p = poisson.pmf(x, lambda_home_team) * poisson.pmf(y, lambda_away_team)
if x == y:
    pr_draw += p
elif x > y:
    pr_home += p
else:
    pr_away += p
```

主队和客队的积分分别计算，然后用于进行最终预测。

```py
points_home_team = 3 * pr_home + pr_draw
points_away_team = 3 * pr_away + pr_draw
```

这就是我们如何借助机器学习模型（在这个例子中是泊松分布）对足球比赛获胜者进行基本预测的方法。

这种特定的方法也可以通过简单地更改考虑中的预测模型公式扩展到其他模型。

最终结果将以比较研究的形式评估不同模型，以确保我们使用最合适的模型获得最佳结果。

让我们简要了解一下其他我们也可以用来进行类似预测的模型。

### 支持向量机

SVM 或支持向量机是一种基于监督学习的算法。

它主要用于分类问题。它通过在各种数据之间创建边界来进行分类。

由于它作为两个数据实体之间的分隔工作，因此它主要被认为是一个二分类解决方案。

但它也可以被修改或扩展为多分类问题。

要使用 Python 编程进行 SVM 预测，我们可以使用以下方法：

```py
svc_predict = svm.SVC()
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.30)
svc_predict.fit(x_train, y_train)
```

在这里，svc_predict 是针对训练数据 x_train 和 y_train 的 SVM 计算。x_train 和 y_train 包含用于训练模型的数据，而 x_test 和 y_test 表示用于测试模型的数据。

### KNN

K-最近邻或 KNN 是一种基于监督学习的算法。

它通过类标签进行数据分类。基本上，类被标记以创建分隔。

每个属于相同类型的数据实体都有相同的类别标签。

对于回归情况，预测是通过取“K”个最近邻的平均值来进行的。

邻居之间的距离通常是它们之间的欧几里得距离。

然而，也可以使用其他距离度量来完成相同的任务。

```py
knn_predict = KNeighborsClassifier()
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.30)
knn_predict.fit(x_train, y_train)
```

### 逻辑回归

逻辑回归是用于二分类问题的线性模型。

它可以用来预测事件的发生概率，这也是我们在此场景中使用它的原因。

在逻辑回归的情况下，因变量被限制在 0 到 1 的范围内。

这就是它为何在二分类问题中表现良好的原因，例如足球比赛中的胜负情景。

```py
logistic_predict = LogisticRegression()
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.30)
logistic_predict.fit(x_train, y_train)
```

## 4\. 使用指标评估结果

为了评估通过不同模型获得的结果，我们可以使用指标来映射哪个模型的表现优于其他模型。

在这里，我们可以计算准确度来确定模型的表现质量。相关的公式如下所示：

准确度 = （真正例 + 真负例）/

（真正例 + 假负例 + 真负例 + 假正例）

真正例是指正确预测的正面结果。同样，真正负例是指正确预测的负面结果。

假负例是指错误预测的负面结果。同样，假正例是指错误预测的正面结果。

要检查准确度，我们需要将预测输出与真实输出进行比较。这是我们检查哪个模型的预测结果最接近实际结果的方法。

# 结论

这个特定问题非常复杂，但我们仍然通过 Python 编程轻松地达到了结果。

尽管结果不是绝对准确，但算法仍然展示了 Python 编程如何每天改变世界。

算法能够轻松地逻辑预测结果，这是一项可能没有游戏相关的先验信息， humans 可能无法完成的任务。

使用这样的预测模型，我们可以对其进行微调，并在未来获得更好的结果。

希望你已经了解了如何使用 Python 和机器学习来预测数据。你可以通过免费的资源如 KDnuggets，[Scaler](https://www.scaler.com/topics/python/)，或者 [freecodecamp](https://www.freecodecamp.org/news/learning-python-from-zero-to-hero-120ea540b567/) 来学习更多关于 Python 的知识。

快乐学习！

**[Vaishnavi Amira Yada](https://www.linkedin.com/in/vaishnavi-amira-yada/)** 是一名技术内容作家。她拥有 Python、Java、DSA、C 等方面的知识。她发现自己在写作方面很有才华，并且非常喜欢它。

### 更多相关内容

+   [数据湖与 SQL：数据天堂中的完美匹配](https://www.kdnuggets.com/2023/01/data-lakes-sql-match-made-data-heaven.html)

+   [学习现代预测技术以帮助预测未来的业务……](https://www.kdnuggets.com/2022/12/sphere-learn-modern-forecasting-techniques-help-predict-future-business-outcomes.html)

+   [学习如何使用 ChatGPT 学习 Python（或其他任何东西）](https://www.kdnuggets.com/2023/02/learn-python-chatgpt.html)

+   [如何使用合成数据克服机器学习数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [你不应该使用机器学习的 4 个理由](https://www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html)

+   [你如何使用机器学习自动标记数据](https://www.kdnuggets.com/2022/02/machine-learning-automatically-label-data.html)
