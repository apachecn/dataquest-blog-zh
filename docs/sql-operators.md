# SQL 运算符:6 种不同的类型(带有 45 个代码示例)

> 原文：<https://www.dataquest.io/blog/sql-operators/>

September 24, 2022![astronaut in shuttle cockpit](img/93557e8afe874b65aa798fd40f4c9df1.png)

本文将介绍什么是 SQL 操作符，各种类型的操作符，以及如何使用它们的许多不同的代码示例。

和任何新技能一样，人们喜欢以不同的方式学习。如果你不喜欢通过阅读博客文章来学习，你可能会喜欢我们的交互式 SQL 课程。在这里免费试用。

## 什么是 SQL 运算符？

SQL 运算符是用于执行任务的特殊单词或字符。这些任务可以是从复杂的比较到基本的算术运算。把 SQL 中的操作符想象成计算器功能上的不同按钮。

我们将讨论六种类型的 SQL 操作符:算术、按位、比较、复合、逻辑和字符串。

## 算术运算符

算术运算符用于对数值数据进行数学运算，如**加**或**减**。

### +(加法)

+符号将两个数相加。

```py
SELECT 10 + 10;
```

### –(减法)

–符号从一个数字中减去另一个数字。

```py
SELECT 10 - 10;
```

### *(乘法)

*符号是两个数的倍数。

```py
SELECT 10 * 10;
```

### /(除法)

/符号将一个数除以另一个数。

```py
SELECT 10 / 10;
```

### %(余数/模数)

%符号(有时称为模数)返回一个数除以另一个数的余数。

```py
SELECT 10 % 10;
```

## 按位运算符

位运算符在整数数据类型的两个表达式之间执行位操作。按位运算符将整数转换为二进制位，然后对每个单独的位执行**和(&符号)**、**或(|、^)** 或 **NOT (~)** 运算，最后将二进制结果转换回整数。

简单提醒一下:计算中的二进制数是由 0 和 1 组成的数字。

### &(按位与)

&符号(按位 AND)将一个值中的每个位与另一个值中相应的位进行比较。在下面的例子中，我们只使用了一位。因为@BitOne 的值不同于@BitTwo，所以返回 0。

```py
DECLARE @BitOne BIT = 1
DECLARE @BitTwo BIT = 0
SELECT @BitOne & @BitTwo;
```

但是如果我们让两者的价值相同呢？在这种情况下，它将返回 1。

```py
DECLARE @BitOne BIT = 1
DECLARE @BitTwo BIT = 1
SELECT @BitOne & @BitTwo;
```

显然这只是针对 BIT 类型的变量。如果我们开始使用数字，会发生什么？以下面的例子为例:

```py
DECLARE @BitOne INT = 230
DECLARE @BitTwo INT = 210
SELECT @BitOne & @BitTwo;
```

这里返回的答案将是 **194** 。

你可能会想，“怎么会是 194 呢？!"这完全可以理解。为了解释原因，我们首先需要将这两个数字转换成它们的二进制形式:

```py
@BitOne (230) - 11100110
@BitTwo (210) - 11010010
```

现在，我们必须遍历每个位并进行比较(因此@BitOne 中的第 1 位和@BitTwo 中的第 1 位)。如果两个数字都是 1，我们记录 1。如果一个或两个都是 0，那么我们记录一个 0:

```py
@BitOne (230) - 11100110
@BitTwo (210) - 11010010
Result        - 11000000
```

我们剩下的二进制数是 11000000，如果你谷歌一下，它等于一个数值 **194** 。

困惑了吗？放心吧！按位运算符可能很难理解，但在实践中很少使用。

### &=(按位 AND 赋值)

&=符号(按位 AND 赋值)的作用与按位 AND (&)运算符相同，但它将变量的值设置为返回的结果。

### |(按位或)

|符号(按位或)在两个值之间执行按位逻辑或运算。让我们重温一下之前的例子:

```py
DECLARE @BitOne INT = 230
DECLARE @BitTwo INT = 210
SELECT @BitOne | @BitTwo;
```

在这种情况下，我们必须再次检查每一位并进行比较，但这一次，如果任一数字为 1，则我们记录 1。如果两者都是 0，那么我们记录一个 0:

```py
@BitOne (230) - 11100110
@BitTwo (210) - 11010010
Result        - 11110110
```

我们剩下的二进制数是 11110110，等于数值 **246** 。

### |=(按位或赋值)

|=符号(按位“或”赋值)的作用与按位“或”(|)运算符相同，但它将变量的值设置为返回的结果。

### ^(按位异或)

^符号(按位异或)在两个值之间执行按位逻辑或运算。

```py
DECLARE @BitOne INT = 230
DECLARE @BitTwo INT = 210
SELECT @BitOne ^ @BitTwo;
```

