# Python 三元组:如何使用它以及它为什么有用(有例子)

> 原文：<https://www.dataquest.io/blog/python-ternary-operator/>

August 10, 2022![Python Ternary Operator](img/cf0f47248184fa279dc16b061a61b166.png)

## 什么是 Python 三元运算符，什么时候有用？本教程将带你了解你需要知道的一切。

Python 三元运算符(或*条件运算符*)测试条件是真还是假，并根据结果返回相应的值——所有这些都在一行代码中完成。换句话说，在我们只需要在两个值之间“切换”的情况下，它是常见的多行 if-else 控制流语句的一个紧凑替代方案。Python 2.5 中引入了三元运算符。

该语法由三个操作数组成，因此称为“三元”:

```
a if condition else b
```

以下是这些操作数:

*   `condition` —测试真或假的布尔表达式
*   `a` —如果条件被评估为真，将返回的值
*   `b` —如果条件被评估为假，将返回的值

在这种情况下，普通 if-else 语句的等效语句如下:

```
if condition:
    a
else:
    b
```

让我们看一个简单的例子:

```
"Is true" if True else "Is false"
```

```
'Is true'
```

虽然三元运算符是重写经典 if-else 块的一种方式，但在某种意义上，它的行为就像一个函数，因为它*返回*一个值。事实上，我们可以将这个操作的结果赋给一个变量:

`my_var = a if condition else b`

例如:

```
x = "Is true" if True else "Is false"
print(x)
```

```
Is true
```

在我们有三元运算符之前，我们将使用`condition and a or b`而不是`a if condition else b`。例如，不运行下面的。。。

```
True if 2 > 1 else False
```

```
True
```

。。。我们将运行这个:

```
2 > 1 and True or False
```

```
True
```

然而，如果语法`condition and a or b`中`a`的值计算为`False`(例如，如果`a`等于`0`，或者`None`，或者`False`，我们将会收到不准确的结果。下面这个三元运算符的例子看起来逻辑上有争议(我们想返回`False`if 2>1；否则，我们希望返回`True`)，但这在技术上是正确的，因为如果条件评估为`True`，则由我们决定返回哪个值，如果条件评估为`False`，则由我们决定返回哪个值。在这种情况下，我们期待`False`，我们得到了它:

```
False if 2 > 1 else True
```

```
False
```

出于同样的目的，使用“旧式”语法而不是三元运算符，我们仍然会期待`False`。然而，我们收到了一个意想不到的结果:

```
2 > 1 and False or True
```

```
True
```

为了避免这样的问题，在类似的情况下最好使用三元运算符。

### Python 三元运算符的局限性

注意，Python 三元运算符的每个操作数都是一个*表达式*，而不是一个*语句*，这意味着我们不能在它们中的任何一个内部使用赋值语句。否则，程序会抛出一个错误:

```
1 if True else x = 0
```

```
 File "C:\Users\Utente\AppData\Local\Temp/ipykernel_13352/3435735244.py", line 1
    1 if True else x=0
    ^
SyntaxError: cannot assign to conditional expression
```

如果我们需要使用语句，我们必须编写一个完整的 if-else 块，而不是三元运算符:

```
if True:
    1
else:
    x=0
```

Python 三元运算符的另一个限制是，我们不应该使用它来测试多个表达式(即，if-else 块包含两个以上的情况)。从技术上讲，我们仍然可以这样做。例如，以下面这段代码为例:

```
x = -1

if x < 0:
    print('x is less than zero')
elif x > 0:
    print('x is greater than zero')
else:
    print('x is equal to 0')
```

```
x is less than zero
```

我们可以使用**嵌套的三元运算符**重写这段代码:

```
x = -1
'x is less than zero' if x < 0 else 'x is greater than zero' if x > 0 else 'x is equal to 0'
```

```
'x is less than zero'
```

(*边注:*在上面这段代码中，我们省略了`print()`语句，因为 Python 三元运算符*总是*返回值。)

虽然第二段代码看起来比第一段更紧凑，但可读性也差得多。为了避免可读性问题，我们应该选择只在有简单的 if-else 语句时使用 Python 三元运算符。

### 如何使用 Python 三元运算符

现在，我们将讨论应用 Python 三元运算符的各种方法。假设我们想检查某个温度的水是否在沸腾。在标准大气压下，水在 100 摄氏度沸腾。假设我们想知道水壶里的水是否在沸腾，因为它的温度达到了 90 摄氏度。在这种情况下，我们可以简单地使用 if-else 块:

```
t = 90

if t >= 100:
    print('Water is boiling')
else:
    print('Water is not boiling')
```

```
Water is not boiling
```

我们可以使用一个简单的 Python 三元运算符重写这段代码:

```
t = 90
'Water is boiling' if t >= 100 else 'Water is not boiling'
```

```
'Water is not boiling'
```

(*边注:*上面，我们省略了`print()`语句，因为 Python 三元运算符*总是*返回值。)

上面两段代码的语法已经很熟悉了。然而，还有一些我们还没有考虑的实现 Python 三元运算符的其他方法。

### 使用元组

重新组织 Python 三元运算符的第一种方法是编写其元组形式。如果 Python 三元运算符的标准语法是`a if condition else b`，这里我们将把它重写为`(b, a)[condition]`，如下所示:

```
t = 90
('Water is not boiling', 'Water is boiling')[t >= 100]
```

```
'Water is not boiling'
```

在上面的语法中，元组的第一项是如果条件评估为`False`(自`False==0`)将返回的值，而第二项是如果条件评估为`True`(自`True==1`)将返回的值。

与 Python 三元运算符的常见语法相比，这种使用方式并不流行，因为在这种情况下，元组的两个元素都被计算，因为程序首先创建元组，然后才检查索引。此外，确定在哪里放置真值和在哪里放置假值可能是违反直觉的。

#### 使用字典

除了元组，我们还可以使用 Python 字典，如下所示:

```
t = 90
{True: 'Water is boiling', False: 'Water is not boiling'}[t >= 100]
```

```
'Water is not boiling'
```

现在，我们没有区分真假值的问题。但是，与前一种情况一样，在返回正确的表达式之前，两个表达式都会被计算。

#### 使用 Lambdas

最后，实现 Python 三元运算符的最后一种方法是应用 Lambda 函数。为此，我们应该按照下面的形式重写初始语法`a if condition else b`:`(lambda: b, lambda: a)[condition]()`

```
t = 90
(lambda: 'Water is not boiling', lambda: 'Water is boiling')[t >= 100]()
```

```
'Water is not boiling'
```

请注意，在这种情况下，我们可能会对在哪里放置真值和假值感到困惑。但是，与前两种方法相比，这种方法的优势在于它的执行效率更高，因为只计算一个表达式。

### 结论

让我们总结一下我们在本教程中学到的关于 Python 三元运算符的知识:

*   Python 三元运算符的工作原理
*   当它优于普通的 if-else 块时
*   Python 三元运算符的语法
*   相当于在普通 if-else 块中编写的 Python 三元运算符
*   Python 三元运算符的旧版本及其问题
*   Python 三元运算符的局限性
*   嵌套 Python 三元运算符及其对代码可读性的影响
*   如何使用元组、字典和 Lambda 函数应用 Python 三元运算符，包括每种方法的优缺点