---
layout: post
title:  "Express快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/express_icon.jpg)

## 一、概述
> Fast, unopinionated, minimalist web framework for Node.js

Express是一个基于Node.js平台的Web应用开发框架。它提供了各种模块，可以快速地创建各种Web和移动应用。

[官网地址](http://expressjs.com/)

## 二、原理

### 1、http模块
Express框架在node.js的http模块之上，对http模块进行了封装，相对于加了一个中间层。

使用Node.js的http模块创建服务器：

    var http = require("http");
    var app = http.createServer(function(request, response) {
      response.writeHead(200, {"Content-Type": "text/plain"});
      response.end("Hello world!");
    });
    app.listen(3000, "localhost");

使用Express框架实现：

    var express = require('express');
    var app = express();
    app.get('/', function (req, res) {
      res.send('Hello world!');
    });
    app.listen(3000);

### 2、中间件
中间件就是处理HTTP请求的函数，用来完成各种特定的任务。其最大特点就是，当一个中间件处理完成之后，再传递给下一个中间件。

模块http的createServer方法，生成一个服务器实例，允许在运行过程中，调用一系列中间件。当一个HTTP请求进入服务器，服务器实例会调用第一个中间件，完成之后根据设置，决定是否再调用下一个中间件。每个中间件包含请求对象和响应对象，根据需要，决定是否调用next回调函数，将对象传递给下一个中间件。如果回调函数next带有参数，则表示抛出错误，参数为错误信息。抛出错误之后，后面的中间件将不再执行，直到发现一个错误处理函数为止。

### 3、use方法
use是express调用中间件的方法，它返回一个函数。

    app.use(function(request, response, next) {});

除了在回调函数内部判断请求的地址，也允许将请求的地址写在use方法的第一个参数。

    app.use('/', function(request, response, next) {});

针对不同的请求，express还提供了use方法的一些别名，包括all和http动词。

    app.all('*', function(request, response, next) {});
    app.get('/', function(request, response, next) {});    

### 4、路由
所谓路由，就是为不同的访问路径，指定不同的处理方法。

* express的Router类，可以创建模块化的路由的处理程序
* router实例对象的route方法，可以接受访问路径作为参数
* use方法为router对象指定中间件，即在数据发送给用户之前，对数据进行处理
* router对象的param方法用于路径参数的处理
* 调用app的route方法，创建路由。该方法会返回一个Route实例，它可以继续使用所有的HTTP方法

### 5、模板引擎
express支持多种模板引擎，如Handlebars等。模板默认存放在views目录，以html为后缀名。

安装模板引擎：

    npm install hbs --save-dev

加载hbs模块：

    require('hbs');

指定模板文件的后缀名：

    app.set('view engine', 'html');

运行hbs模块：

    app.engine('html', hbs.__express);

渲染模板：

    res.render('index');

以上代码把views目录下的index.html文件，交给hbs模板引擎进行渲染。

## 三、创建工程

安装：

    npm install -g express-generator@4

创建：

    express -e <projectname>

下载：

    cd <projectname> && npm install

启动：

    npm start

## 四、工程配置

### 1、目录结构

* bin，存放启动项目的脚本文件
* node_modules，存放所有依赖库
* public，存放静态文件
* routes，存放路由文件
* views，存放页面文件
* package.json，项目依赖配置文件
* app.js，应用核心配置文件

### 2、配置文件app.js

    // 加载依赖库
    var express = require('express'); 
    var path = require('path');
    var favicon = require('serve-favicon');
    var logger = require('morgan');
    var cookieParser = require('cookie-parser');
    var bodyParser = require('body-parser');

    // 加载路由控制
    var routes = require('./routes/index');

    // 创建项目实例
    var app = express();

    // 定义EJS模板引擎和模板文件位置
    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'ejs');

    // 定义icon图标
    app.use(favicon(__dirname + '/public/favicon.ico'));
    // 定义日志和输出级别
    app.use(logger('dev'));
    // 定义数据解析器
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: false }));
    // 定义cookie解析器
    app.use(cookieParser());
    // 定义静态文件目录
    app.use(express.static(path.join(__dirname, 'public')));

    // 匹配路径和路由
    app.use('/', routes);

    // 404错误处理
    app.use(function(req, res, next) {
      var err = new Error('Not Found');
      err.status = 404;
      next(err);
    });

    // 开发环境，500错误处理和错误堆栈跟踪
    if (app.get('env') === 'development') {
      app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
          message: err.message,
          error: err
        });
      });
    }

    // 生产环境，500错误处理
    app.use(function(err, req, res, next) {
      res.status(err.status || 500);
      res.render('error', {
        message: err.message,
        error: {}
      });
    });

    // 输出模型app
    module.exports = app;

### 3、启动文件./bin/www

    #!/usr/bin/env node

    // 依赖加载
    var app = require('../app');
    var debug = require('debug')('nodejs-demo:server');
    var http = require('http');

    // 定义启动端口
    var port = normalizePort(process.env.PORT || '3000');
    app.set('port', port);

    // 创建HTTP服务器实例
    var server = http.createServer(app);

    // 启动网络服务监听端口
    server.listen(port);
    server.on('error', onError);
    server.on('listening', onListening);

    // 端口标准化函数
    function normalizePort(val) {
      var port = parseInt(val, 10);
      if (isNaN(port)) {
        return val;
      }
      if (port >= 0) {
        return port;
      }
      return false;
    }

    // HTTP异常事件处理函数
    function onError(error) {
      if (error.syscall !== 'listen') {
        throw error;
      }

      var bind = typeof port === 'string'
        ? 'Pipe ' + port
        : 'Port ' + port

      // handle specific listen errors with friendly messages
      switch (error.code) {
        case 'EACCES':
          console.error(bind + ' requires elevated privileges');
          process.exit(1);
          break;
        case 'EADDRINUSE':
          console.error(bind + ' is already in use');
          process.exit(1);
          break;
        default:
          throw error;
      }
    }

    // 事件绑定函数
    function onListening() {
      var addr = server.address();
      var bind = typeof addr === 'string'
        ? 'pipe ' + addr
        : 'port ' + addr.port;
      debug('Listening on ' + bind);
    }

---

参考文章：

阮一峰：[JavaScript 标准参考教程（alpha）](http://javascript.ruanyifeng.com/nodejs/express.html)

张丹：[Node.js开发框架Express4.x](http://blog.fens.me/nodejs-express4/)
