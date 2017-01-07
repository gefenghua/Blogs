---
layout: post
title:  "JavaScript快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/javascript_icon.jpg)

## 一、概述
JavaScript是世界上最流行的脚本语言，是一种运行在浏览器中的解释型的编程语言，能够实现跨平台、跨浏览器。虽然只是十多天时间的产物，并且有很多的缺陷和陷阱，但也造就了JavaScript的灵活和强大。随着Node.js的兴起，JavaScript已经从单纯实现前端互动，发展到可以全栈实现整个应用。尤其是在移动互联网蓬勃发展以及应用追求极致用户体验的今天，JavaScript更是必须要重视和掌握的。

## 二、变量

### 1、变量的声明
使用var关键字进行变量的声明，声明的同时也可以进行赋值。变量的声明只能有一次，但赋值可以有多次。如果只声明变量，但是变量没有被赋值，此时变量的值为undefined。

	var s = "Hello";

由于JavaScript属于动态语言，所以在定义变量时，不需要指定变量的类型。可以把任意数据类型赋值给变量，同一个变量可以使用不同的数据类型反复赋值。

### 2、变量的提升
根据JavaScript的规则，变量的声明会被提升，但是变量的初始化并不会被提升。函数外声明的变量，会被提升到文档顶部。函数内声明的变量，会被提升到函数顶部。

	function f() {
        var x = 1 + y;
        alert(x);
        var y = 2;
	}
    alert(x);

如果变量在函数内没有声明，该变量会被提升为全局变量。

	function f() {
        x = 1;
    }
    alert(x);

### 3、变量的作用域
如果变量在函数内声明，则该变量的作用域为整个函数体，在函数外不可访问，属于局部变量。

	function f() {
        var x = 1;
    }
    alert(x);

函数可以嵌套，内部函数可以访问外部函数定义的变量，反之则不可。

	function outer() {
        var x = 1;
        function inner() {
            var y = x + 1;
            alert(y);
        }
        var z = y + 1;
        alert(z);
	}

如果内部函数与外部函数定义了相同的变量，根据就近原则，内部函数的变量会覆盖外部函数的变量。

	function outer() {
        var x = 1;
        function inner() {
            var x = 2;
            alert(x);
        }
    }

## 三、函数

### 1、函数的定义
定义函数有两种方式：声明和表达式。

#### 1.1 函数声明

	function function_name(parameters) {
        ......
	}

函数声明后不会马上执行，只有被调用的时候才会执行。

#### 1.2 函数表达式

	var f = function (parameters) {
        ......    
    };
    f(parameters);

将函数存储在变量中，之后可以通过变量名来调用，而不需要函数名称。

### 2、函数的参数
函数有个内置的对象arguments，包含了函数调用的所有参数。

参数有两种传递方式：值传递和引用传递。

#### 2.1 值传递
函数只是获取参数值，如果修改参数的值，不会改变参数的原始值，在函数外是不可见的。

#### 2.2 引用传递
函数获得的是对象的引用，在函数内部修改对象属性时，会改变属性的原始值，在函数外可见。

### 3、函数的调用
函数可以通过四种方式进行调用。

#### 3.1 自我调用
函数表达式，如果后面紧跟()，则会自动自我调用。声明的函数不能自我调用。

	(function () {
        alert("Hello");
    })();

#### 3.2 作为函数被调用
当函数没有被其他对象调用时，函数默认是window全局对象的函数。此时this的值是window全局对象。

	function f() {
        alert("Hello");
        return this;
    };
    f();

#### 3.3 作为方法被调用
可以将函数定义为对象的方法，该对象是函数的所有者。当使用对象调用方法时，此时的this值指向调用函数的对象。

	var obj = {
        x : 1,
        y : 2,
        f : function () { return this.x + this.y;}
    };
    obj.f();

#### 3.4 作为构造函数被调用
如果使用new关键字，实际是创建了一个新的对象。新对象会继承构造函数的属性和方法。此时this值指向新创建的对象。为了区分普通函数和构造函数，约定构造函数的首字母为大写。

	function F(x, y) {
        this.x = x;
        this.y = y;
    };
    var obj = new F(1, 2);

#### 3.5 call和apply
函数是一个对象，可以调用call或apply方法。此时this值指向方法传递的对象。

	function f(x, y) {
        return x+y;
	};
	arr = [1, 2];
	f.call(obj, 1, 2);
	f.apply(obj, arr);

call和apply方法的唯一区别就是前者传递整个参数列表，而后者传递一个参数数组。

