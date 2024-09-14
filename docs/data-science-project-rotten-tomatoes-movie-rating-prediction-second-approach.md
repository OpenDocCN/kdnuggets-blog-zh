# 烂番茄电影评分预测数据科学项目：第二种方法

> 原文：[https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html](https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html)

这个数据科学项目曾在Meta（Facebook）的招聘过程中作为家庭作业使用。在这个家庭作业中，我们将发现烂番茄如何进行“烂片”、“新鲜”或“认证新鲜”的标签。

数据科学项目链接：[https://platform.stratascratch.com/data-projects/rotten-tomatoes-movies-rating-prediction](https://platform.stratascratch.com/data-projects/rotten-tomatoes-movies-rating-prediction?utm_source=blog&utm_medium=click&utm_campaign=kdn+rotten+tomatoes)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

为此，我们将开发两种不同的方法。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/d79afba4c7ed2b547c017515ca1b08f0.png)

作者提供的图片

在我们的探索过程中，我们将讨论数据预处理、各种分类器，以及可能的改进以增强我们模型的性能。

到本帖结束时，你将了解如何使用机器学习预测电影成功，以及这些知识如何应用于娱乐行业。

但在深入之前，让我们先了解一下我们将处理的数据。

# 第二种方法：基于评论情感预测电影状态

在第二种方法中，我们计划通过评估评论的情感来预测电影的成功。我们将特别应用情感分析来评估评论的整体情感，并根据这种情感将电影分类为“新鲜”或“烂片”。

然而，在开始情感分析之前，我们必须先准备我们的数据集。与之前的策略不同，这种方法涉及处理文本数据（评论），而不是数值和分类变量。对于这个挑战，我们将继续使用随机森林模型。让我们在继续之前，仔细查看一下我们的数据。

首先，让我们读取数据。

这是代码。

```py
df_critics = pd.read_csv('rotten_tomatoes_critic_reviews_50k.csv')
df_critics.head()
```

这是输出。

![数据科学项目：烂番茄电影评分预测的第二种方法](../Images/749cb085c1d57e200b7a99de6fa6e80e.png)![数据科学项目：烂番茄电影评分预测的第二种方法](../Images/3f049646d350b280b0e4cce693543493.png)

作者图片

很好，让我们开始数据预处理。

## 数据预处理

在这个数据集中，我们没有电影名称和相应的状态。对于这个数据集，我们有 review_content 和 review_type 变量。

这就是为什么我们将把这个数据集与之前的数据集在 rotten_tomatoes_link 上合并，并选择必要的特征进行索引括起来，如下所示。

这是代码：

```py
df_merged = df_critics.merge(df_movie, how='inner', on=['rotten_tomatoes_link'])
df_merged = df_merged[['rotten_tomatoes_link', 'movie_title', 'review_content', 'review_type', 'tomatometer_status']]
df_merged.head()
```

这是输出结果。

![数据科学项目：烂番茄电影评分预测的第二种方法](../Images/819de42e235cdfb7eeea13722a570404.png)

在这种方法中，我们将只使用 review_content 列作为输入特征，将 review_type 作为真实标签。

为了确保数据可用，我们需要过滤掉 review_content 列中的任何缺失值，因为空评论不能用于情感分析。

```py
df_merged = df_merged.dropna(subset=['review_content'])
```

过滤掉缺失值后，我们将可视化 review_type 的分布，以便更好地理解数据的分布。

```py
# Plot distribution of the review
ax = df_merged.review_type.value_counts().plot(kind='bar', figsize=(12,9))
ax.bar_label(ax.containers[0])
```

这个可视化将帮助我们确定数据中是否存在类别不平衡，并指导我们选择适合的模型评估指标。

这是完整的代码：

```py
df_merged = df_merged.dropna(subset=['review_content'])
# Plot distribution of the review
ax = df_merged.review_type.value_counts().plot(kind='bar', figsize=(12,9))
ax.bar_label(ax.containers[0])
```

