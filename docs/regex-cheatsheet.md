# 正则表达式备忘单:Python 正则表达式快速指南

> 原文：<https://www.dataquest.io/blog/regex-cheatsheet/>

April 2, 2018![regex-cheat-sheet-data-science-pdf](img/9eb4c67891a975cd089647c06223ca28.png)

学习数据科学的困难之处在于记住所有的语法。虽然在 Dataquest，我们提倡习惯于查阅 [Python 文档](https://docs.python.org/3/)，但有时有一个方便的 PDF 参考也不错，所以我们整理了这个 Python 正则表达式(regex)备忘单来帮助你！

这个正则表达式备忘单基于 Python 3 关于正则表达式的[文档。](https://docs.python.org/3/library/re.html)

如果你对学习 Python 感兴趣，我们有免费的交互式[初级](https://www.dataquest.io/course/python-for-data-science-fundamentals/)和[中级](https://www.dataquest.io/course/python-for-data-science-intermediate/) Python 编程课程，你应该去看看。

## 数据科学的正则表达式(PDF)

[![python-regular-expressions-cheatsheet_pic](img/ba6c3642f32e72960178355e398f7eaf.png)](https://www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf)

[在此下载正则表达式备忘单](https://www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf)

## 特殊字符

`^` |匹配字符串开头右侧的表达式。它匹配字符串中每个`\n`之前的每个这样的实例。

`$` |匹配字符串末尾左侧的表达式。它匹配字符串中每个`\n`之前的每个这样的实例。

`.` |匹配任何字符，除了像`\n`这样的行终止符。

`\` |转义特殊字符或表示字符类别。

`A|B` |匹配表达式`A`或`B`。如果先匹配到`A`，则不尝试`B`。

`+` |贪婪地匹配表达式左侧 1 次或多次。

`*` |贪婪地匹配其左边的表达式 0 次或更多次。

`?` |贪婪地匹配其左边的表达式 0 或 1 次。但是如果将`?`添加到限定符(`+`、`*`和`?`本身)中，它将以非贪婪的方式执行匹配。

`{m}` |匹配其左侧的表达式`m`次，且不少于。

`{m,n}` |匹配其左边的表达式`m`到`n`次，并且不少于。

`{m,n}?` |匹配其左边的表达式`m`次，忽略`n`。参见上面的`?`。

## 字符类(又名特殊序列)

`\w` |匹配字母数字字符，表示`a-z`、`A-Z`和`0-9`。它也匹配下划线，`_`。

`\d` |匹配数字，表示`0-9`。

`\D` |匹配任何非数字。

`\s` |匹配空白字符，包括`\t`、`\n`、`\r`和空格字符。

`\S` |匹配非空白字符。

`\b` |匹配单词开头和结尾的边界(或空字符串)，即`\w`和`\W`之间的边界。

`\B` |匹配`\b`不匹配的地方，即`\w`字符的边界。

`\A` |无论是在单行模式还是多行模式下，都在字符串的绝对起始处匹配其右侧的表达式。

`\Z` |无论是在单行模式还是多行模式下，都匹配位于字符串绝对末尾左侧的表达式。

## 设置

`[ ]` |包含一组要匹配的字符。

`[amk]` |匹配`a`、`m`或`k`。它与`amk`不符。

`[a-z]` |匹配从`a`到`z`的任意字母。

`[a\-z]` |匹配`a`、`-`或`z`。它匹配`-`，因为`\`转义了它。

`[a-]` |匹配`a`或`-`，因为`-`没有被用来表示一系列字符。

`[-a]` |同上，匹配`a`或`-`。

`[a-z0-9]` |匹配从`a`到`z`以及从`0`到`9`的字符。

`[(+*)]` |特殊字符在集合中变成文字，所以匹配`(`、`+`、`*`、`)`。

`[^ab5]` |添加`^`排除集合中的任何字符。这里，它匹配不是`a`、`b`或`5`的字符。

## 组

`( )` |匹配括号内的表达式并对其进行分组。

`(? )` |像这样的括号内，`?`作为扩展符号。它的含义取决于紧挨着它右边的字符。

`(?PAB)` |匹配表达式`AB`，可以用组名访问。

`(?aiLmsux)` |此处，`a`、`i`、`L`、`m`、`s`、`u`、`x`为标志:

*   `a` —仅匹配 ASCII
*   `i` —忽略大小写
*   `L` —依赖于语言环境
*   `m` —多行
*   `s` —匹配所有
*   `u` —匹配 unicode
*   `x` —详细

`(?:A)` |匹配由`A`表示的表达式，但与`(?PAB)`不同，之后无法检索。

`(?#...)` |一个评论。内容是给我们看的，不是用来配的。

`A(?=B)` |前瞻断言。只有在表达式`A`后面跟有`B`时，它才与表达式`A`匹配。

`A(?!B)` |负前瞻断言。只有当表达式`A`后面没有`B`时，它才与表达式`A`匹配。

`(?<=B)A` |正向回顾断言。只有当`B`紧挨着它的左边时，它才与表达式`A`匹配。这只能匹配固定长度的表达式。

`(?<!B)A` |否定回顾断言。只有当`B`不在它的左边时，它才匹配表达式`A`。这只能匹配固定长度的表达式。

`(?P=name)` |匹配先前名为“name”的组所匹配的表达式。

`(...)\1` |数字`1`对应第一个要匹配的组。如果我们想匹配同一个表达式的更多实例，只需使用它的编号，而不是再次写出整个表达式。我们可以使用从`1`到`99`这样的组和它们相应的编号。

## 流行的 Python `re`模块函数

`re.findall(A, B)` |匹配字符串`B`中表达式`A`的所有实例，并在列表中返回它们。

`re.search(A, B)` |匹配字符串`B`中表达式`A`的第一个实例，并将其作为 re match 对象返回。

`re.split(A, B)` |使用分隔符`A`将字符串 B 拆分成一个列表。

`re.sub(A, B, C)` |将`A`替换为`C`字符串中的`B`。

## Python 的有用正则表达式资源:

*   [Python 正则表达式数据科学教程](https://www.dataquest.io/blog/regular-expressions-data-scientists/)
*   [Python 3 re 模块文档](https://docs.python.org/3/library/re.html)
*   [在线正则表达式测试器和调试器](https://regex101.com/)

### 成为数据分析师！

立即学习成为数据分析师所需的技能。注册一个免费帐户，获得免费的交互式 Python、R 和 SQL 课程内容。

[Sign up now!](https://app.dataquest.io/signup)

*(不需要信用卡！)*