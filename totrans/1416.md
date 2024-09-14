# 处理数据科学中的树结构算法面试

> 原文：[https://www.kdnuggets.com/2020/01/handling-trees-data-science-algorithmic-interview.html](https://www.kdnuggets.com/2020/01/handling-trees-data-science-algorithmic-interview.html)

[评论](#comments)

算法和数据结构是数据科学的重要组成部分。虽然我们大多数数据科学家在学习过程中没有修过专门的算法课程，但它们依然至关重要。

许多公司在招聘数据科学家时会将数据结构和算法作为面试的一部分。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

现在许多人会问，为什么要问数据科学家这些问题。***我喜欢将其描述为数据结构问题可以被视为编码能力测试。***

*我们都在生活中的不同阶段做过能力测试，虽然这些测试并不是完美的判断标准，但几乎没有什么是完美的。所以，为什么不使用标准算法测试来评估一个人的编码能力呢？*

但不要自欺欺人，它们的难度与数据科学面试的难度相当，因此，你可能需要花些时间来学习算法和数据结构问题。

***这篇文章旨在加速学习过程，并为数据科学家解释树结构的概念，以便下次在面试中被问到时能够轻松应对。***

### 但首先，为什么树对数据科学如此重要？

对数据科学家来说，树的意义与对软件工程师来说的不同。

对于软件工程师而言，树只是一个简单的数据结构，用于管理层次关系，而对于数据科学家来说，树是一些最有用的分类和回归算法的基础。

那么这两者在哪里相遇呢？

它们实际上是同一个东西。不要感到惊讶。以下是数据科学家和软件工程师如何看待树结构的。

![图示](../Images/0d6ea80e7c5eea532c15d9487cfa0855.png)

它们本质上是相同的。

唯一的区别在于，数据科学树节点包含了更多信息，这有助于我们确定如何遍历树。例如，在预测的数据科学树中，我们会查看节点中的特征，并根据分裂值确定我们要沿着哪个方向移动。

如果你想从头开始编写决策树，你可能还需要了解树在软件工程中的工作原理。

### 树的类型：

在这篇文章中，我将只讨论两种在数据科学面试问题中经常出现的树。二叉树（BT）和二叉树的扩展，称为二叉搜索树（BST）。

### 1\. 二叉树：

二叉树仅仅是每个节点最多有两个子节点的树。决策树是我们日常生活中看到的一个例子。

![图示](../Images/c6f04dde60176a1efe6b381cc205829f.png)

二叉树：每个节点最多有2个子节点

### 2\. 二叉搜索树（BST）：

二叉搜索树是二叉树的一种，其中：

+   节点的所有左侧后代都小于或等于该节点，且

+   节点的所有右侧后代都大于该节点。

关于等式，这一定义有一些变体。有时等式出现在右侧或任一侧。有时树中只允许不同的值。

![图示](../Images/27b84aad70fd2e4011f509de90c91a5a.png)

[来源](https://www.freecodecamp.org/news/data-structures-101-binary-search-tree-398267b6bff0/)

8大于左子树中的所有元素，并小于右子树中的所有元素。树中的任何节点也可以这样说。

### 创建一个简单的树：

那么我们如何构造一个简单的树呢？

根据定义，树由节点组成。因此我们从定义`node`类开始，该类用于创建节点。我们的节点类相当简单，它保存节点的值、左子节点的位置和右子节点的位置。

```py
class node:
    def __init__(self,val):
        self.val = val
        self.left = None
        self.right = None
```

现在我们可以创建一个简单的树，如下：

```py
root = node(1)
root.left = node(2)
root.right = node(3)
```

现在我注意到，我们没有通过一些实际编程就很难掌握基于树的问题。

***那么让我们更深入地探讨一下编码部分，特别是我在处理树时发现的最有趣的一些问题。***

### 中序树遍历：

遍历树有多种方法，但我发现中序遍历是最直观的。

当我们对二叉搜索树的根节点进行中序遍历时，它会按升序访问/打印节点。

```py
def inorder(node):
    if node:
        inorder(node.left)
        print(node.val)
        inorder(node.right)
```

上述方法非常重要，因为它允许我们访问所有节点。

如果我们想在任何二叉树中搜索一个节点，我们可能会尝试使用中序树遍历。

### 从排序数组创建二叉搜索树

如果我们需要像上面那样一块一块手动创建树，我们会成为什么样的程序员呢？

***那么我们可以从一个排序的唯一元素数组中创建一个二叉搜索树吗？***

```py
def create_bst(array,min_index,max_index):
    if max_index<min_index:
        return None
    mid = int((min_index+max_index)/2)
    root = node(array[mid])
    leftbst = create_bst(array,min_index,mid-1)
    rightbst = create_bst(array,mid+1,max_index)
    root.left = leftbst
    root.right = rightbst
    return root
a = [2,4,5,6,7]
root = create_bst(a,0,len(a)-1)
```

树本质上是递归的，因此我们在这里使用递归。我们取数组的中间元素并将其作为节点。然后我们将`create_bst`函数应用于数组的左部分并将其分配给`node.left`，右部分也做相同的操作。

然后我们就得到了我们的二叉搜索树。

我们做对了吗？我们可以通过创建二叉搜索树然后进行中序遍历来检查。

```py
inorder(root)
------------------------------------------------------------
2
4
5
6
7
```

看起来对！

### 让我们检查一下我们的树是否是有效的BST

![图](../Images/380b0d1a2fed9cfe40b8371b528cdea1.png)

思考递归!!!

不过，如果我们需要打印所有元素并手动检查是否满足BST属性，那我们算什么样的编码者呢？

这里是一个简单的代码，用于检查我们的BST是否有效。我们假设我们的二叉搜索树中严格不等。

```py

def isValidBST(node, minval, maxval):
    if node:
        # Base case
        if node.val<=minval or node.val>=maxval:
            return False
        # Check the subtrees changing the min and max values
        return isValidBST(node.left,minval,node.val) &   isValidBST(node.right,node.val,maxval)
    return True
isValidBST(root,-float('inf'),float('inf'))
--------------------------------------------------------------
True
```

我们递归地检查子树是否满足二叉搜索树的性质。在每次递归调用时，我们更改`minval`或`maxval`，以为函数提供子树的允许值范围。

### 结论

***在这篇文章中，我从软件工程的角度讨论了树。如果你想从数据科学的角度了解树，可以看看这篇文章。***

[**三种决策树分裂标准背后的简单数学**](https://towardsdatascience.com/the-simple-math-behind-3-decision-tree-splitting-criterions-85d4de2a75fe)

理解分裂标准

树是数据科学算法面试中最常见问题的基础。我曾经对这些树形问题感到绝望，但现在我喜欢其中的思维锻炼。我喜欢这些问题中涉及的递归结构。

虽然你可以在数据科学中不学习它们，但你可以为了乐趣和提升编程技能而学习它们。

这是一个小的[notebook](https://www.kaggle.com/mlwhiz/tree-data-structure-and-algorithms)，我在里面放置了所有这些小概念供你尝试和运行。

如果你想了解[递归](https://towardsdatascience.com/three-programming-concepts-for-data-scientists-c264fc3b1de8)、[动态规划](https://towardsdatascience.com/dynamic-programming-for-data-scientists-bb7154b4298b)或[链表](https://towardsdatascience.com/a-simple-introduction-of-linked-lists-for-data-scientists-a71f0eb31d87)，可以查看我其他关于[算法面试系列](https://towardsdatascience.com/tagged/algorithms-interview)的帖子。

### 继续学习

如果你想了解更多关于算法和数据结构的内容，这里有一个[**UCSanDiego的Coursera算法专项**](https://click.linksynergy.com/deeplink?id=lVarvwc5BD0&mid=40328&murl=https%3A%2F%2Fwww.coursera.org%2Fspecializations%2Fdata-structures-algorithms)**，**我强烈推荐。

感谢阅读。我将来还会写更多适合初学者的帖子。关注我在 [**Medium**](https://medium.com/@rahul_agarwal?source=post_page---------------------------) 或订阅我的 [**博客**](http://eepurl.com/dbQnuX?source=post_page---------------------------)，以便了解最新内容。欢迎反馈和建设性批评，可以在Twitter上找到我 [@mlwhiz](https://twitter.com/MLWhiz?source=post_page---------------------------)。

此外，小声明：本文中可能包含一些相关资源的附属链接，分享知识永远是好事。

**简介：[Rahul Agarwal](https://www.linkedin.com/in/rahulagwl/)** 是WalmartLabs的高级统计分析师。关注他的Twitter [@mlwhiz](https://twitter.com/MLWhiz)。

[原文](https://towardsdatascience.com/handling-trees-in-data-science-algorithmic-interview-ea14dd1b6236)。经授权转载。

**相关：**

+   [数据科学家6条建议](/2019/09/advice-data-scientists.html)

+   [Kaggle的优质特征构建技巧和窍门](/2018/12/feature-building-techniques-tricks-kaggle.html)

+   [特征提取的搭车指南](/2019/06/hitchhikers-guide-feature-extraction.html)

### 更多相关内容

+   [人工智能在算法交易中的应用如何影响…](https://www.kdnuggets.com/2022/04/adoption-ai-algorithmic-trading-affected-finance-industry.html)

+   [使用SQL处理时间序列中的缺失值](https://www.kdnuggets.com/2022/09/handling-missing-values-timeseries-sql.html)

+   [从零开始学习机器学习：决策树](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)

+   [决策树与随机森林的解释](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [揭开决策树在现实世界中的面纱](https://www.kdnuggets.com/demystifying-decision-trees-for-the-real-world)

+   [广义和可扩展的最优稀疏决策树(GOSDT)](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)*
