# Scikit-Learn 及更多用于机器学习的合成数据集生成工具

> 原文：[`www.kdnuggets.com/2019/09/scikit-learn-synthetic-dataset.html`](https://www.kdnuggets.com/2019/09/scikit-learn-synthetic-dataset.html)

评论

越来越明显的是，大型科技巨头如谷歌、Facebook 和微软在他们最新的机器学习算法和工具方面非常慷慨（他们免费提供这些资源），因为目前进入算法世界的门槛相当低。开源社区和工具（如 scikit-learn）取得了长足的进展，[大量开源项目正在推动数据科学](https://insidebigdata.com/2015/03/16/open-source-software-fuels-a-revolution-in-data-science/)、数字分析和机器学习的发展。站在 2018 年，我们可以安全地说，[算法、编程框架和机器学习包（甚至教程和学习这些技术的课程）并不是稀缺资源，而是高质量数据才是。](https://hbr.org/2015/03/data-monopolists-like-google-are-threatening-the-economy)

这通常在数据科学（DS）和机器学习（ML）从业者面临的一个棘手问题，即调整和优化这些算法时。值得指出的是，本文涉及的是算法调查、教学学习和模型原型制作的数据稀缺性，而非扩展和运行商业操作。讨论的并非如何获取你正在开发的酷炫旅行或时尚应用的优质数据。这类消费者、社交或行为数据收集有其自身的问题。然而，即使是像获取优质数据集来测试某种算法方法的局限性和多变性这样的简单事情，也往往并不那么简单。

### 为什么你需要一个合成数据集？

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

如果你从零开始学习，最明智的建议是从简单的小规模数据集开始，这样你可以将其绘制在二维图中，以直观地理解模式，并亲自查看机器学习算法的工作效果。

然而，随着数据维度的爆炸，视觉判断必须扩展到更复杂的事项——如*学习和样本复杂性、计算效率、类别不平衡*等概念。

此时，实验灵活性和数据集性质之间的权衡开始发挥作用。**你总是可以找到一个大型真实数据集来实践算法。但那仍然是一个固定的数据集，具有固定的样本数量、固定的底层模式以及固定的类别分离程度**，在正负样本之间。你还必须调查，

+   选择的测试数据和训练数据的比例如何影响算法的性能和鲁棒性。

+   在面对不同程度的类别不平衡时，度量指标的鲁棒性如何。

+   需要进行何种偏差-方差权衡。

+   算法在训练数据和测试数据中的各种噪声特征下的表现如何（即标签中的噪声以及特征集中的噪声）。

+   你如何实验并揭示你的机器学习算法的弱点？

事实证明，使用单一的真实数据集来完成这些任务是相当困难的，因此，你必须愿意使用合成数据，这些数据在足够随机的情况下捕捉到真实数据集的所有变化，但又足够可控，以帮助你科学地研究你正在构建的特定机器学习流程的优缺点。

尽管我们在本文中不会讨论这个问题，但可以很容易地评估这种合成数据集在敏感应用中的潜在好处——例如医学分类或金融建模，在这些领域，获取高质量标注数据集通常是昂贵且禁止的。

### 合成数据集在机器学习中的基本特征

目前理解到的是，合成数据集是通过编程生成的，而不是来源于任何类型的社会或科学实验、商业交易数据、传感器读数或手动标注的图像。然而，这些数据集绝不是完全随机的，生成和使用合成数据进行机器学习必须由一些总体需求指导。特别是，

+   数据可以是数值型、二进制型或分类型（有序或无序），特征的数量和数据集的长度可以是任意的。

+   必须存在一定程度的随机性，但同时，用户应能够选择多种统计分布作为数据的基础，即底层随机过程可以被精确控制和调整。

+   如果用于分类算法，则类别分离的程度应该是可控的，以使学习问题变得简单或困难。

+   随机噪声可以以可控的方式插入。

+   生成速度应足够高，以便对各种数据集进行实验，这些数据集适用于特定的 ML 算法，即，如果合成数据是基于真实数据集的数据增强，那么增强算法必须具有计算效率。

+   对于回归问题，可以使用复杂的非线性生成过程来获取数据——实际物理模型可能会对此有所帮助。

在下一节中，我们将展示如何使用一些最流行的 ML 库和编程技术来生成合适的数据集。

### 使用 scikit-learn 和 Numpy 生成标准的回归、分类和聚类数据集

Scikit-learn 是基于 Python 的数据科学软件栈中最流行的 ML 库。除了优化良好的 ML 例程和管道构建方法外，它还拥有一套强大的合成数据生成工具方法。

**使用 scikit-learn 进行回归**

Scikit-learn 的 *dataset.make_regression* 函数可以创建随机回归问题，具有任意数量的输入特征、输出目标以及**可控的信息耦合程度**。

![](img/657ba8203365893684bc10b7db422899.png)

**使用 Scikit-learn 进行分类**

类似于上面的回归函数，`dataset.make_classification` 生成一个随机的多类分类问题，具有**可控的类别分离和添加的噪声**。如果需要，你也可以随机翻转任何百分比的输出标记来创建更难的分类数据集。

![](img/9ea8b00bc786b5565f57c111dfeac9e3.png)

**使用 Scikit-learn 进行聚类**

scikit-learn 的工具函数可以生成各种聚类问题。最简单的方法是使用 *datasets.make_blobs*，它生成具有可控距离参数的任意数量的聚类。

![](img/292621726f680a7ab94415b03c6614d6.png)

在测试基于相似度的聚类算法或高斯混合模型时，生成具有特定形状的聚类是有用的。我们可以使用 *datasets.make_circles* 函数来完成这项任务。

![](img/e97c95527cd22b85be131fed2dba304e.png)

![](img/fb15c4e64f19a76d77bcc3c90df44c21.png)

在测试使用**支持向量机（SVM）**算法的非线性核方法、最近邻方法如**k-NN**，或甚至测试简单神经网络时，通常建议使用特定形状的数据进行实验。我们可以使用 *dataset.make_moon* 函数生成具有可控噪声的数据。

![](img/7071bf9188177f34d4eb9a2683e1c008.png)

**使用 Scikit-learn 的高斯混合模型**

高斯混合模型（GMM）是研究无监督学习和文本处理/NLP 任务中的主题建模的迷人对象。以下是一个简单函数的示例，展示了为此类模型生成合成数据的简便性：

```py
import numpy as np
import matplotlib.pyplot as plt
import random
def gen_GMM(N=1000,n_comp=3, mu=[-1,0,1],sigma=[1,1,1],mult=[1,1,1]):
"""
Generates a Gaussian mixture model data, from a given list of Gaussian components
N: Number of total samples (data points)
n_comp: Number of Gaussian components
mu: List of mean values of the Gaussian components
sigma: List of sigma (std. dev) values of the Gaussian components
mult: (Optional) list of multiplier for the Gaussian components
) """
assert n_comp == len(mu), "The length of the list of mean values does not match number of Gaussian components"
assert n_comp == len(sigma), "The length of the list of sigma values does not match number of Gaussian components"
assert n_comp == len(mult), "The length of the list of multiplier values does not match number of Gaussian components"
rand_samples = []
for i in range(N):
pivot = random.uniform(0,n_comp)
j = int(pivot)
rand_samples.append(mult[j]*random.gauss(mu[j],sigma[j]))
return np.array(rand_samples)

```

![](img/10dbf89f56b673447617cbaf6bc9ea3f.png)

![](img/d6a42569513170155593f3da14baf48d.png)

![](img/28a084e2eeef1eed3a1d81895575456e.png)

![](img/f8e0eb7264d15906f91821e7a858789d.png)

### 超越 scikit-learn：从符号输入生成合成数据

虽然上述功能对许多问题可能足够，但生成的数据是真正随机的，用户对实际机制的控制较少。

生成过程的分析。在许多情况下，可能需要一种可控的方式来基于明确的解析函数（涉及线性、非线性、理性或甚至超越项）生成回归或分类问题。以下文章展示了如何**结合符号数学包 SymPy 和 SciPy 的函数**从给定的符号表达式生成合成的回归和分类问题。

[基于符号表达式的随机回归和分类问题生成](https://towardsdatascience.com/random-regression-and-classification-problem-generation-with-symbolic-expression-a4e190e37b8d)

![](img/4d2ec41a8a3cd5fcdb2fb338f13c7151.png)

从给定的符号表达式生成的回归数据集。

![](img/b3b57cc7402e8accd19850f7225bbda9.png)

从给定的符号表达式生成的分类数据集。

**使用 scikit-image 进行图像数据增强**

深度学习系统和算法是数据的贪婪消费者。然而，为了测试深度学习算法的局限性和鲁棒性，通常需要通过细微的图像变化来馈送算法。**Scikit-image** 是一个出色的图像处理库，基于与 scikit-learn 相同的设计原则和 API 模式，提供了数百种实用的函数来完成这一图像数据增强任务。

以下文章很好地提供了这些想法的全面概述：

[数据增强 | 如何在数据有限的情况下使用深度学习。](https://medium.com/nanonets/how-to-use-deep-learning-when-you-have-limited-data-part-2-data-augmentation-c26971dc8ced)

我们展示了一些选定的增强过程示例，从单一图像开始，并对其进行数十种变化，**有效地扩展数据集的流形，创建一个巨大的合成数据集**，以稳健的方式训练深度学习模型。

**色调、饱和度、明度通道**

![](img/3b34127d9031309c2ca1ce6f8bb1abab.png)

**裁剪**

![](img/03b7fa3370116c332105da6c2b9dcd55.png)

**随机噪声**

![](img/461cc92ce173811ad8d5d6ab971c9cf0.png)

**旋转**

![](img/c2b4cb1495a328907c8ac9186f9fcfbf.png)

**漩涡效果**

![](img/95a563dff0ffbfbc70f721178a37346a.png)

**带分割的随机图像合成器**

NVIDIA 提供了一个名为 NDDS 的 UE4 插件，旨在帮助计算机视觉研究人员导出高质量的合成图像及其元数据。它支持图像、分割、深度、物体姿态、边界框、关键点和自定义模板。除了导出工具，插件还包含各种组件，支持生成随机图像用于数据增强和目标检测算法训练。随机化工具包括照明、物体、相机位置、姿态、纹理和干扰物。这些组件使深度学习工程师能够轻松创建随机场景以训练他们的 CNN。以下是 Github 链接，

[NVIDIA 深度学习数据合成器](https://github.com/NVIDIA/Dataset_Synthesizer)

**使用 pydbgen 生成分类数据**

*Pydbgen*是一个[轻量级纯 Python 库](https://github.com/tirthajyoti/pydbgen)，用于生成随机有用条目（如姓名、地址、信用卡号、日期、时间、公司名称、职位名称、车牌号等），并将其保存到 Pandas 数据框对象、SQLite 数据库文件中的表格或 MS Excel 文件中。[你可以在这里阅读文档](https://pydbgen.readthedocs.io/en/latest/)。以下是描述其用法和工具的文章，

[介绍 pydbgen：一个随机数据框/数据库表生成器](https://towardsdatascience.com/introducing-pydbgen-a-random-dataframe-database-table-generator-b5c7bdc84be5)

这里有一些说明性的示例，

![](img/929da64192dc0676df83ebf465f4decb.png)

![](img/d296932c57b43adf13658bccefae99d7.png)

![](img/34f20a7b06934eae55bcd9b99fc3cbd7.png)

**合成时间序列数据集**

有许多论文和代码库用于生成合成时间序列数据，这些数据使用在现实生活中的多变量时间序列中观察到的特殊函数和模式。以下 Github 链接提供了一个简单的示例：

[合成时间序列](https://nbviewer.jupyter.org/github/tirthajyoti/Machine-Learning-with-Python/blob/master/Synthetic_data_generation/Synth_Time_series.ipynb)

![](img/096403f26039965b65caae44e143feb2.png)

**合成音频信号数据集**

音频/语音处理是深度学习从业者和机器学习爱好者特别感兴趣的领域。**谷歌的 NSynth 数据集**是一个合成生成的（使用神经自编码器和人类及启发式标记的组合）短音频文件库，声音由各种类型的乐器发出。以下是该数据集的详细描述。

[NSynth 数据集](https://magenta.tensorflow.org/datasets/nsynth)

### 强化学习的合成环境

**OpenAI Gym**

用于强化学习的合成学习环境中最伟大的资源是**OpenAI Gym**。它包含了大量的预编程环境，用户可以在这些环境中实施他们的强化学习算法，以进行性能基准测试或解决潜在的弱点。

![](img/e4d973a51aa33a052967f89f9ca2238d.png)

**随机网格世界**

对于强化学习的初学者来说，练习和实验一个简单的网格世界通常会有所帮助，在这个网格世界中，智能体必须穿越迷宫以到达一个终端状态，每一步和终端状态都有给定的奖励/惩罚。

只需几行简单的代码，就可以合成任意大小和复杂度的网格世界环境（用户指定终端状态和奖励向量的分布）。

查看这个 Github 仓库，获取灵感和代码示例。

[`github.com/tirthajyoti/RL_basics`](https://github.com/tirthajyoti/RL_basics)

### 总结

在本文中，我们介绍了一些用于机器学习的合成数据生成示例。读者应当明确，这些示例并不是数据生成技术的详尽列表。实际上，除了 scikit-learn 之外，许多商业应用也提供了相同的服务，因为需要用多样的数据来训练 ML 模型的需求正在快速增长。然而，如果你作为数据科学家或 ML 工程师，创建自己的合成数据生成编程方法，这将为你的组织节省资金和资源，避免投资于第三方应用程序，同时也能让你以整体和有机的方式规划 ML 流水线的开发。

希望你喜欢这篇文章，并且可以很快在你的项目中开始使用这里描述的一些技术。

[原文](https://blog.exxactcorp.com/scikit-learn-and-more-synthetic-dataset-generation-for-ml/)。经许可转载。

**相关内容：**

+   [Scikit Learn 入门：Python 机器学习的金标准](https://www.kdnuggets.com/2019/02/introduction-scikit-learn-gold-standard-python-machine-learning.html)

+   [应对机器学习中数据缺乏的 5 种方法](https://www.kdnuggets.com/2019/06/5-ways-lack-data-machine-learning.html)

+   [合成数据生成：新数据科学家的必备技能](https://www.kdnuggets.com/2018/12/synthetic-data-generation-must-have-skill.html)

### 更多相关内容

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [检索增强生成：信息检索与…的结合](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [如何使用合成数据来克服机器学习中的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [机器学习的合成数据](https://www.kdnuggets.com/synthetic-data-for-machine-learning)

+   [假装直到成功：生成逼真的合成客户数据集](https://www.kdnuggets.com/2022/01/fake-realistic-synthetic-customer-datasets-projects.html)

+   [合成数据的社区来了，这就是我们需要它的原因](https://www.kdnuggets.com/2022/04/community-synthetic-data-need.html)
