# 如何在 R 语言中向数据帧添加行(有 7 个代码示例)

> 原文：<https://www.dataquest.io/blog/append-to-dataframe-in-r/>

July 28, 2022![Append to DataFrame in R](img/71a8f14dce382fbbdaa88c5d9695cd3c.png)

### 在本教程中，我们将介绍如何在 R 中向数据帧追加数据，这是对数据帧执行的最常见的修改之一。

数据帧是 R 编程语言中基本的数据结构之一。众所周知，它非常灵活，因为它可以包含许多数据类型，并且易于修改。在 R 中对数据帧执行的最典型的修改之一是向它添加新的观察值(行)。

在本教程中，我们将讨论在 r 中向数据帧追加一行或多行的不同方法。

开始之前，让我们创建一个简单的数据框架作为实验:

```
super_sleepers_initial <- data.frame(animal=c('koala', 'hedgehog'), 
                                     country=c('Australia', 'Italy'),
                                     avg_sleep_hours=c(21, 18), 
                                     stringsAsFactors=FALSE)
print(super_sleepers_initial)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |

> **注:**在创建上述数据帧时，我们添加了可选参数`stringsAsFactors=FALSE`。虽然默认情况下这个参数是`TRUE`，但是在大多数情况下(除非我们没有字符数据类型的列)，强烈建议**将它设置为`FALSE`，以抑制字符到因子数据类型的默认转换，从而避免不希望的副作用。作为一个实验，您可以从上面的代码中删除这个参数，运行这个和后续的代码单元，并观察结果。**

## 在 R 中向数据帧追加一行

### 使用`rbind()`

要在 R 中向数据帧追加一行，我们可以使用内置函数`rbind()`，它代表“行绑定”。基本语法如下:

```
dataframe <- rbind(dataframe, new_row)
```

注意，在上面的语法中，通过`new_row`，我们很可能理解一个*列表*，而不是一个*向量*，除非我们的数据帧的所有列都是相同的数据类型(这种情况并不常见)。

让我们从最初的数据帧`super_sleepers_initial`重新构建一个新的数据帧`super_sleepers`，并向其追加一行:

```
# Reconstructing the `super_sleepers` dataframe
super_sleepers <- super_sleepers_initial

super_sleepers <- rbind(super_sleepers, list('sloth', 'Peru', 17))
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 懒惰 | 秘鲁 | Seventeen |

新行被追加到数据帧的末尾。

重要的是要记住，在这里和所有后续的例子中，新的一行(或多行)必须反映它所附加到的数据帧的结构，这意味着列表的长度必须等于数据帧中的列数，并且列表中项的数据类型序列必须与数据帧变量的数据类型序列相同。在相反的情况下，程序会抛出一个错误。

### 使用`nrow()`

向 R 数据帧追加一行的另一种方法是使用`nrow()`函数。语法如下:

```
dataframe[nrow(dataframe) + 1,] <- new_row
```

这个语法的字面意思是，我们计算数据帧中的行数(`nrow(dataframe)`)，给这个数加 1(`nrow(dataframe) + 1`)，然后在数据帧的索引处追加一个新行`new_row`(`dataframe[nrow(dataframe) + 1,]`)—即作为新的最后一行。

就像以前一样，我们的`new_row`很可能必须是一个*列表*，而不是一个*向量*，除非 DataFrame 的所有列都是相同的数据类型，这并不常见。

让我们再增加一个“超级睡眠者”:

```
super_sleepers[nrow(super_sleepers) + 1,] <- list('panda', 'China', 10)
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 懒惰 | 秘鲁 | Seventeen |
| four | 熊猫 | 中国 | Ten |

新行再次追加到数据帧的末尾。

### 使用`tidyverse`中的`add_row()`

相反，如果我们不想在数据帧的末尾添加新行，而是想在数据帧的某个特定索引处添加新行，该怎么办呢？例如，我们发现老虎每天睡 16 个小时(即，在我们的评级中比熊猫多，所以我们需要将此观察结果作为第二个插入到数据帧的最后一行)。在这种情况下，使用基础 R 是不够的，但是我们可以使用`tidyverse` R 包的 [`add_row()`](https://www.rdocumentation.org/packages/tibble/versions/3.1.6/topics/add_row) 函数(我们可能需要安装它，如果它还没有安装，通过运行`install.packages("tidyverse")`):

```
library(tidyverse)
super_sleepers <- super_sleepers %>% add_row(animal='tiger', 
                                             country='India', 
                                             avg_sleep_hours=16, 
                                             .before=4)
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 懒惰 | 秘鲁 | Seventeen |
| four | 老虎 | 印度 | Sixteen |
| five | 熊猫 | 中国 | Ten |

