# 教程:如何用 Python 编写 For 循环

> 原文：<https://www.dataquest.io/blog/tutorial-how-to-write-a-for-loop-in-python/>

January 14, 2022![How to Write a For Loop in Python](img/0d7a7541b648576050e412b92a197213.png)

我们在日常生活中遇到的大多数任务都是重复的。虽然这些任务对人类来说可能会变得无聊，但计算机可以快速有效地处理重复的任务。我们将在本教程中学习如何操作。

本教程是为 Python 初学者编写的，但是如果你以前从未写过一行代码，你可能想要完成[我们免费的 Python 基础课程](https://www.dataquest.io/course/python-for-data-science-fundamentals/)，因为我们在这里不会涉及基本语法。
为了理解计算机是如何帮助我们完成重复性任务的，让我们通过一个场景将一万人按年龄分组。让我们假设这个过程需要两步:

1.  查一下这个人的年龄
2.  在适当的年龄组中记录

为了成功地将人口中的每个成员登记到他们的年龄组中，我们将完成一万次两步过程。当我们成功地完成这个过程一次，我们就迭代了一次。当我们对整个群体完成这个过程时，我们已经迭代了 10，000 次。

**迭代**是指多次执行这个两步过程。一次迭代之后，我们从头开始重复这个过程，形成一个**循环**。因此，循环是实现迭代的设置。

有两种类型的迭代:确定的和不确定的。使用**确定的**迭代，你可以遍历一个固定大小的对象。这意味着您从一开始就知道程序将执行的迭代次数。如果你从一开始就不知道迭代的次数，你需要某种形式的条件逻辑来终止循环，这就是一个**不定**迭代。

## Python 中的 For 循环是什么？

在 Python 中，我们使用 for 循环来遍历可迭代对象和迭代器。这是一个明确迭代的示例，简单 Python for 循环操作的语法如下所示:

```py
for item in iterable:
statement
```

并非所有数据类型都支持这种操作。我们称支持循环操作的数据类型为 iterables。可重复项包括字符串、列表、集合、冷冻集、字典、元组、数据帧、范围和数组。Iterables 是可以实现`__iter__()`和`__next()__`方法的对象。

迭代器也支持循环操作。迭代器是在 iterable 上调用`__iter__()`方法时形成的数据类型。因此，所有的可迭代对象都有它们的迭代器。然而，有些数据类型在默认情况下是迭代器。示例包括 zip、枚举和生成器。迭代器只能实现`__next__()`方法。

需要多个 for 循环的操作嵌套在 for 循环操作中。带有两个 for 循环的操作的语法如下所示:

```py
for item_one in iterable_one:
    for item_two in iterable_two:
        statement
```

for 循环有一个可选的 else 块，在 for 循环操作结束时执行。带有两个 for 循环操作的 for-else 语法如下所示:

```py
for item_one in iterable_one:
    for item_two in iterable_two:
        statement:
    else:
        statement
else:
    statement
```

要将 iterable 转换为 iterator 对象，我们可以调用`__iter__()`方法或`iter()`函数。这两种操作的语法略有不同，但它们实现了相同的结果:

```py
# Using the __iter__() method
iterable.__iter__()

# Using the iter() function
iter(iterable)
```

`iter()`函数调用`__iter__()`方法来执行这个转换。

当我们有一个迭代器对象时，我们可以用 `__next__()`方法或`next()`函数返回迭代器对象中的项目。该操作的语法如下所示:

```py
# Using the __next__() method
iterator.__next__()

# Using the next() function
next(iterator, default)
```

`next()`函数调用`__next__()`方法返回迭代器中的下一项。当你穷尽了迭代器中的所有条目后，`__next__()`方法会抛出 StopIteration 异常，让你知道迭代器现在是空的。如果使用默认值调用`next()`函数，则不会引发该异常。而是返回默认值。

Python 中的 for 循环操作类似于一次或多次调用`__iter__()`方法和`__next__()`方法，直到迭代器中的项目用尽，我们将在下面几节中看到

## 遍历一个字符串

字符串是不可变数据类型的例子。一旦创建，其值就不能更新。我们发现，对于一个大小确定的对象，可以使用 for 循环。我们使用`len()`函数来获取字符串的大小。

```py
string = "greatness"
len(string)  # 9
```

我们还声称字符串是可迭代的，它们可以实现 `__iter__()`和`__next__()`方法。下面的代码片段显示了这些方法的实现:

```py
string = "greatness"
string_iter = string.__iter__()
print(string_iter.__next__())
print(string_iter.__next__())
print(string_iter.__next__()) 
```

输出

g
r
e

`__iter__()`方法将数据类型转换为字符串迭代器，`__next__()`方法一个接一个地返回字符串中的项。如果您使用`__next__()`方法足够多次，您将返回字符串中的所有项目。

我们可以使用 for 循环执行类似的操作。有两种方法可以做到这一点。在第一种方法中，您遍历字符串:

```py
# Looping through a string
for item in string:
    print(item)
```

输出

g
r
e
a
t
n
e
s
s

在第二种方法中，循环遍历字符串的索引，并使用索引值访问字符串中的项。我们用 range 函数从字符串的大小得到它的索引。

输出

g
r
e
a
t
n
e
s
s

## 遍历列表

与字符串不同，列表是可变的。像字符串一样，它们是可重复的。因此，列表实现了`__len__()`、`__iter__()`和`__next__()`方法:

```py
list_string = ['programming', 'marathon', 'not', 'sprint']

print(list_string.__len__())

list_iter = list_string.__iter__()
print(list_iter.__next__())
print(list_iter.__next__())
print(list_iter.__next__())
print(list_iter.__next__())
```

输出

4
编程
马拉松
不
短跑

`__iter__()`方法将列表对象转换为列表迭代器，`__next__()`方法一个接一个地返回迭代器中的项目。我们可以使用 for 循环对列表执行类似的操作:

```py
for item in list_string:
    print(item)
```

输出

编程
马拉松
不是
短跑

## 遍历一个元组

元组是由逗号分隔的对象的集合。它的操作非常类似于列表，但是元组是不可变的。元组是可迭代的，它实现了`__len__()`、`__iter__()`和`__next__()`方法:

```py
tuple_string = 'programming', 'marathon', 'not', 'sprint'

print(tuple_string.__len__())

tuple_iter = tuple_string.__iter__()
print(tuple_iter.__next__())
print(tuple_iter.__next__())
print(tuple_iter.__next__())
print(tuple_iter.__next__())
```

输出

4
编程
马拉松
不
短跑

`iter()`函数将元组对象转换为元组迭代器，`next()`函数一个接一个地返回迭代器中的项。我们可以用 for 循环执行类似的操作:

```py
for item in tuple_string:
    print(item)
```

输出

编程
马拉松
不是
短跑

## 遍历 Set 和 Frozenset

在 Python 中，set 是一个可变对象。Frozenset 与 set 具有相似的属性，但它是不可变的。Set 和 frozenset 有确定的大小，它们是可迭代的。因此，在 set 和 frozenset 对象上使用`next()`和`iter()`函数的组合会产生类似于 for 循环的结果:

```py
set_object = set(['programming', 'marathon', 'not', 'sprint'])

set_iter = iter(set_object)
print(next(set_iter))
print(next(set_iter))
print(next(set_iter))
print(next(set_iter))

frozenset_object = frozenset(['programming', 'marathon', 'not', 'sprint'])

frozenset_iter = iter(frozenset_object)
print(next(frozenset_iter))
print(next(frozenset_iter))
print(next(frozenset_iter))
print(next(frozenset_iter))

for item in set_object:
    print(item)

for item in frozenset_object:
    print(item)
```

输出

不是
编程
冲刺
马拉松

`iter()`函数将 set 和 frozenset 对象转换为集合迭代器，而`next()`函数一个接一个地返回集合迭代器中的项。

## 遍历字典

Python 字典是将数据存储为键值对的对象。字典的键必须是不可变的数据类型。因此，字符串、整数和元组是合适的字典键。字典中的条目可以用它们的关键字来评估。像之前讨论的 iterables 一样，字典有一个确定的大小，并实现了`iter()`和`next()`函数:

```py
dict_score = {'Kate': [85, 90, 95], 'Maria':[92, 90, 94], 'Ben':[92, 85, 91]}

print(len(dict_score))

dict_iter = iter(dict_score)
print(next(dict_iter))
print(next(dict_iter))
print(next(dict_iter))
```

输出

凯特玛利亚 T2 本

函数将 dictionary 对象转换成 dictionary_keyiterator。从新对象的名称和输出中可以注意到，下一个操作迭代了字典键。我们可以使用 for 循环执行类似的操作:

```py
for key in dict_score:
    print(key)
```

输出

凯特玛丽亚本

还可以使用以下语法迭代字典值:

```py
for value in dict_score.values():
    print(value)
```

输出

[85，90，95]
【92，90，94】
【92，85，91】

上述代码片段的输出是一个 Python 列表，它是一个可迭代的列表。当我们有两个 iterables 时，我们可以应用嵌套的 for 循环语法:

```py
for value in dict_score.values():
    for item in value:
        print(item)
```

输出

85
90
95
92
90
94
92
85
91

我们已经看到了如何分别迭代字典的键和值的例子。但是，我们可以使用以下语法同时完成这两项操作:

```py
for key, value in dict_score.items():
    print(key)
    for item in value:
        print(item)
```

输出

凯特
85
90
95
玛利亚
92
90
94
本
92
85
91

## 遍历 Zip

我们使用`zip()`函数进行并行迭代。这意味着您可以同时迭代两个或更多的 iterables。它返回一个元组，其中包含通过它传递的 iterables 的并行项:

```py
import string

letters = string.ascii_lowercase[:5]  # abcde
index = range(len(letters))  # 01234

for tup in zip(index, letters):
    print(tup)
```

输出

(0，' a')
(1，' b')
(2，' c')
(3，' d')
(4，' e ')

与我们看到的用`iter()`函数将可迭代对象转换成迭代器的其他可迭代对象不同，`zip()`函数不需要这一步，因为它是迭代器。它使用`next()`函数以元组的形式一个接一个地返回项目

```py
zip_iter = zip(index, letters)
print(next(zip_iter))
print(next(zip_iter))
print(next(zip_iter))
```

输出

(0，' a')
(1，' b')
(2，' c ')

我们还可以使用`zip()`函数对不同大小的迭代进行并行迭代:

```py
letters = ['a', 'b', 'c']  # length equals 3
numbers = [1, 2, 3, 4]  # length equals 4

for tup in zip(letters, numbers):
    print(tup)
```

输出

(' a '，1)
('b '，2)
('c '，3)

结果的大小等于最小的 iterable 的大小，如上面的代码片段所示。

## 遍历数据帧

pandas 数据帧是可变的二维表格数据结构。它是可迭代的，实现了`len()`、`iter()`和`next()`函数:

```py
import pandas as pd

df = pd.DataFrame({
    'Physics': [90, 92, 89, 94],
    'Math': [100, 98, 100, 99]}, 
    index=['Oliver', 'Angela', 'Coleman', 'Agatha'])

df_iter = iter(df)
print(next(df_iter))
print(next(df_iter))
```

输出

物理
数学

函数将一个数据帧转换成一个 map 对象，它是一个迭代器。`next()`函数返回列名。这类似于使用 for 循环:

```py
for col in df:
    print(col)
```

输出

物理
数学

我们可能想要迭代数据帧的行，而不是它们的列名。我们可以用`df.iterrows()`方法来实现。它从索引开始返回每行的元组:

```py
for row in df.iterrows():
    print(row)
```

输出

('奥利弗'，物理 90
数学 100
姓名:奥利弗，dtype: int64)
('安吉拉'，物理 92
数学 98
姓名:安吉拉，dtype: int64)
('科尔曼'，物理 89
数学 100
姓名:科尔曼，dtype: int64)
('阿加莎'，物理 94
数学 99
姓名:阿加莎，dtype: int64)

## 遍历枚举

像`zip()`函数一样，`enumerate()`函数也是一个迭代器。因此，它不需要`iter()`功能来操作。我们用它来迭代一个 iterable。它返回一个包含索引和与该索引相关联的值的元组。我们使用 start 属性来设置起始索引值

```py
list_string = ['programming', 'marathon', 'not', 'sprint']

list_enumerate = enumerate(list_string, start=5)
print(next(list_enumerate))
print(next(list_enumerate))
print(next(list_enumerate))
print(next(list_enumerate))
```

输出

(5，'编程')
(6，'马拉松')
(7，'非')
(8，'冲刺')

我们可以使用 for 循环得到类似的结果:

```py
for item in enumerate(list_string, start=5):
    print(item)
```

输出

(5，'编程')
(6，'马拉松')
(7，'非')
(8，'冲刺')

## 遍历生成器

在 Python 中，生成器是一个特殊的函数，它使用 yield 而不是 return 语句。我们称之为懒惰迭代器，因为它不会将所有内容都存储在内存中；当调用`next()`函数时，它一个接一个地返回它们:

```py
def square_generator(n):
    for i in range(1, n+1):
        yield i**2

squares = square_generator(4)
print(next(squares))
print(next(squares))
print(next(squares))
print(next(squares))
```

输出

1
4
9
16
我们可以用 for 循环实现相同的结果:

```py
for item in square_generator(4):
    print(item)
```

输出

1
4
9
16

## 对于循环和列表的理解

列表理解是一种从任何 iterable 创建列表的方法。这是一种简化以 append 语句结束的 for 循环的有趣方法。

```py
import string

string_iter = string.ascii_lowercase[:5]

ord_list = []
for item in string_iter:
    ord_list.append(ord(item))

print(ord_list)
```

输出

[97, 98, 99, 100, 101]

上面的代码片段写为一个列表理解看起来像这样:

```py
ord_list = [ord(item) for item in string_iter]
print(ord_list)
```

输出

[97, 98, 99, 100, 101]

我们使用列表理解来简化嵌套的 for 循环，如下所示:

```py
multiples_two = [2, 4, 6, 8]
multiples_three = [3, 6, 9, 12]

result = []
for first in multiples_two:
    for second in multiples_three:
        result.append(first*second)     
print(result)
```

输出

[6, 12, 18, 24, 12, 24, 36, 48, 18, 36, 54, 72, 24, 48, 72, 96]

根据列表理解，我们使用以下代码片段:

```py
result = [first * second for first in multiples_two for second in multiples_three]
print(result)
```

输出

[6, 12, 18, 24, 12, 24, 36, 48, 18, 36, 54, 72, 24, 48, 72, 96]

集合理解和列表理解非常相似。输出数据类型是集合而不是列表。表示为集合理解的嵌套 for 循环示例是:

```py
result = {first * second for first in multiples_two for second in multiples_three}
print(result)
```

输出

{6, 12, 18, 24, 36, 48, 54, 72, 96}

集合理解的结果比列表理解的结果小；这是因为器械包中的物品不会重复。

## For 循环中的 Break 和 Continue 关键字

有时，我们希望在满足某个条件时终止循环的执行。我们使用 break 关键字来中断循环。例如:

```py
result = [6, 12, 18, 24, 12, 24, 36, 48, 18, 36, 54, 72, 24, 48, 72, 96]

for item in result:
    if item > 40:
        break
    print(item)
```

输出

6
12
18
24
12
24
36

break 关键字停止 for 循环的执行，但有时我们可能希望跳过满足特定条件的项，继续我们的迭代。这就是 continue key 工作的作用。

```py
for item in result:
    if item > 40:
        continue
    print(item)
```

输出

6
12
18
24
12
24
36
18
36
24

## For-Else 循环

for 循环有一个可选的 else 语句。如果 for 循环在执行过程中没有遇到 break 关键字，else 块就会执行。例如:

```py
result = [6, 12, 18, 24, 12, 24, 36, 48, 18, 36, 54, 72, 24, 48, 72, 96]

for item in result:
    if item > 40:
        break
    print(item)
else:
    print('For loop does not encounter break keyword')
```

输出

6
12
18
24
12
24
36

在上面的代码片段中，for 循环遇到了 break 关键字，并终止了循环的执行。因此，else 块不会执行。如果没有 break 关键字，else 块将在 for 循环之后执行:

```py
for item in result:
    if item > 40:
        continue
    print(item)
else:
    print('For loop does not encounter break keyword')
```

输出

6
12
18
24
12
24
36
18
36
24

For 循环没有遇到 break 关键字

## 结论

我们使用 for 循环来遍历一个大小确定的 iterable。`__int__()`方法将 iterable 转换成它的迭代器，而`__next__()`方法一个接一个地返回 iterable 中的项目。for 循环操作类似于在 iterable 上调用`__iter__()`方法来获取迭代器，以及一次或多次调用`__next__()`方法直到迭代器为空。我们讨论了简单的和嵌套的 for 循环，for 循环如何用于不同的数据类型，以及 break 和 continue 关键字如何在循环中工作。

如果您想继续探索 Python 中的循环，请查看另一个 Dataquest 初学者教程。要更深入地了解 Python 中的 For 循环，请查看[这个 Dataquest 教程](https://www.dataquest.io/blog/tutorial-advanced-for-loops-python-pandas/)。