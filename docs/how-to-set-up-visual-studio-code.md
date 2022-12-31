# 如何在 2022 年设置 Visual Studio 代码(最简单的方法)

> 原文：<https://www.dataquest.io/blog/how-to-set-up-visual-studio-code/>

April 1, 2022![](img/42743ff5acd6938aaf47be9985cf6700.png)

Visual Studio 代码是一个强大的、可扩展的轻量级代码编辑器，已经成为 Python 社区中首选的代码编辑器之一。

在本教程中，我们将学习如何安装 Visual Studio 代码并为 Python 开发进行设置，以及如何更高效地使用 VS 代码。

让我们开始吧！

## 安装 Visual Studio 代码

这一节将逐步介绍如何在 macOS 上安装 VS 代码。让我们开始吧。

* * *

**注**

由于 Windows 和 macOS 之间的本质区别，Windows 用户需要做一些小的修改来安装 VS 代码。然而，在 Windows 上安装 VS 代码很简单，类似于安装其他 Windows 应用程序。

* * *

1.  首先从其[官网](https://code.visualstudio.com/)下载 macOS 或 Windows 的 Visual Studio 代码。下载页面会自动检测您的操作系统，并显示一个大按钮，用于在您的计算机上下载最新版本的 Python 安装程序。如果没有，请单击向下箭头按钮，并选择与您计算机上安装的操作系统相匹配的稳定 VS 代码版本:

![Download Visual Studio](img/37e838a52cba2378a98a133a5f0c5e37.png)

2.  双击下载的文件，提取存档内容:
    ![Double click the download file](img/bc266c9d8d0b13e0d931730aeac7d66e.png)
3.  将 Visual Studio 代码应用程序移动到 application 文件夹，使其在 macOS launchpad 中可用:
    ![Move the Visual Studio Code application to the Application folder](img/4958de549688281e306285d806978656.png)
4.  启动 Visual Studio 代码，然后打开 Python 脚本所在的文件夹或创建一个新文件夹。例如，在你的桌面上创建一个新文件夹，并将其命名为`py_scripts`，然后尝试在 VS 代码上打开该文件夹。
    通常情况下，VS 代码需要你的权限才能访问你桌面文件夹中的文件；点击确定:
    ![Grant the proper permission for VS Code](img/d9d49f96e51c0bd826859e1c95911153.png)
    另外，您可能需要声明您信任存储在您桌面文件夹中的文件的作者:
    ![Declare that you trust the authors of the files](img/f5b459df8030f88431e9b8835d24a3f1.png)
5.  创建一个扩展名为`.py`的新文件。例如，创建一个新文件，并将其命名为`prog_01.py`。VS 代码检测到`.py`扩展，想安装一个 Python 扩展:
    ![Create a new file in VS Code with .py extension](img/cb87bab58832a311e4158f9823854843.png)
    要在 VS 代码内部使用 Python，我们需要使用 Python 扩展，它带来了很多有用的特性，比如带智能感知的代码完成、调试、单元测试支持等:
    ![Why it's important to use VS Code extensions](img/1bc7c98c8cd97faaadbfc11818c0ab51.png)
    点击安装:
    ![Click the install button](img/3ce00ddd4381b781a112da13cbede352.png)
    我们也可以通过浏览扩展来安装 Python 扩展。单击 VS 代码左侧的扩展图标:

![Click the Extensions icon](img/4ff378aa3ce06dd1435da0baec6fa9f0.png)

这将显示 VS 代码市场上最流行的 VS 代码扩展列表。现在，我们可以选择 Python 扩展并安装它:
![Select the Python extension](img/5aea286d3d07ab450e649be016226b8a.png)

6.  一旦安装了扩展，您必须选择一个 Python 解释器。点击选择 Python 解释器:
    ![Select the Python Interpreter button](img/2103c65b5fa8dfd18b9bdf1ec82421d3.png)
    然后在列表中选择推荐的 Python 解释器:
    ![Select the recommended Python interpreter](img/d5fa4b5fc93f2341c504f813e7e212df.png)
    如果您的 Mac 上安装了多个 Python 版本，请选择最新版本:
    ![Select the latest Python version](img/4c46fb1e4fb75b936043b3c15d85a3d0.png)
    您也可以使用命令面板中的**Python:Select Interpreter**命令选择一个 Python 解释器。为此，按下`CMD` + `SHIFT` + `P`，键入 Python，并选择*选择解释器*。

## 用 VS 代码创建和运行 Python 文件

非常好。我们拥有在 VS 代码中编写和运行 Python 代码所需的一切。让我们用 VS 代码写下面的代码，然后运行它。

```py
def palindrome(a):
        a = a.upper()
        return a == a[::-1]
name = input("Enter a name: ")
if palindrome(name):
        print("It's a palindrome name.")
else:
        print("It's not a palindrome name.")
```

通过点击 VS 代码右上角的▶️按钮来运行代码。首先，它在集成终端中询问名称；输入一个名称，然后按回车键。如果输入的名字是回文，它输出`It's a palindrome name.`，否则输出`It's not a palindrome name.`。

> 回文单词是一个字母序列，向后读和向前读是一样的，例如 Hannah、Anna 和 Bob。

![](img/4da768174e450c6e6f047c2c36e32dd6.png)

如您所见，所有输出都出现在集成终端中。让我们再多谈谈这个奇妙的特性。

由于执行终端命令几乎是编写代码不可或缺的一部分，VS 代码通过将这一优秀特性嵌入到 IDE 中为开发人员带来了极大的便利。要查看终端，可以在 macOS 或 Windows 机器上键入`Ctrl` + ```py，或者使用**查看>终端**菜单命令。另外，如果你想关闭一个集成终端，点击终端窗口右上角的 bin 图标。从技术上讲，集成终端使用计算机上安装的 Shell，例如，Windows 上的 PowerShell 或命令提示符，macOS 和 Linux 上的 bash 或 zsh。

Visual Studio 代码允许我们使用终端设置自定义终端的外观。为此，打开终端设置页面，点击终端窗口右上角的向下箭头按钮，选择**配置终端设置**选项；您可以轻松定制字体、间距和光标样式:
![Select the Configure Terminal Settings option](img/4153ba59f428cb0abd58f7a3bf03b4eb.png)
VS 代码的另一个很好的特性是，您可以轻松地在多个 shell 之间切换，甚至可以更改集成终端中使用的默认 shell。为此，单击终端窗口右上角的向下箭头按钮，并选择**选择默认配置文件**选项:
![Select Default Profile option in VS Code](img/f2d31da2f2ee69692fbcd1c2e189c49c.png)
将出现一个预先填充的可用 shell 列表，您可以选择其中一个作为默认终端 shell。让我们选择 bash shell:
![Choose bash shell option](img/f515d1a714e3b2650eee15c19441ae2f.png)
一旦您通过单击终端窗口右上角的加号图标创建了一个新的终端，它将使用 bash shell，如下所示:
![Bash shell settings during setup](img/ba726afc53cf8da2b807673186321f9d.png)

## 使用 REPL

VS 代码中另一个有用的特性是运行单行或多行代码，只需选择它们并从上下文菜单中选择**运行选择 Python 终端中的行**选项。让我们试一试。

在同一个 Python 文件中，编写以下语句:

```
print("Hello, world!")
```py

然后选择语句，右键选择**在 Python 终端**中运行选择/行选项，如下:
![Run Selection/Line in Python Terminal optional](img/9c7fe555eddf926757a2489db083b291.png)
输出出现在集成终端中但是以一种不同的形式出现，叫做 REPL。让我们了解一下 REPL 和它的优点。

REPL 代表阅读，评估，打印，循环。这是一种使用 Python 解释器并直接在终端中运行命令的交互方式。在 REPL，三个右箭头符号表示输入行。

另一种在 VS 代码中启动 REPL 的方法如下:
打开命令调板，搜索 REPL，点击 **Python:启动 REPL** :
![Python: Start REPL](img/18b77839b58d493bdd3d9c0115b465f7.png)
会出现交互式 Python shell，我们可以在`>>>`提示符下输入我们的命令，然后只需按下`Enter`或`return`键就可以执行它们，如下:
![Execute commands in the interactive Python shell](img/8243083d233afa7faa0ce40691964ddc.png)
REPL 的一个很大的特点就是可以即时看到运行命令的结果。所以，如果你想尝试一些代码或者 API，REPL 是一个很好的方法。

## 格式化 Python 代码

一旦开始编写程序，你就应该养成以适当的格式编写代码的习惯。Python 有一个著名的 Python 代码风格指南，叫做 PEP 8，它让你的代码易于阅读和理解。你可以在 Python 官方网站上找到风格指南，网址是[PEP 8——Python 代码风格指南| Python.org](https://www.python.org/dev/peps/pep-0008/)。

在这一节中，我们将学习如何使用 Autopep8 包将格式自动应用到我们的代码中。这个包可以使用`pip`命令安装，它自动格式化 Python 代码以符合 PEP 8 风格指南。好消息是 VS 代码支持使用 Autopep8 包自动格式化代码。

让我们看看如何在 VS 代码中安装包并启用它。

首先，在集成终端中执行以下命令来安装 Autopep8 软件包:

```
pip3 install autopep8
```py

安装完成后，关闭终端。现在打开 VS 代码的设置，搜索 *Python 格式化*,**Autopep8 Path**和 **Provider** 字段都需要用 auto pep 8 填充，如下:
![Fill out Autopep8 Path and Provider](img/38abb7447d48ccaf8c243a9384df3079.png)
最后一步是在保存时启用自动格式化。为此，在保存时搜索*格式，并检查保存**格式**选项:
![Enable automatic formatting](img/7f28426c8765c4502c7796b55eae4fc5.png)
启用此特性将在我们保存文件时应用 Python 源文件上的所有 PEP 8 规则。*

PEP 8 风格指南提供了一些注意事项。我们真的鼓励你学习 PEP 8 规则，并通过 Autopep8 格式化包将它们自动应用到你的源代码中。

## 重构 Python 代码

在讨论在 VS 代码中重构 Python 代码之前，我们先定义一下:

> 代码重构是在不改变其外部行为的情况下，重新构造现有计算机代码的过程——改变因子分解——以使其更易于阅读和维护——[维基百科](https://en.wikipedia.org/wiki/Code_refactoring)。
> Python 扩展提供了基本的重构功能，如重命名符号、提取方法、提取变量等。
> 例如，要将`palindrome()`方法名改为`check_palindrome()`，右击方法名，选择**重命名符号**选项:

![Rename Symptom option](img/37691f3cb037c0084ed678bdbe01da4c.png)
在文本框中输入新名称，*check _ 回文*，然后按回车键重命名:
![check_palindrome](img/e08841fffecdb1a40d2c2e8597c1a991.png)
现在，你可以看到所有的`palindrome`事件都被更改为`check_palindrome` :
![check_palindrome result](img/e9ea132211e3a8ffbfff5424b081f8f1.png)
在关闭本节之前，让我们尝试一下提取方法功能。创建一个新的 Python 文件，并将以下代码粘贴到其中。

```
height = 5
width = 4
area = height * width
print("Room's area =", area, "square meters")
```

选择第三行，点击右键，从上下文菜单中选择重构选项:
![Refactoring option in the context menu](img/0bb82913663c189b645e6980830e35d5.png)
然后点击**提取方法**按钮，在出现的文本框中输入新名称 *calc_area* ，然后回车重命名:
![Extract method button](img/9a918b09181cf5a97c04145d13dfffc3.png)

## Python 交互式窗口

好消息是 Visual Studio 代码支持使用 Jupyter 笔记本。要在交互窗口中运行当前文件，在资源管理器窗格中右击文件名，从上下文菜单中选择**在交互窗口中运行当前文件**选项，如下:
![Run Current File in Interactive Window option](img/1a3fd51907f5176ca68524ee24f6f94b.png)
如果 Jupyter 包还没有安装，会显示一个对话框，要求您安装:
![Installation dialog box](img/5900993ece47fa395ccaf1a4489d4b04.png)
安装完成后，会出现一个交互窗口，在这种情况下， 你需要输入一个名字来检查是否是回文:
![Check if it is palindrome or not](img/e22f3cc01cda302feccea33663e4b0d9.png)
最后，你可以在交互窗口看到结果，如下:
![It's a palindrome result](img/400ee8ad316041e28a792e8df32907c4.png)
另外，要在 VS 代码中创建新的 Jupyter 笔记本，打开命令面板，选择 **Jupyter:新建 Jupyter 笔记本**，如下:
![Jupyter: Create New Jupyter Notebook](img/d172a1d28e09e364cb751fd481b38c1e.png)
会新建一个 Jupyter 笔记本，你可以简单的创建 markdown 和 Code 单元格

## 结论

本教程讨论了 VS 代码的有用特性。然而，不可能在一个教程中涵盖 Visual Studio 代码功能，从而以更少的工作量更高效地进行编码。如果您想了解更多关于使用 VS 代码进行 Python 开发的信息，您可以查看 Visual Studio 官方代码网站上的[入门教程，了解 Visual Studio 中的 Python 代码](https://code.visualstudio.com/docs/python/python-tutorial)。