# 从头开始构建你的第一个机器学习模型的逐步教程

> 原文：[https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model](https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model)

![构建你的第一个机器学习模型](../Images/a39ced9aebcf787ca24dbe305242b487.png)

图片由 pch.vector 提供，来源于 [Freepik](https://www.freepik.com/free-vector/scientists-studying-neural-connections-programmers-writing-codes-machine-brain_12291267.htm#fromView=search&page=1&position=0&uuid=cedf5a0c-dfa5-4c5a-b7cd-6aed3e67bde3)

大家好！我相信你们阅读本文是因为你们对机器学习模型感兴趣并想要构建一个。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

无论你之前是否尝试过开发机器学习模型，或是完全新手，本文都将指导你通过开发机器学习模型的最佳实践。

在本文中，我们将按照以下步骤开发一个客户流失预测分类模型：

1\. 业务理解

2\. 数据收集与准备

+   收集数据

+   探索性数据分析（EDA）和数据清理

+   特征选择

3\. 构建机器学习模型

+   选择合适的模型

+   数据拆分

+   训练模型

+   模型评估

4\. 模型优化

5\. 部署模型

如果你对构建你的第一个机器学习模型感到兴奋，就开始吧。

## 理解基础知识

在深入机器学习模型开发之前，让我们简要解释一下机器学习、机器学习的类型以及本文中将使用的一些术语。

首先，让我们讨论可以开发的机器学习模型的类型。常见的四种机器学习类型是：

+   监督式机器学习是一种从标记数据集中学习的机器学习算法。根据正确的输出，模型从模式中学习并尝试预测新数据。监督式机器学习有两个类别：**分类**（类别预测）和**回归**（数值预测）。

+   无监督机器学习是一种试图在没有指导的情况下发现数据中模式的算法。与监督机器学习不同，这种模型不依赖于标签数据。该类型有两个常见类别：**聚类**（数据分割）和**降维**（特征缩减）。

+   半监督机器学习结合了标记数据集和未标记数据集，其中标记数据集指导模型识别未标记数据中的模式。最简单的例子是自训练模型，它可以根据标记数据的模式对未标记数据进行标记。

+   强化学习是一种可以与环境交互并根据行动（获得奖励或惩罚）做出反应的机器学习算法。它通过奖励系统最大化结果，并通过惩罚避免不良结果。该模型应用的一个例子是自动驾驶汽车。

你还需要了解一些术语来开发机器学习模型：

+   特征：在机器学习模型中用于进行预测的输入变量。

+   标签：模型尝试预测的输出变量。

+   数据拆分：将数据分离为不同集合的过程。

+   训练集：用于训练机器学习模型的数据。

+   测试集：用于评估训练模型性能的数据。

+   验证集：在训练过程中用于调整超参数的数据。

+   探索性数据分析（EDA）：分析和可视化数据集的过程，以总结其信息并发现模式。

+   模型：机器学习过程的结果。它们是数据中模式和关系的数学表示。

+   过拟合：当模型过于拟合数据噪声而失去泛化能力时发生。模型在训练集上表现良好，但在测试集上表现不佳。

+   欠拟合：当模型过于简单而无法捕捉数据中的潜在模式时发生。模型在训练集和测试集上的表现可能较差。

+   超参数：用于调整模型的配置设置，这些设置在训练开始之前确定。

+   交叉验证：一种通过多次将原始样本划分为训练集和验证集来评估模型的技术。

+   特征工程：利用领域知识从原始数据中提取新的特征。

+   模型训练：使用训练数据学习模型参数的过程。

+   模型评估：使用机器学习指标如准确率、精确率和召回率来评估训练模型的性能。

+   模型部署：使训练好的模型在生产环境中可用。

掌握了这些基础知识后，让我们学习如何开发我们的第一个机器学习模型。

## 1\. 业务理解

在任何机器学习模型开发之前，我们必须理解为什么要开发这个模型。这就是为什么理解业务需求对于确保模型有效性是必要的。

业务理解通常需要与相关利益相关者进行适当的讨论。然而，由于本教程没有针对机器学习模型的业务用户，我们将自己假设业务需求。

如前所述，我们将开发一个客户流失预测模型。在这种情况下，业务需要避免公司进一步流失，并希望采取措施针对那些流失概率高的客户。

根据上述业务需求，我们需要特定的指标来衡量模型的表现。虽然有许多测量指标，但我建议使用召回率指标。

在货币价值方面，使用召回率可能更有益，因为它试图最小化假阴性或减少预测中未流失的部分，而实际上却在流失。当然，我们也可以通过使用F1指标来尝试平衡。

有鉴于此，让我们进入教程的第一部分。

## 2\. 数据收集与准备

### 数据收集

数据是任何机器学习项目的核心。没有数据，我们就无法训练机器学习模型。这就是为什么我们需要高质量的数据，并在将其输入到机器学习算法之前进行适当准备。

在现实世界中，清洁数据并不容易获得。通常，我们需要通过应用程序、调查和其他许多来源来收集数据，然后将其存储在数据存储中。然而，本教程仅涵盖收集数据集，因为我们使用的是现有的清洁数据。

在我们的案例中，我们将使用来自[Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)的Telco客户流失数据。这是一个开放源代码的分类数据，涉及电信行业客户历史及流失标签。

### 探索性数据分析（EDA）和数据清理

让我们开始审查我们的数据集。我假设读者已经具备基本的Python知识，并可以在他们的笔记本中使用Python包。我还基于[Anaconda](https://www.anaconda.com/download)环境分发了本教程，以便简化操作。

要理解我们拥有的数据，我们需要将其加载到一个用于数据操作的Python包中。最著名的是Pandas Python包，我们将使用它。我们可以使用以下代码来加载和查看CSV数据。

```py
import pandas as pd

df = pd.read_csv('WA_Fn-UseC_-Telco-Customer-Churn.csv')
df.head() 
```

![逐步教程：构建您的第一个机器学习模型](../Images/d000a3dd8c62d87a83586bbb7e52670b.png)

接下来，我们将探索数据以了解我们的数据集。以下是我们在EDA过程中将执行的一些操作。

1\. 检查特征和总结统计信息。

2\. 检查特征中的缺失值。

3\. 分析标签（流失）的分布。

4\. 为数值特征绘制直方图，为分类特征绘制条形图。

5\. 绘制数值特征的相关性热图。

6\. 使用箱型图来识别分布和潜在的异常值。

首先，我们会检查特征和摘要统计信息。使用Pandas，我们可以通过以下代码查看数据集特征。

```py
# Get the basic information about the dataset
df.info() 
```

```py
Output>>

RangeIndex: 7043 entries, 0 to 7042
Data columns (total 21 columns):
 #   Column            Non-Null Count  Dtype  
---  ------            --------------  -----  
 0   customerID        7043 non-null   object 
 1   gender            7043 non-null   object 
 2   SeniorCitizen     7043 non-null   int64  
 3   Partner           7043 non-null   object 
 4   Dependents        7043 non-null   object 
 5   tenure            7043 non-null   int64  
 6   PhoneService      7043 non-null   object 
 7   MultipleLines     7043 non-null   object 
 8   InternetService   7043 non-null   object 
 9   OnlineSecurity    7043 non-null   object 
 10  OnlineBackup      7043 non-null   object 
 11  DeviceProtection  7043 non-null   object 
 12  TechSupport       7043 non-null   object 
 13  StreamingTV       7043 non-null   object 
 14  StreamingMovies   7043 non-null   object 
 15  Contract          7043 non-null   object 
 16  PaperlessBilling  7043 non-null   object 
 17  PaymentMethod     7043 non-null   object 
 18  MonthlyCharges    7043 non-null   float64
 19  TotalCharges      7043 non-null   object 
 20  Churn             7043 non-null   object 
dtypes: float64(1), int64(2), object(18)
memory usage: 1.1+ MB 
```

我们还会通过以下代码获取数据集的摘要统计信息。

```py
# Get the numerical summary statistics of the dataset
df.describe()

# Get the categorical summary statistics of the dataset
df.describe(exclude = 'number') 
```

![逐步教程：构建你的第一个机器学习模型](../Images/8146f78756732a8535f8f817e1ecf7e0.png)

从上述信息中，我们了解到我们有19个特征和一个目标特征（Churn）。数据集包含7043行，大多数数据集是分类的。

让我们检查一下缺失数据。

```py
# Check for missing values
print(df.isnull().sum()) 
```

```py
Output>>
Missing Values:
customerID          0
gender              0
SeniorCitizen       0
Partner             0
Dependents          0
tenure              0
PhoneService        0
MultipleLines       0
InternetService     0
OnlineSecurity      0
OnlineBackup        0
DeviceProtection    0
TechSupport         0
StreamingTV         0
StreamingMovies     0
Contract            0
PaperlessBilling    0
PaymentMethod       0
MonthlyCharges      0
TotalCharges        0
Churn               0 
```

我们的数据集不包含缺失数据，因此我们不需要执行任何缺失数据处理活动。

然后，我们会检查目标变量，以查看是否存在不平衡情况。

```py
print(df['Churn'].value_counts()) 
```

```py
Output>>
Distribution of Target Variable:
No     5174
Yes    1869 
```

有轻微的不平衡，因为只有接近25%的流失发生，而非流失案例则更多。

我们还会查看其他特征的分布，首先从数值特征开始。然而，我们还会将TotalCharges特征转换为数值列，因为该特征应该是数值型而非类别型。此外，SeniorCitizen特征应为分类特征，因此我将其转换为字符串。由于Churn特征是分类特征，我们还会开发新特征，将其显示为数值列。

```py
import numpy as np
df['TotalCharges'] = df['TotalCharges'].replace('', np.nan)
df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce').fillna(0)

df['SeniorCitizen'] = df['SeniorCitizen'].astype('str')

df['ChurnTarget'] = df['Churn'].apply(lambda x: 1 if x=='Yes' else 0)

df['ChurnTarget'] = df['Churn'].apply(lambda x: 1 if x=='Yes' else 0)

num_features = df.select_dtypes('number').columns
df[num_features].hist(bins=15, figsize=(15, 6), layout=(2, 5)) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/0f784480cd073fbe036e8004f31cc975.png)

除了customerID（因为它们是具有唯一值的标识符）之外，我们还会提供分类特征的绘图。

```py
import matplotlib.pyplot as plt
# Plot distribution of categorical features
cat_features = df.drop('customerID', axis =1).select_dtypes(include='object').columns

plt.figure(figsize=(20, 20))
for i, col in enumerate(cat_features, 1):
    plt.subplot(5, 4, i)
    df[col].value_counts().plot(kind='bar')
    plt.title(col) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/29025557bd7c4be0ea896eab342ff3b0.png)

然后，我们将使用以下代码查看数值特征之间的相关性。

```py
import seaborn as sns

# Plot correlations between numerical features
plt.figure(figsize=(10, 8))
sns.heatmap(df[num_features].corr())
plt.title('Correlation Heatmap') 
```

![逐步教程：构建你的第一个机器学习模型](../Images/3718d0d8ef185d320f9e79a3bfc5c2c5.png)

上述相关性基于[皮尔逊相关系数](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)，即一个特征与另一个特征之间的线性相关性。我们还可以通过[Cramer’s V](https://en.wikipedia.org/wiki/Cram%C3%A9r%27s_V)对分类数据进行相关性分析。为了简化分析，我们将安装Dython Python包来帮助我们的分析。

```py
pip install dython 
```

一旦安装了包，我们将使用以下代码进行相关性分析。

```py
from dython.nominal import associations

# Calculate the Cramer’s V and correlation matrix
assoc = associations(df[cat_features], nominal_columns='all', plot=False)
corr_matrix = assoc['corr']

# Plot the heatmap
plt.figure(figsize=(14, 12))
sns.heatmap(corr_matrix) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/da68faa02ff0bccdeb6ffaff89c081af.png)

最后，我们将基于[四分位距（IQR）](https://en.wikipedia.org/wiki/Interquartile_range)使用箱线图检查数值异常值。

```py
# Plot box plots to identify outliers
plt.figure(figsize=(20, 15))
for i, col in enumerate(num_features, 1):
    plt.subplot(4, 4, i)
    sns.boxplot(y=df[col])
    plt.title(col) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/29025557bd7c4be0ea896eab342ff3b0.png)

从上述分析中，我们可以看出，我们不需要处理缺失数据或异常值。下一步是为我们的机器学习模型执行特征选择，因为我们只希望保留对预测有影响且在业务中可行的特征。

### 特征选择

特征选择有很多方法，通常是通过结合业务知识和技术应用来完成。然而，本教程将仅使用我们之前做的相关性分析来进行特征选择。

首先，让我们基于相关性分析选择数值特征。

```py
target = 'ChurnTarget'
num_features = df.select_dtypes(include=[np.number]).columns.drop(target)

# Calculate correlations
correlations = df[num_features].corrwith(df[target])

# Set a threshold for feature selection
threshold = 0.3
selected_num_features = correlations[abs(correlations) > threshold].index.tolist() 
```

你可以稍后调整阈值，以查看特征选择是否会影响模型的性能。我们还将对分类特征进行特征选择。

```py
categorical_target = 'Churn'

assoc = associations(df[cat_features], nominal_columns='all', plot=False)
corr_matrix = assoc['corr']

threshold = 0.3
selected_cat_features = corr_matrix[corr_matrix.loc[categorical_target] > threshold ].index.tolist()

del selected_cat_features[-1] 
```

然后，我们将使用以下代码将所有选择的特征进行合并。

```py
selected_features = []
selected_features.extend(selected_num_features)
selected_features.extend(selected_cat_features)

print(selected_features) 
```

```py
Output>>
['tenure',
 'InternetService',
 'OnlineSecurity',
 'TechSupport',
 'Contract',
 'PaymentMethod'] 
```

最终，我们有六个特征将用于开发客户流失机器学习模型。

## 3\. 构建机器学习模型

### 选择合适的模型

选择适合的机器学习模型有许多考虑因素，但这总是取决于业务需求。需要记住的几点：

1.  用例问题。是监督学习还是无监督学习，还是分类或回归？是多类别还是多标签？用例问题将决定可以使用的模型。

1.  数据特征。是表格数据、文本还是图像？数据集的大小是大还是小？数据集中是否包含缺失值？根据数据集的不同，我们选择的模型可能会有所不同。

1.  模型的解释性如何？在业务中平衡解释性和性能是至关重要的。

作为一个经验法则，最好先从一个简单的模型作为基准开始，然后再进行复杂模型的开发。你可以阅读我之前的[关于简单模型的文章](https://www.kdnuggets.com/are-we-undervaluing-simple-models)来了解什么构成了简单模型。

对于本教程，让我们从线性模型逻辑回归开始进行模型开发。

### 切分数据

下一步是将数据切分为训练集、测试集和验证集。在机器学习模型训练过程中切分数据的目的是拥有一个作为未见数据（真实世界数据）的数据集，以在没有任何数据泄漏的情况下评估模型的无偏性。

为了切分数据，我们将使用以下代码：

```py
from sklearn.model_selection import train_test_split

target = 'ChurnTarget' 

X = df[selected_features]
y = df[target]

cat_features = X.select_dtypes(include=['object']).columns.tolist()
num_features = X.select_dtypes(include=['number']).columns.tolist()

#Splitting data into Train, Validation, and Test Set
X_train_val, X_test, y_train_val, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)

X_train, X_val, y_train, y_val = train_test_split(X_train_val, y_train_val, test_size=0.25, random_state=42, stratify=y_train_val) 
```

在上述代码中，我们将数据切分为60%的训练数据集和20%的测试及验证集。一旦我们拥有数据集，就可以开始训练模型。

### 训练模型

如前所述，我们将用训练数据训练一个逻辑回归模型。然而，模型只能接受数值数据，因此我们必须对数据集进行预处理。这意味着我们需要将分类数据转换为数值数据。

为了最佳实践，我们还使用 Scikit-Learn 管道来包含所有的预处理和建模步骤。以下代码可以实现这一点。

```py
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LogisticRegression
# Prepare the preprocessing step
preprocessor = ColumnTransformer(
    transformers=[
        ('num', 'passthrough', num_features),
        ('cat', OneHotEncoder(), cat_features)
    ])

pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', LogisticRegression(max_iter=1000))
])

# Train the logistic regression model
pipeline.fit(X_train, y_train) 
```

模型管道的示意图如下所示。

![构建第一个机器学习模型的分步教程](../Images/f2d0503020b1e6d51cc657e7fe5ba4ea.png)

Scikit-Learn 管道会接受未见过的数据，并在进入模型之前经过所有的预处理步骤。在模型训练完成后，让我们评估模型结果。

### 模型评估

如前所述，我们将通过关注召回率（Recall）指标来评估模型。然而，以下代码展示了所有基本的分类指标。

```py
from sklearn.metrics import classification_report

# Evaluate on the validation set
y_val_pred = pipeline.predict(X_val)
print("Validation Classification Report:\n", classification_report(y_val, y_val_pred))

# Evaluate on the test set
y_test_pred = pipeline.predict(X_test)
print("Test Classification Report:\n", classification_report(y_test, y_test_pred)) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/07bc91caaf1ae99286c2d59fa6f9d511.png)

从验证数据和测试数据中我们可以看到，流失（churn）（1）的召回率并不是最好。这就是为什么我们可以优化模型以获得最佳结果。

## 4\. 模型优化

我们总是需要关注数据以获得最佳结果。然而，优化模型也可能会带来更好的结果。这就是为什么我们可以优化模型。优化模型的一种方法是通过超参数优化，它测试这些模型超参数的所有组合，以根据指标找到最佳组合。

每个模型都有一组超参数，我们可以在训练之前设置它们。我们称超参数优化为实验以查看哪个组合最好。为此，我们可以使用以下代码。

```py
from sklearn.model_selection import GridSearchCV
# Define the logistic regression model within a pipeline
pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', LogisticRegression(max_iter=1000))
])

