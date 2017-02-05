---
layout: post
title:  "MongoDB快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/mongodb_icon.jpg)

## 一、NoSQL概述
NoSQL是Not Only SQL的缩写，指的是非关系型数据库，与传统的关系型数据库相对应，主要用于超大规模数据的存储。

与RDBMS相比，具有如下特点：

* 没有声明性查询语言
* 没有预定义模式
* 键值对存储
* 最终一致性
* 非结构化和不可预知的数据
* CAP定理
* 高性能和可伸缩性

优点：高可扩展性；分布式计算；低成本；半结构化数据；关系简单。

缺点：没有标准化；查询功能有限；最终一致性没有ACID直观。

## 二、MongoDB概述
MongoDB是一个基于分布式文件存储的开源数据库系统，为Web应用提供可扩展的高性能数据存储解决方案。将数据存储为一个文档，数据结构由键值对组成。存储的数据与应用的数据，在格式上（JSON）高度一致。

主要特点：

* 面向文档存储，操作简单
* 可以设置任何属性的索引
* 支持丰富的查询表达式
* 允许在服务端执行脚本
* 支持各种编程语言
* 具有更高的扩展性
* 可以将负载分布在各个节点

[官网地址](https://www.mongodb.com/)

## 三、主要概念

### 1、数据库database
与RDBMS的概念相同。MongoDB的默认数据库为“db”，存储在data目录中。不同的数据库放置在不同的文件中。

数据库名称的限制：

* 不能是空字符串
* 不能包含空格、“.”、“/”、“\”、“$”、空字符等
* 应该全部小写
* 最长64个字节

保留的数据库名称：

* admin，相当于一个root数据库，如果将用户添加到该数据库，那么该用户将自动获得所有数据库的权限
* local，这个数据库不会被复制，可以用来存储仅限于本地单个服务器的任意集合
* config，当Mongo用于分片设置时，该数据库在内部使用，用来保存分片的相关信息

### 2、集合collection
相当于RDBMS中“表”的概念。集合没有固定的结构，可以插入不同格式和类型的数据。数据库的信息存储在dbname.system命名空间下的特殊集合中。

集合名称的限制：

* 不能是空字符串
* 不能包含空字符，空字符表示集合名的结尾
* 不能以system开头，属于系统保留的前缀
* 不能包含保留字符

### 3、文档document
相当于RDBMS中“行”的概念。MongoDB的文档不需要设置相同的字段，并且相同字段不需要相同的数据类型。文档的数据结构采用BSON格式，和JSON基本相同，BSON是一种类json的二进制形式的存储格式。

使用文档时需要注意：

* 文档中的键值对是有序的
* 文档中的值可以是任意数据类型
* 区分类型和大小写
* 不能有重复的键
* 文档的键是字符串

### 4、字段field
相当于RDBMS中“列”的概念。

字段的常用类型：

* String，字符串类型，在MongoDB中，UTF-8才是合法编码
* Integer，整形数值
* Boolean，布尔值
* Double，双精度浮点值
* Min/Max keys，将一个值与BSON（二进制的JSON）元素的最低值/最高值相比较
* Arrays，将数组或列表或多个值存储为一个键
* Timestamp，时间戳，记录文档修改或添加的具体时间
* Object，用于内嵌文档
* Null，用于创建空值
* Symbol，符号，基本等同于字符串类型
* Date，日期时间
* Object ID，用于创建文档的ID
* Binary Data，用于存储二进制数据
* Code，代码类型，用于在文档中存储JavaScript代码
* Regular expression，正则表达式类型，用于存储正则表达式

### 5、索引index
与RDBMS的概念相同。

### 6、主键primary key
自动将`_id`字段设置为主键。

### 7、表连接
不支持表连接，但可以通过嵌入文档的方式实现。

## 四、用法

### 1、安装
从官网下载并直接安装，设置path环境变量。

### 2、启动
直接启动：

    mongod

指定配置文件启动：

    mongod --config /etc/mongodb.conf

### 3、操作数据库
创建数据库：

    use DATABASE_NAME

如果数据库不存在，则创建数据库，否则切换到指定的数据库。

查看当前数据库：

    db

查看所有数据库：

    show dbs

删除当前数据库：

    db.dropDatabase()

在删除之前应该使用db命令查看当前数据库名，或者使用use命令切换到要删除的数据库。

删除集合：

    db.collection.drop()

### 4、操作文档
插入：

    db.COLLECTION_NAME.insert(document)

如果集合不存在，MongoDB会自动创建该集合并插入文档。如果不指定`_id`字段，save方法与insert方法类似。如果指定`_id`字段，save方法会更新该`_id`的数据。

更新：

    db.COLLECTION_NAME.update(query, update, {upsert:boolean, multi:boolean, writeConcern:document})

* query，更新的查询条件，相对于sql的where语句
* update，更新的对象和操作符，相对于sql的set语句
* upsert，可选，如果要更新的记录不存在，是否插入新记录，true为插入，默认false为不插入
* multi，可选，默认false为只更新第一条记录，如果为true，则全部更新
* writeConcern，可选，表示抛出异常的级别

通过传入的文档替换已有的文档：

    db.COLLECTION_NAME.save(document, {writeConcern:document})

删除：

    db.COLLECTION_NAME.remove(query, {justOne:boolean, writeConcern:document})

* query，可选，要删除的文档的满足条件
* justOne，可选，如果为true，则只删除一个文档
* writeConcern，可选，表示抛出异常的级别

如果不包含任何参数，则删除集合中的所有文档。建议在执行remove操作之前，先执行find命令来判断执行条件是否正确。

查询：

    db.COLLECTION_NAME.find()

如果希望格式化查询的结果，可以使用pretty方法：

    db.COLLECTION_NAME.find().pretty()

如果希望只返回一个文档：

    db.COLLECTION_NAME.findOne()

在find方法中，传入多个键值对，每个键值对之间以逗号分隔，等价于SQL中的and条件：

    db.COLLECTION_NAME.find({key1:value1, key2:value2})

在find方法中，使用关键字$or，等价于SQL中的or条件：

    db.COLLECTION_NAME.find({$or:[{key1:value1, key2:value2}]})

在MongoDB中，有四种条件操作符，分别是$gt、$lt、$gte、$lte，对应SQL中的大于、小于、大于等于和小于等于。

    db.COLLECTION_NAME.find({key: {$gt : value}})

还有一个条件操作符$type，用来判断字段的类型：

    db.COLLECTION_NAME.find({key: {$type : typeid}})

使用limit方法，指定要读取的记录数量：

    db.COLLECTION_NAME.find().limit(NUMBER)

使用skip方法，跳过指定数量的记录，参数默认为0：

    db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)

