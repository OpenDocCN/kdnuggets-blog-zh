# 使用Dask和PyTorch进行大规模计算机视觉

> 原文：[https://www.kdnuggets.com/2020/11/computer-vision-scale-dask-pytorch.html](https://www.kdnuggets.com/2020/11/computer-vision-scale-dask-pytorch.html)

[评论](#comments)

**由 [Stephanie Kirmer](https://www.linkedin.com/in/skirmer/)，Saturn Cloud的高级数据科学家提供**

将深度学习策略应用于计算机视觉问题为数据科学家打开了无限可能。然而，要在大规模上使用这些技术创造商业价值，需要大量的计算资源——而这正是Saturn Cloud旨在解决的挑战！

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT领域支持你的组织

* * *

在本教程中，你将看到使用流行的Resnet50深度学习模型在Saturn Cloud上的NVIDIA GPU集群进行图像分类推断的步骤。利用Saturn Cloud提供的资源，我们可以将任务运行速度提高40倍，相比非并行化方法更快！

![图示](../Images/bbb23403f8c85487a730973cc0647e36.png)

今天我们将对狗的图像进行分类！

### 你将在这里学到的内容：

+   如何在Saturn Cloud上设置和管理GPU集群以进行深度学习推断任务

+   如何在GPU集群上使用Pytorch运行推断任务

+   如何使用批处理加速在GPU集群上进行Pytorch推断任务

### 设置

首先，我们需要确保我们的图像数据集可用，并且我们的GPU集群正在运行。

在我们的案例中，我们将数据存储在S3上，并使用[`s3fs`](https://s3fs.readthedocs.io/en/latest/)库进行处理，如下所示。

如果你想使用相同的数据集，它是斯坦福狗数据集，可以在这里获取：[http://vision.stanford.edu/aditya86/ImageNetDogs/](http://vision.stanford.edu/aditya86/ImageNetDogs/)

要设置我们的Saturn GPU集群，过程非常简单。

```py
import dask_saturn
from dask_saturn import SaturnCluster

cluster = SaturnCluster(n_workers=4, scheduler_size='g4dnxlarge', worker_size='g4dn8xlarge')
client = Client(cluster)
client
```

```py
[2020-10-15 18:52:56] INFO – dask-saturn | Cluster is ready
```

我们没有明确说明，但我们在集群节点上使用32个线程，总共128个线程。

**提示：个别用户可能需要调整线程数量，如果文件非常大，可以减少线程数量——同时运行大量任务的线程可能会需要比你的工作节点一次性可用的内存更多。**

这一步可能需要一些时间，因为我们请求的所有 AWS 实例都需要启动。调用`client`，它将监控启动过程，并在一切准备好时通知你！

### GPU 能力

目前，我们可以确认我们的集群具有 GPU 能力，并确保我们已经正确设置了一切。

首先，检查 Jupyter 实例是否具有 GPU 能力。

```py
torch.cuda.is_available() 

```

```py
True
```

太棒了——现在让我们检查一下我们的四个工作节点。

```py
client.run(lambda: torch.cuda.is_available())
```

```py
{‘tcp://10.0.24.217:45281’: True,
‘tcp://10.0.28.232:36099’: True,
‘tcp://10.0.3.136:40143’: True,
‘tcp://10.0.3.239:40585’: True}
```

在这里，我们将设置“设备”为始终使用 CUDA，以便我们可以使用这些 GPU。

```py
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

**注意：如果你需要帮助来了解如何运行单张图像分类，我们的 GitHub 上有一个 [扩展代码笔记本](https://github.com/saturncloud/saturn-cloud-examples/tree/main/pytorch-demo)，可以为你提供这些指令以及剩下的内容。**

### 推断

现在，我们准备开始进行一些分类！我们将使用一些自定义编写的函数来高效完成这项任务，并确保我们的工作能够充分利用 GPU 集群的并行化优势。

### 预处理

### 单图像处理

```py
@dask.delayed
def preprocess(path, fs=__builtins__):
    '''Ingest images directly from S3, apply transformations,
    and extract the ground truth and image identifier. Accepts
    a filepath. '''

    transform = transforms.Compose([
        transforms.Resize(256), 
        transforms.CenterCrop(250), 
        transforms.ToTensor()])

    with fs.open(path, 'rb') as f:
        img = Image.open(f).convert("RGB")
        nvis = transform(img)

    truth = re.search('dogs/Images/n[0-9]+-([^/]+)/n[0-9]+_[0-9]+.jpg', path).group(1)
    name = re.search('dogs/Images/n[0-9]+-[a-zA-Z-_]+/(n[0-9]+_[0-9]+).jpg', path).group(1)

    return [name, nvis, truth]
```

这个函数允许我们处理一张图像，但当然，我们有很多图像需要处理！我们将使用一些列表推导策略来创建我们的批次，并为推断做好准备。

首先，我们将从 S3 文件路径获取的图像列表分解成定义批次的块。

```py
3fpath = 's3://dask-datasets/dogs/Images/*/*.jpg'

batch_breaks = [list(batch) for batch in toolz.partition_all(60, s3.glob(s3fpath))]
```

然后我们将每个文件处理成嵌套列表。接着我们会稍微重新格式化这个列表设置，这样我们就准备好了！

```py
image_batches = [[preprocess(x, fs=s3) for x in y] for y in batch_breaks]
```

请注意，我们在所有这些操作中都使用了 Dask 的`delayed`装饰器——我们不希望它现在就实际运行，而是等待我们在 GPU 集群上并行处理时再运行！

### 格式化批次

这一步只是为了确保图像批次按模型所期望的方式组织好。

```py
@dask.delayed
def reformat(batch):
    flat_list = [item for item in batch]
    tensors = [x[1] for x in flat_list]
    names = [x[0] for x in flat_list]
    labels = [x[2] for x in flat_list]
    return [names, tensors, labels]

image_batches = [reformat(result) for result in image_batches]
```

### 运行模型

现在我们准备进行推断任务了！这将有几个步骤，所有这些步骤都包含在下面描述的函数中，但我们会逐一讲解，以确保一切清楚。

在这一点上，我们的工作单位是一次 60 张图像的批次，我们在上面的部分中创建了这些批次。它们都被整齐地安排在列表中，以便我们可以有效地处理它们。

我们需要对列表做的一件事是“堆叠”张量。我们本可以在处理早些时候做这件事，但由于我们在预处理中使用了 Dask 的`delayed`装饰器，我们的函数实际上直到处理后期才知道它们正在接收张量。因此，我们也将“堆叠”操作延迟到预处理之后的这个函数中。

```py
@dask.delayed
def run_batch_to_s3(iteritem):
    ''' Accepts iterable result of preprocessing, 
    generates inferences and evaluates. '''

    with s3.open('s3://dask-datasets/dogs/imagenet1000_clsidx_to_labels.txt') as f:
        classes = [line.strip() for line in f.readlines()]

    names, images, truelabels = iteritem

    images = torch.stack(images)
... 
```

现在我们已经将张量堆叠起来，以便可以将批次传递给模型。我们将使用相当简单的语法来检索我们的模型：

```py
...
    resnet = models.resnet50(pretrained=True)
    resnet = resnet.to(device)
    resnet.eval()
...
```

方便的是，我们加载了库 `torchvision`，其中包含了几个有用的预训练模型和数据集。这就是我们从中获取 Resnet50 的地方。调用 `.to(device)` 方法允许我们将模型对象传递给我们的工作者，使他们能够进行推理而无需回到客户端。

现在我们准备运行推理了！它在同一个函数中，以这种方式呈现：

```py
...
    images = images.to(device)
    pred_batch = resnet(images)
...
```

我们将图像堆栈（即我们正在处理的批次）传递给工作者，然后运行推理，返回该批次的预测结果。

### 结果评估

然而，我们目前的预测和实际情况并不真正易读或可比，因此我们将使用接下来的函数来修正它们，得到可解释的结果。

```py
def evaluate_pred_batch(batch, gtruth, classes):
    ''' Accepts batch of images, returns human readable predictions. '''
    _, indices = torch.sort(batch, descending=True)
    percentage = torch.nn.functional.softmax(batch, dim=1)[0] * 100

    preds = []
    labslist = []
    for i in range(len(batch)):
        pred = [(classes[idx], percentage[idx].item()) for idx in indices[i][:1]]
        preds.append(pred)

        labs = gtruth[i]
        labslist.append(labs)

    return(preds, labslist)
```

这将我们的模型结果和一些其他元素整合在一起，返回易读的预测和模型分配的概率。

```py
preds, labslist = evaluate_pred_batch(pred_batch, truelabels, classes)
```

从这里开始，我们就快完成了！我们希望以整洁、易读的方式将结果传回 S3，因此其余的函数处理了这个过程。它将遍历每张图片，因为这些功能不支持批量处理。`is_match` 是我们自定义的函数之一，你可以在下面查看。

```py
...
    for j in range(0, len(images)):
        predicted = preds[j]
        groundtruth = labslist[j]
        name = names[j]
        match = is_match(groundtruth, predicted)

        outcome = {'name': name, 'ground_truth': groundtruth, 'prediction': predicted, 'evaluation': match}

        # Write each result to S3 directly
        with s3.open(f"s3://dask-datasets/dogs/preds/{name}.pkl", "wb") as f:
            pickle.dump(outcome, f)
...
```

### 将所有内容整合在一起

现在，我们不会手动拼接所有这些函数，而是将它们组装成一个单一的延迟函数，完成我们的工作。重要的是，我们可以将其映射到集群中所有的图像批次上！

```py
def evaluate_pred_batch(batch, gtruth, classes):
    ''' Accepts batch of images, returns human readable predictions. '''
    _, indices = torch.sort(batch, descending=True)
    percentage = torch.nn.functional.softmax(batch, dim=1)[0] * 100

    preds = []
    labslist = []
    for i in range(len(batch)):
        pred = [(classes[idx], percentage[idx].item()) for idx in indices[i][:1]]
        preds.append(pred)

        labs = gtruth[i]
        labslist.append(labs)

    return(preds, labslist)

def is_match(la, ev):
    ''' Evaluate human readable prediction against ground truth. 
    (Used in both methods)'''
    if re.search(la.replace('_', ' '), str(ev).replace('_', ' ')):
        match = True
    else:
        match = False
    return(match)    

@dask.delayed
def run_batch_to_s3(iteritem):
    ''' Accepts iterable result of preprocessing, 
    generates inferences and evaluates. '''

    with s3.open('s3://dask-datasets/dogs/imagenet1000_clsidx_to_labels.txt') as f:
        classes = [line.strip() for line in f.readlines()]

    names, images, truelabels = iteritem

    images = torch.stack(images)

    with torch.no_grad():
        # Set up model
        resnet = models.resnet50(pretrained=True)
        resnet = resnet.to(device)
        resnet.eval()

        # run model on batch
        images = images.to(device)
        pred_batch = resnet(images)

        #Evaluate batch
        preds, labslist = evaluate_pred_batch(pred_batch, truelabels, classes)

        #Organize prediction results
        for j in range(0, len(images)):
            predicted = preds[j]
            groundtruth = labslist[j]
            name = names[j]
            match = is_match(groundtruth, predicted)

            outcome = {'name': name, 'ground_truth': groundtruth, 'prediction': predicted, 'evaluation': match}

            # Write each result to S3 directly
            with s3.open(f"s3://dask-datasets/dogs/preds/{name}.pkl", "wb") as f:
                pickle.dump(outcome, f)

        return(names)
```

### 在集群上

我们已经完成了所有艰苦的工作，可以让我们的函数从这里开始。我们将使用 `.map` 方法高效地分配任务。

```py
futures = client.map(run_batch_to_s3, image_batches) 
futures_gathered = client.gather(futures)
futures_computed = client.compute(futures_gathered, sync=False)
```

使用 `map` 我们确保所有批次都会应用该函数。使用 `gather`，我们可以同时收集所有结果，而不是逐个收集。使用 `compute(sync=False)`，我们返回所有的期货，准备在我们需要时进行计算。这可能看起来很费劲，但这些步骤是允许我们迭代未来所必需的。

现在我们实际运行任务，并且还设有一个简单的错误处理系统，以防我们的文件出现问题或其他意外情况。

```py
import logging

results = []
errors = []
for fut in futures:
    try:
        result = fut.result()
    except Exception as e:
        errors.append(e)
        logging.error(e)
    else:
        results.extend(result)
```

### 评估

我们当然希望从这个模型中得到高质量的结果！首先，我们可以查看一个单独的结果。

```py
with s3.open('s3://dask-datasets/dogs/preds/n02086240_1082.pkl', 'rb') as data:
    old_list = pickle.load(data)
    old_list
```

```py
{‘name’: ‘n02086240_1082’,
‘ground_truth’: ‘Shih-Tzu’,
‘prediction’: [(b”203: ‘West Highland white terrier’,”, 3.0289587812148966e-05)],
‘evaluation’: False}
```

虽然这里有一个错误的预测，但我们得到了预期的结果！为了进行更彻底的检查，我们会下载所有结果文件，然后查看有多少个 `evaluation: True`。

检查的狗照片数量：20580

正确分类的狗的数量：13806

正确分类的狗的百分比：67.085%

不完美，但总体上结果还是很不错的！

### 性能比较

因此，我们已经在大约 5 分钟内对超过 20,000 张图像进行了分类。这听起来不错，但还有什么替代方案呢？

![计算机视觉的规模化：使用 Dask 和 PyTorch](../Images/120e727c28f37e3e011ffef3a5fc3b8d.png)

| 技术 | 运行时间 |
| --- | --- |
| 无集群批处理 | 3 小时 21 分钟 13 秒 |
| **GPU 集群与批处理** | **5 分钟 15 秒** |

添加 GPU 集群能带来巨大的不同！如果你想亲自体验这个效果，[今天就注册获取 Saturn Cloud 的免费试用吧！](https://www.saturncloud.io/s/tryhosted/?utm_source=KDNuggets%20blog%3A%20Pytorch%20on%20a%20GPU%20cluster&utm_medium=Try%20Hosted)

**简历：[Stephanie Kirmer](https://www.linkedin.com/in/skirmer/)** 是 Saturn Cloud 的高级数据科学家。

[原文](https://www.saturncloud.io/s/computer-vision-at-scale-with-dask-and-pytorch/)。转载自原作者许可。

**相关链接：**

+   [云计算中的数据科学与 Dask](/2020/10/data-science-cloud-dask.html)

+   [深度学习、自然语言处理和计算机视觉的顶级 Python 库](/2020/11/top-python-libraries-deep-learning-natural-language-processing-computer-vision.html)

+   [如何获得最受欢迎的数据科学技能](/2020/11/acquire-most-wanted-data-science-skills.html)

### 更多相关内容

+   [数据管理的 6 件你需要知道的事以及它为何重要…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [TensorFlow 在计算机视觉中的应用 - 轻松实现迁移学习](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍 MLM 的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的 5 个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [DINOv2：Meta AI 提供的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：在 5 分钟内构建机器学习网页应用…](https://www.kdnuggets.com/2022/n10.html)
