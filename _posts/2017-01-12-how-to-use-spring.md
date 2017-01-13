---
layout: post
title:  "Spring快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/spring_icon.jpg)

## 一、概述
Spring是J2EE应用程序框架，是轻量级的IoC（控制反转）和AOP（面向切面）容器框架，主要针对javaBean的生命周期进行管理。使用基本的JavaBean代替了传统的EJB，降低了企业应用开发的复杂性。借助依赖注入、AOP应用、面向接口编程等特性，降低了业务组件之间的耦合度，增强了系统的可扩展性。Spring是非侵入式的，应用中的对象不依赖于Spring的特定类。可以整合和兼容其他框架，让已有的技术和框架更加易用。

[官网地址](http://spring.io/)

## 二、框架结构
Spring框架是一个分层架构，由七个模块组成。Spring的各模块构建在核心容器之上，核心容器定义了创建、配置和管理bean的方式。

1. Spring Core

	提供了Spring框架的基本功能。核心容器的主要组件是BeanFactory，是工厂模式的实现。BeanFactory使用IoC将应用程序的配置和依赖性规范与实际的应用程序代码分离。

2. Spring Context

	是一个配置文件，为Spring框架提供上下文信息。Spring上下文包括各种企业服务，如JNDI、EJB、电子邮件、国际化、校验和调度等。

3. Spring AOP

	通过配置管理特性，该模块将面向方面的编程功能集成到了Spring框架中。为基于Spring的应用程序中的对象提供了事务管理服务，将声明性事务管理集成到应用程序中。

4. Spring DAO

	提供了有意义的异常层次结构，可用该结构来管理异常处理和错误信息。异常层次结构简化了错误处理，极大地降低了需要编写的异常代码量。

5. Spring ORM

	Spring框架插入了若干ORM框架，从而提供了ORM的对象关系工具。所有这些框架都遵从Spring的通用事务和DAO异常层次结构。

6. Spring Web

	Web上下文模块建立在应用程序上下文模块之上，为基于Web的应用程序提供了上下文。Web模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。

7. Spring MVC

	SpringMVC框架是一个全功能的构建Web应用程序的MVC实现。

## 三、核心组件
Spring框架的核心组件主要有三个：Bean组件、Context组件、Core组件。

### 1、Bean组件
Bean组件在Spring的org.springframework.beans包下，这个包下的所有类主要负责bean的定义、创建、以及对bean的解析。在实际使用中只需要关注bean的创建即可，bean的定义和bean的解析由框架完成。

* bean的创建使用工厂模式，顶层接口是BeanFactory，最终的实现类是DefaultListableBeanFactory。BeanFactory有三个子接口：ListableBeanFactory、HierarchicalBeanFactory、AutowireCapableBeanFactory。这四个接口共同定义了bean的集合、bean之间的关系、以及bean的行为。
* bean的定义主要由BeanDefinition描述，描述了配置文件中所定义的\<bean>节点中的所有信息。当Spring成功解析节点后，节点信息被转化成BeanDefinition对象，之后所有的操作都是针对该对象。
* bean的解析主要就是对Spring配置文件的解析。

### 2、Context组件
Context组件在Spring的org.springframework.context包下，提供一个运行时环境，用以保存各个对象的状态。ApplicationContext是Context的顶级父类，标识了一个应用环境的基本信息。还继承了五个接口，主要扩展Context的功能。

ApplicationContext主要有两个子类：

* ConfigurableApplicationContext，表示该Context是可修改的，用户在构建Context时可以动态添加或修改已有的配置信息。
* WebApplicationContext，表示是为web准备的Context，可以直接访问ServletContext。

ApplicationContext必须完成以下几件事：

* 标识一个应用环境
* 利用BeanFactory创建Bean对象
* 保存对象关系表
* 能够捕获各种事件

### 3、Core组件
Core组件包含了很多关键类，其中一个重要部分就是定义了资源的访问方式。Resource接口封装了各种可能的资源类型，对使用者屏蔽了文件类型的不同。Resource接口继承了InputStreamSource接口，接口中的getInputStream方法返回InputStream类，所有的资源都可以通过这个类来获取，也屏蔽了资源的提供者。ResourceLoader接口屏蔽了资源的加载者，只需要实现这个接口就可以加载所有类型的资源。Context把资源的加载、解析和描述工作委托给ResourcePatternResolver类来完成，由它整合在一起供其他组件使用。

## 四、原理

### 1、IoC
IoC是Spring的核心之一。传统情况下，当一个对象依赖于另一个对象时，需要通过new操作主动创建另一个对象的实例，这样的话，两个对象之间就产生了耦合。通过IoC，一个对象依赖的其他对象会通过被动的方式传递进来，而不是自己创建或查找依赖对象。创建依赖对象实例的工作通常由Spring容器来完成，然后注入给依赖者，所以也被称为DI（依赖注入）。

### 1.1 IoC容器
BeanFactory是IoC容器的核心接口，负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。XMLBeanFactory实现BeanFactory接口，通过获取xml配置文件数据，组成应用对象及对象间的依赖关系。

* BeanFactoryPostProcessor，在构建BeanFactory时调用
* BeanPostProcessor，在构建Bean对象时调用
* InitializingBean，在Bean实例创建时调用
* DisposableBean，在Bean实例销毁时调用
* FactoryBean，用来创建Bean实例的Bean

### 1.2 注入方法
可以通过三种方法进行注入：set方法、构造函数、接口。

**set方法注入**

可以为属性提供set方法。在配置文件中添加bean标签，属性class的值为对象实现类的完整路径。添加property标签，属性name的值与对象类中对应的属性名称一致，属性value的值就是给对象类中对应的属性注入的值。

实现类：

	public class User {
	    private String username;

	    public String getUsername() {
	        return username;
	    }
	    public void setUsername(String username) {
	        this.username= username;
	    }
	}

配置文件：

	<bean id="userAction" class="com.spring.action.User" >  
	    <property name="username" value="admin"></property>  
	</bean>  

也可以为对象提供set方法。在配置文件中添加对引用对象的bean声明，属性ref的值是所要引用的对象。这样，框架就会将引用对象注入到对象类中。

实现类：

	public class UserAction {
	    private UserService userservice;

	    public void setUserService(UserService userservice) {
	        this.userservice = userservice;
	    }
	}

配置文件：

	<bean id="userService" class="com.spring.service.UserService"></bean>
	<bean id="userAction" class="com.spring.action.UserAction" >
	    <property name="userservice" ref="userService"></property>
	</bean>

**构造方法注入**

直接通过构造函数，将要依赖的属性或对象作为参数传递给构造函数。

实现类：

	public class UserAction {
	    private String username;

	    public UserAction(String username) {
	        this.username= username;
	    }
	}

配置文件：

	<bean id="userAction" class="com.spring.action.UserAction" >
	    <constructor-arg constructor-argvalue="admin"></constructor-arg>
	</bean>

**接口方法注入**

并不常用，所以不做介绍。

### 1.3 自动装配
修改Spring配置文件中bean标签的autowire属性，可以进行自动装配，简化配置，提高开发效率。自动装配属性有六个值，分别代表不同的含义：

* byName

	从Spring环境中获取目标对象时，目标对象中的属性会根据名称在整个Spring环境中查找bean标签的id属性值。如果有相同的，则获取这个对象，实现关联。要保证id不能重复。

* byType

	从Spring环境中获取目标对象时，目标对象中的属性会根据名称在整个Spring环境中查找bean标签的class属性值。如果有相同的，则获取这个对象，实现关联。当存在多个相同类型的bean对象时，会出错。

* constructor

	使用构造方法完成对象注入，也就是根据构造方法的参数类型进行对象查找，相对于采用byType方式。

* autodetect

	如果对象没有无参数的构造方法，则自动选择constructor的装配方式进行构造注入；如果对象含有无参数的构造方法，则自动选择byType的装配方式进行set注入。

* default

	默认采用上一级标签的自动装配的值。

* no

	不进行自动装配。

### 2、AOP
AOP是Spring的另一个核心。Spring通过AOP，允许分离应用的业务逻辑与系统级服务（如日志、事务、权限等），从而提高了内聚性。

#### 2.1 AOP与OOP
AOP即面向切面编程，与OOP面向对象编程相辅相成。OOP要求职责分离，不同的类完成不同的功能，降低了代码的复杂度，便于重用。但在职责分离的同时，如果多个类需要进行相同操作时，需要在每个类中都加入这些内容，增加了代码的重复性。如果将这些内容单独封装在一个类中，再通过每个类进行调用，会增加类之间的耦合度。AOP就是在程序运行时，动态地将这些内容切入到类的指定方法、指定位置上。OOP中，基本单元是类；AOP中，基本单元是切面。OOP横向分成各个类，AOP纵向给对象加入特定代码。

#### 2.2 AOP术语
AOP中包含很多术语，理解这些术语才能全面的理解AOP的真正含义。

* 切面（Aspect），横切面的功能，用于抽象出类或接口

	由Advice和Pointcut组成，既包含了横切面的定义，也包含了切入点的定义。可以使用@Aspect注解实现。

* 增强（Advice），横切面功能的具体实现，对象的实例

	由Aspect添加到特定Joinpoint的代码，

* 连接点（Joinpoint），程序运行期间的时间节点

	在Spring AOP中，Joinpoint总是表现为方法执行。

* 切入点（Pointcut），描述横切面功能应用的限制

	使用Pointcut的目的就是提供一组规则来匹配Joinpoint，只为满足条件的Joinpoint添加Advice代码。

* 代理（Proxy），织入Advice的结果类

	Spring AOP默认使用JDK动态代理为接口实现代理。如果业务逻辑对象没有实现接口，则使用CGLIB代理。Spring AOP建议基于接口编程，对接口进行AOP，而不是基于类。

* 目标（Target），业务操作的实际对象，织入Advice的目标对象，也被称为被增强的对象

	因为Spring AOP是通过运行时代理的方式实现Aspect的，所以目标对象总是一个代理对象。一个类被AOP织入Advice后，会产生一个结果类，是包含原类和增强逻辑的代理类。

* 织入（Weave），将组件应用到业务流程中的过程，将Aspect和其他对象关联并创建目标对象的过程

	Spring采用动态代理织入，而AspectJ则采用编译器和类装载器织入。

#### 2.3 用法

**定义Aspect**

当使用注解@Aspect标注一个Bean后，Spring框架会自动将其添加到Spring AOP中。要想将对象转换成Bean，还需要使用@Component之类的注解。如果一个类被@Aspect标注，那么这个类就不能成为其他Aspect的目标对象了。

	@Component
	@Aspect
	public class Test{
	}

**声明Pointcut**

Pointcut声明由两部分组成：一个pointcut表达式，一个方法签名。使用注解@Pointcut标注pointcut表达式。

	@Pointcut("execution(* com.service.UserService.*(..))")
	private void dataAccessOperation() {}

该声明描述的是：匹配com.service.UserService下的所有方法的执行。

pointcut表达式由指示器和操作参数组成。指示器有以下几种：

* execution，匹配joinpoint的执行
* within，匹配特定类下的所有joinpoint的执行
* this，匹配一个bean，该bean是一个给定类型的实例
* target，匹配一个目标对象，该对象是一个给定类型的实例
* bean，匹配指定bean下的所有方法
* args，匹配参数满足要求的方法
* @annotation，匹配由指定注解所标注的方法

**声明Advice**

Advice通常是和Pointcut表达式关联在一起使用的。

	@Component
	@Aspect
	public class BeforeAspectTest {
	    // 定义一个Pointcut, 使用切点表达式函数来描述对哪些Joinpoint使用advise
	    @Pointcut("execution(* com.service.UserService.*(..))")
	    public void dataAccessOperation() {
	    }
	}

	@Component
	@Aspect
	public class AdviseDefine {
	    // 定义advise
	    @Before("com.aspect.PointcutDefine.dataAccessOperation()")
	    public void doBeforeAccessCheck(JoinPoint joinPoint) {
	        System.out.println("Before advise, method: " + joinPoint.getSignature().toShortString());
	    }
	}

### 3、注解
注解Annotation是一种类似注释的机制，在代码中添加注解可以在之后的某个时间使用这些信息。Java虚拟机不会对注解进行编译，而是通过反射机制读取注解中的信息。使用注解的主要目的是为了提高开发效率，不需要配置过多的xml文件，不必在配置文件中添加标签。

#### 3.1 使用注解
需要在配置文件中增加命名空间和约束文件：

	<beans ...  
	xmlns:context="http://www.springframework.org/schema/context"  
	xsi:schemaLocation="  
	...  
	http://www.springframework.org/schema/contexthttp://www.springframework.org/schema/context/spring-context-2.5.xsd  
	">  

#### 3.2 配置基包
告诉框架哪些类使用注解：

	<context:component-scan base-package="com.spring" />

#### 3.3 注解种类
Spring框架自带很多注解。

**持久层注解@Repository**

	@Repository
	public class UserDao{
	    // ......
	}

等同于配置文件中的：

	<bean id="userDao" class="com.spring.UserDao" />

**服务层注解@Service**

	@Service(value="testService")
	public classTestService {

	    @Resource //相当于自动装配
	    private UserDao userDao;
	   
	    public UserDao getUserDao() {
	        return userDao;
	    }

	    public void setUserDao(UserDao userDao) {
	        this.userDao = userDao;
	    }
	}

等同于配置文件中的：

	<bean id="testService" class="com.spring.UserService" />

其中，@Resource表示对象间的关系，默认采用byName方式进行装配，如果找不到关联对象，则采用byType方式装配。

**控制层注解@Controller**

	@Controller(value="ua")
	@Scope(value="prototype")
	public class UserAction {

	    @Resource
	    private UserService userService；

	    public UserService getUserService() {
	        return userService;
	    }
	}

等同于配置文件中的：

	<bean id="ua" class="com.spring.UserAction " />

Tips：以上三层中的组件关键字都可以使用@Component来代替。

**从Spring环境中获取Action对象**

	ServletContext application =request.getSession().getServletContext();
	ApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(application);

	// 获取控制层对象
	UserAction useraction = (UserAction)ac.getBean("ua");

	// 设置编码
	response.setContentType("text/html;charset=GBK");
	PrintWriter out = response.getWriter();

	// 分别打印三个层的对象
	out.println("Action:" + userAction);
	out.println("Service:" + userAction.getUserService());
	out.println("Dao:" + userAction.getUserService().getUserDao());
