# 学习 Bash 的 11 个理由(也称为命令行)

> 原文：<https://www.dataquest.io/blog/why-learn-the-command-line/>

April 21, 2021![why learn command line data science](img/72ec77cef4a5227b10cde4064eb833b7.png)

bash——基于 Unix 的操作系统的命令行语言——允许您像开发人员一样控制您的计算机。但是这不仅仅是软件开发人员的技能——学习 bash 对于任何使用数据的人来说都是有价值的。

## 什么是 Bash？

简而言之，Bash 就是 Unix 命令行界面(CLI)。您还会看到它被称为终端、命令行或 shell。它是一种命令语言，允许我们以一种比使用 GUI(图形用户界面)更有效和强大的方式在我们的计算机上处理文件。

从图形用户界面(GUI)切换到命令行界面可能会让人感到力不从心。虽然 [Dataquest 让学习命令行变得非常简单](https://www.dataquest.io/course/command-line-beginner/)，但是您可能会想:我为什么要费事呢？

以下是您应该学习 bash 并使用命令行的几个原因:

## 1.Bash 技能很受欢迎，而且报酬丰厚

根据 2020 年 Stack Overflow 的开发者调查，bash/shell(即 Linux 命令语言解释器家族)是第六大使用最多的语言，排在 Python 和 R 之前。根据调查，它还与比 Python 或 R 更高的[工资](https://insights.stackoverflow.com/survey/2020#top-paying-technologies)有关。

它在最受欢迎的技术列表中排名很高(53.7%)，在最令人恐惧的技术列表中排名较低(46%)。

虽然 StackOverflow 的调查涵盖了所有类型的软件开发人员和工程师，但命令行与数据科学家尤其相关，因为 [Bash/Shell 与数据科学技术](https://insights.stackoverflow.com/survey/2018#correlated-technologies)密切相关，如 Python、IPython/Jupyter、TensorFlow 和 PyTorch。这也得到了最近由 [Python 软件基金会](https://en.wikipedia.org/wiki/Python_Software_Foundation)进行的 [Python 开发者调查](https://www.jetbrains.com/lp/python-developers-survey-2019/)的支持。

## 2.命令行技能有助于构建可重复的数据流程

数据科学家的部分职责是确保某些信息定期可用，通常是每天可用。大多数时候，这些数据是以同样的方式获取、处理和显示的。

命令行非常适合这个目的，因为命令很容易自动化和复制。

考虑以下情况:

你的雇主决定投资数据分析。几名数据专业人员将加入该团队。您的任务是确保他们的机器具备启动所需的一切。

如果你能使用 CLI(命令语言解释器),你可以写一些脚本来自动安装、配置和测试一切。

如果你不能，你将不得不求助于一个图形用户界面，在几台机器上重复进行同样的鼠标点击动作。

这只是终端技能如何帮助提高数据科学流程的可扩展性和可重复性的一个例子。

## 3.学习 Bash 让你更灵活

在数据科学角色中，如果您可以使用终端，而不是依赖于通过 GUI 点击，您会经常发现您有更多的灵活性。

由于命令行是运行其他程序的程序(因此得名“shell”)，程序之间的交互往往更容易在命令行中调整。

一旦掌握了 bash 命令，编写脚本就相对容易了，shell 脚本使得构建各种数据管道和工作流更加简单。

更广泛地说，知道如何使用 shell 为您提供了与计算机交互的第二种选择。

您可以随时使用 GUI，但是命令行可以在您需要的时候为您提供更直接的功能和控制。

## 4.使用文本文件更容易

文本文件是存储和处理数据最常用的方法之一。几乎所有的数据科学项目都会涉及到文本文件。因此，能够快速有效地处理文本文件对于数据科学家来说是一项非常有用的技能。

shell 拥有非常强大的文本处理工具，如 [AWK](https://en.wikipedia.org/wiki/AWK) 和 [sed](https://en.wikipedia.org/wiki/Sed) ，这些工具有助于熟悉文件并方便数据清理。

例如，下面的代码使用 AWK 打印名为 a_csv_file 的文件的第一列和第三列，其中第二个字段的值是 Dataquest，使用逗号作为字段分隔符。

```
awk 'BEGIN {FS=","} {if ($2=="Dataquest") {print $1 $3} } a_csv_file'
```

它只需要一行代码！

## 5.它不太耗费资源

当您使用有限的计算资源或者只是想最大化您的速度时，使用命令行实际上总是比使用 GUI 好，因为使用 GUI 意味着资源必须专用于呈现图形输出。

对于本地和远程工作都是如此。远程连接时，GUI 比终端消耗更多的带宽，浪费资源。

此外，当使用 GUI 时，[延迟](https://en.wikipedia.org/wiki/Latency_(engineering))，即“刺激和响应之间的时间间隔”，将会更长，如果你试图控制比你实际移动落后一两秒的鼠标，这可能会特别令人沮丧。

如果您只是在命令行中键入，延迟可能会更低，也更容易处理，因为您知道在任何给定时间光标在哪里。

## 6.您需要关于云的命令行技能

云服务通常通过命令行界面连接和操作。

这对于深度学习等更高级的数据科学工作尤为重要，在深度学习中，您的本地计算资源可能不足以完成您想要执行的任务。引用 Nucleus Research 的这篇 2018 年的文章:

> 在去年的研究中，不到 10%的[深度学习]项目是在本地运行的。这一趋势已经加速，2018 年只有 4%的项目在内部运行。

根据同一篇文章，“今天 96%的深度学习正在云中运行。”

如果你有兴趣学习像深度学习这样的高级技术，命令行技能对于高效地将数据移入和移出云是必要的。

## 7.Unix Shell 技能可以很好地移植到其他 Shell

只有几个流行的 shell(bash，zsh，fish，ksh，tcsh，cmd，Windows PowerShell 等等。)而且它们的相似之处多于不同之处，很容易在它们之间切换。

例如，您在我们的[命令行课程](https://www.dataquest.io/course/command-line-beginner/)中学习的 bash 命令将在基于 Unix 的机器上工作，如 MAC 和 Linux 计算机。但是许多完全相同的命令也可以在 Windows 的命令提示符和/或 Windows PowerShell 中运行。

当您使用需要某种命令行界面的在线服务时，这种交叉兼容性特别有用。即使他们的系统不使用 bash，它也会使用一些足够相似的东西，这样你就能搞清楚了。

另一方面，GUI 有无限多种，学习一种并不一定有助于学习其他任何一种。

## 8.你打字的速度可能比你点击的速度还快

研究表明，使用鼠标会很快达到平稳状态，而使用键盘，尽管学习曲线很陡，但效率会更高。

251 名有经验的 Microsoft Word 用户接受了一份调查问卷，评估他们对最常见命令的选择。与我们的预期相反，大多数有经验的用户很少使用高效的键盘快捷键，而是喜欢使用图标工具栏。

第二项研究证实了键盘快捷键确实是最有效的方法。六名参与者使用菜单选择、图标工具栏和键盘快捷键执行常见命令。正如所料，键盘快捷键是最有效的。

换句话说:即使你觉得通过 GUI 工作很快，至少对于某些任务来说，你在命令行会更有效率。

## 9.审计和调试更加容易

因为在命令行上跟踪您的所有活动非常容易，所以审计和调试要容易得多。

您可以很容易地查看日志来跟踪您在 shell 中执行的每一个操作，而如果在使用 GUI 时一次误点击导致了一个错误，则很可能没有记录。

## 10.Unix Shell 随处可见

虽然它只内置在 Mac 和 Linux 机器上，但 Windows 用户仍然可以使用诸如 [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux) 、 [Cygwin](https://en.wikipedia.org/wiki/Cygwin) 和 [MinGW](https://en.wikipedia.org/wiki/MinGW) 这样的工具来享受乐趣。(如前所述，您将学习的许多 bash 命令都可以在 Windows 的原生 sell 选项中工作，比如命令提示符)。

这意味着你在[这些课程](https://www.dataquest.io/course/command-line-beginner/)中学到的命令行技能将可以在你遇到的几乎每台计算机上使用(包括你的个人机器，无论你使用什么操作系统)。

## 11.命令行比您想象的要简单

有一种误解，认为使用命令行需要您知道几百个命令。事实上，尽管有数百个命令*可供*使用，但您可能只需要其中很小一部分命令来完成大多数常见的数据科学任务。

**还不服气？**我们将为您奉上一句名言(免费！)本书[Linux 命令行](https://linuxcommand.org/tlcl.php):

> 当我被要求解释 Windows 和 Linux 的区别时，我经常用一个玩具做类比。

> Windows 就像一个游戏机。你去商店买一个全新的盒子。你把它带回家，打开它，玩它。漂亮的图形，可爱的声音。然而，过了一段时间，你厌倦了附带的游戏，所以你回到商店再买一个。这个循环一遍又一遍地重复。

> 最后，你回到商店，对柜台后面的人说，“我想要一个这样的游戏！”却被告知不存在这样的游戏，因为它没有“市场需求”。然后你说:“但是我只需要改变这一件事！”柜台后面的人说你不能换它。这些游戏都被密封在它们的盒子里。你发现你的玩具仅限于别人认为你需要的游戏。

> 另一方面，Linux 就像是世界上最大的安装程序。你打开它，它只是一大堆零件。有很多钢支柱，螺钉，螺母，齿轮，滑轮，电机，以及一些关于建造什么的建议。所以，你开始玩它。你建立了一个又一个建议。

> 过了一会儿，你会发现你对做什么有了自己的想法。你不必再去商店，因为你已经拥有了你需要的一切。竖立设置采取你的想象力的形状。它做你想做的。当然，你对玩具的选择是个人的事情，那么你会觉得哪种玩具更令人满意呢？

## 准备好学习命令行了吗？

现在您理解了为什么学习 bash 命令行界面是有价值的了，那么实际上您该如何去做呢？

最简单的方法是通过 Dataquest 的引导式交互式命令行课程，在您的浏览器中直接学习。您将学习高效工作所需的所有命令，在注册后的几分钟内编写真正的 bash 命令。

最棒的是？我们的三门课程都是免费试的！

## 现在就开始学习 Bash 吧！

注册一个 Dataquest 账户，免费尝试任何一门课程(或全部三门课程)。你将在不到五分钟的时间内编写你的第一个命令。

[Elements of the Command Line](https://app.dataquest.io/signup?course=command-line-elements)

新手，从这里开始！

[Text Processing in the Command Line](https://app.dataquest.io/signup?course=text-processing-cli)[Command Line — Intermediate](https://app.dataquest.io/signup?course=command-line-intermediate)

## 你会学到什么？

在这两个命令行课程中，您将学习使用 Mac 和 Linux 机器上内置的 Unix 终端界面。别担心，我们还将为 Windows 用户提供充分利用内容所需的工具。

在第一门课程中，您将了解什么是命令行界面，为什么它在数据科学工作流中如此重要，以及如何通过向计算机发出指令(称为命令)来导航和管理计算机。您还将了解通配符，以及如何将它们与诸如`ls`、`mv`、`cp`、`mkdir`等命令一起使用，以实现更快的搜索和工作流。

第二个课程集中在 shell 中的基本文本处理，使用像`head`、`cat`、`cut`和`grep`这样的命令。它涵盖了如何将这些命令组合起来，从更简单的构建块中创建强大的命令链。您还将了解到[多用户系统](https://www.computerhope.com/jargon/m/multsyst.htm)和输出重定向的强大功能。

与所有 Dataquest 课程一样，这些新的命令行课程使用交互式命令行环境和答案检查，允许您直接在浏览器中应用和检查您学习的所有内容。

## 现在就开始学习 Bash 吧！

注册一个 Dataquest 账户，免费尝试任何一门课程(或全部三门课程)。你将在不到五分钟的时间内编写你的第一个命令。

[Elements of the Command Line](https://app.dataquest.io/signup?course=command-line-elements)

新手，从这里开始！

[Text Processing in the Command Line](https://app.dataquest.io/signup?course=text-processing-cli)[Command Line — Intermediate](https://app.dataquest.io/signup?course=command-line-intermediate)