# Define the hyperparameters for GridSearchCV
param_grid = {
    'classifier__C': [0.1, 1, 10, 100],
    'classifier__solver': ['lbfgs', 'liblinear']
}

# Perform Grid Search with cross-validation
grid_search = GridSearchCV(pipeline, param_grid, cv=5, scoring='recall')
grid_search.fit(X_train, y_train)

# Best hyperparameters
print("Best Hyperparameters:", grid_search.best_params_)

# Evaluate on the validation set
y_val_pred = grid_search.predict(X_val)
print("Validation Classification Report:\n", classification_report(y_val, y_val_pred))

# Evaluate on the test set
y_test_pred = grid_search.predict(X_test)
print("Test Classification Report:\n", classification_report(y_test, y_test_pred)) 
```

![逐步教程：构建你的第一个机器学习模型](../Images/c83df29e42b78c4a31a6d8a78d3349ca.png)

结果仍然没有显示最佳的召回率，但这是预期的，因为它们只是基线模型。让我们尝试几个模型，看看召回率表现是否有所改善。你可以随时调整下面的超参数。

```py
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.svm import SVC
from xgboost import XGBClassifier
from lightgbm import LGBMClassifier
from sklearn.metrics import recall_score

# Define the models and their parameter grids
models = {
    'Logistic Regression': {
        'model': LogisticRegression(max_iter=1000),
        'params': {
            'classifier__C': [0.1, 1, 10, 100],
            'classifier__solver': ['lbfgs', 'liblinear']
        }
    },
    'Decision Tree': {
        'model': DecisionTreeClassifier(),
        'params': {
            'classifier__max_depth': [None, 10, 20, 30],
            'classifier__min_samples_split': [2, 10, 20]
        }
    },
    'Random Forest': {
        'model': RandomForestClassifier(),
        'params': {
            'classifier__n_estimators': [100, 200],
            'classifier__max_depth': [None, 10, 20]
        }
    },
    'SVM': {
        'model': SVC(),
        'params': {
            'classifier__C': [0.1, 1, 10, 100],
            'classifier__kernel': ['linear', 'rbf']
        }
    },
    'Gradient Boosting': {
        'model': GradientBoostingClassifier(),
        'params': {
            'classifier__n_estimators': [100, 200],
            'classifier__learning_rate': [0.01, 0.1, 0.2]
        }
    },
    'XGBoost': {
        'model': XGBClassifier(use_label_encoder=False, eval_metric='logloss'),
        'params': {
            'classifier__n_estimators': [100, 200],
            'classifier__learning_rate': [0.01, 0.1, 0.2],
            'classifier__max_depth': [3, 6, 9]
        }
    },
    'LightGBM': {
        'model': LGBMClassifier(),
        'params': {
            'classifier__n_estimators': [100, 200],
            'classifier__learning_rate': [0.01, 0.1, 0.2],
            'classifier__num_leaves': [31, 50, 100]
        }
    }
}

