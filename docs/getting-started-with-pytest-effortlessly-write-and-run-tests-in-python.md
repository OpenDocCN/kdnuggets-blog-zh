# 使用 PyTest 入门：轻松编写和运行 Python 测试

> 原文：[https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

![使用 PyTest 入门](../Images/e14abc30eccb9a742052e136c5e1a866.png)

图片来源于作者

你是否遇到过软件无法按预期工作的情况？也许你点击了一个按钮，却没有任何反应，或者你期待的某个功能结果却存在缺陷或不完整。这些问题对用户来说可能会很令人沮丧，甚至可能导致企业的经济损失。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

为了解决这些挑战，开发人员采用一种称为**测试驱动开发**（TDD）的编程方法。TDD 旨在最小化软件故障，确保软件符合预期要求。这些测试用例描述了代码的预期行为。通过提前编写这些测试，开发人员可以清楚地了解他们想要实现的目标。测试管道是任何组织软件开发过程中的重要组成部分。每当我们对代码库进行更改时，我们需要确保这些更改不会引入新的错误。这就是测试管道发挥作用的地方。

现在，让我们谈谈 PyTest。PyTest 是一个 Python 包，简化了编写和运行测试用例的过程。这个功能齐全的测试工具已经成熟，成为许多组织的事实标准，因为它能够轻松地扩展到复杂的代码库和功能。

## PyTest 模块的好处

+   **改进的日志记录和测试报告**

    在执行测试后，我们会收到所有执行的测试及每个测试用例状态的完整日志。如果出现失败，将提供每个失败的完整堆栈跟踪，以及导致断言语句失败的确切值。这对调试非常有帮助，使我们更容易追踪代码中的具体问题，以解决错误。

+   **测试用例的自动发现**

    我们不需要手动配置任何测试用例。所有文件都被递归扫描，所有以“test”开头的函数名称都会被自动执行。

+   **夹具和参数化**

    在测试用例中，特定的需求可能并不总是可以访问。例如，从网络获取资源进行测试效率低下，并且在运行测试用例时可能没有互联网访问。在这种情况下，如果我们想执行一个需要互联网请求的测试，我们需要添加存根以创建该特定部分的虚拟响应。此外，可能需要使用不同的参数多次执行函数，以覆盖所有可能的边界情况。PyTest 使用 fixtures 和参数化装饰器使实现变得简单。

## 安装

PyTest 作为 PyPI 包提供，可以通过 Python 包管理器轻松安装。为了设置 PyTest，最好从一个新的环境开始。要创建新的 Python 虚拟环境，请使用以下命令：

```py
python3 -m venv venv
source venv/bin/activate 
```

要设置 PyTest 模块，可以使用 pip 安装官方 PyPI 包：

```py
pip install pytest
```

## 运行你的第一个测试用例

让我们深入探讨如何使用 PyTest 编写和运行你的第一个测试用例。我们将从零开始，构建一个简单的测试，以便了解其工作原理。

### 结构化一个 Python 项目

在开始编写测试之前，必须妥善组织我们的项目。这有助于保持整洁和可管理，尤其是当项目扩大时。我们将遵循将应用程序代码与测试代码分开的常见做法。

下面是我们将如何结构化我们的项目：

```py
pytest_demo/
│
├── src/
│   ├── __init__.py
│   ├── sorting.py
│
├── tests/
│   ├── __init__.py
│   ├── test_sorting.py
│
├── venv/
```

我们的根目录 *pytest_demo* 包含单独的 src 和 tests 目录。我们的应用程序代码位于 src，而测试代码则位于 tests。

### 编写一个简单的程序及其关联的测试用例

现在，让我们使用冒泡排序算法创建一个基本的排序程序。我们将其放置在 src/sorting.py 中：

```py
# src/sorting.py

def bubble_sort(arr):
    for n in range(len(arr)-1, 0, -1):
        for i in range(n):
            if arr[i] > arr[i + 1]:
                arr[i], arr[i + 1] = arr[i + 1], arr[i]

	return arr
```

我们实现了一个基本的冒泡排序算法，这是一种简单而有效的方式，通过重复交换相邻元素（如果它们的顺序错误）来对列表中的元素进行排序。

现在，让我们通过编写全面的测试用例来确保我们的实现有效。

```py
# tests/test_sorting.py

import pytest
from src.sorting import bubble_sort

def test_always_passes():
	assert True

def test_always_fails():
	assert False

def test_sorting():
	assert bubble_sort([2,3,1,6,4,5,9,8,7]) == [1,2,3,4,5,6,7,8,9]
```

在我们的测试文件中，我们编写了三个不同的测试用例。注意每个函数名称都以 *test* 前缀开头，这是 PyTest 用来识别测试函数的规则。

我们从源代码中导入冒泡排序的实现到测试文件中。这现在可以在我们的测试用例中使用。每个测试必须有一个 *"assert"* 语句来检查是否按预期工作。我们给排序函数一个未排序的列表，并将其输出与预期进行比较。如果匹配，测试通过；否则，测试失败。

此外，我们还包括了两个简单的测试，一个始终通过，另一个始终失败。这些只是占位函数，对于检查我们的测试设置是否正常工作很有用。

### 执行测试并理解输出

我们现在可以从命令行运行测试。导航到你的项目根目录并运行：

```py
pytest tests/
```

这将递归地搜索`tests`目录中的所有文件。所有以 `test` 前缀开头的函数和类将被自动识别为测试用例。从我们的 `tests` 目录中，它将搜索 `test_sorting.py` 文件并运行所有三个测试函数。

运行测试后，你会看到类似于以下的输出：

```py
===================================================================    
test session starts ====================================================================
platform darwin -- Python 3.11.4, pytest-8.1.1, pluggy-1.5.0
rootdir: /pytest_demo/
collected 3 items                                                                                                                                     	 

tests/test_sorting.py .F.                                                                                                                     [100%]

========================================================================= FAILURES
=========================================================================
____________________________________________________________________ test_always_fails _____________________________________________________________________

	def test_always_fails():
>   	assert False
E   	assert False

tests/test_sorting.py:22: AssertionError
=================================================================      short test summary info ==================================================================
FAILED tests/test_sorting.py::test_always_fails - assert False
===============================================================         
1 failed, 2 passed in 0.02s ================================================================
```

在运行 PyTest 命令行工具时，它会显示平台元数据和将要运行的测试用例总数。在我们的例子中，从 `test_sorting.py` 文件中添加了三个测试用例。测试用例是顺序执行的。一个点（"."）表示测试用例通过，而 "F" 表示测试用例失败。

如果一个测试用例失败，PyTest 会提供一个回溯，显示导致错误的特定代码行和参数。所有测试用例执行完毕后，PyTest 会提供最终报告。该报告包括总执行时间以及通过和失败的测试用例数量。这个总结让你清晰地了解测试结果。

## 多测试用例的函数参数化

在我们的例子中，我们仅测试了排序算法的一个场景。这是否足够？显然不！我们需要用多个示例和边界情况来测试函数，以确保代码中没有错误。

PyTest 使这个过程变得简单。我们使用 PyTest 提供的参数化装饰器为一个函数添加多个测试用例。代码如下：

```py
@pytest.mark.parametrize(
	"input_list, expected_output",
	[
    	    ([], []),
    	    ([1], [1]),
    	    ([53,351,23,12], [12,23,53,351]),
    	    ([-4,-6,1,0,-2], [-6,-4,-2,0,1])
	]
)
def test_sorting(input_list, expected_output):
	assert bubble_sort(input_list) == expected_output
```

在更新的代码中，我们使用了*pytest.mark.parametrize*装饰器来修改了`test_sorting`函数。这个装饰器允许我们将多个输入值集传递给测试函数。该装饰器需要两个参数：一个字符串，表示函数参数的逗号分隔名称，以及一个包含多个元组的列表，其中每个元组包含特定测试用例的输入值。

注意，函数参数的名称与传递给装饰器的字符串名称相同。这是确保输入值正确映射的严格要求。如果名称不匹配，在测试用例收集期间将会引发错误。

使用这个实现，`test_sorting` 函数将被执行四次，每次针对装饰器中指定的一个输入值集。现在，让我们来看一下测试用例的输出：

```py
===================================================================
 test session starts 
====================================================================
platform darwin -- Python 3.11.4, pytest-8.1.1, pluggy-1.5.0
rootdir: /pytest_demo
collected 6 items                                                                                                                                     	 

tests/test_sorting.py .F....                                                                                                                     	[100%]

=======================================================================
FAILURES ========================================================================
____________________________________________________________________ test_always_fails _____________________________________________________________________

	def test_always_fails():
>   	assert False
E   	assert False

tests/test_sorting.py:11: AssertionError
================================================================= 
short test summary info ==================================================================
FAILED tests/test_sorting.py::test_always_fails - assert False
=============================================================== 
1 failed, 5 passed in 0.03s ================================================================
```

在这次运行中，总共执行了六个测试用例，包括来自 `test_sorting` 函数的四个测试用例和两个虚拟函数。如预期的那样，只有虚拟测试用例失败了。

我们现在可以自信地说我们的排序实现是正确的 :)

