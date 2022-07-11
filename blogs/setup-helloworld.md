+++
author = "Murphy"
title = "创建第一个GitHub Page"
date = "2021-11-23"
description = "Guide to setup hello world in GitHub Pages"
tags = [
    "GitHub Pages",
]

categories = [
    "CS",
]

+++

如果想把自己日常的学习心得记录下来，可以创建一个个人博客网站。经过几天的研究比较，发现使用GitHub提供的Pages功能，可以满足基本要求。

参照网上的多个总结，今天完成了我自己个人博客首页的创建（虽然只是一个hello world）。

建议先花半天时间，看看[廖雪峰大神的git总结](https://www.liaoxuefeng.com/wiki/896043488029600)和[GitHub Pages官方文档](https://pages.github.com/)，系统了解一下git、GitHub等的关系，以及GitHub Pages的创建方法，搞清楚背景知识，实际动手会更顺利。
<!--more-->
我的创建过程还比较顺利，基本过程记录如下：

1. 访问[GitHub官网](https://github.com/)，注册账号并登录。

   注册仅需要个人邮箱。

   

2. 在Github上创建仓库。

   ![单击创建仓库按钮](//jasmine617.github.io/post/images/20211123001.png)

   

   这个仓库是计划给GitHub Pages用的，所以注意仓库名要和用户名一致。

   例如：MurphyHanxu.github.io

   ![创建仓库](//jasmine617.github.io/post/images/20211123002.png)

   

3. 在本机安装git。

   参考：[廖雪峰博客-安装git](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

   注意：安装完git后，一定要配置用户名和邮箱，否则后续commit会报错。

   在Git Bash中执行如下命令，name和email请根据实际情况替换：

   ```shell
   $ git config --global user.name "Murphy"
   
   $ git config --global user.email "123456@qq.com"
   ```

   

4. 在本机生成SSH密钥，并配置到GitHub上。

   配置密钥，是为了GitHub确认本机上传文件合法；如果不配置，后续将无法将文件push到GitHub上。

   参考：[廖雪峰博客-对接远程仓库](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

   1. 检查用户主目录（Windows环境为"C:\Users\yourusername"目录下是否存在".ssh"文件夹。如果不存在，需要生成公钥文件。

      使用Git Bash执行，注意替换email：

      ```shell
      $ ssh-keygen -t rsa -C "123456@qq.com"
      ```

   
   2. 再将公钥内容配置到GitHub中。
   
      ![创建SSH密钥](//jasmine617.github.io/post/images/20211123003.png)
      

      ![配置密钥](//jasmine617.github.io/post/images/20211123004.png)
   
   
   
5. 将GitHub上的仓库克隆到本地。

   1. 先在GitHub上获取仓库地址：
   
      ![获取仓库URL](//jasmine617.github.io/post/images/20211123005.png)

      

   2. 在本机创建一个文件夹，用于保存从公仓获取的文件，例如"D:\myplog"。打开该目录后鼠标右键选择“Git Bash Here”，进入Git Bash窗口，执行后续命令。
   
      推荐使用SSH协议进行clone（请用从GitHub中拷贝的git仓库地址替换下例中的地址）：
   
      ```shell
      $ git clone git@github.com:MurphyHanxu/MurphyHanxu.github.io.git
      Cloning into 'MurphyHanxu.github.io'...
      warning: You appear to have cloned an empty repository.
      ```
      因为步骤2中创建的是一个空仓库，没有任何文件，上例的系统响应中有一个warning，无需理会。
      如果使用HTTP协议，则为：
   
      ```shell
      $ git clone https://github.com/Jasmine617/jasmine617.github.io.git
      ```




6. 创建主页文件index.html，并提交到本机git库。

   1. 检查上一步中的"D:\myplog"文件夹，可以看到克隆成功后新增了一个文件夹"MurphyHanxu.github.io"。

   2. 在新增的"MurphyHanxu.github.io"文件夹下创建一个网页文件index.html，按html语法随便写一点内容。html样例如下：

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>my first blog</title>
      </head>
      <body>
      <p>hello world</p>
      </body>
      </html>
      ```
   
   3. 在Git Bash中进入git库目录。将文件提交到本机git库中。commit备注可以根据实际需要修改。

      ```shell
      $ cd MurphyHanxu.github.io
      $ git add --all
      $ git commit -m "test commit index.html"
      ```
   
   
   
7. 将index.html推送到GitHub公仓。

   1. 检查本机git库是否已经提交成功。下例中的系统响应代表本地git库当前的分支名是main，并且本地已经没有需要commit的文件了（即新创建的index.html已经成功提交到本机git版本库）。

      ```shell
      $ git status
      On branch main
      Your branch is up to date with 'origin/main'.
      
      nothing to commit, working tree clean
      ```
   

   2. 使用如下命令将新增的index.html提交到GitHub公仓，其中main是上个命令中查询到的本机git分支名：

      ```shell
      $ git push origin main
      Enumerating objects: 3, done.
      Counting objects: 100% (3/3), done.
      Delta compression using up to 8 threads
      Compressing objects: 100% (2/2), done.
      Writing objects: 100% (3/3), 338 bytes | 338.00 KiB/s, done.
      Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
      To https://github.com/MurphyHanxu/MurphyHanxu.github.io.git
       * [new branch]      main -> main
      ```

   

8. 检查GitHub Page。

   1. 首先在GitHub仓库中检查index.html文件是否已经存在。

      ![检查上传结果](//jasmine617.github.io/post/images/20211123006.png	"检查上传结果")
   

   2. 再检查Pages设置是否正确。本例中index.html是上传仓库根目录下的，所以这里设置为root并保存。

      ![页面设置](//jasmine617.github.io/post/images/20211123007.png	"页面设置")
   
   
   3. 打开GitHub Pages页面，如图显示则说明整个配置成功。

      ![创建结果](//jasmine617.github.io/post/images/20211123008.png	"创建结果")

   

   