在这个例子中，我们比较每个位，如果一个位等于 1，则返回 1，但不是两个位都等于 1。

```py
@BitOne (230) - 11100110
@BitTwo (210) - 11010010
Result        - 00110100
```

我们剩下的二进制数是 00110100，等于一个数值 **34** 。

### ^=(按位异或赋值)

^=符号(按位异或赋值)的作用与按位异或(^)运算符相同，但它将变量的值设置为返回的结果。

## 比较运算符

比较运算符用于比较两个值并测试它们是否相同。

### =(等于)

=符号用于过滤等于特定值的结果。在下面的示例中，该查询将返回年龄为 20 岁的所有客户。

```py
SELECT * FROM customers
WHERE age = 20;
```

### ！=(不等于)

的！=符号用于过滤不等于特定值的结果。在下面的示例中，该查询将返回年龄不到 20 岁的所有客户。

```py
SELECT * FROM customers
WHERE age != 20;
```

### >(大于)

>符号用于筛选列值大于查询值的结果。在下面的示例中，该查询将返回年龄在 20 岁以上的所有客户。

```py
SELECT * FROM customers
WHERE age > 20;
```

### ！>(不大于)

的！>符号用于过滤列值不大于查询值的结果。在下面的示例中，该查询将返回年龄不超过 20 岁的所有客户。

```py
SELECT * FROM customers
WHERE age !> 20;
```

### 

```py
SELECT * FROM customers
WHERE age < 20;
```

### ！

的！

```py
SELECT * FROM customers
WHERE age !< 20;
```

### > =(大于或等于)

> =符号用于筛选列值大于或等于查询值的结果。在下面的示例中，该查询将返回年龄等于或大于 20 岁的所有客户。

```py
SELECT * FROM customers
WHERE age >= 20;
```

### < =(小于或等于)

< =符号用于筛选列值小于或等于查询值的结果。在下面的示例中，该查询将返回年龄等于或小于 20 岁的所有客户。

```py
SELECT * FROM customers
WHERE age <= 20;
```

### <>(不等于)

<>符号执行的操作与！=符号，用于过滤不等于特定值的结果。您可以使用任何一种，但是<>是 SQL-92 标准。

```py
SELECT * FROM customers
WHERE age <> 20;
```

## 复合运算符

复合运算符对变量执行运算，然后将变量的结果设置为运算的结果。就当是做 a = a (+，-，*，等等)b。

### +=(加等于)

+=运算符将在原始值上加一个值，并将结果存储在原始值中。以下示例将值设置为 10，然后将该值加 5 并打印结果(15)。

```py
DECLARE @addValue int = 10
SET @addValue += 5
PRINT CAST(@addvalue AS VARCHAR);
```

这也可以用在字符串上。下面的例子将两个字符串连接在一起，并打印“dataquest”。

```py
DECLARE @addString VARCHAR(50) = “data”
SET @addString += “quest”
PRINT @addString;
```

### -=(减去等于)

-=运算符将从原始值中减去一个值，并将结果存储在原始值中。以下示例将值设置为 10，然后从该值中减去 5，并打印结果(5)。

```py
DECLARE @addValue int = 10
SET @addValue -= 5
PRINT CAST(@addvalue AS VARCHAR);
```

### *=(乘等于)

*=运算符将一个值乘以原始值，并将结果存储在原始值中。以下示例将值设置为 10，然后将其乘以 5 并打印结果(50)。

```py
DECLARE @addValue int = 10
SET @addValue *= 5
PRINT CAST(@addvalue AS VARCHAR);
```

### /=(除以等于)

/=运算符将一个值除以原始值，并将结果存储在原始值中。以下示例将值设置为 10，然后除以 5 并打印结果(2)。

```py
DECLARE @addValue int = 10
SET @addValue /= 5
PRINT CAST(@addvalue AS VARCHAR);
```

### %=(模等于)

%=运算符将一个值除以原始值，并将余数存储在原始值中。以下示例将值设置为 25，然后除以 5 并输出结果(0)。

```py
DECLARE @addValue int = 10
SET @addValue %= 5
PRINT CAST(@addvalue AS VARCHAR);
```

## 逻辑运算符

逻辑运算符是那些返回 true 或 false 的运算符，例如 AND 运算符，当两个表达式都满足时返回 true。

### 全部

如果所有子查询值都满足指定的条件，ALL 运算符将返回 TRUE。在下面的例子中，我们过滤了年龄大于伦敦用户最高年龄的所有用户。

```py
SELECT first_name, last_name, age, location
FROM users
WHERE age > ALL (SELECT age FROM users WHERE location = ‘London’);
```

### 任何/一些

如果任何子查询值满足指定的条件，ANY 运算符将返回 TRUE。在下面的例子中，我们过滤了在 orders 表中有任何记录的所有产品。SOME 操作符实现了相同的结果。

