# 再回首… RAPIDS 故事

> 原文：[`www.kdnuggets.com/2023/06/back-again-rapids-tale.html`](https://www.kdnuggets.com/2023/06/back-again-rapids-tale.html)

由[Kris Manohar](https://www.linkedin.com/in/kris-manohar-phd-4b9117a/)和[Kevin Baboolal](https://www.linkedin.com/in/kevin-baboolal-b3313595/)撰写

![再回首… RAPIDS 故事](img/0aaf39d85cdb9016557725d139bcb00e.png)

图片由编辑提供

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

> **编辑备注：** 我们很高兴宣布这篇文章被选为 KDnuggets & NVIDIA 博客写作比赛的获奖者。

# 介绍

机器学习通过利用大量数据彻底改变了各个领域。然而，在数据获取因成本或稀缺而变得困难的情况下，传统方法往往难以提供准确的预测。本文探讨了小数据集带来的限制，并揭示了 TTLAB 提出的一种创新解决方案，该方案利用了最近邻方法和专门的核函数。我们将深入探讨他们的算法、其优势以及 GPU 优化如何加速其执行。

# 有限数据的挑战

在机器学习中，拥有大量数据对于训练准确的模型至关重要。然而，当面临只有几百行的小数据集时，其不足之处变得显而易见。一个常见的问题是某些分类算法（如朴素贝叶斯分类器）中遇到的零频率问题。这发生在算法在测试期间遇到未见过的类别值时，导致该案例的概率估计为零。类似地，回归任务在测试集包含训练集中不存在的值时也面临挑战。你可能会发现，当这些缺失特征被排除时，你选择的算法虽然次优但有所改进。这些问题在具有高度不平衡类别的大型数据集中也会显现。

# 克服数据稀缺

尽管训练-测试拆分通常能缓解这些问题，但在处理较小数据集时仍然存在隐性问题。强迫算法根据较少的样本进行泛化可能会导致次优预测。即使算法能够运行，其预测也可能缺乏稳健性和准确性。由于成本或可用性限制，获取更多数据的简单解决方案并不总是可行。在这种情况下，TTLAB 提出的创新方法被证明是稳健且准确的。

# TTLAB 算法

TTLAB 的算法应对了偏倚和有限数据集带来的挑战。他们的方法包括对训练数据集中的所有行进行加权平均，以预测测试样本中目标变量的值。关键在于根据参数化的非线性函数调整每个训练行的权重，这个函数计算特征空间中两个点之间的距离。虽然使用的加权函数有一个单一参数（训练样本与测试样本距离增加时影响衰减率），但优化这个参数的计算工作量可能很大。通过考虑整个训练数据集，该算法提供了稳健的预测。这种方法在提高随机森林和朴素贝叶斯等流行模型的性能方面取得了显著成功。随着算法的普及，正在努力进一步提升其效率。目前的实现涉及调整超参数 kappa，这需要网格搜索。为了加快这一过程，正在探索连续二次近似法，这有望实现更快的参数优化。此外，正在进行的同行评审旨在验证和完善该算法，以便更广泛地采用。

实现 TTLAG 算法时，使用循环和 numpy 进行分类证明效率低下，导致运行时间非常长。展示在链接出版物中的 CPU 实现专注于分类问题，展示了方法的多功能性和有效性。 [`arxiv.org/pdf/2205.14779.pdf`](https://arxiv.org/pdf/2205.14779.pdf)。出版物还显示该算法从向量化中获益良多，暗示了通过使用 CuPy 进行 GPU 加速可以获得进一步的速度提升。事实上，对超参数调优和随机 K 折交叉验证的执行在测试的多个数据集上需要几周时间。通过利用 GPU 的强大计算能力，计算任务得到了有效分配，从而提升了性能。

# 使用 GPU 加速执行

即使有了像矢量化和 `.apply` 重构这样的优化，执行时间对于实际应用仍然不切实际。然而，通过 GPU 优化，运行时间显著减少，将执行时间从小时缩短到分钟。这种显著的加速打开了在需要快速结果的场景中使用算法的可能性。

根据从 CPU 实现中获得的经验教训，我们尝试进一步优化我们的实现。为此，我们将层级上移至 CuDF 数据框。将计算矢量化到 GPU 上对于 CuDF 来说是轻而易举的。对我们来说，就像将 `import pandas` 改为 `import CuDF` 一样简单（你必须在 pandas 中正确进行矢量化。）

```py
train_df["sum_diffs"] = 0
train_df["sum_diffs"] = train_df[diff_cols].sum(axis=1).values
train_df["d"] = train_df["sum_diffs"] ** 0.5
train_df["frac"] = 1 / (1 + train_df["d"]) ** kappa
train_df["part"] = train_df[target_col] * train_df["frac"]
test_df.loc[index, "pred"] = train_df["part"].sum() / train_df["frac"].sum()
```

在我们的探索中，我们需要依赖 NumPy 内核。这时，事情变得棘手。回顾一下为什么算法的预测如此可靠，因为每个预测都使用了训练数据框中的所有行。然而，NumPy 内核不支持传递 CuDF 数据框。目前，我们正在尝试一些在 Github 上建议的技巧来处理这种情况。 ([`github.com/rapidsai/cudf/issues/13375`](https://github.com/rapidsai/cudf/issues/13375))

现在，我们至少可以通过 `.apply_rows` 将原始计算传递给 NumPy 内核

```py
def predict_kernel(F, T, numer, denom, kappa):
    for i, (x, t) in enumerate(zip(F, T)):
        d = abs(x - t)  # the distance measure
        w = 1 / pow(d, kappa)  # parameterize non-linear scaling
        numer[i] = w
        denom[i] = d

_tdf = train_df[[att, target_col]].apply_rows(
    predict_kernel,
    incols={att: "F", "G3": "T"},
    outcols={"numer": np.float64, "denom": np.float64},
    kwargs={"kappa": kappa},
)

p = _tdf["numer"].sum() / _tdf["denom"].sum()  # prediction - weighted average
```

目前，我们没有消除所有的 for 循环，但将大部分计算推送到 NumPy 内核使 CuDF 运行时间减少了超过 50%，标准的 80-20 训练测试拆分的时间约为 2 到 4 秒。

# 总结

探索 RAPIDS、CuPy 和 CuDF 库在各种机器学习任务中的能力是一次令人振奋和愉快的旅程。这些库证明了它们用户友好且易于理解，使大多数用户都能轻松使用。库的设计和维护值得称赞，使得用户在必要时可以深入了解其复杂性。在一周的时间里，每天只需几个小时，我们就能够从新手进展到通过实现一个高度定制的预测算法来推动库的边界。我们的下一个目标是实现前所未有的速度，力争突破微秒级别的障碍，处理从 20K 到 30K 的大数据集。一旦达成这一里程碑，我们计划将该算法作为一个由 RAPIDS 提供支持的 pip 包发布，以便更广泛地采用和使用。

**[Kris Manohar](https://www.linkedin.com/in/kris-manohar-phd-4b9117a/)** 是 ICPC, Trinidad 和 Tobago 的执行董事。

### 更多相关信息

+   [RAPIDS cuDF 速查表](https://www.kdnuggets.com/2023/05/cudf-data-science-cheat-sheet.html)

+   [使用 RAPIDS cuDF 利用 GPU 进行特征工程](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [RAPIDS cuDF 提升您的下一个数据科学工作流速度](https://www.kdnuggets.com/2023/04/rapids-cudf-speed-next-data-science-workflow.html)

+   [RAPIDS cuDF 在 Google Colab 上加速数据科学](https://www.kdnuggets.com/2023/01/rapids-cudf-accelerated-data-science-google-colab.html)

+   [是否有方法弥合 MLOps 工具的差距？](https://www.kdnuggets.com/2022/08/way-bridge-mlops-tools-gap.html)

+   [R 与 Python（再次）：从人因角度的观点](https://www.kdnuggets.com/2022/01/r-python-human-factor-perspective.html)