这是输出结果。

![数据科学项目：烂番茄电影评分预测的第二种方法](../Images/696df7516e1530102fb86bbcabe412ab.png)

看起来我们在特征之间存在不平衡问题。

而且，我们的数据点也太多了，这可能会降低我们的速度。

所以，我们将首先从原始数据集中挑选5000条记录。

```py
df_sub = df_merged[0:5000]
```

然后我们将进行有序编码。

```py
review_type = pd.DataFrame(df_sub.review_type.replace(['Rotten','Fresh'],[0,1]))
```

最后，我们将创建一个数据框，包含使用 Python 的 concat() 方法编码的标签和评论内容，并通过使用 head() 方法查看前5行。

```py
df_feature_critics = pd.concat([df_sub[['review_content']]
                        ,review_type], axis=1).dropna()
df_feature_critics.head()
```

这是完整的代码。

```py
# Pick only 5000 entries from the original dataset
df_sub = df_merged[0:5000]

# Encode the label
review_type = pd.DataFrame(df_sub.review_type.replace(['Rotten','Fresh'],[0,1]))

# Build final DataFrame
df_feature_critics = pd.concat([df_sub[['review_content']]
                        ,review_type], axis=1).dropna()
df_feature_critics.head()
```

这是输出结果。

![数据科学项目：烂番茄电影评分预测的第二种方法](../Images/4546f8377d91a7bbad2aadab16a6ad20.png)

很好，现在作为这一部分的最终步骤，让我们将数据集分成训练集和测试集。

```py
X_train, X_test, y_train, y_test = train_test_split( df_feature_critics['review_content'], df_feature_critics['review_type'], test_size=0.2, random_state=42)
```

## 默认随机森林

为了在机器学习方法中使用 DataFrame 中的文本评论，我们必须将其转换为可处理的格式。在自然语言处理（NLP）中，这被称为标记化，我们将文本或单词转换为 n 维向量，然后使用这些向量表示作为机器学习算法的训练数据。

为此，我们将使用 scikit-learn 的 CountVectorizer 类将文本评论转换为标记计数矩阵。我们首先通过创建输入文本的唯一术语字典开始。

例如，根据两个评论"This movie is good"和"The movie is bad"，算法将创建一个唯一短语的字典，如；

然后，根据输入文本，我们计算字典中每个单词出现的次数。

["this", "movie", "is", "a", "good", "the", "bad"]。

例如，输入"This movie is a good movie"将生成一个向量[1, 2, 1, 1, 1, 0, 0]

最后，我们将生成的向量输入到我们的 Random Forest 模型中。

我们可以通过在向量化文本数据上训练我们的 Random Forest 分类器来预测评论的情感，并将电影分类为“新鲜”或“腐烂”。

以下代码实例化一个 CountVectorizer 类，将文本数据转换为数值向量，并指定一个单词必须出现在至少一个文档中才能包含在词汇表中。

```py
# Instantiate vectorizer class
vectorizer = CountVectorizer(min_df=1)
```

接下来，我们将使用实例化的 CountVectorizer 对象将训练数据转换为向量。

```py
# Transform our text data into vector
X_train_vec = vectorizer.fit_transform(X_train).toarray()
```

然后，我们实例化一个具有指定随机状态的 RandomForestClassifier 对象，并使用训练数据拟合随机森林模型。

```py
# Initialize random forest and train it
rf = RandomForestClassifier(random_state=2)
rf.fit(X_train_vec, y_train) 
```

现在是时候使用训练好的模型和转换后的测试数据进行预测了。

然后我们将打印出包含评估指标如精确度、召回率和 f1-score 的分类报告。

```py
# Predict and output classification report
y_predicted = rf.predict(vectorizer.transform(X_test).toarray())

print(classification_report(y_test, y_predicted))
```

