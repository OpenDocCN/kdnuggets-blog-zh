# 在手机上进行深度学习：用于移动平台的 PyTorch C++ API

> 原文：[`www.kdnuggets.com/2021/11/deep-learning-mobile-phone-pytorch-c-api.html`](https://www.kdnuggets.com/2021/11/deep-learning-mobile-phone-pytorch-c-api.html)

评论

**作者：[Dhruv Matani](https://www.linkedin.com/in/dhruvbird/)，Meta（Facebook）的软件工程师**。

![](img/653abbe8932e54f81ab10230ca067d73.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 方面

* * *

[PyTorch](https://pytorch.org/) 是一个用于训练和运行机器学习（ML）模型的深度学习框架，加快了从研究到生产的速度。

[PyTorch C++ API](https://pytorch.org/cppdocs/) 可以用来编写紧凑的、对性能敏感的代码，具有深度学习能力，在移动平台上执行 ML 推理。有关如何将 PyTorch 模型部署到生产环境的一般介绍，请参见 [这篇文章](https://www.kdnuggets.com/2019/03/deploy-pytorch-model-production.html)。

[PyTorch Mobile](https://pytorch.org/mobile/home/) 当前支持在 [Android](https://pytorch.org/mobile/android/) 和 [iOS](https://pytorch.org/mobile/ios/) 平台上部署预训练模型进行推理。

### 高级步骤

1.  训练一个模型（或获取一个预训练的模型），并保存为 lite-interpreter 格式。PyTorch 模型的 lite-interpreter 格式使模型兼容于移动平台运行。本文未涵盖此步骤。

1.  下载 PyTorch 源代码，并 [从源代码构建 PyTorch](https://github.com/pytorch/pytorch#from-source)。这对于移动部署是推荐的。

1.  创建一个 .cpp 文件，包含加载模型、使用 forward() 运行推理并打印结果的代码。

1.  构建 .cpp 文件，并链接到 PyTorch 共享对象文件。

1.  运行文件并查看输出。

1.  获利！

### 详细步骤

**从源代码下载并安装 PyTorch 到 Linux 机器上。**

对于其他平台，请参见 [这个链接](https://github.com/pytorch/pytorch#from-source)。

```py
# Setup conda and install dependencies needed by PyTorch
conda install astunparse numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses

# Get the PyTorch source code from github
cd /home/$USER/
# Repo will be cloned into /home/$USER/pytorch/
git clone --recursive https://github.com/pytorch/pytorch
cd pytorch
# if you are updating an existing checkout
git submodule sync
git submodule update --init --recursive --jobs 0

# Build PyTorch
export CMAKE_PREFIX_PATH=${CONDA_PREFIX:-"$(dirname $(which conda))/../"}
python setup.py install

```

**创建源代码 .cpp 文件**

将 .cpp 文件存储在与您的 *AddTensorsModelOptimized.ptl* 文件相同的文件夹中。我们称这个 C++ 源文件为 *PTDemo.cpp*。我们称这个目录为 */home/$USER/ptdemo/*。

```py
#include <ATen/ATen.h>
#include <torch/library.h>
#include <torch/csrc/jit/mobile/import.h>
#include 

using namespace std;

int main() {
  // Load the PyTorch model in lite interpreter format.
  torch::jit::mobile::Module model = torch::jit::_load_for_mobile(
    "AddTensorsModelOptimized.ptl");

  std::vector inputs;

  // Create 2 float vectors with values for the 2 input tensors.
  std::vector v1 = {1.0, 5.0, -11.0};
  std::vector v2 = {10.0, -15.0, 9.0};

  // Create tensors efficiently from the float vector using the
  // from_blob() API.
  std::vector dims{static_cast(v1.size())};
  at::Tensor t1 = at::from_blob(
    v1.data(), at::IntArrayRef(dims), at::kFloat);
  at::Tensor t2 = at::from_blob(
    v2.data(), at::IntArrayRef(dims), at::kFloat);

  // Add tensors to inputs vector.
  inputs.push_back(t1);
  inputs.push_back(t2);

  // Run the model and capture results in 'ret'.
  c10::IValue ret = model.forward(inputs);

  // Print the return value.
  std::cout << "Return Value:\n" << ret << std::endl;

  // You can also convert the return value into a tensor and
  // fetch the underlying values using the data_ptr() method.
  float *data = ret.toTensor().data_ptr();
  const int numel = ret.toTensor().numel();

  // Print the data buffer.
  std::cout << "\nData Buffer: ";
  for (int i = 0; i < numel; ++i) {
    std::cout << data[i];
    if (i != numel - 1) {
      std::cout << ", ";
    }
  }
  std::cout << std::endl;
}

```

注意：我们使用来自*at::*命名空间的符号，而不是*torch::*命名空间（这是 PyTorch 网站教程中使用的），因为对于移动构建，我们不会包含完整的 jit（TorchScript）解释器。相反，我们将仅访问 lite-interpreter。

**构建和链接**

```py
# This is where your PTDemo.cpp file is
cd /home/$USER/ptdemo/

# Set LD_LIBRARY_PATH so that the runtime linker can
# find the .so files
LD_LIBRARY_PATH=/home/$USER/pytorch/build/lib/
export LD_LIBRARY_PATH

# Compile the PTDemo.cpp file. The command below should
# produce a file named 'a.out'
g++ PTDemo.cpp \
  -I/home/$USER/pytorch/torch/include/ \
  -L/home/$USER/pytorch/build/lib/ \
  -lc10 -ltorch_cpu -ltorch

```

**运行应用程序**

```py
 ./a.out

```

命令应输出以下内容：

```py
Return Value:
 11
 -5
  8
[ CPUFloatType{3} ]

Data Buffer: 11, -5, 8

```

### 结论

在本文中，我们看到如何使用跨平台的 C++ PyTorch API 加载一个经过训练的 lite-interpreter 模型，并使用该模型进行推理。所使用的 C++代码是平台无关的，可以在所有支持 PyTorch 的移动平台上构建和运行。

**参考文献**

+   [深度学习的 PyTorch 简介](https://www.kdnuggets.com/2018/11/introduction-pytorch-deep-learning.html)

+   [PyTorch 入门](https://www.kdnuggets.com/2020/10/getting-started-pytorch.html)

+   [将你的 PyTorch 模型部署到生产环境](https://www.kdnuggets.com/2019/03/deploy-pytorch-model-production.html)

+   [对 PyTorch 的贡献：一个对 PyTorch 了解不多的人](https://www.kdnuggets.com/2019/10/contributing-pytorch.html)

+   [PyTorch Mobile](https://pytorch.org/mobile/home/)

+   [PyTorch C++ API](https://pytorch.org/cppdocs/)

+   [从源代码构建 PyTorch](https://github.com/pytorch/pytorch#from-source)

+   [PyTorch iOS 101 视频教程](https://www.youtube.com/watch?v=GjsUWAaBIH8&ab_channel=PyTorch)

**简介：** [Dhruv Matani](https://www.linkedin.com/in/dhruvbird/) 是 Meta（Facebook）的软件工程师，他领导与 PyTorch（开源 AI 框架）相关的项目。他是 PyTorch 内部结构、PyTorch Mobile 的专家，并且是 PyTorch 的重要贡献者。他的工作影响着全球数十亿用户。他在为 Facebook 数据平台构建和扩展基础设施方面有着丰富的经验。值得注意的是，他对 Facebook 实时数据分析平台 Scuba 的贡献，该平台用于快速获取产品和系统洞察。他拥有石溪大学计算机科学硕士学位。

**相关：**

+   [机器学习在移动应用开发中的益处是什么？](https://www.kdnuggets.com/2021/09/machine-learning-beneficial-mobile-app-development.html)

+   [Facebook 开源新框架推动深度学习研究](https://www.kdnuggets.com/2020/11/facebook-open-source-frameworks-advance-deep-learning-research.html)

+   [PyTorch Lightning 入门](https://www.kdnuggets.com/2021/10/getting-started-pytorch-lightning.html)

### 更多相关主题

+   [免费 ChatGPT 课程：使用 OpenAI API 编写 5 个项目](https://www.kdnuggets.com/2023/05/free-chatgpt-course-openai-api-code-5-projects.html)

+   [如何免费访问和使用 Gemini API](https://www.kdnuggets.com/how-to-access-and-use-gemini-api-for-free)

+   [数据科学如何改变移动应用开发？](https://www.kdnuggets.com/2023/03/data-science-transform-mobile-app-development.html)

+   [人工智能将如何改变移动应用](https://www.kdnuggets.com/2022/12/artificial-intelligence-change-mobile-apps.html)

+   [深度学习库简介：PyTorch 和 Lightning AI](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)

+   [完整免费的 PyTorch 深度学习课程](https://www.kdnuggets.com/2022/10/complete-free-pytorch-course-deep-learning.html)