### 4、闭包
内嵌函数可以访问上一层函数的变量。闭包是可以访问上一层函数作用域里变量的函数。如果上一层函数返回闭包函数，那么就可以在上一层函数的外部访问该函数的私有变量。利用闭包，可以实现调用公共方法，访问私有变量。

此时返回的函数并没有立刻执行，只有在调用返回函数时才真正执行。因为返回函数不会马上执行，所以返回函数不要引用循环变量或后续会发生变化的变量。否则，在真正执行的时候，引用的变量可能已经发生改变。

如果必须引用的话，可以再创建一个函数，用该函数的参数绑定循环变量的值。这样，循环变量的值虽然发生了改变，但绑定到参数的值不会随之改变。

## 四、对象

### 1、对象的定义
对象的定义有两种方式。

由若干键值对组成，并将其赋值给一个变量。

	var person = {
	    name : "sean",
	    age : "20",
	    sayHello : function() {
	        alert("Hello " + name);
	    }
	};
	person.sayHello();

使用函数定义对象，然后创建新的对象实例。

	function person(name, age) {
	    this.name = name;
	    this.age = age;
	};
	var p = new person("sean", 20);

### 2、属性和方法
由于JavaScript的对象是动态类型，所以可以给一个对象添加或删除属性。当访问的属性不存在时，会返回undefined。如果要判断一个属性是否属于某对象，可以使用in操作符。这里要注意的是，该属性可能是对象继承得来的。如果要判断一个属性是否属于某对象自身，可以使用hasOwnProperty方法。

### 3、原型
虽然JavaScript可以面向对象编程，但所有的对象都是实例，不会区分类和实例的概念。要想实现类的概念，就需要借助原型。

首先创建一个相当于类概念的对象实例。

	var person = {
	    name : "",
	    age : "",
	    sayHello : function() {
	        alert("Hello " + this.name);
	    }
	};

然后创建一个新的对象实例，使其依赖这个原型。

	var sean = {
	    name : "sean"
	};
	sean.__prototype__ = person;
	sean.sayHello();

也可以使用Object.create()方法，传入一个原型对象，并创建一个基于该原型的新对象。

	var sean = Object.create(person);
	sean.name = "sean";
	sean.sayHello();

### 4、创建对象
JavaScript对每个创建的对象都会设置一个原型，指向该对象的原型对象。除了直接创建一个对象外，还可以使用构造函数来创建对象。使用构造函数创建的对象，会从原型获得一个constructor属性，该属性指向构造函数本身。

### 5、原型链
当访问一个对象的属性时，首先在当前对象上查找该属性。如果没有找到，就到其原型对象上查找。如果还没有找到，就一直上溯到Object的原型对象。如果还没有找到，就返回undefined。这样就形成了一条原型链。

因为函数也是对象，所以函数也拥有原型链。首先是函数本身，然后是Function.prototype，最后是Object.prototype。

### 6、继承
继承不仅仅是对象的属性和方法的继承，原型链也要体现出继承关系。

首先，定义新的构造函数，并在内部调用希望继承的构造函数，实现属性和方法的继承。

其次，借助中间函数，将新的构造函数的原型指向继承的构造函数的原型，实现原型链继承。

最后，将新的构造函数的constructor属性重新指向新的构造函数。

### 7、浏览器对象

window对象不但是全局对象，还表示浏览器窗口。
属性innerWidth和innerHeight分别表示浏览器窗口的内部宽度和高度，即用于显示网页的净宽高。属性outerWidth和outerHeight分别表示浏览器窗口的整个宽度和高度。

navigator对象表示浏览器的信息。属性appName表示浏览器名称。属性appVersion表示浏览器版本。属性language表示浏览器设置的语言。属性platform表示操作系统类型。属性userAgent表示浏览器设定的User-Agent字符串。

screen对象表示屏幕的信息。属性width表示屏幕宽度，单位是像素。属性height表示屏幕高度，单位是像素。属性colorDepth表示颜色位数。

location对象表示当前页面的URL信息。属性href表示整个URL。属性protocal表示使用的协议。属性host表示主机名。属性port表示端口号。属性pathname表示访问的路径。属性search表示查询参数。方法assign加载一个新页面。方法reload重载当前页面。

document对象表示当前页面。属性title表示浏览器窗口的标题。属性cookie获取当前页面的Cookie。方法getElementById按照元素的id获取一个DOM节点。方法getElementByTabName按照元素的标签名称获取一组DOM节点。

history对象保存浏览器的历史记录。

## 五、DOM
当页面被加载时，浏览器解析HTML文档，并会创建页面的文档对象模型DOM。DOM是一个树形结构，通过操作DOM节点，可以访问或改变文档的所有元素。针对DOM的操作主要有四种：查找、更新、插入、删除。

