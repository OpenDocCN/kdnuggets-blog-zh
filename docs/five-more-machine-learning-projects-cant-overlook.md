# 5 个你不能再忽视的机器学习项目

> 原文：[https://www.kdnuggets.com/2016/06/five-more-machine-learning-projects-cant-overlook.html](https://www.kdnuggets.com/2016/06/five-more-machine-learning-projects-cant-overlook.html)

上个月的文章 "[5 个你不能再忽视的机器学习项目](/2016/05/five-machine-learning-projects-cant-overlook.html)" 受到了广泛好评，内容涉及 Python 生态系统中的 5 个鲜为人知的机器学习项目，包括深度学习库、辅助支持、数据清理和自动化工具。因此，我们认为有必要做一个后续文章，这次将范围扩展到更多领域。

本文将展示 5 个你可能尚未听说的机器学习项目。然而，这次这些项目将涵盖多个不同的生态系统和编程语言，而不仅仅专注于 Python 工具。即使你对这些特定工具没有需求，检查它们的广泛实现细节或特定代码可能会帮助你产生一些自己的想法。像之前的版本一样，除了那些在网上时间中引起我注意的项目之外，没有正式的选择标准，并且这些项目都有 Github 仓库。这是主观的，确实如此。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

这里是：5 个你应该考虑查看的**机器学习**项目。它们的顺序没有特定要求，但为了方便起见，进行了编号，因为编号本身就是重点。

**1\. [Rusty Machine](https://github.com/AtheMathmo/rusty-machine)**

Rusty Machine 是[Rust](https://www.rust-lang.org/)中的机器学习库。Rust 本身只有约 6 年的历史，开发由 Mozilla 资助。对于不熟悉 Rust 的人，它是一种与 C 和 C++ 相似的系统语言，自我描述为：

> Rust 是一种系统编程语言，运行速度极快，防止段错误，并保证线程安全。

Rusty Machine 正在积极开发，目前支持多种学习技术，包括线性回归、逻辑回归、K-均值聚类、神经网络、支持向量机等。该项目相对较新，目前将交叉验证和数据处理等功能留给用户。该项目还拥有[完善的文档](https://athemathmo.github.io/rusty-machine/rusty-machine/doc/rusty_machine/index.html)。

支持的数据结构，如向量和矩阵，都是内置的。Rusty Machine 提供了每个支持的模型的训练和预测功能，作为模型的通用接口。如果你是一个寻找通用机器学习库的 Rust 用户，[下载 Rusty Machine](https://github.com/AtheMathmo/rusty-machine)并试用一下。

**2\. [scikit-image](https://github.com/scikit-image/scikit-image)**

`scikit-image` 是 SciPy 中的 Python 图像处理库。`scikit-image` 本身是否属于机器学习？好吧，请记住这是一个机器学习项目的列表（没有什么实际上说它们 *必须* 执行机器学习），并回忆一下之前的文章也包括了支持项目，如数据处理和准备工具。`scikit-image` 也属于这个类别。该项目包含了许多图像处理算法，如点检测、滤波器、特征选择和形态学。

![scikit-image](../Images/d05c54ed926699f79f6d0a0d148fc7f5.png)

[这篇来自 y-hat 的文章](http://blog.yhat.com/posts/image-processing-with-scikit-image.html) 对 `scikit-image` 的图像处理进行了很好的概述。该文章还认识到图像处理在机器学习中的重要性：

> 强调重要特征和稀释噪声特征是良好特征设计的核心。在机器视觉的背景下，这意味着图像预处理扮演了重要角色。在从图像中提取特征之前，能够增强图像，使得对机器学习任务重要的方面突出，非常有用。

这是一个使用 `scikit-image` 过滤图像的快速示例：

```py
from skimage import data, io, filters

image = data.coins() # or any NumPy array!
edges = filters.sobel(image)
io.imshow(edges)
io.show()

```

![scikit-image](../Images/b79fa0b87dcccd9327c887541788f579.png)

如果你有兴趣使用 `scikit-image` 进行图像处理任务，我建议参考[项目文档](http://scikit-image.org/docs/stable/)和[y-hat 文章](http://blog.yhat.com/posts/image-processing-with-scikit-image.html)作为好的起点。

**3\. [NLP Compromise](https://github.com/nlp-compromise/nlp_compromise)**

NLP Compromise 是用 Javascript 编写的，可以在浏览器中进行自然语言处理。它拥有一个[完整文档的 API](https://github.com/nlp-compromise/nlp_compromise/wiki)，正在积极开发，并且有一个[正在进行的 wiki](https://github.com/nlp-compromise/nlp_compromise/wiki)，承诺提供一些额外的有用信息。

NLP Compromise 非常容易安装和使用。以下是一组简短的示例：

```py
let nlp = require('nlp_compromise'); // or nlp = window.nlp_compromise

nlp.noun('dinosaur').pluralize();
// 'dinosaurs'

nlp.verb('speak').conjugate();
// { past: 'spoke',
//   infinitive: 'speak',
//   gerund: 'speaking',
//   actor: 'speaker',
//   present: 'speaks',
//   future: 'will speak',
//   perfect: 'have spoken',
//   pluperfect: 'had spoken',
//   future_perfect: 'will have spoken'
// }

nlp.statement('She sells seashells').negate().text()
// "She doesn't sell seashells"

nlp.sentence('I fed the dog').replace('the [Noun]', 'the cat').text()
// 'I fed the cat'

nlp.text('Tony Hawk did a kickflip').people();
// [ Person { text: 'Tony Hawk' ..} ]

nlp.noun('vacuum').article();
// 'a'

nlp.person('Tony Hawk').pronoun();
// 'he'

```

该项目的 GitHub 仓库获得了大量的 stars（接近 6,000），它被 [一些下游项目](https://github.com/nlp-compromise/nlp_compromise/wiki/Downstream-projects) 采用也让人放心。浏览器中的 NLP 可能再也不会变得更简单或更轻量级。

**4\. [Datatest](https://github.com/shawnbrown/datatest)**

这很有趣。Datatest 是基于测试驱动的数据处理，在 Python 中。

来自项目文档：

> Datatest 扩展了标准库的 unittest 包，提供了用于断言数据正确性的测试工具。

Datatest 有 [详细的文档](http://datatest.readthedocs.io/en/0.6.0.dev1/)，也许最好的了解方法是 [查看示例](http://datatest.readthedocs.io/en/0.6.0.dev1/intro.html)。

```py
import datatest

def setUpModule():
    global subjectData
    subjectData = datatest.CsvSource('users.csv')

class TestUserData(datatest.DataTestCase):
    def test_columns(self):
        self.assertDataColumns(required={'user_id', 'active'})

    def test_user_id(self):
        def must_be_digit(x):  # <- Helper function.
            return str(x).isdigit()
        self.assertDataSet('user_id', required=must_be_digit)

    def test_active(self):
        self.assertDataSet('active', required={'Y', 'N'})

if __name__ == '__main__':
    datatest.main()

```

你可以查看 [这里所有可用的断言方法列表](http://datatest.readthedocs.io/en/0.6.0.dev1/api.html#assert-methods)。

Datatest 是一种不同的数据处理和准备方式。不过，由于你可能会花费大量时间在这个任务上，也许值得尝试一下新的方法。

**5\. [GoLearn](https://github.com/sjwhitworth/golearn)**

![GoLearn](../Images/cc1b37990088f9cf33255f8c6c82a2b1.png)

在我们收集的非 Python 机器学习库和/或框架中，GoLearn 是一个通用的机器学习库，适用于 [Go](https://golang.org/)。

这是 GoLearn 对自己的描述：

> GoLearn 是一个“内置电池”的 Go 机器学习库。简洁性与可定制性是我们的目标。我们正在积极开发中，欢迎用户提出意见。

对于考虑拓展的 Python 用户以及希望转向机器学习的 Go 用户来说，有一个好消息：GoLearn 实现了熟悉的 Scikit-learn Fit/Predict 接口，支持快速的估算器测试和切换。它还允许平滑过渡，并使得专注于 Go 的用户可以利用所有的 Scikit-learn 教程材料，而无需重新创建基础的实际机器学习概念指令。

GoLearn 是一个足够成熟的项目，它提供了交叉验证和训练/测试拆分的辅助函数，而相对较新的 Rusty Machine 尚未实现这些功能。你是想在 Go 中进行一些机器学习，还是寻找一个借口来尝试 Go 语言？GoLearn 可能正是你所需要的。

**相关：**

+   [5 个你不能忽视的机器学习项目](/2016/05/five-machine-learning-projects-cant-overlook.html)

+   [Javascript 顶级机器学习库](/2016/06/top-machine-learning-libraries-javascript.html)

+   [10 必备的数据科学技能（更新）](/2016/05/10-must-have-skills-data-scientist.html)

### 更多相关话题

+   [可以帮助你解决实际问题的数据科学项目](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [为什么越来越多的开发者在机器学习项目中使用Python？](https://www.kdnuggets.com/2022/01/developers-python-machine-learning-projects.html)

+   [9个专业证书可以帮助你获得学位……如果……](https://www.kdnuggets.com/9-professional-certificates-that-can-take-you-onto-a-degree-if-you-really-want-to)

+   [如何使用机器学习自动标记数据](https://www.kdnuggets.com/2022/02/machine-learning-automatically-label-data.html)

+   [你不能错过的7种机器学习算法](https://www.kdnuggets.com/7-machine-learning-algorithms-you-cant-miss)

+   [2024年你可以参加的5个顶级机器学习课程](https://www.kdnuggets.com/5-top-machine-learning-courses-you-can-take-in-2024)
