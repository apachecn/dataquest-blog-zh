# Python 字典理解教程(含 39 个代码示例)

> 原文：<https://www.dataquest.io/blog/python-dictionary-comprehension-tutorial/>

August 3, 2022![Dictionary comprehension](img/b7acef470d7fdb42fcd2567d9744fd63.png)

你可能已经掌握了 Python 中的[字典，也学到了很多关于](https://www.dataquest.io/blog/python-dictionaries/)[列表理解](https://app.dataquest.io/c/64/m/355/list-comprehensions-and-lambda-functions/1/the-json-format)的知识。现在是时候结合这些技能，学习新的东西了:Python 中的字典理解。

## Python 中的字典理解是什么？

不过，在此之前，让我们提醒自己什么是 Python 中的理解。它仅仅意味着对迭代器的每个元素(比如列表、字典或元组)应用特定种类的操作。当然，我们可以通过引入例如[条件语句](https://www.dataquest.io/blog/tutorial-using-if-statements-in-python/)来使这个逻辑更加复杂。

让我们直截了当地做一个简单的例子！我们将创建一个字典，包含一些人的年龄。

```py
d = {"Alex": 29, "Katherine": 24, "Jonathan": 22}
print(d)
```

```py
 {'Alex': 29, 'Katherine': 24, 'Jonathan': 22}
```

如果我们想给每个年龄加一年，然后把结果保存在一个新的字典里呢？我们可以用一个`for`循环来完成。

```py
# Dictionary to store new ages
new_d = {}

# Add one year to each age in the dictionary
for name, age in d.items(): # .items() method displays dictionary keys and values as a list of tuple pairs (name and age in this case)
    new_d[name] = age + 1
print(new_d)
```

```py
 {'Alex': 30, 'Katherine': 25, 'Jonathan': 23}
```

然而，`for`循环可以使用字典理解用一行代码重写。

```py
# Add one year to each age in the dictionary using comprehension
new_d = {name: age + 1 for name, age in d.items()}

print(new_d)
```

```py
 {'Alex': 30, 'Katherine': 25, 'Jonathan': 23}
```

好看吗？如果你习惯于列出理解，你可能也会注意到代码非常相似。让我们把它分成几块。

我们用名字作为键，用年龄作为值。我们的新词典应该和旧词典有同样的结构。在这里，名字不变，但年龄变了。我们通过打开花括号创建一个新的字典`new_d`，并在里面写上`name`，后面跟着一个冒号、`:`和`age`。现在停下来，想一想`name`和`age`是从哪里来的，我们应该如何修改`age`。它们来自原始字典，所以我们可以使用`items()`方法和一个`for`关键字循环遍历它，其中第一个变量是一个键，第二个是一个值。因此，我们将这个`for`循环写在括号内。现在，唯一剩下的事情就是通过加 1 来修改新字典的`age`值。

这是我平时用字典理解时的思维过程。这个例子非常简单，如果有足够的经验，您几乎可以自动完成，但是一旦代码变得稍微复杂一点，最好仔细想想您的最终结果应该是什么样子，以及您如何实现它。

此外，最好先编写一个`for`循环，确保它按照您设计的方式运行，然后使用字典理解重写它。

## 更现实的例子

好了，简单不切实际的例子已经够多了。在现实生活中，我们可能需要出于测试目的创建一些随机字典，或者仅仅因为我们需要某种统计技术的随机性，但是现在我们将分析来自 [Kaggle](https://www.kaggle.com/) 的真实世界数据集，并应用这种 Python 字典技术。请注意，原始数据集是通过从`user_review`列中删除`tbd`字符串来修改的。

我们将使用一个包含 1995-2021 年在 [Metacritic](https://www.metacritic.com/) 上热门视频游戏的[数据集](https://www.kaggle.com/datasets/deepcontractor/top-video-games-19952021-metacritic)。

```py
from csv import reader

# Open and read the dataset
all_games = open("all_games.csv")
all_games = reader(all_games)
all_games = list(all_games)
```

该数据集包含以下信息:

1.  名字
2.  平台
3.  出厂日期
4.  故事概要
5.  元分数
6.  用户分数

比方说，我们有兴趣创建一个以名称为键、以平台为值的字典。

```py
# Dictionary to store video game platforms
platform_dict = {}

# Create dictionary of platforms
for game in all_games[1:]:  # Do not include the header
    name = game[0]
    platform = game[1]
    platform_dict[name] = platform

# Print five items of the dictionary (need to transform it into a list before)
print(list(platform_dict.items())[:5])
```

```py
 [('The Legend of Zelda: Ocarina of Time', ' Nintendo 64'), ("Tony Hawk's Pro Skater 2", ' Nintendo 64'), ('Grand Theft Auto IV', ' PC'), ('SoulCalibur', ' Xbox 360'), ('Super Mario Galaxy', ' Wii')]
```

请注意，每个平台的名称前都有一个空格，所以让我们删除它。这可以通过`for`循环或字典理解来实现。

```py
# Using a for loop
for name, platform in platform_dict.items():
    platform_dict[name] = platform.strip()
```

以上是从值中删除空格的一种方法。我们使用了 [`str.strip`](https://docs.python.org/3/library/stdtypes.html#str.strip) 方法，如果没有给定参数，它会删除我们指定的前导和尾随字符或空格。现在，在阅读答案之前，尝试使用字典理解重写代码。

```py
platform_dict = {name: platform.strip() for name, platform in platform_dict.items()}
```

这是编写相同代码的更好的方式！

有时我们可能需要提取数据集的每一列，将它们转换成列表，然后将一个列表中的每个元素用作字典键，将另一个列表中的每个元素用作字典值。这个过程允许以一种简单的方式访问字典元素，过滤和操作它们。这可能吗？是的，但是让我们先把每一列转换成一个单独的列表。

```py
# Initialize empty lists to store column values
name = []
platform = []
date = []
summary = []
meta_score = []
user_score = []

# Iterate over columns and append values to lists
for game in all_games[1:]:
    name.append(game[0])
    platform.append(game[1].replace(" ", ""))
    date.append(game[2])
    summary.append(game[3])
    meta_score.append(float(game[4]))
    user_score.append(float(game[5]))
```

现在我们有了所有不同的列表，我们可以用其中的两个创建一个字典。我们已经有了一个平台字典，但是现在让我们提取发布**年**(不是日期！).我们将使用 [`zip()`](https://docs.python.org/3/library/functions.html#zip) 函数，该函数允许同时迭代多个可迭代对象。

```py
# Dictionary to store dates
year_dict = {}

# Populate dictionary with game's names and release years
for key, value in zip(name, date):
    year_dict[key] = value[-4:]

# Print release year of Grand Theft Auto IV
print(f"Grand Theft Auto IV was released in {year_dict['Grand Theft Auto IV']}.")
```

```py
 Grand Theft Auto IV was released in 2008.
```

现在试着做同样的事情，但是在看答案之前用字典理解。

```py
# Dictionary with game's names and release years using dictionary comprehension
year_dict = {key: value[-4:] for key, value in zip(name, date)}

# Print release year of Red Dead Redemption 2
print(f"Red Dead Redemption 2 was released in {year_dict['Red Dead Redemption 2']}.")
```

```py
 Red Dead Redemption 2 was released in 2019.
```

正如我们所看到的，Python 中的字典理解支持更短、更简单的代码。注意，我们使用[字符串切片](https://www.dataquest.io/blog/python-strings/)(即`[-4:]`)从日期中提取年份。

## 词典理解中的条件语句

现在你应该能够理解 Python 中字典理解的基本逻辑了。因此，让我们把事情复杂化一点。

我们可以在创建字典之前使用[条件语句](https://www.dataquest.io/blog/tutorial-using-if-statements-in-python/)过滤一些信息！让我们重新创建 2014 年后发布的电子游戏词典。

```py
# Video games released after 2014
after_2014 = {key: value[-4:] for key, value in zip(name, date) if int(value[-4:]) > 2014}

# Print the first five items of the dictionary
print(list(after_2014.items())[:5])
```

```py
 [('Red Dead Redemption 2', '2019'), ('Disco Elysium: The Final Cut', '2021'), ('The Legend of Zelda: Breath of the Wild', '2017'), ('Super Mario Odyssey', '2017'), ('The House in Fata Morgana - Dreams of the Revenants Edition -', '2021')]
```

在上面的代码片段中，我们重用了相同的代码，但是添加了一个`if`条件语句来应用过滤器。我们希望按值进行过滤，因此我们提取了年份，将数字转换为整数，然后将其与整数 2014 进行比较。如果年份在 2014 年之前，则字典中不包含该字典元素。

当然，我们可以通过引入一个逻辑运算符(`and`、`or`、`not`)来使逻辑变得更加复杂。例如，如果我们想要 2012 年到 2018 年(含)之间发布的所有游戏，那么代码如下。

```py
# Video games released between 2014 and 2018 (both inclusive)
y_2012_2018 = {
    key: value[-4:]
    for key, value in zip(name, date)
    if int(value[-4:]) >= 2014 and int(value[-4:]) <= 2018
}

# Print the first five items of the dictionary
print(list(y_2012_2018.items())[:5])
```

```py
 [('Red Dead Redemption 2', '2018'), ('Grand Theft Auto V', '2015'), ('The Legend of Zelda: Breath of the Wild', '2017'), ('Super Mario Odyssey', '2017'), ('The Last of Us Remastered', '2014')]
```

此时，代码变得相当复杂难以理解，所以如果我们需要额外的逻辑操作符，最好求助于标准的`for`循环。

让我们再举一个例子，通过低于 25 或高于 97 的元分数进行过滤。

```py
# Games with meta score below 25 or above 97
meta_25_or_97 = {
    key: value
    for key, value in zip(name, meta_score)
    if float(value) < 25 or float(value) > 97
}
print(meta_25_or_97)
```

```py
 {'The Legend of Zelda: Ocarina of Time': 99.0, "Tony Hawk's Pro Skater 2": 98.0, 'Grand Theft Auto IV': 98.0, 'SoulCalibur': 98.0, 'NBA Unrivaled': 24.0, 'Terrawars: New York Invasion': 24.0, 'Gravity Games Bike: Street Vert Dirt': 24.0, 'Postal III': 24.0, 'Game Party Champions': 24.0, 'Legends of Wrestling II': 24.0, 'Pulse Racer': 24.0, 'Fighter Within': 23.0, 'FlatOut 3: Chaos & Destruction': 23.0, 'Homie Rollerz': 23.0, "Charlie's Angels": 23.0, 'Rambo: The Video Game': 23.0, 'Fast & Furious: Showdown': 22.0, 'Drake of the 99 Dragons': 22.0, 'Afro Samurai 2: Revenge of Kuma Volume One': 21.0, 'Infestation: Survivor Stories (The War Z)': 20.0, 'Leisure Suit Larry: Box Office Bust': 20.0}
```

为了练习，用用户分数在 6 到 8(包括 6 和 8)之间的游戏创建一个字典。另外，确保字典中的值是一个`float`数字。

## 用例

接下来的四个部分将演示字典理解的不同用例:

1.  如何在嵌套字典中使用理解
2.  如何对字典进行分类
3.  如何展平字典
4.  如何计算字符串中的词频

### 嵌套词典理解

有时，我们需要使用嵌套字典，也就是说，当一个字典在另一个字典中时。让我们首先创建一个嵌套字典。

```py
# Create a nested dictionary
nested_d = {}
for n, p, dt in zip(name, platform, date):
    nested_d[n] = {"platform": p, "date": dt}

# Print the first five items of the nested dictionary
print(list(nested_d.items())[:5])
```

```py
 [('The Legend of Zelda: Ocarina of Time', {'platform': 'Nintendo64', 'date': 'November 23, 1998'}), ("Tony Hawk's Pro Skater 2", {'platform': 'Nintendo64', 'date': 'August 21, 2001'}), ('Grand Theft Auto IV', {'platform': 'PC', 'date': 'December 2, 2008'}), ('SoulCalibur', {'platform': 'Xbox360', 'date': 'July 2, 2008'}), ('Super Mario Galaxy', {'platform': 'Wii', 'date': 'November 12, 2007'})]
```

有一种方法可以使用字典理解重写上面的`for`循环。为了练习而做。

为了不使事情复杂化，我们将只使用平台的内在价值。通常，这是数据通过 [API](https://www.dataquest.io/blog/python-api-tutorial/) 传递给我们的格式。

```py
# Create another nested dictionary
nested_d = {}
for n, dt in zip(name, date):
    nested_d[n] = {"date": dt}

# Print the first five items of the dictionary
print(list(nested_d.items())[:5])
```

```py
 [('The Legend of Zelda: Ocarina of Time', {'date': 'November 23, 1998'}), ("Tony Hawk's Pro Skater 2", {'date': 'August 21, 2001'}), ('Grand Theft Auto IV', {'date': 'December 2, 2008'}), ('SoulCalibur', {'date': 'July 2, 2008'}), ('Super Mario Galaxy', {'date': 'November 12, 2007'})]
```

假设我们想要提取年份并将其转换为整数，同时保持相同的嵌套字典格式。一种方法是使用一个`for`循环。

```py
# Extract release years and transform them into integers inside the nested dictionary
for (title, outer_value) in nested_d.items():
    for (inner_key, date) in outer_value.items():
        try:
            outer_value.update({inner_key: int(date[-4:])})
        except:
            print(date)
nested_d.update({title: outer_value})

# Print the first five items of the dictionary
print(list(nested_d.items())[:5])
```

```py
 [('The Legend of Zelda: Ocarina of Time', {'date': 1998}), ("Tony Hawk's Pro Skater 2", {'date': 2001}), ('Grand Theft Auto IV', {'date': 2008}), ('SoulCalibur', {'date': 2008}), ('Super Mario Galaxy', {'date': 2007})]
```

当然，使用字典理解重写上面的`for`循环是可能的。为了避免错误，下面的代码单元将在运行字典理解之前重新创建列表和嵌套字典。

```py
# Initialize empty lists to store column values
name = []
platform = []
date = []
summary = []
meta_score = []
user_score = []

# Iterate over columns and append values to lists
for game in all_games[1:]:
    name.append(game[0])
    platform.append(game[1].replace(" ", ""))
    date.append(game[2])
    summary.append(game[3])
    meta_score.append(float(game[4]))
    user_score.append(float(game[5]))

# Create a nested dictionary
nested_d = {}
for n, dt in zip(name, date):
    nested_d[n] = {"date": dt}
```

```py
# The previous for loop using nested dictionary comprehension
dict_comprehension = {
    title: {inner_key: int(date[-4:]) for (inner_key, date) in outer_value.items()}
    for (title, outer_value) in nested_d.items()
}

# Print the first five items of the dictionary
print(list(dict_comprehension.items())[:5])
```

```py
 [('The Legend of Zelda: Ocarina of Time', {'date': 1998}), ("Tony Hawk's Pro Skater 2", {'date': 2001}), ('Grand Theft Auto IV', {'date': 2008}), ('SoulCalibur', {'date': 2008}), ('Super Mario Galaxy', {'date': 2007})]
```

这里的外`for`回路是第二`for`回路，而内`for`回路是第一`for`回路。此时代码变得相当复杂，所以在许多情况下，继续尝试使用字典理解是不值得的。记住 Python 的[主要原则之一是**可读性**。](https://peps.python.org/pep-0020/#the-zen-of-python)

### 根据理解对词典进行分类

Python 中字典理解的另一个用途是字典排序。比如说我们要把字典按游戏标题和年份从第一年到最近一年排序？我们先把 years 从`str`数据类型转换成`int`数据类型(使用 comprehension！).

```py
# Convert years from strings into intergers
year_dict_int = {key: int(value) for key, value in year_dict.items()}

# Sort by year
sorted_year = {key: value for key, value in sorted(year_dict_int.items(), key=lambda x: x[1])}

# Print the first five items of the dictionary sorted by year
print(list(sorted_year.items())[:5])
```

```py
 [('Full Throttle', 1995), ("Sid Meier's Civilization II", 1996), ('Diablo', 1996), ('Super Mario 64', 1996), ('Wipeout XL', 1996)]
```

我们使用了接受`key`参数的`sorted`函数，我们需要告诉函数我们想要对哪个元素进行排序。在这种情况下，我们有两个选择:按字典键或值排序。值排在第二位，所以索引应该是 1，而键应该是 0。密钥通常分配给 [`lambda`函数](https://www.dataquest.io/m/49-lambda-functions/)，这些函数是 Python 中常用的匿名函数。你认为有可能应用一个`for`循环来达到同样的结果吗？

### 字典的扁平化列表

有时我们面对一系列字典，我们想要一本字典。也可以用字典理解来完成。

```py
# Generate list of dictionaries
list_d = [
    {"Full Throttle": 1995},
    {"Sid Meier's Civilization II": 1996},
    {"Diablo": 1996},
]

# Flatten list of dictionary
list_d_flat = {
    title: year for dictionary in list_d for title, year in dictionary.items()
}

# Print flattened dictionary
print(list_d_flat)
```

```py
 {'Full Throttle': 1995, "Sid Meier's Civilization II": 1996, 'Diablo': 1996}
```

当然，我们也可以在上面的代码中添加一些条件语句，就像我们对前面的字典理解所做的那样。例如，在下面，我们过滤掉所有不是 1996 年的年份。

```py
# Flatten list of dictionary
list_d_flat_1996 = {
    title: year
    for dictionary in list_d
    for title, year in dictionary.items()
    if year == 1996
}

# Print flattened dictionary with only the games released in 1996
print(list_d_flat_1996)
```

```py
 {"Sid Meier's Civilization II": 1996, 'Diablo': 1996}
```

你能用一个`for`循环重现上面的字典理解吗？相信我，这是理解词典理解背后逻辑的最好方法之一。

### 字频率

自然语言处理中的一个步骤是对文本中出现的单词进行计数。表示这些数据的一种自然方式是使用字典，其中的键是单词，值是这个单词在文本中出现的次数。也是字典理解的工作！

```py
# Quote by Henry Van Dyke
quote = """time is too slow for those who wait too swift for those who fear too long for 
those who grieve too short for those who rejoicebut for those who love, time is eternity"""

# Count frequency of words in the quote
frequency_dict = {word: quote.split(" ").count(word) for word in quote.split(" ")}

print(frequency_dict)
```

```py
 {'time': 2, 'is': 2, 'too': 4, 'slow': 1, 'for': 5, 'those': 4, 'who': 5, 'wait': 1, 'swift': 1, 'fear': 1, 'long': 1, '\nthose': 1, 'grieve': 1, 'short': 1, 'rejoicebut': 1, 'love,': 1, 'eternity': 1}
```

如果我们使用一个`for`循环(但是试着重新创建它),这将花费我们三行代码。

## 包扎

我们可以利用 Python 中的字典理解来改进代码的方法太多了，以至于要列出所有的方法需要太多的纸张。我建议你只要一看到可能性就马上开始使用它们(但是要记住可读性),并且阅读其他人的代码来获得关于代码用例的想法。

在某种程度上，直接写字典理解会变得很自然，但要达到这一点，你必须写代码，做[项目](https://www.dataquest.io/data-science-projects/)。阅读教程和做代码练习有助于理解这个概念，但项目是你的数据科学或编码职业中真正的游戏规则改变者。

在本教程中，我们学习了:

*   Python 中的字典理解是什么
*   如何使用这种技术创建词典
*   如何在词典理解中使用条件句
*   什么是嵌套式词典理解
*   如何对字典进行分类
*   如何展平字典
*   以及如何计算字符串中的单词出现次数

我希望你今天学到了一些新东西。请随时在 LinkedIn 或 T2 GitHub 上与我联系。编码快乐！