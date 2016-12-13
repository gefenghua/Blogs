---
layout: post
title:  "Servlet快速入门"
categories: jekyll update
---

## 一、概述
Servlet（Server Applet），全称Java Servlet，是用Java编写的服务器端程序。主要功能在于交互式地浏览和修改数据，生成动态Web内容。

狭义的Servlet是指Java语言实现的一个接口；广义的Servlet是指任何实现了这个Servlet接口的类。一般情况下将Servlet理解为后者。

Servlet运行于支持Java的应用服务器中，原理上Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器。

提供了Servlet功能的服务器，叫做Servlet容器。Servlet必须部署在Servlet容器中才能使用。常见的容器包括：Tomcat、WebLogic等。

---

## 二、工作流程

1. 客户端发送请求到服务器。
2. 根据web.xml文件的配置，查找<url-pattern>对应的<servlet-mapping>，再通过<servlet-mapping>中的<servlet-name>值，查找对应的<servlet-class>。
3. 服务器启动并调用Servlet，Servlet根据客户端请求生成响应内容并将其传给服务器。
4. 服务器将响应返回给客户端。

---

## 三、生命周期
Servlet没有main()方法，不能独立运行，由Servlet引擎控制和调度。

针对客户端的多次Servlet请求，通常服务器只会在第一次请求的时候创建一个Servlet实例对象，并驻留在内存中，直到Web容器退出时，该实例对象才会被销毁。

1. Web容器调用init()方法初始化Servlet。该方法在Servlet实例的生命周期里只调用一次。可以传递一个实现ServletConfig接口的对象给它，这个对象使Servlet能够读取web.xml文件里使用<init-param>标签定义的初始参数。
2. 调用service()方法处理不同类型的请求。该方法使用HttpServletRequest请求对象和HttpServletResponse响应对象作为参数。根据请求的方式调用适当的方法来处理请求（如doGet或doPost等）。
3. 调用destroy()方法销毁Servlet。该方法在Servlet实例的生命周期里只调用一次。

---

## 四、体系结构
Servlet编程需要用到javax.servlet和javax.servlet.http两个包下面的类和接口。接口javax.servlet.servlet定义了所有servlet必须实现的方法。所有servlet程序必须实现该接口或继承实现了该接口的类。为了实现该接口，可以继承javax.servlet.GenericServlet或javax.servlet.http.HttpServlet类。

接口javax.servlet.servlet定义的方法：

	public void init(ServletConfig config)
	public ServletConfig getServletConfig()
	public String getServletInfo()
	public void service(ServletHttpRequest req, ServletHttpResponse res)
	public void destroy()

主要的类和接口包括：

	javax.servlet.ServletConfig
	javax.servlet.ServletException
	javax.servlet.ServletContext
	javax.servlet.GenericServlet
	javax.servlet.http.HttpServlet
	javax.servlet.http.HttpServletRequest
	javax.servlet.http.HttpServletResponse
	javax.servlet.http.HttpSession
	javax.servlet.http.Cookie

---

## 五、请求和响应
Web服务器会对收到的每一次客户端http请求，分别创建一个用于代表请求的request对象和代表响应的response对象。要获取客户端提交的数据需要通过request，要想容器输出数据需要通过response。

HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在此对象中。为了获取请求参数，需要调用HttpServletRequest对象的getParameter()方法，并且将要获取的输入参数的id传递给该方法。

HttpServletResponse对象代表服务器的响应，该对象封装了向客户端发送数据、发送响应头、发送响应状态码的方法。为了发送内容给客户端，需要使用从HttpServletResponse对象获取的PrintWriter对象，任何写到这个对象的内容都会被写进outputstream里，并会把内容发送到客户端。

为了获取配置文件中的初始化参数，可以调用getServletConfig().getInitParameter("name")方法。

---

## 六、配置
进行Servlet的配置有两种方法：

1. 通过web.xml文件
2. 使用注解@WebServlet(name=””, urlPatterns=””)

---

## 七、监听器
为了创建一个基于容器事件的监听器，必须创建一个实现ServletContextListener接口的类。该类必须实现contextInitialized()和contextDestroyed()两个方法。这两个方法都需要ServletContextEvent作为参数，并且在每次初始化或关闭容器时都会被自动调用。

注册监听器有三种方法：

1. 在web.xml应用部署文件里注册监听器
2. 使用@WebListener注解
3. 使用ServletContext里定义的addListener()方法

常用的监听器类：

	javax.servlet.ServletRequestListener
	javax.servlet.ServletRequestAttributeListener
	javax.servlet.ServletContextListener
	javax.servlet.ServletContextAttributeListener
	javax.servlet.HttpSessionListener
	javax.servlet.HttpSessionAttributeListener

---

## 八、过滤器
Web过滤器在给定的URL被访问时，对请求进行预处理，会在Servlet调用前被调用。很多情况下，使用过滤器是很有用的，如执行日志、验证、或其他不需要与用户交互的后台服务等。

