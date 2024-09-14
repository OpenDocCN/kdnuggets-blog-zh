# 在 5 分钟内构建机器学习网页应用

> 原文：[https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

# 介绍

过去一年见证了[数据相关职位范围的巨大增长](https://www.bloomtech.com/article/data-science-job-growth-in-2021-and-beyond)。大多数有志于数据专业人士往往将大量精力放在模型构建上，而对数据科学生命周期中的其他元素关注较少。

由于这一点，许多数据科学家无法在 Jupyter Notebook 之外的环境中工作。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

他们无法将模型交到最终用户手中，而依赖于外部团队来完成这些工作。在没有数据管道的小公司中，这些模型往往永远不会被使用。由于公司忽视了招聘具有部署和监控模型所需技能的人才，这些模型没有创造任何商业价值。

在本文中，你将学会如何导出模型并在 Jupyter Notebook 环境之外使用它们。你将构建一个简单的网页应用，能够将用户输入传递到机器学习模型中，并向用户显示预测结果。

到本教程结束时，你将学会以下内容：

+   构建并调整机器学习模型以解决分类问题。

+   序列化并保存机器学习模型。

+   将这些模型加载到不同的环境中，使你能够超越 Jupyter Notebook 的局限。

+   使用 Streamlit 从机器学习模型构建网页应用。

这个网页应用将接受用户的人口统计数据和健康指标作为输入，并生成一个预测，判断他们在未来十年是否会发展成心脏病：

![在 5 分钟内构建机器学习网页应用](../Images/045541431656df653f751f03a8c11eaf.png)

## 步骤 1：背景

Framingham 心脏研究是对马萨诸塞州 Framingham 居民进行的长期心血管研究。一系列临床试验在一组患者中进行，记录了如 BMI、血压和胆固醇水平等风险因素。

这些患者每隔几年会报告到一个检测中心，以提供更新的健康信息。

在本教程中，我们将使用来自 [Framingham Heart Study](https://github.com/Natassha/streamlit_fhs/blob/main/framingham.csv) 的 [数据集](https://github.com/Natassha/streamlit_fhs/blob/main/framingham.csv)，预测研究中的患者在十年内是否会发展心脏病。可以从 [BioLincc 网站](https://biolincc.nhlbi.nih.gov/studies/framcohort/) 请求获得，数据集包含约 3600 名患者的风险因素。

## 第2步：前提条件

你需要在设备上安装一个 Python IDE。如果你通常在 Jupyter Notebook 中工作，请确保安装其他 IDE 或文本编辑器。我们将使用 Streamlit 创建一个 Web 应用程序，而使用 notebook 无法运行它。

我建议在 Jupyter 中编写代码以构建和保存模型（第3步和第4步）。然后，切换到其他 IDE 加载模型并运行应用程序（第5步）。

如果你尚未安装，请选择以下一些选项： [Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)， [Pycharm](https://www.jetbrains.com/help/pycharm/installation-guide.html)， [Atom](https://atom.io/)， [Eclipse](https://www.eclipse.org/)。

## 第3步：模型构建

确保下载 [这个](https://github.com/Natassha/streamlit_fhs/blob/main/framingham.csv) 数据集。然后，导入 Pandas 库并加载数据框。

```py
import pandas as pd
framingham = pd.read_csv('framingham.csv')# Dropping null values
framingham = framingham.dropna()
framingham.head()
```

![在5分钟内构建机器学习 Web 应用程序](../Images/a2a31618917370464105e45ff450f371.png)

查看数据框的前几行，可以看到有 15 个风险因素。这些是我们的自变量，我们将使用它们来预测十年内心脏病的发生（*TenYearCHD*）。

现在，让我们看看我们的目标变量：

```py
framingham['TenYearCHD'].value_counts()
```

注意，这一列中只有两个值——0 和 1。值为 0 表示患者在十年内未发展 CHD，值为 1 表示他们发展了 CHD。

该数据集也相当不平衡。结果为 0 的患者有 3101 名，而结果为 1 的患者仅有 557 名。

为了确保我们的模型不会在不平衡的数据集上训练并且始终预测多数类，我们将对训练数据进行随机过采样。然后，我们将为数据框中的所有变量拟合一个随机森林分类器。

```py
from sklearn.model_selection import train_test_split
from imblearn.over_sampling import RandomOverSampler
from sklearn.ensemble import RandomForestClassifier
from sklearn.pipeline import Pipeline
from sklearn.metrics import accuracy_scoreX = framingham.drop('TenYearCHD',axis=1)
y = framingham['TenYearCHD']
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.20)oversample = RandomOverSampler(sampling_strategy='minority')
X_over, y_over = oversample.fit_resample(X_train,y_train)rf = RandomForestClassifier()
rf.fit(X_over,y_over)
```

现在，让我们评估模型在测试集上的表现：

```py
preds = rf.predict(X_test)
print(accuracy_score(y_test,preds))
```

我们模型的最终准确率大约为 0.85。

## 第4步：保存模型

让我们保存刚刚构建的随机森林分类器。我们将使用 [Joblib](https://joblib.readthedocs.io/en/latest/installing.html) 库来完成此操作，因此请确保已安装该库。

```py
import joblib
joblib.dump(rf, 'fhs_rf_model.pkl') 
```

这个模型可以在不同的环境中轻松访问，并可以用于对外部数据进行预测。

## 第5步：构建 Web 应用程序

**a) 创建用户界面**

最后，我们可以开始使用上述创建的模型构建 Web 应用程序。开始之前，请确保安装了 [Streamlit](https://docs.streamlit.io/library/get-started/installation) 库。

如果你之前使用的是Jupyter Notebook来构建分类器，你现在需要转到不同的Python IDE。创建一个名为*streamlit_fhs.py*的文件。

你的目录应包含以下文件：

![5分钟内构建一个机器学习网页应用](../Images/609726e154e6e6681ff3b89bb48e48d6.png)

然后，在你的 *.py *文件中导入以下库：

```py
import streamlit as st
import joblib
import pandas as pd
```

让我们为我们的网页应用创建一个标题，并运行它以检查一切是否正常：

```py
st.write("# 10 Year Heart Disease Prediction")
```

要运行Streamlit应用，请在终端中输入以下命令：

```py
streamlit run streamlit_fhs.py
```

现在，导航到 [http://localhost:8501](http://localhost:8501/) 你可以看到你的应用所在的页面。你应该会看到如下页面：

![5分钟内构建一个机器学习网页应用](../Images/abc547aa03901db5995eedc635d234c5.png)

太棒了！这意味着一切正常。

现在，让我们为用户创建输入框，以便他们输入他们的数据（年龄、性别、血压等）。

这是如何在Streamlit中创建一个多选下拉框，让用户选择他们的性别的示例代码（*这是示例代码。运行后请删除这一行，完整示例请见下方*）：

```py
gender = st.selectbox("Enter your gender",["Male", "Female"])
```

再次导航到你的Streamlit应用并刷新页面。你会看到屏幕上出现这个下拉框：

![5分钟内构建一个机器学习网页应用](../Images/441bb801f8a16b754ca03e2416cf3a91.png)

记住，我们需要从用户那里收集15个独立变量。

运行以下代码行以创建输入框，供用户输入数据。我们将页面分成三列，以使应用更具视觉吸引力。

```py
col1, col2, col3 = st.columns(3)

# getting user inputgender = col1.selectbox("Enter your gender",["Male", "Female"])

age = col2.number_input("Enter your age")
education = col3.selectbox("Highest academic qualification",["High school diploma", "Undergraduate degree", "Postgraduate degree", "PhD"])

isSmoker = col1.selectbox("Are you currently a smoker?",["Yes","No"])

yearsSmoking = col2.number_input("Number of daily cigarettes")

BPMeds = col3.selectbox("Are you currently on BP medication?",["Yes","No"])

stroke = col1.selectbox("Have you ever experienced a stroke?",["Yes","No"])

hyp = col2.selectbox("Do you have hypertension?",["Yes","No"])

diabetes = col3.selectbox("Do you have diabetes?",["Yes","No"])

chol = col1.number_input("Enter your cholesterol level")

sys_bp = col2.number_input("Enter your systolic blood pressure")

dia_bp = col3.number_input("Enter your diastolic blood pressure")

bmi = col1.number_input("Enter your BMI")

heart_rate = col2.number_input("Enter your resting heart rate")

glucose = col3.number_input("Enter your glucose level")
```

再次刷新应用以查看更改：

![5分钟内构建一个机器学习网页应用](../Images/9fc6edbe5dcdcc690c1914a25dffdfb9.png)

最后，我们需要在页面底部添加一个‘*预测*’按钮。用户点击这个按钮后，将显示输出结果。

```py
st.button('Predict')
```

太棒了！刷新页面后你会看到这个按钮。

**b) 进行预测**

应用界面已准备好。现在，我们只需在每次系统接收到用户输入时收集这些数据。我们需要将这些数据传递给分类器，以得出预测结果。

用户输入现在存储在我们之前创建的变量中——*年龄、性别、教育背景*等。

然而，这些输入的格式并不容易被分类器处理。我们正在收集*是/否*问题形式的字符串，这需要以与训练数据相同的方式进行编码。

运行以下代码行以转换用户输入的数据：

```py
df_pred = pd.DataFrame([[gender,age,education,isSmoker,yearsSmoking,BPMeds,stroke,hyp,diabetes,chol,sys_bp,dia_bp,bmi,heart_rate,glucose]],

columns= ['gender','age','education','currentSmoker','cigsPerDay','BPMeds','prevalentStroke','prevalentHyp','diabetes','totChol','sysBP','diaBP','BMI','heartRate','glucose'])

df_pred['gender'] = df_pred['gender'].apply(lambda x: 1 if x == 'Male' else 0)

df_pred['prevalentHyp'] = df_pred['prevalentHyp'].apply(lambda x: 1 if x == 'Yes' else 0)

df_pred['prevalentStroke'] = df_pred['prevalentStroke'].apply(lambda x: 1 if x == 'Yes' else 0)

df_pred['diabetes'] = df_pred['diabetes'].apply(lambda x: 1 if x == 'Yes' else 0)

df_pred['BPMeds'] = df_pred['BPMeds'].apply(lambda x: 1 if x == 'Yes' else 0)

df_pred['currentSmoker'] = df_pred['currentSmoker'].apply(lambda x: 1 if x == 'Yes' else 0)def transform(data):
    result = 3
    if(data=='High school diploma'):
        result = 0
    elif(data=='Undergraduate degree'):
        result = 1
    elif(data=='Postgraduate degree'):
        result = 2
    return(result)df_pred['education'] = df_pred['education'].apply(transform)
```

我们可以直接加载之前保存的模型，并用它对用户输入的值进行预测：

```py
model = joblib.load('fhs_rf_model.pkl')
prediction = model.predict(df_pred)
```

最后，我们需要在屏幕上显示这些预测结果。

导航到你之前创建预测按钮的代码行，并按如下所示修改它：

```py
if st.button('Predict'):

    if(prediction[0]==0):
        st.write('<p class="big-font">You likely will not develop heart disease in 10 years.</p>',unsafe_allow_html=True)

    else:
        st.write('<p class="big-font">You are likely to develop heart disease in 10 years.</p>',unsafe_allow_html=True)
```

这些更改已被添加，以便只有当用户点击“预测”按钮后才显示输出结果。同时，我们希望向用户显示文本，而不仅仅是显示预测值（0和1）。

保存你所有的代码并刷新页面，你将看到屏幕上的完整应用程序。输入随机数字并点击预测按钮，确保一切正常：

![5分钟内构建机器学习Web应用](../Images/507ef4f898fa003865eb903d9edd9c2c.png)

如果你完成了整个教程，恭喜你！你已经成功构建了一个能够与终端用户交互的Streamlit ML Web应用。

下一步，你可以考虑部署应用程序，使其能够被互联网用户访问。可以使用像[Heroku](https://devcenter.heroku.com/categories/reference)、[GCP](https://towardsdatascience.com/deploying-streamlit-apps-to-gcp-79ad5933013e)和[AWS](https://towardsdatascience.com/how-to-deploy-a-streamlit-app-using-an-amazon-free-ec2-instance-416a41f69dc3)这样的工具来完成这项工作。

**[Natassha Selvaraj](https://www.natasshaselvaraj.com/)** 是一位自学成才的数据科学家，热衷于写作。你可以在[LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/)上与她联系。

### 更多相关信息

+   [KDnuggets 新闻 2022年3月9日：在5分钟内构建机器学习Web应用…](https://www.kdnuggets.com/2022/n10.html)

+   [用Python在5分钟内构建网络抓取器](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)

+   [使用Heroku部署机器学习Web应用](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)

+   [用Python在7个简单步骤中构建命令行应用](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

+   [用Python在5分钟内构建文本转语音转换器](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

+   [用Hugging Face和Gradio在5分钟内构建AI聊天机器人](https://www.kdnuggets.com/2023/06/build-ai-chatbot-5-minutes-hugging-face-gradio.html)
