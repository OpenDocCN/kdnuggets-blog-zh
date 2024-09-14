# 数据科学家的编码习惯

> 原文：[https://www.kdnuggets.com/2020/05/coding-habits-data-scientists.html](https://www.kdnuggets.com/2020/05/coding-habits-data-scientists.html)

[评论](#comments)

**作者：[David Tan](https://www.thoughtworks.com/profiles/david-tan)，ThoughtWorks**。

*[最初发布于](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists) ThoughtWorks Insights。经许可转载。*

作为一个ML从业者，你会知道代码很快就会失控。一个出色的ML模型很容易变成一个难以理解的大块代码。因此，修改代码变得痛苦且容易出错，同时ML从业者在演进其ML解决方案时变得越来越困难。

本文分享了一些识别增加代码复杂性的坏习惯的技巧，以及有助于我们分解复杂性的习惯。它现在也是一个**[视频系列](https://www.youtube.com/watch?v=Edn6XxWmtEs&list=PLO9pkowc_99ZhP2yuPU8WCfFNYEx2IkwR&index=2)**，涵盖了以下主题：

+   如何重构Jupyter notebook

+   针对你的ML代码库的自动化测试

+   如何使用IDE提高生产力

+   在17分钟内入门Docker

如果你尝试过机器学习或数据科学，你会知道代码可以迅速变得[混乱](https://github.com/davified/clean-code-ml/blob/master/notebooks/titanic-original.ipynb)。

通常，训练ML模型的代码是用Jupyter notebooks编写的，里面充满了（i）副作用（例如打印语句、格式化的数据框、数据可视化）和（ii）没有任何抽象、模块化和自动化测试的粘合代码。虽然这对于旨在教授机器学习过程的笔记本可能没问题，但在实际项目中，这会变成一个难以维护的混乱。缺乏良好的编码习惯使得代码难以理解，因此，修改代码变得痛苦且容易出错。这使得数据科学家和开发人员在演进其ML解决方案时变得越来越困难。

在本文中，我们将分享识别增加代码复杂性的不良习惯的技巧，以及有助于我们分解复杂性的习惯。

### 复杂性的贡献因素是什么？

> *管理软件复杂性的最重要技巧之一是设计系统，使开发人员在任何给定时间只需要面对总体复杂性的一小部分。 - [John Ousterhout](https://www.amazon.com/Philosophy-Software-Design-John-Ousterhout/dp/1732102201)*

要处理复杂性，我们必须首先了解它的样子。当某物是[由相互关联的部分组成](https://www.etymonline.com/word/complex)时，它就是复杂的。每当我们以添加另一个活动部分的方式编写代码时，我们就增加了复杂性，并且需要在脑海中记住更多的内容。

虽然我们不能——也不应尝试——逃避问题的本质复杂性，但我们经常通过不良实践如：

+   **没有抽象。** 当我们在单个 Python 笔记本或脚本中编写所有代码而没有将其抽象成函数或类时，我们迫使读者阅读大量代码行并弄清楚“如何”来了解代码的功能。

+   **长函数做多个事情。** 这迫使我们在处理函数的一部分时必须在脑海中保持所有中间数据转换。

+   **没有单元测试。** 当我们重构时，确保没有破坏任何东西的唯一方法是重新启动内核并运行整个笔记本。即使我们只想处理其中的一小部分，我们也被迫承担整个代码库的复杂性。

复杂性是不可避免的，但可以进行分隔。在我们的家庭中，当我们没有主动组织和理顺物品的放置位置、原因和方式时，杂乱就会积累，本应简单的任务（例如，找钥匙）变得不必要地耗时和令人沮丧。我们的代码库也是如此。新代码不断被添加用于数据清理、特征工程、错误修复、处理新数据等。除非我们严格维护我们的代码库并不断重构（而且没有单元测试我们无法重构），否则混乱和复杂性是不可避免的。

在本文的其余部分，我们将分享一些增加复杂性的常见不良习惯和帮助管理复杂性的更好习惯：

+   保持代码简洁

+   使用函数来抽象复杂性

+   尽快将代码从 Jupyter 笔记本中提取出来

+   应用测试驱动开发

+   进行小而频繁的提交

### 减少复杂性的习惯

**保持代码简洁**

不洁净的代码通过使代码难以理解和修改来增加复杂性。因此，修改代码以响应业务需求变得越来越困难，有时甚至是不可能的。

一种糟糕的编码习惯（或称为“代码异味”）是死代码。死代码是已经执行但其结果从未在其他计算中使用的代码。死代码是开发人员在编写代码时不得不记住的另一件无关紧要的事。例如，对比这两个代码示例：

```py
# bad example

df = get_data()

print(df)

# do_other_stuff()

# do_some_more_stuff()

df.head()

print(df.columns)

# do_so_much_stuff()

model = train_model(df)

# good example

df    = get_data()

model = train_model(df)

```

关于清洁代码的实践已经在 [几种语言](https://github.com/abiodunjames/Awesome-Clean-Code-Resources)中广泛书写，包括 [Python](https://github.com/zedr/clean-code-python)。我们已经调整了这些“清洁代码”原则，你可以在这个 [clean-code-ml 仓库](https://github.com/davified/clean-code-ml)中找到它们：

+   设计 ([代码示例](https://github.com/davified/clean-code-ml/blob/master/docs/design.md))

    +   不要暴露你的内部实现（保持实现细节隐藏）

+   可有可无的 ([代码示例](https://github.com/davified/clean-code-ml/blob/master/docs/dispensables.md))

    +   删除死代码

    +   避免使用打印语句（即使是像 **head()**、**df.describe()**、**df.plot()** 这样的夸张打印语句）

+   变量（[代码示例](https://github.com/davified/clean-code-ml/blob/master/docs/variables.md)）

    +   变量名应该揭示意图

+   函数（[代码示例](https://github.com/davified/clean-code-ml/blob/master/docs/functions.md)）

    +   使用函数来保持代码的“DRY” (不要重复自己)

    +   函数应该做一件事

**使用函数来抽象复杂性**

函数通过抽象复杂的实现细节并用更简单的表示——它的名称——来简化我们的代码。

想象你在一家餐厅，你得到了一份菜单。这个菜单不是告诉你菜品的名称，而是详细列出了每道菜的食谱。例如，其中一道菜是：

> *第1步。在一个大锅中，加热油。加入胡萝卜、洋葱和芹菜；搅拌至洋葱变软。加入香草和大蒜，再煮几分钟。*
> 
> *第2步。加入扁豆、番茄和水。将汤煮沸后转小火炖煮30分钟。加入菠菜并煮至菠菜变软。最后，用醋、盐和胡椒调味。*

如果菜单隐藏了菜谱中的所有步骤（即实现细节），而只给出了菜品的名称（一个接口，即菜品的抽象），对我们来说会更容易（答案是：那是扁豆汤）。

为了说明这一点，这里是来自Kaggle Titanic比赛的一个笔记本中的代码示例，重构前后对比。

```py
# bad example
pd.qcut(df['Fare'], q=4, retbins=True)[1] # returns array([0., 7.8958, 14.4542, 31.275, 512.3292])

df.loc[ df['Fare'] <= 7.90, 'Fare'] = 0 df.loc[(df['Fare'] > 7.90) & (df['Fare'] <= 14.454), 'Fare'] = 1 df.loc[(df['Fare'] > 14.454) & (df['Fare'] <= 31), 'Fare'] = 2 df.loc[ df['Fare'] > 31, 'Fare'] = 3
df['Fare'] = df['Fare'].astype(int)
df['FareBand'] = df['Fare']

# good example (after refactoring into functions)
df['FareBand'] = categorize_column(df['Fare'], num_bins=4)

```

通过将复杂性抽象为函数，我们得到了什么？

+   **可读性**。我们只需阅读接口（即 `categorize_column()`）即可了解其功能。我们不必阅读每一行代码或在互联网上搜索我们不理解的内容（例如 `pd.qcut`）。如果我还是不理解这个函数的功能，我可以查看它的单元测试或定义。

+   因为现在它是一个函数，我们可以很容易地为其编写单元测试。如果我们不小心更改了它的行为，单元测试会失败，并在毫秒内给我们反馈。

+   为任何列（例如“Age”或“Income”）重复相同的转换，我们只需要一行（而不是七行）代码。

当我们重构为函数时，我们的整个笔记本可以简化并变得更优雅：

*# 糟糕的例子*

[查看笔记本](https://github.com/davified/clean-code-data-science/blob/master/notebooks/titanic-notebook-1.ipynb)

```py
# good example
df = impute_nans(df, categorical_columns=['Embarked'],
                     Continuous_columns =['Fare', 'Age'])
df = add_derived_title(df)
df = encode_title(df)
df = add_is_alone_column(df)
df = add_categorical_columns(df)
X, y = split_features_and_labels(df)

# an even better example. Notice how this reads like a story
prepare_data = compose(impute_nans, 
                       add_derived_title, 
                       encode_title, 
                       add_is_alone_column, 
                       add_categorical_columns,
                       split_features_and_labels)

X, y = prepare_data(df)

```

我们的心理负担现在大大减少了。我们不再被迫处理大量的实现细节来理解整个流程。相反，抽象（即函数）抽象了复杂性，告诉我们它们的功能，免去了我们在弄清楚它们如何实现时所需的心理努力。

**尽快将代码从Jupyter笔记本中剥离出来**

在室内设计中，有一个概念（“[平面表面法则](https://en.wikipedia.org/wiki/Law_of_flat_surfaces)”）指出“家或办公室中的任何平面表面都倾向于积累杂物。” Jupyter 笔记本是机器学习世界的平面表面。

![](../Images/6555c8c503fcbd08787c0828595c5e45.png)

当然，Jupyter 笔记本非常适合快速原型开发。但这也是我们倾向于放入许多东西的地方——粘合代码、打印语句、被夸大的打印语句**（df.describe() **或** df.plot()）**、未使用的导入语句，甚至堆栈跟踪。尽管我们尽了最大努力，但只要笔记本存在，混乱往往会积累。

笔记本很有用，因为它们给我们快速反馈，这通常是我们在面对新数据集和新问题时所需要的。然而，笔记本变得越长，获取我们更改是否有效的反馈就越困难。

相反，如果我们将代码提取到函数和 Python 模块中，并且有单元测试，测试运行器会在几秒钟内对我们的更改进行反馈，即使有数百个函数也是如此。

![](../Images/aab1436e8dea2ef7b6d4dccc99589dae.png)

*图 1: 代码越多，笔记本就越难快速反馈我们是否一切正常。*

因此，我们的目标是尽早将代码从笔记本中迁移到 Python 模块和包中。这样，它们可以在单元测试和领域边界的安全范围内。这将通过提供逻辑组织代码和测试的结构来帮助管理复杂性，并使我们更容易发展我们的机器学习解决方案。

那么，我们如何将代码从 Jupyter 笔记本中移出呢？

假设你已经在 Jupyter 笔记本中有了代码，你可以遵循这个过程：

![](../Images/f182b2d0c197d0d9b1c6b448a2d23c67.png)

*图 2: [如何重构 Jupyter 笔记本](https://github.com/davified/clean-code-ml/blob/master/docs/refactoring-process.md)。*

这个过程每一步的详细信息（例如，如何以监视模式运行测试）可以在 [clean-code-ml 仓库](https://github.com/davified/clean-code-ml/blob/master/docs/refactoring-exercise.md)中找到。

**应用测试驱动开发**

到目前为止，我们已经讨论了在代码已经写在笔记本之后编写测试。这种推荐并不是理想的，但总比没有单元测试要好得多。

有一个误解认为我们不能将测试驱动开发（[TDD](https://www.thoughtworks.com/insights/blog/test-driven-development-best-thing-has-happened-software-design)）应用于机器学习项目。对我们来说，这显然是不正确的。在任何机器学习项目中，大部分代码涉及数据转换（例如数据清洗、特征工程），而小部分代码才是实际的机器学习。这样的数据转换可以写成纯函数，对于相同的输入返回相同的输出，因此我们可以应用TDD并获得其好处。例如，TDD可以帮助我们将大型复杂的数据转换拆解为可以逐个处理的小问题。

至于测试代码的实际机器学习部分是否按预期工作，我们可以编写功能测试以验证模型的指标（如准确性、精确度等）是否高于我们预期的阈值。换句话说，这些测试验证模型是否按照我们的预期运行（因此得名功能测试）。以下是一个[示例](https://github.com/davified/clean-code-ml/blob/master/src/tests/test_model_metrics.py)：

```py
import unittest 
from sklearn.metrics import precision_score, recall_score

from src.train import prepare_data_and_train_model

class TestModelMetrics(unittest.TestCase):
    def test_model_precision_score_should_be_above_threshold(self):
        model, X_test, Y_test = prepare_data_and_train_model()
        Y_pred = model.predict(X_test)

        precision = precision_score(Y_test, Y_pred)

        self.assertGreaterEqual(precision, 0.7) 

```

**进行小而频繁的提交**

当我们不进行小而频繁的提交时，我们会增加心理负担。当我们在处理当前问题时，早期的更改仍显示为未提交，这在视觉上和潜意识中都造成了干扰，使我们更难以专注于当前问题。

例如，请看下面的第一张和第二张图片。你能找出我们正在处理哪个函数吗？哪张图片让你更容易理解？

![](../Images/5876a792c417d3bf346971b0694a3176.png)

![](../Images/2d0e582607aa4da59dc8d5eb21d22d8e.png)

当我们进行小而频繁的提交时，我们会获得以下好处：

+   减少视觉干扰和认知负担。

+   如果代码已经被提交，我们不必担心意外地破坏工作中的代码。

+   除了[红-绿-重构](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)方法，我们还可以使用[红-红-红-回退](https://www.facebook.com/notes/kent-beck/one-bite-at-a-time-partitioning-complexity/1716882961677894/)方法。如果我们不小心破坏了什么，我们可以轻松回退到最新提交，然后重试。这可以避免我们在解决关键问题时，浪费时间撤销意外创建的问题。

那么，提交的大小要小到什么程度才算足够小呢？当存在一组逻辑相关的更改并且测试通过时，尝试进行提交。一种技巧是留意提交信息中的“and”一词，例如，“添加探索性数据分析并将句子拆分为标记并重构模型训练代码。”这三项更改可以拆分成三次逻辑提交。在这种情况下，你可以使用**[git add --patch](https://nuclearsquid.com/writings/git-add/)**将代码分批暂存以便提交。

### 结论

> *“我不是一个伟大的程序员；我只是一个有着良好习惯的好程序员。” - Kent Beck，极限编程和xUnit测试框架的先驱*

这些习惯帮助我们管理机器学习和数据科学项目中的复杂性。我们希望这些习惯也能帮助你在数据项目中变得更加灵活和高效。

**简介：** [David](https://www.thoughtworks.com/profiles/david-tan)在ThoughtWorks工作了2年，在决定转行做软件工程之前，他在政府部门从事非技术性工作。在过去两年中，他参与了多个机器学习的副项目，包括股票市场价格预测、欺诈保护以及啤酒数量图像识别。他还是ThoughtWorks JumpStart!计划的培训师。David对敏捷软件开发和知识共享充满热情。在空闲时间，他喜欢和家人一起度过时光，作为一个新晋爸爸。

**相关：**

+   [你今天应该学习的10个Python技巧和窍门](https://www.kdnuggets.com/2020/01/10-python-tips-tricks-learn-today.html)

+   [数据科学家常犯的10个编程错误](https://www.kdnuggets.com/2019/04/top-10-coding-mistakes-data-scientists.html)

+   [你的机器学习代码可能很糟糕的4个原因](https://www.kdnuggets.com/2019/02/4-reasons-machine-learning-code-probably-bad.html)

* * *

## 我们的前三推荐课程

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT

* * *

### 更多相关内容

+   [KDnuggets 新闻，5月4日：9个免费的哈佛课程来学习数据…](https://www.kdnuggets.com/2022/n18.html)

+   [停止在数据科学项目中硬编码 - 改用配置文件](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

+   [如何回答数据科学编程面试问题](https://www.kdnuggets.com/2022/01/answer-data-science-coding-interview-questions.html)

+   [15 道你必须知道的数据科学 Python 编程面试题](https://www.kdnuggets.com/2022/04/15-python-coding-interview-questions-must-know-data-science.html)

+   [3 个数据科学领域的困难 Python 编程面试题](https://www.kdnuggets.com/2023/03/3-hard-python-coding-interview-questions-data-science.html)

+   [5 门免费大学课程学习数据科学编码](https://www.kdnuggets.com/5-free-university-courses-to-learn-coding-for-data-science)
