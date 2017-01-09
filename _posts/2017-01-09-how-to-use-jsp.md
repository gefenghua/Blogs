---
layout: post
title:  "JSP快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/jsp_icon.jpg)

## 一、概述
JSP是Java Server Pages的简称，是一种动态网页开发技术，运行在服务端。以Java语言作为脚本语言，使用JSP标签在HTML网页中插入Java代码。

JSP是一种Java servlet，主要用于实现web应用程序的用户界面部分。可以响应客户端请求，动态生成HTML、XML或其他格式文档的web网页。

* 可以直接在HTML网页中动态嵌入元素，性能更加优越
* 服务器调用的是已经编译好的JSP文件
* 拥有强大的企业级Java API
* JSP页面可以与处理业务逻辑的servlet一起使用
* 编写和修改HTML网页比较容易，不需要在servlet中使用大量的输出语句
* JSP开发的web应用可以跨平台使用

## 二、处理流程

1. 浏览器发送一个HTTP请求
2. web服务器将请求传递给JSP引擎
3. JSP引擎将JSP文件转化成servlet
4. JSP引擎将servlet编译成可执行类，并将请求传递给servlet引擎
5. servlet引擎载入并执行servlet类，生成HTML格式的输出并嵌入到HTTP响应中，上传给web服务器
6. web服务器以静态HTML网页的形式将HTTP响应返回到浏览器

## 三、生命周期
JSP的生命周期就是从创建到销毁的整个过程。与servlet的生命周期类似，只是还包括将JSP文件编译成servlet的阶段。

### 1、编译阶段
当浏览器请求JSP页面时，JSP引擎首先会检查是否需要编译这个JSP文件。只有在该文件从没有被编译过，或在上次编译之后有过变更，才会编译该文件。

编译过程包括三个步骤：

* 解析JSP文件
* 将JSP文件转化成servlet
* 编译servlet

### 2、初始化阶段
加载编译生成的servlet类，调用jspInit()方法进行初始化，如建立数据库连接或打开文件等。和servlet一样，程序一般只初始化一次。

	public void jspInit() {
	    // 初始化代码
	}

### 3、执行阶段
调用_jspService()方法，处理与请求相关的交互行为。该方法在每个请求中被调用一次并产生与之对应的响应。

	void _jspService(HttpServletRequest request, HttpServletResponse response) {
	    // 服务端处理代码
	}

### 4、销毁阶段
调用jspDestroy()方法执行清理工作，如释放数据库连接或关闭文件等。

	public void jspDestroy() {
	    // 清理代码
	}

## 四、基本语法

### 1、脚本程序
脚本程序包含任意的Java语句、变量、方法或表达式。任何文本、HTML标签、JSP元素必须写在脚本程序的外面。

	<% %>

### 2、JSP声明
声明语句可以声明任意变量、方法，供后面的Java代码使用。

	<%! declaration; %>

### 3、JSP表达式
表达式的值先被转化成String，再将其插入到表达式所在的地方。

	<%= expression %>

### 4、JSP注释
可以为代码行注释，也可以为代码块注释。

	<%-- annotation --%>

### 5、JSP指令
用来设置与整个JSP页面相关的属性。JSP有三种指令标签：page、include和taglib。

#### 5.1 page指令
定义页面的依赖属性，提供当前页面的使用说明，如脚本语言、error页面、缓存等。

	<%@ page attribute="value" %>

与page指令相关的属性：

* buffer，指定out对象使用的缓冲区大小
* autoFlush，控制out对象的缓冲区
* contentType，指定当前页面的MIME类型和字符编码
* errorPage，指定当页面发生异常时转向的错误处理页面
* isErrorPage，指定当前页面是否可以作为其他页面的错误处理页面
* extends，指定servlet继承的类
* import，导入使用的Java类
* info，定义页面的描述信息
* language，定义页面使用的脚本语言，默认是Java
* session，指定页面是否使用session
* isThreadSafe，指定页面的访问是否为线程安全
* isELIgnored，指定是否执行EL表达式
* isScriptingEnabled，指定脚本元素能否被使用

#### 5.2 include指令
通过该指令来包含其他文件，被包含的文件如同页面的一部分，会被同时编译执行。

	<%@ include file="name" %>

指令中的文件名实际上是一个相对的URL地址，默认是当前路径。

#### 5.3 taglib指令
引入标签库，可以是自定义标签。

	<%@ taglib uri="uri" prefix="prefixOfTag" %>

uri属性确定标签库的位置，prefix属性指定标签库的前缀。

### 6、JSP行为
JSP行为标签是一些预定义的函数，用来控制servlet引擎，实现特殊操作。

	<jsp:action_name attribute="value" />

所有的动作元素都有两个属性：id和scope。id属性是元素的唯一标识，scope属性是元素的生命周期。

#### 6.1 jsp:include元素
在当前页面中包含静态或动态资源。插入文件的时间是在页面被请求的时候。

	<jsp:include page="url" flush="true" />

page属性是被包含的资源的相对URL地址。flush属性是定义在包含资源前是否刷新缓冲区。

#### 6.2 jsp:useBean元素
用来加载一个在页面中使用的JavaBean组件。

	<jsp:useBean id="name" class="package.class" />

