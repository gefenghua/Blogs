---
layout: post
title:  "Ajax快速入门"
categories: jekyll update
---

## 一、概述
Ajax是Asynchronous JavaScript and XML的缩写。Asynchronous，是任务的一种执行模式，程序的执行顺序与任务的排列顺序是不一致的、异步的。JavaScript，是程序的核心，用来进行交互以及通信的控制与实现。XML，是进行交互以及通信的数据格式，目前通常采用JSON的格式。

传统web应用，每次用户的交互都需要向服务器发送请求，服务器接收并处理请求之后，返回新的页面给客户端浏览器，在此期间用户必须等待页面重新绘制完成。

使用Ajax，用户的交互交给JavaScript来处理而不是直接发送给服务器，此时页面不进行刷新，在此期间用户可以继续进行页面交互。当服务器将数据返回给JavaScript时，可以局部更新页面，从而用户在页面没有提交或刷新就得到新的数据。

---

## 二、原理
### 1、XMLHttpRequest对象
XMLHttpRequest对象是Ajax技术的核心，在IE 5中首次引入，是一种支持异步请求的技术。通过该对象，可以使用JavaScript向服务器提出请求并处理响应，而不阻塞用户的其他操作。使web应用程序像桌面应用程序一样，能够及时响应用户与服务器之间的交互，不必进行页面刷新或跳转，缩短等待时间，减轻服务器的负载。

使用XMLHttpRequest对象向服务器发送请求和处理响应之前，必须先创建一个XMLHttpRequest对象。在不同的浏览器中，创建XMLHttpRequest对象的创建方式也不相同，所以需要对浏览器进行判断。

	var xhr = new XMLHttpRequest();

或

	var xhr = new ActiveXObject("Microsoft.XMLHTTP");

后者支持IE 5和IE 6。

### 2、发送请求
从表单获取数据：

	var city = document.getElementById("city").value;

建立要连接的URL：

	var url = "/scripts/getCity.html?city=" + escape(city)

打开服务器连接：

	xhr.open("GET", url, true);

最后一个参数设为true，表示请求一个异步连接；如果设为false，表示发送请求后将等待服务器返回的响应。

设置连接之后服务器要进行的处理：

	xhr.onreadystatechange = updatePage;

属性onreadystatechange告诉服务器处理完请求之后，执行哪些操作。

发送请求：

	xhr.send();

### 3、处理响应
判断响应是否处于就绪状态：

	if (xhr.readyState === 4 && xhr.status === 200)

获得服务器的响应：

	var response = xhr.responseText;

设置表单数据：

	document.getElementById("zipCode").value = response;

---

## 三、改进的XMLHttpRequest
老版本的XMLHttpRequest对象存在很多缺点：

* 只支持文本数据的传送，无法读取和上传二进制文件
* 传送数据时，只能提示是否完成，没有进度信息
* 不支持跨域

新版本针对老版本的缺点，做出很大改进：

* 可以设置HTTP请求的时限
* 可以使用FormData对象管理表单数据
* 可以上传文件
* 可以获取二进制数据
* 可以获得数据传输的进度信息
* 支持跨域请求

### 1、HTTP请求时限
设置最长等待时间，超过这个时限，会自动停止HTTP请求：

	xhr.timeout = 3000;

### 2、FormData对象
为了方便表单处理，新增FormData对象，可以模拟表单：

	var formData = new FormData();
	formData.append('username', 'sean');
	formData.append('age', 20);
	xhr.send(formData);

### 3、上传文件
要上传的文件是表单元素，所以可以将它放到FormData对象中，实现文件上传：

	for (var i=0; i<files.length; i++) {
		formData.append('files[]', files[i]);
	}

### 4、获取二进制数据
利用新增的responseType属性，指定服务器返回的数据类型，默认是text文本类型。如果设置为blob，表示服务器返回的是二进制对象。

	xhr.responseType = 'blob';

还可以设置为arraybuffer，把二进制数据放在一个数组里：

	xhr.responseType = "arraybuffer";

### 5、获取进度信息
传送数据时，使用progress事件，返回进度信息。它分上传和下载两种情况：下载属于XMLHttpRequest对象，上传属于XMLHttpRequest.upload对象。

定义progress事件的回调函数：

	xhr.onprogress = updateProgress;
	xhr.upload.onprogress = updateProgress;

在回调函数中，处理事件：

	function updateProgress(event) {
		if (event.lengthComputable) {
			var percentComplete = event.loaded / event.total;
		}
	}

与progress事件相关的其他事件：

* load事件，传送成功完成
* abort事件，传送被用户取消
* error事件，传送中出现错误
* loadStart事件，传送开始
* loadEnd事件，传送结束

### 6、跨域请求
向不同域名的服务器发送HTTP请求，叫做跨域资源共享，简称CORS。前提是浏览器必须支持该功能，并且服务器端必须同意进行跨域。

	xhr.open('GET', 'http://www.google.com/');

---

## 四、jQuery中的用法
### 1、load(url [, data] [, callback])
可以远程载入HTML，并插入到DOM中。常用来从服务器上获取静态的数据文件。load方法的传递方式根据参数data来自动指定。如果没有参数，则采用GET方式，否则会采用POST方式。

参数说明：

* url，服务端资源的url
* data，发送到服务器的key/value数据
* callback，请求完成时的回调函数

### 2、get(url [, data] [, callback] [, type])
使用GET方式进行异步请求。服务器的状态和应用的模型数据不受GET操作的影响。

参数说明：

* url，服务端资源的url
* data，以key/value的形式构造查询字符串添加到url
* callback，在请求成功时被调用，将请求结果和状态传递给该方法
* type，服务器端返回内容的格式

### 3、post(url [, data] [, callback] [, type])
使用POST方式进行异步请求。发送到服务器的数据可以用来修改应用的模型数据。

get和post方式的区别：

* GET请求会将参数放在url后进行传递；POST请求会作为HTTP的消息体发送给服务器。
* GET对传输的数据大小有限制，通常不大于2KB；POST方式传递的数据大小理论上不受限制。
* GET方式请求的数据会被浏览器缓存，可能会带来安全问题；POST方式不会被浏览器缓存。

### 4、getScript
用来加载js文件，js文件会自动执行。

### 5、getJSON
用来加载JSON文件。

### 6、ajax(options)
是jQuery最底层的实现。参数包含了所需要的请求设置以及回调函数，以key/value形式存在。

选项：

* url，发送请求地址
* type，请求方式，默认为GET
* data，发送到服务器的key/value数据
* dataType，服务器返回的数据类型
* complete，请求完成后的回调函数。参数是XMLHttpRequest对象和一个描述成功请求类型的字符串
* success，请求成功的回调函数。参数是服务器返回的数据和描述状态的字符串
* error，请求失败时调用的函数。参数是XMLHttpRequest对象、错误信息和捕获的错误对象
* contentType，发送信息给服务器时的内部编码类型
* jsonp，在jsonp请求中替代callback的名称

---

参考资料：

[阮一峰： XMLHttpRequest Level 2 使用指南](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)