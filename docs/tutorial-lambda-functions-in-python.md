# 教程:Python 中的 Lambda 函数

> 原文：<https://www.dataquest.io/blog/tutorial-lambda-functions-in-python/>

March 9, 2022![Lambda Functions in Python](img/2288c1ebb8a8dbfea472e48c41516d93.png)

在本教程中，我们将在 Python 中定义 lambda 函数，并探讨使用它们的优势和局限性。

## Python 中的 Lambda 函数是什么？

lambda 函数是一个匿名函数(即定义时没有名字),它可以接受任意数量的参数，但与普通函数不同，它只计算并返回一个表达式。

Python 中的 lambda 函数具有以下语法:

*λ参数:表达式*

lambda 函数的结构包括三个元素:

*   **关键字`lambda`**——正常功能中`def`的模拟
*   **参数** —支持传递位置和关键字
    参数，就像普通函数一样
*   **主体** —使用 lambda 函数评估给定参数的表达式

注意，与普通函数不同，我们不用括号将 lambda 函数的参数括起来。如果一个 lambda 函数有两个或更多的参数，我们用逗号把它们列出来。

我们使用 lambda 函数只计算一个短表达式(理想情况下是一行),并且只计算一次，这意味着我们以后不会应用这个函数。通常，我们将一个 lambda 函数作为参数传递给一个更高阶的函数(接受其他函数作为参数的函数)，比如 Python 内置函数，如`filter()`、`map()`或`reduce()`。

## Python 中 Lambda 函数的工作原理

让我们看一个 lambda 函数的简单例子:

```py
 lambda x: x + 1
```

```py
 <function __main__.<lambda>(x)>
```

上面的 lambda 函数接受一个参数，将其递增 1，然后返回结果。这是下面带有关键字`def`和`return`的普通函数的简单版本:

```py
 def increment_by_one(x):
        return x + 1
```

然而现在，我们的 lambda 函数`lambda x: x + 1`只创建一个函数对象，不返回任何东西。我们预料到了这一点:我们没有为它的参数`x`提供任何值(一个参数)。让我们先赋一个变量，把它传递给 lambda 函数，看看这次我们得到了什么:

```py
 a = 2
    print(lambda x: a + 1)
```

```py
 <function <lambda> at 0x00000250CB0A5820>
```

正如我们所料，我们的 lambda 函数没有返回`3`，而是返回了函数对象本身及其内存位置。事实上，这不是调用 lambda 函数的正确方式。要将参数传递给 lambda 函数，执行它并返回结果，我们应该使用以下语法:

```py
 (lambda x: x + 1)(2)
```

```py
 3
```

请注意，虽然我们的 lambda 函数的参数没有用括号括起来，但当我们调用它时，我们会在 lambda 函数的整个构造和传递给它的参数周围添加括号。

上面代码中需要注意的另一点是，使用 lambda 函数，我们可以在函数创建后立即执行它并接收结果。这就是所谓的**立即调用函数执行**(或**命**)。

我们可以创建一个带有多个参数的 lambda 函数。在这种情况下，我们用逗号分隔函数定义中的参数。当我们执行这样一个 lambda 函数时，我们以相同的顺序列出相应的参数，并用逗号分隔它们:

```py
 (lambda x, y, z: x + y + z)(3, 8, 1)
```

```py
 12
```

也可以使用 lambda 函数来执行条件运算。下面是一个简单的 *if-else* 函数的 lambda 模拟:

```py
 print((lambda x: x if(x > 10) else 10)(5))
```

```py
 print((lambda x: x if(x > 10) else 10)(12))
```

```py
 10
    12
```

如果存在多个条件( *if-elif-…-else* )，我们必须将它们嵌套:

```py
 (lambda x: x * 10 if x > 10 else (x * 5 if x < 5 else x))(11)
```

```py
 110
```

这种方法的问题是，已经有了一个嵌套条件，代码变得难以阅读，正如我们在上面看到的。在这种情况下，带有一组条件的普通函数比 lambda 函数更好。事实上，我们可以用下面的方式编写上面例子中的 lambda 函数:

```py
 def check_conditions(x):
        if x > 10:
            return x * 10
        elif x < 5:
            return x * 5
        else:
            return x

    check_conditions(11)
```

```py
 110
```

尽管上面的函数比相应的 lambda 函数跨越了更多的行，但它更容易阅读。

我们可以将 lambda 函数赋给一个变量，然后将该变量作为普通函数调用:

```py
 increment = lambda x: x + 1
    increment(2)
```

```py
 3
```

然而，根据 Python 代码的 PEP 8 风格指南，这是一种不好的做法:

> 赋值语句的使用消除了 lambda 表达式可以提供的优于显式 def 语句的唯一好处(即，它可以嵌入到更大的表达式中)。

所以，如果我们真的需要存储一个函数以备后用，与其给一个变量赋一个 lambda 函数，不如定义一个等价的 normal 函数。

## Lambda 函数在 Python 中的应用

### 带有`filter()`功能的λ

我们使用 Python 中的`filter()`函数从
一个 iterable(比如列表、集合、元组、序列等)中选择某些项目。)基于预定义的
标准。它需要两个参数:

*   定义过滤标准的函数
*   运行函数的可迭代对象

作为该操作的结果，我们得到一个过滤器对象:

```py
 lst = [33, 3, 22, 2, 11, 1]
    filter(lambda x: x > 10, lst)
```

```py
 <filter at 0x250cb090520>
```

为了从 filter 对象中获得一个新的 iterable，其中包含原始 iterable 中满足预定义标准的所有项，我们需要将 filter 对象传递给 Python 标准库的相应函数:`list()`、`tuple()`、`set()`、`frozenset()`或`sorted()`(返回一个排序列表)。

