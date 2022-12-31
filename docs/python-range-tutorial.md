# Python 范围教程:学习使用这个有用的内置函数

> 原文：<https://www.dataquest.io/blog/python-range-tutorial/>

October 24, 2019![python range tutorial - counting through numbers one by one](img/30bbf5c9f9b7745b761fa41173ba97f8.png)

循环是任何编程语言不可或缺的一部分。在 Python 中，循环的一个重要组成部分是内置的`range`函数。

在这份详细的指南中，我们将通过示例带您了解`range`函数的工作方式，并讨论它的局限性及其解决方法。尽管`range`对于各种各样的 Python 编程任务都很有用，但本指南将以几个关于`range`函数的数据科学用例来结束。

出于本教程的目的，我们假设您至少了解一些 Python 语法。如果你以前从未使用过 Python，我们建议你先从这个[交互式 Python 基础课程](https://www.dataquest.io/course/python-for-data-science-fundamentals/)开始。

#### Python 中范围的简史

本教程主要关注 Python 3，但是如果你以前使用过 Python 2，就需要做一些解释，因为`range`的含义在这两个版本之间发生了变化。

Python 2 中的`range`函数生成了一个可以迭代的数字列表。因此，对于较大的列表，这个过程占用了大量内存。Python 2 中的`xrange`函数通过惰性求值返回项目，这意味着只有在需要时才生成数字，这样使用的内存更少。

Python 2 中的`xrange`函数在 Python 3 中被重命名为`range`，而 Python 2 中的`range`则被弃用。在本教程中，我们正在使用 Python 3 的`range`函数，所以它*没有*曾经与 Python 2 中的`range`相关的性能问题。

## Python 范围:基本用途

让我们首先看看 Python 中`for`循环和`range`函数的基本用法。让我们打印前五个整数。

```py
for i in range(5):
    print(i)
```

```py
0
1
2
3
4
```

上面的代码片段在数字 0 到 4 之间循环。请注意，五没有包含在循环中。因此，`range`的基本用途是遍历一系列数字。我们将很快重新审视`range`的范围。

对于`range`，我们可以使用三个参数:开始、停止和步进。我们可以将这三者说明如下:

*   range(stop):这将创建一个从零到比停止号小 1 的数字范围，增量为 1。
*   range(start，stop):这将创建一个数字范围，从起始数字到比终止数字小 1 的数字，增量为 1。
*   range(start，stop，step):这将创建一个数字范围，从起始数字到小于终止数字的数字，按步长递增。

上面的简单例子使用了声明`range`函数的第一种方式。让我们探索另外两种方法。

```py
# range(start, stop)
for i in range(3, 8):
    print(i)
```

```py
3
4
5
6
7
```

请注意，开始编号包含在范围内，而停止编号不包含在范围内。

```py
# range (start, stop, step)
for i in range(1, 10, 3):
    print(i)
```

```py
1
4
7
```

在声明`range`的第三种方式中，我们从开始数开始，然后向上数三(步数)，直到到达停止数。

## 范围:数据类型

让我们检查由`range`函数返回的对象的类型。

```py
print(type(range(5)))
```

```py
<class 'range'>
```

注意`range`是 Python 中的一种类型。类的默认打印方法打印 range 对象将迭代的数字范围。请注意，仍然没有生成数字—这是由于前面提到的节省内存的“惰性评估”。这些数字只有在它们以某种方式被实际使用时才会生成(比如在上面的 print 函数中被调用)。

```py
print(range(5))
```

```py
range(0, 5)
```

## 范围对象:高级使用

有趣的是，我们可以通过索引访问 range 对象中的项目，就像我们访问列表一样。我们范围内的第三个对象是 2。

```py
range(5)[2]
```

```py
2
```

像列表一样，我们也可以分割一个范围对象。这将返回一个新的 range 对象！

```py
range(5)[:3]
```

```py
range(0, 3)
```

我们也可以反转一个范围对象，使用同样适用于列表的`reversed()`函数。

```py
# reversed
for i in reversed(range(5)):
    print(i)
```

```py
4
3
2
1
0
```

Range 可用于生成负数。

```py
# show negative numbers
for i in range(-10, 5, 3):
    print(i)
```

```py
-10
-7
-4
-1
2
```

我们也可以定义一个负的阶跃函数来生成递减顺序的数字，而不是使用`reversed`函数。

```py
# show negative iteration
for i in range(-10, -17, -2):
    print(i)
```

```py
-10
-12
-14
-16
```

注意，如果您使用带有`range`的 step 参数，它不能为零(这将导致无限循环，从而抛出 ValueError)。

此外，如果从开始参数开始计数不会到达结束参数，`range`不会返回任何内容。请注意，当我们运行下面的代码时，不会打印任何内容，因为如果我们从 17 开始计数，我们将永远无法达到指定的 end 参数 10:

```py
# What if no numbers are in range?
for i in range(17, 10):
    print(i)
```

## 带浮点数的范围对象

range 函数不适用于浮点数。只能将整数值指定为 start、stop 和 step 参数。

```py
# floats with python range
for i in range(0.1, 0.5, 0.1):
    print(i)
```

```py
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-13-a83306d87fcd> in <module>
      1 # floats with python range
----> 2 for i in range(0.1, 0.5, 0.1):
      3     print(i)

TypeError: 'float' object cannot be interpreted as an integer
```

但是，如果我们需要用 floats 生成类似 range 的返回，有几个变通办法。

首先，我们可以定义一个带有三个参数的简单函数，它逐步增加起始数字，直到到达停止:

```py
# floats with python range

def range_with_floats(start, stop, step):
    # Works only with positive float numbers!
    while stop > start:
        yield start
        start += step

for i in range_with_floats(0.1, 0.5, 0.1):
    print(i)
```

```py
0.1
0.2
0.30000000000000004
0.4
```

我们也可以使用 NumPy 从 NumPy 的`arange()`函数中获得相同的结果。

```py
import numpy as np

for i in np.arange(0.1, 0.5, 0.1):
    print(i)
```

```py
0.1
0.2
0.30000000000000004
0.4
```

数字`0.30000000000000004`从何而来？系统中只存储浮点数的近似值。因此，如果我们想生成一个更清晰的输出，我们可能需要在处理浮点数时使用 [`round()`函数](https://docs.python.org/3/library/functions.html#round)。

```py
def range_with_floats(start, stop, step):
    while stop > start:
        yield round(start, 2)   # adding round() to our function and rounding to 2 digits
        start += step

for i in range_with_floats(0.1, 0.5, 0.1):
    print(i)
```

```py
0.1
0.2
0.3
0.4
```

## 在数据科学中使用 Python 的 Range 函数 {#Using-Python 的-Range-Function-in-Data-Science }

#### 读取大文件

Python 的 range 函数在数据科学环境中的一个用途是当我们读取大文件时。

例如，考虑 UCI 提供的【1985 年的汽车进口数据集。该文件采用 CSV 格式，其中的值用逗号分隔。下载文件并用`open()`功能打开。让我们使用`.readline()`方法打印前五行。

```py
data_file = open('imports-85.data')

for i in range(5):
    print(data_file.readline())
```

```py
3,?,alfa-romero,gas,std,two,convertible,rwd,front,88.60,168.80,64.10,48.80,2548,dohc,four,130,mpfi,3.47,2.68,9.00,111,5000,21,27,13495

3,?,alfa-romero,gas,std,two,convertible,rwd,front,88.60,168.80,64.10,48.80,2548,dohc,four,130,mpfi,3.47,2.68,9.00,111,5000,21,27,16500

1,?,alfa-romero,gas,std,two,hatchback,rwd,front,94.50,171.20,65.50,52.40,2823,ohcv,six,152,mpfi,2.68,3.47,9.00,154,5000,19,26,16500

2,164,audi,gas,std,four,sedan,fwd,front,99.80,176.60,66.20,54.30,2337,ohc,four,109,mpfi,3.19,3.40,10.00,102,5500,24,30,13950

2,164,audi,gas,std,four,sedan,4wd,front,99.40,176.60,66.40,54.30,2824,ohc,five,136,mpfi,3.19,3.40,8.00,115,5500,18,22,17450
```

[头文件](https://archive.ics.uci.edu/ml/machine-learning-databases/autos/imports-85.names)描述了列名。最后一列包含汽车进口的价格。如果我们研究文件的前几行，我们会注意到缺失的项目被存储为一个问号(？).

每一行之间都有一个空行，因为它们以换行符(n)结束。我们需要在下面的分析中考虑这一点。让我们检查一下文件中有多少行。在 UNIX 终端中，您可以使用带有文件名的命令 wc -l 作为参数来计算行数。如果您正在使用 Jupyter 笔记本，您可以在命令前使用感叹号来从单元格内运行终端命令。

```py
!wc -l imports-85.data
```

```py
205 imports-85.data
```

有了 205 行数据，让我们试着找到价格最高的汽车进口和汽车在数据集中的行号。首先，我们将在文件长度的范围内循环。接下来，用`.readline()`方法读取该行，去掉每行末尾的换行符，并使用`split()`函数将该行转换成一个条目列表。

单子上的最后一项是进口汽车的价格。如果缺少价格，我们就循环下一个项目。如果价格比我们的`max_price`高，我们改变`max_price`的值并更新行号，存储在变量`max_price_loc`中。

```py
# basic looping in Python
# reading lines in a file (time with a magic function)

data_file = open('imports-85.data')

max_price = 0
max_price_loc = 0

for i in range(205):
    row = data_file.readline().rstrip('\n').split(",")
    price = row[-1]
    # Missing price
    if price == '?':
        continue
    if (max_price < int(price)):
        max_price = int(price)
        max_price_loc = i + 1

print("Maximum Price: ", max_price)
print("Maximum Price Location: Row number ", max_price_loc)
```

```py
Maximum Price:  45400
Maximum Price Location: Row number  75
```

最贵的汽车在 1985 年的价格是 45400 美元，根据 2019 年的价格[扣除通货膨胀](https://www.in2013dollars.com/us/inflation/1985?amount=45400)大约是 108000 美元。

#### 网页抓取时迭代页面

range 函数在数据科学环境中的另一个用途是从某些网站抓取信息。

例如，假设我们想从一个 BBS 论坛中提取数据。通常，文章分布在大量的页面上，在 URL 的某个地方包含一个页码。我们可以只输入一次 URL，而不是一个接一个地输入每个页面的 URL，通过用一个`range`函数产生的每个数字替换 URL 中的页码来交互每个页面。

例如，如果我们想发送一个请求到一个 URL 格式:`https://www.website.com/?p=page_number`的页面，我们可以使用 range 按顺序生成每个 URL。在下面的例子中，我们获得了前十个页面的 URL，但是这种技术可以用来快速生成成百上千个 URL，然后您可以从这些 URL 中一个一个地抓取内容，而实际上不必在代码中放入多个 URL。

```py
for i in range(1, 11):
    current_url = 'https://www.website.com/?p={page_num}'.format(page_num = i)
    print(current_url)
    # Make a request to the URL
```

```py
https://www.website.com/?p=1
https://www.website.com/?p=2
https://www.website.com/?p=3
https://www.website.com/?p=4
https://www.website.com/?p=5
https://www.website.com/?p=6
https://www.website.com/?p=7
https://www.website.com/?p=8
https://www.website.com/?p=9
https://www.website.com/?p=10
```

## 最终想法

在本教程中，我们学习了使用 range()函数的一些不同方法，并看到了它如何在数据科学工作中特别有用的一些示例。

如果您处理数据，range()不太可能是您每天都要使用的函数，但是在某些情况下，对它的工作原理有一个扎实的了解可以节省大量的时间和精力。

想要为数据科学打下更坚实的 Python 基础吗？[注册一个免费账户](https://app.dataquest.io/signup)并查看我们的 [Python 基础](https://www.dataquest.io/course/python-for-data-science-fundamentals/)和[中级](https://www.dataquest.io/course/python-for-data-science-intermediate/)课程。它们是深入的，互动的，完全免费的！

## 用正确的方法学习 Python。

从第一天开始，就在你的浏览器窗口中通过编写 Python 代码来学习 Python。这是学习 Python 的最佳方式——亲自看看我们 60 多门免费课程中的一门。

![astronaut floating over code](img/8cbc4821ae1245a9fd02da67c90ed420.png)

[尝试 Dataquest](https://app.dataquest.io/signup)