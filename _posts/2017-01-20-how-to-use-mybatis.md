---
layout: post
title:  "MyBatis快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/mybatis_icon.jpg)

## 一、概述
MyBatis是Apache的一个开源项目，是一个基于Java的持久层框架。它支持普通SQL查询、存储过程以及高级映射。消除了几乎所有的JDBC代码，并且基本不需要手动去设置参数和获取检索结果。使用XML或者注解进行配置，能够映射基本数据元素、Map接口和POJO到数据库。

## 二、框架
MyBatis的功能架构分三层：基础支撑层、数据处理层、API接口层。

1. 基础支撑层

	负责最基本的功能支撑，包括连接管理、事务管理、配置加载和缓存处理。基础支撑层为上层的数据处理层提供了最基础的支撑。

2. 数据处理层

	负责具体的SQL查询、SQL解析、SQL执行和执行结果的映射处理等。数据处理层的主要目的是根据调用的请求完成一次数据库操作。

3. API接口层

	提供给外部使用的接口API，通过这些API来操作数据库。接口层接收到调用请求会立刻调用数据处理层来完成具体的操作。

## 三、工作流程

### 1、加载配置并初始化
通过配置文件或注解，将SQL的配置信息加载成MappedStatement对象，存储在内存中。

### 2、接收调用请求
调用MyBatis提供的API，并将请求传递给下层的数据处理层进行处理。

### 3、处理操作请求

* 根据传入的SQL的ID查找对应的MappedStatement对象
* 根据传入的参数解析MappedStatement对象，得到要执行的SQL
* 获取数据库连接，执行SQL，并得到执行结果
* 根据MappedStatement对象中的结果映射配置，对执行结果进行转换处理，得到最终结果
* 释放连接资源

### 4、返回处理结果

## 四、实现

### 1、创建与表对应的实体类

### 2、创建映射文件xxxMapper.xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper .//EN" "http://mybatis.org/dtd/mybatis--mapper.dtd">

	<mapper namespace="com.demo.ssm.mapping.userMapper">
	    <select id="getUser" parameterType="int" resultType="User">
	        select * from user where id = #{id}
	    </select>
	</mapper>

其中，namespace属性用来定义mapper的命名空间。\<select>标签中的id属性用来标识sql语句，值必须唯一。parameterType属性用来定义查询时使用的参数类型。resultType属性用来定义查询返回的结果集类型。如果返回的结果集是个列表，应该定义resultMap类型。

	<resultMap id="resultListUser" type="User">
	    <id column="id" property="id" />
	    <result column="userName" property="userName" />
	    <id column="userAge" property="userAge" />
	</resultMap>

	<mapper namespace="com.demo.ssm.mapping.userMapper">
	    <select id="getUsers" parameterType="string" resultMap="resultListUser">
	        select * from user where username like #{userName}
	    </select>
	</mapper>

### 3、配置MyBatis的配置文件

	<?xml version="." encoding="UTF-"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config .//EN" "http://mybatis.org/dtd/mybatis--config.dtd">

	<configuration>
	    <environments default="development">
	        <environment id="development">
	            <transactionManager type="JDBC" />
	            <dataSource type="POOLED">
	                <property name="driver" value="com.mysql.jdbc.Driver" />
	                <property name="url" value="jdbc:mysql://localhost:/mybatis" />
	                <property name="username" value="root" />
	                <property name="password" value="root" />
	            </dataSource>
	        <environment>
	    <environments>

	    <mappers>
	        <mapper resource="com.demo.ssm.mapping.userMapper.xml">
	    </mappers>
	</configuration>

## 五、关联查询
在实际应用中，经常需要进行关联查询，如一对多、多对一等。这时，就需要在配置文件中使用\<association>标签进行关联。关联查询的配置有两种方式：内部关联和外部关联。

### 1、在内部关联

	<resultMap id="resultUserArticleList" type="Article">
	    <id property="id" column="aid" />
	    <result property="title" column="title" />
	    <result property="content" column="content" />
	    <association property="user" javaType="User">
	        <id property="id" column="id" />
	        <result property="userName" column="userName" />
	        <result property="userAge" column="userAge" />
	    </association>
	</resultMap>

	<select id="getUserArticles" parameterType="int" resultMap="resultUserArticleList">
	    select user.id,user.userName,user.userAge,article.id aid,article.title,article.content from user,article
	    where user.id=article.userid and user.id=#{id}
	</select>