最后，我们创建一个指定大小的新图形来绘制混淆矩阵，并绘制混淆矩阵。

```py
fig, ax = plt.subplots(figsize=(12, 9))
plot_confusion_matrix(rf, vectorizer.transform(X_test).toarray(), y_test, cmap ='cividis', ax=ax
```

这是完整代码。

```py
# Instantiate vectorizer class
vectorizer = CountVectorizer(min_df=1)

# Transform our text data into vector
X_train_vec = vectorizer.fit_transform(X_train).toarray()

# Initialize random forest and train it
rf = RandomForestClassifier(random_state=2)
rf.fit(X_train_vec, y_train)

# Predict and output classification report
y_predicted = rf.predict(vectorizer.transform(X_test).toarray())

print(classification_report(y_test, y_predicted))

fig, ax = plt.subplots(figsize=(12, 9))
plot_confusion_matrix(rf, vectorizer.transform(X_test).toarray(), y_test, cmap ='cividis', ax=ax
```

这是输出。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/5f1de493df13d2aeaf572daaedffcc82.png)

## 加权随机森林

从我们最新的混淆矩阵可以看出，我们模型的表现还不够好。

然而，由于处理的数据点数量有限（5000而非100000），这可能是预期中的结果。

让我们看看是否可以通过解决类别不平衡问题来提高性能。

这是代码。

```py
class_weight = compute_class_weight(class_weight= 'balanced', classes= np.unique(df_feature_critics.review_type), 
                      y = df_feature_critics.review_type.values)

class_weight_dict = dict(zip(range(len(class_weight.tolist())), class_weight.tolist()))
class_weight_dict
```

这是输出。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/24cfa8796c7db9e130b3a194681aa333.png)

我们现在在向量化的文本输入上训练我们的 Random Forest 分类器，但这次包括了类权重信息，以提高评估指标。

我们首先创建 CountVectorizer 类，并像之前一样将文本输入转换为向量。

并将我们的文本数据转换为向量。

```py
vectorizer = CountVectorizer(min_df=1)
X_train_vec = vectorizer.fit_transform(X_train).toarray()
```

然后我们将定义一个具有计算类权重的随机森林并进行训练。

```py
# Initialize random forest and train it
rf_weighted = RandomForestClassifier(random_state=2, class_weight=class_weight_dict)
rf_weighted.fit(X_train_vec, y_train)
```

现在是时候通过使用测试数据进行预测并打印出分类报告了。

```py
# Predict and output classification report
y_predicted = rf_weighted.predict(vectorizer.transform(X_test).toarray())

print(classification_report(y_test, y_predicted))
```

最后一步，我们设置图形大小并绘制混淆矩阵。

```py
fig, ax = plt.subplots(figsize=(12, 9))
plot_confusion_matrix(rf_weighted, vectorizer.transform(X_test).toarray(), y_test, cmap ='cividis', ax=ax)
```

这是完整代码。

```py
# Instantiate vectorizer class
vectorizer = CountVectorizer(min_df=1)

# Transform our text data into vector
X_train_vec = vectorizer.fit_transform(X_train).toarray()

# Initialize random forest and train it
rf_weighted = RandomForestClassifier(random_state=2, class_weight=class_weight_dict)
rf_weighted.fit(X_train_vec, y_train)

# Predict and output classification report
y_predicted = rf_weighted.predict(vectorizer.transform(X_test).toarray())

print(classification_report(y_test, y_predicted))

fig, ax = plt.subplots(figsize=(12, 9))
plot_confusion_matrix(rf_weighted, vectorizer.transform(X_test).toarray(), y_test, cmap ='cividis', ax=ax)
```

这是输出。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/add1c22409040bb1eb38f42b80aa6b02.png)

现在我们模型的准确性略高于没有类权重的模型。

此外，由于类 0（'Rotten'）的权重大于类 1（'Fresh'）的权重，模型现在在预测 'Rotten' 电影评论方面表现更好，但在预测 'Fresh' 电影评论方面表现较差。

