# SQL 联接教程:使用数据库

> 原文：<https://www.dataquest.io/blog/sql-joins-tutorial/>

January 18, 2021![sql-joins-tutorial](img/fd915960ef09572e3b72f51b116bffd6.png)

SQL 连接不必如此具有挑战性！

当第一次学习 SQL 时，处理单个表中的数据是很常见的。在现实世界中，数据库通常在多个表中存储数据。如果我们希望能够处理这些数据，我们必须在一个查询中组合多个表。在这个 SQL 连接教程中，我们将学习如何使用**连接**从多个表中选择数据。

我们假设您了解使用 SQL 的基础知识，包括过滤、排序、聚合和子查询。如果你不知道，我们的 [SQL 基础课程](https://www.dataquest.io/path/sql-skills/)会教授所有这些概念，你可以注册并开始免费的课程**。**

 **### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)

## 事实手册数据库

我们将使用 CIA World Factbook (Factbook)数据库的一个版本，它有两个表。第一个表叫做`facts`，每行代表一个国家。以下是`facts`表格的前 5 行:

| 身份证明（identification） | 密码 | 名字 | 区域 | 面积 _ 土地 | 面积 _ 水 | 人口 | 人口增长 | 出生率 | 死亡率 | 迁移率 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| one | 视频（同 audio frequency） | 阿富汗 | Six hundred and fifty-two thousand two hundred and thirty | Six hundred and fifty-two thousand two hundred and thirty | Zero | Thirty-two million five hundred and sixty-four thousand three hundred and forty-two | Two point three two | Thirty-eight point five seven | Thirteen point eight nine | One point five one |
| Two | -艾尔 | 阿尔巴尼亚 | Twenty-eight thousand seven hundred and forty-eight | Twenty-seven thousand three hundred and ninety-eight | One thousand three hundred and fifty | Three million twenty-nine thousand two hundred and seventy-eight | Zero point three | Twelve point nine two | Six point five eight | Three point three |
| three | （对他人的话作出反应或表示生气、恼怒）唔，啊，哎呀 | 阿尔及利亚 | Two million three hundred and eighty-one thousand seven hundred and forty-one | Two million three hundred and eighty-one thousand seven hundred and forty-one | Zero | Thirty-nine million five hundred and forty-two thousand one hundred and sixty-six | One point eight four | Twenty-three point six seven | Four point three one | Zero point nine two |
| four | 一；一个 | 安道尔 | Four hundred and sixty-eight | Four hundred and sixty-eight | Zero | Eighty-five thousand five hundred and eighty | Zero point one two | Eight point one three | Six point nine six | Zero |
| five | 安哥拉 | 安哥拉 | One million two hundred and forty-six thousand seven hundred | One million two hundred and forty-six thousand seven hundred | Zero | Nineteen million six hundred and twenty-five thousand three hundred and fifty-three | Two point seven eight | Thirty-eight point seven eight | Eleven point four nine | Zero point four six |

除了`facts`表之外，还有一个名为`cities`的表，它包含了来自 Factbook 中各个国家的[主要城市区域](https://www.cia.gov/the-world-factbook/field/major-urban-areas-population/)的信息(在本教程的其余部分，我们将使用“城市”一词来表示“主要城市区域”)。让我们看一下该表的前几行以及对每一列所代表的内容的描述:

| 身份证明（identification） | 名字 | 人口 | 首都 | 事实 _id |
| --- | --- | --- | --- | --- |
| one | 奥兰耶斯塔德 | Thirty-seven thousand | one | Two hundred and sixteen |
| Two | 圣约翰学院 | Twenty-seven thousand | one | six |
| three | 阿布扎比 | Nine hundred and forty-two thousand | one | One hundred and eighty-four |
| four | 迪拜 | One million nine hundred and seventy-eight thousand | Zero | One hundred and eighty-four |
| five | 沙迦 | Nine hundred and eighty-three thousand | Zero | One hundred and eighty-four |

*   `id`–每个城市的唯一 ID。
*   `name`–城市的名字。
*   城市的人口。
*   `capital`–该城市是否为首都:1 表示是，0 表示不是。
*   `facts_id`–国家的 ID，来自事实表。

最后一列是我们特别感兴趣的，因为这一列数据也存在于我们的原始`facts`表中。表之间的这种链接很重要，因为它用于组合查询中的数据。下面是一个**模式图**，它显示了我们数据库中的两个表，其中的列以及这两个表是如何链接的。

![schema diagram](img/2b8a19c82c62b7b469c9da196db789a5.png)

模式图中的线条清楚地显示了`facts`表中的`id`列和城市表中的`facts_id`列之间的链接。

如果您想下载数据库以便在自己的计算机上使用，您可以[将数据集下载为 SQLite 数据库](https://dq-blog-files.s3.amazonaws.com/joins-tutorial/factbook.db)。

## 我们的第一个 SQL 连接

使用 SQL 连接数据的最常见方式是使用一个**内部连接**。内部联接的语法是:

```py
SELECT [column_names] FROM [table_name_one]
INNER JOIN [table_name_two] ON [join_constraint];
```

inner join 子句由两部分组成:

*   `INNER JOIN`，它告诉 SQL 引擎您希望在查询中连接的表的名称，以及您希望使用内部连接。
*   `ON`，它告诉 SQL 引擎使用什么列来连接两个表。

连接通常用在查询中的`FROM`子句之后。让我们来看一个基本的内部连接，其中我们组合了来自两个表的数据。

```py
SELECT * FROM facts
INNER JOIN cities ON cities.facts_id = facts.id
LIMIT 5;
```

让我们看看包含连接的查询行:

*   `INNER JOIN cities`:这告诉 SQL 引擎我们希望使用内部连接将城市表连接到我们的查询中。
*   `ON cities.facts_id = facts.id`:这告诉 SQL 引擎在连接数据时使用哪些列，遵循语法 table_name.column_name。

您可能会认为`SELECT * FROM facts`意味着查询只返回来自`facts`表的列，但是`*`通配符在与连接一起使用时会给出来自两个表的所有列。以下是该查询的结果:

| 身份证明（identification） | 密码 | 名字 | 区域 | 面积 _ 土地 | 面积 _ 水 | 人口 | 人口增长 | 出生率 | 死亡率 | 迁移率 | 身份证明（identification） | 名字 | 人口 | 首都 | 事实 _id |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Two hundred and sixteen | 嗜酒者互诫协会 | 阿鲁巴岛 | one hundred and eighty  | one hundred and eighty  | Zero | One hundred and twelve thousand one hundred and sixty-two | One point three three | Twelve point five six | Eight point one eight | Eight point nine two | one | 奥兰耶斯塔德 | Thirty-seven thousand | one | Two hundred and sixteen |
| six | 交流电 | 安提瓜和巴布达 | Four hundred and forty-two | Four hundred and forty-two | Zero | Ninety-two thousand four hundred and thirty-six | One point two four | Fifteen point eight five | Five point six nine | Two point two one | Two | 圣约翰学院 | Twenty-seven thousand | one | six |
| One hundred and eighty-four | 行政帐户(account executive) | 阿拉伯联合酋长国 | Eighty-three thousand six hundred | Eighty-three thousand six hundred | Zero | Five million seven hundred and seventy-nine thousand seven hundred and sixty | Two point five eight | Fifteen point four three | One point nine seven | Twelve point three six | three | 阿布扎比 | Nine hundred and forty-two thousand | one | One hundred and eighty-four |
| One hundred and eighty-four | 行政帐户(account executive) | 阿拉伯联合酋长国 | Eighty-three thousand six hundred | Eighty-three thousand six hundred | Zero | Five million seven hundred and seventy-nine thousand seven hundred and sixty | Two point five eight | Fifteen point four three | One point nine seven | Twelve point three six | four | 迪拜 | One million nine hundred and seventy-eight thousand | Zero | One hundred and eighty-four |
| One hundred and eighty-four | 行政帐户(account executive) | 阿拉伯联合酋长国 | Eighty-three thousand six hundred | Eighty-three thousand six hundred | Zero | Five million seven hundred and seventy-nine thousand seven hundred and sixty | Two point five eight | Fifteen point four three | One point nine seven | Twelve point three six | five | 沙迦 | Nine hundred and eighty-three thousand | Zero | One hundred and eighty-four |

这个查询给出了两个表中的所有列，以及来自`facts`的`id`列和来自`cities`的`facts_id`列匹配的每一行，仅限于前 5 行。

## 了解 SQL 内部连接

我们现在已经连接了两个表，为我们提供了关于`cities`中每一行的额外信息。让我们仔细看看这个内部连接是如何工作的。

内部连接的工作方式是只包含每个表中使用`ON`子句指定的匹配行。让我们看一下上一个屏幕中的连接是如何工作的。我们选择了一些最能说明这种连接的行:

![inner join](img/81c25867b188237636fe2a9b7ed835ec.png)

我们的内部连接**将**包括:

*   来自`cities`表中具有与来自`facts`的`facts.id`相匹配的`cities.facts_id`的行。

我们的内部联接**不会**包括:

*   来自`cities`表的行，其`cities.facts_id`与事实中的任何`facts.id`都不匹配。
*   事实表中 facts.id 与城市中的任何`cities.facts_id`都不匹配的行。

你可以看到这表现为一个文氏图:

![Inner join Venn diagram](img/6851aad45e192a6478ae6b0513faffdd.png)

我们已经知道如何使用[别名](https://www.tutorialspoint.com/sqlite/sqlite_alias_syntax.htm)来指定列的自定义名称，例如:

```py
SELECT AVG(population) AS average_population
```

我们还可以为表名创建别名，这使得带有连接的查询更容易读写。而不是:

```py
SELECT * FROM facts
INNER JOIN cities ON cities.facts_id = facts.id
```

我们可以写:

```py
SELECT * FROM facts AS f
INNER JOIN cities AS c ON c.facts_id = f.id
```

就像列名一样，使用`AS`是可选的。我们可以通过书写得到相同的结果:

```py
SELECT * FROM facts f
INNER JOIN cities c ON c.facts_id = f.id
```

我们还可以将别名和通配符结合起来——例如，使用上面创建的别名，`c.*`将给出表`cities`中的所有列。

虽然上一个屏幕中的查询包含了来自`ON`子句的两列，但是在最终的列列表中，我们不需要使用来自`ON`子句的任何一列。这很有用，因为这意味着我们可以只显示我们感兴趣的信息，而不是每次都必须包含两个连接列。

让我们使用我们所学的知识来构建我们的原始查询。我们将:

*   使用`INNER JOIN`将`cities`连接到`facts`。
*   对表名使用别名。
*   依次包括:
    *   从`cities`开始的所有列。
    *   从`facts`到`country_name`的名称列别名。
*   只包括前 5 行。

```py
SELECT
    c.*,
    f.name country_name
FROM facts f
INNER JOIN cities c ON c.facts_id = f.id
LIMIT 5;
```

| 身份证明（identification） | 名字 | 人口 | 首都 | 事实 _id | 国家名称 |
| --- | --- | --- | --- | --- | --- |
| one | 奥兰耶斯塔德 | Thirty-seven thousand | one | Two hundred and sixteen | 阿鲁巴岛 |
| Two | 圣约翰学院 | Twenty-seven thousand | one | six | 安提瓜和巴布达 |
| three | 阿布扎比 | Nine hundred and forty-two thousand | one | One hundred and eighty-four | 阿拉伯联合酋长国 |
| four | 迪拜 | One million nine hundred and seventy-eight thousand | Zero | One hundred and eighty-four | 阿拉伯联合酋长国 |
| five | 沙迦 | Nine hundred and eighty-three thousand | Zero | One hundred and eighty-four | 阿拉伯联合酋长国 |

## 在 SQL 中练习内部连接

让我们练习编写一个查询，使用内部连接来回答数据库中的一个问题。假设我们想利用我们目前所学的知识，从数据库中生成一个国家及其首都的表格。我们的第一步是考虑在最终查询中需要哪些列。我们需要:

*   来自`facts`的`name`列
*   来自`cities`的`name`列

假设我们已经确定需要来自两个表的数据，我们需要考虑如何连接它们。前面的模式图表明，每个表中只有一列将它们链接在一起，因此我们可以使用这些列的内部连接来连接数据。

到目前为止，通过思考我们的问题，我们已经可以编写我们的大部分查询(它几乎与我们编写的前一个查询相同):

```py
SELECT
    f.name,
    c.name
FROM cities c
INNER JOIN facts f ON f.id = c.facts_id;
```

我们过程的最后一部分是确保我们有正确的行。从前面的两个屏幕中我们知道，这样的查询将返回来自`cities`的所有行，这些行在`facts_id`列中与来自`facts`的行有对应的匹配。我们只对城市表中的首都感兴趣，所以我们需要在`capital`列中使用一个`WHERE`子句，如果该城市是首都，则该子句的值为`1`，如果不是，则为`0`:

```py
WHERE c.capital = 1
```

我们现在可以将这些放在一起，编写一个查询来回答我们的问题。我们将把它限制在前 10 行，这样输出的数量是可管理的。

```py
SELECT
    f.name country,
    c.name capital_city
FROM cities c
INNER JOIN facts f ON f.id = c.facts_id
WHERE c.capital = 1
LIMIT 10;
```

| 国家 | 首都城市 |
| --- | --- |
| 阿鲁巴岛 | 奥兰耶斯塔德 |
| 安提瓜和巴布达 | 圣约翰学院 |
| 阿拉伯联合酋长国 | 阿布扎比 |
| 阿富汗 | 喀布尔 |
| 阿尔及利亚 | 阿尔及尔 |
| 阿塞拜疆 | 巴库 |
| 阿尔巴尼亚 | 地拉那 |
| 亚美尼亚 | 埃里温 |
| 安道尔 | Andorra La Vella |
| 安哥拉 | 罗安达 |

## SQL 中的左连接

正如我们前面提到的，内部连接将不包括两个表中没有相互匹配的任何行。这意味着，在我们的查询中，可能存在行不匹配的信息。

我们可以使用 SQL 查询来探索这一点:

```py
SELECT COUNT(DISTINCT(name)) FROM facts;
```

数数

| Two hundred and sixty-one |

```py
SELECT COUNT(DISTINCT(facts_id)) FROM cities;
```

数数

| Two hundred and ten |

通过运行这两个查询，我们可以看到在`facts`表中有一些国家在`cities`表中没有相应的城市，这表明我们可能有一些不完整的数据。

让我们看看如何使用一种新的连接类型——**左连接**,创建一个查询来探索缺失的数据。

左联接包括内部联接将选择的所有行，加上第一个(或左)表中在第二个表中没有匹配项的所有行。我们可以用文氏图来表示。

![Venn diagram left join](img/1dc3f9b0eb8d1b21c4e5ebc1470cc404.png)

让我们来看一个例子，用我们编写的第一个查询中的`LEFT JOIN`替换`INNER JOIN`，并从我们之前的图表中查看相同的行选择

```py
SELECT * FROM facts
LEFT JOIN cities ON cities.facts_id = facts.id
```

![left join](img/9c91bf46790f383cd9eb4cd6469706e8.png)

这里我们可以看到，对于`facts.id`与`cities.facts_id`中的任何值都不匹配的行(237、238、240 和 244)，这些行仍然包含在结果中。当这种情况发生时，`cities`表中的所有列都用空值填充。

我们可以使用这些空值过滤我们的结果，只过滤那些不存在于带有`WHERE`子句的`cities`中的国家。当在 SQL 中与 null 进行比较时，我们使用`IS`关键字，而不是`=`符号。如果我们想选择列为空的行，我们可以写:

```py
WHERE column_name IS NULL
```

如果我们想选择列名不为空的行，我们使用:

```py
WHERE column_name IS NOT NULL
```

让我们使用一个左连接来探索不存在于`cities`表中的国家。

```py
SELECT
    f.name country,
    f.population
FROM facts f
LEFT JOIN cities c ON c.facts_id = f.id
WHERE c.name IS NULL;
```

| 国家 | 人口 |
| --- | --- |
| 科索沃 | One million eight hundred and seventy thousand nine hundred and eighty-one |
| 摩纳哥 | Thirty thousand five hundred and thirty-five |
| 瑙鲁 | Nine thousand five hundred and forty |
| 圣马力诺 | Thirty-three thousand and twenty |
| 新加坡 | Five million six hundred and seventy-four thousand four hundred and seventy-two |
| 罗马教廷(梵蒂冈城) | Eight hundred and forty-two |
| 台湾 | Twenty-three million four hundred and fifteen thousand one hundred and twenty-six |
| 欧洲联盟 | Five hundred and thirteen million nine hundred and forty-nine thousand four hundred and forty-five |
| 阿什莫尔－卡捷群岛 |  |
| 圣诞岛 | One thousand five hundred and thirty |
| 科科斯(基林)群岛 | Five hundred and ninety-six |
| 珊瑚海群岛 |  |
| 赫德岛和麦克唐纳群岛 |  |
| 诺福克岛 | Two thousand two hundred and ten |
| 香港 | Seven million one hundred and forty-one thousand one hundred and six |
| 澳门 | Five hundred and ninety-two thousand seven hundred and thirty-one |
| 克利珀顿岛 |  |
| 法国南部和南极地区 |  |
| 圣巴特列米 | Seven thousand two hundred and thirty-seven |
| 圣马丁岛 | Thirty-one thousand seven hundred and fifty-four |
| 柑桂酒 | One hundred and forty-eight thousand four hundred and six |
| 你的尺寸 | Thirty-nine thousand six hundred and eighty-nine |
| 库克群岛 | Nine thousand eight hundred and thirty-eight |
| 纽埃 | One thousand one hundred and ninety |
| 托克劳群岛 | One thousand three hundred and thirty-seven |
| 布维岛 |  |
| Jan Mayen |  |
| 斯瓦尔巴群岛 | One thousand eight hundred and seventy-two |
| 阿克罗蒂里 | Fifteen thousand seven hundred |
| 英属印度洋领地 |  |
| Dhekelia | Fifteen thousand seven hundred |
| 直布罗陀 | Twenty-nine thousand two hundred and fifty-eight |
| 根西岛 | Sixty-six thousand and eighty |
| 泽西岛 | Ninety-seven thousand two hundred and ninety-four |
| 蒙特塞拉特岛 | Five thousand two hundred and forty-one |
| 皮特凯恩群岛 | Forty-eight |
| 南乔治亚岛和南桑威奇群岛 |  |
| 纳瓦拉岛 |  |
| 威克岛 |  |
| 美国太平洋岛屿野生动物保护区 |  |
| 南极洲 | Zero |
| 加沙地带 | One million eight hundred and sixty-nine thousand and fifty-five |
| 帕拉塞尔群岛 |  |
| “斯普拉特利群岛” |  |
| 西岸 | Two million seven hundred and eighty-five thousand three hundred and sixty-six |
| 北冰洋 |  |
| 大西洋 |  |
| 印度洋 |  |
| 太平洋 |  |
| 南冰洋 |  |
| 世界 | Seven billion two hundred and fifty-six million four hundred and ninety thousand and eleven |

浏览我们在上一个屏幕中编写的查询结果，我们可以看到国家/地区在`cities`中没有对应值的许多不同原因:

*   人口少和/或没有主要城市地区(定义为人口超过 750，000)的国家，如圣马力诺、科索沃和瑙鲁。
*   城邦，如摩纳哥和新加坡。
*   本身不是国家的领土，如香港、直布罗陀和库克群岛。
*   非国家的区域和海洋，如欧盟和太平洋。
*   真正缺失数据的案例，比如台湾。

无论何时使用内部联接，都要注意可能会排除重要的数据，尤其是当基于数据库模式中未链接的列进行联接时。

## 右连接和外连接

您应该知道 SQLite 不支持两种不太常见的连接类型。第一个是**右连接**。顾名思义，右连接与左连接正好相反。左连接包括表*中在*连接子句之前的所有行，而右连接包括新表*中在*连接子句中的所有行。我们可以在下面的文氏图中看到一个右连接:

![Venn diagram of a right join](img/567c9ac55bd43b5646f621d2c28d71df.png)

下面两个查询，一个使用左连接，一个使用右连接，产生相同的结果。

```py
SELECT f.name country, c.name city
FROM facts f
LEFT JOIN cities c ON c.facts_id = f.id
LIMIT 5;
```

```py
SELECT f.name country, c.name city
FROM cities c
RIGHT JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

使用右连接的主要原因是当您连接两个以上的表时。在这些情况下，最好使用右连接，因为它可以避免为了连接一个表而重新构造整个查询。除此之外，右连接很少使用，所以对于简单连接，使用左连接比右连接更好，因为其他人更容易阅读和理解您的查询。

SQLite 不支持的另一种连接类型是**完全外部连接**。完整的外部联接将包括联接两侧表中的所有行。我们可以在下面的维恩图中看到一个完整的外部连接:

![Venn diagram of a full outer join](img/5fde08b52e1c6de6ca5f8ea3f88b1902.png)

像右连接一样，完全外连接相当少见。完整外部连接的标准 SQL 语法是:

```py
SELECT f.name country, c.name city
FROM cities c
FULL OUTER JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

当用一个完整的外部连接连接`cities`和`facts`时，结果将与上面的左右连接相同，因为`cities.facts_id`中没有不存在于`facts.id`中的值。

让我们一起来看看每种连接类型的文氏图，这将有助于您比较到目前为止我们讨论的四种连接的不同之处。

![Join Venn Diagram](img/dd9cbfec9ecaf4db7d3f28df8a64cc16.png)

接下来，让我们练习使用连接来回答一些关于数据的问题。

## 寻找人口最多的首都城市

以前，我们在指定查询结果的顺序时使用了列名，如下所示:

```py
SELECT
    name,
    migration_rate
FROM facts
ORDER BY migration_rate desc;
```

我们可以在查询中使用一个方便的快捷方式，让我们跳过列名，而是使用列在`SELECT`子句中出现的顺序。在这个实例中，`migration_rate`是我们的`SELECT`子句中的第二列，所以我们可以只使用`2`来代替列名:

```py
SELECT
    name,
    migration_rate
FROM facts
ORDER BY 2 desc;
```

您可以在`ORDER BY`或`GROUP BY`子句中使用此快捷方式。请注意，您希望确保查询仍然可读，因此对于更复杂的查询，键入完整的列名可能更好。

让我们用我们所学的来列出人口最多的 10 个首都城市。因为我们对来自`facts`的在`cities`中没有对应城市的国家不感兴趣，所以我们应该使用一个`INNER JOIN`。

```py
SELECT
    c.name capital_city,
    f.name country,
    c.population
FROM facts f
INNER JOIN cities c ON c.facts_id = f.id
WHERE c.capital = 1
ORDER BY 3 DESC
LIMIT 10;
```

| 首都城市 | 国家 | 人口 |
| --- | --- | --- |
| 东京 | 日本 | Thirty-seven million two hundred and seventeen thousand |
| 新德里 | 印度 | Twenty-two million six hundred and fifty-four thousand |
| 墨西哥城 | 墨西哥 | Twenty million four hundred and forty-six thousand |
| 北京 | 中国 | Fifteen million five hundred and ninety-four thousand |
| 达卡 | 孟加拉国 | Fifteen million three hundred and ninety-one thousand |
| 布宜诺斯艾利斯 | 阿根廷 | Thirteen million five hundred and twenty-eight thousand |
| 马尼拉 | 菲律宾 | Eleven million eight hundred and sixty-two thousand |
| 莫斯科 | 俄罗斯 | Eleven million six hundred and twenty-one thousand |
| 开罗 | 埃及 | Eleven million one hundred and sixty-nine thousand |
| 雅加达 | 印度尼西亚 | Nine million seven hundred and sixty-nine thousand |

## 将 SQL 连接与子查询相结合

子查询可以用来替代部分查询，使我们能够找到更复杂问题的答案。我们还可以连接到子查询的结果，就像我们连接表一样。

这是一个使用联接和子查询来生成国家及其首都的表的示例，就像我们在本课前面所做的那样。

![subqueries](img/63ae850f555b53969f6aea5b32824aea.png)

起初，读取子查询可能会让人不知所措，所以我们将把本例中发生的事情分成几个步骤。要记住的重要一点是，任何子查询的结果总是首先计算的，所以我们从里到外读取。

*   首先计算红框中的子查询。这个简单的查询选择来自`cities`的所有列，通过将`capital`的值设为 1 来过滤标记为首都城市的行。
*   `INNER JOIN`基于`ON`子句将别名为`c`的子查询结果连接到`facts`表。
*   从连接的结果中选择了两列:
    *   `f.name`，别名为`country`。
    *   `c.name`，别名为`capital_city`。
*   结果限于前 10 行。

以下是该查询的输出:

| 国家 | 首都城市 |
| --- | --- |
| 阿鲁巴岛 | 奥兰耶斯塔德 |
| 安提瓜和巴布达 | 圣约翰学院 |
| 阿拉伯联合酋长国 | 阿布扎比 |
| 阿富汗 | 喀布尔 |
| 阿尔及利亚 | 阿尔及尔 |
| 阿塞拜疆 | 巴库 |
| 阿尔巴尼亚 | 地拉那 |
| 亚美尼亚 | 埃里温 |
| 安道尔 | Andorra La Vella |
| 安哥拉 | 罗安达 |

使用这个示例作为模型，我们将编写一个类似的查询来查找人口超过 1000 万的非首都城市。

```py
SELECT
    c.name city,
    f.name country,
    c.population population
FROM facts f
INNER JOIN (
            SELECT * FROM cities
            WHERE capital = 0
            AND population > 10000000
           ) c ON c.facts_id = f.id
ORDER BY 3 DESC;
```

| 城市 | 国家 | 人口 |
| --- | --- | --- |
| 纽约-纽瓦克 | 美国 | Twenty million three hundred and fifty-two thousand |
| 上海 | 中国 | Twenty million two hundred and eight thousand |
| 圣保罗(葡萄牙) | 巴西 | Nineteen million nine hundred and twenty-four thousand |
| 孟买 | 印度 | Nineteen million seven hundred and forty-four thousand |
| 马赛-艾克斯-普罗旺斯 | 法国 | Fourteen million eight hundred and ninety thousand one hundred |
| 加尔各答 | 印度 | Fourteen million four hundred and two thousand |
| 卡拉奇 | 巴基斯坦 | Thirteen million eight hundred and seventy-six thousand |
| 洛杉矶-长滩-圣安娜 | 美国 | Thirteen million three hundred and ninety-five thousand |
| 大阪-神户 | 日本 | Eleven million four hundred and ninety-four thousand |
| 伊斯坦布尔 | 火鸡 | Eleven million two hundred and fifty-three thousand |
| 拉各斯 | 尼日利亚 | Eleven million two hundred and twenty-three thousand |
| 广州 | 中国 | Ten million eight hundred and forty-nine thousand |

## SQL 挑战:包含连接和子查询的复杂查询

让我们把之前学过的东西拿来，用它来编写一个更复杂的查询。“用 SQL 思考”需要一点时间来适应，这并不罕见，所以如果这个查询一开始看起来很难理解，也不要气馁。通过练习会变得更容易！

当您编写包含连接和子查询的复杂查询时，遵循以下过程会有所帮助:

*   考虑在最终输出中需要哪些数据
*   确定需要连接哪些表，以及是否需要连接子查询。
    *   如果需要联接到子查询，请先编写子查询。
*   然后开始编写 SELECT 子句，接着是 join 子句和您需要的任何其他子句。
*   不要害怕一步一步地编写查询，边编写边运行—例如，在编写外部查询之前，您可以先将子查询作为“独立”查询运行，以确保它看起来像您想要的那样。

我们将编写一个查询来查找城市中心(城市)人口超过国家总人口一半的国家。有多种方法可以编写这个查询，但是我们将逐步介绍一种方法。

我们可以从编写一个查询开始，对每个国家的所有城市人口进行求和。通过对`facts_id`进行分组，我们可以在没有连接的情况下做到这一点(在下面的例子中，我们将使用一个限制来使输出可管理):

```py
SELECT
    facts_id,
    SUM(population) urban_pop
FROM cities
GROUP BY 1
LIMIT 5;
```

| 事实 _id | 都市流行 |
| --- | --- |
| one | Three million and ninety-seven thousand |
| Ten | One hundred and seventy-two thousand |
| One hundred | One million one hundred and twenty-seven thousand |
| One hundred and one | Five thousand |
| One hundred and two | Five hundred and forty-six thousand |

接下来，我们将把`facts`表连接到该子查询，选择国家名称、城市人口和总人口(同样，为了保持整洁，我们使用了一个限制):

```py
SELECT
    f.name country,
    c.urban_pop,
    f.population total_pop
FROM facts f
INNER JOIN (
            SELECT
                facts_id,
                SUM(population) urban_pop
            FROM cities
            GROUP BY 1
           ) c ON c.facts_id = f.id
LIMIT 5;
```

| 国家 | 都市流行 | 总计 _pop |
| --- | --- | --- |
| 阿富汗 | Three million and ninety-seven thousand | Thirty-two million five hundred and sixty-four thousand three hundred and forty-two |
| 奥地利 | One hundred and seventy-two thousand | Eight million six hundred and sixty-five thousand five hundred and fifty |
| 利比亚 | One million one hundred and twenty-seven thousand | Six million four hundred and eleven thousand seven hundred and seventy-six |
| 列支敦士登 | Five thousand | Thirty-seven thousand six hundred and twenty-four |
| 立陶宛 | Five hundred and forty-six thousand | Two million eight hundred and eighty-four thousand four hundred and thirty-three |

最后，我们将创建一个新列，将城市人口除以总人口，并使用`WHERE`和`ORDER BY`对结果进行过滤/排序:

```py
SELECT
    f.name country,
    c.urban_pop,
    f.population total_pop
FROM facts f
INNER JOIN (
            SELECT
                facts_id,
                SUM(population) urban_pop
            FROM cities
            GROUP BY 1
           ) c ON c.facts_id = f.id
LIMIT 5;
```

| 国家 | 都市流行 | 总计 _pop | 城市百分比 |
| --- | --- | --- | --- |
| 乌拉圭 | One million six hundred and seventy-two thousand | Three million three hundred and forty-one thousand eight hundred and ninety-three | 0.500315 |
| 刚果共和国 | Two million four hundred and forty-five thousand | Four million seven hundred and fifty-five thousand and ninety-seven | 0.514185 |
| 文莱 | Two hundred and forty-one thousand | Four hundred and twenty-nine thousand six hundred and forty-six | 0.560927 |
| 新喀里多尼亚 | One hundred and fifty-seven thousand | Two hundred and seventy-one thousand six hundred and fifteen | 0.578024 |
| 维尔京群岛 | Sixty thousand | One hundred and three thousand five hundred and seventy-four | 0.579296 |
| 福克兰群岛(伊斯拉斯·福克兰) | Two thousand | Three thousand three hundred and sixty-one | 0.595061 |
| 吉布提 | Four hundred and ninety-six thousand | Eight hundred and twenty-eight thousand three hundred and twenty-four | 0.598800 |
| 澳大利亚 | Thirteen million seven hundred and eighty-nine thousand | Twenty-two million seven hundred and fifty-one thousand and fourteen | 0.606083 |
| 冰岛 | Two hundred and six thousand | Three hundred and thirty-one thousand nine hundred and eighteen | 0.620635 |
| 以色列 | Five million two hundred and twenty-six thousand | Eight million forty-nine thousand three hundred and fourteen | 0.649248 |
| 阿拉伯联合酋长国 | Three million nine hundred and three thousand | Five million seven hundred and seventy-nine thousand seven hundred and sixty | 0.675288 |
| 波多黎各 | Two million four hundred and seventy-five thousand | Three million five hundred and ninety-eight thousand three hundred and fifty-seven | 0.687814 |
| 巴哈马群岛 | Two hundred and fifty-four thousand | Three hundred and twenty-four thousand five hundred and ninety-seven | 0.782509 |
| 科威特 | Two million four hundred and six thousand | Two million seven hundred and eighty-eight thousand five hundred and thirty-four | 0.862819 |
| 圣皮埃尔和密克隆 | Five thousand | Five thousand six hundred and fifty-seven | 0.883861 |
| 关岛 | One hundred and sixty-nine thousand | One hundred and sixty-one thousand seven hundred and eighty-five | 1.044596 |
| 北马里亚纳群岛 | Fifty-six thousand | Fifty-two thousand three hundred and forty-four | 1.069846 |
| 美属萨摩亚群岛 | Sixty-four thousand | Fifty-four thousand three hundred and forty-three | 1.177705 |

您可以看到，虽然我们的最终查询*很复杂，但是如果您一步一步地构建它，就会更容易理解。*

## SQL 连接教程:后续步骤

在本 sql 连接教程中，我们学习了:

*   内连接和左连接的区别。
*   右连接和外连接的作用
*   如何选择适合您的任务的联接。
*   使用带有子查询、聚合函数和其他 SQL 技术的连接。

您可能感兴趣的其他资源包括我们的 [SQL 备忘单](https://www.dataquest.io/blog/sql-cheat-sheet/)，我们的[关于 SQL 认证的文章](https://www.dataquest.io/blog/sql-certification/)，我们的，当然还有我们的交互式 SQL 课程。点击下面注册并免费开始！

### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)**