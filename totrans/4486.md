# 使用 Biopython 进行冠状病毒 COVID-19 基因组分析

> 原文：[https://www.kdnuggets.com/2020/04/coronavirus-covid-19-genome-analysis-biopython.html](https://www.kdnuggets.com/2020/04/coronavirus-covid-19-genome-analysis-biopython.html)

[评论](#comments)![图示](../Images/0517607ccfc500033765e6a56834403d.png)

[图片来源](https://www.intelligentcio.com/eu/2020/03/19/qumulo-offers-free-software-to-help-medical-and-healthcare-research-organisations-fight-covid-19/)

新兴的全球传染病 COVID-19 由新型**严重急性呼吸综合症冠状病毒 2 型 (SARS-CoV-2)** 引起，自2019年12月底在中国首次发现以来，对全球公共卫生和经济带来了重大影响。

冠状病毒是一个庞大的病毒家族，能够引起范围广泛的疾病。第一个已知的由冠状病毒引起的严重疾病出现在 2003 年中国的严重急性呼吸综合症 (**SARS**) 疫情中。第二次严重疾病的爆发发生在 2012 年沙特阿拉伯的中东呼吸综合症 (**MERS**) 中。现在是 COVID-19 的持续爆发。

“通过比较已知冠状病毒株的可用基因组序列数据，我们可以确定 COVID-19 是通过自然过程起源的，”Scripps 研究所免疫学和微生物学副教授、论文通讯作者 Kristian Andersen 博士说。

因此，在这篇文章中，我们将解释和分析 COVID-19 DNA 序列数据，并尝试获取关于组成其蛋白质的尽可能多的见解。之后我们将把 COVID-19 DNA 与 MERS 和 SARS 进行比较，并理解它们之间的关系。

如果你对基因组学不熟悉，我建议你在继续之前查看 [**用机器学习和 Python 解密 DNA 测序**](https://medium.com/analytics-vidhya/demystify-dna-sequencing-with-machine-learning-and-python-bdbaeb177f56) 以获得 DNA 数据分析的基本理解。

![图示](../Images/b2664cb674434ed8fa593d5defc443fa.png)

[致命冠状病毒株的电子显微镜图像](https://www.fox5ny.com/news/electron-microscope-images-of-deadly-coronavirus-strain)

冠状病毒是包膜病毒家族的一员，这些病毒在动物宿主细胞的细胞质中复制。它们通过具有 5′ 端帽结构和 3′ 聚腺苷酸化链的单链正义 RNA 基因组（*(+)ssRNA 分类的病毒*），长度约为 30 kb 来识别。（已知最大病毒）。

现在，让我们使用 Python 来处理 COVID-19 DNA 序列数据。

首先，安装 Python 包如 [Biopython](https://biopython.org/) 和 [squiggle](https://squiggle.readthedocs.io/en/latest/index.html)，它们将帮助你在 Python 中处理生物序列数据。

```py
pip install biopython
pip install Squiggle
```

加载基本库：

```py
import numpy as np
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import os
```

数据集可以从 [Kaggle](https://www.kaggle.com/paultimothymooney/coronavirus-genome-sequence) 下载。

我们将使用 `Bio.SeqIO` 来解析 DNA 序列数据（fasta）。它提供了一个简单统一的接口来输入和输出各种序列文件格式。

```py
from Bio import SeqIOfor sequence in SeqIO.parse('/coronavirus/MN908947.fna', "fasta"):
print(sequence.seq)
print(len(sequence),'nucliotides')
```

所以它产生了序列及其长度。

```py
GCAATGGATACAACTAGCTACAGAGAAGCTGCTTGTTGTCATCTCGCAAAGGCTCTCAATGACTTCAGTAACTCAGGTTCTGATGTTCTTTACCAACCACCACAAACCTCTATCACCTCAGCTGTTTTGCAGAGTGGTTTTAGAAAAATGGCATTCCCATC......AGAATGACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA29903 nucliotides
```

将互补 DNA 序列加载到可比对的文件中

```py
from Bio.SeqRecord import SeqRecord
from Bio import SeqIO
DNAsequence = SeqIO.read('/coronavirus/MN908947.fna', "fasta")
```

`SeqIO.read()` 将提供有关序列的基本信息。

```py
SeqRecord(seq=Seq('ATTAAAGGTTTATACCTTCCCAGGTAACAAACCAACCAACTTTCGATCTCTTGT...AAA', SingleLetterAlphabet()), id='MN908947.3', name='MN908947.3', description='MN908947.3 Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome', dbxrefs=[])
```

![图](../Images/38f355316729e4273fa0b680adf9886e.png)

[DNA 与 RNA](https://scholarsark.com/question/i-need-insights-into-rna-vs-dna-comparison-functions-structure-reactivity-and-key-differences/)

由于输入序列是 FASTA（DNA），而冠状病毒是 RNA 类型的病毒，我们需要：

1.  将 DNA 转录为 RNA（ATTAAAGGTT… => AUUAAAGGUU…）

1.  将 RNA 翻译为氨基酸序列（AUUAAAGGUU… => IKGLYLPR*Q…）

在当前的场景中，.fna 文件以 ATTAAAGGTT 开始，然后我们调用 transcribe()，所以 T（胸腺嘧啶）被替换为 U（尿嘧啶），因此我们得到的 RNA 序列以 AUUAAAGGUU 开始。

```py
DNA = DNAsequence.seq#Convert DNA into mRNA Sequence
mRNA = DNA.transcribe() #Transcribe a DNA sequence into RNA.
print(mRNA)
print('Size : ',len(mRNA))
```

transcribe() 将 DNA 转换为 mRNA。

```py
UAUUUUAGUGGAGCAAUGGAUACAACUAGCUACAGAGAAGCUGCUUGUUGUCAUCUCGCAAAGGCUCUCAAUGACUUCAGUAACUCAGGUUC...UAAUAGCUUCUUAGGAGAAUGACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Size : 29903
```

DNA 和 mRNA 之间的区别在于 **碱基 T（胸腺嘧啶）被 U（尿嘧啶）取代。**

接下来，我们需要使用 translate() 方法将 mRNA 序列翻译为氨基酸序列，我们会得到类似 IKGLYLPR*Q（这是所谓的 STOP 密码子，实际上是蛋白质的分隔符）的东西。

```py
Amino_Acid = mRNA.translate(table=1, cds=False)
print('Amino Acid', Amino_Acid)
print("Length of Protein:",len(Amino_Acid))
print("Length of Original mRNA:",len(mRNA))
```

在下面的输出中，氨基酸由 * 分隔。

```py
Amino Acid :  IKGLYLPR*QTNQLSISCRSVL*TNFKICVAVTRLHA*CTHAV*LITNYCR*QDTSNSSIFCRLLTVSSVLQPIISTSRFRPGVTER*DGEPCPWFQRENTRPTQFACFTGSRRARTWLWRLRGGGLIRGTSTS*RWHLWLSRS*KRRFAST*TALCVHQTFGCSNCTSWSCYG...*SHIAIFNQCVTLGRT*KSHHIFTEATRSTIECTVNNARESCLYGRALMCKINFSSAIPM*F**LLRRMTKKKKKKKKKKLength of Protein :  9967 
Length of Original mRNA :  29903
```

在我们的场景中，序列看起来像这样：`IKGLYLPR*QTNQLSISCRSVL*TNFKICVAVTRLHA`，其中：

`IKGLYLPR` 编码第一个蛋白质（每个字母编码一个氨基酸），`QTNQLSISCRSVL` 编码第二个蛋白质，以此类推。

请注意，蛋白质中的序列少于 mRNA 中的序列，因为 3 个 mRNA 用于生成一个蛋白质的单体，即氨基酸，使用下面的密码子表。* 用于表示终止密码子，在这些区域蛋白质已完成其完整长度。这些密码子经常出现，导致蛋白质长度较短，通常这些蛋白质在生物学上作用不大，将在进一步分析中被排除。

**好的，首先了解什么是遗传密码和 DNA 密码子？**

**遗传密码** 是由活细胞用来翻译编码在遗传物质（DNA 或 mRNA 的核苷酸三联体或 **密码子**）中的信息成蛋白质的一组规则。

标准遗传密码表通常表示为 RNA 密码子表，因为当细胞中的核糖体制造蛋白质时，是 mRNA 指导蛋白质合成。mRNA 序列由基因组 DNA 的序列决定。以下是一些密码子的特征：

1.  大多数密码子指定一个氨基酸。

1.  三个 “终止” 密码子标志着蛋白质的结束。

1.  一个 “起始” 密码子 AUG 标志着蛋白质的开始，同时编码氨基酸甲硫氨酸。

![图](../Images/99e3dc6ccf3e8d6e088daf8fca37fdf1.png)

一系列密码子组成了信使RNA（mRNA）分子的一部分。每个密码子由三个核苷酸组成，通常对应一个单独的[氨基酸](https://en.wikipedia.org/wiki/Amino_acid)。这些核苷酸用字母A、U、G和C表示。这是mRNA，使用U（[尿嘧啶](https://en.wikipedia.org/wiki/Uracil)）。DNA则使用T（[胸腺嘧啶](https://en.wikipedia.org/wiki/Thymine)）。这个mRNA分子将指示一个[核糖体](https://en.wikipedia.org/wiki/Ribosome)根据这个代码合成蛋白质。[来源](https://en.wikipedia.org/wiki/Genetic_code)

```py
from Bio.Data import CodonTable
print(CodonTable.unambiguous_rna_by_name['Standard'])
```

![图](../Images/d42d2afb58614ac0bb973f3616a55215.png)

RNA密码子表

现在让我们识别所有蛋白质（氨基酸链），基本上在终止密码子（由*标记）处进行分离。然后，移除任何少于20个氨基酸的序列，因为这是已知的最小功能性蛋白质（如果感兴趣）。

```py
#Identify all the Proteins (chains of amino acids)
Proteins = Amino_Acid.split('*') # * is translated stop codon
df = pd.DataFrame(Proteins)
df.describe()
print('Total proteins:', len(df))def conv(item):
    return len(item)def to_str(item):
    return str(item)df['sequence_str'] = df[0].apply(to_str)
df['length'] = df[0].apply(conv)
df.rename(columns={0: "sequence"}, inplace=True)
df.head()# Take only longer than 20
functional_proteins = df.loc[df['length'] >= 20]
print('Total functional proteins:', len(functional_proteins))
functional_proteins.describe()
```

![图](../Images/05a555b4c986909eea33743e5eaa219a.png)

使用ProtParam的Biopython中的Protparam模块进行蛋白质分析。

**ProtParam中的可用工具：**

+   `count_amino_acids`：简单地计算氨基酸在蛋白质序列中重复的次数。

+   `get_amino_acids_percent`：与之前的方法相同，只是返回整个序列的百分比数量。

+   `molecular_weight`：计算蛋白质的分子量。

+   `aromaticity`：根据Lobry & Gautier (1994, [Nucleic Acids Res., 22, 3174-3180](https://dx.doi.org/10.1093/nar/22.15.3174))计算蛋白质的芳香性值。

+   `flexibility`：实现了Vihinen *et al.* (1994, [Proteins, 19, 141-149](https://dx.doi.org/10.1002/prot.340190207))的柔性方法。

+   `isoelectric_point`：此方法使用模块`IsoelectricPoint`来计算蛋白质的pI值。

+   `secondary_structure_fraction`：此方法返回一组氨基酸在螺旋、转角或片段中的比例。

+   螺旋中的氨基酸：V、I、Y、F、W、L。

+   转角中的氨基酸：N、P、G、S。

+   表中的氨基酸：E、M、A、L。

列表包含3个值：[螺旋、转角、片段]。

```py
from __future__ import division
poi_list = []
from Bio.SeqUtils import ProtParam
for record in Proteins[:]:
    print("\n")
    X = ProtParam.ProteinAnalysis(str(record))
    POI = X.count_amino_acids()
    poi_list.append(POI)
    MW = X.molecular_weight()
    MW_list.append(MW)
    print("Protein of Interest = ", POI)
    print("Amino acids percent =    ",str(X.get_amino_acids_percent()))
    print("Molecular weight = ", MW_list)
    print("Aromaticity = ", X.aromaticity())
    print("Flexibility = ", X.flexibility())
    print("Isoelectric point = ", X.isoelectric_point())
    print("Secondary structure fraction = ",   X.secondary_structure_fraction())
```

![](../Images/d45a889fee4a9c1639246bb4b8c1c956.png)

绘制结果：

```py
MoW = pd.DataFrame(data = MW_list,columns = ["Molecular Weights"] )#plot POI
poi_list = poi_list[48]
plt.figure(figsize=(10,6));
plt.bar(poi_list.keys(), list(poi_list.values()), align='center')
```

![](../Images/fa047c3645dfe9ae1e1a43cbe750d54c.png)

看起来这个蛋白质中的`[**赖氨酸(L)**](https://www.sciencedirect.com/topics/medicine-and-dentistry/lysine)`和`[**缬氨酸(V)**](https://pubchem.ncbi.nlm.nih.gov/compound/Valine)`数量较多，这表明有较多的α-螺旋。

![图](../Images/1b311994dcdc8974529bca7e1e575c69.png)

[来源](https://alevelnotes.com/notes/biology/biological-molecules/biological-molecules/protein-structure)

现在让我们比较COVID-19/COV2、MERS和SARS之间的相似性。

加载SARS、MERS和COVID-19的DNA序列文件（FASTA）。

```py
#Comparing Human Coronavirus RNA
from Bio import pairwise2SARS = SeqIO.read("/coronavirus/sars.fasta", "fasta")MERS = SeqIO.read("/coronavirus/mers.fasta", "fasta")COV2 = SeqIO.read("/coronavirus/cov2.fasta", "fasta")
```

#序列长度：SARS：29751，COV2：29903，MERS：30119

在比较相似性之前，让我们分别可视化COV2、SARS和MERS的DNA。

```py
#Execute on terminal
Squiggle cov2.fasta sars.fasta mers.fasta --method=gates --separate
```

![图](../Images/83b8c95bdaf06dcd959fe614bc8e3e57.png)

分别对 COV2、SARS 和 MERS 的 DNA 进行可视化。

我们可以观察到 COV2 和 SARS 的 DNA 结构几乎相同，而 MERS 的 DNA 结构与前两者有所不同。

现在让我们使用序列比对技术来比较所有 DNA 序列的相似性。

**序列比对**是将两个或多个序列（DNA、RNA 或蛋白质序列）按特定顺序排列的过程，以识别它们之间的相似区域。

识别相似区域使我们能够推断大量信息，如物种之间的保守性状、不同物种的遗传关系、物种的进化等。

**两序列比对**一次只比较两个序列，并提供最佳可能的序列比对。**两序列比对**易于理解，并且从结果序列比对中推断出的信息非常出色。

Biopython 提供了一个特殊的模块，**Bio.pairwise2**，用于使用两序列比对方法识别比对序列。Biopython 应用了最佳算法来找到比对序列，其表现与其他软件相当。

```py
# Alignments using pairwise2 alghoritmSARS_COV = pairwise2.align.globalxx(SARS.seq, COV2.seq, one_alignment_only=True, score_only=True)print('SARS/COV Similarity (%):', SARS_COV / len(SARS.seq) * 100)MERS_COV = pairwise2.align.globalxx(MERS.seq, COV2.seq, one_alignment_only=True, score_only=True)print('MERS/COV Similarity (%):', MERS_COV / len(MERS.seq) * 100)MERS_SARS = pairwise2.align.globalxx(MERS.seq, SARS.seq, one_alignment_only=True, score_only=True)print('MERS/SARS Similarity (%):', MERS_SARS / len(SARS.seq) * 100)
```

![图](../Images/c04b818d9356c0ba308ca47c3d3e7cc2.png)

比较结果

完整病毒基因组（29,903 个核苷酸）的系统发育分析显示，COVID-19 病毒与一组[SARS 类冠状病毒](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5287300/)（贝塔冠状病毒属，萨尔贝科病毒亚属）关系最为密切（83.3% 核苷酸相似性），这些病毒之前在中国的蝙蝠中被发现。

绘制结果：

```py
# Plot the data
X = ['SARS/COV2', 'MERS/COV2', 'MERS/SARS']
Y = [SARS_COV/ len(SARS.seq) * 100, MERS_COV/ len(MERS.seq)*100, MERS_SARS/len(SARS.seq)*100]
plt.title('Sequence identity (%)')
plt.bar(X,Y)
```

![](../Images/d27ca543c40462d662b24e788f0e163b.png)

[你可以在这个 GitHub 仓库中获取代码](https://github.com/nageshsinghc4/COVID-19-coronavirus)。

### 结论

我们已经看到了如何解读和分析 COVID-19 DNA 序列数据，并尝试获取尽可能多的关于构成其蛋白质的见解。在我们的结果中，我们发现该蛋白质中的`[**亮氨酸(L**](https://www.sciencedirect.com/topics/medicine-and-dentistry/lysine)**)**`和`[**缬氨酸(V)**](https://pubchem.ncbi.nlm.nih.gov/compound/Valine)`的数量较高，这表明该蛋白质中有较多的α-螺旋。

后来我们将 COVID-19 的 DNA 与 MERS 和 SARS 进行了比较。我们发现 COVID-19 与 SARS 关系密切。

这篇文章的内容就是这些。我希望你们喜欢阅读这篇文章，请在评论区分享你的建议/观点/问题。

### 感谢阅读!!!

**个人简介: [纳格什·辛格·乔汉](https://www.linkedin.com/in/nagesh-singh-chauhan-6936bb13b/)** 是一位数据科学爱好者，关注大数据、Python 和机器学习。

[原文](https://medium.com/analytics-vidhya/coronavirus-covid-19-genome-analysis-using-biopython-8b8cb1f4a041)。经许可转载。

**相关：**

+   [通过机器学习探索生物信息学的世界](/2019/09/explore-world-bioinformatics-machine-learning.html)

+   [人工智能如何帮助管理传染病](/2020/04/ai-manage-infectious-diseases.html)

+   [数据科学家如何训练和更新模型以应对 COVID-19 复苏](/2020/04/data-scientists-train-models-covid-19-recovery.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

### 更多相关主题

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一笔 90 亿美元的 AI 失败，剖析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
