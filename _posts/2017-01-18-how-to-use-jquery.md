---
layout: post
title:  "jQuery快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/jquery_icon.png)

## 一、概述
>jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers.

jQuery是一个快速、简洁、具有丰富特性的JavaScript库。通过简单易用的API，兼容各主流浏览器，并使得HTML文档操作、事件处理、动画、Ajax交互等事情变得更加简单。

* 快速获取文档元素

	jQuery提供了快速查询DOM文档中元素的能力，而且强化了JavaScript中获取页面元素的方式。

* 漂亮的页面动态效果

	jQuery中内置了一系列的动画效果，如淡入淡出、元素移除等动态特效。

* 创建Ajax无刷新网页

	通过Ajax，可以对页面进行局部刷新，而不必每次数据更新都需要刷新整个页面。

* 对JavaScript的增强

	jQuery提供了对基本JavaScript结构的增强，如元素迭代、数组处理等操作。

* 增强的事件处理

	jQuery提供了各种页面事件，避免在HTML中添加太多的事件处理代码。

* 更改网页内容

	jQuery可以修改网页中的内容，简化了原本使用JavaScript处理的方式，如更改网页文本、插入图像等。

## 二、模块划分
jQuery的模块可以分为三部分：入口模块、底层支持模块和功能模块。

### 1、入口模块
在构造jQuery对象模块中，如果调用构造函数创建jQuery对象时传入了选择器表达式，则会调用选择器引擎遍历文档，查找与之匹配的DOM元素，并创建一个包含了这些DOM元素引用的jQuery对象。

	var div = $('#div-id');

其中，`$()`表示构造函数，`#div-id`表示选择器，`div`表示指向id为div-id的DOM元素的jQuery对象。

### 2、底层支持模块
回调函数列表模块用于增强对回调函数的管理，支持添加、移除、触发、锁定、禁用等功能。

异步队列模块用于解耦异步任务和回调函数，为回调函数增加了状态，支持传播任意同步或异步回调函数的状态。

数据缓存模块用于为DOM元素和JavaScript对象附加任意类型的数据。

队列模块用于管理一组函数，支持函数的入队和出队操作，并确保函数按顺序执行。

### 3、功能模块
事件系统提供了统一的事件绑定、响应、触发和移除机制，基于数据缓存模块来管理事件，而不是将事件直接绑定到DOM元素上。

Ajax模块允许从服务器上加载数据，而不用刷新页面，基于异步队列模块来管理和触发回调函数。

动画模块用于向网页添加动画效果，基于队列模块来管理和执行动画函数。

属性操作模块用于对HTML属性和DOM属性进行读取、设置和移除操作。

DOM遍历模块用于在DOM树中遍历父元素、子元素和兄弟元素。

DOM操作模块用于插入、移除、复制和替换DOM元素。

样式操作模块用于获取计算样式或设置样式。

坐标模块用于读取或设置DOM元素的文档坐标。

尺寸模块用于获取DOM元素的高度和宽度。

## 三、jQuery选择器
选择器通过标签名、属性名或内容对DOM元素进行快速、准确的定位。根据所获取页面中元素的不同，可以将选择器分为：基本选择器、层次选择器、过滤选择器和表单选择器。

### 1、基本选择器
使用最频繁的选择器，包括元素ID、Class名、元素名等。

id选择器：

	$('#element-id')

class选择器：

	$('.class-name')

元素选择器：

	$('element-name')

### 2、层次选择器
通过DOM元素间的层次关系获取元素，主要层次关系包括后代、父子、相邻、兄弟关系等。

根据祖先元素匹配所有后代元素：

	$('ancestor descendant')

根据父元素匹配所有的子元素：

	$('parent > child')

匹配所有紧接在prev元素后的相邻元素：

	$('prev + next')

匹配prev元素之后的所有兄弟元素：

	$('prev ~ siblings')

### 3、过滤选择器
过滤选择器根据某类过滤规则进行元素的匹配，以`:`开头。过滤选择器又分为：简单过滤选择器、内容过滤选择器、可见性过滤选择器、属性过滤选择器、子元素过滤选择器和表单对象属性过滤选择器。

#### 3.1 简单过滤选择器
获取页面第一个和最后一个X元素：

	$('element:first')
	$('element:last')

获取所有索引值为偶数和奇数的元素，索引值从0开始：

	$('element:even')
	$('element:odd')

获取等于、大于和小于索引值的元素：

	$('element:eq(index)')
	$('element:gt(index)')
	$('element:lt(index)')

