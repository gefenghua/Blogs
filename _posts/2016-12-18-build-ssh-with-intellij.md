---
layout: post
title:  "使用IntelliJ IDEA构建SSH框架"
categories: jekyll update
---

## 一、SSH概述
传统的Java Web程序采用JSP+Servlet+JavaBean来实现。这种模式实现了最基本的MVC分层，JSP负责前台展示、Servlet负责流程逻辑控制、JavaBean负责数据封装。但是在JSP页面中嵌入很多Java代码会造成页面结构混乱，Servlet和JavaBean负责大量跳转和运算，耦合紧密，程序复用度低。

SSH框架采用Struts+Spring+Hibernate来实现。集成SSH框架的系统从职责上分为四层：表示层、业务逻辑层、数据持久层、域模块层（实体层）。Struts作为系统的整体基础架构，负责MVC的分离，在Struts框架的模型部分，控制业务跳转。Spring负责查找、定位、创建和管理对象及对象之间的依赖关系。Hibernate框架对持久层提供支持。通过分层，实现了低耦合，使得程序可复用，系统更容易扩展和维护。

表现层通过JSP页面实现交互并传递请求，调用相关业务逻辑，最后接收响应。业务层通过Spring实现业务逻辑和事务控制，并调用相关数据库操作。持久层依赖于Hibernate的对象化映射和数据库交互，处理请求的数据，并返回处理结果。用来封装数据库的操作，不包含任何业务逻辑。实体层将关系型数据库中的表映射成对象实体，对数据库表的操作就转换成了对实体对象的操作。

通常情况下，根据不同的层级和功能，在项目的src目录下定义如下package：

* action，负责数据验证、数据传递、调用服务，相对于控制器的角色
* service，定义业务逻辑的接口
* serviceimpl，定义业务逻辑的实现类
* dao，定义数据库操作的接口
* daoimpl，定义数据库操作的实现类
* model/entity，定义持久化的对象

在WEB-INF目录下定义jsp页面，负责页面展示、数据输入、数据提交、简单的数据校验以及结果显示，相对于视图的角色。

---

## 二、开发环境
OS：Windows 7 SP1

IDE：IntelliJ IDEA 2016.1 [下载地址](http://www.jetbrains.com/idea/)

JDK：1.7.0 [下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

Struts：2.3.16 [下载地址](http://struts.apache.org/download.cgi)

Spring：4.3.4 [下载地址](http://repo.spring.io/release/org/springframework/spring/4.3.4.RELEASE/)

Hibernate：4.3.4 [下载地址](http://hibernate.org/orm/downloads/)

---

## 三、创建Web项目
注：使用IntelliJ IDEA创建Java Web项目时，可以将Struts2、Spring、Hibernate框架一起添加到项目中，也可以使用项目构建工具如Maven来创建。为了能够更清楚地了解SSH每个框架的应用，采用手动搭建的方式，将框架逐一添加到项目中，最终整合成完整的SSH框架。

在新建项目的时候，选择Java EE下的Web Application框架

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/01_create_project_01.jpg)

输入项目名和模块名

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/01_create_project_02.jpg)

生成web项目

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/01_create_project_03.jpg)

此时的项目中，只包含两个文件：web.xml和index.jsp，需要通过项目设置导入其他框架。

---

## 四、添加Struts2框架

### 项目设置
选择项目的SDK和语言级别，以及编译之后的输出路径

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/02_project_settings_01.jpg)

为模块添加Struts2框架（这里需要选中模块下的Web，才能够显示Struts2框架）

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/02_project_settings_02.jpg)

为项目添加Struts2的lib库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/02_project_settings_03.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/02_project_settings_04.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/02_project_settings_05.jpg)

### 创建Action实现类
在src目录下，创建action目录，并新建一个LoginAction类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/03_action_class_01.jpg)

### 编写JSP页面
登录页面

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/04_index_jsp_01.jpg)

登录成功页面

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/04_index_jsp_02.jpg)

