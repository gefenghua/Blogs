---
layout: post
title:  "在Github Pages上使用Jekyll搭建免费的个人博客"
categories: jekyll update
---

[TOC]

## 一、Git

### 1、Git概述
Git是一个开源的分布式版本控制系统。

[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

### 2、Git安装
如果是在windows环境下，不建议安装和配置Cygwin这样的模拟环境，推荐使用msysgit。

[msysgit下载地址](https://git-for-windows.github.io/)

### 3、Git配置
安装成功之后，还需要做一些配置，设置用户名和邮箱地址：

	$ git config --global user.name USER_NAME
	$ git config --global user.email EMAIL_ADDRESS

## 二、Github

### 1、Github概述
Github是一个免费的远程仓库，用来进行代码托管。同时，还是一个开源协作社区。

[Github官网主页](https://github.com/)

### 2、Github设置
+ 注册Github账号
+ 打开Git Bash，在用户主目录下，使用git配置的邮箱地址创建SSH Key

	```$ ssh-keygen -t rsa -C EMAIL_ADDRESS```
+ 登录GitHub，进入【Settings】页面
+ 选择【SSH and GPG keys】，点击【New SSH Key】
+ 在Key文本框里粘贴创建SSH Key时生成的id_rsa.pub文件的内容，点击【Add SSH key】

### 3、创建远程仓库
+ 在Github主页，点击【New repository】
+ 输入仓库名称Blogs，点击【Create repository】
+ 进入新建的仓库，选择【Settings】
+ 在【GitHub Pages】组中，点击【Launch automatic page generator】
+ 编辑标题和描述，选择主题，点击【Publish page】
+ 生成博客主页http://username.github.io/Blogs

## 三、Github Pages
### 1、Github Pages概述
Github Pages提供了一个免费的网页，用来介绍托管在Github上的项目。

[Github Pages官网主页](https://pages.github.com/)

### 2、Github Pages使用
由于Github Pages提供免费（300M）、稳定的空间，所以很适合用来创建个人博客。虽然可以使用html来编辑博客，但是显然这样做的工作量比较大，并且博客越复杂就越难维护。庆幸的是，可以通过模板引擎快速创建静态博客。鉴于Github Pages官网推荐了Jekyll模板引擎，下面就介绍如何使用Jekyll来创建博客。

## 四、Jekyll模板引擎
### 1、Jekyll概述
Jekyll是一个静态站点生成工具，不需要数据库的支持，通过markdown编写静态文件，生成html页面，并且可以先在本地查看效果，满意之后再提交到Github上，最终在博客主页上看到结果。

[Jekyll官网主页](https://jekyllrb.com/)

### 2、Jekyll安装
由于Jekyll是基于Ruby开发的，所以，要想在本地构建一个Jekyll的测试环境，需要具有Ruby的开发和运行环境。下载与本地环境相符的Ruby和RubyDevKit。

[RubyInstaller](http://rubyinstaller.org/downloads/)

#### 2.1 安装Ruby

很简单的Windows安装程序，不再赘述。

#### 2.2 安装RubyDevKit

Ruby开发包是一个压缩文件，解压并进入解压缩的目录，执行命令：

	$ ruby dk.rb init

生成一个config.yml配置文件，记录了系统安装ruby的位置。

初始化成功之后，开始安装：

	$ ruby dk.rb install

#### 2.3 安装并启动Jekyll

在安装之前，由于众所周知的原因，需要修改一下安装源。

删除默认安装源：

	$ gem sources --remove https://rubygems.org/

添加新的安装源：

	$ gem sources -a http://gems.ruby-china.org/

确认安装源：

	$ gem source -l

开始安装：

	$ gem install jekyll

安装成功之后，创建本地博客站点，站点名与远程仓库名相同：

	$ jekyll new Blogs

进入新建的Blogs目录，启动jekyll服务：

	$ jekyll serve

Jekyll服务的默认端口是4000。服务成功启动后，访问http://localhost:4000就可以看到默认的站点主页。

### 3、工作目录
在新建的Blogs目录下有如下的主要文件和文件夹：

+ _posts文件夹，文章默认的存放位置
+ _site文件夹，默认的转化结果存放位置
+ .gitignore文件，git将忽略该文件中列出的文件或文件夹
+ _config.yml文件，jekyll模板引擎的配置文件，修改之后需要重启服务
+ index.md文件，默认的主页

为了能够更好的使用模板引擎，可以添加如下文件夹（可选）：

+ _includes文件夹，存放在模板中可以引用的文件
+ _layouts文件夹，存放模板文件
+ images文件夹，存放资源文件

### 4、关联仓库
打开Git Bash，进入工作目录Blogs，初始化本地仓库：

	$ git init

创建一个没有父节点的分支gh-pages，并切换到这个分支上：

	$ git checkout --orphan gh-pages

为远程仓库在本地添加一个origin引用：

	$ git remote add origin https://github.com/username/Blogs.git

将远程仓库中的文件拖到本地：

	$ git pull origin gh-pages

删除远程仓库中的文件：

	$ git rm <filename>

### 5、编辑文章

在工作目录下的_posts文件夹中，创建并编辑文章，文件名必须是YYYY-MM-DD-title格式。

文章可以包含如下的头信息：

	---
	layout: post
	title:  "Welcome to Jekyll!"
	categories: jekyll update
	---

其中，layout表示使用_layouts目录下的post布局文件。title表示文章的标题。categories是文章生成的html文件存放的目录，多级目录用空格分隔。

文章编辑完成并保存之后，刷新站点主页查看编辑之后的效果。

### 6、上传本地文件

在经过本地检验，确认没有问题之后，就可以将本地创建的文章上传到远程仓库。

将当前的改动添加到暂存区：

	$ git add .

将暂存区的内容提交到本地仓库，并添加本次提交的备注：

	$ git commit -m "post blogs"

将本地仓库的内容推送到远程仓库的gh-pages分支：

	$ git push origin gh-pages

这样，再次访问博客主页http://username.github.io/Blogs，就会看到新生成的内容了。

