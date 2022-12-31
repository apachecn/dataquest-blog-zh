# 教程:将记录和数据帧插入 SQL 数据库

> 原文：<https://www.dataquest.io/blog/sql-insert-tutorial/>

August 12, 2019![sql-insert](img/97474a20353a0df0747fe5a4b162e297.png)

*学会像专业人员一样将数据插入 SQL 数据库！*

数据科学家的关键角色之一是从原始数据中提取模式和见解。由于世界上许多政府和企业数据都是在关系数据库中组织的，因此数据科学家需要知道如何使用这些数据库结构是有意义的。对于任何对数据分析或数据科学感兴趣的人来说，编写 SQL 查询来插入、提取和过滤数据库中的数据是一项关键技能。

SQL(结构化查询语言)基于 E. F. Codd 的关系模型和代数来管理关系数据库。它是一种数据库查询语言，用于创建、插入、查询和操作关系数据库，被大量应用程序使用。

虽然 SQL 已经存在了几十年，但对于现代数据科学家来说，学习 SQL 仍然是一项关键技能，实际上对于任何处理数据的人来说都是如此，因为 SQL 被用于各种关系数据库软件，包括 MySQL、SQL Server、Oracle 和 PostgreSQL。

在本教程中，我们将详细了解 SQL 插入操作。以下是我们将在本教程中学习的主题列表:

*   SQL 插入
*   将记录插入数据库
*   使用 insert 命令将 Pandas 数据帧插入数据库
*   使用 to_sql()命令将 Pandas 数据帧插入数据库
*   从数据库中读取记录
*   更新数据库中的记录