获取除给定的选择器外的元素：

	$('element:not(selector)')

#### 3.2 内容过滤选择器
获取包含给定文本的元素：

	$('element:contains(text)')

获取所有不包含子元素或者文本的空元素：

	$('element:empty')

获取含有选择器所匹配的元素的元素：

	$('element:has(selector)')

获取含有子元素或文本的元素：

	$('element:parent')

#### 3.3 可见性过滤选择器
获取所有不可见的元素，或者type为hidden的元素：

	$('element:hidden')

获取所有可见的元素：

	$('element:visible')

#### 3.4 属性过滤选择器
获取包含给定属性的元素：

	$('element[attribute]')

获取属性是给定值的元素：

	$('element[attribute=value]')

获取属性不是给定值的元素：

	$('element[attribute!=value]')

获取属性是以给定值开始的元素：

	$('element[attribute^=value]')

获取属性是以给定值结束的元素：

	$('element[attribute$=value]')

获取属性是包含给定值的元素：

	$('element[attribute*=value]')

#### 3.5 子元素过滤选择器
获取父元素下的第一个、最后一个、唯一一个子元素：

	$('parent:first-child')
	$('parent:last-child')
	$('parent:only-child')

获取父元素下的特定位置的元素，索引值从1开始：

	$('parent:nth-child(eq|even|odd|index)')

#### 3.6 表单对象属性过滤选择器
获取表单中所有属性为可用的元素：

	$('element:enabled')

获取表单中所有属性为不可用的元素：

	$('element:disabled')

获取表单中所有被选中的元素：

	$('element:checked')

获取表单中所有被选中option的元素：

	$('element:selected')

### 4、表单选择器
通过它可以在页面中快速定位某表单对象。

获取所有input、textarea、select等input元素：

	$('form:input')

获取所有单行文本框：

	$('form:text')

获取所有密码框：

	$('form:password')

获取所有单项按钮：

	$('form:radio')

获取所有复选框：

	$('form:checkbox')

获取所有提交按钮：

	$('form:submit')

获取所有图像域：

	$('form:image')

获取所有重置按钮：

	$('form:reset')

获取所有按钮：

	$('form:button')

获取所有文件域：

	$('form:file')

## 四、DOM操作
在与页面中的元素进行交互式的操作中，主要包括对元素属性、内容、值、CSS等的操作。同时，还有对页面节点的操作，包括节点元素的创建、插入、复制、替换、删除等操作。

### 1、元素属性操作
在jQuery中，可以对元素属性进行获取、设置、删除等操作。

获取指定属性名的元素属性：

	$(selector).attr(name)

设置元素属性值，key为属性名称，value为属性值：

	$(selector).attr(key, value)

设置多个属性值：

	$(selector).attr({keyN:valueN})

删除指定属性名的元素属性：

	$(selector).removeAttr(name)

### 2、元素内容操作
在jQuery中，可以获取和设置元素的HTML或文本内容。

获取元素的HTML/文本内容：

	$(selector).html()
	$(selector).text()

设置元素的HTML/文本内容：

	$(selector).html(value)
	$(selector).text(value)

两者的区别是，`html()`方法仅支持HTML类型的文档，不支持XML。而`text()`方法不仅支持HTML类型，也支持XML类型。

### 3、元素值操作
在jQuery中，可以获取和设置元素的值。

获取元素的值：

	$(selector).val()

设置元素的值：

	$(selector).val(value)

Tips：通过`val().join(",")`获取select标签中的多个选项值。

### 4、元素样式操作
在jQuery中，可以直接设置样式、增加CSS类别、类别切换、删除类别等操作。

为指定name的样式设置值：

	$(selector).css(name, value)

为元素增加样式类：

	$(selector).addClass(class)

切换不同的样式类：

	$(selector).toggleClass(class)

删除元素的样式类：

	$(selector).removeClass(class)

### 5、创建节点元素
如果要在页面中添加某个元素，需要先通过构造函数创建节点元素：

	$(html)

### 6、插入节点元素
按照插入元素的位置区分，可以分为内部和外部两种插入方法。

#### 6.1 内部插入节点
向所选择的元素内部追加/前置内容：

	$(selector).append(content)
	$(selector).prepend(content)

向所选择的元素内部追加/前置function方法所返回的内容：

	$(selector).append(function())
	$(selector).prepend(function())

