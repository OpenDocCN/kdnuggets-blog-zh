# 提升你的数据科学技能。学习线性代数。

> 原文：[https://www.kdnuggets.com/2018/05/boost-data-science-skills-learn-linear-algebra.html](https://www.kdnuggets.com/2018/05/boost-data-science-skills-learn-linear-algebra.html)

[评论](#comments)

**由 [Hadrien Jean](https://hadrienj.github.io/) 提供，机器学习科学家**

![标题图像](../Images/75e3109a91dc7c11ec03c852eb7eddd5.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

我想介绍一系列博客文章及其对应的 Python Notebooks，这些笔记汇总了 Ian Goodfellow、Yoshua Bengio 和 Aaron Courville（2016）所著的[《深度学习书籍》](http://www.deeplearningbook.org/)中的内容。这些 Notebooks 的目的是帮助初学者/高级初学者掌握深度学习和机器学习所涉及的线性代数概念。掌握这些技能可以提高你理解和应用各种数据科学算法的能力。在我看来，这些概念是机器学习、深度学习和数据科学的基础之一。

这些笔记涵盖了线性代数的第二章。我喜欢这一章，因为它给出了在机器学习和深度学习领域中最常用的知识。因此，它是任何想要深入了解深度学习并掌握有助于更好理解深度学习算法的线性代数概念的人的绝佳大纲。

你可以在[Github](https://github.com/hadrienj/deepLearningBook-Notes)上找到所有的笔记，以及这个帖子在[我的博客](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-Introduction/)上的版本。

### 开始学习线性代数

本系列的目标是为初学者提供足够的线性代数知识，以便能够掌握机器学习和深度学习。然而，我认为深度学习书籍中的线性代数章节对初学者来说有点困难。因此，我决定为本章的每个部分制作代码、示例和图示，以增加可能对初学者不明显的步骤。我还认为，通过示例传达的信息和知识与通过一般定义一样多。插图是了解一个思想大局的方式。最后，我认为编码是具体实验这些抽象数学概念的绝佳工具。与纸笔一起，它为你可以尝试推动理解的新领域添加了一层。

> 编码是具体实验抽象数学概念的绝佳工具

图形表示对于理解线性代数也非常有帮助。我尝试将概念与图表（以及生成这些图表的代码）结合起来。我在制作这系列时最喜欢的表示方式是你可以将任何矩阵视为空间的线性变换。在几个章节中，我们将扩展这一思想，看看它如何有助于理解特征分解、奇异值分解（SVD）或主成分分析（PCA）。

### Python/Numpy 的使用

此外，我发现创建和阅读示例对理解理论非常有帮助。这就是为什么我构建了 Python 笔记本。目标有两个方面：

1.  提供一个使用 Python/Numpy 应用线性代数概念的起点。由于最终目标是将线性代数概念应用于数据科学，因此在理论和代码之间不断切换是很自然的。你只需一个包含主要数学库如 Numpy/Scipy/Matplotlib 的工作 Python 环境。

1.  提供一个更具体的基础概念视角。我发现通过在这些笔记本中玩耍和实验对建立对某些复杂理论概念或符号的理解非常有用。我希望阅读它们同样有用。

### 课程大纲

课程大纲严格遵循 [深度学习书籍](http://www.deeplearningbook.org/)，因此如果你在阅读时无法理解某个具体点，可以找到更多细节。这里是内容的简要描述：

1. [标量、向量、矩阵和张量](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.1-Scalars-Vectors-Matrices-and-Tensors/)

![](../Images/721460e64383e9c263fb973eca1996c3.png)

对向量、矩阵、转置和基本操作（向量或矩阵的加法）进行轻量介绍。还介绍了 Numpy 函数，最后提到广播。

2. [矩阵和向量的乘法](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.2-Multiplying-Matrices-and-Vectors/)

![](../Images/a0acdaab1b8703dfee0c951183414585.png)

本章主要讨论点积（向量和/或矩阵乘法）。我们还将看到一些相关属性。然后，我们将了解如何使用矩阵表示法综合线性方程组。这是接下来章节的主要过程。

3. [单位矩阵和逆矩阵](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.3-Identity-and-Inverse-Matrices/)

![](../Images/ba1ebba2b70d5005390b1a04b7ef1444.png)

我们将讨论两个重要的矩阵：单位矩阵和逆矩阵。我们将探讨它们在线性代数中的重要性以及如何使用 Numpy 来操作它们。最后，我们将看到如何用逆矩阵解决线性方程组的例子。

4. [线性依赖与张成](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.4-Dependence-and-Span/)

![](../Images/40e9175ac89a6a0944a2734ef847bee2.png)

在本章中，我们将继续研究线性方程组。我们将看到这样的系统不能有多于一个解，也不能少于无限多个解。我们将探讨这种说法的直观感受、图形表示和证明。然后，我们将回到系统的矩阵形式，并考虑 Gilbert Strang 所说的*行图*（即查看行，也就是多个方程）和*列图*（即查看列，也就是系数的线性组合）。我们还将了解什么是线性组合。最后，我们将看到过定方程组和欠定方程组的示例。

5. [范数](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.5-Norms/)

![](../Images/8f3bde6e39888b2e71e9cc92366d27f0.png)

向量的范数是一个函数，它接受一个向量作为输入并输出一个正值。它可以被看作是向量的*长度*。例如，它被用来评估模型预测值与实际值之间的距离。我们将看到不同种类的范数（L⁰、L¹、L²…）及其示例。

6. [特殊类型的矩阵和向量](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.6-Special-Kinds-of-Matrices-and-Vectors/)

![](../Images/28a70818cd310bdd33a37b8cc06575c7.png)

我们在 [2.3](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.3-Identity-and-Inverse-Matrices/) 中见过一些非常有趣的特殊矩阵。在本章中，我们将看到其他类型的向量和矩阵。虽然这一章不长，但理解它对接下来的章节很重要。

7. [特征分解](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.7-Eigendecomposition/)

![](../Images/ebfb0bae806ec9e30ffbca3eceba51d1.png)

本章将介绍线性代数的一些主要概念。我们将从了解特征向量和特征值开始。我们将看到矩阵可以被视为线性变换，并且对其特征向量应用矩阵会得到方向相同的新向量。接着我们将了解如何将二次方程表达为矩阵形式。我们将看到对应于二次方程的矩阵的特征分解可以用来找到其最小值和最大值。作为额外内容，我们还将看到如何在 Python 中可视化线性变换！

8. [奇异值分解](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.8-Singular-Value-Decomposition/)

![](../Images/ea7e7f1055d0fd6d387da59c86ffd071.png)

我们将看到另一种矩阵分解的方法：奇异值分解（SVD）。从本系列开始，我就强调了矩阵可以看作是空间中的线性变换。通过 SVD，你将一个矩阵分解为另外三个矩阵。我们将看到这些新矩阵可以被视为空间的*子变换*。而不是一次性完成变换，我们将其分解为三个步骤。作为额外内容，我们还将把 SVD 应用于图像处理。我们将看到 SVD 对一张卢西鹅的示例图像的效果，所以请继续阅读！

9. [摩尔-彭若斯伪逆](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.9-The-Moore-Penrose-Pseudoinverse/)

![](../Images/516836aabbb7a5391cde0cd545a12dc4.png)

我们已经看到并不是所有的矩阵都有逆矩阵。这很遗憾，因为逆矩阵用于解决方程组。在某些情况下，方程组没有解，因此逆矩阵不存在。然而，找到一个接近解的值（在最小化误差方面）可能很有用。这可以通过伪逆来实现！我们将看到如何使用伪逆找到数据点集合的最佳拟合线。

10. [迹运算符](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.10-The-Trace-Operator/)

![](../Images/80c46dd752d9d9ba9fe293e315671626.png)

我们将了解矩阵的迹。它将在关于主成分分析（PCA）的最后一章中用到。

11. [行列式](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.11-The-determinant/)

![](../Images/51c9c1ba39cf42a58dcd1ecafe77c24e.png)

本章讲解的是矩阵的行列式。这个特殊的数字可以告诉我们关于矩阵的很多信息！

12. [示例：主成分分析](https://hadrienj.github.io/posts/Deep-Learning-Book-Series-2.12-Example-Principal-Components-Analysis/)

![](../Images/5b332dbd21fe278fa0dccb8456cb94ae.png)

这是本系列关于线性代数的最后一章！它讲解的是主成分分析（PCA）。我们将使用在前几章中获得的一些知识来理解这一重要的数据分析工具！

### 要求

这些内容面向初学者，但对于至少有一些数学经验的人来说应该会更容易理解。

### 享受

希望你能在这一系列内容中找到一些有趣的东西。我尽力做到准确。如果你发现错误/误解/打字错误… 请报告！你可以发送电子邮件或在notebooks的Github上打开问题和拉取请求。

### 参考资料

Goodfellow, I., Bengio, Y., & Courville, A. (2016). 深度学习。MIT出版社。

**简介：[Hadrien Jean](https://hadrienj.github.io/)** 是一位机器学习科学家。他拥有巴黎高等师范学校的认知科学博士学位，在那里他通过行为和电生理数据研究听觉感知。他曾在行业中工作，构建了用于语音处理的深度学习管道。在数据科学与环境交汇处，他从事使用深度学习应用于音频录音的生物多样性评估项目。他还定期在Le Wagon（数据科学训练营）创建内容和教学，并在他的博客([hadrienj.github.io](http://hadrienj.github.io))上撰写文章。

[原文](https://towardsdatascience.com/boost-your-data-sciences-skills-learn-linear-algebra-2c30fdd008cf)。已获许可转载。

**相关内容：**

+   [掌握数据科学和机器学习数学基础的7本书](/2018/04/7-books-mathematical-foundations-data-science.html)

+   [15个数据科学数学MOOCs](/2015/09/15-math-mooc-data-science.html)

+   [实际数据科学家需要的实用技能](/2016/05/practical-skills-practical-data-scientists-need.html)

### 更多相关话题

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目的，并以目的来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
