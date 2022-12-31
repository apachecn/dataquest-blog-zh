# 教程:Python 请求库简介

> 原文：<https://www.dataquest.io/blog/tutorial-an-introduction-to-python-requests-library/>

February 9, 2022![](img/c06a31ba441b7157592a56f3da6be15b.png)

请求库简化了向 web 服务器发出 HTTP 请求以及处理它们的响应。在本教程中，我们将学习如何安装和使用该库，并重点介绍其主要特性。

## 什么是 Python 请求库？

Requests 库提供了一个简单的 API 来与 HTTP 操作交互，比如`GET`、`POST`等。
在请求库中实现的方法针对由 URL 指定的特定 web 服务器执行 HTTP 操作。
它还支持通过参数和报头向 web 服务器发送额外的信息，对服务器响应进行编码，检测错误，以及处理重定向。
除了简化我们处理 HTTP 操作的方式，请求库还提供了一些高级特性，比如处理 HTTP 异常和认证，我们将在本教程中讨论。

## 什么是 HTTP？

超文本传输协议(HTTP)是一种基于客户端-服务器架构的请求/响应协议，它依靠 TCP/IP 连接来交换请求和响应消息。

HTTP 客户端(如 web 浏览器或移动应用程序)向 HTTP 服务器发送请求，服务器用包含状态行、标题和正文的消息来响应它们。

## 安装请求库

开始使用请求库的第一步是安装它。您可以使用`pip`或`conda`命令来安装库。为此，让我们首先创建一个新的 Python 虚拟环境，然后在其中安装库。

```py
~ % mkdir req-prj
~ % python3 -m venv req-prj/venv
~ % source re   q-prj/venv/bin/activate
(venv) ~ % python3 -m pip install requests
```

在终端窗口中键入以上命令，在 macOS 上创建环境。在上面的前三个命令中，我们在`req-prj`文件夹中创建了`venv`环境，然后激活该环境。最后，我们在环境中安装最新版本的请求包。

* * *

**注**

