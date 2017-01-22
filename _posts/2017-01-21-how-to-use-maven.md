---
layout: post
title:  "Maven快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/maven_icon.png)

## 一、概述
> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.

Maven是一个软件项目管理的综合工具。它基于项目对象模型（POM）的概念，把所有的项目配置信息都定义在pom.xml文件中。通过配置文件，Maven可以管理项目的整个生命周期，包括构建、编译、测试、发布、报告等。

[官网地址](http://maven.apache.org/index.html)

## 二、下载和配置

### 1、下载
从Maven的官网下载与环境匹配的最新版本压缩包。下载之后无需安装，直接解压到指定目录。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-21-how-to-use-maven/01_download.png)

[下载地址](http://maven.apache.org/download.cgi)

### 2、配置
解压缩之后，需要进行环境变量的设置。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-21-how-to-use-maven/02_environment_setting.png)

### 3、验证
设置完环境变量之后，在命令行输入命令：`mvn -version` 验证是否安装成功。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-21-how-to-use-maven/03_validation.png)

### 4、添加本地仓库
进入Maven目录，打开conf目录下的settings.xml配置文件，添加本地仓库。这样，所有用户都可以共用这个仓库来构建项目。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-21-how-to-use-maven/04_conf_local_repo.png)

## 三、约定
Maven遵循“约定优于配置”的原则进行配置。主要有如下约定：

* 源代码路径：src/main/java
* 资源文件路径：src/main/resource
* 测试代码路径：src/test
* 编译后文件路径：/target/classes
* 可发布的文件路径：/target

[官网推荐的标准目录结构](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

## 四、创建项目
IntelliJ IDEA已经整合了Maven到IDE中，可以直接用来创建Maven项目。

[如何在IntelliJ IDEA中使用Maven](https://www.jetbrains.com/help/idea/2016.3/maven.html)

## 五、pom.xml
Maven的核心是pom.xml，Maven通过这个配置文件完成各种各样的任务。在实际项目中，不必完全理解全部配置项目，可以只关注主要的配置信息。关于各配置项目的详细定义，可以参照官网的说明。

[POM详细定义](http://maven.apache.org/pom.html)

### 1、配置文件

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>

	  <!-- The Basics -->
	  <groupId>...</groupId>
	  <artifactId>...</artifactId>
	  <version>...</version>
	  <packaging>...</packaging>
	  <dependencies>...</dependencies>
	  <parent>...</parent>
	  <dependencyManagement>...</dependencyManagement>
	  <modules>...</modules>
	  <properties>...</properties>

	  <!-- Build Settings -->
	  <build>...</build>
	  <reporting>...</reporting>

	  <!-- More Project Information -->
	  <name>...</name>
	  <description>...</description>
	  <url>...</url>
	  <inceptionYear>...</inceptionYear>
	  <licenses>...</licenses>
	  <organization>...</organization>
	  <developers>...</developers>
	  <contributors>...</contributors>

	  <!-- Environment Settings -->
	  <issueManagement>...</issueManagement>
	  <ciManagement>...</ciManagement>
	  <mailingLists>...</mailingLists>
	  <scm>...</scm>
	  <prerequisites>...</prerequisites>
	  <repositories>...</repositories>
	  <pluginRepositories>...</pluginRepositories>
	  <distributionManagement>...</distributionManagement>
	  <profiles>...</profiles>
	</project>

### 2、基本配置

* `<groupId>`表示组织标识
* `<artifactId>`表示项目名称
* `<version>`表示版本号
* `<packaging>`表示打包的格式
* `<dependencies>`表示依赖关系
* `<parent>`表示继承关系
* `<dependencyManagement>`用于管理依赖信息
* `<modules>`表示聚合关系
* `<properties>`表示值占位符，使用${X}访问，X代表属性

### 3、构建设置

* `<build>`处理声明项目目录结构、管理插件等
* `<reporting>`镜像构建元素

### 4、项目信息

* `<name>`表示项目名称
* `<description>`表示项目描述
* `<url>`表示项目URL
* `<inceptionYear>`表示项目开始年份
* `<licenses>`表示项目licenses
* `<organization>`表示项目组织
* `<developers>`表示项目开发人员
* `<contributors>`表示项目贡献者

### 5、环境设置

* `<issueManagement>`定义所用的缺陷跟踪系统
* `<ciManagement>`定义持续集成构建系统
* `<mailingLists>`定义邮件列表
* `<scm>`定义软件配置系统
* `<prerequisites>`定义前提条件
* `<repositories>`定义仓库
* `<pluginRepositories>`定义插件仓库
* `<distributionManagement>`定义分布式管理系统
* `<profiles>`定义相关设置

## 六、生命周期

* validate：验证工程是否正确，所有需要的资源是否可用
* compile：编译项目的源代码
* test：使用合适的单元测试框架测试已编译的源代码
* package：把已编译的代码打包成可发布的格式
* verify：运行所有检查，验证包是否有效
* install：把包安装在本地的仓库中
* deploy：在集成或者发布环境下执行，将包拷贝到远程仓库
* clean：清除之前构建的artifacts
* site：为项目生成文档站点

## 七、常用命令

* mvn archetype:create，创建Maven项目
* mvn compile，编译源代码，在目标目录下生成结果文件
* mvn test-compile，编译测试代码
* mvn test，运行单元测试
* mvn site，生成项目信息站点
* mvn clean，清楚目标目录下生成的结果文件
* mvn package，项目打包
* mvn install，将打包好的包安装到本地仓库
* cargo:deploy，部署到私有服务器

这些命令可以一起使用，但要注意命令的执行顺序：

	clean compile package install
