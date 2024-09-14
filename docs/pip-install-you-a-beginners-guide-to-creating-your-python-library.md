# Pip Install YOU: 创建 Python 库的初学者指南

> 原文：[`www.kdnuggets.com/pip-install-you-a-beginners-guide-to-creating-your-python-library`](https://www.kdnuggets.com/pip-install-you-a-beginners-guide-to-creating-your-python-library)

![创建 Python 库的初学者指南](img/17334bd5288d4384279ba35d611305ed.png)

图片由作者 | Canva

作为程序员，我们经常依赖各种外部库来解决不同的问题。这些库由技术娴熟的开发者创建，提供节省时间和精力的解决方案。但你是否想过，*“我也可以创建自己的库吗？”* 答案是肯定的！本文解释了完成这一目标的必要步骤，无论你是专业开发者还是刚入门。从编写和结构化代码到文档和发布，本指南涵盖了所有内容。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

## 创建库的逐步指南

#### 第 1 步：初始化你的项目

首先为你的项目创建一个根目录。

```py
multiples_library/
```

#### 第 2 步：为你的包创建目录

下一步是在你的项目目录中创建一个包目录。

```py
multiples_library/
└──multiples/
```

#### 第 3 步：添加 `__init.py__`

现在，在你的包目录中添加 `__init.py__` 文件。这个文件是 Python 识别其所在目录是一个包的主要标志。它包含初始化代码（如果有的话），并在包或其任何模块被导入时自动执行。

```py
multiples_library/
└── multiples/
    └──__init__.py
```

#### 第 4 步：添加模块

现在，你需要将模块添加到包的目录中。这些模块通常包括类和函数。给每个模块一个描述其目的的有意义名称是一种良好的实践。

```py
multiples_library/
│
└── multiples/
    ├── __init__.py
    ├── is_multiple_of_two.py
    └── is_multiple_of_five.py
```

#### 第 5 步：编写模块

在这一步，你需要定义每个模块的功能。例如，在我的案例中：

**模块：multiple_of_two.py**

```py
def is_multiple_of_two(number):
    """ Check if a number is a multiple of two. """
    return number % 2 == 0
```

**模块：multiple_of_five.py**

```py
def is_multiple_of_five(number):
    """ Check if a number is a multiple of five. """
    return number % 5 == 0
```

#### 第 6 步：添加 setup.py

下一步是向你的包目录中添加另一个名为 setup.py 的文件。

```py
multiples_library/
│
├── multiples/
│   ├── __init__.py
│   ├── is_multiple_of_two.py
│   └── is_multiple_of_five.py
│
└──setup.py
```

该文件包含有关你的包的元数据，如名称、依赖项、作者、版本、描述等。它还定义了要包含的模块，并提供了构建和安装包的说明。

```py
from setuptools import setup, find_packages

setup(
    name='multiples_library',  # Replace with your package’s name
    version='0.1.0',
    packages=find_packages(),
    install_requires=[
        # List your dependencies here
    ],
    author='Your name',  
    author_email='Your e-mail',
    description='A library for checking multiples of 2 and 5.',
    classifiers=[
        'Programming Language :: Python :: 3',
        'License :: OSI Approved :: MIT License',  # License type
        'Operating System :: OS Independent',
    ],
    python_requires='>=3.6',

)
```

#### 第 7 步：添加测试及其他文件 [可选]

这一步不是必需的，但如果你想构建一个没有错误且专业的库，这是一种良好的实践。在这一步，项目结构是最终的，看起来有些像这样：

```py
multiples_library/
│
├── multiples/
│   ├── __init__.py
│   ├── is_multiple_of_two.py
│   └── is_multiple_of_five.py
│
│
├── tests/ 
│   ├── __init__.py   
│   ├── test_is_multiple_of_two.py
│   └── test_is_multiple_of_five.py
│
├── docs/
│
├── LICENSE.txt
├── CHANGES.txt
├── README.md
├── setup.py
└── requirements.txt
```

现在我将向你解释根目录中提到的可选文件和文件夹的目的：

+   **tests/:** 包含你的库的测试用例，以确保它按预期运行。

+   **docs/:** 包含你的库的文档。

+   **LICENSE.txt：**包含其他人使用你代码的许可条款。

+   **CHANGES.txt：**记录对库的更改。

+   **README.md：**包含你的包的描述和安装说明。

+   requirements.txt：列出了你的库所需的外部依赖项，你可以通过单个命令 (`pip install -r requirements.txt`) 安装这些包。

这些描述相当直接，你很快就能理解可选文件和文件夹的目的。不过，我想稍微讨论一下可选的测试目录，以澄清其用法。

**tests/ 目录**

重要的是要注意，你可以在根目录中添加一个测试目录，即 `\multiples_library`，或者在包的目录中，即 `\multiples`。选择权在你，但我喜欢把它放在根目录的顶层，因为我认为这样更好地模块化你的代码。

有几个库可以帮助你编写测试用例。我将使用最著名的且我个人最喜欢的“unittest”。

**is_multiple_of_two 的单元测试**

该模块的测试用例包含在 `test_is_multiple_of_two.py` 文件中。

```py
import unittest
import sys
import os

sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))

from multiples.is_multiple_of_two import is_multiple_of_two

class TestIsMultipleOfTwo(unittest.TestCase):

	def test_is_multiple_of_two(self):
		self.assertTrue(is_multiple_of_two(4))
if __name__ == '__main__': 
      unittest.main() 
```

**is_multiple_of_five 的单元测试**

该模块的测试用例包含在 `test_is_multiple_of_five.py` 文件中。

```py
import unittest
import sys
import os
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))

from multiples.is_multiple_of_five import is_multiple_of_five

class TestIsMultipleOfFive(unittest.TestCase):

	def test_is_multiple_of_five(self):
		self.assertTrue(is_multiple_of_five(75)) 

if __name__ == '__main__':
      unittest.main()
```

上述单元测试相当直接，但我将解释两个函数以进一步澄清。

+   `self.assertTrue(expression)` 检查表达式是否计算为“True”。只有当表达式的结果是“True”时，测试才会通过。

+   调用 `unittest.main()` 函数以运行文件中定义的所有测试用例。

#### 步骤 8：使用 PyPI 分发你的包

为了使你的库对他人更易访问，你可以将它上传到 PyPI。请按照以下步骤分发你的包：

+   在 [PyPI](https://pypi.org/) 上创建一个帐户并启用双重身份验证。

+   通过提供一个令牌名称并将范围选择为“整个帐户”来创建一个 [API 令牌](https://pypi.org/manage/account/token/)。然后小心地复制它，因为它只会出现一次。

+   现在，你需要创建一个 .pypirc 文件。

    对于**MacOS/Linux**，打开终端并运行以下命令：

```py
cd ~
touch .pypirc
```

对于**Windows**，打开命令提示符并运行以下命令：

```py
cd %USERPROFILE%
type NUL > .pypirc
```

在 MacOS/Linux 的情况下，该文件被创建并位于 ~/.pypirc，而在 Windows 的情况下则位于 %USERPROFILE%/.pypirc。

+   通过复制并粘贴以下配置来编辑 **.pypirc** 文件：

```py
[distutils]
index-servers =
    pypi

[pypi]
username = __token__
password = pypi-<your-api-token></your-api-token>
```

将**<your-api-token></your-api-token>**替换为你从 PyPI 生成的实际 API 令牌。不要忘记包括 pypi-前缀。

+   确保你的项目根目录中有一个 setup.py 文件。运行以下命令以创建分发文件：

```py
python3 setup.py sdist bdist_wheel 
```

+   Twine 是一个用于将包上传到 PyPI 的工具。通过运行以下命令来安装 twine：

```py
pip install twine
```

+   现在通过运行以下命令将你的包上传到 PyPI：

```py
twine upload dist/*
```

#### 第 9 步：安装并使用库

你可以通过以下命令安装该库：

```py
pip install [your-package]
```

在我的情况下：

```py
pip install multiples_library
```

现在，你可以如下使用该库：

```py
from multiples.is_multiple_of_five import is_multiple_of_five
from multiples.is_multiple_of_two import is_multiple_of_two

print(is_multiple_of_five(10))
**# Outputs True**
print(is_multiple_of_two(11))
**# Outputs False**
```

## 总结

总之，创建一个 Python 库非常有趣，而将其分发则使其他人能够使用它。我尽量将创建 Python 库所需的内容解释得尽可能清晰。然而，如果你在任何环节遇到困难或困惑，请随时在评论区提问。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学以及人工智能与医学的交汇处充满热情。她共同编写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年亚太地区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 技术多样性奖学金获得者、Mitacs Globalink Research 奖学金获得者以及哈佛 WeCode 奖学金获得者。Kanwal 是变革的坚定倡导者，创办了 FEMCodes，以赋能女性在 STEM 领域的发展。

### 更多相关主题

+   [使用 Poetry 与 Conda 和 Pip 管理 Python 依赖关系](https://www.kdnuggets.com/managing-python-dependencies-with-poetry-vs-conda-pip)

+   [忘记 PIP、Conda 和 requirements.txt！改用 Poetry，之后会感激的…](https://www.kdnuggets.com/2023/07/forget-pip-conda-requirementstxt-poetry-instead-thank-later.html)

+   [像专家一样测试：Python Mock 库的逐步指南](https://www.kdnuggets.com/testing-like-a-pro-a-step-by-step-guide-to-pythons-mock-library)

+   [使用 Python 创建一个从音频中提取主题的 Web 应用程序](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [为机器学习创建特征的挑战](https://www.kdnuggets.com/2022/02/challenges-creating-features-machine-learning.html)

+   [赢得房间：创建和传递有效的数据驱动演示文稿…](https://www.kdnuggets.com/2022/04/franks-winning-room-creating-delivering-effective-data-driven-presentation.html)
