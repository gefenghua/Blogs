---
layout: post
title:  "HTML快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/html_icon.jpg)

## 一、概述
HTML是超文本标记语言（Hyper Text Markup Language）的缩写。它不是一种编程语言，而是一种标记语言，HTML通过使用标记标签来描述网页。Web浏览器读取HTML文档，并以网页的形式显示出来。浏览器不会显示HTML标签，而是使用标签来解释页面的内容。

## 二、概念

### 1、标签
HTML文档和HTML元素是通过HTML标签进行标记的。HTML标签由开始标签和结束标签组成，某些HTML元素没有结束标签。

### 2、元素
HTML元素是从开始标签到结束标签的所有代码，开始标签与结束标签之间的内容被称为元素的内容。大多数HTML元素可以拥有属性，并且可以嵌套。

### 3、属性
HTML标签可以拥有属性，属性提供了有关HTML元素的更多信息。属性总是以名称-值对的形式出现，并且总是在HTML元素的开始标签中定义。属性值应该始终被包括在引号内，通常使用双引号，如果属性值本身包含双引号，则必须使用单引号。

## 三、用法

### 1、标题\<h>
标题通过`<h1>`至`<h6>`等标签进行定义，其中`<h1>`定义最大的一级标题，`<h6>`定义最小的六级标题。在同一文档中，`<h1>`只能出现一次，其他标题标签可以出现多次。默认情况下，HTML会自动在块级元素前后添加一个空行，如标题、段落等。搜索引擎会使用标题为网页的结构和内容编制索引。

	<h1>This is a heading</h1>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/01_tag_h.png)

### 2、段落\<p>
段落是通过`<p>`标签定义的，用于HTML文档的分割。在段落中进行换行，可以使用`<br />`标签。显示页面时，浏览器会移除HTML文档中多余的空格和空行，所有连续的空格或空行会被当成一个空格。

	<p>This is<br />a para<br />graph with line breaks</p>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/02_tag_p.png)

### 3、元素容器\<div>
`<div>`元素是块级元素，它是可用于组合其他HTML元素的容器。与CSS一同使用，`<div>`元素可用于对大的内容块设置样式属性。另一个常见的用途是文档布局。

	<div style="color:#0000FF">
	  <h3>title in div</h3>
	  <p>text in div</p>
	</div>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/03_tag_div.png)

### 4、文本容器\<span>
`<span>`元素是内联元素，可用作文本的容器。与CSS一同使用时，`<span>`元素可用于为部分文本设置样式属性。

	<p>Please click the <span style="color:blue">blue</span> text.</p>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/04_tag_span.png)

### 5、样式style
通过使用`style`属性直接将样式添加到HTML元素，或者间接地在CSS文件中进行定义。样式包括背景颜色、字体、颜色、尺寸、文本对齐等。

### 6、链接\<a>
使用`<a>`标签来创建连接到另一个文档的链接。`<a>`用来创建锚，锚可以指向网络上的任何资源，如HTML页面、图像、多媒体文件等。开始标签和结束标签之间的文字作为超级链接显示。

	<a href="http://www.google.cn/" target="_blank">Google!</a>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/05_tag_a.png)

* `href`属性用于定位需要链接的文档
* `target`属性可以定义被链接的文档在何处显示
* `name`属性用于创建命名锚，将#号和锚名称添加到URL的尾部，可以直接链接到命名锚

tips：总是将正斜杠添加到地址的尾部，否则服务器会产生两次HTTP请求。

### 7、表格\<table>
表格由`<table>`标签来定义，表格的表头由`<th>`标签进行定义，表格的行由`<tr>`标签定义，单元格由`<td>`标签定义。单元格可以包含文本、图片、列表、段落、表单、表格、水平线等元素。部分浏览器，空单元格不会显示边框，可以使用空格占位符`&nbsp;`来显示边框。

	<table border="1">
		<tr>
			<td>row 1, cell 1</td>
			<td>row 1, cell 2</td>
		</tr>
		<tr>
			<td>row 2, cell 1</td>
			<td>&nbsp;</td>
		</tr>
	</table>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/06_tag_table.png)

