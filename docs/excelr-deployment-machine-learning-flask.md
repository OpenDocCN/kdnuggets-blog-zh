# 使用 Flask 部署机器学习模型

> 原文：[https://www.kdnuggets.com/2019/12/excelr-deployment-machine-learning-flask.html](https://www.kdnuggets.com/2019/12/excelr-deployment-machine-learning-flask.html)

赞助帖子。

到目前为止，我们已经开发了许多数据科学模型，对测试数据进行了预测，并离线检查了结果。在现实世界中，生成预测只是数据科学项目的一部分。让我们考虑一个使用 CPU/GPU 的机器学习来检测垃圾短信的情况。一个朴素贝叶斯分类器在 CPU/GPU 上用垃圾短信和非垃圾短信进行训练。训练好的模型作为服务部署到网上，供大众或用户使用。

本博客将向我们解释机器学习算法部署的基本知识。在本博客中，我们将专注于开发一个用于垃圾短信识别的朴素贝叶斯模型，并使用 Flask（Flask 是一个 Python 的 Web 服务开发微框架）为模型创建 API。API 允许我们通过 HTTP 请求利用算法的分类能力。

Flask 基于 2 个组件：WSGI（Web 服务器网关接口）工具包和 Jinja2 模板。WSGI 用于 Web 应用程序，Jinja2 提供网页。Flask 总是用于较大规模的机器学习项目。Flask 可用于构建 REST 应用程序、电子邮件服务、聊天应用等。

开始吧！

> **有关 [数据科学课程](https://www.excelr.com/data-science-course-training-in-bangalore) 的更多信息，请关注我们 @ExcelR**

**第 1 步：**

首先，我们将使用一个数据集（messages.csv）来构建一个分类模型，该模型将准确识别哪些文本是垃圾信息。[该分类器模型基于词袋特征来识别垃圾邮件](https://en.wikipedia.org/wiki/Naive_Bayes_spam_filtering)。一旦我们训练好模型，建议保存该模型以便将来使用，从而减少重新训练的时间。为此，我们将模型保存为.pkl 文件以备将来使用。这是一个 pickle 文件，是用于保存和加载 Python 对象文件的原生 Python 库。

![图示](../Images/5fda73b0870dc601a816120d9d068ac5.png)

**第 2 步：**

下一步，我们需要开发一个具有用户友好界面的 Web 应用程序，界面上包含一个用于用户输入的表单字段。在将用户定义的消息输入到 Web 应用程序后，它将把消息传递到结果页面，显示是否为垃圾信息。

**第 3 步：**

为了开发 Web 应用程序，我们在桌面上创建一个名为 Spam Identifier 的项目文件夹，确保在其中创建文件目录。文件夹中的目录结构将如下所示：

```py``` ``` app.py  HTML files/  homepage.html  output.html  style/  design.css ```py      **步骤 4：**    检查这个`app.py`文件是否包含用于执行Flask网络应用程序的Python源代码，它应该包括用于分类垃圾信息的ML程序（.pkl文件）。此文件是链接到HTML文件和调用模型的API，以显示用户定义输入的输出。      **步骤 5：**    然后将应用程序作为一个模块运行，以使用参数`__name__`初始化一个新的Flask实例，这将帮助Flask在同一文件夹（垃圾识别器）中找到HTML文件夹（包含HTML文件）。   ````` ```py app = Flask(__name__) ```      **Step 6:**    To furnish the web page, flask will look for html files present in the subdirectory called html files. In this case, we have two create 2 html files: homepage.html and output.html.      **Step 7:**    CSS can be used to design the look of 2 html documents.  Don’t forget to save design.css file in a subdirectory called style, which happens to be the default directory of Flask.      **Step 8:**    Next use the route decorator (@app.route('/')) url to activate the execution of homepage function. On the back of which homepage function will simply provision homepage.html file available in the sub category.    ![Figure](../Images/57430ef9739a3b907f33bbc63fb7a635.png)        **Step 9:**    Thereafter, we define predict function, wherein we read messages.csv set, pre-process the text, make estimations, and store the model.    ![Figure](../Images/5245a7b61b83b3da1217558417d4196d.png)        **Step 10:**    Next, we access the new message inputted by the user and make a prediction. Afterwards, make use of the POST method to transfer the form data to the server.    ![Figure](../Images/28016e58470791e2a254766acbd5a479.png)        **Step 11:**    Post which output.html file can be activated through the render_htmlfiles function inside the predict function which we defined in the app.py script.    ![Figure](../Images/05c90d4806d1b9ea9afab42525f833a3.png)        **Step 12:**    Finally, use the run function to only run the application on the server, by using if statement with __name__ == '__main__'. Later, set the debug=True argument inside the app.run method, to trigger flask's debugger.    ![Figure](../Images/876e1daadac7086724d0d73fc53be2e2.png)        **Step 13:**    For running the model , you can start running the API either by double clicking app.py, or executing the command from the terminal as follows:   ```py` ``` cd -Spam Identifier  Python app.py ```py      **Step 14:**    Next, open a web browser and navigate to http://127.0.0.1:5000/, where you should see a simple website which is similar to the one below  ![Figure](../Images/b0aae6a00972d2e411ac2d0d6f0b37fd.png)  Congratulations! You have now created an end-to-end NLP application at zero cost and little effort. Hosting and sharing data science models can be uncomplicated. Developing android apps, chatbots and many more applications dependent on machine learning algorithms back-end can be created with no difficulty. When you have time, I recommend to start reading about deployment in machine learning. That’s it for flask. Thanks for reading.    For More Information related to [Data Science Course](https://www.excelr.com/data-science-course-training-in-bangalore) Follow us @ExcelR     * * *      ## Our Top 3 Course Recommendations      ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - Get on the fast track to a career in cybersecurity.    ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - Up your data analytics game    ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - Support your organization in IT    * * *      ### More On This Topic    *   [A Full End-to-End Deployment of a Machine Learning Algorithm into a…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html) *   [From Data Collection to Model Deployment: 6 Stages of a Data…](https://www.kdnuggets.com/2023/01/data-collection-model-deployment-6-stages-data-science-project.html) *   [Back to Basics Week 4: Advanced Topics and Deployment](https://www.kdnuggets.com/back-to-basics-week-4-advanced-topics-and-deployment) *   [Top 7 Model Deployment and Serving Tools](https://www.kdnuggets.com/top-7-model-deployment-and-serving-tools) *   [Predicting Cryptocurrency Prices Using Regression Models](https://www.kdnuggets.com/2022/05/predicting-cryptocurrency-prices-regression-models.html) *   [How to Make Large Language Models Play Nice with Your Software…](https://www.kdnuggets.com/how-to-make-large-language-models-play-nice-with-your-software-using-langchain) ```` ```py`` `````
