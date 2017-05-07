---
layout:     post
title:      "Jekyll/GithubPages-Blog 搭建"
subtitle:   "个人Blog搭建过程，纪念下而已"
date:       2017-05-01 
author:     "Will"
header-img: "img/jekyll/jekyll.png"
catalog: true
tags:
    - Jekyll
    - Blog
---

> “walk beside you ”


## 前言 

　　作为一个程序员，很早就有搭建个人Blog的念头，但一直没有付出行动。所以自己工作学习中的整理的知识笔记存放的很凌乱。最终决定利用劳动节的时间把Blog搭建起来，好好整理下笔记。

---

## 正文

　　本次搭建Blog使用的是[GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/)

优点：

  * GitHub 程序员应该都知道的平台
  * Jekyll 模板很多而且都很简洁漂亮
  * 本地编写Markdown博文使用Git上传，简单方便
  * 重点是一分钱不要

这里只介绍windows 下的安装方式

#### 安装 Ruby

1. 到 [Ruby](http://rubyinstaller.org/downloads/) 官网上下载安装包。我这里使用的是 rubyinstaller-2.2.6-x64 版本的安装包
2. 安装时需要注意：
* 安装路径不可以有空格
* 需要勾选**Add Ruby executables to your PATH**,当然了也可以自己手动配置环境变量![Ruby](/img/jekyll/ruby2.2.6.png)
* 安装完成，在 cmd 下 ```  ruby -v ``` ,显示安装版本则按照成功

#### 安装 DevKit

1. 继续在 [Ruby](http://rubyinstaller.org/downloads/) 官网上下载DevKit 安装包，我这里使用的是 DevKit-mingw64-32-4.7.2-20130224-1151-sfx 版本
2. 解压下载好的安装包，在cmd 下进入 解压好的devkit 目录下 执行 ``` ruby dk.rb init ```  然后继续输入  ``` ruby dk.rb install ```

#### 安装 Jekyll

1. 先安装Jekyll依赖包bundler，在cmd 下输入  ``` gem install bundler ```
2. 安装Jekyll，在cmd 下输入 ``` gem install jekyll ```进行安装
3. 检查是否安装成功，在cmd 下输入 ``` jekyll -v ```
4. 有的 Jekyll 模板可以需要不同的插件需要单独安装。如分页 ** jekyll-paginate ** ，运行 ``` gem install jekyll-paginate ``` 进行安装

#### 新建 Jekyll Blog

1. 新建 blog ，在cmd 下输入 ``` jekyll new blog ```，之后会生产一个 blog文件夹，到时博文就放到**_posts** 文件夹下![Blog](/img/jekyll/blog.png)
2. 也可以下载一下 [Jekyll主题](http://jekyllthemes.org/)放到**blog**文件夹里
3. 启动Jekyll 在 cmd 下输入 ``` jekyll serve ``` ， 启动成功后默认访问路径是``` http://127.0.0.1:4000/ ``` 

#### 在 GitHub 创建Blog 项目

1. 在 GitHub 上创建一个新的**Repository**，这里**Repository name**有一定的规则：比如帐号是blog，那么此仓库名称为blog.github.io 。这样访问博客时，直接访问 blog.github.io即可
2. 创建成功后需要在新的仓库**Setting** 里 **GitHub Pages**下的**Source** 选择 **master branch**保存后会生成对应的URL。访问此URL即可访问创建好的blog
3. 使用Git把本地写好的 **Jekyll Blog项目** 上传到 GitHub上创建好的仓库 ，访问刚才生成的URL即可看到自己的博客网站
4. 如果想使用自己的域名，在域名解析里添加一条 **CNAME** ，还要在仓库里的 **CNAME**文件添加自己的域名，比如： ```blog.com ``` 。这样访问自己的域名即可看到自己的Blog了

---

## 后记

　　本博客使用的模板是从这里 [Huxpro/huxblog-boilerplate](https://github.com/Huxpro/huxblog-boilerplate.git) 下载的，自己做了一点修改。如果想使用我修改后的模板可以在[bigfat-will](https://github.com/bigfat-will/bigfat-will.github.io.git) 这里下载。

　　使用**Jekyll**创建blog，写博文使用的是[Markdown](http://www.appinn.com/markdown/)标记语言，Markdown的语法还是比较简单的，很容易掌握。


