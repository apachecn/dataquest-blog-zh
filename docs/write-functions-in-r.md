# 如何用 R 写函数(附 18 个代码示例)

> 原文：<https://www.dataquest.io/blog/write-functions-in-r/>

June 15, 2022![Using Functions in R](img/76426c5be604de17c663f2a7b3fd4737.png)

### 函数是 r 中必不可少的工具。下面是关于创建和调用函数的一些知识。

R 中的函数是最常用的对象之一。理解 R 函数的用途和语法以及知道如何创建或使用它们是非常重要的。在本教程中，我们将学习所有这些东西，甚至更多:什么是 R 函数，R 中存在哪些类型的函数，何时应该使用函数，最流行的内置函数，如何创建和调用用户定义的函数，如何在另一个函数中调用一个函数，以及如何嵌套函数。

## R 中的函数是什么？

R 中的函数是一个对象，它包含多个相互关联的语句，每次调用函数时，这些语句都以预定义的顺序一起运行。R 中的函数可以是内置的，也可以由用户创建(用户自定义)。创建用户定义函数的主要目的是优化我们的程序，避免在特定项目中频繁执行的特定任务所使用的相同代码块的重复，防止我们出现与复制粘贴操作相关的不可避免且难以调试的错误，并使代码更具可读性。一个好的做法是，每当我们需要运行一组特定的命令两次以上时，就创建一个函数。

## R 中的内置函数

R 中有许多有用的内置函数用于各种目的。一些最受欢迎的是:

*   `min()`、`max()`、`mean()`、`median()`——相应地返回一个数字向量的最小值/最大值/平均值/中值
*   `sum()`–返回一个数值向量的和
*   `range()`–返回数值向量的最小值和最大值
*   `abs()`–返回一个数字的绝对值
*   `str()`–显示 R 对象的结构
*   `print()`–在控制台上显示一个 R 对象
*   `ncol()`–返回矩阵或数据帧的列数
*   `length()`–返回 R 对象(向量、列表等)中的项目数。)
*   `nchar()`–返回字符对象中的字符数
*   `sort()`–按升序或降序(`decreasing=TRUE`)对向量进行排序
*   `exists()`–根据变量是否在 R 环境中定义，返回`TRUE`或`FALSE`

让我们来看看上面的一些函数的运行情况:

```py
vector <- c(3, 5, 2, 3, 1, 4)

print(min(vector))
print(mean(vector))
print(median(vector))
print(sum(vector))
print(range(vector))
print(str(vector))
print(length(vector))
print(sort(vector, decreasing=TRUE))
print(exists('vector'))  ## note the quotation marks
```

```py
[1] 1
[1] 3
[1] 3
[1] 18
[1] 1 5
 num [1:6] 3 5 2 3 1 4
NULL
[1] 6
[1] 5 4 3 3 2 1
[1] TRUE
```

### 在 R 中创建函数

虽然应用内置函数方便了许多常见的任务，但我们通常需要创建自己的函数来自动执行特定的任务。为了在 R 中声明一个用户定义的函数，我们使用关键字`function`。语法如下:

```py
function_name <- function(parameters){
  function body 
}
```

以上，一个 R 函数的主要组成部分有:**函数名**、**函数参数**、**函数体**。让我们分别来看看它们。

### 函数名

这是函数对象的名称，它将在函数定义后存储在 R 环境中，并用于调用该函数。它应该简洁、清晰、有意义，这样阅读我们代码的用户就可以很容易理解这个函数到底是做什么的。例如，如果我们需要创建一个计算已知半径的圆的周长的函数，我们最好调用这个函数`circumference`，而不是`function_1`或`circumference_of_a_circle`。(*旁注:*虽然我们通常在函数名中使用动词，但是如果一个名词非常具有描述性且明确，那么只使用这个名词也是可以的。)

#### 功能参数

有时，它们被称为**正式论点**。函数参数是函数定义中的变量，放在括号内并用逗号分隔，每次我们调用函数时，这些变量将被设置为实际值(称为**参数**)。例如:

```py
circumference <- function(r){
    2*pi*r
}
print(circumference(2))
```

```py
[1] 12.56637
```

上面，我们使用公式$C = 2\pi$$r$创建了一个函数来计算半径已知的圆的周长，因此该函数只有唯一的参数`r`。定义函数后，我们用半径等于 2(因此，参数为 2)来调用它。

一个函数没有参数是可能的，尽管很少有用:

```py
hello_world <- function(){
    'Hello, World!'
}
print(hello_world())
```

```py
[1] "Hello, World!"
```

