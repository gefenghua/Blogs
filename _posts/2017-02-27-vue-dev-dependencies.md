---
layout: post
title:  "Vue常用开发依赖"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/vue_icon.png)

## 一、概述

Vue框架是一个渐进式的框架，在需要某个依赖的时候才进行安装，并不需要提前安装好所有的依赖。

## 二、加载器

xxx-loader是各种文件的加载器。通过加载器，webpack打包时才能够识别各种类型的文件，并将其转换成js文件。

使用vue-cli脚手架会自动安装好很多加载器：

* babel-loader：用于转换jsx和ES6的语法
* css-loader：用于转换样式
* eslint-loader：用于检查JavaScript的语法错误
* file-loader：用于文件的移动
* url-loader：用于处理图片的引用关系，大图片可以直接引用，小图片则会转换成base64文件
* vue-loader：用于转换*.vue文件
* vue-style-loader：用于转换vue的样式

如果还想安装其他加载器，可手动添加，如css预处理器stylus的加载器：

    npm i stylus stylus-loader --save-dev

## 三、路由

使用vue-cli脚手架时可以选择是否安装vue-router，如果没有选择，可手动添加：

	npm i vue-router --save

## 四、网络请求

Vue 1.x，使用vue-resource处理网络请求：

    npm i vue-resource --save

Vue 2.x，使用axios处理网络请求：

    npm i axios --save

## 五、状态管理

vuex负责数据状态管理：

    npm i vuex --save

## 六、UI框架

[element-ui](http://element.eleme.io/#/zh-CN/component/installation)是基于Vue2的组件库，利用一系列的资源，可以用来快速生成网站：

	npm i element-ui --save

## 七、后端框架

[koa](http://koajs.com/)是Express的升级版，koa1支持ES6，而koa2则支持ES7：

    npm i koa koa-router@5.4 koa-logger koa-json koa-bodyparser koa-jwt --save

## 八、ORM框架

[sequelize](http://docs.sequelizejs.com/en/latest/)是一个常用的ORM框架：

	npm i sequelize-auto -g
	npm i sequelize -g
    npm i mysql -g

## 九、其他

安装依赖或框架之后，还需要在main.js文件中引入才能使用，如：

    import VueRouter from 'vue-router'
	import Element from 'element-ui'
    import 'element-ui/lib/theme-default/index.css'

    Vue.use(VueRouter)
	Vue.use(Element)

注意：Element的样式文件需要单独引入。
