---
layout: post
title:  "SpringMVC快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/springmvc_icon.jpg)

## 一、概述
SpringMVC属于Spring Framework的后续产品，已经融合到Spring Web Flow中。SpringMVC基于Model2而实现，利用处理器分离了模型对象、视图、控制，达到了松散耦合的效果，提高了系统的可重用性、可维护性以及可扩展性。其功能与Struts类似，只是实现原理和方式上有所不同。

优点：

* 使用简单，学习成本低
* 功能强大，很容易写出性能优秀的程序
* 十分灵活，并且可与Spring无缝衔接

## 二、框架
SpringMVC框架主要由DispatcherServlet、HandlerMapping、Controller、ViewResolver四个接口组成。

### 1、前端控制器DispatcherServlet
是整个SpringMVC的核心。从名称来看，它是一个Servlet，负责统一分发所有请求。

* 拦截符合特定格式的URL请求
* 初始化DispatcherServlet上下文对应的WebApplicationContext，并与业务层、持久化层建立联系
* 初始化SpringMVC的各个组件，并装配到DispatcherServlet中

在web.xml文件中进行配置，负责接收HTTP请求、组织协调SpringMVC的各个组成部分。

	<servlet>
	  <servlet-name>springMVC</servlet-name>
	  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	  <init-param>
	    <param-name>contextConfigLocation</param-name>
	    <param-value>classpath*:/springMVC.xml</param-value>
	  </init-param>
	  <load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
	  <servlet-name>springMVC</servlet-name>
	  <url-pattern>/</url-pattern>
	</servlet-mapping>

如果不指定\<param-value>的值，则默认配置文件为/WEB-INF/\<servlet-name>-servlet.xml。\<load-on-startup>是启动顺序，通常让Servlet跟随Servlet容器一起启动。\<url-pattern>定义要拦截的URL请求。

拦截规则：

* `*.xxx`，指定要拦截的特定类型，最简单实用的方式，并且不会拦截静态文件
* `/`，使用REST风格进行拦截，但是会导致静态文件被拦截不能正常显示
* `/*`，不能像Struts那样使用，会导致不能访问jsp

如果使用`/`进行拦截，并且希望正常访问静态文件，可以在DispatcherServlet之前，使用DefaultServlet先拦截特定类型的请求（如：\*.js、\*.css等）。

### 2、处理器映射HandlerMapping
负责完成请求到控制器的映射。在servlet的配置文件中，进行uri与控制器的映射。同时，还可以对控制器进行拦截。

SpringMVC默认的处理器映射，直接将uri与实现类进行绑定，书写方便，但是耦合性高。

	<bean id="defaultHandlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
	<bean name="/hello.html" class="com.demo.ssm.HelloController"></bean>

使用SimpleUrlHandlerMapping，将uri与类的id进行绑定，彼此的耦合性低，更加灵活。

	<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	  <property name="urlMap">
	    <map>
	      <entry key="/hello.html" value-ref="hello"></entry>
	    </map>
	  </property>
	</bean>
	<bean id="hello" class="com.demo.ssm.controller.HelloController"></bean>

对控制器进行拦截，首先声明拦截器：

	<bean id="myInterceptor" class="com.demo.ssm.interceptor.MyInterceptor"></bean>

然后利用SimpleUrlHandlerMapping，映射拦截器与控制器：

	<property name="interceptors">
	  <list>
	    <ref bean="myInterceptor" />
	  </list>
	</property>

### 3、控制器Controller
负责处理用户请求，完成之后返回ModelAndView对象给前端控制器。因为需要考虑并发，所以必须保证线程安全并且可重用。

SpringMVC中的Controller与Struts中的Action基本相同。通过实现Controller接口或继承父类的方式编写控制器。

