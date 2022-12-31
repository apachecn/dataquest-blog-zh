# 如何在 R 语言中向数据帧添加一列(包含 18 个代码示例)

> 原文：<https://www.dataquest.io/blog/add-column-to-dataframe-in-r/>

June 14, 2022![Add column to DataFrame](img/764cafaed84b3d56df422e38a476966f.png)

## 在本教程中，您将学习在 R 中操作数据帧最常用的操作之一——添加列。

数据帧是 R 编程语言的基本数据结构之一。它也是一种非常通用的数据结构，因为它可以存储多种数据类型，易于修改和更新。

在本教程中，我们将考虑在 R 中操纵数据帧最常用的操作之一:如何在 base R 中向数据帧添加列。

### R 中的数据帧是什么？

从技术上讲，R 中的 DataFrame 是一个特定的例子，它是一列相同长度的向量，不同的向量可以是(通常是)不同的数据类型。由于 DataFrame 具有表格形式的二维形式，因此它具有列(变量)和行(数据条目)。

### 在 R 中向数据帧添加列

出于各种原因，我们可能想要向 R 数据帧添加新列:基于现有变量计算新变量，基于可用变量添加新列但具有不同格式(以这种方式保留两列)，追加空列或占位符列以进一步填充它，添加包含全新信息的列。

让我们探索向 r 中的数据帧添加新列的不同方法。在我们的实验中，我们将主要使用名为`super_sleepers`的相同数据帧，我们每次都将从以下初始数据帧中重建该数据帧:

```py
super_sleepers_initial <- data.frame(rating=1:4, 
                                     animal=c('koala', 'hedgehog', 'sloth', 'panda'), 
                                     country=c('Australia', 'Italy', 'Peru', 'China'))
print(super_sleepers_initial)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China
```

我们的任务是将一个名为`avg_sleep_hours`的新列添加到该数据帧中，该列根据以下方案表示上述每种动物每天睡眠的平均时间(小时):

| 动物 | 每天平均睡眠时间 |
| --- | --- |
| 树袋熊 | Twenty-one |
| 刺猬 | Eighteen |
| 懒惰 | Seventeen |
| 熊猫 | Ten |

对于一些例子，我们将尝试添加另外两列:`avg_sleep_hours_per_year`和`has_tail`。

现在，让我们开始吧。

#### 使用\$符号向 R 中的数据帧添加列

因为 R 中的数据帧是一个向量列表，其中每个向量代表该数据帧的一个单独的列，所以我们可以通过将相应的新向量添加到该“列表”中来将一列添加到数据帧中。语法如下:

```py
dataframe_name$new_column_name <- vector
```

让我们从初始的`super_sleepers_initial`数据帧重建我们的`super_sleepers`数据帧(我们将对每个后续实验都这样做),并向其添加一个名为`avg_sleep_hours`的列，由向量`c(21, 18, 17, 10)`表示:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')  # printing an empty line

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
super_sleepers$avg_sleep_hours <- c(21, 18, 17, 10)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10
```

请注意，添加到 vector 中的项数必须等于 DataFrame 中的当前行数，否则，程序会抛出一个错误:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Attempting to add a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
# with the number of items in the vector NOT EQUAL to the number of rows in the DataFrame
super_sleepers$avg_sleep_hours <- c(21, 18, 17)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

Error in `$<-.data.frame`(`*tmp*`, avg_sleep_hours, value = c(21, 18, : replacement has 3 rows, data has 4
Traceback:

1\. `$<-`(`*tmp*`, avg_sleep_hours, value = c(21, 18, 17))

2\. `$<-.data.frame`(`*tmp*`, avg_sleep_hours, value = c(21, 18, 
 . 17))

3\. stop(sprintf(ngettext(N, "replacement has %d row, data has %d", 
 .     "replacement has %d rows, data has %d"), N, nrows), domain = NA)
```

