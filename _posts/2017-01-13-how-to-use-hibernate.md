---
layout: post
title:  "Hibernate快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/hibernate_icon.png)

## 一、概述
Hibernate是一种对象/关系映射的解决方案，就是将Java中对象与对象之间的关系映射至关系数据库中的表与表之间的关系。对JDBC进行了轻量级的对象封装，解决了面向对象语言操作关系型数据库出现的不匹配问题。

作为模型层/数据访问层，通过配置文件（hibernate.cfg.xml或hibernate.properties）和映射文件（*.hbm.xml）把Java对象或持久化对象映射到数据库中的表。其中实体类对应表，类的属性对应数据库的表字段，然后通过操作PO（持久化对象），对数据库中的表进行各种操作。

优点：

* 对象化：以对象的思维操作数据库，开发过程中只需要操作对象即可
* 可移植：对持久层做了封装，不需考虑具体的数据库类型，代码具有可重用性
* 非侵入：不需要继承任何类，不需要实现任何接口，属于POJO对象

缺点：

* 使用数据库特性的语句，不容易调优
* 大批量数据的更新存在问题
* 存在大量的攻击查询功能

[官网地址](http://hibernate.org/)

## 二、核心接口
1. Configuration，负责管理Hibernate的配置信息，包括数据库URL、数据库用户名等，保存在配置文件hibernate.cfg.xml中。
2. SessionFactory，负责创建Session实例，保存了对应当前数据库配置的所有映射关系。
3. Session，提供了众多持久化的方法，如save、update、delete等，通过SessionFactory创建。
4. Transaction，负责事务操作，包括JDBC的事务、JTA中的UserTransaction等。
5. Query，负责执行HQL语句。

## 三、工作流程
1. 开发持久化类，由POJO和映射文件组成
2. 获取Configuration
3. 获取SessionFactory
4. 获取Session，打开事务
5. 用面向对象的方式操作数据库
6. 关闭事务，关闭Session

## 四、HQL
HQL是一种面向对象的查询语言，SQL的操作对象是数据库表，而HQL的操作对象是类、实例、属性等。HQL的抽象依赖于Query类，每个Query实例对应一个查询对象。使用HQL查询步骤如下：

1. 获取Session对象
2. 编写HQL语句
3. 以HQL语句作为参数，调用Session的createQuery方法创建查询语句
4. 如果HQL语句包含参数，则调用Query的setXXX方法赋值
5. 调用Query对象的list等方法返回查询结果列表（持久化实体集）

## 五、映射
对象关系映射有很多种，除了基本映射之外，还包括集合映射、组件映射、继承映射、关联关系映射等。其中，关联关系映射是最常用的一种映射，它包括一对一、一对多、多对一和多对多几种类型。

基本映射是对一个实体进行映射，关联关系映射是处理多个实体之间的关系，关联关系在对象模型中有一个或多个引用。

### 1、基本映射
映射文件一般以所创建的bean对象的名称来命名，声明bean对象及其属性对应数据库中的哪个表及哪个字段。除了使用XML方式配置映射外，还可以通过使用注解的方式配置映射。

	<hibernate-mapping>
	    <class name="beanName" table="tableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName" column="columnName"></property>
	    </class>
	</hibernate-mapping>

配置文件hibernate.cfg.xml，用来指定URL、用户名、密码、JDBC驱动等数据库连接信息，同时包含已经定义好的映射文件。

	<hibernate-configuration>
	    <session-factory>
	        <property name="hibernate.connection.url"></property>
	        <property name="hibernate.connection.driver_class"></property>
	        <property name="hibernate.connection.username"></property>
	        <property name="hibernate.connection.password"></property>

	        <mapping resource="com/hibernate/pojo/User.hbm.xml"/>
	    </session-factory>
	</hibernate-configuration></span>

### 2、多对一映射
多对一关联映射是最常见的映射，也是Hibernate中最简单的一种映射关系。原理是“多端”维护关联关系，在“多端”加入外键，以此指向“一端”，这样“多端”也就拥有了“一端”的引用。

	<hibernate-mapping>
	    <class name="manyBeanName" table="manyTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <many-to-one name="foreignKey" class="oneBeanName" column="foreignKey+Id"></many-to-one>
	    </class>
	</hibernate-mapping>

	<hibernate-mapping>
	    <class name="oneBeanName" table="oneTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	    </class>
	</hibernate-mapping>

### 3、一对一映射
一对一关联映射就是两个对象之间是一对一的关系。由于关系模型是与方向无关的，即是双向性的，而对象模型是有方向性的，是由对象模型即映射文件决定的。所以一对一单向关联映射和双向关联映射还是有区别的，主要体现在实体类和映射文件的不同。

对象模型与关系模型关联关系的维护有两种方式：主键关联和唯一外键关联。

**主键关联**

两个对象使用相同的主键，不需要使用其他外键。

	<hibernate-mapping>
	    <class name="beanNameOne" table="tableNameOne"/>
	        <id name="id">
	            <generator class="foreign">
	                <param name="property">primaryKey</param>
	            </generator>
	        </id>
	        <property name="propertyName"></property>
	        <one-to-one name="objectTwo" class="beanNameTwo" constrained="true"></one-to-one>
	    </class>
	</hibernate-mapping>

	<!-- 单向 -->
	<hibernate-mapping>
	    <class name="beanNameTwo" table="tableNameTwo"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	    </class>
	</hibernate-mapping>

	<!-- 双向 -->
	<hibernate-mapping>
	    <class name="beanNameTwo" table="tableNameTwo"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <one-to-one name="objectOne" class="beanNameOne" fetch="join"></one-to-one>
	    </class>
	</hibernate-mapping>

**唯一外键关联**

某一端使用一个外键进行关联，属于多对一的特例。

	<hibernate-mapping>
	    <class name="beanNameOne" table="tableNameOne"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <many-to-one name="objectTwo" class="beanNameTwo" column="objectTwo+Id" unique="true"></many-to-one>
	    </class>
	</hibernate-mapping>

	<!-- 单向 -->
	<hibernate-mapping>
	    <class name="beanNameTwo" table="tableNameTwo"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	    </class>
	</hibernate-mapping>

	<!-- 双向 -->
	<hibernate-mapping>
	    <class name="beanNameTwo" table="tableNameTwo"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <one-to-one name="objectOne" class="beanNameOne" property-ref="propertyNameOne"></one-to-one>
	    </class>
	</hibernate-mapping>

由于采用主键关联映射不能改成多对一关联映射，灵活性差，所以通常会采用唯一外键关联映射。

### 4、一对多映射
一对多关联映射和多对一关联映射的映射原理一样，都是在“多端”加入外键，指向“一端”。

	<!-- 单向 -->
	<hibernate-mapping>
	    <class name="oneBeanName" table="oneTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <set name="objectMany">
	            <key column="primaryKey"/>
	            <one-to-many class="manyBeanName"/>
	        </set>
	    </class>
	</hibernate-mapping>

	<hibernate-mapping>
	    <class name="manyBeanName" table="manyTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	    </class>
	</hibernate-mapping>


	<!-- 双向 -->
	<hibernate-mapping>
	    <class name="oneBeanName" table="oneTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <set name="objectMany" inverse="true">
	            <key column="primaryKey" not-null="true"/>
	            <one-to-many class="manyBeanName"/>
	        </set>
	    </class>
	</hibernate-mapping>

	<hibernate-mapping>
	    <class name="manyBeanName" table="manyTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <many-to-one name="objectOne" class="oneBeanName" column="primaryKey"/>
	    </class>
	</hibernate-mapping>

### 5、多对多映射
多对多关联映射是最常见、也是最容易理解的映射。无论是单向关联还是双向关联，都是将两个表中的主键通过第三张表进行关联，避免数据冗余。

	<hibernate-mapping>
	    <class name="oneBeanName" table="oneTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <set name="objectTwo" table="twoTableName">
	            <key column="primaryKeyOne"/>
	            <many-to-many class="twoBeanName" column="primaryKeyTwo"/>
	        </set>
	    </class>
	</hibernate-mapping>

	<!-- 单向 -->
	<hibernate-mapping>
	    <class name="twoBeanName" table="twoTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	    </class>
	</hibernate-mapping>

	<!-- 双向 -->
	<hibernate-mapping>
	    <class name="twoBeanName" table="twoTableName"/>
	        <id name="id"><generator class="native"/></id>
	        <property name="propertyName"></property>
	        <set name="objectOne" table="oneTableName">
	            <key column="primaryKeyTwo"/>
	            <many-to-many class="oneBeanName" column="primaryKeyOne"/>
	        </set>
	    </class>
	</hibernate-mapping>

## 六、实现dao操作

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

---

参考资料：

[Hibernate详细教程](http://blog.csdn.net/qq_26676207/article/details/51422472)

[Hibernate原理](http://blog.csdn.net/jiuqiyuliang/article/details/39078749)
