+++

author = "Murphy"
title = "使用Hugo创建个人博客框架"
date = "2022-04-24"

description = "Guide to setup blog by hugo"
tags = [
    "GitHub Pages",
    "Hugo",
]

categories = [
    "CS",
]

+++

&#160; &#160; &#160; &#160;在已经跑通了[使用GitHub Pages建了自己的第一个hello world页面](https://murphyhanxu.github.io/post/setup-helloworld/)的前提下，今天尝试使用Hugo来建一个完整的静态网站框架。

&#160; &#160; &#160; &#160;这里先介绍如何安装Hugo，使用Hugo创建站点，并引入一个主题。按照本文操作完，站点的页面还是themes带的样例页面。我将在下一篇文章中总结如何加入自己的博文，以及修改网站的其他个人信息。

<!--more-->
# 前言

&#160; &#160; &#160; &#160;为什么我们能够使用不同的电脑（客户端）成功地访问同一个网站？  
&#160; &#160; &#160; &#160;我们在浏览器输入网页地址，此地址称为统一资源定位器（URL），并且可以使用其自己的个人URL访问每个页面。浏览器会先在其缓存中查找请求的URL，如果不存在，它会请求操作系统地DNS服务器找到所需的IP地址。DNS服务器负责名称解析。可以在操作系统和路由器中配置要请求的DNS服务器。由于请求域名系统需要一些时间，因此已访问过的站点的IP地址通常存储在操作系统或浏览器的DNS缓存中。  
&#160; &#160; &#160; &#160;接着，路由器作为互联网和家庭网络之间的接口。它从互联网请求数据并将其分发到台式计算机，笔记本电脑和平板电脑等网络设备。由于家庭网络中的设备使用本地IP地址相互通信，同时向外共享路由器的公共IP地址，因此需要路由器作为链路。然后，网络地址通过称为网络地址转换（NAT）的过程进行转换。  
&#160; &#160; &#160; &#160;当识别出所选网页的IP地址时，浏览器从适当的Web服务器请求该页面的相关数据。此请求通过HTTP以数据包的形式发生，该数据包包含Web服务器为传递网页数据所需的所有信息。浏览器传达所选网页的IP地址，并提供有关操作系统本身以及应在其上显示网页的设备的信息。路由器将自己的公共IP地址添加为发送方，并将数据包转发到公共Internet。Web服务器处理该信息并发送一个HTTP状态码。如果请求成功，服务器发送状态码200和数据包，包含该页面所需的所有信息。如果服务器无法在请求的地址找到网页，则会发送状态码404。  
&#160; &#160; &#160; &#160;最后，传入数据包从路由器转发到正在访问网页的计算机。然后，Web浏览器承担分析数据包的任务。网页通常包含HTML、Css和Java文件。这样我们想要访问的网页就呈现在我们眼前了。
![setup-hugo-1](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/setup-hugo-1.png)  
现在我们想要自己搭建一个网站需要什么？  
&#160; &#160; &#160; &#160;需要自己编写网页的HTML、Css、Java文件和一台云服务器以及提供上云的软件和管理云服务器的软件等等，这种搭建网站的方式详情见[教程][2]。  
&#160; &#160; &#160; &#160;搭建这样的动态网站方式也有缺点，就是搭建云上服务太耗时，而且上传的网站页面生成很慢，也还有维护服务器、数据库的烦恼。以Wordpress作为博客的载体太笨重，对markdown格式的支持不是很好，且打开速度极慢。  

&#160; &#160; &#160; &#160;今天介绍一种快速简便的搭建静态网站的方式，不需要租云服务器等，只需使用到Hugo和GitHub pages。其中Hugo提供静态网站的生成，包括主题、样式等。GitHub pages提供网站的托管服务。


Hugo是什么？  
&#160; &#160; &#160; &#160;Hugo 是一个快速且现代的静态网站生成器，采用 Go 编程语言开发，Hugo 的设计目标是让创建网站重新变得有趣。  
&#160; &#160; &#160; &#160;Hugo 是一个通用的网站框架。从技术上讲，Hugo是一个静态站点生成器。与动态构建页面的系统不同，Hugo在创建或更新内容时构建页面。由于网站的浏览频率远高于编辑频率，因此Hugo旨在为您的网站最终用户提供最佳的浏览体验，并为网站作者提供理想的写作体验。

Hugo能干什么？  
&#160; &#160; &#160; &#160;从技术的角度来说，Hugo从源目录读取文件和模板，并将其作为输入来创建完整的网站。

