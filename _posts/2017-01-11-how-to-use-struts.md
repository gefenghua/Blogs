---
layout: post
title:  "Struts快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/struts_icon.jpg)

## 一、概述
Struts是Apache基于Model2模型（MVC设计模式）开发的一个开源的Web应用框架，由一组相互协作的类或组件、Servlet以及jsp标签库组成。

Struts采用拦截器的机制来处理用户的请求，也使得业务逻辑控制器能够与Servlet完全脱离，大大缩减了使用MVC模式开发web应用的时间，降低了程序的复杂度，提高了开发效率。

Struts具有如下一些特点：

* 提供了拦截器，利用拦截器可以进行AOP（面向方面）编程，实现如权限拦截等功能
* 提供了类型转换器，可以把特殊的请求参数转化成需要的类型
* 提供支持多种表现层技术，如：JSP、FreeMarker等
* 输入校验可以对指定的方法进行校验
* 提供了全局范围、包范围和Action范围的国际化资源文件管理实现

[官网地址](http://struts.apache.org/)

## 二、原理

### 1、核心控制器FilterDispatcher
FilterDispatcher是Struts框架的核心和基础，包含了框架内部的控制流程和处理机制。该控制器作为一个Filter运行在Web应用中，负责拦截web.xml文件中<url-pattern>指定的用户请求。如果请求以action结尾，该请求将被转入Struts框架处理，否则将略过该请求。Struts框架获得请求后，将根据请求的前面部分决定调用哪个业务逻辑组件。

### 2、业务控制器Action
Struts应用中的Action都被定义在struts.xml文件中，在该文件中定义了Action的name属性和class属性。其中name属性决定了该Action处理哪个请求，class属性决定了该Action的实现类。Struts用于处理用户请求的Action实例，并不是用户所实现的业务控制器，而是Action代理。Struts框架提供了拦截器，负责将HttpServletRequest请求中的参数解析出来，传入到Action中，并回调Action的execute方法来处理用户请求。

### 3、业务逻辑组件
用于实现具体的业务逻辑。

## 三、配置
1. 从Struts官网下载相应版本的压缩文件，解压缩
2. 将需要的jar包拷贝到工程WEB-INF/lib目录下
3. 配置struts.xml文件，添加Action的相关配置
4. 配置web.xml文件，添加启动Struts的过滤器

## 四、工作流程
1. 客户端浏览器发送HTTP请求
2. Servlet容器通过web.xml映射请求，获得控制器的名字，并调用控制器FilterDispatcher（2.1版本以前）或StrutsPrepareAndExecuteFilter（2.1版本以后）
3. 控制器通过ActionMapper获得Action的信息，并调用ActionProxy
4. ActionProxy通过ConfigurationManager读取struts.xml文件获取action和interceptor信息，并把请求传递给ActionInvocation
5. ActionInvocation依次调用action和interceptor，根据action的配置信息，产生result
6. 将Result信息返回给ActionInvocation
7. 生成HttpServletResponse响应，并将其发送给客户端

## 五、拦截器
Struts2框架的绝大部分功能是通过拦截器来完成的。当FilterDispatcher拦截到用户请求后，对用户请求进行处理，然后调用用户自定义的Action类中的方法来处理请求。

在struts.xml文件中，使用\<interceptor>元素来进行定义：

	<interceptor name="拦截器名" class="拦截器实现的类">
	    <param name="参数名">参数值</param>
	</interceptor>

有时一个Action要配置不止一个拦截器，这时就要配置多个拦截器组成的拦截器栈：

	<interceptor-stack name="拦截器栈名">
	    <interceptor-ref name="拦截器一"></interceptor-ref>
	    <interceptor-ref name="拦截器二"></interceptor-ref>
	    <interceptor-ref name="拦截器三"></interceptor-ref>
	</interceptor-stack>

Tips：每个包中只能有一个默认的拦截器，一旦为包中的某个action指定了拦截器，则默认的拦截器将不起作用。

Struts提供了拦截器接口Interceptor，其中包含三个方法：

* init()，初始化要使用的资源
* intercept(ActionInvocation invocation)，实现拦截的动作
* destroy()，销毁在init方法中打开的资源

## 六、校验
可以通过两种方式进行校验：实现validate方法和通过validation配置文件。

### 1、实现validate方法
在Validateable接口中定义了一个validate()方法，在用户自定义的Action类中重写该方法就可以实现校验功能。当数据校验发生错误时，调用addFieldError()方法向相同的fieldErrors添加校验错误信息。

	public void validate() {//会对action中的所有方法进行校验

	    if(this.username == null || this.username.trim().equals("")) {
	        this.addFieldError("username", "用户名不能为空！");
	    }

	    if(this.mobile == null || this.mobile.trim().equals("")) {
	        this.addFieldError("mobile", "手机号不能为空！");
	    } else {
	        if(!Pattern.compile("^1[358]\\d{9}{1}quot;).matcher(this.mobile).matches()) {
	            this.addFieldError("mobile", "手机号格式不正确！");
	        }
	    }
	}

如果系统的fieldErrors包含错误信息，Struts会将请求结果发送到名为input的result。在Action的配置中添加对input的响应并指定要跳转的页面：

	<action name="LoginAction" class="com.login.LoginAction">
	    <!-- 定义逻辑视图和物理资源之间的映射 -->
	    <result name="input">/login.jsp</result>
	    <result name="error">/error.jsp</result>
	    <result name="success">/main.jsp</result>
	</action>

result的name属性有如下几种方式：

* success：表示请求处理成功，默认值
* error：表示请求处理失败
* none：表示请求处理完成后不跳转
* input：表示输入时如果验证失败应跳转何处
* login：表示登录失败后跳转何处

在input映射的页面中通过<s:fielderror/>显示错误信息：

	<s:fielderror></s:fielderror>
	<form method="post" action="<%=basePath%>person/save.action">
	    用户名:<input type="text" name="username"/>不能为空<br/>
	    手机号:<input type="text" name="mobile"/>不能为空，并且要符合手机号的格式<br/>
	    <input type="submit" value="提交"/>
	</form>

### 2、通过validation配置
另一种方式是提供校验文件，校验文件和action类在同一个包下，文件命名规则是ActionClassName-validation.xml。在校验文件中，使用数据校验器来对表单进行校验。

	<validators>
	    <field name="username">
	        <field-validator type="requiredstring">
	            <param name="trim">true</param>
	            <message>用户名不能为空！</message>
	        </field-validator>
	    </field>
	    <field name="mobile">
	        <field-validator type="requiredstring">
	            <param name="trim">true</param>
	            <message>手机号不能为空！</message>
	        </field-validator>
	        <field-validator type="regex">
	            <param name="expression"><![CDATA[^1[358]\d{9}$]]></param>
	            <message>手机格式不正确！</message>
	        </field-validator>
	    </field>
	</validators>

Struts提供了大量的数据校验器，包括表单校验器和非表单校验器：

* 必填校验器，类型为required
* 整数校验器，类型为int
* 日期校验器，类型为date
* 邮件地址校验器，类型为email
* 网址校验器，类型为url
* 必填字符串校验器，类型为requiredstring
* 字符串长度校验器，类型为stringlength
* 正则表达式校验器，类型为regex

## 七、标签
按照标签的功能大致可以分为两类：UI标签和非UI标签。要想使用Struts标签，需要在使用标签的jsp页面引入标签库。

	<%@ taglib uri="/struts-tags" prefix="s" %>

### 1、UI标签
主要用于生成HTML元素，可分为表单标签和非表单标签。

表单标签在语法上与普通的HTML元素标签类似，在HTML元素前加上“s:”前缀即可。在用法上也基本与普通元素相同。通常情况下，会使用与UI有关的框架来代替这部分标签。

* \<s:form>表单标签，通常会使用action属性指定Action类
* \<s:submit>、\<s:reset>用来提交/重置表单
* \<s:hidden>用来表示表单的隐藏域

非表单标签主要用于生成非表单的可视化元素，如：Tab页、树形结构等，也可以用于自定义的组件。还可以用来显示Action的信息。

* \<s:component>用来表示自定义组件
* \<s:fielderror>用来输出表单域的错误信息
* \<s:actionerror>用来输出getActionErrors()方法返回的非空信息
* \<s:actionmessage>用来输出getActionMessage()方法返回的信息

### 2、非UI标签
主要用于逻辑控制、数据访问等，可分为控制标签和数据访问标签。

控制标签包括：

* \<s:if>、\<s:elseif>、\<s:else>用于控制选择输出
* \<s:append>用于将多个集合拼成一个新的集合，按照集合的排列顺序拼接
* \<s:generator>用于将一个字符串解析成一个集合
* \<s:iterator>用于将集合中的元素迭代输出
* \<s:merge>用于将多个集合拼成一个新的集合，按照各集合中元素的顺序拼接
* \<s:sort>用于对集合进行排序
* \<s:subset>用于截取集合生成新的子集合

数据访问标签包括：

* \<s:action>允许在jsp页面中直接使用Action
* \<s:bean>用来实例化一个JavaBean
* \<s:date>用来格式化输出日期
* \<s:property>用来输出对象的属性值
* \<s:url>用来创建一个URL地址
* \<s:param>用来为其他标签提供参数

---

参考资料：

[Struts2工作流程](http://blog.csdn.net/wuwenxiang91322/article/details/11070513)
