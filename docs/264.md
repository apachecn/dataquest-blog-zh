# SQL 基础教程:今天就开始学习 SQL！

> 原文：<https://www.dataquest.io/blog/sql-fundamentals/>

February 11, 2020

无论您希望对数据做什么，拥有 SQL 技能可能是关键。

这是因为尽管事实上 SQL 已经很老了，但它的某个版本在地球上几乎每个公司的数据库中使用。无论您是对数据科学感兴趣，还是只是访问和分析公司的一点点数据，都有可能需要 SQL 基础知识。

## 什么是 SQL？

我们将以一个非常基本的问题开始这个 SQL 基础教程:到底什么是 SQL？SQL(通常读作“sequel”)是结构化查询语言的缩写。这是一种专门为查询和更新数据库而设计的编程语言。

这很有用，因为以电子表格或 CSV 等其他格式存储数据并不总是可行或可取的。例如，想象一下，一家公司拥有万亿字节的数据，这些数据由公司不同部门的人员定期更新。试图用电子表格来管理这将是一场后勤和安全噩梦！数据库允许安全存储大量数据，而 SQL 是我们用来访问和更改这些数据的工具，而不需要将它们全部存储在本地机器上的一个文件中。

在 SQL 中，我们将对数据库的请求表示为“查询”。例如，我们可能会向数据库发送一个查询(一条指令)，以返回特定的数据子集，如特定的表，或者更新数据库中的特定值。

假设我们想从公司的数据库中获取一些工资数据。我们当然不想下载*公司的所有*数据，所以我们将编写一个 SQL 查询，只给出我们需要的特定数据。如果我们假设公司数据库中有一个名为`salaries`的表，该查询可能如下所示:

`SELECT * FROM salaries`

这里的`*`字符表示“一切”——从`salaries`表中选择每一列。通过这个简单的查询，我们将能够从我们公司的数据库中得到我们需要的表。

但是 SQL 可以比查询一个完整的表做更多的事情。它可以帮助我们有效地合并、过滤、更新和分析数据，以回答各种问题。因此，让我们开始深入学习如何编写 SQL 查询吧！

注意:对于许多学生来说，以交互方式学习 SQL 并在浏览器中编写查询可能比跟着教程学习更容易。您可以在我们的 interactive SQL 基础课程中免费完成这项工作:

### 通过*做*来学习 SQL

使用我们的交互式学习平台，立即动手操作 SQL。

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

