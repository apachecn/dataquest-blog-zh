# Python API 教程:API 入门

> 原文：<https://www.dataquest.io/blog/python-api-tutorial/>

August 15, 2020![python api tutorial about the ISS](img/997c437e64637b283373ac24a7264f60.png "python api tutorial about the ISS")

在这个 Python API 教程中，我们将学习如何为数据科学项目检索数据。网上有数百万个 API 提供数据访问。像 [Reddit](https://www.reddit.com/dev/api/) 、 [Twitter](https://developer.twitter.com/en/docs.html) 和[脸书](https://developers.facebook.com/)这样的网站都通过他们的 API 提供某些数据。

要使用 API，您需要向远程 web 服务器发出请求，并检索您需要的数据。

但是为什么要使用 API 而不是可以从网上下载的静态 CSV 数据集呢？API 在下列情况下很有用:

*   **数据变化很快**。股票价格数据就是一个例子。重新生成数据集并每分钟下载它实际上没有意义-这将占用大量带宽，并且非常慢。
*   你想要一个大得多的数据集的一小部分。Reddit 评论就是一个例子。如果你只想在 Reddit 上发布自己的评论呢？下载整个 Reddit 数据库，然后只过滤你自己的评论是没有意义的。
*   **存在重复计算**。Spotify 有一个 API 可以告诉你一段音乐的流派。理论上，你可以创建自己的分类器，并使用它来计算音乐类别，但你永远不会拥有像 Spotify 那样多的数据。

在上述情况下，API 是正确的解决方案。在这篇博文中，我们将查询一个简单的 API 来检索关于国际空间站的数据。

## 关于这个 Python API 教程

本教程是基于我们关于 Python 中的 API 和 Webscraping 的交互式课程的一部分，你可以[免费开始](https://www.dataquest.io/course/apis-and-scraping/)。

对于本教程，我们假设您了解使用 Python 处理数据的一些基础知识。如果你没有，你可能想试试我们的[免费 Python 基础课程](https://www.dataquest.io/course/python-for-data-science-fundamentals/)。

如果你正在寻找更高级的东西，请查看我们的[中级 API 教程](https://www.dataquest.io/blog/last-fm-api-python/)。

## 什么是 API？

API 或应用程序编程接口是一种服务器，您可以使用代码来检索数据和向其发送数据。API 最常用于检索数据，这将是本初学者教程的重点。

当我们想从一个 API 接收数据时，我们需要发出一个**请求**。请求在网络上随处可见。例如，当您访问这篇博客文章时，您的 web 浏览器向 Dataquest web 服务器发出一个请求，它用这个 web 页面的内容作出响应。

![r api tutorial api request](img/abf452fbdf0b92d0726c3d2d301010b1.png "api-request")

API 请求的工作方式完全相同——您向 API 服务器发出数据请求，它会响应您的请求。

## 用 Python 制作 API 请求

为了使用 Python 中的 API，我们需要能够发出这些请求的工具。在 Python 中，最常见的发出请求和使用 API 的库是 [**请求**库](https://2.python-requests.org/en/master/)。请求库不是标准 Python 库的一部分，所以您需要安装它才能开始。

如果使用 pip 管理 Python 包，可以使用以下命令安装请求:

```
pip install requests
```

如果您使用 conda，您需要的命令是:

```
conda install requests
```

一旦你安装了库，你需要导入它。让我们从重要的一步开始:

```
import requests
```

现在我们已经安装并导入了请求库，让我们开始使用它。

## 发出我们的第一个 API 请求

有许多不同类型的请求。最常用的一个是 **GET** 请求，用于检索数据。因为我们只是检索数据，所以我们的重点是发出“get”请求。

当我们发出请求时，来自 API 的响应带有一个**响应代码**，它告诉我们我们的请求是否成功。响应代码很重要，因为它们会立即告诉我们是否出现了问题。

![](img/abf452fbdf0b92d0726c3d2d301010b1.png)要发出一个“GET”请求，我们将使用 [`requests.get()`函数](https://2.python-requests.org/en/master/user/quickstart/#make-a-request)，它需要一个参数——我们要向其发出请求的 URL。我们首先向一个不存在的 API 端点发出请求，这样我们就可以看到响应代码是什么样子。

```
response = requests.get("https://api.open-notify.org/this-api-doesnt-exist")
```

`get()`函数返回一个 [`response`对象](https://2.python-requests.org/en/master/user/advanced/#request-and-response-objects)。我们可以使用`[response.status_code](https://2.python-requests.org/en/master/user/quickstart/#response-status-codes)`属性来接收请求的状态代码:

```
print(response.status_code)
```

```
404
```

“404”状态代码对您来说可能很熟悉——它是服务器在找不到我们请求的文件时返回的状态代码。在这种情况下，我们要求的`this-api-doesnt-exist`是(惊喜，惊喜)不存在的！

让我们再多了解一些常见的状态代码。

## API 状态代码

向 web 服务器发出的每个请求都会返回状态代码。状态代码表示关于请求发生了什么的信息。以下是一些与 *GET* 请求相关的代码:

*   一切顺利，结果已经返回(如果有的话)。
*   `301`:服务器正在将您重定向到不同的端点。当公司切换域名或端点名称更改时，可能会发生这种情况。
*   服务器认为你提出了一个错误的请求。当你没有发送正确的数据时，就会发生这种情况。
*   服务器认为你没有通过认证。许多 API 需要登录凭证，所以当您没有发送正确的凭证来访问 API 时，就会发生这种情况。
*   你试图访问的资源被禁止:你没有权限查看它。
*   `404`:在服务器上找不到您试图访问的资源。
*   `503`:服务器未准备好处理请求。

您可能会注意到，所有以“4”开头的状态代码都表示某种错误。状态代码的第一个数字表示它们的类别。这很有用—您可以知道，如果您的状态代码以“2”开头，则表示成功，如果以“4”或“5”开头，则表示有错误。如果你感兴趣，你可以[在这里](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)阅读更多关于状态码的信息。

## API 文档

为了确保我们发出成功的请求，当我们使用 API 时，参考文档是很重要的。文档一开始看起来很可怕，但是随着你越来越多地使用文档，你会发现它变得越来越容易。

我们将使用 [Open Notify](https://open-notify.org/) API，它提供对国际空间站数据的访问。这是一个很好的学习 API，因为它的设计非常简单，并且不需要认证。我们将在以后的文章中教你如何使用需要认证的 API。

通常在一个特定的服务器上会有多个可用的 API。这些 API 通常被称为**端点**。我们将使用的第一个端点是 http://api.open-notify.org/astros.json 的，它返回当前在太空中的宇航员的数据。

如果您点击上面的链接来查看这个端点的文档，您会看到它说*这个 API 不接受任何输入。*这使它成为我们开始使用的一个简单的 API。我们将从使用请求库向端点发出 GET 请求开始:

```
response = requests.get("https://api.open-notify.org/astros.json")
print(response.status_code)
```

```
200
```

我们收到一个“200”代码，告诉我们我们的请求是成功的。文档告诉我们，我们将得到的 API 响应是 JSON 格式的。在下一节中，我们将了解 JSON，但是首先让我们使用 [response.json()方法](https://2.python-requests.org/en/master/user/quickstart/#json-response-content)来查看我们从 API 接收到的数据:

```
print(response.json())
```

```
{'message': 'success', 'people': [{'name': 'Alexey Ovchinin', 'craft': 'ISS'}, {'name': 'Nick Hague', 'craft': 'ISS'}, {'name': 'Christina Koch', 'craft': 'ISS'}, {'name': 'Alexander Skvortsov', 'craft': 'ISS'}, {'name': 'Luca Parmitano', 'craft': 'ISS'}, {'name': 'Andrew Morgan', 'craft': 'ISS'}], 'number': 6}
```

## 在 Python 中使用 JSON 数据

[JSON](https://json.org/) (JavaScript 对象表示法)是 API 的语言。JSON 是一种对数据结构进行编码的方法，这种方法可以确保它们易于被机器读取。JSON 是数据在 API 之间来回传递的主要格式，大多数 API 服务器将以 JSON 格式发送响应。

您可能已经注意到，我们从 API 收到的 JSON 输出看起来包含 Python 字典、列表、字符串和整数。您可以将 JSON 看作是用字符串表示的这些对象的组合。让我们看一个简单的例子:

![](img/483f8f33734d73b2f6cc6e1d211f6a30.png "json")

Python 的 [`json`包](https://docs.python.org/3/library/json.html)提供了强大的 JSON 支持。`json`包是标准库的一部分，所以我们不需要安装任何东西来使用它。我们既可以将*列表*和*字典*转换为 JSON，也可以将字符串转换为*列表*和*字典*。在我们的 ISS Pass 数据中，它是一个以 JSON 格式编码为字符串的字典。

json 库有两个主要功能:

*   `[json.dumps()](https://web.archive.org/web/20200107014409/https://docs.python.org/3/library/json.html#json.dumps)` —接收 Python 对象，并将其转换(转储)为字符串。
*   [`json.loads()`](https://web.archive.org/web/20200107014409/https://docs.python.org/3/library/json.html#json.loads) —获取 JSON 字符串，并将其转换(加载)为 Python 对象。

`dumps()`函数特别有用，因为我们可以用它来打印一个格式化的字符串，这样更容易理解 JSON 输出，就像我们上面看到的图表一样:

```
import json

def jprint(obj):
    # create a formatted string of the Python JSON object
    text = json.dumps(obj, sort_keys=True, indent=4)
    print(text)

jprint(response.json())
```

```
{
    "message": "success",
    "number": 6,
    "people": [
        {
            "craft": "ISS",
            "name": "Alexey Ovchinin"
        },
        {
            "craft": "ISS",
            "name": "Nick Hague"
        },
        {
            "craft": "ISS",
            "name": "Christina Koch"
        },
        {
            "craft": "ISS",
            "name": "Alexander Skvortsov"
        },
        {
            "craft": "ISS",
            "name": "Luca Parmitano"
        },
        {
            "craft": "ISS",
            "name": "Andrew Morgan"
        }
    ]
}
```

我们可以立即更容易地理解数据的结构——我们可以看到他们目前在太空中有六个人，他们的名字作为字典存在于一个列表中。

如果我们将它与端点的文档[进行比较，我们会看到它与端点的指定输出相匹配。](https://open-notify.org/Open-Notify-API/People-In-Space/#json)

## 使用带有查询参数的 API

我们之前使用的 http://api.open-notify.org/astros.json 端点不带任何参数。我们只需发送一个 GET 请求，API 就会发回关于当前太空中人数的数据。

然而，拥有一个要求我们指定参数的 API 端点是很常见的。这样的一个例子是[https://api.open-notify.org/iss-pass.json 端点](https://open-notify.org/Open-Notify-API/ISS-Pass-Times/)。这个终点告诉我们下一次国际空间站将经过地球上一个给定位置的时间。

如果我们看一下文档，它指定了必需的`lat`(纬度)和`long`(经度)参数。

我们可以通过在请求中添加一个可选的关键字参数`params`来做到这一点。我们可以用这些参数做一个字典，然后把它们传入`requests.get`函数。使用纽约市的坐标，我们的字典看起来是这样的:

```
parameters = {
    "lat": 40.71,
    "lon": -74
}
```

我们也可以通过将参数直接添加到 URL 来做同样的事情。像这样:
`https://api.open-notify.org/iss-pass.json?lat=40.71&lon;=-74`

将参数设置为一个字典几乎总是更可取的，因为`requests`会处理出现的一些事情，比如正确格式化查询参数，并且我们不需要担心将值插入到 URL 字符串中。

让我们用这些坐标发出一个请求，看看会得到什么样的响应。

```
response = requests.get("https://api.open-notify.org/iss-pass.json", params=parameters)

jprint(response.json())
```

```
{
    "message": "success",
    "request": {
        "altitude": 100,
        "datetime": 1568062811,
        "latitude": 40.71,
        "longitude": -74.0,
        "passes": 5
    },
    "response": [
        {
            "duration": 395,
            "risetime": 1568082479
        },
        {
            "duration": 640,
            "risetime": 1568088118
        },
        {
            "duration": 614,
            "risetime": 1568093944
        },
        {
            "duration": 555,
            "risetime": 1568099831
        },
        {
            "duration": 595,
            "risetime": 1568105674
        }
    ]
}
```

## 了解通行时间

JSON 响应与文档中指定的内容相匹配:

*   一本有三个键的字典
*   第三个键`response`，包含通行时间列表
*   每个通过时间都是一个带有`risetime`(通过开始时间)和`duration`键的字典。

让我们从 JSON 对象中提取通过时间:

```
pass_times = response.json()['response']
jprint(pass_times)
```

```
[
    {
        "duration": 395,
        "risetime": 1568082479
    },
    {
        "duration": 640,
        "risetime": 1568088118
    },
    {
        "duration": 614,
        "risetime": 1568093944
    },
    {
        "duration": 555,
        "risetime": 1568099831
    },
    {
        "duration": 595,
        "risetime": 1568105674
    }
]
```

接下来，我们将使用一个循环来提取五个`risetime`值:

```
risetimes = []

for d in pass_times:
    time = d['risetime']
    risetimes.append(time)

print(risetimes)
```

```
[1568082479, 1568088118, 1568093944, 1568099831, 1568105674]
```

这些时间很难理解——它们的格式被称为时间戳或[纪元](https://en.wikipedia.org/wiki/Unix_time)。从 1970 年 1 月 1 日开始，时间基本上是以秒数来计算的。我们可以使用 Python [`datetime.fromtimestamp()`方法](https://docs.python.org/3/library/datetime.html#datetime.date.fromtimestamp)将这些转换成更容易理解的时间:

```
from datetime import datetime

times = []

for rt in risetimes:
    time = datetime.fromtimestamp(rt)
    times.append(time)
    print(time)
```

```
2019-09-09 21:27:59
2019-09-09 23:01:58
2019-09-10 00:39:04
2019-09-10 02:17:11
2019-09-10 03:54:34
```

看起来国际空间站经常经过纽约市——接下来的五次发生在七个小时内！

## Python API 教程:后续步骤

在本教程中，我们学习了:

*   什么是 API
*   请求和响应代码的类型
*   如何发出 get 请求
*   如何使用参数发出请求
*   如何从 API 中显示和提取 JSON 数据

这些基本步骤将帮助您开始使用 API。记住，我们每次使用 API 的关键是仔细阅读 API 文档，并利用它来理解要发出什么请求和提供什么参数。

现在您已经完成了我们的 Python API 教程，您可能希望:

*   完成我们的交互式 Dataquest APIs 和抓取课程[，您可以免费开始学习](https://www.dataquest.io/course/apis-and-scraping)。
*   尝试使用这个免费公共 API 列表中的一些数据——我们建议选择一个不需要认证的 API 作为良好的第一步。
*   试试我们的[中级 API 教程](https://www.dataquest.io/blog/last-fm-api-python/)，它涵盖了 API 认证、分页和速率限制