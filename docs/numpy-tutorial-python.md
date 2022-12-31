# NumPy 教程:使用 Python 进行数据分析

> 原文：<https://www.dataquest.io/blog/numpy-tutorial-python/>

October 18, 2016

*不要错过这篇文章底部的免费数字备忘单*

[NumPy](https://www.numpy.org/) 是一个常用的 Python 数据分析包。通过使用 NumPy，您可以加快工作流程，并与 Python 生态系统中的其他包进行交互，比如使用 NumPy 的 [scikit-learn](https://scikit-learn.org/) 。NumPy 最初是在 2000 年代中期开发的，起源于一个更古老的名为 Numeric 的包。这种长寿意味着几乎每一个 Python 的数据分析或机器学习包都以某种方式利用了 NumPy。

在本教程中，我们将使用 NumPy 来分析葡萄酒质量的数据。该数据包含葡萄酒各种属性的信息，如`pH`和`fixed acidity`，以及每种葡萄酒在`0`和`10`之间的质量分数。质量分数至少是`3`人类味觉测试者的平均值。当我们学习如何与 NumPy 合作时，我们将尝试了解更多关于葡萄酒的感知质量。

![minho](img/def4e6a61613a58dc3342f15d3a28ed5.png)

我们将要分析的葡萄酒来自葡萄牙的米尼奥地区。

数据是从 [UCI 机器学习库](https://archive.ics.uci.edu/ml/)下载的，在这里[可以得到。](https://archive.ics.uci.edu/ml/datasets/Wine+Quality)这里是`winequality-red.csv`文件的前几行，我们将在整个教程中使用:

```py
"fixed acidity";"volatile acidity";"citric acid";"residual sugar";"chlorides";"free sulfur dioxide";"total sulfur dioxide";"density";"pH";"sulphates";"alcohol";"quality"
7.4;0.7;0;1.9;0.076;11;34;0.9978;3.51;0.56;9.4;5
7.8;0.88;0;2.6;0.098;25;67;0.9968;3.2;0.68;9.8;5
```

数据是我称之为 *ssv* (分号分隔值)的格式——每条记录由分号(`;`)分隔，行由新行分隔。文件中有`1600`行，包括一个标题行和`12`列。

在我们开始之前，一个简短的版本说明——我们将使用 [Python 3.5](https://www.python.org/downloads/release/python-350/) 。我们的代码示例将使用 [Jupyter 笔记本](https://jupyter.org/)来完成。

## CSV 数据列表的列表

在使用 NumPy 之前，我们将首先尝试使用 Python 和 [csv](https://docs.python.org/3/library/csv.html) 包来处理数据。我们可以使用 [csv.reader](https://docs.python.org/3/library/csv.html#csv.reader) 对象读入文件，这将允许我们从 *ssv* 文件中读入并拆分所有内容。

在下面的代码中，我们:

*   导入`csv`库。
*   打开`winequality-red.csv`文件。
    *   打开文件，创建一个新的`csv.reader`对象。
        *   传入关键字参数`delimiter=";"`以确保记录在分号字符而不是默认的逗号字符上分割。
    *   调用 [list](https://docs.python.org/3/library/functions.html#func-list) 类型从文件中获取所有行。
    *   将结果分配给`wines`。

```py
 import csv
with open('winequality-red.csv', 'r') as f:
    wines = list(csv.reader(f, delimiter=';')) 
```

一旦我们读入了数据，我们就可以打印出前`3`行:

```py
print(wines[:3])
```

```py
[['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol', 'quality'], ['7.4', '0.7', '0', '1.9', '0.076', '11', '34', '0.9978', '3.51', '0.56', '9.4', '5'], ['7.8', '0.88', '0', '2.6', '0.098', '25', '67', '0.9968', '3.2', '0.68', '9.8', '5']]
```

数据已被读入一个列表列表。每个内部列表都是来自 *ssv* 文件的一行。您可能已经注意到，整个列表中的每一项都表示为一个字符串，这使得计算更加困难。

我们将数据格式化成表格，以便于查看:

| 固定酸度 | 挥发性酸度 | 柠檬酸 | 残糖 | 氯化物 | 游离二氧化硫 | 二氧化硫总量 | 密度 | pH 值 | 硫酸盐 | 酒精 | 质量 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Seven point four | Zero point seven | Zero | One point nine | Zero point zero seven six | Eleven | Thirty-four | 0.9978 | Three point five one | Zero point five six | Nine point four | five |
| Seven point eight | Zero point eight eight | Zero | Two point six | Zero point zero nine eight | Twenty-five | Sixty-seven | 0.9968 | Three point two | Zero point six eight | Nine point eight | five |

从上表可以看出，我们已经读入了三行，第一行包含列标题。标题行之后的每一行代表一种葡萄酒。每一行的第一个元素是`fixed acidity`，第二个是`volatile acidity`，依此类推。我们可以找到葡萄酒的平均 T2。以下代码将:

*   从标题行之后的每一行中提取最后一个元素。
*   将每个提取的元素转换为浮点型。
*   将所有提取的元素分配到列表`qualities`。
*   将`qualities`中所有元素的总和除以`qualities`中的元素总数，得到平均值。

```py
qualities =
[float(item[-1]) for item in wines[1:]]
sum(qualities) / len(qualities)
```

```py
5.6360225140712945
```

虽然我们能够进行我们想要的计算，但是代码相当复杂，而且每次我们想要计算一个量时都必须做类似的事情，这并不有趣。幸运的是，我们可以使用 NumPy 来简化数据处理。

## Numpy 二维数组

使用 NumPy，我们可以处理多维数组。稍后我们将深入探讨所有可能的多维数组类型，但是现在，我们将把重点放在二维数组上。二维数组也称为矩阵，是您应该熟悉的东西。事实上，这只是对列表列表的不同思考方式。矩阵有行和列。通过指定行号和列号，我们能够从矩阵中提取元素。

在下面的矩阵中，第一行是标题行，第一列是`fixed acidity`列:

| 固定酸度 | 挥发性酸度 | 柠檬酸 | 残糖 | 氯化物 | 游离二氧化硫 | 二氧化硫总量 | 密度 | pH 值 | 硫酸盐 | 酒精 | 质量 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Seven point four | Zero point seven | Zero | One point nine | Zero point zero seven six | Eleven | Thirty-four | 0.9978 | Three point five one | Zero point five six | Nine point four | five |
| Seven point eight | Zero point eight eight | Zero | Two point six | Zero point zero nine eight | Twenty-five | Sixty-seven | 0.9968 | Three point two | Zero point six eight | Nine point eight | five |

如果我们选择第一行第二列的元素，我们会得到`volatile acidity`。如果我们选择第三行第二列的元素，我们会得到`0.88`。

在 NumPy 数组中，维数称为秩，每个维称为一个轴。所以行是第一个轴，列是第二个轴。

现在你已经理解了矩阵的基础知识，让我们看看如何从我们的列表中得到一个 NumPy 数组。

### 创建 NumPy 数组

我们可以使用 [numpy.array](https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html) 函数创建一个 NumPy 数组。如果我们传入一个列表的列表，它将自动创建一个具有相同行数和列数的 NumPy 数组。因为我们希望数组中的所有元素都是易于计算的*浮点*元素，所以我们将省略包含*字符串*的标题行。NumPy 的限制之一是数组中的所有元素必须是同一类型，所以如果我们包含标题行，数组中的所有元素都将作为字符串读入。因为我们希望能够计算出葡萄酒的平均值，所以我们需要所有的元素都是浮点数。

在下面的代码中，我们:

*   导入`numpy`包。
*   将列表列表`wines`传递给`array`函数，该函数将其转换成一个 NumPy 数组。
    *   使用列表切片排除标题行。
    *   指定关键字参数`dtype`以确保每个元素都被转换成浮点数。稍后我们将深入探讨`dtype`是什么。

```py
 import csv
with open("winequality-red.csv", 'r') as f:
    wines = list(csv.reader(f, delimiter=";"))
import numpy as np
wines = np.array(wines[1:], dtype=np.float) 
```

试着运行上面的代码，看看会发生什么！

如果我们显示`wines`，我们将得到一个 NumPy 数组:

```py
wines
```

```py
 array([[ 7.4 , 0.7 , 0\. , ..., 0.56 , 9.4 , 5\. ],
[ 7.8 , 0.88 , 0\. , ..., 0.68 , 9.8 , 5\. ],
[ 7.8 , 0.76 , 0.04 , ..., 0.65 , 9.8 , 5\. ],
...,
[ 6.3 , 0.51 , 0.13 , ..., 0.75 , 11\. , 6\. ],
[ 5.9 , 0.645, 0.12 , ..., 0.71 , 10.2 , 5\. ],
[ 6\. , 0.31 , 0.47 , ..., 0.66 , 11\. , 6\. ]]) 
```

我们可以使用 NumPy 数组的 [shape](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.shape.html) 属性来检查数据中的行数和列数:

```py
wines.shape
```

```py
(1599, 12)
```

### 可选的 NumPy 数组创建方法

有多种方法可以用来创建 NumPy 数组。首先，您可以创建一个数组，其中每个元素都是零。下面的代码将创建一个具有`3`行和`4`列的数组，其中每个元素都是`0`，使用 [numpy.zeros](https://docs.scipy.org/doc/numpy/reference/generated/numpy.zeros.html) :

```py
import numpy as np
```

```py
empty_array = np.zeros((3,4)) 

empty_array
```

当你需要一个固定大小的数组，但是还没有任何值的时候，创建一个全零元素的数组是很有用的。

您还可以使用[numpy . rand . rand](https://docs.scipy.org/doc/numpy-1.15.1/reference/generated/numpy.random.randint.html)创建一个数组，其中每个元素都是一个随机数。这里有一个例子:

```py
np.random.rand(3,4)
```

```py
 array([[ 0.2247223 , 0.92240549, 0.14541893, 0.61731257],
[ 0.00154957, 0.82342197, 0.74044906, 0.11466845],
[ 0.6152478 , 0.14433138, 0.13009583, 0.22981301]])
```

当您希望使用示例数组快速测试代码时，创建充满随机数的数组会很有用。

### 使用 NumPy 读入文件

可以使用 NumPy 直接将 csv 或其他文件读入数组。我们可以使用 [numpy.genfromtxt](https://docs.scipy.org/doc/numpy/reference/generated/numpy.genfromtxt.html) 函数来实现这一点。我们可以用它来读取红酒的初始数据。

在下面的代码中，我们:

*   使用`genfromtxt`功能读入`winequality-red.csv`文件。
*   指定关键字参数`delimiter=";"`，以便正确解析字段。
*   指定关键字参数`skip_header=1`，以便跳过标题行。

```py
wines = np.genfromtxt("winequality-red.csv", delimiter=";", skip_header=1)
```

看起来和我们把它读入一个列表，然后把它转换成一个浮点数组一样。NumPy 将根据元素的格式自动为数组中的元素选择数据类型。

### 索引数字数组

我们现在知道了如何创建数组，但是除非我们可以从数组中检索结果，否则我们无法使用 NumPy 做很多事情。我们可以使用数组索引来选择单个元素、元素组或整行整列。需要记住的重要一点是，就像 Python 列表一样，NumPy 是零索引的，这意味着第一行的索引是`0`，第一列的索引是`0`。如果我们想要处理第四行，我们将使用索引`3`，如果我们想要处理第二行，我们将使用索引`1`，等等。我们将再次使用`wines`数组:

| Seven point four | Zero point seven | Zero | One point nine | Zero point zero seven six | Eleven | Thirty-four | 0.9978 | Three point five one | Zero point five six | Nine point four | five |
| Seven point eight | Zero point eight eight | Zero | Two point six | Zero point zero nine eight | Twenty-five | Sixty-seven | 0.9968 | Three point two | Zero point six eight | Nine point eight | five |
| Seven point eight | Zero point seven six | Zero point zero four | Two point three | Zero point zero nine two | Fifteen | Fifty-four | 0.9970 | Three point two six | Zero point six five | Nine point eight | five |
| Eleven point two | Zero point two eight | Zero point five six | One point nine | Zero point zero seven five | Seventeen | Sixty | 0.9980 | Three point one six | Zero point five eight | Nine point eight | six |
| Seven point four | Zero point seven | Zero | One point nine | Zero point zero seven six | Eleven | Thirty-four | 0.9978 | Three point five one | Zero point five six | Nine point four | five |

让我们选择位于行`3`和列`4`的元素。在下面的代码中，我们传入索引`2`作为行索引，传入索引`3`作为列索引。这将从第三行的第四列中检索值:

```py
wines[2,3]
```

```py
2.2999999999999998
```

因为我们在 NumPy 中使用的是二维数组，所以我们指定了两个索引来检索一个元素。第一个索引是行或轴`1`的索引，第二个索引是列或轴`2`的索引。使用`2`索引可以检索`wines`中的任何元素。

### 切片 NumPy 数组

如果我们想从第四列中选择前三项，我们可以使用冒号(`:`)来实现。冒号表示我们想要选择从起始索引到结束索引的所有元素，但不包括结束索引。这也称为切片:

```py
wines[0:3,3]
```

```py
array([ 1.9, 2.6, 2.3])
```

就像列表切片一样，可以省略`0`来检索从开始到元素`3`的所有元素:

```py
wines[:3,3]
```

```py
array([ 1.9, 2.6, 2.3])
```

我们可以通过指定想要的所有元素来选择整个列，从第一个到最后一个。我们仅使用冒号(`:`)来指定，没有起始或结束索引。下面的代码将选择整个第四列:

```py
wines[:,3]
```

```py
array([ 1.9, 2.6, 2.3, ..., 2.3, 2\. , 3.6])
```

我们在上面选择了整个列，但是我们也可以提取整个行:

```py
wines[3,:]
```

```py
 array([ 11.2 , 0.28 , 0.56 , 1.9 , 0.075, 17\. , 60.,
0.998, 3.16 , 0.58 , 9.8 , 6\. ]) 
```

如果我们将索引发挥到极致，我们可以使用两个冒号来选择整个数组，以选择`wines`中的所有行和列。这是一个很棒的聚会小把戏，但是没有太多好的应用:

```py
wines[:,:]
```

```py
 array([[ 7.4 , 0.7 , 0\. , ..., 0.56 , 9.4 , 5\. ],
[ 7.8 , 0.88 , 0\. , ..., 0.68 , 9.8 , 5\. ],<br />
[ 7.8 , 0.76 , 0.04 , ..., 0.65 , 9.8 , 5\. ],<br />
...,
[ 6.3 , 0.51 , 0.13 , ..., 0.75 , 11\. , 6\. ],
[ 5.9 , 0.645, 0.12 , ..., 0.71 , 10.2 , 5\. ],<br />
[ 6\. , 0.31 , 0.47 , ..., 0.66 , 11\. , 6\. ]])
```

### 给 NumPy 数组赋值

我们还可以使用索引为数组中的某些元素赋值。我们可以通过直接给索引值赋值来做到这一点:

```py
wines[1,5] = 10
```

我们可以对切片做同样的事情。要覆盖整个列，我们可以这样做:

```py
wines[:,10] = 50
```

上面的代码用`50`覆盖了第 11 列中的所有值。

## 一维 NumPy 数组

到目前为止，我们已经使用了二维数组，比如`wines`。然而，NumPy 是一个用于处理多维数组的包。最常见的多维数组类型之一是一维数组或向量。正如你可能已经注意到的，当我们切片`wines`时，我们获得了一个一维数组。一维数组只需要一个索引来检索一个元素。二维数组中的每一行和每一列都是一维数组。就像列表的列表类似于二维数组一样，单个列表类似于一维数组。如果我们对葡萄酒进行切片，只检索第三行，我们会得到一个一维数组:

```py
third_wine = wines[3,:]
```

下面是`third_wine`的样子:

```py
 11.200
0.280
0.560
1.900
0.075
17.000
60.000
0.998
3.160
0.580
9.800
6.000 
```

我们可以使用单个索引从`third_wine`中检索单个元素。以下代码将显示`third_wine`中的第二项:

```py
third_wine[1]
```

```py
0.28000000000000003
```

我们使用过的大多数 NumPy 函数，比如[NumPy . rand . rand](https://docs.scipy.org/doc/numpy-1.15.1/reference/generated/numpy.random.randint.html)，都可以用于多维数组。下面是我们如何使用`numpy.random.rand`生成一个随机向量:

```py
np.random.rand(3)
```

```py
array([ 0.88588862, 0.85693478, 0.19496774])
```

以前，当我们调用`np.random.rand`时，我们为一个二维数组传入了一个形状，所以结果是一个二维数组。这一次，我们为一维数组传入了一个形状。该形状指定维度的数量以及每个维度中数组的大小。`(10,10)`的形状将是一个具有`10`行和`10`列的二维数组。`(10,)`的形状将是一个包含`10`个元素的一维数组。

NumPy 变得更复杂的地方是当我们开始处理超过`2`维的数组时。

## n 维 NumPy 数组

这种情况并不经常发生，但是在某些情况下，您会希望处理维度大于`3`的数组。一种理解方式是把它看作列表的列表。假设我们希望存储一家商店的月收入，但是我们希望能够快速查找一个季度和一年的结果。一年的收益可能是这样的:

```py
 [500, 505, 490, 810, 450, 678, 234, 897, 430, 560, 1023, 640]
```

店铺一月赚`$500`，二月赚`$505`，以此类推。我们可以按季度将这些收益分成一系列的列表:

```py
 year_one = [
    [500,505,490],
    [810,450,678],
    [234,897,430],
    [560,1023,640]
]
```

我们可以通过调用`year_one[0][0]`来检索 1 月份的收益。如果我们想要整个季度的结果，我们可以打电话给`year_one[0]`或`year_one[1]`。我们现在有了一个二维数组或矩阵。但是如果我们现在想添加另一年的结果呢？我们必须增加第三维度:

```py
earnings = [
    [
        [500,505,490],
        [810,450,678],
        [234,897,430],
        [560,1023,640]
    ],
    [
        [600,605,490],
        [345,900,1000],
        [780,730,710],
        [670,540,324]
    ]
]
```

我们可以通过调用`earnings[0][0][0]`来检索第一年 1 月的收益。我们现在需要三个索引来检索单个元素。NumPy 中的三维数组也差不多。事实上，我们可以将`earnings`转换成一个数组，然后获得第一年 1 月的收益:

```py
 earnings = np.array(earnings)
earnings[0,0,0]
```

```py
500
```

我们还可以找到数组的形状:

```py
earnings.shape
```

```py
(2, 4, 3)
```

索引和切片的工作方式与三维数组完全相同，但是现在我们有了一个额外的轴可以传入。如果我们想得到所有年份中一月份的收益，我们可以这样做:

```py
earnings[:,0,0]
```

```py
array([500, 600])
```

如果我们想获得两年的第一季度收益，我们可以这样做:

```py
earnings[:,0,:]
```

```py
array([[500, 505, 490],
[600, 605, 490]])
```

如果数据是以某种方式组织的，添加更多的维度可以使查询数据变得更加容易。当我们从 3 维数组发展到 4 维和更大的数组时，同样的属性也适用，它们可以用同样的方式进行索引和切片。

### NumPy 数据类型

正如我们前面提到的，每个 NumPy 数组可以存储单一数据类型的元素。例如，`wines`只包含浮点值。NumPy 使用自己的数据类型存储值，这与 Python 类型如`float`和`str`截然不同。这是因为 NumPy 的核心是用一种叫做 C 的编程语言编写的，它存储数据的方式不同于 Python 数据类型。NumPy 数据类型在 Python 和 C 之间映射，允许我们使用 NumPy 数组而没有任何转换障碍。

您可以通过访问 [dtype](https://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html) 属性来查找 NumPy 数组的数据类型:

```py
wines.dtype
```

```py
dtype('float64')
```

NumPy 有几种不同的数据类型，它们大多映射到 Python 数据类型，比如`float`和`str`。您可以在这里找到 NumPy 数据类型[的完整列表，但这里有几个重要的:](https://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html)

*   `float` —数字浮点数据。
*   `int` —整数数据。
*   `string` —字符数据。
*   `object` — Python 对象。

数据类型还以一个后缀结尾，表明它们占用了多少位内存。所以`int32`是 32 位整数数据类型，`float64`是`64`位浮点数据类型。

### 转换数据类型

您可以使用 [numpy.ndarray.astype](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.astype.html) 方法将数组转换为不同的类型。该方法将实际复制数组，并返回具有指定数据类型的新数组。例如，我们可以将`wines`转换为`int`数据类型:

```py
wines.astype(int)
```

```py
array([[ 7, 0, 0, ..., 0, 9, 5],
[ 7, 0, 0, ..., 0, 9, 5],
[ 7, 0, 0, ..., 0, 9, 5],
...,
[ 6, 0, 0, ..., 0, 11, 6],
[ 5, 0, 0, ..., 0, 10, 5],
[ 6, 0, 0, ..., 0, 11, 6]])
```

正如您在上面看到的，结果数组中的所有项都是整数。注意，在转换`wines`时，我们使用 Python `int`类型而不是 NumPy 数据类型。这是因为几种 Python 数据类型，包括`float`、`int`和`string`，都可以与 NumPy 一起使用，并自动转换成 NumPy 数据类型。

我们可以检查结果数组的 [dtype](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dtype.html#numpy.dtype) 的 [name](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dtype.name.html#numpy.dtype.name) 属性，以查看结果数组映射到了什么数据类型 NumPy:

```py
 int_wines = wines.astype(int)
int_wines.dtype.name
```

```py
'int64'
```

该数组已被转换为 64 位整数数据类型。这允许很长的整数值，但是比将值存储为 32 位整数占用更多的内存空间。

如果您想要更多地控制数组在内存中的存储方式，您可以直接创建 NumPy dtype 对象，如 [numpy.int32](https://docs.scipy.org/doc/numpy/user/basics.types.html) :

```py
np.int32
```

```py
numpy.int32
```

您可以直接使用这些在类型之间转换:

```py
wines.astype(np.int32)
```

```py
array([[ 7, 0, 0, ..., 0, 9, 5],
[ 7, 0, 0, ..., 0, 9, 5],
[ 7, 0, 0, ..., 0, 9, 5],
...,
[ 6, 0, 0, ..., 0, 11, 6],
[ 5, 0, 0, ..., 0, 10, 5],
[ 6, 0, 0, ..., 0, 11, 6]], dtype=int32)
```

## NumPy 数组操作

NumPy 使得对数组执行数学运算变得简单。这是 NumPy 的主要优点之一，它使计算变得非常容易。

### 单数组数学

如果您对一个数组和一个值进行任何基本的数学运算(`/`、`*`、`-`、`+`、`^`)，它会将运算应用于数组中的每个元素。

假设我们想给每个质量分数加上`10`分，因为我们喝醉了，感觉很慷慨。我们是这样做的:

```py
wines[:,11] + 10
```

```py
array([ 15., 15., 15., ..., 16., 15., 16.])
```

注意，上面的操作不会改变`wines`数组——它将返回一个新的一维数组，其中`10`已被添加到葡萄酒质量列的每个元素中。

如果我们改为使用`+=`，我们将就地修改数组:

```py
wines[:,11] += 10
wines[:,11]
```

```py
array([ 15., 15., 15., ..., 16., 15., 16.])
```

所有其他操作都以同样的方式工作。例如，如果我们想将每个质量分数乘以`2`，我们可以这样做:

```py
wines[:,11] * 2
```

```py
array([ 30., 30., 30., ..., 32., 30., 32.])
```

### 多重数组数学

也可以在数组之间进行数学运算。这将把操作应用于元素对。例如，如果我们将`quality`列添加到自身，我们会得到以下结果:

```py
wines[:,11] + wines[:,11]
```

```py
array([ 10., 10., 10., ..., 12., 10., 12.])
```

注意，这相当于`wines[11] * 2` —这是因为 NumPy 将每对元素相加。第一个数组中的第一个元素与第二个数组中的第一个元素相加，第二个元素与第二个元素相加，依此类推。

我们也可以用这个来乘数组。假设我们想挑选一款酒精含量和质量最大化的葡萄酒(我们想喝醉，但我们很优雅)。我们将`alcohol`乘以`quality`，选择得分最高的葡萄酒:

```py
wines[:,10] * wines[:,11]
```

```py
array([ 47., 49., 49., ..., 66., 51., 66.])
```

所有常见操作(`/`、`*`、`-`、`+`、`^`)都将在阵列之间工作。

### 广播

除非你操作的数组大小完全相同，否则不可能进行元素操作。在这种情况下，NumPy 执行广播来尝试匹配元素。本质上，广播包括几个步骤:

*   比较每个数组的最后一个维度。
    *   如果维度长度相等，或者其中一个维度的长度为`1`，那么我们继续前进。
    *   如果尺寸长度不相等，并且没有一个尺寸有长度`1`，那么就有一个错误。
*   继续检查尺寸，直到最短的数组超出尺寸。

例如，以下两种形状是兼容的:

```py
A: (50,3)
B (3,)
```

这是因为数组`A`的尾维长度是`3`，数组`B`的尾维长度是`3`。它们是相等的，所以量纲没问题。然后，数组`B`没有元素了，所以我们没事了，数组对于数学运算是兼容的。

以下两种形状也是兼容的:

```py
A: (1,2)
B (50,2)
```

最后一个维度匹配，`A`的长度为第一维度的`1`。

这两个数组不匹配:

```py
A: (50,50)
B: (49,49)
```

维度的长度不相等，并且没有一个数组的维度长度等于`1`。

这里有一个广播[的详细解释](https://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)，但是我们将通过几个例子来说明这个原理:

```py
wines * np.array([1,2])
```

```py
------------------------------------------------------------------------
ValueError Traceback (most recent call last)
<ipython -input-40-821086ccaf65> in <module>()
----> 1 wines * np.array([1,2])
ValueError: operands could not be broadcast together with shapes (1599,12) (2,)
```

上面的例子不起作用，因为两个数组没有匹配的尾部维度。以下是最后一个维度匹配的示例:

```py
array_one = np.array(
    [
        [1,2],
        [3,4]
    ]
)
array_two = np.array([4,5])
array_one + array_two
```

```py
array([[5, 7],
[7, 9]])
```

大家可以看到，`array_two`已经跨`array_one`的每一行播出了。下面是我们的`wines`数据的一个例子:

```py
rand_array = np.random.rand(12)
wines + rand_array
```

```py
 array([[ 8.08375389, 0.89047394, 0.77022918, ..., 0.94917479,
10.34668852, 5.34569289],
[ 8.48375389, 1.07047394, 0.77022918, ..., 1.06917479,
10.74668852, 5.34569289],
[ 8.48375389, 0.95047394, 0.81022918, ..., 1.03917479,
10.74668852, 5.34569289],
...,
[ 6.98375389, 0.70047394, 0.90022918, ..., 1.13917479,
11.94668852, 6.34569289],
[ 6.58375389, 0.83547394, 0.89022918, ..., 1.09917479,
11.14668852, 5.34569289],
[ 6.68375389, 0.50047394, 1.24022918, ..., 1.04917479,
11.94668852, 6.34569289]]) 
```

`rand_array`的元素在`wines`的每一行中传播，所以`wines`的第一列加上了`rand_array`中的第一个值，依此类推。

## NumPy 数组方法

除了常见的数学运算之外，NumPy 还有几个方法可以用于更复杂的数组计算。这方面的一个例子是 [numpy.ndarray.sum](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.sum.html) 方法。默认情况下，这将查找数组中所有元素的总和:

```py
wines[:,11].sum()
```

```py
9012.0
```

我们所有质量评级的总和是`154.1788`。我们可以将`axis`关键字参数传递到`sum`方法中，以计算一个轴上的总和。如果我们通过`wines`矩阵调用`sum`，并传入`axis=0`，我们将找到数组第一个轴上的总和。这将给出每一列中所有值的总和。第一个轴上的总和会给出每一列的总和，这看起来似乎是反过来的，但考虑这个问题的一种方式是，指定的轴是“离开”的轴。因此，如果我们指定`axis=0`，我们希望这些行消失，并且我们希望找到每一行中剩余轴的总和:

```py
wines.sum(axis=0)
```

```py
 array([ 13303.1 , 843.985 , 433.29 , 4059.55 ,
139.859 , 25384\. , 74302\. , 1593.79794,
5294.47 , 1052.38 , 16666.35 , 9012\. ])
```

我们可以通过检查形状来验证我们做的加法是否正确。形状应为`12`，对应列数:

```py
wines.sum(axis=0).shape
```

```py
(12,)
```

如果我们传入`axis=1`，我们将在数组的第二个轴上找到总和。这将给我们每一行的总和:

```py
wines.sum(axis=1)
```

```py
array([ 74.5438 , 123.0548 , 99.699 , ...,<br />
100.48174, 105.21547,
92.49249])
```

还有其他几种方法的行为类似于`sum`方法，包括:

*   [numpy.ndarray.mean](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.mean.html#numpy.ndarray.mean) —计算数组的平均值。
*   [numpy.ndarray.std](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.std.html) —查找数组的标准偏差。
*   [numpy.ndarray.min](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.min.html) —查找数组中的最小值。
*   [numpy.ndarray.max](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.max.html) —查找数组中的最大值。

你可以在这里找到数组方法的完整列表。

## NumPy 数组比较

NumPy 可以使用数学比较操作，如`<`、`>`、`>=`、`<=`和`==`，来测试行是否匹配某些值。例如，如果我们想查看哪些葡萄酒的质量等级高于`5`，我们可以这样做:

```py
wines[:,11] > 5
```

```py
array([False, False, False, ..., True, False, True], dtype=bool)
```

我们得到一个布尔数组，告诉我们哪些葡萄酒的质量等级高于`5`。我们可以对其他运营商做类似的事情。例如，我们可以查看是否有葡萄酒的质量等级等于`10`:

```py
wines[:,11] == 10
```

```py
array([False, False, False, ..., False, False, False], dtype=bool)
```

### sb 设置

我们可以用布尔数组和 NumPy 数组做的一件强大的事情是只选择 NumPy 数组中的某些行或列。例如，下面的代码将只选择质量超过`7`的`wines`中的行:

```py
high_quality = wines[:,11] > 7
wines[high_quality,:][:3,:]
```

```py
 array([[ 7.90000000e+00, 3.50000000e-01, 4.60000000e-01,
3.60000000e+00, 7.80000000e-02, 1.50000000e+01,
3.70000000e+01, 9.97300000e-01, 3.35000000e+00,
8.60000000e-01, 1.28000000e+01, 8.00000000e+00],
[ 1.03000000e+01, 3.20000000e-01, 4.50000000e-01,
6.40000000e+00, 7.30000000e-02, 5.00000000e+00,
1.30000000e+01, 9.97600000e-01, 3.23000000e+00,
8.20000000e-01, 1.26000000e+01, 8.00000000e+00],
[ 5.60000000e+00, 8.50000000e-01, 5.00000000e-02,
1.40000000e+00, 4.50000000e-02, 1.20000000e+01,
8.80000000e+01, 9.92400000e-01, 3.56000000e+00,
8.20000000e-01, 1.29000000e+01, 8.00000000e+00]])
```

我们只选择那些`high_quality`包含一个`True`值的行，以及所有的列。这种子集化使得根据特定标准过滤数组变得简单。例如，我们可以寻找酒精含量高、质量好的葡萄酒。为了指定多个条件，我们必须将每个条件放在括号中，并用一个&符号(`&`)分隔条件:

```py
high_quality_and_alcohol = (wines[:,10] > 10) & (wines[:,11] > 7)
wines[high_quality_and_alcohol,10:]
```

```py
 array([[ 12.8, 8\. ],
[ 12.6, 8\. ],
[ 12.9, 8\. ],
[ 13.4, 8\. ],
[ 11.7, 8\. ],
[ 11\. , 8\. ],
[ 11\. , 8\. ],
[ 14\. , 8\. ],
[ 12.7, 8\. ],
[ 12.5, 8\. ],
[ 11.8, 8\. ],
[ 13.1, 8\. ],
[ 11.7, 8\. ],
[ 14\. , 8\. ],
[ 11.3, 8\. ],
[ 11.4, 8\. ]])
```

我们可以结合子集化和赋值来覆盖数组中的某些值:

```py
high_quality_and_alcohol = (wines[:,10] > 10) & (wines[:,11] > 7)
wines[high_quality_and_alcohol,10:] = 20
```

## 重塑 NumPy 数组

我们可以改变数组的形状，同时仍然保留所有的元素。这通常会使访问数组元素变得更容易。最简单的整形是翻转轴，这样行就变成了列，反之亦然。我们可以用 [numpy.transpose](https://docs.scipy.org/doc/numpy/reference/generated/numpy.transpose.html) 函数来实现这一点:

```py
np.transpose(wines).shape
```

```py
(12, 1599)
```

我们可以使用 [numpy.ravel](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ravel.html) 函数将数组转换成一维表示。它实际上将一个数组展平成一长串值:

```py
wines.ravel()
```

```py
array([ 7.4 , 0.7 , 0\. , ..., 0.66, 11\. , 6\. ])
```

这里有一个例子，我们可以看到`numpy.ravel`的排序:

```py
 array_one = np.array(
    [
        [1, 2, 3, 4],
        [5, 6, 7, 8]
    ]
)
array_one.ravel() 
```

```py
array([1, 2, 3, 4, 5, 6, 7, 8])
```

最后，我们可以使用[numpy . shape](https://docs.scipy.org/doc/numpy/reference/generated/numpy.reshape.html#numpy.reshape)函数将数组整形为我们指定的形状。下面的代码将把第二行`wines`变成一个有`2`行和`6`列的二维数组:

```py
wines[1,:].reshape((2,6))
```

```py
 array([[ 7.8 , 0.88 , 0\. , 2.6 , 0.098 , 25\. ],
[ 67\. , 0.9968, 3.2 , 0.68 , 9.8 , 5\. ]])
```

### 组合 NumPy 数组

使用 NumPy，将多个阵列组合成一个统一阵列是非常常见的。我们可以使用 [numpy.vstack](https://docs.scipy.org/doc/numpy/reference/generated/numpy.vstack.html) 来垂直堆叠多个数组。想象一下，第二个数组的项目作为新行添加到第一个数组中。我们可以读入包含白葡萄酒质量信息的`winequality-white.csv`数据集，然后将其与包含红葡萄酒信息的现有数据集`wines`相结合。

在下面的代码中，我们:

*   读入`winequality-white.csv`。
*   显示`white_wines`的形状。

```py
white_wines = np.genfromtxt("winequality-white.csv", delimiter=";", skip_header=1)
white_wines.shape
```

```py
(4898, 12)
```

正如你所看到的，我们有葡萄酒的属性。现在我们有了白葡萄酒的数据，我们可以合并所有的葡萄酒数据。

在下面的代码中，我们:

*   使用`vstack`功能组合`wines`和`white_wines`。
*   显示结果的形状。

```py
all_wines = np.vstack((wines, white_wines))
all_wines.shape
```

```py
(6497, 12)
```

可以看到，结果有`6497`行，这是`wines`中的行数和`red_wines`中的行数之和。

如果我们想要水平组合数组，其中行数保持不变，但列是连接的，那么我们可以使用 [numpy.hstack](https://docs.scipy.org/doc/numpy/reference/generated/numpy.hstack.html) 函数。我们组合的数组需要有相同的行数才能工作。

最后，我们可以使用 [numpy.concatenate](https://docs.scipy.org/doc/numpy/reference/generated/numpy.concatenate.html#numpy.concatenate) 作为`hstack`和`vstack`的通用版本。如果我们想要连接两个数组，我们将它们传递给`concatenate`，然后指定我们想要连接的`axis`关键字参数。沿第一轴串联类似于`vstack`，沿第二轴串联类似于`hstack`:

```py
np.concatenate((wines, white_wines), axis=0)
```

```py
 array([[ 7.4 , 0.7 , 0\. , ..., 0.56, 9.4 , 5\. ],
[ 7.8 , 0.88, 0\. , ..., 0.68, 9.8 , 5\. ],
[ 7.8 , 0.76, 0.04, ..., 0.65, 9.8 , 5\. ],
...,
[ 6.5 , 0.24, 0.19, ..., 0.46, 9.4 , 6\. ],
[ 5.5 , 0.29, 0.3 , ..., 0.38, 12.8 , 7\. ],
[ 6\. , 0.21, 0.38, ..., 0.32, 11.8 , 6\. ]])
```

## 免费数字备忘单

如果你有兴趣了解更多关于 NumPy 的信息，请查看我们关于 NumPy 和熊猫的互动课程。你可以注册并免费上第一堂课。

你也可以通过我们的免费数字小抄将你的数字技能提升到一个新的水平！

## 进一步阅读

您现在应该很好地掌握了 NumPy，以及如何将它应用到数据集。

如果您想深入了解，这里有一些资源可能会有所帮助:

*   [NumPy 快速入门](https://docs.scipy.org/doc/numpy/user/quickstart.html) —有很好的代码示例，涵盖了最基本的 NumPy 功能。
*   [Python NumPy 教程](https://cs231n.github.io/python-numpy-tutorial/#numpy) —关于 NumPy 和其他 Python 库的优秀教程。
*   [视觉数字简介](https://github.com/rougier/numpy-tutorial) —用生活游戏来说明数字概念的指南。

在我们的下一个教程中，我们将更深入地研究 [Pandas](https://www.dataquest.io/blog/pandas-python-tutorial) ，这是一个基于 NumPy 构建的库，它使数据分析变得更加容易。它解决了两个最大的难题:

*   不能在一个数组中混合多种数据类型。
*   你必须记住每一列包含什么类型的数据。

## 用正确的方法学习 Python。

从第一天开始，就在你的浏览器窗口中通过编写 Python 代码来学习 Python。这是学习 Python 的最佳方式——亲自看看我们 60 多门免费课程中的一门。

![astronaut floating over code](img/8cbc4821ae1245a9fd02da67c90ed420.png)

[尝试 Dataquest](https://app.dataquest.io/signup)