实现Controller接口：

	public class HelloController implements Controller {
	  // 相当于servlet的doGet和doPost方法
	  public ModelAndView handleRequest(HttpServletRequest request,HttpServletResponse response) throws Exception {
	    // 接收数据
	    // 调用服务层
	    return new ModelAndView("success","username","sean");
	  }
	}

继承AbstractController类，该类与接口类似，需要重写里边的方法。

继承MultiActionController类，可以实现多个方法，处理多个请求。

	public class MultiController extends MultiActionController {
	  // 自定义处理请求的方法
	  public ModelAndView insert(HttpServletRequest request,HttpServletResponse response) throws Exception {
	    return new ModelAndView("insertSuccess");
	  }
	  public ModelAndView update(HttpServletRequest request,HttpServletResponse response) throws Exception {
	    return new ModelAndView("updateSuccess");
	  }
	}

相应的配置文件：

	<bean id="multi" class="com.demo.ssm.controller.MultiController"></bean>

	<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	  <property name="urlMap">
	    <map>
	      <!-- 使用通配符进行模糊匹配 -->
	      <entry key="/multi-*.html" value-ref="multi"></entry>
	    </map>
	  </property>
	</bean>

	<bean id="multiActionController" class="org.springframework.web.servlet.mvc.multiaction.MultiActionController">
	  <property name="methodNameResolver" ref="methodNameResolver"></property>
	  <property name="delegate">
	    <ref bean="multi" />
	  </property>
	</bean>

	<bean id="methodNameResolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
	  <property name="mappings">
	    <props>
	      <prop key="/multi-insert.html">insert</prop>
	      <prop key="/multi-update.html">update</prop>
	    </props>
	  </property>
	</bean>

继承AbstractCommandController类，用于获取页面的参数，将参数封装到指定的对象模型中。

	public class CommandController extends AbstractCommandController {
	  public CommandController() {
	    // User是用于接收请求参数的数据模型类
	    this.setCommandClass(User.class);
	  }

	  protected ModelAndView handle(HttpServletRequest request,HttpServletResponse response,
	    Object command, BindException errors) throws Exception {
	    // command参数是指定的数据模型对象
	    User user = (User)command;
	    return new ModelAndView("command");
	  }
	}

### 4、视图解析器ViewResolver
负责对ModelAndView对象的解析，并查找对应的View对象。SpringMVC框架默认通过转发进行页面跳转，如果想通过重定向的方式进行跳转，有以下几种实现方式。

在控制器中，返回一个RedirectView对象，由对象来指定重定向的URL：

	View view = new RedirectView("/index.jsp");
	return new ModelAndView(view);

通过配置文件设置，在控制器返回对象的方法中只需要设置一个与bean id一致的字符串即可：

	<bean id="viewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver"></bean>
	<bean id="index" class="org.springframework.web.servlet.view.RedirectView">
	  <property name="url" value="/index.jsp"></property>
	</bean>

	return new ModelAndView("index");

直接跳转：

	return "redirect:/index.jsp";

如果一个配置文件中出现多个视图解析器，可以通过设置order属性来设定优先级。值越低，优先级越高。

## 三、工作原理

1. 将客户端请求提交给DispatcherServlet
2. 根据\<servlet-name>servlet.xml的配置，查找HandlerMapping
3. 通过HandlerMapping找到处理请求的具体Controller
4. Controller调用业务逻辑处理
5. 处理完成之后，返回ModelAndView对象给DispatcherServlet
6. 通过ViewResolver找到负责显示的具体View
7. 由View将结果渲染到客户端

## 四、常用注解

* @Controller：声明Action组件，负责注册bean到Spring上下文
* @RequestMapping：用于为控制器指定可以处理的url请求
* @RequestParam：用于指定参数的name属性
* @RequestBody：用于读取Request请求的body部分数据
* @ResponseBody：用于将控制器方法返回的对象写入到Response对象的body数据区
* @PathVariable：用于指定url作为参数
* @Resource用于注入，( 由j2ee提供 ) 默认按名称装配
* @Autowired用于注入，(由spring提供) 默认按类型装配
* @ExceptionHandler：用于异常处理的方法
* @ControllerAdvice：用于使控制器成为全局的异常处理类
* @ModelAttribute：用于优先调用被注解的方法，或注解参数中的隐藏对象