这是因为模型更关注被分类为 'Rotten' 的数据。

## 电影状态预测

现在我们已经训练了模型来预测电影评论的情感，让我们使用随机森林模型来预测电影状态。我们将通过以下阶段来确定电影的状态：

+   收集某部电影的所有评论。

+   利用我们的随机森林模型来估计每个评论的状态（例如，'Fresh' 或 'Rotten'）。

+   根据总的评论状态来分类电影的最终状态，使用 Rotten Tomatoes 网站上给出的基于规则的方法。

在以下代码中，我们首先创建一个名为 predict_movie_statust 的函数，它接受一个预测作为参数。

然后，根据 positive_percentage 值，我们确定电影状态，将 'Fresh' 或 'Rotten' 分配给预测变量。

最终，它将输出带有电影状态的正面评论百分比。

这是代码。

```py
def predict_movie_status(prediction):
    """Assign label (Fresh/Rotten) based on prediction"""
    positive_percentage = (prediction == 1).sum()/len(prediction)*100

    prediction = 'Fresh' if positive_percentage >= 60 else 'Rotten'

    print(f'Positive review:{positive_percentage:.2f}%')
    print(f'Movie status: {prediction}')
```

在这个例子中，我们将预测三部电影的状态：Body of Lies、Angel Heart 和 The Duchess。让我们从 Body of Lies 开始。

### 'Body of Lies' 预测

如上所述，首先收集 Body of Lies 电影的所有评论。

这是代码。

```py
# Gather all of the reviews of Body of Lies movie
df_bol = df_merged.loc[df_merged['movie_title'] == 'Body of Lies']

df_bol.head()
```

这是输出。

![数据科学项目：Rotten Tomatoes 电影评分预测：第二种方法](../Images/997a30ce9ecd213bf0d9b65717f229bd.png)

太好了，在这个阶段，我们应用加权随机森林算法来预测状态。然后我们在之前定义的自定义函数中使用它，该函数接受一个预测作为参数。

这是代码。

```py
y_predicted_bol = rf_weighted.predict(vectorizer.transform(df_bol['review_content']).toarray())
predict_movie_status(y_predicted_bol)
```

这是输出。

![数据科学项目：Rotten Tomatoes 电影评分预测：第二种方法](../Images/18908c0362d7de3784801139be380238.png)

这是我们的结果，让我们通过与 ground_truth 状态进行比较来检查结果是否有效。

这是代码。

```py
df_merged['tomatometer_status'].loc[df_merged['movie_title'] == 'Body of Lies'].unique()
```

这是输出。

![数据科学项目：Rotten Tomatoes 电影评分预测：第二种方法](../Images/d681f9bdd39e3cd79513d900d693a6a4.png)

看起来我们的预测非常有效，因为我们预测的电影状态是 Rotten。

### 'Angel Heart' 预测

在这里我们将重复所有步骤。

+   收集所有评论

+   进行预测

+   比较

首先，收集 Anna Karenina 电影的所有评论。

这是代码。

```py
df_ah = df_merged.loc[df_merged['movie_title'] == 'Angel Heart']
df_ah.head()
```

这是输出。

![数据科学项目：Rotten Tomatoes 电影评分预测：第二种方法](../Images/50a607e97a05bfe7228b92d989082558.png)

现在是时候使用随机森林和我们的自定义函数进行预测了。

这是代码。

```py
y_predicted_ah = rf_weighted.predict(vectorizer.transform(df_ah['review_content']).toarray())
predict_movie_status(y_predicted_ah)
```

这是输出。

![数据科学项目：Rotten Tomatoes 电影评分预测：第二种方法](../Images/c43d69b0d86a15ef24d7f635f836a1a5.png)

让我们进行比较。

这是代码。

