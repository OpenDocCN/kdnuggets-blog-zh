# Pyjanitor 数据清理初学者指南

> 原文：[https://www.kdnuggets.com/beginners-guide-to-data-cleaning-with-pyjanitor](https://www.kdnuggets.com/beginners-guide-to-data-cleaning-with-pyjanitor)

![使用 PyJanitor 进行数据清理](../Images/9fe5bbb5c146b54a62f4af26d2c242c8.png)

图片由作者提供 | DALLE-3 & Canva

你是否曾处理过混乱的数据集？它们是任何数据科学项目中最大的障碍之一。这些数据集可能包含不一致、缺失值或不规则性，妨碍分析。数据清理是打下准确可靠洞察基础的必要第一步，但它既漫长又耗时。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加入网络安全职业的快速通道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 支持

* * *

不用担心！让我向你介绍 Pyjanitor，一个可以拯救你的出色 Python 库。它是一个方便的 Python 包，提供了一个简单的解决方案来应对这些数据清理挑战。在这篇文章中，我将讨论 Pyjanitor 的重要性以及它的功能和实际使用。

在本文结束时，你将清楚了解 Pyjanitor 如何简化数据清理及其在日常数据相关任务中的应用。

## 什么是 Pyjanitor？

Pyjanitor 是一个扩展的 R 包，基于 pandas 构建，简化了数据清理和预处理任务。它通过提供各种有用的功能来扩展其功能，优化数据清理、转换和准备的过程。可以把它看作是你数据清理工具包的升级版。你是否迫不及待想了解 Pyjanitor 了？我也是。让我们开始吧。

## 入门指南

首先，你需要安装 Pyjanitor。打开你的终端或命令提示符并运行以下命令：

```py
pip install pyjanitor
```

下一步是将 Pyjanitor 和 Pandas 导入你的 Python 脚本。可以通过以下方式完成：

```py
import janitor
import pandas as pd
```

现在，你已经准备好使用 Pyjanitor 进行数据清理任务。接下来，我将介绍 Pyjanitor 一些最有用的功能，包括：

### 1\. 清理列名称

举手如果你曾因列名称不一致而感到沮丧。没错，我也是。使用 Pyjanitor 的 `clean_names()` 函数，你可以迅速标准化列名称，使其统一且一致，只需简单调用即可。这个强大的函数将空格替换为下划线，将所有字符转换为小写，去除前后空格，甚至将点替换为下划线。让我们通过一个基本的例子来理解它。

```py
#Create a data frame with inconsistent column names
student_df = pd.DataFrame({
    'Student.ID': [1, 2, 3],
    'Student Name': ['Sara', 'Hanna', 'Mathew'],
    'Student Gender': ['Female', 'Female', 'Male'],
    'Course': ['Algebra', 'Data Science', 'Geometry'],
    'Grade': ['A', 'B', 'C']
})

#Clean the column names
clean_df = student_df.clean_names()
print(clean_df)
```

**输出：**

```py
 student_id    student_name    student_gender        course    grade
0           1            Sara            Female       Algebra        A
1           2           Hanna            Female  Data Science        B
2           3          Mathew              Male      Geometry        C
```

### 2\. 重命名列

有时，重命名列不仅能提升我们对数据的理解，还能改善数据的可读性和一致性。感谢 `rename_column()` 函数，这项任务变得轻而易举。一个展示该函数实用性的简单例子如下：

```py
student_df = pd.DataFrame({
    'stu_id': [1, 2],
    'stu_name': ['Ryan', 'James'],
})
# Renaming the columns
student_df = student_df.rename_column('stu_id', 'Student_ID')
student_df =student_df.rename_column('stu_name', 'Student_Name')
print(student_df.columns) 
```

**输出：**

```py
Index(['Student_ID', 'Student_Name'], dtype='object') 
```

### 3\. 处理缺失值

处理缺失值确实是处理数据集时的一个难题。幸运的是，`fill_empty()` 函数可以用来解决这些问题。让我们通过一个实际例子来探索如何使用 Pyjanitor 处理缺失值。首先，我们将创建一个虚拟数据框，并填充一些缺失值。

```py
# Create a data frame with missing values
employee_df = pd.DataFrame({
    'employee_id': [1, 2, 3],
    'name': [None, 'James', 'Alicia'],
    'department': ['HR', None, 'Engineering'],
    'salary': [60000, 55000, None]
}) 
```

现在，让我们看看 Pyjanitor 如何帮助填补这些缺失值：

```py
# Fill missing values in 'department' and 'name' with 'Unknown' and 'salary' with the mean salary
employee_df = employee_df.fill_empty(column_names=['name', 'department'], value='Unknown')
employee_df = employee_df.fill_empty(column_names='salary', value=employee_df['salary'].mean())

print(employee_df) 
```

**输出：**

```py
 employee_id     name   department   salary
0            1  Unknown           HR  60000.0
1            2    James      Unknown  55000.0
2            3   Alicia  Engineering  57500.0 
```

在这个例子中，员工**‘詹姆斯’**的部门被替换为‘**未知**’，**‘艾莉西亚’**的工资则被替换为**‘未知’**和**‘詹姆斯’**工资的平均值。你可以使用各种策略来处理缺失值，如前向填充、后向填充或填入特定值。

### 4\. 过滤行和选择列

过滤行和列是数据分析中的一个关键任务。Pyjanitor 通过提供允许你根据特定条件选择列和过滤行的函数，简化了这一过程。假设你有一个包含学生记录的数据框，你想要过滤出成绩低于 60 分的学生（行）。让我们探索 Pyjanitor 如何帮助实现这一点。

```py
# Create a data frame with student data
students_df = pd.DataFrame({
    'student_id': [1, 2, 3, 4, 5],
    'name': ['John', 'Julia', 'Ali', 'Sara', 'Sam'],
    'subject': ['Maths', 'General Science', 'English', 'History','Biology'],
    'marks': [85, 58, 92, 45, 75],
    'grade': ['A', 'C', 'A+', 'D', 'B']
})

# Filter rows where marks are less than 60
filtered_students_df = students_df.query('marks >= 60')
print(filtered_students_df) 
```

**输出：**

```py
 student_id  name  subject  marks grade
0           1  John    Maths     85     A
2           3   Ali  English     92    A+
4           5   Sam  Biology     75     B 
```

现在假设你还希望只输出特定的列，比如仅输出姓名和ID，而不是所有数据。Pyjanitor 也可以通过以下方式来实现这一点：

```py
# Select specific columns
selected_columns_df = filtered_students_df.loc[:,['student_id', 'name']] 
```

**输出：**

```py
 student_id	name
0	1	John
2	3	Ali
4	5	Sam 
```

### 5\. 方法链

利用 Pyjanitor 的方法链功能，你可以在一行中执行多个操作。这一能力被认为是其最优秀的特性之一。举例来说，假设有一个包含汽车数据的数据框：

```py
# Create a data frame with sample car data
cars_df = pd.DataFrame({
    'Car ID': [101, None, 103, 104, 105],
    'Car Model': ['Toyota', 'Honda', 'BMW', 'Mercedes', 'Tesla'],
    'Price': [25000, 30000, None, 40000, 45000],
    'Year': [2018, 2019, 2017, 2020, None]
})
print("Cars Data Before Applying Method Chaining:")
print(cars_df) 
```

**输出：**

```py
Cars Data Before Applying Method Chaining:
   Car ID Car Model    Price    Year
0   101.0    Toyota  25000.0  2018.0
1     NaN     Honda  30000.0  2019.0
2   103.0       BMW      NaN  2017.0
3   104.0  Mercedes  40000.0  2020.0
4   105.0     Tesla  45000.0     NaN 
```

现在我们看到数据框包含缺失值和不一致的列名。我们可以通过按顺序执行操作来解决这个问题，例如 `clean_names()`、`rename_column()` 和 `dropna()` 等。或者，我们可以将这些方法链在一起——在一行中执行多个操作——以获得流畅的工作流程和更简洁的代码。

```py
# Chain methods to clean column names, drop rows with missing values, select specific columns, and rename columns
cleaned_cars_df = (
    cars_df
    .clean_names()  # Clean column names
    .dropna()  # Drop rows with missing values
    .select_columns(['car_id', 'car_model', 'price'])  # Select columns
    .rename_column('price', 'price_usd')  # Rename column
)

print("Cars Data After Applying Method Chaining:")
print(cleaned_cars_df) 
```

**输出：**

```py
Cars Data After Applying Method Chaining:
   car_id car_model  price_usd
0   101.0    Toyota    25000.0
3   104.0  Mercedes    40000.0 
```

在这个流程中，执行了以下操作：

+   `clean_names()` 函数清理列名。

+   `dropna()` 函数丢弃缺失值所在的行。

+   `select_columns()` 函数选择特定的列，即 ‘car_id’、‘car_model’ 和 ‘price’。

+   `rename_column()` 函数将列名 ‘price’ 重命名为 ‘price_usd’。

## 总结

因此，总结来说，Pyjanitor 证明了它是一个对任何数据工作者来说都很神奇的库。它提供了比本文中讨论的更多功能，如编码分类变量、获取特征和标签、识别重复行等等。所有这些高级功能和方法都可以在其[文档](https://pyjanitor-devs.github.io/pyjanitor/api/functions/)中进行探索。你对其功能的了解越深入，你会对它强大的功能感到越惊讶。最后，享受使用 Pyjanitor 操作数据的乐趣吧。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学及人工智能与医学的交叉领域充满热情。她共同编写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年 APAC 地区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性技术学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的热心倡导者，她创立了 FEMCodes，旨在赋能 STEM 领域的女性。

### 相关主题

+   [数据科学中的异常检测技术入门指南](https://www.kdnuggets.com/2023/05/beginner-guide-anomaly-detection-techniques-data-science.html)

+   [数据工程入门指南](https://www.kdnuggets.com/2023/07/beginner-guide-data-engineering.html)

+   [数据科学入门指南](https://www.kdnuggets.com/2023/07/introduction-data-science-beginner-guide.html)

+   [端到端机器学习入门指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [必备的机器学习算法：初学者指南](https://www.kdnuggets.com/2021/05/essential-machine-learning-algorithms-beginners.html)

+   [Q 学习入门指南](https://www.kdnuggets.com/2022/06/beginner-guide-q-learning.html)