results = []

# Train and evaluate each model
for model_name, model_info in models.items():
    pipeline = Pipeline(steps=[
        ('preprocessor', preprocessor),
        ('classifier', model_info['model'])
    ])

    grid_search = GridSearchCV(pipeline, model_info['params'], cv=5, scoring='recall')
    grid_search.fit(X_train, y_train)

    # Best model from Grid Search
    best_model = grid_search.best_estimator_

    # Evaluate on the validation set
    y_val_pred = best_model.predict(X_val)
    val_recall = recall_score(y_val, y_val_pred, pos_label=1)

    # Evaluate on the test set
    y_test_pred = best_model.predict(X_test)
    test_recall = recall_score(y_test, y_test_pred, pos_label=1)

    # Save results
    results.append({
        'model': model_name,
        'best_params': grid_search.best_params_,
        'val_recall': val_recall,
        'test_recall': test_recall,
        'classification_report_val': classification_report(y_val, y_val_pred),
        'classification_report_test': classification_report(y_test, y_test_pred)
    })

# Plot the test recall scores
plt.figure(figsize=(10, 6))
model_names = [result['model'] for result in results]
test_recalls = [result['test_recall'] for result in results]
plt.barh(model_names, test_recalls, color='skyblue')
plt.xlabel('Test Recall')
plt.title('Comparison of Test Recall for Different Models')
plt.show() 
```

![逐步教程：构建你的第一个机器学习模型](../Images/b01f8a8b62fcc4995e786ea84f9153ef.png)

召回率结果变化不大；即使是基线逻辑回归模型似乎也是最佳的。如果我们想要更好的结果，我们应该回到更好的特征选择。

然而，让我们继续使用当前的逻辑回归模型并尝试部署它们。

## 5\. 部署模型

我们已经构建了机器学习模型。在拥有模型后，下一步是将其部署到生产环境中。让我们使用一个简单的 API 来模拟它。

首先，让我们重新开发我们的模型并将其保存为 joblib 对象。

```py
import joblib

