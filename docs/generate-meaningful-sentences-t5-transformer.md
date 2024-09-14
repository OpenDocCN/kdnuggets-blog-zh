# 如何使用 T5 Transformer 生成有意义的句子

> 原文：[`www.kdnuggets.com/2021/05/generate-meaningful-sentences-t5-transformer.html`](https://www.kdnuggets.com/2021/05/generate-meaningful-sentences-t5-transformer.html)

评论

**由 [Vatsal Saglani](https://www.linkedin.com/in/vatsalsaglani) 编写，Quinnox 的机器学习工程师**

![](img/ebd7ecb3538f515a84f574de22bf1de9.png)

由 [Tech Daily](https://unsplash.com/@techdailyca?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/streaming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄（[Tech Daily](https://techdaily.ca/)）

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

在博客中，[**使用 T5 Transformer 生成故事情节**](https://pub.towardsai.net/generating-cool-storylines-using-a-t5-transformer-and-having-fun-4a79f6ab8adb)我们看到如何通过提供诸如类型、导演、演员和种族等输入来微调 Sequence2Sequence (Text-To-Text) Transformer (T5) 以生成故事情节/情节。在这篇博客中，我们将检查如何使用经过训练的 T5 模型进行推断。之后，我们还将看到如何使用`gunicorn`和`flask`进行部署。

### 如何进行模型推断？

+   *让我们设置带有导入项的脚本*

```py
import os
import re
import random
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from tqdm import tqdm_notebook, tnrange
from sklearn.utils import shuffle
import pickle
import math
import torch
import torch.nn.functional as F
from transformers import T5Tokenizer, T5ForConditionalGeneration
```

+   *设置`*SEED*`值并加载模型和分词器*

```py
torch.manual_seed(3007)model = T5ForConditionalGeneration.from_pretrained('./outputs/model_files')
tokenizer = T5Tokenizer.from_pretrained('./outputs/model_files')
```

+   *使用`*model.generate*`函数生成序列*

```py
text = "generate plot for genre: horror"
input_ids = tokenizer.encode(text, return_tensors="pt")
greedyOp = model.generate(input_ids, max_length=100)
tokenizer.decode(greedyOp[0], skip_special_tokens=True)
```

*注意：阅读 [*这篇*](https://huggingface.co/blog/how-to-generate) 令人惊叹的 Hugging Face 博客，了解如何使用不同的解码策略进行文本生成*

+   *让我们将其放入函数中*

```py
def generateStoryLine(text, seq_len, seq_num):
				'''
				args:
					text: input text eg. generate plot for: {genre} or generate plot for: {director}
					seq_len: Max sequence length for the generated text
					seq_num: Number of sequences to generate
				'''
        outputDict = dict()
        outputDict["plots"] = {}
        input_ids = tokenizer.encode(text, return_tensors = "pt")
        beamOp = model.generate(
            input_ids,
            max_length = seq_len,
            do_sample = True,
            top_k = 100,
            top_p = 0.95,
            num_return_sequences = seq_num
        )        for ix, sample_op in enumerate(beamOp):
            outputDict["plots"][ix] = self.tokenizer.decode(sample_op, skip_special_tokens = True)

        return outputDict
```

### 如何用 Flask 部署这个？

用户可以通过多种方式提供输入，模型可能需要生成情节。用户可以仅提供类型，或者可以提供类型和演员，甚至可以提供所有四项，即类型、导演、演员和种族。但为了实现这个功能，我要求至少提供一个类型。

你可以查看下面的链接，了解 API 将如何工作。

[**电影情节生成器**](https://movie-plot-generator.vercel.app/)

我在网上生成模糊的电影情节（但有时它们还不错）。但我可以向你保证，它总是会是……](https://movie-plot-generator.vercel.app/)

*让我们开发一个后端来实现上述链接中使用的 API 调用*

### 安装需求

```py
pip install flask flask_cors tqdm rich gunicorn
```

### 创建一个 app.py 文件

+   *导入项*

```py
# app.pyfrom flask import Flask, request, jsonify
import json
from flask_cors import CORS
import uuidfrom predict import PredictionModelObjectapp = Flask(__name__)
CORS(app)print("Loading Model Object")
predictionObject = PredictionModelObject()
print("Loaded Model Object")
```

+   *添加 API 路由*

```py
@app.route('/api/generatePlot', methods=['POST'])
def gen_plot():    req = request.get_json()
    genre = req['genre']
    director = req['director'] if 'director' in req else None
    cast = req['cast'] if 'cast' in req else None
    ethnicity = req['ethnicity'] if 'ethnicity' in req else None
    num_plots = req['num_plots'] if 'num_plots' in req else 1
    seq_len = req['seq_len'] if 'seq_len' in req else 200    if not isinstance(num_plots, int) or not isinstance(seq_len, int):
        return jsonify({
            "message": "Number of words in plot and Number of plots must be integers",
            "status": "Fail"
        })

    try:
        plot, status = predictionObject.returnPlot(
            genre = genre, 
            director = director,
            cast = cast,
            ethnicity = ethnicity,
            seq_len = seq_len,
            seq_num = num_plots
        )        if status == 'Pass':

            plot["message"] = "Success!"
            plot["status"] = "Pass"
            return jsonify(plot)

        else:            return jsonify({"message": plot, "status": status})

    except Exception as e:        return jsonify({"message": "Error getting plot for the given input", "status": "Fail"})
```

+   *运行*`*flask*`*应用的主块*

```py
if __name__ == "__main__":
    app.run(debug=True, port = 5000)
```

*这个脚本还不能使用。你在执行脚本时可能会收到 ImportError，因为我们还没有创建带有*`*PredictionModelObject*`*的*`*predict.py*`*脚本

### 创建`PredictionModelObject`

+   *创建一个*`*predict.py*`*文件并导入以下内容*

```py
# predict.py
import os
import re
import random
import torch
import torch.nn as nn
from rich.console import Console
from transformers import T5Tokenizer, T5ForConditionalGeneration
from collections import defaultdictconsole = Console(record = True)torch.cuda.manual_seed(3007)
torch.manual_seed(3007)
```

+   *创建*`*PredictionModelObject*`*类*

```py
# predict.py
class PredictionModelObject(object):    def __init__(self):console.log("Model Loading")
        self.model = T5ForConditionalGeneration.from_pretrained('./outputs/model_files')
        self.tokenizer = T5Tokenizer.from_pretrained('./outputs/model_files')
        console.log("Model Loaded")

    def beamSearch(self, text, seq_len, seq_num):        outputDict = dict()
        outputDict["plots"] = {}
        input_ids = self.tokenizer.encode(text, return_tensors = "pt")
        beamOp = self.model.generate(
            input_ids,
            max_length = seq_len,
            do_sample = True,
            top_k = 100,
            top_p = 0.95,
            num_return_sequences = seq_num
        )        for ix, sample_op in enumerate(beamOp):
            outputDict["plots"][ix] = self.tokenizer.decode(sample_op, skip_special_tokens = True)

        return outputDict    def genreToPlot(self, genre, seq_len, seq_num):        text = f"generate plot for genre: {genre}"        return self.beamSearch(text, seq_len, seq_num)    def genreDirectorToPlot(self, genre, director, seq_len, seq_num):        text = f"generate plot for genre: {genre} and director: {director}"

        return self.beamSearch(text, seq_len, seq_num)    def genreDirectorCastToPlot(self, genre, director, cast, seq_len, seq_num):        text = f"generate plot for genre: {genre} director: {director} cast: {cast}"        return self.beamSearch(text, seq_len, seq_num)    def genreDirectorCastEthnicityToPlot(self, genre, director, cast, ethnicity, seq_len, seq_num):        text = f"generate plot for genre: {genre} director: {director} cast: {cast} and ethnicity: {ethnicity}"        return self.beamSearch(text, seq_len, seq_num)

    def genreCastToPlot(self, genre, cast, seq_len, seq_num):        text = f"genreate plot for genre: {genre} and cast: {cast}"        return self.beamSearch(text, seq_len, seq_num)    def genreEthnicityToPlot(self, genre, ethnicity, seq_len, seq_num):        text = f"generate plot for genre: {genre} and ethnicity: {ethnicity}"        return self.beamSearch(text, seq_len, seq_num)    def returnPlot(self, genre, director, cast, ethnicity, seq_len, seq_num):
        console.log('Got genre: ', genre, 'director: ', director, 'cast: ', cast, 'seq_len: ', seq_len, 'seq_num: ', seq_num, 'ethnicity: ',ethnicity)

        seq_len = 200 if not seq_len else int(seq_len)

        seq_num = 1 if not seq_num else int(seq_num)

        if not director and not cast and not ethnicity:            return self.genreToPlot(genre, seq_len, seq_num), "Pass"

        elif genre and director and not cast and not ethnicity:            return self.genreDirectorToPlot(genre, director, seq_len, seq_num), "Pass"        elif genre and director and cast and not ethnicity:            return self.genreDirectorCastToPlot(genre, director, cast, seq_len, seq_num), "Pass"        elif genre and director and cast and ethnicity:            return self.genreDirectorCastEthnicityToPlot(genre, director, cast, ethnicity, seq_len, seq_num), "Pass"        elif genre and cast and not director and not ethnicity:            return self.genreCastToPlot(genre, cast, seq_len, seq_num), "Pass"

        elif genre and ethnicity and not director and not cast:            return self.genreEthnicityToPlot(genre, ethnicity, seq_len, seq_num), "Pass"        else:            return "Genre cannot be empty", "Fail"
```

保存`predict.py`文件，然后使用以下命令以调试模式运行`app.py`文件，

```py
python app.py
```

### 测试你的 API

+   *创建一个*`*test_api.py*`*文件并执行*

```py
# test_api.py
import requests
import osurl = "<http://localhost:5000/api/generatePlot>"
json = {
    "genre": str(input("Genre: ")),
    "director": str(input("Director: ")),
    "cast": str(input("Cast: ")),
    "ethnicity": str(input("Ethnicity: ")),
    "num_plots": int(input("Num Plots: ")),
    "seq_len": int(input("Sequence Length: ")),
}r = requests.post(url, json = json)
print(r.json())
```

### 如何使用`gunicorn`运行？

使用`gunicorn`和`flask`非常简单。在开始时安装要求时，我们已经安装了`gunicorn`命令，现在需要通过终端进入包含`app.py`文件的文件夹，并运行以下命令

```py
gunicorn -k gthread -w 2 -t 40000 --threads 3 -b:5000 app:app
```

我们上面使用的格式和标志表示如下

+   *k: 类型（工作线程的类型）- *`*gthread*`*，*`*gevent*`*等...*

+   *w: 工作线程的数量*

+   *t: 超时*

+   *threads: 每个工作线程的数量*

+   *b: 绑定端口号*

*如果你的文件名是*`*server.py*`*或*`*flask_app.py*`*，则*`*app:app*`*部分将更改为*`*server:app*`*或*`*flask_app:app*`

### 总结

在这篇博客中，我们介绍了如何使用之前训练好的 T5 变换器生成故事情节，并使用`flask`和`gunicorn`进行部署。此博客旨在易于跟随，以免你浪费时间在不同平台上查找问题。希望你阅读和实施时感到愉快。

**简介: [Vatsal Saglani](https://www.linkedin.com/in/vatsalsaglani)** ([@saglanivatsal](https://twitter.com/saglanivatsal)) 是 Quinnox 的机器学习工程师。

[原文](https://pub.towardsai.net/how-to-generate-meaningful-sentences-using-a-t5-transformer-b755bee64882)。转载已获许可。

**相关:**

+   Hugging Face Transformers 包 – 它是什么以及如何使用它

+   使用 Huggingface 和 PyTorch Lightning 的多语言 CLIP

+   GPT-2 与 GPT-3: OpenAI 对决

### 更多相关话题

+   [使用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [使用稳定扩散生成超现实面孔的 3 种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [将数据管理与数据讲述结合以生成价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)