### 1、查找
要想操作DOM节点，首先需要找到要操作的节点。

* 通过id查找

	调用document.getElementById方法查找指定id的元素。

* 通过标签名查找

	调用document.getElementByTagName方法查找指定标签名的所有元素。

* 通过类名查找

	调用document.getElementByClassName方法查找指定class的所有元素。

Tips：要精确地定位DOM，可以先定位父节点，再逐步缩小范围。

### 2、更新
定位好节点之后，就可以进行更新操作。

* 修改innerHTML属性

	不但可以修改一个DOM节点的文本内容，还可以通过HTML片段修改DOM节点内部的子树。

* 修改innerText或textContent属性

	可以对字符串进行HTML编码，保证无法设置任何HTML标签。这两个属性的区别是前者不返回隐藏元素的文本，而后者则返回所有文本。

* 修改style属性

	可以用来设置CSS样式。如果是无效的属性名，可以在JavaScript中使用驼峰式命名属性。

### 3、插入
可以插入新的节点，从而改变DOM结构。

* appendChild方法

	把一个子节点添加到父节点的最后一个子节点。如果子节点已经存在，会先从原来的位置删除，然后再插入到新的位置。通常会新创建一个节点，然后插入到指定位置，实现动态添加节点。

* insertBefore方法

	父节点会将新建的子节点插入到参照节点之前。

### 4、删除
可以删除节点，从而改变DOM结构。

* removeChild方法

	首先获得要删除的节点和它的父节点，然后调用父节点的方法将其删除。遍历父节点的子节点并进行删除操作时，父节点的children属性会在子节点发生变化时实时更新，需要特别注意。

## 六、异步处理
JavaScript语言是单线程的，一次只能完成一个任务。好处是实现简单，坏处是如果有任务耗时很长，其他的任务就必须得排队等待，直到该任务完成。导致的结果就是造成浏览器无响应，其他任务无法执行，体验极差。

因此，JavaScript语言分成两种任务执行模式：同步和异步。所谓同步，就是指程序的执行顺序与任务的排列顺序是一致的、同步的。所谓异步，是指每个任务有一个或多个回调函数，后一个任务无需等待前一个任务完成就可执行，当前一个任务完成后，执行回调函数。这样，程序的执行顺序与任务的排列顺序是不一致的、异步的。

### 1、回调函数
这是异步处理最常用的的方法。

两个同步执行的函数：

	f1();
	f2();

两个异步执行的函数：

	function f1(callback){
        setTimeout(function () {
            // f1的任务代码
            callback();
        }, 1000);
    }

	f1(f2);

回调函数的优点是简单、容易理解。缺点是不利于阅读和维护、高耦合、流程混乱，每个任务只能指定一个回调函数。

### 2、事件监听
采用事件驱动模式，由事件来决定要进行的处理，而不是代码的顺序。

为f1绑定一个事件，当f1发生done事件，就执行f2：

	f1.on('done', f2);

执行完成后，立即触发done事件，从而开始执行f2：

	function f1(){
        setTimeout(function () {
            // f1的任务代码
            f1.trigger('done');
        }, 1000);
    }

事件监听的优点是容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，低耦合。缺点是流程不够清晰。

### 3、发布/订阅
与事件监听类似。某个任务执行完成之后，会发布一个信号，其他任务可以订阅这个信号，从而知道什么时候可以开始执行。又称为观察者模式。

订阅信号：

	jQuery.subscribe("done", f2);

发布信号：

	function f1(){
        setTimeout(function () {
            // f1的任务代码
            jQuery.publish("done");
        }, 1000);
    }

取消订阅：

	jQuery.unsubscribe("done", f2);

可以了解存在多少信号、每个信号有多少订阅者，从而监听程序的运行。

### 4、Promises对象
该对象是CommonJS提出的一种规范，旨在为异步编程提供统一接口。

每一个异步任务返回一个Promises对象：

	function f1(){
        var dfd = $.Deferred();
        setTimeout(function () {
            // f1的任务代码
            dfd.resolve();
        }, 500);
        return dfd.promise;
    }

该对象有一个then方法，可以指定回调函数：

	f1().then(f2);	

优点是回调函数可以使用链式写法，流程清晰，并且如果一个任务已经完成，再添加回调函数，该回调函数会立刻执行。缺点是编写和理解比较困难。

---

参考资料：

[w3cschool](http://www.w3cschool.cn/javascript/)

[廖雪峰的JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)

阮一峰：[Javascript异步编程的4种方法](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html)
