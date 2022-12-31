# SettingwithCopyWarning:如何在熊猫中修复此警告

> 原文：<https://www.dataquest.io/blog/settingwithcopywarning/>

July 5, 2017

是人们在学习熊猫时遇到的最常见的障碍之一。快速的网络搜索会发现大量的堆栈溢出问题、GitHub 问题和来自程序员的论坛帖子，他们试图理解这个警告在他们的特定情况下意味着什么。这并不奇怪，许多人都在为此挣扎；有很多方法来索引 pandas [数据结构](https://en.wikipedia.org/wiki/Data_structure)，每种方法都有其独特的细微差别，甚至 pandas 本身也不能保证两行看起来相同的代码会有一个单一的结果。

本指南解释了产生警告的原因，并向您展示了解决方法。它还包括引擎盖下的细节，让您更好地了解正在发生的事情，并提供了一些关于该主题的历史，让您了解为什么它都是这样工作的。

为了探究`SettingWithCopyWarning`，我们将使用《建模在线拍卖一书[中易贝 3 天拍卖中售出的 Xboxes 价格数据集。让我们来看看:](https://www.modelingonlineauctions.com/datasets)

```py
 import pandas as pd
data = pd.read_csv('xbox-3-day-auctions.csv')
data.head()
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | Ninety-five | 2.927373 | jake7870 | Zero | Ninety-five | One hundred and seventeen point five |
| one | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred and fifteen | 2.943484 | 大卫·布雷斯勒 2 | one | Ninety-five | One hundred and seventeen point five |
| Two | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred | 2.951285 | gladimacowgirl | Fifty-eight | Ninety-five | One hundred and seventeen point five |
| three | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred and seventeen point five | 2.998947 | 戴斯鲁斯 | Ten | Ninety-five | One hundred and seventeen point five |
| four | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Two | 0.065266 | 多尼 4814 | five | One | One hundred and twenty |

正如你所看到的，我们数据集的每一行都涉及到一次特定的易贝 Xbox 拍卖的单一出价。以下是对每列的简要描述:

*   `auctionid` —每个拍卖的唯一标识符。
*   `bid` —投标的价值。
*   `bidtime` —投标时的拍卖期限，以天为单位。
*   `bidder` —投标人的易贝用户名。
*   `bidderrate`–投标人的易贝用户评级。
*   `openbid` —卖方为拍卖设定的开价。
*   `price` —拍卖结束时胜出的出价。

## 什么是 SettingWithCopyWarning？

首先要明白的是`SettingWithCopyWarning`是一个警告，而不是一个错误。

虽然错误表明出现了问题，例如无效的语法或试图引用未定义的变量，但警告的作用是提醒程序员他们的代码中存在潜在的错误或问题，而这些错误或问题仍然是该语言中允许的操作。在这种情况下，警告很可能表示一个严重但不明显的错误。

`SettingWithCopyWarning`通知您您的操作可能没有按预期进行，您应该检查结果以确保没有出错。

如果您的代码仍然像预期的那样工作，忽略这个警告是很有诱惑力的。这是不好的做法，`SettingWithCopyWarning`应该**永远不要**被忽视。在采取行动之前，花些时间去理解为什么你会得到警告。

为了理解`SettingWithCopyWarning`是关于什么的，理解 pandas 中的一些动作可以返回你的数据的视图，而其他动作将返回一个副本是有帮助的。

![](img/bf7b9255302176d82deb0592be86563b.png)

正如你在上面看到的，左边的视图`df2`只是原始视图`df1`的子集，而右边的副本创建了一个新的、唯一的对象`df2`。

当我们试图进行更改时，这可能会导致问题:

![](img/4baa635f2924d477c0ea656f82e39236.png)

根据我们正在做的事情，我们可能想要修改原始的`df1`(左图)，或者我们可能想要只修改`df2`(右图)。警告是让我们知道我们的代码可能做了一件事，而我们希望它做了另一件事。

稍后我们将深入探讨这个问题，但是现在让我们先来解决这个警告的两个主要原因以及如何修复它们。

## 常见问题#1:链式分配

Pandas 在检测到一种叫做链式分配的东西时会发出警告。让我们定义几个我们将用来解释事情的术语:

*   赋值——设置值的操作，例如`data = pd.read_csv('xbox-3-day-auctions.csv')`。通常被称为**集**。
*   访问—返回某个值的操作，例如下面的索引和链接示例。通常被称为 **get** 。
*   索引—引用数据子集的任何分配或访问方法；比如`data[1:5]`。
*   链接——连续使用多个索引操作；比如`data[1:5][1:3]`。

链式赋值是链接和赋值的组合。让我们快速看一个例子，它包含了我们之前加载的数据集。我们稍后将更详细地讨论这一点。为了这个例子，假设我们被告知用户`'parakeet2004'`的投标人评级不正确，我们必须更新它。让我们从当前值开始。

```py
data[data.bidder == 'parakeet2004']
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| six | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Three | 0.186539 | 鹦鹉 2004 | five | One | One hundred and twenty |
| seven | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Ten | 0.186690 | 鹦鹉 2004 | five | One | One hundred and twenty |
| eight | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Twenty-four point nine nine | 0.187049 | 鹦鹉 2004 | five | One | One hundred and twenty |

我们有三行来更新`bidderrate`字段；让我们继续做那件事。

```py
data[data.bidder == 'parakeet2004']['bidderrate'] = 100
```

```py
/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ipykernel/__main__.py:1: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy if __name__ == '__main__':
```

哦不！我们神秘地发现了`SettingWithCopyWarning`！

如果我们看一下，我们可以看到在这种情况下，值没有改变:

```py
data[data.bidder == 'parakeet2004']
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| six | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Three | 0.186539 | 鹦鹉 2004 | five | One | One hundred and twenty |
| seven | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Ten | 0.186690 | 鹦鹉 2004 | five | One | One hundred and twenty |
| eight | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | Twenty-four point nine nine | 0.187049 | 鹦鹉 2004 | five | One | One hundred and twenty |

生成该警告是因为我们将两个索引操作链接在一起。这更容易发现，因为我们已经使用了两次方括号，但是如果我们使用其他访问方法，如`.bidderrate`、`.loc[]`、`.iloc[]`、`.ix[]`等等，情况也是如此。我们的连锁经营包括:

*   `data[data.bidder == 'parakeet2004']`
*   `['bidderrate'] = 100`

这两个连锁操作一个接一个地独立执行。第一个是访问方法(get 操作)，它将返回一个包含所有行的`DataFrame`，其中`bidder`等于`'parakeet2004'`。第二个是赋值操作(set 操作)，在这个新的`DataFrame`上调用。我们根本没有在原版`DataFrame`上做手术。

解决方案很简单:使用`loc`将链接的操作组合成一个操作，这样 pandas 就可以确保原来的`DataFrame`被设置。熊猫将始终确保不被束缚的集合操作(如下所示)正常工作。

```py
 # Setting the new value
data.loc[data.bidder == 'parakeet2004', 'bidderrate'] = 100
# Taking a look at the result
data[data.bidder == 'parakeet2004']['bidderrate'] 
```

```py
 6 100
7 100
8 100
Name: bidderrate, dtype: int64
```

这是警告建议我们做的，并且在这种情况下非常有效。

## 常见问题#2:隐藏链接

接下来是人们遇到的第二种最常见的方式。让我们调查中标情况。我们将创建一个新的数据框架来处理它们，注意使用`loc`,因为我们已经学习了关于链式赋值的课程。

```py
 winners = data.loc[data.bid == data.price]
winners.head()
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| three | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred and seventeen point five | 2.998947 | 戴斯鲁斯 | Ten | Ninety-five | One hundred and seventeen point five |
| Twenty-five | Eight billion two hundred and thirteen million sixty thousand four hundred and twenty | One hundred and twenty | 2.999722 | djnoeproductions | Seventeen | One | One hundred and twenty |
| forty-four | Eight billion two hundred and thirteen million sixty-seven thousand eight hundred and thirty-eight | One hundred and thirty-two point five | 2.996632 | *香槟泡泡* | Two hundred and two | Twenty-nine point nine nine | One hundred and thirty-two point five |
| Forty-five | Eight billion two hundred and thirteen million sixty-seven thousand eight hundred and thirty-eight | One hundred and thirty-two point five | 2.997789 | *香槟泡泡* | Two hundred and two | Twenty-nine point nine nine | One hundred and thirty-two point five |
| Sixty-six | Eight billion two hundred and thirteen million seventy-three thousand five hundred and nine | One hundred and fourteen point five | 2.999236 | rr6kids | four | One | One hundred and fourteen point five |

我们可能会用 winners 变量写几行后续代码。

```py
 mean_win_time = winners.bidtime.mean()
... # 20 lines of code
mode_open_bid = winners.openbid.mode() 
```

一个偶然的机会，我们在`DataFrame`中遇到了另一个错误。这一次，标签为`304`的行中缺少了`bidder`值。

```py
winners.loc[304, 'bidder']
```

```py
nan
```

为了我们的例子，让我们说，我们知道这个投标人的真实用户名，并更新我们的数据。

```py
winners.loc[304, 'bidder'] = 'therealname'
```

```py
/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/indexing.py:517: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy self.obj[item] = s
```

又一个`SettingWithCopyWarning`！但是我们用了`loc`，怎么又出现这种情况了？为了进行研究，让我们看看代码的结果:

```py
print(winners.loc[304, 'bidder'])
```

```py
therealname
```

这次成功了，那我们为什么会收到警告呢？

链式索引既可以出现在两行中，也可以出现在一行中。因为`winners`是作为 get 操作(`data.loc[data.bid == data.price]`)的输出创建的，所以它可能是原始`DataFrame`的副本，也可能不是，但是在我们检查之前没有办法知道！当我们索引`winners`时，我们实际上使用的是链式索引。

这意味着当我们试图修改`winners`时，我们可能也修改了`data`。

在真正的代码库中，这些代码行可能相隔很远，因此追踪问题的根源可能更困难，但情况是一样的。

为了防止在这种情况下出现警告，解决方案是明确地告诉 pandas 在我们创建新的数据帧时制作一个副本:

```py
 winners = data.loc[data.bid == data.price].copy()
winners.loc[304, 'bidder'] = 'therealname'
print(winners.loc[304, 'bidder'])
print(data.loc[304, 'bidder'])
```

```py
therealname
nan
```

就是这样！就这么简单。

诀窍是学会识别链式索引，并不惜一切代价避免它。如果你想改变原来的，使用单一的赋值操作。如果你想要一份拷贝，确保你强迫熊猫这么做。这将节省时间，并使您的代码滴水不漏。

还要注意的是，即使`SettingWithCopyWarning`只会在你设置的时候出现，也最好避免 gets 的链式索引。链式操作速度较慢，如果您决定稍后添加赋值操作，会导致问题。

## 处理设置 SettingWithCopyWarning 的提示和技巧

在我们下面做更深入的分析之前，让我们拿出显微镜，看看`SettingWithCopyWarning`的一些细微之处和本质细节。

### 关闭警告

首先，如果不讨论如何显式控制`SettingWithCopy`设置，这篇文章将是不完整的。熊猫`mode.chained_assignment`选项可以取其中一个值:

*   `'raise'` —引发异常而不是警告。
*   `'warn'` —生成警告(默认)。
*   `None` —完全关闭警告。

例如，让我们关闭警告:

```py
 pd.set_option('mode.chained_assignment', None)
data[data.bidder == 'parakeet2004']['bidderrate'] = 100
```

因为这不会给我们任何警告，所以除非你完全明白自己在做什么，否则不推荐这样做。如果你感到一丝怀疑，这是不可取的。一些开发人员非常认真地对待`SettingWithCopy`，并选择将其提升为一个例外，就像这样:

```py
pd.set_option('mode.chained_assignment', 'raise')
data[data.bidder == 'parakeet2004']['bidderrate'] = 100
```

```py
---------------------------------------------------------------------------SettingWithCopyError Traceback (most recent call last)<ipython-input-13-80e3669cab86> in <module>() 1 pd.set_option('mode.chained_assignment', 'raise')----> 2 data[data.bidder == 'parakeet2004']['bidderrate'] = 100/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/frame.py in __setitem__(self, key, value) 2427 else: 2428 # set column-> 2429 self._set_item(key, value) 2430 2431 def _setitem_slice(self, key, value):/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/frame.py in _set_item(self, key, value) 2500 # value exception to occur first 2501 if len(self):-> 2502 self._check_setitem_copy() 2503 2504 def insert(self, loc, column, value, allow_duplicates=False):/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/generic.py in _check_setitem_copy(self, stacklevel, t, force) 1758 1759 if value == 'raise':-> 1760 raise SettingWithCopyError(t) 1761 elif value == 'warn': 1762 warnings.warn(t, SettingWithCopyWarning, stacklevel=stacklevel)SettingWithCopyError: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
```

如果您的团队中有一个没有经验的 pandas 开发人员，或者一个需要高水平的严格性或完整性的确定性的项目，这可能特别有用。

使用这个设置的更精确的方法是使用一个[上下文管理器](https://www.geeksforgeeks.org/context-manager-in-python/)。

```py
 # resets the option we set in the previous code segment
pd.reset_option('mode.chained_assignment')
with pd.option_context('mode.chained_assignment', None):
    data[data.bidder == 'parakeet2004']['bidderrate'] = 100 
```

如您所见，这种方法支持细粒度的警告抑制，而不是不加区别地影响整个环境。

### is_copy 属性

另一个可以用来避免警告的技巧是修改熊猫用来解释`SettingWithCopy`场景的工具之一。每个`DataFrame`都有一个默认为`None`的`is_copy`属性，但是如果是副本，则使用一个 [`weakref`](https://docs.python.org/3/library/weakref.html) 来引用源`DataFrame`。通过将`is_copy`设置为`None`，可以避免生成警告。

```py
winners = data.loc[data.bid == data.price]
winners.is_copy = None
winners.loc[304, 'bidder'] = 'therealname'
```

然而，请注意，这将**而不是**奇迹般地解决问题，但它确实使缺陷检测变得潜在非常困难。

### 单类型与多类型对象

值得强调的另一点是单数据类型和多数据类型对象之间的区别。如果一个`DataFrame`的所有列都是相同的数据类型，那么它就是单数据类型的；例如:

```py
 import numpy as np
single_dtype_df = pd.DataFrame(np.random.rand(5,2), columns=list('AB'))
print(single_dtype_df.dtypes)
single_dtype_df
```

```py
 A float64
B float64dtype: object
```

|  | A | B |
| --- | --- | --- |
| Zero | 0.383197 | 0.895652 |
| one | 0.077943 | 0.905245 |
| Two | 0.452151 | 0.677482 |
| three | 0.533288 | 0.768252 |
| four | 0.389799 | 0.674594 |

而如果一个`DataFrame`的列不具有相同的数据类型，那么它就是多数据类型的，例如:

```py
 multiple_dtype_df = pd.DataFrame({'A': np.random.rand(5),'B': list('abcde')})
print(multiple_dtype_df.dtypes)
multiple_dtype_df
```

```py
 A float64
B object
dtype: object
```

|  | A | B |
| --- | --- | --- |
| Zero | 0.615487 | a |
| one | 0.946149 | b |
| Two | 0.701231 | c |
| three | 0.756522 | d |
| four | 0.481719 | e |

由于下面*历史*部分解释的原因，在多数据类型对象上的索引器获取操作将总是返回一个副本。然而，主要为了提高效率，索引器对单数据类型对象的 get 操作几乎总是返回一个视图；这里需要注意的是，这取决于对象的内存布局，并且不能保证。

### 假阳性

假阳性，或无意中报告了连锁分配的情况，过去在早期版本的 pandas 中更常见，但后来基本上被消除了。为了完整起见，这里包括一些固定误报的例子是有用的。如果您在使用早期版本的 pandas 时遇到以下任何情况，那么可以安全地忽略或取消警告(或者通过升级来完全避免警告！)

使用用于[的当前列的值向`DataFrame`添加一个新列会产生一个警告](https://stackoverflow.com/q/42105859/604687)，但是这个问题已经被修复。

```py
 data['bidtime_hours'] = data.bidtime.map(lambda x: x * 24)
data.head(2)
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 | 投标时间 _ 小时 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | Ninety-five | 2.927373 | jake7870 | Zero | Ninety-five | One hundred and seventeen point five | 70.256952 |
| one | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred and fifteen | 2.943484 | 大卫·布雷斯勒 2 | one | Ninety-five | One hundred and seventeen point five | 70.643616 |

直到最近，当在一个`DataFrame`的片上使用`apply`方法设置时，一个假阳性[也发生了](https://stackoverflow.com/q/42190175/604687)，尽管这个问题也已经被修复。

```py
 data.loc[:, 'bidtime_hours'] = data.bidtime.apply(lambda x: x * 24)
data.head(2)
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 | 投标时间 _ 小时 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | Ninety-five | 2.927373 | jake7870 | Zero | Ninety-five | One hundred and seventeen point five | 70.256952 |
| one | Eight billion two hundred and thirteen million thirty-four thousand seven hundred and five | One hundred and fifteen | 2.943484 | 大卫·布雷斯勒 2 | one | Ninety-five | One hundred and seventeen point five | 70.643616 |

最后，直到版本 0.17.0，在 [`DataFrame.sample`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sample.html) 方法中有一个导致虚假`SettingWithCopy`警告的 bug。`sample`方法现在每次都返回一个副本。

```py
 sample = data.sample(2)
sample.loc[:, 'price'] = 120
sample.head()
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 | 投标时间 _ 小时 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Four hundred and eighty-one | Eight billion two hundred and fifteen million four hundred and eight thousand and twenty-three | Ninety-one point zero one | 2.990741 | sailor4eva | one | Zero point nine nine | One hundred and twenty | 71.777784 |
| Five hundred and three | Eight billion two hundred and fifteen million five hundred and seventy-one thousand and thirty-nine | One hundred | 1.965463 | lambonius1 | Zero | Fifty | One hundred and twenty | 47.171112 |

## 深度链式赋值

让我们重新使用之前的例子，我们试图用`'parakeet2004'`的`bidder`值来更新`data`中每一行的`bidderrate`列。

```py
data[data.bidder == 'parakeet2004']['bidderrate'] = 100
```

```py
/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ipykernel/__main__.py:1: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy if __name__ == '__main__':
```

pandas 用这个`SettingWithCopyWarning`真正告诉我们的是，我们代码的行为是不明确的，但是要理解为什么是这样以及警告的措辞，回顾一些概念将是有帮助的。

我们之前简单地讨论了视图和副本。访问一个`DataFrame`的子集有两种可能的方法:要么在内存中创建一个对原始数据的引用(一个视图)，要么将子集复制到一个新的、更小的`DataFrame`(一个副本)。视图是查看**原始**数据的特定部分的一种方式，而副本是该数据在内存中新位置的**克隆**。正如我们前面的图表所示，修改视图会修改原始变量，但修改副本不会。

由于我们将在后面讨论的原因，熊猫的“get”操作的输出不能保证。当您索引 pandas 数据结构时，可能会返回一个视图或一个副本，这意味着对一个`DataFrame`的 get 操作会返回一个新的`DataFrame`，它可以包含:

*   原始对象中数据的副本。
*   对原始对象数据的引用，但不制作副本。

因为我们不知道会发生什么，每种可能性都有非常不同的行为，忽视警告是在玩火。

为了更清楚地说明视图、副本和这种模糊性，让我们创建一个简单的`DataFrame`并将其编入索引:

```py
df1 = pd.DataFrame(np.arange(6).reshape((3,2)), columns=list('AB'))
df1
```

|  | A | B |
| --- | --- | --- |
| Zero | Zero | one |
| one | Two | three |
| Two | four | five |

让我们将`df1`的子集分配给`df2`:

```py
df2 = df1.loc[:1]
df2
```

|  | A | B |
| --- | --- | --- |
| Zero | Zero | one |
| one | Two | three |

根据我们所了解的，我们知道`df2`可能是`df1`上的一个视图，或者是`df1`的一个子集的副本。

在我们着手解决这个问题之前，我们还需要再看一看链式索引。用`'parakeet2004'`扩展我们的例子，我们将两个索引操作链接在一起:

```py
 data[data.bidder == 'parakeet2004']
__intermediate__['bidderrate'] = 100
```

其中`__intermediate__`代表第一次调用的输出，对我们完全隐藏。请记住，如果我们使用属性访问，也会得到同样有问题的结果:

```py
data[data.bidder == 'parakeet2004'].bidderrate = 100
```

这同样适用于任何其他形式的链式调用**，因为我们正在生成这个中间对象**。

实际上，链式索引意味着不止一次调用`__getitem__`或`__setitem__`来完成一个操作。这些是[特殊的 Python 方法](https://docs.python.org/3/reference/datamodel.html#special-method-names)，通过在实现它们的类的实例上使用方括号来调用，这是所谓的[语法糖](https://en.wikipedia.org/wiki/Syntactic_sugar)的一个例子。让我们看看 Python 解释器将在我们的例子中执行什么。

```py
 # Our code
data[data.bidder == 'parakeet2004']['bidderrate'] = 100
# Code executed
data.__getitem__(data.__getitem__('bidder') == 'parakeet2004').__setitem__('bidderrate', 100)
```

您可能已经意识到，`SettingWithCopyWarning`是这个链式`__setitem__`调用的结果。您可以自己尝试一下——上面几行的功能完全相同。为了清楚起见，请注意第二个`__getitem__`调用(针对`bidder`列)是嵌套的，根本不属于这里的链接问题。

一般来说，如前所述，pandas 不保证 get 操作将返回数据的视图还是副本。如果在我们的例子中返回了一个视图，那么我们的链式赋值中的第二个表达式将是对原始对象的调用`__setitem__`。但是，如果返回一个副本，将被修改的是该副本——原始对象不会被修改。

这就是警告所说的“试图在数据帧的切片副本上设置值”的含义。由于没有引用这个副本，它最终将被[垃圾收集](https://en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29)。`SettingWithCopyWarning`让我们知道 pandas 不能确定第一个`__getitem__`调用返回的是视图还是副本，因此不清楚赋值是否改变了原始对象。另一种思考熊猫为什么给我们这个警告的方式是因为“我们在修改原作吗？”是未知的。

我们确实想修改原始操作，警告建议的解决方案是使用`loc`将这两个独立的、链接的操作转换成一个赋值操作。这将从我们的代码中删除链式索引，我们将不再收到警告。我们的固定代码及其扩展版本将如下所示:

```py
 # Our code
data.loc[data.bidder == 'parakeet2004', 'bidderrate'] = 100
# Code executeddata.loc.__setitem__((data.__getitem__('bidder') == 'parakeet2004', 'bidderrate'), 100)
```

我们的 DataFrame 的`loc`属性保证是原始的`DataFrame`本身，但是具有扩展的索引功能。

### 假阴性

使用`loc`并不能解决我们的问题，因为使用`loc`的 get 操作仍然可以返回视图或副本。让我们快速检查一个有点复杂的例子。

```py
data.loc[data.bidder == 'parakeet2004', ('bidderrate', 'bid')]
```

|  | 投标价格 | 出价 |
| --- | --- | --- |
| six | One hundred | Three |
| seven | One hundred | Ten |
| eight | One hundred | Twenty-four point nine nine |

这次我们抽出了两列，而不是一列。让我们尝试设置所有的`bid`值。

```py
data.loc[data.bidder == 'parakeet2004', ('bidderrate', 'bid')]['bid'] = 5.0
data.loc[data.bidder == 'parakeet2004', ('bidderrate', 'bid')]
```

|  | 投标价格 | 出价 |
| --- | --- | --- |
| six | One hundred | Three |
| seven | One hundred | Ten |
| eight | One hundred | Twenty-four point nine nine |

没有效果也没有警告！我们在切片的副本上设置了一个值，但熊猫没有检测到它—这是一个假阴性。仅仅因为我们已经使用了`loc`并不意味着我们可以再次开始使用链式赋值。GitHub 上有一个关于这个特殊 bug 的旧的未解决的[问题。](https://github.com/pandas-dev/pandas/issues/9767)

正确的做法如下:

```py
 data.loc[data.bidder == 'parakeet2004', 'bid'] = 5.0
data.loc[data.bidder == 'parakeet2004', ('bidderrate', 'bid')]
```

|  | 投标价格 | 出价 |
| --- | --- | --- |
| six | One hundred | Five |
| seven | One hundred | Five |
| eight | One hundred | Five |

您可能想知道在实践中怎么会有人遇到这样的问题，但是当我们在下一节中把`DataFrame`查询的结果赋给变量时，这比您想象的要容易。

### 隐藏链接

让我们再看看前面的隐藏链接示例，我们试图从我们的`winners`变量中标有`304`的行中设置`bidder`值。

```py
 winners = data.loc[data.bid == data.price]
winners.loc[304, 'bidder'] = 'therealname'
```

```py
/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/indexing.py:517: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy self.obj[item] = s
```

即使我们使用了`loc`，我们还是得到了另一个`SettingWithCopyWarning`。这个问题非常令人困惑，因为警告信息似乎在建议我们做我们已经做过的事情。

但是想想`winners`这个变量。到底是什么？假设我们通过`data.loc[data.bid == data.price]`实例化了它，我们无法知道它是视图还是原始`data`T3 的副本(因为 get 操作要么返回视图，要么返回副本)。将实例化与生成警告的代码行结合起来，可以清楚地看出我们的错误。

```py
data.loc[data.bid == data.price].loc[304, 'bidder'] = 'therealname'
```

```py
/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/pandas/core/indexing.py:517: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy self.obj[item] = s
```

我们再次使用了链式赋值，但是这次它被分成了两行。思考这个问题的另一种方式是问这样一个问题“这是修改了一个还是两个东西？”在我们的例子中，答案是未知的:如果`winners`是一个副本，那么只有`winners`受到影响，但是如果它是一个视图，`winners`和`data`都将显示更新的值。这种情况可能发生在脚本或代码库中相距很远的代码行之间，使得问题的根源很难追踪。

这里警告的目的是防止我们认为我们的代码会修改原来的`DataFrame`,或者我们在修改一个副本而不是原来的。[钻研熊猫 GitHub repo 的老问题，你可以阅读开发者自己对此的解释。](https://github.com/pandas-dev/pandas/issues/6757#issuecomment-39204438)

我们如何解决这个问题在很大程度上取决于我们自己的意图。如果我们乐于使用原始数据的副本，解决方案就是简单地强迫熊猫制作一个副本。

```py
 winners = data.loc[data.bid == data.price].copy()
winners.loc[304, 'bidder'] = 'therealname'
print(data.loc[304, 'bidder']) # Original
print(winners.loc[304, 'bidder']) # Copy
```

```py
nan
therealname
```

另一方面，如果您要求更新原始的`DataFrame`,那么您应该使用原始的`DataFrame`,而不是实例化具有未知行为的其他变量。我们之前的代码会变成:

```py
 # Finding the winners
winner_mask = data.bid == data.price
# Taking a peek
data.loc[winner_mask].head()
# Doing analysis
mean_win_time = data.loc[winner_mask, 'bidtime'].mean()
... # 20 lines of code
mode_open_bid = data.loc[winner_mask, 'openbid'].mode()
# Updating the username
data.loc[304, 'bidder'] = 'therealname'
```

在更复杂的情况下，比如修改一个`DataFrame`子集的一个子集，而不是使用链式索引，你可以通过在原始`DataFrame`上的`loc`来修改你正在制作的切片。例如，您可以更改上面的新`winner_mask`变量，或者创建一个新变量来选择获胜者的子集，如下所示:

```py
 high_winner_mask = winner_mask & (data.price > 150)
data.loc[high_winner_mask].head()
```

|  | 拍卖 id | 出价 | 就寝时间 | 投标人 | 投标价格 | openid | 价格 | 投标时间 _ 小时 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Two hundred and twenty-five | Eight billion two hundred and thirteen million three hundred and eighty-seven thousand four hundred and forty-four | One hundred and fifty-two | 2.919757 | uconnbabydoll1975 年 | Fifteen | Zero point nine nine | One hundred and fifty-two | 70.074168 |
| Three hundred and twenty-eight | Eight billion two hundred and thirteen million nine hundred and thirty-five thousand one hundred and thirty-four | Two hundred and seven point five | 2.983542 | toby2492 | Zero | Zero point one | Two hundred and seven point five | 71.605008 |
| Four hundred and sixteen | Eight billion two hundred and fourteen million four hundred and thirty thousand three hundred and ninety-six | One hundred and ninety-nine | 2.990463 | volpendesta | four | Nine point nine nine | One hundred and ninety-nine | 71.771112 |
| Five hundred and thirty-one | Eight billion two hundred and fifteen million five hundred and eighty-two thousand two hundred and twenty-seven | One hundred and fifty-two point five | 2.999664 | 最后通牒 _ 男人 | Two | Sixty | One hundred and fifty-two point five | 71.991936 |

这种技术对于未来的代码库维护和扩展更加健壮。

## 历史

您可能想知道为什么不能通过显式地指定返回视图或副本的索引方法来完全避免整个`SettingWithCopy`问题，而不是制造我们发现自己所处的混乱局面。为了理解这一点，我们必须了解熊猫的过去。

pandas 用来确定它是返回一个视图还是一个副本的逻辑源于它对 NumPy 库的使用，这是 pandas 操作的基础。观点实际上是通过 NumPy 进入熊猫词典的。事实上，视图在 NumPy 中非常有用，因为它们是可预测地返回的。因为 NumPy 数组是单一类型的，pandas 试图通过使用最合适的 [dtype](https://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html) 来最小化空间和处理需求。因此，包含单个 dtype 的`DataFrame`片段可以作为单个 NumPy 数组上的视图返回，这是一种高效的操作处理方式。然而，在 NumPy 中，多数据类型片不能以同样的方式高效地存储。Pandas 将通用的索引功能与最有效地使用其 NumPy 内核的能力结合在一起。

最终，pandas 中的索引被设计成有用和通用的，其核心并不完全结合底层 NumPy 数组的功能。随着时间的推移，设计和功能的这些元素之间的相互作用导致了一组复杂的规则，这些规则决定了视图或副本是否可以被返回。有经验的 pandas 开发人员通常对 pandas 的行为感到满意，因为他们可以轻松地导航其索引行为。

不幸的是，对于库的新手来说，链式索引几乎是不可避免的，尽管这并不是想要的方法，因为 get 操作返回可索引的 pandas 对象。再者，[用熊猫几年的核心开发者之一 Jeff Reback](https://github.com/pandas-dev/pandas/issues/10954#issuecomment-136446453) 的话来说，“从语言的角度来说，直接检测链式索引是根本不可能的；必须推断出来”。

因此，该警告在 2013 年底的版本 0.13.0 中被引入，作为许多开发人员遇到的链式分配的[静默失败](https://stackoverflow.com/q/16553298/604687)的解决方案。

在 0.12 版本之前， [`ix`索引器](https://pandas.pydata.org/pandas-docs/version/0.19.2/generated/pandas.DataFrame.ix.html)是最流行的(在 pandas 命名法中，`ix`、`loc`和`iloc`等“索引器”是简单的构造，允许对象像数组一样用方括号索引，但具有特殊的行为)。但是大约在这个时候，也就是 2013 年年中，熊猫项目开始获得动力，迎合新用户变得越来越重要。从这个版本开始，`loc`和`iloc`索引器因其更清晰的本质和更容易解释的用法而更受青睐。

![](img/6b09f9ce48bbcd200825167599889579.png)

谷歌趋势:熊猫

在推出之后，`SettingWithCopyWarning`一直在继续发展，几年来在许多 GitHub 问题上被[热议](https://github.com/pandas-dev/pandas/issues/10954)，甚至现在[仍在更新](https://pandas.pydata.org/pandas-docs/stable/whatsnew.html?highlight=settingwithcopywarning)，但它将继续存在，理解它对于成为熊猫专家仍然至关重要。

## 包扎

隐藏在`SettingWithCopyWarning`背后的复杂性是熊猫库中为数不多的粗糙边缘之一。它的根源深深植根于图书馆，不应该被忽视。用杰夫·雷巴克自己的话来说,“就我所知，没有任何情况下你真的应该忽略这个警告。…如果您执行某些类型的索引，它将永远不会工作，而其他类型的索引将会工作。你真是在玩火。”

幸运的是，解决这个警告只需要您识别链式赋值并修复它。如果说我们能从这一切中吸取什么的话，那就是。

如果你想了解更多关于这个话题的信息，请查看 Dataquest 的交互式 [Pandas 和 NumPy Fundamentals](https://www.dataquest.io/course/pandas-fundamentals/) 课程，以及我们的[Python 数据分析师](https://www.dataquest.io/path/data-analyst)和[Python 数据科学家](https://www.dataquest.io/path/data-scientist)路径，它们将帮助你在大约 6 个月内做好工作准备。

![YouTube video player for 6a5jbnUNE2E](img/1abf55e66817f421c9b041572037fe56.png)

*[https://www.youtube.com/embed/6a5jbnUNE2E?rel=0](https://www.youtube.com/embed/6a5jbnUNE2E?rel=0)*

 *提升您的数据技能。

[查看计划](/subscribe)*