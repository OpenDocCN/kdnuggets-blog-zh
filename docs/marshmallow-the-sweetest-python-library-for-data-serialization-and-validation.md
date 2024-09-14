# Marshmallow: 最甜美的 Python 数据序列化与验证库

> 原文：[https://www.kdnuggets.com/marshmallow-the-sweetest-python-library-for-data-serialization-and-validation](https://www.kdnuggets.com/marshmallow-the-sweetest-python-library-for-data-serialization-and-validation)

![Marshmallow: 最甜美的 Python 数据序列化与验证库](../Images/b6ac43e0310a98ea737b8b8744447f8b.png)

图片由作者提供 | Leonardo AI & Canva

数据序列化是一个基础的编程概念，在日常程序中具有很大价值。它指的是将复杂的数据对象转换为中间格式，以便保存并可以轻松地转换回原始形式。然而，像 JSON 和 pickle 这样的常见数据序列化 Python 库功能非常有限。随着结构化程序和面向对象编程的发展，我们需要更强的支持来处理数据类。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

Marshmallow 是最著名的数据处理库之一，广泛被 Python 开发者用于开发健壮的软件应用程序。它支持数据序列化，并提供了在面向对象范式中处理数据验证的强大抽象解决方案。

在本文中，我们将通过下面给出的示例来了解如何在现有项目中使用 Marshmallow。代码展示了三个表示简单电子商务模型的类：`Product`、`Customer` 和 `Order`。每个类都最小地定义了它的参数。我们将看到如何保存对象的实例并确保在代码中重新加载时其正确性。

```py
from typing import List

class Product:
    def __init__(self, _id: int, name: str, price: float):
    	self._id = _id
    	self.name = name
    	self.price = price

class Customer:
    def __init__(self, _id: int, name: str):
    	self._id = _id
    	self.name = name

class Order:
    def __init__(self, _id: int, customer: Customer, products: List[Product]):
    	self._id = _id
    	self.customer = customer
    	self.products = products
```

## 开始使用 Marshmallow

#### 安装

Marshmallow 作为一个 Python 库可以在 PyPI 上找到，并且可以通过 pip 轻松安装。要安装或升级 Marshmallow 依赖项，请运行以下命令：

```py
pip install -U marshmallow
```

这会在活动环境中安装最新稳定版本的 Marshmallow。如果你想要包含所有最新功能的开发版本，可以使用下面的命令安装：

```py
pip install -U git+https://github.com/marshmallow-code/marshmallow.git@dev
```

#### 创建 Schemas

让我们首先将 Marshmallow 功能添加到`Product`类中。我们需要创建一个新的类，表示`Product`类的实例必须遵循的 schema。可以把 schema 想象成一个蓝图，定义了`Product`类中的变量及其数据类型。

让我们分解并理解下面的基本代码：

```py
from marshmallow import Schema, fields

class ProductSchema(Schema):
    _id = fields.Int(required=True)
    name = fields.Str(required=True)
    price = fields.Float(required=True)
```

我们创建一个继承自Marshmallow中的`Schema`类的新类。然后，我们声明与`Product`类相同的变量名称并定义它们的字段类型。Marshmallow中的字段类支持各种数据类型；在这里，我们使用原始类型Int、String和Float。

#### 序列化

既然我们已经为对象定义了模式，我们现在可以将Python类实例转换为JSON字符串或Python字典进行序列化。这里是基本实现：

```py
product = Product(_id=4, name="Test Product", price=10.6)
schema = ProductSchema()

# For Python Dictionary object
result = schema.dump(product)

**# type(dict) -> {'_id': 4, 'name': 'Test Product', 'price': 10.6}**

# For JSON-serializable string
result = schema.dumps(product)

**# type(str) -> {"_id": 4, "name": "Test Product", "price": 10.6}**
```

我们创建一个`ProductSchema`对象，它将`Product`对象转换为可序列化的格式，如JSON或字典。

> 注意`dump`和`dumps`函数结果之间的区别。一个返回一个可以使用pickle保存的Python字典对象，另一个返回一个遵循JSON格式的字符串对象。

#### 反序列化

为了逆转序列化过程，我们使用反序列化。一个对象被保存，以便以后可以加载和访问，而Marshmallow可以帮助实现这一点。

Python字典可以通过load函数进行验证，该函数验证变量及其相关的数据类型。下面的函数展示了它是如何工作的：

```py
product_data = {
    "_id": 4,
    "name": "Test Product",
    "price": 50.4,
}
result = schema.load(product_data)
print(result)  	

**# type(dict) -> {'_id': 4, 'name': 'Test Product', 'price': 50.4}**

faulty_data = {
    "_id": 5,
    "name": "Test Product",
    "price": "ABCD" # Wrong input datatype
}
result = schema.load(faulty_data) 

**# Raises validation error**
```

模式验证字典是否具有正确的参数和数据类型。如果验证失败，将引发`ValidationError`，因此将`load function`封装在try-except块中是必要的。如果验证成功，结果对象仍然是一个字典，当原始参数也是字典时，这并不是很有用，对吧？我们通常希望的是验证字典并将其转换回最初序列化的原始对象。

为了实现这一点，我们使用Marshmallow提供的`post_load`装饰器：

```py
from marshmallow import Schema, fields, post_load

class ProductSchema(Schema):
  _id = fields.Int(required=True)
  name = fields.Str(required=True)
  price = fields.Float(required=True)

  @post_load
  def create_product(self, data, **kwargs):
      return Product(**data)
```

我们在模式类中创建一个带有`post_load`装饰器的函数。这个函数接收经过验证的字典并将其转换回`Product`对象。包括`**kwargs`非常重要，因为Marshmallow可能会通过装饰器传递其他必要的参数。

对load功能的这种修改确保在验证后，Python字典被传递到`post_load`函数中，该函数从字典中创建`Product`对象。这使得使用Marshmallow进行对象反序列化成为可能。

#### 验证

通常，我们需要针对我们的使用案例进行额外的验证。虽然数据类型验证是必不可少的，但它并不能涵盖我们可能需要的所有验证。即使在这个简单的示例中，我们的`Product`对象也需要额外的验证。我们需要确保价格不低于0\. 我们还可以定义更多规则，例如确保产品名称的长度在3到128个字符之间。这些规则有助于确保我们的代码库符合定义的数据库模式。

现在让我们看看如何使用Marshmallow实现这种验证：

```py
from marshmallow import Schema, fields, validates, ValidationError, post_load

class ProductSchema(Schema):
    _id = fields.Int(required=True)
    name = fields.Str(required=True)
    price = fields.Float(required=True)

    @post_load
    def create_product(self, data, **kwargs):
        return Product(**data)

    @validates('price')
    def validate_price(self, value):
        if value <= 0:
            raise ValidationError('Price must be greater than zero.')

    @validates('name')
    def validate_name(self, value):
        if len(value) < 3 or len(value) > 128:
            raise ValidationError('Name of Product must be between 3 and 128 letters.')
```

我们修改了 `ProductSchema` 类，添加了两个新函数。一个用于验证价格参数，另一个用于验证名称参数。我们使用 validates 函数装饰器并标注函数要验证的变量名称。这些函数的实现很简单：如果值不正确，我们会引发 `ValidationError`。

#### 嵌套模式

现在，随着基本的 `Product` 类验证，我们已经涵盖了 Marshmallow 库提供的所有基本功能。现在让我们增加复杂性，看看其他两个类如何进行验证。

`Customer` 类相当简单，因为它包含了基本属性和原始数据类型。

```py
class CustomerSchema(Schema):
    _id = fields.Int(required=True)
    name = fields.Int(required=True)
```

然而，为 `Order` 类定义模式迫使我们学习一个新的必需概念——嵌套模式。一个订单将关联到一个特定的客户，而客户可以订购任意数量的产品。这在类定义中得以定义，当我们验证 `Order` 模式时，我们还需要验证传递给它的 `Product` 和 `Customer` 对象。

我们将避免在 `OrderSchema` 中重新定义一切，使用嵌套模式来避免重复。订单模式定义如下：

```py
class OrderSchema(Schema):
    _id = fields.Int(require=True)
    customer = fields.Nested(CustomerSchema, required=True)
    products = fields.List(fields.Nested(ProductSchema), required=True)
```

在 `Order` 模式中，我们包括了 `ProductSchema` 和 `CustomerSchema` 定义。这确保了这些模式的定义验证被自动应用，遵循编程中的 **DRY（不要重复自己）** 原则，从而实现现有代码的重用。

## 总结

在这篇文章中，我们介绍了 Marshmallow 库的快速入门和使用案例，这是 Python 中最受欢迎的序列化和数据验证库之一。虽然与 Pydantic 类似，但许多开发人员更喜欢 Marshmallow，因为它的模式定义方法类似于其他语言（如 JavaScript）中的验证库。

Marshmallow 易于与像 FastAPI 和 Flask 这样的 Python 后端框架集成，使其成为 Web 框架和数据验证任务的热门选择，同时也适用于像 SQLAlchemy 这样的 ORM。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)** Kanwal 是一名机器学习工程师和技术作家，对数据科学以及人工智能与医学的交汇点充满热情。她共同撰写了电子书《用 ChatGPT 最大化生产力》。作为 2022 年 APAC 地区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性技术学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的积极倡导者，她创立了 FEMCodes 以赋能 STEM 领域的女性。

### 更多相关主题

+   [Pydantic 教程：简化的 Python 数据验证](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)

+   [使用 Pandera 对 PySpark 应用进行数据验证](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)

+   [为什么使用 k-fold 交叉验证？](https://www.kdnuggets.com/2022/07/kfold-cross-validation.html)

+   [Pandas AI：生成性 AI Python 库](https://www.kdnuggets.com/2023/05/pandas-ai-generative-ai-python-library.html)

+   [像专业人士一样测试：Python Mock 库的逐步指南](https://www.kdnuggets.com/testing-like-a-pro-a-step-by-step-guide-to-pythons-mock-library)

+   [Pip 安装 YOU：创建 Python 库的初学者指南](https://www.kdnuggets.com/pip-install-you-a-beginners-guide-to-creating-your-python-library)
