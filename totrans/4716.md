# 如何为机器学习设置 Python 环境

> 原文：[https://www.kdnuggets.com/2019/02/setup-python-environment-machine-learning.html](https://www.kdnuggets.com/2019/02/setup-python-environment-machine-learning.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![Header image](../Images/7bae1bb29491cd1ca0909193a110506c.png)

设置您的 Python 环境以进行机器学习可能是一项棘手的任务。如果您以前从未设置过这样的环境，可能会花费数小时尝试不同的命令来使其正常工作。但我们只想直接进入机器学习！

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

在本教程中，您将学习如何设置一个*稳定的*Python机器学习开发环境。您将能够直接深入到机器学习中，再也不必担心安装软件包的问题。

### (1) 设置 Python 3 和 Pip

第一步是安装 pip，一个 Python 包管理工具：

```py
sudo apt-get install python3-pip
```

使用 pip，我们可以通过简单的`pip install *your_package*`安装任何在[*Python 包索引*](https://pypi.org/)中的 Python 包。*很快您将看到我们如何用它来设置我们的*虚拟环境*。

接下来，我们将设置 Python 3 为运行`pip`或`python`命令时的默认版本。这使得使用 Python 3 更加方便。如果我们不这样做，那么如果我们想使用 Python 3，就必须记住每次输入`pip3`和`python3`！

为了强制将 Python 3 设置为默认版本，我们将修改`~/.bashrc`文件。从命令行执行以下命令来查看该文件：

```py
nano ~/.bashrc
```

向下滚动到**# 一些更多的 ls 别名**部分，并添加以下行：

```py
alias python='python3'
```

保存文件并重新加载您的更改：

```py
source ~/.bashrc
```

哇！Python 3 现在是您的默认 Python！您可以通过简单地在命令行运行`python *your_program*`来使用它。

### (2) 创建一个虚拟环境

现在我们将设置一个*虚拟环境*。在这个环境中，我们将安装进行机器学习所需的所有 Python 包。

我们使用虚拟环境来分隔我们的编码设置。想象一下，如果你在某个时刻想在电脑上做两个不同的项目，而这两个项目需要不同版本的不同库。如果将它们都放在同一个工作环境中可能会很混乱，你可能会遇到库版本冲突的问题。你的项目1的ML代码需要`numpy`的1.0版本，但项目2需要1.15版本。糟糕！

虚拟环境允许我们隔离工作区域，以避免这些冲突。

首先，安装相关的包：

```py
sudo pip install virtualenv virtualenvwrapper
```

一旦我们安装了virtualenv和virtualenvwrapper，我们将需要再次编辑我们的`~/.bashrc`文件。将这3行放在文件底部并保存。

```py
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

保存文件并重新加载更改：

```py
source ~/.bashrc
```

很好！现在我们可以像这样创建我们的虚拟环境：

```py
mkvirtualenv ml
```

我们刚刚创建了一个名为`ml`的虚拟环境。要进入它，请执行以下操作：

```py
workon ml
```

很棒！在`ml`虚拟环境中进行的任何库安装都会被隔离在其中，不会与其他环境冲突！因此，每当你想运行依赖于`ml`环境中安装的库的代码时，只需首先使用`workon`命令进入该环境，然后像平常一样运行你的代码。

如果你需要退出虚拟环境，请运行以下命令：

```py
deactivate
```

### (3) 安装机器学习库

现在我们可以安装我们的ML库了！我们将使用最常用的库：

+   **numpy:** 处理矩阵，尤其是数学运算

+   **scipy:** 科学和技术计算

+   **pandas:** 数据处理、操作和分析

+   **matplotlib:** 数据可视化

+   **scikit learn:** 机器学习

这是一个简单的小技巧，可以一次性安装所有这些库！创建一个`requirements.txt`文件，并列出你希望安装的所有包，如下所示：

```py
numpy
scipy
pandas
matplotlib
scikit-learn
```

一旦完成，只需执行以下命令：

```py
pip install -r requirements.txt
```

看！Pip会一次性安装文件中列出的所有包。

恭喜，你的环境已设置好，你准备好进行机器学习了！

### 想要学习？

关注我[ twitter](https://twitter.com/GeorgeSeif94)，我会发布最新最精彩的AI、技术和科学内容！

**Bio: [George Seif](https://towardsdatascience.com/@george.seif94)** 是一名认证极客和AI / 机器学习工程师。

[原文](https://towardsdatascience.com/how-to-setup-a-python-environment-for-machine-learning-354d6c29a264)。转载已获许可。

**相关:**

+   [为你的回归问题选择最佳机器学习算法](/2018/08/selecting-best-machine-learning-algorithm-regression-problem.html)

+   [数据科学家需要了解的5种聚类算法](/2018/06/5-clustering-algorithms-data-scientists-need-know.html)

+   [Python中的5种快速简单的数据可视化（附代码）](/2018/07/5-quick-easy-data-visualizations-python-code.html)

### 更多相关主题

+   [成为伟大数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家都应该掌握的 6 个预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
