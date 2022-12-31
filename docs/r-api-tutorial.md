# R API 教程:R 中的 API 入门

> 原文：<https://www.dataquest.io/blog/r-api-tutorial/>

February 13, 2020![iss-r-api-tutorial](img/2616da91f5cb7e84276ca1259ca0f44e.png)

有趣的数据集是好的数据科学项目的燃料。虽然 Kaggle 等网站向感兴趣的数据科学家提供免费数据集，但 API 是访问和获取有趣数据的另一种非常常见的方式。

API 允许程序员通过所谓的应用编程接口(因此称为“API”)直接从某些网站请求数据，而不是下载数据集。许多大型网站，如 Reddit、Twitter 和脸书，都提供 API，以便数据分析师和数据科学家可以访问有趣的数据。

在本教程中，我们将讲述使用 R 编程语言访问 API 的基础知识。您不需要任何 API 经验，但是您需要熟悉 R 的基础知识。

如果你对自己的研发能力还不自信，你可以看看我们免费的互动研发基础课程和研发中级课程:

> install.packages("Dataquest ")

从我们的[R 课程简介](/course/intro-to-r/)开始学习 R——不需要信用卡！

[SIGN UP](https://app.dataquest.io/signup)

已经具备了你需要的 R 编程技能？太好了！让我们深入研究用 r 访问 API。

## 带 R 的 API 简介

“API”是一个通用术语，指一个计算机程序与另一个程序或其自身交互的地方。在本教程中，我们将具体使用 web APIs，其中两台不同的计算机—客户机和服务器—将相互交互，分别请求和提供数据。

API 为数据科学家提供了一种从网站请求干净有序的数据的完美方式。当像脸书这样的网站建立一个 API 时，他们实际上是在建立一台等待数据请求的计算机。

一旦这台计算机收到数据请求，它将自己处理数据，并将其发送到请求它的计算机。从我们作为请求者的角度来看，我们需要用 R 编写代码来创建请求，并告诉运行 API 的计算机我们需要什么。然后，该计算机将读取我们的代码，处理请求，并返回格式良好的数据，这些数据很容易被现有的 R 库解析。

这为什么有价值？将 API 方法与纯 web 抓取进行对比。当一个程序员抓取一个网页时，他们收到的数据是一大堆杂乱的 HTML。虽然确实有一些库可以使解析 HTML 文本变得容易，但这些都是在我们得到我们想要的数据之前需要采取的清理步骤！

通常，我们可以立即使用从 API 中获得的数据，这节省了我们的时间和挫折。

## 在 R 中发出 API 请求

为了在 R 中使用 API，我们需要引入一些库。这些库包含了 API 请求的所有复杂性，并将它们封装在我们可以在单行代码中使用的函数中。我们将使用的 R 库是`httr`和`jsonlite`。它们在我们的 API 介绍中扮演不同的角色，但都是必不可少的。

如果您的 R 控制台或 RStudio 中没有这两个库，您需要先下载它们。使用`install.packages()`功能引入这些包。

```py
install.packages(c("httr", "jsonlite"))
```

下载完这些库之后，我们将能够在 R 脚本或 RMarkdown 文件中使用它们。

```py
library(httr)
library(jsonlite)
```

## 发出我们的第一个 API 请求

从 API 获取数据的第一步是在 r 中发出实际的请求。这个请求将被发送到拥有 API 的计算机服务器，假设一切顺利，它将发回一个响应。下图说明了这一点:

![r api tutorial api request](img/abf452fbdf0b92d0726c3d2d301010b1.png "api-request")

可以向 API 服务器发出几种类型的请求。这些类型的请求对应于您希望服务器执行的不同操作。

出于我们的目的，我们将只要求数据，这对应于一个`GET`请求。其他类型的请求有`POST`和`PUT`，但是在这篇以数据科学为中心的 R API 教程中，我们不需要担心它们。

为了创建一个`GET`请求，我们需要使用`httr`库中的 GET()函数。`GET()`函数需要一个 URL，它指定请求需要发送到的服务器的地址。

GET()函数封装了一个`GET`请求的所有复杂性。对于我们的例子，我们将使用开放的 Notify API，它开放了各种 NASA 项目的数据。使用 Open Notify API，我们可以了解国际空间站的位置以及目前有多少人在太空中。

我们将首先使用后一个 API。我们首先使用`GET()`函数发出请求，并指定 API 的 URL:

```py
>> res = GET("https://api.open-notify.org/astros.json")
```

`GET()`函数的输出是一个列表，其中包含 API 服务器返回的所有信息。换句话说，`res`变量包含 API 服务器对我们的**请求**的**响应**

## 检查`GET()`输出

让我们看看`res`变量在 R 控制台中是什么样子的:

```py
>> res
```

```py
Response [https://api.open-notify.org/astros.json]
  Date: 2020-01-30 18:07
  Status: 200
  Content-Type: application/json
  Size: 314 B
```

研究 res 变量给我们一个结果响应的概要。首先要注意的是，它包含了`GET`请求被发送到的 URL。我们还可以看到发出请求的日期和时间，以及响应的大小。

内容类型让我们了解数据的形式。这个特定的响应表示数据采用 json 格式，这暗示了我们为什么需要`jsonlite`库。

这一地位值得特别关注。“状态”是指 API 请求的成功或失败，它以数字的形式出现。返回的数字告诉您请求是否成功，还可以详细说明失败的原因。

200 这个数字是我们想看到的；它对应于一个成功的请求，这就是我们这里所拥有的。这里有更多关于我们可能遇到的其他状态代码的信息。由于我们有一个成功的 200 状态响应，我们知道我们手头有数据，我们可以开始使用它。

## 处理 JSON 数据

[JSON](https://www.json.org/json-en.html) 代表 JavaScript 对象符号。虽然 JavaScript 是另一种编程语言，但我们对 JSON 的关注点是它的*结构*。JSON 很有用，因为它很容易被计算机读取，正因为如此，它已经成为通过 API 传输数据的主要方式。大多数 API 将以 JSON 格式发送它们的响应。

JSON 被格式化为一系列的*键值对*，其中一个特定的单词(“key”)与一个特定的值相关联。这种键值结构的一个示例如下所示:

```py
{
    “name”: “Jane Doe”,
    “number_of_skills”: 2
}
```

在当前状态下，`res`变量中的数据不可用。实际数据作为原始 Unicode 包含在`res`列表中，最终需要转换成 JSON 格式。

要做到这一点，我们首先需要将原始的 Unicode 转换成一个类似于上面显示的 JSON 格式的字符向量。`rawToChar()`函数只执行这项任务，如下所示:

```py
>> rawToChar(res$content)
```

```py
[1] "{\"people\": [{\"name\": \"Christina Koch\", \"craft\": \"ISS\"}, {\"name\": \"Alexander Skvortsov\", \"craft\": \"ISS\"}, {\"name\": \"Luca Parmitano\", \"craft\": \"ISS\"}, {\"name\": \"Andrew Morgan\", \"craft\": \"ISS\"}, {\"name\": \"Oleg Skripochka\", \"craft\": \"ISS\"}, {\"name\": \"Jessica Meir\", \"craft\": \"ISS\"}], \"number\": 6, \"message\": \"success\"}"
```

虽然得到的字符串看起来很混乱，但它确实是字符格式的 JSON 结构。

从一个字符向量，我们可以使用`jsonlite`库中的`fromJSON()`函数将其转换成`list`数据结构。

`fromJSON()`函数需要一个包含 JSON 结构的字符向量，这是我们从 rawToChar()输出中得到的。所以，如果我们把这两个函数串在一起，我们将得到我们想要的数据，其格式在 r 中更容易操作。

```py
>> data = fromJSON(rawToChar(res$content))
>> names(data)
```

```py
[1] "people"  "number"  "message"
```

在我们的`data`变量中，我们感兴趣的数据集包含在`people`数据框中。我们可以使用`$`运算符直接查看这个数据框:

```py
>> data$people
```

```py
name craft
1      Christina Koch   ISS
2 Alexander Skvortsov   ISS
3      Luca Parmitano   ISS
4       Andrew Morgan   ISS
5     Oleg Skripochka   ISS
6        Jessica Meir   ISS
```

所以，这就是我们的答案:在写这篇博文的时候，太空中有 6 个人。但是如果你自己尝试，你可能会得到不同的名字和不同的数字。这是 API 的优势之一——与可下载的数据集不同，它们通常是实时或接近实时更新的，因此它们是访问最新数据的一种很好的方式。

一个数据框，就像我们上面看到的，是我们在 Dataquest 课程中学到的典型的存储结构化数据的方式，以便在`tidyverse`库中进行进一步的分析。(如果有兴趣，您可以在我们位于 R Path 的[数据分析师那里了解更多信息。)](https://www.dataquest.io/path/data-analyst-r/)

上面，我们已经走过了一个非常简单的 API 工作流程。大多数 API 会要求你遵循同样的通用模式，但是它们可能更复杂。

我们上面使用的 API URL 只是要求我们从它发出一个请求，没有任何额外的细节。有些 API 需要用户提供更多信息。在本教程的最后一部分，我们将介绍如何根据您的请求向 API 提供附加信息。

## API 和查询参数

如果我们想知道国际空间站什么时候会经过地球上一个给定的位置呢？与 People in Space API 不同，Open Notify 的 [ISS Pass Times API](https://open-notify.org/Open-Notify-API/ISS-Pass-Times/) 要求我们提供额外的参数，然后它才能返回我们想要的数据。

具体来说，我们需要在我们的`GET()`请求中指定我们要询问的地点的纬度和经度。一旦纬度和经度被指定，它们就与原始 URL 组合成**查询参数**。

让我们使用这个 API 来找出国际空间站何时会经过布鲁克林大桥(大约在纬度 40.7，经度:-74):

```py
res = GET(“http://api.open-notify.org/iss-pass.json",
    query = list(lat = 40.7, lon = -74))

# Checking the URL that gets used in the API request yields
# https://api.open-notify.org/iss-pass.json?lat=40.7&lon=-74
```

我们传递给`GET()`中的`query`参数的列表的不同元素被正确地格式化成 URL，所以你不必自己做这些。

您需要查看正在使用的 API 的文档，看看是否有任何必需的查询参数。这里的是国际空间站通过时间的 API 文件。

您可能想要访问的绝大多数 API 都有文档，您可以(也应该)阅读这些文档，以便清楚地了解您的请求需要哪些参数。许多 API 都需要一个公共参数，尽管不是我们在本教程中使用的参数，那就是 API 密钥或其他形式的身份验证。查看 API 的文档，了解如何获取 API 密钥以及如何将其插入到您的请求中。

无论如何，现在我们已经发出了请求，包括位置参数，我们可以使用前面使用的相同函数来检查响应。让我们从回答中提取数据:

```py
>> res = GET("https://api.open-notify.org/iss-pass.json",
    query = list(lat = 40.7, lon = -74))
>> data = fromJSON(rawToChar(res$content))

>> data$response
```

```py
duration   risetime
1      623 1580439398
2      101 1580445412
3      541 1580493826
4      658 1580499550
5      601 1580505413
```

这个 API 以 [Unix time](https://en.wikipedia.org/wiki/Unix_time) 的形式向我们返回时间。Unix 时间只是自 1970 年 1 月 1 日以来经过的时间量。有许多函数可以实现从 Unix 到熟悉的时间形式的轻松转换——我们不会在本教程中讨论日期和时间，但会在我们的 R interactive learning path 中的[数据分析师中讨论。](https://www.dataquest.io/path/data-analyst-r/)

我们还可以在像这样的网站上快速地将 Unix 时间转换成可读性更好的格式。

## 您已经掌握了 R 中 API 的基础知识！

在本教程中，我们学习了什么是 API，以及它们如何对数据分析师和数据科学家有用。

使用我们的 R 编程技能以及`httr`和`jsonlite`库，我们从 API 中获取数据，并将其转换成熟悉的格式以供分析。

在这里，我们只是触及了使用 API 的表面，但是希望这篇介绍已经给了你信心去研究一些更复杂和更强大的 API，并且帮助你打开一个全新的数据世界，让你去探索！

### 准备好提升你的 R 技能了吗？

我们 R path 的[数据分析师涵盖了你找到工作所需的所有技能，包括:](/path/data-analyst-r/)

*   使用 **ggplot2** 进行数据可视化
*   使用 **tidyverse** 软件包的高级数据清理技能
*   R 用户的重要 SQL 技能
*   **统计**和概率的基础知识
*   ...还有**多得多的**

没有要安装的东西，**没有先决条件**，也没有时间表。

[Start learning for free!](https://app.dataquest.io/signup)