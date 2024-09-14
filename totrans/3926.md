# 传统编程和机器学习有多大不同？

> 原文：[https://www.kdnuggets.com/2018/12/different-conventional-programming-machine-learning.html](https://www.kdnuggets.com/2018/12/different-conventional-programming-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Avneesh Sharma，技术产品管理**

![标题图片](../Images/15028624695298db41beacf31e05677d.png)

工程让我们突破了人类能力的极限。我们利用对自然的理解来服务于我们的目的。无论是高性能的机械设备还是编码的硅芯片，计算机迄今为止是自然力量最复杂的应用之一，帮助人类推动能力的极限，即计算机能够执行的许多任务，往往是人类或一组人无法如此迅速和高效完成的。正如史蒂夫·乔布斯所说，[计算机就像我们思想的自行车](https://www.youtube.com/watch?v=ob_GX50Za6c&feature=youtu.be&t=34)。

我从小就对计算机充满了好奇。我在2001年第一次接触计算机，当时我写了一个BASIC程序来加两个数字。我惊讶于无论加法有多么困难，计算机都能立即给出答案。

当我听到**机器学习**时，我无法抑制惊讶。我无法理解，跟我习惯的普通软件程序不同，我不需要事先详细教会计算机如何处理所有未来的场景。计算机可以自己学习如何解决问题。这对人类来说是一个巨大的飞跃。

### 什么是机器学习？

术语‘[机器学习](https://en.wikipedia.org/wiki/Machine_learning#Overview)’并不新鲜。它是由亚瑟·塞缪尔在1959年创造的。它借鉴了计算机科学、统计学、线性代数和概率论等不同领域的理解，并应用于解决实际问题。机器学习使计算机能够从历史数据中学习，制定出可以用于解决未来类似问题的解决方案，而不需要显式地教会计算机所有可能场景的组合。而机器学习的实际应用正是那些连明确的数学解决方案都难以阐明的问题。

如果一个现实世界的问题符合以下条件，它就是机器学习应用的候选：

1.  历史数据存在大量

1.  数据中存在模式

1.  数学上极难确定一个解决方案

### 它与传统编程有何不同？

传统编程的方法是给计算机输入一组指令以处理特定场景。之后，计算机将利用其计算能力帮助人类更快、更高效地处理数据。而在机器学习中，大量数据被输入到计算机中，计算机处理这些数据并得到一个称为训练模型（解决方案）的东西。然后使用这个模型来解决现实世界中未见过的问题。

### 示例

让我们以一个玩具问题来演示区别。这个问题接受一个输入数字，并尝试用3和5进行除法。如果数字能被3整除，则打印'fizz'；如果能被5整除，则打印'buzz'；如果能被两者整除，则打印'fizzbuzz'。如果不能被3或5整除，则打印'other'。这被称为[Fizzbuzz](https://en.wikipedia.org/wiki/Fizz_buzz)游戏。

### **传统编程**

在传统编程中，给计算机输入指令非常简单，因为我们只有4种场景需要验证，并根据这些场景打印输出。Python代码可以如下编写，但如果你不喜欢编码，可以跳过阅读代码。

```py
def fizzbuzz(n):  # if the number is divisible by 3 as well as by 5 and returns # "FizzBuzz"
    if n % 3 == 0 and n % 5 == 0:
        return 'FizzBuzz'# If the first condition is not satisfied then it checks if it is # divisible by 3 and return "Fizz"
    elif n % 3 == 0:
        return 'Fizz'# If both of the above tests are not satisfying, then it will check # whether it is divisible by 5 and return "Buzz"
    elif n % 5 == 0:
        return 'Buzz'# If all the conditions above do not satisfy then it returns "Other"
    else:
        return 'Other'

```

### **机器学习**

假设我们已经有很多数字，其输出结果已知，即它是'fizz'、'buzz'还是'fizzbuzz'。我们现在需要做的就是编写机器学习代码，并输入（训练）现有数据。然后通过用未见过的数据进行测试来验证我们是否成功创建了模型。如果模型能在不实际计算结果的情况下提供输出，那么我们就达到了目的。

我们将使用Google的[Tensorflow](https://www.tensorflow.org/)库来实现。以下是实现中的一些代码片段。

再次强调，如果你不喜欢编码，可以跳过代码。完整的工作代码可以在我的Github账户的[这里](https://github.com/sharmaavneesh/MachineLearning/tree/master/fizzbuzz)找到。

**a. 创建模型**

```py
# Placeholders are the type of variables nodes where data can be 
# fed from outside when we actually run the model

#Placeholder for input data
inputTensor  = tf.placeholder(tf.float32, [None, 10]) 
# Placeholder for output data  
outputTensor = tf.placeholder(tf.float32, [None, 4])    
# The number of neurons which 1st hidden neurons will have i.e. 1000
NUM_HIDDEN_NEURONS_LAYER_1 = 1000          

# Learning rate, which will be later used to optimize for optmizer function
# Learning rate defines at what rate the optimizwer function will move towards minima per iteration
# Less than optimum learning rate will slow down the process where as higher learning rate will have chances of 
# skipping the minima all together. So it will never converge rather it will keep on going back and forth.
LEARNING_RATE = 0.05                     

# Initializing the weights to Normal Distribution
# The weights will keep on adjusting towards optimum values in each iteration. Conceptually, weights determine the 
# discrimination factor of a particular variable in the newural network. More the weight, more will be it's 
# contribution in determining the solution.
def init_weights(shape):
    return tf.Variable(tf.random_normal(shape,stddev=0.01))

# Initializing the input to hidden layer weights
# We will need a total of 10(input layer number) * 100(number of hidden layers)
input_hidden_weights  = init_weights([10, NUM_HIDDEN_NEURONS_LAYER_1])

# Initializing the hidden to output layer weights
# In this case we will need 100(number of hidden neurons layers) * 4(output neurons, we have only 4 categories)
hidden_output_weights = init_weights([NUM_HIDDEN_NEURONS_LAYER_1, 4])

# Computing values at the hidden layer
# Matrix multiplication is done and then rectifier neural network activation function is used 
# for regularization of the resulting multiplication
hidden_layer = tf.nn.relu(tf.matmul(inputTensor, input_hidden_weights))

# Computing values at the output layer
# Matrix multiplication of hidden layer and output weights it done. 
output_layer = tf.matmul(hidden_layer, hidden_output_weights)

# Defining Error Function
# Error function computes the difference between actual output and model output.
# Here we are calculating the error in the output as compared to the output label
error_function = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=output_layer, labels=outputTensor))

# Defining Learning Algorithm and Training Parameters
# We are using Gradient Descent function to optimize the error or to reach the minima.
training = tf.train.GradientDescentOptimizer(LEARNING_RATE).minimize(error_function)

# Prediction Function
prediction = tf.argmax(output_layer, 1)

```

**b. 训练模型**

```py
NUM_OF_EPOCHS = 5000
BATCH_SIZE = 128

training_accuracy = []

with tf.Session() as sess:

    # Set Global Variables ?
    # We had only defined the model previously. To run the model, all the variables need to be initialized and run.
    # Actual computation can only start after the initialization.
    tf.global_variables_initializer().run()

    for epoch in tqdm_notebook(range(NUM_OF_EPOCHS)):

        #Shuffle the Training Dataset at each epoch
        #Shuffling is done to have even more randmized data, which adds to the generalization of model even more.
        p = np.random.permutation(range(len(processedTrainingData)))
        processedTrainingData  = processedTrainingData[p]
        processedTrainingLabel = processedTrainingLabel[p]

        # Start batch training
        # With batch size of 128, there will be total of 900/128 runs in each epoch where 900 is the total 
        # training data.
        for start in range(0, len(processedTrainingData), BATCH_SIZE):
            end = start + BATCH_SIZE
            sess.run(training, feed_dict={inputTensor: processedTrainingData[start:end], 
                                          outputTensor: processedTrainingLabel[start:end]})
        # Training accuracy for an epoch
        # We are checking here the accuracy of model after each epoch
        training_accuracy.append(np.mean(np.argmax(processedTrainingLabel, axis=1) ==
                             sess.run(prediction, feed_dict={inputTensor: processedTrainingData,
                                                             outputTensor: processedTrainingLabel})))
        # Testing
        predictedTestLabel = sess.run(prediction, feed_dict={inputTensor: processedTestingData})

```

**c. 测试模型**

```py
wrong   = 0
right   = 0

predictedTestLabelList = []

#Comparing the predicted value with the actual label in training data
for i,j in zip(processedTestingLabel,predictedTestLabel):
    predictedTestLabelList.append(decodeLabel(j))

    if np.argmax(i) == j:
        right = right + 1
    else:
        wrong = wrong + 1

print("Errors: " + str(wrong), " Correct :" + str(right))

print("Testing Accuracy: " + str(right/(right+wrong)*100))

```

**d. 结果：**

```py
Errors: 2  Correct :98
Testing Accuracy: 98.0 %

```

### 结论

我们重复地输入相同的数据5000次，每次都重新打乱。每次运行后，我们通过测量测试数据的误差率来评估模型的准确性。如图所示，模型的准确率为98%。我绘制了每次迭代后的误差图。数值越接近1，模型表现越好。图表如下：

![](../Images/cadee22cf29994bce3ff797d5cda9e61.png)

### 结束语

如上所示，在这个特定的玩具案例中——传统程序将始终给出正确的输出，而机器学习可能无法达到100%的准确水平。

现在设想一个场景，你正在经营一家像Netflix这样的公司，拥有数百万客户。每个客户都有一组独特的偏好。例如，有些人可能喜欢纪录片和喜剧电影，但他在周二观看纪录片，在周日观看喜剧电影。而另一些人则在周六晚上狂看视频，喜欢在周日观看恐怖片等等。你可以想象，当有这么多变量决定确切的偏好时，记录每个人的习惯会多么复杂。因为如果你想向每个客户推荐他/她将要观看的电影，你必须了解该人的确切偏好。

对于传统编程，这个问题变得越来越困难，因为：

1.  你不知道决定一个人观看习惯的所有因素。

1.  即使你知道了，解决方案也无法扩展到数百万用户，因为对于每个人，你都需要根据他/她的习惯编写一个单独的解决方案。

这里实现了机器学习。一般来说，在这种情况下，机器学习将基于两个前提进行训练：

1.  根据你的过去数据，你最有可能观看哪些电影？

1.  现在人们像你一样在看什么？

请通过下面的评论告诉我你的想法。

PS：文章中使用的完整代码可以在 [这里](https://github.com/sharmaavneesh/MachineLearning/tree/master/fizzbuzz) 找到。

**参考资料**

1.  https://www.amazon.com/Pattern-Recognition-Learning-Information-Statistics/dp/0387310738

1.  https://www.youtube.com/watch?v=mbyG85GZ0PI&index=1&list=PLD63A284B7615313A

**简介： [Avneesh Sharma](https://www.linkedin.com/in/avneesh-sharma/)** 是布法罗大学（2018-19）的管理信息系统硕士候选人。他拥有8年的软件产品/功能从头开发经验。他喜欢解决对业务有直接影响的问题，并使最终用户满意。他相信，创造技术通常是解决现实世界痛点的一种手段，以简化生活。

[原文](https://www.linkedin.com/pulse/how-different-conventional-programming-machine-learning-sharma/)。经许可转载。

**相关内容：**

+   [回归分析真的算作机器学习吗？](/2017/06/regression-analysis-really-machine-learning.html)

+   [机器学习与统计学]( /2016/11/machine-learning-vs-statistics.html)

+   [大数据、圣经密码和邦费罗尼]( /2016/07/big-data-bible-codes-bonferroni.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关话题

+   [数据挖掘与机器学习的不同之处是什么？](https://www.kdnuggets.com/2022/06/data-mining-different-machine-learning.html)

+   [使用 Python 的自动化机器学习：不同方法的比较](https://www.kdnuggets.com/2023/03/automated-machine-learning-python-comparison-different-approaches.html)

+   [在 Python 中加载数据的 5 种不同方法](https://www.kdnuggets.com/2020/08/5-different-ways-load-data-python.html)

+   [自然语言处理中的不同词嵌入技术终极指南](https://www.kdnuggets.com/2021/11/guide-word-embedding-techniques-nlp.html)

+   [AI 生成的体育亮点：不同的方法](https://www.kdnuggets.com/2022/03/aigenerated-sports-highlights-different-approaches.html)

+   [现实世界中 NLP 应用的范围：一种不同的解决方案](https://www.kdnuggets.com/2022/03/different-solution-problem-range-nlp-applications-real-world.html)
