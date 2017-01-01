---
layout: post
title:  "Markdown快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/markdown_icon.jpg)

## 一、概述
Markdown是一种**轻量级**的标记语言，与之对应的是**重量级**的HTML。

它通过少量简单的语法就可以轻易实现常见的排版格式，将关注点放在内容而不是排版上，从而大大提高了文字工作者的工作效率。同时，还支持不同类型的导出格式（如html、pdf等），利于在不同环境中的使用。

## 二、常用语法
根据元素所在区域进行划分，主要分为区块元素和区段元素。

### 1、区块元素
* 标题

	用`#`标识符标记。`#`代表一级标题，`##`代表二级标题，以此类推，最多支持到六级标题。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/01-sample-topic.jpg)

* 段落和换行

	段落是由一个或多个连续的文本行组成。

	换行是由一个以上的空行来实现。

* 引用

	在段落的开头处使用`>`标识符。可以根据层次增加`>`标识符的数量，表示引用的嵌套。

	引用的区块内还可以使用其他的Markdown语法。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/03-sample-quote.jpg)

* 列表

	Markdown中的列表分为两种：无序列表和有序列表。

	无序列表使用`*`、`+`、`-`标识符标记。

	有序列表使用数字加上`.`标识符标记。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/04-sample-list-no-order.jpg)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/05-sample-list-order.jpg)

* 代码块

	使用四个空格或一个制表符缩进。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/06-sample-code-block.jpg)

* 分割线

	使用三个以上的`*`、`-`、`_`字符就可以作成一条分割线。可以在中间插入空格，但行内不能有其他字符。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/07-sample-split.jpg)

### 2、区段元素
* 粗体与斜体

	使用`*`包含一段文本，表现为*斜体*形式。

	使用`**`包含一段文本，表现为**粗体**形式。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/08-sample-font.jpg)

* 图片与链接

	使用一个方括号`[]`紧跟一个圆括号`()`来标记链接。方括号中是链接的文字，圆括号中是链接的具体地址。

	使用一个惊叹号`!`，后面跟着一个方括号和一个圆括号来标记图片。方括号中是图片的替代文字，圆括号中是图片的具体地址。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/09-sample-link.jpg)

* 行内代码

	使用`标识符将代码包含起来。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-02-how-to-use-markdown/10-sample-inline-code.jpg)

### 3、其他
* 内容目录

	使用`[TOC]`引用目录。

* 代码块增强

	使用```+语言名称进行标记。

* 注脚

	使用`[^footnote]`表示注脚。

* 标签和分类

	一般在文首使用`tags`添加标签，使用`categories`添加分类。

* TODO列表

	使用带有`[]`或`[X]`项的列表生成一个待办事项列表。

* 表格

	使用Markdown生成表格比较麻烦，并且效果也不理想，不建议使用。。。

