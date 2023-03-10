# 如何通过开展大型调查来收集自己的数据

> 原文：<https://www.dataquest.io/blog/how-to-conduct-a-survey-and-collect-data-for-a-data-science-project/>

November 27, 2018In this post, we’ll learn to create an online survey and how to prevent some common mistakes made in surveys. We’ll cover all steps of the survey process, including:

*   选择人口
*   取样方法
*   制定数据分析计划
*   写好问题
*   分发选项

数据科学家知道，如果基于的数据不可靠，那么即使是最巧妙的代码、最好的数据分析和最漂亮的可视化效果也是毫无价值的。那么，我们如何确保我们的调查数据是准确和有意义的呢？这是一个过程，但是让我们从一个快速测试开始。你能从这些真实的调查问题中发现问题吗？

你认为大多数政客都不诚实还是像其他人一样？[【来源】](https://www.ssc.wisc.edu/~jpiliavi/357/neuman.pdf)

*❍是*❍否

你上一次升级电脑和打印机是什么时候？[【来源】](https://s3.amazonaws.com/dq-blog-files/Bad-Questions-Lecture-Examples.pdf)

*❍年前* *❍月前* *❍几周前* *❍几天前*

这些调查问题很难解释，也很难回答。如果读者不认为政客不诚实或者不喜欢其他人呢？而如果读者前几天升级了电脑，却没有打印机呢？我们将设计一个调查来避免犯这样的错误。

## 在线调查和调查过程

数据科学家对了解人们的想法和行为感兴趣，调查是收集这类数据的最直接的方式之一。管理调查的传统方法包括电话采访、邮件调查和个人采访。但是今天，在线调查是收集人们信息的最受欢迎的方式之一，因为它有几个优点:

*   **轻松创建和管理**:有许多工具可以让您轻松设计和分发调查。
*   **接触大量人群**:没有印刷成本、发行时间等限制。
*   广泛的地理覆盖范围:我们可以将我们的调查发送给世界各地可以访问互联网的人。
*   **高响应速度**:受访者很容易完成——不需要打印调查表、填写并通过邮件返回。
*   **直接数据输入**:这减少了导入数据的时间和数据错误，这是数据科学家的一大优势。

当然，没有什么是完美的，在线调查也有缺点:

*   **低回复率**:只有一小部分参与在线调查的人可能会点击并填写全部内容(尽管这可以通过良好的调查设计和有趣的问题得到一定程度的缓解)。
*   **样本并不总是代表人口**:在线调查只接触到能上网的人。

这篇文章主要关注在线调查这种方法，但是确保在线调查是实现你的研究目标的最佳方法是很重要的。如果你工作的人群少，面对面的面试可能会更好。在人口非常多的情况下，寻找能够最好地绘制概率样本的方法，因为这允许最多的数据分析选项。包括在线调查在内的所有调查类型都遵循相同的步骤:

**1。确定你的目标。**

**2。选择受访者。**

**3。创建数据分析计划。**

**4。开展调查。**

**5。预测试调查。**

**6。分发并开展调查。**

**7。分析数据。**

**8。报告结果。**

让我们一步一步地完成调查过程。

## 第一步:确定你的目标

每个调查都以一个目的或主题开始，这个目的或主题需要分解成目标。我们的目标应该明确定义，因为它们为我们的问题和数据分析提供了信息。确保目标是具体的，可衡量的，并告知行动(如定价策略或营销活动)。让我们看一个例子:

调查的目的:评估员工对允许狗进入办公室的看法。

目标 1:评估人们对在办公室养狗的看法。

目标 2:了解员工是否对狗过敏。

*–目标 3:评估关于每月花费大约 100 美元购买狗粮的意见。*

目标 4:确定人们是否养狗以及数量。

为了定义明确的目标，我们可以研究文献，征求他人的意见，并进行定性研究。定性研究，如面对面的采访，有助于我们更多地了解我们正在研究的问题及其背景。向他人征求意见和研究文献可能有助于我们快速发现问题，为进一步研究锁定主题，或发现可能为我们的分析提供信息的现有数据集。

## 第二步:人口和抽样

#### 定义人口

我们试图通过调查来研究的一群人被称为**人口**。在我们的办公室里的狗的例子中，人口是公司的所有雇员。因此，我们将从生成所有群体成员的列表开始，我们称之为**采样框架**。在我们的狗的例子中，我们可以从人力资源部门获得员工姓名和电子邮件地址的列表。像 [SurveyMonkey](https://help.surveymonkey.com/articles/en_US/kb/SurveyMonkey-Audience) 和 [Google Surveys](https://support.google.com/surveys/answer/2447244?hl=en) 这样的调查公司也可以帮助你找到受访者。

**受访者**或**参与者**是实际参与我们研究项目的人。如果我们向所有员工(全体员工)发出参与调查的邀请，我们就是在进行人口普查。但是，通常不可能收集整个人口的数据——例如，当研究是关于美国青少年的消费习惯时。

为了解决这个问题，我们可以选择一个小组:样本。这并不理想，但如果我们抽取一个与我们的总体非常相似的样本(即，在统计上代表总体)，我们就可以根据我们调查的样本得出关于总体的结论。让我们假设我们例子中的公司非常大。我们可能需要抽取一个样本，因为调查整个公司太困难了。这意味着我们需要确保样本没有偏见(例如，我们不想只选择喜欢猫的人)，所以让我们在下一节看看如何绘制一个好的样本。

#### 在线调查的抽样方法

讨论所有的抽样方法超出了本文的范围，所以我们将集中讨论在线调查的抽样方法的选择，正如 Sue 和 Ritter 在他们关于在线调查的优秀著作中所推荐的。数据科学家通常使用概率样本。要绘制一个概率样本，我们需要一个明确定义的抽样框架，比如一个员工姓名列表。概率样本取决于随机选择，这意味着群体中的每个个体都有相同的机会被选中。使用随机选择允许您根据统计数据得出关于总体的结论。一些概率抽样方法包括:

*   **简单随机抽样和系统随机抽样，**群体中每个人被选中的机会相同。
*   **分层抽样和整群抽样，**允许我们从不同的亚群体中选择人，如年龄组或性别。

(讨论如何抽取概率样本超出了本文的范围，但是[这门课](https://www.dataquest.io/course/statistics-fundamentals/)可以帮助你了解更多关于那个话题的知识。)有一些非概率抽样技术常用于在线调查，但它们并不理想，因为它们不能用来概括整个人口。例如，方便抽样是一种对谁可以参与研究没有限制的方法。

使用这种方法的一种常见方式是在网站上发布一个调查链接来寻找参与者。这可能会导致大量的响应，但是如果没有控制谁响应的方法，我们将很难从这些数据中得出有意义的结论。另一种技术是拦截抽样，通过弹出窗口邀请网站访问者参与调查，例如，提供关于他们体验的反馈。同样，这种方法并不理想:我们无法使用这种方法对人口进行概括，而且它极大地限制了我们在分析数据时可以使用的统计方法。

#### 误差来源和样本量

我们希望我们的样本与总体相似。然而，我们从总体和样本中收集的数据之间总是存在差异，我们称之为**采样误差**。当样本看起来非常像我们正在调查的人口时，抽样误差很低。误差幅度和置信度反映了样本代表总体的程度。误差幅度越大，人们就越不应该相信调查结果能很好地代表人口。研究人员通常决定置信度，反映样本与从实际人群中收集的结果有多大差异。

减少误差的一个方法是增加样本量。[同样的 Dataquest 课程](https://www.dataquest.io/course/statistics-fundamentals/)可以帮助您衡量您的样本是否能很好地代表您的总体，以及您应该使用多大的样本量。请记住，非概率样本比概率样本需要不同的样本大小准则。当我们选择一个样本时，并不一定意味着我们知道所有将参与我们调查的人。

**未回答错误**是一个术语，描述了有多少被选中的人实际填写了调查。测量无应答是重要的，因为它影响样本是否能很好地代表总体。也有一些人开始填写调查，但没有完成或跳过问题，我们称之为**辍学者**。稍后，当我们测试我们的调查时，分析退出和跳过的问题将非常重要，因为它们可以表明我们的问题存在问题，如内容、长度、问题措辞或软件错误，这些问题将影响我们的结果。

## 第三步:创建数据分析计划

在设计我们的调查之前，我们需要一个分析计划。这将确保我们考虑我们想要分析的一切，以及我们如何才能获得统计上有代表性的结果，让我们进行分析。创建分析计划最简单的方法是写出我们的调查目标以及我们计划如何分析每个目标。例如:*目标 4:确定人们是否养狗以及有多少只*

*   调查问题:你家有几只狗？
*   *答案选项:0，1，2，3，4，5，6+。*
*   *计量水平:区间数据。*
*   *分析计划:计算最小值、最大值、平均值。*

在我们撰写调查问题时，我们希望继续参考我们的分析计划，以确保我们为所需的分析收集了正确的数据。让我们花点时间来看看衡量的水平，它决定了我们正在收集的信息的性质。我们可以收集四种类型的数据:**名义**、**序数**、**比率**和**区间**。

这些级别是分层的，间隔是我们可以收集的最高级别的数据。名义数据是与数值无关的信息，不以任何方式排序。

例如，询问性别、喜欢的狗的种类或某人的种族:

你是什么种族？

*□亚裔/太平洋岛民* *□非裔美国人* *□白人* *□拉美裔* *□美洲土著/阿拉斯加土著* *□其他*

序数数据是可以排序的数据。然而，等级之间没有相等的距离。例如:教育水平或购买产品的经历:

您对在我们网站上购买产品的体验满意吗？

*❍很不满意*❍有点不满意 ❍中立 ❍有点满意 ❍很满意

比率数据是数值，带有可解释的距离和绝对零度。一个例子是年龄:我们知道 50 岁的人比 10 岁的人大 5 倍。

你的年龄是多少？

*…..岁。*

区间数据与比率相同，但没有绝对零点。例如，用摄氏度测量的温度，其中零点是任意的，实际上并不意味着没有任何东西。

## 第四步:设计调查

至此，我们已经确定了您的目标、人口、抽样策略、调查方法和分析计划。这些都将有助于下一步:写问题和设计我们的调查。大多数调查收集三种类型的信息:

*   **被调查者的人口统计数据**，包括年龄、性别、收入、受教育程度，可以用来描述被调查者和比较被调查者的群体。
*   **可量化信息**我们可以统计分析。
*   **允许回答者添加定性数据的开放式问题**。

问题可以开放式和封闭式两种形式提出。开放式问题允许用户输入自己的答案，但不提供预定义的回答选项。开放式问题会产生难以分析的回答，对受访者来说可能是一个障碍，会增加辍学率。出于这些原因，专家们说应该少用它们，主要是为了探索一个主题和获得可引用的材料。下面是一个开放式问题的例子:

你最喜欢你工作的哪一点？

封闭式问题提供不同类型的回答选项，受访者可以从中选择一个选项。与开放式问题相比，封闭式问题易于回答，并允许我们对结果进行统计分析。封闭式问题的形式包括:

*   **选择题**。
*   **二分问题**:提供两个(或三个)选项，如“是”和“否”。它们还可以包含第三个选项，如“不知道”。
*   排名(Rankings):允许你对你认为某件事相对于其他选项的重要性进行排名。
*   **评定等级**:允许你表明你对某事的认同程度或对某事的评价。

让我们看一些例子:

**选择题**:

你在我们公司的就业状况如何？

*□全职* *□兼职* *□包工头* *□其他*

**二分问题**:

你喜欢狗吗？

*❍是*❍否

**排名问题**:

请按重要性从 1 到 4 排列以下各项，其中 1 最重要。

*干净的办公室☐* *热闹的办公室☐* *安静的办公室心无旁骛的☐* *好玩的办公室☐*

**评定量表问题**:

表明你在多大程度上同意这种说法:有狗在身边能提高我的幸福感。

*❍强烈不同意* *❍不同意* *❍既不同意也不反对* *❍同意* *❍强烈同意*

对于所有类型的问题，我们的答案选项必须详尽，这一点很重要。由于这个原因，调查问题通常包括答案选项“其他”和“我不知道”。我们认为应该包括这些选项，但要谨慎使用，尽量包括所有的答案选项，这样回答者就不会用它们来回避问题。

答案还需要是互斥的，这意味着它们不会彼此重叠。大多数调查还使用偶然性问题，即一个接一个的问题，并允许受访者跳过问题。在线调查有助于自动跳过与之前的答案不相关的问题。

例如:

**应急问题** : *你养狗吗？*

*❍是*❍否

你愿意偶尔带你的狗去上班吗？

*❍是*❍否

在精心设计的在线调查中，对第一个问题回答“否”的用户不会被要求回答第二个问题。不要忘记，我们的问题应该由我们的目标和分析计划提供信息。

#### 写好问题

写出好的调查问题需要付出努力，这些问题要容易理解，对受访者有意义。难以理解的问题会导致烦恼和辍学，而不感兴趣或不相关的问题会导致“无态度”,参与者不断地使用猜测或“不知道”选项。这篇文章的引言展示了糟糕的调查问题的例子:难以解释和回答的问题。这是我们简介中最糟糕的调查问题之一:

你认为大多数政客都不诚实还是像其他人一样？ *❍有* *❍没有*这个问题是:

*   **双管齐下**:它确实由两个问题组成(“你认为政客诚实吗？”以及“你认为政治家和其他人一样吗？”)，很难完整回答。
*   这意味着“其他人”通常是诚实的，这表明如果我们认为政客不像其他人，那么我们也必须相信他们不诚实。
*   **暧昧**:有待解读。

总的来说，这个问题回答起来很烦，烦的问题会造成掉线和不回应。我们不想那样。这是引言中第二个“糟糕”的调查问题:

你上次升级电脑和打印机是什么时候？

*❍年前* *❍月前* *❍几周前* *❍几天前*

这个问题很难回答，因为它假设我们同时拥有一台计算机和一台打印机，并且我们在相似的时间升级它们。如果我们不拥有其中一个，或者我们拥有它们，但只升级了一个，不清楚我们应该选择哪个答案。好的调查问题包括:

*   **不言自明**:它们应该容易理解和回答。
*   **明确的**:应该只有一种方式来解释或理解问题。
*   **对我们的受访者有意义**。
*   **穷尽**。
*   **互斥**。
*   **短**。
*   **没有行话**。
*   **视觉吸引力**。
*   **没有绝对**:我们应该提供多种答案选项，避免在我们的问题中使用“总是”或“曾经”这样的词。
*   **匹配受访者的语言**:我们应该使用受访者能够理解的语言，并解释任何需要的技术术语。

让我们改写引言中的两个“糟糕”的调查问题:

**糟糕的调查问题**:

你认为大多数政客都不诚实还是像其他人一样？

*❍是*❍否

**好的调查问题**:

*表明你在多大程度上同意这种说法:“我认为美国目前在职的政客不诚实”。*

*❍强烈不同意* *❍不同意* *❍既不同意也不反对* *❍同意* *❍强烈同意*

**糟糕的调查问题**:

你上一次升级电脑和打印机是什么时候？

*❍年前* *❍月前* *❍几周前* *❍几天前*

**好的调查问题**(分成两个问题):

你上次升级电脑是什么时候？

*❍从未* *❍年前* *❍月前* *❍几周前* *❍几天前* *❍我没有自己的电脑*

*您上次升级打印机是什么时候？*

*❍从未* *❍年前* *❍月前* *❍几周前* *❍几天前* *❍我没有自己的打印机*

#### 提出有效的问题

我们希望我们的调查问题能够衡量它们应该衡量的东西。如果我们试图衡量人们是否升级了他们的电脑，人们用“几年前”来回答“不好”的调查问题，因为他们没有打印机，也从来没有升级过他们的电脑，我们实际上没有衡量任何东西。

此外，我们需要确保我们正在收集准确的信息。回答者回答问题不准确有几个原因。正如 Schaeffer 和 Presser 所指出的，人们可能并不总是对与你的调查相关的事件有准确的记忆。为了最大限度地减少这种影响，我们应该尝试要求受访者只回忆最近发生的事件，我们应该尝试询问具体的事件(而不是一系列事件)。参与者也可以根据他们认为对他们的期望，或者根据社会规范来回答问题。我们称之为社会期望偏见。

但是这里有一个好消息:根据克罗伊特等人的说法。艾尔。与其他调查方法相比，在线调查实际上增加了敏感信息的报告。因此，这种偏见不会像电话调查那样强烈地影响我们的调查。即便如此，所有的调查类型都有社会期望偏差的风险。

我们可以通过强调匿名性和保密性来减少这种偏见，并在可能有社会理由撒谎时，以一种使答案看起来可以接受的方式来措辞问题。

比如:**不好的调查问题**:

你曾经违章超速吗？

**更好的调查问题**:

在过去的一周内，你是否超速驾驶过？(提醒:本次调查的回答是匿名的)

参与者可能无法准确回答我们的调查问题的其他原因是他们不知道如何回答，或者回答他们实际上没有意见的问题。在测试您的调查问题时寻找这一点并相应地调整它们是很重要的。询问受访者他们是如何理解某个问题的，或者询问他们为什么选择某个选项。他们的回答可能会揭示这类问题。

#### 格式化调查

在线调查工具使设计调查变得容易，并使其具有吸引力。但是，有几点需要注意。正如 Mick P. Couper 在他的书 [*设计有效的网络调查*](https://www.amazon.com/Designing-Effective-Surveys-Mick-Couper/dp/0521717949?pldnSite=1) 中指出的，在线调查的设计非常重要，因为没有面试官来补偿糟糕的设计并解释其含义。

如果我们决定在调查中使用起始页，我们需要确保以有趣的方式介绍调查。我们还应该提到完成需要多长时间，并感谢参与者花时间填写。强调匿名和保密。简介要简短。问题的顺序应该基于我们的回答者(不仅仅是我们)感兴趣的内容，因为我们希望他们填写整个调查。

你有没有做过一个让你觉得自己在回答无穷无尽的人口统计学问题的调查？这是一个众所周知的烦恼。保持调查简短，以人口统计学问题结束调查，而不是以人口统计学问题开始调查。我们可以使用任何数量的不同格式和风格，但是无论我们选择什么都需要是常规的和可读的(在颜色、字体和大小方面)。它还需要包括明确的指示。如果我们使用评级和尺度，它们也应该是清晰和常规的。以下是一些众所周知的响应格式:

*   **小圆圈(单选按钮)**:这些通常表示你可以在选择题中选择一个选项。
*   复选框:这通常意味着你可以选择所有适用的选项。
*   **下拉菜单**:这些菜单通常会显示“选择一个”的文字，这样回答者就知道他们可以向下滚动选择。请注意，下拉菜单对用户不太友好，因为用户需要单击/点击并滚动才能看到所有可能的答案。只有当我们必须包含一长串答案时(例如，询问居住国家)，才最好使用下拉列表。
*   **文本框**:用于回答开放式问题。
*   **排名选项**:这些选项允许回答者添加 1、2、3、4、5 等。对偏好进行排序。

重要的是，我们的调查可以被我们样本中的每个人访问，包括残障人士、慢速互联网连接和不同类型的设备。这对我们的影响程度将取决于我们的样本。不同的语言选项和声音选项是我们可能要考虑添加的常见功能。

## 第五步:预测试

仅仅因为我们已经写好了调查问题，并不意味着调查已经准备好了！下一步是测试，以确保我们将获得我们所期待的响应质量。预测试可以揭示许多问题，比如措辞不当的问题和遗漏的回答类别。首先，我们可以自己做一些测试:

*   检查不同的电脑、平板电脑、手机和浏览器，确保一切正常。比如:评分量表在手机上能用吗？
*   检查是否有任何应急问题自动跳过。
*   确保图像，图形，声音，链接和图表的工作和加载。
*   寻找风格问题。你能向下滚动吗？字体好读吗？

其次，我们将召集一小群以前没有看过调查的人。然后，我们将让他们参加调查，如果他们有问题，不提供任何帮助或澄清。之后，我们可以和他们谈谈，看看他们的回答，看看我们的调查中是否有需要纠正的问题。

最后，在更大的人群中测试调查，并跟踪他们是如何做的。我们希望测量完成问题和整个调查所需的时间，我们将关注人们在哪些地方“辍学”,以及他们跳过了哪些问题。有一些分析工具可以帮助解决这个问题。所有这些测试阶段都应该提供有用的见解，我们可以使用这些见解来提高我们调查的质量。做出更改后，我们将继续测试循环，直到测试发现的所有问题都得到解决。那么终于到了…

## 第六步:进行调查

分发调查有不同的方式:

*   通过电子邮件发送。
*   使用链接在网站或应用程序上嵌入或做广告。
*   在网站或应用程序上使用弹出窗口。
*   在社交媒体上发帖。

这些方法中的每一种都与不同的采样策略相关，并且每一种都会影响我们在获得结果后所拥有的数据分析选项。我们可以用来帮助设计和分发调查的在线工具包括 [SurveyMonkey](https://www.surveymonkey.com/) 、[谷歌调查](https://surveys.google.com/)和 [Typeform](https://www.typeform.com/surveys/) 。这些工具可以帮助调查过程中的大多数步骤，包括抽样框架、设计调查、调查广告和简单的数据分析。我们可以通过多种方式提高调查的回复率:

*   使用吸引人的调查邀请(或广告)。
*   保持调查简短。
*   向回答者提供一些东西，作为他们填写调查的交换条件，比如某个产品的折扣(但请注意，这会影响抽样误差)。
*   后续邀请提醒人们填写您的调查。

## 第七步:数据分析

大多数调查工具包括基本的统计和简单的数据分析选项。但这是 Dataquest，我们是数据科学家！我们肯定希望下载数据并进行我们自己的分析，因为这为我们提供了更多的可能性。我们在处理数据时不要忘记的一件事是:我们还应该分析受访者接受调查的方式。

我们应该跟踪人们从哪里退出，他们花了多长时间完成调查，等等。，并使用这些见解来增强我们的数据分析。我们还应该分析数据丢失的原因(如果有丢失的话)，并查看异常值以了解这些异常值如何影响结果。具体的方法超出了本文的范围，但是 [Dataquest 课程可以帮助你掌握这些技能](https://www.dataquest.io/data-science-courses-directory/)。

## 第八步:报告结果

最后一步是报告调查结果。根据我们调查的目的，我们可能会做以下事情:

*   写一份报告。
*   在会议上介绍结果。
*   将结果作为更大研究项目的一部分。

如果我们要写一份报告，我们需要事先确定它的读者群，并把他们考虑在内。例如，如果听众是一个忙碌的主管，他们可能想要一个最相关和可操作的信息的快速鸟瞰总结；其他受众可能想要更深入的逐问题分析。在任何类型的报告中，我们都要记住，对于某些人来说，数字似乎很抽象，很难理解。我们应该使用视觉辅助工具(如图表)来描述我们的结果。

#### 结论

在线调查是为数据科学项目收集数据的有效方式。然而，需要对调查过程中的每一步给予认真关注，以确保调查将产生有用、有效、有代表性的答复数据。下面快速回顾一下这些步骤，以供参考:

**1。确定你的目标。**

**2。选择受访者。**

**3。创建数据分析计划。**

**4。开展调查。**

**5。预测试调查。**

**6。分发并开展调查。**

**7。分析数据。**

**8。报告结果。**

#### 资源

偏导数:克里斯·阿尔邦、黄邦贤·摩根和维迪亚·斯潘达纳(2017 年)、

[“你的调查糟透了”](https://partiallyderivative.com/podcast/2017/02/13/your-surveys-suck)。Mick P. Couper (2008)，*设计有效的网络调查*，剑桥大学出版社。斯坦利·普莱塞、米克·p·库帕、朱迪思·t·莱斯勒、伊丽莎白·马丁、让·马丹、詹妮弗·m·罗斯格布&埃莉诺·辛格(2004)、

[《调查问题的测试与评估方法》](https://academic.oup.com/poq/article/68/1/109/1855073)《民意季刊》。Kreuter，f .，Presser，s .，和 Tourangeau，R. (2008 年)，

[“CATI、IVR 和网络调查中的社会期望偏差:模式和问题敏感性的影响”](https://academic.oup.com/poq/article/72/5/847/1833162)，《公共舆论季刊》。Bradburn，s . sud man 和 b . Wansink(2004 年)，

[“提问:问卷设计的权威指南——用于市场研究、政治民意调查以及社会和健康问卷(社会科学的研究方法)”](https://www.wiley.com/en-us/Asking+Questions%3A+The+Definitive+Guide+to+Questionnaire+Design+For+Market+Research%2C+Political+Polls%2C+and+Social+and+Health+Questionnaires%2C+2nd%2C+Revised+Edition-p-9780787970888)，Wiley and sons。谢弗，北卡罗来纳州，和普雷斯，S. (2003 年)，

[《提问的科学》](https://www.annualreviews.org/doi/10.1146/annurev.soc.29.110702.110112)《社会学年刊》。瓦莱丽·m·苏&洛伊斯·a·里特(2007)、

*开展在线调查*，Sage Publications。