```py
df_merged['tomatometer_status'].loc[df_merged['movie_title'] == 'Angel Heart'].unique()
```

这是输出。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/d681f9bdd39e3cd79513d900d693a6a4.png)

我们的模型再次预测正确。

现在再试一次。

### 'The Duchess' 预测

首先，让我们收集所有评论。

这里是代码。

```py
df_duchess = df_merged.loc[df_merged['movie_title'] == 'The Duchess']
df_duchess.head()
```

这是输出结果。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/ab6cc352f6fa8be8df43e19b58ee2563.png)

现在是时候进行预测了。

这里是代码。

```py
y_predicted_duchess = rf_weighted.predict(vectorizer.transform(df_duchess['review_content']).toarray())
predict_movie_status(y_predicted_duchess)
```

这是输出结果。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/4afdc0278cd0b83d3149c868ec77513c.png)

让我们将预测结果与实际情况进行比较。

这里是代码。

```py
df_merged['tomatometer_status'].loc[df_merged['movie_title'] == 'The Duchess'].unique()
```

这是输出结果。

![烂番茄电影评分预测数据科学项目：第二种方法](../Images/68020877ddebd9867658c3fdf8f1cb5b.png)

电影的实际标签是 '新鲜'，这表明我们模型的预测是错误的。

但可以注意到，我们模型的预测值非常接近60%的阈值，这表明对模型进行微调可能会将预测结果从 '烂' 改变为 '新鲜'。

显然，我们上面训练的随机森林模型并不是最佳模型，因为仍有改进的潜力。在下一部分，我们将提供许多建议，以提高我们模型的性能。

# 性能改进建议

1.  增加你拥有的数据量。

1.  设置随机森林模型的不同超参数。

1.  应用不同的机器学习模型以找到最佳模型。

1.  调整用于表示文本数据的方法。

# 结论

在本文中，我们探索了两种不同的方法来根据数值和分类特征预测电影状态。

我们首先进行了数据预处理，然后应用决策树分类器和随机森林分类器来训练我们的模型。

我们还尝试了特征选择和加权随机森林分类器。

在第二种方法中，我们使用了默认的随机森林和加权随机森林来预测三部不同电影的状态。

我们提供了改进模型性能的建议。希望本文对您有所帮助。

如果你想要一些初学者级别的项目，可以查看我们的帖子 “[数据科学项目想法（适合初学者）](https://www.stratascratch.com/blog/19-data-science-project-ideas-for-beginners/?utm_source=blog&utm_medium=click&utm_campaign=kdn+rotten+tomatoes)”。

**[Nate Rosidi](https://www.stratascratch.com)** 是一位数据科学家，专注于产品策略。他还是一位兼职教授，教授分析课程，并且是 [StrataScratch](https://www.stratascratch.com/)、一个帮助数据科学家准备面试的在线平台的创始人。可以在 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 与他联系。

### 更多相关信息

+   [数据科学项目：Rotten Tomatoes 电影评分预测：…](https://www.kdnuggets.com/2023/06/data-science-project-rotten-tomatoes-movie-rating-prediction-first-approach.html)

+   [KDnuggets 新闻，7月5日：一个烂数据科学项目 • 10 人工智能…](https://www.kdnuggets.com/2023/n24.html)

+   [311 呼叫中心表现：服务水平评分](https://www.kdnuggets.com/2023/03/boxplot-outlier-311-call-center-performance.html)

+   [Kubernetes 实战：第二版](https://www.kdnuggets.com/2022/03/manning-kubernetes-action-second-edition.html)

+   [用 Python 深度学习：第二版，作者 François Chollet](https://www.kdnuggets.com/2022/01/manning-deep-learning-python-second-edition-francois-chollet.html)

+   [如何处理每日 150 亿条日志，并将大查询保持在 1 秒内](https://www.kdnuggets.com/how-to-digest-15-billion-logs-per-day-and-keep-big-queries-within-1-second)
