Title: 使用Pelican和Github pages搭建博客
Date: 2022-01-27 16:30
Modified: 2022-01-27 16:30
Category: Python
Tags: pelican, publishing
Slug: build-blogs-with-pelican-and-github-pages
Author: Jerry
Summary: Build blogs with pelican and github pages

## 创建Github仓库
GitHub可以托管各种git库的站点。通过GitHub Pages生成的静态站点，可以免费托管、自定义主题、并且自制网页界面。

1. 首先到Github进行账号注册：https://github.com/。
2. 注册后登录Github，右上角点击“Creat a new repo”，跳转到新页面后填写相关内容，注意版本库名使用'username.github.io'的格式，这里将username替换成自己的用户名即可。
3. 设置和选择好页面模板后就可以生成然后发布新网页了。
4. 创建SSH密钥并上传到Github。

关于SSH认证：[Windows/Linux/Mac下使用SSH密钥连接Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

官方文档：[Github官方文档在这里](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)

## 安装Pelican
Pelican 是一个静态站点生成器，用Python编写。
通过在首选终端中运行以下命令，在 Python 3.6+ 上安装 Pelican（如果您打算使用它，还可以选择 Markdown）：
```
$ python3 -m pip install "pelican[markdown]"
```
官方文档：[Pelican官方文档](https://docs.getpelican.com/en/latest/)

## 快速配置
将username.github.io项目克隆到本地，切换到该目录
```
$ git clone git@github.com:username/username.github.io.git
$ cd username.github.io
```
通过命令创建一个框架项目pelican-quickstart，首先询问有关您网站的一些问题，对于括号中表示默认值的问题，请随时使用 Return 键接受这些默认值。
这些问题只是用来生成配置文件的，我们后面都可以通过修改配置文件来手动修改这些设置。我填写的内容如下：
```
$ pelican-quickstart 
Welcome to pelican-quickstart v4.7.1.

This script will help you create a new Pelican-based website.

Please answer the following questions so this script can generate the files
needed by Pelican.

    
> Where do you want to create your new web site? [.] 
> What will be the title of this web site? Jerry's blog
> Who will be the author of this web site? Jerry
> What will be the default language of this web site? [en] 
> Do you want to specify a URL prefix? e.g., https://example.com   (Y/n) n
> Do you want to enable article pagination? (Y/n) 
> How many articles per page do you want? [10] 
> What is your time zone? [Europe/Rome] Asia/Shanghai
> Do you want to generate a tasks.py/Makefile to automate generation and publishing? (Y/n) 
> Do you want to upload your website using FTP? (y/N) 
> Do you want to upload your website using SSH? (y/N) 
> Do you want to upload your website using Dropbox? (y/N) 
> Do you want to upload your website using S3? (y/N) 
> Do you want to upload your website using Rackspace Cloud Files? (y/N) 
> Do you want to upload your website using GitHub Pages? (y/N) y
> Is this your personal page (username.github.io)? (y/N) y
Done. Your new project is available at /home/jerry/Code/Jerry-se.github.io
```
回答完所有问题后，项目将包含以下层次结构（pages除外- 显示在下面的括号中 - 如果您计划创建非按时间顺序排列的内容，您可以选择添加自己）：
```
yourproject/
├── content
│   └── (pages)
├── output
├── tasks.py
├── Makefile
├── pelicanconf.py       # Main settings file
└── publishconf.py       # Settings to use when ready to publish
```
下一步是开始将内容添加到为您创建的内容文件夹中。

## 创建文章
在创建一些内容之前，您无法运行 Pelican。使用您喜欢的文本编辑器创建您的第一篇文章，内容如下：
```
Title: My super title
Date: 2010-12-03 10:20
Modified: 2010-12-05 19:30
Category: Python
Tags: pelican, publishing
Slug: my-super-post
Authors: Alexis Metaireau, Conan Doyle
Summary: Short version for index and feeds

This is the content of my super blog post.
```
鉴于此示例文章为 Markdown 格式，请将其另存为 username.github.io/content/xxxx.md

官方文档：[Pelican写作文章](https://docs.getpelican.com/en/latest/content.html)

## 生成发布网站
从您的项目根目录中，运行pelican命令以生成您的站点：
```
pelican content
```
您的站点现已在output/目录中生成。（您可能会看到与提要相关的警告，但在本地开发时这是正常的，现在可以忽略。）

虽然该pelican命令是生成站点的规范方式，但可以使用自动化工具来简化生成和发布流程。在pelican-quickstart运行过程中提问是否要自动化站点生成和发布。如果对该问题回答“是”，则会在项目的根目录中生成一个tasks.py和Makefile。这些文件预先填充了从 pelican-quickstart过程中提供的其他答案中收集的某些信息，作为起点，应进行定制以适合的特定需求和使用模式。如果发现这些自动化工具中的一个或两个的实用性有限，则可以随时删除这些文件，并且不会影响规范pelican 命令的使用。

下面介绍POSIX系统中使用make来生成和发布网站，想要在广泛的系统环境中使用，请参考文档，使用由Python编写的[Invoke](https://www.pyinvoke.org/)工具。

如果要使用pelicanconf.py中设置的make生成站点，请运行：
```
make html
```
要生成用于生产的站点，请使用publishconf.py中的设置，运行：
```
make publish
```
如果您希望 Pelican 在每次检测到更改时自动重新生成您的站点（这在本地测试时很方便），请改用以下命令：
```
make regenerate
```
为生成的站点提供服务，以便可以在浏览器中的 http://localhost:8000/进行预览：
```
make serve
```
通常，您需要在两个单独的终端会话中运行make regenerate和make serve，但您可以通过以下方式同时运行两者：
```
make devserver
```
上述命令将同时以再生模式运行 Pelican 并在http://localhost:8000处提供输出。

当您准备好发布您的网站时，您可以通过您在pelican-quickstart调查问卷中选择的方法上传它。对于这个例子，我们将使用 rsync over ssh：
```
make rsync_upload
```

官方文档：[Pelican发布网站](https://docs.getpelican.com/en/latest/publish.html)

## 发布到Github
GitHub pages有两种类型： 项目页面和用户页面。Pelican 站点可以发布为项目页面和用户页面。可以使用pip安装的优秀的ghp-import让这个过程变得非常简单。
```
$ python3 -m pip install ghp-import
```
如上在[pelican-quickstart](#快速配置)中询问是否使用Github Pages上传您的网站和是否是您的个人页面(username.github.io)，我选择了是。当我需要将Pelican 生成的 output 中的内容推送到 GitHub 上的存储库的主分支中时，只需要运行命令：
```
make github
```
因为我想将文章和Pelican配置上传到Github的主分支上，将生成的页面上传到gh-pages分支上，然后让Github Pages托管显示gh-pages分支上的网站。所以我将[Makefile](https://github.com/Jerry-se/Jerry-se.github.io/blob/main/Makefile)文件中的变量"GITHUB_PAGES_BRANCH"的值从"main"修改为"gh-pages"，然后运行`make github`。这样我的所有文章、Pelican和博客页面都放在username.github.io项目中了。

有关项目页面和用户页面的其他设置，请参考：[Pelican发布到Github pages](https://docs.getpelican.com/en/latest/tips.html)

## 设置主题
有一个由社区管理的[Pelican 主题](https://github.com/getpelican/pelican-themes)存储库，供人们共享和使用，现场版本可以在 http://www.pelicanthemes.com 看到。

可以在配置文件pelicanconf.py中设置主题，可以设置为主题文件夹的相对或绝对路径，也可以是默认主题的名称或通过 pelican-themes安装的主题。若要查询已安装的主题，请使用`pelican-themes -l`命令。以下是指定首选主题的示例方法：
```
# Specify name of a built-in theme
THEME = "notmyidea"
# Specify name of a theme installed via the pelican-themes tool
THEME = "chunk"
# Specify a customized theme, via path relative to the settings file
THEME = "themes/mycustomtheme"
# Specify a customized theme, via absolute path
THEME = "/home/myuser/projects/mysite/themes/mycustomtheme"
```
内置notmyidea主题可以很好地利用以下设置。也可以随意在您的主题中使用它们。

若您要创建自制主题或者修改主题，请参考：[Pelican theme](https://docs.getpelican.com/en/latest/themes.html)
