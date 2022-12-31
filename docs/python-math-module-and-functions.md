# Python 数学模块指南(22 个示例和 18 个函数)

> 原文：<https://www.dataquest.io/blog/python-math-module-and-functions/>

May 10, 2022![Using the Math Library in Python](img/fc638737347cc39be62faae45b06b101.png)

# 使用 Python 中的`math`模块

`math`是 Python 3 标准库中的内置模块，提供标准的数学常数和函数。您可以使用`math`模块执行各种数学计算，如数值、三角函数、对数和指数计算。

本教程将探索在`math`模块中实现的常用常量和函数——以及如何使用它们。

## `math`模块常数

`math`模块中有几个内置常量。我们将在这一节中讨论一些最重要的常数。

### `math.pi`

𝜋是一个数学常数，大约等于 3.14159。导入`math`模块后，只需要写`math.pi`就可以访问𝜋号:

```py
import math
print(math.pi)
```

```py
3.141592653589793
```

让我们用𝜋数来计算圆的面积。计算圆面积的公式如下:

$Area = \pi r^2$

```py
import math
def circle_area(r):
    return math.pi*r**2
radius = 3
print("Area =", circle_area(radius))
```

```py
Area = 28.274333882308138
```

### `math.tau`

τ常量返回几乎精确的值$2\pi$。让我们打印它的值:

```py
print(math.tau)
```

```py
6.283185307179586
```

### `math.e`

我们可以简单地通过使用`math.e`常数来访问数字 *e* (或欧拉数):

```py
print(math.e)
```

```py
2.718281828459045
```

### `math.nan`

`math.nan`常量代表*而不是数字*，它可以初始化那些不是数字的变量。从技术上讲，`math.nan`常量的数据类型是*浮点*；但是，这不是一个有效的数字。

```py
print(math.nan)
print(type(math.nan))
```

```py
nan
<class 'float'>
```

### `math.inf`

`math.inf`常数代表浮点正无穷大。它可以表示正无穷大和负无穷大常数，如下所示:

```py
print(math.inf)
print(-math.inf)
```

```py
inf
-inf
```

## `math`模块功能

`math`模块为许多不同的科学和工程应用提供了广泛的数学函数，包括:

*   数字函数
*   幂函数和对数函数
*   三角函数
*   角度转换函数
*   双曲函数
*   和一些特殊功能

然而，我们在这一节中将只讨论最重要的几个。让我们来探索它们。

### 数字函数

#### `math.ceil()`

`math.ceil()`方法将一个浮点数映射到最小的后续整数:

```py
p = 10.1
print(math.ceil(p))
```

```py
11
```

#### `math.floor()`

`math.floor()`方法将一个浮点数映射到前面最大的整数:

```py
q = 9.99
print(math.floor(q))
```

```py
9
```

#### `math.factorial()`

如果`n`是正整数，`math.factorial(n)`方法返回所有小于或等于`n`的自然数的乘积。然而，如果`n = 0`，它返回`1`。下面的代码使用`math.factorial()`来计算$5！$:

```py
n = 5
print("{}! = {}".format(n, math.factorial(n)))
```

```py
5! = 120
```

#### `math.gcd()`

`math.gcd()`方法返回两个数的最大公分母；我们可以用它来减少分数。

例如，25 和 120 的 GCD 是 5，因此我们可以将分子和分母除以 5，得到一个缩减的分数(例如\ frac { 25 } { 120 } = \ frac { 5 } { 24 })。让我们看看它是如何工作的:

```py
gcd = math.gcd(25, 120)
print(gcd)
```

```py
5
```

#### `math.fabs()`

`math.fabs()`方法删除给定数字的负号(如果有的话),并以浮点形式返回其绝对值:

```py
print(math.fabs(-25))
```

```py
25.0
```

### 幂函数和对数函数

#### `math.pow()`

`math.pow()`方法返回一个浮点值，表示 x 值的 y $(x^y)$.次方让我们通过预测一项投资来尝试一下`math.pow()`方法。为了做到这一点，我们需要知道初始存款，年利率，以及你把钱投资到投资账户的年数。最后，通过使用下面的公式，我们可以计算出投资的最终金额:

$ { amount } = deposit(1+interest)^{years}$

例如，考虑以下值:

*   初始存款:10，000 美元
*   年利率:4%
*   年数:5 年

该代码计算五年后存入投资账户的最终金额:

```py
deposit = 10000
interest_rate = 0.04
number_years = 5
final_amount = deposit * math.pow(1 + interest_rate, number_years)
print("The final amount after five years is", final_amount)
```

```py
The final amount after five years is 12166.529024000001
```

`math.pow(x, y)`方法总是返回浮点值，让我们检查函数返回值的数据类型:

```py
print(type(math.pow(2, 4)))
```

```py
<class 'float'>
```

#### `math.exp()`

`math.exp(x)`方法等于$e^x$，其中$e$是欧拉数。我们可以说`math.exp(x)`方法等价于下面的语句:

```py
math.pow(math.e, x)
```

```py
print(math.exp(3))
print(math.pow(math.e, 3))
```

```py
20.085536923187668
20.085536923187664
```

#### `math.sqrt()`

`math.sqrt()`方法返回一个数的平方根。让我们来试试:

```py
print(math.sqrt(16))
```

```py
4.0
```

#### `math.log()`

`math.log()`方法接受两个参数，`x`和`base`，其中 base 的缺省值是$e$。因此，如果我们只传递一个参数，该方法将返回自然对数 *x* $(\log_e x)$的值。另一方面，如果我们提供两个参数，它计算 x 对给定底数的对数($\log_b x$)。让我们计算不同的对数:

```py
print(math.log(10)) 
print(math.log(10, 3))
```

```py
2.302585092994046
2.095903274289385
```

第一行返回 10 的自然对数，第二行返回 10 的以 3 为底的对数。

尽管我们能够使用`math.log(x, 10)`计算任何以 10 为底的数的对数，但是`math`模块提供了一种更精确的方法来执行同样的计算。让我们来看看:

```py
print(math.log10(1000))
```

```py
3.0
```

### 三角函数

`math`模块也提供了一些做三角学的有用方法。在本节中，我们将学习如何使用`math`模块中提供的以下方法计算给定值的正弦、余弦和正切值。

#### `math.sin()`

`math.sin()`方法返回给定值的正弦值，该值必须以弧度为单位。返回值是介于-1 和 1 之间的浮点数:

```py
print(math.sin(math.pi/4))
```

```py
0.7071067811865476
```

#### `math.cos()`

`math.cos()`方法返回给定值的余弦值，和`math.sin()`方法一样，该值必须以弧度为单位。返回值是介于-1 和 1 之间的浮点数:

```py
print(math.cos(math.pi/2))
```

```py
6.123233995736766e-17
```

#### `math.tan()`

`math.tan()`方法返回一个浮点值，表示给定值的正切值。该值必须以弧度为单位。让我们找出不同角度的切线:

```py
print(math.tan(math.pi/4))
print(math.tan(math.pi/6))
print(math.tan(0))
```

```py
0.9999999999999999
0.5773502691896257
0.0
```

* * *

**注**

`math‍‍‍`模块提供了两种有用的角度转换方法。使用`math.degrees()`将给定的角度从弧度转换成角度，使用`math.radians(x)`将给定的角度从角度转换成弧度。

* * *

### 双曲函数

双曲函数与三角函数非常相似；但是，还是有区别的。`math`模块提供了科学和工程应用中出现的所有双曲线函数，如下所示:

| 功能 | 描述 |
| --- | --- |
| `math.cosh(x)` | 计算 x 的双曲余弦。 |
| `math.sinh(x)` | 计算 x 的双曲正弦。 |
| `math.tanh(x)` | 计算 x 的双曲正切。 |
| `math.acosh(x)` | 计算 x 的反双曲余弦值。 |
| `math.asinh(x)` | 计算 x 的反双曲正弦值。 |
| `math.atanh(x)` | 计算 x 的反双曲正切。 |

让我们使用双曲函数进行一些计算:

```py
x = 0.5
print(math.cosh(x))
print(math.sinh(x))
print(math.tanh(x))
```

```py
1.1276259652063807
0.5210953054937474
0.46211715726000974
```

## 结论

`math`内置模块包括许多常量和方法，支持从基础到高级的数学运算。我们探讨了一些最重要和最广泛使用的常数和方法，包括数字、幂和对数、三角函数等等。