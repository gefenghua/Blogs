---
layout: post
title:  "Webpack快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/webpack_icon.jpg)

## 一、概述
> webpack is a tool to build JavaScript modules in your application. webpack simplifies your workflow by quickly constructing a dependency graph of your application and bundling them in the right order. webpack can be configured to customise optimisations to your code, to split vendor/css/js code for production, run a development server that hot-reloads your code without page refresh and many such cool features.

webpack是一款前端资源模块化管理和打包的工具。它可以将模块按照依赖和规则进行打包，还可以将按需加载的模块进行代码分离，实现异步加载。通过加载器，任何形式的资源都可以被看作成模块。

[Github地址](http://webpack.github.io/)

从一个主文件开始，依次寻找项目的所有依赖文件，通过加载器进行处理，最后打包成浏览器能够识别的JavaScript文件。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-25-how-to-use-webpack/what-is-webpack.png)

## 二、特点
webpack和其他模块化工具相比，具有如下特点：

* 代码分离

	webpack有两种组织模块依赖的方式：同步和异步。异步依赖会将代码分离成不同的区块，每一个区块都作为一个文件被打包。

* 加载器

	webpack自身只能处理原生的JavaScript模块，通过加载器，可以将不同类型的资源转换成JavaScript模块。换句话说，任何资源都可以被webpack当成模块来处理。

* 智能解析

	webpack通过智能解析器，几乎可以处理任何的第三方库。并且在加载依赖的时候，允许使用动态表达式进行加载。

* 插件

	webpack还提供了插件系统，大多数功能都是基于这个插件系统运行的。可以通过开发和使用webpack插件，满足不同的需求。

* 运行效率

	webpack通过异步I/O和多级缓存，大大提高了运行效率，能够快速地实现增量编译。

## 三、概念
为了能够更好地理解和使用webpack，首先需要了解webpack的四大核心概念：

* 入口entry

	webpack为应用程序的所有依赖创建了一个图表，这个图表的起始点被称为入口点。入口点会告诉webpack从哪里开始，并且根据依赖图表，清楚哪些资源会被打包。`entry`属性用来定义入口点。

* 输出output

	一旦将资源打包在一起之后，需要告诉webpack将要把应用程序打包到何处。`output`属性用来描述打包文件的位置。

* 加载器loaders

	当非JavaScript资源被添加到依赖图表时，通过加载器将这些文件转换成模块。`test`属性用来定义要转换的文件类型。`use`属性指定加载器的类型。

* 插件plugins

	相对于加载器只能基于每个文件进行转换，插件通常用来针对复杂应用执行操作和自定义功能。要想使用插件，首先需要使用require()方法引入插件模块，然后在plugins数组中，创建一个该插件的实例。

## 四、安装和配置

### 1、安装Node.js
由于webpack需要使用npm进行安装，所以应该首先安装Node.js。

### 2、安装webpack
通常不建议使用`$ npm install webpack -g`命令进行全局安装。而是将webpack安装到项目的依赖中，这样，就可以使用项目本地版本的webpack。

进入到项目目录，创建一个package.json文件：

	npm init

安装webpack依赖：

	npm install webpack --save-dev

也可以指定版本进行安装：

	npm install webpack@<version> --save-dev

如果需要使用webpack开发工具，需要单独安装：

	$ npm install webpack-dev-server --save-dev

### 3、运行
如果采用非全局安装，执行如下命令进行打包：

	node_modules/.bin/webpack app/index.js public/bundle.js

其中，index.js是入口文件，bundle.js是目标文件。

### 4、配置
除了使用命令行运行webpack的方式，更好的方法是使用配置文件`webpack.config.js`。它是一个node.js模块，返回一个json格式的配置信息对象。也可以通过`--config`选项指定配置文件。

	module.exports = {
	  entry: __dirname + "/app/index.js",
	  output: {
	    path: __dirname + "/public",
	    filename: "bundle.js"
	  }
	}

其中，`entry`属性指向入口文件，`output`属性指定打包后文件存放的位置，`__dirname`指向当前执行脚本所在的目录。

此时，执行`node_modules/.bin/webpack`命令，会自动参考配置文件中的配置选项打包项目。

还有一种更快捷的运行方式。在package.json中设置npm的脚本：

	"scripts": {
	    "start": "webpack" // 将npm的start命令指向webpack命令
	},

这样，直接执行`npm start`命令，就会自动运行webpack进行打包。

## 五、用法
webpack很多强大的功能，都是通过一系列可供配置选项完成的。这里主要介绍一些常用的选项。

### 1、开发工具devtool
开发工具主要用来方便调试，提高开发效率。通过打包之后的文件，不太容易找到出现错误的地方对应的源码位置。这时，可以使用Source Map来解决这个问题。

	devtool: "source-map"

`devtool`属性有四种不同的选项值：