谁应该使用 Hugo ？  
&#160; &#160; &#160; &#160;Hugo 适合那些喜欢用文本编辑器而不是浏览器来写作的人。  
&#160; &#160; &#160; &#160;Hugo 是为那些想手工编写自己的网站代码而不必担心设置复杂的运行时、依赖项和数据库的人设计的。  
&#160; &#160; &#160; &#160;Hugo 能够用于构建博客、公司网站、投资组合网站、文档、单一的登陆页面或有数千页面的网站。

Hugo的特点  
&#160; &#160; &#160; &#160;相比于使用Wordpress搭建网站，Hugo拥有惊人的速度，强大的内容管理和模板语言，非常适合各种静态网站。它的构建时间极快（每页<1ms）。  
&#160; &#160; &#160; &#160;使用Hugo搭建网站的HTML文件都呈现在你自己的计算机上。在将文件复制到承载HTTP服务器的计算机之前，因为HTML文件不是动态生成的，你可以在本地查看这些万件。

GitHub是什么？  
&#160; &#160; &#160; &#160;GitHub是一个面向开源及私有软件项目的托管平台，因为只支持Git作为唯一的版本库格式进行托管，故名GitHub。GitHub于2008年4月10日正式上线，除了Git代码仓库托管及基本的Web管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目Ruby on Rails、jQuery、python等。

GitHub功能特点  
&#160; &#160; &#160; &#160;GitHub可以托管各种git库，并提供一个web界面，但它与外国的SourceForge、Google Code或中国的coding的服务不同，GitHub的独特卖点在于从另外一个项目进行分支的简易性。为一个项目贡献代码非常简单：首先点击项目站点的“fork”的按钮，然后将代码检出并将修改加入到刚才分出的代码库中，最后通过内建的“pull request”机制向项目负责人申请代码合并。已经有人将GitHub称为代码玩家的MySpace。  

# 搭建步骤

#### 1. 打开[Hugo下载页面](https://github.com/gohugoio/hugo/releases)，获取与本机环境相匹配的安装包。
例如，我下载的是：“hugo_0.89.4_Windows-64bit.zip”。  

#### 2. 安装Hugo  
&#160; &#160; &#160; &#160;1. 在本机建一个Hugo文件夹，为方便后续管理，在其下再建一个bin目录。
例如，我建的目录是“D:\works\Hugo\bin”  

&#160; &#160; &#160; &#160;2. 将安装包拷贝到bin目录下并解压。
解压成功后，可以在bin目录下看到“hugo.exe”。  
&#160; &#160; &#160; &#160;3. 配置系统环境变量，为变量Path添加“hugo.exe”所在目录。
对于我来说，添加的是“D:\works\Hugo\bin”。
![setup-hugo-2](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/setup-hugo-2.png)  
&#160; &#160; &#160; &#160;4. 执行如下命令，如果能显示版本号，则说明安装成功。

 ```shell
$ hugo version
hugo v0.89.4-AB01BA6E windows/amd64 BuildDate=2021-11-17T08:24:09Z VendorInfo=gohugoio
 ```
#### 3. 建立站点。
&#160; &#160; &#160; &#160;1. 在Windows cmd窗口下进入Hugo文件夹。
（注意这里是“D:\works\Hugo”，不是“D:\works\Hugo\bin”。)

&#160; &#160; &#160; &#160;2. 假设要创建的站点目录为“myblog”，则执行如下命令：

```shell
>$ hugo new site myblog
Congratulations! Your new Hugo site is created in D:\works\Hugo\myblog.  

Just a few more steps and you are ready to go:  

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ orcreate your own with the "hugo new theme <THEMENAME>" command.  

2. Perhaps you want to add some content. You can add single files
with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".  

3. Start the built-in live server via "hugo server".
Visit https://gohugo.io/ for quickstart guide and full documentation.
```

