# 教程:Python 的计数器类和概率质量函数

> 原文：<https://www.dataquest.io/blog/python-counter-class/>

April 6, 2015![python-counter-pmf-tutorial](img/f9289afcf177659611bd0f07e147a8b8.png)

## Python 计数器类

Python 中的 Counter 类是[集合](https://docs.python.org/3.5/library/collections.html)模块的一部分。计数器提供了一种快速计算列表中存在的唯一项的数量的方法。Counter 类也可以扩展为表示概率质量函数和贝叶斯假设集。计数器是从值到它们的频率的映射。如果你用一个字符串初始化一个计数器，你会得到一个从每个字母到它出现次数的映射。如果两个单词是字谜，它们会产生相等的计数器，因此您可以使用计数器在线性时间内测试字谜。这一课是根据艾伦·唐尼的[小笔记本改编的。](https://github.com/AllenDowney/PythonCounterPmf)

```py
 from collections import Counter

def is_anagram(word1, word2):
    """
    Checks whether the words are anagrams.
    word1: string    
    word2: string

    returns: boolean
    """    
    return Counter(word1) == Counter(word2)

print(is_anagram('tachymetric', 'mccarthyite'))
print(is_anagram('banana', 'peach')) 
```

```py
True
False
```

## 多重集

计数器是多重集的自然表示，多重集是指元素可以出现多次的集合。您可以使用 is_subset 之类的集合操作来扩展 Counter:您可以在 Scrabble 之类的游戏中使用`is_subset`,看看给定的一组瓷砖是否可以用来拼写给定的单词。

```py

class Multiset(Counter):
    """A multiset is a set where elements can appear more than once."""

    def is_subset(self, other):
        """Checks whether self is a subset of other.

        other: Multiset

        returns: boolean
        """
        for char, count in self.items():
            if other[char] < count:
                return False
        return True

    # map the <= operator to is_subset
    __le__ = is_subset

def can_spell(word, tiles):
    """Checks whether a set of tiles can spell a word.

    word: string
    tiles: string

    returns: boolean
    """
    return Multiset(word) <= Multiset(tiles)

print(can_spell('SYZYGY', 'AGSYYYZ'))
```

```py
True
```

## 概率质量函数

你也可以扩展计数器来表示一个概率质量函数(PMF)。`normalize`计算频率的总和并除以，产生加到 1 的概率。`__add__`枚举所有值对，并返回一个新的 Pmf，它表示总和的分布。`__hash__`和`__id__`使 PMF 可散列；这不是最好的方法，因为它们是可变的。所以这个实现带有一个警告，如果您使用 Pmf 作为键，您不应该修改它。一个更好的选择是定义一个冻结的 Pmf。`render`以可用于绘图的形式返回数值和概率:

```py
 class Pmf(Counter):
    """A Counter with probabilities."""

    def normalize(self):
        """Normalizes the PMF so the probabilities add to 1."""
        total = float(sum(self.values()))
        for key in self:
            self[key] /= total

    def __add__(self, other):
        """Adds two distributions.

        The result is the distribution of sums of values from the
        two distributions.

        other: Pmf

        returns: new Pmf
        """
        pmf = Pmf()
        for key1, prob1 in self.items():
            for key2, prob2 in other.items():
                pmf[key1 + key2] += prob1 * prob2
        return pmf

    def __hash__(self):
        """Returns an integer hash value."""
        return id(self)

    def __eq__(self, other):
        return self is other

    def render(self):
        """Returns values and their probabilities, suitable for plotting."""
        return zip(*sorted(self.items())) 
```

## 使用 Pmf 对象

例如，我们可以制作一个 Pmf 对象来表示一个 6 面骰子。

```py
 d6 = Pmf([1,2,3,4,5,6])
d6.normalize()
d6.name = 'one die'
print(d6)
```

```py
Pmf({1: 0.16666666666666666, 2: 0.16666666666666666, 3: 0.16666666666666666, 4: 0.16666666666666666, 5: 0.16666666666666666, 6: 0.16666666666666666})
```

## 添加运算符

使用加法运算符，我们可以计算两个骰子之和的分布。

```py
 d6_twice = d6 + d6
d6_twice.name = 'two dice'

for key, prob in d6_twice.items():
    print(key, prob)
```

```py
 2 0.027777777777777776
3 0.05555555555555555
4 0.08333333333333333
5 0.1111111111111111
6 0.1388888888888889
7 0.16666666666666669
8 0.1388888888888889
9 0.1111111111111111
10 0.08333333333333333
11 0.05555555555555555
12 0.027777777777777776 
```

## 计算分布

使用 numpy.sum，我们可以计算三个骰子总和的分布。然后绘制结果(使用 Pmf.render)

```py
 pmf_ident = Pmf([0])
d6_thrice = sum([d6]*3, pmf_ident)
d6_thrice.name = 'three dice'

for die in [d6, d6_twice, d6_thrice]:
    xs, ys = die.render()
    plt.plot(xs, ys, label=die.name, linewidth=3, alpha=0.5)

plt.xlabel('Total')
plt.ylabel('Probability')
plt.legend()
plt.show()
```

## 贝叶斯统计

一个套件是一个 Pmf，它表示一组假设及其概率；它提供了`bayesian_update`，根据新数据更新假设的概率。Suite 是抽象的父类；子类应该提供一个可能性方法，在给定的假设下评估数据的可能性。`bayesian_update`遍历假设，评估每个假设下数据的可能性，并相应地更新概率。然后 PMF 恢复正常。

```py
 class Suite(Pmf):
    """Map from hypothesis to probability."""

    def bayesian_update(self, data):
        """Performs a Bayesian update.

        Note: called bayesian_update to avoid overriding dict.update

        data: result of a die roll
        """
        for hypo in self:
            like = self.likelihood(data, hypo)
            self[hypo] *= like

        self.normalize() 
```

## 套件示例

作为一个例子，我将使用 Suite 来解决“骰子问题”，来自第三章的 *Think Bayes* :

> “假设我有一盒骰子，其中包含 4 面骰子、6 面骰子、8 面骰子、12 面骰子和 20 面骰子。如果你玩过龙与地下城，你知道我在说什么。假设我从盒子里随机选择一个骰子，掷骰子，得到一个 6。我掷出每个骰子的概率是多少？”

首先，我将列出代表骰子的 PMF 列表:

```py
 def make_die(num_sides):
    die = Pmf(range(1, num_sides+1))
    die.name = 'd%d' % num_sides
    die.normalize()
    return die

dice = [make_die(x) for x in [4, 6, 8, 12, 20]]
print(dice)
```

## 说〔t0〕

接下来我将定义 DiceSuite，它从 Suite 继承了`bayesian_update`，并提供了`likelihood`。`data`是观察到的掷骰子，例子中为 6。`hypo`是我可能掷出的假设骰子；为了得到数据的可能性，我从给定的骰子中选择给定值的概率。

```py
 class DiceSuite(Suite):

    def likelihood(self, data, hypo):
        """Computes the likelihood of the data under the hypothesis.

        data: result of a die roll
        hypo: Die object
        """
        return hypo[data] 
```

## 更新分布

最后，我使用骰子列表来实例化一个套件，该套件从每个骰子映射到其先验概率。默认情况下，所有骰子都有相同的先验。然后我用给定的值更新分布并打印结果:

```py
 dice_suite = DiceSuite(dice)

dice_suite.bayesian_update(6)

for die, prob in dice_suite.items():
    print(die.name)
    print(prob)
```

## 又一更新

正如所料，4 面模具已被淘汰；它现在的概率是 0。6 面模具是最有可能的，但是 8 面模具还是很有可能的。现在假设我再次掷骰子，得到一个 8。我们可以用新数据再次更新套件。现在 6 面骰子已经被淘汰了，8 面骰子是最有可能的，我掷出 20 面骰子的几率不到 10%。这些例子展示了 Counter 类的多功能性，Counter 类是 Python 中未被充分利用的数据结构之一。

```py
 dice_suite.bayesian_update(8)

for die, prob in dice_suite.items():
    print(die.name)
    print(prob)
```

## 接下来的步骤

关于 Python 的计数器类的更多信息，你可以查看我们的[互动教程](https://app.dataquest.io/m/13)

。