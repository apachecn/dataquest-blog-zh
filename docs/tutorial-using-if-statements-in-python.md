# 教程:在 Python 中使用 If 语句

> 原文：<https://www.dataquest.io/blog/tutorial-using-if-statements-in-python/>

March 3, 2022![](img/003004299b2177c8ae16cb0f11ca0424.png)

我们的生活充满了各种情况，即使我们大多数时候没有注意到它们。让我们看几个例子:

*   如果明天不下雨，我会和朋友去公园。否则，我会呆在家里喝一杯热茶，看电视。
*   如果明天天气不太热，我将去海边，但如果天气太热，我将去森林里散步。然而，如果下雨，我会呆在家里。

你明白了。让我们看看计算机中的条件是如何工作的。你可能已经知道 Python 中的程序是逐行执行的。然而，有时，我们需要跳过一些代码，只有在满足某些条件的情况下才执行其中的一部分。这就是[控制结构](https://en.wikipedia.org/wiki/Control_flow#Control_structures_in_practice)变得有用的地方。Python 中的条件语句就是建立在这些控制结构之上的。它们将指导计算机执行程序。

在本教程中，您将学习如何使用条件语句。本指南是为 Python 初学者准备的，但是你需要了解一些 Python 编码的基础知识。如果你不知道，那就去看看这个免费的 [Python 基础课程](https://www.dataquest.io/course/variables-data-types-and-lists-in-python/)。

## 基本的 *if* 语句

在 Python 中，`if`语句是实现条件的起点。让我们看一个最简单的例子:

```py
if <condition>:
    <expression>
```

当 Python 对`<condition>`求值时，它会变成`True`或`False`(布尔值)。因此，如果条件为`True`(即满足条件)，将执行`<expression>`，但是如果`<condition>`为`False`(即不满足条件)，将不执行`<expression>`。

我们可以自由决定条件和表达式，因为 Python 非常灵活。

我们来看一个具体的例子。

```py
# Basic if statement
x = 3
y = 10

if x < y:
    print("x is smaller than y.")
```

```py
x is smaller than y.
```

首先我们定义两个变量，`x`和`y`。然后我们说如果变量`x`小于变量`y`，打印出`x is smaller than y`。实际上，如果我们执行这段代码，我们会打印出这个输出，因为 3 小于 10。

输出:`x is smaller than y.`

让我们看一个更复杂的例子。

```py
# A slightly more complex example
x = 3
y = 10
z = None

if x < y:
    z = 13
print(f"Variable z is now {z}.")
```

```py
Variable z is now 13.
```

在这种情况下，如果满足条件，那么值 13 将被赋给变量`z`。然后`Variable z is now 13.`会被打印出来(注意`print`语句在`if`语句内外都可以使用)。

如您所见，我们在选择要执行的表达式时不受限制。您现在可以通过编写更复杂的代码来进行更多的练习。

让我们看看如果我们执行下面的代码会发生什么:

```py
# What happens here?
x = 3
y = 10

if x > y:
    print("x is greater than y.")
```

这里我们改变了比较符号的方向(原来是*小于*，现在是*大于*)。你能猜出产量吗？

不会有输出！发生这种情况是因为条件没有得到满足。3 不大于 10，所以条件评估为`False`，表达式没有执行。我们如何解决这个问题？用`else`语句。

## *else* 语句

如果我们想在条件不满足的情况下执行一些代码呢？我们在`if`语句下面添加一个`else`语句。让我们看一个例子。

```py
# else statement
x = 3
y = 10

if x > y:
    print("x is greater than y.")
else:
    print("x is smaller than y.")
```

```py
x is smaller than y.
```

输出:`x is smaller than y.`

这里，Python 首先执行 if 条件并检查它是否是`True`。因为 3 不大于 10，所以不满足条件，所以我们不打印出“x 大于 y”。然后我们说在所有其他情况下，我们应该执行 else 语句下的代码:`x is smaller than y.`

让我们回到条件语句的第一个例子:

如果明天不下雨，我会和朋友去公园。否则，我会呆在家里喝一杯热茶，看电视。

这里的 else 语句是“否则”

如果满足条件会怎么样？

```py
# What if the condition is met?
x = 3
y = 10

if x < y:
    print("x is smaller than y.")
else:
    print("x is greater than y.")
```

```py
x is smaller than y.
```

在这种情况下，Python 只是像以前一样打印出第一句话。

输出:`x is smaller than y.`

如果`x`等于`y`呢？

```py
# x is equal to y
x = 3
y = 3

if x < y:
    print("x is smaller than y.")
else:
    print("x is greater than y.")
```

```py
x is greater than y.
```

输出明显是错误的，因为 3 等于 3！在大于或小于比较符号之外，我们还有另一个条件；因此，我们必须使用`elif`语句。

## *elif* 声明

让我们重写上面的例子，添加一个`elif`语句。

```py
# x is equal to y with elif statement
x = 3
y = 3

if x < y:
    print("x is smaller than y.")
elif x == y:
    print("x is equal to y.")
else:
    print("x is greater than y.")
```

```py
x is equal to y.
```

输出:`x is equal to y`。

Python 首先检查条件`x < y`是否满足。它不是，所以它继续到第二个条件，在 Python 中，我们写为`elif`，这是 *else if* 的缩写。如果第一个条件不满足，检查第二个条件，如果满足，执行表达式。别的，干点别的。输出是“x 等于 y。”

现在让我们回到条件语句的第一个例子:

如果明天天气不太热，我将去海边，但如果天气太热，我将去森林里散步。然而，如果下雨，我会呆在家里。

在这里，我们的首要条件是明天天气不要太热(`if`语句)。如果这个条件不满足，那么我们去森林里散步(`elif`语句)。最后，如果两个条件都不满足，我们就呆在家里(`else`语句)。

现在我们把这句话翻译成 Python。

在这个例子中，我们将使用字符串而不是整数来演示 Python 中的`if`条件的灵活性。

```py
# elif condition
tomorrow = "warm"

if tomorrow == "warm":
    print("I'll go to the sea.")
elif tomorrow == "very hot":
    print("I'll go to the forest.")
else:
    print("I'll stay home.")
```

```py
I'll go to the sea.
```

Python 首先检查变量`tomorrow`是否等于“warm ”,如果是，则打印出`I'll go to the sea.`,并停止执行。如果第一个条件没有满足会怎么样？

```py
# Tomorrow is very hot
tomorrow = "very hot"

if tomorrow == "warm":
    print("I'll go to the sea.")
elif tomorrow == "very hot":
    print("I'll go to the forest.")
else:
    print("I'll stay home.")
```

```py
I'll go to the forest.
```

在这种情况下，Python 将第一个条件评估为`False`，并转到第二个条件。这个条件是`True`，所以打印出`I'll go to the forest.`并停止执行。

如果两个条件都不满足，那么它将打印出`I’ll stay home.`

当然，**您可以使用任意数量的`elif`语句。**让我们添加更多的条件，并将`else`语句下输出的内容改为`Weather not recognized.`(例如，如果明天是“f”，我们不知道它的含义)。

```py
# Several elif conditions
tomorrow = "snowy"

if tomorrow == "warm":
    print("I'll go to the sea.")
elif tomorrow == "very hot":
    print("I'll go to the forest.")
elif tomorrow == "snowy":
    print("I'll build a snowman.")
elif tomorrow == "rainy":
    print("I'll stay home.")
else:
    print("Weather not recognized.")
```

```py
I'll build a snowman.
```

猜猜打印出来的是什么？

## 多重条件

现在让我们增加一些复杂性。如果我们想让**在一个`if`语句中满足多个条件呢？**

假设我们想要基于两个气候测量值来预测一个生物群落(即沙漠或热带森林):温度和湿度。例如，如果天气炎热干燥，那么它是一个炎热的沙漠，但如果天气寒冷干燥，那么它是一个北极沙漠。你可以看到，我们不能只根据湿度对这两个生物群落进行分类(它们都是干燥的)，所以我们还必须添加温度测量。

在 Python 中，我们可以使用**逻辑运算符**(即 and、or)在同一个`if`语句中使用多个条件。

看看下面的代码。

```py
# Biome prediction with and logical operator
humidity = "low"
temperature = "high"

if humidity == "low" and temperature == "high":
    print("It's a hot desert.")
elif humidity == "low" and temperature == "low":
    print("It's an arctic desert.")
elif humidity == "high" and temperature == "high":
    print("It's a tropical forest.")
else:
    print("I don't know!")
```

```py
It's a hot desert.
```

输出将是`It's a hot desert.`，因为只有当湿度低而温度高时，组合条件才是`True`。仅仅有一个条件成为`True`是不够的。

形式上，Python 检查第一个条件湿度是否是`True`(确实如此)，然后检查第二个条件温度是否是`True`(确实如此)，只有在这种情况下组合条件才是`True`。如果不满足这些条件中的至少一个，那么组合条件评估为`False`。

如果我们希望两个(或更多)条件中的任何一个得到满足呢？在这种情况下，我们应该使用`or`逻辑运算符。

让我们看一个例子。假设您有一个从 1 到 14(包括 1 和 14)的数字列表，您想要提取所有小于 3 或大于或等于 10 的数字。您可以使用一个`or`操作符来获得结果！

```py
# or logical operator
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
nums_less_3_greater_equal_10 = []

for num in nums:
    if num < 3 or num >= 10:
        nums_less_3_greater_equal_10.append(num)

print(nums_less_3_greater_equal_10)
```

```py
[1, 2, 10, 11, 12, 13, 14]
```

输出:`[1, 2, 10, 11, 12, 13, 14]`

这里 Python 检查了`for`循环中的当前数是否小于 3，如果是`True`，那么组合的`if`语句的计算结果为`True`。如果当前数字等于或大于 10，也会发生同样的情况。如果组合的`if`语句是`True`，那么表达式被执行，当前数字被附加到列表`nums_less_3_greater_equal_10`。

为了实验方便，我们把`or`改成`and`。

```py
# Change or to and
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
nums_less_3_greater_equal_10 = []

for num in nums:
    if num < 3 and num >= 10:
        nums_less_3_greater_equal_10.append(num)

print(nums_less_3_greater_equal_10)
```

```py
[]
```

输出:`[]`

在这种情况下，当前数字应该同时小于 3 并且大于或等于 10，这显然是不可能的，因此组合的`if`语句计算为`False`，并且不执行表达式。

为了让事情更清楚，看看这个`print`声明。

```py
print(False or True)
```

```py
True
```

输出:`True`

这里 Python 计算了`False`和`True`的组合，因为我们有了`or`逻辑操作符，所以这些布尔中至少有一个是`True`就足够了，可以计算组合到`True`的语句。

现在，如果我们把`or`改成`and`会发生什么？

```py
print(False and True)
```

```py
False
```

输出:`False`

两个布尔值都应该是`True`，以将组合条件求值为`True`。既然其中一个是`False`，那么组合条件也是`False`。这就是数字示例中发生的情况。

您甚至可以在一个表达式中组合多个逻辑运算符。让我们使用相同的数字列表，但是现在，我们想要找到所有小于 3 或大于或等于 10 并且同时是偶数的数字。

我们将使用`%`操作符来判断数字是否是偶数。表达式`number % another_number`将得出`number`除以`another_number`的余数。如果我们想知道一个数是否是偶数，那么这个数除以 2 的余数应该是 0。

```py
# More complex logical statements
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
nums_less_3_greater_equal_10_multiple_2 = []

for num in nums:
    if (num < 3 or num >= 10) and num % 2 == 0:
        nums_less_3_greater_equal_10_multiple_2.append(num)

print(nums_less_3_greater_equal_10_multiple_2)
```

```py
[2, 10, 12, 14]
```

输出:`[2, 10, 12, 14]`

为什么输出的第一个数字是 2？在第二个`for`循环中，在括号中的第一个条件中计算 2。小于 3，所以括号里的组合条件是`True`。2 也能被余数为 0 的 2 整除，所以第二个条件也是`True`。两个条件都是`True`，所以这个数字被追加到列表中。

我们为什么要用括号？是因为 Python 中的**运算符优先级**。如果我们移除它们呢？

```py
# More complex logical statements without parentheses
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
nums_less_3_greater_equal_10_multiple_2 = []

for num in nums:
    if num < 3 or num >= 10 and num % 2 == 0:
        nums_less_3_greater_equal_10_multiple_2.append(num)

print(nums_less_3_greater_equal_10_multiple_2)
```

```py
[1, 2, 10, 12, 14]
```

输出:`[1, 2, 10, 12, 14]`

我们在列表中有 1 个！在 Python 中，所有操作符都按照精确的顺序进行计算。例如，`and`操作符优先于`or`操作符。但是如果我们将`or`操作符放在括号中，它将优先于`and`操作符。

首先我们评估`and`操作符两边的条件(它有优先权)。1 既不大于 10，除以 2 也不会得到 0，所以组合条件是`False`。我们只剩下条件`if num < 3 or False`。1 小于 3，所以第一个条件是`True`。条件变成了`True or False`。我们有一个`or`操作符，所以组合条件的计算结果是`True`，1 被追加到列表中。通过检查其他数字发生了什么来练习。

最后，看看这个**真值表**来理解逻辑运算符是如何工作的。这里，我们将只描述`and`和`or`逻辑操作符，但是在 Python 中，我们也有`not`操作符。我们邀请您更多地了解它，并在`if`语句中练习使用它。

| 输入 A | 输入 B | 和 | 运筹学 |
| --- | --- | --- | --- |
| 错误的 | 错误的 | 错误的 | 错误的 |
| 真实的 | 错误的 | 错误的 | 真实的 |
| 错误的 | 真实的 | 错误的 | 真实的 |
| 真实的 | 真实的 | 真实的 | 真实的 |

我们有两个输入，A 和 B，可以是`True`或`False`。比如第二排，A 是`True`，B 是`False`；因此，`A AND B`评估为`False`，而`A OR B`评估为`True`。表格的其余部分以同样的方式阅读。花一分钟去理解它告诉你什么。

## 嵌套的 *if* 语句

Python 是一种非常灵活的编程语言，它允许你在其他 if 语句中使用 if 语句，即所谓的**嵌套`if`语句**。让我们看一个例子。

```py
# Nested if statements
mark =  85

if mark >= 60 and mark <= 100:
    if mark >= 90:
        print("You are the best!")
    elif mark >= 80:
        print("Well done!")
    elif mark >= 70:
        print("You can do better.")
    else:
        print("Pass.")
elif mark > 100:
    print("This mark is too high.")
elif mark < 0:
    print("This mark is too low.")
else:
    print("Failed.")
```

```py
Well done!
```

输出:`Well done!`

这里，如果标记在 60 和 100 之间，则执行`if`语句下的表达式。但是我们还有其他条件也要评估。所以，我们的分数是 85，介于 60 和 100 之间。但是，85 小于 90，所以第一个嵌套的`if`条件是`False`，并且第一个嵌套的表达式没有被执行。但是 85 高于 80，所以执行第二个表达式并“做得好！”是打印出来的。

当然，在第一个`if`语句下面的表达式之外，我们还有`elif`语句。例如，什么`if`的分数高于 100 分？如果第一个条件(60 到 100 之间的数字)是`False`，那么我们直接进入`elif`语句`mark > 100`并打印出`This mark is too low.`。

试着给`mark`变量分配不同的数字来理解这段代码的逻辑。

## Python 3.10 中的模式匹配

2021 年 10 月发布的 Python 3.10 中加入了[模式匹配](https://en.wikipedia.org/wiki/Pattern_matching)。简而言之，可以看出`if..elif`语句的不同语法。让我们看一个例子，用模式匹配重写前面的例子。

```py
# Previous example
tomorrow = "snowy"

if tomorrow == "warm":
    print("I'll go to the sea.")
elif tomorrow == "very hot":
    print("I'll go to the forest.")
elif tomorrow == "snowy":
    print("I'll build a snowman.")
elif tomorrow == "rainy":
    print("I'll stay home.")
else:
    print("Weather not recognized.")
```

```py
I'll build a snowman.
```

```py
# Pattern matching with match..case syntax
tomorrow = "snowy"

match tomorrow:
    case "warm":
        print("I'll go to the sea.")
    case "very hot":
        print("I'll go to the forest.")
    case "snowy":
        print("I'll build a snowman.")
    case "rainy":
         print("I'll stay home.")
    case _:
        print("Weather not recognized.")
```

```py
I'll build a snowman.
```

我们可以看到使用`if..elif`语句和`match..case`语法之间的相似之处。首先，我们定义什么变量是我们想要的`match`，以及我们何时定义用例(或者这个变量可以取的值)。代码的其余部分是类似的。如果匹配了一个案例(相当于一个双等号)，那么执行`print`表达式。

注意最后一个`case`语句，是`_`案例，相当于`else`:如果没有匹配到案例，那么我们`print` `Weather not recognized`。

## *通过*语句

当您开始编写更复杂的代码时，您可能会发现自己不得不使用一个**占位符来代替您稍后想要实现的代码。**`pass`语句就是这个占位符。让我们看一个有和没有`pass`语句的例子。

```py
# Without pass
num = 3
if num == 3:

print("I'll write this code later.")
```

```py
 Input In [24]
    print("I'll write this code later.")
    ^
IndentationError: expected an indented block after 'if' statement on line 3
```

输出:

```py
File "<ipython-input-26-4af22b0ed55d>", line 4
    print("I'll write this code later.")
    ^
IndentationError: expected an indented block
```

Python 需要一些“if”语句下的代码，但是你还没有实现它！你可以在那里写“通过”来解决这个问题。

```py
# With pass
num = 3
if num == 3:
    pass

print("I'll write this code later.")
```

```py
I'll write this code later.
```

输出:`我以后再写这段代码. '

相反，如果你在“If”语句中放置“pass ”, Python 不会抛出任何错误，而是传递给“if”语句下的任何代码。即使在第一个“if”语句下面有其他条件，这也是有效的。

```py
# With pass
num = 4
if num == 3:
    pass
elif num == 4:
    print("The variable num is 4.")
else:
    print("The variable num is neither 3 nor 4.")
```

```py
The variable num is 4.
```

输出:`The variable num is 4.`

## 结论

在 Python 中，`if`语句无时无刻不在使用，你会发现自己在构建的任何项目或脚本中都在使用它们，所以理解它们背后的逻辑是很重要的。在本文中，我们已经介绍了 Python 中`if`条件最重要的方面:

*   创建基本的`if`语句
*   通过使用`else`和`elif`语句增加复杂性
*   使用逻辑运算符(`or`、`and`)在一个`if`语句中组合多个条件
*   使用嵌套的`if`语句
*   使用`pass`语句作为占位符

有了这些知识，您现在可以开始使用 Python 中的条件语句了。

请随时在 [LinkedIn](https://www.linkedin.com/in/artur-sannikov-1b245a111/) 和 [GitHub](https://github.com/artur-sannikov/) 上联系我。编码快乐！