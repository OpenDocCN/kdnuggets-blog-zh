# 使用 Python 自动化 Microsoft Excel 和 Word

> 原文：[https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

![](../Images/3da70b1153fb84477014c33753751857.png)

照片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

毫无疑问，Microsoft Excel 和 Word 是企业界和非企业界使用最广泛的两个软件。它们实际上与“工作”这个词几乎是同义的。我们几乎每周都会使用这两个软件的组合，或多或少地发挥它们的作用。虽然对于日常使用，自动化可能不必要，但有时自动化是必须的。特别是当你需要生成大量图表、数据、表格和报告时，选择手动方式可能会变得非常繁琐。不过，事情并不一定非得这样。实际上，你可以通过 Python 创建一个管道，轻松地将两者集成起来，生成 Excel 电子表格，然后将结果传输到 Word 中，几乎可以瞬间生成报告。

## Openpyxl

认识一下 Openpyxl，这可能是 Python 中最通用的绑定之一，使与 Excel 的接口简直就像散步一样轻松。凭借它，你可以读取和写入所有当前和遗留的 Excel 格式，即 xlsx 和 xls。Openpyxl 允许你填充行和列，执行公式，创建 2D 和 3D 图表，标记轴和标题，以及其他许多 [功能](https://openpyxl.readthedocs.io/en/stable/index.html) 都会非常有用。然而，最重要的是，这个软件包使你能够遍历 Excel 中无数的行和列，从而免去你以前不得不进行的繁琐的数据处理和绘图工作。

## Python-docx

然后就是 Python-docx，这个软件包对 Word 的作用就像 Openpyxl 对 Excel 的作用一样。如果你还没有研究过它们的 [文档](https://python-docx.readthedocs.io/en/latest/)，那么你应该看看。Python-docx 毫不夸张地说是我自从开始使用 Python 以来，最简单、最自解释的工具包之一。它允许你通过自动插入文本、填充表格和将图像渲染到报告中来自动化文档生成，完全不需要任何额外的开销。

事不宜迟，让我们创建我们自己的自动化管道。请启动 Anaconda（或任何你选择的 IDE）并安装以下软件包：

```py
pip install openpyxlpip install python-docx
```

## Microsoft Excel 自动化

首先，我们将加载一个已经创建的 Excel 工作簿（如下所示）：

```py
workbook = xl.load_workbook('Book1.xlsx')
sheet_1 = workbook['Sheet1']
```

![](../Images/52f4ba3dd0aade877b8554f81494495b.png)

图片由作者提供。

随后，我们将遍历电子表格中的所有行，通过将电流乘以电压来计算和插入功率值：

```py
for row in range(2, sheet_1.max_row + 1):
    current = sheet_1.cell(row, 2)
    voltage = sheet_1.cell(row, 3)
    power = float(current.value) * float(voltage.value)
    power_cell = sheet_1.cell(row, 1)
    power_cell.value = power
```

完成后，我们将使用计算出的功率值生成一个折线图，并将其插入到下面所示的指定单元格中：

```py
values = Reference(sheet_1, min_row = 2, max_row = sheet_1.max_row, min_col = 1, max_col = 1)
chart = LineChart()
chart.y_axis.title = 'Power'
chart.x_axis.title = 'Index'
chart.add_data(values)
sheet_1.add_chart(chart, 'e2') 
workbook.save('Book1.xlsx')
```

![](../Images/22ae0f23a3d6984b894822a78cbea668.png)

自动生成的Excel电子表格。图片由作者提供。

## 提取图表

现在我们生成了图表，我们需要将其提取为图像，以便在我们的Word报告中使用。首先，我们将声明Excel文件的确切位置以及输出图表图像应该保存的位置：

```py
input_file = "C:/Users/.../Book1.xlsx"
output_image = "C:/Users/.../chart.png"
```

然后使用以下方法访问电子表格：

```py
operation = win32com.client.Dispatch("Excel.Application")
operation.Visible = 0
operation.DisplayAlerts = 0
workbook_2 = operation.Workbooks.Open(input_file)
sheet_2 = operation.Sheets(1)
```

随后，你可以遍历电子表格中的所有图表对象（如果有多个），并将它们保存到指定位置：

```py
for x, chart in enumerate(sheet_2.Shapes):
    chart.Copy()
    image = ImageGrab.grabclipboard()
    image.save(output_image, 'png')
    passworkbook_2.Close(True)
operation.Quit()
```

## Microsoft Word自动化

现在我们生成了图表图片，我们必须创建一个模板文档，这个文档基本上是一个正常的Microsoft Word文档（.docx），按照我们希望报告的样子进行格式化，包括字体、字号、格式和页面结构。然后，我们只需创建自动内容的占位符，即表格值和图片，并用变量名声明，如下所示。

![](../Images/1a3c67ea6dd71be4518c41b89adbc7e2.png)

Microsoft Word文档模板。图片由作者提供。

任何自动生成的内容都可以放在一对双大括号{{*variable_name*}}内，包括文本和图片。对于表格，你需要创建一个包含所有列的模板行，然后你需要在上面和下面各追加一行，使用以下标记：

**第一行：**

```py
{%tr for item in *variable_name* %}
```

**最后一行：**

```py
{%tr endfor %}
```

在上图中，变量名是

+   *table_contents*用于存储我们的表格数据的Python字典

+   *索引*用于字典键（第一列）

+   *功率、电流和电压*用于字典值（第二、第三和第四列）

然后我们将模板文档导入Python，并创建一个字典来存储我们表格的值：

```py
template = DocxTemplate('template.docx')
table_contents = []for i in range(2, sheet_1.max_row + 1):
    table_contents.append({
        'Index': i-1,
        'Power': sheet_1.cell(i, 1).value,
        'Current': sheet_1.cell(i, 2).value,
        'Voltage': sheet_1.cell(i, 3).value
        })
```

接下来我们将导入先前由Excel生成的图表图片，并创建另一个字典来实例化模板文档中声明的所有占位符变量：

```py
image = InlineImage(template,'chart.png',Cm(10))context = {
    'title': 'Automated Report',
    'day': datetime.datetime.now().strftime('%d'),
    'month': datetime.datetime.now().strftime('%b'),
    'year': datetime.datetime.now().strftime('%Y'),
    'table_contents': table_contents,
    'image': image
    }
```

最后，我们将用包含值的表格和图表图片渲染报告：

```py
template.render(context)
template.save('Automated_report.docx')
```

## 结果

就这样，一个自动生成的Microsoft Word报告，其中包含数字和在Microsoft Excel中创建的图表。这样，你就有了一个完全自动化的管道，可以用来创建你可能需要的任何数量的表格、图表和文档。

![](../Images/51cb8d13e890c7eb47fc3eb40aaa4a49.png)

自动生成的报告。图片由作者提供。

## 源代码

如果你想了解更多关于数据可视化和Python的内容，可以查看以下（附属链接的）课程：

[****用 Python 进行数据可视化****](https://www.coursera.org/learn/python-for-data-visualization?ranMID=40328&ranEAID=hOGDdF2uhHQ&ranSiteID=hOGDdF2uhHQ-gyVyBrINeBGN.FkaHKhFYw&siteID=hOGDdF2uhHQ-gyVyBrINeBGN.FkaHKhFYw&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=hOGDdF2uhHQ)

[****人人学 Python 专项课程****](https://www.coursera.org/specializations/python?ranMID=40328&ranEAID=hOGDdF2uhHQ&ranSiteID=hOGDdF2uhHQ-kfqIujfL9KjRC898fWsllg&siteID=hOGDdF2uhHQ-kfqIujfL9KjRC898fWsllg&utm_content=10&utm_medium=partners&utm_source=linkshare&utm_campaign=hOGDdF2uhHQ)

本教程的源代码和模板可以在以下 GitHub 仓库中找到。

[**mkhorasani/excel_word_automation**](https://github.com/mkhorasani/excel_word_automation)

此外，欢迎订阅 Medium，探索更多我的教程 [**在这里**](https://khorasani.medium.com/membership)。

**[穆罕默德·霍拉萨尼](https://www.linkedin.com/in/mkhorasani/)** 是数据科学家和工程师的混合体。后勤学家。直言不讳。现实政治。一点一点地摒弃教条。 [阅读更多穆罕默德的文章](https://khorasani.medium.com/)。

[原文](https://towardsdatascience.com/automate-microsoft-excel-and-word-using-python-1eee9c003471)。已获许可转载。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关内容

+   [免费的初学者微软 Excel 课程](https://www.kdnuggets.com/2022/09/free-microsoft-excel-beginners-course.html)

+   [用 GPT-4 和 Python 自动化无聊的事情](https://www.kdnuggets.com/2023/03/automate-boring-stuff-chatgpt-python.html)

+   [用 Python 自动化的 5 个任务](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

+   [用 Python 自动化数据清洗的 5 个简单步骤](https://www.kdnuggets.com/5-simple-steps-to-automate-data-cleaning-with-python)

+   [用 Promptr 和 GPT 自动化您的代码库](https://www.kdnuggets.com/2023/04/automate-codebase-promptr-gpt.html)

+   [用 ChatGPT Canva 插件自动化图形设计活动](https://www.kdnuggets.com/automate-graphic-design-activity-with-chatgpt-canva-plugin)
