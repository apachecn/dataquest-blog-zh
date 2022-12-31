# 如何在 2022 年使用 Python 数据类(初学者指南)

> 原文：<https://www.dataquest.io/blog/how-to-use-python-data-classes/>

November 1, 2022![Dataclasses](img/cb0bfe4cbc81f2044faccf86eb842c80.png)

在 Python 中，数据类是被设计成只保存数据值的类。它们和普通的类没有什么不同，但是它们通常没有其他的方法。它们通常用于存储将在程序或系统的不同部分之间传递的信息。

然而，当创建仅作为数据容器的类时，重复编写`__init__`方法会产生大量的工作和潜在的错误。

`dataclasses` 模块是 Python 3.7 中引入的一个特性，它提供了一种以更简单的方式创建数据类的方法，无需编写方法。

在本文中，我们将看到如何利用这个模块来快速创建新的类，这些类不仅包含了`__init__`，还包含了其他一些已经实现的方法，因此我们不需要手动实现它们。此外，我们只用几行代码就可以做到这一点。

我们希望您有一些中级 python 经验，包括对如何创建类和一般面向对象编程的理解。

## 使用`dataclasses`模块

作为一个开始的例子，假设我们正在实现一个类来存储关于某一组人的数据。对于每个人，我们将有诸如姓名、年龄、身高和电子邮件地址等属性。这是一个普通班级的样子:

```
class Person():
    def __init__(self, name, age, height, email):
        self.name = name
        self.age = age
        self.height = height
        self.email = email
```

然而，如果我们使用`dataclasses`模块，我们需要导入`dataclass`来在我们创建的类中使用它作为装饰器。当我们这样做时，我们不再需要编写 **init** 函数，只需要指定类的属性和它们的类型。下面是同样的`Person`类，以这种方式实现:

```
from dataclasses import dataclass

@dataclass
class Person():
    name: str
    age: int
    height: float
    email: str
```

我们还可以为类属性设置默认值:

```
@dataclass
class Person():
    name: str = 'Joe'
    age: int = 30
    height: float = 1.85
    email: str = '[[email protected]](/cdn-cgi/l/email-protection)'

print(Person())
```

```
 Person(name='Joe', age=30, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)')
```

提醒一下，Python 不接受类和函数中 default 之后的非默认属性，所以这会抛出一个错误:

```
@dataclass
class Person():
    name: str = 'Joe'
    age: int = 30
    height: float = 1.85
    email: str 
```

```
 ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_5540/741473360.py in <module>
          1 @dataclass
    ----> 2 class Person():
          3     name: str = 'Joe'
          4     age: int = 30
          5     height: float = 1.85

    ~\anaconda3\lib\dataclasses.py in dataclass(cls, init, repr, eq, order, unsafe_hash, frozen)
       1019 
       1020     # We're called as @dataclass without parens.
    -> 1021     return wrap(cls)
       1022 
       1023 

    ~\anaconda3\lib\dataclasses.py in wrap(cls)
       1011 
       1012     def wrap(cls):
    -> 1013         return _process_class(cls, init, repr, eq, order, unsafe_hash, frozen)
       1014 
       1015     # See if we're being called as @dataclass or @dataclass().

    ~\anaconda3\lib\dataclasses.py in _process_class(cls, init, repr, eq, order, unsafe_hash, frozen)
        925                 if f._field_type in (_FIELD, _FIELD_INITVAR)]
        926         _set_new_attribute(cls, '__init__',
    --> 927                            _init_fn(flds,
        928                                     frozen,
        929                                     has_post_init,

    ~\anaconda3\lib\dataclasses.py in _init_fn(fields, frozen, has_post_init, self_name, globals)
        502                 seen_default = True
        503             elif seen_default:
    --> 504                 raise TypeError(f'non-default argument {f.name!r} '
        505                                 'follows default argument')
        506 

    TypeError: non-default argument 'email' follows default argument
```

一旦定义了类，就很容易实例化一个新对象并访问其属性，就像使用标准类一样:

```
person = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
print(person.name)
```

```
 Joe
```

到目前为止，我们已经使用了常规的数据类型，如 string、integer 和 float 我们还可以将`dataclass`与`typing`模块结合起来，在类中创建任何类型的属性。例如，让我们给`Person`添加一个`house_coordinates`属性:

```
from typing import Tuple

@dataclass
class Person():
    name: str
    age: int
    height: float
    email: str
    house_coordinates: Tuple

print(Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)', (40.748441, -73.985664)))
```

```
 Person(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)', house_coordinates=(40.748441, -73.985664))
```