想要达到更高水平的 SQL 技能？[免费注册并查看 Dataquest 的 SQL 课程](https://app.dataquest.io/signup?course=funds-sql-i&source=https://www.dataquest.io/blog/sql-insert-tutorial/)了解数据科学所需的所有 SQL 技能的全面互动课程。

## SQL 插入

SQL 插入是数据工作者需要理解的基本操作。在大多数数据科学项目中，插入缺失数据或添加新数据是数据清理过程的主要部分。

插入也是大多数数据首先进入数据库的方式，因此它在您收集数据的任何时候都很重要。例如，当您的公司获得一个客户的新数据时，SQL insert 很有可能会将该数据插入到您现有的客户数据库中。

事实上，不管您是否意识到了这一点，数据一直在使用 SQL inserts 流入数据库！当您填写市场调查、完成交易、在线提交政府表格或做成千上万的其他事情时，您的数据可能会使用 SQL 插入到某个数据库中。

让我们深入了解如何使用 SQL 将数据插入数据库。我们可以逐行插入数据，或者一次添加多行。

#### 将记录插入数据库

在 SQL 中，我们使用`INSERT`命令将记录/行添加到表数据中。这个命令不会修改我们要插入的表格的实际结构，它只是添加数据。

假设我们有一个如下所示的数据表，用来存储公司员工的一些信息。

现在，让我们想象一下，我们有新的员工需要输入到系统中。

这个`employee`表可以使用`CREATE TABLE`命令创建，所以我们可以使用该命令创建一个全新的表。但是每当我们想要添加数据时，创建一个全新的表是非常低效的！相反，让我们使用`INSERT`命令将新数据添加到现有的表中。

下面是在 SQL 中使用`INSERT`的基本语法:

我们从命令`INSERT INTO`开始，后跟我们想要插入数据的表的名称。在表名之后，我们在圆括号中列出我们要逐列插入的新数据的列。然后，在下一行，我们使用了命令`VALUES`以及我们想要插入的值(在括号内按顺序)。

因此，对于我们的`employee`表，如果我们添加一个名为 Kabir 的新雇员，我们的`INSERT`命令可能如下所示:

#### 从 Python 向数据库中插入记录

因为我们在做数据科学时经常使用 Python 中的数据，所以让我们将 Python 中的数据插入到 MySQL 数据库中。这是一项常见任务，在数据科学中有多种应用。

我们可以通过在 Python 和 MySQL 之间建立连接来向 MySQL 数据库发送和接收数据。有各种方法可以建立这种联系；这里，我们将使用`pymysql`进行数据库连接。

下面是我们需要完成的大致步骤，以连接`pymysql`，插入我们的数据，然后从 MySQL 中提取数据:

让我们一步一步地完成这个过程。

#### **第一步:**导入 pymysql 模块。

```
# Import pymysql module
import pymysql
```

#### **步骤 2:** 创建到 MySQL 数据库的连接 a

使用带有参数 host、user、database name 和 password 的`pymysql`函数创建一个连接。

(以下参数仅用于演示目的；您需要填写您正在访问的 MySQL 数据库所需的特定访问细节。)

```
# Connect to the database
connection = pymysql.connect(host='localhost',
                             user='root',
                             password='12345',
                             db='employee')
```

#### **第三步:**使用 cursor()函数创建一个游标。

这将允许我们在编写完 SQL 查询后执行它。

```
cursor = connection.cursor()
```

#### **第四步:**执行所需的 SQL 查询

使用`commit()`函数提交更改，并检查插入的记录。注意，我们可以创建一个名为`sql`的变量，将我们的查询语法赋给它，然后将 sql 和我们想要插入的特定数据作为参数传递给`cursor.execute()`。

然后，我们将使用`commit()`提交这些更改。

```
# Create a new record
sql = "INSERT INTO `employee` (`EmployeeID`, `Ename`, `DeptID`, `Salary`, `Dname`, `Dlocation`) VALUES (%s, %s, %s, %s, %s, %s)"

# Execute the query
cursor.execute(sql, (1008,'Kabir',2,5000,'IT','New Delhi'))

# the connection is not autocommited by default. So we must commit to save our changes.
connection.commit()
```

让我们快速检查一下，看看我们想要插入的记录是否已经被插入。

我们可以通过在数据库中查询`employee`的全部内容，然后获取并打印这些结果。

```
# Create a new query that selects the entire contents of `employee`
sql = "SELECT * FROM `employee`"
cursor.execute(sql)

# Fetch all the records and use a for loop to print them one line at a time
result = cursor.fetchall()
for i in result:
    print(i)
```

```
(1001, 'John', 2, 4000, 'IT', 'New Delhi') 
(1002, 'Anna', 1, 3500, 'HR', 'Mumbai') 
(1003, 'James', 1, 2500, 'HR', 'Mumbai') 
(1004, 'David', 2, 5000, 'IT', 'New Delhi') 
(1005, 'Mark', 2, 3000, 'IT', 'New Delhi') 
(1006, 'Steve', 3, 4500, 'Finance', 'Mumbai') 
(1007, 'Alice', 3, 3500, 'Finance', 'Mumbai') 
(1008, 'Kabir', 2, 5000, 'IT', 'New Delhi')
```

成功了！在上面，我们可以看到新记录已经被插入，现在是 MySQL 数据库中的最后一行。

#### **第五步:**关闭数据库连接

现在我们完成了，我们应该使用`close()`方法关闭数据库连接。

```
# Close the connection
connection.close()
```

当然，以一种能够更好地处理异常和错误的方式编写这段代码会更好。我们可以使用`try`来包含我们的代码体，并在出现错误时打印出来。然后，一旦完成，我们可以使用`finally`来关闭连接，不管`try`是成功还是失败。

这是所有这些看起来的样子:

```
import pymysql

try:
    # Connect to the database
    connection = pymysql.connect(host='localhost',
                             user='root',
                             password='12345',
                             db='employee')

    cursor=connection.cursor()

    # Create a new record
    sql = "INSERT INTO `employee` (`EmployeeID`, `Ename`, `DeptID`, `Salary`, `Dname`, `Dlocation`) VALUES (%s, %s, %s, %s, %s, %s)"
    cursor.execute(sql, (1009,'Morgan',1,4000,'HR','Mumbai'))

    # connection is not autocommit by default. So we must commit to save our changes.
    connection.commit()

    # Execute query
    sql = "SELECT * FROM `employee`"
    cursor.execute(sql)
    # Fetch all the records
    result = cursor.fetchall()
    for i in result:
        print(i)

except Error as e:
    print(e)

finally:
    # close the database connection using close() method.
    connection.close()
```

```
((1001, 'John', 2, 4000, 'IT', 'New Delhi'), (1002, 'Anna', 1, 3500, 'HR', 'Mumbai'), (1003, 'James', 1, 2500, 'HR', 'Mumbai'), (1004, 'David', 2, 5000, 'IT', 'New Delhi'), (1005, 'Mark', 2, 3000, 'IT', 'New Delhi'), (1006, 'Steve', 3, 4500, 'Finance', 'Mumbai'), (1007, 'Alice', 3, 3500, 'Finance', 'Mumbai'), (1008, 'Kabir', 2, 5000, 'IT', 'New Delhi'), (1009, 'Morgan', 1, 4000, 'HR', 'Mumbai'), (1009, 'Morgan', 1, 4000, 'HR', 'Mumbai'))
```

## 使用 INSERT 将 Pandas 数据帧插入数据库

在 Python 中处理数据时，我们经常使用`pandas`，并且我们经常将数据存储为 pandas DataFrame。令人欣慰的是，如果我们想对数据帧使用 SQL，我们不需要做任何转换；我们可以使用`INSERT`将熊猫数据帧直接插入 MySQL 数据库。

同样，我们将一步一步来。

#### 第一步:使用字典创建数据帧

我们还可以从 CSV 导入数据，或者以任何其他方式创建数据帧，但是对于本例，我们只是创建一个小的数据帧来保存一些数据科学教科书的标题和价格。

```
# Import pandas
import pandas as pd

# Create dataframe
data = pd.DataFrame({
    'book_id':[12345, 12346, 12347],
    'title':['Python Programming', 'Learn MySQL', 'Data Science Cookbook'],
    'price':[29, 23, 27]
})

data
```

|  | 图书 id | 标题 | 价格 |
| --- | --- | --- | --- |
| Zero | one two three four five | Python 编程 | Twenty-nine |
| one | Twelve thousand three hundred and forty-six | 学习 MySQL | Twenty-three |
| Two | Twelve thousand three hundred and forty-seven | 数据科学食谱 | Twenty-seven |

#### 步骤 2: 在我们的 MySQL 数据库中创建一个表

在将数据插入 MySQL 之前，我们将在`MySQL`中创建一个`book`表来保存我们的数据。如果这样的表已经存在，我们可以跳过这一步。

我们将使用 CREATE TABLE 语句来创建我们的表，后跟我们的表名(在本例中为`book_details`)，然后列出每一列及其对应的数据类型。

![syntax for creating a table in SQL](img/a812c921cfa204cb884fd31911e1a502.png)

#### 步骤 3: 创建一个到数据库的连接

一旦我们创建了这个表，我们就可以再次使用`pymysql`从 Python 创建一个到数据库的连接。

```
import pymysql

# Connect to the database
connection = pymysql.connect(host='localhost',
                         user='root',
                         password='12345',
                         db='book')

# create cursor
cursor=connection.cursor()
```

#### **步骤 4:** 创建列列表并插入行

接下来，我们将创建一个列列表，通过遍历每一行并使用`INSERT INTO`将该行的值插入到数据库中，将我们的 dataframe 行一行一行地插入到数据库中。

(也可以一次插入整个数据帧，我们将在下一节中研究如何插入，但首先让我们看看如何逐行插入)。

```
# creating column list for insertion
cols = "`,`".join([str(i) for i in data.columns.tolist()])

# Insert DataFrame recrds one by one.
for i,row in data.iterrows():
    sql = "INSERT INTO `book_details` (`" +cols + "`) VALUES (" + "%s,"*(len(row)-1) + "%s)"
    cursor.execute(sql, tuple(row))

    # the connection is not autocommitted by default, so we must commit to save our changes
    connection.commit()
```

#### **第五步:**查询数据库检查我们的工作

同样，让我们查询数据库，以确保我们插入的数据已被正确保存。

```
# Execute query
sql = "SELECT * FROM `book_details`"
cursor.execute(sql)

# Fetch all the records
result = cursor.fetchall()
for i in result:
    print(i)
```

```
(12345, 'Python Programming', 29) 
(12346, 'Learn MySQL', 23) 
(12347, 'Data Science Cookbook', 27)
```

一旦我们确信一切正常，我们就可以关闭连接。

```
connection.close()
```

## 使用 to_sql()函数将 Pandas 数据帧插入数据库

现在让我们尝试做同样的事情——将熊猫数据帧插入 MySQL 数据库——使用不同的技术。这一次，我们将使用模块`sqlalchemy`来创建连接，并使用`to_sql()`函数来插入数据。

这种方法以更直接的方式实现了相同的最终结果，并允许我们一次将整个数据帧添加到 MySQL 数据库中。

```
# Import modules
import pandas as pd

# Create dataframe
data=pd.DataFrame({
    'book_id':[12345,12346,12347],
    'title':['Python Programming','Learn MySQL','Data Science Cookbook'],
    'price':[29,23,27]
})

data
```

|  | 图书 id | 标题 | 价格 |
| --- | --- | --- | --- |
| Zero | one two three four five | Python 编程 | Twenty-nine |
| one | Twelve thousand three hundred and forty-six | 学习 MySQL | Twenty-three |
| Two | Twelve thousand three hundred and forty-seven | 数据科学食谱 | Twenty-seven |

导入模块`sqlalchemy`并使用参数 user、password 和 database name 创建一个引擎。这就是我们连接和登录 MySQL 数据库的方式。

```
# import the module
from sqlalchemy import create_engine

# create sqlalchemy engine
engine = create_engine("mysql+pymysql://{user}:{pw}@localhost/{db}"
                       .format(user="root",
                               pw="12345",
                               db="employee"))
```

一旦我们连接上了，我们就可以使用带有参数表名、引擎名、if_exists 和 chunksize 的`to_sql()`函数将整个数据帧导出到 MySQL。

我们一会儿将仔细看看这些参数分别指的是什么，但是首先，看看使用这种方法将 pandas 数据帧插入 MySQL 数据库有多简单。我们只用一行代码就可以做到:

```
# Insert whole DataFrame into MySQL
data.to_sql('book_details', con = engine, if_exists = 'append', chunksize = 1000)
```

现在让我们仔细看看这些参数在代码中的作用。

*   `book_details`是我们要插入数据帧的表的名称。
*   `con = engine`提供连接细节(回想一下，我们在上一步中使用身份验证细节创建了引擎)。
*   `if_exists = 'append'`检查我们指定的表格是否已经存在，然后追加新数据(如果存在)或创建新表格(如果不存在)。
*   `chunksize`一次成批写入给定大小的记录。默认情况下，所有行都将一次写入。

## 从数据库中读取记录

一旦我们使用 SQL 插入将我们的数据放入数据库，我们将希望能够读回它！到目前为止，在本教程中，我们已经通过简单地打印整个数据库检查了我们的 SQL 插入，但是很明显，对于要打印数千行(甚至更多)的大型数据库来说，这不是一个可行的选择。因此，让我们更深入地看看如何回读我们已经创建或插入到 SQL 数据库中的记录。

我们可以使用`SELECT`命令从 SQL 数据库中读取记录。我们可以选择特定的列，或者使用`*`从给定的表中选择所有内容。我们还可以使用`WHERE`命令选择只返回满足特定条件的记录。

以下是这些命令的语法:

![reading-records-syntax-in-sql](img/821e8b123067449d1bfbf20d3ba3d612.png)

我们从一个`SELECT`子句开始，然后是列的列表，或者如果我们想要选择所有列，就从`*`开始。然后我们将使用一个`FROM`子句来命名我们想要查看的表。`WHERE`可以用来过滤记录，后面跟一个过滤条件，我们也可以用`ORDER BY`对记录进行排序。(条款`WHERE`和`ORDER BY`是可选的)。

对于较大的数据库，`WHERE`对于只返回我们想看的数据很有用。因此，例如，如果我们刚刚插入了一些关于特定部门的新数据，我们可以在查询中使用`WHERE`来指定部门`ID`，它将只返回部门 ID 与我们指定的部门 ID 相匹配的记录。

例如，使用前面的`employee`表比较这两个查询的结果。在第一个例子中，我们返回所有的行。在第二个例子中，我们只得到我们请求的行。当我们的表有 7 行时，这可能不会产生很大的差异，但是当您处理 7*千*行，甚至 7*百万*行时，使用`WHERE`只返回您想要的结果是非常重要的！

如果我们想在 Python 中这样做，我们可以使用本教程前面使用的相同脚本来查询这些记录。唯一的区别是，我们将告诉`pymysql`执行`SELECT`命令，而不是我们之前使用的`INSERT`命令。

```
# Import module
import pymysql

# create connection
connection = pymysql.connect(host='localhost',
                             user='root',
                             password='12345',
                             db='employee')

# Create cursor
my_cursor = connection.cursor()

# Execute Query
my_cursor.execute("SELECT * from employee")

# Fetch the records
result = my_cursor.fetchall()

for i in result:
    print(i)

# Close the connection
connection.close()
```

```
(1001, 'John', 2, 4000, 'IT', 'New Delhi') 
(1002, 'Anna', 1, 3500, 'HR', 'Mumbai') 
(1003, 'James', 1, 2500, 'HR', 'Mumbai') 
(1004, 'David', 2, 5000, 'IT', 'New Delhi') 
(1005, 'Mark', 2, 3000, 'IT', 'New Delhi') 
(1006, 'Steve', 3, 4500, 'Finance', 'Mumbai') 
(1007, 'Alice', 3, 3500, 'Finance', 'Mumbai') 
(1008, 'Kabir', 2, 5000, 'IT', 'New Delhi') 
(1009, 'Morgan', 1, 4000, 'HR', 'Mumbai') 
(1009, 'Morgan', 1, 4000, 'HR', 'Mumbai')
```

上面，我们已经选择并打印了整个数据库，但是如果我们想使用`WHERE`进行更仔细、有限的选择，方法是相同的:

```
my_cursor.execute("SELECT * FROM employee WHERE DeptID=2")
```

## 更新数据库中的记录

通常，我们需要在创建记录后修改表中的记录。

例如，假设我们的雇员表中的一名雇员获得了晋升。我们想更新他们的工资数据。这里,`INSERT INTO`命令帮不了我们，因为我们不想添加一个全新的行。

要修改表中的现有记录，我们需要使用`UPDATE`命令。`UPDATE`用于改变现有记录的内容。我们可以使用`SET`指定要更改的特定列和值，还可以使用`WHERE`进行有条件的更改，将这些更改仅应用于满足该条件的行。

![update table syntax](img/74bbbd4ea76ab09ddf96c66cc21397a1.png)

现在，让我们更新 employee 表中的记录并显示结果。在这种情况下，假设大卫得到了提升——我们将使用`UPDATE`编写一个查询，仅在雇员 id 为 1004(大卫的 ID)的列中将`Salary`设置为`6000`。

小心——如果没有`WHERE`子句，这个查询将更新表中的每一条记录，所以不要忘记这一点！

执行上述查询后，更新后的表如下所示:

## 结论

在本教程中，我们已经了解了 SQL 插入以及如何从 Python 向 MySQL 数据库插入数据。我们还学习了使用两种不同的方法将 Pandas 数据帧插入 SQL 数据库，包括高效的`to_sql()`方法。

当然，这只是 SQL 查询的冰山一角。如果你真的想成为一名 SQL 大师，请免费注册，深入 Dataquest 的交互式 SQL 课程之一[,获得交互式指导和编写高效、专业数据科学工作所需的所有查询的实践经验。](https://app.dataquest.io/signup?course=funds-sql-i&source=https://www.dataquest.io/blog/sql-insert-tutorial/)

另外，请查看我们的其他免费 SQL 相关资源:

*   [需要 SQL 认证吗？](https://www.dataquest.io/blog/sql-certification/)
*   [准备工作面试的 SQL 面试问题](https://www.dataquest.io/blog/sql-interview-questions/)
*   [我们的 SQL 备忘单](https://www.dataquest.io/blog/sql-cheat-sheet/)