### 2、从外部关联

	<resultMap type="User" id="resultListUser">
	    <id column="id" property="id" />
	    <result column="userName" property="userName" />
	    <result column="userAge" property="userAge" />
	</resultMap>

	<resultMap id="resultUserArticleList" type="Article">
	    <id property="id" column="aid" />
	    <result property="title" column="title" />
	    <result property="content" column="content" />
	    <association property="user" javaType="User" resultMap="resultListUser" />
	</resultMap>

	<select id="getUserArticles" parameterType="int" resultMap="resultUserArticleList">
	    select user.id,user.userName,user.userAge,article.id aid,article.title,article.content from user,article
	    where user.id=article.userid and user.id=#{id}
	</select>

很显然，第二种方式更容易达到复用的效果。

## 六、分页
在实际应用中，如果查询返回的结果记录很多，就需要做物理分页。不同的数据库，对应不同的实现方法。mysql利用limit offset和pagesize来实现，而oracle利用rownum来实现。实现MyBatis的物理分页。

### 1、在mapper的sql语句中直接使用分页条件

	<select id="getUserArticles" parameterType="params" resultMap="resultUserArticleList">
	    select user.id,user.userName,user.userAge,article.id aid,article.title,article.content from user,article
	    where user.id=article.userid and user.id=#{id} limit #{offset},#{pagesize}
	</select>

这里的parameterType是传入的参数类或者Map，包含offset和pagesize，以及其他需要的参数。相对来说，这是比较简单的一种方式。

### 2、使用MyBatis插件
更通用的一种方式是使用插件进行分页。使用插件的话，需要在MyBatis的配置文件中进行设置。

## 七、动态SQL
MyBatis的动态sql语句是基于OGNL表达式的，可以很方便地在sql语句中实现某些逻辑。

### 1、if

	<select id="dynamicIf" parameterType="User" resultType="User">
	    select * from user where 1 = 1
	    <if test="userName != null">
	        and userName = #{userName}
	    </if>
	</select>

如果提供的userName参数不为空，就为sql语句动态添加userName=#{userName}的语句。

### 2、choose

	<select id="dynamicChoose" parameterType="User" resultType="User">
	    select * from user where 1 = 1
	    <choose>
	        <when test="userName != null">
	            and userName = #{userName}
	        </when>
	        <otherwise>
	            and userName = "sean"
	        </otherwise>
	    </choose>
	</select>


当userName不为空时，为sql语句动态添加userName=#{userName}。否则，为sql语句动态添加userName="sean"语句。当条件满足时，不再继续判断，直接跳出choose。当所有条件都不满足时，输出otherwise中的内容。

### 3、where

	<select id="dynamicWhere" parameterType="User" resultType="User">
	    select * from user
	    <where>
	        <if test="userName != null">
	            userName = #{userName}
	        </if>
	    </where>
	</select>

在where元素的地方输出一个where，并且能够智能地处理and和or条件。主要用来简化sql语句中where条件判断。

### 4、trim

	<select id="dynamicTrim" parameterType="User" resultType="User">
	    select * from user
	    <trim prefix="where" prefixOverrides="and | or">
	        <if test="userName != null">
	            userName = #{userName}
	        </if>
	    </trim>
	</select>

用来给包含的内容加上前缀或后缀，也可以把包含内容的首部或尾部的某些内容覆盖。通常可以利用trim来代替where元素的功能。

### 5、set

	<update id="dynamicSet" parameterType="User">
	    update user
	    <set>
	        <if test="userName != null">
	            userName = #{userName}
	        </if>
	    </set>
	    where userId = #{userId}
	</update>

主要用于更新操作，在包含的语句前输出一个set，功能和where元素基本相同。

### 6、foreach
主要用于构建in条件，可以在sql语句中迭代一个集合。foreach元素主要有几个属性：

* item：表示每个元素在进行迭代时的别名
* index：表示在迭代时，每次迭代到的位置
* open：表示语句以什么开始
* separator：表示迭代之间以什么作为分隔符
* close：表示语句以什么结束
* collection：表示集合