### 配置struts.xml文件
在src目录下，添加Struts配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/05_struts_xml_01.jpg)

### 配置web.xml文件
添加Struts的核心filter，以及对应的filter映射

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/06_web_xml_01.jpg)

### 配置web服务器
在运行之前，首先需要配置一个web服务器，如Tomcat

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/07_run_configuration_01.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/07_run_configuration_02.jpg)

### 测试验证
启动配置好的tomcat web服务器

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_01.jpg)

登录失败

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_02.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_03.jpg)

登录成功

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_04.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_05.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/08_test_struts_06.jpg)

至此，说明Struts框架添加成功。

---

## 五、添加Hibernate框架
### 项目设置
为模块添加Hibernate框架

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/09_project_settings_01.jpg)

为项目添加Hibernate的lib库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/09_project_settings_02.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/09_project_settings_03.jpg)

另外，还需要导入可选的lib库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/09_project_settings_04.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/09_project_settings_05.jpg)

### 创建数据源
新建一个MySQL的数据源

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/10_data_source_01.jpg)

导入数据库驱动的lib库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/10_data_source_02.jpg)

### 创建Model实体类
在src目录下，创建model目录，并新建一个User类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/11_model_class_01.jpg)

### 创建映射文件
通常在创建一个实体类之后，需要创建与之对应的映射文件，将实体类对象与数据库表关联

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/12_model_mapping_01.jpg)

注：也可以通过Persistence视图中的“Generate Persistence Mapping”来完成映射，这样就不需要创建每个实体类的映射文件。配置hibernate.cfg.xml的时候，不是指定映射文件，而是直接指定实体类。

### 配置hibernate.cfg.xml文件
在src目录下，添加Hibernate配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/13_hibernate_xml_01.jpg)

### 测试验证
新建一个HibernateTest测试类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/14_hibernate_test_class_01.jpg)

新建一个帮助类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/14_utils_class_01.jpg)

运行结果

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/14_hibernate_test_class_02.jpg)

数据库被更新

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/14_hibernate_test_class_03.jpg)

至此，说明Hibernate框架添加成功。

---

## 六、添加Spring框架
### 项目设置
为模块添加Spring框架

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/15_project_settings_01.jpg)

为项目添加Spring的lib库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/15_project_settings_02.jpg)

### 创建Dao接口类
在src目录下，创建dao目录，并新建一个UserDao接口类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/16_dao_class_01.jpg)

### 创建DaoImpl实现类
在src目录下，创建daoimpl目录，并新建一个UserDaoImpl实现类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/17_daoimpl_class_01.jpg)

### 创建Service接口类
在src目录下，创建service目录，并新建一个UserService接口类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/18_service_class_01.jpg)

### 创建ServiceImpl实现类
在src目录下，创建serviceimpl目录，并新建一个UserServiceImpl实现类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/19_serviceimpl_class_01.jpg)

### 配置applicationContext.xml文件
在src目录下，添加Spring配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/20_spring_xml_01.jpg)

### 配置web.xml文件
在web.xml配置文件中添加Spring的监听

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/21_web_xml_01.jpg)

### 测试验证
新建一个SpringTest测试类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/22_spring_test_class_01.jpg)

运行结果

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/22_spring_test_class_02.jpg)

至此，说明Spring框架添加成功。

---

## 七、整合Spring和Hibernate框架
### 配置jdbc.properties文件
在src目录下，添加jdbc.properties配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/23_jdbc_properties_01.jpg)

### 配置applicationContext.xml文件
修改Spring配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/24_spring_xml_01.jpg)

### 配置hibernate.cfg.xml文件
修改Hibernate配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/25_hibernate_xml_01.jpg)

---

## 八、整合Spring和Struts框架
### 导入Spring插件jar包
整合Struts和Spring框架，需要添加Struts的Spring插件jar包

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/26_project_settings_01.jpg)

### 配置struts.xml文件
修改Struts配置文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-20-create-ssh-with-intellij/27_struts_xml_01.jpg)
