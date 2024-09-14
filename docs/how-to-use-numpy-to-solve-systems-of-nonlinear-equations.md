# 如何使用 NumPy 解决非线性方程组

> 原文：[https://www.kdnuggets.com/how-to-use-numpy-to-solve-systems-of-nonlinear-equations](https://www.kdnuggets.com/how-to-use-numpy-to-solve-systems-of-nonlinear-equations)

![如何使用 NumPy 解决非线性方程组](../Images/438919ca72831b7d6f15c3b444146298.png)

作者提供的图片

非线性方程式是数学中非常有趣的一个方面，其应用范围涉及科学、工程和日常生活。在学校时，我花了很长时间才对其概念有了深刻的理解。与形成直线的线性方程式不同，非线性方程式会创建曲线、螺旋或更复杂的形状。这使得它们解决起来有点棘手，但也极具价值，用于建模现实世界的问题。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

简单来说，非线性方程式涉及的变量的指数不为一，或嵌入了更复杂的函数中。以下是几种常见类型：

+   二次方程式：涉及平方项，如 ax² + bx + c = 0。它们的图形形成抛物线，可以向上或向下开口。

+   指数方程式：例如 e^x = 3x，其中变量作为指数出现，导致快速增长或衰减。

+   三角方程式：例如 sin(x) = x/2，其中变量位于三角函数内，形成波浪状模式。

这些方程可以生成各种图形，从抛物线到振荡波，使它们成为建模各种现象的多用途工具。以下是一些非线性方程应用的例子：

+   **物理学**：建模行星的运动、粒子的行为或混沌系统的动力学。

+   **工程学**：设计具有反馈回路的系统，如控制系统或电路行为。

+   **经济学**：分析市场趋势、预测经济增长，或理解不同经济因素之间的复杂互动。

NumPy 可以用来简化解决非线性方程组的过程。它提供了处理复杂计算、找到近似解和可视化结果的工具，使得解决这些挑战性问题变得更容易。

在接下来的部分中，我们将探讨如何利用 NumPy 来解决这些引人入胜的方程，将复杂的数学挑战转化为可管理的任务。

在深入探讨使用 NumPy 求解非线性方程系统的技术细节之前，了解如何有效地制定和设置这些问题是很重要的。要制定一个系统，请遵循以下步骤：

1.  **识别变量**：确定将成为系统一部分的变量。这些是你试图求解的未知数。

1.  **定义方程**：将系统中的每个方程写下来，确保它包含已识别的变量。非线性方程包括 x²、e^x 或 xy 等项。

1.  **安排方程**：清晰地组织方程，将其转换为 NumPy 更易处理的格式。

## 步骤逐步解决过程

在这一部分中，我们将把非线性方程的求解分解为易于处理的步骤，使问题更加可处理。以下是如何使用**NumPy**和**SciPy**系统地解决这些问题。

#### **定义函数**

第一步是将你的非线性方程系统转换为 Python 可以处理的格式。这涉及到将方程定义为函数。

在 Python 中，你将每个方程表示为一个函数，该函数在给定一组变量的情况下返回方程的值。对于非线性系统，这些函数通常包括平方项、指数项或变量的乘积。

例如，你有一个由两个非线性方程组成的系统：

+   f[1]​ (x, y) = x² + y² − 4

+   f[2] (x, y) = x² − y − 1

下面是你如何在 Python 中定义这些函数：

```py
def equations(vars):
    x, y = vars
    eq1 = x**2 + y**2 - 4
    eq2 = x**2 - y - 1
    return [eq1, eq2]
```

在这个函数中，`vars` 是你希望求解的变量列表。每个方程被定义为这些变量的函数，并返回一个结果列表。

#### **设置初始猜测**

在找到解决方案之前，你必须为变量提供初始猜测。这些猜测是必不可少的，因为像 `fsolve` 使用的迭代方法依赖于这些猜测来开始寻找解决方案。

良好的初始猜测可以帮助我们更有效地收敛到解决方案。差的猜测可能会导致收敛问题或不正确的解决方案。可以把这些猜测看作是寻找方程根的起点。

选择有效初始猜测的提示：

+   **领域知识**：利用有关问题的先前知识进行有根据的猜测。

+   **图形分析**：绘制方程图，以便对解决方案可能所在的位置有一个直观的了解。

+   **实验**：有时，尝试几个不同的猜测并观察结果可能会有所帮助。

对于我们的示例方程，你可以从以下开始：

```py
initial_guesses = [1, 1]  # Initial guesses for x and y
```

#### 求解系统

在定义函数和设置初始猜测后，你现在可以使用 `scipy.optimize.fsolve` 来找到非线性方程的根。`fsolve` 旨在通过找到函数为零的地方来处理非线性方程系统。

下面是如何使用 `fsolve` 来解决系统的方法：

```py
from scipy.optimize import fsolve
# Solve the system
solution = fsolve(equations, initial_guesses)
print("Solution to the system:", solution)
```

在这段代码中，`fsolve` 接受两个参数：表示方程系统的函数和初始猜测。它返回满足方程的变量值。

解决之后，你可能想要解释结果：

```py
# Print the results
x, y = solution
print(f"Solved values are x = {x:.2f} and y = {y:.2f}")

# Verify the solution by substituting it back into the equations
print("Verification:")
print(f"f1(x, y) = {x**2 + y**2 - 4:.2f}")
print(f"f2(x, y) = {x**2 - y - 1:.2f}")
```

![结果显示值接近零。](../Images/f91b4781e8306558b22ab73f66f7cb0b.png)

这段代码打印出解，并通过将值代入原始方程来验证，以确保它们接近零。

## 解决方案可视化

一旦你解出了一个非线性方程组，可视化结果可以帮助你更好地理解和解释这些结果。无论你处理的是两个变量还是三个变量，绘制解提供了这些解在问题背景下的清晰视图。

让我们用几个例子来说明如何可视化这些解：

#### 2D 可视化

假设你已经解出了具有两个变量 x 和 y 的方程。以下是如何在 2D 中绘制这些解：

```py
import numpy as np
import matplotlib.pyplot as plt

# Define the system of equations
def equations(vars):
    x, y = vars
    eq1 = x**2 + y**2 - 4
    eq2 = x**2 - y - 1
    return [eq1, eq2]

# Solve the system
from scipy.optimize import fsolve
initial_guesses = [1, 1]
solution = fsolve(equations, initial_guesses)
x_sol, y_sol = solution

# Create a grid of x and y values
x = np.linspace(-3, 3, 400)
y = np.linspace(-3, 3, 400)
X, Y = np.meshgrid(x, y)

# Define the equations for plotting
Z1 = X**2 + Y**2 - 4
Z2 = X**2 - Y - 1

# Plot the contours
plt.figure(figsize=(8, 6))
plt.contour(X, Y, Z1, levels=[0], colors='blue', label='x^2 + y^2 - 4')
plt.contour(X, Y, Z2, levels=[0], colors='red', label='x^2 - y - 1')
plt.plot(x_sol, y_sol, 'go', label='Solution')
plt.xlabel('x')
plt.ylabel('y')
plt.title('2D Visualization of Nonlinear Equations')
plt.legend()
plt.grid(True)
plt.show()
```

这是输出结果：

![2D 可视化](../Images/5b7cb4d03f8fa4b5d6a93059988920cf.png)

图中的蓝色和红色轮廓表示每个方程为零的曲线。绿色点显示了这些曲线交点的解。

#### 3D 可视化

对于涉及三个变量的系统，3D 图可以提供更多信息。假设你有一个包含 x、y 和 z 变量的系统。以下是如何可视化这个系统：

```py
from mpl_toolkits.mplot3d import Axes3D

# Define the system of equations
def equations(vars):
    x, y, z = vars
    eq1 = x**2 + y**2 + z**2 - 4
    eq2 = x**2 - y - 1
    eq3 = z - x * y
    return [eq1, eq2, eq3]

# Solve the system
initial_guesses = [1, 1, 1]
solution = fsolve(equations, initial_guesses)
x_sol, y_sol, z_sol = solution

# Create a grid of x, y, and z values
x = np.linspace(-2, 2, 100)
y = np.linspace(-2, 2, 100)
X, Y = np.meshgrid(x, y)
Z = np.sqrt(4 - X**2 - Y**2)

# Plotting the 3D surface
fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')
ax.plot_surface(X, Y, Z, alpha=0.5, rstride=100, cstride=100, color='blue')
ax.plot_surface(X, Y, -Z, alpha=0.5, rstride=100, cstride=100, color='red')

# Plot the solution
ax.scatter(x_sol, y_sol, z_sol, color='green', s=100, label='Solution')

ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set_title('3D Visualization of Nonlinear Equations')
ax.legend()
plt.show()
```

输出：

![3D 可视化](../Images/18fbad2925ad9eacd975313bff44e6ad.png)

在这个 3D 图中，蓝色和红色表面表示方程的解，而绿色点显示了 3D 空间中的解。

## 结论

在这篇文章中，我们探讨了使用 NumPy 解决非线性方程组的过程。通过分解步骤，从定义问题到可视化解决方案，我们使复杂的数学概念变得易于接近和实际应用。

我们从在 Python 中公式化和定义非线性方程开始。我们强调了初始猜测的重要性，并提供了选择有效起始点的技巧。接着，我们利用 `scipy.optimize.solve` 找到方程的根。最后，我们展示了如何使用 `matplotlib` 可视化这些解，使结果的解释和验证更加容易。

[](https://www.linkedin.com/in/olumide-shittu)****[Shittu Olumide](https://www.linkedin.com/in/olumide-shittu/)**** 是一位软件工程师和技术写作人员，热衷于利用前沿技术编写引人入胜的叙述，对细节有敏锐的洞察力，并擅长简化复杂概念。你也可以在 [Twitter](https://twitter.com/Shittu_Olumide_) 上找到 Shittu。

### 更多相关话题

+   [想用你的数据技能解决全球问题？这里是…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [5 个实用的数据科学项目，帮助你解决实际问题…](https://www.kdnuggets.com/2021/12/5-practical-data-science-projects.html)

+   [数据科学项目，帮助你解决现实世界问题](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [HuggingGPT：解决复杂AI任务的秘密武器](https://www.kdnuggets.com/2023/05/hugginggpt-secret-weapon-solve-complex-ai-tasks.html)

+   [人工智能系统中的不确定性量化](https://www.kdnuggets.com/2022/04/uncertainty-quantification-artificial-intelligencebased-systems.html)

+   [在商业中实施推荐系统的十个关键经验](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)
