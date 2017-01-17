---
layout: post
title:  "logback快速入门"
categories: jekyll update
---

## 一、slf4j

![](https://github.com/gefenghua/MarkdownPictures/raw/master/slf4j_icon.png)

> The Simple Logging Facade for Java (SLF4J) serves as a simple facade or abstraction for various logging frameworks, such as java.util.logging, logback and log4j. SLF4J allows the end-user to plug in the desired logging framework at deployment time.

简单日志门面（SLF4J）为各种日志框架提供了一个简单的接口，使得用户能够在部署的时候配置其所希望的日志系统。

在实际的开发过程中，可能会使用各种不同的日志系统，每个日志系统的风格、样式和布局也不尽相同，在不同日志系统之间进行切换耗时耗力。使用slf4j无需考虑具体使用哪个日志系统，统一使用slf4j的API进行开发。当要在不同日志系统之间进行切换时，只需要选择对应的日志系统包即可，十分灵活方便（当然，日志系统本身的jar包和配置文件还是需要的）。还有一点好处是，slf4j能够支持多个参数，并且通过{}占位符进行替换，避免了各种各样的判断条件，从而提升了性能。

到官网下载最新版本的slf4j，解压缩。slf4j-api-\<version>.jar是slf4j的核心jar包。除此之外，还包含许多与日志系统对应的jar包。

[官网地址](http://www.slf4j.org/)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-14-how-to-use-logback/01-slf4j-jar.png)

slf4j中与各日志系统之间的对应关系如下：

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-14-how-to-use-logback/02-log-bindings.png)

## 二、logback

![](https://github.com/gefenghua/MarkdownPictures/raw/master/logback_icon.png)

Logback是一款开源的日志组件，可以应用在不同的环境下。主要包括三个模块：logback-core、logback-classic和logback-access。

logback-core是其他模块的基础模块。logback-classic可以看做是log4j的改良版本，完整地实现了slf4j API，使得logback可以与其他日志系统自由切换。logback-access集成Servlet容器，提供通过http访问日志的功能。

Logback由三个主要的类构建而成：Logger、Appender和Layout。Logger是日志的记录器，主要用于存放日志对象，也可以定义日志类型和级别。Appender主要用于指定日志的输出目标，输出目标可以是控制台、文件、数据库等。Layout主要负责格式化日志信息。

Logger的级别按照优先级的高低顺序分为：ERROR、WARN、INFO、DEBUG、TRACE。

[官网地址](http://logback.qos.ch/)

## 三、下载与配置
到官网下载最新版本的logback，解压缩。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-14-how-to-use-logback/03-logback-jar.png)

通常，在classpath目录下创建一个logback.xml来进行logback的配置。如果没有创建配置文件，logback会调用BasicConfigurator，创建一个默认的最小化配置。

	<configuration>
	  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
	    <!-- Assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
	    <encoder>
	      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
	    </encoder>
	  </appender>

	  <root level="debug">
	    <appender-ref ref="STDOUT" />
	  </root>
	</configuration>

## 四、配置说明
配置文件以\<configuration>开始，包含一个\<root>元素、若干\<appender>元素以及若干\<logger>元素。

	<?xml version="1.0" encoding="UTF-8"?>
	<!--
	  scan：当设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
	  scanPeriod：设置监测配置文件是否有修改的时间间隔，默认单位是毫秒。
	    当scan为true时，此属性生效。默认的时间间隔为1分钟。
	  debug：当设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。
	    默认值为false。
	-->
	<configuration scan="false" scanPeriod="60 seconds" debug="false">
	  <!-- 定义日志的根目录 -->
	  <property name="LOG_HOME" value="/app/log" />
	  <!-- 定义日志文件名称 -->
	  <property name="appName" value="netty"></property>

	  <!-- 控制台输出 -->
	  <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
	    <Encoding>UTF-8</Encoding>
	    <!-- 日志输出格式：
	      %d表示日期时间
	      %thread表示线程名
	      %-5level表示级别从左向右显示5个字符宽度
	      %logger{50}表示logger名字最长50个字符，否则按照句点分割
	      %msg表示日志消息
	      %n表示换行符
	    -->
	    <layout class="ch.qos.logback.classic.PatternLayout">
	      <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
	    </layout>
	  </appender>

	  <!-- 滚动文件输出。当符合某个条件时，将日志记录滚动到其他文件 -->
	  <appender name="appLogAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
	    <Encoding>UTF-8</Encoding>
	    <!-- 指定日志文件的名称 -->
	    <file>${LOG_HOME}/${appName}.log</file>
	    <!--
	      制定滚动策略，涉及文件移动和重命名。
	      TimeBasedRollingPolicy： 最常用的滚动策略，它根据时间来制定滚动策略，
	      既负责滚动也负责触发滚动。
	    -->
	    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
	      <!--
	        滚动时产生的文件的存放位置及文件名称
	        %d{yyyy-MM-dd}表示按天进行滚动
	        %i表示当文件大小超过maxFileSize时，按照i进行文件滚动
	      -->
	      <fileNamePattern>${LOG_HOME}/${appName}-%d{yyyy-MM-dd}-%i.log</fileNamePattern>
	      <!--
	        可选节点，控制保留的归档文件的最大数量，超出数量就删除旧文件。
	        假设设置每天滚动，且maxHistory是365，则只保存最近365天的文件，
	        删除之前的旧文件。注意，删除旧文件时，那些为了归档而创建的目录也会被删除。
	      -->
	      <MaxHistory>365</MaxHistory>
	      <!--
	        当日志文件超过maxFileSize指定的大小时，根据上面提到的%i进行日志文件滚动。
	        注意此处配置SizeBasedTriggeringPolicy是无法实现按文件大小进行滚动的，
	        必须配置timeBasedFileNamingAndTriggeringPolicy
	      -->
	      <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
	        <maxFileSize>100MB</maxFileSize>
	      </timeBasedFileNamingAndTriggeringPolicy>
	    </rollingPolicy>
	    <!-- 日志输出格式 -->
	    <layout class="ch.qos.logback.classic.PatternLayout">
	      <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [ %thread ] - [ %-5level ] [ %logger{50} : %line ] - %msg%n</pattern>
	    </layout>
	  </appender>

	  <!--
	    name：表示匹配的logger类型前缀，也就是包的前半部分
	    level：表示要记录的日志级别
	    additivity：作用在于children-logger是否使用rootLogger配置的appender进行输出，
	      false：表示只用当前logger的appender-ref，
	      true：表示当前logger的appender-ref和rootLogger的appender-ref都有效
	  -->
	  <!-- hibernate logger -->
	  <logger name="org.hibernate" level="error" />
	  <!-- Spring framework logger -->
	  <logger name="org.springframework" level="error" additivity="false"></logger>

	  <!--
	    root与logger是父子关系，没有特别定义则默认为root。
	    任何一个类只会和一个logger对应，要么是定义的logger，要么是root。
	    判断的关键在于找到这个logger，然后判断这个logger的appender和level。
	  -->
	  <root level="info">
	    <appender-ref ref="stdout" />
	    <appender-ref ref="appLogAppender" />
	  </root>
	</configuration>

[详细的配置说明](http://logback.qos.ch/manual/configuration.html)

## 五、用法
通常，logback与slf4j一同使用。

1. 创建logback.xml配置文件
2. 在Java程序中引入slf4j的jar包：org.slf4j.Logger和org.slf4j.LoggerFactory
3. 在Java类中使用slf4j的API记录日志

所有的logger都关联到一个LoggerContext对象，由它负责logger的创建和排列。通过LoggerFactory类的静态方法getLogger()取得所创建的logger实例，参数为当前类名，然后调用logger中与级别对应的打印方法来输出日志。

	final static Logger logger = LoggerFactory.getLogger(this.class);
	logger.info("logback info");

如果想查看logback内部状态信息，可以先通过slf4j的LoggerFactory.getLoggerFactory()方法创建一个LoggerContext实例，然后调用logback核心包中的静态类StatusPrinter的print()方法打印日志信息。

	LoggerContext lc = (LoggerContext) LoggerFactory.getILoggerFactory();
	StatusPrinter.print(lc);

运行时，将与所希望使用的日志系统对应的jar包添加到classpath，这样就切换到该日志系统。最终，在部署时实现不同日志系统之间的自由切换，与开发分离。
