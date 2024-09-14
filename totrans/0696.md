# 开发端到端数据科学管道，包括数据摄取、处理和可视化

> 原文：[https://www.kdnuggets.com/developing-end-to-end-data-science-pipelines-with-data-ingestion-processing-and-visualization](https://www.kdnuggets.com/developing-end-to-end-data-science-pipelines-with-data-ingestion-processing-and-visualization)

![开发端到端数据科学管道，包括数据摄取、处理和可视化](../Images/0451f63ff8c9c4f86db4caa973eda2d8.png)图片来源 [macrovector on Freepik](https://www.freepik.com/free-vector/ai-powered-content-creation-isometric-composition-with-human-characters-cute-robot-generating-art-computer-screen-3d-vector-illustration_43868976.htm#fromView=search&page=3&position=0&uuid=b4c38b5b-880f-4b62-97e8-75687fe757e3)

数据科学项目不仅仅是开发完成后就不再关注。它们涉及整个过程，从获取数据集到维护数据集。这个迭代过程确保模型始终能提供价值。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

数据科学项目的关键不在于模型本身，而在于开始。如果数据质量受损且预处理不正确，后续模型将无法产生有价值的输出。维护一个能够处理端到端数据科学流程的管道变得重要。

在开发端到端数据科学管道过程中，我们将学习并重点关注数据摄取、处理和可视化。

## 标准端到端数据科学项目

在讨论端到端数据科学项目时，我们会讨论技术和业务方面。数据科学项目存在的目的是解决业务问题，因此每一步都需要记住与业务相关的内容。

一般来说，端到端数据科学项目或多或少遵循这些步骤：

1.  业务理解

1.  数据收集和准备

1.  构建机器学习模型

1.  模型优化

1.  模型部署

1.  模型监控

每一步都是必要的，需要按照顺序执行。即使缺少其中一步，我们的数据科学项目也不会提供最佳价值。

鉴于上述步骤，本文将重点介绍数据收集和准备步骤。它将基于数据摄取、处理和可视化。我们将创建一个简单的数据科学端到端项目管道，但重点强调数据收集和准备步骤。

让我们从探索步骤开始，从那里开始构建管道。

## 数据导入

数据导入将之前收集的数据集放入环境中，以便进行后续处理。如何导入数据会根据数据源的不同而有所不同。

让我展示代码示例。最简单的方法是从 CSV 或 Excel 文件中导入数据，我们可以使用以下代码完成。

```py
import pandas as pd
data = pd.read_csv('data.csv') 
```

然后，我们可以通过创建到数据库的连接，从数据仓库中导入数据。

```py
import sqlalchemy

engine = sqlalchemy.create_engine('sqlite:///example.db')
data = pd.read_sql('SELECT * FROM table_name', engine)
```

另一种流行的方法是从数据源调用 API 请求。

```py
import requests
response = requests.get('https://api.example.com/data')
data = response.json()
```

根据你的需求，数据导入过程还有很多其他使用方式。例如，你可以使用网页抓取。

```py
from bs4 import BeautifulSoup
response = requests.get('https://example.com')
soup = BeautifulSoup(response.content, 'html.parser')
```

这就是数据导入的基础。让我们来探索数据处理。

## 数据处理

在获取数据后，我们必须进一步处理，以适应业务需求和任务。我们必须关注这一步，因为项目的质量通常取决于数据处理。

数据探索和处理通常是紧密相关的，因此决定如何处理数据是在数据探索之后的步骤。以下是一些数据处理的例子。

我们会看到的第一个数据处理是数据清理。我们清理数据以提高数据集的质量。下面的例子是删除缺失数据和重复数据的处理。

```py
# Data Cleaning
data.dropna(inplace=True)
data.drop_duplicates(inplace=True)
```

数据转换也是数据处理的一部分，其中数据被转换为数据科学项目所需的其他形式。

```py
# Data Transformation
data['date'] = pd.to_datetime(data['date'])  
data['category_encoded'] = pd.get_dummies(data['category'])
```

我们还可以将数据转换为我们需要的规模。

```py
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
data[['feature1', 'feature2']] = scaler.fit_transform(data[['feature1', 'feature2']]) 
```

此外，在处理数据时，我们经常会从现有特征中创建新特征。这个过程被称为特征工程。

```py
# Feature Engineering
data['new_feature'] = data['feature1'] * data['feature2']
```

最后，我们可以进行数据拆分，将数据分为训练数据和测试数据，使用以下代码。

```py
# Data Splitting
from sklearn.model_selection import train_test_split
train, test = train_test_split(data, test_size=0.2, random_state=42)
```

数据处理的方式还有很多，因为这取决于你的项目。现在，让我们深入数据可视化。

## 数据可视化

数据可视化可能与机器学习开发无关，但在数据科学项目中至关重要。通过使用数据可视化，我们可以更好地理解数据洞察，并轻松地传达我们得到的任何结果。

这里有一些使用 Python 生成数据可视化的代码示例。

首先，我们有相关性热图，以帮助理解特征之间的关系。

```py
import seaborn as sns
import matplotlib.pyplot as plt

corr_matrix = data.corr()

sns.heatmap(corr_matrix, annot=True)
plt.title('Correlation Heatmap')
plt.show()
```

接下来，我们有配对图，它可视化了每个特征之间的二维图，并展示了目标特征的分布。

```py
sns.pairplot(data, hue='target', diag_kind='kde') 
plt.title('Pair Plot') 
plt.show()
```

然后，我们可以从模型中可视化特征的重要性。

```py
importance = model.coef_[0]
features = np.array(numeric_features.tolist() + list(preprocessor.named_transformers_['cat']['onehot'].get_feature_names_out(categorical_features)))

plt.figure(figsize=(10, 8))
sns.barplot(x=importance, y=features)
plt.xlabel('Importance')
plt.title('Feature Importance')
plt.show()
```

最后，我们可以在模型评估步骤中可视化混淆矩阵。

```py
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay

cm = confusion_matrix(y_test, model.predict(X_test))

disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=model.classes_)
disp.plot(cmap=plt.cm.Blues)
plt.title('Confusion Matrix')
plt.show()
```

## 开发数据科学管道

让我们将以上学到的内容结合成一个数据科学管道，涵盖数据导入、处理和可视化。

我们将使用 Titanic 数据来开发一个分类模型作为本例。

首先，让我们使用 Pandas 导入数据。

```py
import pandas as pd

url = 'https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv'
df_titanic = pd.read_csv(url)
```

在这之后，我们将进行数据处理。让我们使用下面的代码来清理数据集并进行数据转换。

```py
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer

df_titanic = df_titanic[['Survived', 'Pclass', 'Sex','Parch', 'Fare','Age', 'Embarked']]

df_titanic = df_titanic.dropna(subset=['Survived'])

X = df_titanic.drop('Survived', axis=1)
y = df_titanic['Survived']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

numeric_features = ['Age', 'Parch', 'Fare']
categorical_features = ['Pclass', 'Sex', 'Embarked']

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

X_train = preprocessor.fit_transform(X_train)
X_test = preprocessor.transform(X_test)
```

数据处理后，我们将开发机器学习模型。

```py
from sklearn.linear_model import LogisticRegression
import joblib

model = LogisticRegression(random_state=42)
model.fit(X_train, y_train)

joblib.dump(model, 'titanic_logistic_regression_model.joblib')
joblib.dump(preprocessor, 'titanic_preprocessor.joblib')

accuracy = model.score(X_test, y_test)
print(accuracy)
```

最后，我们可以通过以下代码来可视化模型特征的重要性，并向观众展示。

```py
import matplotlib.pyplot as plt
import numpy as np

importance = model.coef_[0]
features = np.array(numeric_features + list(preprocessor.named_transformers_['cat']['onehot'].get_feature_names_out(categorical_features)))

plt.figure(figsize=(10, 8))
plt.barh(features, importance)
plt.xlabel('Importance')
plt.title('Feature Importance')
plt.show()
```

以上就是开发一个简单的端到端数据科学管道的全部内容，包括数据摄取、处理和可视化。根据你的数据科学项目，你可以在中间添加更多步骤。

## 结论

标准化端到端数据科学管道对于我们持续为业务提供价值至关重要。通过了解每一步的细节，特别是数据摄取、处理和可视化，我们可以提高项目质量，为解决业务问题提供最佳结果。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一位数据科学助理经理和数据撰稿人。在全职工作于Allianz Indonesia期间，他喜欢通过社交媒体和写作媒体分享Python和数据技巧。Cornellius在各种AI和机器学习主题上撰写文章。

### 更多相关主题

+   [模型训练和推理的端到端隐私保护与Concrete ML](https://www.kdnuggets.com/end-to-end-privacy-for-model-training-and-inference-with-concrete-ml)

+   [关于开发安全、可靠和值得信赖的AI框架的专家见解](https://www.kdnuggets.com/expert-insights-on-developing-safe-secure-and-trustworthy-ai-frameworks)

+   [将机器学习算法全面部署到……](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [5款最佳端到端开源MLOps工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

+   [使用HuggingFace实现的简单端到端项目](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)

+   [2024年必须尝试的7个端到端MLOps平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)
