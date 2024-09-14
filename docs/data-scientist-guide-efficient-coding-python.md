# 数据科学家的 Python 高效编码指南

> 原文：[https://www.kdnuggets.com/2021/08/data-scientist-guide-efficient-coding-python.html](https://www.kdnuggets.com/2021/08/data-scientist-guide-efficient-coding-python.html)

[评论](#comments)

**由[Dr. Varshita Sher](https://varshitasher.medium.com/)，数据科学家**

在这篇文章中，我想分享一些我在过去一年中从配对编程中吸收的编写更清晰代码的技巧。一般来说，将它们作为我日常编码例程的一部分，帮助我生成了高质量的 Python 脚本，这些脚本随着时间的推移容易维护和扩展。

> 有没有想过为什么**高级开发者的代码看起来比初级开发者的代码好得多**？继续阅读，来弥合这个差距……

我将提供实际的编码场景来说明如何使用这些技巧，而不是给出通用的示例！这是一个[Jupyter Colab Notebook](https://colab.research.google.com/drive/1gSIJd_HY88A_bq-Z0zMMzFYb1hjRI8DO?usp=sharing)，如果你想跟着一起操作！

## 1\. 在处理`for`循环时使用`tqdm`。

想象一下遍历一个*大型*的可迭代对象（列表、字典、元组、集合），却不知道代码是否已经运行完成！*真糟糕*，*对吧*！在这种情况下，请确保使用`tqdm`构造来显示进度条。

例如，在我读取存在于44个不同目录中的所有文件时（这些路径已经存储在名为`fpaths`的列表中），以显示进度：

```py
from tqdm import tqdmfiles = list()
fpaths = ["dir1/subdir1", "dir2/subdir3", ......]

for fpath in tqdm(fpaths, desc="Looping over fpaths")):
         files.extend(os.listdir(fpath))
```

![](../Images/9bcd861ff49e3bd8b0e25a728332679a.png)

使用 tqdm 与 “for” 循环

*注意：使用*`*desc*`*参数来为循环指定一个简短的描述。*

## 2\. 在编写函数时使用`类型提示`。

简单来说，就是在 Python 函数定义中明确声明所有参数的类型。

我希望有具体的用例来强调*何时*使用类型提示，但事实是，我经常使用它们。

这是一个假设的函数`update_df()`示例。它通过附加一行包含来自模拟运行的有用信息（例如使用的分类器、得分的准确率、训练-测试分割大小以及该特定运行的额外备注）的数据帧来更新给定的数据帧。

```py
def update_df(**df: pd.DataFrame**, 
              **clf: str**, 
              **acc: float**,
              **remarks: List[str] = []**
              **split:float** = 0.5) -> **pd.DataFrame**:

    new_row = {'Classifier':clf, 
               'Accuracy':acc, 
               'split_size':split,
               'Remarks':remarks}

    df = df.append(new_row, ignore_index=True)
    return df
```

![](../Images/930d1b32e90748482182847e086597d5.png)

几点需要注意：

+   函数定义中的`->`符号后面的数据类型（`def update_df(.......) **->** pd.DataFrame`）表示函数返回值的类型，即在这种情况下是 Pandas 的数据框。

+   如果有默认值，可以像往常一样以`param:type = value`的形式指定。（例如：`split: float = 0.5`）

+   如果一个函数没有返回任何内容，可以自由使用`None`。例如：`def func(a: str, b: int) -> None: print(a,b)`

+   要返回混合类型的值，例如，假设一个函数可以在标志`option`设置时打印语句，或者在标志未设置时返回一个`int`：

```py
from typing import Union
def dummy_args(*args: list[int], option = True) -> Union[None, int]:

     if option:

          print(args)

     else:

          return 10
```

*注意：从 Python 3.10 开始，*`*Union*`* 不再是必需的，因此你可以直接这样做：*

```py
def dummy_args(*args: list[int], option = True) -> None | int:

     if option:

          print(args)

     else:

          return 10
```

+   在定义参数类型时，你可以尽可能具体，就像我们对 `remarks: List[str]` 所做的那样。我们不仅指定它应该是一个 `List`，而且它应该仅仅是 `str` 类型的列表。

    为了好玩，尝试在调用函数时传递一个整数列表到 `remarks`。你会看到没有错误返回！*为什么会这样？* 因为 Python 解释器不会根据你的类型提示执行任何类型检查。

> [类型提示对你的代码没有其他影响，只是增加了文档](https://www.pythonlikeyoumeanit.com/Module5_OddsAndEnds/Writing_Good_Code.html#What-is-It-Good-For?-(Absolutely-Nothing))。

尽管如此，包含它仍然是一个好的实践！我觉得它在编写函数时能带来更多的清晰度。此外，当有人调用这样的函数时，他们会看到输入参数的清晰提示。

![](../Images/9f1d83d3b0fc37b952d7bfb93d64ea76.png)

带类型提示的函数调用提示

## 3. 使用 args 和 kwargs 处理参数数量未知的函数。

想象一下：你想写一个函数，接收 *一些* 目录路径，并打印每个目录中的文件数量。问题是，我们不知道用户会输入 *多少* 个路径！可能是 2 个，也可能是 20 个！所以我们不确定应该在函数定义中定义多少个参数。显然，写一个像 `def count_files(file1, file2, file3, …..file20)` 这样的函数会很傻。在这种情况下，`args` 和（有时 `kwargs`）非常有用！

> **Args** 用于指定未知数量的 **位置** 参数。
> 
> **Kwargs** 用于指定未知数量的 **关键字** 参数。

### Args

这是一个函数 `count_files_in_dir()` 的示例，它接收 `project_root_dir` 和一个任意数量的文件夹路径（在函数定义中使用 `*fpaths`）。作为输出，它会打印每个这些文件夹中的文件数量。

```py
def count_files_in_dir(project_root_dir, *fpaths: str):

       for path in fpaths:

            rel_path = os.path.join(project_root_dir, path)
            print(path, ":", len(os.listdir(rel_path)))
```

![](../Images/8e8c20d0e9910db055928a7971a3ff0a.png)

计算 Google Colab 目录中的文件数量

在函数调用中，我们传入了 5 个参数。由于函数定义期望一个 *必需* 的位置参数，即 `project_root_dir`，它会自动知道 `"../usr"` 必须是这个参数。其余的参数（在这个例子中是四个）都被 `*fpaths` 吸收，用于计算文件数量。

*注意：这种吸收技术的正确术语是“参数打包”，即剩余参数被打包成 *`**fpaths*`*。*

### Kwargs

让我们来看一下必须接收未知数量的 *关键字* 参数的函数。在这种情况下，我们必须使用 `kwargs` 而不是 `args`。以下是一个简短的（无用的）示例：

```py
def print_results(**results):

     for key, val in results.items():
        print(key, val)
```

![](../Images/25cd205f9fd4647a869c6dfc99d2f52b.png)

使用方式与`*args`非常相似，但现在我们能够将任意数量的*关键字*参数传递给函数。这些参数作为键值对存储在`**results`字典中。从这里开始，可以使用`.items()`轻松访问字典中的项。

我在工作中发现了`kwargs`的两个主要应用：

+   合并字典（*有用但较少有趣*）

```py
dict1 = {'a':2 , 'b': 20}
dict2 = {'c':15 , 'd': 40}

merged_dict = {**dict1, **dict2}

*************************
{'a': 2, 'b': 20, 'c': 15, 'd': 40}
```

+   扩展现有方法（*更有趣*）

```py
def myfunc(a, b, flag, **kwargs):

       if flag:
           a, b = do_some_computation(a,b)

       actual_function(a,b, **kwargs)
```

*注意：查看*[*matplotlib的绘图函数使用*](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib-pyplot-plot)`[*kwargs*](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib-pyplot-plot)`* 来指定图表的可选修饰，如线宽和标签。*

这里有一个实际使用`**kwargs`扩展方法的案例，来自我最近的一个项目：

我们通常使用 Sklearn 的`train_test_split()`来拆分`X`和`y`。在处理其中一个 GAN 项目时，我需要将生成的合成图像拆分为与拆分真实图像及其相应标签所用的*相同*的训练测试集。此外，我还希望能够传递任何其他通常传递给`train_test_split()`的参数。最后，`stratify`必须始终传递，因为我在处理人脸识别问题（并希望所有标签都在训练集和测试集中存在）。

为此，我们创建了一个名为`custom_train_test_split()`的函数。我包含了一些打印语句来展示内部发生的情况（并省略了一些片段以简化说明）。

```py
def custom_train_test_split(clf, y, *X, stratify, **split_args):    *print("Classifier used: ", classifier)
    print("Keys:", split_args.keys())
    print("Values: ", split_args.values())
    print(X)
    print(y)
    print("Length of passed keyword arguments: ", len(split_args))*

    trainx,testx,*synthetic,trainy,testy = train_test_split(
                                               *X,
                                               y,
                                               stratify=stratify,
                                               **split_args
                                               ) *######### OMITTED CODE SNIPPET #############
    # Train classifier on train and synthetic ims
    # Calculate accuracy on testx, testy
    ############################################*

    *print("trainx: ", trainx, "trainy: ",trainy, '\n',  "testx: ", 
    testx, "testy:", testy)* *print("synthetic: ", *synthetic)*
```

*注意：在调用此函数时，为了便于理解，我使用了虚拟数据替换了实际的图像向量和标签（见下图）。不过，代码同样适用于真实图像！*

![](../Images/5c62be74580ff4d94753b0667954be87.png)

图 A 使用函数定义中的kwargs调用函数

注意事项：

+   在函数调用语句中使用的所有*关键字*参数（**除了**`stratify`），将作为键值对存储在`**split_args`字典中。（要验证，请查看蓝色输出。）

    你可能会问为什么不使用`stratify`？这是因为根据函数定义，它是一个*必需的*仅限关键字参数，而不是一个*可选的*参数。

+   所有*非关键字*（即位置）参数（如`"SVM"`、`labels`等）在函数调用中会存储在函数定义中的前三个参数，即`clf`、`y`和`*X`（是的，传递的顺序很重要）。然而，在函数调用中我们有*四个*参数，即`"SVM"`、`labels`、`ims`和`synthetic_ims`。*那第四个参数该存储在哪里？*

    记住我们在函数定义中使用了`*X`作为第三个参数，因此传递给函数的所有参数在前两个参数之后都被*打包*（浸泡）到`*X`中。（要验证，请检查绿色输出）。

+   当在我们的函数中调用`train_test_split()`方法时，我们实际上是在使用`*`运算符解包`X`和`split_args`参数（`*X`和`**split_args`），以便将所有元素作为不同的参数传递。

也就是说，

```py
train_test_split(*X, y, stratify = stratify, **split_args)
```

相当于写

```py
train_test_split(ims, synthetic_ims, y, stratify = stratify, train_size = 0.6, random_state = 50)
```

+   当存储`train_test_split()`方法的结果时，我们再次*打包*`synthetic_train`和`synthetic_test`集合到一个单独的`*synthetic`变量中。

![](../Images/eff64186647cd1a947a83aabcbda8884.png)

要检查里面有什么，我们可以使用`*`运算符再次解包它（见粉色输出）。

*注意：如果你想深入了解使用*`***`*运算符进行打包和解包，请查看这篇*[*文章*](https://realpython.com/python-kwargs-and-args/)*。*

## 4\. 使用预提交hooks`。`

我们编写的代码通常很凌乱，缺乏适当的格式，比如尾随空格、尾随逗号、未排序的导入语句、缩进中的空格等。

虽然可以手动修复所有这些问题，但使用[pre-commit hooks](https://pre-commit.com/)可以节省你大量的时间。简单来说，这些hooks可以通过一行命令进行自动格式化——`pre-commit run`。

这里有一些来自官方文档的[简单步骤](https://pre-commit.com/#install)来开始并[创建一个](https://pre-commit.com/index.html#2-add-a-pre-commit-configuration)`[.pre-commit-config.yaml](https://pre-commit.com/index.html#2-add-a-pre-commit-configuration)`[文件](https://pre-commit.com/index.html#2-add-a-pre-commit-configuration)。它将包含你关心的所有格式化问题的[hooks](https://pre-commit.com/hooks.html)！

作为纯个人偏好，我倾向于保持我的`.pre-commit-config.yaml`配置文件简单，并使用[Black的](https://black.readthedocs.io/en/stable/integrations/source_version_control.html)预提交配置。

*注意：需要记住的一点是，文件必须被暂存，即在执行*`*pre-commit run*`*之前使用*`*git add .*`*，否则你会看到所有文件都会被跳过：*

![](../Images/74bb1092d28f8ab78b952edfb55eebad.png)

## 5\. 使用.yml配置文件来存储常量`。`

如果你的项目包含大量配置变量，例如数据库主机名、密码、AWS凭证等，请使用`.yml`文件来跟踪所有这些变量。你可以在任何Jupyter Notebook或你希望的脚本中使用这个文件。

由于我大部分工作是为客户提供模型框架，以便他们可以在自己的数据集上重新训练它，我通常使用配置文件来存储文件夹和文件的路径。这也是确保客户在运行你的脚本时只需更改一个文件的好方法。

让我们在项目目录中创建一个`fpaths.yml`文件。我们将存储需要存放图像的根目录。此外，还会存储文件名、标签、属性等的路径。最后，我们还存储合成图像的路径。

```py
image_data_dir: path/to/img/dir *# the following paths are relative to images_data_dir*

fnames:

      fnames_fname: fnames.txt

      fnames_label: labels.txt

      fnames_attr: attr.txt

synthetic:

       edit_method: interface_edits

       expression: smile.pkl

       pose: pose.pkl
```

你可以像这样阅读这个文件：

```py
# open the yml file

with open(CONFIG_FPATH) as f:
     dictionary = yaml.safe_load(f)

# print elements in dictionary

for key, value in dictionary.items():
     print(key + " : " + str(value))
     print()
```

*注意：如果你想深入了解，这里有一个精彩的* [*教程*](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started) *来帮助你入门 yaml。*

## 6\. 奖励：有用的 VS-Code 扩展

虽然确实有很多不错的 Python 编辑器，但我必须说 VSCode 是我见过的最好的 (*对不起，Pycharm*)。为了更好地利用它，考虑从市场中安装这些扩展：

+   [括号配对着色器](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)——允许用颜色识别匹配的括号。

![](../Images/22f7c332c972b0d391e893eca83873db.png)

+   [路径智能感知](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)——允许自动补全文件名。

![](../Images/bc298c53b24cd3daaed9de47e1e6b762.png)

+   [Python Docstring 生成器](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring)——允许为 Python 函数生成 docstring。

![](../Images/85c8b32707fe9632374d81b585627221.png)

使用 VSCode 扩展生成 docstring

*技巧：使用*`*"""*`* 在你编写了函数并使用了类型提示之后生成 docstring。这样生成的 docstring 将包含更多信息，如默认值、参数类型等（见上图右侧）。*

+   [Python Indent](https://marketplace.visualstudio.com/items?itemName=KevinRose.vsc-python-indent)——(*我最喜欢的；由* [Kevin Rose](https://marketplace.visualstudio.com/publishers/KevinRose) *发布*) 允许对多行代码/括号进行正确的缩进。

![](../Images/eee5fbeebed8adbf8634e945e0bf7d3d.png)

来源：[VSCode 扩展市场](https://marketplace.visualstudio.com/items?itemName=KevinRose.vsc-python-indent)

+   [Python 类型提示](https://marketplace.visualstudio.com/items?itemName=njqdev.vscode-python-typehint)——允许在编写函数时自动补全类型提示。

![](../Images/92497f3d02c0316705bc33017a00d027.png)

+   [TODO tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)：(*第二喜欢*) 追踪在编写脚本时插入的所有`TODO`。

![](../Images/495629a719138e57c940c0da96e57c0b.png)

追踪项目中插入的所有 TODO 注释

+   [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)——允许代码自动补全、参数建议（还有很多其他功能，能更快地编写代码）。

恭喜你离成为专业 Python 开发者更近了一步。我打算在学习到更多有趣的技巧时更新这篇文章。如有更简单的方法完成本文提到的某些任务，请随时告知我。

下次再见 :)

### 数据科学面试中逐步解释你的 ML 项目的指南。

### 面试官最喜欢的问题 - 你会如何“扩展你的 ML 模型”？

### 使用 Pandas 进行时间序列分析

### Podurama: 播客播放器

### 阅读 Varshita Sher 博士的每一篇故事（以及 Medium 上其他成千上万的作者的故事）

**简介：[Varshita Sher 博士](https://varshitasher.medium.com/)** 是艾伦·图灵研究所的数据科学家，同时也是牛津大学和 SFU 校友。

[原文](https://towardsdatascience.com/data-scientists-guide-to-efficient-coding-in-python-670c78a7bf79). 已获得授权转载。

**相关：**

+   [编写干净 R 代码的 5 个技巧](/2021/08/5-tips-writing-clean-r-code.html)

+   [Python 数据结构比较](/2021/07/python-data-structures-compared.html)

+   [GitHub Copilot 开源替代品](/2021/07/github-copilot-open-source-alternatives-code-generation.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关话题

+   [KDnuggets 新闻，5 月 4 日：9 门免费的哈佛课程来学习数据…](https://www.kdnuggets.com/2022/n18.html)

+   [你必须知道的 15 个 Python 编程面试问题](https://www.kdnuggets.com/2022/04/15-python-coding-interview-questions-must-know-data-science.html)

+   [3 个困难的 Python 编程面试问题用于数据科学](https://www.kdnuggets.com/2023/03/3-hard-python-coding-interview-questions-data-science.html)

+   [免费 Python 项目编码课程](https://www.kdnuggets.com/2022/08/free-python-project-coding-course.html)

+   [7 个你必须知道的 Python 编程面试技巧](https://www.kdnuggets.com/2023/03/7-mustknow-python-tips-coding-interviews.html)

+   [使用 Ruff 提升你的 Python 编码风格](https://www.kdnuggets.com/enhance-your-python-coding-style-with-ruff)
