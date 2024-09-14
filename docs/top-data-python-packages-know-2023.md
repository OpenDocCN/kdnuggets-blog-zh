# 2023年需了解的顶级数据Python包

> 原文：[https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html](https://www.kdnuggets.com/2023/01/top-data-python-packages-know-2023.html)

![2023年需了解的顶级数据Python包](../Images/5c56afb622e46da52752f3a80a9fdc62.png)

图片来自[Unsplash](https://unsplash.com/photos/95YRwf6CNw8)，由[Clément Hélardot](https://unsplash.com/@clemhlrdt)提供

2022年对任何数据人员来说都是一个优秀的年份，尤其是对于那些使用Python的人，因为有许多令人兴奋的包来提升我们的数据能力。已列出了各种必须学习的[数据Python包2022](/2022/04/python-libraries-data-scientists-know-2022.html)，我们可能希望在新的一年中添加一些新东西来改善我们的技术栈。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的IT

* * *

面对2023年，各种Python包将改善我们在新的一年的数据工作流程。这些包是什么呢？让我们看看我的推荐。

从数据清理包到机器学习实现，这些是2023年你需要了解的顶级数据Python包。

# 1\. Pyjanitor

[Pyjanitor](https://pyjanitor-devs.github.io/pyjanitor/)是一个开源Python包，专门为通过方法链进行数据清理而开发，旨在改进Pandas API的数据清理功能。

我们知道许多用于数据清理的Pandas方法，例如dropna来删除所有缺失值。通过Pyjanitor，数据清理过程将通过在API中引入额外的方法而得到提升。它是如何工作的？让我们用示例数据来尝试一下这个包。

我们将使用Kaggle许可证下的[泰坦尼克号训练数据](https://www.kaggle.com/c/titanic/data?select=train.csv)作为样本。让我们开始安装Pyjanitor包。

**安装**

```py
pip install pyjanitor
```

在使用Pyjanitor进行数据清理之前，我们先查看当前的数据集。

```py
import pandas as pd
df = pd.read_csv('train.csv')
df.head()
```

**输出**

![2023年需了解的顶级数据Python包](../Images/776cb93821d58d98923eebc4c2d986d1.png)

作者提供的图片

使用Pyjanitor包，我们可以进行各种扩展的数据清理，并实现Pandas API的链式方法。让我们通过以下代码看看这个包是如何工作的。

**代码示例**

```py
import janitor
df.remove_columns(["Cabin"]).expand_column(column_name = 'Embarked').clean_names()
```

**输出**

![2023年需了解的顶级数据Python包](../Images/c6cd759e4b7d40664d8fe95e81d99cd6.png)

作者提供的图片

通过导入Pyjanitor包，它将自动在Pandas DataFrame中实现。在上面的代码中，我们使用Pyjanitor完成了以下操作：

1.  使用remove_columns方法删除‘Cabin’列，

1.  对‘Embarked’列进行类别编码（独热编码），使用expand_column方法，

1.  使用clean_names方法将所有变量标题名称转换为小写，如果有空格，将用下划线替换。

Pyjanitor中还有许多我们可以用于数据清理的函数。请参阅他们的[文档](https://pyjanitor-devs.github.io/pyjanitor/api/functions/)以获取完整的API列表。

# 2\. Pingouin

[Pingouin](https://pingouin-stats.org/)是一个用于任何常见统计活动的开源Python包，适用于任何数据科学家。该包通过提供一行代码而设计为简单，同时仍然提供各种统计测试。

**安装**

```py
pip install pingouin
```

安装完包后，让我们尝试使用Pingouin进行统计分析。例如，我们将使用之前的Titanic数据集进行T检验和ANOVA检验。

**代码示例**

```py
import pingouin as pg

#T-Test
print('T-Test example')
pg.ttest( df['Age'], df['Fare'])

print('\n')
# ANOVA test
print('ANOVA test example')
pg.anova(data=df, dv='Age', between='SibSp', detailed=True)
```

**输出**

![2023年必须了解的顶级数据Python包](../Images/4da797b7162a0d5a6d63ce89040e41f3.png)

图片来源于作者

使用一行代码，Pingouin在数据框对象中提供统计测试结果。还有许多其他函数可以帮助我们的分析，我们可以在Pingouin APIs [文档](https://pingouin-stats.org/api.html)中进行探索。

# 3\. PyCaret

[PyCaret](https://pycaret.gitbook.io/docs/)是一个开源的Python包，用于自动化机器学习工作流。该包提供了一个低代码环境，通过提供端到端的机器学习模型工具，加快模型实验。

在典型的数据科学工作中，存在许多活动，如清理数据、选择模型、进行超参数调整和评估模型。PyCaret旨在通过将所有必要的代码最小化为尽可能少的行，从而消除所有麻烦。该包将多个机器学习框架集合在一起。让我们尝试使用PyCaret以了解更多信息。

**安装**

```py
pip install pycaret
```

使用之前的Titanic数据集；我们将开发一个模型分类器来预测“Survive”变量。

**代码示例**

```py
from pycaret.classification import *
clf_exp = setup(data = df, target = 'Survived') 
```

**输出**

![2023年必须了解的顶级数据Python包](../Images/c6f83df95aaee03ee400b29fc3a916ec.png)

图片来源于作者

在上述代码中，我们使用setup函数启动实验。通过传递数据和目标，PyCaret将推断我们的数据，并基于给定的数据开发机器学习模型。实际输出信息比上面的图像要长，并且对我们建模过程中的发生情况具有洞察力。

让我们查看模型结果，并从训练数据中推断最佳模型。

```py
best_model = compare_models(sort = 'precision')
```

**输出**

![2023年必须了解的顶级数据Python包](../Images/a90b4885d88d23a9f99d923cbfc6d497.png)

图片来源于作者

```py
print(best_model)
```

**输出**

![2023年值得了解的顶级数据 Python 包](../Images/0854c5dc4378a8f29730d09fee35a5d2.png)

图片由作者提供

PyCaret 分类器实验将训练数据测试到 14 个不同的分类器中，并给出最佳模型。在我们的例子中，它是 RidgeClassifier。

你仍然可以使用 PyCaret 进行许多实验。要探索更多内容，请参考他们的 [文档](https://pycaret.gitbook.io/docs/get-started/tutorials)。

# 4\. BentoML

[BentoML](https://www.bentoml.com/) 是一个开源的 Python 包，用于快速将模型部署到生产环境，并尽可能少的代码行。该包旨在专注于生产化机器学习模型，使用户能够轻松使用。

让我们尝试 BentoML 包并了解它是如何工作的。

**安装**

```py
pip install bentoml 
```

对于 BentoML 示例，我们将使用 [包教程](https://colab.research.google.com/github/bentoml/BentoML/blob/main/examples/quickstart/iris_classifier.ipynb#scrollTo=83205567) 中的代码，并进行一些修改。

**代码示例**

我们将使用鸢尾花数据集训练模型分类器。

```py
from sklearn import svm, datasets

iris = datasets.load_iris()
X, y = iris.data, iris.target

iris_clf = svm.SVC()
iris_clf.fit(X, y)
```

使用 BentoML，我们可以将机器学习模型存储在本地或云端模型存储库中，并在生产环境中检索它。

```py
import bentoml

bentoml.sklearn.save_model("iris_clf", iris_clf)
```

然后我们可以在 BentoML 环境中使用 runner 实例来使用存储的模型。

```py
# Create a Runner instance and implement a runner instance in local
iris_clf_runner = bentoml.sklearn.get("iris_clf:latest").to_runner()
iris_clf_runner.init_local()

# Using the predictor on unseen data
iris_clf_runner.predict.run([[4.1, 2.3, 5.5, 1.8]])
```

**输出**

```py
array([2])
```

接下来，我们可以通过运行以下代码来初始化 BentoML 中保存的模型服务，以创建一个 Python 文件并启动服务器。

```py
%%writefile service.py
import numpy as np
import bentoml
from bentoml.io import NumpyNdarray

iris_clf_runner = bentoml.sklearn.get("iris_clf:latest").to_runner()

svc = bentoml.Service("iris_clf_service", runners=[iris_clf_runner])

@svc.api(input=NumpyNdarray(), output=NumpyNdarray())
def classify(input_series: np.ndarray) -> np.ndarray:
    return iris_clf_runner.predict.run(input_series)
```

我们通过运行下面的代码来启动服务器。

```py
!bentoml serve service.py:svc --reload
```

**输出**

![2023年值得了解的顶级数据 Python 包](../Images/ba85c3a64c900d27393e286501a6edc1.png)

图片由作者提供

输出将显示开发服务器的当前日志以及我们可以访问的位置。如果我们对开发结果满意，我们可以继续进行生产。我建议你参考 [文档](https://docs.bentoml.org/en/latest/tutorial.html) 以了解生产过程。

# 5\. Streamlit

[Streamlit](https://streamlit.io/) 是一个开源的 Python 包，用于为数据科学家创建自定义 Web 应用。这个包提供了有见地的代码来构建和自定义各种数据应用。让我们尝试这个包来了解它是如何工作的。

**安装**

```py
pip install streamlit 
```

Streamlit Web 应用通过执行 Python 脚本来运行。那就是为什么我们在使用 streamlit 命令运行之前需要准备脚本。我们可以使用你喜欢的 IDE 或 Jupyter Notebook 来运行下一个示例，但我会展示如何在 Jupyter Notebook 中使用 Streamlit 创建 Web 应用。

**代码示例**

```py
%%writefile streamlit_example.py
import streamlit as st
import pandas as pd
import numpy as np

st.title('Titanic Data')

data = pd.read_csv('train.csv')

st.write('Shows top 5 of the data')
st.dataframe(data.head())

st.title('Bar Chart Visualization with Age')

col = st.selectbox('Select the categorical columns', data.select_dtypes('object').columns)

st.bar_chart(data, x = col, y='Age')
```

上述代码会创建一个名为 streamlit_example.py 的脚本，并在我们运行 Streamlit 命令时创建一个类似于下面输出的 Web 应用。

```py
!streamlit run streamlit_example.py
```

![2023年值得了解的顶级数据 Python 包](../Images/bd4950cf73e0c9e4107c12e04d15f449.png)

图片由作者提供

这段代码易于学习，你几乎不需要任何时间就能用Streamlit创建你的网络应用。如果你想了解更多关于如何使用Streamlit包创建的内容，你可以参考[文档](https://docs.streamlit.io/)。

# 结论

面对2023年，我们应当比2022年更好地提升我们的数据技能。还有什么比通过学习令人惊叹的Python包来扩展我们的数据工具更好的方法呢？这些顶级的Python包包括

1.  Pyjanitor

1.  Pingouin

1.  PyCaret

1.  BentoML

1.  Streamlit

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于印尼安联保险公司期间，他喜欢通过社交媒体和写作平台分享Python和数据的技巧。

### 更多相关主题

+   [5个用于地理空间数据分析的Python包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [用于数据可视化的3个Julia包](https://www.kdnuggets.com/2023/02/3-julia-packages-data-visualization.html)

+   [2023年成为数据科学家需要掌握的19项技能](https://www.kdnuggets.com/2023/04/top-19-skills-need-know-2023-data-scientist.html)

+   [2023年你需要了解的数据分析工具](https://www.kdnuggets.com/2023/05/data-analytics-tools-need-know-2023.html)

+   [2023年你应该了解的10个令人惊叹的机器学习可视化](https://www.kdnuggets.com/2022/11/10-amazing-machine-learning-visualizations-know-2023.html)

+   [每位机器学习工程师都应掌握的5项机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)