```py
SELECT product_name
FROM products
WHERE product_id > ANY (SELECT product_id FROM orders);
```

### 和

如果由 AND 分隔的所有条件都为真，则 AND 运算符返回 TRUE。在下面的例子中，我们正在过滤年龄为 20 岁、地点为伦敦的用户。

```py
SELECT *
FROM users
WHERE age = 20 AND location = ‘London’;
```

### 在...之间

BETWEEN 运算符筛选查询，只返回符合指定范围的结果。

```py
SELECT *
FROM users
WHERE age BETWEEN 20 AND 30;
```

### 存在

EXISTS 运算符用于通过查找子查询中是否存在任何记录来筛选数据。

```py
SELECT name
FROM customers
WHERE EXISTS
(SELECT order FROM ORDERS WHERE customer_id = 1);
```

### 在…里

IN 运算符包括设置在 WHERE 子句中的多个值。

```py
SELECT *
FROM users
WHERE first_name IN (‘Bob’, ‘Fred’, ‘Harry’);
```

### 喜欢

LIKE 运算符在列中搜索指定的模式。(关于这里如何/为什么使用%的更多信息，参见[关于通配符运算符](#tve-jump-1783c37eb2a)的部分)。

```py
SELECT *
FROM users
WHERE first_name LIKE ‘%Bob%’;
```

### 不

如果条件不为真，NOT 运算符将返回结果。

```py
SELECT *
FROM users
WHERE first_name NOT IN (‘Bob’, ‘Fred’, ‘Harry’);
```

### 运筹学

如果由 or 分隔的任何条件为真，OR 运算符将返回 TRUE。在下面的例子中，我们正在过滤年龄为 20 岁或居住在伦敦的用户。

```py
SELECT *
FROM users
WHERE age = 20 OR location = ‘London’;
```

### 为空

IS NULL 运算符用于筛选值为 NULL 的结果。

```py
SELECT *
FROM users
WHERE age IS NULL;
```

## 字符串运算符

字符串运算符主要用于字符串连接(将两个或多个字符串组合在一起)和字符串模式匹配。

### +(字符串连接)

+运算符可用于将两个或多个字符串组合在一起。以下示例将输出“dataquest”。

```py
SELECT ‘data’ + ‘quest’;
```

### +=(字符串串联赋值)

+=用于组合两个或多个字符串，并将结果存储在原始变量中。下面的示例设置了一个变量“data”，然后将“quest”添加到该变量中，给原始变量赋予值“dataquest”。

```py
DECLARE @strVar VARCHAR(50)
SET @strVar = ‘data’
SET @strVar += ‘quest’
PRINT @strVar;
```

### %(通配符)

%符号——有时称为通配符——用于匹配零个或多个字符的任何字符串。通配符可以用作前缀或后缀。在下面的示例中，查询将返回名字以“dan”开头的任何用户。

```py
SELECT *
FROM users
WHERE first_name LIKE ‘dan%’;
```

### [](个字符匹配)

[]用于匹配方括号中指定的特定范围或集合内的任何字符。在下面的例子中，我们搜索名字以 d 开头，第二个字符在 c 到 r 之间的任何用户。

```py
SELECT *
FROM users
WHERE first_name LIKE ‘d[c-r]%’’;
```

### [^](字符不匹配)

[^]用于匹配不在方括号中指定的特定范围或集合内的任何字符。在下面的例子中，我们搜索名字以 d 开头，第二个字符不是 a 的用户。

```py
SELECT *
FROM users
WHERE first_name LIKE ‘d[^a]%’’;
```

### _(通配符匹配一个字符)

_ 符号——有时称为下划线字符——用于在字符串比较操作中匹配任何单个字符。在下面的示例中，我们搜索第一个字符以 d 开头，第三个字符是 n 的任何用户。第二个字符可以是任何字母。

```py
SELECT *
FROM users
WHERE first_name LIKE ‘d_n%’;
```

## 更多有用的 SQL 资源:

*   [SQL 命令引用列表](https://www.dataquest.io/blog/sql-commands/)
*   [【SQL 备忘单(可下载 PDF)](https://www.dataquest.io/blog/sql-cheat-sheet/)
*   [SQL 面试题练习同](https://www.dataquest.io/blog/sql-interview-questions/)
*   [需要 SQL 认证吗？](https://www.dataquest.io/blog/sql-certification/)

或者，尝试一下最好的 SQL 学习资源:您可以在浏览器中直接学习的 interactive SQL 课程。注册一个免费帐户，开始学习吧！

### 用正确的方法学习 SQL！

*   编写真正的查询
*   使用真实数据
*   就在你的浏览器里！

当你可以 ***边做边学*** 的时候，为什么要被动的看视频讲座？

[Sign up & start learning!](https://app.dataquest.io/signup)