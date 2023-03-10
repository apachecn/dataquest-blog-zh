# 数据科学家访谈:本杰明·鲁特

> 原文：<https://www.dataquest.io/blog/benjamin-root-interview/>

November 2, 2015

在 [Dataquest](https://www.dataquest.io/) ，作为数据科学教育过程的一部分，我们努力帮助我们的用户更好地了解数据科学在行业中的工作方式。我们已经开始了一系列采访经验丰富的数据科学家的活动。我们重点介绍了他们的故事，他们对初露头角的数据科学家的建议，以及他们解决的各种问题。这是我们这个系列的第二篇文章，是对数据科学家和工程师 [Benjamin Root](https://github.com/WeatherGod) 的采访。

Benjamin Root 是 Matplotlib 数据可视化库的贡献者，致力于改进 Matplotlib 中的文档和 mplot3d 工具包。Ben 以前是气象专业的研究生，他尝试使用 MATLAB 处理数据的经历促使他学习 Python，并进一步参与 SciPy 社区。

#### 你参与 Matplotlib 项目有多久了，是什么推动你参与进来的？

**Benjamin:** 我在 2009 年初开始使用 Python，当时几个同事注意到我在使用 MATLAB 时遇到的一些问题，建议我尝试一下 Python。有了一些示例 pylab 脚本可以学习，我开始将我的一些`.m`文件转换成`.py`文件。我有一些非常复杂的 MATLAB 代码，所以我的同事向我推荐了 NumPy、SciPy 和 Matplotlib 的各种邮件列表。有了这些列表的反馈，我设法将我所有的 MATLAB 代码转换成 Python，声明我将“永远不再”使用 MATLAB。

好了，是时候进行新的开发和新的可视化了，我一直在 NumPy 和 Matplotlib 中遇到各种烦人的错误和边缘情况。我会用示例代码报告错误，开发人员通常会修复错误。最终，开发人员会开始引导我找到问题所在，我开始自己编写补丁。提交补丁大约一年后，John Hunter 给了我提交权限。他后来说，他把提交权给了任何一个“惹恼”他的人。**所以，参与 Matplotlib 是因为惹恼了我的同事，然后惹恼了开发人员……**

#### 你和 Matplotlib 有什么关系？你在项目中扮演什么样的角色？

Benjamin: 最初，我在 Matplotlib 中关注的是文档，因为我有“新鲜的眼睛”，所以我更容易注意到错误和粗糙的边缘。现在我已经阅读文档很多次了，我甚至再也看不到错误了。**顺便说一下，这就是为什么开发人员鼓励新人提交文档补丁的原因！**我还试图打磨我不断遇到的粗糙边缘，特别是用 Matplotlib 打包的 mplot3d 工具包。我向 mplot3d 提交了足够多的补丁，以至于我成了它事实上的维护者。

目前，我的重点是审查 GitHub 上的 pull 请求，以及诊断/验证 bug 报告。我仍然不时地提交新特性(特别是，在即将到来的 1.5 版本中寻找新的“属性循环”和“经典风格”特性)。我参与设计讨论，试图确保我们在新功能的早期做出好的设计决策。我也将很快成为底图工具包的维护者。在邮件列表中，我倾向于回答新来者的大多数问题，尽管自从 StackOverflow 到来后，邮件列表流量大幅下降。

#### 许多数据科学家和数据工程师依赖开源库，但不知道如何回报。对于那些想更多地参与开源的人，你有什么最好的建议？

本杰明:这类用户最适合像 Matplotlib 这样的库。他们通常是在图书馆能力的边界上工作的人。所以，对于那些用户来说，遇到错误和限制是很自然的。把那些错误报告归档！在邮件列表上提问，并给予反馈和批评！开发人员确实希望从他们的用户那里听到反馈，因为这意味着他们的工作正在被使用！参与开源不能用补丁数量来统计。开源开发不仅仅是代码和文档。

软件不会自发地从以太中“产生”。软件满足需求，是用户定义了需求。我个人认为，对开源项目最有价值的贡献者是那些向开发人员提供反馈和批评的人。没有他们，项目就会停滞不前，并因冷漠而死亡。所以，我对你的读者的挑战是找出他们直接使用最多的三个库和工具，并订阅他们的用户邮件列表。然后，下一次在使用这些工具时发生“奇怪的事情”或“意想不到的事情”,不要把它当作你自己的错误或你必须忍受的限制。给这个列表发一封电子邮件，询问是否有人认为这很奇怪或出乎意料。求解答。鼓励讨论。

最后，如果开发人员要求对某件事进行反馈，接受他们的要求。拜托，看在上帝的份上，**请**在测试版上试用你的软件，发布这些库的候选库，并报告问题！

#### 你最近出版了一本名为使用 Matplotlib 的[交互式应用的书。写这本书的目的是什么？你的目标读者是谁？](https://www.amazon.com/Interactive-Applications-using-Matplotlib-Benjamin/dp/1783988843/) 

Benjamin: 主要是，我想提高自己在 Matplotlib 这方面的知识，同时也为它制作更好的文档。交互性是 Matplotlib 的一个非常重要的特性，但是它的文档却很少。我能在网上找到的所有例子和教程都只是展示了不同的方面，它们彼此之间是如此的脱节。没有任何叙述可以帮助用户**从坚实的基础上建立他们的理解。在书中，我们不是让所有的示例代码完全相互独立，而是一点一点地构建一个应用程序。**

我非常喜欢这种叙述方式的一点是，在 widgets 一章中，我有一种特别的满足感，我们添加了一个滑块，然后我向读者指出，新的滑块会自动绑定到键盘快捷键和其他一些按钮上，这是我们在上一章中所做的。这在典型的示例驱动文档中是不可能的，因为键盘快捷键的演示与小部件的演示完全无关。

本书的目标读者是那些对 Python 和面向对象编程有一定经验，并且需要用 Python 制作交互式可视化效果的人。您不需要任何 Matplotlib 或任何其他科学 Python 工具的经验，但我也不会花太多时间解释可能的可视化以及如何定制它们。有很多关于这方面的教程。我假设您已经可以按照您想要的方式显示一个图，但是需要构建与该图交互的工具。

#### 你作为气象学研究生的经历如何影响了你现在的工作？

本杰明:气象学和许多其他科学一样，是一种非常直观的体验。将数据视为文本是理解数据的一种糟糕方式。所以，一个研究生必须能够创造他们自己的作品的可视化。有几个工具可以做到这一点，有些是非常具体的大气科学，如 NCL 和 GEMPAK，而其他人更普遍，如 MATLAB 语言。

作为一名研究生，我一直在尝试新的事物，并且我正在以一种前所未有的方式使用工具。因此，我一直在这些工具中遇到错误。真正将我从 MATLAB 世界推向 Python 的是修复这些错误的能力。此外，现在 Matplotlib 中有一些功能是从气象学的角度培养出来的。例如，我经常需要使用非常精确的地图来可视化我的数据，因此我对底图和制图等项目特别感兴趣，以确保它们满足我的需求。流图和风倒钩是我们在 Matplotlib 中为气象学家开发的另外两个功能。

#### 你对新兴的数据可视化工具包 [Vispy](https://vispy.org/) 有什么想法？

本杰明: Vispy 是一个令人惊叹的项目，显示了巨大的潜力。它的一个原始组件 glumpy 实际上是在我的鼓励下创建的，因为 Matplotlib 附带的 mplot3d 工具包存在局限性。Vispy 的几个原始组件是在 Matplotlib 开发人员的要求下创建的，因为我们看到了在 GPU 上执行可视化的价值，但我们没有足够的专业知识，也没有带宽在维护 Matplotlib 的同时进行这样的项目。

我们也不认为我们可以将这样的实验特性添加到 Matplotlib 中，而不会对我们的用户造成重大的干扰。因此，我们鼓励一些带着这些奇妙想法来到邮件列表的开发人员从头开始，将他们的想法变成现实，即使这只是一个概念证明。这些项目和其他一些项目合并到了 Vispy 中。

在 2015 年 SciPy 大会上，Matplotlib 开发团队有机会与一些 Vispy 开发人员深入讨论我们的项目在 Python 生态系统中的位置、我们各自的路线图以及我们两个项目的未来。这是一次非常激动人心的讨论。虽然我们还没有任何长期计划，但肯定有一些更直接的目标，例如 Vispy 使用 Matplotlib 的文本渲染代码来提高情节的视觉质量。我们还将与 Vispy 开发人员合作，为将 Vispy 作为后端连接到 Matplotlib 创建一些概念验证。的确，Python 中数据可视化的未来看起来非常光明！