不同情况下，collection属性的值是不同的。如果传入的是单参数且参数类型是一个List的时候，属性值为list。

	<select id="dynamicForeachList" resultType="User">
	    select * from user where userId in
	    <foreach collection="list" index="index" item="item" open="(" separator="," close=")">
	        #{item}
	    </foreach>
	</select>

如果传入的是单参数且参数类型是一个Array的时候，属性值为array。

	<select id="dynamicForeachArray" resultType="User">
	    select * from user where userId in
	    <foreach collection="array" index="index" item="item" open="(" separator="," close=")">
	        #{item}
	    </foreach>
	</select>

如果传入的是多个参数时，将参数封装成一个Map，属性值为map里的key。

	<select id="dynamicForeachMap" resultType="User">
	    select * from user where userName like "%"#{userName}"%" and userId in
	    <foreach collection="ids" index="index" item="item" open="(" separator="," close=")">
	        #{item}
	    </foreach>
	</select>

## 八、代码生成工具
由于MyBatis应用程序需要大量的配置文件，如果完全手工配置，工作量巨大。MyBatis官方推出一个代码生成工具mybatis-generator-core的jar包，以便提高效率。

代码生成工具主要有几个功能：

* 生成pojo与数据库结构相对应
* 如果有主键，能够匹配主键；如果没有主键，可以用其他字段匹配
* 动态select、update、delete方法
* 自动生成接口
* 自动生成mapper，以及对单表的增删改查语句配置
* 提供例子以供参考

首先需要创建一个配置文件，进行生成工具的各种设置。

	<generatorConfiguration>
	    <!-- 配置mysql驱动jar包路径 -->
	    <classPathEntry location=""
	    <context id="mysql_tables" targetRuntime="MyBatis3">
	    <!-- 控制生成代码中的注释 -->
	    <commentGenerator>
	        <property name="suppressAllComments" value="true" />
	        <property name="suppressDate" value="true" />
	    </commentGenerator>
	    <!-- 数据库连接 -->
	    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
	        connectionURL="jdbc:mysql://127.0.0.1:3306/mybatis?characterEncoding=utf8"
	        userId="root"
	        password="root">
	    </jdbcConnection>
	    <javaTypeResolver>
	        <property name="forceBigDecimals" value="false" />
	    </javaTypeResolver>
	    <!-- model层 -->
	    <javaModelGenerator targetPackage="com.demo.ssm.model" targetProject="src">
	        <property name="enableSubPackages" value="true" />
	        <property name="trimStrings" value="true" />
	    </javaModelGenerator>
	    <!-- mapper映射文件 -->
	    <sqlMapGenerator targetPackage="com.demo.ssm.mapper" targetProject="src">
	        <property name="enableSubPackages" value="true" />
	    </sqlMapGenerator>
	    <!-- mapper接口 -->
	    <javaClientGenerator type="XMLMAPPER" targetPackage="com.demo.ssm.inter" targetProject="src">
	        <property name="enableSubPackages" value="true" />
	    </javaClientGenerator>
	    <!-- 指定要生成代码的数据库表 -->
	    <table schema="mybatis" tableName="user" domainObjectName="User"
	        enableCountByExample="false" enableUpdateByExample="false"
	        enableDeleteByExample="false" enableSelectByExample="false"
	        selectByExampleQueryId="false">
	    </table>
	    </context>
	</generatorConfiguration>

然后创建一个代码生成类，用于生成代码。

	public class GenMain {
	    public static void main(String[] args) {
	        List<String> warnings = new ArrayList<String>();
	        boolean overwrite = true;
	        String genCfg = "/mbgConfiguration.xml";
	        File configFile = new File(GenMain.class.getResource(genCfg).getFile());
	        ConfigurationParser cp = new ConfigurationParser(warnings);
	        Configuration config = null;
	        try {
	            config = cp.parseConfiguration(configFile);
	        } catch (IOException e) {
	            e.printStackTrace();
	        } catch (XMLParserException e) {
	            e.printStackTrace();
	        }
	        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
	        MyBatisGenerator myBatisGenerator = null;
	        try {
	            myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
	        } catch (InvalidConfigurationException e) {
	            e.printStackTrace();
	        }
	        try {
	            myBatisGenerator.generate(null);
	        } catch (SQLException e) {
	            e.printStackTrace();
	        } catch (IOException e) {
	            e.printStackTrace();
	        } catch (InterruptedException e) {
	            e.printStackTrace();
	        }
	    }
	}