此外，一些参数可以在函数定义中设置为默认值(那些与典型情况相关的值)，然后可以在调用函数时重置这些值。回到我们的`circumference`函数，我们可以将一个圆的默认半径设置为 1，那么如果我们在没有传递参数的情况下调用该函数，它将计算单位圆(即半径为 1 的圆)的周长。否则，它将使用提供的半径计算圆的周长:

```py
circumference <- function(r=1){
    2*pi*r
}
print(circumference())
print(circumference(2))
```

```py
[1] 6.283185
[1] 12.56637
```

### 功能体

函数体是花括号中的一组命令，每次我们调用函数时，这些命令都会按照预先定义的顺序运行。换句话说，在函数体中，我们放置了我们需要函数做的事情:

```py
sum_two_nums <- function(x, y){
    x + y
}
print(sum_two_nums(1, 2))
```

```py
[1] 3
```

注意，函数体中的语句(在上面的例子中——唯一的语句`x + y`)应该缩进 2 或 4 个空格，这取决于我们运行代码的 IDE，但重要的是在整个程序中保持缩进一致。虽然它不影响代码性能，也不是必须的，但它使代码更容易阅读。

如果函数体包含一个语句，可以去掉花括号。例如:

```py
sum_two_nums <- function(x, y) x + y
print(sum_two_nums(1, 2))
```

```py
[1] 3
```

正如我们从上面的例子中看到的，在 R 中，定义函数时通常不需要显式地包含 return 语句，因为 R 函数只是自动返回函数体中最后一个求值的表达式。然而，我们仍然可以使用语法`return(expression_to_be_returned)`在函数体内添加 return 语句。如果我们需要从一个函数返回多个结果，这是不可避免的。例如:

```py
mean_median <- function(vector){
    mean <- mean(vector)
    median <- median(vector)
    return(c(mean, median))
}
print(mean_median(c(1, 1, 1, 2, 3)))
```

```py
[1] 1.6 1.0
```

注意，在上面的 return 语句中，我们实际上返回了一个包含必要结果的**向量**，而不仅仅是由逗号分隔的变量(因为`return()`函数只能返回一个 R 对象)。除了向量，我们还可以返回一个列表，特别是如果返回的结果应该是不同的数据类型。

## 在 R 中调用函数

在上面所有的例子中，我们实际上已经多次调用了创建的函数。要做到这一点，我们只需在括号中输入点名称并添加必要的参数。在 R 中，函数参数可以通过位置、名称(所谓的*命名参数*)、混合基于位置和基于名称的匹配或者完全省略参数来传递。

如果我们通过位置传递参数*，我们需要遵循函数中定义的相同的参数序列:*

```py
subtract_two_nums <- function(x, y){
    x - y
}
print(subtract_two_nums(3, 1))
```

```py
[1] 2
```

在上面的例子中，x 等于 3，y–等于 1，反之亦然。

如果我们按名称传递参数*，即明确指定函数中定义的每个参数取什么值，参数的顺序并不重要:*

```py
subtract_two_nums <- function(x, y){
    x - y
}
print(subtract_two_nums(x=3, y=1))
print(subtract_two_nums(y=1, x=3))
```

```py
[1] 2
[1] 2
```

因为我们显式地指定了`x=3`和`y=1`，我们可以将它们作为`x=3, y=1`或`y=1, x=3`传递——结果是一样的。

