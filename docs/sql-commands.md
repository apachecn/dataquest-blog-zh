# SQL 命令:完整列表(带示例)

> 原文：<https://www.dataquest.io/blog/sql-commands/>

February 17, 2021![SQL commands](img/88a69ef289b7f463c598a42d8207a9a3.png)

为了在 2021 年得到一份数据工作，[你需要学习 SQL](https://www.dataquest.io/blog/why-sql-is-the-most-important-language-to-learn/) 。与任何语言一样，尤其是当您是初学者时，在一个地方有一个常用 SQL 命令和操作符的列表是非常有用的，无论何时您需要它都可以参考——我们希望成为您的首选！

下面是 SQL 命令的完整列表，按每个命令的顶层组织(例如，SELECT TOP 在 SELECT 类别中)。

如果你正在学习 SQL，并且对缺乏结构或由谷歌搜索组成的枯燥课程感到沮丧，那么你可能会喜欢 Dataquest 的交互式 [SQL 课程](https://www.dataquest.io/path/sql-skills/)。无论您是一个试图做好工作准备的初学者，还是一个希望保持敏锐的经验丰富的开发人员，都有适合您的 SQL 课程。

**以下是我们初学 SQL 的一些课程:**

1.  [SQL 和数据库简介](https://www.dataquest.io/course/funds-sql-i)
2.  [过滤和排序 SQL 中的数据](https://www.dataquest.io/course/funds-sql-ii)
3.  [汇总 SQL 中的数据](https://www.dataquest.io/course/sql-summary)
4.  [在 SQL 中组合表格](https://www.dataquest.io/course/combining-tables-in-sql)
5.  [SQL 子查询](https://www.dataquest.io/course/sql-subqueries)

## SQL 命令列表

### 挑选

SELECT 可能是最常用的 SQL 语句。每次用 SQL 查询数据时都会用到它。它允许您定义希望查询返回什么数据。

例如，在下面的代码中，我们从名为`customers`的表中选择名为`name`的列。

```
SELECT name
FROM customers;
```

### 选择*

带星号(*)的 SELECT 将返回我们正在查询的表中所有列的*。*

```
SELECT * FROM customers;
```

### 选择不同

SELECT DISTINCT 仅返回不同的数据，换句话说，如果有重复的记录，它将只返回每条记录的一个副本。

下面的代码将只返回来自`customers`表的具有唯一`name`的行。

```
SELECT DISTINCT name
FROM customers;
```

### 选择进入

SELECT INTO 将指定数据从一个表复制到另一个表中。

```
SELECT * INTO customers
FROM customers_backup;
```

### 选择顶部

SELECT TOP only 返回表格中最上面的`x`数字或百分比。

下面的代码将从`customers`表中返回前 50 个结果:

```
SELECT TOP 50 * FROM customers;
```

下面的代码将返回 customers 表中前 50%的内容:

```
SELECT TOP 50 PERCENT * FROM customers;
```

### 如同

AS 用我们可以选择的别名重命名列或表。例如，在下面的代码中，我们将`name`列重命名为`first_name`:

```
SELECT name AS first_name
FROM customers;
```

### 从

FROM 指定从中提取数据的表:

```
SELECT name
FROM customers;
```

### 在哪里

WHERE 筛选您的查询，只返回符合设定条件的结果。我们可以将它与条件运算符一起使用，如`=`、`>`、`<`、`>=`、`<=`等。

```
SELECT name
FROM customers
WHERE name = ‘Bob’;
```

### 和

并在单个查询中组合两个或多个条件。要返回结果，必须满足所有条件。

```
SELECT name
FROM customers
WHERE name = ‘Bob’ AND age = 55;
```

### 运筹学

或者在单个查询中组合两个或多个条件。要返回结果，只需满足其中一个条件。

```
SELECT name
FROM customers
WHERE name = ‘Bob’ OR age = 55;
```

### 在...之间

BETWEEN 筛选您的查询以仅返回符合指定范围的结果。

```
SELECT name
FROM customers
WHERE age BETWEEN 45 AND 55;
```

### 喜欢

LIKE 在列中搜索指定的模式。在下面的示例代码中，将返回名称中包含字符 Bob 的任何行。

```
SELECT name
FROM customers
WHERE name LIKE ‘%Bob%’;
```

LIKE 的其他运算符:

*   `%x` —将选择所有以 x 开头的值
*   `%x%` —将选择包含 x 的所有值
*   `x%` —将选择所有以 x 结尾的值
*   `x%y` —将选择所有以 x 开头以 y 结尾的值
*   `_x%` —将选择第二个字符为 x 的所有值
*   `x_%` —将选择所有以 x 开头且长度至少为两个字符的值。您可以添加附加 _ 字符来扩展长度要求，即`x___%`

### 在…里

IN 允许我们在使用 WHERE 命令时指定要选择的多个值。

```
SELECT name
FROM customers
WHERE name IN (‘Bob’, ‘Fred’, ‘Harry’);
```

### 为空

IS NULL 将只返回具有空值的行。

```
SELECT name
FROM customers
WHERE name IS NULL;
```

### 不为空

IS NOT NULL 的作用正好相反——它将只返回没有空值的行*。*

```
SELECT name
FROM customers
WHERE name IS NOT NULL;
```

### 创造

CREATE 可用于建立数据库、表、索引或视图。

### 创建数据库

CREATE DATABASE 创建一个新的数据库，假设运行该命令的用户具有正确的管理权限。

```
CREATE DATABASE dataquestDB;
```

### 创建表格

CREATE TABLE 在数据库中创建新表。本例中的术语 **int** 和 **varchar(255)** 指定了我们正在创建的列的数据类型。

```
CREATE TABLE customers (
    customer_id int,
    name varchar(255),
    age int
);
```

### 创建索引

CREATE INDEX 为表生成索引。索引用于更快地从数据库中检索数据。

```
CREATE INDEX idx_name
ON customers (name);
```

### 创建视图

CREATE VIEW 基于 SQL 语句的结果集创建虚拟表。视图就像一个普通的表(可以像普通的表一样被查询)，但是它是*而不是*作为一个永久的表保存在数据库中。

```
CREATE VIEW [Bob Customers] AS
SELECT name, age
FROM customers
WHERE name = ‘Bob’;
```

### 滴

DROP 语句可用于删除整个数据库、表或索引。

不用说，DROP 命令应该只在绝对必要的情况下使用。

### 删除数据库

DROP DATABASE 删除整个数据库，包括它的所有表、索引等以及其中的所有数据。

同样，这是一个我们希望非常小心使用的命令！

```
DROP DATABASE dataquestDB;
```

### 翻桌

DROP TABLE 删除一个表以及其中的数据。

```
DROP TABLE customers;
```

### 下降指数

DROP INDEX 删除数据库中的索引。

```
DROP INDEX idx_name;
```

### 更新

UPDATE 语句用于更新表中的数据。例如，下面的代码会将`customers`表中任何名为`Bob`的客户的年龄更新为`56`。

```
UPDATE customers
SET age = 56
WHERE name = ‘Bob’;
```

### 删除

DELETE 可以删除表中的所有行(使用*)，或者可以作为 WHERE 子句的一部分删除满足特定条件的行。*

```
DELETE FROM customers
WHERE name = ‘Bob’;
```

### 更改表格

ALTER TABLE 允许您在表中添加或删除列。在下面的代码片段中，我们将为`surname`添加然后移除一列。文本`varchar(255)`指定了列的数据类型。

```
ALTER TABLE customers
ADD surname varchar(255);
```

```
ALTER TABLE customers
DROP COLUMN surname;
```

### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)

### 聚合函数(计数/总和/AVG/最小值/最大值)

聚合函数对一组值执行计算，并返回单个结果。

### 数数

COUNT 返回符合指定条件的行数。在下面的代码中，我们使用了`*`，因此将返回`customers`的总行数。

```
SELECT COUNT(*)
FROM customers;
```

[https://youtu.be/JFlukJudHrk](https://youtu.be/JFlukJudHrk)

### 总和

SUM 返回数值列的总和。

```
SELECT SUM(age)
FROM customers;
```

### AVG

AVG 返回数值列的平均值。

```
SELECT AVG(age)
FROM customers;
```

### 部

MIN 返回数值列的最小值。

```
SELECT MIN(age)
FROM customers;
```

### 马克斯(男子名ˌ等于 Maximilian)

MAX 返回数值列的最大值。

```
SELECT MAX(age)
FROM customers;
```

### 分组依据

GROUP BY 语句将具有相同值的行分组到汇总行中。该语句通常与聚合函数一起使用。例如，下面的代码将显示出现在我们的`customers`表中的每个名字的平均年龄。

```
SELECT name, AVG(age)
FROM customers
GROUP BY name;
```

### 拥有

HAVING 执行与 WHERE 子句相同的操作。不同之处在于 HAVING 用于聚合函数，而 WHERE 不用于聚合函数。

下面的例子将返回每个名字的行数，但只返回超过 2 条记录的名字。

```
SELECT COUNT(customer_id), name
FROM customers
GROUP BY name
HAVING COUNT(customer_id) > 2;
```

### 以...排序

ORDER BY 设置返回结果的顺序。默认情况下，顺序是升序的。

```
SELECT name
FROM customers
ORDER BY age;
```

### DESC

DESC 将按降序返回结果。

```
SELECT name
FROM customers
ORDER BY age DESC;
```

### 抵消

OFFSET 语句使用 ORDER BY，并指定在开始从查询返回行之前要跳过的行数。

```
SELECT name
FROM customers
ORDER BY age
OFFSET 10 ROWS;
```

### 取得

FETCH 指定处理 OFFSET 子句后要返回的行数。OFFSET 子句是必需的，而 FETCH 子句是可选的。

```
SELECT name
FROM customers
ORDER BY age
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```

### 连接(内部、左侧、右侧、完全)

JOIN 子句用于合并两个或多个表中的行。四种类型的联接是内联接、左联接、右联接和全联接。

### 内部连接

内部联接选择在两个表中具有匹配值的记录。

```
SELECT name
FROM customers
INNER JOIN orders
ON customers.customer_id = orders.customer_id;
```

### 左连接

左连接从左表中选择与右表中的记录相匹配的记录。在下面的例子中，左边的表格是`customers`。

```
SELECT name
FROM customers
LEFT JOIN orders
ON customers.customer_id = orders.customer_id;
```

### 右连接

右联接从右表中选择与左表中的记录相匹配的记录。在下面的例子中，右边的表格是`orders`。

```
SELECT name
FROM customers
RIGHT JOIN orders
ON customers.customer_id = orders.customer_id;
```

### 完全连接

完全联接选择在左表或右表中有匹配项的记录。与“和”连接(内部连接)相比，可以将其视为“或”连接。

```
SELECT name
FROM customers
FULL OUTER JOIN orders
ON customers.customer_id = orders.customer_id;
```

### 存在

EXISTS 用于测试子查询中是否存在任何记录。

```
SELECT name
FROM customers
WHERE EXISTS
(SELECT order FROM ORDERS WHERE customer_id = 1);
```

### 同意

授予特定用户对数据库对象(如表、视图或数据库本身)的访问权限。下面的示例将向名为“usr_bob”的用户授予对 customers 表的选择和更新权限。

```
GRANT SELECT, UPDATE ON customers TO usr_bob;
```

### 取消

REVOKE 删除用户对特定数据库对象的权限。

```
REVOKE SELECT, UPDATE ON customers FROM usr_bob;
```

### 保存点

保存点允许您确定事务中的一个点，以后可以回滚到该点。类似于创建备份。

```
SAVEPOINT SAVEPOINT_NAME;
```

### 犯罪

提交用于将每个事务保存到数据库。COMMIT 语句将释放任何可能正在使用的现有保存点，一旦发出该语句，就不能回滚事务。

```
DELETE FROM customers
WHERE name = ‘Bob’;
COMMIT
```

### 反转

回滚用于撤消未保存到数据库的事务。这只能用于撤消自上次提交或回滚命令发出以来的事务。您还可以回滚到以前创建的保存点。

```
ROLLBACK TO SAVEPOINT_NAME;
```

### 缩短

TRUNCATE TABLE 从数据库的表中删除所有数据条目，但保留表和结构。类似于删除。

```
TRUNCATE TABLE customers;
```

### 联盟

UNION 使用两个或多个 SELECT 语句组合多个结果集，并消除重复行。

从客户工会中选择姓名从订单中选择姓名；

### 联合所有

UNION ALL 使用两个或多个 SELECT 语句组合多个结果集，并保留重复的行。

```
SELECT name FROM customers
UNION
SELECT name FROM orders;
```

我们希望这个页面可以作为 SQL 命令的快速参考指南。但是如果你真的想学习你的 SQL 技能，复制代码是不行的。查看[我们的 interactive SQL 课程](https://www.dataquest.io/path/sql-skills)并开始边做边学！

### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)