## 五、拦截器
Spring提供了HandlerInterceptor接口和HandlerInterceptorAdapter适配器。实现这个接口或继承此类，就可以实现自己的拦截器。接口HandlerInterceptor包含三个方法，每个方法的参数handler，用来指向下一个拦截器。

* preHandle()，在Action之前执行的预处理，可以进行编码、安全控制等处理
* postHandle()，在生成View之前执行的后处理，调用了Service并返回ModelAndView，但未进行页面渲染，可以修改ModelAndView
* afterCompletion()，最后执行的返回处理，这时已经进行了页面渲染，可以进行日志记录、释放资源等处理

在实现了接口之后，就可以在配置文件中进行拦截器的配置。

拦截所有url：

	<mvc:interceptors>
	  <bean class="com.core.mvc.MyInteceptor" />
	</mvc:interceptors>

拦截匹配的url：

	<mvc:interceptors>
	  <mvc:interceptor>
	    <mvc:mapping path="/user/*" />
	    <bean class="com.core.mvc.MyInteceptor" />
	  </mvc:interceptor>
	</mvc:interceptors>

HandlerMapping上的拦截器：

	<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping">
	  <property name="interceptors">
	    <list>
	      <bean class="com.core.mvc.MyInteceptor" />
	    </list>
	  </property>
	</bean>

注意：如果配置了\<mvc:annotation-driven />，会自动注册DefaultAnnotationHandlerMapping与AnnotationMethodHandlerAdapter这两个bean。这样就不能再注入interceptors属性，也就无法指定拦截器了。

## 六、异常处理
可以在配置文件中设置SimpleMappingExceptionResolver，也可以实现HandlerExceptionResolver接口，编写自己的异常处理。通过exceptionMappings属性的配置，可以将不同的异常映射到不同的页面。通过defaultErrorView属性的配置，可以为所有异常指定一个默认的异常处理页面。

	<bean id="exceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
	  <property name="defaultErrorView">
	    <value>/error/error</value>
	  </property>
	  <property name="defaultStatusCode">
	    <value>500</value>
	  </property>
	</bean>

## 七、获取Spring管理的Bean
首先在配置文件中添加：

	<bean class="com.xxxx.SpringContextHolder" lazy-init="false" />

然后定义一个Spring上下文持有类：

	import org.springframework.context.ApplicationContext;
	import org.springframework.context.ApplicationContextAware;

	public class SpringContextHolder implements ApplicationContextAware {
	  // 声明静态变量, 保证在任何代码任何地方任何时候能够取得ApplicaitonContext
	  private static ApplicationContext applicationContext;

	  // 实现ApplicationContextAware接口的context注入函数, 将其存入静态变量
	  public void setApplicationContext(ApplicationContext applicationContext) {
	    SpringContextHolder.applicationContext = applicationContext;
	  }

	  // 取得ApplicationContext
	  public static ApplicationContext getApplicationContext() {
	    return applicationContext;
	  }

	  // 从静态变量ApplicationContext中取得Bean, 自动转型为所赋值对象的类型
	  @SuppressWarnings("unchecked")
	  public static <T> T getBean(String name) {
	    return (T) applicationContext.getBean(name);
	  }

	  @SuppressWarnings("unchecked")
	  public static <T> T getBean(Class<T> clazz) {
	    return (T) applicationContext.getBeansOfType(clazz);
	  }

	  // 清除applicationContext静态变量
	  public static void cleanApplicationContext() {
	    applicationContext = null;
	  }
	}

---

参考文章：

[SpringMVC教程](http://elf8848.iteye.com/blog/875830)