如果你不熟悉 Python 虚拟环境，Dataquest 博客上有一个很棒的教程，在[Python 虚拟环境完全指南(2022)–data quest](https://www.dataquest.io/blog/a-complete-guide-to-python-virtual-environments/)。

* * *

现在您可以导入这个库并编写您的第一个代码片段来试用它。

```py
import requests
r = requests.get('https://www.dataquest.io/')
print(r)
```

运行上面的代码输出`<Response [200]>`，这意味着请求成功并且 URL 是可到达的。

让我们检查上面代码中`r`变量的数据类型:

```py
print(type(r))
```

它返回`<class 'requests.models.Response'>`，这意味着它是*响应*类的一个实例。一个*响应*对象包含一个 HTTP 请求的结果。

* * *

**注**

在本教程中，我们使用[httpbin.org]([http://httpbin.org/](http://httpbin.org/))网站。 *httpbin* 工具是一个免费且简单的 HTTP 请求-响应服务，它提供了一组 URL 端点。我们使用这些端点来测试使用 HTTP 操作的各种方式。我们将使用 *httpbin* 工具，因为它有助于我们专注于学习 Python 请求库，而无需设置真正的 web 服务器或使用在线 web 服务。

* * *

## 使用获取请求

我们使用`get`方法从特定的 web 服务器请求数据。让我们试一试:

```py
url = 'http://httpbin.org/json'
r = requests.get(url)
print('Response Code:', r.status_code)
print('Response Headers:\n', r.headers)
print('Response Content:\n',r.text)
```

运行上面的代码会输出一个状态代码`200`，表示该 URL 是可访问的。然后，它返回页面的标题数据，后跟页面的内容。

`headers`属性返回一个专门针对 HTTP 头的特殊字典，因此您可以简单地使用它的键来访问每个项目:

```py
print(r.headers['Content-Type'])
```

继续运行语句。它返回页面的内容类型 `application/json`。
在代码的最后一行，我们可以使用将页面内容作为一系列字节返回的`content`属性，但是我们更喜欢使用将页面内容作为 Unicode 格式的解码文本打印出来的`text`属性。

## 使用获取参数

我们使用 GET 参数通过 URL 将键值对格式的信息传递给 web 服务器。`get`方法允许我们使用`params`参数传递一个键值对字典。让我们试一试。

```py
url = 'http://httpbin.org/get'
payload = {
    'website':'dataquest.io', 
    'courses':['Python','SQL']
    }
r = requests.get(url, params=payload)
print('Response Content:\n',r.text)
```

运行上面的代码。您将看到以下输出:

```py
Response Content:
 {
  "args": {
    "courses": [
      "Python", 
      "SQL"
    ], 
    "website": "dataquest.io"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.27.1", 
    "X-Amzn-Trace-Id": "Root=1-61e7e066-5d0cacfb49c3c1c3465bbfb2"
  }, 
  "origin": "121.122.65.155", 
  "url": "http://httpbin.org/get?website=dataquest.io&courses=Python&courses=SQL"
}
```

响应内容是 JSON 格式的，我们通过`params`参数传递的键值对出现在响应的`args`部分。另外，`url`部分包含编码的 URL 以及传递给服务器的参数。

## 使用发布请求

我们使用 POST 请求将从 web 表单收集的数据提交到 web 服务器。要在请求库中做到这一点，您需要首先创建一个数据字典，并将其分配给`post`方法的`data`参数。让我们看一个使用`post`方法的例子:

```py
url = 'http://httpbin.org/post'
payload = {
    'website':'dataquest.io', 
    'courses':['Python','SQL']
    }
r = requests.post(url, data=payload)
print('Response Content:\n',r.text)
```

运行上面的代码会返回以下响应:

```py
Response Content:
 {
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "courses": [
      "Python", 
      "SQL"
    ], 
    "website": "dataquest.io"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Content-Length": "47", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.27.1", 
    "X-Amzn-Trace-Id": "Root=1-61e7ec9f-6333082d7f0b73d317acc1f6"
  }, 
  "json": null, 
  "origin": "121.122.65.155", 
  "url": "http://httpbin.org/post"
}
```

这一次，通过`post`方法提交的数据出现在响应的`form`部分，而不是在`args`中，因为我们没有将数据作为 URL 参数的一部分发送，而是将它们作为表单编码的数据发送。

## 处理异常

与远程服务器通信时可能会出现一些异常。例如，服务器可能无法访问，请求的 URL 在服务器上不存在，或者服务器没有在适当的时间范围内响应。在这一节中，我们将学习如何使用请求库的 *HTTPError* 类来检测和解决 HTTP 错误。

```py
import requests
from requests import HTTPError

url = "http://httpbin.org/status/404"
try:
    r = requests.get(url)
    r.raise_for_status()
    print('Response Code:', r.status_code)
except HTTPError as ex:
    print(ex)
```

在上面的代码中，我们导入了 *HTTPError* 类，用于从请求库中捕获和解析 HTTP 异常。然后，我们尝试请求 httpbin.org 上的一个端点，这会生成一个`404`状态代码。每当 HTTP 响应包含错误状态代码(4XX 客户端错误或 5XX 服务器错误响应)时，`raise_for_status()`方法就会抛出异常。一旦出现错误，请求库不会自动引发异常。所以我们需要使用`raise_for_status()`方法来识别是否出现了错误状态代码。最后，异常处理程序块捕获错误并按如下方式打印出来:

```py
404 Client Error: NOT FOUND for url: http://httpbin.org/status/404
```---
**NOTE**

If you try to access the <code>http://httpbin.org/status/200 endpoint, the code above outputs <code>Response Code: 200 because the status code is not in the range of error status codes. The <code>raise_for_status() method will return <code>None, which won't trigger the exception handler.
- - - -
In addition to the exception that we just discussed, let’s see how we can resolve server timeouts. It's crucial because we need to ensure our application doesn’t hang indefinitely. In the Requests library, if a request times out, a *Timeout*  exception will occur. To specify the timeout of a request to a number of seconds, we can use the <code>timeout argument:
```pypy
import requests
from requests import Timeout
url = "http://httpbin.org/delay/10"

try:
    r = requests.get(url, timeout=3)
    print('Response Code:', r.status_code)
except Timeout as ex:
    print(ex)
```

先说代码。我们导入了 *Timeout* 类来解决超时异常，并提供了一个 URL 来引用一个有 10 秒延迟的端点来测试我们的代码。然后，我们将超时参数设置为 3 秒，这意味着如果服务器在 3 秒内没有响应，get 请求将抛出一个异常。最后，异常处理程序捕获超时错误(如果有的话)，并将其打印出来。所以运行上面的代码会输出下面的错误消息，因为服务器不会在给定的时间内响应。

```py
HTTPConnectionPool(host='httpbin.org', port=80): Read timed out. (read timeout=3)
```## Authentication
The Requests library supports various web authentications, such as basic, digest, the two versions of OAuth, etc. We can use these authentication methods when we want to work with any data sources that require us to be logged in.

We will implement the basic authentication using HTTPBin service in the following example. The endpoint for Basic Auth is /basic-auth/{*user*}/{*password*}. For example, if you use the following endpoint . . .

http://httpbin.org/basic-auth/data/quest

. . . you can authenticate using the username *'data'* and the password *'quest'* by assigning them as a tuple to the <code>auth argument. Once you authenticate successfully, it responds with JSON data <code>{ "authenticated": true, "user": "data"}.
```pypy
import requests
r = requests.get('http://httpbin.org/basic-auth/data/quest', auth=('data', 'quest'))
print('Response Code:', r.status_code)
print('Response Content:\n', r.text)
```

运行上面的代码。它的输出如下:

```py
Response Code: 200
Response Content:
 {
  "authenticated": true, 
  "user": "data"
}
```

如果我们使用户名或密码不正确，它的输出如下:

```py
Response Code: 401
Response Content:
```

## 结论

我们了解了最强大和下载量最大的 Python 库之一。我们讨论了 HTTP 的基础知识和 Python 请求库的不同方面。我们还处理了 GET 和 POST 请求，并学习了如何解决请求的异常。最后，我们尝试通过 web 服务中的基本身份验证方法进行身份验证。