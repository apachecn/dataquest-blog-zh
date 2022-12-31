# Postgres 内部:构建描述工具

> 原文：<https://www.dataquest.io/blog/postgres-internals/>

January 10, 2018

在之前的[博客](https://www.dataquest.io/blog/sql-intermediate/) [帖子](https://www.dataquest.io/blog/loading-data-into-postgres/)中，我们已经描述了 Postgres 数据库以及使用 Python 与它交互的方式。这些帖子提供了基础知识，但是如果您想在生产系统中使用数据库，那么有必要知道如何使查询更快、更有效。要理解效率在 Postgres 中意味着什么，了解 Postgres 如何在幕后工作是很重要的。在这篇文章中，我们将关注 Postgres 和关系数据库的更高级的概念。首先，我们将学习 Postgres 如何存储自己的内部数据来描述、调试和识别系统中的瓶颈。然后，我们将使用我们对 Postgres 内部数据的了解来构建我们自己版本的 Python 数据库描述工具。像我们之前的博文一样，我们将使用以下工具:

*   Postgres 的本地版本(v9.2 或更高版本)
*   python3
*   Postgres 的 Python 驱动程序，`psycopg2`

我们将使用的数据集来自美国的[住房和城市发展部](https://www.hud.gov/)——也被称为 **HUD** 。我们已经将文件打包到一个包含 CSV 格式数据的 [zip 文件](https://dq-content.s3.amazonaws.com/blog/dq_postgres_internals.zip)和一个 Python 3 脚本(`load_hud_tables.py`)中，该脚本将 CSV 复制到本地运行的 Postgres 中。如果您在没有默认连接的情况下运行 Postgres 服务器，您将需要更新脚本中的连接字符串。使用 HUD 的数据集，我们将使用每个 Postgres 引擎中可用的命令来处理真实世界的数据示例。从空白开始，我们将研究这些表及其数据类型。然后，我们将使用**内部 Postgres 表**来探索 HUD 表，以给我们关于数据库内容的详细描述。首先，下载并解压`dq_postgres_internals.zip` [文件](https://dq-content.s3.amazonaws.com/blog/dq_postgres_internals.zip)。进入`dq_postgres_internals/`目录，更改`load_hud_tables.py`中的连接参数，然后运行脚本。这将把 CSV 文件加载到本地 Postgres 实例中。一旦您将文件加载到 Postgres 服务器上，我们就可以开始连接数据库了。如果本地 Postgres 实例不同于默认值，请更改连接值。在这篇文章中，任何未来对`cur`对象的引用都将是以下连接的光标:

```
import psyocpg2conn = psycopg2.connect(host="127.0.0.1", dbname="postgres", user="postgres", password="password")
cur = conn.cursor()
```

## 调查表格

现在我们已经连接好了，是时候开始探索我们有哪些可用的牌桌了。首先，我们将检查 Postgres 内部表，这些表为我们提供了关于数据库的详细信息。让我们从描述这些内部表开始。在每一个 Postgres 引擎中，都有一组 Postgres 用来管理其整个结构的内部表。这些表位于 Postgres 文档中，作为组 [**信息 _ 模式**](https://www.postgresql.org/docs/9.1/static/information-schema.html) 和 [**系统目录**](https://www.postgresql.org/docs/current/static/catalogs-overview.html) 。这些包含了 Postgres 数据库中存储的关于数据、表名和类型的所有信息。例如，当我们使用属性`cur.description`时，它从内部表中提取信息显示给用户。不幸的是，没有数据集的详细模式。因此，我们需要对所包含的内容创建自己的详细描述。让我们使用来自`information_schema`的一个内部表来获得数据库中存储了哪些表的高级概述。内部表名为[information _ schema . tables](https://www.postgresql.org/docs/9.1/static/infoschema-tables.html)，从文档中我们可以看到有大量的列可供选择:

| 名字 | 数据类型 | 描述 |
| 表格 _ 目录 | sql _ 标识符 | 包含该表的数据库的名称(总是当前数据库) |
| 表模式 | sql _ 标识符 | 包含该表的架构的名称 |
| 表名 | sql _ 标识符 | 表的名称 |
| 表格类型 | 字符 _ 数据 | 表的类型:永久基表的基表(普通表类型)、视图的视图、外部表的外部表或临时表的本地临时表 |
| 自引用列名 | sql _ 标识符 | 适用于 PostgreSQL 中不可用的功能 |
| 参考 _ 生成 | 字符 _ 数据 | 适用于 PostgreSQL 中不可用的功能 |
| 用户定义的类型目录 | sql _ 标识符 | 如果该表是类型化表，则为包含基础数据类型的数据库的名称(始终为当前数据库)，否则为 null。 |
| 用户定义类型模式 | sql _ 标识符 | 如果表是类型化表，则为包含基础数据类型的架构的名称，否则为 null。 |
| 用户定义类型名称 | sql _ 标识符 | 如果表是类型化表，则为基础数据类型的名称，否则为 null。 |
| is _ insertable _ into | 是或否 | 如果表是可插入的，则为“是”,否则为“否”(基表总是可插入的，视图不一定。) |
| 是类型化的 | 是或否 | 如果该表是类型化表，则为“是”,否则为“否” |
| 提交 _ 操作 | 字符 _ 数据 | 如果该表是临时表，则 PRESERVE，否则为 null。(SQL 标准为临时表定义了其他提交操作，PostgreSQL 不支持这些操作。) |

此时，我们只关心数据库中的表名。看一下上面的表描述，有一个列`table_name`公开了这些信息。让我们查询该列，看看我们在处理什么。

```
cur.execute("SELECT table_name FROM information_schema.tables ORDER BY table_name")
table_names = cur.fetchall()
for name in table_names:
    print(name)
```

## 使用模式

当您运行前面的命令时，您会注意到输出包含了大量的表。这些表中的每一个都可以在`postgres`数据库中找到。问题是:我们在结果中包含了内部表。在输出中，您会注意到许多表格以前缀`pg_*`开始。这些表中的每一个都是内部表的`pg_catalog`组的一部分。这些是我们之前描述的*系统目录*表。然后，你可能已经猜到了，在`information_schema`名下还有一组其他的表。但是，这些表很难找到，因为它们没有明显的前缀模式。我们如何知道哪些表是内部的，哪些表是用户创建的？我们需要描述**模式**来解释上面的问题。在处理关系数据库时，模式这个词被概括为一个具有多重含义的术语。您可能听说过模式被用来描述表(它们的数据类型、名称、列等)。)或模式作为数据库的**蓝图**。图式的模糊含义只会使我们困惑。然而，对于 Postgres 来说，图式这个术语是为一个特定的目的而保留的。在 Postgres 中，模式被用作表的*名称空间*，其独特的目的是将它们分成单个数据库中的**隔离组**或**集合**。

![database_schemas](img/b02bf9be870851c6af5e684d87aaf0ab.png)

让我们进一步分析一下。Postgres 使用数据库的概念来分离 Postgres 服务器中的用户和数据。当您创建数据库时，您正在创建一个隔离的环境，在该环境中，用户可以查询只能在该特定数据库中找到的表。这里有一个例子:假设国土安全部(DHS)和 HUD 共享同一个政府 Postgres 数据库，但是他们想要分离他们的用户和数据。然后，他们将使用*数据库*来分离他们的数据和用户，每个机构有一个单独的数据库。现在，当用户想要连接到他们的数据时，他们需要指定他们将连接到哪个数据库，并且只有在那里，他们才能使用他们的表。然而，假设有分析师想要对公民数据(`citizens`表)和城市住房开发(`developments`表)进行横截面分析。那么，他们会想要查询`dhs`数据库和`hud`数据库中的表。然而，在 Postgres 考试中，这是不可能的。

```
 # Connect to the `dhs` database.
conn = psycopg2.connect(dbname='dhs')
cur = conn.cursor()
# This query works.
cur.execute('SELECT * FROM citizens') 
# This query will fail because it is in the `hud` database.
cur.execute('SELECT * FROM developments')
```

如果我们想把表分成不同的组，但仍然允许跨表查询，那该怎么办？这是模式的完美用例。代替数据库，为每个机构使用不同的模式将使用名称空间来分隔表，但是仍然允许分析师查询两个表。

```
 # Connect to the US Government database.
conn = psycopg2.connect(dbname='us_govt_data')
cur = conn.cursor()
# Using schemas, both queries work!
cur.execute('SELECT * FROM dhs.citizens')
cur.execute('SELECT * FROM hud.developments') 
```

这就是 Postgres 如何划分内部表(以及用户创建的表！).当创建一个数据库时，有 3 个模式被实例化:`pg_catalog`(对于系统编目表)、`information_schema`(对于信息模式表)和`public`(对于用户创建的表的默认模式)。每次在数据库中发出`CREATE TABLE`命令时，默认情况下 Postgres 会将该表分配给`public`模式。现在，回到我们之前的问题，我们如何将用户创建的表与内部表分开？再看一下`information_schema.tables`的栏目。现在，检查是否存在可以将用户创建的表与内部表分开的列。

| 名字 | 数据类型 | 描述 |
| 表格 _ 目录 | sql _ 标识符 | 包含该表的数据库的名称(总是当前数据库) |
| 表模式 | sql _ 标识符 | 包含该表的架构的名称 |
| 表名 | sql _ 标识符 | 表的名称 |
| 表格类型 | 字符 _ 数据 | 表的类型:永久基表的基表(普通表类型)、视图的视图、外部表的外部表或临时表的本地临时表 |
| 自引用列名 | sql _ 标识符 | 适用于 PostgreSQL 中不可用的功能 |
| 参考 _ 生成 | 字符 _ 数据 | 适用于 PostgreSQL 中不可用的功能 |
| 用户定义的类型目录 | sql _ 标识符 | 如果该表是类型化表，则为包含基础数据类型的数据库的名称(始终为当前数据库)，否则为 null。 |
| 用户定义类型模式 | sql _ 标识符 | 如果表是类型化表，则为包含基础数据类型的架构的名称，否则为 null。 |
| 用户定义类型名称 | sql _ 标识符 | 如果表是类型化表，则为基础数据类型的名称，否则为 null。 |
| is _ insertable _ into | 是或否 | 如果表是可插入的，则为“是”,否则为“否”(基表总是可插入的，视图不一定。) |
| 是类型化的 | 是或否 | 如果该表是类型化表，则为“是”,否则为“否” |
| 提交 _ 操作 | 字符 _ 数据 | 如果该表是临时表，则 PRESERVE，否则为 null。(SQL 标准为临时表定义了其他提交操作，PostgreSQL 不支持这些操作。) |

有一个名为`table_schema`的栏目符合我们的要求。我们可以对该列进行筛选，以选择所有公共表。我们可以这样编写查询:

```
 conn = psycopg2.connect(dbname="dq", user="hud_admin", password="eRqg123EEkl")
cur = conn.cursor()
cur.execute("SELECT table_name FROM information_schema.tables WHERE table_schema='public' ORDER BY table_name")
for table_name in cur.fetchall():
    name = table_name[0]
    print(name)
```

## 描述表格

从输出来看，我们将只使用三个表。这些是:

```
homeless_by_coc
state_info
state_household_incomes
```

使用表名，我们可以调用`cur.description`属性来详细查看每个表的列、类型和任何其他元信息。之前，我们了解到可以发出一个`SELECT`查询，然后调用`description`属性来获取表信息。然而，如果我们想在`for`循环中为每个表做这件事呢？希望你的第一个想法是**而不是**使用`.format()`。在之前的帖子中，我们已经提到了关于使用`.format()`进行字符串插值的问题。答案是使用`mogrify()`方法或`execute()`方法中的第二个位置参数来 *mogirfy* 字符串。可惜事情没那么容易。试图插入表名(使用`mogirfy()`)而不是列名、筛选键或分组键会导致错误。以下是此错误的一个示例:

```
table_name = "state_info"
bad_interpolation = cur.mogrify("SELECT * FROM %s LIMIT 0", [table_name])
# This will execute the query: SELECT * FROM 'state_info' LIMIT 0
# Notice the single quotation marks around state_info.
cur.execute(bad_interpolation)  
# Throws an error
```

从代码片段中，您可能已经注意到，在表名`"state_info"`上使用`mogrify()`会将名称转换为 Postgres 字符串。这是列名或筛选查询所必需的行为，但不是表名。相反，你必须使用来自 [`psycopg2.extensions`模块的一个名为`AsIs`](http://initd.org/psycopg/docs/extensions.html#psycopg2.extensions.AsIs) 的类。

```
 from psycopg2.extensions import AsIs
table_name = "state_info"
proper_interpolation = cur.mogrify("SELECT * FROM %s LIMIT 0", [AsIs(table_name)])
cur.execute(proper_interpolation)  
# Executes!
```

`SELECT`查询中的表名不需要用字符串括起来。因此，`AsIs`将其保持为非字符串引用的有效 SQL 表示，而不是转换它。使用`AsIs`，我们可以检查每个表的描述，而不必写出每个请求！

```
 from psycopg2.extensions import AsIs
cur.execute("SELECT table_name 
FROM information_schema.tables WHERE table_schema='public'")
for table in cur.fetchall():
    table = table[0]
    cur.execute("SELECT * FROM %s LIMIT 0", [AsIs(table)])
    print(cur.description, "\n") 
```

## 类型代码映射

随着每个描述的打印出来，我们现在有了一个我们将要使用的表的详细视图。下面是来自`homeless_by_coc`描述的输出片段:

```
(Column(name='id', type_code=23, display_size=None, internal_size=4, precision=None, scale=None, null_ok=None), Column(name='year', type_code=1082, display_size=None, internal_size=4, precision=None, scale=None, null_ok=None), Column(name='state', type_code=1042, display_size=None, internal_size=2, precision=None, scale=None, null_ok=None), Column(name='coc_number', type_code=1042, display_size=None, internal_size=8, precision=None, scale=None, null_ok=None), Column(name='coc_name', type_code=1043, display_size=None, internal_size=128, precision=None, scale=None, null_ok=None), Column(name='measures', type_code=1043, display_size=None, internal_size=64, precision=None, scale=None, null_ok=None), Column(name='count', type_code=23, display_size=None, internal_size=4, precision=None, scale=None, null_ok=None))
```

理解了`description`属性，您应该对可用的元数据感到满意。然而，我们再次面对一个整数`type_code`，而不是人类可读的类型。当记住它们所代表的人类可读类型(即`TEXT`、`INTEGER`或`BOOLEAN`。您可以使用`psycopg2`类型值来查找每一列的近似类型，但是我们可以做得比近似这些值更好。使用内部表格，我们可以准确地映射 HUD 表格中每一列的类型。我们将使用的内部表来自系统编目模式`pg_catalog`，它被准确地命名为`pg_type`。我们建议查看文档中的表描述，因为本节中要添加的行太多了。你可以在这里找到表格描述[。在这个表中，有许多已定义的列，其中许多您不需要关心。然而，关于这个表需要注意的一件有趣的事情是，它可以用来从头开始创建自己的 Postgres 类型。例如，使用这个表，您可以创建一个`HEX`类型，用于在您的列中只存储十六进制字符。让我们遍历返回的`SELECT`查询，并将整数类型代码映射到字符串。它应该看起来像这样:](https://www.postgresql.org/docs/9.6/static/catalog-pg-type.html)

```
 type_mappings = {
    16: 'bool',
    18: 'char',
    19: 'name',
    ...
}
```

利用字典理解，我们可以写出以下内容:

```
 cur.execute("SELECT oid, typname 
FROM pg_catalog.pg_type")
type_mappings = {
    int(oid): typname
    for oid, typname in cur.fetchall()}
```

## 可读描述类型

太好了！现在我们有了所有类型代码到它们的类型名的映射。使用`type_mappings`字典，无需在文档中查找就可以提供类型。让我们把所有这些放在一起，创建我们自己的表描述。我们希望将元组列表中的`description`属性重写为人类可读的内容。我们希望将之前练习的结果汇集到这本词典中:

```
 {
    "homeless_by_coc":
        {
            columns: [
                {
                    name: "id",
                    type: "int4",
                    internal_size: 4
                },
                {
                    name: "year",
                    type: "date",
                    internal_size: 4
                },
                {
                    name: "state",
                    type: "char",
                    internal_size: 2
                },
                {
                    name: "coc_number",
                    type: "char",
                    internal_size: 128
                },
                {
                    name: "measures",
                    type: "varchar",
                    internal_size: 64
                 },
                {
                    name: "count",
                    type: "int4",
                    internal_size: 4
                }
            ]
                    }
    ...
}
```

使用`type_mappings`和`table_names`，我们将获得所需结果的步骤如下:

1.  用`table`变量循环通过`table_names`。
2.  获取给定`table`的`description`属性。
3.  用一个`columns`键将`table`的名称映射到一个字典。
4.  通过遍历`description`并映射适当的类型，从屏幕示例中重新创建`columns`列表。

```
cur.execute("SELECT oid, typname FROM pg_catalog.pg_type")
type_mappings = {
    int(oid): typname
    for oid, typname in cur.fetchall()}
cur.execute("SELECT table_name FROM information_schema.tables WHERE table_schema='public'")
table_names = [table[0] for table in cur.fetchall()]
readable_description = {}
for table in table_names:
    cur.execute("SELECT * FROM %s LIMIT 0", [AsIs(table)])
    readable_description[table] = dict(
        columns=[
            dict(
                name=col.name,
                type=type_mappings[col.type_code],
                length=col.internal_size
            )
            for col in cur.description
        ]    )print(readable_description)
```

## 行数

事情开始有头绪了。现在，为了完成我们的调查，我们想提供一些关于表中行的附加信息。让我们用表中的行数来提供我们的描述。我们可以使用`COUNT()`聚合函数找到行数。这与 SQLite 的 aggreggate 函数以及 SQL 语法的其他实现非常相似。如果你想进一步了解 Postgres 的聚合函数，它们都在 [`pg_catalog.pg_aggregate`内部表](https://www.postgresql.org/docs/9.4/static/catalog-pg-aggregate.html)中定义。提醒一下，下面是如何使用 Postgres 中的`COUNT()`函数:

```
SELECT COUNT(*) FROM example_table
```

我们希望描述表看起来像这样:

```
{
    "homeless_by_coc":
        {
            columns: [
                {
                    name: "id",
                    type: "int4",
                    internal_size: 4
                },
                ...
                {
                    name: "count",
                    type: "int4",
                    internal_size: 4
                }
            ],
            total: 86529
        }
    ...
}
```

我们不再遍历`table_names`列表，而是遍历`readable_description`字典键:

```
 for table in readable_description.keys():
    cur.execute("SELECT COUNT(*) FROM %s", [AsIs(table)])
    readable_description[table]["total"] = cur.fetchone()
```

## 样本行

最后，让我们在`readable_description`字典中添加一些示例行。由于`homeless_by_coc`表有很多行，我们应该为每个查询增加一个限制。即使您添加了比可用行数更高的限制，查询仍会执行。

```
SELECT * FROM example_table LIMIT 100
```

我们将在检索计数的同一个循环中添加限制查询。我们可以在同一个循环中执行这两个操作，而不是在键上迭代两次。不过要谨慎`cur.fetchall()`的调用顺序。如果我们不能立即获取查询结果，我们可以覆盖查询执行。`cur.execute()`命令不返回读取结果，请求结果是用户的责任。例如，下面的查询将只返回`LIMIT`的结果，而不返回`COUNT`的结果:

```
 cur.execute("SELECT COUNT(*) FROM homeless_by_coc")
cur.execute("SELECT * FROM homless_by_coc LIMIT 100")
# Calling .fetchall() will only return the rows in the LIMIT query.
print(cur.fetchall())
```

让我们将这两个查询添加到一个代码块中:

```
 for table in readable_description.keys():
    cur.execute("SELECT COUNT(*) FROM %s", [AsIs(table)])
    readable_description[table]["total"] = cur.fetchone()
    cur.execute("SELECT * FROM %s LIMIT 100", [AsIs(table)])
    readable_description[table]["sample_rows"] = cur.fetchall()
```

综上所述，我们现在有了一个通用脚本，它将返回一个人类可读的字典，其中包含数据库中所有用户创建的表。

```
 import psyocpg2 
from psyocpg2.extensions import AsIs
conn = psycopg2.connect(host="127.0.0.1", dbname="postgres", user="postgres", password="password")
cur = conn.cursor()
cur.execute("SELECT oid, typname FROM pg_catalog.pg_type")
type_mappings = {
    int(oid): typname
    for oid, typname in cur.fetchall()}
cur.execute("SELECT table_name FROM information_schema.tables WHERE table_schema='public'")
table_names = [table[0] for table in cur.fetchall()]
readable_description = {}
for table in table_names:
    cur.execute("SELECT * FROM %s LIMIT 0", [AsIs(table)])
    readable_description[table] = dict(
        columns=[
            dict(
                name=col.name,
                type=type_mappings[col.type_code],
                length=col.internal_size
            )
            for col in cur.description
        ]
    ) 
```

## 后续步骤

在这篇文章中，我们从对数据库及其表一无所知，到对我们将要使用的表进行人类可读的描述。首先，我们学习了 Postgres 的内部表和模式背后的概念。然后，我们应用我们的知识从零开始建立我们自己的描述字典。这篇文章改编自我们的数据工程课程。本课是课程[优化 Postgres 数据库](https://www.dataquest.io/course/optimizing-postgres-databases-data-engineering)的一部分，我们将扩展 Postgres 内部表的知识，以优化 HUD 表及其查询。本课程侧重于解决您将在生产级数据分析系统中遇到的真实场景。