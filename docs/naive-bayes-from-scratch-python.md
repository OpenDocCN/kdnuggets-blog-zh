# 仅使用Python从零开始的朴素贝叶斯 – 无需华丽的框架

> 原文：[https://www.kdnuggets.com/2018/10/naive-bayes-from-scratch-python.html](https://www.kdnuggets.com/2018/10/naive-bayes-from-scratch-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2018/10/naive-bayes-from-scratch-python.html/2#comments)

**作者 [Aisha Javed](https://towardsdatascience.com/@aisha.jv70)**。

![](../Images/2ad3a747bf9c8c8e67cdfb4deec57050.png)

### **从零开始展开朴素贝叶斯! 第2部分 ****????**

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

所以在我之前的博客文章[**从零开始展开朴素贝叶斯! 第1部分**](https://www.kdnuggets.com/2018/09/unfolding-naive-bayes.html)????中，我试图解码朴素贝叶斯（NB）机器学习算法的工作原理，通过它的算法洞察，你也一定会发现这个算法相当简单。在这篇博客中，我们将详细讲解其完整的逐步Python实现（仅使用基础Python），并且会非常明显*从零开始编码NB有多么简单*，而且NB*在分类上并不那么天真！*

### **目标受众是谁？** **????** **????** **????** **机器学习初学者** **????** **????????**

由于我一直想为绝对初学者解读机器学习，而且有人说如果你不能解释清楚，那你可能没真正理解它，所以这篇博客也特别针对*寻找深入但没有那些可怕的希腊数学公式的机器学习初学者*（老实说，那些吓人的数学对我来说也没什么意义！）

### **本教程的成果 — 朴素贝叶斯的实用Python实现**

如我刚刚提到的，完整的朴素贝叶斯Python实现讲解

*一旦你阅读完这篇博客文章，你将完全掌握90%的朴素贝叶斯理解和实现，剩下的10%则是从应用角度掌握它！*

![](../Images/d107ef572d935909b0b62daf11e48e7a.png)

**机器学习小鸟从零到英雄！！！**

### 定义路线图….. ????

**里程碑 # 1: **[**数据预处理函数**](https://towardsdatascience.com/na%C3%AFve-bayes-from-scratch-using-python-only-no-fancy-frameworks-a1904b37222d#6154)

**里程碑 # 2：**[**实现 NaiveBayes 类——定义训练和测试函数**](https://towardsdatascience.com/na%C3%AFve-bayes-from-scratch-using-python-only-no-fancy-frameworks-a1904b37222d#fc37)

**里程碑 # 3：**[**在训练数据集上训练 NB 模型**](https://towardsdatascience.com/na%C3%AFve-bayes-from-scratch-using-python-only-no-fancy-frameworks-a1904b37222d#0287)

**里程碑 # 4：**[**使用训练好的 NB 模型进行测试**](https://towardsdatascience.com/na%C3%AFve-bayes-from-scratch-using-python-only-no-fancy-frameworks-a1904b37222d#a15f)

**里程碑 # 5：**[**证明 NaiveBayes 类的代码绝对通用！**](https://towardsdatascience.com/na%C3%AFve-bayes-from-scratch-using-python-only-no-fancy-frameworks-a1904b37222d#02f4)

在我们开始编写 Naive Bayes 的 Python 代码之前，我假设你已经熟悉：

1.  Python 列表

1.  Numpy 以及一点向量化代码

1.  字典

1.  正则表达式

*让我们开始 Pythonic 的实现吧！*

### **定义数据预处理函数**

让我们从几个在实现 Naive Bayes 时需要的导入开始

> **里程碑 # 1 达成** 🤩

### **实现 NaiveBayes 类——定义训练和测试函数**

***附加部分***：我们将为 NB 分类器编写一个完全通用的代码！无论训练数据集中有多少类，以及给定什么文本数据集——它仍然能够训练一个完全有效的模型 🤔 🤔 🤔

NaiveBayes 的代码只是 *稍微复杂*——但我们只需要花费最多 10-15 分钟就能掌握它！之后，你绝对可以自称为“NB 大师” 🤓

#### 这段代码在做什么？🤔

NaiveBayes 类中总共定义了四个函数：

```py

1\. def addToBow(self,example,dict_index)
2\. def train(self,dataset,labels)
3\. def getExampleProb(self,test_example)
4\. def test(self,test_set)

```

代码被分为两个主要函数，即训练和测试函数。一旦你理解了这两个函数内部定义的语句，你将确切了解代码的实际作用以及其他两个函数的调用顺序。

```py

1\. Training function that trains NB Model :
   def train(self,dataset,labels)
2\. Testing function that is used to predict class labels 
   for the given test examples :
   def test(self,test_set)

```

另外两个函数是为了补充这两个主要函数而定义的

```py

1\. BoW function that supplements training function 
   It is called by the train function.
   It simply splits the given example using space as a tokenizer 
   and adds every tokenized word to its corresponding BoW : 
   def addToBow(self,example,dict_index)
2\. Probability function that supplements test function. 
   It is called by the test function.
   It estimates probability of the given test example so that 
   it can be classified for a class label :
   def getExampleProb(self,test_example)

```

> 你也可以在这个 [**Jupyter Notebook**](https://github.com/aishajv/Unfolding-Naive-Bayes-from-Scratch/blob/master/%23%20Unfolding%20Na%C3%AFve%20Bayes%20from%20Scratch!%20Take-2%20%F0%9F%8E%AC.ipynb) 中查看上述代码

如果我们定义一个 NB 类，而不是使用传统的结构化编程方法，组织和重用代码会容易得多。这就是定义一个 NB 类及其相关函数的原因。

> 我们不仅仅想编写代码，更希望编写美观、整洁、实用且可重用的代码。没错，我们希望拥有一个优秀数据科学家可能具备的所有特质！

你知道吗？每当我们处理一个文本分类问题并打算使用NB解决时，我们只需实例化其对象，通过相同的编程接口，就可以训练一个NB分类器。而且，根据面向对象编程的一般原则，我们只在类内部定义与该类相关的函数，因此所有与NB类无关的函数将会被单独定义。

> **里程碑 # 2 达成** 🎉 🎉

### 相关话题

+   [KDnuggets新闻，4月13日：数据科学家应该关注的Python库…](https://www.kdnuggets.com/2022/n15.html)

+   [高斯朴素贝叶斯，解释](https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html)

+   [朴素贝叶斯算法：你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)

+   [理解贝叶斯定理的3种方式将提升你的数据科学技能](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [接近机器学习过程的框架](https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html)

+   [Chip Huyen 分享实施ML系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)
��了实施 ML 系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)
