# 使用 PuLP 进行线性规划和离散优化

> 原文：[https://www.kdnuggets.com/2019/05/linear-programming-discrete-optimization-python-pulp.html/2](https://www.kdnuggets.com/2019/05/linear-programming-discrete-optimization-python-pulp.html/2)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2019/05/linear-programming-discrete-optimization-python-pulp.html?page=2#comments)

### 解决问题并打印解决方案

PuLP 提供了[多种求解器算法](https://pythonhosted.org/PuLP/solvers.html)（例如 COIN_MP、Gurobi、CPLEX 等）。对于这个问题，我们不指定任何选择，而是让程序根据问题结构默认选择。

```py
prob.solve()

```

我们可以打印**解决方案状态**。注意，虽然在这种情况下状态是*最优*的，但不一定如此。如果问题表述不清或信息不足，解决方案可能是*不可行*或*无界*的。

```py
# The status of the solution is printed to the screen
print("Status:", LpStatus[prob.status])

>> Status: Optimal

```

完整的解决方案包含所有变量，包括权重为零的变量。但对我们来说，**只有那些具有非零系数**的变量才是有趣的，即应该包含在最优饮食计划中的变量。因此，我们可以扫描问题变量，并仅在变量数量为正时打印出来。

```py
for v in prob.variables():
    if v.varValue>0:
        print(v.name, "=", v.varValue)

>> Food_Frozen_Broccoli = 6.9242113
   Food_Scrambled_Eggs = 6.060891
   Food__Baked_Potatoes = 1.0806324

```

所以，最优解决方案是吃 6.923 份冷冻西兰花、6.06 份炒鸡蛋和 1.08 份烤土豆！

![figure-name](../Images/8c51d2b4285256094b43f79f0c1255ab.png)

欢迎下载整个笔记本、数据文件，并尝试各种约束来更改您的饮食计划。[代码在我的 GitHub 仓库中](https://github.com/tirthajyoti/Optimization-Python)。

最后，我们可以打印**目标函数即饮食成本**，

```py
obj = value(prob.objective)
print("The total cost of this balanced diet is: ${}".format(round(obj,2)))

```

```py
>> The total cost of this balanced diet is: $5.52

```

### 如果我们希望得到一个整数解怎么办？

正如我们所看到的，最优结果返回了一组食物项的分数。这个结果可能不够实际，我们可能希望解决方案强制只有整数份量。

这引出了[**整数规划**](https://en.wikipedia.org/wiki/Integer_programming)技术。之前优化中使用的算法是简单的线性规划，其中变量可以取任何实数值。**整数规划强制某些或所有变量只能取整数值。**

实际上，整数规划是一个比线性规划更[难解的计算问题](https://stackoverflow.com/questions/51084738/what-is-the-run-time-complexity-of-integer-linear-programming-ilp)。整数变量使优化问题变得[非凸](https://www.solver.com/convex-optimization)，因此更难以解决。随着整数变量的增加，内存和解决时间可能会呈指数级增长。

幸运的是，PuLP 也可以解决具有这种限制的优化问题。

代码几乎与之前相同，因此这里没有重复。唯一的区别是变量被定义为属于`**Integer**`类别，而不是`**Continuous**`。

```py
food_integer = LpVariable.dicts("Food",food_items,0,cat='Integer')

```

对于这个问题，它稍微改变了最优解，添加了冰山生菜到饮食中，并将成本增加了$0.06。你还会注意到**计算时间的显著增加**。

```py
Therefore, the optimal balanced diet with whole servings consists of
--------------------------------------------------------------------
Food_Frozen_Broccoli = 7.0
Food_Raw_Lettuce_Iceberg = 1.0
Food_Scrambled_Eggs = 6.0
Food__Baked_Potatoes = 1.0

```

整数规划的一个酷应用是解决[**司机调度问题**](https://en.wikipedia.org/wiki/Driver_scheduling_problem)，这可能是一个[NP难题](https://en.wikipedia.org/wiki/NP-hardness)问题。请参阅这篇文章（也注意文章中他们如何计算各种操作的成本并将其用于优化问题），

[**工程师的整数规划入门：简化的公交调度**

*这篇文章是Remix关于我们面临的软件工程问题系列的一部分。在这一篇中，Remix…*blog.remix.com](https://blog.remix.com/an-intro-to-integer-programming-for-engineers-simplified-bus-scheduling-bd3d64895e92)

### 如何在一个线性规划问题中纳入二进制决策？

通常，我们希望在优化问题中包含某种*“如果-那么-否则”*的决策逻辑。

> 如果我们不想在饮食中同时包含西兰花和冰山生菜（但只包含其中之一是可以的），该怎么办？

我们如何在这个框架中表示这样的决策逻辑？

![figure-name](../Images/0ff0686336587e9951a2deb8ee9d14e4.png)

结果是，对于这种逻辑，你需要引入另一种类型的变量，称为**指示变量**。它们是二进制的，可以指示最优解中变量的存在或缺失。

[**在LP问题中将指示函数作为约束**

*感谢你为Mathematics Stack Exchange贡献了答案！请务必回答这个问题。提供详细信息…*math.stackexchange.com](https://math.stackexchange.com/questions/2220355/involving-indicator-function-as-a-constraint-in-a-lp-problem)

但对于这个特定的问题，使用指示变量存在一个明显的问题。理想情况下，如果指示变量为1，你希望将食物项的成本/营养价值包含在约束方程中；如果为0，则忽略它。从数学上讲，将其写为**原始项（涉及食物项）与指示变量的乘积**是直观的。但一旦你这样做，你就是在乘以两个变量，使问题变得非线性！在这种情况下，它属于[**二次规划**](https://en.wikipedia.org/wiki/Quadratic_programming)（QP）的范畴（*二次*因为项现在是两个线性项的乘积）。

> 流行的机器学习技术支持向量机本质上解决了一个二次规划问题。

[**什么是二次规划的直观解释？在SVM中如何定义它？**

*答案：二次规划涉及最小化一个在未知向量组件中是二次形式的目标函数……* [www.quora.com](https://www.quora.com/What-is-an-intuitive-explanation-of-quadratic-programming-and-how-is-it-defined-in-SVM)

然而，使用指示变量在线性规划问题中表达二进制逻辑的这一一般概念也极为有用。我们在下一部分给出了一个使用这一技巧解决数独谜题的问题链接。

结果发现，有一个巧妙的技巧可以将这种二进制逻辑融入到这个线性规划中，而不会使其变成一个二次规划问题。

我们可以将二进制变量表示为 `food_chosen` 并将其实例化为 `Integer`，下限和上限为0和1。

```py
food_chosen = LpVariable.dicts("Chosen",food_items,0,1,cat='Integer')
```

然后我们编写了一段特殊代码，将通常的 `food_vars` 和二进制 `food_chosen` 关联起来，并将此约束添加到问题中。

```py
for f in food_items:
    prob += food_vars[f]>= food_chosen[f]*0.1
    prob += food_vars[f]<= food_chosen[f]*1e5

```

如果你长时间盯着代码看，你会意识到这实际上意味着只有在相应的 `food_chosen` 指示变量为1时，我们才会给予 `food_vars` 重要性。但这样我们避免了直接乘法，保持了问题结构的线性。

为了包含西兰花和冰山生菜的“或”条件，我们只需写一段简单的代码，

```py
prob += food_chosen['Frozen Broccoli']+food_chosen['Raw Iceberg Lettuce']<=1

```

这确保了这两个二进制变量的和至多为1，这意味着在最优解中只能包含其中一个，但不能同时包含两个。

### 线性/整数规划的更多应用

在这篇文章中，我们展示了用 Python 设置和解决一个简单线性规划问题的基本流程。然而，如果你四处查看，你会发现无数的工程和商业问题可以转化为某种形式的线性规划，然后使用高效的求解器解决。以下是一些经典的示例，帮助你开始思考，

+   [作为线性规划问题解决数独](https://pythonhosted.org/PuLP/CaseStudies/a_sudoku_problem.html)

+   [作为线性规划问题最大化长期投资回报](https://www.mathworks.com/help/optim/ug/maximize-long-term-investments-using-linear-programming.html)

+   [线性规划应用于生产规划](http://www.me.utexas.edu/~jensen/or_site/models/unit/lp_model/prod/prod.html)

+   [使用整数线性规划解决仓库位置问题](https://www.ibm.com/support/knowledgecenter/SSSA5P_12.8.0/ilog.odms.ide.help/OPL_Studio/opllanguser/topics/opl_languser_app_areas_IP_warehse.html)

许多机器学习算法也使用优化的一般类别，其中线性规划是一个子集——[**凸优化**](https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf)。有关更多信息，请参阅以下文章，

[**深层次的内容是什么？机器学习核心的优化**](https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf)

*我们展示了最受欢迎的机器学习/统计建模技术背后的核心优化框架。* [towardsdatascience.com](https://towardsdatascience.com/a-quick-overview-of-optimization-models-for-machine-learning-and-statistics-38e3a7d13138)

### 总结与结论

在这篇文章中，我们展示了使用 Python 包 PuLP 解决简单的饮食优化问题的线性和整数编程技术。值得注意的是，即使是广泛使用的 [SciPy 也内置了线性优化方法](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html#scipy.optimize.linprog)。鼓励读者尝试各种 Python 库，选择适合自己的方法。

如果你有任何问题或想法，请通过 [**tirthajyoti[AT]gmail.com**](mailto:tirthajyoti@gmail.com) 联系作者。你还可以查看作者的 [**GitHub**](https://github.com/tirthajyoti?tab=repositories) **代码库**，寻找其他有趣的 Python、R 或 MATLAB 代码片段和机器学习资源。如果你和我一样，对机器学习/数据科学充满热情，欢迎 [在 LinkedIn 上添加我](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 或 [在 Twitter 上关注我](https://twitter.com/tirthajyotiS)。

[**Tirthajyoti Sarkar - 高级首席工程师 - 半导体、人工智能、机器学习 - ON…**](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)

*乔治亚理工学院科学硕士 - MS，分析学 该 MS 课程传授理论和实践…* [www.linkedin.com](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)

**简介：[Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)** 是 ON Semiconductor 的高级首席工程师，专注于基于深度学习/机器学习的设计自动化项目。

[原文](https://towardsdatascience.com/linear-programming-and-discrete-optimization-with-python-using-pulp-449f3c5f6e99?sk=881261aaf3fcbc3c45bd7c47e6a41ef0)。经许可转载。

**相关：**

+   [优化如何运作](/2019/04/how-optimization-works.html)

+   [使用 R 进行优化](/2018/05/optimization-using-r.html)

+   [机器学习中的优化：鲁棒性还是全局最小值？](/2017/06/robust-global-minimum.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 维护

* * *

### 更多相关内容

+   [数据科学家的线性规划 101](https://www.kdnuggets.com/2023/02/linear-programming-101-data-scientists.html)

+   [超参数优化：10 个顶级 Python 库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)

+   [基于研究的高级提示技术提升LLM效率](https://www.kdnuggets.com/3-research-driven-advanced-prompting-techniques-for-llm-efficiency-and-speed-optimization)

+   [使用TPOT进行机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)

+   [SQL查询优化技术](https://www.kdnuggets.com/2023/03/sql-query-optimization-techniques.html)

+   [数据库优化：探索SQL中的索引](https://www.kdnuggets.com/2023/07/database-optimization-exploring-indexes-sql.html)