class属性指定Bean的完整包名。type属性指定引用该对象的变量类型。

#### 6.3 jsp:setProperty元素
用来设置已经实例化的Bean对象的属性。

	<jsp:setProperty name="beanName" property="property" />

name属性表示要设置属性的Bean。property属性表示要设置哪个属性。value属性用来指定Bean属性的值。param属性指定用哪个请求参数来设置Bean属性的值。

如果jsp:setProperty元素位于jsp:useBean元素外部，不管是新创建一个Bean实例，还是已有一个Bean实例，都会执行。如果jsp:setProperty元素位于jsp:useBean元素内部，只有在新创建一个Bean实例时才会执行。

#### 6.4 jsp:getProperty元素
获取指定Bean对象的属性值。

	<jsp:getProperty name="beanName" property="property" />

name属性是要检索的Bean的名称。property属性表示要获取的属性。

#### 6.5 jsp:forward元素
将请求转发给其他页面。

	<jsp:forward page="url" />

page属性是一个相对URL。

#### 6.6 jsp:plugin元素
用于在生成的HTML页面中包含JavaBean对象。

#### 6.7 jsp:element、jsp:attribute、jsp:body元素
定义动态的XML元素、元素的属性以及元素的内容。

#### 6.8 JSP:text元素
允许在JSP页面和文档中使用写入文本的模板，该模板只能包含文本和EL表达式。

	<jsp:text>template</jsp:text>

### 7、JSP隐含对象
JSP包含九个预定义的对象，对应servlet中的类：

* request，是HttpServletRequest类的实例
* response，是HttpServletResponse类的实例
* session，是HttpSession类的实例
* application，是ServletContext类的实例
* out，是PrintWriter类的实例，用于把结果输出到网页上
* config，是ServletConfig的实例
* pageContext，是PageContext类的实例，提供对页面所有对象的访问
* Exception，是Exception类的实例
* page，类似于Java类中的this关键字

## 五、表单处理
在浏览网页的时候，经常需要向服务器提交信息，由后台程序进行处理。

### 1、GET和POST方法
GET方法是浏览器默认的传递参数的方法。将请求的信息添加到URL后面，之间用“?”分隔，参数之间用“&”分隔。

	http://url?key1=value1&key2=value2

POST提交的数据是不可见的，即不在url显示。

### 2、读取表单数据
JSP读取表单数据有以下几种方法：

* getParameter()，获取表单参数的值
* getParameterValues()，获取名字相同但有多个值的数据，返回数组
* getParameterNames()，获取所有变量的名称，返回枚举
* getInputStream()，获取来自客户端的二进制数据流

## 六、过滤器
过滤器可以动态地拦截请求和响应。可以将一个或多个过滤器附加到一个servlet或一组servlet，也可以附加到JSP文件和HTML页面。

过滤器通过web.xml中的标签声明，然后映射到servlet或url。当web容器启动web应用程序时，会为声明的每一个过滤器创建一个实例。过滤器的执行顺序与配置文件中的配置顺序一致。

过滤器是一个实现javax.servet.Filter接口的类，接口定义了三个方法，分别对应了过滤器生命周期的三个阶段。

初始化阶段：

	public void init(FilterConfig filterConfig)

web应用程序启动时，web服务器会创建Filter的实例对象并调用其init()方法，读取配置文件，完成对象的初始化，从而为拦截请求做好准备。

过滤阶段：

	public void doFilter (ServletRequest, ServletResponse, FilterChain)

当客户端的请求方法与过滤器的设置匹配时，servlet容器会调用过滤器的doFilter()方法，完成过滤操作。参数FilterChain用来访问后续过滤器。

销毁阶段：

	public void destroy()

servlet容器在销毁过滤器实例前调用该方法，释放过滤器占用的资源。

## 七、跳转
按照跳转的处理方式分成两类：客户端跳转和服务器跳转。

### 1、客户端跳转
使用超链接：

	<a href="newPage.jsp">跳转</a>

使用JavaScript脚本：

	<script>
	    function onSubmit() {
	        submit();
	    }
	</script>
	<form name="form1" method="post" action="onSubmit();">
	    <input type="submit">
	</form>

提交表单：

	<form name="form1" method="post" action="newPage.jsp">
	    <input type="text" name="username">
	    <input type="text" name="password">
	    <input type="submit">
	</form>

使用jsp的内置对象response：

	<%
	    response.sendRedirect("http://www.google.cn");
	%>

或者

	<%
	    response.sendHeader("Refresh","1;url=http://www.google.cn");
	%>

### 2、服务器跳转
使用RequestDispatcher类的forward方法。

	RequestDispatcher rd = request.getRequestDispatcher("/newPage.jsp");
	rd.forward(request, response);

### 3、sendRedirect和forward的区别
sendRedirect：

* 执行完所有代码再跳转到目标页面
* 跳转后浏览器的url会改变
* 在浏览器中重定向
* 可以跳转到其他服务器

forward：

* 直接跳转到目标页面，不执行后续代码
* 跳转后url不会改变
* 在服务器端重定向
* 不能跳转到其他服务器

---

参考文章：

[JSP常用跳转方式](http://blog.csdn.net/wanghuan203/article/details/8836326)