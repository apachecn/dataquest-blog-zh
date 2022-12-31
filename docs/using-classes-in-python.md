# 在 Python 中使用类

> 原文：<https://www.dataquest.io/blog/using-classes-in-python/>

January 26, 2022![Using Classes in Python](img/1db0d0a4704de8d86d721261d41c8828.png)

在本教程中，我们将学习如何创建和使用 Python 类。特别是，我们将讨论什么是 Python 类，我们为什么使用它们，存在什么类型的类，如何在 Python 中定义类和声明/调整类对象，以及许多其他事情。

## Python 中有哪些类？

因为 [Python 是面向对象编程(OOP)](https://app.dataquest.io/m/39) 语言，里面的一切都代表一个对象。[列出了](https://app.dataquest.io/m/335)，元组，集合，字典，[函数](https://app.dataquest.io/m/315)等。都是 Python 对象的例子。一般来说，Python 对象是数据项和描述这些项的行为的方法的实体。

相比之下，Python 类是一个模板或构造函数，用于创建 Python 对象。我们可以把它想象成一份烹饪食谱，里面列出了所有的配料和它们的数量，包括它们可能的范围，并一步一步地描述了烹饪的整个过程。在这种情况下，我们可以将蛋糕食谱比作一个类，将按照该食谱烹饪的蛋糕比作一个对象(即该类的一个实例)。使用相同的配方(类)，我们可以创建许多蛋糕(对象)。这正是在 Python 中创建类的范围:定义数据元素和建立这些元素如何交互和改变它们状态的规则——然后使用这个原型以预定义的方式构建各种对象(类的实例),而不是每次都从头开始创建它们。

## Python 中的类类型

从 Python 3 开始，Python 原生对象的术语类和类型是相同的，这意味着对于任何对象`x`,`x.**class**`和`type(x)`的值都是相等的。让我们确认一下:

```py
a = 5
b = 'Hello World'
c = [1, 2, 3]

for var in [a, b, c]:
    print(type(var)==var.__class__)
```

```py
True
True
True
```

Python 中有几个内置类(数据类型):整型、浮点型、字符串型、布尔型、列表型、范围型、元组型、集合型、冷冻集型、字典型，以及其他一些很少使用的类型:

```py
a = 5.2
b = 'Hello World'
c = [1, 2, 3]
d = False
e = range(4)
f = (1, 2, 3)
g = complex(1, -1)

for var in [a, b, c, d, e, f, g]:
    print(var.__class__)
```

```py
<class 'float'>
<class 'str'>
<class 'list'>
<class 'bool'>
<class 'range'>
<class 'tuple'>
<class 'complex'>
```

内置和用户定义的函数有自己的类/类型:

```py
def my_func():
    pass

print(my_func.__class__)
print(min.__class__)
```

```py
<class 'function'>
<class 'builtin_function_or_method'>
```

当我们使用额外的 Python 库时，比如 [pandas](https://app.dataquest.io/m/291) 或 [NumPy](https://app.dataquest.io/m/289) ，我们可以创建只与那些库相关的类/类型的对象:

```py
import pandas as pd
import numpy as np

s = pd.Series({'a': 1, 'b': 2})
df = pd.DataFrame(s)
arr = np.array([1, 2, 3])

print(s.__class__)
print(df.__class__)
print(arr.__class__)
```

```py
<class 'pandas.core.series.Series'>
<class 'pandas.core.frame.DataFrame'>
<class 'numpy.ndarray'>
```

然而，Python 中类的真正好处是我们可以创建自己的类，并用它们来解决特定的任务。让我们看看怎么做。

## 在 Python 中定义类

要定义一个 Python 类，使用`class`关键字，后跟新类的名称和冒号。让我们创建一个非常简单的空类:

```py
class MyClass:
    pass
```

注意 Python 对类的命名约定:使用 **camel case** 样式，其中每个单词都以大写字母开头，单词写在一起，没有任何分隔符。另一个强烈推荐的约定是添加一个 **docstring** :简单描述类用途的类构造函数中的第一个字符串。让我们给我们的类添加一个 docstring:

```py
class MyClass:
    '''This is a new class'''
    pass
```

上面的代码创建了一个名为`MyClass`的新类对象。有趣的是，因为 Python 中的每个对象都与某个类(类型)相关，所以我们的新类(以及所有其他 Python 类，包括内置类)的类/类型是 type:

```py
print(MyClass.__class__)
print(type(MyClass))
```

```py
<class 'type'>
<class 'type'>
```

不过，让我们回到主题上来。一般来说，当定义一个新类时，我们还会创建一个新的`class object`,它包含与该类相关的所有属性(变量和方法)。还有一些具有特殊意义的属性，那些以双下划线**开始和结束的属性，并且与所有的类相关。例如，`doc__`属性返回该类的文档字符串，或者如果没有添加文档字符串，返回`None`。**

 **要访问类属性，请使用点(`.`)运算符:

```py
class TrafficLight:
    '''This is a traffic light class'''
    color = 'green'

    def action(self):
        print('Go')

print(TrafficLight.__doc__)
print(TrafficLight.__class__)
print(TrafficLight.color)
print(TrafficLight.action)
```

这是一堂交通灯课

```py
<class 'type'>
green
<function TrafficLight.action at 0x00000131CD046160>
```

上面的`TrafficLight`类中定义的函数`action()`是一个类方法的例子，而变量`color`是一个类变量。

## 用 Python 声明类对象

我们可以将一个类对象赋给一个变量，创建该类的一个新对象(实例)。我们称这个过程为**实例化**。分配给变量的类对象是该类的副本，具有将该对象与同一类中的其他对象区分开的实值。回到我们的烹饪例子，这是一个由 370 克面粉(当食谱中的范围是 350-400)制成的蛋糕，上面有蜜饯水果装饰，而不是巧克力。

类对象具有以下属性:

*   **状态**–对象的数据(变量)
*   **行为**–对象的方法
*   **身份**–对象的唯一名称

让我们将`TrafficLight`类的一个类对象赋给一个变量`traffic`:

```py
class TrafficLight:
    '''This is a traffic light class'''
    color = 'green'

    def action(self):
        print('Go')

traffic = TrafficLight()

print(traffic.__doc__)
print(traffic.__class__)
print(traffic.color)
print(traffic.action)
print(traffic.action())
```

这是一堂交通灯课

```py
<class '__main__.TrafficLight'>
green
<bound method TrafficLight.action of <__main__.TrafficLight object at 0x00000131CD0313D0>>
Go
None
```

我们创建了一个名为`traffic`的 `TrafficLight`类的新对象实例，并使用对象名(identity)访问其属性。注意新对象的类不同于类本身的类`(**main**.TrafficLight`而不是`type`。另外，`action`属性为类实例和类本身返回不同的值:第一种情况是方法对象，第二种情况是函数对象。调用 `traffic`对象上的`action()`方法(通过指定 `traffic.action()`)给出“Go”，这是目前 `TrafficLight`类的这个方法唯一可能的输出。

在上面的代码中需要注意的另一件重要的事情是，我们在类内部的函数定义中使用了`self` **参数**。然而，当我们在 `traffic`对象上调用`action()`方法时，我们没有添加任何参数。这就是`self`参数的工作方式:每当一个类实例调用它的一个方法时，对象本身作为第一个参数被传递。换句话说，`traffic.action()`相当于`TrafficLight.action(traffic)`。更一般地说，当我们用几个参数定义一个 Python 类方法，创建这个类的一个对象，并在这个对象上调用那个方法时，Python 根据下面的方案在幕后转换它:

就是使用一个适合使用这样一个电子游戏描述的网站来处理

因此，每个 Python 类方法都必须有这个额外的第一个参数来表示对象本身，即使它没有任何其他参数。根据 Python 的命名惯例，建议将其命名为`self`。

让我们使我们的`TrafficLight`类更有意义，同时引入一个新的特殊方法:

```
class TrafficLight:
    '''This is an updated traffic light class'''
    def __init__(self, color):
        self.color = color

    def action(self):
        if self.color=='red':
            print('Stop & wait')
        elif self.color=='yellow':
            print('Prepare to stop')
        elif self.color=='green':
            print('Go')
        else:
            print('Stop drinking 😉')

yellow = TrafficLight('yellow')
yellow.action()
```py

```
Prepare to stop
```py

我们使用`****init**()**` **方法**(又名`class constructor`)来初始化对象的状态(即在对象实例化的时候分配所有的类变量)。在这种情况下，我们创建了一个新的 Python 类`TrafficLight`，它有两个方法:`**init**()`初始化交通灯的颜色，`action()`根据交通灯的颜色建议相应的动作。

## 在 Python 中创建和删除对象属性

在实例化之后，可以向类对象添加新的属性:

```
green = TrafficLight('green')
yellow = TrafficLight('yellow')

green.next_color = 'red'

print(green.next_color)
print(yellow.next_color)
```py

```
red
```py

```
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_3860/40540793.py in <module>
      5 
      6 print(green.next_color)
----> 7 print(yellow.next_color)

AttributeError: 'TrafficLight' object has no attribute 'next_color'
```py

在上面的代码中，我们为`green`对象创建了一个新属性`next_color`，并将其赋给“red”由于这个属性不在`TrafficLightclass`定义中，当我们试图检查另一个对象(`yellow`)时，我们得到一个`AttributeError`。

我们可以使用`del`语句删除一个类对象的任何属性:

```
print(yellow.color)
del yellow.color
print(yellow.color)
```py

```
yellow
```py

```
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_3860/1130729614.py in <module>
      1 print(yellow.color)
      2 del yellow.color
----> 3 print(yellow.color)

AttributeError: 'TrafficLight' object has no attribute 'color'
```py

```
<__main__.TrafficLight object at 0x00000131CAD50610>
```py

```
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_3860/808519506.py in <module>
      1 print(yellow)
      2 del yellow
----> 3 print(yellow)

NameError: name 'yellow' is not defined
```py

## 类和实例变量

类和实例变量之间的区别在于它们的值在类定义中的赋值位置，这也决定了变量的范围。

*   **类变量:**在类声明中赋值，但在任何方法定义之外(包括类构造函数)。它们与该类的所有对象实例相关。
*   **实例变量:**在类方法或构造函数定义内部赋值。它们对于该类的每个对象实例都是唯一的。

```
class TrafficLight:
    '''This is an updated traffic light class'''

    # Class variable
    traffic_light_address = 'NYC_Cranberry_Hicks'

    def __init__(self, color):

        # Instance variable assigned inside the class constructor
        self.color = color

    def action(self):
        if self.color=='red':

            # Instance variable assigned inside a class method
            self.next_color = 'yellow'
            print('Stop & wait')
        elif self.color=='yellow':
            self.next_color = 'green'
            print('Prepare to stop')
        elif self.color=='green':
            self.next_color = 'red'
            print('Go')
        else:
            self.next_color = 'Brandy'
            print('Stop drinking 😉')

# Creating class objects
for c in ['red', 'yellow', 'green', 'fuchsia']:
    c = TrafficLight(c)
    print(c.traffic_light_address)
    print(c.color)
    c.action()
    print(c.next_color)
    print('\n')
```py

```
NYC_Cranberry_Hicks
red
Stop & wait
yellow

NYC_Cranberry_Hicks
yellow
Prepare to stop
green

NYC_Cranberry_Hicks
green
Go
red

NYC_Cranberry_Hicks
fuchsia
Stop drinking 😉
Brandy
```

对于上面所有的类实例，我们有相同的`traffic_light_address`变量的值，这是一个类变量。在类构造函数内部分配了`color`变量，在`action()`类方法内部根据每个类实例的`color`值计算了`next_color`变量。这两个是实例变量。

## 结论

总而言之，我们已经讨论了创建和使用 Python 类的许多方面:

*   什么是 Python 类以及何时使用它们
*   不同类型的 Python 内置类
*   Python 对象的术语*类*和*类型*之间的关系
*   如何在 Python 中定义一个新类
*   类的 Python 命名约定
*   什么是类属性以及如何访问它们
*   如何实例化一个类对象
*   一个类对象的属性
*   什么是`self`参数及其工作原理
*   类构造函数的属性
*   如何访问、创建或删除类对象属性
*   类变量和实例变量的区别

现在您有了一个完整的工具包，可以开始使用 Python 中的类了！**