让我们筛选一个数字列表，只选择大于 10 的数字，并返回一个按升序排序的列表:

```py
 lst = [33, 3, 22, 2, 11, 1]
    sorted(filter(lambda x: x > 10, lst))
```

```py
 [11, 22, 33]
```

我们不必创建一个新的与原对象类型相同的可迭代对象。此外，我们可以将该操作的结果存储在一个变量中:

```py
 lst = [33, 3, 22, 2, 11, 1]
    tpl = tuple(filter(lambda x: x > 10, lst))
    tpl
```

```py
 (33, 22, 11)
```

### 带有`map()`功能的λ

我们使用 Python 中的`map()`函数对 iterable 的每一项执行特定的操作。它的语法与`filter()`相同:要执行的函数和该函数适用的 iterable。`map()`函数返回一个 map 对象，通过将这个对象传递给相应的 Python 函数，我们可以得到一个新的 iterable:`list()`、`tuple()`、`set()`、`frozenset()`或`sorted()`。

与`filter()`函数一样，我们可以从一个 map 对象中提取一个不同于原始类型的 iterable，并将其赋给一个变量。下面是一个使用`map()`函数将列表中的每一项乘以 10 并输出映射值作为赋给变量`tpl`的元组的例子:

```py
 lst = [1, 2, 3, 4, 5]
    print(map(lambda x: x * 10, lst))
    tpl = tuple(map(lambda x: x * 10, lst))
    tpl
```

```py
 <map object at 0x00000250CB0D5F40>

    (10, 20, 30, 40, 50)
```

`map()`和`filter()`函数的一个重要区别是，第一个*函数总是返回一个与原始函数*长度相同的迭代函数。因此，由于熊猫系列对象也是可迭代的，我们可以在 DataFrame 列上应用`map()`函数来创建一个新列:

```py
 import pandas as pd
    df = pd.DataFrame({'col1': [1, 2, 3, 4, 5], 'col2': [0, 0, 0, 0, 0]})
    print(df)
    df['col3'] = df['col1'].map(lambda x: x * 10)
    df
```

```py
 col1  col2
0     1     0
1     2     0
2     3     0
3     4     0
4     5     0

   col1  col2  col3
0     1     0    10
1     2     0    20
2     3     0    30
3     4     0    40
4     5     0    50
```

注意，为了在上述情况下获得相同的结果，也可以使用`apply()`函数:

```py
 df['col3'] = df['col1'].apply(lambda x: x * 10)
    df
```

```py
 col1  col2  col3
0     1     0    10
1     2     0    20
2     3     0    30
3     4     0    40
4     5     0    50
```

我们还可以根据另一列的某些条件创建一个新的 DataFrame 列。同样，对于下面的代码，我们可以互换使用`map()`或`apply()`函数:

```py
 df['col4'] = df['col3'].map(lambda x: 30 if x < 30 else x)
    df
```

```py
 col1  col2  col3  col4
0     1     0    10    30
1     2     0    20    30
2     3     0    30    30
3     4     0    40    40
4     5     0    50    50
```

### 带有`reduce()`功能的λ

`reduce()`函数与 *functools* Python 模块相关，其工作方式如下:

1.  对 iterable 的前两项进行操作并保存结果
2.  对保存的结果和 iterable 的下一项进行操作
3.  以这种方式处理成对的值，直到使用完 iterable 的所有项

这个函数具有与前两个函数相同的两个参数:一个函数和一个 iterable。但是，与前面的函数不同，这个函数不需要传递给任何其他函数，而是直接返回结果标量值:

```py
 from functools import reduce
    lst = [1, 2, 3, 4, 5]
    reduce(lambda x, y: x + y, lst)
```

```py
 15
```

上面的代码展示了当我们使用`reduce()`函数来计算一个列表的总和时，它的作用(尽管对于这样一个简单的操作，我们会使用一个更好的解决方案:`sum(lst)`)。

请注意，`reduce()`函数总是需要一个正好有两个参数的 lambda 函数，而且我们必须首先从 *functools* Python 模块中导入它。

## Python 中 Lambda 函数的利与弊

### 赞成的意见

*   这是评估单个表达式的理想选择，该表达式应该只评估一次。
*   它一旦被定义就可以被调用。
*   与相应的普通
    函数相比，它的语法更加简洁。
*   它可以作为参数传递给更高阶的函数，比如
    `filter()`、`map()`和`reduce()`。

### 骗局

*   它不能执行多个表达式。
*   它很容易变得繁琐，例如当它
    包含一个 *if-elif-…-else* 循环时。
*   它不能包含任何变量赋值(例如，`lambda x: x=0`
    会抛出一个`SyntaxError`)。
*   我们不能为 lambda 函数提供 docstring。

## 结论

总而言之，我们已经详细讨论了在 Python 中定义和使用 lambda 函数的许多方面:

*   lambda 函数与普通 Python 函数的区别
*   Python 中 lambda 函数的语法和剖析
*   何时使用 lambda 函数
*   lambda 函数的工作原理
*   如何调用 lambda 函数
*   被调用函数执行的定义(IIFE)
*   如何用 lambda 函数执行条件运算，如何
    嵌套多个条件，以及为什么我们应该避免它
*   为什么我们应该避免将 lambda 函数赋给变量
*   如何将 lambda 函数与`filter()`函数一起使用
*   如何将 lambda 函数与`map()`函数一起使用
*   我们如何使用传递了 lambda 函数的
    `map()`函数在 pandas 数据帧中创建新列，以及在这种情况下使用的
    替代函数
*   如何将 lambda 函数与`reduce()`函数一起使用
*   使用 lambda 函数优于普通 Python
    函数的利弊

希望本教程有助于让 Python 中 lambda 函数这个看似吓人的概念变得更清晰、更易于应用！