# 使用 Faker 在 Python 中生成简单的合成数据

> 原文：[`www.kdnuggets.com/2021/11/easy-synthetic-data-python-faker.html`](https://www.kdnuggets.com/2021/11/easy-synthetic-data-python-faker.html)

评论![图示](img/9948a4e44a27c8a43b26295628cd8b5e.png)

图片来自[geralt on Pixabay](https://pixabay.com/users/geralt-9301/)

真实的数据，来自真实世界，是数据科学的黄金标准，这一点可能很明显。当然，诀窍在于能够找到所需的真实世界数据。有时候，你会很幸运地发现所需的数据触手可及，格式符合要求，而且数量正好。

* * *

## 我们的三大推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

然而，现实世界的数据通常不足以满足项目需求。在这种情况下，可以使用合成数据代替真实数据或用来增强不足够大的数据集。人工制造数据的方法有很多，其中一些远比其他方法复杂。然而，也有一些相对简单的选项，例如，**[Faker](https://github.com/joke2k/faker)** 就是 Python 中的一个解决方案。

来自 Faker 的[文档](https://faker.readthedocs.io/en/master/)：

> *Faker* 是一个生成假数据的 Python 包。无论你是需要为数据库进行引导，创建美观的 XML 文档，填充你的持久化存储以进行压力测试，还是匿名化从生产服务中获得的数据，Faker 都能满足你的需求。

可以使用 pip 安装 Faker：

```py
pip install faker
```

导入并实例化一个 Faker 实例的方式如下：

```py
from faker import Faker
fake = Faker()
```

### Faker 的基本使用

Faker 的使用非常简单。让我们先看几个示例。

对于我们的第一个技巧，让我们生成一个假名字。

```py
print(fake.name())
```

```py
Deborah Brooks
```

其他数据类型怎么样？

```py
# generate an address
print(fake.address())

# generate a phone number
print(fake.phone_number())

# generate a date
print(fake.date())

# generate a social security number
print(fake.ssn())

# generate some text
print(fake.text())
```

```py
123 Danielle Forges Suite 506
Stevenborough, RI 36008

968-491-2711

1974-05-27

651-27-9994

Weight where while method. Rock final environmental gas provide. Remember continue sure. Create resource determine fine. Else red across participant. People must interesting spend some us.
```

### Faker 优化

如果你对特定地区的数据感兴趣呢？例如，如果我想生成一种在墨西哥会出现的西班牙名字怎么办？

```py
fake = Faker(['es_MX'])
for i in range(10):
    print(fake.name())

```

```py
Diana Lovato
Ing. Ariadna Palacios
Salvador de la Fuente
Margarita Naranjo
Alvaro Prado Melgar
Tomás Menchaca
Conchita Francisca Velázquez Zedillo
Ivonne Ana Luisa Bueno
Ramiro Raquel Vélez Urbina
Porfirio Esther Irizarry Varela
```

生成的输出可以通过使用 Faker 的权重设置进一步优化：

> Faker 构造函数接受一个与性能相关的参数叫做`use_weighting`。它指定是否尝试使值的频率匹配真实世界的频率（例如，英文名字 Gary 的出现频率会远高于名字 Lorimer）。如果`use_weighting`为`False`，则所有项目被选择的机会相等，选择过程也更快。默认值是`True`。

让我们看看一些美国英语的示例是如何工作的：

```py
fake = Faker(['en_US'], use_weighting=True)
for i in range(20):
    print(fake.name())
```

```py
Mary Mckinney
Margaret Small
Dominic Carter
Elizabeth Gibson
Kelsey Garcia
Chelsea Bradford
Robert West
Timothy Howe
Gary Turner
Cynthia Strong
Joshua Henry
Amanda Jenkins
Jacqueline Daniels
Catherine Jones
Desiree Hodge
Shannon Mason DVM
Marcia West
Dustin Parrish
Christopher Rodriguez
Brett Webb
```

将此与*未*使用加权的类似输出进行比较：

```py
fake = Faker(['en_US'], use_weighting=False)
for i in range(20):
    print(fake.name())
```

```py
Mr. Benjamin Horton
Miss Maria Hardin
Tina Good DDS
Dr. Terry Barr MD
Meredith Mason
Roberta Velasquez
Mr. Tim Woods V
Marilyn Conway
Mr. Dwayne Leblanc III
Dr. Dan Krause IV
Mia Newman DVM
Thomas Small
Joseph Holmes
Dr. Tanner Zhang
Alan Dixon
Miss Rebecca Davila DVM
Joseph Becker MD
Dr. Erin Pugh PhD
Mr. Ernest Juarez
Ross Thompson
```

注意，例如，在生成的“样本”中，医生的数量不成比例。

### 更全面的示例

假设我们要为一家国际公司生成一些虚假的客户数据记录，我们希望模拟现实世界中名字的分布，并将这些数据保存到 CSV 文件中，以便后续在某种数据科学任务中使用。为了清理数据，我们还将把生成的地址中的换行符替换为逗号，并完全移除生成文本中的换行符。

这是一个代码片段，可以实现这一点，展示了 Faker 在几行代码中的强大功能。

```py
from faker import Faker
import pandas as pd

fake = Faker(['en_US', 'en_UK', 'it_IT', 'de_DE', 'fr_FR'], use_weighting=True)

customers = {}

for i in range(0, 10000):
    customers[i]={}
    customers[i]['id'] = i+1
    customers[i]['name'] = fake.name()
    customers[i]['address'] = fake.address().replace('\n', ', ')
    customers[i]['phone_number'] = fake.phone_number()
    customers[i]['dob'] = fake.date()
    customers[i]['note'] = fake.text().replace('\n', ' ')

df = pd.DataFrame(customers).T
print(df)

df.to_csv('customer_data.csv', index=False)
```

```py
         id                   name  ...         dob                                               note
0         1        Burkhardt Junck  ...  1982-08-11  Across important glass stop. Score include rel...
1         2          Ilja Weihmann  ...  1975-03-24  Iusto velit aspernatur nemo. Aliquid ipsum ita...
2         3          Agnolo Tafuri  ...  1990-10-03  Aspernatur fugit voluptatibus. Cumque accusant...
3         4   Sig. Lamberto Cutuli  ...  1973-01-15  Maiores temporibus beatae. Ipsam non autem ist...
4         5          Marcus Turner  ...  2005-12-17  Témoin âge élever loi.\nFatiguer auteur autori...
...     ...                    ...  ...         ...                                                ...
9995   9996  Miss Alexandra Waters  ...  1985-01-20  Commodi omnis assumenda sit ratione non. Commo...
9996   9997         Natasha Harris  ...  2003-10-26  Voluptatibus dolore a aspernatur facere. Aliqu...
9997   9998           Adrien Marin  ...  1983-05-29  Et unde iure. Reiciendis doloribus dignissimos...
9998   9999        Nermin Heydrich  ...  2005-03-29  Plan moitié charge note convenir.\nSang précip...
9999  10000           Samuel Allen  ...  2011-09-29  Total gun economy adult as nor. Age late gas p...

[10000 rows x 6 columns]
```

我们还有一个包含所有数据的 customer_data.csv 文件，供进一步处理和使用。

![图示](https://i.ibb.co/Fhf8dZ1/customer-data-csv.jpg)

生成的客户数据 CSV 文件的截图

上面提到的特定类型数据生成器——如姓名、地址、电话等——称为提供者。通过启用 Faker 生成专门类型的数据，学习如何扩展其功能，使用[标准提供者](https://faker.readthedocs.io/en/stable/providers.html)和[社区提供者](https://faker.readthedocs.io/en/stable/communityproviders.html)。

查看 Faker 的[GitHub 仓库](https://github.com/joke2k/faker)和[文档](https://faker.readthedocs.io/en/stable/)，了解更多功能并立即创建你自己的数据集。

**相关内容**：

+   在 Python 中创建带有异常签名的合成时间序列

+   教 AI 用合成数据分类时间序列模式

+   3 个数据获取、注释和增强工具

### 更多相关内容

+   [高保真合成数据：适用于数据工程师和数据科学家](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [如何利用合成数据克服机器学习模型训练中的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [如何用 AI 生成的合成数据使 AI/ML 和数据科学民主化](https://www.kdnuggets.com/2022/11/mostly-ai-democratize-aiml-data-science-aigenerated-synthetic-data.html)

+   [合成数据平台：释放生成式 AI 的力量…](https://www.kdnuggets.com/2023/07/synthetic-data-platforms-unlocking-power-generative-ai-structured-data.html)

+   [数据访问在大多数公司中严重不足，71%的人认为…](https://www.kdnuggets.com/2023/07/mostly-data-access-severely-lacking-synthetic-data-help.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)
