# 教程:用 Python 连接、安装和查询 PostgreSQL

> 原文：<https://www.dataquest.io/blog/tutorial-connect-install-and-query-postgresql-in-python/>

February 21, 2022![PostgreSQL Function in Python](img/7adb060fbfc566599aecdaf74d9264a9.png)

数据库无处不在——在你的手机里，在你的电脑上，在你心爱的应用程序背后。但是，如果不能从数据库中查询数据，数据库又有什么价值呢？在本文中，我们将展示从 Python 代码中查询任何基于 PostgreSQL 的数据库的示例。首先，您将获得对 PostgreSQL 和数据库连接器的高级理解。在本文的后面，我们将深入研究实际的代码示例和其他关于如何使用 Python 查询数据库的技巧。我们开始吧！

## PostgreSQL 是什么？

PostgreSQL 是最流行的开源关系数据库之一。全球各种规模的公司和开发人员都在使用它。根据 DB-Engines，PostgreSQL [在世界上最受欢迎的数据库中排名第四](https://db-engines.com/en/ranking)，并且有上升趋势。这并不奇怪，因为您可以在许多 web 和移动应用程序甚至分析软件背后找到 PostgreSQL 数据库。

PostgreSQL 也有一个丰富的生态系统，有大量的工具和扩展，可以很好地与核心数据库集成。由于这些原因，无论您是需要事务性数据库还是分析性数据库，或者希望构建自己的定制数据库解决方案，PostgreSQL 都是一个很好的选择。

现在您对 PostgreSQL 有了一个概念，让我们来看看如何使用 Python 来连接数据库。

## 如何从 Python 连接 PostgreSQL？

为了从 Python 脚本连接到 PostgreSQL 数据库实例，您需要使用数据库连接器库。在 Python 中，有几个选项可供选择。一些用纯 Python 编写的库包括 [pg8000](https://github.com/tlocke/pg8000) 和 [py-postgresql](https://github.com/python-postgres/fe) 。但是最受欢迎和广为人知的是[心理战 2](https://www.psycopg.org/docs/) 。

在本文的其余部分，您将看到使用 Psycopg2 连接到数据库并查询数据的示例。

但首先，什么是 Psycopg2？

## 什么是 Psycopg2？

Psycopg2 是 Python 中最广为人知和使用最多的 PostgreSQL 连接器库。Psycopg2 库实现了 [Python DB API 2.0](https://www.python.org/dev/peps/pep-0249/) 规范，并且(在幕后)使用 C 编程语言作为对 [libpq](https://www.postgresql.org/docs/current/static/libpq.html) PostgreSQL 库的包装。由于它的 C 实现，Psycopg2 非常快速和高效。

您可以使用 Psycopg2 根据 SQL 查询从数据库中提取一行或多行。如果您想将一些数据插入数据库，这个库也是可行的——有多个选项可以进行单次或批量插入。

Python PostgreSQL 连接器的一个完全重写的实现目前正在积极开发中: [Psycopg3](https://www.psycopg.org/psycopg3/) 。Psycopg3 提供了与 Psycopg2 相同的功能，此外还有一些额外的功能，如异步通信、使用 COPY 的数据接收等等。它努力更好地利用新一代 Python 和 PostgreSQL 提供的功能。

## 如何安装 Psycopg2

要使用 Psycopg2，您需要先安装它。最简单的方法是使用 pip。与其他 [Python 项目](https://www.dataquest.io/blog/python-projects-for-beginners/)一样，建议使用虚拟环境来安装库:

```py
virtualenv env && source env/bin/activate
pip install psycopg2-binary
```

这个代码片段将把 Psycopg2 库安装到您的 [Python 虚拟环境](https://www.dataquest.io/blog/a-complete-guide-to-python-virtual-environments/)及其所有依赖项中。之后，您可以将`psycopg2`模块导入到您的 Python 代码中并使用它。

## 应该用 Psycopg2 还是 Psycopg2-binary？

您可能已经注意到了，我们安装了`psycopg2-binary`包，这是 Psycopg2 的二进制版本。这意味着这个版本的库自带了自己版本的 C 库，即 liboq 和 libssl。对于 Psycopg2 初学者和大多数用户来说，这个版本非常好。另一方面，如果您希望 psycop 2 使用您系统的 C 库，您需要从源代码构建 psycop 2:

```py
pip install psycopg2
```

作为一个数据库连接器库，Psycopg2 在许多基于无服务器架构的数据管道中是必不可少的。让我们快速介绍一下如何在无服务器环境中安装 Psycopg2。

### 如何在无服务器环境中安装 Psycopg2

Psycopg2 通常用于 AWS Lambda 函数或其他无服务器环境中。因为 Psycopg2 依赖于几个 PostgreSQL 库——它们在无服务器环境中不一定可用，或者不容易使用——所以建议使用预编译版本的库，例如， [this one](https://github.com/jetbridge/psycopg2-lambda-layer) 。

您还可以通过搜索“Psycopg2 无服务器”或“psycopg2 aws lambda”关键字来查找 psycopg2 的其他预编译版本。

最后，在您的环境中安装了 Psycopg2 之后，这里有一个关于如何从数据库中查询数据的快速教程。

## 如何使用 Python 查询 PostgreSQL

为了查询您的数据库，您需要使用两个 Psycopg2 对象:

*   一个[连接对象](https://www.psycopg.org/docs/connection.html)
*   一个[光标对象](https://www.psycopg.org/docs/cursor.html)

首先，您需要导入 Psycopg2 模块并创建一个连接对象:

```py
import psycopg2

conn = psycopg2.connect(database="db_name",
                        host="db_host",
                        user="db_user",
                        password="db_pass",
                        port="db_port")
```

如您所见，为了连接到数据库，您需要定义相当多的参数。以下是对这些论点的含义的简要总结:

*   **数据库:**您要连接的数据库的名称(一个连接对象只能连接一个数据库)
*   **主机:**这可能是指向数据库服务器的 IP 地址或 URL(例如，xyz.example.com)
*   **用户:**PostgreSQL 用户的名称
*   **密码:**该用户的匹配密码
*   **端口:**PostgreSQL 服务器使用的端口(如果服务器位于本地，通常为 5432，但也可以是其他端口)

如果您提交了正确的数据库凭据，您将获得一个活动的数据库连接对象，可用于创建游标对象。

游标对象将帮助您在数据库上执行任何查询和检索数据。下面是创建光标对象的方法:

```py
cursor = conn.cursor()
```

现在让我们使用刚刚创建的游标来查询数据库:

```py
cursor.execute("SELECT * FROM example_table")
```

我们使用`execute()`函数并提交一个查询字符串作为它的参数。我们提交的这个查询将针对数据库运行。需要注意的是，在查询执行之后，您仍然需要使用 Psycopg2 函数之一来检索数据行:

*   [T2`fetchone()`](https://www.psycopg.org/docs/cursor.html#cursor.fetchone)
*   [T2`fetchall()`](https://www.psycopg.org/docs/cursor.html#cursor.fetchall)
*   [T2`fetchmany()`](https://www.psycopg.org/docs/cursor.html#cursor.fetchmany)

让我们看看他们每个人是如何工作的！

## 示例:`fetchone()`

从数据库获取数据的最基本方法是使用`fetchone()`函数。在执行 SQL 查询后，该函数将恰好返回一行，即第一行。

这里有一个例子:

```py
print(cursor.fetchone())
```

```py
(1, 'Budapest', 'newly-built', 'after 2011', 30, 1)
```

在本例中，`fetchone()`以元组的形式返回数据库中的一行，其中数据在元组中的顺序将基于您在查询中指定的列的顺序。

因此，在创建查询字符串时，一定要确保正确指定列的顺序，这样才能知道元组中的数据。

## 示例:`fetchall()`

如果您需要数据库中不止一行数据，该怎么办？如果您需要 10、100、1000 或更多行呢？您可以使用`fetchall()` Psycopg2 函数，它的工作方式与`fetchone()`相同，只是它返回的结果不是一行，而是所有行。

```py
print(cursor.fetchall())
```

```py
[(1, 'Budapest', 'newly-built', 'after 2011', 30, 1),
 (2, 'Budapest', 'newly-built', 'after 2011', 45, 2),
 (3, 'Budapest', 'newly-built', 'after 2011', 32, 2),
 (4, 'Budapest', 'newly-built', 'after 2011', 48, 2),
 (5, 'Budapest', 'newly-built', 'after 2011', 49, 2),
 (6, 'Budapest', 'newly-built', 'after 2011', 49, 2),
 (7, 'Budapest', 'newly-built', 'after 2011', 71, 3),
 (8, 'Budapest', 'newly-built', 'after 2011', 50, 2),
 (9, 'Budapest', 'newly-built', 'after 2011', 50, 2),
 (10, 'Budapest', 'newly-built', 'after 2011', 57, 3)]
[...]
```

注意我们如何获得更多的行，而不仅仅是一行。

## 示例:`fetchmany()`

使用`fetchmany()`，您可以从数据库中检索多条记录，并对检索的确切行数有更多的控制。

```py
print(cursor.fetchmany(size=5))
```

```py
[(1, 'Budapest', 'newly-built', 'after 2011', 30, 1),
 (2, 'Budapest', 'newly-built', 'after 2011', 45, 2),
 (3, 'Budapest', 'newly-built', 'after 2011', 32, 2),
 (4, 'Budapest', 'newly-built', 'after 2011', 48, 2),
 (5, 'Budapest', 'newly-built', 'after 2011', 49, 2)]
```

这里我们只收到 5 行，因为我们将参数`size`设置为 5。这个函数让您在代码级别上更好地控制从数据库表中返回多少行。

## 包扎

在查询完数据库并在 Python 代码中使用 connection 对象之后，确保总是使用`conn.close()`关闭连接。

我希望这篇文章有助于您开始使用 Python 和 PostgreSQL。我强烈建议阅读 [Psycopg2 文档](https://www.psycopg.org/docs/),了解更多关于不同的数据检索方法、游标类等等。

编码快乐！