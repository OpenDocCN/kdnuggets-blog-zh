# Python 和 SciKit Learn 0.18 的神经网络初学者指南！

> 原文：[`www.kdnuggets.com/2016/10/beginners-guide-neural-networks-python-scikit-learn.html/2`](https://www.kdnuggets.com/2016/10/beginners-guide-neural-networks-python-scikit-learn.html/2)

### 数据预处理

如果数据未归一化，神经网络可能在允许的最大迭代次数之前就无法收敛。多层感知器对特征缩放很敏感，因此强烈建议对数据进行缩放。请注意，必须对测试集应用相同的缩放，以获得有意义的结果。有很多不同的数据归一化方法，我们将使用内置的 StandardScaler 进行标准化。

```py` ```   StandardScaler(copy=True, with_mean=True, with_std=True)   ```py ````

### 训练模型

现在是时候训练我们的模型了。SciKit Learn 使这一过程非常简单，通过使用估算器对象。在这个例子中，我们将从 SciKit-Learn 的 neural_network 库中导入我们的估算器（多层感知器分类器模型）！

接下来，我们创建模型的一个实例，这里有很多参数可以选择定义和自定义，我们只会定义 hidden_layer_sizes。对于这个参数，你需要传入一个元组，其中包含每一层所需的神经元数量，元组中的第 n 个条目表示 MLP 模型中第 n 层的神经元数量。选择这些数字有很多方法，但为了简单起见，我们将选择 3 层，每层的神经元数量与数据集中的特征数相同：

既然模型已经建立，我们可以将训练数据拟合到模型中，记住这些数据已经过处理和缩放：

```py` ``` MLPClassifier(activation='relu', alpha=0.0001, batch_size='auto', beta_1=0.9,         beta_2=0.999, early_stopping=False, epsilon=1e-08,         hidden_layer_sizes=(30, 30, 30), learning_rate='constant',         learning_rate_init=0.001, max_iter=200, momentum=0.9,         nesterovs_momentum=True, power_t=0.5, random_state=None,         shuffle=True, solver='adam', tol=0.0001, validation_fraction=0.1,         verbose=False, warm_start=False) ```py ````

你可以看到输出显示了模型中其他参数的默认值。我鼓励你尝试不同的参数值，发现它们对模型的影响！

### 预测与评估

现在我们有了一个模型，是时候使用它来获取预测了！我们可以通过对拟合模型调用 predict()方法来轻松完成这一任务：

现在我们可以使用 SciKit-Learn 内置的指标，如分类报告和混淆矩阵来评估我们的模型表现如何：

```py`` ``` [[50  3]   [ 0 90]] ```py   ```` ```py              precision    recall  f1-score   support              0       1.00      0.94      0.97        53            1       0.97      1.00      0.98        90    avg / total       0.98      0.98      0.98       143 ``` ```py`   Looks like we only misclassified 3 tumors, leaving us with a 98% accuracy rate (as well as 98% precision and recall). This is pretty good considering how few lines of code we had to write! The downside however to using a Multi-Layer Preceptron model is how difficult it is to interpret the model itself. The weights and biases won't be easily interpretable in relation to which features are important to the model itself.    However, if you do want to extract the MLP weights and biases after training your model, you use its public attributes coefs_ and intercepts_.    **coefs_** is a list of weight matrices, where weight matrix at index i represents the weights between layer i and layer i+1.    **intercepts_** is a list of bias vectors, where the vector at index i represents the bias values added to layer i+1.    `4`    `30`    `30`    ### Conclusion      Hopefully you've enjoyed this brief discussion on Neural Networks! Try playing around with the number of hidden layers and neurons and see how they effect the results!    Want to learn more? You can check out my Python for Data Science and Machine Learning on Udemy! Get it for 90% off at this link: [www.udemy.com/python-for-data-science-and-machine-learning-bootcamp/?couponCode=KDNUGGETSPY](https://www.udemy.com/python-for-data-science-and-machine-learning-bootcamp/?couponCode=KDNUGGETSPY)    ![Jose Portilla](img/d90b94efa106c950a24ba45cb656336d.png)If you are looking for corporate in-person training, feel free to contact me at: training AT pieriandata.com    **Bio: [Jose Portilla](https://www.udemy.com/user/joseporitlla/)** is a Data Science consultant and trainer who currently teaches online courses on Udemy. He also conducts training as the Head of Data Science for Pierian Data Inc.    **Related:**    *   Deep Learning Key Terms, Explained *   A Beginner’s Guide to Neural Networks with R! *   Why Do Deep Learning Networks Scale?     * * *      ## Our Top 3 Course Recommendations      ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - Get on the fast track to a career in cybersecurity.    ![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - Up your data analytics game    ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - Support your organization in IT    * * *      ### More On This Topic    *   [Stop Learning Data Science to Find Purpose and Find Purpose to…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html) *   [What Makes Python An Ideal Programming Language For Startups](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html) *   [Three R Libraries Every Data Scientist Should Know (Even if You Use Python)](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html) *   [A $9B AI Failure, Examined](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html) *   [Top Resources for Learning Statistics for Data Science](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html) *   [The 5 Characteristics of a Successful Data Scientist](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html) `````
