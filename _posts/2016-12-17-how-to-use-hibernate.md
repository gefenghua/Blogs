---
layout: post
title:  "Hibernate快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/hibernate_logo.png)

## 一、概述
Hibernate是一种对象/关系映射的解决方案，就是将Java中对象与对象之间的关系映射至关系数据库中的表与表之间的关系。对JDBC进行了轻量级的对象封装，解决了面向对象语言操作关系型数据库出现的不匹配问题。

作为模型层/数据访问层，通过配置文件（hibernate.cfg.xml或hibernate.properties）和映射文件（*.hbm.xml）把Java对象或持久化对象映射到数据库中的表。其中实体类对应表，类的属性对应数据库的表字段，然后通过操作PO（持久化对象），对数据库中的表进行各种操作。

---

## 二、核心接口
1. Configuration，负责管理Hibernate的配置信息，包括数据库URL、数据库用户名等，保存在配置文件hibernate.cfg.xml中。
2. SessionFactory，负责创建Session实例，保存了对应当前数据库配置的所有映射关系。
3. Session，提供了众多持久化的方法，如save、update、delete等，通过SessionFactory创建。
4. Transaction，负责事务操作，包括JDBC的事务、JTA中的UserTransaction等。
5. Query，负责执行HQL语句。

---

## 三、工作流程
1. 开发持久化类，由POJO和映射文件组成
2. 获取Configuration
3. 获取SessionFactory
4. 获取Session，打开事务
5. 用面向对象的方式操作数据库
6. 关闭事务，关闭Session

---

## 四、HQL
HQL是一种面向对象的查询语言，SQL的操作对象是数据库表，而HQL的操作对象是类、实例、属性等。HQL的抽象依赖于Query类，每个Query实例对应一个查询对象。使用HQL查询步骤如下：

1. 获取Session对象
2. 编写HQL语句
3. 以HQL语句作为参数，调用Session的createQuery方法创建查询语句
4. 如果HQL语句包含参数，则调用Query的setXXX方法赋值
5. 调用Query对象的list等方法返回查询结果列表（持久化实体集）

---

## 五、用法
### 1、导入jar包
（略）

### 2、创建bean对象
定义POJO对象。

### 3、创建映射文件
一般以所创建的bean对象的名称来命名，声明bean对象及其属性对应数据库中的哪个表及哪个字段。

	<hibernate-mapping>
	    <class name="beanName" table="tableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName" column="columnName"></property>
	</hibernate-mapping>

### 4、创建配置文件
配置文件hibernate.cfg.xml，指定数据库的URL、用户名、密码、JDBC驱动等，同时指定已经定义好的映射文件。

	<hibernate-configuration>
	    <session-factory>
	        <property name="hibernate.connection.url"></property>
	        <property name="hibernate.connection.driver_class"></property>  
	        <property name="hibernate.connection.username"></property>
	        <property name="hibernate.connection.password"></property>

	        <mapping resource="com/hibernate/pojo/User.hbm.xml"/>
	    </session-factory>
	</hibernate-configuration></span>

### 5、实现dao操作

	public class UserDao{  
	    public void insert(User user){          
	        // 加载配置文件hibernate.cfg.xml  
	        Configuration config = new Configuration().configure();  
	        // 创建会话工厂  
	        SessionFactory sf = config.buildSessionFactory();  
	        // 会话对象，表示与数据库的连接会话过程  
	        Session session = null;  
	        Transaction tx = null ;          
	        try{  
	            session= sf.openSession();  
	            // 开启事务  
	            tx= session.beginTransaction();  
	            // 调用sava方法 
	            session.save(user); 
	            // 提交事务  
	            tx.commit();                          
	        }        
	    }  
    }
