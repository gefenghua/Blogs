---
layout: post
title:  "Emmet快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/emmet_icon.jpg)

## 一、概述
Emmet的前身是Zen coding，是一款用于快速生成HTML或CSS代码的插件，可用于绝大多数主流文本编辑器。它的语法类似于CSS选择器，通过使用元素或属性的缩写，大大提高了HTML和CSS的编写速度，从而大幅度提高前端开发的效率。

[官网地址](http://www.emmet.io/)

[速查表](http://docs.emmet.io/cheat-sheet/)

## 二、HTML

### 1、初始模板
输入：`!`或`html:5`，然后按`Tab`键，自动生成HTML文档的初始模板。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/01-init-template.png)

### 2、元素
输入：元素名 + `#` + id名，自动生成带id属性的元素标签。

用例：`p#id-prop`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/02-el-id.png)

输入：元素名 + `.` + class名，自动生成带class属性的元素标签。

用例：`p.class-prop`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/03-el-class.png)

输入：元素名 + `#` + id名 + `.` + class名，可以实现链式操作。

用例：`p#id-prop.class-prop`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/04-el-id-class.png)

输入：元素名 + `{}` + 元素内容，自动生成带内容的元素标签。

用例：`p{content}`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/05-el-content.png)

输入：元素名 + `[]` + 属性名=属性值，自动生成带属性的元素标签。

用例：`img[src=example.jpg]`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/06-el-attribute.png)

### 3、嵌套
输入：父元素名 + `>` + 子元素名，实现父子关系的元素标签。

用例：`div>p`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/07-el-parent-child.png)

输入：元素名 + `+` + 元素名，实现同级的元素标签。

用例：`h1+h2`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/08-el-same-level.png)

输入：元素名 + `^` + 元素名，实现符号前的元素标签提升一行。

用例：`div>p^span`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/09-el-promote.png)

### 4、分组
输入：`()` + 元素名，实现元素标签的分组。

用例：`(div>h1)+(div>h2)`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/10-el-group.png)

### 5、隐式标签
根据父元素标签，自动判断子元素标签。

* li：用于ul和ol中
* tr：用于table中
* td：用于tr中
* option：用于select中

### 6、迭代
输入：元素名 + `*` + 数字，实现元素标签的多次迭代。

用例：`ul>li*3`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/11-el-iterator.png)

输入：属性值 + `*` + 数字，实现元素属性值的多次迭代。

用例：`ul>li.item$*3`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/12-el-value-iterator.png)

## 三、CSS

### 1、属性值
输入：属性缩写 + 属性值，自动生成带有属性值的元素属性。

用例：`w100`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/13-css-attribute-value.png)

输入：属性缩写 + 属性值 + 单位，自动生成指定单位的属性值的元素属性。

用例：`h10p`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/14-css-attribute-value-unit.png)

### 2、附加属性
输入：元素缩写，自动生成元素及其主要属性。

用例：`@f`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/15-css-el-abbr.png)

输入：元素缩写 + `+`，自动生成元素的附加属性。

用例：`@f+`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/16-css-el-attr-plus.png)

### 3、匹配
输入：元素缩写，自动匹配最接近的元素语法。

用例：`ov`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/17-css-el-mapping.png)

### 4、前缀
如果使用非w3c标准的CSS属性，自动添加供应商前缀。

用例：`trs`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/18-css-el-prefix.png)

输入：`-` + 前缀缩写，指定特定的前缀。

用例：`-w-trs`

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-23-how-to-use-emmet/19-css-el-prefix-abbr.png)

## 四、定制

* snippets.json：添加或修改缩写
* preferences.json：更改过滤器和操作的行为
* syntaxProfiles.json：定义如何生成HTML或XML代码

---

参考文章：

[Emmet：HTML/CSS代码快速编写神器](http://www.iteye.com/news/27580)