按照同样的逻辑，我们可以创建一个数据类来保存`Person`类的多个实例:

```
from typing import List

@dataclass
class People():
    people: List[Person]
```

注意，`People`类中的`people`属性被定义为`Person`类的实例列表。例如，我们可以像这样实例化一个`People`对象:

```
joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)', (40.748441, -73.985664))
mary = Person('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)', (-73.985664, 40.748441))

print(People([joe, mary]))
```

```
 People(people=[Person(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)', house_coordinates=(40.748441, -73.985664)), Person(name='Mary', age=43, height=1.67, email='[[email protected]](/cdn-cgi/l/email-protection)', house_coordinates=(-73.985664, 40.748441))])
```

这允许我们将属性定义为我们想要的任何类型，也可以是数据类型的组合。

## 表示和比较

正如我们前面提到的，`dataclass`不仅实现了`__init__`方法，还实现了其他几个方法，包括`__repr__`方法。在一个常规的类中，我们使用这个方法来显示类中一个对象的表示。

例如，当我们调用对象时，我们将定义如下例所示的方法:

```
class Person():
    def __init__(self, name, age, height, email):
        self.name = name
        self.age = age
        self.height = height
        self.email = email

    def __repr__(self):
        return (f'{self.__class__.__name__}(name={self.name}, age={self.age}, height={self.height}, email={self.email})')

person = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
print(person)
```

```
 Person(name=Joe, age=25, height=1.85, [[email protected]](/cdn-cgi/l/email-protection))
```

然而，当使用`dataclass`时，没有必要写这些:

```
@dataclass
class Person():
    name: str
    age: int
    height: float
    email: str    

person = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
print(person)
```

```
 Person(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)')
```

注意，如果没有这些代码，输出就相当于标准 Python 类的输出。

如果我们想要定制我们类的表示，我们总是可以覆盖它:

```
@dataclass
class Person():
    name: str
    age: int
    height: float
    email: str

    def __repr__(self):
        return (f'''This is a {self.__class__.__name__} called {self.name}.''')

person = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
print(person)
```

```
 This is a Person called Joe.
```

请注意，表示的输出是定制的。

说到比较，`dataclasses`模块让我们的生活更轻松。例如，我们可以像这样直接比较一个类的两个实例:

```
@dataclass
class Person():
    name: str = 'Joe'
    age: int = 30
    height: float = 1.85
    email: str = '[[email protected]](/cdn-cgi/l/email-protection)'

print(Person() == Person())
```

```
 True
```

请注意，我们使用了默认属性来缩短示例。

在这种情况下，比较是有效的，因为`dataclass`在幕后创建了一个执行比较的`__eq__`方法。没有装饰器，我们必须自己创建这个方法。

如果使用标准 Python 类，即使这些类实际上彼此相等，相同的比较也会产生不同的结果:

```
class Person():
    def __init__(self, name='Joe', age=30, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)'):
        self.name = name
        self.age = age
        self.height = height
        self.email = email

print(Person() == Person())
```

```
 False
```

如果不使用`dataclass`装饰器，该类不会测试两个实例是否相等。因此，默认情况下，Python 将使用对象的`id`进行比较，正如我们在下面看到的，它们是不同的:

```
print(id(Person()))
print(id(Person()))
```

```
1734438049008
1734438050976
```

所有这些意味着我们必须编写一个`__eq__`方法来进行比较:

```
class Person():
    def __init__(self, name='Joe', age=30, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)'):
        self.name = name
        self.age = age
        self.height = height
        self.email = email

    def __eq__(self, other):
        if isinstance(other, Person):
            return (self.name, self.age,
                    self.height, self.email) == (other.name, other.age,
                                                 other.height, other.email)
        return NotImplemented

print(Person() == Person())
```

```
 True
```

现在我们看到这两个对象是相等的，但是我们不得不写更多的代码来得到这个结果。

## `@dataclass`参数

正如我们在上面看到的，当使用`dataclass`装饰器时，`__init__`、`__repr__`和`__eq__`方法为我们实现。所有这些方法的创建由`dataclass`的`init`、`repr`和`eq`参数设置。这三个参数默认为`True`。如果其中一个是在类内部创建的，那么该参数将被忽略。

然而，在继续之前，我们还需要了解`dataclass`的其他参数:

*   `order`:启用类的排序，我们将在下一节看到。默认为`False`。
*   `frozen`:当`True`时，类的实例内的值在创建后不能修改。默认是`False`。

您可以在[文档](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass)中查看其他一些方法。

## 整理

