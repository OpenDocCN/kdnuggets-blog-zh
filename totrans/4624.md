# 使用 Python 进行实用语音识别：基础知识

> 原文：[https://www.kdnuggets.com/2019/07/practical-speech-recognition-python-basics.html](https://www.kdnuggets.com/2019/07/practical-speech-recognition-python-basics.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

你是否曾经想尝试一个语音识别项目，但发现它太令人畏惧了？

那么，创建一些更复杂的东西，比如一个完整的音频聊天机器人或语音助手，怎么样？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

为这种类型的项目组装骨架代码实际上非常简单，这要归功于一些我们可以依赖的开源库。考虑到这一点，让我们看看如何使用 Python 创建一个基本的玩具语音识别应用。掌握基础后，我们可以讨论如何使其变得更加实用。

![头图](../Images/c0581b7a709c7285150f2a47121bf610.png)

我们的玩具 Python 应用实际上会相当无用。但它会介绍一些概念，这些概念对于之后构建更复杂的东西会有用。如果我们把这个玩具做好，修改它以完成其他任务应该会相对轻松。至少，某种程度上是这样。

这就是我们的应用完成后会做的事情：它会听我们说的话，并将其重复给我们。这就是全部！我们将获得的有用功能是将语音识别和音频播放集成到我们的应用中。

首先，让我们导入需要的一些库：

```py
import os

import speech_recognition as sr

from pydub import AudioSegment
from pydub.playback import play
from gtts import gTTS as tts

```

这是其理由：

+   **`[speech_recognition](https://pypi.org/project/SpeechRecognition/)`** - “用于执行语音识别的库，支持多种引擎和 API，包括在线和离线”

+   **`[pydub](http://pydub.com/)`** - “使用简单易用的高级接口来操作音频”

+   **`[gTTS](https://pypi.org/project/gTTS/)`** - “与 Google 翻译的文本转语音 API 交互的 Python 库和 CLI 工具”

接下来要做的事情——可能对语音识别应用最重要——就是识别语音。为此，我们需要首先从麦克风捕获输入的音频，然后进行语音识别。这一切都通过 `speech_recognition` 库来处理。

这是一个用于捕获语音的函数。

```py
def capture():
    """Capture audio"""

    rec = sr.Recognizer()

    with sr.Microphone() as source:
        print('I\'M LISTENING...')
        audio = rec.listen(source, phrase_time_limit=5)

    try:
        text = rec.recognize_google(audio, language='en-US')
        return text

    except:
        speak('Sorry, I could not understand what you said.')
        return 0

```

就这样。语音已捕获并识别。还觉得这令人畏惧吗？

请注意，一旦应用程序开始运行，它将每5秒钟监听一次，并逐一处理这些5秒钟的时间间隔。实际吗？不太实际，但一旦我们做一些更复杂的事情，我们可以调整这个方法，或许可以监听一个激活关键词，然后听取我们讲话的整个时长，无论其长度如何。然而，这是一种足够简单的起始方式。

那么，我们在捕获语音后会做什么？我们将处理它。这究竟意味着什么？

你正在构建的应用程序类型将在很大程度上决定“处理它”意味着什么。这一次，我们的处理将或多或少是一个占位函数，用于将来做其他事情。因此，目前，我们的玩具应用程序将通过将捕获的语音反向输出给我们（并将其输出到控制台，以备查阅）来处理捕获的语音。

这是一个简单的处理函数。

```py
def process_text(name, input):
    """Process what is said"""

    speak(name + ', you said: "' + input + '".')
    return

```

我们还希望我们的应用程序能够说话，所以让我们编写一个使用 Google 文本转语音引擎来实现这一点的函数。

```py
def speak(text):
    """Say something"""

    # Write output to console
    print(text)

    # Save audio file
    speech = tts(text=text, lang='en')
    speech_file = 'input.mp3'
    speech.save(speech_file)

    # Play audio file
    sound = AudioSegment.from_mp3(speech_file)
    play(sound)
    os.remove(speech_file)

```

首先，我们将传递给函数的内容打印到控制台；然后使用 Google 文本转语音创建一个音频文件；将音频文件保存到磁盘；接着重新打开并使用 `pydub` 库播放该文件。

这就是处理“困难”部分的工作了。现在我们只需几行代码来驱动这一过程。

```py
if __name__ == "__main__":

    # First get name
    speak('What is your name?')
    name = capture()
    speak('Hello, ' + name + '.')

    # Then just keep listening & responding
    while 1:
        speak('What do you have to say?')
        captured_text = capture().lower()

        if captured_text == 0:
            continue

        if 'quit' in str(captured_text):
            speak('OK, bye, ' + name + '.')
            break

        # Process captured text
        process_text(name, captured_text)

```

`speak()` 和 `capture()` 函数用于在提示时获取用户的名字，然后进行问候。接着进入一个循环，在这个循环中，我们在捕获语音输入和执行一些非常基础的检查之间切换，以确保*捕获到了一些东西*，并且用户没有说“退出”以退出。捕获到的文本被传递到 `process_text()` 函数，该函数会回显所说的内容。这一过程然后被重复进行 *无限次*。

我再说一遍：这里没有什么复杂的东西。

将上述所有代码保存到文件 `voice_recognition_test.py` 中。

现在让我们查看一下与我们简约语音识别应用程序的对话。使用以下命令运行它，然后查看下面的结果（当然要想象我在说话，并且我的话被重复回来）。

```py` ```   $ python voice_recognition_test.py ```py    ``` 你叫什么名字？  我在听...  你好，Matthew。  你有什么话要说？  我在听...  Matthew，你说：“你从哪里来”。  你有什么话要说？  我在听...  Matthew，你说：“我想要一些披萨”。  你有什么话要说？  我在听...  Matthew，你说：“生命的意义是什么”。  你有什么话要说？  我在听...  好的，再见，Matthew。    进程以退出代码0结束 ```py    Pretty cool. Of course, it could be way cooler if it actually *did* something. So let's turn our attention to that next.    For next time, let's ease into something more complex like integrating spaCy into our code and trying some simple NLP tasks, such as spoken sentence classification, sentiment analysis, and named entity recognition.    We can then look at something more practically useful such as making a personal voice assistant, which will require some additional tweaks to our interface. But one thing at a time...      **Related**:    *   [Comparison of the Top Speech Processing APIs](/2018/12/activewizards-comparison-speech-processing-apis.html) *   [Build Your First Chatbot Using Python & NLTK](/2019/05/build-chatbot-python-nltk.html) *   [Building NLP Classifiers Cheaply With Transfer Learning and Weak Supervision](/2019/03/building-nlp-classifiers-cheaply-transfer-learning-weak-supervision.html)     ### More On This Topic    *   [The Evolution of Speech Recognition Metrics](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html) *   [Build a Text-to-Speech Converter with Python in 5 Minutes](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html) *   [Transfer Learning for Image Recognition and Natural Language Processing](https://www.kdnuggets.com/2022/01/transfer-learning-image-recognition-natural-language-processing.html) *   [5 IT Jobs That Are High in Demand But Don’t Get Enough Recognition](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition) *   [Free eBook: 10 Practical Python Programming Tricks](https://www.kdnuggets.com/2023/04/free-ebook-10-practical-python-programming-tricks.html) *   [Customer Segmentation in Python: A Practical Approach](https://www.kdnuggets.com/customer-segmentation-in-python-a-practical-approach) ````