在这里，我们应该注意以下几点:

*   我们按照与现有数据帧相同的顺序传递相同的列名，并为它们分配相应的新值。
*   在这个序列之后，我们添加了可选参数`.before`并指定了必要的索引。如果我们不这样做，默认情况下，该行将被添加到数据帧的末尾。或者，我们可以使用可选参数`.after`,并为其分配在之后插入新观察的行*的索引。*
*   我们使用赋值操作符`<-`来保存应用于当前数据帧的修改。

## 在 R 中向数据帧追加多行

### 使用`rbind()`

通常，我们需要在 R 数据帧中添加多行而不是一行。这里最简单的方法还是使用`rbind()`函数。更准确地说，在这种情况下，我们实际上需要组合两个数据帧:初始数据帧和包含要追加的行的数据帧。

```
dataframe_1 <- rbind(dataframe_1, dataframe_2)
```

为了了解它是如何工作的，让我们从最初的数据帧`super_sleepers_initial`重新构建一个新的数据帧`super_sleepers`，将其打印出来以回忆它的样子，并在其末尾追加两行:

```
# Reconstructing the `super_sleepers` DataFrame
super_sleepers <- super_sleepers_initial
print(super_sleepers)
cat('\n\n')  # printing an empty line

# Creating a new DataFrame with the necessary rows
super_sleepers_2 <- data.frame(animal=c('squirrel', 'panda'), 
                               country=c('Canada', 'China'),
                               avg_sleep_hours=c(15, 10), 
                               stringsAsFactors=FALSE)

# Appending the rows of the new DataFrame to the end of the existing one
super_sleepers <- rbind(super_sleepers, super_sleepers_2)
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 松鼠 | 加拿大 | Fifteen |
| four | 老虎 | 印度 | Sixteen |

提醒一下，新行必须反映它们所附加到的数据帧的结构，这意味着两个数据帧中的列数、列名、它们的顺序和数据类型必须相同。

### 使用`nrow()`

或者，我们可以使用`nrow()`函数向 r 中的一个数据帧追加多行。然而，这里不推荐使用这种方法**，因为语法变得非常笨拙，难以阅读。事实上，在这种情况下，我们需要计算开始和结束索引。为此，我们必须用`nrow()`函数做更多的操作。从技术上讲，语法如下:**

```
dataframe_1[(nrow(dataframe_1) + 1):(nrow(dataframe_1) + nrow(dataframe_2)),] <- dataframe_2
```

让我们从`super_sleepers_initial`重新构建我们的`super_sleepers`，并将已经存在的数据帧`super_sleepers_2`的行追加到它们中:

```
# Reconstructing the `super_sleepers` dataframe
super_sleepers <- super_sleepers_initial

super_sleepers[(nrow(super_sleepers) + 1):(nrow(super_sleepers) + nrow(super_sleepers_2)),] <- super_sleepers_2
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 松鼠 | 加拿大 | Fifteen |
| four | 熊猫 | 中国 | Ten |

我们获得了与前一个例子相同的数据帧，并观察到前一个方法更加优雅。

### 使用`tidyverse`中的`add_row()`

最后，我们可以再次使用`tidyverse`包的`add_row()`函数。这种方法更加灵活，因为我们既可以在当前数据帧的末尾添加新行，也可以在索引指定的某一行之前/之后插入新行。

让我们在当前数据帧`super_sleepers`中的刺猬和松鼠之间插入树懒和老虎的观察结果:

```
library(tidyverse)
super_sleepers <- super_sleepers %>% add_row(animal=c('sloth', 'tiger'), 
                                             country=c('Peru', 'India'),
                                             avg_sleep_hours=c(17, 16), 
                                             .before=3)
print(super_sleepers)
```

|  | 动物 | 国家 | 平均睡眠时间 |
| one | 树袋熊 | 澳大利亚 | Twenty-one |
| Two | 刺猬 | 意大利 | Eighteen |
| three | 懒惰 | 秘鲁 | Seventeen |
| four | 老虎 | 印度 | Sixteen |
| five | 松鼠 | 加拿大 | Fifteen |
| six | 熊猫 | 中国 | Ten |

注意，为了获得上面的结果，我们可以用`.after=2`代替`.before=3`。

## 结论

在本教程中，我们学习了如何在 R 中向数据帧追加一行(或多行)——或者如何在数据帧的特定索引处插入一行(或多行)。特别是，我们考虑了 3 种方法:使用基 R 的`rbind()`或`nrow()`函数，或者`tidyverse` R 包的`add_row()`函数。我们特别关注每种方法的最佳用例，以及在各种情况下必须考虑的细微差别。**