# 使用 Wav2Vec 2.0 进行语音转文本

> 原文：[`www.kdnuggets.com/2021/03/speech-text-wav2vec.html`](https://www.kdnuggets.com/2021/03/speech-text-wav2vec.html)

评论

**由 [Dhilip Subramanian](https://medium.com/@sdhilip)，数据科学家和 AI 爱好者**

![图示](img/f1afdce2c2ee8cce809cd1a6f89536a7.png)

来源于 [Wav2vec 2.0: 从原始音频学习语音结构](https://ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio/)

[在我之前的博客中](https://towardsdatascience.com/easy-speech-to-text-with-python-3df0d973b426)，我解释了如何使用 Speech Recognition 库和 Google 语音识别 API 将语音转换为文本。在本博客中，我们将探讨如何使用 Facebook Wav2Vec 2.0 模型将语音转换为文本。

[Facebook](https://ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio) 最近推出并 [开源了他们的新框架](https://ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio)，用于从原始音频数据中自监督学习表示，称为 Wav2Vec 2.0。Facebook 研究人员声称，这个框架可以使 [自动语音识别模型](https://analyticsindiamag.com/facebook-makes-advancements-in-automatic-speech-recognition/) 仅凭 10 分钟的转录语音数据实现。

众所周知，Transformers 在自然语言处理中的作用重大。Hugging Face transformers 的最新版本是 4.30，它包含了 Wav2Vec 2.0。这是 Transformers 中首个包含的自动语音识别模型。

模型架构超出了本博客的范围。有关详细的 Wav2Vec 模型架构，请查看 [这里](https://ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio/)。

让我们看看如何使用 Hugging Face transformers 通过一些简单的代码将音频文件转换为文本。

### 安装 Transformer 库

```py
# Installing Transformer

!pip install -q transformers
```

### 导入必要的库

```py
# Import necessary library

# For managing audio file
import librosa

#Importing Pytorch
import torch

#Importing Wav2Vec
from transformers import Wav2Vec2ForCTC, Wav2Vec2Tokenizer
```

Wav2Vec2 是一个语音模型，它接受一个浮点数组，该数组对应于语音信号的原始波形。Wav2Vec2 模型使用连接时序分类（CTC）进行训练，因此模型输出需要使用 Wav2Vec2Tokenizer 进行解码（[参考: Hugging Face](https://huggingface.co/transformers/model_doc/wav2vec2.html)）

### 阅读音频文件

在这个示例中，我使用了来自电影《Taken》的 Liam Neeson 著名对话音频片段，其中说 *“我会找你，我会找到你，我会杀了你”*

请注意，Wav2Vec 模型是在 16 kHz 频率下预训练的，因此我们需要确保我们的原始音频文件也被重采样为 16 kHz 采样率。我使用了在线 [音频工具转换](https://audio.online-convert.com/convert-to-wav) 将“Taken”音频片段重采样为 16kHz。

使用 librosa 库加载音频文件，并提到我的音频片段大小为 16000 Hz。它将音频片段转换为数组，并存储到‘audio’变量中。

```py
# Loading the audio file

audio, rate = librosa.load("taken_clip.wav", sr = 16000)
```

```py
# printing audio 
print(audio)

array([0., 0., 0., ..., 0., 0., 0.], dtype=float32)
```

```py
# printing rate
print(rate)

16000
```

### 导入预训练的 Wav2Vec 模型

```py
# Importing Wav2Vec pretrained model

tokenizer = Wav2Vec2Tokenizer.from_pretrained("facebook/wav2vec2-base-960h")
model = Wav2Vec2ForCTC.from_pretrained("facebook/wav2vec2-base-960h")
```

下一步是获取输入值，将音频（数组）传递给分词器，并希望我们的张量采用 PyTorch 格式而不是 Python 整数。return_tensors = “pt” 仅仅是 PyTorch 格式。

```py
# Taking an input value

input_values = tokenizer(audio, return_tensors = "pt").input_values
```

获取 logits 值（非标准化值）

```py
# Storing logits (non-normalized prediction values)
logits = model(input_values).logits
```

将 logits 值传递给 softmax 以获取预测值

```py
# Storing predicted ids
prediction = torch.argmax(logits, dim = -1)
```

### 将音频转换为文本

最后一步是将预测结果传递给分词器解码器以获取转录文本

```py
# Passing the prediction to the tokenzer decode to get the transcription
transcription = tokenizer.batch_decode(prediction)[0]
```

```py
# Printing the transcription
print(transcription)

'I WILL LOOK FOR YOU I WILL FIND YOU  AND I WILL KILL YOU'
```

它完全匹配我们的音频片段。

在这篇博客中，我们了解了如何使用 Transformers 中的预训练 Wav2Vec 模型将语音转换为文本。这对 NLP 项目尤其是处理音频转录数据非常有帮助。如果你有任何补充，请随时留言！

你可以在我的 [GitHub 仓库](https://github.com/sdhilip200/speech-to-text) 中找到完整的代码和数据。

感谢阅读。继续学习，敬请关注更多内容！

### 参考

1.  [`huggingface.co/transformers/model_doc/wav2vec2.html`](https://huggingface.co/transformers/model_doc/wav2vec2.html)

1.  [`ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio/`](https://ai.facebook.com/blog/wav2vec-20-learning-the-structure-of-speech-from-raw-audio/)

**简介：[Dhilip Subramanian](https://medium.com/@sdhilip)** 是一名机械工程师，已完成分析学硕士学位。他在数据相关的多个领域拥有 9 年的经验，包括 IT、市场营销、银行、电力和制造业。他对 NLP 和机器学习充满热情。他是[SAS 社区](https://communities.sas.com/t5/user/viewprofilepage/user-id/271305)的贡献者，喜欢在 Medium 平台上撰写有关数据科学各个方面的技术文章。

[原文](https://pub.towardsai.net/speech-to-text-with-wav2vec-2-0-b21c1e1ad701)。经许可转载。

**相关：**

+   使用 Python 进行简单的语音转文本

+   入门 5 个必备的自然语言处理库

+   Hugging Face Transformers 包 – 这是什么，如何使用

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [用 Python 在 5 分钟内构建文本转语音转换器](https://www.kdnuggets.com/2022/09/build-texttospeech-converter-python-5-minutes.html)

+   [语音识别指标的发展](https://www.kdnuggets.com/2022/10/evolution-speech-recognition-metrics.html)

+   [使用 BERT 对长文本进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [文本摘要的方法概述](https://www.kdnuggets.com/2019/01/approaches-text-summarization-overview.html)

+   [自动化文本摘要入门](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [什么是文本分类？](https://www.kdnuggets.com/2022/07/text-classification.html)
