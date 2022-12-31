# Python if else 教程:控制代码流

> 原文：<https://www.dataquest.io/blog/python-if-else/>

October 23, 2019![photograph of tacos](img/d6dfa350908ba9a0a7da5e0a8e9c6040.png)

编程时，控制在什么情况下运行什么代码的流程是极其重要的。Python `if` `else`命令的作用类似于数字交通警察，允许您定义在满足特定条件时运行的代码块。`if` `else`语法是您将要学习的最重要的 Python 语法之一。

在本教程中，你将学习如何使用 Python `if` `else`来控制你的代码。我们假设您已经了解一些 Python 基础知识，例如:

*   如何读取 CSV 文件
*   列表、字符串和整数等基本 Python 类型
*   使用 for 循环处理列表。

如果你对这些还不适应，我们推荐这个免费的交互式 [Python 基础课程](https://www.dataquest.io/course/python-for-data-science-fundamentals/)，它教授所有这些(以及 Python `if` `else`)!)

## Taco 数据集

我们将学习如何使用 Python `if` `else`，同时使用一个数据集，该数据集总结了 Dataquest 在线聊天中特定月份使用的虚拟玉米卷。

在 Dataquest，我们在空闲时间提供虚拟玉米卷(使用 [HeyTaco](https://www.heytaco.chat/) )作为一种感谢或奖励出色完成工作的同事的方式。你可以给某人一个玉米卷来表达你的感激之情，就像这样:

![giving a taco](img/f62e3aed5d50ca2ba5816004d77c5dff.png)

我们将对 HeyTaco 的数据进行分析，回答一些关于人们捐赠习惯的基本问题。数据集存储在 CSV 文件`"tacos.csv"`中，如果你想跟随本教程，你可以在这里下载它[。(在此数据集中，我们更改了名称以保护 Dataquest 员工的隐私)。](https://dq-blog-files.s3.amazonaws.com/if-else/tacos.csv)

让我们从读入 CSV 文件开始，查看文件的前几行:

```
import csv

f = open('tacos.csv')
tacos = list(csv.reader(f))

print(tacos[:5])
```

```
[['name', 'department', 'given', 'received'],
 ['Amanda', 'content', '4', '3'],
 ['Angela', 'engineering', '7', '20'],
 ['Brandon', 'marketing', '31', '26'],
 ['Brian', 'product', '13', '6']]
```

每行代表一个为公司工作的人。数据集有四列:

*   `name`:人员姓名(这些姓名是虚构的，但数据代表 Dataquest 的实际员工！)
*   `department`:这个人在哪个部门(或团队)工作。
*   `given`:那个人给别人的玉米饼的数量。
*   这个人从其他人那里收到的玉米卷的数量

让我们删除第一行，因为它包括列名——我们的数据结构很简单，所以我们可以在进行过程中记住它们(或者如果您忘记了，可以参考文章的顶部！)

删除列名后，让我们再次查看数据的前五行:

```
tacos = tacos[1:]

print(tacos[:5])
```

```
[['Amanda', 'content', '4', '3'],
 ['Angela', 'engineering', '7', '20'],
 ['Brandon', 'marketing', '31', '26'],
 ['Brian', 'product', '13', '6'],
 ['Clinton', 'operations', '38', '9']]
```

## 准备数据

尽管第三列和第四列中的数据(代表每个人给予和接受的玉米饼数量)是数字，但它们被存储为字符串。我们可以看出它们是字符串，因为它们周围有引号:`'4'`而不是`4`。

为了对数据进行计算，我们需要将它们转换成整数，一种数字 Python 类型。

让我们使用一个`for`循环来遍历数据的每一行，并将第 3 列和第 4 列(位于索引 2 和 3 处)转换为整数类型:

```
for person in tacos:
    person[2] = int(person[2])
    person[3] = int(person[3])

print(tacos[:5])
```

```
[['Amanda', 'content', 4, 3],
 ['Angela', 'engineering', 7, 20],
 ['Brandon', 'marketing', 31, 26],
 ['Brian', 'product', 13, 6],
 ['Clinton', 'operations', 38, 9]]
```

您现在可以看到引号被移除了(例如`4`)，表明这些值现在是整数而不是字符串。

## 在数据中寻找平均值

让我们从一些基本的分析开始——找出每个人给予和接受的玉米卷的平均数量。

为此，我们将把 given 和 received 列提取到单独的列表中，这样我们可以更容易地进行计算:

```
given = []
received = []

for person in tacos:
    given.append(person[2])
    received.append(person[3])

print(given[:5])
```

```
[4, 7, 31, 13, 38]

```

接下来，我们将这两个列表相加，然后除以长度(或值的数量)来求平均值:

```
given_avg = sum(given) / len(given)
received_avg = sum(received) / len(received)

print("Avg tacos given: ", given_avg)
print("Avg tacos received: ", received_avg)
```

```
Avg tacos given:  16.322580645161292
Avg tacos received:  16.322580645161292

```

平均数量的玉米卷给予和收到是相同的！当你想到这一点时，这是有道理的，因为某人给的每一个玉米卷都必须被其他人收到。

我们可能有兴趣回答的另一个问题是，公司不同部门的平均付出和收获相比如何。让我们从考察“内容”团队开始。

要做到这一点，我们需要像以前一样提取给定和收到的 tacos 的列表，但仅当行的部门是“content”时。我们刚刚描述的称为**条件**，我们将需要使用 Python `if`来检查该条件！

## Python if

你可以把 Python `if`看作是一个决定。在我们的例子中，我们需要问一个问题:这个人属于“内容”团队吗？我们在代码中采取的行动取决于这个问题的答案或条件。这就是为什么 Python `if`有时也被称为**条件表达式**的原因。

下图显示了我们需要用来创建符合条件的值列表的逻辑:

![Python if flow diagram](img/cdec95e75c54a310b7a649c19fcd2e6d.png)

让我们看看如何使用 Python `if`处理两个单独的行。首先，让我们打印第一行和第二行，这样我们可以提醒自己它们的值:

```
first_row = tacos[0]
print(first_row)
```

```
['Amanda', 'content', 4, 3]

```

```
second_row = tacos[1]
print(second_row)
```

```
['Angela', 'engineering', 7, 20]

```

第一行包含内容团队的 Amanda，而第二行包含工程团队的 Angela。让我们看看如何使用 Python `if`语法来打印一些输出，前提是这个人来自内容团队。

我们将使用`==`操作符来比较团队和字符串“content”。Python 中的`==`运算符的意思是“等于”。

我们可以在`if`条件中使用的其他一些常见运算符包括:

*   `!=`:不等于
*   `>`:大于
*   `<`:小于
*   `>=`:大于或等于
*   `<=`:小于或等于

```
team = first_row[1]

if team == 'content':
    print("This person comes from the content team.")
```

```
This person comes from the content team.

```

因为 Amanda 来自内容团队，所以我们的`print()`函数被执行，我们看到了输出。让我们从之前的图表中追溯路径，以了解发生了什么:

![Python if flow diagram highlighted for when condition is met](img/be560a7bcd68b63812c815f7f352a2ad.png)

让我们花一点时间仔细看看我们使用的语法，并标记不同的部分，以便我们可以理解发生了什么。

![Python if syntax with labeled components](img/1f827180f609260d4446a359e7dffd4e.png)

现在我们对代码有了更好的理解，让我们对第二行尝试同样的代码，看看会发生什么:

```
team = second_row[1]

if team == 'content':
    print("This person comes from the content team.")
```

当我们运行上面的代码时，我们没有得到任何输出，因为 Angela 来自工程团队，而不是内容团队。让我们从之前的图表中追溯路径，以了解发生了什么。

![Python if flow diagram highlighted for when condition isn't met](img/85a6dd64777ce09ebd1fffe5ab64b29e.png)

## 使用 Python if 带 For 循环

现在我们已经了解了 Python `if`的基本工作原理，让我们在一个循环中使用它来获取内容团队的“给定”和“接收”值:

```
given_content = []
received_content = []

for person in tacos:
    team = person[1]
    if team == 'content':
        given_content.append(person[2])
        received_content.append(person[3])

print(given_content)
```

```
[4, 25, 10, 6, 0, 16, 8, 32]

```

我们打印了上面的`given_content`列表，我们可以看到内容团队的 8 名成员的值已经收集在一起。现在让我们来计算团队平均值:

```
given_content_avg = sum(given_content) / len(given_content)
received_content_avg = sum(received_content) / len(received_content)

print("Avg tacos given, content team: ", given_content_avg)
print("Avg tacos received, content team: ", received_content_avg)
```

```
Avg tacos given, content team:  12.625
Avg tacos received, content team:  6.0

```

我们可以看到，内容团队成员给予玉米卷的频率大约是他们收到的两倍。我们还可以将这些数字与总体平均值进行比较，发现:

*   内容团队成员给的玉米卷比总体平均少 25%
*   内容团队成员收到的玉米卷比总体平均水平少 60%

## 使用 Python if else 来改进我们的分析

当我们将内容团队成员与总体平均水平进行比较时，总体平均水平*包括*内容团队成员。将内容团队与内容团队中的每个人*而不是*进行比较可能会很有趣。

为此，我们需要使用 Python 的一个新部分`if`—`else`子句。else 子句在`if`之后，指定如果`if` *中的条件不匹配*时要运行的一行或多行代码。

让我们看看前面的图表，看看添加的`else`子句是什么样子的:

![Python if else flow diagram](img/c652bf06487c4c35bf9cf7e23cde23a4.png)

让我们修改前面只查看第二行的代码，添加一个`else`子句。在我们开始之前，让我们快速提醒自己第二行的内容

```
print(second_row)
```

```
['Angela', 'engineering', 7, 20]

```

好，我们来添加`else`子句:

```
team = second_row[1]

if team == 'content':
    print("This person comes from the content team.")
else:
    print("This person doesn't come from the content team.")
```

```
This person doesn't come from the content team.

```

您可以看到我们的`else`子句中的代码被执行，因为 Angela 不属于内容团队。

让我们追溯之前图表中的路径:

![Python if else flow diagram highlighted for when condition isn't met](img/e75f7e0208b512cbf26f29d4b99e4599.png)

最后，让我们在循环中添加一个`else`子句，并计算两组的平均值:

```
given_content = []
received_content = []

given_other = []
received_other = []

for person in tacos:
    team = person[1]
    if team == 'content':
        given_content.append(person[2])
        received_content.append(person[3])
    else:
        given_other.append(person[2])
        received_other.append(person[3])

given_content_avg = sum(given_content) / len(given_content)
received_content_avg = sum(received_content) / len(received_content)
given_other_avg = sum(given_other) / len(given_other)
received_other_avg = sum(received_other) / len(received_other)

print("Avg tacos given, content team: ", given_content_avg)
print("Avg tacos given, other teams: ", given_other_avg)
print("Avg tacos received, content team: ", received_content_avg)
print("Avg tacos received, other teams: ", received_other_avg)
```

```
Avg tacos given, content team:  12.625
Avg tacos given, other teams:  17.608695652173914
Avg tacos received, content team:  6.0
Avg tacos received, other teams:  19.91304347826087

```

我们可以看到，内容团队给的玉米卷比其他团队少 30%左右，收到的玉米卷比其他团队少 70%左右。

## Python elif

如果我们想计算在以下情况下赠送和收到的玉米卷会怎么样:

*   内容团队
*   工程团队
*   所有其他团队

为此，我们需要一个新工具:Python `elif`。与`else`子句一样，`elif`子句必须跟在`if`子句之后。它允许我们堆叠第二个条件，只有在第一个条件不满足时才进行评估。这一开始听起来可能会令人困惑，但是当你想到它的名字——else if——你就会明白这是在一个`else`内添加另一个`if`的捷径。

让我们看看前面的图表，看看添加的`elif`子句是什么样子的:

![Python if elif else flow diagram](img/8117acf3e42af22056832e4713bbf60c.png)

让我们在独立代码中添加一个`elif`来检查某人是在内容团队还是在工程团队。首先，让我们再次快速提醒自己第二行的内容:

```
print(second_row)
```

```
['Angela', 'engineering', 7, 20]

```

让我们添加`elif`子句:

```
team = second_row[1]

if team == 'content':
    print("This person comes from the content team.")
elif team == 'engineering':
    print("This person comes from the engineering team.")
else:
    print("This person doesn't come from the content or engineering teams.")
```

```
This person comes from the engineering team.

```

您可以看到我们的`elif`子句中的代码被执行，因为 Angela 属于工程团队。

让我们追溯之前图表中的路径:

![Python if elif else flow diagram highlighted for when elif condition is met](img/537b41f11d60f90c3b5ebee42e6d35b4.png)

最后，让我们在循环中添加一个`elif`子句，并计算所有三组的平均值:

```
given_content = []
received_content = []

given_engineering = []
received_engineering = []

given_other = []
received_other = []

for person in tacos:
    team = person[1]
    if team == 'content':
        given_content.append(person[2])
        received_content.append(person[3])
    elif team == 'engineering':
        given_engineering.append(person[2])
        received_engineering.append(person[3])
    else:
        given_other.append(person[2])
        received_other.append(person[3])

given_content_avg = sum(given_content) / len(given_content)
received_content_avg = sum(received_content) / len(received_content)

given_engineering_avg = sum(given_engineering) / len(given_engineering)
received_engineering_avg = sum(received_engineering) / len(received_engineering)

given_other_avg = sum(given_other) / len(given_other)
received_other_avg = sum(received_other) / len(received_other)

print("Avg tacos given, content team: ", given_content_avg)
print("Avg tacos given, engineering team: ", given_engineering_avg)
print("Avg tacos given, other teams: ", given_other_avg)
print() # this prints an empty line
print("Avg tacos received, content team: ", received_content_avg)
print("Avg tacos received, engineering team: ", received_engineering_avg)
print("Avg tacos received, other teams: ", received_other_avg)
```

```
Avg tacos given, content team:  12.625
Avg tacos given, engineering team:  20.166666666666668
Avg tacos given, other teams:  16.705882352941178

Avg tacos received, content team:  6.0
Avg tacos received, engineering team:  26.166666666666668
Avg tacos received, other teams:  17.705882352941178

```

我们的分析表明，虽然内容团队给予和接受玉米卷的比率低于其他团队的平均水平，但工程团队给予和接受玉米卷的比率高于平均水平。

## Python if else:后续步骤

在本教程中，我们学习了:

*   Python `if` `else`让我们可以根据条件控制代码流。
*   如何使用`if`只在符合某个条件的情况下执行代码？
*   如何在代码*与*不匹配的情况下使用`else`来执行代码。

您可能希望扩展本教程，并通过计算数据集中每个团队的平均值来练习使用 Python `if` `else`。

如果你想在互动教程中学习 Python `if` `else`，你可以在我们免费的互动 [Python 基础](https://www.dataquest.io/course/python-for-data-science-fundamentals/)课程中学习如何分析应用数据。