# 为什么你应该学习数据工程

> 原文：<https://www.dataquest.io/blog/why-learn-data-engineering/>

October 16, 2019![why-learn-data-engineering](img/1d63cc9b31314784a84444afbde1741d.png)

令人兴奋的消息:我们刚刚推出了一个完全改进的数据工程途径 ，为任何想成为数据工程师或学习一些数据工程技能的人提供从头开始的培训。

![YouTube video player for DU1PirFI21A](img/0ebe79f9b8295cd3ea998d4b74e08ba3.png)

*[https://www.youtube.com/embed/DU1PirFI21A?feature=oembed](https://www.youtube.com/embed/DU1PirFI21A?feature=oembed)*

 *看起来很酷，对吧？但是它回避了一个问题:为什么首先要学习数据工程？

通常，数据科学团队由数据分析师、数据科学家和数据工程师组成。在[之前的一篇文章](https://www.dataquest.io/blog/data-analyst-data-scientist-data-engineer)中，我们已经讨论了这些角色之间的区别，但是在这里，让我们更深入地探讨成为一名数据工程师的一些优势。

数据工程师是连接公司或机构内所有数据生态系统的人。他们通过以下方式实现这一目标:

*   从应用程序和系统中访问、收集、审核和清理数据，使其处于可用状态
*   创建和维护高效的数据库
*   构建数据管道
*   监控和管理所有数据系统(可扩展性、安全性等)
*   以可扩展的方式实现数据科学家的成果

做上面列出的所有事情主要需要一种特殊的技能:**编程**。数据工程师是专门研究数据和数据技术的软件工程师。

这使得他们与数据科学家截然不同，后者当然有编程技能，但通常不是工程师。数据科学家将他们的工作(例如，推荐系统)交给数据工程师进行实际实施的情况并不少见。

虽然进行分析的是数据分析师和数据科学家，但通常是数据工程师在构建数据管道和其他必要的系统，以确保每个人都可以轻松访问他们需要的数据(并且没有人可以访问不应该访问的数据)。

强大的软件工程和编程基础使数据工程师能够构建数据团队及其公司成功所需的工具。或者，正如杰夫·马格努松所说:“我喜欢把它想象成乐高积木。工程师设计新的乐高积木，数据科学家以创造性的方式组装这些积木，以创造新的数据科学。”

这就引出了你想成为数据工程师的第一个原因:

## 1.为什么要学数据工程？这是数据科学的支柱

数据工程师站在数据策略的第一线，这样其他人就不需要站在第一线。他们是第一批处理进入公司系统的结构化和非结构化数据的人。它们是任何数据策略的基础。没有乐高积木，毕竟建不出乐高城堡。

![ai_hierarchy](img/7642aa27756220b69a77b1862998af3f.png)

在上面的*数据科学需求层次*(由 [Monica Rogati](https://en.wikipedia.org/wiki/Monica_Rogati) 提出)中，数据工程师完全负责最下面的两行，与数据分析师和数据科学家共同负责倒数第三行。

为了更好地理解数据工程有多重要，想象一下上图中的金字塔被用作一个漏斗，并上下颠倒。数据被倒入这个漏斗的顶端，第一个接触到它的人是数据工程师。他们过滤、清理和引导数据的效率越高，随着数据沿着漏斗进一步流向其他团队成员，其他事情的效率也越高。

相反，如果数据工程师*不*高效，他们会成为漏斗中的一个阻碍，损害下游每个人的工作。例如，如果一个构建糟糕的数据管道最终向数据科学团队提供了不完整的数据，他们对这些数据执行的任何分析都可能是无用的。

这样，数据工程师就成了数据策略成果的倍增器。他们是站在数据分析师和数据科学家肩膀上的巨人。

拥有良好数据战略的公司组建团队的方式就是明证。据数据工程师、[大数据研究所](https://www.bigdatainstitute.io/)董事总经理 Jesse Anderson 称:

“一个常见的起点是每个数据科学家对应 2-3 名数据工程师。对于一些具有更复杂的数据工程要求的组织，每个数据科学家可以有 4-5 名数据工程师。”

## 2.这在技术上很有挑战性

数据分析师和科学家使用最多的 Python 函数之一是 read _ CSV——来自 [pandas](https://en.wikipedia.org/wiki/Pandas_(software)) 库。该函数将存储在文本文件中的表格数据读入 Python，以便可以对其进行浏览和操作。

如果您以前在 Python 中处理过数据，那么您可能非常习惯于输入如下内容:

```py
import pandas as pd df = pd.read_csv("a_text_file.csv")
```

简单方便吧？`read_csv`函数是软件工程本质的一个很好的例子:创建抽象的、广泛的、有效的和可伸缩的解决方案。

这是什么意思，它与学习数据工程有什么关系？让我们深入了解一下。

*   抽象。当在计算机中读取文件时，一个非常复杂的过程在幕后发生。然而，我们对函数的使用非常简单，后台发生的事情是从用法中抽象出来的。你不需要理解`read_csv`在“引擎盖下”做什么来有效地使用它。
*   广阔。这个函数还允许我们明确地选择在文本的文件表格数据中使用什么分隔符(例如，逗号、分号、制表符等等)。这使得它很容易与各种 CSV 风格一起使用，这对于数据科学家来说是一种享受。还有许多其他选择，允许数据从业者专注于他们的目标，而不必担心编程细节。
*   高效。快速高效地工作，读入代码也很高效。
*   可扩展。此功能包含的另一个选项允许我们按块读取文件，因此，如果文件太大而无法读入计算机的 RAM，则可以按块读取，这样用户就可以按文件大小来处理文件。

[罗伯特·A·海因莱因](https://en.wikipedia.org/wiki/Robert_A._Heinlein)据说说过“一个人的*魔法*是另一个人的工程”。正是数据工程师发挥了这种魔力，构建了像`read_csv`函数这样抽象、广泛、高效且可扩展的工具，这样团队的其他成员就可以专注于数据本身及其分析，而不必纠结于编程难题。

(同时，数据工程可能比数据科学需要更少的数学，所以如果你喜欢编程而不是数学，数据工程可能是一个理想的选择！)

## 3.这是值得的

让数据科学家的生活更轻松并不是激励数据工程师的唯一事情。不可否认，数据工程师正在对整个世界产生重大且日益增长的影响。

每天，我们都会创建 2.5 万亿字节的数据，今天的数据量之大使得数据工程师比以往任何时候都更加重要。 [Business Insider](https://en.wikipedia.org/wiki/Business_Insider) 报告称，到 2025 年，物联网设备将超过 640 亿台，高于 2018 年的 100 亿台和 2017 年的 90 亿台。随着这种增长，来自更多来源的数据越来越多，因此对能够有效处理和传输这些数据的工程师的需求也越来越多。

这意味着数据工程师有大量不同的方式来追求他们的兴趣和深化他们的技能。为了让你了解这个世界有多大，这里列出了一些流行的数据工具和技术:[亚马逊红移](https://en.wikipedia.org/wiki/Amazon_Redshift)、[亚马逊 S3](https://en.wikipedia.org/wiki/Amazon_S3) 、[阿帕奇卡珊德拉](https://en.wikipedia.org/wiki/Apache_Cassandra)、[阿帕奇 HBase](https://en.wikipedia.org/wiki/Apache_HBase) 、[阿帕奇卡夫卡](https://en.wikipedia.org/wiki/Apache_Kafka)、[阿帕奇火花](https://en.wikipedia.org/wiki/Apache_Spark)、[阿帕奇动物园管理员](https://en.wikipedia.org/wiki/Apache_ZooKeeper)、 [Azure](https://en.wikipedia.org/wiki/Microsoft_Azure) 、 [ElephantDB](https://github.com/nathanmarz/elephantdb) [Memcached](https://en.wikipedia.org/wiki/Memcached) ，[微软 SQL Server](https://en.wikipedia.org/wiki/Microsoft_SQL_Server) ， [Mongo DB](https://en.wikipedia.org/wiki/MongoDB) ， [Oracle 数据库](https://en.wikipedia.org/wiki/Oracle_Database)， [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL) ， [Redis](https://en.wikipedia.org/wiki/Redis) ， [SQLite](https://en.wikipedia.org/wiki/SQLite) ， [Storm](https://en.wikipedia.org/wiki/Storm_(software)) ， [SAP IQ](https://en.wikipedia.org/wiki/SAP_IQ) ， [Teradata](https://en.wikipedia.org/wiki/Teradata#Technology_and_products) 和 [Vertica](https://en.wikipedia.org/wiki/Vertica) 。

当然，数据工程师不需要知道所有这些，但是这个列表说明了在数据工程的世界里有多少事情要做。一旦你掌握了找工作的技能，你就有了很大的自由来选择你在做什么和使用什么工具。

由于数据工程师同时拥有数据和软件工程技能，他们也有能力构建各种产品。想为早期创业做贡献，还是想成为一名企业家并在某一天找到自己的事业？数据工程技能为您提供了构建优秀产品和分析这些产品性能所需的工具。你将能够实现和衡量你能想到的几乎任何事情的成功。

想要远程工作？根据 [2019 年的未来劳动力报告](https://www.upwork.com/i/future-workforce/fw/2019/)，“未来三年，五分之二的全职员工将远程工作”。因此，如果办公室之外的工作适合你，数据工程可以帮助你实现这个目标。因为对数据工程师的需求很高，而且因为大部分工作可以远程完成，所以绝对有可能找到远程数据工程工作，或者作为短期数据工程项目的自由承包商为自己工作。

最后，数据工程师也有很多机会回馈社区。根据 [2019 的 Stack Overflow 开发者调查](https://insights.stackoverflow.com/survey/2019#developer-roles)，“Stack Overflow 上约 65%的职业开发者每年为开源项目贡献一次或更多”。由于你将拥有数据和工程技能，你将能够为数据科学社区开发新的酷工具。

## 4.报酬很高

你不应该只根据薪水来选择一份工作，但是不可否认薪水很重要！

根据 IBM 的 [The Quant Crunch:对数据科学技能的需求如何扰乱就业市场](https://www.ibm.com/downloads/cas/3RL3VXGA)，“机器学习技能的工作平均薪酬为 114，000 美元。广告上的数据科学家职位平均工资为 10.5 万美元，广告上的**数据工程职位平均工资为 11.7 万美元。***【着重强调另加。]*

至于为什么，这并不奇怪。在 StackOverflow 的开发人员调查中，像 Python、SQL 和 shell 这样的数据工程技能经常被列为薪酬最高的技能。在撰写本文时，LinkedIn 上的搜索词*数据科学家*有大约 70，000 个结果，搜索词*数据工程师*有大约 112，500 个结果。在 GlassDoor 上，差异甚至更明显:数据科学家约为 22，500，而数据工程师约为 77，100(过滤了上个月发布的职位)。

不仅对数据工程师的需求很大，而且这种需求还在持续增长！截至 2019 年 6 月，数据工程师需求同比增长*88%*([来源](https://insights.dice.com/2019/06/04/data-engineer-remains-top-demand-job/))。

这还不是全部！[据 Statista](https://www.statista.com/statistics/254266/global-big-data-market-forecast) 称，“到 2027 年，全球大数据市场预计将增长至 1030 亿美元，是其 2019 年预期市场规模的两倍多”。

## 5.即使你不想成为数据工程师，它也是有价值的

即使你不想从事数据工程师的职业，如果你想从事数据科学方面的工作，掌握一些数据工程知识也会非常有用。好处是多方面的:

*   作为一名数据从业者，您很有可能会定期被要求完成与其他工作角色有一些重叠的任务，包括数据工程。
*   学习一种不同的看待事物的方式有利于你的理解，它给你一个机会去温习你可能有一段时间没用过的技能。
*   拥有工程技能会让你更加自给自足。这可以极大地帮助你的职业生涯，因为你不再需要被封锁，等待别人为你做些什么。
*   学习数据工程技能可以让你与数据工程师产生共鸣，更好地与他们沟通。这也将帮助你的团队，因为你可以成为连接你的团队和数据工程团队的桥梁。

### 成为一名数据工程师！

现在就学习成为一名数据工程师所需的技能。注册一个免费帐户，访问我们的交互式 Python 数据工程课程内容。

[Sign up now!](https://app.dataquest.io/signup)

*(免费)*

![YouTube video player for ddM21fz1Tt0](img/5a85348206993fc2a430506128b76684.png)

*[https://www.youtube.com/embed/ddM21fz1Tt0?rel=0](https://www.youtube.com/embed/ddM21fz1Tt0?rel=0)**