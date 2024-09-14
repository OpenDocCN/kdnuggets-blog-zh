# AI 和机器学习的数据结构入门指南

> 原文：[`www.kdnuggets.com/guide-data-structures-ai-and-machine-learning`](https://www.kdnuggets.com/guide-data-structures-ai-and-machine-learning)

![AI 和机器学习的数据结构指南](img/54546439d60a71686a2119c44b07541a.png)

图片由作者创建

## 介绍

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

数据结构在某种意义上是算法的构建块，对于任何 AI 或 ML 算法的有效运行至关重要。这些结构，虽然常被视为数据的简单容器，但实际上它们是非常丰富的工具，并且对算法的性能、效率和总体计算复杂度的影响远比人们所认为的要大。因此，选择数据结构是一个需要深思熟虑的任务，它可以决定数据处理的速度、ML 模型的操作规模，甚至是特定计算问题的可行性。

本文将介绍一些在 AI 和 ML 领域中重要的数据结构，旨在服务于从业者、学生以及 AI 和 ML 爱好者。我们希望通过撰写这篇文章，提供关于 AI 和 ML 领域中重要数据结构的一些知识，并提供如何有效使用这些结构以发挥最佳优势的指南。

在逐一介绍这些数据结构时，将举例说明它们在 AI 和 ML 场景中的应用，每种结构都有其独特的优缺点。任何实现都会用 Python 给出，这是一种在数据科学领域中极受欢迎的语言，适用于 AI 和 ML 中的各种任务。掌握这些核心构建块对于数据科学家可能面临的各种任务至关重要：排序大型数据集、创建既快速又节省内存的高性能算法，以及以逻辑和高效的方式维护数据结构，仅举几例。

在从简单数组和动态数组的基础开始后，我们将继续讨论更高级的数据结构，如链表和二叉搜索树，最后介绍哈希表，这是一种非常有用的结构，并且学习投入的回报极高。我们将涵盖这些结构的机械生产，以及它们在 AI 和 ML 应用中的实际使用，这种理论与实践的结合为读者提供了决定哪种结构最适合特定问题的理解，并在强健的 AI 系统中实施这些结构。

在本文中，我们将深入探讨对 AI 和机器学习至关重要的各种数据结构，从数组和动态数组开始。通过了解每种数据结构的特点、优点和局限性，实践者可以做出明智的选择，从而提升其 AI 系统的效率和可扩展性。

## 1\. 数组与动态数组

数组可能是计算机科学中最基本的数据结构，它是存储在相邻内存位置的同一类型元素的集合，允许直接随机访问每个元素。动态数组，如 Python 中的列表，基于简单数组，但添加了自动调整大小的功能，当元素添加或删除时会分配额外的内存。这种自动内存分配能力是动态数组的核心。数组最佳使用的一些基本建议可能包括数据的线性遍历问题或元素数量完全不波动的情况，例如机器学习算法可能处理的不可变数据集。

让我们首先讨论优点：

+   通过索引轻松访问元素：快速检索操作，这在许多 AI 和 ML 场景中至关重要，因为时间效率是关键。

+   适用于已知或固定大小的问题：当元素数量是预先确定的或变化不频繁时效果最佳。

缺点：

+   固定大小（对于静态数组）：需要提前知道最大元素数量，这可能会有一定限制。

+   插入和删除成本高（对于静态数组）：每次插入或删除可能需要移动元素，这在计算上是昂贵的。

数组，可能因为它们易于理解和实用性，几乎无处不在计算机科学教育中；它们是自然的课堂主题。由于在从计算机内存位置访问随机元素时具有 O(1)的时间复杂度，使得它们在运行时效率至上的系统中受到青睐。

在 ML 的世界里，数组和动态数组对于处理数据集以及通常安排特征向量和矩阵至关重要。像 NumPy 这样的高性能数值库使用数组与高效执行跨数据集任务的例程相结合，允许快速处理和转换训练模型所需的数值数据并进行预测。

使用 Python 内置的动态数组数据结构列表执行的一些基本操作包括：

```py
# Initialization
my_list = [1, 2, 3]

# Indexing
print(my_list[0])        # output: 1

# Appending
my_list.append(4)        # my_list becomes [1, 2, 3, 4]

# Resizing
my_list.extend([5, 6])   # my_list becomes [1, 2, 3, 4, 5, 6]
```

## 2\. 链表

链表是另一种基本的数据结构，由一系列节点组成。列表中的每个节点都包含一些数据以及指向下一个节点的指针。单向链表是指列表中的每个节点只引用下一个节点，只允许向前遍历；而双向链表则引用下一个和前一个节点，能够进行前向和后向遍历。这使得链表在某些任务中比数组更具灵活性。

优点：

+   它们是：链表的动态扩展或收缩不会增加重新分配和移动整个结构的额外开销。

+   它们能够快速插入和删除节点，而不需要像数组那样进行额外的节点移动。

不利之处：

+   元素存储位置的不确定性会造成缓存效果差，尤其是与数组相比。

+   通过索引定位元素所需的线性或更差的访问时间，需要从头开始完全遍历，效率较低。

它们对于元素数量不明确且需要频繁插入或删除的结构尤其有用。这种应用使得它们在需要动态数据、频繁变化的情况下变得有用。的确，链表的动态调整能力是它们的强项之一；它们显然适用于那些元素数量无法提前准确预测并且可能产生大量浪费的情况。能够在没有大量复制或重写的主要开销的情况下调整链表结构是一项明显的好处，特别是当例行的数据结构调整可能是必需的。

尽管在 AI 和 ML 领域的实用性不如数组，但链表确实在需要高度可变数据结构和快速修改的特定应用中找到了用处，例如在遗传算法中管理数据池或其他需要对单个元素进行常规操作的情况。

我们来一个简单的 Python 链表操作实现吧？当然可以。请注意，以下基本链表实现包括一个 Node 类来表示每个列表元素，还有一个 LinkedList 类来处理列表上的操作，包括追加和删除节点。

```py
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def delete_node(self, key):
        temp = self.head
        if temp and temp.data == key:
            self.head = temp.next
            temp = None
            return
        prev = None
        while temp and temp.data != key:
            prev = temp
            temp = temp.next
        if temp is None:
            return
        prev.next = temp.next
        temp = None

    def print_list(self):
        current = self.head
        while current:
            print(current.data, end=' ')
            current = current.next
        print()
```

以下是上述代码的解释：

+   这个 LinkedList 类负责管理链表，包括创建、追加数据、删除节点和显示链表，并且在初始化时会创建头指针 head，默认标记为空链表。

+   append 方法将数据追加到链表的末尾，当链表为空时，会在链表头部创建一个新节点；否则，会遍历到非空链表的末尾添加新节点。

+   delete_node 方法通过考虑以下三种情况来删除具有给定键（数据）的节点：目标键在头节点中；目标键在链表的其他节点中；没有节点包含该键。

+   通过正确设置指针，能够在不破坏剩余节点顺序的情况下移除一个节点。

+   print_list 方法从头节点开始遍历链表，按顺序打印每个节点的内容，提供了一种简单的理解链表的方式。

这里是上述 LinkedList 代码的一个使用示例：

```py
# Create a new LinkedList
my_list = LinkedList()

# Append nodes with data
my_list.append(10)
my_list.append(20)
my_list.append(30)
my_list.append(40)
my_list.append(50)

# Print the current list
print("List after appending elements:")
my_list.print_list()       # outputs: 10 20 30 40 50

# Delete a node with data '30'
my_list.delete_node(30)

# Print the list after deletion
print("List after deleting the node with value 30:")
my_list.print_list()       # outputs: 10 20 40 50

# Append another node
my_list.append(60) 

# Print the final state of the list
print("Final list after appending 60:")
my_list.print_list()       # outputs: 10 20 40 50 60
```

## 3\. 树，特别是二叉搜索树（BST）

树是一种非线性数据结构（与数组比较），其中节点之间存在父子关系。每棵树都有一个根节点，节点可以包含零个或多个子节点，形成层次结构。二叉搜索树（BST）是一种树，其中每个节点最多可以包含两个子节点，通常称为左子节点和右子节点。在这种树中，节点中包含的键必须分别大于或等于其左子树中所有节点的键，或小于或等于其右子树中所有节点的键。这些 BST 的属性可以促进更高效的搜索、插入和删除操作，前提是树保持平衡。

BST 优点：

+   相对于更常用的数据结构，如数组或链表，BST 促进了更快速的访问、插入和删除操作。

BST 缺点：

+   然而，前面提到过，当 BST 不平衡或偏斜时，会导致性能下降。

+   这可能会导致操作的时间复杂度在最坏情况下降到 O(n)。

当需要对数据集进行大量搜索、插入或删除操作时，BST 特别有效。对于频繁访问的数据集，它们确实更为合适。

此外，树是一种理想的结构，用于以树状关系描述层次数据，比如文件系统或组织结构图。这使得它们在需要这种层次数据结构的应用中特别有用。

二叉搜索树（BST）能够确保搜索操作快速，因为它们的平均时间复杂度为 O(log n)，适用于访问、插入和删除操作。这使得它们在需要快速数据访问和更新的应用中尤其重要。

决策树，一种广泛用于机器学习分类和回归任务的树形数据结构，使模型能够基于由特征确定的规则预测目标变量。树在 AI 中也有广泛应用，例如游戏编程；尤其在策略游戏如国际象棋中，树用于模拟场景并确定决定最优移动的约束。

下面是如何使用 Python 实现基本 BST 的概述，包括插入、搜索和删除方法：

```py
class TreeNode:
    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

def insert(root, key):
    if root is None:
        return TreeNode(key)
    else:
        if root.val < key:
            root.right = insert(root.right, key)
        else:
            root.left = insert(root.left, key)
    return root

def search(root, key):
    if root is None or root.val == key:
        return root
    if root.val < key:
        return search(root.right, key)
    return search(root.left, key)

def deleteNode(root, key):
    if root is None:
        return root
    if key < root.val:
        root.left = deleteNode(root.left, key)
    elif(key > root.val):
        root.right = deleteNode(root.right, key)
    else:
        if root.left is None:
            temp = root.right
            root = None
            return temp
        elif root.right is None:
            temp = root.left
            root = None
            return temp
        temp = minValueNode(root.right)
        root.val = temp.val
        root.right = deleteNode(root.right, temp.val)
    return root

def minValueNode(node):
    current = node
    while current.left is not None:
        current = current.left
    return current
```

上述代码的解释：

+   二叉搜索树的基础是 TreeNode 类，它包含节点的值（val）以及其左子节点和右子节点的指针（left 和 right）。

+   插入函数是将值插入 BST 的递归策略的实现：在没有根节点的基本情况下创建一个新的 TreeNode，否则将比自身大的键放入右子树，将较小的节点放入左子树，保持 BST 的结构。

+   搜索函数处理没有找到指定值的节点和没有找到指定根的值的基本情况，然后根据键的值与当前节点比较，在正确的子树中递归搜索。

+   delete_node 方法可以分为三种情况：如没有子节点的键的删除调用（由右子节点替代）；没有右子节点的（由左子节点替代）；以及删除具有两个子节点的节点（由其“中序后继”，即右子树中的最小值替代），使递归节点删除并保持 BST 结构。

+   辅助函数是查找子树的最小值节点（即最左侧节点），它在删除具有两个子节点的节点时使用。

下面是上述 BST 代码实现的一个示例。

```py
# Create the root node with an initial value
root = TreeNode(50)

# Insert elements into the BST
insert(root, 30)
insert(root, 20)
insert(root, 40)
insert(root, 70)
insert(root, 60)
insert(root, 80)

# Search for a value
searched_node = search(root, 70)
if searched_node:
    print(f"Found node with value: {searched_node.val}")
else:
    print("Value not found in the BST.")

# output -> Found node with value: 70

# Delete a node with no children
root = deleteNode(root, 20)

# Attempt to search for the deleted node
searched_node = search(root, 20)
if searched_node:
    print(f"Found node with value: {searched_node.val}")
else:
    print("Value not found in the BST - it was deleted.")

# output -> Value not found in the BST - it was deleted.
```

## 4\. 哈希表

哈希表是一种适合快速数据访问的数据结构。它们利用哈希函数计算索引到一系列槽或桶中，从中返回所需的值。由于这些哈希函数，哈希表能够提供几乎即时的数据访问，并且可以扩展到大型数据集而不降低访问速度。哈希表的效率在很大程度上依赖于哈希函数，该函数将条目均匀地分布在桶数组中。这种分布有助于避免键冲突，即不同的键解析到相同的槽；正确的键冲突解决是哈希表实现的核心问题。

哈希表的优点：

+   快速数据检索：提供平均情况下常数时间复杂度（O(1)）用于查找、插入和删除。

+   平均时间复杂度效率：大多数情况下迅速，这使得哈希表适合一般实时数据处理。

哈希表的缺点：

+   最坏情况下时间复杂度不佳：如果有很多项哈希到相同的桶，则可能退化为 O(n)。

+   依赖于良好的哈希函数：哈希函数对哈希表性能的重要性显著，因为它直接影响数据在桶中的分布效果。

哈希表通常在需要快速查找、插入和删除操作时使用，而不需要有序的数据。当通过键快速访问项是必要的，以使操作更加迅速时，它们特别有用。哈希表在基本操作上的常数时间复杂度特性使它们在需要高性能操作时非常有用，尤其是在时间至关重要的情况下。

它们非常适合处理大量数据，因为它们提供了一种高速的数据查找方式，数据量增加时不会出现性能退化。AI 通常需要处理大量数据，在这种情况下，哈希表用于检索和查找非常有意义。

在机器学习中，哈希表帮助对大型数据集合进行特征索引——在预处理和模型训练中，通过哈希表促进了快速访问和数据操作。它们还可以使某些算法更高效——在某些情况下，在 k 最近邻计算中，它们可以存储已经计算的距离，并从哈希表中调用这些距离，以加快大数据集计算的速度。

在 Python 中，字典类型是哈希表的一种实现。如何使用 Python 字典以及处理冲突的策略将在下文中解释：

```py
# Creating a hash table using a dictionary
hash_table = {}

# Inserting items
hash_table['key1'] = 'value1'
hash_table['key2'] = 'value2'

# Handling collisions by chaining
if 'key1' in hash_table:
    if isinstance(hash_table['key1'], list):
        hash_table['key1'].append('new_value1')
    else:
        hash_table['key1'] = [hash_table['key1'], 'new_value1']
else:
    hash_table['key1'] = 'new_value1'

# Retrieving items
print(hash_table['key1'])

# output: can be 'value1' or a list of values in case of collision

# Deleting items
del hash_table['key2']
```

## 结论

对支撑 AI 和机器学习模型的一些数据结构进行调查，可以让我们了解这些相对简单的基础技术块的能力。数组的固有线性、链表的适应性、树的层次组织以及哈希表的 O(1)查找时间各自提供了不同的好处。这种理解可以帮助工程师更好地利用这些结构——不仅在他们组建的机器学习模型和训练集里，而且在他们选择和实施的推理过程中。

精通与机器学习和 AI 相关的基本数据结构是一项具有实际意义的技能。有很多地方可以学习这一技能，从大学到研讨会再到在线课程。即使是开源代码也可以是熟悉学科工具和最佳实践的重要资产。处理数据结构的实际能力不容忽视。因此，对今天、明天以及未来的数据科学家和 AI 工程师们：实践、实验，并从可用的数据结构材料中学习。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/)和[Statology](https://www.statology.org/)的执行编辑，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的贡献编辑，Matthew 致力于使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法和探索新兴 AI。他的使命是使数据科学社区的知识民主化。Matthew 从 6 岁起就开始编程。

### 更多相关话题

+   [OpenAI API 初学者指南：你的易于跟随的入门指南](https://www.kdnuggets.com/openai-api-for-beginners-your-easy-to-follow-starter-guide)

+   [超级学习指南：免费的算法与数据结构电子书](https://www.kdnuggets.com/2022/06/super-study-guide-free-algorithms-data-structures-ebook.html)

+   [Python 基础：语法、数据类型和控制结构](https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures)

+   [5 步开始学习 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [AI 和机器学习职业入门指南](https://www.kdnuggets.com/beginners-guide-to-careers-in-ai-and-machine-learning)

+   [如何像老板一样进行 MLOps：无泪机器学习指南](https://www.kdnuggets.com/2023/06/mlops-like-boss-guide-machine-learning-without-tears.html)
