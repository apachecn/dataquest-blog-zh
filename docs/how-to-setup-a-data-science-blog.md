# 为你的投资组合建立一个数据科学博客

> 原文：<https://www.dataquest.io/blog/how-to-setup-a-data-science-blog/>

June 14, 2016Data science blogs can be a fantastic way to demonstrate your skills, learn topics in more depth, and build an audience. There are quite a few examples of [data science](https://github.com/rushter/data-science-blogs) and [programming](https://www.quora.com/What-are-the-best-programming-blogs) blogs that have helped their authors land jobs or make important connections. Writing a data science blog is thus one of the most important things that any aspiring programmer or data scientist should be doing on a regular basis. *(This is the second in a series of posts on how to build a Data Science Portfolio. You can find links to the other posts in this series at the bottom of the post.)* Unfortunately, one very arbitrary barrier to blogging can be knowing how to set up a blog in the first place. In this post, we’ll cover how to create a blog using Python, how to create posts using Jupyter notebook, and how to deploy the blog live using GitHub Pages. After reading this post, you’ll be able to create your own data science blog, and author posts in a familiar and simple interface.

## 静态站点

从根本上说，静态站点只是一个装满 HTML 文件的文件夹。我们可以运行一个允许其他人连接到这个文件夹并检索文件的服务器。这样做的好处是，它不需要数据库或任何其他移动部件，并且很容易在 GitHub 这样的网站上托管。让您的数据科学博客成为一个静态网站是一个很好的主意，因为这使得维护它变得非常简单。创建静态站点的一种方法是手动编辑 HTML，然后将装满 HTML 的文件夹上传到服务器。在这种情况下，您至少需要一个

`index.html`文件。如果你的网址是`thebestblog.com`，访问者访问了`https://www.thebestblog.com`，他们就会看到`index.html`的内容。下面是一个 HTML 文件夹如何寻找`thebestblog.com`:

```
 thebestblog.com
│   index.html
│   first-post.html
│   how-to-use-python.html
│   how-to-do-machine-learning.html
│   styles.css 
```

在上述网站上，访问

`https://www.thebestblog.com/first-post.html`会显示`first-post.html`中的内容，以此类推。`first-post.html`可能是这样的:

```
 <html>
<head>
  <title>The best blog!</title>
  <meta name="description" content="The best blog!"/>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <h1>First post!</h1>
  <p>This is the first post in what will soon become (if it already isn't) the best blog.</p>
  <p>Future posts will teach you about data science.</p>

<div class="footer">
  <p>Thanks for visiting!</p>
</div>
</body>
</html> 
```

您可能会立即注意到手动编辑 HTML 时的一些问题:

*   手动编辑 HTML 非常痛苦。
*   如果你想写多篇文章，你必须复制样式和其他元素，比如标题和页脚。
*   如果你想集成评论或其他插件，你必须写 JavaScript。

一般来说，当你写博客的时候，你想把注意力集中在内容上，而不是花时间和 HTML 较劲。幸运的是，您可以创建一个数据科学博客，而无需使用静态站点生成器工具手工编辑 HTML。

## 静态现场发电机

静态站点生成器允许你以简单的格式写博客文章，通常是 markdown，然后定义一些设置。然后生成器会自动将你的帖子转换成 HTML。使用静态站点生成器，我们可以大大简化

`first-post.html`变成`first-post.md`:

```
 # First post!

This is the first post in what will soon become (if it already isn't) the best blog.

Future posts will teach you about data science. 
```

这比 HTML 文件更容易管理！常见的元素，如标题和页脚，可以放在模板中，因此可以很容易地更改它们。有几种不同的静态站点生成器。最流行的叫做

[杰基尔](https://jekyllrb.com/)，而且是用红宝石写的。由于我们将创建一个数据科学博客，我们需要一个静态的站点生成器来处理 Jupyter 笔记本。 [Pelican](https://blog.getpelican.com/) 是一个用 Python 编写的静态站点生成器，它可以接收 Jupyter 笔记本文件并将它们转换成 HTML 博客文章。Pelican 还使得将我们的博客部署到 GitHub 页面变得容易，其他人可以在那里阅读我们的博客。

## 安装鹈鹕

在我们开始之前，

[这是](https://github.com/dataquestio/jupyter-blog)一个回购协议，这是我们最终会达成的一个例子。如果您没有安装 Python，那么在我们开始之前，您需要做一些初步的设置。[这里的](https://www.anaconda.com/distribution/)是 Python 的设置说明。我们推荐使用`Python 3.5`。安装 Python 后:

*   创建一个文件夹—我们会将我们的博客内容和风格放在该文件夹中。在本教程中，我们将它称为`jupyter-blog`，但是您可以随意称呼它。
*   `cd`变成`jupyter-blog`。
*   创建一个名为`.gitignore`的文件，并将来自[的内容添加到这个文件](https://github.com/github/gitignore/blob/master/Python.gitignore)中。我们最终需要将我们的回购提交给 git，当我们这样做时，这将排除一些文件。
*   创建并激活一个虚拟环境。
*   在`jupyter-blog`中创建一个名为`requirements.txt`的文件，内容如下:

```
 Markdown==2.6.6
pelican==3.6.3
jupyter>=1.0
ipython>=4.0
nbconvert>=4.0
beautifulsoup4
ghp-import==0.4.1
matplotlib==1.5.1 
```

*   运行`jupyter-blog`中的`pip install -r requirements.txt`，安装`requirements.txt`中的所有软件包。

## 创建您的数据科学博客

一旦你完成了初步的设置，你就可以创建你的博客了！奔跑

在`jupyter-blog`中的`pelican-quickstart`，为您的博客启动一个交互式设置序列。你会得到一系列的问题来帮助你正确地建立你的博客。对于大多数问题，只要点击`Enter`并接受默认值就可以了。您应该填写的只是网站的标题、网站的作者、URL 前缀的`n`和时区。这里有一个例子:

```
 (jupyter-blog)➜  jupyter-blog ✗ pelican-quickstart
Welcome to pelican-quickstart v3.6.3.

This script will help you create a new Pelican-based website.

Please answer the following questions so this script can generate the files
needed by Pelican.

> Where do you want to create your new web site? [.]
> What will be the title of this web site? Vik's Blog
> Who will be the author of this web site? Vik Paruchuri
> What will be the default language of this web site? [en]
> Do you want to specify a URL prefix? e.g., https://example.com   (Y/n) n
> Do you want to enable article pagination? (Y/n)
> How many articles per page do you want? [10]
> What is your time zone? [Europe/Paris] America/Los_Angeles
> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n)
> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n)
> Do you want to upload your website using FTP? (y/N)
> Do you want to upload your website using SSH? (y/N)
> Do you want to upload your website using Dropbox? (y/N)
> Do you want to upload your website using S3? (y/N)
> Do you want to upload your website using Rackspace Cloud Files? (y/N)
> Do you want to upload your website using GitHub Pages? (y/N) 
```

跑步后

`pelican-quickstart`，你应该有两个新文件夹在`jupyter-blog`、`content`、`output`，还有几个文件，比如`pelicanconf.py`、`publishconf.py`。以下是文件夹中应包含内容的示例:

```
 jupyter-blog
│   output
│   content
│   .gitignore
│   develop_server.sh
│   fabfile.py
│   Makefile
│   requirements.txt
│   pelicanconf.py
│   publishconf.py 
```

## 安装 Jupyter 插件

默认情况下，Pelican 不支持使用 Jupyter 写博客——我们需要安装一个

支持此行为的插件。我们将把这个插件作为 [git 子模块](https://git-scm.com/docs/git-submodule)来安装，以便于管理。如果你没有安装 git，你可以在这里找到说明[。一旦安装了 git:](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

*   运行`git init`将当前文件夹初始化为 git 存储库。
*   创建文件夹`plugins`。
*   运行`git submodule add git://github.com/danielfrg/pelican-ipynb.git plugins/ipynb`添加插件。

您现在应该有一个

`.gitmodules`文件和一个`plugins`文件夹:

```
 jupyter-blog
│   output
│   content
│   plugins
│   .gitignore
│   .gitmodules
│   develop_server.sh
│   fabfile.py
│   Makefile
│   requirements.txt
│   pelicanconf.py
│   publishconf.py 
```

为了激活插件，我们需要修改

在底部加上这几行:

```
 MARKUP = ('md', 'ipynb')

PLUGIN_PATH = './plugins'
PLUGINS = ['ipynb.markup'] 
```

这些行告诉 Pelican 在生成 HTML 时激活插件。

## 写你的第一篇文章

一旦插件安装完毕，我们就可以创建第一篇文章了:

*   用一些基本的内容创建一个 Jupyter 笔记本。这里有一个例子，你可以下载。
*   将笔记本文件复制到`content`文件夹中。
*   创建一个与您的笔记本同名的文件，但扩展名为`.ipynb-meta`。[这里有一个](https://github.com/dataquestio/jupyter-blog/blob/master/content/first-post.ipynb-meta)的例子。
*   将以下内容添加到`ipynb-meta`文件中，但更改字段以匹配您自己的帖子:

```
 Title: First Post
Slug: first-post
Date: 2016-06-08 20:00
Category: posts
Tags: python firsts
author: Vik Paruchuri
Summary: My first post, read it to find out. 
```

以下是对这些字段的解释:

*   `Title` —帖子的标题。
*   `Slug` —在服务器上访问帖子的路径。如果 slug 是`first-post`，而你的服务器是`jupyter-blog.com`，你将在`访问帖子。`
`*   `Date` —帖子发布的日期。*   `Category` —文章的类别(可以是任何内容)。*   `Tags` —用于帖子的标签的空格分隔列表。这些可以是任何东西。*   `Author` —帖子作者的姓名。*   `Summary` —帖子的简短摘要。`

 `您需要复制一个笔记本文件，并创建一个

每当你想在博客上发表新文章时，就提交文件。一旦创建了笔记本和元文件，就可以生成博客 HTML 文件了。下面是一个示例，展示了`jupyter-blog`文件夹现在应该是什么样子:

```
 jupyter-blog
│   output
│   content
    │   first-post.ipynb
    │   first-post.ipynb-meta
│   plugins
│   .gitignore
│   .gitmodules
│   develop_server.sh
│   fabfile.py
│   Makefile
│   requirements.txt
│   pelicanconf.py
│   publishconf.py 
```

## 生成 HTML

为了从我们的帖子生成 HTML，我们需要运行 Pelican 将笔记本转换为 HTML，然后运行本地服务器来查看它们:

*   切换到`jupyter-blog`文件夹。
*   运行`pelican content`来生成 HTML。
*   切换到`output`目录。
*   运行`python -m pelican.server`。
*   在浏览器中访问`localhost:8000`来预览博客。

您应该能够浏览数据科学博客中所有帖子的列表，以及您创建的特定帖子。

## 创建 GitHub 页面

[GitHub Pages](https://pages.github.com/) 是 GitHub 的一个特性，允许你快速部署一个静态站点，并允许任何人使用唯一的 URL 访问它。为了进行设置，您需要:

*   [如果你还没有注册 GitHub，请注册](https://github.com/)。
*   创建一个名为`username.github.io`的存储库，其中`username`是您的 GitHub 用户名。[这里有一个关于如何做的更详细的指南。](https://help.github.com/articles/create-a-repo/)
*   切换到`jupyter-blog`文件夹。
*   通过运行`git remote add origin [[email protected]](/cdn-cgi/l/email-protection):username/username.github.io.git`将存储库添加为本地 git 存储库的远程存储库——用您的 GitHub 用户名替换对`username`的两个引用。

GitHub 页面将显示推送到

位于 URL `username.github.io`的仓库`username.github.io`的`master`分支(仓库名称和 URL 相同)。首先，我们需要修改 Pelican，使 URL 指向正确的位置:

*   编辑`publishconf.py`中的`SITEURL`，使其设置为`https://username.github.io`，其中`username`是你的 GitHub 用户名。
*   运行`pelican content -s publishconf.py`。当你想在本地预览你的博客时，运行`pelican content`。在你部署之前，运行`pelican content -s publishconf.py`。这将使用正确的设置文件进行部署。

## 提交您的文件

如果您想将实际的笔记本和其他文件存储在与 GitHub 页面相同的 Git repo 中，可以使用 Git 分支。

*   运行`git checkout -b dev`创建并切换到一个名为`dev`的分支。我们不能使用`master`来存储我们的笔记本，因为那是 GitHub Pages 使用的分支。
*   像平常一样创建一个提交并推送到 GitHub(使用`git add`、`git commit`和`git push`)。

## 部署到 GitHub 页面

我们需要将博客的内容添加到

`master` branch for GitHub 页面正常工作。目前，HTML 内容在文件夹`output`中，但是我们需要它在存储库的根目录中，而不是在子文件夹中。为此，我们可以使用 [ghp-import](https://github.com/davisp/ghp-import) 工具:

*   运行`ghp-import output -b master`将`output`文件夹中的所有内容导入到`master`分支。
*   使用`git push origin master`将您的内容推送到 GitHub。
*   尝试访问`username.github.io` —您应该会看到您的页面！

每当您对数据科学博客进行更改时，只需重新运行

上面的`pelican content -s publishconf.py`、`ghp-import`和`git push`命令，你的 GitHub 页面就会更新。

## 添加注释

评论是与客人互动的一种方式。Disqus 是一个很好的工具，它与 Pelican 无缝集成。请遵循以下步骤:

1.  前往 [Disqus 网站](https://disqus.com/)进行注册。
2.  点击“开始”，然后选择“我想在我的网站上安装 Disqus”。
3.  输入您的网站名称。通过将 Disqus 传递到`publishconf.py`文件中，这将作为链接 Disqus 到您的博客的唯一键。(在以后的步骤中会详细介绍这一点。)
4.  选择 Disqus 订阅计划——基本计划最适合个人博客。
5.  当 Disqus 询问你的站点在哪个平台上时，向下滚动并选择“我没有看到我的平台被列出，用通用代码手动安装”。
6.  在通用代码页面上，再次向下滚动并单击“配置”。
7.  在“配置”页面上，用您的实际网站地址(`https://username.github.io`)填写“网站 URL”部分。您还可以添加关于您的评论策略的信息(如果您没有评论策略，Disqus 会给出建议)，并为您的站点输入描述。点击“完成设置”。
8.  现在，您将能够配置您站点的社区设置。点击进入这一部分，环顾四周。在其他事情中，你可以控制客人是否可以评论，并激活广告。
9.  在左边的工具栏中，点击“高级”并将您的网站作为`username.github.io`添加到可信域中。
10.  最后，更新`publishconf.py`。确保指定`DISQUS_SITENAME = "website-name"`，其中“网站名称”来自步骤 3。

现在重新运行

`pelican content -s publishconf.py`、`ghp-import output -b master`和`git push origin master`命令来更新你的 GitHub 页面。刷新你的网站，你会看到每个帖子下面都有 Disqus。

## 选择一个主题

鹈鹕社区提供了各种各样的主题

[pelicanthemes.com](https://www.pelicanthemes.com/)。您可以选择您喜欢的任何主题，但这里有一些快速提示:

*   保持简单。设计不应该偏离实际内容。
*   记住“三色法则”。根据多伦多大学的研究，大多数人更喜欢两到三种颜色的组合。这样颜色就不会争抢注意力。
*   注意你的页面的宽度——它应该足以包含你可能想要发布的信息图和代码。

一旦你选择了一个主题，去你想存放你的主题的文件夹并创建一个仓库:

`git clone --recursive https://github.com/getpelican/pelican-themes pelican-themes`。在您的`pelicanconf.py`文件中创建一个`THEME`变量，并将其值设置为主题的位置:`THEME = 'E:\\Pelican\\pelican-themes\\flex`这里，我们使用了一个由 [Alexandre Vincenzi](https://www.alexandrevicenzi.com/) 设计的 flex 主题。运行常用的修整命令— `pelican content`、`ghp-import`和`git push` —享受全新的外观！

## 接下来的步骤

我们已经走了很长的路了！您现在应该能够创作博客文章并将其推送到 GitHub 页面。任何人都应该能够访问您的数据科学博客

`username.github.io`(用你的 GitHub 用户名替换`username`)。这为您展示您的数据科学产品组合提供了一个绝佳的方式。随着你写的文章越来越多并赢得了读者，你可能会想深入几个领域:

*   你自己的自定义网址。
    *   使用`username.github.io`很好，但是有时你想要一个更加定制的域。[这里是](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)关于使用 GitHub 页面自定义域名的指南。
*   插件
    *   点击查看插件列表[。插件可以帮助你设置分析、评论等等。](https://github.com/getpelican/pelican-plugins)
*   促进
    *   尝试在网站 [Twitter](https://www.twitter.com) 、 [Quora](https://www.quora.com) 和其他网站上推广你的博客文章，以获得受众。

在

[Dataquest](https://www.dataquest.io) ，我们的互动指导项目旨在帮助您开始构建数据科学组合，向雇主展示您的技能，并获得一份数据方面的工作。如果你感兴趣，你可以[注册并免费学习我们的第一个模块](https://www.dataquest.io)。

* * *

*如果你喜欢这篇文章，你可能会喜欢阅读我们“构建数据科学组合”系列中的其他文章:*

*   *[用数据讲故事](https://www.dataquest.io/blog/data-science-portfolio-project/)。*
*   *[打造机器学习项目](https://www.dataquest.io/blog/data-science-portfolio-machine-learning/)。*
*   *[建立数据科学投资组合的关键是让你找到工作](https://www.dataquest.io/blog/build-a-data-science-portfolio/)。*
*   *[寻找数据科学项目数据集的 17 个地方](https://www.dataquest.io/blog/free-datasets-for-projects)*
*   *[如何在 GitHub](https://www.dataquest.io/blog/how-to-share-data-science-portfolio/)* 上展示您的数据科学作品集`