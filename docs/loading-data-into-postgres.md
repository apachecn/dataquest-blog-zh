# 教程:使用 Python 和 CSV 将数据加载到 Postgres

> 原文：<https://www.dataquest.io/blog/loading-data-into-postgres/>

September 30, 2017![tutorial-sql-python-postgres](img/b304a62788cc56bf83d23b490acd5dc8.png)

## Python Postgres 简介

数据存储是数据系统最重要的组成部分之一。你会在网上找到数百个 SQL 教程，详细介绍如何编写疯狂的 SQL 分析查询，如何在数 Pb 的训练数据上运行复杂的机器学习算法，以及如何在数据库的数千行上建立统计模型。唯一的问题是:没有人首先提到你如何获得存储的数据。

无论您是数据分析师、数据科学家、数据工程师，甚至是 web 开发人员，知道如何存储和访问您的数据都是非常重要的。在这篇博客文章中，我们将关注一种叫做关系数据库的数据存储。关系数据库是用于 web 内容、大型商业存储以及最相关的数据平台的最常见存储。

具体来说，我们将重点关注 [**Postgres** (或 PostgreSQL)](https://www.postgresql.org/) ，最大的开源关系数据库之一。我们喜欢 Postgres 是因为它的高稳定性，在云提供商(AWS，Google Cloud 等)中的易用性，以及它是开源的事实！使用 Python 库`psycopg2`，我们将通过一个例子演示如何从头开始创建自己的表，然后将数据集加载到本地运行的 Postgres 服务器中。

在我们开始之前，您应该确保已经安装了必要的工具。最重要的是在你的电脑上安装一个 Postgres 的本地版本。Postgres wiki 有一个[安装页面](https://wiki.postgresql.org/wiki/Detailed_installation_guides)，上面有关于最流行的操作系统的指南。

接下来，您应该确保安装了`psycopg2`库。如果没有，您可以运行:

```py
pip install psycopg2
```

在我们的代码示例中，我们将在 Mac 或 Linux 操作系统上使用 Python 版。如果您运行的是 2.7 版或在 Windows 机器上，命令应该仍然是相似的。设置好一切后，让我们开始连接本地 Postgres 服务器吧！

## Postgres 和客户机-服务器模型

如果您过去曾经使用过 SQL 引擎，那么您很可能已经了解过 [SQLite](https://www.sqlite.org/) ，因为它是最常见的数据库引擎之一。您的所有数据都保存在一个文件中，使其易于移植和开发。此外，SQLite 包含大部分用于大型引擎的 SQL 命令——因此，如果您了解 SQLite，那么您就会了解所有其他 SQL 数据库的基础知识

然而，在数据生产系统中使用 SQLite 也有缺点。由于其简单的用例，SQLite 不是为多连接而构建的。SQLite 只允许单个进程写入数据库，因此很难与多个用户和服务共享。

另一方面，Postgres 是一个更加健壮的引擎，它被实现为一个**服务器**，而不是一个单独的文件。作为服务器，Postgres 接受来自**客户端**的连接，客户端**可以**请求`SELECT`、`INSERT`或任何其他类型的 SQL 查询。这种类型的设计被称为 [**客户机-服务器模型**](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) ，其中客户机可以与服务器交互。

这种模式不是偶然出现的，它实际上是一种非常常见的模式，你现在用它来连接 dq-staging.t79ae38x-liquidwebsites.com/blog.，你的电脑，笔记本电脑，或者任何你用来阅读这篇文章的设备就是从 dq-staging.t79ae38x-liquidwebsites.com`server`请求信息的`client`。每当您访问一个网站时，您的浏览器(客户端)将不断向服务器请求网站数据。

然而，在 Postgres 的例子中，连接到 Postgres 服务的客户机将使用 database=specific 请求，而不是请求网站数据。这些类型的请求是根据定义的**协议**实现的，协议是客户机和服务器都同意的一组规则。你可以把协议想象成当客户端请求时，服务器**用数据响应**时，客户端和服务器都会使用的语言。

使用这个模型，Postgres 可以处理到数据库的多个连接，解决了将多个用户连接到一个数据库的难题。此外，作为一个服务器，Postgres 可以通过适当的安全措施实现更高级的查询功能。所有这些原因使得 Postgres 成为数据分析的绝佳选择。

## 连接到 Postgres

如果我们希望与 Postgres 服务器通信，我们需要使用一种使用前面描述的数据库协议的客户机。我们将使用 [psycopg2](http://initd.org/psycopg/) ，这是一个实现 Postgres 协议的开源库。你可以把 [psycopg2](http://initd.org/psycopg/) 想象成类似于使用 [sqlite3](https://docs.python.org/3.5/library/sqlite3.html) 连接到 SQLite 数据库。

以下面的例子为例，使用 [psycopg2](http://initd.org/psycopg/) 连接到 Postgres 数据库

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
```

让我们描述一下传递给`psycopg2.connect()`方法的这三个参数。首先，需要指定一个主机名`host`,描述 Postgres 服务器在哪里运行。因为我们运行的是 Postgres 的本地版本，所以我们使用默认的主机名`localhost`。

然后，我们需要传入数据库名称`dbname`和用户`user`。由于存在多个连接，Postgres 使用多个用户和数据库作为提高安全性和数据分割的方法。我们对这两个值都使用`postgres`,因为这是 Postgres 安装时的默认值。

如果没有`psycopg2`中的这些值，Postgres 将不知道您想要连接到哪里，并且会失败。请记住，无论何时您想要连接到 Postgres 服务器，无论是在 web 上还是在本地，您都必须传递适当的连接参数。现在我们已经连接上了，是时候利用 Postgres 的特性了！

## 与数据库交互

在前面的例子中，我们通过使用`psycopg2`模块的`connect()`方法打开了一个到 Postgres 的连接。`connect()`方法接受一系列参数，库使用这些参数连接到 Postgres 服务器。`connect()`方法的返回值是一个[连接](http://initd.org/psycopg/docs/connection.html)对象。

连接对象创建了一个与数据库服务器的客户端**会话**，该会话实例化了一个**持久客户端**进行对话。为了对数据库发出命令，您还需要创建另一个名为 [Cursor](http://initd.org/psycopg/docs/cursor.html) 对象的对象。`Cursor`是由`Connection`对象创建的，使用`Cursor`对象我们将能够执行我们的命令。

要在 Postgres 数据库上执行命令，可以用一个字符串化的 SQL 命令调用`Cursor`对象上的`execute`方法。下面是一个如何在假的`notes`表上运行的例子:

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
cur.execute('SELECT * FROM notes') 
```

在这个例子中，`cur`对象调用`execute`方法，如果成功，将返回`None`。要从查询中获得返回值，需要调用两个方法之一:`fetchone()`或`fetchall()`。`fetchone()`方法返回第一行结果或`None`，而`fetchall()`方法返回表中每一行的列表，如果没有行，则返回空列表`[]`。

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
cur.execute('SELECT * FROM notes')
one = cur.fetchone()
all = cur.fetchall()
```

遗憾的是，对于我们当前的数据库，我们没有创建任何表。没有任何表，就没有什么有趣的查询。为了解决这个问题，让我们继续创建我们的第一个表！

## 创建表格

现在我们已经对如何连接和执行数据库查询有了基本的了解，是时候创建您的第一个 Postgres 表了。从 [Postgres 文档](https://www.postgresql.org/docs/9.1/static/sql-createtable.html)中，可以看到如何在 Postgres 中创建表格:

```py
 CREATE TABLE tableName(
    column1 dataType1 PRIMARY KEY,
    column2 dataType2,
    column3 dataType3,
    ...
);
```

每个`column[n]`都是列名的占位符，`dataType[n]`是要为该列存储的数据类型，`PRIMARY KEY`是要添加到表中的可选参数的示例。在 Postgres 中，每个表至少需要一个包含一组唯一值的`PRIMARY KEY`列。现在让我们看看我们希望加载到数据库中的 CSV 文件(注意，CSV 文件不包含真实用户，而是使用名为 [faker](https://github.com/joke2k/faker) 的 Python 库随机生成的用户)。

| 身份证明（identification） | 电子邮件 | 名字 | 地址 |
| Zero | [【电子邮件保护】](/cdn-cgi/l/email-protection) | 安娜·杰克逊 | 22871 东北 30037 汉纳营阿姆斯壮顿 |
| one | [【电子邮件保护】](/cdn-cgi/l/email-protection) | 娜塔莉·霍洛韦 | 7357 芭芭拉课穆尔茅斯，你好 03826 |
| Two | [【电子邮件保护】](/cdn-cgi/l/email-protection)' | 亚伦·赫尔 | 362 考克斯旁路套房 052 新达伦茅斯，爱荷华州 67749-2829 |

您可以从 Dataquest 服务器下载这个 CSV 文件，。在您刚刚下载的 CSV 文件中，`user_accounts.csv`，看起来我们基本上有两种不同的数据类型需要考虑。第一个是每个 id 的整数类型，其余的是字符串类型。像其他关系数据库一样，Postgres 是类型敏感的——这意味着您必须为您创建的表的每一列声明类型。你可以在 [Postgres 文档](https://www.postgresql.org/docs/9.1/static/datatype-numeric.html)中找到所有类型的列表。

为了创建一个适合我们数据集的表，我们必须运行`CREATE TABLE`命令，其中的列与 CSV 文件的顺序以及它们各自的类型相同。类似于运行一个`SELECT`查询，我们将把命令写成一个字符串，并把它传递给`execute()`方法。下面是该表的外观:

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE users(
    id integer PRIMARY KEY,
    email text,
    name text,
    address text
)
""")
```

## SQL 事务

如果您现在检查`postgres`数据库，您会注意到其中实际上没有一个`users`表。这不是一个错误——这是因为一个叫做 **SQL 事务**的概念。与 SQLite 相比，在该引擎中执行的每个查询都会立即反映为数据库的变化。

使用 Postgres，我们要处理的是可能同时更改数据库的多个用户。让我们想象一个简单的场景，我们跟踪一家银行不同客户的账户。我们可以编写一个简单的查询来为此创建一个表:

```py
 CREATE TABLE accounts(
    id integer PRIMARY KEY,
    name text,
    balance float
); 
```

我们的表在表中有以下两行:

```py
 id name balance
1 Jim 100
2 Sue 200
```

假设`Sue`给`100`美元给`Jim`。我们可以用两个查询对此建模:

```py
 UPDATE accounts SET balance=200 WHERE name="Jim";
UPDATE accounts SET balance=100 WHERE name="Sue";
```

在上面的例子中，我们从`Sue`中删除了`100`美元，并将`100`美元添加到`Jim`中。如果第二个`UPDATE`语句有错误，数据库失败，或者另一个用户有冲突的查询，该怎么办？第一个查询可以正常运行，但是第二个查询会失败。这将导致以下结果:

```py
 id name balance
1 Jim 100
2 Sue 200
```

`Jim`将被记入`100`美元，但`100`美元不会从`Sue`中移除。这将导致银行亏损。

事务通过确保事务块中的所有查询同时执行来防止这种行为。如果任何事务失败，整个组都会失败，并且不会对数据库进行任何更改。

每当我们在 [psycopg2](http://initd.org/psycopg/) 中打开一个[连接](http://initd.org/psycopg/docs/connection.html)，就会自动创建一个新的事务。在调用[提交](http://initd.org/psycopg/docs/connection.html#connection.commit)方法之前运行的所有查询都将被放入同一个事务块中。当调用 [commit](http://initd.org/psycopg/docs/connection.html#connection.commit) 时，PostgreSQL 引擎将立即运行所有查询。

如果我们不想在事务块中应用更改，我们可以调用 [rollback](http://initd.org/psycopg/docs/connection.html#connection.rollback) 方法来删除事务。不调用[提交](http://initd.org/psycopg/docs/connection.html#connection.commit)或[回滚](http://initd.org/psycopg/docs/connection.html#connection.rollback)将导致事务停留在未决状态，并将导致更改不会应用到数据库。

为了提交我们的更改并从前面创建`users`表，我们需要做的就是在事务结束时运行`commit()`方法。现在应该是这样的:

```py
 conn = psycopg2.connect("dbname=dq user=dq")
cur = conn.cursor()
cur.execute("""CREATE TABLE users(
    id integer PRIMARY KEY,
    email text,
    name text,
    address text
)
""")
conn.commit()
```

## 插入数据

创建并提交了我们的表之后，是时候将 CSV 文件加载到数据库中了！

将数据加载到 Postgres 表中的一种常见方式是在表上发出一个`INSERT`命令。insert 命令需要插入的表名和插入的值序列。下面是一个对`users`表进行插入查询的示例:

```py
 INSERT INTO users VALUES (10, "[[email protected]](/cdn-cgi/l/email-protection)", "Some Name", "123 Fake St.")
```

使用`INSERT`命令，我们可以*使用`pyscopg2`将*插入到`users`表中。首先，为`execute()`方法编写一个字符串`INSERT` SQL 命令。然后，用所有值格式化字符串:

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
insert_query = "INSERT INTO users VALUES {}".format("(10, '[[email protected]](/cdn-cgi/l/email-protection)', 'Some Name', '123 Fake St.')")
cur.execute(insert_query)
conn.commit()
```

不幸的是，`format()`字符串方法有一个问题。问题是你必须手动格式化类型，比如字符串转义`"'[[email protected]](/cdn-cgi/l/email-protection)'"`。使用这种插入方式很可能会出错。

幸运的是，`psycopg2`提供了另一种无需`format()`就能执行字符串插值的方法。这是使用`psycopg2`呼叫`INSERT`的推荐方式:

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
cur.execute("INSERT INTO users VALUES (%s, %s, %s, %s)", (10, '[[email protected]](/cdn-cgi/l/email-protection)', 'Some Name', '123 Fake St.'))
conn.commit()
```

这种类型的插入会自动将每种类型转换成表所期望的正确数据类型。另一个好处是您的插入查询实际上加快了，因为数据库中的`INSERT`语句是**准备好的**。在我们的 Postgres 课程中，如果你感兴趣的话，我们会介绍这种优化，但是现在让我们先插入 CSV 文件。

我们首先使用 [`csv`模块](https://docs.python.org/3.6/library/csv.html)加载 Python 中的 CSV 文件。然后，我们将为每一行运行`INSERT`查询，然后提交事务:

```py
 import csv
import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
with open('user_accounts.csv', 'r') as f:
    reader = csv.reader(f)
    next(reader) # Skip the header row.
    for row in reader:
        cur.execute(
        "INSERT INTO users VALUES (%s, %s, %s, %s)",
        row
    )
conn.commit()
```

虽然这完成了加载数据的任务，但实际上并不是最有效的方式。如您所见，我们必须遍历文件中的每一行，才能将它们插入到数据库中！幸运的是，Postgres 有一个专门用于将文件加载到表中的命令。

## 复制数据

将文件直接加载到表格中的 Postgres 命令称为 [`COPY`](https://www.postgresql.org/docs/9.2/static/sql-copy.html) 。它接收一个文件(类似于 CSV ),并自动将文件加载到 Postgres 表中。不同于创建查询，然后像`INSERT`一样通过`execute()`运行它，`psycopg2`有一个专门为这个查询编写的方法。

将文件加载到表格中的方法称为 [`copy_from`](http://initd.org/psycopg/docs/cursor.html#cursor.copy_from) 。像`execute()`方法一样，它被附加到`Cursor`对象上。然而，由于其参数的原因，它与`execute()`方法有很大的不同。

`copy_from`参数需要加载一个文件(没有头文件)，它应该加载的表名，以及一个分隔符(关键参数`sep`)。然后，运行`commit()`，文件被传输到 ths 中，这是将 CSV 文件加载到 Postgres 表中的最有效的方法，也是推荐的方法。

这就是我们如何使用`copy_from()`来加载我们的文件，而不是循环使用`INSERT`命令:

```py
 import psycopg2
conn = psycopg2.connect("host=localhost dbname=postgres user=postgres")
cur = conn.cursor()
with open('user_accounts.csv', 'r') as f:
    # Notice that we don't need the `csv` module.
    next(f) # Skip the header row.
    cur.copy_from(f, 'users', sep=',')

conn.commit()
```

就这么简单！最后，我们使用最优 Postgres 方式成功地将`user_accounts.csv`文件加载到我们的表中。让我们花一点时间回顾一下到目前为止我们所学的内容。

## 摘要

*   Postgres 使用客户机-服务器模型来支持到数据库的多个连接。
*   使用流行的`psycopg2`库，我们可以使用 Python 连接 Postgres。
*   Postgres 是类型敏感的，所以我们必须在每一列中声明类型。
*   Postgres 使用 SQL 事务来保存数据库的状态。
*   插入数据时，使用`psycopg2`字符串插值代替`.format()`。
*   将文件加载到 Postgres 表中最有效的方法是使用`COPY`或`psycopg2.copy_from()`方法。

## 后续步骤

如果您想了解更多，本教程基于我们的 Data quest[Postgres](https://www.dataquest.io/course/postgres-for-data-engineers)课程简介，这是我们数据工程学习路径的一部分。该课程扩展了本文中的模型，并在您编写代码的过程中提供实践学习。

如果您想继续使用此表格，您还可以做以下几件事:

*   删除数据，然后使用`DELETE`命令重新加载。
*   对正在加载的数据使用更有效的类型。例如，对于字符串数据，Postgres 有比`TEXT`更多的类型。如果你感兴趣的话，我们会在一节课中详细介绍 Postgres 的类型。
*   尝试使用`COPY ... TO`或 [`cur.copy_to()`](http://initd.org/psycopg/docs/cursor.html#cursor.copy_to) 方法将表格导出到另一个文件中。在本课程中，我们将介绍这一点以及加载数据的其他`INSERT`和`COPY`选项。

### 成为一名数据工程师！

现在就学习成为一名数据工程师所需的技能。注册一个免费帐户，访问我们的交互式 Python 数据工程课程内容。

[Sign up now!](https://app.dataquest.io/signup)

*(免费)*

![YouTube video player for ddM21fz1Tt0](img/5a85348206993fc2a430506128b76684.png)

*[https://www.youtube.com/embed/ddM21fz1Tt0?rel=0](https://www.youtube.com/embed/ddM21fz1Tt0?rel=0)*