我们可以为新列的所有行分配一个值，无论是数字还是字符，而不是分配一个向量:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame and assigning it to 0
super_sleepers$avg_sleep_hours <- 0
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia               0
2      2 hedgehog     Italy               0
3      3    sloth      Peru               0
4      4    panda     China               0
```

在这种情况下，新列充当指定数据类型(在上面的例子中是数字)的实际值的占位符，我们可以在以后插入这些值。

或者，我们可以基于现有的列计算一个新列。让我们首先将`avg_sleep_hours`列添加到我们的 DataFrame 中，然后从中计算一个新的列`avg_sleep_hours_per_year`。我们想知道这些动物每年平均睡眠多少小时:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
super_sleepers$avg_sleep_hours <- c(21, 18, 17, 10)
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours_per_year` calculated from `avg_sleep_hours`
super_sleepers$avg_sleep_hours_per_year <- super_sleepers$avg_sleep_hours * 365
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10

  rating   animal   country avg_sleep_hours avg_sleep_hours_per_year
1      1    koala Australia              21                     7665
2      2 hedgehog     Italy              18                     6570
3      3    sloth      Peru              17                     6205
4      4    panda     China              10                     3650
```

同样，可以使用下面的语法将一列从一个数据帧复制到另一个数据帧:`df1$new_col <- df2$existing_col`。让我们复制这样一种情况:

```py
# Creating the `super_sleepers_1` dataframe with the only column `rating`
super_sleepers_1 <- data.frame(rating=1:4)
print(super_sleepers_1)
cat('\n\n')

# Copying the `animal` column from `super_sleepers_initial` to `super_sleepers_1`
# Note that in the new DataFrame, the column is called `ANIMAL` instead of `animal` 
super_sleepers_1$ANIMAL <- super_sleepers_initial$animal
print(super_sleepers_1)
```

```py
 rating
1      1
2      2
3      3
4      4

  rating   ANIMAL
1      1    koala
2      2 hedgehog
3      3    sloth
4      4    panda
```

这种方法(即使用\$操作符将列追加到数据帧)的缺点是，我们不能以这种方式添加名称包含空格或特殊符号的列。事实上，它不能包含任何不是字母(大写或小写)、数字、点或下划线的内容。此外，这种方法不适用于添加多个列。

#### 使用方括号将列添加到 R 中的数据帧

向 R 数据帧添加新列的另一种方式是更“数据帧风格”而不是“列表风格”:通过使用括号符号。让我们看看它是如何工作的:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame:
super_sleepers['avg_sleep_hours'] <- c(21, 18, 17, 10)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10
```

在上面这段代码中，我们可以用下面这行代码代替:

```py
super_sleepers['avg_sleep_hours'] <- c(21, 18, 17, 10)
```

这一行也可以替换为:

```py
super_sleepers[['avg_sleep_hours']] <- c(21, 18, 17, 10)
```

最后，这个也可以替换:

```py
super_sleepers[,'avg_sleep_hours'] <- c(21, 18, 17, 10)
```

结果是一样的，那只是语法的 3 个不同版本。

与前一种方法一样，我们可以为新列分配一个值，而不是一个向量:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame and assigning it to 'Unknown'
super_sleepers['avg_sleep_hours'] <- 'Unknown'
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia         Unknown
2      2 hedgehog     Italy         Unknown
3      3    sloth      Peru         Unknown
4      4    panda     China         Unknown
```

或者，我们可以基于现有的列计算一个新列:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
super_sleepers['avg_sleep_hours'] <- c(21, 18, 17, 10)
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours_per_year` calculated from `avg_sleep_hours`
super_sleepers['avg_sleep_hours_per_year'] <- super_sleepers['avg_sleep_hours'] * 365
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10

  rating   animal   country avg_sleep_hours avg_sleep_hours_per_year
1      1    koala Australia              21                     7665
2      2 hedgehog     Italy              18                     6570
3      3    sloth      Peru              17                     6205
4      4    panda     China              10                     3650
```

使用另一个选项，我们可以从另一个数据帧中复制一列:

```py
# Creating the `super_sleepers_1` dataframe with the only column `rating`
super_sleepers_1 <- data.frame(rating=1:4)
print(super_sleepers_1)
cat('\n\n')