&#160; &#160; &#160; &#160;3. 加入主题并预览，下载themes。
访问[Hugo主题页面](https://themes.gohugo.io/)，找一个自己喜欢的主题下载到上一步创建的“themes”文件夹下，并解压。  
我这次下载的是[NexT主题](https://themes.gohugo.io/themes/hugo-theme-next/)，它支持多设备显示自适应，同时支持评论、转发等博客需要的功能。  
![setup-hugo-3](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/setup-hugo-3.png)
![setup-hugo-4](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/setup-hugo-4.png)
下载完成后，需要按照themes的说明做一些配置。对于我下载的主题，我按照要求把hugo-theme-next(-main)目录下的两个子目录config和content到站点根目录（即“D:\works\Hugo\myblog”)下。

&#160; &#160; &#160; &#160;4. 在本地启动站点。在站点根目录执行如下cmd命令：
```shell
>$ hugo server
Start building sites …
hugo v0.89.4-AB01BA6E windows/amd64 BuildDate=2021-11-17T08:24:09Z VendorInfo=gohugoio
                 | ZH-CN | EN
-------------------+-------+-----
Pages            |    28 | 34
Paginator pages  |     0 |  0
Non-page files   |     8 |  0
Static files     |    25 | 25
Processed images |     0 |  0
Aliases          |     9 | 12
Sitemaps         |     2 |  1
Cleaned          |     0 |  0

Built in 42196 ms
Watching for changes inD:\Hugo\myblog\{archetypes,content,data,layouts,static,themes}
Watching for config changes in D:\Hugo\myblog\config.toml, D:\Hugo\myblog\config\_default, D:\Hugo\myblog\config\development
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```


&#160; &#160; &#160; &#160;5. hugo服务启动完成时，将出现与上面相类似的响应。

&#160; &#160; &#160; &#160;&#160;6. 在浏览器中访问“http://localhost:1313/”，检查本地站点效果。

&#160; &#160; &#160; &#160;7. 在cmd窗口中按Ctrl+C，停止Hugo服务。

#### 4. 发布需要提交到到GitHub的站点文件。  
&#160; &#160; &#160; &#160;1. 将站点的BaseURL配置为GitHub Pages主页地址。待修改的文件包括“D:\works\Hugo\myblog\config.toml”、“D:\works\Hugo\myblog\config\development\config.toml”和“D:\works\Hugo\myblog\config\_default\config.toml”，将BaseURL的值修改为“https://MurphyHanxu.github.io/”。  

&#160; &#160; &#160; &#160;2. 发布站点。
在站点根目录执行如下cmd命令：  

```shell
$ hugo
hugo v0.89.4-AB01BA6E windows/amd64 BuildDate=2021-11-17T08:24:09Z VendorInfo=gohugoio
                   | ZH-CN | EN
-------------------+-------+-----
  Pages            |    28 | 34
  Paginator pages  |     0 |  0
  Non-page files   |     8 |  0
  Static files     |    25 | 25
  Processed images |     0 |  0
  Aliases          |     9 | 12
  Sitemaps         |     2 |  1
  Cleaned          |     0 |  0
  
Total in 42190 ms
```

发布成功，可以在“D:\works\Hugo\myblog\”下看到public文件夹。将这个文件夹发布到GitHub即可。

#### 5. 将站点发布到GitHub,详细参考[廖雪峰git教程][5]。
&#160; &#160; &#160; &#160;1. 进入上一步创建的"D:\works\Hugo\myblog\public"文件夹，鼠标右键选择“Git Bash Here”，进入Git Bash窗口，执行后续命令。  

&#160; &#160; &#160; &#160;2. 初始化git库：
```shell
$ git init
Reinitialized existing Git repository in D:/Hugo/myblog/public/.git/
```

&#160; &#160; &#160; &#160;3. 将文件提交到本机git库中。commit备注可以根据实际需要修改。
```shell
$ git add --all
$ git commit -m "first commit of Hugo site"
```

&#160; &#160; &#160; &#160;4.  检查本机git库是否已经提交成功。下例中的系统响应代表本地git库当前的分支名是master，并且本地已经没有需要commit的文件了。
```shell
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits)

nothing to commit, working tree clean
```

&#160; &#160; &#160; &#160;5. 使用如下命令，与远程GitHub公仓建立连接，并将站点提交到远程：
请根据自己的情况，修改GitHub仓库地址和本地分支名。
```shell
$ git remote add origin https://github.com/MurphyHanxu/MurphyHanxu.github.io.git
$ git push -u origin master
Enumerating objects: 206, done.
Counting objects: 100% (206/206), done.
Delta compression using up to 8 threads
Compressing objects: 100% (64/64), done.
Writing objects: 100% (205/205), 194.83 KiB | 32.47 MiB/s, done.
Total 205 (delta 90), reused 190 (delta 83), pack-reused 0
remote: Resolving deltas: 100% (90/90), done.
To https://github.com/Jasmine617/jasmine617.github.io.git
   5e8f0dc..8850953  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

![setup-hugo-5](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/setup-hugo-5.png)
如果git操作不熟练，这里可能会出现关联远程仓库失败或者推送失败的情况。请根据错误提示，搜索解决方法。
到此就完成了网页的上传，托管到了GitHub上。再根据上一篇blog的方法查看自己的网站页面。



[2]: https://www.bilibili.com/video/BV1rU4y1J785/?spm_id_from=pageDriver&vd_source=b3b5aca8a2675b381887886fba026f67
[5]: https://www.liaoxuefeng.com/wiki/896043488029600