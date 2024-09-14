# 通过通用API分享您的机器学习模型

> 原文：[https://www.kdnuggets.com/2020/02/sharing-machine-learning-models-common-api.html](https://www.kdnuggets.com/2020/02/sharing-machine-learning-models-common-api.html)

[评论](#comments)

**作者 [Álvaro López García](https://alvarolopez.github.io/)，西班牙国家研究委员会（CSIC）**

![图示](../Images/8a404df0196b49ebc2f6e08719d438ee.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织IT工作

* * *

构建机器学习模型的数据科学家没有简单且通用的方法与同事或任何感兴趣的使用者分享他们开发的应用。整个模型（即代码和所需的任何配置资源）可以被分享，但这要求接收者具备足够的知识来执行它。在大多数情况下，我们只是想分享模型以展示其功能（给其他同事或对我们预测模型感兴趣的公司），因此没有必要分享整个实验。

正如已经在[KDnuggets](https://www.kdnuggets.com/2019/01/build-api-machine-learning-model-using-flask.html)中展示的那样，在我们共同工作的互联世界中，最直接的方法是通过HTTP端点公开模型，以便潜在用户可以通过网络远程访问。这可能听起来很简单，但开发一个合适且正确的REST API并不是一项容易的任务。数据科学家需要掌握API编程、网络、REST语义、安全性等知识。此外，如果每个科学家都提出一个实现，我们将得到大量不同且不可互操作的API，这些API大致上完成相同的工作，导致生态系统碎片化。

介绍[DEEPaaS API](https://deepaas.readthedocs.io/): 一个基于[aiohttp](https://docs.aiohttp.org/)构建的机器学习、深度学习和人工智能REST API框架。DEEPaaS是一个软件组件，允许通过HTTP端点公开Python模型的功能（使用您选择的框架实现）。它不需要对原始代码进行修改，并提供自定义选项（输入参数、预期输出等）供科学家选择。

DEEPaaS API遵循[OpenAPI规范（OAS）](https://www.openapis.org/)，因此它允许人类和计算机发现并理解底层模型的能力、输入参数和输出值，而无需检查模型的源代码。

让我们通过一个演示示例来看一下它是如何工作的。

### 将模型插入DEEPaaS

为了更好地说明如何将模型与DEEPaaS集成，我们将使用[scikit-learn](https://scikit-learn.org/)中最知名的示例之一：一个针对[IRIS数据集](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html#sphx-glr-auto-examples-datasets-plot-iris-dataset-py)训练的[支持向量机](https://scikit-learn.org/stable/modules/svm.html)。在这个简单的示例中，我们定义了两个不同的函数，一个用于训练，一个用于执行预测，如下所示：

```py
from joblib import dump, load                                                      
import numpy                                                                       
from sklearn import svm                                                            
from sklearn import datasets                                                       

def train():                                                                       
    clf = svm.SVC()                                                                
    X, y = datasets.load_iris(return_X_y=True)                                     
    clf.fit(X, y)                                                                  
    dump(clf, 'iris.joblib')                                                                                                                                          

def predict(data):                                                                 
    clf = load('iris.joblib')                                                      
    data = numpy.array(data).reshape(1, -1)                                        
    prediction = clf.predict(data)                                                 
    return {"labels": prediction.tolist()} 
```

如你所见，训练函数会将训练好的模型持久化到磁盘，遵循[scikit-learn的教程](https://scikit-learn.org/stable/tutorial/basic/tutorial.html#model-persistence)。接下来的操作是定义训练和预测调用的输入参数。由于这个示例相当简单，我们仅定义预测调用的输入参数。通常，你需要在一个不同的文件中完成这项工作，以避免干扰你的代码，但为了简化，我们将这个特殊函数与我们的IRIS SVM一同添加：

```py
from joblib import dump, load                                                      
import numpy                                                                       
from sklearn import svm                                                            
from sklearn import datasets                                                       
from webargs import fields, validate                                                                                                                                 

def train():                                                                       
    clf = svm.SVC()                                                                
    X, y = datasets.load_iris(return_X_y=True)                                     
    clf.fit(X, y)                                                                  
    dump(clf, 'iris.joblib')                                                                                                                                     

def predict(data):                                                              
    clf = load('iris.joblib')                                                   
    data = numpy.array(data).reshape(1, -1)                                     
    prediction = clf.predict(data)                                              
    return {"labels": prediction.tolist()}                                      

def get_predict_args():                                                         
    args = {                                                                    
        "data": fields.List(                                                    
            fields.Float(),                                                     
            required=True,                                                      
            description="Data to make a prediction. The IRIS dataset expects "  
                        "for values containing the Sepal Length, Sepal Width, " 
                        "Petal Length and Petal Width.",                        
            validate=validate.Length(equal=4),                                  
        ),                                                                      
    }                                                                           
    return args
```

最后一步是为了将其与DEEPaaS API集成，你需要使其可安装（你应该这样做）并使用[Python的setuptools](https://docs.python.org/3.8/distutils/setupscript.html)定义一个入口点。这个入口点将被DEEPaaS用来了解如何加载以及如何将不同的函数连接到定义的端点。我们当前使用`deepaas.model.v2`入口点命名空间，因此我们可以按如下方式创建`setup.py`文件：

```py
from distutils.core import setup                                                   

setup(                                                                             
    name='test-iris-with-deepaas',                                                 
    version='1.0',                                                                 
    description='This is an SVM trained with the IRIS dataset',                    
    author='Álvaro López',                                                         
    author_email='aloga@ifca.unican.es',                                           
    py_modules="iris-deepaas.py",                                                          
    dependencies=['joblib', 'scikit-learn'],                                       
    entry_points={                                                                 
        'deepaas.v2.model': ['iris=iris-deepaas'],                                 
    }                                                                              
) 
```

### 安装和运行DEEPaaS

一旦你的代码准备好，你只需安装你的模块和DEEPaaS API，以便它能够检测到并通过API暴露其功能。为了简化操作，我们可以创建一个虚拟环境并在其中安装所有内容：

```py
$ virtualenv env --python=python3
    (...)
$ source env/bin/activate
(env) $ pip3 install .
    (...)
(env) $ pip3 install deepaas
    (...)
(env) $ deepaas-run

         ##         ###
         ##       ######  ##
     .#####   #####   #######.  .#####.
    ##   ## ## //   ##  //  ##  ##   ##
    ##. .##  ###  ###   // ###  ##   ##
      ## ##    ####     ####    #####.
              Hybrid-DataCloud  ##

Welcome to the DEEPaaS API API endpoint. You can directly browse to the
API documentation endpoint to check the API using the builtint Swagger UI
or you can use any of our endpoints.

    API documentation: http://127.0.0.1:5000/ui
    API specification: http://127.0.0.1:5000/swagger.json
          V2 endpoint: http://127.0.0.1:5000/v2

-------------------------------------------------------------------------

2020-02-04 13:10:50.027 21186 INFO deepaas [-] Starting DEEPaaS version 1.0.0
2020-02-04 13:10:50.231 21186 INFO deepaas.api [-] Serving loaded V2 models: ['iris-deepaas']
```

### 访问API并进行训练和预测

如果一切顺利，你现在应该能够将浏览器指向控制台中打印的URL（`http://127.0.0.1:5000/ui`），并看到一个美观的[Swagger UI](https://swagger.io/tools/swagger-ui/)，它将允许你与模型进行交互。

由于这是一个简单的示例，我们没有提供训练好的模型，因此首先需要进行训练。这将调用`train()`函数并保存训练好的SVM以备后用。你可以通过UI进行此操作，或者通过命令行：

```py
curl -s -X POST "http://127.0.0.1:5000/v2/models/iris-deepaas/train/" -H  "accept: application/json" | python -mjson.tool
{
    "date": "2020-02-04 13:14:49.655061",
    "uuid": "16a3141af5674a45b61cba124443c18f",
    "status": "running"
}
```

训练将异步进行，以便API不会阻塞。你可以从UI检查其状态，或者使用以下调用：

```py
curl -s -X GET "http://127.0.0.1:5000/v2/models/iris-deepaas/train/" | python -mjson.tool
[
    {
        "date": "2020-02-04 13:14:49.655061",
        "uuid": "16a3141af5674a45b61cba124443c18f",
        "status": "done"
    }
]
```

现在模型已经训练完成，我们可以进行预测。IRIS 数据集包含 3 种不同类型的鸢尾花（Setosa, Versicolour 和 Virginica）的花瓣和萼片长度。样本有四列，分别对应萼片长度、萼片宽度、花瓣长度和花瓣宽度。在我们的示例中，让我们尝试获取 `[`5.1\. 3.5, 1.4, 0.2`]` 的观察结果，并查看结果。你可以通过 UI 或命令行如下进行操作：

```py
curl -s -X POST "http://127.0.0.1:5000/v2/models/iris-deepaas/predict/?data=5.1&data=3.5&data=1.4&data=0.2" -H  "accept: application/json" | python -mjson.tool
{
    "predictions": {
        "labels": [
            0
        ]
    },
    "status": "OK"
}
```

正如你所见，结果包含了我们的 SVM 执行的预测。在这种情况下，输入数据的标签是 `0`，这确实是正确的。

### 结论

在这个简单的示例中，我们展示了机器学习从业者如何通过 DEEPaaS API 曝露任何基于 Python 的模型，而不是开发自己制作的 API。这样，数据科学家可以专注于他们的工作，而不必担心编写和开发复杂的 REST 应用程序。此外，使用通用 API，不同模块将共享相同的接口，使其更易于在生产中部署并被不同程序员使用。

**个人简介：[Álvaro López García](https://alvarolopez.github.io/)** ([@Alvaretas](https://www.twitter.com/Alvaretas)) 是 [西班牙国家研究委员会 (CSIC)](http://www.csic.es) 高级计算与电子科学组的研究员，从事分布式计算工作。他是 [DEEP-Hybrid-DataCloud](https://deep-hybrid-datacloud.eu/) H2020 项目的项目协调员，正在建立一个用于在欧洲分布式电子基础设施上进行机器学习和深度学习的平台。

**相关：**

+   [如何在 5 分钟内使用 Flask 构建机器学习模型 API](/2019/01/build-api-machine-learning-model-using-flask.html)

+   [构建一个 Flask API 来自动提取命名实体使用 SpaCy](/2019/04/building-flask-api-automatically-extract-named-entities-spacy.html)

+   [谷歌、Uber 和 Facebook 的开源项目：数据科学和 AI](https://www.kdnuggets.com/2019/11/open-source-projects-google-uber-facebook-data-science-ai.html)

### 更多相关主题

+   [数据科学家的代码块共享新方式](https://www.kdnuggets.com/2022/03/new-ways-sharing-code-blocks.html)

+   [数据共享平台的 5 个关键组件](https://www.kdnuggets.com/2022/05/5-key-components-data-sharing-platform.html)

+   [HuggingChat Python API：你的免费替代方案](https://www.kdnuggets.com/2023/05/huggingchat-python-api-alternative.html)

+   [OpenAI API 初学者指南：你的易于跟随的入门指南](https://www.kdnuggets.com/openai-api-for-beginners-your-easy-to-follow-starter-guide)

+   [如何应对 3 个常见的机器学习挑战](https://www.kdnuggets.com/2022/09/comet-tackle-3-common-machine-learning-challenges.html)

+   [探索思维树提示：AI 如何通过搜索学习推理…](https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html)
