---
title: MongoDB 安装和使用
date: 2023-06-10 20:25:49
tags: 
- Database
- Docker
cover: 2023-06-10-21-22-01.png
---
# MongoDB 安装和使用

## 使用 Docker 安装

### Pull the MongoDB Docker Image

```bash
docker pull mongo
```

### Run the Image as a Container

```bash
mkdir -p ~/.mongodb/db
docker run --name mongo -d -p 27017:27017 --privileged=true \
 -e MONGO_INITDB_ROOT_USERNAME=root \
 -e MONGO_INITDB_ROOT_PASSWORD=021009 \
 -v ~/docker/mongodb/db:/data/db mongo 
```

### Connect to the MongoDB Deployment with mongosh

Open an interactive container instance of mongo and connect to the deployment with mongosh.

```bash
docker exec -it mongo mongosh
```

![](2023-06-10-20-54-19.png)

## mongo 体系结构

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| ------------ | ---------------- | ----------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table        | joins            | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

## 数据库(Database)

"show dbs" 命令可以显示所有数据的列表。
![](2023-06-10-21-02-13.png)

执行 "db" 命令可以显示当前数据库对象或集合。
![](2023-06-10-21-02-34.png)

运行"use"命令，可以连接到一个指定的数据库。
![](2023-06-10-21-02-58.png)

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

    admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
    local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
    config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

## 集合(Collection)

集合就是 MongoDB 文档组，类似于 RDBMS 中的表

集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。

比如，我们可以将以下不同数据结构的文档插入到集合中：

    {"site":"www.baidu.com"}
    {"site":"www.google.com","name":"Google"}
    {"site":"www.quanwei.fun","name":"quanwei","age":20}

## 文档(Document)

文档是一组键值(key-value)对(即 BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。

一个简单的文档例子如下：

    { "name":"quanwei"}

    文档中的键/值对是有序的。
    文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。
    MongoDB区分类型和大小写。
    MongoDB的文档不能有重复的键。
    文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

    键不能含有\0 (空字符)。这个字符用来表示键的结尾。
    .和$有特别的意义，只有在特定环境下才能使用。
    以下划线"_"开头的键是保留的(不是严格要求的)。

## MongoDB 数据类型

| 数据类型           | 描述                                             |
| ------------------ | ------------------------------------------------ |
| String             | 字符串。UTF-8                                    |
| Integer            | 整型数值。分为 32 位或 64 位。                   |
| Boolean            | 布尔值。                                         |
| Double             | 双精度浮点值。                                   |
| Min/Max keys       | 将一个值与 BSON元素的最低值和最高值相对比。      |
| Array              | 数组或列表 键                                    |
| Timestamp          | 时间戳。                                         |
| Object             | 用于内嵌文档。                                   |
| Null               | 用于创建空值。                                   |
| Symbol             | 符号。该一般用于采用特殊符号类型的语言           |
| Date               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。 |
| Object ID          | 对象 ID。用于创建文档的 ID。                     |
| Binary Data        | 二进制数据。                                     |
| Code               | 代码类型。用于在文档中存储 JavaScript 代码。     |
| Regular expression | 正则表达式类型。用于存储正则表达式。             |

## "DDL"

创建数据库：

```bash
use <DATABASE_NAME>
```

有则切换，无则创建

删除数据库：

```bash
db.dropDatabase()
```

创建集合：

```bash
db.createCollection(name, options)
```

删除集合:

```bash
db.<collection_name>.drop()
```

## "DML"

### 插入文档

```sql
db.<COLLECTION_NAME>.insert(document)

db.collection.insertOne() 

db.collection.insertMany()
```

### 更新已存在的文档：

```sql
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

    query : update的查询条件，类似sql update查询内where后面的。
    update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
    upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
    multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
    writeConcern :可选，抛出异常的级别。

### 替换已有文档

_id 主键存在就更新，不存在就插入：

```sql
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

### 删除文档:

```sql
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

    query :（可选）删除的文档的条件。
    justOne : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
    writeConcern :（可选）抛出异常的级别。

### 查询数据：

```bash
db.collection.find(query, projection)
```

    query ：可选，使用查询操作符指定查询条件
    projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

## 用户

```js
db.createUser({
  user:"quanwei",
  pwd:"021009",
  customData:{
      name:'唐权威',
      email:'1076451802@qq.com'
  },
  roles:[
      {
        "role":"readWrite",
        "db":"final"
      }
  ] 
  })
```

use admin
db.auth("quanwei","021009")
db.auth("root","021009")
show users