使用sort方法，指定排序的字段，参数为1表示升序，-1表示降序，默认按照升序排列：

    db.COLLECTION_NAME.find().sort({KEY:1})

使用aggregate方法，处理数据并返回计算后的数据结果：

    db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)

聚合操作包括：

* $sum，计算求和
* $avg，计算平均值
* $min，获取最小值
* $max，获取最大值
* $push，插入值到一个数组中
* $addToSet，插入值到一个数组中，但不创建副本
* $first，根据排序获取第一个文档数据
* $last，根据排序获取最后一个文档数据

在MongoDB中，可以使用聚合管道，将文档在一个管道处理完毕之后把结果传递给下一个管道处理。

常用的管道操作：

* $project，修改输入文档的结构，可以用来重命名、增加或删除字段，也可以用来创建计算结果以及嵌套文档
* $match，用于过滤数据，只输出符合条件的文档
* $limit，用来限制聚合管道返回的文档数
* $skip，在聚合管道中跳过指定数量的文档
* $unwind，将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值
* $group，将集合中的文档分组，用于统计结果
* $sort，将输入文档进行排序之后输出
* $geoNear，输出接近某一地理位置的有序文档

索引是特殊的数据结构，存储在一个易于遍历读取的数据集合中，是对数据库表中的若干字段的值进行排序的一种结构。MongoDB还提供多个可选参数，用来限定索引的规则。

创建索引：

    db.COLLECTION_NAME.ensureIndex({KEY:1})

### 5、数据库的备份和恢复
复制：将数据同步在多个服务器的过程。提供了数据的冗余备份，并在多个服务器上存储数据副本。允许从硬件故障和服务中断中恢复数据。复制至少需要两个节点，其中一个是主节点，负责处理客户端请求，其余都是从节点，负责复制主节点的数据。

分片：当存储海量数据时，一台机器不足以存储数据，也不足以提供可接受的读写量。可以通过在多台机器上分割数据，使得数据库系统能够存储和处理更多的数据。Shard用于存储实际的数据块，实际使用中一个shard server可以由几台机器组成。Config server存储整个ClusterMetadata，其中包括chunk信息。Query routers前端路由，客户端由此接入。

备份：在MongoDB中，可以使用mongodump命令来备份数据，该命令可以导出所有数据到指定目录。

    mongodump -h dbhost -d dbname -o dbdirectory

恢复：在MongoDB中，可以使用mongorestore命令来恢复备份的数据。

    mongorestore -h dbhost -d dbname --directoryperdb dbdirectory

### 6、数据库的监控
在安装部署并启动MongoDB服务后，必须了解运行情况，并查看其性能。

mongostat是MongoDB自带的状态检测工具。mongotop用来跟踪一个MongoDB实例，查看读写所花费的时间。这两个工具都位于MongoDB的安装目录的bin目录下。
