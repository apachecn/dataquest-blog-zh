# 如何在 2022 年使用 Python 计数器(带有 23 个代码示例)

> 原文：<https://www.dataquest.io/blog/how-to-use-python-counters/>

November 7, 2022![Python counters](img/dadd18eecb399f118f96673aeb725f67.png)

作为 Python 开发人员，我们可能需要开发一段代码来同时计算几个重复的对象。有不同的方法来执行这项任务。然而，Python 的`collections`模块中的`Counter`类提供了最有效和直接的解决方案来计算 iterable 对象中的单个项目。

在本教程中，我们将学习`Counter`类以及在 Python 中使用它的基础知识。
完成本教程后，您将会对以下内容有很好的理解

*   什么是`Counter`类。
*   如何创建一个`Counter`对象？
*   如何更新现有的`Counter`对象。
*   如何确定 iterable 对象中最频繁出现的项？
*   如何组合或减去`Counter`对象。
*   如何对两个`Counter`对象进行并集和交集运算？

我们假设您了解 Python 的基础知识，包括基本的数据结构和函数。如果你不熟悉这些或者渴望提高你的 Python 技能，你可能想试试我们的[用于数据分析的 Python 基础——Data quest](https://www.dataquest.io/path/python-basics-for-data-analysis/)。

## Python 的`Counter`类是什么

Python 的`Counter`是 dictionary 数据类型的一个子类，用于通过创建一个字典来对可散列对象进行计数，其中 iterable 的元素存储为键，它们的计数存储为值。
换句话说，`Counter`的构造函数接受一个 iterable 对象，并返回一个字典，其中包含 iterable 对象中每一项的频率，只要这些对象是可散列的。
好消息是一些算术运算适用于`Counter`对象。

在下一节中，我们将讨论使用`Counter`来确定列表、字符串和其他可迭代对象中可哈希对象的频率。

## 使用计数器对象

如前所述，`Counter`对象提供了一种快速计算 iterable 对象中项的简单方法。
为了演示`Counter`对象如何工作并显示其结果，让我们从从`collections`模块导入`Counter`类开始，然后将它应用于一个字符串，如下所示:

```py
from collections import Counter
a_str = 'barbara'
counter_obj = Counter(a_str)
print(counter_obj)
```

```py
 Counter({'a': 3, 'b': 2, 'r': 2})
```

如图所示，上面的代码计算单词中的字母。让我们通过在前面的代码中添加以下几行来使输出更有吸引力。

```py
for item in counter_obj.items():
    print("Item: ", item[0]," Frequency: ", item[1])
```

```py
 Item:  b  Frequency:  2
    Item:  a  Frequency:  3
    Item:  r  Frequency:  2
```

`Counter`类实现了有用的方法，比如`update()`和`most_common([n])`。`update()`方法获取一个 iterable 对象，并将其添加到现有的 counter 对象中。
让我们试试吧:

```py
counter_obj.update("wallace")
print(counter_obj)
```

```py
 Counter({'a': 5, 'b': 2, 'r': 2, 'l': 2, 'w': 1, 'c': 1, 'e': 1})
```

`most_common([n])`方法返回一个有序的元组列表，其中包含`n`个最常见的
项及其计数。

```py
print(counter_obj.most_common(1))
```

```py
 [('a', 5)]
```

上面的代码返回了`a_str`对象中最频繁出现的项目。

为了更好地理解`Counter`对象的用法，让我们来确定下列句子中出现最频繁的项目:

> “恐惧导致愤怒；愤怒导致仇恨；仇恨导致冲突；冲突导致痛苦。”下面是我们的做法:

```py
quote_1 = "Fear leads to anger; anger leads to hatred; hatred leads to conflict; conflict leads to suffering."
words_1 = quote_1.replace(';','').replace('.','').split()
word_counts = Counter(words_1)
the_most_frequent = word_counts.most_common(1)
print(the_most_frequent)
```

```py
 [('leads', 4)]
```

在上面的代码中，`Counter`对象生成一个字典，将输入序列中的可散列项映射到出现的次数，如下所示:

```py
{
 'Fear': 1,
 'leads': 4,
 'to': 4,
 'anger': 2,
 'hatred': 2,
 'conflict': 2,
 'suffering': 1
}
```

正如本教程第一部分所提到的，`Counter`对象的一个有价值的特性是它们可以用数学运算来组合。例如:

```py
from pprint import pprint
quote_2 = "Fear, anger, and hatred essentially come from a lack of perspective as to what life is all about."
words_2 = quote_2.replace(',','').replace('.','').split()

dict_1 = Counter(words_1)
dict_2 = Counter(words_2)
pprint(dict_1)
print()
pprint(dict_2)
```

```py
 Counter({'leads': 4,
             'to': 4,
             'anger': 2,
             'hatred': 2,
             'conflict': 2,
             'Fear': 1,
             'suffering': 1})

    Counter({'Fear': 1,
             'anger': 1,
             'and': 1,
             'hatred': 1,
             'essentially': 1,
             'come': 1,
             'from': 1,
             'a': 1,
             'lack': 1,
             'of': 1,
             'perspective': 1,
             'as': 1,
             'to': 1,
             'what': 1,
             'life': 1,
             'is': 1,
             'all': 1,
             'about': 1})
```

现在，我们能够合并或减去这两个计数器实例。让我们试试它们:

```py
pprint(dict_1 + dict_2)
```

```py
 Counter({'to': 5,
             'leads': 4,
             'anger': 3,
             'hatred': 3,
             'Fear': 2,
             'conflict': 2,
             'suffering': 1,
             'and': 1,
             'essentially': 1,
             'come': 1,
             'from': 1,
             'a': 1,
             'lack': 1,
             'of': 1,
             'perspective': 1,
             'as': 1,
             'what': 1,
             'life': 1,
             'is': 1,
             'all': 1,
             'about': 1})
```

```py
pprint(dict_1 - dict_2)
```

```py
 Counter({'leads': 4,
             'to': 3,
             'conflict': 2,
             'anger': 1,
             'hatred': 1,
             'suffering': 1})
```

到目前为止，我们已经尝试了带有字符串值的`Counter`对象。同样，我们可以将 list、tuple 或 dictionary 对象给`Counter`，并计算它们出现的频率。让我们在接下来的部分中探索它们。

## Python 的`Counter`包含列表、元组和字典对象

为了探索如何将赋予 Python 的`Counter`的 list、tuple 和 dictionary 对象转换为键值对形式的可散列对象，让我们考虑一家销售三种颜色(绿色、白色和红色)t 恤的精品店。

以下 iterable 对象显示了连续两天的 t 恤销售额，如下所示:

```py
sales_day_1 = ['red','green','white','red','red','green', 'white','green','red','red','red', 'white', 'white']
sales_day_2 = ('red','red','green','white','green','white','red','red','green', 'white','green','red','green','red','red')
price = {'red':74.99, 'green':83.99, 'white':51.99}

sales_day_1_counter = Counter(sales_day_1)
sales_day_2_counter = Counter(sales_day_2)
price_counter = Counter(price)

total_sales = sales_day_1_counter + sales_day_2_counter
sales_increment = sales_day_2_counter - sales_day_1_counter
minimum_sales = sales_day_1_counter & sales_day_2_counter
maximum_sales = sales_day_1_counter | sales_day_2_counter

print("Total Sales:", total_sales)
print("Sales Increment over Two Days:", sales_increment)
print("Minimun Sales:", minimum_sales)
print("Maximum Sales:", maximum_sales)
```

```py
 Total Sales: Counter({'red': 13, 'green': 8, 'white': 7})
    Sales Increment over Two Days: Counter({'green': 2, 'red': 1})
    Minimun Sales: Counter({'red': 6, 'green': 3, 'white': 3})
    Maximum Sales: Counter({'red': 7, 'green': 5, 'white': 4})
```

在上面的例子中，`sales_increment`计数器不包含白衬衫，因为计数器对象不显示负值。这就是为什么柜台里只有绿色和红色的衬衫。白衬衫的数量减少到-1 并被忽略。

我们在上面的代码中使用了`&`和`|`操作符来返回每件 t 恤颜色的最小和最大销售额。这里就来讨论一下这两个算子。交集运算符(`&`)返回两个计数器中计数最小的对象，而并集运算符(`|`)返回两个计数器中计数最大的对象。
要计算每种颜色的总销售额，我们可以使用下面的代码片段:

```py
for color,quantity in total_sales.items():
    print(f"Total sales of '{color}' T-shirts: ${quantity*price_counter[color]:.2f}")
```

```py
 Total sales of 'red' T-shirts: $974.87
    Total sales of 'green' T-shirts: $671.92
    Total sales of 'white' T-shirts: $363.93
```

为了检索用于创建`Counter`的原始数据，我们可以使用`elements()`方法，如下所示:

```py
for item in sales_day_1_counter.elements():
    print(item)
```

```py
 red
    red
    red
    red
    red
    red
    green
    green
    green
    white
    white
    white
    white
```

上面的输出显示，计数器的元素以与`sales_day_1_counter`对象相同的顺序返回。

## 结论

本教程讨论了 Python 的`Counter`，它提供了一种有效的方法来计算 iterable 对象中的项数，而无需处理循环和不同的数据结构。我们还了解了`Counter`的方法和操作符，这些方法和操作符帮助我们从存储在集合中的数据中获取最大的洞察力。

我希望你今天学到了一些新东西。请随时在 LinkedIn 或 Twitter 上与我联系。