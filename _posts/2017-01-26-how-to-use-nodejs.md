---
layout: post
title:  "Node.js快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/nodejs_icon.jpg)

## 一、概述
> Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.

Node.js是一个基于Google Chrome的V8引擎的JavaScript运行环境，它允许使用JavaScript语言编写服务器端代码。Node.js具有事件驱动和I/O非阻塞两大特点，轻量而高效。

[官网地址](https://nodejs.org/en/)

## 二、安装与运行
从官网下载最新版本的Node.js，直接安装。安装时选择全部组件，并添加到环境变量Path中。

运行命令：`node -v`查看是否安装成功。

运行命令：`node`进入到node.js的交互环境，可以输入任意JavaScript语句，并查看运行结果。

运行命令：`node <*.js>`直接执行js文件里的内容。

运行命令：`node --use_strict <*.js>`为js文件开启严格模式。

安装Node.js的时候，会自动安装包管理工具npm。

运行命令：`npm -v`查看npm的版本。

命令行模式会一次性执行js文件，中间没有交互；交互模式则是每一行单独执行，可以进行交互。

如果不想使用集成的IDE，可以使用VS Code编辑器进行Node.js的开发和调试。具体操作和配置可以参照VS Code的官网教程。

[VS Code官网地址](http://code.visualstudio.com/)

[Node.js Applications with VS Code](https://code.visualstudio.com/Docs/runtimes/nodejs)

## 三、模块
在Node.js环境中，一个js文件就称之为一个模块。使用模块提高了代码的可维护性以及可重用性，还避免了函数名和变量名冲突。

当在模块中定义一个对象（对象、函数、数组等）时，可以输出这个对象以供其他模块使用：

	module.exports = object_name;

其他模块要想使用这个对象，可以在模块中引入该对象：

	var variable_name = require('/path/to/the/module/module_name');

Tips：如果不指定路径，Node会按照内置模块、全局模块、当前模块的顺序进行查找。

### 1、基本模块
在浏览器中，JavaScript有一个全局对象window；而在Node.js环境中，也有一个全局对象global。由于JavaScript代码既能在浏览器执行，也能在Node环境下执行，可以通过全局对象名称来判断JavaScript代码是在哪个环境下执行的。

process也是Node.js提供的一个对象，表示当前Node.js进程，进程本身的事件由process来处理。

### 2、常用内置模块
fs模块是文件系统模块，负责文件的读写操作，提供了同步和异步两种方法。大部分作为服务端的代码，必须使用异步代码。但是在服务器启动时读取配置文件，或结束时写入状态等操作，因为这些代码只执行一次，所以可以使用同步方法。

读文本文件：

	var fs = require('fs');
	fs.readFile('input.txt', 'utf-8', function (err, data) {
	  if (err) {
	    console.log(err);
	  } else {
	    console.log(data);
	  }
	});

其中，readFile函数的第一个参数是文件名，第二个参数是文件编码，第三个参数是一个回调函数。回调函数的第一个参数代表错误信息，第二个参数代表返回结果。

读二进制文件：

	var fs = require('fs');
	fs.readFile('input.png', function (err, data) {
	  if (err) {
	    console.log(err);
	  } else {
	    console.log(data);
	    console.log(data.length + ' bytes');
	  }
	});

其中，不需要文件编码，并且回调函数的data参数将返回一个Buffer对象。Buffer对象是一个包含零个或任意个字节的数组。

写文件：

	var fs = require('fs');
	var data = 'Hello world';
	fs.writeFile('output.txt', data, function (err) {
	  if (err) {
	    console.log(err);
	  } else {
	    console.log('success');
	  }
	});

其中，writeFile函数的第一个参数是文件名，第二个参数是要写入的数据，第三个参数是回调函数。

读取文件的相关信息：

	fs.stat('input.txt', function (err, stat) {
	  ...
	});

stream是一个只能在服务端使用的模块，用来支持流数据结构，流中的数据是有序的。流是一个对象，使用时只需关注流的事件即可。data事件表示流的数据可以读取，end事件表示流没有数据可以读取，error事件表示出现错误。

读文件：

	var fs = require('fs');
	var rs = fs.createReadStream('input.txt', 'utf-8');
	rs.on('data', function (chunk) {
	  console.log('DATA:');
	  console.log(chunk);
	});
	rs.on('end', function () {
	  console.log('END');
	});
	rs.on('error', function (err) {
	  console.log('ERROR: ' + err);
	});

写文件：

	var fs = require('fs');
	var ws1 = fs.createWriteStream('output1.txt', 'utf-8');
	ws1.write('使用Stream写入文本数据...\n');
	ws1.write('END.');
	ws1.end();

	var ws2 = fs.createWriteStream('output2.txt');
	ws2.write(new Buffer('使用Stream写入二进制数据...\n', 'utf-8'));
	ws2.write(new Buffer('END.', 'utf-8'));
	ws2.end();

可以使用管道操作pipe将读取流和写入流串联起来：

	var fs = require('fs');
	var rs = fs.createReadStream('input.txt');
	var ws = fs.createWriteStream('output.txt');
	rs.pipe(ws);

默认情况下，当读取流的数据读取完毕，会触发end事件，并自动关闭写入流。如果不想关闭，需要传递额外的参数。

	readable.pipe(writable, { end: false });

http模块是web应用最常用的一个模块，提供了request和response对象。request对象封装HTTP请求，调用request对象的属性和方法，可以获取HTTP请求的所有信息；response对象封装HTTP响应，调用response对象的方法，可以把HTTP响应返回给浏览器。

	var http = require('http');
	var server = http.createServer(function (request, response) {
	  // 获取HTTP请求的method和url
	  console.log(request.method + ':' + request.url);
	  // 将HTTP响应的内容写入response
	  response.writeHead(200, {'Content-Type': 'text/html'});
	  response.end('<h1>Hello world!</h1>');
	});
	// 监听8080端口
	server.listen(8080);

url模块可以用来将URL字符串解析成一个Url对象，从而获取想要的信息。

	var url = require('url');
	console.log(url.parse('http://user:pass@host.com:8080/path/to/file?query=string#hash'));

path模块可以用来处理本地文件目录。

	var path = require('path');
	// 解析当前目录
	var workDir = path.resolve('.');
	// 组合完整的文件路径
	var filePath = path.join(workDir, 'pub', 'index.html');

crypto模块提供通用的加密和哈希算法。

MD5哈希算法用来给任意数据一个签名，通常用一个十六进制字符串表示：

	const crypto = require('crypto');
	const hash = crypto.createHash('md5');
	hash.update('Hello, world!');
	console.log(hash.digest('hex'));

Hmac哈希算法可以利用MD5等算法，另外还需要一个密钥：

	const crypto = require('crypto');
	const hmac = crypto.createHmac('sha256', 'secret-key');
	hmac.update('Hello, world!');
	console.log(hmac.digest('hex'));

AES是一种常用的对称加密算法，加密与解密使用同一个密钥。crypto模块提供了AES支持，但需要自行封装。DiffieHellman算法是一种密钥交换协议，可以让双方自行协商一个密钥。crypto模块还可以处理数字证书。

## 四、Web开发
使用Node.js开发Web服务器端，有几点好处：

* 前后端统一使用JavaScript，不需要切换语言
* 异步处理大大提高了运行速度

针对Node.js，出现了很多后端相关的Web框架、ORM框架、模板引擎、测试框架、构建工具等。

### 1、Web框架Express/koa
Express是第一代最流行的Web服务端框架，它对Node.js的http进行了封装。虽然Express的API非常简单，但是由于是基于ES5，要实现异步，只能使用回调。如果异步嵌套层次过多，代码很难理解。

	var express = require('express');
	var app = express();
	app.get('/', function (req, res) {
	  res.send('Hello World!');
	});
	app.listen(3000, function () {
	  console.log('Example app listening on port 3000!');
	});

koa1基于ES6，使用generator实现异步。虽然使用generator在写法上比回调简单，但其本意并不是用于异步。真正设计用来实现异步的是Promise，但Promise的写法过于复杂。

	var koa = require('koa');
	var app = koa();
	app.use('/test', function *() {
      yield doReadFile1();
      var data = yield doReadFile2();
      this.body = data;
	});
	app.listen(3000);

koa2基于ES7，使用Promise和关键字`async`配合实现异步，同时兼容generator的写法。

	app.use(async (ctx, next) => {
      await next();
      var data = await doReadFile();
      ctx.response.type = 'text/plain';
      ctx.response.body = data;
	});

### 2、ORM框架Sequelize
ORM技术是将关系型数据库的表结构映射到对象上，Sequelize返回Promise对象，可以更好地进行异步处理。

ES6：

	Pet.findAll()
     .then(function (pets) {
       for (let pet in pets) {
         console.log(`${pet.id}: ${pet.name}`);
       }
     }).catch(function (err) {
       // error
     });

ES7：

	(async () => {
      var pets = await Pet.findAll();
	})();

要想使用Sequelize操作数据库，首先需要创建一个Sequelize实例。然后定义数据模型Model，使用Sequelize映射数据库表。这样就可以调用实例的相应方法来对数据库进行操作。

Sequelize操作数据库的一般步骤：

* 通过Model对象的findAll()方法获取实例
* 如果要更新实例，先对实例属性进行赋值，然后调用save()方法
* 如果要删除实例，直接调用destroy()方法

### 3、模板引擎Jade/Pug
Node.js默认使用Jade作为模板引擎（现改名为Pug）。Jade是Node.js的一个模块，jade文件可以被预编译为.js文件，也可以被编译为目标html代码。

[主页](https://pugjs.org/api/getting-started.html)

### 4、测试框架Mocha
Mocha是JavaScript的单元测试框架，既可以在浏览器环境运行，也可以在Node.js的环境运行。

特点：

* 既可以测试简单的JavaScript函数，又可以测试异步代码
* 既可以自动运行所有测试，也可以只运行特定的测试
* 可以支持before、after、beforeEach和afterEach来编写初始化代码

### 5、构建工具Webpack

### 6、WebSocket协议
WebSocket利用HTTP协议建立连接，在浏览器和服务器之间建立双向通信的通道，服务器可以主动向浏览器发送消息，而不需要通过浏览器发送请求。

首先，WebSocket连接必须由浏览器发起，因为请求协议是一个标准的HTTP请求。

	GET ws://localhost:3000/ws/chat HTTP/1.1
	Host: localhost
	Upgrade: websocket
	Connection: Upgrade
	Origin: http://localhost:3000
	Sec-WebSocket-Key: client-random-string
	Sec-WebSocket-Version: 13

与普通HTTP请求的区别：

* GET请求的地址以`ws://`开头
* 请求头`Upgrade: websocket`和`Connection: Upgrade`表示连接将被转换为WebSocket连接
* `Sec-WebSocket-Key`用于标识这个连接
* `Sec-WebSocket-Version`指定WebSocket的协议版本

然后，如果服务器接受请求，会返回响应：

	HTTP/1.1 101 Switching Protocols
	Upgrade: websocket
	Connection: Upgrade
	Sec-WebSocket-Accept: server-random-string

其中，响应码101表示将切换协议，切换后的协议通过Upgrade来指定。

在Node.js中，最常用的WebSocket模块是ws。创建一个WebSocket的服务器实例：

	const WebSocket = require('ws');
	const WebSocketServer = WebSocket.Server;
	const wss = new WebSocketServer({
	  port: 3000
	});

如果有WebSocket请求接入，wss对象可以响应connection事件来处理这个WebSocket。对于每个WebSocket连接，都要对它绑定某些事件方法来处理不同的事件。

---

参考文章：

廖雪峰：[Node.js教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434501245426ad4b91f2b880464ba876a8e3043fc8ef000)

[使用node.js进行服务器端JavaScript编程](http://www.ibm.com/developerworks/cn/web/1107_chengfu_nodejs/index.html)
