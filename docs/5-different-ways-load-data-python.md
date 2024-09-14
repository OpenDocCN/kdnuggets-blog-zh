# 在Python中加载数据的5种不同方法

> 原文：[https://www.kdnuggets.com/2020/08/5-different-ways-load-data-python.html](https://www.kdnuggets.com/2020/08/5-different-ways-load-data-python.html)

![](../Images/0a347571eee0ed768118961a39020925.png)

作为一个初学者，你可能只知道一种加载数据的方法（通常是CSV），那就是使用*pandas.read_csv*函数读取它。这是一个最成熟和强大的函数，但其他方法也非常有用，有时会派上用场。

我将要讨论的方法有：

+   **手动**函数

+   **loadtxt**函数

+   **genfromtxt**函数

+   **read_csv**函数

+   **Pickle**

我们将使用的数据集可以在[**这里**](http://eforexcel.com/wp/downloads-18-sample-csv-files-data-sets-for-testing-sales/)找到。它被命名为100-Sales-Records。

## **导入**

我们将使用Numpy、Pandas和Pickle包，所以要导入它们。

```py
import numpy as np
import pandas as pd
import pickle
```

# 1. 手动函数

这是最困难的，因为你必须设计一个自定义函数来加载数据。你需要处理Python的常规文件概念，并使用这些概念来读取*.csv*文件。

让我们对100销售记录文件进行操作。

```py
def load_csv(filepath):
    data =  []
    col = []
    checkcol = False
    with open(filepath) as f:
        for val in f.readlines():
            val = val.replace("\n","")
            val = val.split(',')
            if checkcol is False:
                col = val
                checkcol = True
            else:
                data.append(val)
    df = pd.DataFrame(data=data, columns=col)
    return df
```

嗯，这是什么？？？看起来有点复杂的代码！！让我们一步步解析，以便你了解发生了什么，并可以应用类似的逻辑来读取自己的*.csv*文件。

在这里，我创建了一个*load_csv*函数，它接受一个参数，即你想读取的文件路径。

我有一个名为*data*的列表，它将包含我的CSV文件数据，还有另一个名为*col*的列表，它将包含我的列名。现在在手动检查CSV文件后，我知道我的列名在第一行，所以在第一次迭代中，我需要将第一行的数据存储在*col*中，其余的行存储在*data*中。

为了检查第一次迭代，我使用了一个名为*checkcol*的布尔变量，它的值为False，当第一次迭代中它的值为False时，它会将第一行的数据存储在*col*中，然后将*checkcol*设置为True，这样我们就会处理*data*列表，并将其余的值存储在*data*列表中。

## 逻辑

这里的主要逻辑是，我使用*readlines()*函数遍历文件。这个函数返回一个包含文件中所有行的列表。

在读取标题时，它将新行检测为*\n*字符，这是一种行终止字符，因此为了去除它，我使用了*str.replace*函数。

由于这是一个*.csv*文件，所以我必须根据*逗号*来分隔内容，因此我将使用*string.split(',')*来分割字符串。在第一次迭代中，我将把包含列名的第一行存储在一个名为*col*的列表中。然后，我将把所有数据追加到名为*data*的列表中。

为了更优雅地读取数据，我将其返回为数据框格式，因为数据框比numpy数组或Python列表更易于阅读。

## **输出**

```py
myData = load_csv('100 Sales Record.csv')
print(myData.head())
```

![XXXXX](../Images/f03b047096fbdb9279487fda55df4680.png)

自定义函数中的数据。

## **优点与缺点**

其重要好处是你对文件结构有全部的灵活性和控制权，可以按照你想要的格式和方式读取并存储数据。

你也可以使用自己的逻辑读取没有标准结构的文件。

其重要缺点是编写复杂，特别是对于标准类型的文件，因为它们可以很容易地被读取。你必须硬编码逻辑，这需要反复试验。

你应该仅在文件格式非标准或你需要灵活性并以库无法提供的方式读取文件时使用它。

# 2\. Numpy.loadtxt 函数

这是Numpy库中的一个内置函数，Numpy是Python中的一个著名数值库。这个函数非常简单，适合加载数据。它对于读取相同数据类型的数据非常有用。

当数据更复杂时，使用这个函数很难读取，但当文件简单易读时，这个函数确实非常强大。

要获取单一类型的数据，你可以下载[this](https://docs.google.com/spreadsheets/d/16mgiYbNz-XaW_r6GXUy2cJ0hy2E-lxwFLaVXAYIAOj0/edit?usp=sharing)虚拟数据集。让我们跳到代码部分。

```py
df = np.loadtxt('convertcsv.csv', delimeter = ',')
```

这里我们简单地使用了*loadtxt*函数，并将*delimeter*设置为*','*，因为这是一个CSV文件。

现在如果我们打印*df*，我们会看到我们的数据以相当不错的numpy数组形式呈现，准备好使用。

```py
print(df[:5,:])
```

![](../Images/935fd87bb21b2d7076aaa36908b7c53a.png)

由于数据量大，我们只打印了前5行。

## **优点与缺点**

使用这个函数的一个重要方面是你可以快速地将数据从文件加载到numpy数组中。

它的缺点是你不能在数据中拥有不同的数据类型或缺失的行。

# 3\. Numpy.genfromtxt()

我们将使用数据集‘100 Sales Records.csv’，这是我们在第一个例子中使用的，以演示我们可以在其中拥有多种数据类型。

让我们跳到代码部分。

```py
data = np.genfromtxt('100 Sales Records.csv', delimiter=',')
```

为了更清楚地看到这一点，我们可以以数据框格式查看，即：

```py
>>> pd.DataFrame(data)
```

![](../Images/93ddc725447b2fa270c1266adf649ede.png)

等等？这是什么？哦，它跳过了所有包含字符串数据类型的列。怎么处理这个问题？

只需添加另一个*dtype*参数并将*dtype*设置为None，这意味着它必须自行处理每一列的数据类型，而不是将整个数据转换为单一的数据类型。

```py
data = np.genfromtxt('100 Sales Records.csv', delimiter=',', dtype=None)
```

然后输出为：

```py
>>> pd.DataFrame(data).head()
```

![](../Images/3247ba2a7d4a9375df900b0bbdd56566.png)

比第一个更好，但是这里我们的列标题变成了行，要将它们设置为列标题，我们必须添加另一个参数，即*names*，并将其设置为*True*，这样它会将第一行作为列标题。

即

```py
data = np.genfromtxt('100 Sales Records.csv', delimiter=',', dtype=None, names = True)
```

我们可以将其打印为：

```py
>>> pd.DataFrame(df3).head()
```

![](../Images/879d28bfba1322bb92bff7c200cdd2fe.png)

在这里我们可以看到，它成功地将列名添加到了数据框中。

现在最后一个问题是那些数据类型为字符串的列实际上并不是字符串，而是以*字节*格式存储的。你可以看到每个字符串前面都有一个*b'*，所以为了处理它们，我们需要将它们解码为utf-8格式。

```py
df3 = np.genfromtxt('100 Sales Records.csv', delimiter=',', dtype=None, names=True, encoding='utf-8')
```

这将返回我们所需格式的数据框。

```py
>>> pd.DataFrame(df3)
```

![](../Images/7538bc624a1120c71dda7e8a2ea65797.png)

# 4\. Pandas.read_csv()

Pandas是一个非常流行的数据处理库，而且使用非常广泛。它非常重要且**成熟**的一个函数是*read_csv()*，它可以非常容易地读取任何**.csv**文件，并帮助我们处理它。让我们在我们的100-Sales-Record数据集上做这个。

这个函数因其易用性而非常受欢迎。你可以将它与我们之前的代码进行比较，并检查它。

```py
>>> pdDf = pd.read_csv('100 Sales Record.csv')
>>> pdDf.head()
```

![](../Images/a1f2478cfae283ced2691eff19112fe5.png)

你猜怎么着？我们完成了。这实际上是非常简单和易于使用的。Pandas.read_csv确实提供了许多其他参数来调整我们的数据集，例如在我们的*convertcsv.csv*文件中，我们没有列名，所以我们可以这样读取它：

```py
>>> newdf = pd.read_csv('convertcsv.csv', header=None)
>>> newdf.head()
```

![](../Images/36f53da09649fd35f12d2f84d3bf7dec.png)

我们可以看到它读取了没有标题的*csv*文件。你可以在官方文档中探索所有其他参数[这里](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv)。

# 5\. Pickle

当你的数据不是以良好的、可读的格式存在时，你可以使用pickle将其保存为二进制格式。然后你可以使用pickle库轻松重新加载它。

我们将取出我们的100-Sales-Record CSV文件，并首先将其保存为pickle格式，以便我们可以读取它。

```py
with open('test.pkl','wb') as f:
    pickle.dump(pdDf, f)
```

这将创建一个新的文件*test.pkl*，其中包含我们来自**Pandas**标题的*pdDf*。

现在要使用pickle打开它，我们只需使用*pickle.load*函数。

```py
with open("test.pkl", "rb") as f:
    d4 = pickle.load(f)

>>> d4.head()
```

![](../Images/196bc7ff09954d29f60e934e5f3dbc18.png)

在这里，我们成功地从pickle文件中加载了数据，格式为*pandas.DataFrame*。

你现在知道了5种在Python中加载数据文件的方法，这可以帮助你在日常项目中以不同的方式加载数据集。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT方面支持你的组织

* * *

### 更多相关主题

+   [使用Python进行自动化机器学习：不同方法的比较…](https://www.kdnuggets.com/2023/03/automated-machine-learning-python-comparison-different-approaches.html)

+   [数据挖掘如何不同于机器学习？](https://www.kdnuggets.com/2022/06/data-mining-different-machine-learning.html)

+   [学习不同的数据可视化是如何工作的](https://www.kdnuggets.com/2022/09/datacamp-learn-different-data-visualizations-work.html)

+   [如何从不同背景过渡到数据科学？](https://www.kdnuggets.com/2023/05/transition-data-science-different-background.html)

+   [Interview Kickstart 数据科学面试课程—它的独特之处](https://www.kdnuggets.com/2022/10/interview-kickstart-data-science-interview-course-makes-different.html)

+   [终极指南：NLP中的不同词嵌入技术](https://www.kdnuggets.com/2021/11/guide-word-embedding-techniques-nlp.html)
