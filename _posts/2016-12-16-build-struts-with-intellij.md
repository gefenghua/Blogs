---
layout: post
title:  "使用IntelliJ IDEA构建SSH框架之Struts篇"
categories: jekyll update
---

## 一、环境
OS：Windows 7 SP1

IDE：IntelliJ IDEA 2016.1 [下载地址](http://www.jetbrains.com/idea/)

Struts：2.5.5 [下载地址](http://struts.apache.org/download.cgi)

JDK：1.7.0_02 [下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

---

## 二、创建Java Web项目
* 在新建项目的时候，选择Web Application框架

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_01_new_project_01.jpg)

* 输入项目名和模块名

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_01_new_project_02.jpg)

* 生成web项目

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_01_new_project_03.jpg)

---

## 三、项目设置
* 选择项目的SDK和语言级别

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_02_project_settings_01.jpg)

* 为模块导入外部jar包

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_02_project_settings_02.jpg)

* 导入Struts库

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_02_project_settings_03.jpg)

* 导入Java Lib

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_02_project_settings_04.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_02_project_settings_05.jpg)

---

## 四、配置web.xml文件
添加Struts的核心filter，以及对应的filter映射。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_03_web_xml_01.jpg)

---

## 五、创建Action实现类

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_04_action_class_01.jpg)

---

## 六、编写JSP页面

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_05_jsp_form_01.jpg)

---

## 七、配置struts.xml文件

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_06_struts_xml_01.jpg)

---

## 八、配置web服务器
在运行之前，首先需要配置一个web服务器，如Tomcat。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_07_run_configuration_01.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_07_run_configuration_02.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_07_run_configuration_03.jpg)

---

## 九、测试验证
启动配置好的tomcat web服务器。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_01.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_02.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_03.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_04.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_05.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2016-12-16-create-struts-with-intellij/20161216_08_page_presetation_06.jpg)