过滤器必须实现javax.servlet.Filter接口，该接口包含了init()、destroy()、doFilter()方法。init和destroy方法会被容器调用，doFilter方法用来在过滤器类里实现逻辑任务。

为了配置过滤器，可以使用@WebFilter注解或在web.xml文件中使用\<filter>和<filter-mapping>注册。

---

## 九、转发和重定向
转发请求时不重定向客户端的URL，即浏览器地址栏的URL不会改变。ServletContext已经内置实现转发的方法，调用getRequestDispatcher()方法获取用来转发请求的对象。调用该方法时，需要传递包含处理转发请求的Servlet名称的字符串。获取RequestDispatcher对象后，传递HttpServletRequest和HttpServletResponse对象给转发方法forward()。

	RequestDispatcher rd = servletContext.getRequestDispatcher("/NextServlet");
	rd.forward(request, response);

当特定URL被访问时，可以把浏览器的URL重定向到另外一个，即URL会改变。需要调用HttpServletResponse对象的sendRedirect()方法，传递重定向后的URL字符串。

	httpServletResponse.sendRedirect("/anotherURL");

---

## 十、Cookie与Session
用户打开浏览器，点击多个超链接，访问服务器多个Web资源，关闭浏览器，整个过程称之为一个会话。保存会话数据有两种方式：

1. Cookie，客户端技术，程序把每个用户的数据以cookie的形式写给用户各自的浏览器，当访问服务器中的Web资源时，会带着用户各自的数据
2. Session，服务器端技术，服务器为每个用户的浏览器创建一个独享的HttpSession对象，当访问服务器的Web资源时，把各自的数据放在各自的session中

Cookie可以看做是保存在客户端的键值对数据，当使用浏览器打开应用时，可以对这些数据进行读写。为了创建cookie，需要实例化一个javax.servlet.http.Cookie对象，并为它分配键值，可以设置属性来配置cookie。

	Cookie cookie = new Cookie("sessionId","123456789");
	cookie.setHttpOnly(true);
	response.addCookie(cookie);

读取服务端的cookie信息：

	Cookie[] cookies = request.getCookies();
    for(Cookie cookie : cookies){
        ......
    }

通过请求获取session对象：

	HttpSession session = request.getSession(boolean create);

如果当前请求不属于任何会话，而且create参数为true，则创建一个会话，否则返回null。如果为false的时候，那就和不带参数的getSession()等价了。

获取session对象之后，就可以直接使用它进行各种操作了。

	// 将value对象以name名称绑定到会话
	public void setAttribute(String name, Object value)
	// 取得name的属性值，如果属性不存在则返回null
	public object getAttribute(String name)
	// 从会话中删除name属性，如果不存在不会执行，也不会抛出错误
	public void removeAttribute(String name)
	// 返回和会话有关的枚举值
	public Enumeration getAttributeNames()
	// 使会话失效，同时删除属性对象
	public void invalidate()
	// 用于检测当前用户是否为新的会话
	public Boolean isNew()
	// 返回会话创建时间
	public long getCreationTime()
	// 返回在会话时间内web容器接收到客户最后发出的请求的时间
	public long getLastAccessedTime()
	// 返回在会话期间内客户请求的最长时间，单位是秒
	public int getMaxInactiveInterval()
	// 允许客户客户请求的最长时间
	public void setMasInactiveInterval(int seconds)
	// 返回当前会话的上下文环境，ServletContext对象可以使Servlet与web容器进行通信
	ServletContext getServletContext()
	// 返回会话期间的识别号
	public String getId()

---

## 十一、上传和下载
### 1、上传文件
首先要将enctype属性的值设置成multipart/form-data，让表单提交的数据以二进制编码的方式提交，在接收此请求的Servlet中用二进制流来获取内容，就可以取得上传文件的内容，从而实现文件的上传。

### 2、下载文件
通过HttpServletResponse.setContentType方法设置响应类型Content-Type头字段的值，让浏览器无法使用某种方式或激活某个程序来处理的MIME类型。

	response.setContentType("application/x-msdownload");

通过HttpServletResponse.setHeader方法设置响应头Content-Disposition的值。

	response.setHeader("Content-Disposition", "attachment;filename=\"" + filename + "\"");

调用HttpServletRespnse.getOutputStream方法返回的ServletOutputStream对象来向客户端写入附件文件的内容。

	InputStream inputStream = new FileInputStream(file);
	ServletOutputStream ouputStream = response.getOutputStream();
	byte b[] = new byte[1024];
	while((n = inputStream.read(b)) != -1) {
	    ouputStream.write(b, 0, n);
	}

---

参考资料：

[用JSP+Servlet实现文件的上传与下载](http://blog.csdn.net/weixian52034/article/details/52133543)