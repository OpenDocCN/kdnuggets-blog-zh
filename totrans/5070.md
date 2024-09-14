# 使用Python和Apache Flink入门

> 原文：[https://www.kdnuggets.com/2015/11/getting-started-python-apache-flink.html](https://www.kdnuggets.com/2015/11/getting-started-python-apache-flink.html)

**由[Will McGinnis](http://willmcginnis.com/about/)**。

在我关于Apache中大数据/机器学习项目广度的[上一篇文章](http://willmcginnis.com/2015/11/04/apache-machine-learning-data-analysis-projects/)之后，我决定尝试一些更大的项目。这篇文章作为一个简要指南，帮助你开始使用全新的python API来进入Apache Flink。Flink在高层次上与Spark非常相似，但其底层是一个真正的流处理平台（与Spark的小而快速的批处理流式处理方法相对）。这催生了许多有趣的用例，在这些用例中，大量的数据需要快速且复杂地处理。

基本思想是一个代码流平台，上面有两个处理API和一组库。

![Apache Flink](../Images/a38d47bb333712375a73d699ccc4dcd0.png)

**图1** Flink架构。

在Flink的1.0版本中，将提供一个python API，类似于Spark。虽然在1.0之前的版本中已有，但存在已知的错误，使其使用变得困难或不可能。因此，首先，我们需要构建master分支（除非你正在阅读的是v1.0版本的内容，如果是这种情况，只需按照[Flink的](https://flink.apache.org/)说明进行构建）。

`git clone https://github.com/apache/flink`

`cd flink mvn clean install -DskipTests`

此时，最新版本的Flink构建将会在flink目录下的build-target中创建符号链接。你可以用以下命令启动Flink。

`./build-target/bin/start-cluster.sh ./build-target/bin/start-webclient.sh`

这将启动一个简单的用户界面，地址是localhost:8080，以及一个作业管理器和一个任务管理器。现在我们可以运行一个简单的脚本，为你的项目创建一个新的目录，并在其中创建一个python文件：

`cd .. mkdir flink-examples cd flink-examples touch wordcount.py`

然后将Flink文档中的示例稍微修改后添加到wordcount.py中：

```py
from flink.plan.Environment import get_environment
from flink.plan.Constants import INT, STRING, WriteMode
from flink.functions.GroupReduceFunction \
import GroupReduceFunction

class Adder(GroupReduceFunction):
    def reduce(self, iterator, collector):
        count, word = iterator.next()
        count += sum([x[0] for x in iterator])
        collector.collect((count, word))

if __name__ == "__main__":
    output_file = 'file:///.../flink-examples/out.txt'
    print('logging results to: %s' % (output_file, ))

    env = get_environment()
    data = env.from_elements("Who's there? I think \
         I hear them. Stand, ho! Who's there?")

    data \
        .flat_map(lambda x, c: [(1, word) for word in \
         x.lower().split()], (INT, STRING)) \
        .group_by(1) \
        .reduce_group(Adder(), (INT, STRING), combinable=True) \
        .map(lambda y: 'Count: %s Word: %s' % (y[0], y[1]), STRING) \
        .write_text(output_file, write_mode=WriteMode.OVERWRITE)

    env.execute(local=True)
```

并用以下命令运行它：

`cd .. ./flink/build-target/bin/pyflink3.sh ~./flink-examples/word_count.py`

在out.txt中你现在应该能看到：

`Count: 1 Word: hear Count: 1 Word: ho! Count: 2 Word: i Count: 1 Word: stand, Count: 1 Word: them. Count: 2 Word: there? Count: 1 Word: think Count: 2 Word: who's`

就这样，完全是一个最小的示例，用于在Apache Flink中使用python。代码在这里：[https://github.com/wdm0006/flink-python-examples](https://github.com/wdm0006/flink-python-examples)，我会在这个仓库中以及这里添加更多的高级示例。

**简介: [Will McGinnis](http://willmcginnis.com/about/ )**, @WillMcGinnis，拥有奥本大学机械工程学位，但现在主要从事软件开发。他是Predikto的首位员工，目前帮助构建该公司在重工业领域的预测性维护顶级平台。在工作之余，他通常会从事与Python、Flask、scikit-learn或骑行相关的工作。

[原文](http://willmcginnis.com/2015/11/08/getting-started-with-python-and-apache-flink/ ).

**相关内容**

+   [Apache Flink及流处理的案例](/2015/08/apache-flink-stream-processing.html)

+   [快速大数据: Apache Flink与Apache Spark在流数据处理中的对比](/2015/11/fast-big-data-apache-flink-spark-streaming.html)

+   [采访: Stefan Groschupf，Datameer谈分析中的准确性与简洁性的平衡](/2015/08/interview-stefan-groschupf-accuracy-simplicity-analytics.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### 更多相关主题

+   [快速入门PyTest: 轻松编写和运行Python测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [快速入门Python生成器](https://www.kdnuggets.com/2023/02/getting-started-python-generators.html)

+   [5个步骤快速入门Python数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [快速入门Python数据科学](https://www.kdnuggets.com/getting-started-with-python-for-data-science)

+   [快速入门Claude 3 Opus，它刚刚击败了GPT-4和Gemini](https://www.kdnuggets.com/getting-started-with-claude-3-opus-that-just-destroyed-gpt-4-and-gemini)

+   [快速入门自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)
