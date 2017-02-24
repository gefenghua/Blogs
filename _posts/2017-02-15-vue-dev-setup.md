---
layout: post
title:  "Vue开发环境搭建"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/vue_icon.png)

### 1、安装Node.js
由于Vue使用组件化的模式来组织工程，所以需要使用打包工具（如：Webpack）对文件进行打包。而Webpack打包工具以及其他依赖包，运行在Node环境中，所以首先需要安装Node.js。

### 2、安装webpack
安装好Node.js之后，通过npm安装Webpack打包工具。首先创建一个工程目录（如：vue-demo），然后进入到工程目录。

初始化目录，并生成一个package.json文件：

	npm init

安装webpack依赖：

	npm i webpack --save-dev

### 3、安装vue-cli
vue-cli是vue的脚手架工具，用来构建工程。可以采用全局安装的方式进行安装。

	npm i vue-cli -g

### 4、创建工程
使用脚手架创建一个工程：

	vue init webpack

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/01-vue-init-webpack.png)

根据提示，继续安装依赖：

	npm i

安装完毕，启动服务：

	npm run dev

该命令会查找package.json的scripts对象，并执行node build/dev-server.js。在dev-server.js文件里，配置了webpack，通过webpack编译项目文件，并运行服务器。

不出意外的话，会自动打开一个浏览器窗口，显示vue运行页面。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/02-npm-run.png)

### 5、目录结构
创建工程生成的目录结构如下：

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/03-project-structure.png)

* build目录，webpack打包编译配置文件
* config目录，工程的环境配置
* node_modules目录，工程的依赖模块
* src目录，工程的程序文件
* components目录，工程的组件文件
* static目录，工程的资源文件
* test目录，工程的测试文件
* App.vue文件，工程的根组件，所有的子组件都在这里被引用
* main.js文件，工程的入口逻辑，使用webpack打包之后被注入到入口文件index.html中
* index.html，工程的入口文件，在这里引用根组件App.vue
* package.json，工程的配置文件

**入口文件index.html**

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/04-index-html.png)

声明了一个id为`app`的\<div>元素，用来挂载Vue实例。

**入口逻辑main.js**

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/05-main-js.png)

导入所需依赖，然后创建一个Vue实例。`el`指向该实例要挂载的元素id。`template`定义使用的模板。`components`指向使用的组件。

**根组件文件App.vue**

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/06-app-vue.png)

**子组件文件Hello.vue**

通常一个组件包含三个部分：模板、脚本、样式。

模板：

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/07-hello-vue-template.png)

脚本：

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/08-hello-vue-script.png)

样式：

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/09-hello-vue-style.png)

**配置文件package.json**

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-02-15-vue-dev-setup/10-package-json.png)