best_params = {'classifier__C': 1, 'classifier__solver': 'lbfgs'}
logreg_model = LogisticRegression(C=best_params['classifier__C'], solver=best_params['classifier__solver'], max_iter=1000)

preprocessor = ColumnTransformer(
    transformers=[
        ('num', 'passthrough', num_features),
        ('cat', OneHotEncoder(), cat_features)

pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', logreg_model)
])

pipeline.fit(X_train, y_train)

# Save the model
joblib.dump(pipeline, 'logreg_model.joblib') 
```

一旦模型对象准备好，我们将进入 Python 脚本以创建 API。但首先，我们需要安装一些用于部署的包。

```py
pip install fastapi uvicorn
```

我们不会在笔记本中完成这个，而是在诸如 Visual Studio Code 这样的 IDE 中。在你喜欢的 IDE 中，创建一个名为 **app.py** 的 Python 脚本，并将以下代码放入该脚本中。

```py
from fastapi import FastAPI
from pydantic import BaseModel
import joblib
import numpy as np

# Load the logistic regression model pipeline
model = joblib.load('logreg_model.joblib')

# Define the input data for model
class CustomerData(BaseModel):
    tenure: int
    InternetService: str
    OnlineSecurity: str
    TechSupport: str
    Contract: str
    PaymentMethod: str