把所选择的元素追加/前置到另一个指定的元素集合中：

	$(selector).appendTo(content)
	$(selector).prependTo(content)

#### 6.2 外部插入节点
向所选择的元素外部追加/前置内容：

	$(selector).after(content)
	$(selector).before(content)

向所选择的元素外部追加/前置function方法所返回的内容：

	$(selector).after(function())
	$(selector).before(function())

把所选择的元素追加/前置到另一个指定的元素：

	$(selector).insertAfter(content)
	$(selector).insertBefore(content)

### 7、复制节点元素
将某个元素节点复制到另一个节点之后。

复制匹配的DOM元素并且选中复制成功的元素：

	$(selector).clone()

在复制时将该元素的所有行为也进行复制：

	$(selector).clone(true)

### 8、替换节点元素
将所有选择的元素替换成指定的HTML或DOM元素：

	$(selector).replaceWith(content)

将所有选择的元素替换成指定selector的元素：

	$(selector).replaceAll(selector)

一旦完成替换，被替换元素中的全部事件将会消失。

### 9、删除节点元素
删除指定的元素：

	$(selector).remove()

删除指定的元素，但保留被移除元素的事件：

	$(selector).detach()

清空所选择的页面元素的内容，但不移除该元素：

	$(selector).empty()

### 10、通用操作
这类操作不需要选择元素就可以直接使用。

	$.trim() 			//去除字符串两端的空格。
	$.each() 			//遍历一个数组或对象。
	$.inArray()			//返回一个值在数组中的索引位置。如果该值不在数组中，则返回-1。
	$.grep() 			//返回数组中符合某种标准的元素。
	$.extend()			//将多个对象，合并到第一个对象。
	$.makeArray() 		//将对象转化为数组。
	$.type() 			//判断对象的类别（函数对象、日期对象、数组对象、正则对象等等）。
	$.isArray()			//判断某个参数是否为数组。
	$.isEmptyObject() 	//判断某个对象是否为空（不含有任何属性）。
	$.isFunction() 		//判断某个参数是否为函数。
	$.isPlainObject() 	//判断某个参数是否为用"{}"或"new Object"建立的对象。
	$.support()			//判断浏览器是否支持某个特性。

## 五、事件
jQuery可以把事件直接绑定在网页元素上，使代码更加简洁。

### 1、事件机制
事件在触发后被分为两个阶段：一个是捕获capture，一个是冒泡bubbling。为阻止事件的冒泡现象，可以调用`stopPropagation()`方法或者语句`return false;`来实现。

### 2、载入事件
在jQuery脚本加载到页面时，会设置一个isReady标记，用于监听页面加载的进度，当遇到执行`ready()`方法时，通过查看isReady是否被设置，如果未被设置，说明页面并未加载完成，将未完成的部分用数组缓存起来，当全部加载完成后，再将未完成的部分通过缓存一一执行。

### 3、绑定事件
为所选择的元素绑定事件：

	$(selector).bind(type, [data], function)

其中，参数type为一个或多个事件类型的字符串，也可以自定义类型；参数data是作为`event.data`属性值传递给事件对象的额外数据对象；参数function是绑定到每个选择元素的事件中的处理方法。

如果在元素中绑定多个事件，可以将事件用空格隔开。还可以通过传入一个映射，对所选对象绑定多个事件处理方法。

### 4、切换事件
有两个以上的事件绑定于一个元素，在元素的行为动作间进行切换。

使元素在鼠标悬停和鼠标移出的事件间进行切换：

	hover()

可以依次调用N个指定的方法，直到最后一个方法：

	toggle()

### 5、移除事件
移除元素绑定的事件：

	$(selector).unbind([type], [function])

### 6、常用事件

	.blur()			//表单元素失去焦点。
	.change()		//表单元素的值发生变化
	.click() 		//鼠标单击
	.dblclick()		//鼠标双击
	.focus() 		//表单元素获得焦点
	.focusin() 		//子元素获得焦点
	.focusout()		//子元素失去焦点
	.keydown() 		//按下键盘（长时间按键，只返回一个事件）
	.keypress()		//按下键盘（长时间按键，将返回多个事件）
	.keyup() 		//松开键盘
	.load()			//元素加载完毕
	.mousedown()	//按下鼠标
	.mouseenter()	//鼠标进入（进入子元素不触发）
	.mouseleave()	//鼠标离开（离开子元素不触发）
	.mousemove()	//鼠标在元素内部移动
	.mouseout()		//鼠标离开（离开子元素也触发）
	.mouseover()	//鼠标进入（进入子元素也触发）
	.mouseup() 		//松开鼠标
	.ready() 		//DOM加载完成
	.resize() 		//浏览器窗口的大小发生改变
	.scroll() 		//滚动条的位置发生变化
	.select() 		//用户选中文本框中的内容
	.submit() 		//用户递交表单
	.unload() 		//用户离开页面

