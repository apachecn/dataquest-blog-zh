# Python 字符串:深入教程(55+代码示例)

> 原文：<https://www.dataquest.io/blog/python-strings/>

April 21, 2022![Python Strings](img/41920b1cb64942e1952d5b29afc3fdbd.png)

数据类型帮助我们对数据项进行分类。它们决定了我们可以对数据项执行的操作种类。在 Python 中，常见的标准[数据类型](http://https://app.dataquest.io/c/114/m/607/ "data types")包括*数字*、*字符串*、*列表*、*元组*、*布尔*、*集合*、*字典*。

在本教程中，我们将重点关注字符串数据类型。我们将讨论如何声明字符串数据类型，字符串数据类型与 ASCII 表之间的关系，字符串数据类型的属性，以及一些重要的字符串方法和操作。

* * *

## 什么是 Python 字符串？

一个**字符串**是一个包含字符序列的对象。字符是长度为 1 的字符串。在 Python 中，单个字符也是字符串。具有讽刺意味的是，Python 编程语言中没有字符数据类型。然而，我们在其他编程语言中发现了字符数据类型，如 C、Kotlin 和 Java。

我们可以使用单引号、双引号、三引号或`str()`函数来声明 Python 字符串。以下代码片段显示了如何在 Python 中声明字符串:

```py
# A single quote string
single_quote = 'a'  # This is an example of a character in other programming languages. It is a string in Python

# Another single quote string
another_single_quote = 'Programming teaches you patience.'

# A double quote string
double_quote = "aa"

# Another double-quote string
another_double_quote = "It is impossible until it is done!"

# A triple quote string
triple_quote = '''aaa'''

# Also a triple quote string
another_triple_quote = """Welcome to the Python programming language. Ready, 1, 2, 3, Go!"""

# Using the str() function
string_function = str(123.45)  # str() converts float data type to string data type

# Another str() function
another_string_function = str(True)  # str() converts a boolean data type to string data type

# An empty string
empty_string = ''

# Also an empty string
second_empty_string = ""

# We are not done yet
third_empty_string = """"""  # This is also an empty string: ''''''
```

在 Python 中获取字符串的另一种方法是使用`input()`函数。`input()`功能允许我们用键盘向程序中插入数值。插入的值作为字符串读取，但是我们可以将它们转换成其他数据类型:

```py
# Inputs into a Python program
input_float = input()  # Type in: 3.142
input_boolean = input() # Type in: True

# Convert inputs into other data types
convert_float = float(input_float)  # converts the string data type to a float
convert_boolean = bool(input_boolean) # converts the string data type to a bool
```

我们使用`type()`函数来确定 Python 中对象的数据类型。它返回对象的类。当对象是字符串时，它返回`str`类。同样，当对象是字典、整数、浮点、元组或布尔时，它分别返回`dict`、`int`、`float`、`tuple`、`bool`类。现在让我们使用`type()`函数来确定前面代码片段中声明的变量的数据类型:

```py
# Data types/ classes with type()

print(type(single_quote))
print(type(another_triple_quote))
print(type(empty_string))

print(type(input_float))
print(type(input_boolean))

print(type(convert_float))
print(type(convert_boolean))
```

```py
 <class><class><class><class><class><class><class>
```

我们已经讨论了如何声明字符串。现在让我们来看看字符串和 ASCII 表之间的关系。

* * *

## ASCII 表与 Python 字符串字符

开发美国信息交换标准代码( [ASCII](https://www.asciitable.com/) )是为了帮助我们将字符或文本映射到数字，因为数字组比文本更容易存储在计算机内存中。ASCII 编码 128 个字符，主要在英语中，用于在计算机和编程中处理信息。ASCII 编码的英文字符包括小写字母(a-z)、大写字母(A-Z)、数字(0-9)和标点符号等符号。

`ord()`函数将长度为 1(一个字符)的 Python 字符串转换成 ASCII 表上的十进制表示，而`chr()`函数将十进制表示转换回字符串。例如:

```py
import string

# Convert uppercase characters to their ASCII decimal numbers
ascii_upper_case = string.ascii_uppercase  # Output: ABCDEFGHIJKLMNOPQRSTUVWXYZ

for one_letter in ascii_upper_case[:5]:  # Loop through ABCDE
    print(ord(one_letter)) 
```

```py
65
66
67
68
69
```

```py
# Convert digit characters to their ASCII decimal numbers
ascii_digits = string.digits  # Output: 0123456789

for one_digit in ascii_digits[:5]:  # Loop through 01234
    print(ord(one_digit)) 
```

```py
48
49
50
51
52
```

在上面的代码片段中，我们遍历了字符串`ABCDE`和`01234`，并将每个字符转换成它们在 [ASCII 表](https://www.asciitable.com/)中的十进制表示。我们也可以用`chr()`函数执行相反的操作，将 ASCII 表上的十进制数转换成 Python 字符串。例如:

```py
decimal_rep_ascii = [37, 44, 63, 82, 100]

for one_decimal in decimal_rep_ascii:
    print(chr(one_decimal))
```

```py
%
,
?
R
d
```

在 ASCII 表上，上述程序输出中的字符串字符映射到它们各自的十进制数。到目前为止，我们已经讨论了如何声明 Python 字符串以及字符串字符如何映射到 ASCII 表。接下来，我们来讨论一个字符串的属性。

* * *

## 字符串属性

**零索引:**字符串中第一个元素的索引为零，而最后一个元素的索引为`len(string) - 1`。例如:

```py
immutable_string = "Accountability"

print(len(immutable_string))
print(immutable_string.index('A'))
print(immutable_string.index('y'))
```

```py
14
0
13
```

**不变性**。这意味着我们不能更新字符串中的字符。例如，我们不能从一个字符串中删除一个元素，或者试图在它的任何索引位置分配一个新元素。如果我们试图更新字符串，它会抛出一个`TypeError`:

```py
immutable_string = "Accountability"

# Assign a new element at index 0
immutable_string[0] = 'B'
```

```py
---------------------------------------------------------------------------

TypeError                                 Traceback (most recent call last)

~\AppData\Local\Temp/ipykernel_11336/2351953155.py in <module>
      2 
      3 # Assign a new element at index 0
----> 4 immutable_string[0] = 'B'

TypeError: 'str' object does not support item assignment
```

然而，我们可以将一个字符串重新分配给`immutable_string`变量，但是我们应该注意到它们不是同一个字符串，因为它们没有指向内存中的同一个对象。Python 不更新旧的字符串对象；它创建了一个新的，正如我们在 ids 中看到的:

```py
immutable_string = "Accountability"
print(id(immutable_string))

immutable_string = "Bccountability"  
print(id(immutable_string)

test_immutable = immutable_string
print(id(test_immutable))
```

```py
2693751670576
2693751671024
2693751671024
```

你会得到和例子中不同的 id，因为我们在不同的计算机上运行程序，所以我们的内存地址是不同的。但是，在同一台计算机上，这两个 id 也应该是不同的。这意味着两个`immutable_string`变量指向内存中不同的地址。我们将最后一个`immutable_string`变量赋给了`test_immutable`变量。可以看到`test_immutable`变量和最后一个`immutable_string`变量指向同一个地址。

**串联:**将两个或多个字符串连接在一起，得到一个带有`+`符号的新字符串。例如:

```py
first_string = "Data"
second_string = "quest"
third_string = "Data Science Path"

fourth_string = first_string + second_string
print(fourth_string)

fifth_string = fourth_string + " " + third_string
print(fifth_string)
```

```py
Dataquest
Dataquest Data Science Path
```

**重复:**一个字符串可以用`*`符号重复。例如:

```py
print("Ha" * 3)
```

```py
HaHaHa
```

**索引和切片:**我们已经知道字符串是零索引的。我们可以用它的索引值访问字符串中的任何元素。我们也可以通过在两个索引值之间切片来获取字符串的子集。例如:

```py
main_string = "I learned R and Python on Dataquest. You can do it too!"

# Index 0
print(main_string[0])

# Index 1
print(main_string[1])

# Check if Index 1 is whitespace
print(main_string[1].isspace())

# Slicing 1
print(main_string[0:11])

# Slicing 2: 
print(main_string[-18:])

# Slicing and concatenation
print(main_string[0:11] + ". " + main_string[-18:])
```

```py
I

True
I learned R
You can do it too!
I learned R. You can do it too!
```

* * *

## 字符串方法

**`str.split(sep=None, maxsplit=-1):`** 字符串拆分方法包含两个属性:`sep`和`maxsplit`。当使用默认值调用该方法时，它会在有空格的地方拆分字符串。此方法返回字符串列表:

```py
string = "Apple, Banana, Orange, Blueberry"
print(string.split())
```

```py
['Apple,', 'Banana,', 'Orange,', 'Blueberry']
```

我们可以看到字符串没有被很好地分割，因为分割后的字符串包含`,`。我们可以使用`sep=','`来拆分任何有`,`的地方:

```py
print(string.split(sep=','))
```

```py
['Apple', ' Banana', ' Orange', ' Blueberry']
```

这比之前的分裂要好。然而，我们可以在一些分割的字符串前看到空白。我们可以用`(sep=', ')`删除它:

```py
# Notice the whitespace after the comma
print(string.split(sep=', '))
```

```py
['Apple', 'Banana', 'Orange', 'Blueberry']
```

现在，绳子被很好地分开了。有时候，我们不想分最大次数。我们可以使用`maxsplit`属性来指定我们想要分割的次数:

```py
print(string.split(sep=', ', maxsplit=1))

print(string.split(sep=', ', maxsplit=2))
```

```py
['Apple', 'Banana, Orange, Blueberry']
['Apple', 'Banana', 'Orange, Blueberry']
```

**`str.splitlines(keepends=False):`** 有时候我们想处理一个边界处有不同换行符(`'\n'`、`\n\n'`、`'\r'`、`'\r\n'`)的语料库。我们想分成句子，而不是单个的单词。我们将使用`splitline`方法来做到这一点。当`keepends=True`时，文本中包含换行符；否则，他们将被排除在外。让我们看看莎士比亚的《麦克白》是如何做到这一点的:

```py
import nltk  # You may have to `pip install nltk` to use this library.

macbeth = nltk.corpus.gutenberg.raw('shakespeare-macbeth.txt')
print(macbeth.splitlines(keepends=True)[:5])
```

```py
['[The Tragedie of Macbeth by William Shakespeare 1603]\n', '\n', '\n', 'Actus Primus. Scoena Prima.\n', '\n']
```

**`str.strip([chars]):`** 我们用`strip`的方法去除字符串两边的尾随空格或字符。例如:

```py
string = "    Apple Apple Apple no apple in the box apple apple             "

stripped_string = string.strip()
print(stripped_string)

left_stripped_string = (
    stripped_string
    .lstrip('Apple')
    .lstrip()
    .lstrip('Apple')
    .lstrip()
    .lstrip('Apple')
    .lstrip()
)
print(left_stripped_string)

capitalized_string = left_stripped_string.capitalize()
print(capitalized_string)

right_stripped_string = (
    capitalized_string
    .rstrip('apple')
    .rstrip()
    .rstrip('apple')
    .rstrip()
)
print(right_stripped_string)
```

```py
Apple Apple Apple no apple in the box apple apple
no apple in the box apple apple
No apple in the box apple apple
No apple in the box
```

在上面的代码片段中，我们使用了`lstrip`和`rstrip`方法，分别从字符串的左侧和右侧删除尾随空格或字符。我们还使用了`capitalize`方法，它将字符串转换成句子的大小写。

**`str.zfill(width):`**`zfill`方法用`0`前缀填充一个字符串，得到指定的`width`。例如:

```py
example = "0.8"  # len(example) is 3
example_zfill = example.zfill(5) # len(example_zfill) is 5
print(example_zfill)
```

```py
000.8
```

**`str.isalpha():`** 如果字符串中的所有字符都是字母，该方法返回`True`；否则，返回`False`:

```py
# Alphabet string
alphabet_one = "Learning"
print(alphabet_one.isalpha())

# Contains whitspace
alphabet_two = "Learning Python"
print(alphabet_two.isalpha())

# Contains comma symbols
alphabet_three = "Learning,"
print(alphabet_three.isalpha())
```

```py
True
False
False
```

类似地，如果字符串字符是字母数字，则`str.isalnum()`返回`True`；如果字符串字符为十进制，则`str.isdecimal()`返回`True`；如果字符串字符是数字，`str.isdigit()`返回`True`；如果字符串字符是数字，则`str.isnumeric()`返回`True`。

如果字符串中的所有字符都是小写，则 **`str.islower()`** 返回`True`。如果字符串中的所有字符都是大写的，`str.isupper()`返回`True`，如果每个单词的首字母都是大写的，`str.istitle()`返回`True`:

```py
# islower() example
string_one = "Artificial Neural Network"
print(string_one.islower())

string_two = string_one.lower()  # converts string to lowercase
print(string_two.islower())

# isupper() example
string_three = string_one.upper() # converts string to uppercase
print(string_three.isupper())

# istitle() example
print(string_one.istitle())
```

```py
False
True
True
True
```

**`str.endswith(suffix)`** 返回的`True`是以指定后缀结尾的字符串。如果字符串以指定的前缀开始，则`str.startswith(prefix)`返回`True`:

```py
sentences = ['Time to master data science', 'I love statistical computing', 'Eat, sleep, code']

# endswith() example
for one_sentence in sentences:
    print(one_sentence.endswith(('science', 'computing', 'Code')))
```

```py
True
True
False
```

```py
# startswith() example
for one_sentence in sentences:
    print(one_sentence.startswith(('Time', 'I ', 'Ea')))
```

```py
True
True
True
```

**`str.find(substring)`** 如果字符串中存在子串，则返回最低索引；否则，它返回-1。`str.rfind(substring)`返回最高指数。如果找到子串，`str.index(substring)`和`str.rindex(substring)`也分别返回子串的最低和最高索引。如果子串不在字符串中，他们抛出`ValueError`。

```py
string = "programming"

# find() and rfind() examples
print(string.find('m'))
print(string.find('pro'))
print(string.rfind('m'))
print(string.rfind('game'))

# index() and rindex() examples
print(string.index('m'))
print(string.index('pro'))
print(string.rindex('m'))
print(string.rindex('game'))
```

```py
6
0
7
-1
6
0
7

---------------------------------------------------------------------------

ValueError                                Traceback (most recent call last)

~\AppData\Local\Temp/ipykernel_11336/3954098241.py in <module>
     11 print(string.index('pro'))  # Output: 0
     12 print(string.rindex('m'))  # Output: 7
---> 13 print(string.rindex('game'))  # Output: ValueError: substring not found

ValueError: substring not found
```

**`str.maketrans(dict_map)`** 从字典映射创建翻译表，`str.translate(maketrans)`用新值替换翻译中的元素。例如:

```py
example = "abcde"
mapped = {'a':'1', 'b':'2', 'c':'3', 'd':'4', 'e':'5'}
print(example.translate(example.maketrans(mapped))) 
```

```py
12345
```

* * *

## 字符串操作

**通过一根绳子循环。**字符串是可迭代的。因此，它们支持使用`for loop`和`enumerate`的循环操作:

```py
# For-loop example
word = "bank"
for letter in word:
    print(letter)
```

```py
b
a
n
k
```

```py
# Enumerate example
for idx, value in enumerate(word):
    print(idx, value)
```

```py
0 b
1 a
2 n
3 k
```

**字符串和关系运算符**:当使用关系运算符(`>`、`<`、`==`等)比较两个字符串时。)，两个字符串的元素通过它们的 ASCII 十进制数逐个索引地进行比较。例如:

```py
print('a' > 'b')
print('abc' > 'b')
```

```py
False
False
```

两种情况下，输出都是`False`。关系运算符首先比较两个字符串的索引`0`中元素的 ASCII 十进制数。由于`b`大于`a`，所以返回`False`；在这种情况下，其他元素的 ASCII 十进制数以及字符串的长度都无关紧要。

当字符串长度相同时，它会比较索引`0`中每个元素的 ASCII 十进制数，直到找到 ASCII 十进制数不同的元素。例如:

```py
print('abd' > 'abc')
```

```py
True
```

在上面的代码片段中，前两个元素具有相同的 ASCII 十进制数；但是，在第三个元素中有一个不匹配，因为`d`大于`c`，所以它返回`True`。在元素的所有 ASCII 数字都匹配的情况下，较长的字符串大于较短的字符串。例如:

```py
print('abcd' > 'abc')
```

```py
True
```

**检查字符串的成员资格。**`in`操作符用于检查一个子串是否是一个字符串的成员:

```py
print('data' in 'dataquest')
print('gram' in 'programming')
```

```py
True
True
```

另一种检查字符串成员、替换子串或匹配模式的方法是使用**正则表达式**

```py
import re

substring = 'gram'
string = 'programming'
replacement = '1234'

# Check membership
print(re.search(substring, string))

# Replace string
print(re.sub(substring, replacement, string))
```

```py
 <re.match object="" span="(3," match="gram">pro1234ming</re.match>
```

**字符串格式化。** `f-string`和`str.format()`方法用于格式化字符串。两者都使用花括号`{}`占位符。例如:

```py
monday, tuesday, wednesday = "Monday", "Tuesday", "Wednesday"

format_string_one = "{} {} {}".format(monday, tuesday, wednesday)
print(format_string_one)

format_string_two = "{2} {1} {0}".format(monday, tuesday, wednesday)
print(format_string_two)

format_string_three = "{one} {two} {three}".format(one=tuesday, two=wednesday, three=monday)
print(format_string_three)

format_string_four = f"{monday} {tuesday} {wednesday}"
print(format_string_four)
```

```py
Monday Tuesday Wednesday
Wednesday Tuesday Monday
Tuesday Wednesday Monday
Monday Tuesday Wednesday
```

`f-strings`可读性更好，并且比`str.format()`方法实现得更快。因此，`f-string`是字符串格式化的首选方法。

**处理引号和撇号**:撇号`(')`代表 Python 中的一个字符串。为了让 Python 知道我们不是在处理一个字符串，我们必须使用 Python 转义字符`(\)`。因此，在 Python 中，撇号被表示为`\'`。与处理撇号不同，Python 中有许多处理引号的方法。它们包括以下内容:

```py
# 1\. Represent string with single quote (`""`) and quoted statement with double quote (`""`)
quotes_one =  '"Friends don\'t let friends use minibatches larger than 32" - Yann LeCun'
print(quotes_one)

# 2\. Represent string with double quote `("")` and quoted statement with escape and double quote `(\"statement\")`
quotes_two =  "\"Friends don\'t let friends use minibatches larger than 32\" - Yann LeCun"
print(quotes_two)

# 3\. Represent string with triple quote `("""""")` and quoted statment with double quote ("")
quote_three = """"Friends don\'t let friends use minibatches larger than 32" - Yann LeCun"""
print(quote_three)
```

```py
"Friends don't let friends use minibatches larger than 32" - Yann LeCun
"Friends don't let friends use minibatches larger than 32" - Yann LeCun
"Friends don't let friends use minibatches larger than 32" - Yann LeCun
```

* * *

## 结论

Python 字符串是不可变的，它们是基本的数据类型之一。我们可以使用单引号、双引号或三引号，或者使用`str()`函数来声明它们。

我们可以将字符串的每个元素映射到 ASCII 表上的一个数字。这是我们在使用关系运算符比较字符串时使用的属性。有许多方法可用于处理字符串。

本文讨论最常用的方法，包括拆分字符串、检查开始和结束字符、填充、检查字符串大小写以及替换字符串中的元素的方法。我们还讨论了用于遍历字符串成员、检查字符串成员资格、格式化字符串以及处理引号和撇号的字符串操作。