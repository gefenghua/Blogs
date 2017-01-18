---
layout: "post"
title: "JUnit快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/junit_icon.png)

## 一、概述
> JUnit is a simple framework to write repeatable tests. It is an instance of the xUnit architecture for unit testing frameworks.

JUnit是一个简单的单元测试框架，可以用来编写和运行可重复的自动化测试。JUnit非常适合TDD（测试驱动开发）的开发模式，即先根据业务需求编写测试程序，然后再编写代码保证其能够通过测试。

[官网地址](http://junit.org/junit4/)

[下载地址](https://github.com/junit-team/junit4/releases/)

## 二、注解
JUnit 4利用Java 5的注解新特性，大大简化了单元测试的代码，从而提高了测试效率。

* @Test：使用该注解的方法就是测试方法，能够自动被识别和运行
* @Ignore：忽略使用该注解的测试方法，主要用于未完成的测试方法
* @Before：使用该注解的方法会在每个测试方法之前都执行一次，用于初始化或准备测试数据等
* @After：使用该注解的方法会在每个测试方法之后都执行一次，用于清除测试数据
* @BeforeClass：使用该注解的方法只在所有测试方法之前执行一次，主要用于文件读取或数据库连接等
* @AfterClass：使用该注解的方法只在所有测试方法之后执行一次，用于释放资源
* @RunWith：指定测试类所使用的运行器，只能修饰类而不能修饰方法
* @Parameters：指定测试类的测试数据集合
* @Rule：定义测试类中每个测试方法的行为规则
* @FixMehodOrder：指定测试方法的执行顺序

注意：使用@BeforeClass和@AfterClass注解的方法，必须是公有的静态方法。每个测试类中Before和After这四个注解不能重复使用，并且一个方法只能选择其中的一个。

## 三、断言
断言Assert是编写测试用例的核心实现方法，将期望值与测试结果进行比较，以此来判断测试是否通过。通常会静态导入Assert的包，使用更加方便：

	import static org.junit.Assert.*;

* assertEquals()：比较两个对象是否相等
* assertNotEquals()：比较两个对象是否不相等
* assertSame()：比较两个对象的引用是否相等
* assertNotSame()：比较两个对象的引用是否不相等
* assertNull()：查看对象是否为空
* assertNotNull()：查看对象是否不为空
* assertTrue()：查看运行结果是否为true
* assertFalse()：查看运行结果是否为false
* assertThat()：查看实际值是否满足指定条件
* fail()：使测试失败

## 四、基本用法

### 1、创建测试目标类

	public class Calculator {
	  public int evaluate(String expression) {
	    int sum = 0;
	    for (String summand: expression.split("\\+"))
	      sum += Integer.valueOf(summand);
	    return sum;
	  }
	}

### 2、创建测试类

	import static org.junit.Assert.assertEquals;
	import org.junit.Test;

	public class CalculatorTest {
	  @Test
	  public void evaluatesExpression() {
	    Calculator calculator = new Calculator();
	    int sum = calculator.evaluate("1+2+3");
	    assertEquals(6, sum);
	  }
	}

### 3、运行测试类

**TODO**

## 五、其他用法

### 1、限时测试
有的时候测试会出现死循环，为了避免这种情况，可以为测试方法设定超时限制。设定的时间单位为毫秒。

	@Test(timeout=1000)

### 2、异常测试
Java中的异常处理很常见，经常需要编写抛出异常的方法。为了判断方法抛出的异常是否符合预期，就要为测试方法指定所期望的异常类型。

	@Test(expected=ArithmeticException.class)

### 3、使用运行器
当把测试代码提交给JUnit框架后，框架通过运行器Runner来调用测试代码。不同的运行器具有不同的功能，需要根据实际情况来选择合适的运行器。如果没有指定运行器，JUnit会使用默认的TestClassRunner来处理测试代码。

### 4、参数化测试
有些测试方法，功能相同，只是不同的参数会产生不同的结果。如果把参数值的每种情况都列出来并做断言处理，既耗时又费力。这时，就可以采用参数化测试的方法来进行简化。

首先，参数化测试的类需要单独生成，不与其他测试共用同一个类，并且要使用特殊的运行器：

	@RunWith(Parameterized.class)

其次，定义两个成员变量，一个存放测试参数，一个存放该参数产生的预期结果。

然后，定义测试数据的集合方法，该方法要用@Parameters注解来修饰，返回一个二维数组：

	@Parameters
	public static Collection data() { return Arrays.asList(...) }

最后，定义带参的构造函数，这里参数的顺序要与数据集合的顺序一致。

其余的测试部分与普通测试类似。

### 5、打包测试
通常一个项目会包含很多测试类，一个一个执行会十分麻烦。JUnit提供了打包测试的功能，将所有需要运行的测试类集中起来，一次性运行，大大提高了测试效率。

首先，打包测试需要使用特殊的运行器：

	@RunWith(Suite.class)

其次，使用特殊的注解标明打包测试类，并将需要打包的类传递给该注解：

	@Suite.SuiteClasses({ A.class, B.class, ... })

最后，打包测试类可以为空。

## 六、扩展用法
在开发基于Spring的应用时，如果直接使用JUnit进行单元测试，会存在很多不足之处。

* 会导致Spring容器多次初始化问题
* 需要使用硬编码方式手工获取Bean
* 数据库现场容易遭到破坏
* 不方便对数据操作的正确性进行检查

Spring提供了一套扩展于JUnit的测试套件，主要由org.springframework.test包下的若干类组成。不仅弥补了JUnit方面的不足，也使得测试Spring的应用更加方便快捷。

### 1、基本用法

在测试类上使用注解@RunWith(SpringJUnit4ClassRunner.class)用于配置Spring中的测试环境，使用注解@ContextConfiguration(locations = { "classpath:config/applicationContext-*.xml", "classpath:services/ext/service-*.xml" })用于指定配置文件所在的位置。在属性上使用注解@Resource注入Spring容器的Bean对象。

### 2、事务用法

在测试类上使用注解@TransactionConfiguration(transactionManager="transactionManager")用于读取Spring配置文件中名为transactionManager的事务配置。在类或方法上使用注解@Transactional开启事务。在方法上使用注解@Rollback用于回滚事务。

### 3、继承用法

AbstractTransactionalJUnit4SpringContextTests类已经在类级别配置了事务支持，因此不必再配置@RunWith和@Transactional。

可以创建一个基测试类，对其进行注解。其他测试类继承该基类的时候，也继承了该基类的注解。

---

参考文章：

[Spring测试套件的使用](http://blog.csdn.net/wangpeng047/article/details/9631193)