### 7、其他事件
为所选的元素绑定一个仅触发一次的处理方法：

	$(selector).one(type, [data], function)

在所选的元素上触发指定类型的事件：

	$(selector).trigger(type, [data])

## 六、特效
jQuery可以很方便地实现很多特效，并且还可以自定义动画效果。

### 1、显示与隐藏
显示/隐藏页面中的元素：

	$(selector).show()
	$(selector).hide()

Tips：可以调用`toggle()`方法来切换显示与隐藏状态。

### 2、淡入淡出
改变元素的透明度，实现淡入/淡出的动画效果：

	$(selector).fadeIn()
	$(selector).fadeOut()

还可以改变元素的透明度到指定的值：

	$(selector).fadeTo()

### 3、上下滚动
向下展开/向上卷起页面中的元素：

	$(selector).slideDown()
	$(selector).slideUp()

Tips：可以调用`slideToggle()`方法来切换展开与卷起状态。

### 4、自定义动画
复杂的特效可以使用`animate()`方法进行自定义。

	$('div').animate(
	  {
	    left : "+=50",	 //不断右移
	    opacity : 0.25 	 //指定透明度
	  },
	  300, 		         // 持续时间
	  function() { alert('done!'); } 	//回调函数
	);

Tips：可以使用`stop()`和`delay()`方法来停止或延迟特效的执行。

## 七、最佳实践

* 使用最新的版本

	新版本不止会带来新功能，还会提升性能。

* 选择合理的选择器

	选择同一个网页元素，可以使用多种选择器，但每种选择器的性能却是不一样的。按照性能高低排列：id选择器和元素标签选择器>class选择器>属性选择器。

* 通过父元素选择子元素

	最佳选择是使用`$parent.find('.child')`。由于`$parent`通常在前面的操作已经生成，jQuery会进行缓存，提高了执行速度。

* 不要过度使用jQuery

	在能够使用原生的JavaScript的场合，尽量避免使用jQuery。

* 利用缓存

	使用选择器的次数越少越好，并且尽可能地缓存选中的结果，以便复用。

* 使用链式写法

	因为采用链式写法时jQuery会自动缓存每一步的结果，所以比非链式写法要快很多。

* 事件的委托处理

	将需要多次绑定在子元素上的事件，委托给父元素来处理，减少绑定次数，从而提高性能。

* 尽量少地改动DOM结构

	不要频繁改动DOM结构，如果要插入多个元素，可以先将其合并，然后再一次性插入。如果要对DOM元素进行大量处理，应该先使用`.detach()`方法，将元素从DOM中移除，处理完之后再重新插回文档。在DOM元素上存储数据时，应该使用`$.data()`方法。插入html代码时，原生的`innterHTML()`方法要比jQuery对象的`html()`方法更快。

* 合理地使用循环

	如果可以使用复杂的选择器直接选中元素，就不要使用循环。应该优先使用原生的循环方法。

* 尽量少生成jQuery对象

	每使用一次选择器，都会生成一个jQuery对象，带有很多属性和方法，会占用不少资源。能够使用jQuery函数的场合，尽量不要使用jQuery对象。

* 选择作用域链最短的方法

	JavaScript的变量采用链式作用域，读取局部变量要比全局变量快很多。在调用对象方法时，closure模式要比prototype模式更快。

* 使用Pub/Sub模式管理事件

	当发生某事件后，如果要连续执行多个操作，可以改用事件触发的形式。

* DOM对象与jQuery对象转换

	DOM对象指的是通过传统的JavaScript方法获取的DOM元素对象。jQuery对象指的是通过jQuery语法包装原始的DOM对象后生成的新对象。如果需要使DOM对象与jQuery对象之间的方法互相调用，必须先实现对象之间的类型转换。不能使用DOM对象调用jQuery对象的方法，也不能使用jQuery对象调用DOM对象的方法。

	调用jQuery中提供的`[index]`与`get(index)`方法即可将jQuery对象转换成DOM对象。DOM对象只要通过jQuery的方法`$()`进行封装，就可以转换成jQuery对象。
