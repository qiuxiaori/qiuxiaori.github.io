---
title: ORM简记
tags: orm
categories: program
---

### 一. 概述

ORM 是通过实例对象的语法，完成关系型数据库的操作的技术，是"对象-关系映射"（Object/Relational Mapping） 的缩写。

<!--more-->
##### 对应关系
| 数据库 | 对象 |
|:---|:---|
| 数据库的表（table）| 类（class) |
| 记录（record，行数据 | 对象（object） |
| 字段（field） | 对象的属性（attribute） |


##### 例

* 一行 SQL 语句

```
SELECT id, first_name, last_name, phone, birth_date, sex
 FROM persons 
 WHERE id = 10
```

* 程序直接运行 SQL，操作数据库的写法如下

```
res = db.execSql(sql);
name = res[0]["FIRST_NAME"];
```

* 改成 ORM 的写法如下

```
p = Person.get(10);
name = p.first_name;
```

### 二. 优缺点

##### 优点

1. 数据模型都在一个地方定义，更容易更新和维护，也利于重用代码。
2. ORM 有现成的工具，很多功能都可以自动完成，比如数据消毒、预处理、事务等等。
3. 它迫使你使用 MVC 架构，ORM 就是天然的 Model，最终使代码更清晰。
4. 基于 ORM 的业务代码比较简单，代码量少，语义性好，容易理解。
5. 你不必编写性能不佳的 SQL。

##### 缺点

1. ORM 库不是轻量级工具，需要花很多精力学习和设置。
2. 对于复杂的查询，ORM 要么是无法表达，要么是性能不如原生的 SQL。
3. ORM 抽象掉了数据库层，开发者无法了解底层的数据库操作，也无法定制一些特殊的 SQL。

### 三. 命名规范

1. 一个类对应一张表。类名是单数，且首字母大写；表名是复数，且全部是小写。比如，表books对应类Book。
2. 如果名字是不规则复数，则类名依照英语习惯命名，比如，表mice对应类Mouse，表people对应类Person。
3. 如果名字包含多个单词，那么类名使用首字母全部大写的骆驼拼写法，而表名使用下划线分隔的小写单词。比如，表book_clubs对应类BookClub，表line_items对应类LineItem。
4. 每个表都必须有一个主键字段，通常是叫做id的整数字段。外键字段名约定为单数的表名 + 下划线 + id，比如item_id表示该字段对应items表的id字段。

### 四. Node.js orm 框架

##### Mongoose

目前比较常见的 MongoDB ORM 框架，官方说法是 ODM 框架，可见对关系型数据库支持一般

* 官网：https://mongoosejs.com/
* 数据库：仅支持 MongoDB
* 编程风格：
* * 支持 Promise/async/await
* * 基于 JS 内置类型的 Schema 声明
* * 基于链式构造的 Query Builder 查询
* 周边技术：
* * Typegoose
https://www.npmjs.com/package/typegoose
可以增加 TypeScript 支持，支持使用 Reflect Metadata 自动映射 TS 类型标注

##### Sequelize

较老牌的 Node.js ORM 框架，相对简易

* 官网：http://docs.sequelizejs.com/
* 数据库：支持关系型数据库（MySQL/MSSQL/PostgreSQL/SQLite）
* 编程风格：
* * 支持 Promise/async/await
* * 基于自带的一套类型枚举声明
* * 基于 JSON 对象的查询方式
* * 基于自带的一套操作符描述

##### Bookshelf

Sequelize 之后出现的 ORM 框架，风格与 Sequelize 较相似，看上去比 Sequelize 易用性高

* 官网：http://bookshelfjs.org/
* 数据库：支持关系型数据库
* 编程风格：
* * 基本上是 Eloquent ORM 的 JS 版本
* * 支持 Promise/async/await
* * 支持基于链式构造的 Query Builder 查询

##### TypeORM

基于 Decorator 的 ORM 框架，对 TypeScript 支持较好，同时支持在 JavaScript 中通过手动声明使用，以及 JSON 方式的 Entity 配置声明

* 官网：https://github.com/typeorm/typeorm/
* 数据库：支持关系型数据库，Beta 支持 MongoDB
* 编程风格：
* * 基本上是 Hibernate 的 JS 版本
* * 支持 Promise/async/await
* * 支持基于链式构造的 Query Builder 查询
* * 支持 CLI 工具