# Copying the `animal` column from `super_sleepers_initial` to `super_sleepers_1`
# Note that in the new DataFrame, the column is called `ANIMAL` instead of `animal` 
super_sleepers_1['ANIMAL'] <- super_sleepers_initial['animal']
print(super_sleepers_1)
```

```py
 rating
1      1
2      2
3      3
4      4

  rating   ANIMAL
1      1    koala
2      2 hedgehog
3      3    sloth
4      4    panda
```

使用方括号而不是$运算符将列追加到 DataFrame 的优点是，我们可以添加名称包含空格或任何特殊符号的列。

#### 使用`cbind()`函数向 R 中的数据帧添加一列

向 R 数据帧添加新列的第三种方式是应用代表“列绑定”的`cbind()`函数，该函数也可用于组合两个或更多数据帧。使用这个函数比前两个方法更通用，因为它允许一次添加几列。其基本语法如下:

```py
df <- cbind(df, new_col_1, new_col_2, ..., new_col_N)
```

下面这段代码将`avg_sleep_hours`列添加到`super_sleepers`数据帧中:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours=c(21, 18, 17, 10))
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10
```

下一段代码一次向`super_sleepers`数据帧添加两个新列——`avg_sleep_hours`和`has_tail`:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding two new columns `avg_sleep_hours` and `has_tail` to the `super_sleepers` DataFrame
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours=c(21, 18, 17, 10), 
                        has_tail=c('no', 'yes', 'yes', 'yes'))
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours has_tail
1      1    koala Australia              21       no
2      2 hedgehog     Italy              18      yes
3      3    sloth      Peru              17      yes
4      4    panda     China              10      yes
```

除了一次添加多列之外，使用`cbind()`函数的另一个优点是，它允许将该操作的结果(即向 R 数据帧添加一列或多列)分配给新的数据帧，而不改变初始数据帧:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Creating a new DataFrame `super_sleepers_new` based on `super_sleepers` with a new column `avg_sleep_hours`
super_sleepers_new <- cbind(super_sleepers,
                            avg_sleep_hours=c(21, 18, 17, 10),
                            has_tail=c('no', 'yes', 'yes', 'yes'))
print(super_sleepers_new)
cat('\n\n')
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours has_tail
1      1    koala Australia              21       no
2      2 hedgehog     Italy              18      yes
3      3    sloth      Peru              17      yes
4      4    panda     China              10      yes

  rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China
```

与前两种方法一样，在`cbind()`函数中，我们可以为整个新列分配一个值:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame and assigning it to 0.999
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours=0.999)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia           0.999
2      2 hedgehog     Italy           0.999
3      3    sloth      Peru           0.999
4      4    panda     China           0.999
```

另一个选项允许我们基于现有的列来计算它:

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours` to the `super_sleepers` DataFrame
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours=c(21, 18, 17, 10))
print(super_sleepers)
cat('\n\n')

# Adding a new column `avg_sleep_hours_per_year` calculated from `avg_sleep_hours`
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours_per_year=super_sleepers['avg_sleep_hours'] * 365)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

  rating   animal   country avg_sleep_hours
1      1    koala Australia              21
2      2 hedgehog     Italy              18
3      3    sloth      Peru              17
4      4    panda     China              10

  rating   animal   country avg_sleep_hours avg_sleep_hours
1      1    koala Australia              21            7665
2      2 hedgehog     Italy              18            6570
3      3    sloth      Peru              17            6205
4      4    panda     China              10            3650
```

使用以下选项，我们可以从另一个数据帧中复制一列:

```py
# Creating the `super_sleepers_1` DataFrame with the only column `rating`
super_sleepers_1 <- data.frame(rating=1:4)
print(super_sleepers_1)
cat('\n\n')

