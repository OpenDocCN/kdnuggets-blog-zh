# 朴素贝叶斯：机器学习分类性能的基准模型

> 原文：[`www.kdnuggets.com/2019/04/naive-bayes-baseline-model-machine-learning-classification-performance.html/2`](https://www.kdnuggets.com/2019/04/naive-bayes-baseline-model-machine-learning-classification-performance.html/2)

### 多项式朴素贝叶斯

首先，需要对分类变量进行编码。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

```py
o = {'sunny': 1, 'overcast': 2, 'rainy': 3}
data.outlook = [o[item] for item in data.outlook.astype(str)]

t = {'hot': 1, 'mild': 2, 'cool': 3}
data.temp = [t[item] for item in data.temp.astype(str)]

h = {'high': 1, 'normal': 2}
data.humidity = [h[item] for item in data.humidity.astype(str)]

w = {'True': 1, 'False': 2}
data.windy = [w[item] for item in data.windy.astype(str)]

```

然后我们可以创建训练集和测试集

```py
x = tennis.iloc[:,0:-1] # X is the features in our dataset
y = tennis.iloc[:,-1]   # y is the Labels in our dataset

X_train, X_test, y_train, y_test = train_test_split(x, y, test_size=0.33, random_state=42)

```

接下来，我们可以继续拟合模型并进行预测

```py
modelM = MultinomialNB().fit(X_train, y_train)
predM = model.predict(X_test)
predM

```

```py
 array(['yes', 'yes', 'yes', 'yes', 'yes'], dtype='<U3') 
```

似乎所有的预测结果都返回了‘是’。这在评估模型时会有影响，如你所见。

让我们用 pandas 制作混淆矩阵，因为我个人不喜欢 Scikit-learn 的混淆矩阵。

```py
pd.crosstab(y_test, predy, rownames=['Actual'], colnames=['Predicted'], margins=True)

```

```py
 Predicted 	yes All
Actual
no 	         2  2
yes 	         3  3
All 	         5  5 
```

```py
accuracy_score = accuracy_score(y_test, predy)
print('The accuracy of the Multinomial model is ', accuracy_score)

```

```py
 The accuracy of the Multinomial model is 0.6 
```

多项式模型给出了 60% 的准确率

**由于没有 false negatives，因为没有预测出‘0’类，因此模型的召回率（真正率）为 100%。召回率通过 [真正例/(真正例+假负例)] 计算。** 不幸的是，这在现实情况下是不接受的，因为 100% 的召回率在实际中难以实现。这只是数学计算，需要人为解释以评估其适用性。

### 高斯朴素贝叶斯

由于高斯朴素贝叶斯更偏好连续数据，我们将使用[Pima 印第安人糖尿病数据集](https://www.kaggle.com/uciml/pima-indians-diabetes-database)

```py
diabetes = pd.read_csv('diabetes.csv')

```

```py
diabetes.dtypes

```

```py
 Pregnancies                   int64
  Glucose                       int64
  BloodPressure                 int64
  SkinThickness                 int64
  Insulin                       int64
  BMI                         float64
  DiabetesPedigreeFunction    float64
  Age                           int64
  Outcome                       int64
  dtype: object 
```

我们可以看到所有特征都是连续的。

现在让我们测试特征是否符合高斯分布（正态分布），因为这是高斯朴素贝叶斯模型的一个必要假设（尽管如果数据不符合正态分布，模型仍然可以使用）

循环将告诉我们数据是否符合正态分布，使用著名的 Shapiro-Wilkes 检验。

```py
  for i in range(0,9):
      stat,p = shapiro(diabetes[diabetes.columns[i]])
      print(diabetes.columns[i], 'Test-Statistic=%.3f, p-value=%.3f' % (stat, p));
      alpha = 0.05
      if p > alpha:
          print(diabetes.columns[i], 'looks Gaussian (fail to reject H0)')
          print('---------------------------------------')
      else:
          print(diabetes.columns[i],'does not look Gaussian (reject H0)')
          print('---------------------------------------')

```

```py
 Pregnancies Test-Statistic=0.904, p-value=0.000
  Pregnancies does not look Gaussian (reject H0)
  ---------------------------------------
  Glucose Test-Statistic=0.970, p-value=0.000
  Glucose does not look Gaussian (reject H0)
  ---------------------------------------
  BloodPressure Test-Statistic=0.819, p-value=0.000
  BloodPressure does not look Gaussian (reject H0)
  ---------------------------------------
  SkinThickness Test-Statistic=0.905, p-value=0.000
  SkinThickness does not look Gaussian (reject H0)
  ---------------------------------------
  Insulin Test-Statistic=0.722, p-value=0.000
  Insulin does not look Gaussian (reject H0)
  ---------------------------------------
  BMI Test-Statistic=0.950, p-value=0.000
  BMI does not look Gaussian (reject H0)
  ---------------------------------------
  DiabetesPedigreeFunction Test-Statistic=0.837, p-value=0.000
  DiabetesPedigreeFunction does not look Gaussian (reject H0)
  ---------------------------------------
  Age Test-Statistic=0.875, p-value=0.000
  Age does not look Gaussian (reject H0)
  ---------------------------------------
  Outcome Test-Statistic=0.603, p-value=0.000
  Outcome does not look Gaussian (reject H0)
  --------------------------------------- 
```

**所有特征似乎都不符合正态分布。**

让我们更进一步，来可视化它们的分布情况

```py
diabetes.hist(figsize=(20, 10));

```

![pima-diabetes-histogram](img/1f931e10b3854819507e2bba3e123b3b.png)

Pima 糖尿病特征的直方图

通过视觉检查，BMI 和血压似乎符合正态分布，但两侧的离群值和假设检验会让我们认为情况并非如此。尽管假设

如果不符合，我们仍然可以继续前进以适应模型。

```py
  xG = diabetes.iloc[:,0:-1] # X is the features in our dataset
  yG = diabetes.iloc[:,-1]   # y is the Labels in our dataset

  X_trainG, X_testG, y_trainG, y_testG = train_test_split(xG, yG, test_size=0.33, random_state=42)

```

```py
modelG = GaussianNB().fit(X_trainG, y_trainG)
predG = modelG.predict(X_testG)

```

```py
pd.crosstab(y_testG, predG, rownames=['Actual'], colnames=['Predicted'], margins=True)

```

```py
 Predicted 	0 	1 	All
Actual
0 	        136 	32 	168
1             	33 	53 	86
All 	        169 	85 	254 
```

这次我们可以计算召回率（真正率），因为现在两个类别都已被预测。

```py
recall = recall_score(y_testG, predG, average='binary')
print('The Recall of the Gaussian model is', recall)

```

```py
 The Recall of the Gaussian model is 0.6162790697674418 
```

我使用`average='binary'`因为我们的目标变量是二元目标（0 和 1）。

模型给出了 62%的真正率（召回率）。

我在获取模型的准确率时遇到困难，所以我们可以手动计算：

```py
tn, fn, fp, tp = confusion_matrix(y_testG, predG).ravel()
accuracy = (tp + tn) /(tp+fp+tn+fn)
print('The accuracy of the Gaussian model is', accuracy)

```

```py
 The accuracy of the Gaussian model is 0.7440944881889764 
```

高斯模型给出了 74%的准确率。

### 朴素贝叶斯的优点

1.  可以处理缺失值。

    +   在准备模型时会忽略缺失值，并在计算类别值的概率时也会被忽略。

1.  可以处理小样本量。

    +   朴素贝叶斯不需要大量的训练数据。它只需要足够的数据来理解每个特征与目标变量之间的概率关系。**如果仅有少量训练数据，朴素贝叶斯通常会比其他模型表现更好**。

1.  尽管违反了独立性假设，模型仍然表现良好。

    +   尽管独立性在现实世界数据中很少成立，但模型仍会像往常一样表现。

1.  易于解释，相较而言预测速度较快。

    +   朴素贝叶斯不是一种黑箱算法，最终结果可以很容易地向观众解释。

1.  可以处理数值型和分类数据。

    +   朴素贝叶斯是分类器，因此在处理分类数据时表现更好。尽管数值数据也可以使用，但它假设所有数值数据都是正态分布的，而现实数据中这一点不太可能成立。

### 朴素贝叶斯的缺点

1.  **朴素假设**

    +   朴素贝叶斯假设所有特征彼此独立。在现实生活中，几乎不可能获得一组完全独立的预测变量。

1.  无法结合特征之间的交互作用。

1.  模型的性能将对数据偏斜高度敏感。

    +   当训练集不能代表总体人口的类别分布时，先验估计将会不准确。

1.  **零频率问题**

    +   在测试数据中存在但在训练数据中没有的类别的分类变量将被分配为**零（0）**概率，从而无法进行预测。

    +   *作为解决方案，必须对类别应用**平滑技术**。其中一种最简单且最著名的技术是[拉普拉斯平滑技术](https://stats.stackexchange.com/questions/108797/in-naive-bayes-why-bother-with-laplacian-smoothing-when-we-have-unknown-words-i)。Python 的 Sklearn 默认实现了拉普拉斯平滑。*

1.  数据集中的相关特征必须被移除，否则在模型中会被投票两次，从而**过度夸大该特征的重要性**。

### 为什么使用朴素贝叶斯作为性能基准分类器？

我认为朴素贝叶斯应该作为第一个模型进行创建和比较，原因是：

+   **它在预测中高度依赖先验目标类概率。不准确或不现实的先验可能导致误导性结果。由于朴素贝叶斯是基于概率的机器学习技术，目标的概率将极大地影响最终预测。**

+   由于无需去除缺失值，你无需冒着丢失原始数据的风险。

+   独立性假设实际上几乎从未得到满足，因此由于其最基本的假设存在缺陷，结果不太可靠。

+   模型中未考虑特征之间的交互作用。然而，现实世界中的特征几乎总是存在交互作用的。

+   没有错误或方差需要最小化，而只是要在给定预测因子的情况下寻求更高的类概率。

以上所有都可以作为其他分类器应当超越朴素贝叶斯模型的有效点。虽然朴素贝叶斯在**垃圾邮件过滤**和**推荐系统**方面表现优异，但在大多数其他应用中可能并不理想。

### 结论

总体而言，朴素贝叶斯模型快速、强大且易于解释。然而，对目标变量的先验概率的过度依赖可能会产生非常误导和不准确的结果。分类器如决策树、逻辑回归、随机森林和集成方法应当能够超越朴素贝叶斯，成为真正有用的工具。这并不意味着朴素贝叶斯不再是一个强大的分类器。独立性假设、无法处理交互作用以及高斯分布假设使其成为一个非常难以完全信赖的算法，因为这些模型必须不断更新。

**相关内容：**

+   仅使用 Python 实现的朴素贝叶斯 – 无需复杂框架

+   机器学习以 88% 的准确率发现“假新闻”

+   从零开始展开朴素贝叶斯

### 更多相关话题

+   [高斯朴素贝叶斯的解释](https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html)

+   [朴素贝叶斯算法：你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [KDnuggets 新闻，4 月 13 日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [分类问题的更多性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)

+   [使用 Tensorflow 训练图像分类模型的指南](https://www.kdnuggets.com/2022/12/guide-train-image-classification-model-tensorflow.html)

+   [理解分类指标：评估模型的指南](https://www.kdnuggets.com/understanding-classification-metrics-your-guide-to-assessing-model-accuracy)
