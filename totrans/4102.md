# 使用 Python 自动化的 5 个任务

> 原文：[https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html](https://www.kdnuggets.com/2021/06/5-tasks-automate-python.html)

![](../Images/271ec7f71d6461a8afd003f2eb9511ba.png)

作者照片

你自动化，我自动化，我们都自动化。我们自动化我们的财务、待办事项和社交生活。那么，为什么在自动化我们的职业生活时仍然有这么多抵触呢？我当了十多年的软件工程师，并且一直是自动化的倡导者。我亲眼见证了自动化的好处，并帮助公司采用了它。在这篇博客文章中，我将分享 10 个你可以用 Python 自动化的小任务。

# 介绍

无论你是在编写软件、编写业务逻辑，还是仅仅在做笔记，自动化都是你的朋友。软件界长期以来一直在与竞争对手进行“人工智能军备竞赛”。即便是 Google 也在开发自主机器人。作为开发者，我们如何竞争？通过专注于自己的优势。我们可以通过将用于产品开发的相同技术应用到软件开发中来做到这一点。我们可以将先进的技术应用于问题解决，然后自动化收集用于这些解决方案的信息。我个人发现，我解决的问题越深入，我就越容易成为解决方案的高手，并且专注于我觉得最有趣的部分。

# 使用 Python 自动化的 5 个任务

这绝不是一个全面的列表，也不会对每个任务提供相同的细节。但它应该为你提供一个坚实的起点。如果你对自动化不太熟悉，我建议你查看[机器人学院](https://medium.com/robotacademy)的档案以了解更多信息。

# #1\. 阅读（将任何文件转化为有声书）

你可以使用下面的脚本将 Mac 上的任何文件转化为有声书，并在后台收听。

首先，安装以下依赖项。

```py
pip install mac-say
```

然后创建一个你将用于执行此任务的 Python 文件。

```py
import sys
import mac_say
mac_say.say(["-f", sys.argv[1], "-v", "Alex"])
```

然后在命令行中指向你选择的文件，尽情享受

```py
python audiobook.py fileofyourchoice.txt
```

# #2\. 快速天气报告

查看天气通常是一件很快的事情，但通过点击一个按钮来完成它可能会带来一些满足感。

这也只需要一个依赖项。

```py
pip install requests
```

安装后，只需创建一个文件并使用下面的脚本运行。

```py
import sys
import requests
resp = requests.get(f'[https://wttr.in/{sys.argv[1].replace(](https://wttr.in/%7Bsys.argv[1].replace()" ", "+")}')
print(resp.text)
```

然后，你准备好每天运行或安排以下任务。

```py
python weather.py "Your City"
```

![](../Images/bcc66567545eefeb679c6f37f1fa3bac.png)

# #3\. 货币转换

这项任务稍微简单一些，我们只需按如下方式安装库。

```py
pip install --user currencyconverter
```

这个安装应该将`currency_converter`放到我们的`$PATH`中，因此要执行转换，只需像示例执行那样写下以下内容。

```py
currency_converter 1 USD --to EUR
```

# #4\. 自动整理你的下载文件夹

在这个示例中，我们将只监听 PDF、图片、音频和视频，但这可以扩展得相当多，并且应该足够让你开始。我对此稍微过于投入了。

```py
import sys
import os
import time
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler

folder_to_monitor = sys.argv[1]

file_folder_mapping = {
    ".png": "images",
    ".jpg": "images",
    ".jpeg": "images",
    ".gif": "images",
    ".pdf": "pdfs",
    ".mp4": "videos",
    ".mp3": "audio",
    ".zip": "bundles",
}

class DownloadedFileHandler(FileSystemEventHandler):
    def on_created(self, event):
        if any(event.src_path.endswith(x) for x in file_folder_mapping):
            parent = os.path.join(
                os.path.dirname(os.path.abspath(event.src_path)),
                file_folder_mapping.get(f".{event.src_path.split('.')[-1]}"),
            )
            if not os.path.exists(parent):
                os.makedirs(parent)
            os.rename(
                event.src_path, os.path.join(parent, os.path.basename(event.src_path))
            )
            event_handler = DownloadedFileHandler()

observer = Observer()
observer.schedule(event_handler, folder_to_monitor, recursive=True)
print("Monitoring started")
observer.start()
try:
    while True:
        time.sleep(10)
except KeyboardInterrupt:
    observer.stop()
    observer.join()
```

一旦你创建了这个文件，你所需要做的就是运行它，并指向你的下载目录以开始监控它。

```py
python downloads-watchdog.py "/your/downloads/folder"
```

# #5\. 早晨设置脚本

早晨通常你想要做很少的事情，直到咖啡因发挥作用。这个脚本会通过打开你每早通常需要打开的所有浏览器标签页来提前启动你的早晨。保存一个包含你选择的网址的脚本文件，如下面的示例所示。

```py
python -m webbrowser -t "https://www.google.com"
python -m webbrowser -t "https://www.dylanroy.com"
python -m webbrowser -t "https://www.usesql.com"
```

# 结论

Python 是一个强大的工具，但你学习和实践得越多，你会变得越高效和生产力更强。我很高兴与大家分享一些有趣或有用的自动化任务，希望你们觉得它们有用。如果你有任何问题，请随时询问。

# 资源

+   [Mac 文本转语音 Python 库](https://github.com/andrewp-as-is/mac-say.py)

+   [Python 天气库](https://github.com/vierofernando/python-weather)

+   [Python 货币转换器](https://github.com/alexprengere/currencyconverter)

+   [Python Watchdog](https://github.com/gorakhargosh/watchdog)

+   [Python 网页浏览器](https://docs.python.org/3/library/webbrowser.html)

**[迪伦·罗伊](https://www.linkedin.com/in/dylan-roy/)** 目前与道琼斯公司合作，利用尖端技术和创业精神提供创新产品。经常利用大数据和云技术持续为客户创造价值。他在爱荷华州立大学工程学院获得计算机工程学士学位。点击这里订阅更多内容（[**dylanroy.com**](http://dylanroy.com)）

[原文](https://medium.com/robotacademy/5-tasks-to-automate-with-python-e7146996f3)。已获许可转载。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [使用 Python 自动化 Microsoft Excel 和 Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [使用 GPT-4 和 Python 自动化枯燥的任务](https://www.kdnuggets.com/2023/03/automate-boring-stuff-chatgpt-python.html)

+   [使用 Python 自动化数据清理的 5 个简单步骤](https://www.kdnuggets.com/5-simple-steps-to-automate-data-cleaning-with-python)

+   [使用 Promptr 和 GPT 自动化你的代码库](https://www.kdnuggets.com/2023/04/automate-codebase-promptr-gpt.html)

+   [使用 ChatGPT Canva 插件自动化图形设计活动](https://www.kdnuggets.com/automate-graphic-design-activity-with-chatgpt-canva-plugin)

+   [AI-自动化网络安全：应该自动化什么？](https://www.kdnuggets.com/ai-automated-cybersecurity-what-to-automate)
