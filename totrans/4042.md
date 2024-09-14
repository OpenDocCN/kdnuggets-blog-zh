# Python中的多线程和多进程介绍

> 原文：[https://www.kdnuggets.com/introduction-to-multithreading-and-multiprocessing-in-python](https://www.kdnuggets.com/introduction-to-multithreading-and-multiprocessing-in-python)

![Python中的多线程和多进程介绍](../Images/cb420b3a256c558e26a8195c8f8b8f19.png)

作者提供的图片

本教程将讨论如何利用Python执行多线程和多进程任务。它们提供了一种在单个进程或多个进程中执行并发操作的途径。并行和并发执行可以提高系统的速度和效率。我们将首先讨论多线程和多进程的基础知识，然后讨论如何使用Python库实现这些功能。首先，我们简要讨论并行系统的好处。

1.  **性能提升：** 通过并发执行任务，我们可以减少执行时间，提高系统的整体性能。

1.  **可扩展性：** 我们可以将一个大型任务拆分成多个较小的子任务，并为每个子任务分配一个独立的核心或线程进行独立执行。这在大规模系统中非常有用。

1.  **高效的I/O操作：** 借助并发，CPU无需等待进程完成其I/O操作。CPU可以立即开始执行下一个进程，直到之前的进程忙于其I/O操作。

1.  **资源优化：** 通过分配资源，我们可以防止单个进程占用所有资源。这可以避免[资源饥饿](https://unstop.com/blog/starvation-in-os)问题，确保较小的进程也能获得资源。

* * *

## 我们的三大推荐课程

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT需求

* * *

![Python中的多线程和多进程介绍](../Images/f451d0f072135ea807e674af48b2f151.png)

并行计算的好处 | 作者提供的图片

这些是需要并发或并行执行的一些常见原因。现在，回到主要主题，即多线程和多进程，并讨论它们的主要区别。

# 什么是多线程？

多线程是实现单个进程中并行性的方式之一，能够同时执行多个任务。可以在单个进程中创建多个线程，并在该进程内并行执行较小的任务。

单个进程内的线程共享一个公共内存空间，但它们的堆栈跟踪和寄存器是分开的。由于共享内存，它们的计算开销较小。

![Python中的多线程和多进程简介](../Images/c8f8774c7606708d2d0b57d88e162529.png)

单线程与多线程环境 | 图片来自[GeeksForGeeks](https://www.geeksforgeeks.org/multithreading-python-set-1/)

多线程主要用于执行I/O操作，即如果程序的某部分在忙于I/O操作，剩余的程序仍然可以响应。然而，在Python的实现中，由于全局解释器锁（GIL），多线程无法实现真正的并行。

简而言之，GIL是一个互斥锁，只允许一个线程在任何时间与Python字节码交互，即使在多线程模式下，也只能有一个线程同时执行字节码。

这样做是为了在CPython中保持线程安全，但这限制了多线程的性能优势。为了解决这个问题，Python有一个单独的多进程库，我们将在之后讨论。

**什么是守护线程？**

在后台不断运行的线程称为守护线程。它们的主要工作是支持主线程或非守护线程。守护线程不会阻塞主线程的执行，即使它已经完成了执行，仍然会继续运行。

在Python中，守护线程主要用作垃圾回收器。它将默认销毁所有无用的对象并释放内存，以便主线程可以正常使用和执行。

# 什么是多进程？

多进程用于同时执行多个进程。它帮助我们实现真正的并行，因为我们同时执行独立的进程，这些进程有自己的内存空间。它使用CPU的不同核心，并且在进行进程间通信以在多个进程之间交换数据时也很有帮助。

与多线程相比，多进程计算开销更大，因为我们不使用共享内存空间。尽管如此，它允许我们独立执行，并克服了全局解释器锁的限制。

![Python中的多线程和多进程简介](../Images/7c416ec2fca38d97f879cfd44bbbb45c.png)

多进程环境 | 图片来自[GeeksForGeeks](https://www.geeksforgeeks.org/multiprocessing-python-set-1/)

上图演示了一个多进程环境，其中一个主进程创建了两个独立的进程，并将不同的工作分配给它们。

# 多线程实现

现在是时候使用Python实现一个基本的多线程示例了。Python有一个内置的`threading`模块，用于多线程实现。

1.  **导入库：**

```py
import threading
import os
```

1.  **计算平方的函数：**

这是一个用于计算数字平方的简单函数。输入的是一组数字列表，它输出列表中每个数字的平方，以及使用的线程名称和与该线程相关的进程 ID。

```py
def calculate_squares(numbers):
    for num in numbers:
        square = num * num
        print(
            f"Square of the number {num} is {square} | Thread Name {threading.current_thread().name} | PID of the process {os.getpid()}"
        )
```

1.  **主函数：**

我们有一组数字，我们将把这个列表平均分成两个部分，并分别命名为 fisrt_half 和 `second_half`。现在我们将为这些列表分配两个独立的线程 `t1` 和 `t2`。

`Thread` 函数创建一个新线程，该线程接受一个带有参数列表的函数。你也可以为线程指定一个单独的名称。

`.start()` 函数将开始执行这些线程，而 `.join()` 函数会阻塞主线程的执行，直到给定线程完全执行完毕。

```py
if __name__ == "__main__":
    numbers = [1, 2, 3, 4, 5, 6, 7, 8]
    half = len(numbers) // 2
    first_half = numbers[:half]
    second_half = numbers[half:]

    t1 = threading.Thread(target=calculate_squares, name="t1", args=(first_half,))
    t2 = threading.Thread(target=calculate_squares, name="t2", args=(second_half,))

    t1.start()
    t2.start()

    t1.join()
    t2.join()
```

输出：

```py
Square of the number 1 is 1 | Thread Name t1 | PID of the process 345
Square of the number 2 is 4 | Thread Name t1 | PID of the process 345
Square of the number 5 is 25 | Thread Name t2 | PID of the process 345
Square of the number 3 is 9 | Thread Name t1 | PID of the process 345
Square of the number 6 is 36 | Thread Name t2 | PID of the process 345
Square of the number 4 is 16 | Thread Name t1 | PID of the process 345
Square of the number 7 is 49 | Thread Name t2 | PID of the process 345
Square of the number 8 is 64 | Thread Name t2 | PID of the process 345
```

> **注意：** 上述创建的所有线程都是非守护线程。要创建一个守护线程，你需要写 `t1.setDaemon(True)` 来使线程 `t1` 成为守护线程。

现在，我们将了解上述代码生成的输出。我们可以观察到进程 ID（即 PID）对于两个线程是相同的，这意味着这两个线程是同一进程的一部分。

你也可以观察到输出不是顺序生成的。在第一行，你会看到线程1生成的输出，然后在第3行看到线程2生成的输出，然后在第四行再次看到线程1的输出。这清楚地表明这些线程是并发工作的。

并发并不意味着这两个线程是并行执行的，因为每次只有一个线程在执行。这并不会减少执行时间。它需要与顺序执行相同的时间。CPU 开始执行一个线程，但在中途离开它并转到另一个线程，过一段时间后，返回到主线程，并从上次离开的点继续执行。

# 多进程实现

我希望你对多线程及其实现和局限性有了基本了解。现在，是时候学习多进程实现以及如何克服这些局限性了。

我们将继续使用相同的例子，但这次不是创建两个独立的线程，而是创建两个独立的进程，并讨论观察结果。

1.  **导入库：**

```py
from multiprocessing import Process
import os
```

我们将使用 `multiprocessing` 模块来创建独立的进程。

1.  **计算平方的函数：**

这个函数将保持不变。我们只是去除了线程信息的打印语句。

```py
def calculate_squares(numbers):
    for num in numbers:
        square = num * num
        print(
            f"Square of the number {num} is {square} | PID of the process {os.getpid()}"
        )
```

1.  **主函数：**

主函数有一些修改。我们只是创建了一个独立的进程，而不是线程。

```py
if __name__ == "__main__":
    numbers = [1, 2, 3, 4, 5, 6, 7, 8]
    half = len(numbers) // 2
    first_half = numbers[:half]
    second_half = numbers[half:]

    p1 = Process(target=calculate_squares, args=(first_half,))
    p2 = Process(target=calculate_squares, args=(second_half,))

    p1.start()
    p2.start()

    p1.join()
    p2.join()
```

输出：

```py
Square of the number 1 is 1 | PID of the process 1125
Square of the number 2 is 4 | PID of the process 1125
Square of the number 3 is 9 | PID of the process 1125
Square of the number 4 is 16 | PID of the process 1125
Square of the number 5 is 25 | PID of the process 1126
Square of the number 6 is 36 | PID of the process 1126
Square of the number 7 is 49 | PID of the process 1126
Square of the number 8 is 64 | PID of the process 1126
```

我们观察到每个列表都由一个单独的进程执行。这两个进程有不同的进程 ID。为了检查我们的进程是否是并行执行的，我们需要创建一个独立的环境，我们将在下面讨论。

## 计算有无多进程的运行时间

为了检查我们是否实现了真正的并行性，我们将计算算法在使用和不使用多进程情况下的运行时间。

为此，我们需要一个包含超过10^6个整数的广泛整数列表。我们可以使用`random`库生成一个列表。我们将使用Python的`time`模块来计算运行时间。下面是实现代码。代码本身已经很清晰，尽管你可以随时查看代码注释。

```py
from multiprocessing import Process
import os
import time
import random

def calculate_squares(numbers):
    for num in numbers:
        square = num * num

if __name__ == "__main__":
    numbers = [
        random.randrange(1, 50, 1) for i in range(10000000)
    ]  # Creating a random list of integers having size 10^7.
    half = len(numbers) // 2
    first_half = numbers[:half]
    second_half = numbers[half:]

    # ----------------- Creating Single Process Environment ------------------------#

    start_time = time.time()  # Start time without multiprocessing

    p1 = Process(
        target=calculate_squares, args=(numbers,)
    )  # Single process P1 is executing all list
    p1.start()
    p1.join()

    end_time = time.time()  # End time without multiprocessing
    print(f"Execution Time Without Multiprocessing: {(end_time-start_time)*10**3}ms")

    # ----------------- Creating Multi Process Environment ------------------------#

    start_time = time.time()  # Start time with multiprocessing

    p2 = Process(target=calculate_squares, args=(first_half,))
    p3 = Process(target=calculate_squares, args=(second_half,))

    p2.start()
    p3.start()

    p2.join()
    p3.join()

    end_time = time.time()  # End time with multiprocessing
    print(f"Execution Time With Multiprocessing: {(end_time-start_time)*10**3}ms")
```

输出：

```py
Execution Time Without Multiprocessing: 619.8039054870605ms
Execution Time With Multiprocessing: 321.70287895202637ms
```

你可以观察到，使用多进程的时间几乎是没有使用多进程时间的一半。这表明这两个过程同时执行，表现出真正的并行性。

你还可以阅读这篇文章[顺序与并发与并行](https://blog.bitsrc.io/sequential-vs-concurrent-vs-parallelism-87d1907e5be0)来自Medium，这将帮助你理解这些顺序、并发和并行过程之间的基本区别。

**[](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)**** 是一名B.Tech. 电气工程专业的学生，目前在本科的最后一年。他的兴趣领域在于Web开发和机器学习。他已经追求了这一兴趣，并渴望在这些方向上进一步发展。

### 更多相关内容

+   [使用PyCaret进行Python聚类简介](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [多标签分类：使用Python的Scikit-Learn简介](https://www.kdnuggets.com/2023/08/multilabel-classification-introduction-python-scikitlearn.html)

+   [Python中的内存分析简介](https://www.kdnuggets.com/introduction-to-memory-profiling-in-python)

+   [鸭子、鸭子、代码：Python的鸭子类型简介](https://www.kdnuggets.com/duck-duck-code-an-introduction-to-pythons-duck-typing)

+   [__getitem__简介：Python中的魔法方法](https://www.kdnuggets.com/2023/03/introduction-getitem-magic-method-python.html)

+   [Python数据清洗库简介](https://www.kdnuggets.com/2023/03/introduction-python-libraries-data-cleaning.html)