# Create FastAPI app
app = FastAPI()

# Define prediction endpoint
@app.post("/predict")
def predict(data: CustomerData):
    # Convert input data to a dictionary and then to a DataFrame
    input_data = {
        'tenure': [data.tenure],
        'InternetService': [data.InternetService],
        'OnlineSecurity': [data.OnlineSecurity],
        'TechSupport': [data.TechSupport],
        'Contract': [data.Contract],
        'PaymentMethod': [data.PaymentMethod]
    }

    import pandas as pd
    input_df = pd.DataFrame(input_data)

    # Make a prediction
    prediction = model.predict(input_df)

    # Return the prediction
    return {"prediction": int(prediction[0])}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000) 
```

在你的命令提示符或终端中，运行以下代码。

```py
uvicorn app:app --reload 
```

使用上述代码，我们已经有了一个接受数据并进行预测的 API。让我们在新的终端中尝试一下以下代码。

```py
curl -X POST "http://127.0.0.1:8000/predict" -H "Content-Type: application/json" -d "{\"tenure\": 72, \"InternetService\": \"Fiber optic\", \"OnlineSecurity\": \"Yes\", \"TechSupport\": \"Yes\", \"Contract\": \"Two year\", \"PaymentMethod\": \"Credit card (automatic)\"}" 
```

```py
Output>>
{"prediction":0}
```

如你所见，API 结果是一个字典，预测为 0（未流失）。你可以进一步调整代码以获得所需的结果。

恭喜你。你已经开发了机器学习模型，并成功地将其部署到 API 中。

## 结论

我们已经学习了如何从头开始开发机器学习模型到部署的全过程。尝试其他数据集和用例，进一步感受效果。本文使用的所有代码将会在我的[GitHub 仓库](https://github.com/CornelliusYW/churn_prediction_machine_learning_development)上提供。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作分享 Python 和数据技巧。Cornellius 涉猎多种 AI 和机器学习话题。

### 更多相关话题

+   [部署机器学习模型：逐步教程](https://www.kdnuggets.com/deploying-machine-learning-models-a-step-by-step-tutorial)

+   [部署你的第一个机器学习模型](https://www.kdnuggets.com/deploying-your-first-machine-learning-model)

+   [使用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [使用 Bash 构建你的第一个 ETL 管道](https://www.kdnuggets.com/building-your-first-etl-pipeline-with-bash)

+   [从零到英雄：使用 PyTorch 创建你的第一个 ML 模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)

+   [构建机器学习模型的结构化方法](https://www.kdnuggets.com/2022/06/structured-approach-building-machine-learning-model.html)