[Sign up go!](https://app.dataquest.io/signup)

*(开始免费)*

## SQL 基础教程:简介

在本教程中，我们将探究美国社区调查中基于大学专业的工作结果统计数据。虽然最初的 CSV 版本可以在 [FiveThirtyEight 的 Github](https://github.com/fivethirtyeight/data/tree/master/college-majors) 上找到，但我们将使用作为数据库存储的数据的稍加修改的版本。(记住，SQL 用于查询存储在数据库中的数据)。

具体来说，我们将只处理包含 2010-2012 年大学毕业生数据的数据。这些数据存储为 SQLite 数据库(SQLite 是最流行的基于 SQL 的数据库管理系统之一)。

如果您熟悉如何查询 SQLite 数据库，您可以在这里下载数据库文件。如果你不习惯在本地机器上建立并查询 SQLite 数据库，那么通过[我们免费的 SQL 基础课程](https://app.dataquest.io/course/sql-fundamentals)可能会更容易，该课程是交互式的，允许你只使用浏览器编写该数据库的真正 SQL 查询。

#### 使用选择预览表格

每当我们处理一个新的数据集时，从预览我们实际处理的东西开始是很有帮助的。对于本教程，我们将使用数据库文件`jobs.db`，它包含一个名为`recent_grads`的表:

这是该结构的可视化表示:

![SQL Table](img/ae4f35ecbf33ccf087cf8a831684990f.png)

让我们从只显示表格的前五行开始。这将使我们很好地了解我们正在处理什么，将查询限制在五行将确保它很快，我们不会被一个巨大的表淹没。

像其他编程语言一样，SQL 中的代码必须遵循定义的结构和词汇。要指定我们想要从`recent_grads`返回前 5 行，我们需要运行以下 SQL 查询:

```
SELECT * FROM recent_grads LIMIT 5
```

在此查询中，我们指定:

*   我们使用`SELECT *`想要的列
*   我们想要使用`FROM recent_grads`查询的表
*   使用`LIMIT 5`得到我们想要的行数

下面是查询的不同组成部分的可视化分解:

![SQL Select Breakdown 2](img/d1144f50d73aa1a43bb4bf79cae18282.png)

现在，让我们看看这个查询返回什么！以下是运行该查询时我们将得到的结果:

| 指数 | 军阶 | 专业代码 | 重要的 | 专业 _ 类别 | 总数 | 样本大小 | 男人 | 女人 | 分享女性 | 被雇用的 | 全职 | 兼职 | 全年无休 | 失业的 | 失业率 | 中位数 | p25 | 第 75 页 | 大学 _ 工作 | 非大学工作 | 低工资工作 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | one | Two thousand four hundred and nineteen | 石油钻采工程 | 工程 | Two thousand three hundred and thirty-nine | Thirty-six | Two thousand and fifty-seven | Two hundred and eighty-two | 0.120564 | One thousand nine hundred and seventy-six | One thousand eight hundred and forty-nine | Two hundred and seventy | One thousand two hundred and seven | Thirty-seven | 0.018381 | One hundred and ten thousand | Ninety-five thousand | One hundred and twenty-five thousand | One thousand five hundred and thirty-four | Three hundred and sixty-four | One hundred and ninety-three |
| one | Two | Two thousand four hundred and sixteen | 采矿和矿物工程 | 工程 | Seven hundred and fifty-six | seven | Six hundred and seventy-nine | Seventy-seven | 0.101852 | Six hundred and forty | Five hundred and fifty-six | One hundred and seventy | Three hundred and eighty-eight | eighty-five | 0.117241 | Seventy-five thousand | Fifty-five thousand | Ninety thousand | Three hundred and fifty | Two hundred and fifty-seven | Fifty |
| Two | three | Two thousand four hundred and fifteen | 冶金工程 | 工程 | Eight hundred and fifty-six | three | Seven hundred and twenty-five | One hundred and thirty-one | 0.153037 | Six hundred and forty-eight | Five hundred and fifty-eight | One hundred and thirty-three | Three hundred and forty | Sixteen | 0.024096 | Seventy-three thousand | Fifty thousand | One hundred and five thousand | Four hundred and fifty-six | One hundred and seventy-six | Zero |
| three | four | Two thousand four hundred and seventeen | 船舶建筑和海洋工程 | 工程 | One thousand two hundred and fifty-eight | Sixteen | One thousand one hundred and twenty-three | One hundred and thirty-five | 0.107313 | Seven hundred and fifty-eight | One thousand and sixty-nine | One hundred and fifty | Six hundred and ninety-two | Forty | 0.050125 | Seventy thousand | Forty-three thousand | Eighty thousand | Five hundred and twenty-nine | One hundred and two | Zero |
| four | five | Two thousand four hundred and five | 化学工程 | 工程 | Thirty-two thousand two hundred and sixty | Two hundred and eighty-nine | Twenty-one thousand two hundred and thirty-nine | Eleven thousand and twenty-one | 0.341631 | Twenty-five thousand six hundred and ninety-four | Twenty-three thousand one hundred and seventy | Five thousand one hundred and eighty | Sixteen thousand six hundred and ninety-seven | One thousand six hundred and seventy-two | 0.061098 | Sixty-five thousand | Fifty thousand | Seventy-five thousand | Eighteen thousand three hundred and fourteen | Four thousand four hundred and forty | Nine hundred and seventy-two |

正如我们所看到的，我们的查询返回了我们想要的结果:我们选择的表中的每一列，但只有前五行。如果我们将查询的结尾改为`LIMIT 10`，它将返回前十行。如果我们完全删除`LIMIT`命令，它将返回表中的每一行。

#### 关于大写的快速注释

在我们继续之前，您可能已经注意到我们输入了一些东西，比如`SELECT`，全部用大写字母，而其他的东西比如表名是用小写字母写的。

根据您查询的特定数据库和它使用的数据库管理系统的类型，表名的大小写可能重要，也可能不重要。但是，为了确保您的查询在任何系统下都能正常工作并易于阅读，最好遵循以下约定:

*   语句或命令(如`SELECT`、`LIMIT`等)。)应该全大写。
*   无论表名和列名存储在您查询的数据库中，它们都应该以小写形式书写(因此，如果数据库以小写形式存储它们，它们在您的查询中也应该以小写形式存储)。

现在，进入我们的下一个语句:`WHERE`！

#### 使用 WHERE 筛选行

如果我们看一下[这个数据集](https://github.com/fivethirtyeight/data/tree/master/college-majors)的细节，我们可以更好地了解这里有什么数据，以及我们可能能够回答什么类型的问题。**花些时间熟悉每一列所代表的内容。**

基于对每列所代表内容的理解，我们可能会有以下问题:

*   哪些专业女生居多？哪些学校男生居多？
*   哪些专业的起薪在 25%和 75%之间的差距最大？
*   哪些工程专业的全职就业率最高？

我们先来关注第一个问题。我们需要将这个问题翻译成 SQL，这样我们就可以从数据库中得到我们想要的答案。

为了确定哪些专业以女生为主，我们需要以下数据子集:

*   只有`Major`列
*   只有`ShareWomen`大于`0.5`的行(相当于 50%)

为了只返回`Major`列，我们需要将特定的列名添加到查询的`SELECT`语句部分(而不是使用`*`操作符返回所有列):

```
SELECT Major FROM recent_grads
```

这将返回`Major`列中所有值的*。我们也可以用这种方式指定多个列，结果表将保持列的顺序:*

```
SELECT Major, Major_category FROM recent_grads
```

为了只返回`ShareWomen`大于或等于`0.5`的值，我们需要添加一个`WHERE` *子句*:

```
SELECT Major FROM recent_gradsWHERE ShareWomen >= 0.5
```

最后，我们可以使用`LIMIT`来限制返回的行数:

```
SELECT Major FROM recent_gradsWHERE ShareWomen >= 0.5 LIMIT 5
```

如果我们运行该查询，我们将看到以下内容:

| 重要的 |
| --- |
| 保险统计计算科学 |
| 计算机科学 |
| 环境工程 |
| 护理 |
| 工业生产技术 |

下面是我们的查询的不同组成部分的分类:

![SQL WHERE Breakdown](img/cfa622a23ae83b3177fc14f30fa80f6c.png)

而在查询的`SELECT`部分，我们表达我们想要的特定列。在`WHERE`部分，我们表达了我们想要的特定行。SQL 的美妙之处在于这些可以是独立的。

让我们编写一个 SQL 查询，返回女性占少数的专业。我们将只返回`Major`和`ShareWomen`列(按此顺序),我们不会限制返回的行数:

```
SELECT Major, ShareWomen FROM recent_grads WHERE ShareWomen < 0.5
```

| 重要的 | 分享女性 |
| --- | --- |
| 石油钻采工程 | 0.120564 |
| 采矿和矿物工程 | 0.101852 |
| 冶金工程 | 0.153037 |
| 船舶建筑和海洋工程 | 0.107313 |
| 化学工程 | 0.341631 |
| 核技术 | 0.144967 |
| 天文学和天体物理学 | 0.441356 |
| 机械工程 | 0.139793 |
| 电机工程 | 0.437847 |
| 计算机工程 | 0.199413 |
| 航空和航天工程 | 0.196450 |
| 生物医学工程 | 0.119559 |
| 材料科学 | 0.310820 |
| 工程力学物理和科学 | 0.183985 |
| 生物工程 | 0.320784 |
| 工业和制造工程 | 0.343473 |
| 普通工程 | 0.252960 |
| 建筑工程 | 0.350442 |
| 法庭记录 | 0.236063 |
| 营养学 | 0.222695 |
| 电气工程技术 | 0.325092 |
| 材料工程和材料科学 | 0.292607 |
| 管理信息系统和统计 | 0.278790 |
| 土木工程 | 0.227118 |
| 建筑服务 | 0.342229 |
| 运营物流和电子商务 | 0.322222 |
| 杂项工程 | 0.189970 |
| 国家政策 | 0.251389 |
| 工程技术 | 0.090713 |
| 杂项美术 | 0.410180 |
| 地质和地球物理工程 | 0.324838 |
| 金融 | 0.355469 |
| 经济学 | 0.340825 |
| 企业经济学 | 0.249190 |
| 核、工业放射和生物… | 0.430537 |
| 会计 | 0.253583 |
| 数学 | 0.244103 |
| 物理学 | 0.448099 |
| 医疗技术技术员 | 0.434298 |
| 统计和决策科学 | 0.281936 |
| 工程和工业管理 | 0.174123 |
| 医疗辅助服务 | 0.178982 |
| 计算机编程和数据处理 | 0.269194 |
| 一般业务 | 0.417925 |
| 体系结构 | 0.321770 |
| 国际商业 | 0.282903 |
| 药学药学科学与管理 | 0.451465 |
| 分子生物学 | 0.077453 |
| 杂项商业和医疗管理 | 0.200023 |
| 杂项工程技术 | 0.000000 |
| 机械工程相关技术 | 0.377437 |
| 工业和组织心理学 | 0.436302 |
| 自然科学 | 0.426924 |
| 军事技术 | 0.429685 |
| 电气、机械和精密技术… | 0.232444 |
| 营销和营销研究 | 0.382900 |
| 政治学与政府 | 0.485930 |
| 地理 | 0.473190 |
| 计算机管理和安全 | 0.180883 |
| 计算机网络和电信 | 0.305005 |
| 地质学和地球科学 | 0.470197 |
| 公共行政 | 0.476461 |
| 通信 | 0.305109 |
| 刑事司法和消防 | 0.125035 |
| 商业艺术和平面设计 | 0.374356 |
| 特殊需要教育 | 0.366177 |
| 交通科学与技术 | 0.321296 |
| 神经系统科学 | 0.475010 |
| 多学科/跨学科研究 | 0.495397 |
| 大气科学和气象学 | 0.124950 |
| 教育管理和监督 | 0.448732 |
| 哲学和宗教研究 | 0.416810 |
| 英语语言和文学 | 0.339671 |
| 科学和计算机教师教育 | 0.423209 |
| 音乐 | 0.444582 |
| 美容服务和烹饪艺术 | 0.383719 |

#### 使用 AND 表示多个筛选条件

为了按照特定的标准过滤行，我们需要使用`WHERE`语句。一个简单的`WHERE`语句需要三样东西:

*   我们希望数据库过滤的列:`ShareWomen`
*   一个比较操作符，指定我们希望如何比较一个列中的值:`>`
*   我们希望数据库与每个值进行比较的值:`0.5`

下面是我们可以使用的比较运算符:

*   小于:`<`
*   小于或等于:`<=`
*   大于:`>`
*   大于或等于:`>=`
*   等于:`=`
*   不等于:`!=`

运算符后的比较值必须是文本或数字，具体取决于字段。因为`ShareWomen`是一个数字列，我们不需要用引号将数字`0.5`括起来。

**最后，大多数数据库系统要求`SELECT`和`FROM`语句排在前面，在`WHERE`或任何其他语句之前。**我们可以使用`AND`操作符来组合多个过滤标准。例如，要确定哪个工程专业女生占多数，我们需要指定 2 个过滤标准。

```
SELECT Major FROM recent_gradsWHERE Major_category = 'Engineering' AND ShareWomen > 0.5
```

| 重要的 |
| --- |
| 环境工程 |
| 工业生产技术 |

看起来只有两个专业符合这个标准。如果我们想“缩小”回去查看这两个专业的所有列的“T2 ”( T3 ),看看它们是否有一些共同的属性，我们可以修改语句“T0 ”,使用符号“T1”来表示所有列

```
SELECT * FROM recent_gradsWHERE Major_category = 'Engineering' AND ShareWomen > 0.5
```

| 指数 | 军阶 | 专业代码 | 重要的 | 专业 _ 类别 | 总数 | 样本大小 | 男人 | 女人 | 分享女性 | 被雇用的 | 全职 | 兼职 | 全年无休 | 失业的 | 失业率 | 中位数 | p25 | 第 75 页 | 大学 _ 工作 | 非大学工作 | 低工资工作 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Thirty | Thirty-one | Two thousand four hundred and ten | 环境工程 | 工程 | Four thousand and forty-seven | Twenty-six | Two thousand six hundred and thirty-nine | Three thousand three hundred and thirty-nine | 0.558548 | Two thousand nine hundred and eighty-three | Two thousand three hundred and eighty-four | Nine hundred and thirty | One thousand nine hundred and fifty-one | Three hundred and eight | 0.093589 | Fifty thousand | Forty-two thousand | Fifty-six thousand | Two thousand and twenty-eight | Eight hundred and thirty | Two hundred and sixty |
| Thirty-eight | Thirty-nine | Two thousand five hundred and three | 工业生产技术 | 工程 | Four thousand six hundred and thirty-one | Seventy-three | Five hundred and twenty-eight | One thousand five hundred and eighty-eight | 0.750473 | Four thousand four hundred and twenty-eight | Three thousand nine hundred and eighty-eight | Five hundred and ninety-seven | Three thousand two hundred and forty-two | One hundred and twenty-nine | 0.028308 | Forty-six thousand | Thirty-five thousand | Sixty-five thousand | One thousand three hundred and ninety-four | Two thousand four hundred and fifty-four | Four hundred and eighty |

在考虑新问题时快速迭代查询的能力是 SQL 最强大的方面之一。SQL 工作流让数据专业人员专注于提问和回答问题，而不是担心底层编程概念。

现在，让我们编写一个 SQL 查询，返回所有女生占多数的专业**和**的平均工资高于`50000`。让我们也限制结果中显示的列，如下所示:

```
SELECT Major, Major_category, Median, ShareWomen FROM recent_grads WHERE ShareWomen > 0.5 AND Median > 50000
```

| 重要的 | 专业 _ 类别 | 中位数 | 分享女性 |
| --- | --- | --- | --- |
| 保险统计计算科学 | 商业 | Sixty-two thousand | 0.535714 |
| 计算机科学 | 计算机和数学 | Fifty-three thousand | 0.578766 |

似乎只有两个以女生为主的专业的平均工资高于 5 万美元。

#### 用 OR 返回几个条件之一

我们使用了`AND`操作符来指定我们的过滤器需要符合两个布尔条件。这两个条件都必须评估为`True`,记录才会出现在结果中。

如果我们想指定一个过滤器满足**或者**的条件，我们可以使用`OR`操作符。

```
SELECT [column1, column2,...] FROM [table1]WHERE [condition1] OR [condition2]
```

让我们编写一个 SQL 查询，返回**的前 20 个**专业，其中**或者**的`Median`薪水大于或等于`10,000`，**或者**的人数小于或等于`1,000` `Unemployed`。让我们只在结果中包括以下各列，并按顺序包括**:**

*   `Major`
*   `Median`
*   `Unemployed`

```
SELECT Major, Median, Unemployed FROM recent_grads WHERE Median >= 10000 OR Unemployed <= 1000 LIMIT 20
```

| 重要的 | 中位数 | 失业的 |
| --- | --- | --- |
| 石油钻采工程 | One hundred and ten thousand | Thirty-seven |
| 采矿和矿物工程 | Seventy-five thousand | eighty-five |
| 冶金工程 | Seventy-three thousand | Sixteen |
| 船舶建筑和海洋工程 | Seventy thousand | Forty |
| 化学工程 | Sixty-five thousand | One thousand six hundred and seventy-two |
| 核技术 | Sixty-five thousand | four hundred |
| 保险统计计算科学 | Sixty-two thousand | Three hundred and eight |
| 天文学和天体物理学 | Sixty-two thousand | Thirty-three |
| 机械工程 | Sixty thousand | Four thousand six hundred and fifty |
| 电机工程 | Sixty thousand | Three thousand eight hundred and ninety-five |
| 计算机工程 | Sixty thousand | Two thousand two hundred and seventy-five |
| 航空和航天工程 | Sixty thousand | Seven hundred and ninety-four |
| 生物医学工程 | Sixty thousand | One thousand and nineteen |
| 材料科学 | Sixty thousand | seventy-eight |
| 工程力学物理和科学 | Fifty-eight thousand | Twenty-three |
| 生物工程 | Fifty-seven thousand one hundred | Five hundred and eighty-nine |
| 工业和制造工程 | Fifty-seven thousand | Six hundred and ninety-nine |
| 普通工程 | Fifty-six thousand | Two thousand eight hundred and fifty-nine |
| 建筑工程 | Fifty-four thousand | One hundred and seventy |
| 法庭记录 | Fifty-four thousand | Eleven |

现在，让我们开始使用更高级的 SQL 查询。

#### 用括号将运算符分组

有一类问题仅靠我们目前所学的技术是无法回答的。例如，如果我们想编写一个查询，返回所有的专业，或者是 T2 的女毕业生居多，或者是 T4 的失业率低于 5.1%，我们需要用括号来表达这个更复杂的逻辑。

我们需要的三个原始条件是:

```
Major_category = 'Engineering'ShareWomen >= 0.5Unemployment_rate < 0.051
```

使用 parantheses 的 SQL 查询看起来像什么:

```
SELECT Major, Major_category, ShareWomen, Unemployment_rate FROM recent_grads
WHERE (Major_category = 'Engineering') AND (ShareWomen > 0.5 OR Unemployment_rate < 0.051);
```

请注意，我们将希望一起评估的逻辑括在括号中。这非常类似于我们如何使用括号将数学计算组合在一起，以定义首先评估哪些计算。括号清楚地向数据库表明，我们希望所有语句中表达式的*和*都等于`True`的行:

```
(Major_category = 'Engineering' AND ShareWomen > 0.5) -> True or False(ShareWomen > 0.5 OR Unemployment_rate < 0.051) -> True or False
```

如果我们编写了不带任何括号的`where`语句，数据库会猜测我们的意图，并实际执行以下查询:

```
WHERE (Major_category = 'Engineering' AND ShareWomen > 0.5) OR (Unemployment_rate < 0.051)
```

去掉括号意味着我们希望计算从左到右进行，按照编写逻辑的顺序，这样它就不会返回我们想要的数据。

现在让我们运行预期的查询并查看结果！我们将运行上面探索过的查询，它将返回所有的`Engineering`专业，这些专业要么是**要么是**的女性毕业生居多**要么是**的失业率低于 **5.1%** ，这是 2015 年 8 月的全国失业率。

让我们只在结果中包括以下各列，并按顺序包括**:**

*   `Major`
*   `Major_category`
*   `ShareWomen`
*   `Unemployment_rate`

我们的疑问是:

```
SELECT Major, Major_category, ShareWomen, Unemployment_rateFROM recent_gradsWHERE (Major_category = 'Engineering') AND (ShareWomen > 0.5 OR Unemployment_rate < 0.051)
```

| 重要的 | 专业 _ 类别 | 分享女性 | 失业率 |
| --- | --- | --- | --- |
| 石油钻采工程 | 工程 | 0.120564 | 0.018381 |
| 冶金工程 | 工程 | 0.153037 | 0.024096 |
| 船舶建筑和海洋工程 | 工程 | 0.107313 | 0.050125 |
| 材料科学 | 工程 | 0.310820 | 0.023043 |
| 工程力学物理和科学 | 工程 | 0.183985 | 0.006334 |
| 工业和制造工程 | 工程 | 0.343473 | 0.042876 |
| 材料工程和材料科学 | 工程 | 0.292607 | 0.027789 |
| 环境工程 | 工程 | 0.558548 | 0.093589 |
| 工业生产技术 | 工程 | 0.750473 | 0.028308 |
| 工程和工业管理 | 工程 | 0.174123 | 0.033652 |

这看起来很棒，但是如果我们想根据特定列的值对结果进行排序，以使它们更具可读性，该怎么办呢？

#### 使用 ORDER BY 对结果进行排序

到目前为止，我们编写的每个查询的结果都是按照`Rank`列排序的。回想一下本文前面返回所有列的查询，您会看到 Rank 列:

```
SELECT * FROM recent_grads LIMIT 5
```

| 指数 | 军阶 | 专业代码 | 重要的 | 专业 _ 类别 | 总数 | 样本大小 | 男人 | 女人 | 分享女性 | 被雇用的 | 全职 | 兼职 | 全年无休 | 失业的 | 失业率 | 中位数 | p25 | 第 75 页 | 大学 _ 工作 | 非大学工作 | 低工资工作 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Zero | one | Two thousand four hundred and nineteen | 石油钻采工程 | 工程 | Two thousand three hundred and thirty-nine | Thirty-six | Two thousand and fifty-seven | Two hundred and eighty-two | 0.120564 | One thousand nine hundred and seventy-six | One thousand eight hundred and forty-nine | Two hundred and seventy | One thousand two hundred and seven | Thirty-seven | 0.018381 | One hundred and ten thousand | Ninety-five thousand | One hundred and twenty-five thousand | One thousand five hundred and thirty-four | Three hundred and sixty-four | One hundred and ninety-three |
| one | Two | Two thousand four hundred and sixteen | 采矿和矿物工程 | 工程 | Seven hundred and fifty-six | seven | Six hundred and seventy-nine | Seventy-seven | 0.101852 | Six hundred and forty | Five hundred and fifty-six | One hundred and seventy | Three hundred and eighty-eight | eighty-five | 0.117241 | Seventy-five thousand | Fifty-five thousand | Ninety thousand | Three hundred and fifty | Two hundred and fifty-seven | Fifty |
| Two | three | Two thousand four hundred and fifteen | 冶金工程 | 工程 | Eight hundred and fifty-six | three | Seven hundred and twenty-five | One hundred and thirty-one | 0.153037 | Six hundred and forty-eight | Five hundred and fifty-eight | One hundred and thirty-three | Three hundred and forty | Sixteen | 0.024096 | Seventy-three thousand | Fifty thousand | One hundred and five thousand | Four hundred and fifty-six | One hundred and seventy-six | Zero |
| three | four | Two thousand four hundred and seventeen | 船舶建筑和海洋工程 | 工程 | One thousand two hundred and fifty-eight | Sixteen | One thousand one hundred and twenty-three | One hundred and thirty-five | 0.107313 | Seven hundred and fifty-eight | One thousand and sixty-nine | One hundred and fifty | Six hundred and ninety-two | Forty | 0.050125 | Seventy thousand | Forty-three thousand | Eighty thousand | Five hundred and twenty-nine | One hundred and two | Zero |
| four | five | Two thousand four hundred and five | 化学工程 | 工程 | Thirty-two thousand two hundred and sixty | Two hundred and eighty-nine | Twenty-one thousand two hundred and thirty-nine | Eleven thousand and twenty-one | 0.341631 | Twenty-five thousand six hundred and ninety-four | Twenty-three thousand one hundred and seventy | Five thousand one hundred and eighty | Sixteen thousand six hundred and ninety-seven | One thousand six hundred and seventy-two | 0.061098 | Sixty-five thousand | Fifty thousand | Seventy-five thousand | Eighteen thousand three hundred and fourteen | Four thousand four hundred and forty | Nine hundred and seventy-two |

随着我们想要回答的问题变得越来越复杂，我们希望对结果的排序有更多的控制。我们可以使用 ORDER BY 子句指定顺序。

例如，我们可能想了解哪些符合`WHERE`陈述中标准的专业失业率最低。下面的查询将按照`Unemployment_rate`列的升序返回结果。

```
SELECT Rank, Major, Major_category, ShareWomen, Unemployment_rate
FROM recent_grads
WHERE (Major_category = 'Engineering') AND (ShareWomen > 0.5 OR Unemployment_rate < 0.051)
ORDER BY Unemployment_rate
```

| 军阶 | 重要的 | 专业 _ 类别 | 分享女性 | 失业率 |
| --- | --- | --- | --- | --- |
| Fifteen | 工程力学物理和科学 | 工程 | 0.183985 | 0.006334 |
| one | 石油钻采工程 | 工程 | 0.120564 | 0.018381 |
| Fourteen | 材料科学 | 工程 | 0.310820 | 0.023043 |
| three | 冶金工程 | 工程 | 0.153037 | 0.024096 |
| Twenty-four | 材料工程和材料科学 | 工程 | 0.292607 | 0.027789 |
| Thirty-nine | 工业生产技术 | 工程 | 0.750473 | 0.028308 |
| Fifty-one | 工程和工业管理 | 工程 | 0.174123 | 0.033652 |
| Seventeen | 工业和制造工程 | 工程 | 0.343473 | 0.042876 |
| four | 船舶建筑和海洋工程 | 工程 | 0.107313 | 0.050125 |
| Thirty-one | 环境工程 | 工程 | 0.558548 | 0.093589 |

如果我们想让结果按同一列排序，但按*降序*排序，我们可以添加`DESC`关键字:

```
SELECT Rank, Major, Major_category, ShareWomen, Unemployment_rate
FROM recent_gradsWHERE (Major_category = 'Engineering') AND (ShareWomen > 0.5 OR Unemployment_rate < 0.051)
ORDER BY Unemployment_rate DESC
```

| 军阶 | 重要的 | 专业 _ 类别 | 分享女性 | 失业率 |
| --- | --- | --- | --- | --- |
| Thirty-one | 环境工程 | 工程 | 0.558548 | 0.093589 |
| four | 船舶建筑和海洋工程 | 工程 | 0.107313 | 0.050125 |
| Seventeen | 工业和制造工程 | 工程 | 0.343473 | 0.042876 |
| Fifty-one | 工程和工业管理 | 工程 | 0.174123 | 0.033652 |
| Thirty-nine | 工业生产技术 | 工程 | 0.750473 | 0.028308 |
| Twenty-four | 材料工程和材料科学 | 工程 | 0.292607 | 0.027789 |
| three | 冶金工程 | 工程 | 0.153037 | 0.024096 |
| Fourteen | 材料科学 | 工程 | 0.310820 | 0.023043 |
| one | 石油钻采工程 | 工程 | 0.120564 | 0.018381 |
| Fifteen | 工程力学物理和科学 | 工程 | 0.183985 | 0.006334 |

让我们编写一个查询，返回所有专业，其中`ShareWomen`大于`0.3`，`Unemployment_rate`小于`.1`。让我们只在结果中包括以下各列，并按此顺序包括**:**

 ***   `Major`，
*   `ShareWomen`，
*   `Unemployment_rate`

我们将按照`ShareWomen`列对结果进行降序排序。这将使我们能够很容易地比较女性比例较高或较低的专业的失业率。

```
SELECT Major, ShareWomen, Unemployment_rate FROM recent_grads
WHERE ShareWomen > 0.3 AND Unemployment_rate < .1O
RDER BY ShareWomen DESC
```

这是我们的结果！

| 重要的 | 分享女性 | 失业率 |
| --- | --- | --- |
| 早期儿童教育 | 0.967998 | 0.040105 |
| 数学和计算机科学 | 0.927807 | 0.000000 |
| 初等教育 | 0.923745 | 0.046586 |
| 动物科学 | 0.910933 | 0.050862 |
| 生理学 | 0.906677 | 0.069163 |
| 杂项心理学 | 0.905590 | 0.051908 |
| 人类服务和社区组织 | 0.904075 | 0.037819 |
| 护理 | 0.896019 | 0.044863 |
| 地学 | 0.881294 | 0.024374 |
| 大众传媒 | 0.877228 | 0.089837 |
| 认知科学和生物心理学 | 0.854523 | 0.075236 |
| 艺术史和批评 | 0.845934 | 0.060298 |
| 教育心理学 | 0.817099 | 0.065112 |
| 普通教育 | 0.812877 | 0.057360 |
| 社会福利工作 | 0.810704 | 0.068828 |
| 教师教育:多层次 | 0.798920 | 0.036546 |
| 咨询心理学 | 0.798746 | 0.053621 |
| 数学教师教育 | 0.792095 | 0.016203 |
| 心理学 | 0.779933 | 0.083811 |
| 一般医疗卫生服务 | 0.774577 | 0.082102 |
| 卫生和医疗行政服务 | 0.770901 | 0.089626 |
| 土壤学 | 0.764427 | 0.000000 |
| 区域民族与文明研究 | 0.758060 | 0.063429 |
| 应用数学 | 0.753927 | 0.090823 |
| 家庭和消费者科学 | 0.752144 | 0.067128 |
| 工业生产技术 | 0.750473 | 0.028308 |
| 社会心理学 | 0.747561 | 0.029650 |
| 人文学科 | 0.745662 | 0.068584 |
| 酒店管理 | 0.733992 | 0.061169 |
| 社会科学或历史教师教育 | 0.733968 | 0.054083 |
| 神学和宗教职业 | 0.728495 | 0.062628 |
| 法语德语拉丁语和其他常见的外国语言… | 0.728033 | 0.075566 |
| 跨学科社会科学 | 0.721866 | 0.092306 |
| 杂项农业 | 0.719974 | 0.059767 |
| 新闻工作 | 0.719859 | 0.069176 |
| 杂项教育 | 0.718365 | 0.059212 |
| 计算机和信息系统 | 0.707719 | 0.093460 |
| 交流障碍科学与服务 | 0.707136 | 0.047584 |
| 杂项健康医疗职业 | 0.702020 | 0.081411 |
| 人文科学 | 0.700898 | 0.078268 |
| 林业 | 0.690365 | 0.096726 |
| 海洋学 | 0.688999 | 0.056995 |
| 艺术和音乐教育 | 0.686024 | 0.038638 |
| 体育健身公园休闲娱乐 | 0.683943 | 0.051467 |
| 广告和公共关系 | 0.673143 | 0.067961 |
| 人力资源和人事管理 | 0.672161 | 0.059570 |
| 多学科或普通科学 | 0.669999 | 0.055807 |
| 美术 | 0.667034 | 0.084186 |
| 作文和修辞 | 0.666119 | 0.081742 |
| 历史 | 0.651741 | 0.095667 |
| 生态学 | 0.651660 | 0.054475 |
| 遗传学 | 0.643331 | 0.034118 |
| 治疗专业 | 0.640000 | 0.059821 |
| 营养科学 | 0.638147 | 0.068701 |
| 动物学 | 0.637293 | 0.046320 |
| 国际关系 | 0.632987 | 0.096799 |
| 美国历史 | 0.630716 | 0.047179 |
| 戏剧和戏剧艺术 | 0.629505 | 0.077541 |
| 犯罪学 | 0.618223 | 0.097244 |
| 微生物学 | 0.615727 | 0.066776 |
| 植物科学和农学 | 0.606889 | 0.045455 |
| 生物 | 0.601858 | 0.070725 |
| 中等师范教育 | 0.601752 | 0.052229 |
| 农业生产与管理 | 0.594208 | 0.050031 |
| 法律预科和法律研究 | 0.591001 | 0.071965 |
| 农业经济学 | 0.589712 | 0.077250 |
| 工作室艺术 | 0.584776 | 0.089552 |
| 环境科学 | 0.584556 | 0.078585 |
| 商业管理和行政 | 0.580948 | 0.072218 |
| 计算机科学 | 0.578766 | 0.063173 |
| 语言和戏剧教育 | 0.576360 | 0.050306 |
| 杂项生物学 | 0.566641 | 0.058545 |
| 自然资源管理 | 0.564639 | 0.066619 |
| 环境工程 | 0.558548 | 0.093589 |
| 健康和医疗预备项目 | 0.556604 | 0.069780 |
| 杂项社会科学 | 0.543405 | 0.073080 |
| 保险统计计算科学 | 0.535714 | 0.095652 |
| 社会学 | 0.532334 | 0.084951 |
| 植物学 | 0.528969 | 0.000000 |
| 信息科学 | 0.526476 | 0.060741 |
| 药理学 | 0.524153 | 0.085532 |
| 普通农业 | 0.515543 | 0.019642 |
| 生物化学科学 | 0.515406 | 0.080531 |
| 跨文化和国际研究 | 0.507377 | 0.083634 |
| 体育与健康教学 | 0.506721 | 0.074667 |
| 化学 | 0.505141 | 0.053972 |
| 多学科/跨学科研究 | 0.495397 | 0.070861 |
| 神经系统科学 | 0.475010 | 0.048482 |
| 地质学和地球科学 | 0.470197 | 0.075449 |
| 药学药学科学与管理 | 0.451465 | 0.055521 |
| 教育管理和监督 | 0.448732 | 0.000000 |
| 物理学 | 0.448099 | 0.048224 |
| 音乐 | 0.444582 | 0.075960 |
| 天文学和天体物理学 | 0.441356 | 0.021167 |
| 电机工程 | 0.437847 | 0.059174 |
| 医疗技术技术员 | 0.434298 | 0.036983 |
| 核、工业放射和生物… | 0.430537 | 0.071540 |
| 自然科学 | 0.426924 | 0.035354 |
| 科学和计算机教师教育 | 0.423209 | 0.047264 |
| 一般业务 | 0.417925 | 0.072861 |
| 哲学和宗教研究 | 0.416810 | 0.096052 |
| 杂项美术 | 0.410180 | 0.089375 |
| 美容服务和烹饪艺术 | 0.383719 | 0.055677 |
| 营销和营销研究 | 0.382900 | 0.061215 |
| 机械工程相关技术 | 0.377437 | 0.056357 |
| 商业艺术和平面设计 | 0.374356 | 0.096798 |
| 特殊需要教育 | 0.366177 | 0.041508 |
| 金融 | 0.355469 | 0.060686 |
| 建筑工程 | 0.350442 | 0.061931 |
| 工业和制造工程 | 0.343473 | 0.042876 |
| 建筑服务 | 0.342229 | 0.060023 |
| 化学工程 | 0.341631 | 0.061098 |
| 经济学 | 0.340825 | 0.099092 |
| 英语语言和文学 | 0.339671 | 0.087724 |
| 电气工程技术 | 0.325092 | 0.087557 |
| 地质和地球物理工程 | 0.324838 | 0.075038 |
| 运营物流和电子商务 | 0.322222 | 0.047859 |
| 交通科学与技术 | 0.321296 | 0.072725 |
| 生物工程 | 0.320784 | 0.087143 |
| 材料科学 | 0.310820 | 0.023043 |
| 通信 | 0.305109 | 0.075177 |

## 掌握 SQL 的基础知识

如您所见，SQL 是一种强大的数据访问语言，这篇文章只是触及了皮毛。如果你想了解更多，我们鼓励你免费注册并开始[我们的 interactive SQL 基础课程](https://app.dataquest.io/signup?lesson=252&source=https://www.dataquest.io/blog/sql-fundamentals/)，这篇博文就是基于此。不过，本课程甚至更深入，将教您如何使用 SQL 做以下事情:

*   计算汇总统计数据
*   使用分组进一步细分数据
*   使用子查询编写更复杂的查询

在本课程中，您将继续使用最近大学毕业生的工资数据以及《中央情报局世界概况》中的数据。

### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)**