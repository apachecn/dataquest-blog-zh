# 教程:过滤熊猫数据帧

> 原文：<https://www.dataquest.io/blog/filtering-pandas-dataframes/>

February 24, 2022![](img/ce638696f4e709b547b05556999061f5.png)

Pandas 库是一个快速、强大且易于使用的数据处理工具。它通过提供改变游戏规则的功能，帮助我们清理、探索、分析和可视化数据。将数据作为 Pandas DataFrame 允许我们以各种方式分割数据，并毫不费力地过滤数据帧的行。本教程将介绍使用不同的过滤技术从数据帧中获取数据的主要方法。

## 什么是数据帧？

在讨论不同的数据帧过滤技术之前，让我们先回顾一下数据帧。从技术上讲，DataFrame 建立在 Pandas 系列对象的基础上，系列是索引数据的一维数组。直观上，数据框架在许多方面类似于电子表格；它可以有多个包含不同数据类型的列和标有行索引的行。

## 创建数据帧

首先，让我们使用下面的代码片段[创建一个包含公司员工个人详细信息的虚拟数据帧](https://www.dataquest.io/blog/tutorial-how-to-create-and-use-a-pandas-dataframe/):

```py
import pandas as pd
import numpy as np
from IPython.display import display

data = pd.DataFrame({'EmployeeName': ['Callen Dunkley', 'Sarah Rayner', 'Jeanette Sloan','Kaycee Acosta', 'Henri Conroy', 'Emma Peralta','Martin Butt', 'Alex Jensen', 'Kim Howarth', 'Jane Burnett'],
                    'Department': ['Accounting', 'Engineering', 'Engineering', 'HR', 'HR','HR','Data Science','Data Science', 'Accounting', 'Data Science'],
                    'HireDate': [2010, 2018, 2012, 2014, 2014, 2018, 2020, 2018, 2020, 2012],
                    'Gender': ['M','F','F','F','M','F','M','M','M','F'],
                    'DoB':['04/09/1982', '14/04/1981','06/05/1997','08/01/1986','10/10/1988','12/11/1992', '10/04/1991','16/07/1995','08/10/1992','11/10/1979'],
                    'Weight':[78,80,66,67,90,57,115,87,95,57],
                    'Height':[176,160,169,157,185,164,195,180,174,165]
                  })
display(data)
```

|  | 员工姓名 | 部门 | 你在说什么 | 性别 | 告发 | 重量 | 高度 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | 卡伦·邓克利 | 会计 | Two thousand and ten | M | 04/09/1982 | seventy-eight | One hundred and seventy-six |
| one | 莎拉·雷纳 | 工程 | Two thousand and eighteen | F | 14/04/1981 | Eighty | One hundred and sixty |
| Two | 珍妮特·斯隆 | 工程 | Two thousand and twelve | F | 06/05/1997 | Sixty-six | One hundred and sixty-nine |
| three | 凯茜·阿科斯塔 | 人力资源（部） | Two thousand and fourteen | F | 08/01/1986 | Sixty-seven | One hundred and fifty-seven |
| four | 亨利康罗伊 | 人力资源（部） | Two thousand and fourteen | M | 10/10/1988 | Ninety | One hundred and eighty-five |
| five | 艾玛·佩拉尔塔 | 人力资源（部） | Two thousand and eighteen | F | 12/11/1992 | Fifty-seven | One hundred and sixty-four |
| six | 马丁·巴特 | 数据科学 | Two thousand and twenty | M | 10/04/1991 | One hundred and fifteen | One hundred and ninety-five |
| seven | 艾利克斯詹森 | 数据科学 | Two thousand and eighteen | M | 16/07/1995 | Eighty-seven | one hundred and eighty  |
| eight | 金·豪沃思 | 会计 | Two thousand and twenty | M | 08/10/1992 | Ninety-five | One hundred and seventy-four |
| nine | 简·伯内特 | 数据科学 | Two thousand and twelve | F | 11/10/1979 | Fifty-seven | One hundred and sixty-five |

运行上面的代码。它用上述数据创建一个数据帧。
让我们通过运行以下语句来看看列的数据类型:

```py
print(data.dtypes)
```

```py
EmployeeName    object
Department      object
HireDate         int64
Gender          object
DoB             object
Weight           int64
Height           int64
dtype: object
```

Pandas 字符串值的数据类型是**对象**，这就是为什么像 *EmployeeName* 或 *Department* 这样包含字符串的列的数据类型是**对象**。数据帧的每一列都有特定的数据类型；换句话说，数据帧的列不共享相同的数据类型。
另一个有用的 DataFrame 属性是`.values`，它返回一个列表数组，每个列表代表 DataFrame 的一行。

```py
print(data.values)
```

```py
[['Callen Dunkley' 'Accounting' 2010 'M' '04/09/1982' 78 176]
 ['Sarah Rayner' 'Engineering' 2018 'F' '14/04/1981' 80 160]
 ['Jeanette Sloan' 'Engineering' 2012 'F' '06/05/1997' 66 169]
 ['Kaycee Acosta' 'HR' 2014 'F' '08/01/1986' 67 157]
 ['Henri Conroy' 'HR' 2014 'M' '10/10/1988' 90 185]
 ['Emma Peralta' 'HR' 2018 'F' '12/11/1992' 57 164]
 ['Martin Butt' 'Data Science' 2020 'M' '10/04/1991' 115 195]
 ['Alex Jensen' 'Data Science' 2018 'M' '16/07/1995' 87 180]
 ['Kim Howarth' 'Accounting' 2020 'M' '08/10/1992' 95 174]
 ['Jane Burnett' 'Data Science' 2012 'F' '11/10/1979' 57 165]]
```

如果您想知道数据帧的行数和列数，您可以如下使用`.shape`属性:

```py
print(data.shape)
```

```py
(10, 7)
```

上面的元组分别表示行数和列数。

## 从每个可能的角度切割数据帧

到目前为止，我们已经创建并检查了一个数据帧。如前所述，过滤数据帧有不同的方法。在这一节中，我们将讨论最流行和最有效的方法。但是，最好先看看我们如何选择特定的列。
要获取特定列的全部内容，我们可以使用以下语法:

```py
DataFrame.ColumnName  # if the column name does not contain any spaces
OR 
DataFrame["ColumnName"]
```

例如，让我们抓住数据帧的部门列。

```py
display(data.Department)
```

```py
0      Accounting
1     Engineering
2     Engineering
3              HR
4              HR
5              HR
6    Data Science
7    Data Science
8      Accounting
9    Data Science
Name: Department, dtype: object
```

运行上面的语句将返回包含部门列数据的序列。

此外，我们可以通过传递列名列表来选择多个列，如下所示:

```py
display(data[['EmployeeName','Department','Gender']])
```

|  | 员工姓名 | 部门 | 性别 |
| --- | --- | --- | --- |
| Zero | 卡伦·邓克利 | 会计 | M |
| one | 莎拉·雷纳 | 工程 | F |
| Two | 珍妮特·斯隆 | 工程 | F |
| three | 凯茜·阿科斯塔 | 人力资源（部） | F |
| four | 亨利康罗伊 | 人力资源（部） | M |
| five | 艾玛·佩拉尔塔 | 人力资源（部） | F |
| six | 马丁·巴特 | 数据科学 | M |
| seven | 艾利克斯詹森 | 数据科学 | M |
| eight | 金·豪沃思 | 会计 | M |
| nine | 简·伯内特 | 数据科学 | F |

运行上面的语句给我们一个数据帧，它是原始数据帧的子集。

### `.loc`和`.iloc`方法

我们可以使用切片技术从数据帧中提取特定的行。分割数据帧并选择特定列的最佳方式是使用`.loc`和`.iloc`方法。这两种方法分别使用基于标签或基于整数的索引创建数据帧的子集。

```py
DataFrame.loc[row_indexer,column_indexer]
DataFrame.iloc[row_indexer,column_indexer]
```

两种方法中的第一个位置指定行索引器，第二个位置指定列索引器，用逗号分隔。
在我们想练习使用`.loc`和`.iloc`方法之前，让我们将 *EmployeeName* 列设置为 DataFrame 的索引。这样做不是必需的，但有助于我们讨论这些方法的所有功能。为此，使用`set_index()`方法使 *EmployeeName* 列成为 DataFrame 的索引:

```py
data = data.set_index('EmployeeName')
display(data)
```

|  | 部门 | 你在说什么 | 性别 | 告发 | 重量 | 高度 |
| --- | --- | --- | --- | --- | --- | --- |
| 员工姓名 |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 卡伦·邓克利 | 会计 | Two thousand and ten | M | 04/09/1982 | seventy-eight | One hundred and seventy-six |
| 莎拉·雷纳 | 工程 | Two thousand and eighteen | F | 14/04/1981 | Eighty | One hundred and sixty |
| 珍妮特·斯隆 | 工程 | Two thousand and twelve | F | 06/05/1997 | Sixty-six | One hundred and sixty-nine |
| 凯茜·阿科斯塔 | 人力资源（部） | Two thousand and fourteen | F | 08/01/1986 | Sixty-seven | One hundred and fifty-seven |
| 亨利康罗伊 | 人力资源（部） | Two thousand and fourteen | M | 10/10/1988 | Ninety | One hundred and eighty-five |
| 艾玛·佩拉尔塔 | 人力资源（部） | Two thousand and eighteen | F | 12/11/1992 | Fifty-seven | One hundred and sixty-four |
| 马丁·巴特 | 数据科学 | Two thousand and twenty | M | 10/04/1991 | One hundred and fifteen | One hundred and ninety-five |
| 艾利克斯詹森 | 数据科学 | Two thousand and eighteen | M | 16/07/1995 | Eighty-seven | one hundred and eighty  |
| 金·豪沃思 | 会计 | Two thousand and twenty | M | 08/10/1992 | Ninety-five | One hundred and seventy-four |
| 简·伯内特 | 数据科学 | Two thousand and twelve | F | 11/10/1979 | Fifty-seven | One hundred and sixty-five |

现在，我们有了基于标签的索引，而不是基于整数的索引，这使得尝试用于切片和切割数据帧的`.loc`和`.iloc`方法的所有功能更加容易。

* * *

**注**

`.loc`方法是基于标签的，所以您必须根据它们的标签来指定行和列。另一方面，`iloc`方法是基于整数索引的，所以您必须根据它们的整数索引来选择行和列。

* * *

以下语句实际上返回相同的结果:

```py
display(data.loc[['Henri Conroy', 'Kim Howarth']])
display(data.iloc[[4,8]])
```

|  | 部门 | 你在说什么 | 性别 | 告发 | 重量 | 高度 |
| --- | --- | --- | --- | --- | --- | --- |
| 员工姓名 |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 亨利康罗伊 | 人力资源（部） | Two thousand and fourteen | M | 10/10/1988 | Ninety | One hundred and eighty-five |
| 金·豪沃思 | 会计 | Two thousand and twenty | M | 08/10/1992 | Ninety-five | One hundred and seventy-four |

|  | 部门 | 你在说什么 | 性别 | 告发 | 重量 | 高度 |
| --- | --- | --- | --- | --- | --- | --- |
| 员工姓名 |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 亨利康罗伊 | 人力资源（部） | Two thousand and fourteen | M | 10/10/1988 | Ninety | One hundred and eighty-five |
| 金·豪沃思 | 会计 | Two thousand and twenty | M | 08/10/1992 | Ninety-five | One hundred and seventy-four |

在上面的代码中，`.loc`方法获得一个雇员姓名列表作为行标签，并返回一个包含与这两个雇员相关的行的 DataFrame。
`.iloc`方法做同样的事情，但是它不是获取带标签的索引，而是获取行的整数索引。
现在，让我们使用`.loc`和`.iloc`方法提取一个包含前三名雇员的姓名、部门和性别值的子数据帧:

```py
display(data.iloc[:3,[0,2]])
display(data.loc['Callen Dunkley':'Jeanette Sloan',['Department','Gender']])
```

|  | 部门 | 性别 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | 会计 | M |
| 莎拉·雷纳 | 工程 | F |
| 珍妮特·斯隆 | 工程 | F |

|  | 部门 | 性别 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | 会计 | M |
| 莎拉·雷纳 | 工程 | F |
| 珍妮特·斯隆 | 工程 | F |

运行上述语句会返回相同的结果，如下所示:

```py
EmployeeName     Department Gender                      
Callen Dunkley   Accounting      M
Sarah Rayner    Engineering      F
Jeanette Sloan  Engineering      F
```

`.loc`和`.iloc`方法非常相似；唯一的区别是如何引用数据帧中的列和行。
以下语句返回数据帧的所有行:

```py
display(data.iloc[:,[0,2]])
display(data.loc[:,['Department','Gender']])
```

|  | 部门 | 性别 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | 会计 | M |
| 莎拉·雷纳 | 工程 | F |
| 珍妮特·斯隆 | 工程 | F |
| 凯茜·阿科斯塔 | 人力资源（部） | F |
| 亨利康罗伊 | 人力资源（部） | M |
| 艾玛·佩拉尔塔 | 人力资源（部） | F |
| 马丁·巴特 | 数据科学 | M |
| 艾利克斯詹森 | 数据科学 | M |
| 金·豪沃思 | 会计 | M |
| 简·伯内特 | 数据科学 | F |

|  | 部门 | 性别 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | 会计 | M |
| 莎拉·雷纳 | 工程 | F |
| 珍妮特·斯隆 | 工程 | F |
| 凯茜·阿科斯塔 | 人力资源（部） | F |
| 亨利康罗伊 | 人力资源（部） | M |
| 艾玛·佩拉尔塔 | 人力资源（部） | F |
| 马丁·巴特 | 数据科学 | M |
| 艾利克斯詹森 | 数据科学 | M |
| 金·豪沃思 | 会计 | M |
| 简·伯内特 | 数据科学 | F |

我们也可以对列进行切片。下面的语句返回数据帧中每隔一行的`Weight`和`Height`列:

```py
display(data.loc[::2,'Weight':'Height'])
display(data.iloc[::2,4:6])
```

|  | 重量 | 高度 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | seventy-eight | One hundred and seventy-six |
| 珍妮特·斯隆 | Sixty-six | One hundred and sixty-nine |
| 亨利康罗伊 | Ninety | One hundred and eighty-five |
| 马丁·巴特 | One hundred and fifteen | One hundred and ninety-five |
| 金·豪沃思 | Ninety-five | One hundred and seventy-four |

|  | 重量 | 高度 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 卡伦·邓克利 | seventy-eight | One hundred and seventy-six |
| 珍妮特·斯隆 | Sixty-six | One hundred and sixty-nine |
| 亨利康罗伊 | Ninety | One hundred and eighty-five |
| 马丁·巴特 | One hundred and fifteen | One hundred and ninety-five |
| 金·豪沃思 | Ninety-five | One hundred and seventy-four |

我们在上面的代码中对数据帧的行和列执行切片。语法类似于 Python 中的列表切片。我们需要指定开始索引、停止索引和步长。

```py
EmployeeName    Weight  Height                  
Callen Dunkley      78     176
Jeanette Sloan      66     169
Henri Conroy        90     185
Martin Butt        115     195
Kim Howarth         95     174
```

注意，在使用`.loc`方法的带标签切片中，开始和停止标签都包括在内。然而，在使用`.iloc`方法的切片中，停止索引被排除。

### 在 Pandas 中过滤数据帧

到目前为止，我们已经学会了如何分割数据帧，但是我们如何获取符合特定标准的数据呢？在 Pandas 中，使用布尔掩码是过滤数据帧的常用方法。首先让我们看看什么是布尔掩码:

```py
print(data.HireDate > 2015)
```

```py
EmployeeName
Callen Dunkley    False
Sarah Rayner       True
Jeanette Sloan    False
Kaycee Acosta     False
Henri Conroy      False
Emma Peralta       True
Martin Butt        True
Alex Jensen        True
Kim Howarth        True
Jane Burnett      False
Name: HireDate, dtype: bool
```

通过运行上面的代码，我们可以看到`HireDate`列中的哪些条目大于 2015。布尔值表示是否满足条件。
现在，我们可以使用布尔掩码来获取包含 2015 年后雇佣的
员工的数据帧子集:

```py
display(data[data.HireDate > 2015])
```

|  | 部门 | 你在说什么 | 性别 | 告发 | 重量 | 高度 |
| --- | --- | --- | --- | --- | --- | --- |
| 员工姓名 |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 莎拉·雷纳 | 工程 | Two thousand and eighteen | F | 14/04/1981 | Eighty | One hundred and sixty |
| 艾玛·佩拉尔塔 | 人力资源（部） | Two thousand and eighteen | F | 12/11/1992 | Fifty-seven | One hundred and sixty-four |
| 马丁·巴特 | 数据科学 | Two thousand and twenty | M | 10/04/1991 | One hundred and fifteen | One hundred and ninety-five |
| 艾利克斯詹森 | 数据科学 | Two thousand and eighteen | M | 16/07/1995 | Eighty-seven | one hundred and eighty  |
| 金·豪沃思 | 会计 | Two thousand and twenty | M | 08/10/1992 | Ninety-five | One hundred and seventy-four |

我们还可以使用`.loc`方法过滤行并返回某些列，如下所示:

```py
display(data.loc[data.HireDate > 2015, ['Department', 'HireDate']])
```

|  | 部门 | 你在说什么 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 莎拉·雷纳 | 工程 | Two thousand and eighteen |
| 艾玛·佩拉尔塔 | 人力资源（部） | Two thousand and eighteen |
| 马丁·巴特 | 数据科学 | Two thousand and twenty |
| 艾利克斯詹森 | 数据科学 | Two thousand and eighteen |
| 金·豪沃思 | 会计 | Two thousand and twenty |

我们能够使用逻辑运算符将两个或多个标准结合起来。让我们列出那些在数据科学团队工作的男性员工:

```py
display(data.loc[(data.Gender == "M") & (data.Department=='Data Science'), 
                ['Department', 'Gender', 'HireDate']]
        )
```

|  | 部门 | 性别 | 你在说什么 |
| --- | --- | --- | --- |
| 员工姓名 |  |  |  |
| --- | --- | --- | --- |
| 马丁·巴特 | 数据科学 | M | Two thousand and twenty |
| 艾利克斯詹森 | 数据科学 | M | Two thousand and eighteen |

如果我们想让所有员工身高在 170 cm 到 190 cm 之间怎么办？

```py
display(data.loc[data.Height.between(170,190), 
                ['Department', 'Gender', 'Height']]
        )
```

|  | 部门 | 性别 | 高度 |
| --- | --- | --- | --- |
| 员工姓名 |  |  |  |
| --- | --- | --- | --- |
| 卡伦·邓克利 | 会计 | M | One hundred and seventy-six |
| 亨利康罗伊 | 人力资源（部） | M | One hundred and eighty-five |
| 艾利克斯詹森 | 数据科学 | M | one hundred and eighty  |
| 金·豪沃思 | 会计 | M | One hundred and seventy-four |

在`between()`方法中，范围的两端都包括在内。
另一个有用的方法是`isin()`，它为包含在指定值列表中的值创建一个布尔掩码。让我们尝试一下检索在人力资源或会计部门工作的雇员的方法:

```py
display(data.loc[data.Department.isin(['HR', 'Accounting']),['Department', 'Gender', 'HireDate']])
```

|  | 部门 | 性别 | 你在说什么 |
| --- | --- | --- | --- |
| 员工姓名 |  |  |  |
| --- | --- | --- | --- |
| 卡伦·邓克利 | 会计 | M | Two thousand and ten |
| 凯茜·阿科斯塔 | 人力资源（部） | F | Two thousand and fourteen |
| 亨利康罗伊 | 人力资源（部） | M | Two thousand and fourteen |
| 艾玛·佩拉尔塔 | 人力资源（部） | F | Two thousand and eighteen |
| 金·豪沃思 | 会计 | M | Two thousand and twenty |

为了过滤数据帧，我们可以使用正则表达式来搜索模式，而不是精确的内容。以下示例显示了如何使用正则表达式来获取那些姓氏以“tt”或“th”结尾的雇员:

```py
display(data.loc[data.index.str.contains(r'tt$|th$'),
                ['Department', 'Gender']])
```

|  | 部门 | 性别 |
| --- | --- | --- |
| 员工姓名 |  |  |
| --- | --- | --- |
| 马丁·巴特 | 数据科学 | M |
| 金·豪沃思 | 会计 | M |
| 简·伯内特 | 数据科学 | F |

## 结论

在本教程中，我们讨论了对数据帧进行切片和切块以及使用特定标准过滤行的不同方式。我们讨论了如何通过选择列、对行进行切片以及基于条件、方法或正则表达式进行过滤来获取数据子集。