* source-map，在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有良好的source map，但会降低打包文件的构建速度。
* cheap-module-source-map，在一个单独的文件中生成一个不带列映射的map，能够相应地提高构建速度，但不能对应到具体的列，会对调试造成不便。
* eval-source-map，使用eval打包源文件模块，在同一个文件中生成干净且完整的source map，不会影响构建速度，但输出的js文件存在性能和安全隐患。建议在开发阶段使用，一定不要在生产阶段使用此选项。
* cheap-module-eval-source-map，能够以最快的速度生成source map，生成的source map和打包后的js文件同行显示，没有列映射。同样建议只在开发阶段使用。

Tips：在开发阶段使用eval-source-map选项。

### 2、本地服务器devserver
开发过程中，希望浏览器能够监测到代码的修改，并自动刷新，显示修改之后的结果。这时，可以配置devserver构建本地服务器来完成这些功能。

	devServer: {
	    contentBase: "./public",
	    inline: true,
	    colors: true,
	    historyApiFallback: true
	}

因为本地服务器是一个单独组件，所以在配置之前需要单独安装：

	npm install webpack-dev-server --save-dev

`devserver`属性有如下配置选项：

* contentBase，默认情况下，为根文件夹提供本地服务器。如果想为其他目录提供本地服务器，可以在此选项设置目录。
* port，设置监听端口，默认为8080。
* inline，设置为true时，当源文件发生改变会自动刷新页面。
* colors，设置为true时，终端输出的结果为彩色的。
* historyApiFallback，设置为true时，所有的跳转都指向index.html，主要用于单页面应用。

### 3、加载器loaders
Loaders是webpack最强大的功能之一。通过使用不同的loader，webpack调用外部的脚本或工具，可以对各种格式的文件进行处理。

Loaders需要单独安装：

	npm install json-loader --save-dev

并且需要在`modules`属性下进行配置：

	module: {
	    loaders: [
	      {
	        test: /\.json$/,
	        loader: "json"
	      }
	    ]
	}

`loaders`属性有如下配置选项：

* test，匹配loaders所处理文件的扩展名的正则表达式（必须）。
* loader，loader的名称（必须）。
* include，添加必须处理的文件夹或文件（可选）。
* exclude，屏蔽不需要处理的文件夹或文件（可选）。
* query，为loaders提供额外的设置选项（可选）。

webpack提供了很多有用的loader，如css-loader和style-loader，用来处理样式表。css-loader能够使用类似@import和url()的方法实现require()的功能，而style-loader则将所有计算后的样式加到页面中。

安装：

	npm install style-loader css-loader --save-dev

配置：

	module: {
	    loaders: [
	      {
	        test: /\.css$/,
	        loader: "style!css"
	      }
	    ]
	}

其中，“!"表示同一文件能够使用不同类型的loader。

Sass或Less之类的预处理器是对原生CSS的拓展，不同类型的预处理器也对应不同的loader。webpack还提供一个CSS处理平台PostCSS，可以使CSS能够实现更多功能。使用PostCSS可以为CSS代码自动添加适应不同浏览器的CSS前缀。

安装：

	npm install postcss-loader autoprefixer --save-dev

配置：

	postcss: [
	    require('autoprefixer')
	]

Babel是一个编译JavaScript的平台，它支持下一代的JavaScript标准（如：ES6），以及基于JavaScript进行拓展的语言（如：JSX）。

安装：

	npm install babel-core babel-loader babel-preset-es2015 --save-dev

配置：

	module: {
	    loaders: [
	      {
	        test: /\.js$/,
	        exclude: /node_modules/,
	        loader: "babel"，
	        query: {
	          presets: ['es2015']
	        }
	      }
	    ]
	}


### 4、插件plugins
webpack的另一个强大功能。在整个构建过程中，执行相关任务，拓展webpack功能。webpack有很多内置插件，同时也有很多第三方插件。

要想使用插件，首先需要安装：

	npm install html-webpack-plugin --save-dev

然后在`plugins`属性中添加该插件的一个实例：

	plugins: [
	    new HtmlWebpackPlugin({
	        template: __dirname + "/app/index.tmpl.html"
	    })
	]

Hot Module Replacement（HMR）也是webpack中很实用的插件，允许在修改组件代码后，自动刷新实时预览修改后的效果。

实现HMR只需要进行如下几项配置。

在plugins中添加HMR插件：

	new webpack.HotModuleReplacementPlugin()

在devserver中添加hot属性：

	hot: true

在js模块中执行webpack提供的API。

### 5、产品构建
虽然以上内容可以构建一个完整的开发环境，但在产品阶段，可能还需要对打包的文件进行额外的处理，如：优化、压缩、缓存、代码分离等。

* OccurenceOrderPlugin，为组件分配ID，webpack可以分析和优先考虑使用最多的模块，并为其分配最小的ID。
* UglifyJsPlugin，压缩js代码
* ExtractTextPlugin，分离css和js文件

使用缓存最好的方法是保证文件名和文件内容相匹配。webpack可以把一个哈希值添加到打包的文件名中。

	output: {
	    path: __dirname + "/build",
	    filename: "[name]-[hash].js"
	}

---

参考文章：

[入门Webpack](http://www.jianshu.com/p/42e11515c10f)
