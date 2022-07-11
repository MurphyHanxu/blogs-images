+++
author = "Murphy"
title = "定制个人博客"
date = "2022-04-25"

description = "Guide to customize personal blog"
tags = [
    "GitHub Pages",
    "Hugo",
]

categories = [
    "CS",
]

+++

昨天[使用Hugo部署的网站框架](https://murphyhanxu.github.io/post/setup-hugo/)里部署的还是themes带的样例页面。今天进行个性化定制，包括修改页面上的图片、链接等，并放入自己的博文。

<!--more-->
需要修改的文件主要包括：

- 博文内容文件，在“D:\Hugo\myblog”下content文件夹下。
- 网站配置文件，在“D:\Hugo\myblog”下config文件夹下。
- 发布网站是用到的静态资源（如siderbar使用的头像图片），在主题目录下的静态资源文件夹“D:\Hugo\myblog\themes\hugo-theme-next\static”下。
- 如果自己加入的图片和原图的命名不同，就还需要根据实际修改相关的html文件，在主题目录下的布局文件夹“D:\Hugo\myblog\themes\hugo-theme-next\layouts”下。

本机启动Hugo server，下面的修改基本可以立即生效，方便一边修改一边验证。修改过程记录如下：

1. 在“D:\Hugo\myblog\content”下放入自己写的博文。

   注意：中文和英文的文章是分目录存放的。例如，中文博文放在“D:\Hugo\myblog\content\zh-CN\post”下。

   post目录下原有的样例博文，可以直接删除，也可以通过设置draft属性为true来屏蔽。

   博文中用到的图，我暂时放在post目录下了，md文件中的路径写的github上的URL。

   

2. 逐一打开“D:\Hugo\myblog\config”下的配置文件，根据需要修改。

   我实际只修改了“config.toml”、“languages”、“params.en.toml”和“params.zh-CN.toml”。

   本地调试的时候，可以先改“development”子目录；调试完成需要同步修改“_default”子目录，否则Hugo发布的public站点文件还是错误的。

   

3. 在“D:\Hugo\myblog\themes\hugo-theme-next\static\img”替换自己的图片。

   该目录下是页面上的一些公共图片，例如siderbar的个人信息图片、博文下的QQ二维码、微信收款码等。

   static下还有css、js文件夹，如果不修改页面样式，无需修改。

   

4. （可选）根据需要修改“D:\Hugo\myblog\themes\hugo-theme-next\layouts”下的页面组件html文件。

   主要关注“D:\Hugo\myblog\themes\hugo-theme-next\layouts\partials”下的文件，通常有两种情况需要修改：

   - 替换图片时，如果新文件和旧图片文件的名字不同，则将相关html中的link修改成新文件名。
   - 修改页面显示内容。例如，我没有支付宝，就屏蔽了“D:\Hugo\myblog\themes\hugo-theme-next\layouts\partials\widgets\reward.html”中的支付宝收款二维码相关代码段。

   

5. 将站点发布到GitHub。
   1. 清空public目录，再重新发布生成。

      在站点根目录执行如下cmd命令：

      ```shell
      $ hugo
      ```
   
   1. 进入"D:\Hugo\myblog\public"文件夹，鼠标右键选择“Git Bash Here”，进入Git Bash窗口，执行后续命令。
   
   2. 将文件提交到本机git库中。commit备注可以根据实际需要修改。
   
      ```shell
      $ git add --all
      $ git commit -m "commit my own articles"
      ```
      
   4. 将站点提交到远程：
   
      ```shell
      $ git push -u origin master
      ```
      
      
   
   
   
   