* `border`属性定义表格的边框

### 8、列表\<ul>\<ol>
HTML支持有序、无序、自定义列表。

无序列表使用`<ul>`标签，列表项使用`<li>`标签，每个项目使用粗体原点进行标记。

	<ul>
		<li>Coffee</li>
		<li>Milk</li>
	</ul>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/07_tag_ul.png)

有序列表使用`<ol>`标签，列表项使用`<li>`标签，每个项目使用数字进行标记。列表项内部可以使用段落、换行符、图片、链接以及其他列表等元素。

	<ol>
		<li>Coffee</li>
		<li>Milk</li>
	</ol>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/08_tag_ol.png)

自定义列表不只是一列项目，而是项目及其注释的组合。自定义列表使用`<dl>`标签，每个列表项使用`<dt>`标签，每个列表项的定义使用`<dd>`标签。

	<dl>
	 <dt>Coffee</dt>
	   <dd>Black hot drink</dd>
	 <dt>Milk</dt>
	   <dd>White cold drink</dd>
	</dl>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/09_tag_dl.png)

### 9、表单\<form>
表单是一个包含表单元素的区域，使用`<form>`标签定义。表单元素是允许用户在表单中输入信息的元素，如文本框、下拉列表、单选框、复选框等。经常被用到的表单标签是`<input>`输入标签，输入类型由该标签的type属性定义。

	<form name="input" action="form_action.jsp" method="get">
		Username: 
		<input type="text" name="user" />
		<input type="submit" value="Submit" />
	</form>

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-05-how-to-use-html/10_tag_form.png)

* `action`属性定义表单的目标文档的文件名

由动作属性定义的文档通常会对接收到的输入数据进行相关处理。

### 10、图片\<img>
图片资源由`<img>`标签定义。

* `src`属性的值指向图片的URL地址
* `alt`属性用来为图片定义可替换的文本，当无法载入图片时，使用该文本

### 11、头部\<head>
文档的头部使用`<head>`标签定义，包含关于文档的概要信息，也称为元信息。头部中的元素不会被浏览器显示出来。只有几个标签在HTML的头部是合法的。

	<head>
	  <title>This is the title</title>
	</head>

* `<base>`作为文档中所有链接标签的默认链接
* `<link>`定义了文档与外部资源之间的关系，通常用来链接到样式表
* `<meta>`提供与浏览器或者搜索引擎相关的信息，比如描述文档的内容等
* `<title>`定义了不同文档的标题，包括浏览器的标题、搜索引擎结果页面的标题等，是必须项
* `<style>`用来定义样式
* `<script>`用来加载脚本

### 12、主体\<body>
文档的主体部分使用`<body>`标签定义。

	<body bgcolor="black" background="clouds.gif">

* `bgcolor`属性用来设置背景颜色，属性值可以是十六进制、RGB值或颜色名
* `background`属性用来将图片设置为背景，属性值为图片的URL

tips：应该使用CSS来定义HTML元素的布局和显示属性。

### 13、脚本\<script>
脚本使用`<script>`标签进行定义，可以使用`type`属性来指定脚本语言。

	<script type="text/javascript">

### 14、事件
浏览器内置有大量的事件处理器，这些处理器会监视特定的条件或用户行为。将某些特定的事件处理器作为属性添加给特定的标签，并在事件发生时执行对应的JS命令或函数。事件处理器的值是一系列以分号分隔的JS表达式、方法和函数调用，并用引号引起来。事件分为窗口事件、表单元素事件、图像事件、键盘事件、鼠标事件等。

	<a href="/index.html" onmouseover="alert('Welcome');return false"></a>

### 15、URL
URL（统一资源定位器）用于对万维网上的文档或数据进行寻址。一个完整的网址，遵循一定的语法规则：scheme://host.domain:port/path/filename，各部分定义如下：

* scheme定义因特网服务的类型，如http或https
* host定义域中的主机，缺省为www
* domain定义因特网域名，如google.com
* port定义主机的端口号，缺省为80
* path定义服务器上的路径，如果路径被省略，资源（文档）会被定位到网站的根目录
* filename定义文档的名称
