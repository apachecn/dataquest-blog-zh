# Python Datetime:包含 39 个代码示例的简单指南(2022)

> 原文：<https://www.dataquest.io/blog/python-datetime/>

May 11, 2022![Using the Datetime Package](img/56ee6d352fe86f74fa22eefae8a4cc05.png)

你可能已经熟悉了[Python 中的各种数据类型](https://www.dataquest.io/course/variables-data-types-and-lists-in-python/)，如整数、浮点数、字符串等。然而，有时当你开发一个脚本或机器学习算法时，你应该使用日期和时间。在日常生活中，我们可以用许多不同的格式来表示日期和时间，例如，`July 4`、`March 8, 2022`、`22:00`或`31 December 2022, 23:59:59`。它们使用整数和字符串的组合，或者您也可以使用浮点数来表示一天、一分钟等的分数。

但这不是唯一的复杂情况！此外，还列出了不同的时区、夏令时、不同的时间格式约定(在美国是 2022 年 7 月 23 日，在欧洲是 2023 年 7 月 23 日)等。

当我们告诉计算机做什么时，它们需要明确的精确度，但是日期和时间是一个挑战。幸运的是，Python 有`datetime`模块，允许我们轻松地操作表示日期和时间的对象。

在这个 Python `datetime`教程中，我们将学习以下内容:

1.  Python 中`datetime`模块的使用
2.  使用 Python `datetime`函数将字符串转换为`datetime`对象，反之亦然
3.  从`datetime`对象中提取日期和时间
4.  使用时间戳
5.  对日期和时间执行算术运算
6.  使用时区
7.  最后，创建一个倒计时器来确定离 2023 年新年还有多久(在纽约市！)

让我们开始用 Python 处理日期和时间。

**注意**:如果一段代码没有`import`语句，假设代码中使用的类/函数已经被导入。

## 如何在 Python 中使用 datetime？

正如我们前面所说，在编程中表示日期和时间是一个挑战。首先，我们必须用一种标准的、普遍接受的格式来表示它们。幸运的是， [*国际标准化组织*](https://www.iso.org/home.html) (ISO)开发了一个全球标准[【ISO 8601】](https://en.wikipedia.org/wiki/ISO_8601)，它将与日期和时间相关的对象表示为`YYYY-MM-DD HH:MM:SS`，其信息范围从最重要的(年，`YYYY`)到最不重要的(秒，`SS`)。这种格式的每一部分都表示为一个四位数或两位数。

Python 中的`datetime`模块有 5 个主要类(模块的一部分):

*   `date`操纵日期对象
*   `time`操纵时间物体
*   `datetime`是`date`和`time`的组合
*   `timedelta`允许我们用持续时间工作
*   `tzinfo`允许我们使用时区

此外，我们将使用 [`zoneinfo`模块](https://docs.python.org/3/library/zoneinfo.html)，它为我们提供了一种处理时区的现代方式，以及 [`dateutil`包](https://pypi.org/project/python-dateutil/)，它包含了大量处理日期和时间的有用函数。

让我们导入`datetime`模块并创建我们的第一个日期和时间对象:

```py
# From the datetime module import date
from datetime import date

# Create a date object of 2000-02-03
date(2000, 2, 3)
```

```py
datetime.date(2000, 2, 3)
```

在上面的代码中，我们从模块中导入了`date`类，然后创建了一个 2000 年 2 月 3 日的`datetime.date`对象。我们可以注意到，我们用来创建这个对象的数字顺序与 ISO 8061 中的完全相同(但是我们省略了 0，只写了一位数的月和日)。

编码完全是实验，所以假设我们想要创建一个 2000-26-03 的对象:

```py
# Create a date object of 2000-26-03
date(2000, 26, 3)
```

```py
---------------------------------------------------------------------------

ValueError                                Traceback (most recent call last)

Input In [2], in <module>
      1 # Create a date object of 2000-26-03
----> 2 date(2000, 26, 3)

ValueError: month must be in 1..12
```

我们得到`ValueError: month must be in 1..12`，这是有意义的，因为日历中没有 26 月。这个函数严格遵循 ISO 8601 格式。实际上，向`date`函数添加更多的参数，并注意会发生什么。

让我们看看如何创建一个`datetime.time`对象:

```py
# From the datetime module import time
from datetime import time

# Create a time object of 05:35:02
time(5, 35, 2)
```

```py
datetime.time(5, 35, 2)
```

我们成功了，你可以猜到这个函数也遵循 ISO 8061 格式。你能像我们之前用`date`函数破解它吗？

现在，如果我们希望在一个对象中既有日期又有时间呢？我们应该使用`datetime`类:

```py
# From the datetime module import datetime
from datetime import datetime

# Create a datetime object of 2000-02-03 05:35:02
datetime(2000, 2, 3, 5, 35, 2)
```

```py
datetime.datetime(2000, 2, 3, 5, 35, 2)
```

太好了，我们有了目标。我们还可以更明确地将关键字参数传递给`datetime`构造函数:

```py
datetime(year=2000, month=2, day=3, hour=5, minute=35, second=2)
```

```py
datetime.datetime(2000, 2, 3, 5, 35, 2)
```

如果我们只传入三个参数(年、月和日)会怎么样？这会导致错误吗？

```py
# Create a datetime object of 2000-02-03
datetime(2000, 2, 3)
```

```py
datetime.datetime(2000, 2, 3, 0, 0)
```

我们可以看到，现在对象中有两个零分别代表小时和分钟。在这种情况下，秒被省略。

在很多情况下，我们希望知道此刻的准确时间。我们可以使用`datetime`类的`now()`方法:

```py
# Time at the moment
now = datetime.now()
now
```

```py
datetime.datetime(2022, 2, 14, 11, 33, 25, 516707)
```

我们得到一个`datetime`对象。这里最后一个数字是微秒。

如果我们只需要今天的日期，我们可以使用来自`date`类的`today()`方法:

```py
today = date.today()
today
```

```py
datetime.date(2022, 2, 14)
```

如果只是需要时间，我们可能会写`time.now()`，但是行不通。相反，我们必须访问`datetime.now()`对象的`hour`、`minute`和`second`属性，并将它们传递给`time`构造函数:

```py
time(now.hour, now.minute, now.second)
```

```py
datetime.time(11, 33, 25)
```

在实践中，从 Python 中的`datetime`对象获取日期(分配给`now`变量)。

我们还可以使用`isocalendar()`函数从`datetime`对象中提取星期数和日期。它将返回一个包含 ISO 年份、周数和工作日数的三项元组:

```py
# isocalendar() returns a 3-item tuple with ISO year, week number, and weekday number
now.isocalendar()
```

```py
datetime.IsoCalendarDate(year=2022, week=7, weekday=1)
```

在 ISO 格式中，一周从星期一开始，到星期日结束。一周中的每一天都用从 1(星期一)到 7(星期日)的数字编码。如果我们想要访问这些元组元素中的一个，我们使用*括号符号*:

```py
# Access week number
week_number = now.isocalendar()[1]
week_number
```

```py
7
```

## 从字符串中提取日期

在数据科学中，通常在编程中，我们主要处理日期和时间，这些日期和时间以数十种不同的格式存储为字符串，这取决于地区、公司或我们需要的信息的粒度。有时，我们需要日期和确切的时间，但在其他情况下，我们只需要年份和月份。如何从一个字符串中提取出我们需要的数据，方便地把它作为一个`datetime` ( `date`，`time`)对象来操作呢？

### *fromisoformat()* 和 *isoformat()*

我们将学习的第一个将日期字符串转换成`date`对象的函数是`fromisoformat`。我们这样称呼它是因为它使用了 ISO 8601 格式(即`YYYY-MM-DD`)。让我们看一个例子:

```py
# Convert a date string into a date object
date.fromisoformat("2022-12-31")
```

```py
datetime.date(2022, 12, 31)
```

ISO 格式也包含时间，但是如果我们把它传递给函数，它就不起作用了:

```py
date.fromisoformat("2022-12-31 00:00:00")
```

```py
---------------------------------------------------------------------------

ValueError                                Traceback (most recent call last)

Input In [13], in <module>
----> 1 date.fromisoformat("2022-12-31 00:00:00")

ValueError: Invalid isoformat string: '2022-12-31 00:00:00'
```

当然，我们也可以执行相反的操作，将一个`datetime`对象转换成 ISO 格式的日期字符串。为此，我们应该使用`isoformat()`:

```py
# Convert a datetime object into a string in the ISO format
date(2022, 12, 31).isoformat()
```

```py
'2022-12-31'
```

### *strptime()*

为了解决上面的`ValueError`问题，我们可以使用`strptime()`函数，该函数可以将一个*任意的*日期/时间字符串转换成一个`datetime`对象。我们的字符串不一定需要遵循 ISO 格式，但是我们应该指定字符串的哪一部分代表哪一个日期或时间单位(年、小时等)。).让我们看一个例子来阐明。首先，我们将使用严格的 ISO 格式将一个字符串转换成一个`datetime`对象:

```py
# Date as a string
iso_date = "2022-12-31 23:59:58"

# ISO format
iso_format = "%Y-%m-%d %H:%M:%S"

# Convert the string into a datetime object
datetime.strptime(iso_date, iso_format)
```

```py
datetime.datetime(2022, 12, 31, 23, 59, 58)
```

在**的第一行**，我们创建一个日期/时间字符串。在**的第二行**中，我们使用一个特殊的代码指定字符串的**格式**，该代码包含一个百分号，后跟一个编码日期或时间单位的字符。最后，在**第三行**中，我们使用`strptime()`函数将字符串转换为`datetime`对象。这个函数有两个参数:字符串和字符串的格式。

我们上面使用的代码也可以对其他日期和时间单位进行编码，如工作日、月份名称、周数等。

| 密码 | 例子 | 描述 |
| --- | --- | --- |
| %A | 星期一 | 完整的工作日名称 |
| %B | 十二月 | 完整的月份名称 |
| %W | Two | 周数(星期一是一周的第一天) |

Python 中使用的其他代码可以参考这个[网站](https://strftime.org/)。

让我们再看几个使用其他格式的例子:

```py
# European date as a string
european_date = "31-12-2022"

# European format
european_format = "%d-%m-%Y"

# Convert the string into a datetime object
datetime.strptime(european_date, european_format)
```

```py
datetime.datetime(2022, 12, 31, 0, 0)
```

正如我们在上面看到的，字符串被成功转换，但是我们有额外的零来表示时间。让我们看一个使用其他代码的示例:

```py
# Full month name date
full_month_date = "12 September 2022"

# Full month format
full_month_format = "%d %B %Y"

# Convert the string into a datetime object
datetime.strptime(full_month_date, full_month_format)
```

```py
datetime.datetime(2022, 9, 12, 0, 0)
```

再次成功！注意**我们定义的格式应该匹配日期字符串**的格式。因此，如果我们用空格、冒号、连字符或其他字符来分隔时间单位，它们应该在代码字符串中。否则，Python 会抛出一个`ValueError`:

```py
# Full month name date
full_month_date = "12 September 2022"

# Wrong format (missing space)
full_month_format = "%d%B %Y"

# Convert the string into a datetime object
datetime.strptime(full_month_date, full_month_format)
```

```py
---------------------------------------------------------------------------

ValueError                                Traceback (most recent call last)

Input In [18], in <module>
      5 full_month_format = "%d%B %Y"
      7 # Convert the string into a datetime object
----> 8 datetime.strptime(full_month_date, full_month_format)

File ~/coding/dataquest/articles/using-the-datetime-package/env/lib/python3.10/_strptime.py:568, in _strptime_datetime(cls, data_string, format)
    565 def _strptime_datetime(cls, data_string, format="%a %b %d %H:%M:%S %Y"):
    566     """Return a class cls instance based on the input string and the
    567     format string."""
--> 568     tt, fraction, gmtoff_fraction = _strptime(data_string, format)
    569     tzname, gmtoff = tt[-2:]
    570     args = tt[:6] + (fraction,)

File ~/coding/dataquest/articles/using-the-datetime-package/env/lib/python3.10/_strptime.py:349, in _strptime(data_string, format)
    347 found = format_regex.match(data_string)
    348 if not found:
--> 349     raise ValueError("time data %r does not match format %r" %
    350                      (data_string, format))
    351 if len(data_string) != found.end():
    352     raise ValueError("unconverted data remains: %s" %
    353                       data_string[found.end():])

ValueError: time data '12 September 2022' does not match format '%d%B %Y'
```

甚至一个空格都可能导致错误！

## 将日期时间对象转换为字符串

### *strftime()*

在 Python 中，我们还可以使用`strftime()`函数将`datetime`对象转换成字符串。它有两个参数:一个`datetime`对象和输出字符串的格式。

```py
# Create a datetime object
datetime_obj = datetime(2022, 12, 31)

# American date format
american_format = "%m-%d-%Y"

# European format
european_format = "%d-%m-%Y"

# American date string
print(f"American date string: {datetime.strftime(datetime_obj, american_format)}.")

# European date string
print(f"European date string: {datetime.strftime(datetime_obj, european_format)}.")
```

```py
American date string: 12-31-2022.
European date string: 31-12-2022.
```

我们将同一个`datetime`对象转换成两种不同的格式。我们还可以指定其他格式，比如完整的月份名称后面跟着日期和年份。

```py
full_month = "%B %d, %Y"
datetime.strftime(datetime_obj, full_month)
```

```py
'December 31, 2022'
```

使用`strftime`的另一种方法是将其放在`datetime`对象之后:

```py
datetime_obj = datetime(2022, 12, 31, 23, 59, 59)
full_datetime = "%B %d, %Y %H:%M:%S"
datetime_obj.strftime(full_datetime)
```

```py
'December 31, 2022 23:59:59'
```

实际上，如果我们想要提取不同年份中 12 月 31 日的工作日名称，`strftime()`可能会很有用:

```py
# Extract the weekday name of December 31
weekday_format = "%A"

for year in range(2022, 2026):
    print(f"Weekday of December 31, {year} is {date(year, 12, 31).strftime(weekday_format)}.")
```

```py
Weekday of December 31, 2022 is Saturday.
Weekday of December 31, 2023 is Sunday.
Weekday of December 31, 2024 is Tuesday.
Weekday of December 31, 2025 is Wednesday.
```

## 时间戳

在编程中，经常会看到日期和时间以 [Unix 时间戳格式](https://en.wikipedia.org/wiki/Unix_time)存储。这种格式用数字表示任何日期。基本上，时间戳是从 1970 年 1 月 1 日 00:00:00 UTC(协调世界时)开始的 *Unix 纪元*过去的秒数。我们可以使用`timestamp()`函数来计算这个数字:

```py
new_year_2023 = datetime(2022, 12, 31)
datetime.timestamp(new_year_2023)
```

```py
1672441200.0
```

1672441200 是 Unix 纪元开始到 2022 年 12 月 31 日之间的秒数。

我们可以通过使用`fromtimestamp()`功能来执行相反的操作:

```py
datetime.fromtimestamp(1672441200)
```

```py
datetime.datetime(2022, 12, 31, 0, 0)
```

## 带日期的算术运算

有时，我们可能希望计算两个日期之间的差值，或者对日期和时间执行其他算术运算。幸运的是，Python 的工具箱中有许多工具来执行这样的计算。

### 基本算术运算

我们可以执行的第一个操作是计算两个日期之间的差异。为此，我们使用减号:

```py
# Instatiate two dates
first_date = date(2022, 1, 1)
second_date = date(2022, 12, 31)

# Difference between two dates
date_diff = second_date - first_date

# Function to convert datetime to string
def dt_string(date, date_format="%B %d, %Y"):
    return date.strftime(date_format)

print(f"The number of days and hours between {dt_string(first_date)} and {dt_string(second_date)} is {date_diff}.")
```

```py
The number of days and hours between January 01, 2022 and December 31, 2022 is 364 days, 0:00:00.
```

让我们看看``first_date - second_date`` 返回什么类型的对象:

```py
type(date_diff)
```

```py
datetime.timedelta
```

这个对象的类型是`datetime.timedelta`。它的名字中有 **delta** ，它指的是一个绿色的字母 *delta* ，$\Delta$，在科学和工程中，它描述了一种变化。的确，在这里它代表了时间的变化(差异)。

如果我们只对两次约会之间的天数感兴趣呢？我们可以访问`timedelta`对象的不同属性，其中一个叫做`.days`。

```py
print(f"The number of days between {dt_string(first_date)} and {dt_string(second_date)} is {(date_diff).days}.")
```

```py
The number of days between January 01, 2022 and December 31, 2022 is 364.
```

### *时间差()*

现在我们已经知道了`timedelta`对象，是时候介绍一下`timedelta()`函数了。它允许我们对时间对象执行许多算术运算，通过加减时间单位，如日、年、周、秒等。例如，我们可能想知道 30 天后是星期几。为此，我们必须创建一个表示当前时间的对象和一个定义我们添加的时间量的`timedelta`对象:

```py
# Import timedelta
from datetime import timedelta

# Current time
now = datetime.now()

# timedelta of 30 days
one_month = timedelta(days=30)

# Day in one month/using dt_string function defined above
print(f"The day in 30 days is {dt_string(now + one_month)}.")
```

```py
The day in 30 days is March 16, 2022.
```

如果我们查看`timedelta`函数(`help(timedelta)`)的帮助页面，我们会看到它有以下参数:`days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0`。所以，你可以练习给一个日期加上或减去其他时间单位。例如，我们可以计算 2030 年新年前 12 小时的时间:

```py
# New year 2030
new_year_2030 = datetime(2030, 1, 1, 0, 0)

# timedelta of 12 hours
twelve_hours = timedelta(hours=12)

# Time string of 12 hours before New Year 2023
twelve_hours_before = (new_year_2030 - twelve_hours).strftime("%B %d, %Y, %H:%M:%S")

# Print the time 12 hours before New Year 2023
print(f"The time twelve hours before New Year 2030 will be {twelve_hours_before}.")
```

```py
The time twelve hours before New Year 2030 will be December 31, 2029, 12:00:00.
```

我们还可以组合`timedelta()`函数的多个参数来计算一个更具体的时间。例如，从现在起 27 天 3 小时 45 分钟后的时间是什么？

```py
# Current time
now = datetime.now()

# Timedelta of 27 days, 3 hours, and 45 minutes
specific_timedelta = timedelta(days=27, hours=3, minutes=45)

# Time in 27 days, 3 hours, and 45 minutes
twenty_seven_days = (now + specific_timedelta).strftime("%B %d, %Y, %H:%M:%S")

print(f"The time in 27 days, 3 hours, and 45 minutes will be {twenty_seven_days}.")
```

```py
The time in 27 days, 3 hours, and 45 minutes will be March 13, 2022, 15:18:39.
```

### *相对的()*

不幸的是，我们可以从帮助页面看到，该函数不允许我们使用月或年。为了克服这个限制，我们可以使用的 [`dateutil`包中的`relativedelta`函数。这个功能与`timedelta()`非常相似，但是它扩展了它的功能。](https://pypi.org/project/python-dateutil/)

例如，我们想从当前时间中减去 2 年 3 个月 4 天 5 小时:

```py
# Import relativedelta
from dateutil.relativedelta import relativedelta

# Current time
now = datetime.now()

# relativedelta object
relative_delta = relativedelta(years=2, months=3, days=4, hours=5)

two_years = (now - relative_delta).strftime("%B %d, %Y, %H:%M:%S")

print(f"The time 2 years, 3 months, 4 days, and 5 hours ago was {two_years}.")
```

```py
The time 2 years, 3 months, 4 days, and 5 hours ago was November 10, 2019, 06:33:40.
```

我们还可以使用`relativedelta()`来计算两个`datetime`对象之间的差异:

```py
relativedelta(datetime(2030, 12, 31), now)
```

```py
relativedelta(years=+8, months=+10, days=+16, hours=+12, minutes=+26, seconds=+19, microseconds=+728345)
```

这些算术运算可能看起来非常抽象和不切实际，但实际上，它们在许多应用中是有用的。一个这样的场景可能如下所示。

比如说，我们脚本中的某个操作应该在特定日期前 30 天执行。我们可以定义一个保存当前时间的变量，并向它添加一个 30 天的`timedelta`对象。如果今天是这一天，操作将被唤起！

否则，想象我们正在使用[熊猫](https://www.dataquest.io/course/pandas-fundamentals/)处理数据集，其中一列包含一些日期。假设我们有一个数据集，其中保存了公司一年中每天的利润。我们希望创建另一个数据集，该数据集将保存从当前日期算起正好一年的日期以及这些日期中每一天的预测利润。我们一定会在日期上使用算术计算！

迟早，你会在你的[数据科学项目](https://www.dataquest.io/data-science-projects/)中遇到日期和时间。当你这样做的时候，回到这个教程。

## 使用时区

正如我们在介绍中提到的，在编程中使用时间的一个问题是处理时区。它们可以有不同的形状。我们还应该知道，有些地区实行夏令时(DST)，而[有些地区不实行](https://www.timeanddate.com/time/us/arizona-no-dst.html)。

Python 区分了两类日期时间对象: [*天真的*和*觉察的*](https://docs.python.org/3/library/datetime.html#aware-and-naive-objects) 。简单对象不保存任何关于时区的信息，而感知对象保存这些信息。

首先，我们来看一个幼稚的时间对象:

```py
# Import tzinfo
from datetime import tzinfo

# Naive datetime
naive_datetime = datetime.now()

# Naive datetime doesn't hold any timezone information
type(naive_datetime.tzinfo)
```

```py
NoneType
```

从 Python 3.9 开始，我们有了一个使用 [*互联网号码分配机构*](https://www.iana.org/) 数据库的时区的具体实现。实现这个功能的模块叫做 [`zoneinfo`](https://docs.python.org/3/library/zoneinfo.html) 。注意，如果你使用的是以前版本的 Python，你可以使用 [`pytz`](https://pypi.org/project/pytz/) 或者 [`dateutil`](https://pypi.org/project/python-dateutil/) 包。

让我们使用`zoneinfo`创建一个 aware `datetime`对象，特别是`ZoneInfo`类，它是`datetime.tzinfo`抽象类的一个实现:

```py
# Import ZoneInfo 
from zoneinfo import ZoneInfo

utc_tz = ZoneInfo("UTC")

# Aware datetime object with UTC timezone
dt_utc = datetime.now(tz=utc_tz)

# The type of an aware object implemented with ZoneInfo is zoneinfo.ZoneInfo
type(dt_utc.tzinfo)
```

```py
zoneinfo.ZoneInfo
```

我们可以看到一个 aware `datetime`对象拥有关于时区的信息(实现为一个`zoneinfo.ZoneInfo`对象)。

我们来看几个例子。我们想确定中欧和加利福尼亚的当前时间。

首先，我们可以在`zoneinfo`中列出所有可用的时区:

```py
import zoneinfo

# Will return a long list of timezones (opens many files!)
zoneinfo.available_timezones() 
```

```py
{'Africa/Abidjan',
 'Africa/Accra',
 'Africa/Addis_Ababa',
 'Africa/Algiers',
 'Africa/Asmara',
 'Africa/Asmera',
 'Africa/Bamako',
 'Africa/Bangui',
 'Africa/Banjul',
 'Africa/Bissau',
 'Africa/Blantyre',
 'Africa/Brazzaville',
 'Africa/Bujumbura',
 'Africa/Cairo',
 'Africa/Casablanca',
 'Africa/Ceuta',
 'Africa/Conakry',
 'Africa/Dakar',
 'Africa/Dar_es_Salaam',
 'Africa/Djibouti',
 'Africa/Douala',
 'Africa/El_Aaiun',
 'Africa/Freetown',
 'Africa/Gaborone',
 'Africa/Harare',
 'Africa/Johannesburg',
 'Africa/Juba',
 'Africa/Kampala',
 'Africa/Khartoum',
 'Africa/Kigali',
 'Africa/Kinshasa',
 'Africa/Lagos',
 'Africa/Libreville',
 'Africa/Lome',
 'Africa/Luanda',
 'Africa/Lubumbashi',
 'Africa/Lusaka',
 'Africa/Malabo',
 'Africa/Maputo',
 'Africa/Maseru',
 'Africa/Mbabane',
 'Africa/Mogadishu',
 'Africa/Monrovia',
 'Africa/Nairobi',
 'Africa/Ndjamena',
 'Africa/Niamey',
 'Africa/Nouakchott',
 'Africa/Ouagadougou',
 'Africa/Porto-Novo',
 'Africa/Sao_Tome',
 'Africa/Timbuktu',
 'Africa/Tripoli',
 'Africa/Tunis',
 'Africa/Windhoek',
 'America/Adak',
 'America/Anchorage',
 'America/Anguilla',
 'America/Antigua',
 'America/Araguaina',
 'America/Argentina/Buenos_Aires',
 'America/Argentina/Catamarca',
 'America/Argentina/ComodRivadavia',
 'America/Argentina/Cordoba',
 'America/Argentina/Jujuy',
 'America/Argentina/La_Rioja',
 'America/Argentina/Mendoza',
 'America/Argentina/Rio_Gallegos',
 'America/Argentina/Salta',
 'America/Argentina/San_Juan',
 'America/Argentina/San_Luis',
 'America/Argentina/Tucuman',
 'America/Argentina/Ushuaia',
 'America/Aruba',
 'America/Asuncion',
 'America/Atikokan',
 'America/Atka',
 'America/Bahia',
 'America/Bahia_Banderas',
 'America/Barbados',
 'America/Belem',
 'America/Belize',
 'America/Blanc-Sablon',
 'America/Boa_Vista',
 'America/Bogota',
 'America/Boise',
 'America/Buenos_Aires',
 'America/Cambridge_Bay',
 'America/Campo_Grande',
 'America/Cancun',
 'America/Caracas',
 'America/Catamarca',
 'America/Cayenne',
 'America/Cayman',
 'America/Chicago',
 'America/Chihuahua',
 'America/Coral_Harbour',
 'America/Cordoba',
 'America/Costa_Rica',
 'America/Creston',
 'America/Cuiaba',
 'America/Curacao',
 'America/Danmarkshavn',
 'America/Dawson',
 'America/Dawson_Creek',
 'America/Denver',
 'America/Detroit',
 'America/Dominica',
 'America/Edmonton',
 'America/Eirunepe',
 'America/El_Salvador',
 'America/Ensenada',
 'America/Fort_Nelson',
 'America/Fort_Wayne',
 'America/Fortaleza',
 'America/Glace_Bay',
 'America/Godthab',
 'America/Goose_Bay',
 'America/Grand_Turk',
 'America/Grenada',
 'America/Guadeloupe',
 'America/Guatemala',
 'America/Guayaquil',
 'America/Guyana',
 'America/Halifax',
 'America/Havana',
 'America/Hermosillo',
 'America/Indiana/Indianapolis',
 'America/Indiana/Knox',
 'America/Indiana/Marengo',
 'America/Indiana/Petersburg',
 'America/Indiana/Tell_City',
 'America/Indiana/Vevay',
 'America/Indiana/Vincennes',
 'America/Indiana/Winamac',
 'America/Indianapolis',
 'America/Inuvik',
 'America/Iqaluit',
 'America/Jamaica',
 'America/Jujuy',
 'America/Juneau',
 'America/Kentucky/Louisville',
 'America/Kentucky/Monticello',
 'America/Knox_IN',
 'America/Kralendijk',
 'America/La_Paz',
 'America/Lima',
 'America/Los_Angeles',
 'America/Louisville',
 'America/Lower_Princes',
 'America/Maceio',
 'America/Managua',
 'America/Manaus',
 'America/Marigot',
 'America/Martinique',
 'America/Matamoros',
 'America/Mazatlan',
 'America/Mendoza',
 'America/Menominee',
 'America/Merida',
 'America/Metlakatla',
 'America/Mexico_City',
 'America/Miquelon',
 'America/Moncton',
 'America/Monterrey',
 'America/Montevideo',
 'America/Montreal',
 'America/Montserrat',
 'America/Nassau',
 'America/New_York',
 'America/Nipigon',
 'America/Nome',
 'America/Noronha',
 'America/North_Dakota/Beulah',
 'America/North_Dakota/Center',
 'America/North_Dakota/New_Salem',
 'America/Nuuk',
 'America/Ojinaga',
 'America/Panama',
 'America/Pangnirtung',
 'America/Paramaribo',
 'America/Phoenix',
 'America/Port-au-Prince',
 'America/Port_of_Spain',
 'America/Porto_Acre',
 'America/Porto_Velho',
 'America/Puerto_Rico',
 'America/Punta_Arenas',
 'America/Rainy_River',
 'America/Rankin_Inlet',
 'America/Recife',
 'America/Regina',
 'America/Resolute',
 'America/Rio_Branco',
 'America/Rosario',
 'America/Santa_Isabel',
 'America/Santarem',
 'America/Santiago',
 'America/Santo_Domingo',
 'America/Sao_Paulo',
 'America/Scoresbysund',
 'America/Shiprock',
 'America/Sitka',
 'America/St_Barthelemy',
 'America/St_Johns',
 'America/St_Kitts',
 'America/St_Lucia',
 'America/St_Thomas',
 'America/St_Vincent',
 'America/Swift_Current',
 'America/Tegucigalpa',
 'America/Thule',
 'America/Thunder_Bay',
 'America/Tijuana',
 'America/Toronto',
 'America/Tortola',
 'America/Vancouver',
 'America/Virgin',
 'America/Whitehorse',
 'America/Winnipeg',
 'America/Yakutat',
 'America/Yellowknife',
 'Antarctica/Casey',
 'Antarctica/Davis',
 'Antarctica/DumontDUrville',
 'Antarctica/Macquarie',
 'Antarctica/Mawson',
 'Antarctica/McMurdo',
 'Antarctica/Palmer',
 'Antarctica/Rothera',
 'Antarctica/South_Pole',
 'Antarctica/Syowa',
 'Antarctica/Troll',
 'Antarctica/Vostok',
 'Arctic/Longyearbyen',
 'Asia/Aden',
 'Asia/Almaty',
 'Asia/Amman',
 'Asia/Anadyr',
 'Asia/Aqtau',
 'Asia/Aqtobe',
 'Asia/Ashgabat',
 'Asia/Ashkhabad',
 'Asia/Atyrau',
 'Asia/Baghdad',
 'Asia/Bahrain',
 'Asia/Baku',
 'Asia/Bangkok',
 'Asia/Barnaul',
 'Asia/Beirut',
 'Asia/Bishkek',
 'Asia/Brunei',
 'Asia/Calcutta',
 'Asia/Chita',
 'Asia/Choibalsan',
 'Asia/Chongqing',
 'Asia/Chungking',
 'Asia/Colombo',
 'Asia/Dacca',
 'Asia/Damascus',
 'Asia/Dhaka',
 'Asia/Dili',
 'Asia/Dubai',
 'Asia/Dushanbe',
 'Asia/Famagusta',
 'Asia/Gaza',
 'Asia/Harbin',
 'Asia/Hebron',
 'Asia/Ho_Chi_Minh',
 'Asia/Hong_Kong',
 'Asia/Hovd',
 'Asia/Irkutsk',
 'Asia/Istanbul',
 'Asia/Jakarta',
 'Asia/Jayapura',
 'Asia/Jerusalem',
 'Asia/Kabul',
 'Asia/Kamchatka',
 'Asia/Karachi',
 'Asia/Kashgar',
 'Asia/Kathmandu',
 'Asia/Katmandu',
 'Asia/Khandyga',
 'Asia/Kolkata',
 'Asia/Krasnoyarsk',
 'Asia/Kuala_Lumpur',
 'Asia/Kuching',
 'Asia/Kuwait',
 'Asia/Macao',
 'Asia/Macau',
 'Asia/Magadan',
 'Asia/Makassar',
 'Asia/Manila',
 'Asia/Muscat',
 'Asia/Nicosia',
 'Asia/Novokuznetsk',
 'Asia/Novosibirsk',
 'Asia/Omsk',
 'Asia/Oral',
 'Asia/Phnom_Penh',
 'Asia/Pontianak',
 'Asia/Pyongyang',
 'Asia/Qatar',
 'Asia/Qostanay',
 'Asia/Qyzylorda',
 'Asia/Rangoon',
 'Asia/Riyadh',
 'Asia/Saigon',
 'Asia/Sakhalin',
 'Asia/Samarkand',
 'Asia/Seoul',
 'Asia/Shanghai',
 'Asia/Singapore',
 'Asia/Srednekolymsk',
 'Asia/Taipei',
 'Asia/Tashkent',
 'Asia/Tbilisi',
 'Asia/Tehran',
 'Asia/Tel_Aviv',
 'Asia/Thimbu',
 'Asia/Thimphu',
 'Asia/Tokyo',
 'Asia/Tomsk',
 'Asia/Ujung_Pandang',
 'Asia/Ulaanbaatar',
 'Asia/Ulan_Bator',
 'Asia/Urumqi',
 'Asia/Ust-Nera',
 'Asia/Vientiane',
 'Asia/Vladivostok',
 'Asia/Yakutsk',
 'Asia/Yangon',
 'Asia/Yekaterinburg',
 'Asia/Yerevan',
 'Atlantic/Azores',
 'Atlantic/Bermuda',
 'Atlantic/Canary',
 'Atlantic/Cape_Verde',
 'Atlantic/Faeroe',
 'Atlantic/Faroe',
 'Atlantic/Jan_Mayen',
 'Atlantic/Madeira',
 'Atlantic/Reykjavik',
 'Atlantic/South_Georgia',
 'Atlantic/St_Helena',
 'Atlantic/Stanley',
 'Australia/ACT',
 'Australia/Adelaide',
 'Australia/Brisbane',
 'Australia/Broken_Hill',
 'Australia/Canberra',
 'Australia/Currie',
 'Australia/Darwin',
 'Australia/Eucla',
 'Australia/Hobart',
 'Australia/LHI',
 'Australia/Lindeman',
 'Australia/Lord_Howe',
 'Australia/Melbourne',
 'Australia/NSW',
 'Australia/North',
 'Australia/Perth',
 'Australia/Queensland',
 'Australia/South',
 'Australia/Sydney',
 'Australia/Tasmania',
 'Australia/Victoria',
 'Australia/West',
 'Australia/Yancowinna',
 'Brazil/Acre',
 'Brazil/DeNoronha',
 'Brazil/East',
 'Brazil/West',
 'CET',
 'CST6CDT',
 'Canada/Atlantic',
 'Canada/Central',
 'Canada/Eastern',
 'Canada/Mountain',
 'Canada/Newfoundland',
 'Canada/Pacific',
 'Canada/Saskatchewan',
 'Canada/Yukon',
 'Chile/Continental',
 'Chile/EasterIsland',
 'Cuba',
 'EET',
 'EST',
 'EST5EDT',
 'Egypt',
 'Eire',
 'Etc/GMT',
 'Etc/GMT+0',
 'Etc/GMT+1',
 'Etc/GMT+10',
 'Etc/GMT+11',
 'Etc/GMT+12',
 'Etc/GMT+2',
 'Etc/GMT+3',
 'Etc/GMT+4',
 'Etc/GMT+5',
 'Etc/GMT+6',
 'Etc/GMT+7',
 'Etc/GMT+8',
 'Etc/GMT+9',
 'Etc/GMT-0',
 'Etc/GMT-1',
 'Etc/GMT-10',
 'Etc/GMT-11',
 'Etc/GMT-12',
 'Etc/GMT-13',
 'Etc/GMT-14',
 'Etc/GMT-2',
 'Etc/GMT-3',
 'Etc/GMT-4',
 'Etc/GMT-5',
 'Etc/GMT-6',
 'Etc/GMT-7',
 'Etc/GMT-8',
 'Etc/GMT-9',
 'Etc/GMT0',
 'Etc/Greenwich',
 'Etc/UCT',
 'Etc/UTC',
 'Etc/Universal',
 'Etc/Zulu',
 'Europe/Amsterdam',
 'Europe/Andorra',
 'Europe/Astrakhan',
 'Europe/Athens',
 'Europe/Belfast',
 'Europe/Belgrade',
 'Europe/Berlin',
 'Europe/Bratislava',
 'Europe/Brussels',
 'Europe/Bucharest',
 'Europe/Budapest',
 'Europe/Busingen',
 'Europe/Chisinau',
 'Europe/Copenhagen',
 'Europe/Dublin',
 'Europe/Gibraltar',
 'Europe/Guernsey',
 'Europe/Helsinki',
 'Europe/Isle_of_Man',
 'Europe/Istanbul',
 'Europe/Jersey',
 'Europe/Kaliningrad',
 'Europe/Kiev',
 'Europe/Kirov',
 'Europe/Lisbon',
 'Europe/Ljubljana',
 'Europe/London',
 'Europe/Luxembourg',
 'Europe/Madrid',
 'Europe/Malta',
 'Europe/Mariehamn',
 'Europe/Minsk',
 'Europe/Monaco',
 'Europe/Moscow',
 'Europe/Nicosia',
 'Europe/Oslo',
 'Europe/Paris',
 'Europe/Podgorica',
 'Europe/Prague',
 'Europe/Riga',
 'Europe/Rome',
 'Europe/Samara',
 'Europe/San_Marino',
 'Europe/Sarajevo',
 'Europe/Saratov',
 'Europe/Simferopol',
 'Europe/Skopje',
 'Europe/Sofia',
 'Europe/Stockholm',
 'Europe/Tallinn',
 'Europe/Tirane',
 'Europe/Tiraspol',
 'Europe/Ulyanovsk',
 'Europe/Uzhgorod',
 'Europe/Vaduz',
 'Europe/Vatican',
 'Europe/Vienna',
 'Europe/Vilnius',
 'Europe/Volgograd',
 'Europe/Warsaw',
 'Europe/Zagreb',
 'Europe/Zaporozhye',
 'Europe/Zurich',
 'Factory',
 'GB',
 'GB-Eire',
 'GMT',
 'GMT+0',
 'GMT-0',
 'GMT0',
 'Greenwich',
 'HST',
 'Hongkong',
 'Iceland',
 'Indian/Antananarivo',
 'Indian/Chagos',
 'Indian/Christmas',
 'Indian/Cocos',
 'Indian/Comoro',
 'Indian/Kerguelen',
 'Indian/Mahe',
 'Indian/Maldives',
 'Indian/Mauritius',
 'Indian/Mayotte',
 'Indian/Reunion',
 'Iran',
 'Israel',
 'Jamaica',
 'Japan',
 'Kwajalein',
 'Libya',
 'MET',
 'MST',
 'MST7MDT',
 'Mexico/BajaNorte',
 'Mexico/BajaSur',
 'Mexico/General',
 'NZ',
 'NZ-CHAT',
 'Navajo',
 'PRC',
 'PST8PDT',
 'Pacific/Apia',
 'Pacific/Auckland',
 'Pacific/Bougainville',
 'Pacific/Chatham',
 'Pacific/Chuuk',
 'Pacific/Easter',
 'Pacific/Efate',
 'Pacific/Enderbury',
 'Pacific/Fakaofo',
 'Pacific/Fiji',
 'Pacific/Funafuti',
 'Pacific/Galapagos',
 'Pacific/Gambier',
 'Pacific/Guadalcanal',
 'Pacific/Guam',
 'Pacific/Honolulu',
 'Pacific/Johnston',
 'Pacific/Kanton',
 'Pacific/Kiritimati',
 'Pacific/Kosrae',
 'Pacific/Kwajalein',
 'Pacific/Majuro',
 'Pacific/Marquesas',
 'Pacific/Midway',
 'Pacific/Nauru',
 'Pacific/Niue',
 'Pacific/Norfolk',
 'Pacific/Noumea',
 'Pacific/Pago_Pago',
 'Pacific/Palau',
 'Pacific/Pitcairn',
 'Pacific/Pohnpei',
 'Pacific/Ponape',
 'Pacific/Port_Moresby',
 'Pacific/Rarotonga',
 'Pacific/Saipan',
 'Pacific/Samoa',
 'Pacific/Tahiti',
 'Pacific/Tarawa',
 'Pacific/Tongatapu',
 'Pacific/Truk',
 'Pacific/Wake',
 'Pacific/Wallis',
 'Pacific/Yap',
 'Poland',
 'Portugal',
 'ROC',
 'ROK',
 'Singapore',
 'Turkey',
 'UCT',
 'US/Alaska',
 'US/Aleutian',
 'US/Arizona',
 'US/Central',
 'US/East-Indiana',
 'US/Eastern',
 'US/Hawaii',
 'US/Indiana-Starke',
 'US/Michigan',
 'US/Mountain',
 'US/Pacific',
 'US/Samoa',
 'UTC',
 'Universal',
 'W-SU',
 'WET',
 'Zulu',
 'build/etc/localtime'}
```

现在我们可以使用`ZoneInfo`来确定不同地区的当前时间:

```py
# Function to convert datetime into ISO formatted time
def iso(time, time_format="%Y-%m-%d %H:%M:%S"):
    return time.strftime(time_format)

# CET time zone
cet_tz = ZoneInfo("Europe/Paris")

# PST time zone
pst_tz = ZoneInfo("America/Los_Angeles")

# Current time in Central Europe
dt_cet = datetime.now(tz=cet_tz)

# Current time in California
dt_pst = datetime.now(tz=pst_tz)

print(f"Current time in Central Europe is {iso(dt_cet)}.")
print(f"Current time in California is {iso(dt_pst)}.")
```

```py
Current time in Central Europe is 2022-02-14 11:33:42.
Current time in California is 2022-02-14 02:33:42.
```

让我们打印`datetime.now(tz=cet_tz)`:

```py
print(datetime.now(tz=cet_tz))
```

```py
2022-02-14 11:33:43.048967+01:00
```

我们看到有`+01:00`，表示 [UTC 偏移](https://en.wikipedia.org/wiki/UTC_offset)。事实上，欧洲中部时区比世界协调时早一个小时。

此外，`ZoneInfo`类处理夏令时。例如，我们可以在 DST 发生变化的一天中增加一天(24 小时)。在美国，2022 年，它将发生在 11 月 5 日(时钟向后)。

```py
# Define timedelta
time_delta = timedelta(days=1)

# November 5, 2022, 3 PM
november_5_2022 = datetime(2022, 11, 5, 15, tzinfo=pst_tz) # Note that we should use tzinfo in the datetime construct
print(november_5_2022)

# Add 1 day to November 5, 2022
print(november_5_2022 + time_delta)
```

```py
2022-11-05 15:00:00-07:00
2022-11-06 15:00:00-08:00
```

我们可以看到，偏移量从`-07:00`变为`-08:00`，但是时间保持不变(15:00)。

## 纽约市 2023 年新年倒计时

新年前夕，纽约的时代广场吸引了成千上万的人。让我们运用目前所学的一切知识，为时代广场新年前夜制作一个倒计时器！

这里我们将使用来自`dateutil`包的`tz`，它允许我们设置本地时区来展示`dateutil`包的效用。然而，我们也可以使用`zoneinfo`的`build/etc/localtime`时区来做同样的事情。

```py
from zoneinfo import ZoneInfo
from dateutil import tz
from datetime import datetime

def main():
    """Main function."""
    # Set current time in our local time zone
    now = datetime.now(tz=tz.tzlocal())

    # New York City time zone
    nyc_tz = ZoneInfo("America/New_York")

    # New Year 2023 in NYC
    new_year_2023 = datetime(2023, 1, 1, 0, 0, tzinfo=nyc_tz)

    # Compute the time left to New Year in NYC
    countdown = relativedelta(new_year_2023, now)

    # Print time left to New Year 2023
    print(f"New Year in New Your City will come on: {new_year_2023.strftime('%B %-d, %Y %H:%M:%S')}.")
    print(f"Time left to New Year 2023 in NYC is: {countdown.months} months, {countdown.days} days, {countdown.hours} hours, {countdown.minutes} minutes, {countdown.seconds} seconds.")

if __name__ == "__main__":
    main()
```

```py
New Year in New Your City will come on: January 1, 2023 00:00:00.
Time left to New Year 2023 in NYC is: 10 months, 17 days, 18 hours, 26 minutes, 16 seconds.
```

我们将代码包装在 [`main()`函数](https://www.geeksforgeeks.org/python-main-function/)中，现在我们可以在`.py`文件中使用它。在这个脚本中，我们使用时区，创建了一个`datetime`对象，使用`strftime()`将其转换为字符串，甚至访问了一个`relativedelta`对象的时间属性！

为了练习，你可以改进剧本。2022 年我在写这篇教程，所以对我来说，新年还没到。如果你在未来几年阅读它，你可以改进它，并考虑到 2023 年已经到来的事实。您还可以包括未来新年之前的剩余时间。或者你可以计算从前一个新年过去的时间。您还可以使用`pytz`包更新脚本，使其与 Python 的旧版本(3.9 之前)兼容。现在就看你的了。

## 结论

以下是我们在本教程中介绍的内容:

*   在计算机上表示日期和时间的问题
*   创建`date`、`time`和`datetime`对象
*   标准日期格式，ISO 8061
*   使用`fromisoformat()`和`strptime()`从字符串中提取日期
*   使用`strftime()`将`datetime`对象转换成字符串
*   时间戳
*   日期和`timedelta`对象的算术运算
*   用 Python 处理时区
*   在纽约市创建一个 2023 年新年倒计时器

现在，您的工具箱中有大量的工具，您可以开始用 Python 处理日期和时间了。

请随时在 [LinkedIn](https://www.linkedin.com/in/artur-sannikov-1b245a111/) 和 [GitHub](https://github.com/artur-sannikov/) 上联系我。编码快乐！