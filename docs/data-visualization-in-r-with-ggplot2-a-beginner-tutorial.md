# 用 ggplot2 实现 R 中的数据可视化:初学者教程

> 原文：<https://www.dataquest.io/blog/data-visualization-in-r-with-ggplot2-a-beginner-tutorial/>

September 2, 2020

据说一位著名的将军说过:“一篇好的速写胜过一篇长篇大论。”这些建议可能来自战场，但也适用于许多其他领域，包括数据科学。通过在 R 中使用 ggplot2 可视化来“勾画”我们的数据比简单地描述我们发现的趋势更有影响力。

![ggplot2-sketch](img/3ee3844e85c98e0703c377591d183f71.png "ggplot2-sketch")

勾勒出一栋房子的设计比试图用语言描述要清楚得多。对于数据来说也是如此——这就是 ggplot2 数据可视化的用武之地！

这就是为什么我们*将*数据可视化。我们将数据可视化是因为**从我们能看到的东西中学习比阅读**更容易。对于使用 R 的数据分析师和数据科学家来说，值得庆幸的是，有一个名为 [ggplot2](https://ggplot2.tidyverse.org/) 的 [tidyverse 包](https://www.tidyverse.org/packages/)，让数据可视化变得轻而易举！

在这篇博客文章中，我们将学习如何获取一些数据并使用 R 生成一个可视化。要完成它，最好是你已经了解了 [R 编程语法](https://www.dataquest.io/course/introduction-to-data-analysis-in-r/)，但是你不需要成为专家或者有任何使用 ggplot2 的经验。

## 介绍数据

国家健康统计中心从 1900 年开始跟踪美国的死亡率趋势。他们收集了美国公民预期寿命和死亡率的 T2 数据。

我们想知道随着时间的推移，预期寿命是如何变化的。随着医学和技术的进步，我们可以预期寿命会增加，但我们只有看一看才能确定！

如果你想复制我们将在这篇博文中创建的图表，请在此下载数据集[并跟随！](https://data.world/dataquest/life-expectancy-data)

(不确定如何在个人电脑上使用 R？查看[如何开始使用 RStudio](https://www.dataquest.io/blog/tutorial-getting-started-with-r-and-rstudio/) ！)

## 图表中有什么？

在我们开始这篇文章之前，我们需要一些背景知识。有许多类型的可视化，但大多数都可以归结为以下几点:

![line chart](img/dd5bbfbb124168f32cf5970fa5a7a0dd.png)

我们可以将这个图分解成基本的构件:

*   用于创建图的数据:

![line chart](img/3c318fdc54c3ae47a377d3b68e1ac257.png)

2.  该图的坐标轴:

![line chart](img/eb8a16135c05e72820172aff0049c2af.png)

3.  用于可视化数据的几何形状。在这种情况下，一行:

![line chart](img/d4b17df2c886f43dabf67cd65de3b051.png)

4.  有助于读者理解情节的标签或注释:

![line chart](img/3e2800d4b0820b171527e53f0b50b195.png)

将剧情分解成**层**很重要，因为这是 [`ggplot2`包](https://ggplot2.tidyverse.org/index.html)理解和构建剧情的方式。`ggplot2`包是`tidyverse`中的一个包，负责可视化。当你继续阅读这篇文章时，请记住这些层次。

## 导入数据

为了开始可视化，我们需要将数据放入我们的工作空间。我们将引入`tidyverse`包，并使用`read_csv()`函数导入数据。我们将数据命名为`life_expec.csv`，所以您需要根据您对文件的命名来重命名它。

```
library(tidyverse)
life_expec <- read_csv("life_expec.csv")
```

让我们看看我们正在处理哪些数据:

```
colnames(life_expec)
[1] "Year"    "Race"        “Sex"         "Avg_Life_Expec"    "Age_Adj_Death_Rate"
```

我们可以通过 Year 列看到时间是以年为单位编码的。有两栏可以让我们区分不同的种族和性别类别。最后，最后两列对应于预期寿命和死亡率。

让我们快速浏览一下数据，看看某一年的情况如何:

```
life_expec %>%
  filter(Year == 2000)
```

2000 年有九个数据点:

```
## # A tibble: 9 x 5
##    Year Race      Sex        Avg_Life_Expec Age_Adj_Death_Rate
##                                      
## 1  2000 All Races Both Sexes           76.8               869
## 2  2000 All Races Female               79.7               731.
## 3  2000 All Races Male                 74.3              1054.
## 4  2000 Black     Both Sexes           71.8              1121.
## 5  2000 Black     Female               75.1               928.
## 6  2000 Black     Male                 68.2              1404.
## 7  2000 White     Both Sexes           77.3               850.
## 8  2000 White     Female               79.9               715.
## 9  2000 White     Male                 74.7              1029.
```

一年有九个不同的行，每一行对应一个不同的人口统计分区。对于这个可视化，我们将关注美国整体，因此我们需要相应地过滤数据:

```
life_expec <- life_expec %>%
  filter(Race == "All Races", Sex == "Both Sexes")
```

数据放在一个合适的位置，所以我们可以将它放入一个 ggplot()函数，开始创建一个图表。我们使用 ggplot()函数来表示我们想要创建一个图。

```
life_expec %>%
  ggplot()
```

这段代码生成了一个空白图(如下所示)。但是它现在“知道”使用`life_expec`数据，尽管我们还没有看到它的图表。

![ggplot_1](img/09100a1b420b886d677475dd1c44d0a4.png)

## 建造轴线

现在我们已经准备好了数据，我们可以开始构建我们的可视化。我们需要建立的下一层是轴。我们对预期寿命如何随时间变化感兴趣，所以这表示我们的两个轴是什么:`Year`和`Avg_Life_Expec`。

为了指定轴，我们需要使用`aes()`功能。`aes`是“美学”的缩写，它是我们告诉`ggplot`我们想要为情节的不同部分使用什么*列*的地方。我们正试图通过时间来观察预期寿命，所以这意味着`Year`将走向 x 轴，而`Avg_Life_Expec`将走向 y 轴。

```
life_expec %>%
  ggplot(aes(x = Year, y = Avg_Life_Expec))
```

添加了`aes()`函数后，图表现在知道哪些列属于轴:

![plot_example](img/3c7af8baa67b4dd98d19b68d25c71517.png)

但是请注意，图中仍然没有任何内容！我们仍然需要告诉`ggplot()`使用什么样的形状来可视化`Year`和`Avg_Life_Expec`之间的关系。

## 指定几何图形

通常，当我们想到可视化时，我们通常会想到图形的类型，因为我们看到的形状告诉了我们大多数信息。虽然`ggplot2`包在选择绘制数据的形状方面给了我们很大的灵活性，但对于我们的问题来说，花一些时间来考虑哪一个是最好的*是值得的。*

 *我们正试图想象预期寿命是如何随着时间的推移而变化的。这意味着应该有一种方法让我们直接比较过去和未来。换句话说，我们需要一个有助于显示连续两年之间关系的形状。对于这一点，线图是很好的。

为了用 `ggplot()`创建一个线图，我们使用了`geom_line()`函数。`geom`是我们希望用来可视化数据的特定形状的名称。所有用于绘制这些形状的函数前面都有`geom`。`geom_line()`创建折线图，`geom_point()`创建散点图，等等。

```
life_expec %>%
  ggplot(aes(x = Year, y = Avg_Life_Expec)) +
  geom_line()
```

注意在使用了`ggplot()`函数之后，我们开始使用`+`符号添加更多的层。这一点很重要，因为我们使用`%>%`来告诉`ggplot()`哪些数据起作用。在使用`ggplot()`的之后，我们使用`+`给剧情添加更多的图层。

![plot_example](img/33cda88dca760a3e135ec934c94d7494.png)

这张图表正是我们要找的！看看总的趋势，预期寿命随着时间的推移而增长。

如果我们只是快速查看数据，我们可以在这里停止绘图，但这种情况很少发生。更常见的是，您将为报告或团队中的其他人创建可视化。在这种情况下，情节是不完整的:如果我们把它交给一个没有上下文的队友，他们不会理解这个情节。理想情况下，你所有的情节都应该能够通过注解和标题来解释。

## 添加标题和轴标签

目前，图形将列名作为两个轴的标签。这对于`Year`来说已经足够了，但是我们需要改变 y 轴。为了改变绘图的轴标签，我们可以使用`labs()`函数，并将其作为一个层添加到绘图上。`labs()`可以改变轴标签和标题，所以我们将在这里合并。

```
life_expec %>% # data layer
  ggplot(aes(x = Year, y = Avg_Life_Expec)) + # axes layer
  geom_line() + # geom layer
  labs(  # annotations layer
    title = "United States Life Expectancy: 100 Years of Change",
    y = "Average Life Expectancy (Years)"
  )
```

我们最后润色的图表是:

![plot_example](img/540488a0032723833e82d8535c898d64.png)

## 结论:ggplot2 强大！

只用了几行代码，我们就制作出了一个很棒的可视化效果，告诉我们所有我们需要知道的关于美国普通人预期寿命的信息。可视化是所有数据分析师的必备技能，R 让它变得容易掌握。

如果您有兴趣了解更多信息，请查看我们在 R path 的[数据分析师！R path 中的数据分析师包括一门关于使用`ggplot2`在 R](https://www.dataquest.io/path/data-analyst-r/) 中进行[数据可视化的课程，在这里您将学习如何:](https://www.dataquest.io/course/r-data-viz/)

*   使用折线图直观显示随时间的变化。
*   使用直方图了解数据分布。
*   使用条形图和箱线图比较图形。
*   使用散点图了解变量之间的关系。

### 准备好提升你的 R 技能了吗？

我们 R path 的[数据分析师涵盖了你找到工作所需的所有技能，包括:](/path/data-analyst-r/)

*   使用 **ggplot2** 进行数据可视化
*   使用 **tidyverse** 软件包的高级数据清理技能
*   R 用户的重要 SQL 技能
*   **统计**和概率的基础知识
*   ...还有**多得多的**

没有要安装的东西，**没有先决条件**，也没有时间表。

[Start learning for free!](https://app.dataquest.io/signup)*