在处理数据时，我们经常需要对值进行排序。在我们的场景中，我们可能希望根据一些属性对不同的人进行排序。为此，我们将使用上面提到的`dataclass`装饰器的`order`参数，它支持类中的排序:

```
@dataclass(order=True)
class Person():
    name: str
    age: int
    height: float
    email: str
```

当`order`参数设置为`True`时，自动生成用于排序的`__lt__`(小于)、`__le__`(小于等于)、`__gt__`(大于)、`__ge__`(大于等于)方法。

让我们实例化我们的`joe`和`mary`对象，看看一个是否比另一个大:

```
joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
mary = Person('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)')

print(joe > mary)
```

```
 False
```

Python 告诉我们`joe`不大于`mary`，但是基于什么标准呢？该类将对象作为包含其属性的元组进行比较，如下所示:

```
print(('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)') > ('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)'))
```

```
 False
```

由于字母“J”在“M”之前，所以它表示`joe < mary`。如果名称相同，它将移动到每个元组中的下一个元素。实际上，它是按字母顺序比较对象。尽管根据我们正在处理的问题，这可能有一定的意义，但我们希望能够控制对象的排序方式。

为了实现这一点，我们将利用`dataclasses`模块的另外两个特性。

首先是`field`函数。此函数用于单独定制数据类的一个属性，这允许我们定义依赖于另一个属性的新属性，并且仅在对象被实例化后创建。

在我们的排序问题中，我们将使用`field`在类中创建一个`sort_index`属性。该属性只能在对象实例化后创建，并且是`dataclasses`用于排序的属性:

```
from dataclasses import dataclass, field

@dataclass(order=True)
class Person():
    sort_index: int = field(init=False, repr=False)
    name: str
    age: int
    height: float
    email: str
```

我们作为`False`传递的两个参数声明这个属性不在`__init__`中，并且当我们调用`__repr__`时它不应该被显示。在`field`功能中还有其他参数，您可以在文档中查看。

在我们引用了这个新属性之后，我们将使用第二个新工具:`__post_int__`方法。顾名思义，这个方法紧接在`__init__`方法之后执行。我们将使用`__post_int__`来定义`sort_index`，就在对象创建之后。举个例子，假设我们想根据人们的年龄来比较他们。方法如下:

```
@dataclass(order=True)
class Person():
    sort_index: int = field(init=False, repr=False)
    name: str
    age: int
    height: float
    email: str

    def __post_init__(self):
        self.sort_index = self.age
```

如果我们做同样的比较，我们知道乔比玛丽年轻:

```
joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
mary = Person('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)')

print(joe > mary)
```

```
 False
```

如果我们想按身高对人进行排序，我们可以使用以下代码:

```
@dataclass(order=True)
class Person():
    sort_index: float = field(init=False, repr=False)
    name: str
    age: int
    height: float
    email: str

    def __post_init__(self):
        self.sort_index = self.height

joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
mary = Person('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)')

print(joe > mary)
```

```
 True
```

乔比玛丽高。注意，我们将`sort_index`设置为`float`。

我们能够在数据类中实现排序，而不需要编写多个方法。

## 使用不可变数据类

我们上面提到的`@dataclass`的另一个参数是`frozen`。当设置为`True`时，`frozen`不允许我们在对象创建后修改其属性。

有了`frozen=False`，我们可以很容易地进行这样的修改:

```
@dataclass()
class Person():
    name: str
    age: int
    height: float
    email: str

joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')

joe.age = 35
print(joe)
```

```
 Person(name='Joe', age=35, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)')
```

我们创建了一个`Person`对象，然后修改了`age`属性，没有任何问题。

但是，当设置为`True`时，任何修改对象的尝试都会引发错误:

```
@dataclass(frozen=True)
class Person():
    name: str
    age: int
    height: float
    email: str

joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')

joe.age = 35
print(joe)
```

```
 ---------------------------------------------------------------------------

    FrozenInstanceError                       Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_5540/2036839054.py in <module>
          8 joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
          9 
    ---> 10 joe.age = 35
         11 print(joe)

    <string> in __setattr__(self, name, value)

    FrozenInstanceError: cannot assign to field 'age'
```

请注意，错误消息显示的是`FrozenInstanceError`。

有一个技巧可以修改不可变数据类的值。如果我们的类包含一个可变属性，即使类被冻结了，这个属性也会改变。这看起来似乎没有意义，但是让我们看一个例子。

让我们回忆一下我们在本文前面创建的`People`类，但是现在让我们把它变成不可变的:

```
@dataclass(frozen=True)
class People():
    people: List[Person]

@dataclass(frozen=True)
class Person():
    name: str
    age: int
    height: float
    email: str
```

然后我们创建了两个`Person`类的实例，并使用它们创建了一个`People`的实例，我们将其命名为`two_people`:

```
joe = Person('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
mary = Person('Mary', 43, 1.67, '[[email protected]](/cdn-cgi/l/email-protection)')

two_people = People([joe, mary])
print(two_people)
```

```
 People(people=[Person(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)'), Person(name='Mary', age=43, height=1.67, email='[[email protected]](/cdn-cgi/l/email-protection)')])
```

`People`类中的`people`属性是一个列表。我们可以很容易地在`two_people`对象中访问这个列表中的值:

```
print(two_people.people[0])
```

```
 Person(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)')
```

因此，尽管`Person`和`People`类都是不可变的，但列表不是，这意味着我们可以改变列表中的值:

```
two_people.people[0] = Person('Joe', 35, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)')
print(two_people.people[0])
```

```
 Person(name='Joe', age=35, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)')
```

请注意，年龄现在是 35 岁。

我们没有改变不可变类的任何对象的属性，但是我们用不同的元素替换了列表的第一个元素，并且列表是可变的。

记住，为了安全地使用不可变的数据类，类的所有属性也应该是不可变的。

## 用`dataclasses`继承

`dataclasses`模块也支持继承，这意味着我们可以创建一个使用另一个数据类属性的数据类。仍然使用我们的`Person`类，我们将创建一个新的`Employee`类，它继承了`Person`的所有属性。
于是我们有了`Person`:

```
@dataclass(order=True)
class Person():
    name: str
    age: int
    height: float
    email: str
```

还有新的`Employee`类:

```
@dataclass(order=True)
class Employee(Person):
    salary: int
    departament: str
```

现在我们可以使用`Person`类的所有属性创建一个`Employee`类的对象:

```
print(Employee('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)', 100000, 'Marketing'))
```

```
 Employee(name='Joe', age=25, height=1.85, email='[[email protected]](/cdn-cgi/l/email-protection)', salary=100000, departament='Marketing')
```

从现在开始，我们也可以在`Employee`类中使用本文中看到的所有内容。

请注意默认属性。假设我们在`Person`中有默认属性，但在`Employee`中没有。如下面的代码所示，这种情况会引发一个错误:

```
@dataclass
class Person():
    name: str = 'Joe'
    age: int = 30
    height: float = 1.85
    email: str = '[[email protected]](/cdn-cgi/l/email-protection)'

@dataclass(order=True)
class Employee(Person):
    salary: int
    departament: str

print(Employee('Joe', 25, 1.85, '[[email protected]](/cdn-cgi/l/email-protection)', 100000, 'Marketing'))
```

```
 ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_5540/1937366284.py in <module>
          9 
         10 @dataclass(order=True)
    ---> 11 class Employee(Person):
         12     salary: int
         13     departament: str

    ~\anaconda3\lib\dataclasses.py in wrap(cls)
       1011 
       1012     def wrap(cls):
    -> 1013         return _process_class(cls, init, repr, eq, order, unsafe_hash, frozen)
       1014 
       1015     # See if we're being called as @dataclass or @dataclass().

    ~\anaconda3\lib\dataclasses.py in _process_class(cls, init, repr, eq, order, unsafe_hash, frozen)
        925                 if f._field_type in (_FIELD, _FIELD_INITVAR)]
        926         _set_new_attribute(cls, '__init__',
    --> 927                            _init_fn(flds,
        928                                     frozen,
        929                                     has_post_init,

    ~\anaconda3\lib\dataclasses.py in _init_fn(fields, frozen, has_post_init, self_name, globals)
        502                 seen_default = True
        503             elif seen_default:
    --> 504                 raise TypeError(f'non-default argument {f.name!r} '
        505                                 'follows default argument')
        506 

    TypeError: non-default argument 'salary' follows default argument
```

如果基类有默认属性，那么从它派生的类中的所有属性也必须有默认值。

## 结论

在本文中，我们看到了`dataclasses`模块是一个非常强大的工具，可以快速、直观地创建数据类。虽然我们在本文中已经看到了很多，但是该模块包含了更多的工具，并且总是有更多的东西需要学习。

到目前为止，我们已经学会了如何:

*   使用`dataclasses`定义一个类

*   使用默认属性及其规则

*   创建一种表示方法

*   比较数据类

*   排序数据类

*   对数据类使用继承

*   使用不可变数据类