# Copying the `animal` column from `super_sleepers_initial` to `super_sleepers_1`
# Note that in the new DataFrame, the column is still called `animal` despite setting the new name `ANIMAL` 
super_sleepers_1 <- cbind(super_sleepers_1, 
                          ANIMAL=super_sleepers_initial['animal'])
print(super_sleepers_1)
```

```py
 rating
1      1
2      2
3      3
4      4

  rating   animal
1      1    koala
2      2 hedgehog
3      3    sloth
4      4    panda
```

**然而**，与\$运算符和方括号方法不同，请注意以下两个细微差别:

1.  我们不能创建一个新列，然后在同一个`cbind()`函数中基于新列**再计算一列。例如，下面这段代码会抛出一个错误。**

```py
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')

# Attempting to add a new column `avg_sleep_hours` to the `super_sleepers` DataFrame 
# AND another new column `avg_sleep_hours_per_year` based on it
super_sleepers <- cbind(super_sleepers, 
                        avg_sleep_hours=c(21, 18, 17, 10), 
                        avg_sleep_hours_per_year=super_sleepers['avg_sleep_hours'] * 365)
print(super_sleepers)
```

```py
 rating   animal   country
1      1    koala Australia
2      2 hedgehog     Italy
3      3    sloth      Peru
4      4    panda     China

Error in `[.data.frame`(super_sleepers, "avg_sleep_hours"): undefined columns selected
Traceback:

1\. cbind(super_sleepers, avg_sleep_hours = c(21, 18, 17, 10), avg_sleep_hours_per_year = super_sleepers["avg_sleep_hours"] * 
 .     365)

2\. super_sleepers["avg_sleep_hours"]

3\. `[.data.frame`(super_sleepers, "avg_sleep_hours")

4\. stop("undefined columns selected")
```

2.  当我们从另一个数据帧中复制一个列，并试图在`cbind()`函数中给它一个新名称**时，这个新名称将被忽略，新列的调用将与它在原始数据帧中的调用完全相同。例如，在下面这段代码中，新名称`ANIMAL`被忽略，新列被命名为`animal`，就像在复制它的数据帧中一样:**

```py
# Creating the `super_sleepers_1` DataFrame with the only column `rating`
super_sleepers_1 <- data.frame(rating=1:4)
print(super_sleepers_1)
cat('\n\n')

# Copying the `animal` column from `super_sleepers_initial` to `super_sleepers_1`
# Note that in the new DataFrame, the column is still called `animal` despite setting the new name `ANIMAL` 
super_sleepers_1 <- cbind(super_sleepers_1, 
                          ANIMAL=super_sleepers_initial['animal'])
print(super_sleepers_1)
```

```py
 rating
1      1
2      2
3      3
4      4

  rating   animal
1      1    koala
2      2 hedgehog
3      3    sloth
4      4    panda
```

### 结论

在本教程中，我们讨论了为什么需要向 R 数据帧添加新列的各种原因，以及它可以存储什么样的信息。然后，我们探索了三种不同的方法:使用\$符号、方括号和`cbind()`函数。我们考虑了每种方法的语法及其可能的变化，每种方法的优缺点，可能的附加功能，最常见的陷阱和错误，以及如何避免它们。此外，我们还学习了如何一次向 R 数据帧添加多个列。

值得注意的是，所讨论的方法并不是在 r 中向 DataFrame 添加列的唯一方法。例如，出于同样的目的，我们可以使用 [`mutate()`](https://www.rdocumentation.org/packages/dplyr/versions/0.5.0/topics/mutate) 或 [`add_column()`](https://www.rdocumentation.org/packages/tibble/versions/1.4.2/topics/add_column) 函数。然而，为了能够应用这些函数，我们需要安装和加载特定的 R 包( *dplyr* 和 *tibble* )，除了我们在本教程中讨论的功能之外，它们不会给操作增加任何额外的功能。相反，使用\$符号、方括号和`cbind()`函数不需要在基础 r 中实现任何安装