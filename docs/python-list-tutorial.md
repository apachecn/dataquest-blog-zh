# Python 列表教程:列表、循环等等！

> 原文：<https://www.dataquest.io/blog/python-list-tutorial/>

November 15, 2019

列表是 Python 中最强大的数据类型之一。在本 Python 列表教程中，您将了解如何在分析移动应用程序数据时使用列表。

在本教程中，我们假设您了解 Python 的基础知识，包括使用字符串、整数和浮点数。如果你不熟悉这些，你可能想试试我们的[免费 Python 基础课程](https://www.dataquest.io/course/python-for-data-science-fundamentals/)。

我们将使用这张数据表，它取自[移动应用商店数据集(Ramanathan Perumal)](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps) :

| 名字 | 价格 | 货币 | 评级 _ 计数 | 等级 |
| --- | --- | --- | --- | --- |
| 脸谱网 | Zero | 美元 | Two million nine hundred and seventy-four thousand six hundred and seventy-six | Three point five |
| 照片墙 | Zero | 美元 | Two million one hundred and sixty-one thousand five hundred and fifty-eight | Four point five |
| 部族冲突 | Zero | 美元 | Two million one hundred and thirty thousand eight hundred and five | Four point five |
| 神庙逃亡 | Zero | 美元 | One million seven hundred and twenty-four thousand five hundred and forty-six | Four point five |
| 潘多拉——音乐和广播 | Zero | 美元 | One million one hundred and twenty-six thousand eight hundred and seventy-nine | Four |

表格中的每个值都是一个**数据点**。例如，第一行(列标题之后)有五个数据点:

*   `Facebook`
*   `0.0`
*   `USD`
*   `2974676`
*   `3.5`

数据点的集合构成了一个数据集。我们可以将上面的整个表理解为数据点的集合，因此我们将整个表称为数据集。我们可以看到我们的数据集有五行和五列。

利用我们对 Python 类型的理解，我们可能会认为可以将每个数据点存储在它自己的变量中——例如，我们可以这样存储第一行的数据点:


![data stored as individual variables](img/64cc9cad3d529eb7540e8dcc8fe7f249.png)


上面，我们存储了:

*   字符串形式的文本“脸书”
*   价格 0.0 为浮动
*   字符串形式的文本“USD”
*   评级将 2，974，676 计为整数
*   用户将 3.5 评级为浮动

为数据集中的每个数据点创建一个变量将是一个繁琐的过程。幸运的是，我们可以使用**列表**更有效地存储数据。这就是我们如何为第一行创建数据点列表:


![a single rows data stored as a python list](img/2e2af34c7579e3471729307ac5f615ca.png)


为了创建上面的列表，我们:

*   输入一系列数据点，并用逗号分隔:`'Facebook', 0.0, 'USD', 2974676, 3.5`
*   用括号将序列括起来:`['Facebook', 0.0, 'USD', 2974676, 3.5]`

在我们创建了这个列表之后，我们通过将它赋给一个名为`row_1`的变量来将它存储在计算机的内存中。

要创建数据点列表，我们只需:

*   用逗号分隔数据点。
*   用括号将数据点序列括起来。

现在让我们创建五个列表，数据集中的每一行都有一个列表:

```py
row_1 = ['Facebook', 0.0, 'USD', 2974676, 3.5]
row_2 = ['Instagram', 0.0, 'USD', 2161558, 4.5]
row_3 = ['Clash of Clans', 0.0, 'USD', 2130805, 4.5]
row_4 = ['Temple Run', 0.0, 'USD', 1724546, 4.5]
row_5 = ['Pandora - Music & Radio', 0.0, 'USD', 1126879, 4.0]
```

## 索引 Python 列表

列表可以包含各种数据类型。像`[4, 5, 6]`这样的列表有相同的数据类型(只有整数)，而列表`['Facebook', 0.0, 'USD', 2974676, 3.5]`有混合的数据类型:

*   两根弦(`'Facebook', 'USD'`)
*   两个浮动(`0.0`、`3.5`)
*   一个整数(`2974676`)

`['Facebook', 0.0, 'USD', 2974676, 3.5]`列表有五个数据点。要找到一个列表的长度，我们可以使用`len()`命令:


![using len() to find the length of a list](img/c7bed50c2835d893084f91c17c1211b1.png)


对于小列表，我们可以只计算屏幕上的数据点来确定长度，但是无论何时当你处理包含许多元素的列表，或者需要为事先不知道长度的数据编写代码时,`len()`命令将会非常有用。

列表中的每个元素(数据点)都有一个与之相关的特定数字，称为**索引号**。索引总是从 0 开始，因此第一个元素的索引号为 0，第二个元素的索引号为 1，依此类推。


![indexing a python list, 1](img/b818a4da110c83f0301faf74c14b3528.png)


要快速找到列表元素的索引，请确定它在列表中的位置号，然后减去 1。例如，字符串`'USD'`是列表的第三个元素(位置号 3)，因此它的索引号必须是 2，因为 3–1 = 2。

索引号帮助我们从列表中检索单个元素。回头看看上面代码示例中的列表`row_1`，我们可以通过运行代码`row_1[0]`来检索索引号为 0 的第一个元素(字符串`'Facebook'`)。


![index a python list, 2](img/49e32775372e5a53c43ec2a77f17a878.png)


检索单个列表元素的语法遵循模型`list_name[index_number]`。例如，上面列表的名称是`row_1`，第一个元素的索引号是`0`——按照`list_name[index_number]`模型，我们得到`row_1[0]`，其中索引号`0`在变量名`row_1`后面的方括号中。


![syntax explanation for Python list indexing](img/5cfb3d49d04d5162a5c0f5cab64f30bf.png)


这就是我们如何检索`row_1`中的每个元素:


![extracting each element from a list](img/1f45adde5b39c8f57f26db3cfde687a3.png)


检索列表元素使得执行操作更加容易。例如，我们可以选择脸书和 Instagram 的评分，并找出两者之间的平均值或差异:


![using list indexing to extract values and perform a calculation](img/50ecc7c546e0a9c50bed76f03ea7b92f.png)


让我们使用列表索引从前三行中提取评级数量，然后对它们进行平均:

```py
ratings_1 = row_1[3]
ratings_2 = row_2[3]
ratings_3 = row_3[3]

total = ratings_1 + ratings_2 + ratings_3
average = total / 3

print(average)
```

```py
2422346.3333333335

```

## 对列表使用负索引

在 Python 中，我们有两个列表索引系统:

*   **正索引**:第一个元素的索引号为 0，第二个元素的索引号为 1，依此类推。
*   **负索引**:最后一个*元素的索引号为-1，倒数第二个元素的索引号为-2，依此类推。*


![positive vs negative indexing](img/46def60eabd2c193d0c52b2376ade958.png)


在实践中，我们几乎总是使用正索引来检索列表元素。当我们想要选择列表的最后一个元素时，负索引非常有用——尤其是当列表很长，并且我们无法通过计数来判断长度时。


![extracting the last element from a list](img/d3fc3e9fe60efadef929d662e3efe487.png)


注意，如果我们使用两个索引系统范围之外的索引号，我们将得到一个`IndexError`。


![Python indexerror examples](img/d850517401abe3d629c6ed0313bde362.png)


让我们使用负索引从前三行的每一行中提取用户评级(最后一个值),然后对它们进行平均。

```py
rating_1 = row_1[-1]
rating_2 = row_2[-1]
rating_3 = row_3[-1]

total_rating = rating_1 + rating_2 + rating_3
average_rating = total_rating / 3

print(average)
```

```py
2422346.3333333335

```

## 切片 Python 列表

我们可以使用语法快捷方式来选择两个或多个连续的元素，而不是单独选择列表元素:


![list slicing syntax shortcut](img/b71bd6bfe58547c74c2d90165bb9d90a.png)


当我们从名为`a_list`的列表中选择第一个`n`元素(`n`代表一个数字)时，我们可以使用语法快捷键`a_list[0:n]`。在上面的例子中，我们需要从列表`row_3`中选择前三个元素，所以我们使用了`row_3[0:3]`。

当我们选择前三个元素时，我们对列表的一部分进行了切片。因此，选择列表的一部分的过程被称为**列表切片**。

我们可能有很多方法来分割列表:


![Python list slicing examples](img/e713d289c3fb81f2434468b2b1815d8d.png)


要检索我们想要的任何列表片段:

1.  我们首先需要识别切片的第一个和最后一个元素。
2.  然后，我们需要确定切片的第一个和最后一个元素的索引号。
3.  最后，我们可以通过使用语法`a_list[m:n]`来检索我们想要的列表片，其中:
    *   `m`表示切片第一个元素的索引号；和
    *   `n`代表切片最后一个元素**加一个**的索引号(如果最后一个元素的索引号为 2，那么我们`n`将为 3，如果最后一个元素的索引号为 4，那么`n`将为 5，以此类推)。


![Python list slicing syntax explanation](img/03e340193d573e4d5c9243f93af022b0.png)


当我们需要选择第一个或最后一个`x`元素(`x`代表一个数字)时，我们可以使用更简单的语法快捷方式:

*   `a_list[:x]`当我们要选择第一个`x`元素时。
*   `a_list[-x:]`当我们想要选择最后一个`x`元素时。


![list slicing wildcard syntax example](img/bb5b007ec38cf1acff811950c65d6df2.png)


让我们看看如何从第一行中提取前四个元素(带有关于脸书的数据):

```py
first_4_fb = row_1[:4]
print(first_4_fb)
```

```py
['Facebook', 0.0, 'USD', 2974676]

```

同一行的最后三个元素:

```py
last_3_fb = row_1[-3:]
print(last_3_fb)
```

```py
['USD', 2974676, 3.5]

```

第五行的第三和第四个元素(关于潘多拉的数据):

```py
pandora_3_4 = row_5[2:4]
print(pandora_3_4)
```

```py
['USD', 1126879]

```

## Python 列表列表

以前，我们引入了列表作为每个数据点使用一个变量的更好的替代方法。我们可以将数据点捆绑到一个列表中，然后将该列表存储在一个单独的变量中，而不是为五个数据点`'Facebook', 0.0, 'USD', 2974676, 3.5`中的每一个都有一个单独的变量。

到目前为止，我们一直在处理一个有 5 行的数据集，并且我们一直将每一行作为一个列表存储在一个单独的变量中(变量`row_1`、`row_2`、`row_3`、`row_4`和`row_5`)。然而，如果我们有一个有 5000 行的数据集，我们最终会有 5000 个变量，这将使我们的代码混乱不堪，几乎无法使用。

为了解决这个问题，我们可以将五个变量存储在一个列表中:


![creating a list of lists from individual lists](img/38b60f27454e8d19602ab9b4efd84a7a.png)


正如我们所看到的，`data_set`是一个存储其他五个列表(`row_1`、`row_2`、`row_3`、`row_4`和`row_5`)的列表。包含其他列表的列表称为列表列表。

`data_set`变量仍然是一个列表，这意味着我们可以检索单个列表元素，并使用我们所学的语法执行列表切片。下面，我们:

*   使用`data_set[0]`检索第一个列表元素(`row_1`)。
*   使用`data_set[-1]`检索最后一个列表元素(`row_5`)。
*   通过使用`data_set[:2]`执行列表切片来检索前两个列表元素(`row_1`和`row_2`)。


![selecting ](img/db622b6f10fe93fd890dffd1883306ac.png)


我们经常需要从列表的列表中检索单个元素——例如，我们可能想从列表的`data_set`列表中的`['Facebook', 0.0, 'USD', 2974676, 3.5]`中检索值`3.5`。下面，我们用我们所学的从`data_set`中提取`3.5`:

*   我们使用`data_set[0]`检索`row_1`，并将结果赋给一个名为`fb_row`的变量。
*   我们打印`fb_row`，它输出`['Facebook', 0.0, 'USD', 2974676, 3.5]`。
*   我们使用`fb_row[-1]`从`fb_row`中检索最后一个元素(因为`fb_row`是一个列表)，并将结果赋给一个名为`fb_rating`的变量。
*   打印`fb_rating`，输出`3.5`


![selecting individual elements from a list of lists in two steps](img/94bd02fa936cc98f16ff5df1f4f5141b.png)


上面，我们分两步检索`3.5`:首先检索`data_set[0]`，然后检索`fb_row[-1]`。然而，通过链接两个索引(`[0]`和`[-1]`)，有一种更简单的方法来检索`3.5`的相同值——代码`data_set[0][-1]`检索`3.5`:


![selecting individual elements from a list of lists in one step](img/b7680fa058ab213f2d5461668b44f9d8.png)


上面，我们已经看到了两种检索值`3.5`的方法。这两种方法都产生相同的输出(`3.5`)，但是第二种方法包含的输入更少，因为它很好地结合了我们在第一种情况下看到的步骤。虽然你可以选择任何一个选项，但人们通常会选择第二个。

让我们将五个单独的列表转换成一个列表列表:

```py
app_data_set = [row_1, row_2, row_3, row_4, row_5]
print(app_data_set)
```

```py
[['Facebook', 0.0, 'USD', 2974676, 3.5],
 ['Instagram', 0.0, 'USD', 2161558, 4.5],
 ['Clash of Clans', 0.0, 'USD', 2130805, 4.5],
 ['Temple Run', 0.0, 'USD', 1724546, 4.5],
 ['Pandora - Music & Radio', 0.0, 'USD', 1126879, 4.0]]
```

## 重复列表过程

在本课之前，我们对计算应用程序的平均评分感兴趣。当我们只处理三行时，这是一个可行的任务，但是我们添加的行数越多，它就变得越困难。使用我们之前的策略，我们将:

1.  检索每个单独的评分。
2.  总结收视率。
3.  除以收视率。


![Manually calculating app average](img/32dedb86861a2cfa478818710888e98e.png)


如你所见，有了五个等级，这就变得复杂了。如果我们正在处理包含 1000 行的数据，这将需要大量不切实际的代码！我们需要找到一种简单的方法来检索许多评级。

查看上面的代码示例，我们看到一个过程一直在重复:我们为`app_data_set`中的*每个*列表选择最后一个列表元素。`app_data_set`存储五个列表，所以我们重复相同的过程五次。如果我们可以直接告诉 Python 我们想对`app_data_set`中的每个列表重复这个过程，会怎么样？

幸运的是，我们可以做到这一点——Python 为我们提供了一种简单的方法来重复一个过程，当我们需要重复一个过程数百次、数千次甚至数百万次时，这给了我们巨大的帮助。

假设我们有一个列表`[3, 5, 1, 2]`赋给了一个变量`ratings`，我们想要重复下面的过程:**对于** `ratings`中的**每个元素，打印那个元素。这就是我们如何将它翻译成 Python 语法:**


![first for loop example](img/a11b13ac0b2f127e2f626d14931e9bdf.png)


在上面的第一个例子中，我们想要重复的过程是 _“提取 `app_data_set`中每个列表**的最后一个元素**”。这就是我们如何将该过程转换成 Python 语法:****


![second for loop example](img/eb59afc1810db63b99cf323f46ceb787.png)


让我们试着更好地理解上面发生的事情。Python 一次一个地从`app_data_set`中分离出每个列表元素，并将其分配给`each_list`(它基本上变成了一个存储列表的变量——我们将在下一个屏幕中对此进行更多讨论):


![printing each item in a list using a loop](img/d9657641e1235fc7fbc58fd9683935b6.png)


上图中的代码是下面代码的一个更加简化和抽象的版本:


![manual version of printing each item in a list](img/681f12309ce2f8f191ea9680c3354bf7.png)


使用上面的技术需要我们为数据集中的每一行写一行代码。但是使用`for each_list in app_data_set`技术只需要我们写两行代码，而不管数据集中有多少行——数据集可以有五行或一百万行。

我们的中期目标是使用这种新技术来计算上述 5 行的平均评级，我们的最终目标是计算 7，197 行数据集的平均评级。在本课接下来的几个屏幕中，我们将完全做到这一点，但现在，我们将专注于练习这一技巧，以便很好地掌握它。

在编写任何代码之前，我们需要将我们想要重复的代码向右缩进四个空格字符:


![example showing loop block indentation](img/06896b9ec6a1707710ab401ed384eaf7.png)


从技术上讲，我们只需要将代码向右缩进至少一个空格，但是 Python 社区的惯例是使用四个空格。这有助于提高可读性——遵循这一约定的其他人更容易阅读您的代码，您也更容易阅读他们的代码。

让我们使用这种技术来打印每个应用程序的名称和评级:

```py
for each_list in app_data_set:
    name = each_list[0]
    rating = each_list[-1]
    print(name, rating)
```

```py
Facebook 3.5
Instagram 4.5
Clash of Clans 4.5
Temple Run 4.5
Pandora - Music & Radio 4.0

```

## Python 中的列表和 For 循环

我们刚刚学习的技术叫做**循环**。循环是一个非常有用的工具，用于执行 Python 列表的重复过程。因为我们总是从`for`(像在`for some_variable in some_list:`中)开始，这种技术被称为循环的**。**

这些是循环的**的结构部分:**


![Parts of a loop](img/ef0875463d986518a98cb42316d65af3.png)


**主体**中的缩进代码与**可迭代变量**中的元素执行相同的次数。如果 iterable 变量是一个包含三个元素的列表，那么主体中的缩进代码将被执行三次。我们称每一次代码执行为一次**迭代**，因此对于一个包含三个元素的列表将有三次迭代。对于每次迭代，**迭代变量**将采用不同的值，遵循以下模式:

*   对于第一次迭代，值是 iterable 的第一个元素(来自上面的例子，`1`)。
*   对于第二次迭代，值是 iterable 的第二个元素(来自上面的例子，`3`)。
*   对于第三次迭代，该值是 iterable 的第三个元素(来自上面的示例，`5`)。


![step-by-step iteration of a loop](img/24ab8cb654a0ad5663d858d9528eb759.png)


交互变量的名称可以是您喜欢的任何名称——如果您将上面代码中的`value`替换为`dog`,代码将以完全相同的方式工作。也就是说，使用有助于传达数据内容的东西是一种惯例。

循环体外的代码*可以与循环体*内的代码*交互。例如，在下面的代码中，我们:*

*   在循环体之外，用零值*初始化变量`a_sum`。*
*   我们**在`a_list`上循环**(或者**迭代**)。对于循环的每次迭代，我们:
    *   在迭代变量`value`的当前值和`a_sum`中存储的当前值之间执行加法(*在*循环体内)(`a_sum`在循环体外定义)。
    *   将加法的结果赋回`a_sum`(循环体内)。
    *   打印`a_sum`变量的值(在循环体内)。请注意，`a_sum`的值在每次添加后都会发生变化。在循环结束时，`a_sum`的值为`9`，它相当于`a_list` ( `1 + 3 + 5`)中数字的总和。


![summing with a loop](img/3fc20a9cd3ad6dd67f3eb9efdef4d072.png)


上图中，我们创建了一种对列表中的数字求和的方法。我们可以使用这种技术来总结数据集中的评分。一旦我们有了总和，我们只需要除以收视率的数量就可以得到平均值。

```py
rating_sum = 0
for row in app_data_set:
    rating = row[-1]
    rating_sum = rating_sum + rating

avg_rating = rating_sum / len(app_data_set)
print(avg_rating)
```

```py
4.2

```

我们已经在这里介绍了 for 循环的基础知识，但是如果你想要更多的练习，我们也有关于 [for 循环基础知识](https://www.dataquest.io/blog/python-for-loop-tutorial/)和[for 循环高级知识](https://www.dataquest.io/blog/tutorial-advanced-for-loops-python-pandas/)的教程，你可以看看。

## 计算列表平均值的替代方法

现在，我们将学习另一种计算平均评分值的方法。一旦我们创建了一个列表，我们可以使用`append()`命令向它添加(或**追加**值)。


![Using append to add values to a list](img/b18001dc35a0502cb1d15eb57a8d416c.png)


与我们所学的其他命令不同，请注意`append()`有一个特殊的语法用法，遵循模式`list_name.append()`而不是简单地用作`append()`。

既然我们知道了如何将值追加到列表中，我们可以采取以下步骤来计算平均应用评级:

1.  我们初始化一个空列表。
2.  我们开始循环我们的数据集，并*提取*评级。
3.  我们*将评级追加*到我们在第一步创建的空列表中。
4.  一旦我们有了所有的评级，我们:
    *   使用`sum()`命令对所有评级进行求和(为了能够使用`sum()`，我们需要将评级存储为浮点数或整数)；然后
    *   我们将总和除以评级数量(可以使用`len()`命令获得)。

下面，我们可以看到为我们的五行数据集实现的上述步骤:


![Using append to extract values and calculate an average](img/fa1f1bd86a25ae65e1daf95e27304486.png)


我们还可以使用`append()`通过将数据作为列表追加来将另一行添加到我们的列表中。让我们看看它是如何工作的:

```py
row_6 = ['Pinterest', 0.0, 'USD', 1061624, 4]
app_data_set.append(row_6)

print(app_data_set)
```

```py
[['Facebook', 0.0, 'USD', 2974676, 3.5],
 ['Instagram', 0.0, 'USD', 2161558, 4.5],
 ['Clash of Clans', 0.0, 'USD', 2130805, 4.5],
 ['Temple Run', 0.0, 'USD', 1724546, 4.5],
 ['Pandora - Music & Radio', 0.0, 'USD', 1126879, 4.0],
 ['Pinterest', 0.0, 'USD', 1061624, 4],
 ['Pinterest', 0.0, 'USD', 1061624, 4]]
```

现在，让我们使用上面学到的技术来计算所有六款应用的平均评分:

```py
all_ratings = []

for row in app_data_set:
    rating = float(row[-1])
    all_ratings.append(rating)

avg_rating = sum(all_ratings) / len(all_ratings)
print(avg_rating)
```

```py
4.166666666666667

```

## 接下来的步骤

在本教程中，我们学习了如何:

*   使用 Python 列表存储和处理数据
*   使用正负索引访问存储在列表中的值
*   使用列表列表来处理表格数据
*   使用 for 循环来自动执行重复性任务
*   向列表追加值

如果你想练习使用 Python 列表，这个教程是基于我们免费的 Python 基础课程的一部分。该课程可以在您的网络浏览器上进行，您将编写代码来分析超过 7，000 个移动应用程序的完整数据集！

## 这个教程有帮助吗？

选择你的道路，不断学习有价值的数据技能。

![arrow down left](img/2215dd1efd21629477b52ea871afdd98.png)![arrow right down](img/2e703f405f987a154317ac045ee00a68.png)[Python Tutorials](/python-tutorials-for-data-science/)

在我们的免费教程中练习 Python 编程技能。

[Data science courses](/data-science-courses/)

通过我们的交互式浏览器数据科学课程，投入到 Python、R、SQL 等语言的学习中。