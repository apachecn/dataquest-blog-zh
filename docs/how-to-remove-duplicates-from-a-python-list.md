# 如何轻松地从 Python 列表中移除重复项(2022)

> 原文：<https://www.dataquest.io/blog/how-to-remove-duplicates-from-a-python-list/>

September 12, 2022![Removing duplicates from a list in Python](img/a1d1a79aefb3d0664d247c86e846fd1b.png)

Python 列表是有序的、零索引的、可变的对象集合。我们通过将相似或不同数据类型的对象放在用逗号分隔的方括号中来创建它。关于 Python 列表如何工作的复习，请参考这个 [DataQuest 教程](https://www.dataquest.io/blog/python-list-tutorial/)。

从列表中删除重复项是一个重要的数据预处理步骤，例如，确定上个月从礼品店购买促销产品的唯一客户。有几个顾客可能在过去的一个月里不止一次从这家店买过礼物，他们的名字会随着他们光顾这家店的次数而出现。

在本教程中，我们将学习从 Python 列表中移除重复项的不同方法。

* * *

## 1.使用`del`关键字

我们使用`del`关键字从列表中删除对象及其索引位置。当列表很小并且没有很多重复元素时，我们使用这种方法。例如，一个由六名学生组成的班级被问及他们最喜欢的编程语言，他们的回答被保存在`students`列表中。几个学生喜欢相同的编程语言，所以我们在`students`列表中有重复的，我们将使用`del`关键字删除。

```
students = ['Python', 'R', 'C#', 'Python', 'R', 'Java']

# Remove the `Python` duplicate with its index number: 3
del students[3]

print(students)
```

```
 ['Python', 'R', 'C#', 'R', 'Java']
```

```
# Remove the `R` duplicate with its index number: 3
del students[3]

print(students)
```

```
 ['Python', 'R', 'C#', 'Java']
```

我们成功地从列表中删除了重复项。但是为什么我们两次使用索引 3 呢？原`students`列表的`len`为 6。Python 列表是零索引的。列表中的第一个元素的索引为 0，最后一个元素的索引为 5。副本`'Python'`的索引为 3。删除`'Python'`副本后，`students`列表的`len`减 1。副本`'Python'`之后的元素现在呈现其索引位置。这就是为什么重复的`'R'`索引从 4 变为 3。使用这种方法的缺点是，我们必须跟踪不断变化的副本索引。这对于一个非常大的列表来说是很困难的。

接下来，我们将使用`for-loop`更有效地从列表中删除重复项。

* * *

## 2.使用`for-loop`

我们使用`for-loop`来迭代一个 iterable:例如，一个 Python 列表。关于`for-loop`的工作原理，请参考 DataQuest 博客上的 [for-loop 教程](https://www.dataquest.io/blog/tutorial-how-to-write-a-for-loop-in-python/)。

要使用`for-loop`删除重复项，首先创建一个新的空列表。然后，迭代包含重复项的列表中的元素，并在新列表中只追加每个元素的第一个匹配项。下面的代码展示了如何使用`for-loop`从`students`列表中删除重复项。

```
# Using for-loop
students = ['Python', 'R', 'C#', 'Python', 'R', 'Java']

new_list = []

for one_student_choice in students:
    if one_student_choice not in new_list:
        new_list.append(one_student_choice)

print(new_list)
```

```
 ['Python', 'R', 'C#', 'Java']
```

瞧！我们成功地删除了重复项，而无需跟踪元素的索引。这种方法可以帮助我们删除大型列表中的重复项。然而，这需要大量的代码。应该有更简单的方法。有什么猜测吗？

**列表理解！**在下一个例子中，我们将使用列表理解来简化上面的代码:

```
# Using list comprehension
new_list = []

[new_list.append(item) for item in students if item not in new_list]

print(new_list)
```

```
 ['Python', 'R', 'C#', 'Java']
```

我们用更少的代码行完成了工作。我们可以将`for-loop`与`enumerate`和`zip`函数结合起来，编写奇异的代码来消除重复。这些代码的工作原理与上面的例子是一样的。

接下来，我们将看到如何在不使用`set`迭代的情况下从列表中删除重复项。

* * *

## 3.使用`set`

Python 中的集合是唯一元素的无序集合。就其本质而言，重复是不允许的。因此，将列表转换为集合会删除重复项。将集合更改为列表会生成一个没有重复项的新列表。

以下示例显示了如何使用`set`从`students`列表中删除重复项。

```
# Removing duplicates by first changing to a set and then back to a list
new_list = list(set(students))

print(new_list)
```

```
 ['R', 'Python', 'Java', 'C#']
```

请注意，列表中元素的顺序与我们之前的示例不同。这是因为`set`不能维持秩序。接下来，我们将看到如何使用字典从列表中删除重复项。

* * *

## 4.使用`dict`

Python 字典是键值对的集合，要求键必须是唯一的。因此，如果我们能使列表的元素成为字典的键，我们将删除 Python 列表中的重复项。我们不能将简单的`students`列表转换成字典，因为字典是用键值对创建的。如果我们试图将`students`列表转换成一个字典，我们会得到以下错误:

```
# We get ValueError when we try to convert a simple list into a dictionary

print(dict(students))
```

```
 ---------------------------------------------------------------------------

 ValueError                                Traceback (most recent call last)

<ipython-input-6-43bfe4b3db83> in <module>
1 # We get ValueError when we try to convert a simple list into a dictionary
2 
----> 3 dict(students)

ValueError: dictionary update sequence element #0 has length 6; 2 is required
```

然而，我们可以从元组列表中创建一个字典——之后，我们将获得字典的唯一键，并将它们转换成一个列表。从`students`列表中获取元组列表的矢量化方法是使用 map 函数:

```
# Convert `students` list into a list of tuples
list_of_tuples = list(map(lambda x: (x, None), students))

print(list_of_tuples, end='\n\n')
```

```
 [('Python', None), ('R', None), ('C#', None), ('Python', None), ('R', None), ('Java', None)]
```

在上面的代码块中，`students`列表中的每个元素都通过`lambda`函数来创建一个元组`(element, None)`。当元组列表变成字典时，元组中的第一个元素是键，第二个元素是值。使用`keys()`方法从字典中提取唯一键，并将其转换为一个列表:

```
# Convert list of tuples into a dictionary
dict_students = dict(list_of_tuples)

print('The resulting dictionary from the list of tuples:')
print(dict_students, end='\n\n')
```

```
 The resulting dictionary from the list of tuples:
    {'Python': None, 'R': None, 'C#': None, 'Java': None}
```

```
# Get the unique keys from the dictionary and convert into a list
new_list = list(dict_students.keys())

print('The new list without duplicates:')
print(new_list, end='\n\n')
```

```
 The new list without duplicates:
    ['Python', 'R', 'C#', 'Java']
```

`dict.fromkeys()`方法将一个列表转换成一个元组列表，并将元组列表转换成一个字典。然后，我们可以获得唯一的字典键，并将它们转换成一个列表。然而，在转换成列表之前，我们使用了`dict.keys()`方法从字典中获取惟一键。这真的没有必要。默认情况下，字典上的操作(如迭代和转换为列表)使用字典键。

```
# Using dict.fromkeys() methods to get a dictionary from a list
new_dict_students = dict.fromkeys(students)

print('The resulting dictionary from the dict.fromkeys():')
print(new_dict_students, end='\n\n')

print('The new list without duplicates using dict.fromkeys():')
print(list(new_dict_students), end='\n\n')
```

```
 The resulting dictionary from the dict.fromkeys():
    {'Python': None, 'R': None, 'C#': None, 'Java': None}

    The new list without duplicates using dict.fromkeys():
    ['Python', 'R', 'C#', 'Java']
```

* * *

## 4.使用`Counter`和`FreqDist`

我们可以使用字典子类如 [`Counter`](https://docs.python.org/3/library/collections.html#collections.Counter) 和 [`FreqDist`](https://docs.huihoo.com/nltk/0.9.5/api/nltk.probability.FreqDist-class.html) 从 Python 列表中删除重复项。`Counter`和`FreqDist`的工作方式相同。它们是集合，其中唯一的元素是字典键，它们出现的次数是值。如同在字典中一样，没有重复项的列表来自字典键。

```
# Using the dict subclass FreqDist
from nltk.probability import FreqDist

freq_dict = FreqDist(students)
print('The tabulate key-value pairs from FreqDist:')
freq_dict.tabulate()

print('\nThe new list without duplicates:')
print(list(freq_dict), end='\n\n')
```

```
 The tabulate key-value pairs from FreqDist:
    Python      R     C#   Java 
         2      2      1      1 

    The new list without duplicates:
    ['Python', 'R', 'C#', 'Java']
```

```
# Using the dict subclass Counter
from collections import Counter

counter_dict = Counter(students)
print(counter_dict, end='\n\n')

print('The new list without duplicates:')
print(list(counter_dict), end='\n\n')
```

```
 Counter({'Python': 2, 'R': 2, 'C#': 1, 'Java': 1})

    The new list without duplicates:
    ['Python', 'R', 'C#', 'Java']
```

* * *

## 5.使用`pd.unique`和`np.unique`

`pd.unique`和`np.unique`都接受一个有副本的列表，并返回列表中元素的唯一数组。产生的数组被转换成列表。当`np.unique`按升序对唯一元素排序时，`pd.unique`保持列表中元素的顺序。

```
import numpy as np
import pandas as pd

print('The new list without duplicates using np.unique():')
print(list(np.unique(students)), end='\n\n')

print('\nThe new list without duplicates using pd.unique():')
print(list(pd.unique(students)), end='\n\n')
```

```
 The new list without duplicates using np.unique():
    ['C#', 'Java', 'Python', 'R']

    The new list without duplicates using pd.unique():
    ['Python', 'R', 'C#', 'Java']
```

* * *

## 应用:礼品店重游

在这一节中，我们将重温我们的礼品店插图。礼品店在 50 人的附近。平均每天有 10 个人从店里进货，店铺一个月开 10 天。您收到了一个包含上个月在该商店购物的客户姓名的列表，您的任务是获取促销优惠的唯一客户的姓名。

```
# Install the `names` package
!pip install names
```

```
# Get package to generate names
import names
import random

random.seed(43)

# Generate names for 50 people in the neighbourhood
names_neighbourhood = [names.get_full_name() for _ in range(50)]

# Import package that randomly select names
from random import choices

# Customers do not have equal probabilities of purchasing from the shop
weights = [random.randint(-20, 20) for _ in range(50)]

# Randomly generate 20 customers that purchased from the store for 10 days
customers_month = [choices(names_neighbourhood, weights=weights, k=10) for _ in range(10)]
```

在上面的代码块中，我们随机生成了在过去一个月中从商店购买商品的客户列表。我们希望获得每月从商店购买商品的唯一客户，因此我们将为此任务创建`get_customer_names`函数。

我们已经包括了可选的输入和输出数据类型。Python 是一种动态类型的编程语言，在运行时隐式处理这种情况。然而，显示复杂数据结构的输入和输出数据类型是很有用的。或者，我们可以用一个`docstring`描述函数的输入和输出。`customers_purchases: List[List[str]]`告诉我们`customers_purchases`参数是一个包含带字符串的列表的列表，`-> List[Tuple[str, int]]`告诉我们函数返回一个包含带字符串和整数的元组的列表。

```
from typing import List, Tuple

def get_customer_names(customers_purchases: List[List[str]]) -> List[Tuple[str, int]]:

    # Get a single list of all the customers' names from the list of lists
    full_list_month = []
    for a_day_purchase in customers_purchases:
        full_list_month.extend(a_day_purchase)

    return Counter(full_list_month).most_common()

customers_list_tuples = get_customer_names(customers_month)
customers_list_tuples
```

```
 [('Nicole Moore', 14),
     ('Diane Paredes', 13),
     ('Mathew Jacobs', 11),
     ('Katherine Piazza', 10),
     ('Alvin Cloud', 8),
     ('Robert Mcadams', 8),
     ('Roger Lee', 8),
     ('Becky Hubert', 7),
     ('Paul Frisch', 7),
     ('Danielle Mccormick', 5),
     ('Donna Salvato', 3),
     ('Sally Thompson', 2),
     ('Franklin Copeland', 2),
     ('Linda Sample', 2)]
```

客户的名字是随机生成的——如果您不使用相同的`seed`值，您的名字可能会不同。`customers_list_tuples`中唯一客户的名称来自于首先将元组列表转换为字典，然后将字典键转换为列表:

```
# Unique customers for the previous month

list(dict(customers_list_tuples))
```

```
 ['Nicole Moore',
     'Diane Paredes',
     'Mathew Jacobs',
     'Katherine Piazza',
     'Alvin Cloud',
     'Robert Mcadams',
     'Roger Lee',
     'Becky Hubert',
     'Paul Frisch',
     'Danielle Mccormick',
     'Donna Salvato',
     'Sally Thompson',
     'Franklin Copeland',
     'Linda Sample']
```

* * *

## 结论

在本教程中，我们学习了如何从 Python 列表中删除重复项。我们学习了如何用关键字`del`删除小列表中的重复项。对于更大的列表，我们看到使用`for-loop`和列表理解方法比使用`del`关键字更有效。此外，我们了解到`set`的值和字典的键是惟一的，这使得它们适合从列表中删除重复项。最后，我们了解到 dictionary 子类从列表中删除重复项的方式与 dictionary 非常相似，我们还看到了从列表中获取唯一元素的 NumPy 和 pandas 方法。