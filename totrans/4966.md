# 如何不编程 TensorFlow 图

> 原文：[https://www.kdnuggets.com/2017/05/how-not-program-tensorflow-graph.html](https://www.kdnuggets.com/2017/05/how-not-program-tensorflow-graph.html)

**作者：亚伦·舒马赫，深度学习分析专家。**

使用 Python 操作 TensorFlow 就像用 Python 编程 [另一台计算机](http://planspace.org/20170328-tensorflow_as_a_distributed_virtual_machine/)。一些 Python 语句构建你的 TensorFlow 程序，一些 Python 语句执行该程序，当然，还有一些 Python 语句完全不涉及 TensorFlow。对你构建的图保持思考可以帮助你避免混淆和性能陷阱。这里有一些考虑因素。![](../Images/e09c1745abd025c932b954fd964eedfb.png)

### 避免拥有多个相同的操作

TensorFlow 中的许多方法会在计算图中创建操作，但不会执行它们。你可能希望多次执行这些操作，但这并不意味着你应该创建许多相同操作的副本。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

一个明确的例子是 `tf.global_variables_initializer()`。

```py
 >>> import tensorflow as tf
>>> session = tf.Session()
# Create some variables...
>>> initializer = tf.global_variables_initializer()
# Variables are not yet initialized.
>>> session.run(initializer)
# Now variables are initialized.
# Do some more work...
>>> session.run(initializer)
# Now variables are re-initialized.
```

如果 `tf.global_variables_initializer()` 的调用被重复，例如直接作为 `session.run()` 的参数，则会有一些缺点。

```py
 >>> session.run(tf.global_variables_initializer())
>>> session.run(tf.global_variables_initializer())
```

每当 `session.run()` 的参数在这里被评估时，都会创建一个新的初始化操作。这在图中创建了多个初始化操作。对于小的操作来说，在交互式会话中拥有多个副本并不是什么大问题，如果你创建了更多需要在初始化中包含的变量，可能还会希望这样做。但要考虑是否真的需要大量重复的操作。

在 `session.run()` 内部创建操作时，你也没有一个 Python 变量引用这些操作，因此你无法轻易地重用它们。

在 Python 中，如果你创建了一个没有任何引用的对象，它会被垃圾回收。被遗弃的对象将被删除，它所占用的内存将被释放。TensorFlow 图中的对象不会发生这种情况；你放入图中的所有内容都会留在那里。

很明显，`tf.global_variables_initializer()` 返回的是一个操作。但操作也可以通过不那么明显的方式创建。

让我们来比较一下 NumPy 的工作方式。

```py
 >>> import numpy as np
>>> x = np.array(1.0)
>>> y = x + 1.0
```

目前内存中有两个数组，`x` 和 `y`。`y` 的值是 2.0，但没有记录*如何*获得这个值。加法操作没有留下任何记录。

TensorFlow 是不同的。

```py
 >>> x = tf.Variable(1.0)
>>> y = x + 1.0
```

现在只有`x`是一个TensorFlow变量；`y`是一个`add`操作，如果我们运行它，它可以返回该加法的结果。

再比较一下。

```py
 >>> x = np.array(1.0)
>>> y = x + 1.0
>>> y = x + 1.0
```

这里`y`被分配为一个结果数组`x + 1.0`，然后重新分配为指向不同的数组。第一个数组将被垃圾回收并消失。

```py
 >>> x = tf.Variable(1.0)
>>> y = x + 1.0
>>> y = x + 1.0
```

在这种情况下，`y`指的是TensorFlow图中的一个`add`操作，然后`y`被重新分配指向图中的另一个`add`操作。由于`y`现在只指向第二个`add`，我们没有方便的方法来处理第一个。然而这两个`add`操作仍然存在于图中，并且会继续存在。

（顺便提一下，Python用于定义类特定加法的机制[以及其他](http://www.python-course.eu/python3_magic_methods.php)，就是`+`如何用来创建TensorFlow操作的，这个机制很不错。）

特别是如果你只是使用默认图并在常规REPL或笔记本中交互运行，你可能会在图中留下大量被遗弃的操作。每次重新运行定义任何图操作的笔记本单元时，你不仅仅是在重新定义操作——你是在创建新的操作。

在实验时，拥有几个额外的操作通常是可以的。但事情可能会失控。

```py
 for _ in range(1e6):
    x = x + 1
```

如果`x`是一个NumPy数组，或者只是一个普通的Python数字，这将以常量内存运行，并得出一个值。

但如果`x`是一个TensorFlow变量，那么在你的TensorFlow图中将会有超过一百万个操作，仅仅是定义计算而已，甚至*没有执行*。

TensorFlow的一个直接修复方法是使用`tf.assign`操作，它提供了更符合你期望的行为。

```py
est
increment_x = tf.assign(x, x + 1)
for _ in range(1e6):
    session.run(increment_x)
```

这个修订版本在循环内部不创建任何操作，这通常是一个好建议。TensorFlow确实有[控制流构造](https://www.tensorflow.org/api_guides/python/control_flow_ops)，包括[while循环](https://www.tensorflow.org/api_docs/python/tf/while_loop)。但只有在真正需要时才使用这些。

在创建操作时要注意，只创建你需要的操作。尽量将操作创建与操作执行区分开来。在交互式实验之后，最终进入一个状态，可能是在脚本中，只创建你需要的操作。

### 避免图中的常量

一种特别不幸的操作是意外常量操作，尤其是大的常量操作。

```py
 >>> many_ones = np.ones((1000, 1000))
```

在NumPy数组`many_ones`中有一百万个1。我们可以将它们加起来。

```py
 >>> many_ones.sum()
## 1000000.0
```

如果我们用TensorFlow将它们加起来呢？

```py
 >>> session.run(tf.reduce_sum(many_ones))
## 1000000.0
```

结果是一样的，但机制完全不同。这不仅在图中添加了一些操作——它还将整个百万元素的数组作为常量放入图中。

这种模式的变体可能会导致意外地将整个数据集作为常量加载到图中。程序可能仍然可以运行，适用于小数据集。或者你的系统可能会失败。

避免在图中存储数据的一种简单方法是使用`feed_dict`机制。

```py
 >>> many_things = tf.placeholder(tf.float64)
>>> adder = tf.reduce_sum(many_things)
>>> session.run(adder, feed_dict={many_things: many_ones})
## 1000000.0
```

如前所述，要明确你在何时向图中添加了什么。具体数据通常只在评估时进入图中。

### TensorFlow 作为函数式编程

TensorFlow 操作类似于函数。这在操作具有一个或多个占位符输入时尤为明显；在会话中评估操作就像用这些参数调用函数。因此，返回 TensorFlow 操作的 Python 函数类似于[高阶函数](https://en.wikipedia.org/wiki/Higher-order_function)。

你可能会决定将常量放入图中。例如，如果你需要将很多张量重新调整为 28×28，你可以创建一个执行此操作的操作。

```py
 >>> input_tensor = tf.placeholder(tf.float32)
>>> reshape_to_28 = tf.reshape(input_tensor, shape=[28, 28])
```

这类似于[柯里化](https://en.wikipedia.org/wiki/Currying)，因为`shape`参数现在已经设置好了。`[28, 28]`已成为图中的常量并指定了该参数。现在要评估`reshape_to_28`，我们只需提供`input_tensor`。

已经提出了[神经网络、类型和函数式编程](http://colah.github.io/posts/2015-09-NN-Types-FP/)之间的广泛联系。考虑 TensorFlow 作为支持这种构建系统的系统是很有趣的。

* * *

我正在参与[从组件构建 TensorFlow 系统](http://conferences.oreilly.com/oscon/oscon-tx/public/schedule/detail/57823)，这是在 [OSCON 2017](https://conferences.oreilly.com/oscon/oscon-tx) 上的一个研讨会。

* * *

在 [Twitter](https://twitter.com/planarrowspace) | [LinkedIn](https://www.linkedin.com/in/ajschumacher) | [Google+](https://plus.google.com/112658546306232777448/) | [GitHub](https://github.com/ajschumacher) | [email](mailto:ajschumacher@gmail.com) 上找到 [Aaron](http://planspace.org/aaron/)

[原文](http://planspace.org/20170404-how_not_to_program_the_tensorflow_graph/)。经许可转载。

**个人简介: [Aaron Schumacher](https://www.linkedin.com/in/ajschumacher/)** 是 Deep Learning Analytics 的高级数据科学家和软件工程师。他在 [@planarrowspace](https://twitter.com/planarrowspace) 上发推文。

**相关:**

+   [如何在 TensorFlow 中构建递归神经网络](/2017/04/build-recurrent-neural-network-tensorflow.html)

+   [最温和的 Tensorflow 入门介绍 – 第一部分](/2016/08/gentlest-introduction-tensorflow-part-1.html)

+   [Python 深度学习框架概述](/2017/02/python-deep-learning-frameworks-overview.html)

### 更多相关话题

+   [AWS AI & ML 奖学金项目概述](https://www.kdnuggets.com/2022/09/aws-ai-ml-scholarship-program-overview.html)

+   [免费注册 4 年计算机科学学位课程](https://www.kdnuggets.com/enroll-in-a-4-year-computer-science-degree-program-for-free)

+   [加入 UC 的商业硕士信息会](https://www.kdnuggets.com/2022/10/ucincinnati-join-ucs-information-session-masters-business-analytics-program.html)

+   [通过第 3 最佳在线数据硕士课程最大化你的价值…](https://www.kdnuggets.com/2023/05/bay-path-maximize-value-online-masters-data-science.html)

+   [通过第三名最佳在线数据科学硕士项目推动你的职业发展](https://www.kdnuggets.com/2023/07/bay-path-advance-career-3rd-best-online-masters-data-science-program.html)

+   [通过第三名最佳在线数据科学项目攻读硕士学位](https://www.kdnuggets.com/2023/09/bay-path-pursue-masters-data-science-3rd-best-online-program)