有可能混合使用基于位置和基于名称的参数匹配。让我们来看一个根据女性的体重(公斤)、身高(厘米)和年龄(年)计算基础代谢率(基础代谢率)或每日卡路里消耗量的函数示例。将在函数中使用的公式是[米夫林-圣杰尔方程](https://en.wikipedia.org/wiki/Basal_metabolic_rate):

```py
calculate_calories_women <- function(weight, height, age){
    (10 * weight) + (6.25 * height) - (5 * age) - 161
}
```

现在，我们来计算一个 30 岁，体重 60 kg，身高 165 cm 的女性的热量。但是，对于年龄参数，我们将按名称传递参数，对于其他两个参数，我们将按位置传递参数:

```py
print(calculate_calories_women(age=30, 60, 165))
```

```py
[1] 1320.25
```

在上面这样的情况下(当我们混合使用按名称和按位置匹配时)，命名的参数从整个连续的参数中提取并首先匹配，而其余的参数按位置匹配，即按照它们在函数定义中出现的相同顺序。然而，这种做法并不推荐，可能会导致混乱。

最后，我们可以完全省略一些(或全部)论点。如果我们在函数定义中将一些(或全部)参数设置为默认值，就会发生这种情况。让我们返回到我们的`calculate_calories_women`函数，将女性的默认年龄设置为 30 岁；

```py
calculate_calories_women <- function(weight, height, age=30){
    (10 * weight) + (6.25 * height) - (5 * age) - 161
}
print(calculate_calories_women(60, 165))
```

```py
[1] 1320.25
```

在上面的例子中，我们只给函数传递了两个参数，尽管它的定义中有三个参数。但是，由于当我们向函数传递两个参数时，其中一个参数被赋予了一个默认值，R 解释为第三个缺少的参数应该被设置为其默认值，并相应地进行计算，而不会抛出错误。

当调用一个函数时，我们通常将这个操作的结果赋给一个变量，以便以后使用:

```py
circumference <- function(r){
    2*pi*r
}
circumference_radius_5 <- circumference(5)
print(circumference_radius_5)
```

```py
[1] 31.41593
```

## 在其他函数中使用函数

在 R 函数的定义中，我们可以使用其他函数。我们之前已经见过这样的例子，当我们在用户定义的函数`mean_median`中使用内置的`mean()`和`median()`函数时:

```py
mean_median <- function(vector){
    mean <- mean(vector)
    median <- median(vector)
    return(c(mean, median))
}
```

也可以将调用一个函数的输出作为参数直接传递给另一个函数:

```py
radius_from_diameter <- function(d){
    d/2
}

circumference <- function(r){
    2*pi*r
}

print(circumference(radius_from_diameter(4)))
```

```py
[1] 12.56637
```

在上面这段代码中，我们首先创建了两个简单的函数:根据给定的直径计算圆的半径，根据给定的半径计算圆的周长。因为最初我们只知道一个圆的直径(等于 4)，所以我们调用了`circumference`函数中的`radius_from_diameter`函数，首先根据提供的直径值计算半径，然后计算圆的周长。虽然这种方法在很多情况下很有用，但是我们应该小心使用，避免将太多的函数作为参数传递给其他函数，因为这会影响代码的可读性。

最后，函数可以*嵌套*，这意味着我们可以在另一个函数中定义一个新函数。比方说，我们需要一个将三个不相交圆的圆面积相加的函数:

```py
sum_circle_ares <- function(r1, r2, r3){
    circle_area <- function(r){
        pi*r^2
    }
    circle_area(r1) + circle_area(r2) + circle_area(r3)
}

print(sum_circle_ares(1, 2, 3))
```

```py
[1] 43.9823
```

上面，我们在`sum_circle_ares`函数中定义了`circle_area`函数。然后，我们在外部函数中调用内部函数三次(`circle_area(r1)`、`circle_area(r2)`和`circle_area(r3)`)，以计算每个圆的面积，进一步对这些面积求和。现在，如果我们试图在`sum_circle_ares`函数之外调用`circle_area`函数，程序会抛出一个错误，因为内部函数存在并且只在定义它的函数内部工作:

```py
print(circle_area(10))
```

```py
Error in circle_area(10): could not find function "circle_area"
Traceback:

1\. print(circle_area(10))
```

当嵌套函数时，我们必须记住两件事:

1.  与创建任何函数类似，内部函数应该在外部函数中至少使用 3 次。否则，创建它是不可行的。
2.  如果我们希望能够独立于更大的函数使用函数，我们应该在更大的函数之外创建它，而不是嵌套这些函数。例如，如果我们要在`sum_circle_ares`函数之外使用`circle_area`函数，我们将编写以下代码:

```py
circle_area <- function(r){
    pi*r^2
}

sum_circle_ares <- function(r1, r2, r3){
    circle_area(r1) + circle_area(r2) + circle_area(r3)
}

print(sum_circle_ares(1, 2, 3))
print(circle_area(10))
```

```py
[1] 43.9823
[1] 314.1593
```

这里，我们再次在`sum_circle_ares`函数中使用了`circle_area`函数。然而，这一次，我们也能够在函数外部调用它，并得到结果而不是错误。

## 摘要

在本教程中，我们学习了与 r 中的函数相关的许多方面。

*   R 中函数的类型
*   为什么以及何时我们需要创建一个函数
*   R 中一些最流行的内置函数以及它们的用途
*   如何定义用户定义的函数
*   功能的主要组成部分
*   命名函数的最佳实践
*   何时以及如何将功能参数设置为默认值
*   函数体及其语法的细微差别
*   何时应该在函数定义中显式包含 return 语句
*   如何使用命名参数、位置参数或混合参数调用 R 函数
*   当我们混合位置参数和命名参数时会发生什么——为什么这不是一个好的实践
*   当我们可以省略一些(或全部)论证时
*   如何在其他函数中应用函数
*   如何将函数调用作为参数传递给另一个函数
*   何时以及如何嵌套函数

掌握了这些技能和信息，您就可以开始在 r 中创建和使用函数了。