## 有趣的练习任务

在本文中，我们介绍了 PyTest 模块，并通过测试一个带有多个测试用例的冒泡排序实现来演示其用法。我们覆盖了使用命令行工具编写和执行测试用例的基本功能。这应该足够让你开始为自己的代码库实现测试。为了更好地理解 PyTest，给你一个有趣的练习任务：

实现一个名为**validate_password**的函数，该函数接受一个密码作为输入，并检查它是否满足以下标准：

+   包含至少 8 个字符

+   包含至少一个大写字母

+   包含至少一个小写字母

+   包含至少一个数字

+   包含至少一个特殊字符（例如 !、@、#、$、%）

编写 PyTest 测试用例以验证你的实现的正确性，涵盖各种边界情况。祝好运！

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一名机器学习工程师和技术作家，对数据科学以及人工智能与医学的交汇处有着深厚的热情。她合著了电子书《用 ChatGPT 最大化生产力》。作为 2022 年 APAC 区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认定为 Teradata 多样性技术学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的坚定倡导者，创立了 FEMCodes，以赋能 STEM 领域的女性。

### 更多相关主题

+   [如何让 Python 代码运行得非常快](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)

+   [使用 Jupysql 和 GitHub Actions 计划与运行 ETL](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

+   [通过 llamafile 以 5 个简单步骤分发和运行 LLMs](https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps)

+   [在你的设备上仅需几个步骤即可运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [使用 LM Studio 在本地运行 LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)

+   [Python 生成器入门](https://www.kdnuggets.com/2023/02/getting-started-python-generators.html)
